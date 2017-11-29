---
title: win7系统自带无线共享WIFI
tags:
  - wifi
  - win7
id: 566
categories:
  - 技术专栏
date: 2012-05-13 22:20:57
---

当下各种移动终端，自然wifi成了必不可少的，然好多通过安装额外软件实现的共享wifi， 拖累机器运行速度不少，常见者不是广告就是收费，甚是烦人。
其实系统自带实现本机无线为wifi的功能。且听小人一一道来。
废话不多说，正文：<!--more-->
1运行-cmd接着输入
netsh wlan set hostednetwork mode=allow ssid=(这里填写你要广播的id名称) key=(这里填写设置wifi密码)，
注意空格，回车
2.接着启动wifi， 键入： netsh wlan start hostednetwork

3\. 接着把你电脑链接上internet的网络，如本地链接的网卡设置一下共享就可以直接使用了。

##########################
启动wifi脚本：
netsh wlan start hostednetwork 保存为“启动WIFI热点”.bat加入到启动文件夹，这样就开机启动，不用管了。

########################
查看wifi的状态脚本：
netsh wlan show hostednetwork
echo. &amp; pause 保存为 “查看WIFI信息.bat”
好啦， 不废话了，大功告成~

##update 2012-5-13
在日常的使用过程中，由于莫名的原因，时常能连接上wifi，却不能上网……百度后说各种说法， 最终找到解决方法
在cmd后键入 netsh winsock reset