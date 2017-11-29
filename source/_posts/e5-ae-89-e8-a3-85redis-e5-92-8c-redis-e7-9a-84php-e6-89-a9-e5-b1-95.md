---
title: 安装redis和 redis的php扩展
id: 272
categories:
  - Redis
  - 缓存技术
date: 2011-08-24 03:10:27
tags:
---

**Redis介绍**
<div id="sina_keyword_ad_area2"><wbr> <wbr> <wbr> 数据库主要类型有对象数据库，关系数据库，键值数据库等等，对象数据库太超前了，现阶段不提也罢；关系数据库就是平常说的MySQL，PostgreSQL这些熟的不能再熟的东西，至于键值数据库则是本文要着重说的，其代表主要有[MemcacheDB](http://memcachedb.org/)，[Tokyo Cabinet](http://tokyocabinet.sourceforge.net/)等等。Redis本质上也是一种键值数据库的，但它在保持键值数据库简单快捷特点的同时，又吸收了部分关系数据库的优点。从而使它的位置处于关系数据库和键值数 据库之间。Redis不仅能保存Strings类型的数据，还能保存Lists类型（有序）和Sets类型（无序）的数据，而且还能完成排序（SORT） 等高级功能，在实现INCR，SETNX等功能的时候，保证了其操作的原子性，除此以外，还支持主从复制等功能。

详细描述参见[官方手册](http://code.google.com/p/redis/wiki/index)，同时，官方提供了一个名为[Retwis](http://code.google.com/p/redis/downloads/list)的项目的源代码，可以对照着[官方介绍](http://code.google.com/p/redis/wiki/TwitterAlikeExample)学习，注意其中关于Data Layout的描述，其他没什么。

项目实践中，多以关系数据库为主，不过合理的使用Redis这样的键值数据库，往往能扬长避短，比如说实现一个类似消息队列的功能，对MySQL来说，除非使用[Q4M](http://q4m.31tools.com/)，否则很难满足高并发请求，不过对Redis来说，通过内建的Lists支持，消息队列就是小菜一碟。Redis本质上一个Key/Value数据库，与Memcached类似的 NoSQL型数据库，但是他的数据可以持久化的保存在磁盘上，解决了服务重启后数据不丢失的问题，他的值可以是string（字符串）、list（列 表）、sets（集合）或者是ordered <wbr> <wbr>sets（被排序的集合），所有的数据类型都具有push/pop、add/remove、执行服务端的 并集、交集、两个sets集中的差别等等操作，这些操作都是具有原子性的，Redis还支持各种不同的排序能力
<wbr> <wbr> <wbr> <wbr>Redis 2.0更是增加了很多新特性，如：提升了性能、增加了新的数据类型、更少的利用内存（AOF和VM）
<wbr> <wbr> <wbr> <wbr>Redis支持绝大部分主流的开发语言，如：C、Java、C＃、PHP、Perl、Python、Lua、Erlang、Ruby等等
<wbr> <wbr> <wbr> <wbr>官网：[http://code.google.com/p/redis/](http://code.google.com/p/redis/)</wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr>

**Redis性能**
<wbr> <wbr> <wbr> <wbr>根据Redis官方的测试结果：在50个并发的情况下请求10w次，写的速度是110000次/s，读的速度是81000次/s
地址：[http://code.google.com/p/redis/wiki/Benchmarks](http://code.google.com/p/redis/wiki/Benchmarks)<a name="entrymore"></a></wbr></wbr></wbr></wbr>

**安装过程**
1.没有安装过69hn源需要执行

<wbr><wbr> <wbr><wbr><wbr> 安装#rpm -Uvh http://os.69hn.com/custom.rpm <wbr><wbr><wbr>
</wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr>

2.安装redis
# yum -y install redis

3\. 启动：
#service redis start

测试：

存值：
./redis-cli set hx value
取值：
./redis-cli get hx
<wbr> <wbr></wbr></wbr>

附：redis.conf配置文件：
<div>
<div>引用</div>
<div>#是否作为守护进程运行
daemonize yes
#配置pid的存放路径及文件名，默认为当前路径下
pidfile redis.pid
#Redis默认监听端口
port 6379
#客户端闲置多少秒后，断开连接
timeout 300
#日志显示级别
loglevel verbose
#指定日志输出的文件名，也可指定到标准输出端口
logfile stdout
#设置数据库的数量，默认连接的数据库是0，可以通过select N来连接不同的数据库
databases 16
#保存数据到disk的策略
#当有一条Keys数据被改变是，900秒刷新到disk一次
save 900 1
#当有10条Keys数据被改变时，300秒刷新到disk一次
save 300 10
#当有1w条keys数据被改变时，60秒刷新到disk一次
save 60 10000
#当dump <wbr> <wbr>.rdb数据库的时候是否压缩数据对象
rdbcompression yes
#dump数据库的数据保存的文件名
dbfilename dump.rdb
#Redis的工作目录
dir /var/lib/redis/
########### <wbr> <wbr>Replication #####################
#Redis的复制配置
# slaveof &lt;masterip&gt; &lt;masterport&gt;
# masterauth &lt;master-password&gt;############## SECURITY ###########
# requirepass foobared

############### LIMITS ##############
#最大客户端连接数
# maxclients 128
#最大内存使用率
# maxmemory &lt;bytes&gt;

########## APPEND ONLY MODE #########
#是否开启日志功能
appendonly no
# 刷新日志到disk的规则
# appendfsync always
appendfsync everysec
# appendfsync no
################ VIRTUAL MEMORY ###########
#是否开启VM功能
vm-enabled no
# vm-enabled yes
vm-swap-file logs/redis.swap
vm-max-memory 0
vm-page-size 32
vm-pages 134217728
vm-max-threads 4
############# ADVANCED CONFIG ###############
glueoutputbuf yes
hash-max-zipmap-entries 64
hash-max-zipmap-value 512
#是否重置Hash表
activerehashing yes

</wbr></wbr></wbr></wbr></div>
</div>
**安装php客户端**

2.安装redis客户端
# yum -y install php-pecl-redis

</wbr></wbr></wbr></div>
<div>如果yum安装不成功， 责手动编译安装</div>
<div>

从下面的下载地址找个最新的
<div>
<div>
<div>[view plain](http://blog.csdn.net/tianqixin/article/details/6651026# "view plain")[print](http://blog.csdn.net/tianqixin/article/details/6651026# "print")[?](http://blog.csdn.net/tianqixin/article/details/6651026# "?")</div>
</div>

1.  https://github.com/owlient/phpredis/downloads
</div>
<textarea name="code" readonly="readonly">https://github.com/owlient/phpredis/downloads </textarea>
<div>
<div>
<div>[view plain](http://blog.csdn.net/tianqixin/article/details/6651026# "view plain")[print](http://blog.csdn.net/tianqixin/article/details/6651026# "print")[?](http://blog.csdn.net/tianqixin/article/details/6651026# "?")</div>
</div>

1.  如果使用Centos自带php则确保php-devel安装
</div>
<textarea name="code" readonly="readonly">如果使用Centos自带php则确保php-devel安装</textarea>
<div>
<div>
<div>[view plain](http://blog.csdn.net/tianqixin/article/details/6651026# "view plain")[print](http://blog.csdn.net/tianqixin/article/details/6651026# "print")[?](http://blog.csdn.net/tianqixin/article/details/6651026# "?")</div>
</div>

1.  否则执行： yum install php-devel -y
</div>
<textarea name="code" readonly="readonly">否则执行： yum install php-devel -y</textarea>
<pre>如果是源码编译安装则在安装目录的bin/phpize</pre>
<pre>如：
/usr/local/webserver/php/bin/phpize
./configure --with-php-config=/usr/local/webserver/php/bin/php-config
make
make install</pre>
</div>
<div>&lt;?php
$redis = new Redis();
$redis-&gt;connect('127.0.0.1', 6379);$redis-&gt;set('key', 'value');

echo $redis-&gt;get('key')."\n";

$redis-&gt;setex('key', 3600, 'value'); // sets key → value, with 1h TTL.

$redis-&gt;set('key1', 'val1');
$redis-&gt;set('key2', 'val2');
$redis-&gt;set('key3', 'val3');
$redis-&gt;set('key4', 'val4');

$redis-&gt;delete('key1', 'key2');
echo $redis-&gt;get('key3')."\n" ;

$redis-&gt;delete(array('key3', 'key4'));
?&gt;
更多示例请参考:
https://github.com/owlient/phpredis

</div>