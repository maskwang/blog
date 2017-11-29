---
title: php在shell模式定时crontab生成html静态页
id: 351
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:51:36
tags:
---

<div id="blog_text">
<div>
<div>

大家知道，php一样可以通过shell模式运行

最近项目中需要批量生成大量的静态化html文件，于是用php来实现了该功能。优点是效率高，能够实现计划任务定时(crontab)。

文件就不放上来了，与普通的php文件差不多，区别是文件前面一般加一行：#!/usr/local/php/bin/php，还有，文件路径最好 是绝对路径。

php命令有几个参数 -q是安静模式，在这里可能会用到。

计划任务的设定：

crontab -e

加入下面一行，每10分钟执行一次create.sh

*/10 * * * * cd /path;./send.sh

create.sh的内容：

set fdate=`date +%Y%m%d`
set time=`date +%T`
echo "[$time]:&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;" &gt;&gt; "log/$fdate.log"
php create.php &gt;&gt; "log/$fdate.log"
echo "&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;:[$time]" &gt;&gt; "log/$fdate.log"

</div>
</div>
</div>