---
title: mariadb和mysql忘记密码怎么办
id: 750
categories:
  - Mysql
  - 数据库
date: 2014-06-26 17:52:51
tags:
---

linux环境中：/etc/my.cnf

在[mysqld]配置段添加如下一行： 
skip-grant-tables

保存退出编辑。
use mysql; 
update user set password=PASSWORD("123456") where user='root';

flush privileges

密码已经改好了。