---
title: ContOS5.4+Nginx+PHP+Subversion+MySql+Postgresql+Me
id: 371
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:56:17
tags:
---

**ContOS5.4+Nginx+PHP+Subversion+MySql+Postgresql+Memcached+WebMail **
<div>
<div>**本笔记最终目的 ：**</div>
<div>

不要以为这篇文章看都不容易看完，实际上安装起来很简单如果你对Linux的命令基本上了解的话。因为所有的步骤我都记下来了，你只需要COPY执行即可，绝对能正常安装...

让 MySql、Postgresql、Memcached || Nginx+PHP || Subversion || WebMail 能在CentOS下正常的运行即可.

</div>
</div>
&nbsp;
<div>
<div>安装要求和说明 ：</div>
<div>

系统：默认分区(虚拟机里分的80G硬盘)、应用程序(编辑器)、开发(开发工具)、基本系统(基本)、语言支持(中文支持)

为免去一些麻烦： 删除关闭防火墙、使用Root直接登录进行后面的操作.

如果老是无法安装，那么可以考虑把 xxx &amp;&amp; xxx &amp;&amp; xxx 类似的分开执行，实战过程中我发现有时候这样执行总会有那么一些问题，要是有耐心倒不是什么太大的问题。

</div>
</div>
&nbsp;
<div>
<div>IP配置、关闭防火墙(测试环境以免配置麻烦才这么干的,如果是正式环境建议还是不要关防火墙比较好^_^) ：</div>
<div>

# 关闭防火墙

chkconfig --del iptables
service iptables stop

# 配置IP

vi /etc/sysconfig/network-scripts/ifcfg-eth0

DEVICE=eth0
BOOTPROTO=none
HWADDR=00:0c:29:61:32:21
ONBOOT=yes
IPADDR=192.168.11.11
NETMASK=255.255.255.0
GATEWAY=192.168.18.2

# 配置DNS

vi /etc/sysconfig/network

NETWORKING=yes
NETWORKING_IPV6=no
GATEWAY=192.168.18.2
HOSTNAME=CentOS11

vi /etc/resolv.conf

nameserver 192.168.18.2

# 重启网卡

service network restart

</div>
</div>
&nbsp;
<div>
<div>**准备工作和初始化：**</div>
<div>

# 创建后面可能用到的目录

mkdir -p /work/soft
mkdir -p /work/service/svnData

mkdir -p /work/www/vcn2008.com
mkdir -p /work/www/vcn2008.com.mail
mkdir -p /work/www/vcn2008.com.svn
mkdir -p /work/www/vcn2008.com.html1
mkdir -p /work/www/vcn2008.com.html2
mkdir -p /work/www/vcn2008.com.html3
mkdir -p /work/www/vcn2008.com.html4
mkdir -p /work/www/vcn2008.com.html5

mkdir -p /work/www/vcn2008.com.js1
mkdir -p /work/www/vcn2008.com.js2
mkdir -p /work/www/vcn2008.com.js3
mkdir -p /work/www/vcn2008.com.js4
mkdir -p /work/www/vcn2008.com.js5

mkdir -p /work/www/vcn2008.com.img1
mkdir -p /work/www/vcn2008.com.img2
mkdir -p /work/www/vcn2008.com.img3
mkdir -p /work/www/vcn2008.com.img4
mkdir -p /work/www/vcn2008.com.img5

mkdir -p /work/www/vcn2008.com.css1
mkdir -p /work/www/vcn2008.com.css2
mkdir -p /work/www/vcn2008.com.css3
mkdir -p /work/www/vcn2008.com.css4
mkdir -p /work/www/vcn2008.com.css5

mkdir -p /work/www/vcn2008.com.attachment1
mkdir -p /work/www/vcn2008.com.attachment2
mkdir -p /work/www/vcn2008.com.attachment3
mkdir -p /work/www/vcn2008.com.attachment4
mkdir -p /work/www/vcn2008.com.attachment5

# 更新所有已安装软件包

yum -y update

# 安装必要的开发工具

yum -y install gcc gcc-c++ autoconf make libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers libxml2 libxml2-devel patch pcre-devel

# 上面安装的东东，像gcc, make, autoconf是必要的编译工具
# 像libjpeg,freetype,zlib等，编译PHP时用得到
# 像patch, libxml2等，在使用php-fpm对php打补顶时用得着
# 像pcre-dev等，在编译Nginx服务器时用得着

# 更新的过程感觉比后面下载软件需要的时间还长，真是TNND %$^&amp;**()^&amp;*。
# 有人说 yum install fastestmirror 先安装yum的镜像会快些，我试了说不需要更新任何东西，可能我的已经有了吧可Update一样是几K的速度...

</div>
</div>
&nbsp;
<div>
<div>**下载所需要软件** ：</div>
<div>

mkdir -p /work/soft
cd /work/soft

wget http://dev.mysql.com/get/Downloads/MySQL-5.5/mysql-5.5.0-m2.tar.gz/from/http://opensource.become.com/mysql/
wget http://wwwmaster.postgresql.org/redir/391/f/source/v8.4.3/postgresql-8.4.3.tar.gz
wget http://sysoev.ru/nginx/nginx-0.8.33.tar.gz
wget http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.13.1.tar.gz
wget http://blog.s135.com/soft/linux/nginx_php/mhash/mhash-0.9.9.9.tar.gz
wget http://blog.s135.com/soft/linux/nginx_php/mcrypt/libmcrypt-2.5.8.tar.gz
wget http://blog.s135.com/soft/linux/nginx_php/mcrypt/mcrypt-2.6.8.tar.gz
wget http://www.monkey.org/~provos/libevent-1.4.13-stable.tar.gz
wget http://cn.php.net/get/php-5.3.1.tar.gz/from/this/mirror
wget http://launchpad.net/php-fpm/master/0.6/+download/php-fpm-0.6~5.3.1.tar.gz
wget http://memcached.googlecode.com/files/memcached-1.4.4.tar.gz

# 下载的过程是极其漫长的，很是需要耐心，你慢慢等吧我可是为了以后能完全按照这份文档直接安装慢慢的等等着它们下载完。

</div>
</div>
&nbsp;
<div>
<div>安装配置 MySql ：</div>
<div>

# 创建MySql用户
/usr/sbin/groupadd mysql &amp;&amp; /usr/sbin/useradd -g mysql mysql

cd /work/soft &amp;&amp; tar zxvf mysql-5.5.0-m2.tar.gz &amp;&amp; cd mysql-5.5.0-m2/

./configure --prefix=/work/webServer/mysql/ --enable-assembler --with-extra-charsets=complex --enable-thread-safe-client --with-big-tables --with-readline --with-ssl --with-embedded-server --enable-local-infile --with-plugins=partition,innobase,myisammrg &amp;&amp; make &amp;&amp; make install
chmod +w /work/webServer/mysql &amp;&amp; chown -R mysql:mysql /work/webServer/mysql

# 创建MySQL数据库存放目录

mkdir -p /work/webServer/mysqlData/data/ &amp;&amp; mkdir -p /work/webServer/mysqlData/binlog/ &amp;&amp; mkdir -p /work/webServer/mysqlData/relaylog/ &amp;&amp; chown -R mysql:mysql /work/webServer/mysqlData/

# 以mysql用户帐号的身份建立数据表

/work/webServer/mysql/bin/mysql_install_db --basedir=/work/webServer/mysql --datadir=/work/webServer/mysqlData/data --user=mysql

# 创建my.cnf配置文件： vi /work/webServer/my.cnf

[client]
character-set-server = utf8
port = 3306
socket = /tmp/mysql.sock

[mysqld]
character-set-server = utf8
replicate-ignore-db = mysql
replicate-ignore-db = test
replicate-ignore-db = information_schema
user = mysql
port = 3306
socket = /tmp/mysql.sock
basedir = /work/webServer/mysql
datadir = /work/webServer/mysqlData/data
log-error = /work/webServer/mysqlData/mysql_error.log
pid-file = /work/webServer/mysqlData/mysql.pid
open_files_limit = 10240
back_log = 600
max_connections = 5000
max_connect_errors = 6000
table_cache = 614
external-locking = FALSE
max_allowed_packet = 32M
sort_buffer_size = 1M
join_buffer_size = 1M
thread_cache_size = 300
thread_concurrency = 8
query_cache_size = 512M
query_cache_limit = 2M
query_cache_min_res_unit = 2k
default-storage-engine = MyISAM
thread_stack = 192K
transaction_isolation = READ-COMMITTED
tmp_table_size = 246M
max_heap_table_size = 246M
long_query_time = 3
log-slave-updates
log-bin = /work/webServer/mysqlData/binlog/binlog
binlog_cache_size = 4M
binlog_format = MIXED
max_binlog_cache_size = 8M
max_binlog_size = 1G
relay-log-index = /work/webServer/mysqlData/relaylog/relaylog
relay-log-info-file = /work/webServer/mysqlData/relaylog/relaylog
relay-log = /work/webServer/mysqlData/relaylog/relaylog
expire_logs_days = 30
key_buffer_size = 256M
read_buffer_size = 1M
read_rnd_buffer_size = 16M
bulk_insert_buffer_size = 64M
myisam_sort_buffer_size = 128M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
myisam_recover

interactive_timeout = 120
wait_timeout = 120

skip-name-resolve
master-connect-retry = 10
slave-skip-errors = 1032,1062,126,1114,1146,1048,1396

#master-host = 192.168.1.2
#master-user = username
#master-password = password
#master-port = 3306

server-id = 1

innodb_additional_mem_pool_size = 16M
innodb_buffer_pool_size = 512M
innodb_data_file_path = ibdata1:256M:autoextend
innodb_file_io_threads = 4
innodb_thread_concurrency = 8
innodb_flush_log_at_trx_commit = 2
innodb_log_buffer_size = 16M
innodb_log_file_size = 128M
innodb_log_files_in_group = 3
innodb_max_dirty_pages_pct = 90
innodb_lock_wait_timeout = 120
innodb_file_per_table = 0

#log-slow-queries = /work/webServer/mysqlData/slow.log
#long_query_time = 10

[mysqldump]
quick
max_allowed_packet = 32M

# 创建管理MySQL数据库的shell脚本： vi /work/webServer/mysql.sh

#!/bin/sh
printf "Starting MySQL...\n"
/bin/sh /work/webServer/mysql/bin/mysqld_safe --defaults-file=/work/webServer/my.cnf 2&gt;&amp;1 &gt; /dev/null &amp;

# 赋予shell脚本可执行权限：

mkdir -p /work/webServer/mysql/var/ &amp;&amp; chmod +x /work/webServer/mysql.sh &amp;&amp; chmod -R 0700 /work/webServer/my.cnf

# 启动MySQL

/work/webServer/mysql.sh

# 通过命令行登录管理MySQL服务器（提示输入密码时直接回车）：

/work/webServer/mysql/bin/mysql -u root -p -S /tmp/mysql.sock

# 输入以下SQL语句，创建一个具有root权限的用户

GRANT ALL PRIVILEGES ON *.* TO 'li'@'localhost' IDENTIFIED BY '111111' with grant option;

# （可选）停止MySQL

/work/webServer/mysqlData/mysql stop

</div>
</div>
&nbsp;
<div>
<div>安装配置 Postgresql ：</div>
<div>

# 确保如下软件已经被安装好

yum install gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers

# 安装
# 缺省时将自动使用 GNU Readline， （这样你可以方便地编辑和检索命令历史。）注意要--without-readline否则./configure失败，或者补全各种库，我尝试了一下失败，最后还是选择不安装

cd /work/soft &amp;&amp; tar zxvf postgresql-8.4.3.tar.gz &amp;&amp; cd postgresql-8.4.3/ &amp;&amp; ./configure --prefix=/work/webServer/Postgresql8.4 --without-readline &amp;&amp; make &amp;&amp; make install

# 创建PostgreSQL的用户，设定密码

adduser postgres &amp;&amp; passwd postgres

# 创建PostgreSQL的数据库目录，修改目录的权限属性

mkdir /work/webServer/Postgresql8.4/data &amp;&amp; chown -R postgres /work/webServer/Postgresql8.4

# 以 postgres用户登陆

su postgres

# 初始化数据库集群

/work/webServer/Postgresql8.4/bin/initdb -D /work/webServer/Postgresql8.4/data

# 修改配置文件 vi /work/webServer/Postgresql8.4/data/postgresql.conf

找到#listen_addresses = ‘localhost’修改为 listen_addresses = ‘*’

# vi /work/webServer/Postgresql8.4/data/pg_hba.conf

host all all 127.0.0.1/32 trust
host all all 192.168.11.0/24 password
host all all 192.168.18.0/24 password

# 切换到postgres用户并启动数据库服务

su postgres
/work/webServer/Postgresql8.4/bin/pg_ctl start -D /work/webServer/Postgresql8.4/data

# 退出postgres用户

exit

# 查看postgres是否真的启动

ps aux | grep postgres

# 修改postgres密码

/work/webServer/Postgresql8.4/bin/psql -U postgres postgres

postgres=# alter user postgres with password '111111';
postgres=# \q 退出

# 创建常用命令快捷方式

ln -s /work/webServer/Postgresql8.4/bin/psql /usr/sbin/psql
ln -s /work/webServer/Postgresql8.4/bin/createdb /usr/sbin/createdb
ln -s /work/webServer/Postgresql8.4/bin/createuser /usr/sbin/createuser
ln -s /work/webServer/Postgresql8.4/bin/pg_dump /usr/sbin/pg_dump
ln -s /work/webServer/Postgresql8.4/bin/pg_ctl /usr/sbin/pg_ctl

# 向数据库导入数据和备份数据库(有必要的话,如果只是配置的话完全可以没这个必要)

pg_dump -U postgres testdb &gt; testdb.dmp #备份
psql -U postgres testdb &lt; testdb.dmp #还原

# 设置目录属性,其实前面如果是按上面正常的流程是安全没问题的，只是我一不小心执行了chmod -R /work/webServer/Postgresql8.4，结果一直没法启动

chown postgres /work/webServer/Postgresql8.4/data
chmod -R 0700 /work/webServer/Postgresql8.4/data # data目录一定要是0700，否则无法启动

# 启动Postgresql
su - postgres -c "/work/webServer/Postgresql8.4/bin/postmaster -D '/work/webServer/Postgresql8.4/data' &amp;" &gt;&gt;"/work/webServer/PgsqlStartLog" 2&gt;&amp;1

# 如果是安装到默认的目录可以这么做，这里我们已经改变默认目录所以你要是想用这个的话就得稍做修改，把文件里对应的目录稍改就OK了
cp /work/soft/postgresql-8.4.3/contrib/start-scripts/linux /etc/rc.d/init.d/postgres &amp;&amp; chmod 755 /etc/rc.d/init.d/postgres
/etc/rc.d/init.d/postgres start

</div>
</div>
&nbsp;
<div>
<div>安装配置 Nginx+PHP ：</div>
<div>

# 确保安装了如下软件.

yum install gcc openssl-devel pcre-devel zlib-devel

# 创建nginx运行的用户.

groupadd nginx &amp;&amp; useradd nginx -g nginx

# 下载Nginx源码包【[Nginx 官方维基](http://wiki.nginx.org/NginxModules)】.

cd /work/soft &amp;&amp; tar -zxvf nginx-0.8.33.tar.gz &amp;&amp; cd nginx-0.8.33

./configure --prefix=/usr --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --pid-path=/var/run/nginx/nginx.pid --lock-path=/var/lock/nginx.lock --user=nginx --group=nginx --with-http_ssl_module --with-http_flv_module --with-http_gzip_static_module --http-log-path=/var/log/nginx/access.log --http-client-body-temp-path=/var/tmp/nginx/client/ --http-proxy-temp-path=/var/tmp/nginx/proxy/ --http-fastcgi-temp-path=/var/tmp/nginx/fcgi/ --with-http_stub_status_module
make &amp;&amp; make install

# with-http_stub_status_module 模块可用来统计当前连接数 【更多Nginx模块】
# 添加指定的 Nginx 扩展模块只需要 configure 时带上 --with-模块名 即可
# 小技巧：如已经安装好了Nginx，好添加一个新模块，只需要重新配置，重新 configure &amp;&amp; make 但别 make install, 直接将objs/nginx 拷贝到{$prefix}/sbin下即可，【注意备份原来的】

# 创建nginx需要的文件/文件夹.

mkdir -p /var/tmp/nginx
vi /var/log/nginx/error.log
vi /work/webServer/NginxAccess.log

# 创建启动脚本： vi /work/webServer/nginxStart.sh

#!/bin/sh
/usr/sbin/nginx

# 创建重启脚本： vi /work/webServer/nginxRestart.sh

#!/bin/sh
killall -9 nginx
/usr/sbin/nginx

# 设置脚本权限

chmod +x /work/webServer/nginxStart.sh &amp;&amp; chmod +x /work/webServer/nginxRestart.sh

# 启动 nginx.

/usr/sbin/nginx 或 /work/webServer/nginxStart.sh

# 访问一下看看，看到 Welcome to nginx! 安装便算OK了!

# 配置Nginx

vi /etc/nginx/nginx.conf

[这里有一份我的配置文件，太长了我就不列出来了 **查看/下载**](http://tutorial.vcn2008.com/nginx.conf)

# 安装 libiconv.

cd /work/soft &amp;&amp; tar zxvf libiconv-1.13.1.tar.gz &amp;&amp; cd libiconv-1.13.1 &amp;&amp; ./configure --prefix=/usr &amp;&amp; make &amp;&amp; make install

# 安装mhash、libmcrypt、mhash

yum install libmcrypt-devel libmcrypt-devel mhash-devel

# 安装mcrypt.

cd /work/soft &amp;&amp; tar -zxvf libmcrypt-2.5.8.tar.gz &amp;&amp; cd libmcrypt-2.5.8 &amp;&amp; ./configure &amp;&amp; make &amp;&amp; make install
/sbin/ldconfig
cd libltdl/ &amp;&amp; ./configure --enable-ltdl-install &amp;&amp; make &amp;&amp; make install

cd /work/soft &amp;&amp; tar -zxvf mcrypt-2.6.8.tar.gz
cd mcrypt-2.6.8 &amp;&amp; LD_LIBRARY_PATH=/usr/local/lib ./configure --prefix=/usr &amp;&amp; make &amp;&amp; make install

# 编译安装libevent [建议不要使用yum的方式安装libevent，php-fpm建议Libevent 1.4.12-stable or higher is recommended, and at least libevent 1.4.3-stable is required，因此php-fpm需要1.4.3以上版本libevent的支持，所以去libevent的官网下最稳定版的libevent源码包 进行编译安装]

cd /work/soft &amp;&amp; tar zxvf libevent-1.4.13-stable.tar.gz
cd libevent-1.4.13-stable &amp;&amp; ./configure --prefix=/usr &amp;&amp; make &amp;&amp; make install

# 安装PHP

cd /work/soft &amp;&amp; tar -zxvf php-5.3.1.tar.gz &amp;&amp; tar -zxvf php-fpm-0.6~5.3.1.tar.gz

php-fpm-0.6-5.3.1/generate-fpm-patch
cd php-5.3.1 &amp;&amp; patch -p1 &lt; ../fpm.patch
./buildconf --force
mkdir fpm-build &amp;&amp; cd fpm-build &amp;&amp; ../configure --prefix=/usr --with-config-file-path=/etc/php5 --with-mysql=/work/webServer/mysql --with-pgsql=/work/webServer/Postgresql8.4 --with-mysqli=/work/webServer/mysql/bin/mysql_config --with-iconv-dir=/usr --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --enable-discard-path --enable-safe-mode --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --with-curlwrappers --enable-mbregex --enable-fastcgi --with-fpm --with-libevent=/usr/lib --enable-force-cgi-redirect --enable-mbstring --with-mcrypt --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-ldap --with-ldap-sasl --with-xmlrpc --enable-zip --enable-soap --without-pear

make clean
make ZEND_EXTRA_LIBS='-liconv'
make install

# 应当可以看到这一行
# Installing PHP SAPI module: fpm
# 并且存在 /usr/bin/php-fpm 即代表安装成功

cd .. &amp;&amp; mkdir /etc/php5 &amp;&amp; cp php.ini-production /etc/php5/php.ini

安装PEAR
curl http://pear.php.net/go-pear | /usr/bin/php

启动php-fpm
/etc/init.d/php-fpm start

</div>
</div>
&nbsp;
<div>
<div>安装配置 Memcached ：</div>
<div>

# 安装支持libevent：如果单独安装理论上需要这一步，但这里因为上面我们已经安装过，所以就没有必要再重复的干这种没必要的活了是吧.

# 在/usr/lib里创建新版libevent的软链接(网上有人说需要创建,我的安装过程中并没有用到这命令,也就是说在本文所提到的情况并不需要)

ln -s /usr/local/lib/libevent.1.4.so.2 /usr/lib

# 安装memcached.

yum -y install libevent &amp;&amp; yum -y install libevent-devel (我在正式环境下安装的时候才发现的，我本机可能之前就已经安装过了，大意大意...)

cd /work/soft &amp;&amp; tar -zxvf memcached-1.4.4.tar.gz &amp;&amp; cd memcached-1.4.4 &amp;&amp; ./configure &amp;&amp; make &amp;&amp; make install # 为人比较懒,一次执行是我的风格,当然如果这过程中出错那可能就比较OUT了

# 创建memcached链接

find / -name memcached # 看看memcached在哪里,这样就可以执行相关命令了

# 启动：这里我们用nginx用户来运行memcached。
# 如果你是单独安装的话，可能需要创建一个用户来启动memcached。我记得好像root是不行的，就算行也不建议用root来运行memcached

/usr/local/bin/memcached -u nginx -f 1.25 -p 12345 -m 1m -d
/usr/local/bin/memcached -u nginx -f 1.25 -p 12346 -m 2m -d
/usr/local/bin/memcached -u nginx -f 1.25 -p 12347 -m 3m -d
/usr/local/bin/memcached -u nginx -f 1.25 -p 12348 -m 4m -d
/usr/local/bin/memcached -u nginx -f 1.25 -p 12349 -m 5m -d

# 开机启动：把如上命令添加到 /etc/rc.local 里即可

</div>
</div>
&nbsp;
<div>
<div>安装配置 Subversion ：</div>
<div>

# 安装配置subversion

yum install -y subversion

# 配置帐号密码等

vi /work/service/svnData/svnserve.conf

[general]
anon-access = none
auth-access = write
password-db = ../../passwd
authz-db = ../../authz

vi /work/service/svnData/authz

[aliases]
[groups]
root = rw
admin = rw
[/]
root = rw
admin = rw

vi /work/service/svnData/passwd

[users]
root = 111111
admin = 111111

# 创建初始化版本库

mkdir -p /work/service/svnData/CCode &amp;&amp; svnadmin create /work/service/svnData/CCode
mkdir -p /work/service/svnData/PhpCode &amp;&amp; svnadmin create /work/service/svnData/PhpCode

vi /work/service/svnData/CCode/conf/svnserve.conf
vi /work/service/svnData/PhpCode/conf/svnserve.conf

[general]
anon-access = none
auth-access = write
password-db = ../../passwd
authz-db = ../../authz

chmod -R 0777 /work/service/svnData

# 启动Subversion

svnserve -d -r /work/service/svnData/

</div>
</div>
&nbsp;
<div>
<div>安装配置 WebMail ：</div>
<div>[CentOS5.4下配置WebMail](http://tutorial.vcn2008.com/1.0%20CentOS5.4%20WebMail.php)</div>
</div>
&nbsp;
<div>
<div>设置开机启动 ：</div>
<div>

vi /etc/rc.local

/etc/init.d/php-fpm start
/usr/sbin/nginx

/work/webServer/mysql.sh
su - postgres -c "/work/webServer/Postgresql8.4/bin/postmaster -D '/work/webServer/Postgresql8.4/data' &amp;" &gt;&gt;"/work/webServer/PgsqlStartLog" 2&gt;&amp;1

svnserve -d -r /work/service/svnData/

/usr/local/bin/memcached -u nginx -f 1.25 -p 12345 -m 1m -d
/usr/local/bin/memcached -u nginx -f 1.25 -p 12346 -m 2m -d
/usr/local/bin/memcached -u nginx -f 1.25 -p 12347 -m 3m -d
/usr/local/bin/memcached -u nginx -f 1.25 -p 12348 -m 4m -d
/usr/local/bin/memcached -u nginx -f 1.25 -p 12349 -m 5m -d
# 备份 /var/cache/yum 以便可以在别的机器上直接使用
# 备份 /work/soft 下的软件以便下次能直接使用

</div>
</div>
&nbsp;
<div>
<div>最后遗言 ^_^ ：</div>
<div>
<div>　　这么多的文字难免有错漏的地方，如您发现并能告知我那就太感谢了，在此先谢过了.</div>
</div>
</div>