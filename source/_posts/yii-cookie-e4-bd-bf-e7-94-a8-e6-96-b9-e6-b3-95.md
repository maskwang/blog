---
title: Yii cookie使用方法
id: 33
categories:
  - Yii
  - 技术专栏
date: 2011-08-21 05:46:38
tags:
---

<div id="blog_text">
<div>

设置cookie:
<div>$cookie = new CHttpCookie(‘mycookie’,'this is my cookie’);$cookie-&gt;expire = time()+60*60*24*30;  //有限期30天Yii::app()-&amp; gt;request-&gt;cookies['mycookie']=$cookie;</div>
读取cookie:
<div>$cookie = Yii::app()-&gt;request-&gt;getCookies();echo $cookie['mycookie']-&gt;value;</div>
销毁cookie:
<div>$cookie = Yii::app()-&gt;request-&gt;getCookies();unset($cookie[$name]);</div>
</div>
</div>