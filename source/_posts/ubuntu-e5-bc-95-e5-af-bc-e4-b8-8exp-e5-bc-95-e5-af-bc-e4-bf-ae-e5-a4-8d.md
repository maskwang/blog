---
title: ubuntu引导与XP引导修复
id: 84
categories:
  - Linux
  - 技术专栏
date: 2011-08-22 22:39:39
tags:
---

<div id="blog_text">

# **一、XP的引导与修复：**

XP的引导很简单，通常是这样的模式：

通常我们的XP是利用MBR（它不属于任何一个分区，它位于硬盘的第 一个扇区，即主引导扇区）来引导的，

--》MBR引导程序会将活动分区（XP的安装区，一般是C盘）的引 导扇区装入内存

--》NTLDR从引导扇区被装入并初始化--》ntldr读取 boot.ini菜单（用户可以选择一个系统（Operation System）并启动）

--》如果是选择NT/XP，NTLDR运行 Ntdetect.com（ntdetect.com只是为NTLDR提供硬件参数）

-》XP启动（NTLDR将控制权交给XP）

&nbsp;

以上过程依次用到的文件或者程序：MBR--》引导扇区--》 NTLDR（boot.ini,ntdetect.com)-&gt;XP。

其中，ntdetect.com只是启动NT内核的OS时所需要的.

更多内容请参看：http://baike.baidu.com/view /161134.htm

任何一个环节出错都不行，下面给出修复方法：

### 1、mbr损坏或者是改变：

a、插入WINDOWS安装光盘，进入恢复控制台，输 入：fixmbr 或者fdisk /mbr即可。

b、如果是GHOST光盘，是没有恢复控制台的，但是它一盘会在 DOS工具中提供类似fixmbr的命令。只不过名字可能不会是fixmbr（好像叫mbrfix）,大家进入光盘的DOS工具箱看看便知。

c、从光盘进入WINPE，再用WINPE的CMD下运行：MbrFix /drive 0 fixmbr即可，我怀疑这个同b中提到的GHOST光盘中的类fixmbr命令是如出一辙。MbrFix请到这里下载。（不仅仅针对XP！还可以恢复 2000/2003/VISTA等等）

[http://forum.ubuntu.org.cn/viewtopic.php?f=139&amp;t=189240](http://forum.ubuntu.org.cn/viewtopic.php?f=139&amp;t=189240)

[http://www.ylmf.net/read.php?tid=1496366&amp;fpage=0&amp;page=1](http://www.ylmf.net/read.php?tid=1496366&amp;fpage=0&amp;page=1)

### 2、系统分区引导扇区的损坏：

插入WINDOWS安装光盘，进入恢复控制台，输入：fixboot 即可。GHOST光盘好像没有提供此类命令，反正我的几张光盘里面都没有，最后还是为了一个fixboot去买了一张原版的光盘。

**3、引导文件的损坏：**

从别人的电脑上，或者是网上，下载好boot.ini(其实这个可以自己写)，NTLDR，ntdetect.com，然后用各种方法复制到你的C 盘下。

如果不熟悉DOS命令操作的朋友，可以进入winpe(GHOST光盘上的小型XP)，然后把U盘插入电脑，然后把U盘上的这几个文件复制到你的C 盘下就OK！

以上介绍是都是最常用，最原始，最有效，最简单的方法，如果你对分区结构非常了解，也可以利用winhex等工具手动修改。

这里有个网页大家可以参考：[http://www.linux- wiki.cn/index.php/修复被grub覆盖的ntfs分区引导扇区](http://www.linux-wiki.cn/index.php/%E4%BF%AE%E5%A4%8D%E8%A2%ABgrub%E8%A6%86%E7%9B%96%E7%9A%84ntfs%E5%88%86%E5%8C%BA%E5%BC%95%E5%AF%BC%E6%89%87%E5%8C%BA%E3%80%82)

-----------------------------------------------------------------

# 二、Ubuntu的引导修复

它一般是通过grub引导，其实我到现在对grub的了解也相当浅显，如果说错了请大家跟帖指正.

grub分三种：grub,grub2,grub for dos(grub4dos)（见：[http://bbs.znpc.net/viewthread.php?tid=2297](http://bbs.znpc.net/viewthread.php?tid=2297)）

grub2引导入门教程：[谷 歌DOC](http://docs.google.com/Doc?docid=0AeVQ5PqmbeaoZGc0czgyOWtfMjRjbWo0ODVoYw&amp;hl=en)

下面我对我遇到的一些问题给出一些常见的方案：

我的OS是ubuntu10.04,这些方案我基本上都试过了，很有效。

## 1、开机进入：grub rescue&gt;

出现这个问题的原因是因为grub找不到ubuntu所在的分区。

所以需要重新指定分区。这里我直接贴出grub2引导入门教程的方案
<div>
<div>
<div>[view plain](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)[copy to clipboard](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)[print](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)[?](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)</div>
</div>

1.  由于在rescue模式下，只有少量的基本 命令可用，必须通过一定的操作才能加载正常模块，然后进入正常模式。
2.  rescue 模式下可使用的命令有：set，ls，insmod，root，prefix(设 置启动路径)
3.  先假设grub2的核心文件在(hd0,8)分 区，再来看看怎样从rescue模式进入从(hd0,8)启动的正常模式(normal)。
4.  在 rescue模式下search命令不能用，对不清楚grub2文件处于哪个分区的，可以用ls命令查看，比如
5.  ls (hd0,8)/ 查看(hd0,8)分区根目录，看看有没有boot文件夹
6.  ls (hd0,8)/boot/ 查看(hd0,8)分区的/boot目录下文件
7.  ls (hd0,8)/boot/grub/ 查看(hd0,8)分区/boot/grub目录下文 件
8.  通过文件查看，可以确定grub2核心文件处于哪个分区，接下来就可以进行从 rescue到normal的转变动作：
9.  先 ls 看看分区，根据分区列表， 猜下 / 分区的编号再 ls (hd0,x)/ 看分区目录下文件确定找到 / 分区，不对的话继续找。找到 / 分区的 (hd0,x) 继续
10.  grub rescue&gt;root=(hd0,x)
11.  grub rescue&gt;prefix=/boot/grub
12.  grub rescue&gt;set root=(hd0,x)
13.  grub rescue&gt;set prefix=(hd0,x)/boot/grub
14.  grub rescue&gt;insmod normal
15.  rescue&gt;normal     --------&gt;若出现启动菜单，按c进入命令行模 式
16.  rescue&gt;linux /boot/vmlinuz-xxx-xxx root=/dev/sdax
17.  rescue&gt;initrd /boot/initrd.img-xxx-xxx
18.  rescue&gt;boot
19.  内 核 版本号 -xxx-xxx可以按Tab键查看后再手动补全。
20.  有 /boot分区的， 要先找出 /boot 分区 (hd0,x)，再找出 / 分区的 (hd0,y)，同样用 ls (hd0,x)/ 和 ls (hd0,y)/ 的方 式确定分区
21.  grub rescue&gt;root=(hd0,x)
22.  grub rescue&gt;prefix=/grub
23.  grub rescue&gt;set root=(hd0,x)
24.  grub rescue&gt;set prefix=(hd0,x)/grub
25.  grub rescue&gt;insmod normal
26.  rescue&gt;normal     --------&gt;若出现启动菜单，按c进入命令行模 式
27.  rescue&gt;linux /vmlinuz-xxx-xxx root=/dev/sday
28.  rescue&gt;initrd /initrd.img-xxx-xxx
29.  rescue&gt;boot
30.  说 明：
31.  1）由于grub2版本的的不一致，有的可能在第9步 insmod normal.mod加载正常模块后直接进入normal模式，即出现了normal grub&gt;的提示符，这种情况就不能执行第 10步，即可以跳过normal命令的输入。
32.  2）虽然输入normal命令会出现菜 单，但由于缺少加载内核的Linux命令，直接从菜单不能进入系统，需要按c在命令行继续操作。
33.  3）使用/boot单独分区的，要正确修改路径，如
34.  prefix=(hd0,8)/grub
35.  insmod /grub/normal.mod
36.  另 外root=/dev/sda8也要修改根分区的分区号。
37.  4）按boot启动 系统后，再在系统下打开终端，执行命令修复grub
38.  重建配置文件 grub.cfg
39.  sudo update-grub
40.  重建grub到第一硬盘mbr
41.  sudo grub-install /dev/sda
</div>
由 于在rescue模式下，只有少量的基本命令可用，必须通过一定的操作才能加载正常模块，然后进入正常模式。 rescue模式下可使用的命令有：set，ls，insmod，root，prefix(设置启动路径) 先假设grub2的核心文件在(hd0,8)分区，再来看看怎样从rescue模式进入从(hd0,8)启动的正常模式(normal)。 在rescue模式下search命令不能用，对不清楚grub2文件处于哪个分区的，可以用ls命令查看，比如 ls (hd0,8)/ 查看(hd0,8)分区根目录，看看有没有boot文件夹 ls (hd0,8)/boot/ 查看(hd0,8)分区的/boot目录下文件 ls (hd0,8)/boot/grub/ 查看(hd0,8)分区/boot/grub目录下文件 通过文件查看，可以确定grub2核心文件处于哪个分区，接下来就可以进行从rescue到normal的转变动作： 先 ls 看看分区，根据分区列表，猜下 / 分区的编号再 ls (hd0,x)/ 看分区目录下文件确定找到 / 分区，不对的话继续找。找到 / 分区的 (hd0,x) 继续 grub rescue&gt;root=(hd0,x) grub rescue&gt;prefix=/boot/grub grub rescue&gt;set root=(hd0,x) grub rescue&gt;set prefix=(hd0,x)/boot/grub grub rescue&gt;insmod normal rescue&gt;normal --------&gt;若出现启动菜单，按c进入命令行模式 rescue&gt;linux /boot/vmlinuz-xxx-xxx root=/dev/sdax rescue&gt;initrd /boot/initrd.img-xxx-xxx rescue&gt;boot 内 核版本号 -xxx-xxx可以按Tab键查看后再手动补全。 有 /boot分区的，要先找出 /boot 分区 (hd0,x)，再找出 / 分区的 (hd0,y)，同样用 ls (hd0,x)/ 和 ls (hd0,y)/ 的方式确定分区 grub rescue&gt;root=(hd0,x) grub rescue&gt;prefix=/grub grub rescue&gt;set root=(hd0,x) grub rescue&gt;set prefix=(hd0,x)/grub grub rescue&gt;insmod normal rescue&gt;normal --------&gt;若出现启动菜单，按c进入命令行模式 rescue&gt;linux /vmlinuz-xxx-xxx root=/dev/sday rescue&gt;initrd /initrd.img-xxx-xxx rescue&gt;boot 说明： 1）由于grub2版本的的不一致，有的可能在第9步insmod normal.mod加载正常模块后直接进入normal模式，即出现了normal grub&gt;的提示符，这种情况就不能执行第10步，即可以跳过normal命令的输入。 2）虽然输入normal命令会出现菜单，但由于缺少加载内核的Linux命令，直接从菜单不能进入系统，需要按c在命令行继续操作。 3）使用/boot单独分区的，要正确修改路径，如 prefix=(hd0,8)/grub insmod /grub/normal.mod 另外root=/dev/sda8也要修改根分区的分区号。 4）按boot启动系统后，再在系统下打开终端，执行命令修复grub 重建配置文件grub.cfg sudo update-grub 重建grub到第一硬盘mbr sudo grub-install /dev/sdaPS：我的电脑出现这个问题，是因为我利用sudo fdisk /dev/sda删除了分区，分区表已经改变，更改之后，我的OS是在hd0,9.

我用    grub rescue&gt;set 查看后，发现里面还是原来的hd0,10,然后我执行：
<div>
<div>
<div>[view plain](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)[copy to clipboard](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)[print](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)[?](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)</div>
</div>

1.  grub rescue&gt;set root=(hd0,9)
2.  grub rescue&gt;set prefix=(hd0,9)/boot/grub
3.  grub rescue&gt;insmod normal
4.  ue&gt; normal
</div>
grub rescue&gt;set root=(hd0,9) grub rescue&gt;set prefix=(hd0,9)/boot/grub grub rescue&gt;insmod normal rescue&gt; normal之后顺利进入ubuntu10.04,但是重启之后依然是rescue,最后我在终端下用sudo grub-install /dev/sda，重新写了GRUB到MBR。正常了！

## 2、双系统，重装windows引起没有ubuntu启动项

使用安装版的windows重装windows时会改写mbr，造成grub丢失，可以用grub4dos引导进入ubuntu后修复grub或用livecd启动后修复grub。

具体方案请参看：grub2引导入门教程：[谷 歌DOC](http://docs.google.com/Doc?docid=0AeVQ5PqmbeaoZGc0czgyOWtfMjRjbWo0ODVoYw&amp;hl=en)之"五、使用grub2常见错误及修复方法"

注：这里要提醒大家的是，使用grub4dos的话一定要用最新的，至少是 grub4dos-0.4.4-2009.10.16的。（我之前用的grub4dos- 0.4.4-2009-01-11.zip就无法访问EXT4分区，老是提示file not found）

这个问题我曾发在ubuntu论坛上：[http://forum.ubuntu.org.cn/viewtopic.php?f=139&amp;t=273929](http://forum.ubuntu.org.cn/viewtopic.php?f=139&amp;t=273929)

**还有一个比较简单的方案：**在网上下载一个unetbtin，使用它很容易实现ubuntu的引导，也 可以通过它进行linux的硬盘安装。它不会改写引导程序，所以用它很安全。它提供的是菜单式的选项，使用也比较容易的。

教程见（有图，但需注册）：[http://ubuntuforums.org/showthread.php?t=690912](http://ubuntuforums.org/showthread.php?t=690912)

## 3、安装ubuntu时不小心把gurb安装在xp分区，而不是mbr

因为XP分区的引导扇区被grub所覆盖。这种情况下只能引导ubuntu不能引导XP，使用sudo update-grub2也不行，这条命令只会更新配置文件/boot/grub/grub.cfg（这个只是引导菜单）。

方案：

a.首先，把grub 重写到mbr,在终端下输入：
<div>
<div>
<div>[view plain](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)[copy to clipboard](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)[print](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)[?](http://blog.csdn.net/ahui132811/archive/2010/05/19/5607476.aspx#)</div>
</div>

1.  sudo grub-install /dev/sda
</div>
sudo grub-install /dev/sdab.使用fixboot修复XP分区引导扇区：参见上面提及到的“**2、系统分区引导扇区的损坏：”**

</div>