---
title: mysql数据备份3种方案
tags:
  - mysql
  - 备份
id: 651
categories:
  - Mysql
  - 技术专栏
  - 数据库
date: 2012-09-21 10:57:49
---

 mysql按照备份恢复方式分为逻辑备份和物理备份
逻辑备份是备份sql语句，在恢复的时候执行备份的sql语句实现数据库数据的重现
物理备份就是备份数据文件了，比较形象点就是cp下数据文件，但真正备份的时候自然不是的cp这么简单
这2种备份各有优劣，一般来说，物理备份恢复速度比较快，占用空间比较大，逻辑备份速度比较慢，占用空间比较小
下面介绍以下3种常用的备案方法
 <!--more-->
mysqldump工具备份
mysqldump由于是mysql自带的备份工具，所以也是最常用的mysql数据库的备份工具。支持基于InnoDB的热备份。但由于是逻辑备份，所以速度不是很快，适合备份数据量比较小的场景。
mysqldump完全备份+二进制日志 —>实现时间点恢复

温备:
在使用MyISAM引擎中，只能使用温备份，这时候要防止数据的写入，所以先加上读锁
这时候可以进入数据库手动加读锁。这样比较麻烦，在mysqldump工具中直接有一个加锁的选项
mysqldump --databases mydatabase --lock-all-tables --flush-logs> /tmp/backup-`date +%F-%H-%M`.sql
如果是针对某张表备份，只要在数据库名称后面加上表名称就行了
这里注意，要实现时间点的恢复，加上--flush-logs选项，在使用备份文件恢复后，然后再基于二进制日志进行时间点的恢复
时间点的恢复方法
mysqlbinlog mysql-bin.000000x > /tmp/PointTime.sql
然后用mysql命令导入这个sql脚本就行了

热备：
如果使用的是InnoDB引擎，就不必进行对数据库加锁的操作，加一个选项既可以进行热备份：--single-transaction
mysqldump --databases mydb --single-transaction  --flush-logs --master-data=2 > /tmp/backup-`date +%F-%H-%M`.sql

注意点
恢复的时刻关闭二进制日志
mysql>set sql_log_bin=0;
因为这是基于逻辑备份方式，在恢复日志时会执行sql语句插入数据，而恢复时候插入数据的日志没有意义。

基于LVM快照备份
在物理备份中 ，有基于文件系统的物理备份（LVM的快照），也可以直接用tar之类的命令打包。但这些只能进行冷备份
不同的存储引擎能备份的级别也不一样，MyISAM能备份到表级别，而InnoDB不开启每表一文件的话就只能备份整个数据库。

下面就介绍下使用LVM的快照功能进行备份
为了安全 首先在数据库上施加读锁
mysql>FLUSH TABLES WITH READ LOCK;
刷新一下二进制日志，便于做时间点恢复
mysql>FLUSH LOGS；
然后创建快照卷
lvcreate –L 1G –s –n data-snap –p –r /dev/myvg/mydata
最后进入数据库释放读锁
UNLOCK TABLES;
挂载快照卷进行备份
mount –r /dev/myvg/data-snap /mnt/snap
然后对/mnt/snap下的文件进行打包备份
还原的时候，关闭mysqld，然后备份二进制日志后将原来备份的文件还原进去，然后通过二进制日志还原到出错的时间点（通过二进制还原时间点的时候不要忘了暂时关闭二进制日志）

使用percona提供的xtrabackup（推荐）
支持InnoDB的物理热备份，支持完全备份，增量备份，而且速度非常快，而且支持InnoDB引擎的数据在不同数据库迁移
为了让xtrabackup支持更多的功能扩展，配置InnoDB每表一个文件的功能
在my.cnf的mysqld中加入此项： innodb_file_per_table=1
此项不启用将不支持备份单独的表
但如果之前没有启用这个选项，要实现单表一文件的话，可以用mysqldump导出数据，然后启用该选项，恢复回去后就是单表一文件了

首先下载xtrabackup
下载地址：http://www.percona.com/software/percona-xtrabackup
可以直接下载rpm包安装即可
xtrabackup有完全备份，增量备份和部分备份（前面开启innodb每表一文件，就是为了此功能）

完全备份整个数据库
innobackupex --user=root --password=123456 /tmp/backup
此时会在/tmp/backup目录下生成以时间为名的文件夹，里面是备份文件
在这里，备份的数据还不能直接用来还原，因为备份数据中会含有尚未提交的事务或者未同步到数据文件中的事物。这里需要用prepare回滚事物使数据文件处于一致性。
innobackupex --apply-log /tmp/backup/dir
处理完成后才能用来还原数据，用此命令还原
innobackupex --copy-back /tmp/backup/dir
要实现时间点还原，还是需要使用二进制日志

增量备份
增量备份支持Innodb,对于MyISAM只能完全备份
innobackupex –incremental /tmp/backup/incremental --incremental-basedir=/tmp/backup/dir
在进行一次增量备份--incremental-basedir要指向上一次增量备份的目录

如果要进行还原，先进行prepare处理
这里处理的方式，将备份合并
innobackupex --apply-log --redo-only /tmp/backup/dir
innobackupex --apply-log --redo-only /tmp/backup/dir --incremental-dir=/tmp/backup/incremental
最后使用完全备份的那个备份还原

至于差异备份，只要每次将basedir指向完全备份文件夹就行了

最后再废话一句：要实现时间点还原，是需要使用二进制日志的，所以备份好二进制日志至关重要。除非在恢复时间点和上一次备份时间点这段时间的数据对你来说无所谓。。。