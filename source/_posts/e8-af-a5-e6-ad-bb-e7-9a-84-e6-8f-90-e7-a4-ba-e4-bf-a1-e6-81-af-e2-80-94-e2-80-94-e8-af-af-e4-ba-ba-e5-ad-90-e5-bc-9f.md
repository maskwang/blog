---
title: '该死的提示信息——误人子弟 '
id: 455
categories:
  - Linux
  - 技术专栏
date: 2011-11-23 11:57:01
tags:
---

<div id="blog_text">

友好的错误提示可以提升软件产品的竞争力，准确无误的日志输出可以帮助我们迅速定位错误来源、位置，做为一名合格的程序员，知道这一点非常的重要。下面列举一例，希望对你以后的程序开发有所帮助。

近期，本人博客系统出现的问题，点页面上的“分类目录”，显示该分类目录下没有找到文章，这让人感到十分的费解，因为之前都从来没有出现过这种情况，而且每篇文章都有自己的分类。

首先怀疑是博客程序出现了问题，我对分类目录的界面生成、查询部分的代码进行核对，没有发现问题，而且这一块近期根本没有改动过。

马上进入博客管理后台，进入“分类目录”管理，竟然发现以前建立的分类目录全部都没有了，尝试再新增一个分类，界面显示增加成功，再次刷新页面，刚 刚新增的分类又丢失了。退出管理界面，回到前台，再看，分类目录中一个条目都没有了，所有文章均显示未分类，此时，感觉身上有点冒冷汗了！呵呵。

登录putty，进入mysql管理终端，查看博客数据库系统，居然发现category分类目录的表都没有了，还有一些其它表都看不到了，难道是被“黑”了？

尝试重新启动mysql服务，命令：/etc/init.d/mysql restart，显示无法重启动，错误信息如下：

**Rather than invoking init scripts through /etc/init.d, use the service(8)  utility, e.g. service mysql restart **
**Since the script you are attempting to invoke has been converted to an  Upstart job, you may also use the restart(8) utility, e.g. start mysql **
**start: Job failed to restart **

把错误google了一上，都说按命令提示操作就可以了。马上用restart mysql命令，但是显示start: Job failed to start 错误，接着用service mysql restart命令，但又报no instance错误，看来这条路走不通了。

我开始怀疑系统更新过而没有重启，reboot了一下，问题依旧，mysql服务已经完全无法启动。但是服务器上的其它系统都在使用mysql服务，不解决的话会影响到其它系统。
陷入“僵局”了！以前mysql都可以用mysql restart命令正常启动，现在为怎么不行了呢？有一种可能是liunx系统内核完成了升级，但是mysql自身没有升级，造成mysql与新系统不兼 容？马上用apt-get update, apt-get dist-upgrade, apt-get-upgrade更新系统，最后这条命令在运行时说mysql升级失败，并提示/var/lib/mysql目录空间不够，看来这才是问题 的根源所在！希望是，呵呵。

马上du –ssh / ，发现磁盘空间已被耗尽，汗！

原因找到了，立即进行服务器文件清理，删除不用的文件以及老的备份文件，再apt-get-upgrade，成功，mysql又运行起来了，访问博客，恢复已经正常了。

可见，准确的错误提示信息对软件系统的维护是多么的重要，请各位程序员们切记！

</div>