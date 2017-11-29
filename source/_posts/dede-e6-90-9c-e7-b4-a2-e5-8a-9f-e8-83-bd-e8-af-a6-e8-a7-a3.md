---
title: dede 搜索功能详解
id: 119
categories:
  - Dedecms
  - 技术专栏
date: 2011-08-22 22:49:12
tags:
---

1站内<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>增加个仅<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>当前频道功能
其实自己有<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>框加一个栏目选择的项就行了，系统不必要自动去生成
高级<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>里可选的条件，你全都可以用
Quote:
&lt;select name="channeltype" id="channeltype" style="width:100"&gt;
&lt;option value="0" selected&gt;--不限--&lt;/option&gt;
&lt;option value='4'&gt;Flash&lt;/option&gt;
&lt;option value='3'&gt;软件&lt;/option&gt;
&lt;option value='2'&gt;图片集&lt;/option&gt;
&lt;option value='1'&gt;普通文章&lt;/option&gt;
&lt;/select&gt;

如果不想用户选择，你直接加
&lt;input type='hidden' name='channeltype' value="{dede:field name='channeltype'/}"&gt;
这样也行

2
最新5.1随便<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>一串数字出错
提示信息如下：
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server

version for the right syntax to use near ') limit 500' at line 1 - Execute Query False!

Select aid from dede_full_search where arcrank &gt; -1 and () limit 500
因<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>未过滤html标签，导致用户可以在<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>出注入html代码，该补丁修改该问题和utf-8版tag标签找不

到以及部分用户尾部丢0的问题

覆盖补丁后请在后台 内容维护 <span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>关键词管理 处删除不正常的关键词。
解决方法：官方已经出补丁了
下载补丁包下载地址(GBK/UTF8请按版本选择里面的文件)
[<span style="color: #2f5fa1;">http://www.dedecms.com/upimg/soft/2008/patch20080407.zip</span>](http://www.dedecms.com/upimg/soft/2008/patch20080407.zip)
非5.1版请修改 plus/search.php文件
把Copy code$keyword = ereg_replace("[\|\"\r\n\t%\*\?\(\)\$;,'%-]"," ",trim($keyword));
替换为Copy code$keyword = ereg_replace("[\|\"\r\n\t%\*\?\(\)\$;,'%&lt;&gt;]"," ",trim($keyword));

3

文章关键字自动对应<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>的办法
{dede:field name='keywords' runphp='yes' }
if(!empty(@me)){
$kws = explode(' ',@me);
@me = "";
foreach($kws as $k){
@me .= "&lt;a href='/cms/plus/search.php?keyword=".urlencode($k)."' &gt;$k&lt;/a&gt; ";
}
@me= str_replace('+', ' ',trim(@me));
}
{/dede:field}

列表页中的关键字自动连接对应办法,//
关键字： [field:keywords runphp='yes']
if(!empty(@me)){
$kws = explode(' ',@me);
@me = "";
foreach($kws as $k){
@me .= "&lt;a href='/cms/plus/search.php?keyword=".urlencode($k)."' &gt;$k&lt;/a&gt; ";
}
@me= str_replace('+', ' ',trim(@me));
}
[/field:keywords]

4
在文章列表页和<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>结果页调用来源
1 ，改 include 目录下的 inc_arclist_view.php

查找

$query = "Select arc.ID,arc.title,arc.iscommend,arc.color,
arc.typeid,arc.ismake,arc.money,arc.description,arc.shorttitle,
arc.memberid,arc.writer,arc.postnum,arc.lastpost,
arc.pubdate,arc.senddate,arc.arcrank,arc.click,arc.litpic,
tp.typedir,tp.typename,tp.isdefault,tp.defaultname,
tp.namerule,tp.namerule2,tp.ispart,tp.moresite,tp.siteurl
$addField

在 arc.writer, 后面加上 arc.source,
(感谢cms2009分享)

2,改inc_arcsearch_view.php

查找：#@__arctype.siteurl

添加,#@__archives.source
5
调用当天<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>关键字，并过滤色情关键字0
Copy code
{dede:loop table='dede_search_keywords' sort='lasttime' row='40' if='TO_DAYS(NOW())=TO_DAYS

(FROM_UNIXTIME(lasttime)) and keyword regexp "性|黄色|成人|色" =0'}
&lt;a href="/plus/search.php?keyword=[field:keyword/]"&gt;[field:keyword/]&lt;/a&gt;
{/dede:loop}

过滤的关键字可以自已加
这个应该明白是什么吧
sort='lasttime'
sort='count'

TO_DAYS 改成其它MYSQL时间函数还可调用一周内的关键字等，请自行修改！
【教程】实时更新的【热门关键字】！[<span style="color: #2f5fa1;">http://bbs.dedecms.com/read.php?tid=15818</span>](http://bbs.dedecms.com/read.php?tid=15818)

6
大大提高<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>准确度的方法
原来的模板的 请将
&lt;form action="{dede:field name='phpurl'/}/search.php" name="formsearch"&gt;
&lt;input type="hidden" name="kwtype" value="0"&gt;
改成
&lt;form action="{dede:field name='phpurl'/}/search.php" name="formsearch"&gt;
&lt;input type="hidden" name="kwtype" value="1"&gt;

即0改成1

采用“仅<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>标题”的<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>方式，
&lt;option value="title" selected&gt;搜索标题&lt;/option&gt; 可将这个设为默认，或者干脆删除下面的智能模

糊，那个太不准。
&lt;option value="titlekeyword"&gt;智能搜索&lt;/option&gt;

-----------------------------------
高级<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>中，可以将模板中的“与”改成默认
&lt;input type="radio" name="kwtype" value="1" checked="checked"/&gt;
与
&lt;input name="kwtype" type="radio" value="0" /&gt;
或

“与”应该就是value="1"的意思，下面的同样采用“仅<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>标题”的方式

总的说来就是“与”（value="1"）+“仅<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>标题”=准确

7
如何设置让<span style="text-decoration: underline;"><span style="color: #ff0000;">搜索</span></span>条可以搜一个汉字
需要修改2个地方
/plus/search.php

if($keyword==""||strlen($keyword)&lt;1){
ShowMsg("关键字不能小于1个字节！","-1");
exit();
}

/include/inc_arcsearch_view.php
codeif(strlen($k)&lt;2) continue;