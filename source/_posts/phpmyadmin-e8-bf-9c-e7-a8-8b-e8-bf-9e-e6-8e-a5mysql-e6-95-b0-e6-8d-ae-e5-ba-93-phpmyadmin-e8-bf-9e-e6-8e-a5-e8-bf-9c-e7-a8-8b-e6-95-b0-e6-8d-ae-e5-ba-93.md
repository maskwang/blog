---
title: phpmyadmin远程连接mysql数据库_phpmyadmin连接远程数据库
id: 70
categories:
  - Mysql
  - 技术专栏
date: 2011-08-22 22:36:29
tags:
---

<div id="blog_text">
<div>

<span style="color: #ff0000;">**前提：你本地有 PHP环境，如果你本地装有PHPMYADMIN，建议重新下载一个，随便另起一个文件夹名。**</span>

一、下载phpmyadmin到本地

下载地址：[http://www.crsky.com/soft/4190.html](http://www.crsky.com/soft/4190.html)

二、修改libraries文件夹下的config.default.php文件

1、查找<span style="color: #0000ff;">$cfg['PmaAbsoluteUri']</span> ，将其值设置为你本地的phpmyadmin路径，例如[<span style="color: #000000;">http://127.0.0.1:9999/phpmyadmin/</span>](http://127.0.0.1:9999/phpmyadmin/)

2、查找<span style="color: #0000ff;">$cfg['Servers'][$i]['host']</span> ， 将其值设置为你mysql数据库地址，例如125.24.112.19

3、查找<span style="color: #0000ff;">$cfg['Servers'][$i]['user']</span> ， 将其值设置为你mysql数据库用户名，例如abcd

4、查找<span style="color: #0000ff;">$cfg['Servers'][$i]['password']</span> ， 将其值设置为你mysql数据库密码，例如abcd

三、通过你本地的phpmyadmin路径（同第二步设置的路径），通过你的mysql数据库用户名密码即可访问远程数据库。

&nbsp;

以上方法本人亲测，如果不能解决你的问题，在此说声抱歉！！！

</div>
</div>