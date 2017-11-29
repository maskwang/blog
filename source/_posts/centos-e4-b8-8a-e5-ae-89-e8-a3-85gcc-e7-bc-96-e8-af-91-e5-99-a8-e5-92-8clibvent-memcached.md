---
title: centos上安装gcc编译器和libvent memcached
id: 367
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:55:11
tags:
---

由于安装系统的时候没有装c编译器 导致很多源码安装的程序无法编译  所以需要在linux上装c编译器
centos机器上安装比较方便 直接用yum命令在线安装即可 不需要下载安装包
安装步骤如下：
yum install gcc
就这一条命令就行啦  够简单吧
当然 安装时要确保你的主机能够上网
编译器安装后就可以编译安装源码程序包了
下面来安装libvent
<div>Java代码</div>
tar xzvf libevent-1.4.1-beta.tar.gz  cd libevent-1.4.1-beta  ./configure --prefix=/usr/libvent  make  make install
ok libvent安装完毕
接下来安装memcached
<div>Java代码</div>
tar xzvf memcached-1.2.2.tar.gz        cd memcached-1.2.2     ./configure --with-libevent=/usr/libvent --prefix=/usr/memcache      make     make install
ok了