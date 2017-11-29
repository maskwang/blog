---
title: linux 下pdo_mysql扩展安装不成功
id: 337
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:48:45
tags:
---

在编译安装[Nginx 0.8.x + PHP 5.2.13](http://hi.baidu.com/wangfengkun/creat/blog/Linux)过程中， 发现php扩展 pdo_mysql安装好几次都是不成功。 查了网上一些教程， 也还是不行。

经过反复测试， 最后终于解决了。 问题是：php.ini在 加载几个扩展的时候， 顺序有误导致的不能加载成功。 问题比较隐蔽。。汗。

正确顺序是：extension = "memcache.so"     extension = "pdo_mysql.so"    extension = "imagick.so" (注： 关键是这个，一定要放到后面， 不然pdo_mysql不能正常加载)。    小弟不解为什么 imagick.so必须放在后面， 往高手指点