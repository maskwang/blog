---
title: 'error: No curses/termcap library found的解决办'
id: 347
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:50:54
tags:
---

<div id="blog_text">

mysql版本：5.1.30

&nbsp;

已经不记得这次是第几次安装mysql了，遇到这个问题倒是第一次。

之前在tar，./configure，make，make install 经典四步时，从来没有想过其中的过程，只觉得像例行公事一样，做就是了。

不幸的是，这次在./configure后，make时出现以下错误：

make: *** No targets specified and no makefile found. stop.

&nbsp;

本来这次还是想向别人请教的，后来转念一想，前段时间还告诉自己：遇到问题，首先想到自己解决。

&nbsp;

于是，在网上找到相关资料，确认是./configure出了问题，于是回头查看，果然发现问题：

最后几行出了错。完整错误信息如下：

checking for tgetent in -lncurses... no

checking for tgetent in -lcurses... no

checking for tgetent in -ltermcap... no

checking for tgetent in -ltinfo... no

checking for termcap functions library... configure: error: No curses/termcap library found

&nbsp;

原因：

缺少ncurses安装包

&nbsp;

**解决办法：**

下载安装相应软件包

一、如果你的系统是RedHat系列：

yum list|grep ncurses

yum -y install ncurses-devel

yum install ncurses-devel

&nbsp;

二、如果你的系统是Ubuntu或Debian：

apt-cache search ncurses

apt-get install libncurses5-dev

&nbsp;

待安装completed！之后，再./configure，顺利通过，然后make &amp;&amp; make install，成功安装，一切OK！~~~

</div>