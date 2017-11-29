---
title: 在CentOS下安装g++
id: 495
categories:
  - Linux
  - 技术专栏
date: 2012-02-14 16:09:05
tags:
---

在CentOS下安装编译时提示需要g++,若没有安装时,得先安装一下才能继续下面的步骤;
若在debian下直接apt-get install g++就可以了。
按照我以前的想法，在CentOS下:yum install g++ 出错报告说无法找到g++包。查了资料一下，原来这个包的名字叫做gcc-c++。

所以正确的命令应该是 : yum install gcc-c++.