---
title: Share一个翻墙工具
id: 661
categories:
  - 技术专栏
date: 2012-11-08 18:01:24
tags:
---

原理是注册一个Google Appengine，把goagent服务器上传到google appengine上运行；然后启动本地的goagent客户端，他会作为本地http代理服务器运行，把请求转发到google appengine实现翻墙。

&nbsp;

[http://code.google.com/p/goagent/](http://code.google.com/p/goagent/)

<!--more-->

操作如下1-4后，启动/local/gogent.exe（第一次以administrator身份运行），在浏览器中设HTTP代理服务器为127.0.0.1:8087即可翻墙。

**简易教程**

*   如何部署和使用goagent，以Windows为例

    1.  申请Google Appengine并创建appid。
    2.  下载goagent稳定版 [http://code.google.com/p/goagent/](http://code.google.com/p/goagent/)
    3.  修改local\proxy.ini中的<tt>[gae]</tt>下的appid=你的appid(多appid请用|隔开)
    4.  双击server\uploader.bat(Mac/Linux上传方法请见FAQ)，上传成功后即可使用了(地址127.0.0.1:8087)
    5.  chrome请安装[SwitchySharp](https://chrome.google.com/webstore/detail/dpplabbmogkhghncfbfdeeokoefdjegm)插件，然后导入这个设置[http://goagent.googlecode.com/files/SwitchyOptions.bak](http://goagent.googlecode.com/files/SwitchyOptions.bak)
    6.  firefox请安装[FoxyProxy](https://addons.mozilla.org/zh-cn/firefox/addon/foxyproxy-standard/)，Firefox需要导入证书，方法请见FAQ
&nbsp;