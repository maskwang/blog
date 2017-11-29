---
title: Linux基础教程：Ubuntu下Nginx的安装及WEB服务配置
id: 401
categories:
  - Linux
  - 技术专栏
date: 2011-10-19 16:01:26
tags:
---

Ubuntu下安装nginx
sudo apt-get install nginxUbuntu安装之后的文件结构大致为：

所有的配置文件都在/etc/nginx下，并且每个虚拟主机已经安排在了/etc/nginx/sites-available下
程序文件在/usr/sbin/nginx
日志放在了/var/log/nginx中
并已经在/etc/init.d/下创建了启动脚本nginx

默认的虚拟主机的目录设置在了/var/www/nginx-default
启动nginx
sudo /etc/init.d/nginx start
然后就可以访问了，http://localhost/ ， 一切正常！如果不能访问，先不要继续，看看是什么原因，解决之后再继续。

nginx默认页面

配置php和mysql
安装Php和mysql
安装php和MySQL:

sudo apt-get install php5-cli php5-cgi mysql-server php5-mysql安装FastCgi
/usr/bin/spawn-fcgi这个文件来管理 FastCgi，它原属于lighttpd这个包里面，但 9.10 后，spawn-fcgi 被分离出来单独成包：

sudo apt-get install spawn-fcgi
配置 nginx
修改nginx的配置文件：/etc/nginx/sites-available/default 修改主机名：

server_name localhost;
修改index的一行修改为：

index index.php index.html index.htm;
去掉下面部分的注释用于支持 php 脚本：

location ~ \.php$ {
fastcgi_pass 127.0.0.1:9000;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME /var/www/nginx-default$fastcgi_script_name;
include /etc/nginx/fastcgi_params;
}
重新启动nginx:

/etc/init.d/nginx stop
/etc/init.d/nginx start
启动fastcgi php:

spawn-fcgi -a 127.0.0.1 -p 9000 -C 10 -u www-data -f /usr/bin/php-cgi
为了让php-cgi开机自启动：

cd /etc/init.d
cp nginx php-cgi
vim php-cgi
替换nginx为php-cgi

并修改相应部分为：

DAEMON=/usr/bin/spawn-fcgi
DAEMON_OPTS="-a 127.0.0.1 -p 9000 -C 10 -u www-data -f /usr/bin/php-cgi"
...
stop)
echo -n "Stopping $DESC: "
pkill -9 php-cgi
echo "$NAME."
然后运行rcconf设置php-cgi为开机自启动

创建、测试phpinfo：

sudo vi /var/www/nginx-default/info.php

&lt;?php phpinfo(); ?&gt;

打开 http://localhost/info.php 。

Nginx phpinfo页面配置nginx + Django
安装Django
配置nginx
测试
no input file specified错误
sudo vi /etc/nginx/sites-avaiable/default其中这个字段

location ~ \.php$ {
root html;
fastcgi_pass 127.0.0.1:9000;

fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME /var/www/nginx-default$fastcgi_script_name;
include fastcgi_params;注意

fastcgi_param SCRIPT_FILENAME /var/www/nginx-default$fastcgi_script_name;/var/www/nginx-default 改为你的网站根目录，一般就是改成这个。

安装Zend Optimizer
要求PHP版本为5.2，不支持Ubuntu 10.04的PHP5.3，请参照Ubuntu 10.04下安装PHP5.2。 http://www.linuxidc.com/Linux/2011-02/32411.htm

下载 Zend Optimizer。直接贴下载地址，参考版本号改（这是32位的），不然主页要注册才能下

http://downloads.zend.com/optimizer/3.3.9/ZendOptimizer-3.3.9-linux-glibc23-i386.tar.gz tar zxvf ZendOptimizer-3.3.9-linux-glibc23-i386.tar.gzcd ZendOptimizer-3.3.9-linux-glibc23-i386/data/5_2_x_comp
sudo mkdir /usr/local/zend
sudo cp ZendOptimizer.so /usr/local/zend 编辑php.ini

sudo gedit /etc/php5/cgi/php.ini开头加入，注意标点符号要英文。

[Zend Optimizer]
zend_optimizer.optimization_level=1
zend_extension="/usr/local/zend/ZendOptimizer.so"关闭php-cgi

sudo killall -HUP php-cgi重启php-cgi

spawn-fcgi -a 127.0.0.1 -p 9000 -C 10 -u www-data -f /usr/bin/php-cgi不需要重启nginx

还是上面那个phpinfo文件，要能看到如下信息

This program makes use of the Zend Scripting Language Engine:Zend Engine v2.2.0, Copyright (c) 1998-2009 Zend Technologies
with Zend Optimizer v3.3.9, Copyright (c) 1998-2009, by Zend Technologies安装XCache
sudo apt-get install php5-xcacheroot@Ubuntu:/home/qii# dpkg -l | grep xcach
ii php5-xcache 1.2.2-5 Fast, stable PHP opcode cacherxcache配置文件路径是

/etc/php5/conf.d/xcache.ini编辑php.ini

sudo gedit /etc/php5/cgi/php.ini把xcache.ini的内容加入到php.ini。

关闭php-cgi

sudo killall -HUP php-cgi重启php-cgi

spawn-fcgi -a 127.0.0.1 -p 9000 -C 10 -u www-data -f /usr/bin/php-cgi不需要重启nginx 检查安装是否成功

root@Ubuntu:/home/qii# php -v
PHP 5.2.10-2Ubuntu6 with Suhosin-Patch 0.9.7 (cli) (built: Oct 23 2009 16:30:10)
Copyright (c) 1997-2009 The PHP Group
Zend Engine v2.2.0, Copyright (c) 1998-2009 Zend Technologies
with XCache v1.2.2, Copyright (c) 2005-2007, by mOo还有前面info.php页应该有XCache模块

info页面的XCache模块

这里有点奇怪的是，如果不把xcache.ini的内容加入php.ini，apache也能载入XCache，但info.php上没XCache模块。安装eAccelerator
sudo apt-get install php5-dev

下载 eAccelerator

wget http://bart.eaccelerator.net/source/0.9.6.1/eaccelerator-0.9.6.1.tar.bz2tar jxvf eaccelerator-0.9.6.1.tar.bz2cd eaccelerator-0.9.6.1 phpize

sudo ./configure -enable-eaccelerator=shared
sudo makeqii@Ubuntu:~/tmp/eaccelerator-0.9.6.1$ sudo make install
Installing shared extensions: /usr/lib/php5/20060613+lfs/
修改php.ini文件，安装为Zend扩展，最好放在开头，放到[zend]之前，免的出莫名其妙的问题：

sudo vi /etc/php5/cgi/php.ini[eaccelerator]
zend_extension="/usr/lib/php5/20060613+lfs/eaccelerator.so"
eaccelerator.shm_size="16"
eaccelerator.cache_dir="/tmp/eaccelerator"
eaccelerator.enable="1"
eaccelerator.optimizer="1"
eaccelerator.check_mtime="1"
eaccelerator.debug="0"
eaccelerator.filter=""
eaccelerator.shm_max="0"
eaccelerator.shm_ttl="0"
eaccelerator.shm_prune_period="0"
eaccelerator.shm_only="0"
eaccelerator.compress="1"
eaccelerator.compress_level="9"
eaccelerator.allowed_admin_path="/var/www/nginx-default/control.php"创建cache缓存目录

eaccelerator.cache_dir="/var/cache/eaccelerator" 这里定义cache路径默认值是/tmp/eaccelerator，这非常简单因为任何人都对该目录可写，但是并不明智，因为重启后系统会自动清理该目录。一个更好的地方是/var/cache/eaccelerator。创建该目录并确保它对eAccelerator的使用者可写（通常该用户是你的网络服务器运行者，可能是www-data）。使用默认值的话这样继续：

mkdir /tmp/eacceleratorchmod 777 /tmp/eaccelerator改成 /var/cache/eaccelerator的话这样继续，先改php.ini

eaccelerator.cache_dir="/var/cache/eaccelerator" sudo mkdir /var/cache/eaccelerator
sudo chown root:www-data /var/cache/eaccelerator
sudo chmod u=rwx,g=rwx,o= /var/cache/eaccelerator复制控制文件control.php到网站根目录

sudo cp control.php /var/www/nginx-default/修改control.php的$user和$pw，默认是admin和eAccelerator

sudo vi /var/www/nginx-default/control.php 关闭php-cgi

sudo killall -HUP php-cgi重启php-cgi

spawn-fcgi -a 127.0.0.1 -p 9000 -C 10 -u www-data -f /usr/bin/php-cgi不需要重启nginx 打开 http://localhost/control.php

eAccelerator control.php页面

查看之前的info.php页面，有下列字段：

This program makes use of the Zend Scripting Language Engine:
Zend Engine v2.2.0, Copyright (c) 1998-2009 Zend Technologies
with eAccelerator v0.9.6.1, Copyright (c) 2004-2010 eAccelerator, by eAccelerator
屏蔽迅雷
新建

/etc/nginx/agent.confif ($http_user_agent ~ "Mozilla/4.0\ \(compatible;\ MSIE\ 6.0;\ Windows\ NT\ 5.1;\ SV1;\ .NET\ CLR\ 1.1.4322;\ .NET\ CLR\ 2.0.50727\)") { return 404; }
注意的是，空格和括号需要使用“\”进行转义。

然后site配置中

include /etc/nginx/agent.conf;迅雷usera-gent和这种做法失效的情况见Apache#屏蔽迅雷