---
title: x-webkit-speech 语音输入功能
id: 511
categories:
  - Html
  - web前端
  - 技术专栏
date: 2012-02-17 09:57:53
tags:
---

        最近发现各大网站陆续出现了搜索框边上一个小话筒的语音输入功能。网上找了下一定要webkit内核浏览器才能使用，不过win下的safari不知道为什么没有显示，回头看下UA。

检测浏览器是否支持
<pre lang="javascript">
 if (document.createElement("input").webkitSpeech === undefined) {

      alert("Speech input is not supported in your browser.");

} 
</pre>

下面说下怎么实现： 

显示出来了吧，其实很简单。

<pre lang="html">
 <input type="text" x-webkit-speech /> 
</pre>

就一句，当然还有别的属性可以添加:

lang
设置语言种类： 
<pre lang="html">
 <input type="text" x-webkit-speech lang="zh-CN" />
</pre>

onwebkitspeechchange
语音输入事件，当发声语音改变时触发： 
<pre lang="html">
 <input type="text" x-webkit-speech onwebkitspeechchange="foo()" />

 function foo(){
   alert('changed');
 } 
</pre>

x-webkit-grammar
语音输入语法，”builtin:search”值使得语音输入的内容尽量靠近搜索内容，去除多余的字符，例如「的」 
<pre lang="html">
<input type="text" x-webkit-speech x-webkit-grammar="builtin:search" /> 
</pre>

PS : 如果原input中value不为空，输入会直接添加在原有文字后面。既然用webkit就要用placeholder了，不要再使用value为作输入提示了。