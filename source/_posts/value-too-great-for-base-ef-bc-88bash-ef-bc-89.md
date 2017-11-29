---
title: 'value too great for base （bash）   '
id: 448
categories:
  - Linux
  - 技术专栏
date: 2011-11-10 12:59:16
tags:
---

<div>

在bash中，假设定义一个变量

var＝08

当使用var在数值计算时

var_1=$((${var}-7))
bash一般会报错，原因在于在算术计算式$((...))内，bash读到了变量var，而var的字符串值是08，由于是“0”开头，所以bash会把var当成是八进制数，而八进制数最大的基数（base number）是7，所以08在八进制里面是违法的写法。

所以要想顺利地计算，我们必须要么去除var之前的“0”，要么想办法告诉bash，var是十进制数。

方法一：（清除前导0）

items="08"
items=`echo $items|sed 's/^0*//'`
echo $items

方法二：（告诉bash，这是十进制数）

$((10#${var}))

在变量名前的”10#“就是告诉bash，这是十进制数。

</div>