---
title: var_export 与 var_dump的不同
tags:
  - php
id: 638
categories:
  - Php
date: 2012-09-06 14:55:22
---

### 问题发现

在跟踪yratings_get_targets的时候，
<pre id="">   error_log(var_export(yblog_mspconfiginit("ratings"),true));</pre>
老是打印出yblog_mspconfiginit(“ratings”)的返回是NULL

导致我以为是无法建立和DB的连接，走错路了一天。

最后才发现，这是var_export和var_dump的区别之一

这就是：<!--more-->

### 问题原因

var_export必须返回合法的php代码， 也就是说，var_export返回的代码，可以直接当作php代码赋值个一个变量。 而这个变量就会取得和被var_export一样的类型的值

*   但是， 当变量类型为resource的时候， 是无法简单copy复制的，所以， 当var_export的变量是resource类型时， var_export会返回NULL

### 实例

<pre>$res = yblog_mspconfiginit("ratings");
var_dump($res);
var_export($res);</pre>
结果:
<pre>resource(1) of type (yahoo_yblog)
NULL</pre>
再比如:
<pre id="">$res = fopen('status.html', 'r');
var_dump($res);
var_export($res);</pre>

结果:

<pre>resource(2) of type (stream)
NULL</pre>
附录： 经本人测试，file_put_contents的输入数据不像文档说的不支持多维数组，已支持。

版权信息

注： 本文章转自鸟哥blog文章

地址：[http://www.laruence.com/2008/04/03/15.html](http://www.laruence.com/2008/04/03/15.html)