+++
title = "第六回，弃Linux投FreeBSD，稳如磐石路更宽"
date = 2024-10-20
+++
# 第六回，弃Linux投FreeBSD，稳如磐石路更宽
2024-10-20
# 安装freebsd

## 准备镜像

访问[官网下载地址](https://www.freebsd.org/where/)，根据自己的架构选择下载，我是`x64`，所以下载`amd64`  
![](https://img.0pt.icu/learn/freebsd/install-freebsd/01.png)  
下载`FreeBSD-14.1-RELEASE-amd64-disc1.iso`  
![](https://img.0pt.icu/learn/freebsd/install-freebsd/02.png)  
使用`balenaEtcher`进行烧录，选择下载好的镜像和要烧录的u盘，点击`现在烧录`  
![](https://img.0pt.icu/learn/freebsd/install-freebsd/03.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/04.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/05.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/06.png)

烧录完成后，关机重启，按快捷键进入启动项选择节目或者进bios选择启动项，选择进入刚才烧录的u盘，进入freebsd界面后，回车，等待跑码

![](https://img.0pt.icu/learn/freebsd/install-freebsd/07.png)

跑码后出现安装界面，使用左右方向键选中`Install`，回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/08.png)

使用上下方向键，根据自己的情况选择，我这直接默认，选好后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/09.png)

输入主机名，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/10.png)

使用上下方向键选中条目，使用空格键启用该条目/取消该条目，可根据下图快速选择，选好后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/11.png)

使用上下方向键选中条目，这里使用ZFS自动分区，选`Auto (ZFS)`，选好后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/12.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/13.png)

使用上下方向键选择条目，选择`Pool Type/Disks:`，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/14.png)

使用上下方向键选择条目，选择`stripe`，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/15.png)

如果有多个磁盘，这里会显示出来，不过演示只有一块磁盘。使用上下方向键选择条目，使用空格键启用该条目/取消该条目。这里移到要安装freebsd的那块磁盘，按空格键启用，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/16.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/17.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/18.png)

使用上下方向键选择条目，选择`>>> Install`，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/19.png)

使用左右方向键选中`YES`，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/20.png)

等待进度条跑完

![](https://img.0pt.icu/learn/freebsd/install-freebsd/21.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/22.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/23.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/24.png)

进度条跑完后，会来到下面的界面，设置root密码

![](https://img.0pt.icu/learn/freebsd/install-freebsd/25.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/26.png)

设置完root密码的时候选择网卡，有线网卡跟无线网卡都可以，无线网卡还需要选择wifi并输入wifi密码，这里只有一个有线网卡。使用上下方向键选择条目，选择你要配置的网卡，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/27.png)

配置IPv4，使用左右方向键选中`YES`，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/28.png)

使用DHCP，使用左右方向键选中`YES`，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/29.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/30.png)

先不配置IPv6，使用左右方向键选中`No`，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/31.png)

使用左右方向键选中`OK`，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/32.png)

配置时区，根据自己的情况选，使用上下方向键选择时区，然后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/33.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/34.png)

选择`Set Date`设置时间

![](https://img.0pt.icu/learn/freebsd/install-freebsd/35.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/36.png)

使用上下方向键选中条目，使用空格键启用该条目/取消该条目，根据情况启用相应的`services`，选好后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/37.png)

使用上下方向键选中条目，使用空格键启用该条目/取消该条目，根据情况启用相应的系统安全策略，选好后回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/38.png)

添加用户，这里选择`Yes`

![](https://img.0pt.icu/learn/freebsd/install-freebsd/39.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/40.png)

输入用户名，`Uid`和第一个`Login group`直接留空，回车即可

![](https://img.0pt.icu/learn/freebsd/install-freebsd/41.png)

第二个`Login group`，将该用户添加到其他组，填写`wheel` 后回车，接下来一路回车，保持默认设置，直到输入密码。输完密码后再一路回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/42.png)

![](https://img.0pt.icu/learn/freebsd/install-freebsd/43.png)

到这里安装完成，选`Exit`回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/44.png)

选`Yes`回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/45.png)

输入`exit`回车

![](https://img.0pt.icu/learn/freebsd/install-freebsd/46.png)

选择`Reboot`回车，重启电脑

![](https://img.0pt.icu/learn/freebsd/install-freebsd/47.png)

进到tty输入用户名密码，就成功进入freebsd了

![](https://img.0pt.icu/learn/freebsd/install-freebsd/48.png)
