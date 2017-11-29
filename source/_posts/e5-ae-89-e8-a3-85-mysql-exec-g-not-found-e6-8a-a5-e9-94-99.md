---
title: '安装 MYSQL exec: g++: not found 报错'
id: 345
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:50:31
tags:
---

<div>源码安装  MYSQL ，，MAKE  时 报错。</div>
<div></div>
<div>../depcomp: line 512: exec: g++: not found
make[2]: *** [my_new.o] 错误 127
make[2]: Leaving directory `/tmp/lamp/mysql-5.0.56/mysys'
make[1]: *** [all-recursive] 错误 1
make[1]: Leaving directory `/tmp/lamp/mysql-5.0.56'
make: *** [all] 错误 2
[root@localhost mysql-5.0.56]#</div>
<div></div>
<div>解决办法：</div>
yum install -y gcc-c++