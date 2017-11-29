---
title: 如何使用代理服务器访问Facebook、YouTube
id: 467
categories:
  - 其它
  - 技术专栏
date: 2012-01-31 14:48:56
tags:
---

SOCKS代理是目前功能最为全面，使用最为稳定的代理服务器，目前用SSH搭建SOCKS代理服务器上网，访问网络没有任何限制。下面重讲一下如何使用SOCKS代理服务器。

用SSH搭建SOCKS代理上网，建议使用Firefox浏览器，因为Firefox支持SOCKS代理远程域名解析，而IE只能通过类似SocksCap这样的第三方软件实现，不是很方便。

配置Firefox浏览器
在Firefox设置SOCKS远程域名解析，主要是为了防止DNS污染，具体设置方法是，在Firefox地址栏中，输入 about:config ，按确认，修改里面的一项数值，改成 network.proxy.socks_remote_dns=true 就可以了。

然后，打开FireFox浏览器，选择菜单栏的“工具/选项...”。选择“高级/网络”，点设置，就出现下面的界面，就可以进行代理服务器的设置了，选中“手动配置代理”，然后在SOCKS主机上，填写代理服务器的地址127.0.0.1，端口1080，SOCKS类型选择“SOCKS V5”，这时Firefox就配置结束。

设置SSH
配置好了Firefox，就该配置SSH了，安全外壳协议（Secure Shell Protocol / SSH）是一种在不安全网络上提供安全远程登录及其它安全网络服务的协议。常用的SSH工具有开源软件PuTTY，支持SSH远程登录的主机可以实现socks5代理服务器的功能，不过在PuTTY中没有配置文件，需要手动设置才能实现，且无法保存，而PuTTY完整版自带的pLink可以实现命令行方式调用PuTTY实现SSH的加密通道。

具体的方法是，去PuTTY官方网站下载pLink这个文件&gt;&gt;点击这里下载，pLink的调用参数是：plink -C -v -N -pw 密码 -D 本地端口 远程用户@IP或域名:远程希望打开的端口。

新建一个文件，写入以下内容，另存为pLink.bat批处理文件，并放在Putty的安装目录内。

plink -N Username@sshServer -pw Password -D 127.0.0.1:1080

请将Username sshServer Password三处改为用户自己登陆SSH服务器的用户名、服务器地址和密码。这个SSH帐号可以通过多种方法获得，例如用户购买了某些国外主机空间或VPS就会有SSH帐号，或者在淘宝网也有SSH帐号出售，通常SSH帐号的价格大约是每年几十元人民币左右，当然，也有提供免费的SSH帐号&gt;&gt;点击这里了解

执行这个批处理文件，保持其窗口开启，一旦关闭窗口代理便失效。然后打开已经配置好127.0.0.1:1080的Socks5代理的Firefox浏览器，就可以使用SOCKS代理服务器上网了。

其他设置技巧
为了方便代理服务器的快速切换，建议安装一个名为QuickProxy的FireFox代理服务器扩展，可以实现一键切换代理功能，QuickProxy安装后在状态栏有一个按钮，点击后可以启用、关闭Firefox浏览器的默认代理设置，可以快速在代理和非代理之间切换，很方便。界面如下图所示。

设置完成了之后，你就可以自由自在地在开放的互联网上傲游了。基于SSH的SOCKS代理稳定、快速、功能全面，是值得推荐的代理方法，使用过程中流量需要自己把控，其浏览体验要远远高于其他代理软件。