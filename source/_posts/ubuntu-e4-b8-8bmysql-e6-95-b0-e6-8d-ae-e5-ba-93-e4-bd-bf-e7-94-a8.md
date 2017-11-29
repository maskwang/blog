---
title: Ubuntu下MySQL数据库使用
id: 72
categories:
  - Mysql
  - 技术专栏
date: 2011-08-22 22:37:02
tags:
---

<div id="blog_text">
<div>

昨晚,终于安装上了mysql.但是.操作的时候,老是提示'Access denied for user 'root'@'localhost' (using password: YES).闷了一晚.今早G了一下.找到一篇类似我这种问题的文章.还没测试.先保留着.

该文章的来源是：[http://www.oklinux.cn/html/sql/other/20080309/48613.html](http://www.oklinux.cn/html/sql/other/20080309/48613.html)

它是这样描述:在用命令(sudo apt-get install mysql-server mysql-client)安装完.mysql服务即开始运行了.此时需要修改root密码,但经常会出现这么一种情况.'Access denied for user 'root'@'localhost' (using password: YES)' 或者其他致使无法登录mysql的情况。这里可以按如下步骤解决:
1.打开/etc/mysql/debian.cnf文件,里面存储了相关的密 码.
sudo gedit /etc/mysql/debian.cnf
在[client]段有user=以及password=这两 行，此即我们需要的东西

2\. 输入命令:mysql -udebian-sys-maint -p
debian-sys- maint即debian.cnf中user=后面的内容.回车后会提示输入密码，此时把password=后面的内容复制粘贴后回车即可进行mysql 控制台(一般不要照打，容易出错，复制即可)
3.进入控制台后.按以下步骤进行:
use mysql;
update user set password=PASSWORD('新密码') where user='root';
FLUSH PRIVILEGES;
此 时可以输入quit;退出后用root帐号登录，也可以继续其他操作.

关于刚刚安装好mysql,登录时抛出的错误.今天测试了一下.以ubuntu8.10为例.

1、打开终端,输入mysqladmin -u用户名 -p旧密码 password 新密码 //更改密码 (注意：新密码前面有一个空格)

如果没有返回任何错误信息,表示修改成功.

2、测试一下.登录myql.

**mysql** -h主机地址 -u用户名 －p用户密码 //进入**mysql**数据库.

&nbsp;

关于mysql更多的基本操作命令.

一、**mysql**服 务操作
1、net start **mysql** //启动**mysql**服务

2、net stop **mysql** //停止**mysql**服 务

3、**mysql** -h主机地址 -u用户名 －p用户密码 //进入**mysql**数据库

4、 quit //退出**mysql**操作

5、mysqladmin -u用户名 -p旧密码 password 新密码 //更改密码

6、grant select on 数据库.* to 用户名@登录主机 identified by "密码" //增加新用户
exemple:
例2、增加一个用户test2密码为abc,让他只可以在localhost上登录，并可以对数据库 mydb进行查询、插入、修改、删除的操作（localhost指本地主机，即**MYSQL**数据库所在的那台主机），这样用户即**使用**知 道test2的密码，他也无法从internet上直接访问数据库，只能通过**MYSQL**主机上的web页来访问了。
grant select,insert,update,delete on mydb.* to test2@localhost identified by "abc";
如果你不想test2有密码，可以再打一个命令将密码消掉。
grant select,insert,update,delete on mydb.* to test2@localhost identified by "";

二、数据库操作
1、show databases; //列出数据库

2、use database_name //**使用**database_name数据库

3、create database data_name //创建名为data_name的数据库

4、drop database data_name //删除一个名为data_name的数据库

三、表操作
1、show tables //列出所有表
create talbe tab_name(
id int(10) not null auto_increment primary key,
name varchar(40),
pwd varchar(40)
) charset=gb2312; 创建一个名为tab_name的新表
2、drop table tab_name 删除名为tab_name的数据表
3、 describe tab_name //显示名为tab_name的表的数据结构
4、show columns from tab_name //同上
5、delete from tab_name //将表tab_name中的记录清空
6、select * from tab_name //显示表tab_name中的记录
7、mysqldump -uUSER -pPASSWORD --no-data DATABASE TABLE &gt; table.sql //复制表结构

四、修改表结构
1、 ALTER TABLE tab_name ADD PRIMARY KEY (col_name)
说明：更改表得的定义把某个栏位设为主键。
2、 ALTER TABLE tab_name DROP PRIMARY KEY (col_name)
说明：把主键的定义删除
3、 alter table tab_name add col_name varchar(20); //在tab_name表中增加一个名为col_name的字段且类型为varchar(20)
4、alter table tab_name drop col_name //在tab_name中将col_name字段删除
5、alter table tab_name modify col_name varchar(40) not null //修改字段属性，注若加上not null则要求原字段下没有数据
SQL Server200下的写法是：Alter Table table_name Alter Column col_name varchar(30) not null;
6、如何修改表名：alter table tab_name rename to new_tab_name
7、 如何修改字段名：alter table tab_name change old_col new_col varchar(40); //必须为当前字段指定数据类型等属性，否则不能修改
8、create table new_tab_name like old_tab_name //用一个已存在的表来建新表，但不包含旧表的数据

五、数据的备份与恢复
导入外部数据文本:
1\. 执行外部的sql脚本
当前数据库上执行:**mysql** &lt; input.sql
指定数据库上执行:**mysql** [表名] &lt; input.sql
2.数据传入命令 load data local infile "[文件名]" into table [表名];
备份数据库：(dos下)
mysqldump --opt school&gt;school.bbb
mysqldump -u [user] -p [password] databasename &gt; filename (备份)
**mysql** -u [user] -p [password] databasename &lt; filename (恢复)

以上是对 MySQL4.1进行操作时的一些常用命令。有的是标准SQL，有的则只能在**MySQL**中**使用**， 要进行区分。

卸载mysql:sudo apt-get remove mysql-server mysql-client
sudo apt-get autoremove

</div>
</div>