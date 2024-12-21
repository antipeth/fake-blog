+++
title = "x79搭建pve并开启硬件直通"
date = 2024-09-17
+++

# x79搭建pve并开启硬件直通

2024-09-17

从去年就买了一个服务器主板和相关配件学习装机，一直都没用起来。开学后想着利用起来，开个pve玩玩。
## 硬件配置
|Item|name|price| 
|-|-|-|
|主板|x79浪潮m2220(2011针)|152￥|
|cpu|至强e5-2651v2 x 2|26x2=52￥|
|内存|1866Mhz镁光16G ddr3服务器内存条 x 2|27x2=54￥|
|散热器|杂牌塔式散热 x 2|29x2=58￥|
|显卡|盈通游戏高手R9 270|120￥|
|硬盘|梵想ssd固态128G|79￥|
|电源|华为服务器电源750w+电源转接板+模组线|41+71+65=177￥|
|机箱|开放式机箱|39￥|
|总计||731￥|

## 主板介绍
|Item|name| 
|-|-|
|主板尺寸|E-ATX 30.5cm x 33.2cm|
|内存|4通道 20个ddr3|
|sata|2个sata3 8个sata2|
|pcie|3个pci-e 16x 3个pci-e 8x|
|usb|3个usb2|
|vga|板载显卡，有1个vga口|
|网口|2个千兆网口(intel i350芯片) 1个远程口(IPMI)|
## 注意事项
### 显卡
显卡别看pci-e 16x 有3个，能支持的显卡长度必须小于18.5cm，不然会顶到内存插槽，且如果显卡宽度大于1.5cm,则会把旁边的那个pcie口一起占用。
板载显卡在某些情况（非正常情况）下可能会有冲突。
### 散热器
注意买标准的长方形2011针，不是正方形。
## BIOS设置
按`Del`进bios
### cpu
vt-d开启，虚拟化开启
cpu的几个增强设置全部打开
### 硬盘
`Advanced`->`SATA Configuration`->`SATA Mode` ,把`IDE Mode`改为`AHCI Mode`
### 视频输出信号走板载显卡改为走独显
`Chipset`->`CPU Advanced Settings`->`IOH Configuration`->`VGA Priority`,把`Onboard`改为`Offboard`
## 安装pve
没什么好说的，直接安装即可
## pve注意事项
### 使用无线网卡联网
无线网卡连接手机热点，并使用nat配置给虚拟机联网，我使用这种方法有问题，虚拟机内无网络(我看别人的相关文章，无线网卡连接的是路由器wifi，成功配置网络。)
### 使用usb联网
usb网路共享也有问题，每次需要手动配置网络接口。使用以下脚本
通过手机 rndis 自动联网配置
效果是：手机线连接 pve 开启 USB 网络共享后，5秒左右自动配置好 rndis 网络

代码部分：

/etc/systemd/system/rndis.service

```
[Unit]
Description=Run RNDIS script every 5 seconds
After=network.target

[Service]
ExecStart=bash /root/rndis.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

/root/rndis.sh

```
#!/bin/bash

ANDROID_USB_IP="192.168.42.100"
ANDROID_USB_GATEWAY="192.168.42.1"
ANDROID_USB_SUBNET="255.255.255.0"
PVE_DEFAULT_GATEWAY="192.168.100.1"

# 获取 enx 开头的网卡设备名称
enx_interface=$(ip a | grep "enx[0-9a-f:]*:" | awk '{print $2}' | head -n 1 | sed 's/:$//')

# 检查是否获取到网卡名称
if [ -z "$enx_interface" ]; then
  echo "Error: Could not find enx interface."
  exit 1
fi

# 打印输出网卡名称
echo "Detected enx interface: $enx_interface"

# 备份 /etc/network/interfaces
if [ -f /etc/network/interfaces.bak ]; then
  rm /etc/network/interfaces.bak
  echo "Delete original backup."
fi
cp /etc/network/interfaces /etc/network/interfaces.bak
echo "Backup /etc/network/interfaces"

# 检查 /etc/network/interfaces 是否包含 enx[mac] 字段
if grep -q "enx[0-9a-f]*" /etc/network/interfaces; then
  # 检查是否与新获取的网卡名称相同
  if grep -q "$enx_interface" /etc/network/interfaces; then
    echo "Network interface name already matches, no changes made."
  else
    # 替换 /etc/network/interfaces 中的网卡名称
    sed -i "s/enx[0-9a-f]*/$enx_interface/g" /etc/network/interfaces
    echo "Network interface name updated in /etc/network/interfaces."
  fi
else
  # 追加新的网卡配置
  cat >> /etc/network/interfaces <<EOF

auto $enx_interface
iface $enx_interface inet static
        address $ANDROID_USB_IP
        netmask $ANDROID_USB_SUBNET
        gateway $ANDROID_USB_GATEWAY
EOF
  echo "New network interface configuration appended to /etc/network/interfaces."
fi

# 删除旧路由并添加新路由
ip route delete default via $PVE_DEFAULT_GATEWAY
ip route add default via $ANDROID_USB_GATEWAY

echo "Network configuration updated successfully."

# 检查网络连接
if ! ping -c 1 1.1.1.1 > /dev/null 2>&1; then
    echo "Network is down, restarting networking service..."
    # 仅当网络不可用时重启网络服务
    systemctl restart networking.service
else
    echo "Network is up."
fi
    echo "Done."
```
## 开启直通
参考[国光的教程](https://pve.sqlsec.com/4/2/)
以下是自己配的
### 开启IOMMU
```
nano /etc/default/grub
```
将
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet"
```
改为(包含直通优化，关闭显卡以节能的相关参数)
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt textonly nomodeset nofb pci=noaer pcie_acs_override=downstream,multifunction video=vesafb:off video=efifb:off video=simplefb:off initcall_blacklist=sysfb_init"
```
然后使用
```
update-grub
```
验证IOMMU是否开启
```
dmesg | grep -e DMAR -e IOMMU
```
出现`DMAR: IOMMU enabled`则说明开启成功，还不行重启后再看一下。

### 添加 VFIO 模块
为了我们的显卡顺利直通，需要添加 VFIO 模块到 /etc/modules 文件中：

```Bash
echo "vfio" > /etc/modules
echo "vfio_iommu_type1s" >> /etc/modules
echo "vfio_pci" >> /etc/modules
echo "vfio_virqfd" >> /etc/modules
```
将常见的驱动程序加入黑名单，即让 GPU 相关设备在下次系统启动之后不使用这些驱动，把设备腾出来给 vfio 驱动用：

```Bash
echo "blacklist nvidiafb" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf
echo "blacklist amdgpu" >> /etc/modprobe.d/blacklist.conf
echo "blacklist snd_hda_intel" >> /etc/modprobe.d/blacklist.conf
echo "blacklist snd_hda_codec_hdmi" >> /etc/modprobe.d/blacklist.conf
echo "blacklist i915" >> /etc/modprobe.d/blacklist.conf
```
然后更新内核重启机器：
```Bash
update-initramfs -u && reboot
```
### 验证 IOMMU 中断重新映射是否已启用
```
dmesg | grep 'remapping'
```
看到以下行之一：
```
AMD-Vi: Interrupt remapping enabled
DMAR-IR: Enabled IRQ remapping in x2apic mode
```
### 不知道这是啥
```
nano /etc/modprobe.d/pve-blacklist.conf
```
添加
```
options vfio_iommu_type1 allow_unsafe_interrupts=1
```
### 生成显卡romfile
在window下，使用gpu-z提取。
把`.rom文件`（我的是R9-270.rom）放到pve的`/usr/share/kvm/`目录下面
在虚拟机配置下添加pcie设备，选择显卡，勾选主`gpu`和`rombar`
编辑虚拟机配置
```
nano /etc/pve/qemu-server/100.conf
```
修改
```
# hostpci0: 01:00
hostpci0: 01:00,x-vga=1,romfile=R9-270.rom
```
### 配置硬盘直通
参考[佛西博客](https://foxi.buduanwang.vip/virtualization/1754.html/)
我们可以通过下面命令，列出当前的硬盘列表
```
ls -la /dev/disk/by-id/|grep -v dm|grep -v lvm|grep -v part
```
如下面的例子
```
root@pve:~# ls -la /dev/disk/by-id/|grep -v dm|grep -v lvm|grep -v part
total 0
drwxr-xr-x 2 root root 540 Apr 28 16:39 .
drwxr-xr-x 6 root root 120 Mar  3 15:52 ..
lrwxrwxrwx 1 root root  13 Apr 28 16:39 nvme-eui.01000000010000005cd2e431fee65251 -> ../../nvme2n1
lrwxrwxrwx 1 root root  13 Mar  3 15:52 nvme-eui.334843304aa010020025385800000004 -> ../../nvme1n1
lrwxrwxrwx 1 root root  13 Apr 28 17:36 nvme-eui.334843304ab005400025385800000004 -> ../../nvme0n1
lrwxrwxrwx 1 root root  13 Apr 28 16:39 nvme-INTEL_SSDPE2KX020T8_BTLJ039307142P0BGN -> ../../nvme2n1
lrwxrwxrwx 1 root root  13 Mar  3 15:52 nvme-SAMSUNG_MZWLL800HEHP-00003_S3HCNX0JA01002 -> ../../nvme1n1
lrwxrwxrwx 1 root root  13 Apr 28 17:36 nvme-SAMSUNG_MZWLL800HEHP-00003_S3HCNX0JB00540 -> ../../nvme0n1
lrwxrwxrwx 1 root root   9 Mar  3 15:52 scsi-35000c500474cd7eb -> ../../sda
lrwxrwxrwx 1 root root   9 Mar  3 15:52 wwn-0x5000c500474cd7eb -> ../../sda
```
nvme开头的是nvme硬盘，ata开头是走sata或者ata通道的设备。，scsi是scsi设备-阵列卡raid或者是直通卡上的硬盘。
我们可以通过`qm set <vmid> --scsiX /dev/disk/by-id/xxxxxxx` 进行RDM直通
```
#   虚拟机编号 控制器类型编号 物理磁盘挂载点
qm set 101 --scsi1 /dev/disk/by-id/nvme-INTEL_SSDPE2KX020T8_BTLJ039307142P0BGN
```
### 将镜像写成磁盘格式
```
#                    虚拟机编号           存放镜像的位置                                      储存池
qm disk import 100 /var/lib/vz/template/iso/openwrt.img local-lvm
```

参考连接
https://pve.proxmox.com/wiki/PCI_Passthrough#Verify_IOMMU_is_enabled
https://www.bilibili.com/opus/848481909029732376?spm_id_from=333.976.0.0
https://www.bilibili.com/read/cv34418465/
https://pve.sqlsec.com/7/4/#_2

## 配置省电
安装`cpupower`
```
apt install cpupower

# 设置所有CPU为节能模式
cpupower -c all frequency-set -g powersave

# 设置所有CPU为性能模式
cpupower -c all frequency-set -g performance
编辑 root 用户的 crontab:
```
命令重启后设置的会失效，下面持久化这个命令，让其开机自动执行
```bash
sudo crontab -e
```
添加以下内容以在启动时运行命令:

```
@reboot /usr/bin/cpupower -c all frequency-set -g powersave
```
