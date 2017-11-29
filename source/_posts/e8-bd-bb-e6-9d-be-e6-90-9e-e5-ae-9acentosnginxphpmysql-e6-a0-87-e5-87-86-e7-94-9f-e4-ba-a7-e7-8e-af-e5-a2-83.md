---
title: 轻松搞定CentOS+Nginx+PHP+MySQL标准生产环境
id: 363
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:54:17
tags:
---

PHP 5.3.1

MySQL 5.0.89

Nginx 0.8.33 或 0.7.65 （可选）

这个可比网上流传的什么一键安装包要好得多，强烈推荐此法安装，适合所有菜鸟和高手。我服务器上全用的源代码编译安装，也好不到哪去，还很费劲。我这个装完已经包含 php 的一些常用扩展， PDO，eaccelerator，memcache，tidy等等。

CentOS 最小化安装，然后先新建一个 repo

# vi /etc/yum.repos.d/centos.21andy.com.repo

放入如下内容
<table width="95%" border="0" cellspacing="0" cellpadding="6" align="center">
<tbody>
<tr>
<td bgcolor="#ddedfb">[21Andy.com]
name=21Andy.com Packages for Enterprise Linux 5 - $basearch
baseurl=http://www.21andy.com/centos/5/$basearch/
enabled=1
gpgcheck=0
protect=1</td>
</tr>
</tbody>
</table>
启用 EPEL repo

CentOS i386 输入如下命令

rpm -ihv [http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-3.noarch.rpm](http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-3.noarch.rpm)

CentOS x86_64 输入如下命令

rpm -ihv [http://download.fedora.redhat.com/pub/epel/5/x86_64/epel-release-5-3.noarch.rpm](http://download.fedora.redhat.com/pub/epel/5/x86_64/epel-release-5-3.noarch.rpm)

然后导入key

rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL

复制代码

OK，一键安装吧
<table width="95%" border="0" cellspacing="0" cellpadding="6" align="center">
<tbody>
<tr>
<td bgcolor="#ddedfb">yum -y install nginx mysql-server php-fpm php-cli php-pdo php-mysql php-mcrypt php-mbstring php-gd php-tidy php-xml php-xmlrpc php-pear php-pecl-memcache php-eaccelerator</td>
</tr>
</tbody>
</table>
最后 yum -y update 一下，全是最新的

如果 nginx 你要用 0.7.65 最新稳定版，把

yum -y install nginx

换成

yum -y install nginx-stable

就可以了

装完你已经可以这样玩了

service mysqld start

service php-fpm start

service nginx start

别忘了设置开机启动

chkconfig --level 345 mysqld on

chkconfig --level 345 php-fpm on

chkconfig --level 345 nginx on

配置文件都在 /etc 下自己找

看看安装多自动

Dependencies Resolved

==========================================================
Package Arch Version Repository Size
==========================================================
Installing:
mysql x86_64 5.0.89-1.el5 21Andy.com 3.5 M
mysql-server x86_64 5.0.89-1.el5 21Andy.com 10 M
nginx x86_64 0.8.33-3.el5 21Andy.com 422 k
php-cli x86_64 5.3.1-2.el5 21Andy.com 2.4 M
php-eaccelerator x86_64 2:0.9.6-1.el5 21Andy.com 118 k
php-fpm x86_64 5.3.1-2.el5 21Andy.com 1.2 M
php-gd x86_64 5.3.1-2.el5 21Andy.com 110 k
php-mbstring x86_64 5.3.1-2.el5 21Andy.com 1.1 M
php-mcrypt x86_64 5.3.1-2.el5 21Andy.com 27 k
php-mysql x86_64 5.3.1-2.el5 21Andy.com 84 k
php-pdo x86_64 5.3.1-2.el5 21Andy.com 91 k
php-pear noarch 1:1.9.0-1.el5 21Andy.com 420 k
php-pecl-memcache x86_64 2.2.5-3.el5 21Andy.com 44 k
php-tidy x86_64 5.3.1-2.el5 21Andy.com 31 k
php-xml x86_64 5.3.1-2.el5 21Andy.com 115 k
php-xmlrpc x86_64 5.3.1-2.el5 21Andy.com 48 k
Installing for dependencies:
gmp x86_64 4.1.4-10.el5 base 201 k
libXaw x86_64 1.0.2-8.1 base 329 k
libXmu x86_64 1.0.2-5 base 63 k
libXpm x86_64 3.5.5-3 base 44 k
libedit x86_64 2.11-2.20080712cvs.el5 epel 80 k
libmcrypt x86_64 2.5.8-4.el5.centos extras 105 k
libtidy x86_64 0.99.0-14.20070615.el5 epel 140 k
php-common x86_64 5.3.1-2.el5 21Andy.com 554 k
sqlite2 x86_64 2.8.17-5.el5 21Andy.com 165 k
t1lib x86_64 5.1.1-7.el5 epel 208 k
Updating for dependencies:
libevent x86_64 1.4.12-1.el5 21Andy.com 129 k

Transaction Summary
==========================================================
Install 26 Package(s)
Update 1 Package(s)
Remove 0 Package(s)