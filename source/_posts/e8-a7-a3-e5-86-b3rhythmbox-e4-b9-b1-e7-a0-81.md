---
title: 解决Rhythmbox乱码
id: 64
categories:
  - Linux
  - 技术专栏
date: 2011-08-22 22:34:53
tags:
---

<div id="blog_text">
<div>

今天在ubuntu论坛看到怎样解决困惑我已久Rhythmbox乱码，下面是 解决办法：
首先，需要有软件包mid3iconv。如果你的系统中没有安装它，可以通过如下代码自动安装：sudo apt-get install python-mutagen
然后转到你的MP3目录，执行以全命令进行转换：mid3iconv -e GBK *.mp3
如果需要包含子目录，可以将后缀改成如下格式：打命令的时候文件名字给 “*/*.mp3″ 就行了。比如mid3iconv -e GBK */*.mp3
最后，重新导入一次rhythmbox就OK了。解决Rhythmbox乱码

另：也可以批量进行：find . \( -iname “*.mp3″ -o -iname “*.wma” \) -exec mid3iconv -e gbk ‘{}’ \;

</div>
</div>