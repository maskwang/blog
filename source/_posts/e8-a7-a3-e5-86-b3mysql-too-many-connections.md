---
title: 解决mysql too many connections
id: 719
categories:
  - Mysql
  - 技术专栏
  - 数据库
date: 2013-09-15 08:15:47
tags:
---

在日常经常会遇到这个报错，很捉鸡。

解决方法分两方面:

第一种方法:<!--more-->

修改my.cnf,  这个参数max_**connections**=500设置的大一些。(ps: 如果程序bug，这种治标不治本)

第二种方法：

要分析程序的原因。

a. 第一步找出目前有多少链接， 并且找出是哪台服务器链接mysql过多。

netstat -tnop | grep 3306 | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr

b.第二步找出到底是哪个程序链接的过多？

这个目前还没找出方法， 跪求大神们指点。
c. 第三步，慎用mysql持久链接。

---------我是可爱分割线------update 2013-9-18----

关于第二种方法的b，可以通过给不同的应用分配不同的user,并且限制链接数. 如有问题可以迅速定位到有问题的应用. 