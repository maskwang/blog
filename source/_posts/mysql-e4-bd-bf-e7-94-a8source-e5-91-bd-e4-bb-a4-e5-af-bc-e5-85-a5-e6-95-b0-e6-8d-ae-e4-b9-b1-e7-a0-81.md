---
title: mysql使用source命令导入数据 乱码
id: 398
categories:
  - Mysql
  - 技术专栏
date: 2011-10-13 04:34:32
tags:
---

1，数据库备份命令
mysqldump -uroot -p --default-character-set=gbk dbname &gt; /root/newsdata.sql

2，导入数据库
mysql -uroot -p --default-character-set=gbk
use dbname
source /root/newsdata.sql