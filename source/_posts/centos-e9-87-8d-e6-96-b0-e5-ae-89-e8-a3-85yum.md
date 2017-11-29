---
title: CentOS重新安装yum
id: 341
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:49:38
tags:
---

<div id="blog_text">

有的同学相当的牛逼，把系统的yum rmp神马的都给卸载或者弄坏了，幸亏还可以给系统重新安装yum

1，下载最新的yum-3.2.28.tar.gz并解压
wget http://yum.baseurl.org/download/3.2/yum-3.2.28.tar.gz tar xvf yum-3.2.28.tar.gz
2，进入目录，运行安装
cd yum-3.2.28
cp etc/yum.conf /etc/yum.conf
yummain.py install yum yum check-update
yum update
yum clean all
提醒下各位同学yum的remove不要随便乱玩，有时候会要命的！

</div>