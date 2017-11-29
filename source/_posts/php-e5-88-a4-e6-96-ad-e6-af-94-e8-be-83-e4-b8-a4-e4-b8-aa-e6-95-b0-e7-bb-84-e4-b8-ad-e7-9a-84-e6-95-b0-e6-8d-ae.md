---
title: php判断比较两个数组中的数据
id: 60
categories:
  - Php
  - 技术专栏
date: 2011-08-22 22:33:21
tags:
---

<div id="blog_text">

$q = array(1,2,30,40,50);
$s = array(1,2,4,8,9,9,34,2,12,324,3,34,343);
$m=array_intersect($q,$s); //共同的部分
//老的数据
$old=array_diff($q,$m);
print_r($old);
//新的数据
$new=array_diff($s,$m);
print_r($new);

获取不一样的记过

</div>