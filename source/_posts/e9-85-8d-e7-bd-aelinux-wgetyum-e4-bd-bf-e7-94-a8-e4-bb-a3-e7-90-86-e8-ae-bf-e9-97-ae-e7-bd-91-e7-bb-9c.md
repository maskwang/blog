---
title: '配置Linux wget,yum 使用代理访问网络'
id: 361
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:53:29
tags:
---

配置Linux wget,yum 使用代理访问网络。

如果你的linux主机需要通过代理服务器才能访问外部网络。可以通过如下方式实现。

1.wget

需要在当前用户的目录下创建一个".wgetrc"文件

[root@linux ~]#vi .wgetrc

http-proxy = 10.1.18.34:3128
ftp-proxy = 10.1.18.34:3128

分别表示http的代理服务器和ftp的代理服务器。如果代理服务器需要密码则使用：
--proxy-user=USER设置代理用户
--proxy-passwd=PASS设置代理密码
这两个参数。
使用参数--proxy=on/off 使用或者关闭代理。

2.yum

[root@linux ~]# vi /etc/yum.conf

#about yum proxy
#day 2010-03-06
#wellpan
#=================
proxy=http://10.1.18.34:3128/
#================

因为我测试的代理没有要求密码验证，如果要求则输入用户名和密码.