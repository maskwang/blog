---
title: 'Arch: 安装LNMP笔记'
id: 553
categories:
  - Linux
  - 技术专栏
date: 2012-05-12 12:37:08
tags:
---

最近发现Arch才几百m，而且系统很干净，蛮喜欢的，就在上面安装lnmp玩玩。

1.更新下系统
pacman -Syu

2\. 安装nginx php
pacman -S nginx php-cgi
<!--more-->
3\. 接着建立个启动fastcgi脚本
touch /etc/rc.d/fastcgi:
<pre lang="shell"> #!/bin/bash

/etc/rc.conf
/etc/rc.d/functions

case "$1" in

start)
echo 'Starting Fastcgi Server'

if /usr/bin/php-cgi -b 127.0.0.1:9000 &amp;

then

echo 'success'

else
echo 'fail'
fi

;;

stop)
echo 'Stopping Fastcgi Server'

kill $(pidof php-cgi) &amp;&gt; /dev/null;

if [ $? -gt 0 ]; then

echo 'fail'
else

echo 'success'
fi
;;

restart)
$0 stop
$0 start
;;

*)
echo "Usage: $0 {start|stop|restart}"

esac</pre>
启动cgi， 接着配置nginx，不赘述

4\. 我配的web目录/var/www, 然后怎么访问php都是404， 经过查看error_log, 原来跟php.ini的open_basedir有关，果断注释掉， 重启， ok了

5\. 接着安装mysql
pacman -Sv mysql

然后mysql_install_db --user=mysql
然后报错， “FATAL ERROR: Could not find ./bin/my_print_defaults”
然后进入 /usr目录下，重新执行ok

6\. 接着arch可以登陆mysql， weindow死活telnet不通arch的3306
起初以为是防火墙的原因， 果断停掉，问题依然。
经过百度，原来是用户没有远程访问的权限。
一下百度解决的方法：
“1。 改表法。可能是你的帐号不允许从远程登陆，只能在localhost。这个时候只要在localhost的那台电脑，登入mysql后，更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%"
mysql -u root -pvmwaremysql&gt;use mysql;mysql&gt;update user set host = '%' where user = 'root';mysql&gt;select host, user from user;
2\. 授权法。例如，你想myuser使用mypassword从任何主机连接到mysql服务器的话。
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
如果你想允许用户myuser从ip为192.168.1.3的主机连接到mysql服务器，并使用mypassword作为密码
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'192.168.1.3' IDENTIFIED BY
'mypassword' WITH GRANT OPTION”

我用的是第二种方法解决了问题， ok， 大功告成！

后续持续安装redis、mongo等

+++++++++++++++++++++update 2012.5.13++++++++++++++++++++++++
7\. 安装redis
pacman -Sy redis

启动redis服务
redis-server /etc/redis.conf

验证是否成功
ps -ef | grep redis
没有成功。。去对应的日志一看，/var/logs/redis.log "Can't chdir to '/var/lib/redis/': No such file or directory" 问题找到了。

8.接着安装redis php扩展，这里用phpredis

下载phpredis https://github.com/nicolasff/phpredis
解压后进去相应目录，执行
a) phpize
错误，显示：Cannot find autoconf. Please check your autoconf installation and the $PHP_AUTOCONF environment variable. Then, rerun this script.
b)安装autoconf ： sudo pacman -S　autoconf
重新运行 phpize
./configure –prefix=/usr –enable-redis
make (make: command not found,安装make包)
sudo make install
安装完毕
c) 修改php.ini
添加一行：extension=redis.so
d) 重启服务器
apache : rc.d restart httpd
nginx: rc.d restart php-fpm
e) 测试PHP代码
运行： redis-server (很容易忽略的哦)
运行以下PHP代码：
$redis = new Redis();
$redis-&gt;connect(‘127.0.0.1′,6379);
$redis-&gt;set(‘test’,’hello world!’);
echo $redis-&gt;get(‘test’);
若一切正常则应输出”hello world!”
(本人在命令行运行ok， 结果远程浏览器访问，提示找不到redis类， 擦。忘记重启cgi了)