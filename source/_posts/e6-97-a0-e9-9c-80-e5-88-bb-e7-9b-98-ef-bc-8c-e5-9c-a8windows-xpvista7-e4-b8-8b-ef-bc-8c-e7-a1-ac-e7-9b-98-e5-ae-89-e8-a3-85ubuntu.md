---
title: 无需刻盘，在windows XP/VISTA/7下，硬盘安装ubuntu
id: 92
categories:
  - Linux
  - 技术专栏
date: 2011-08-22 22:41:18
tags:
---

<span style="color: #000000;">以ubuntu9.10为例，ubuntu9.04等之前的版本也可使用此方法。
（ubuntu9.10之前版本没有initrd.lz，使用initrd.gz）

目录
一、前期准备
二、在XP下，硬盘安装ubuntu
三、在windows VISTA/7下，硬盘安装ubuntu
（方法一）：使用boot.ini
（方法二）：使用EasyBCD（推荐）

注：本文只描述，如何不刻盘，使用ubuntu的ISO文件进行硬盘安装（非wubi安装），
如何分区，及安装的心里准备、后续配置，则需查阅相关文档；
推荐安装前，在win下使用分区工具，删除一部分空闲空间出来，不致安装时选错。
数据无价，操作须谨慎！！！

一、前期准备：

1、下载ubuntu 9.10的ISO文件：
下载地址：[http://releases.ubuntu.com/karmic/ubuntu-9.10-desktop-i386.iso](http://releases.ubuntu.com/karmic/ubuntu-9.10-desktop-i386.iso)

2、下载引导文件

（1）若现行操作系统为XP，下载最新版本Grub4DOS
下载地址：[http://download.gna.org/grub4dos/grub4dos-0.4.4-2009-06-20.zip](http://download.gna.org/grub4dos/grub4dos-0.4.4-2009-06-20.zip)

（2）若现行操作系统为win VISTA/7，且使用方法一也就是使用boot.ini文件，则和XP一样，
下载Grub4DOS
下载地址：[http://download.gna.org/grub4dos/grub4dos-0.4.4-2009-06-20.zip](http://download.gna.org/grub4dos/grub4dos-0.4.4-2009-06-20.zip)

（3）若现行操作系统为win VISTA/7，且使用第二种方法也就是使用EasyBCD，
则下载EasyBCD
EasyBCD官方下载：[http://neosmart.net/dl.php?id=1](http://neosmart.net/dl.php?id=1)
EasyBCD华军下载：[http://www.onlinedown.net/soft/58174.htm](http://www.onlinedown.net/soft/58174.htm)

二、在XP下，硬盘安装ubuntu：
1、把下载后的Grub4DOS解压缩后，将目录中的grldr （非grldr.mbr）这一个文件复制到C盘根目录下

2、在下载好的iso文件中，casper文件夹目录下，找到vmlinuz、initrd.lz解压，并复制到C盘根目录下
（无需解压整个casper文件夹）

3、C盘根目录下建立menu.lst文件，内容为：
============================分割线 勿复制==========================================
title Install Ubuntu 9.10
root (hd0,0)
kernel (hd0,0)/vmlinuz boot=casper iso-scan/filename=/ubuntu-9.10-desktop-i386.iso ro quiet splash locale=zh_CN.UTF-8
initrd (hd0,0)/initrd.lz
============================分割线 勿复制==========================================
注：红色字体部分就是需要安装的ISO映像文件的文件名，根据实际情况作调整，我这个是9.10正式版，
如果是9.04正式版，红色字体部分就应该是ubuntu-9.04-desktop-i386.iso

4、 接着，在我的电脑–&gt;工具–&gt;文件夹选项–&gt; 的查看标签下去掉“隐藏受保护的操作系统文件”之前的勾，并勾选“显示所有文件和文件夹”。取消C盘根目录下的boot.ini文件的“只读”属性，然后 用记事本打开boot.ini文件，做如下更改：timeout=0 改成 timeout=5 或者更大的数字，在boot.ini 文件内容末尾加上一行 C:\grldr="GRUB"
（附：boot.ini 文件路径 c:\boot.ini ）

5、将ubuntu-9.10-desktop-i386.iso复制或移到U盘根目录下，硬盘上原有的iso文件则修改文件名，切记

6、插上U盘，重启电脑，开机选择“GRUB”，进入live CD模式

7、双击桌面的“安装”图标就可以开始开始安装

三、在windows VISTA/7下，硬盘安装ubuntu ：

方法 一：使用boot.ini

1、解压下载的Grub4DOS，把其中的grldr和grldr.mbr两个文件复制到C盘根目录（也许你需要以管理员身份完成此步骤）

2、在C盘新建文本文档，重命名为“boot.ini”，并在其中写入以下文字：
============================分割线 勿复制==========================================
[boot loader]
timeout=10
default=C:\grldr.mbr
[operating systems]
C:\grldr.mbr="GRUB"
============================分割线 勿复制==========================================

3、在下载好的iso文件中，casper文件夹目录下，找到vmlinuz、initrd.lz解压，并复制到C盘根目录下
（无需解压整个casper文件夹）

4、C盘根目录下建立menu.lst文件，内容为：
============================分割线 勿复制==========================================
title Install Ubuntu 9.10
root (hd0,0)
kernel (hd0,0)/vmlinuz boot=casper iso-scan/filename=/ubuntu-9.10-desktop-i386.iso ro quiet splash locale=zh_CN.UTF-8
initrd (hd0,0)/initrd.lz
============================分割线 勿复制==========================================
注：红色字体部分就是需要安装的ISO映像文件的文件名，根据实际情况作调整，我这个是9.10正式版，
如果是9.04正式版，红色字体部分就应该是ubuntu-9.04-desktop-i386.iso

5、将ubuntu-9.10-desktop-i386.iso复制或移到U盘根目录下，硬盘上原有的iso文件则修改文件名，切记

6、插上U盘，重启电脑，开机选择“GRUB”，进入live CD模式

7、双击桌面的“安装”图标就可以开始开始安装

方法 二：使用EasyBCD（推荐）

1、以管理员的身份运行easybcd；
依次点击“Add/RemoveEntries”–&gt;”neogrub”–&gt;”InstallNeogrub”,添加了一个名为 “Neogrub Bootloader”的启动项。
接着选择“Configue”，在弹出的menu.lst文件末尾加入以下文字
============================分割线 勿复制==========================================
title Install Ubuntu 9.10
root (hd0,0)
kernel (hd0,0)/vmlinuz boot=casper iso-scan/filename=/ubuntu-9.10-desktop-i386.iso ro quiet splash locale=zh_CN.UTF-8
initrd (hd0,0)/initrd.lz
============================分割线 勿复制==========================================
注：红色字体部分就是需要安装的ISO映像文件的文件名，根据实际情况作调整，我这个是9.10正式版，
如果是9.04正式版，红色字体部分就应该是ubuntu-9.04-desktop-i386.iso

2、在EasyBCD里设置显示开机选单时间为10秒左右。

3、在下载好的iso文件中，casper文件夹目录下，找到vmlinuz、initrd.lz解压，并复制到C盘根目录下
（无需解压整个casper文件夹）

4、将ubuntu-9.10-desktop-i386.iso复制或移到U盘根目录下，硬盘上原有的iso文件则修改文件名，切记

5、插上U盘，重启电脑，开机选择“Neogrub Bootloader”，进入live CD模式

6、双击桌面的“安装”图标就可以开始开始安装

PS：
1、在XP与win VISTA/7下，硬盘安装ubuntu 9.10的方法很多步骤都一样，为便于阅读，每种方法都写全步骤。

2、这里把文件ubuntu-9.10-desktop-i386.iso复制到u盘里是为了避免在安装过程中出现对硬盘不能操作的情况，
如果把ubuntu-9.10-desktop-i386.iso放到硬盘任意一分区根目录下，
安装前也许需要执行命令sudo umount -l /isodevice来避免出现对磁盘不能操作的情况；
安装后也许需要自己添加win 的启动项。

3、硬盘分区格式为NTFS不受此方法影响，
如果有朋友用此方法成功安装，请顶一下本贴让更多的朋友看到，让它能帮助更多的朋友；
如果有朋友用此方法安装时出现问题，或者本方法有错误，也请回贴告诉大家，以便改正或找寻更好的方法。</span>