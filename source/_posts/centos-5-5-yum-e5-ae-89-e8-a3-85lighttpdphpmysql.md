---
title: CentOS 5.5 yum安装Lighttpd+PHP+MySQL
id: 339
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:49:15
tags:
---

<div>ldj | [idc](http://ldjslife.com/archives/category/idc) | 2011 年 06 月 28 日</div>
CentOS 5.5 yum安装Lighttpd+PHP+MySQL

Lighttpd是一个德国人领导的开源软件，其根本的目的是提供一个专门针对高性能网站，安全、快速、兼容性好并且灵活的web server环境。具有非常低的内存开销，cpu占用率低，效能好，以及丰富的模块等特点。lighttpd是众多OpenSource轻量级的web server中较为优秀的一个。支持FastCGI, CGI, Auth, 输出压缩(output compress), URL重写, Alias等重要功能，而Apache之所以流行，很大程度也是因为功能丰富，在lighttpd上很多功能都有相应的实现了，这点对于apache的用 户是非常重要的，因为迁移到lighttpd就必须面对这些问题。

在这个教程中我们使用主机名server1.example.com，IP地址192.168.0.100。这些设置可能跟你的不同，操作时要替换成你自己的。
** 一、安装MySQL**

yum安装mysql

yum install mysql mysql-server

设置开机启动并启动MySQL

chkconfig –levels 235 mysqld on
/etc/init.d/mysqld start

为root设置一个密码（把yourrootsqlpassword改为要设置的密码）

mysqladmin -u root password yourrootsqlpassword
mysqladmin -h server1.example.com -u root password yourrootsqlpassword

**二、安装Lighttpd**

Lighttpd不存在于官方版centos软件库，但存在于RPMforge软件库。我们安装RHEL 5的RPMforge软件包同样适用于CentOS 5.5：
如果你是64位的系统：

wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.2-2.el5.rf.x86_64.rpm
rpm -Uvh rpmforge-release-0.5.2-2.el5.rf.x86_64.rpm

如果是32位系统：

wget http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.2-2.el5.rf.i386.rpm
rpm -Uvh rpmforge-release-0.5.2-2.el5.rf.i386.rpm

然后，你可以像这样安装Lighttpd：

yum install lighttpd

接着我们设置开机启动lighttpd并立即启动它：

chkconfig –levels 235 lighttpd on
/etc/init.d/lighttpd start

**三、安装PHP**

我们要Lighttpd通过FastCGI使PHP工作。因为我们需要安装软件包lighttpd-fastcgi和php-cli:

yum install lighttpd-fastcgi php-cli

**四、配置Lighttpd和PHP**

为了在Lighttpd激活PHP支持，我们必须修改两个文件，/etc/php.ini和/etc/lighttpd/lighttpd.conf。
首先我们打开/etc/php.ini并在文件尾加入cgi.fix_pathinfo = 1。
然后打开/etc/lighttpd/lighttpd.conf文件并在server.modules块中取消“mod_fastcgi”的批注（即删除前面的“#”）：
接着在同样的文件，找到fastcgi.server块，也取消注释。确保在“socket”行中是使用/tmp/php-fastcgi.socket，修改结果如下：

[...]
#### fastcgi module
## read fastcgi.txt for more info
## for PHP don’t forget to set cgi.fix_pathinfo = 1 in the php.ini
fastcgi.server             = ( “.php” =&gt;
( “localhost” =&gt;
(
“socket” =&gt; “/tmp/php-fastcgi.socket”,
“bin-path” =&gt; “/usr/bin/php-cgi”
)
)
)
[...]

最后重启Lighttpd:

/etc/init.d/lighttpd restart

**五、安装相关PHP组件以支持MySQL**

安装相关组件
你可以通过yum search php以查询可用的php组件。
选择你需要的组件并安装，如下：

yum install php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc

现在重启Lighttpd

/etc/init.d/lighttpd restart

Lighttpd默认根目录是在/srv/www/lighttpd，你可以到/etc/lighttpd/lighttpd.conf作相应修改。