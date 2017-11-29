---
title: ' php中serialize序列化缺陷'
id: 575
categories:
  - Php
  - 技术专栏
  - 编程语言
date: 2012-06-18 11:17:56
tags:
---

<pre lang="php">
//序列化一个数组：
serialize(array("asdoasod\'asdasd", "asdaspdaso\\\\\\pdopasopd"));
//返回结果：
a:2:{i:0;s:16:"asdoasod\'asdasd";i:1;s:22:"asdaspdaso\\\pdopasopd";}

//我们一般存进数据库，带\号直接存进数据库会有一个问题，会出现自动去除'\'
//假如去除了'\'
//s:16:  这个16代表长度
//再从数据库中取出来数据，s:16的长度将会变短，这个时候：

unserialize(); //就会出现问题！
</pre>