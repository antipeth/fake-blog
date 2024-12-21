+++
title = "记录一次杂牌x79黑苹果"
date = 2024-10-04
+++
# 记录一次杂牌x79 黑苹果
2024-10-04
送同学的，拿来测试黑苹果，EFI链接如下
https://github.com/antipeth/EFI-Motherboard-X79-OpenCore-Hackintosh
## 硬件配置
|Item|name|price| 
|-|-|-|
|主板|x79魔改1356针,不知道什么牌的杂牌|板U 78￥|
|cpu|至强e5-2450v2，非常小众的e5||
|内存|1066Mhz三星16G ddr3服务器内存条带马甲|23￥|
|散热器|杂牌塔式散热|28￥|
|显卡|七彩虹HD7750(刷了个支持uefi的vbios)|60￥|
|硬盘|之前的梵想ssd固态128G||
|电源|之前的华为服务器电源750w+电源转接板+模组线||
|机箱|鞋盒||
|总计||189￥|
## 主板介绍
|Item|name|
|-|-|
|主板尺寸|m-ATX 17cm x 21.5cm|
|内存|2通道 2个ddr3|
|sata|3个可能是sata3|
|m2|一个m2口|
|pcie|1个pci-e 16x 1个pci-e 1x|
|usb|6个usb2|
|vga|板载显卡，有1个vga口|
|网口|1个网口(RealtekRTL8100系)|
|其他|PS/2键鼠，音视频接口|
## 注意事项
### 散热器
注意买标准的正方形2011针，不带固定底座。可以兼容这个1356针的平台。
### m2接口
有点假，读取速度780MB/s,写入速度180MB/s,然后插了m2就识别不到硬盘了。
## BIOS设置
按`Del`进bios
c ,改为`Disabled`
`Advanced`->`USB Configuration`->`XHCI Hand-off` ,改为`Enabled`
`Advanced`->`USB Configuration`->`EHCI Hand-off` ,改为`Disabled`
`Advanced`->`Super IO Configuration`->`Serial Port 0 Configuration` ->`Serial Port`,改为`Disabled`
`Boot`->`Quiet Boot`,改为`Disabled`
`Boot`->`Fast Boot`,改为`Disabled`
 
可能会弹出
```
Warning!!!
Video is in Legacy mode. Select Video policy UEFI first, reboot and try again
```
这时候，需要先把`Boot`->`CSM parameters`->`Launch Video OpROM policy`改为`UEFI only`(你没看错，是`UEFI only`而不是警告让你改的`UEFI first`，如果改为`UEFI first`，警告还是会存在，不让你关掉`CSM parameters`)，然后重启进bios后，就可以关掉`Launch CSM`了
## EFI OC 0.7.7
我的EFI文件如下
有可能会遇到反复重启的情况，但是最后肯定还是可以进去的。
解决方法

Multible Reboot: Some times it takes multiple reboots to get into the system. It gets stuck in the following then follow the below given solution.
![](https://img.0pt.icu/learn/hackintosh/x79/1.avif)  
Solution:

1. Open your DSDT in Rehabman's MacIasland remove SCK1, SCK2 and SCK3

2. Also remove unused Cores. For Eg. If your CPU has 12 threads then only keep the first 12 CPU Cores and delete the rest like this. It depends on how much threads your CPU has and accordingly remove the rest. As my CPU has only 12 threads I have only kept the first 12 and will be removing the rest that I have shown in the image below.

3. Save this file as DSDT.aml
 ![](https://img.0pt.icu/learn/hackintosh/x79/2.avif)  
4. Place this DSDT.aml in the ACPI Folder

5. Reboot.

Now there will be no random reboots while rebooting.
参考链接:
https://www.hackintosh-forum.de/forum/thread/55510-install-monterey-big-sur-on-any-x79-motherboard-huananzhi-chinese-gigabyte-etc-a/
