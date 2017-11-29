---
title: svn 版本过低的问题(This client is too old to work with working copy)
id: 382
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:58:26
tags:
---

在公司使用ubuntu时，出现‘svn版本过低的问题’。从晚上获取了一些信息，已经解决，现做下记录。

如果使用的linux的话，都自带有python.下面是它的下载地址：

（1）：下载地址：[change-svn-wc-format.py ](http://svn.collab.net/repos/svn/trunk/tools/client-side/change-svn-wc-format.py)

保存下文件。

（2）：直接使用命令

user@user-desktop:~$ python change-svn-wc-format.py /home/user/workspace 1.5
1：python  ： 是系统自带的一种语言。

2： change-svn-wc-format.py : 是所保存的文本文件的名称。

3： /home/user/workspace : 工作目录。

4： 1.5 把系统装的svn的版本该为1.5

（3）：如果出现以下的信息说明成功：

Converted WC at '/home/user/workspace' into format 9 for Subversion 1.5