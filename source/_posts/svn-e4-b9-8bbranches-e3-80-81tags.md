---
title: svn之branches、tags
id: 285
categories:
  - Svn
  - 技术专栏
  - 版本控制
date: 2011-08-26 01:24:55
tags:
---

直以来用svn只是当作cvs，也从来没有仔细看过文档，直到今天用到，才去翻看svn book文档，惭愧

需求一：
有一个客户想对产品做定制，但是我们并不想修改原有的svn中trunk的代码。

方法：
用svn建立一个新的branches，从这个branche做为一个新的起点来开发
<div id="">

1.  svn copy svn://server/trunk svn://server/branches/ep -m "init ep"
</div>
<pre title="svn之branches、tags 小介">svn copy svn://server/trunk svn://server/branches/ep -m "init ep"</pre>
Tip:
如果你的svn中以前没有branches这个的目录，只有trunk这个，你可以用
<div id="">

1.  svn mkdir branches
</div>
<pre title="svn之branches、tags 小介">svn mkdir branches</pre>
新建个目录

需求二：
产品开发已经基本完成，并且通过很严格的测试，这时候我们就想发布给客户使用，发布我们的1.0版本
<div id="">

1.  svn copy svn://server/trunk svn://server/tags/release-1.0 -m "1.0 released"
</div>
<pre title="svn之branches、tags 小介">svn copy svn://server/trunk svn://server/tags/release-1.0 -m "1.0 released"</pre>
咦，这个和branches有什么区别，好像啥区别也没有？
是的，branches和tags是一样的，都是目录，只是我们不会对这个release-1.0的tag做修改了，不再提交了，如果提交那么就是branches

需求三：
有一天，突然在trunk下的core中发现一个致命的bug,那么所有的branches一定也一样了，该怎么办？
<div id="">

1.  svn -r 148:149 merge svn://server/trunk branches/ep
</div>
<pre title="svn之branches、tags 小介">svn -r 148:149 merge svn://server/trunk branches/ep</pre>
其中148和149是两次修改的版本号。

其他的呢？看文档