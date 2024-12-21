+++
title = "x79上pve黑苹果"
date = 2024-09-19
+++
# x79上pve黑苹果

2024-09-19

## 准备
选用黑果魏叔上的mac 12 Monterey的虚拟机镜像，下载后使用`dmg2img`转为img镜像后上传到pve.  
创建虚拟机  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/01.avif)  
选择黑苹果镜像`12.7.5.img`  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/02.avif)  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/03.avif)  
选用virtIO  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/04.avif)  
注意，这里的cpu核心改为`8核`，而不是`16核`，不然黑苹果识别不了cpu型号  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/05.avif)  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/06.avif)  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/07.avif)  
然后，编辑虚拟机配置文件，我的虚拟机编号是100

```
nano /etc/pve/qemu-server/103.conf 
```

![](https://img.0pt.icu/learn/pve/pve-hackintosh/08.avif)  
根据自己的 CPU 类别，在第 1 行添加如下配置：

```
args: -device isa-applesmc,osk="ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc" -smbios type=2 -device usb-kbd,bus=ehci.0,port=2 -cpu host,kvm=on,vendor=GenuineIntel,+kvm_pv_unhalt,+kvm_pv_eoi,+hypervisor,+invtsc
```

再将CD 光盘配置(在ide2那个)，删掉`media=cdrom`换成 `cache=unsafe`  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/09.avif)  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/10.avif)  
接下来修改`引导顺序`，把镜像放在第一位  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/11.avif)  
然后开机  
## 第一遍启动
![](https://img.0pt.icu/learn/pve/pve-hackintosh/12.avif)  
选中间那个`Install macOS Monterey`  
等待跑码  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/13.avif)  
进来后选择磁盘工具  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/14.avif)
点击我们分配给苹果的硬盘，点击抹除  
![](https://img.0pt.icu/learn/pve/pve-hackintosh/15.avif)
把磁盘命名一下，格式选择APFS,确认
![](https://img.0pt.icu/learn/pve/pve-hackintosh/16.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/17.avif)
格式化完成后，选择第二个安装
![](https://img.0pt.icu/learn/pve/pve-hackintosh/18.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/19.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/20.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/21.avif)
选择我们刚才格式化的那个磁盘
![](https://img.0pt.icu/learn/pve/pve-hackintosh/22.avif)
等待进度条，跑完后会自动重启，进入第二遍
![](https://img.0pt.icu/learn/pve/pve-hackintosh/23.avif)
## 第二遍启动
出现了一个`macOS Installer`，选择这个回车
![](https://img.0pt.icu/learn/pve/pve-hackintosh/24.avif)
等待跑码
![](https://img.0pt.icu/learn/pve/pve-hackintosh/25.avif)
出现以下情况跑码卡住，我直接强制关机后重启
![](https://img.0pt.icu/learn/pve/pve-hackintosh/26.avif)
可能会重启几次,还选`macOS Installer`，直到看见了进度条，等待进度条跑完，跑完后自动重启，进入第三遍启动
![](https://img.0pt.icu/learn/pve/pve-hackintosh/27.avif)
## 第三遍启动
目标是进入苹果初始化设置
此时出现了一个我们之前设置的磁盘名称`montereyOS` 的选项,选择它回车
![](https://img.0pt.icu/learn/pve/pve-hackintosh/28.avif)
等待跑码
![](https://img.0pt.icu/learn/pve/pve-hackintosh/29.avif)
中间可能还会重启几次，或者跑码卡住(这种情况直接强制关机重启)，还选`montereyOS`
![](https://img.0pt.icu/learn/pve/pve-hackintosh/30.avif)
下面是我自己跑码重启的记录
![](https://img.0pt.icu/learn/pve/pve-hackintosh/31.avif)
卡住了，强制关机重启
![](https://img.0pt.icu/learn/pve/pve-hackintosh/32.avif)
进到进度条了，但是进度条完还是重启
![](https://img.0pt.icu/learn/pve/pve-hackintosh/33.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/34.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/35.avif)
终于这次进来了,进行简单的初始化设置
![](https://img.0pt.icu/learn/pve/pve-hackintosh/36.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/37.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/38.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/39.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/40.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/41.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/42.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/43.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/44.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/45.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/46.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/47.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/48.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/49.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/50.avif)
这个可以不选，维持默认，因为选的话由于没有显卡驱动，会卡一会
![](https://img.0pt.icu/learn/pve/pve-hackintosh/51.avif)
终于是看到mac桌面了
![](https://img.0pt.icu/learn/pve/pve-hackintosh/52.avif)
## 拷贝EFI文件夹
下载[OCC](https://mackie100projects.altervista.org/download-opencore-configurator/)后打开
挂载efi分区，上面是这台虚拟机创建的efi分区，里面没有文件
下面是黑苹果镜像的efi分区，里面放了efi文件
![](https://img.0pt.icu/learn/pve/pve-hackintosh/53.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/54.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/55.avif)
我们把有efi文件夹的那个分区里的文件拷贝到那个空的里面去
![](https://img.0pt.icu/learn/pve/pve-hackintosh/56.avif)
然后关机，在虚拟机设置里面把黑苹果镜像移除
![](https://img.0pt.icu/learn/pve/pve-hackintosh/57.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/58.avif)
现在，不需要镜像也可以正常启动黑苹果了，记得把启动选项调整成虚拟磁盘为第一
![](https://img.0pt.icu/learn/pve/pve-hackintosh/62.avif)
## 添加usb直通
虚拟机设置`添加USB设备`
然后随便找个设备找个usb设备插到你要直通的usb口上面
选使用USB端口，就看到了插上去的设备激活的那个usb口。我的设备是`Razer DeathAdder 2013`,添加后，相当于把usb口直通给虚拟机使用，随便插什么设备都行。
![](https://img.0pt.icu/learn/pve/pve-hackintosh/59.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/60.avif)
我把另一个usb口也直通进去了,可以看到usb设备有2个
![](https://img.0pt.icu/learn/pve/pve-hackintosh/61.avif)
## 添加显卡直通
先把虚拟机的显示设置调为无
![](https://img.0pt.icu/learn/pve/pve-hackintosh/65.avif)
虚拟机设置`添加PCI设备`，选择你对于的显卡设备,其他如下设置
![](https://img.0pt.icu/learn/pve/pve-hackintosh/66.avif)
有些显卡需要romfile才能启动
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
然后显卡接上外接显示屏，启动虚拟机，会发现苹果的视频信号输出到外接显示屏上，配合上usb直通，无限接近物理机黑苹果。
![](https://img.0pt.icu/learn/pve/pve-hackintosh/63.avif)
![](https://img.0pt.icu/learn/pve/pve-hackintosh/64.avif)
我的R9 270需要仿冒显卡才能免驱，使用opencore configuretor打开efi文件夹下面的config.plist,在`DeviceProperties`下的`PciRoot(0x0)/Pci(0x1b,0x0)`条目添加
|Key|Type|Value| 
|-|-|-|
|device-id|Data|68100000|

即可免驱
