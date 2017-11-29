---
title: MongoDB 安装与启动
id: 276
categories:
  - MongoDB
  - 技术专栏
  - 数据库
date: 2011-08-25 01:38:52
tags:
---

<div id="blog_text">
<div>主要介绍在[**Windows**](http://%20/;)与[**Linux**](http://%20/;)下的安装与启动</div>
<div>下载链接:http://www.mongodb.org/display/DOCS/Downloads</div>
<div>-----------------------------------------------------------------------------------</div>
<div>**Windows**</div>
<div></div>
<div>推荐下载版本1.4.3（Windows 32 bit）</div>
<div>下载链接：[http://downloads.mongodb.org/win32/mongodb-win32-i386-1.4.3.zip](http://downloads.mongodb.org/win32/mongodb-win32-i386-1.4.3.zip)</div>
<div>假设安装路径在E:\mongodb，设置Mongodb数据库的数据路径E:\data\mongodb</div>
<div>开启命令行(开始——运行)</div>
<div></div>
<div>输入以下命令：</div>
<div>1\. mongod命令建立一个mongodb数据库链接，数据存放路径为E:\data\mongodb</div>
<div>

E:

cd mongodb\bin

mongod.exe –port 11111 –dbpath E:\data\mongodb

2.链接已有的mongodb数据库

E:

cd mongodb\bin

mongo.exe 192.168.135.212:10001

&nbsp;

</div>
<div>-----------------------------------------------------------------------------------</div>
<div>**Linux**</div>
<div></div>
<div>推荐下载版本1.4.3</div>
<div>下载链接：[http://downloads.mongodb.org/linux/mongodb-linux-i686-1.4.3.tgz](http://downloads.mongodb.org/linux/mongodb-linux-i686-1.4.3.tgz)</div>
<div>输入以下命令：</div>
<div>解压：tar -zxvf mongodb-linux-i686-1.4.3.tgz</div>
<div>定位到mongodb/bin目录中 cd ../mongodb/bin</div>
<div></div>
<div>1\. mongod命令建立一个mongodb数据库链接，端口号为10001，数据存放路径为/data/mongodb/data，[**日志**](http://%20/;)存放路径为/data/mongodb/log</div>
<div>先建立相关目录及文件，使用mkdir 及touch</div>
<div>启动命令——</div>
<div>./mongodb/bin/mongod -port 10001 -dbpath /data/mongodb/data/m1 --logpath/data/mongodb/log/m1.log</div>
<div></div>
<div>2.链接已有的mongodb数据库</div>
<div>./mongodb/bin/mongo 192.168.135.212:10001</div>
<div></div>
<div>
<div>Windows 和 Linux 安装有些异同，但链接[**数据库**](http://%20/;)后，使用一样。</div>
<div>查询所有数据库show dbs</div>
<div>使用[**test**](http://%20/;)数据库use test</div>
<div>显示test所有表（collections）show collections</div>
<div>查询服务器状态db.serverStatus()</div>
</div>
</div>