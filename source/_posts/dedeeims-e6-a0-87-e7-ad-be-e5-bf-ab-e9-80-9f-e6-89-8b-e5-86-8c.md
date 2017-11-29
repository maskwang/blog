---
title: Dedeeims标签快速手册
id: 133
categories:
  - Dedecms
  - 技术专栏
date: 2011-08-22 22:59:46
tags:
---

<div id="blog_text">

Dedeeims标签快速手册

{dede:global.cfg_webname/} 站点名称
{dede:global.cfg_basehost/} 站点url(后台设置)
{dede:global.cfg_cmsurl/} 站点实际url(奇奇推荐)
{dede:global.cfg_memberurl/} 会员中心地址
{dede:global.cfg_dataurl/} 站点data目录地址
{dede:global.cfg_templeturl/} 模板目录地址
{dede:global.cfg_powerby/} 底部版权
{dede:global.cfg_beian/} 备案信息

{dede:field.description function=’html2text(@me)’/} 站点描述
{dede:field.phpurl/} 站点plus目录站点地址
{dede:field.title/} 标题
{dede:field.keywords/} 关键字

{dede:flink row=’24′/}友情链接

{dede:field.content/} 栏目内容

{dede:field.position/} 当前位置
{dede:field.pubdate function=”MyDate(’Y-m-d H:i’,@me)”/} 时间
{dede:field.source/} 来源
{dede:field.writer/} 作者
&lt;script src=”{dede:field name=’phpurl’/}/count.php?view=yes&amp;aid={dede:field name=’id’/}&amp;mid={dede:field name=’mid’/}” type=’text/javascript’ language=”javascript”&gt;&lt;/script&gt; 点击次数
{dede:field.body/} 文章内容
{dede:adminname/} 责任编辑
{dede:pagebreak/} 页码
{dede:prenext get=’pre’/} 上一篇
{dede:prenext get=’next’/} 下一篇

导航
{dede:channel type=’self’ currentstyle=”&lt;span&gt;&lt;a href=’~typelink~’ class=’thisclass’&gt;~typename~&lt;/a&gt;&lt;/span&gt;”}
&lt;span&gt;&lt;a href=’[field:typeurl/]‘&gt;[field:typename/]&lt;/a&gt;&lt;/span&gt;{/dede:channel}

{dede:include filename=”*.htm”/} 调用模板文件
{dede:memberinfos}
&lt;a href=”[field:spaceurl /]“&gt;&lt;img src=”[field:face/]” width=”52″ height=”52″ /&gt;&lt;/a&gt; 头像
&lt;a href=’[field:spaceurl /]‘&gt;[field:uname/]&lt;/a&gt; 用户名
&lt;a href=”[field:spaceurl /]“&gt;查看详细资料&lt;/a&gt;
&lt;a href=”[field:spaceurl /]&amp;action=guestbook”&gt;发送留言&lt;/a&gt;
&lt;a href=”[field:spaceurl /]&amp;action=newfriend”&gt;加为好友&lt;/a&gt;
用户等级:&lt;/small&gt;[field:rankname /]
注册时间:&lt;/small&gt;[field:jointime function="MyDate('Y-m-d H:m',@me)"/]
最后登录:&lt;/small&gt;[field:logintime function="MyDate('Y-m-d H:m',@me)"/]
{/dede:memberinfos}

&lt;a href=”{dede:field name=’phpurl’/}/stow.php?aid={dede:field.id/}” target=”_blank”&gt;收藏&lt;/a&gt;
&lt;a href=”{dede:field name=’phpurl’/}/erraddsave.php?aid={dede:field.id/}&amp;title={dede:field.title/}” target=”_blank”&gt;挑错&lt;/a&gt;
&lt;a href=”{dede:field name=’phpurl’/}/recommend.php?aid={dede:field.id/}” target=”_blank”&gt;推荐&lt;/a&gt;
&lt;a href=”#” onClick=”window.print();”&gt;打印&lt;/a&gt;

文档列表
{dede:arclist titlelen=42 row=10}
&lt;li&gt;&lt;a href=”[field:arcurl/]“&gt;[field:title/]&lt;/a&gt;
&lt;p&gt;[field:description function='cn_substr(@me,80)'/]…&lt;/p&gt;
&lt;/li&gt;{/dede:arclist}

</div>