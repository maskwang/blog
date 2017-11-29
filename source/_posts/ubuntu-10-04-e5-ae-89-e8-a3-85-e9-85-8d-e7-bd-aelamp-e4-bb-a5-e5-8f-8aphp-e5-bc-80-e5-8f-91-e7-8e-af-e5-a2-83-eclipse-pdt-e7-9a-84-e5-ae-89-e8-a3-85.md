---
title: Ubuntu 10.04 安装配置LAMP以及php开发环境 Eclipse PDT的安装
id: 82
categories:
  - Linux
  - 技术专栏
date: 2011-08-22 22:39:17
tags:
---

安装LAMP组件：

系统-&gt;系统管理-&gt;新立得软件包管理器-&gt;编辑-&gt;使用任务标记分组软件包 -&gt;LAMP Server(打勾选定)-&gt;确定-&gt;最后“应用”完成安装。期间会要求输入Root账户的密码并记住。

在 Firefox输 入地址http://localhost/或者http://127.0.0.1/，显示 It works! 即表明LAMP组件包已经正常安装了（比在Windows下方便不少）。

修改文件夹读写权限

PHP网络服务器根目录默认 位置：/var/www，默认属性只允许root用户执行操作，但是在Ubuntu中因为安全性的考虑默认关闭了 root账户。为了可以在这个文件夹新建修改php、html文件等等，可以通过终端命令行修改文件夹的这个属性：
sudo chmod -R 777 /var/www

安装phpmyadmin等进行Mysql数据库管理

phpmyadmin是最为常用的 Mysql数据库管理软件，本身由php写成，拥有和谐的web界面。你可以进入 软件中心或者新立得软件包管理器中搜索安装这个软件。

安 装过程中需要选择Web Server，选择apache2后前进，之后会需要输入之前设置的MySQL数据库连接密码。

将 phpmyadmin和apache2建立连接并测试，终端输入命令：
sudo ln -s /usr/share/phpmyadmin /var/www

完成后打开浏览器，访问http://localhost/phpmyadmin，出现欢迎界面，用户名使用root， 密码是刚才设置的那个。

安装Eclipse和PDT

1.安装Java运行环境Jre/Jdk：Ubuntu软件中心搜索 “JDK”就可以安装；

2.安装Eclipse和PDT等插件：Eclipse官网上有打包好的Eclipse for PHP Developers，很直接下载解压缩就可以使用；

3.在“应用程序”里添加Eclipse快捷方式，终端命令：
sudo gedit /usr/share/applications/eclipse.desktop

gedit编辑，添加如下内容：
[Desktop Entry]
Encoding=UTF-8
Name=Eclipse
Comment=Eclipse IDE
Exec=/home/banux/eclipse/eclipse
Icon=/home/banux/eclipse/icon.xpm
Terminal=false
StartupNotify=true
Type=Application
Categories=Application;Development;

其中“Exec=”和“Icon=”后面内容修改为eclipse解压的相应位置；

4.建议将Eclipse工作区位 置设置为“/var/www”，这样在eclipse内就可以正常进行php开发了。

在Eclipse中新建PHP工程，测试

新 建php文件，输入一些php语句例如：
&lt;?php
echo "LAMP is ok now";
phpinfo();
?&gt;

保 存，Run，运行出现结果一切应该都是OK的了。