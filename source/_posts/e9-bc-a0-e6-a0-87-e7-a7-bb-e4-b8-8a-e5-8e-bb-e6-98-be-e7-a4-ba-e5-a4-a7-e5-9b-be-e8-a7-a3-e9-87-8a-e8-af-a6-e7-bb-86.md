---
title: '鼠标移上去显示大图{解释详细}'
id: 194
categories:
  - Css
date: 2011-08-22 23:32:14
tags:
---

<div>

&lt;style type="text/css"&gt;
&lt;!--
.pr { position:relative; }
.pr a { border:0px solid gray; padding:3px; width:200px; height:153px; font-size:12px; text-decoration:none; background:url("no.gif"); position:absolute; left:0; top:0; z-index:2; text-indent:-9999px; }----------------这里不用管他，是在屏幕外面的图层-------------
.pr a:hover { width:10px; height:454px---------大图高度-----------; padding:10px 617px---------大图宽度-------- 0 10px; text-indent:0; background:#fff url("big.jpg") no-repeat 25px------大图左顶点坐标------ 3px; }
.pr img { border:0px solid gray; padding:3px; width:150px; position:relative; z-index:1;}
--&gt;
&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div&gt;
&lt;img src="small.jpg" alt="big" /&gt;
&lt;a href="[http://192.168.1.249/default.asp](http://192.168.1.249/default.asp)"&gt;效果一&lt;/a&gt;

&nbsp;

:::::::::::::::::::::::::::::以下为代码部分:::::::::::::::::::::::::::::::::::::::::::::::::

&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "[http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd](http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd)"&gt;
&lt;html xmlns="[http://www.w3.org/1999/xhtml](http://www.w3.org/1999/xhtml)"&gt;
&lt;head&gt;
&lt;meta http-equiv="Content-Type" content="text/html; charset=gb2312" /&gt;
&lt;title&gt;鼠标移上去显示大图&lt;/title&gt;
&lt;style type="text/css"&gt;
&lt;!--
.pr { position:relative; }
.pr a { border:0px solid gray; padding:3px; width:200px; height:153px; font-size:12px; text-decoration:none; background:url("no.gif"); position:absolute; left:0; top:0; z-index:2; text-indent:-9999px; }
.pr a:hover { width:10px; height:454px; padding:10px 617px 0 10px; text-indent:0; background:#fff url("big.jpg") no-repeat 25px 3px; }
.pr img { border:0px solid gray; padding:3px; width:150px; position:relative; z-index:1;}
--&gt;
&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div&gt;
&lt;img src="small.jpg" alt="big" /&gt;
&lt;a href="[http://192.168.1.249/default.asp](http://192.168.1.249/default.asp)"&gt;效果一&lt;/a&gt;
&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;

</div>