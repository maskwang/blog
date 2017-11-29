---
title: Linux网络环境配置
id: 357
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:52:45
tags:
---

1.指定DNS：

&nbsp;

$sudo vi /etc/resolv.conf

&nbsp;

添加如下内容：

nameserver 208.67.222.222

&nbsp;

修改完后保存直接生效。

&nbsp;

2.指定IP：

&nbsp;

$ sudo vi /etc/network/interfaces

&nbsp;

添加如下内容：

# The primary network interface

auto eth0

iface eth0 inet static //少了这一行一直报错

address 192.168.88.111

netmask 255.255.255.0

gateway 192.168.88.1

#iface eth0 inet dhcp

&nbsp;

保存退出后，使用重启networking命令让新配置生效。

&nbsp;

$sudo /etc/init.d/networking restart