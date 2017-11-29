---
title: Ubuntu Linux下安装PHP开发环境Zend Studio 5.5.1
id: 76
categories:
  - Linux
  - 技术专栏
date: 2011-08-22 22:38:01
tags:
---

<div id="blog_text">
<div>

Ubuntu娱乐功能已经非常不错，无非是看看电 影，听听歌，玩玩小游戏。接下来研究下Ubuntu下安装zend，google一下还真是有Zend for Linux，于是下载了下来，下面就将我的安装配置过程图文并茂地展示给大家：

第 一步当然是解压zip，解压出来一个 bin 格式的安装文件， cp到我的用户目录home/ibm中，在终端地直接输入./ZendStudio-5_5_1.bin，安装便自动安装（需要jre支持，我已经提前安 装过了，安装命令是：sudo apt-get install sun-java6-jre sun-java6-jdk），过不了多久，便开始了界面安装，见图：

&nbsp;
<div>[![](http://hiphotos.baidu.com/wangfengkun/pic/item/fbedab64902114cef73654c8.jpg)](http://www.okajax.com/uploads/allimg/081204/0722260.png)</div>
&nbsp;

和Windows下的安装界面一样，一路NEXT 即可。

请注意，在安装过程中可能会出现：

awk: error while loading shared libraries: libdl.so.2: cannot open shared object file: No such file or directory
dirname: error while loading shared libraries: libc.so.6: cannot open shared object file: No such file or directory
/bin/ls: error while loading shared libraries: librt.so.1: cannot open shared object file: No such file or directory
basename: error while loading shared libraries: libc.so.6: cannot open shared object file: No such file or directory
dirname: error while loading shared libraries: libc.so.6: cannot open shared object file: No such file or directory
basename: error while loading shared libraries: libc.so.6: cannot open shared object file: No such file or directory
hostname: error while loading shared libraries: libc.so.6: cannot open shared object file: No such file or directory

Launching installer...

grep: error while loading shared libraries: libc.so.6: cannot open shared object file: No such file or directory
/tmp/install.dir.17548/Linux/resource/jre/bin/java: error while loading shared libraries: libpthread.so.0: cannot open shared object file: No such file or directory

这可能是因为你安装的java与系统不兼容的结 果，你可以这样来解决在你的中端
<pre>`$ cp ZendSafeGuard-4_0_0.bin ZendSafeGuard-4_0_0.bin.bak 
 $ cat ZendSafeGuard-4_0_0.bin.bak | \ 
       sed "s/export LD_ASSUME_KERNEL/#xport LD_ASSUME_KERNEL/" &gt; \ 
       ZendSafeGuard-4_0_0.bin`
   答案来源http://hi.baidu.com/i0n_p/blog/item/bb855909659d1eae2fddd473.html</pre>
安装完毕后，打开软件， 查看了下软件信息，发现软件许可到期时间是2008年12月3日，也就是一个月时间的试用期；在编码区输入汉字，发现全是口口（乱码），见图：

&nbsp;

&nbsp;
<div>[![](http://hiphotos.baidu.com/wangfengkun/pic/item/269759eeef0339c5b2fb95c9.jpg)](http://www.okajax.com/uploads/allimg/081204/0722261.png)</div>
&nbsp;

不急，一个一个解决它！首先要破解软件许可日期， 在网上找到了一个在线生成zend studio序列号的网站，网址：[http://www.zendstudio.net/libs/zendstudio5_5_1-keymaker-php/](http://www.zendstudio.net/libs/zendstudio5_5_1-keymaker-php/)

然后就是解决中文乱码问题，网上有一个不错的解决 办法：

1\. 创建文件夹fallback

mkdir zend安装目录/ZendStudio-5.5.1/jre/lib/fonts/fallback

2\. 复制字体simsun.ttc（WINDOWS系统的Fonts目录下面有这个字体） 到fallback后名字为simsun.ttf<span>程序开发，操作系统，服务器，源码下 载，Linux,Unix,BSD,PHP,Apach,asp,下载,源码,黑客,安全,技术社区,技术论坛5x8t&amp;C;I:X#g0d)m</span>
<span>程序开发，操作系统，服务器，源 码下载，Linux,Unix,BSD,PHP,Apach,asp,下载,源码,黑客,安全,技术社区,技术论坛 'U0F#q2l6X(p#i&amp;A</span>
cp simsun.ttc zend安装目录/ZendStudio-5.5.1/jre/lib/fonts/fallback/simsun.ttf<span>TechWeb-技术社 区!b%u$?/X.n.\3[.W-p</span>
<span>tech.techweb.com.cn5[*L#A2{;s)M9o%O</span>
从新启动Zend，乱码问题解决了，见图：

&nbsp;
<div>[![](http://hiphotos.baidu.com/wangfengkun/pic/item/3d6d55fb66952f286d22ebc9.jpg)](http://www.okajax.com/uploads/allimg/081204/0722262.png)</div>
OK，Zend Studio安装好了，开始工作！

</div>
</div>