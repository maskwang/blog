---
title: windows下硬盘安装Ubuntu 9.04的方法
id: 90
categories:
  - Linux
  - 技术专栏
date: 2011-08-22 22:40:53
tags:
---

[  ](http://article.wxiu.com/tag.php?/ubuntu/)**[Ubuntu](http://hi.baidu.com/wangfengkun/blog/item/%20:void%280%29;/*1242204018687*/) 9.04**是近期 热度最高的一个操作系统，目前官方已经提供了**Ubuntu9.04**的各个版本的ISO文件**下载**。 如果我们没有刻录光驱，我们可以通过**Ubuntu9.04**的**wubi**在**[windows](http://article.wxiu.com/tag.php?/Windows/)**下安 装，**wubi**的兼容性有时候让人很郁闷，**Ubuntu9.04**能在**硬 盘**下**安装**么？答案是肯定的，下面就是**硬盘安装Ubuntu 9.04**的 方法。

1.下载Grub4Dos,解压至XP的C盘根下

修改menu.lst文件,在末尾添加如下内容：

(注意其中粗体Ubuntu-9.04-desktop-i386.iso是desktop版本，如果你下载的不是desktop版，请将其替换成你下 载的镜像的文件名)

title Install Ubuntu

root (hd0,0)

kernel (hd0,0)/vmlinuz boot=casper iso-scan/filename=/Ubuntu-9.04-desktop-i386.iso ro quIEt

splash locale=zh_CN.UTF-8

initrd (hd0,0)/initrd.gz

2.修改XP的boot.ini文件（该文件为系统文件，具有只读隐藏属性）

在boot.ini末尾添加:

C:\grldr="GRUB"

3.下载Ubuntu 9.04的desktopCD的镜像文件。这里有个链接，喜欢哪种自己选吧(http://Linux.chinaitlab.com/info /782532.HTML)

将下载好的镜像文件直接放在C: ，将其中的.disk文件夹加压至C: ， 将casper目录下的initrd.gz和vmlinuz这两个文件也解压至C:

4.重启计算机，选择Grub，进入Grub引 导程序，选择最后一项（Install Ubuntu），稍等即可进入ubuntu

的liveCD模式，此时打开一个终端，在里面 输入：

sudo umount -l /iosdevice

回车即可。点击桌面上的安装图标即可完成安装过 程。

注意Ubuntu 9.04默认的文件系统格式是ext3，而不是ext4,格式分区的时候应注意选择。

不妨关注以下文章：[U盘安装 Linux详解（Debian/Ubuntu）](http://article.wxiu.com/system/linux/200905/08-5605.html)        [Ubuntu 9.04的wubi安装无效解决方案](http://article.wxiu.com/system/linux/200905/06-5586.html)