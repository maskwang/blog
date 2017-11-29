---
title: 解决IE FF下图片链接边框兼容
id: 111
categories:
  - Html
  - 技术专栏
date: 2011-08-22 22:46:57
tags:
---

解决IE FF下图片链接边框兼容

css:

.marq a{text-align:center;margin:3px;&gt;width:1px;&gt;border:1px solid #fff;&gt;padding:3px}
.marq a:hover{&gt;border:1px solid #ccc;&gt;width:1px;}
.marq a img{vertical-align:middle;border:1px solid #fff!important;;padding:3px!important;}
.marq a img:hover{border:1px solid #ccc!important;}

html:

&lt;div&gt;
&lt;a href="#"&gt;&lt;img src="images/default.gif" /&gt;&lt;/a&gt;
&lt;a href="#"&gt;&lt;img src="images/default.gif" /&gt;&lt;/a&gt;
&lt;a href="#"&gt;&lt;img src="images/default.gif" /&gt;&lt;/a&gt;
&lt;a href="#"&gt;&lt;img src="images/bodybg.jpg" /&gt;&lt;/a&gt;
&lt;/div&gt;