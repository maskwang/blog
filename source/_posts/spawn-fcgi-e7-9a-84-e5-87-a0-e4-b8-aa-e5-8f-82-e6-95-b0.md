---
title: spawn-fcgi的几个参数
id: 404
categories:
  - Nginx
  - 技术专栏
date: 2011-10-19 16:14:41
tags:
---

spawn-fcgi -a 127.0.0.1 -p 9000 -C 10 -u www-data -f /usr/bin/php-cgi<wbr><wbr><wbr>

-f fastcgi可执行文件的位置（绝对路径） <wbr>
-a fastcgi进程监听的ip <wbr>
-p fastcgi进程监听的端口 <wbr>
-C 子进程数目（该参数为php专用） <wbr>
-P fastcgi进程的pid文件位置 <wbr>
-u fastcgi进程所属的unix用户 <wbr>
-g fastcgi进程所属的unix用户组</wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr>