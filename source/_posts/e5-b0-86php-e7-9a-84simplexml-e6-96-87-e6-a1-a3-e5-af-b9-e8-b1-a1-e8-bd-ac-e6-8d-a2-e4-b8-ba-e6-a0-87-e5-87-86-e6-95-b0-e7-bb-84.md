---
title: '将PHP的SimpleXML文档对象转换为标准数组 '
id: 325
categories:
  - Php
  - 技术专栏
  - 编程语言
date: 2011-09-20 23:34:09
tags:
---

function simplexml_obj2array($obj) {
if( count($obj) &gt;= 1 )
{
$result = $keys = array();

foreach( $obj as $key=&gt;$value)
{
isset($keys[$key]) ? ($keys[$key] += 1) : ($keys[$key] = 1);

if( $keys[$key] == 1 )
{
$result[$key] = simplexml_obj2array($value);
}
elseif( $keys[$key] == 2 )
{
$result[$key] = array($result[$key], simplexml_obj2array($value));
}
else if( $keys[$key] &gt; 2 )
{
$result[$key][] = simplexml_obj2array($value);
}
}
return $result;
}
else if( count($obj) == 0 )
{
return (string)$obj;
}
}