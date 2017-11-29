---
title: linux mysql sh自动备份
id: 349
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:51:16
tags:
---

<div id="blog_text">

#!/bin/sh
# File: autoback.sh
# Database info
DB_NAME="数据库"
DB_USER=" 数据库用户名"
DB_PASS="数据库密码"

# Others vars
BIN_DIR="mysql安装目录"
BCK_DIR="备份目录"
DATE=`date +%Y%m%d`

# TODO
$BIN_DIR/mysqldump --opt -u$DB_USER -p$DB_PASS $DB_NAME | gzip &gt; $BCK_DIR/$DB_NAME_$DATE.gz

&nbsp;

保存退出，然后把这个文件赋予可执行的权限：
#chmod 777 mysqlautobackup.sh

然后编辑crontab：
#vi /etc/crontab
在最 后一行加入以下内容：
01 5 * * * root /www/mysqlautobackup.sh
然后重启一下crontab：
# /etc/rc.d/init.d/crond restart
这样就搞定了，以后每天临晨的5点就会自动执行一次mysql自动备份的命令。

</div>