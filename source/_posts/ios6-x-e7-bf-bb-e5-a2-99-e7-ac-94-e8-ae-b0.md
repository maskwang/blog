---
title: IOS6.x翻墙笔记
id: 689
categories:
  - 技术专栏
date: 2013-02-06 10:43:07
tags:
---

  这两天，IOS6.x的越狱版本已经放出，我这颗骚动的心于是又开始跃跃欲试了，网上教程很多，地址：http://www.chinaz.com/mobile/2013/0205/291802.shtml， 不再赘述。
  今天的主题是翻墙，俗话说：墙外风光无限好~ 

步骤如下：<!--more-->
   1\. 卸载掉之前的goagent-local和python2.7.1，还有CA证书也一并卸载掉。（你懂的，乱七八糟的东西混在一起，谁也不保证各种诡异的事情发生）
   2\. 下载两个安装包（切记：勿要在Cydia下载）
      https://code.google.com/p/yangapp/downloads/detail?name=python_2.7.3-3_iphoneos-arm.deb

https://goagent.googlecode.com/files/org.goagent.local.ios_0.0.3-1_iphoneos-arm.deb
  以上安装好后， 用ifile修改proxy.ini填入appid，profile = google_hk(切记改为hk)
   3\. 然后在运行桌面图标（goagent for iOS）安装证书并启动
   4\. 在Wifi里自动代理“http://127.0.0.1:8089/goagent.pac”。翻墙成功！