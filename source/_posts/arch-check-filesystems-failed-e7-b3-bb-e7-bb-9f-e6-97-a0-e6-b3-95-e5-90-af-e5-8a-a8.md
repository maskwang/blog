---
title: arch check filesystems failed系统无法启动
id: 573
categories:
  - Linux
  - 技术专栏
date: 2012-05-14 11:07:04
tags:
---

机器突然黑屏，强制关闭电源重启系统后，系统check filesystem。失败后提示check filesystems failed 。系统无法启动。
解决办法：输入密码后（不知道进入什么模式了，反正和正常的模式不一样）。
输入e2fsck -p /dev/sda3
手工检查完分区sda7后，又有一个提示。不记得了，只记两个单词（也就认识这两个）:意外、fsck
自己认为是用了上述命令产生了意外，需要再用fsck命令，并且不能加-p参数。
然后就键入fsck /dev/sda3
在几个提示后都敲了“y”，完成后“ctrl + D”重启系统（这个模式下，ctrl + D系统即重启）。能够正常进入arch了。哈哈好了
但是还是没搞清楚，请同学们指点！谢谢！