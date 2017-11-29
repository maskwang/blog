---
title: PHP下编码转换函数mb_convert_encoding与iconv的使用说明
id: 58
categories:
  - Php
  - 技术专栏
date: 2011-08-22 22:32:23
tags:
---

mb_convert_encoding这个函数是用来转换编码的。原来一直对程序编码这一概念不理解，不过现在好像有点开窍了。
不过英文一般不会存在编码问题，只有中文数据才会有这个问题。比如你用Zend Studio或Editplus写程序时，用的是gbk编码，如果数据需要入数据库，而数据库的编码为utf8时，这时就要把数据进行编码转换，不然进到数据库就会变成乱码。

mb_convert_encoding的用法见官方：
http://cn.php.net/manual/zh/function.mb-convert-encoding.php

做一个GBK To UTF-8
&lt; ?php
header("content-Type: text/html; charset=Utf-8");
echo mb_convert_encoding("妳係我的友仔", "UTF-8", "GBK");
?&gt;

再来个GB2312 To Big5
&lt; ?php
header("content-Type: text/html; charset=big5");
echo mb_convert_encoding("你是我的朋友", "big5", "GB2312");
?&gt;
不过要使用上面的函数需要安装但是需要先enable mbstring 扩展库。

PHP中的另外一个函数iconv也是用来转换字符串编码的，与上函数功能相似。

下面还有一些详细的例子：
iconv — Convert string to requested character encoding
(PHP 4 &gt;= 4.0.5, PHP 5)
mb_convert_encoding — Convert character encoding
(PHP 4 &gt;= 4.0.6, PHP 5)

用法：
string mb_convert_encoding ( string str, string to_encoding [, mixed from_encoding] )
需要先enable mbstring 扩展库，在 php.ini里将; extension=php_mbstring.dll 前面的 ; 去掉
mb_convert_encoding 可以指定多种输入编码，它会根据内容自动识别,但是执行效率比iconv差太多；

string iconv ( string in_charset, string out_charset, string str )
注 意：第二个参数，除了可以指定要转化到的编码以外，还可以增加两个后缀：//TRANSLIT 和 //IGNORE，其中 //TRANSLIT 会自动将不能直接转化的字符变成一个或多个近似的字符，//IGNORE 会忽略掉不能转化的字符，而默认效果是从第一个非法字符截断。
Returns the converted string or FALSE on failure.

使用：

发现iconv在转换字符”—”到gb2312时会出错，如果没有ignore参数，所有该字符后面的字符串都无法被保存。不管怎么样，这个”—”都无法转换成功，无法输出。 另外mb_convert_encoding没有这个bug.

一般情况下用 iconv，只有当遇到无法确定原编码是何种编码，或者iconv转化后无法正常显示时才用mb_convert_encoding 函数.

from_encoding is specified by character code name before conversion. it can be array or string - comma separated enumerated list. If it is not specified, the internal encoding will be used.
/* Auto detect encoding from JIS, eucjp-win, sjis-win, then convert str to UCS-2LE */
$str = mb_convert_encoding($str, “UCS-2LE”, “JIS, eucjp-win, sjis-win”);
/* “auto” is expanded to “ASCII,JIS,UTF-8,EUC-JP,SJIS” */
$str = mb_convert_encoding($str, “EUC-JP”, “auto”);

例子：
$content = iconv(”GBK”, “UTF-8″, $content);
$content = mb_convert_encoding($content, “UTF-8″, “GBK”);

**PHP中使用mb_convert_encoding转码的小陷阱
**在 php程序中使用mb_convert_encoding()方法进行字符编码转换大家都很熟悉了，平时也在大量的使用。而且在一般情况下该方法也表现的 足够好，值得表扬。但在一个项目中我们需要使用它进行UTF8到GBK的转换，在转换一些特殊字符时发现了一个不大不小的问题。具体表现为mb把在 utf8可编码的字符而在gbk中不可编码的字符都转成了\0x00\0x80,这样就导致转换后的gbk字符是有问题的。
在我们的意识中，在进行字符编码转换的过程中，如果遇到目标编码不可表现的字符，转码程序应该做的是舍弃这种字符，这样虽然丢失了部分数据，但不会导致转码的字符序列不可用。不清楚mb为什么要使用上述方式而不是舍弃方式。
临时的解决方式是对转码后的字符串序列进行过滤，过滤掉所有\x00\80的字符；又或者在转义之前对utf8的字符串进行过滤，过滤掉ut8可表示而gbk不可表示的所有字符，从实现难度上来讲，第一种过滤方式比较容易做到。