---
title: 'Flash幻灯显示图片,图片轮播效果,Flash+JS轮播图片'
id: 179
categories:
  - Js
  - 技术专栏
date: 2011-08-22 23:15:14
tags:
---

<div id="blog_text">

&lt;%
m=3
i=1

set rs=server.createobject("adodb.recordset")
if leixing="big" then
rs.open "select * from lt_books_m where anclassid="&amp;anclassid&amp;" order by adddate desc",conn,1,1
elseif leixing="small" then
rs.open "select * from lt_books_m where anclassid="&amp;anclassid&amp;" and nclassid="&amp;nclassid&amp;" order by adddate desc",conn,1,1
end if

%&gt;
&lt;table width="612" height="13" border="0" cellpadding="0" cellspacing="0"&gt;
&lt;tr&gt;
&lt;td width="400" height="13"&gt;&lt;script type=text/javascript&gt;
&lt;!--
var focus_width=300
var focus_height=400
var text_height=0
var swf_height = focus_height+text_height
var pics='&lt;% for i=1 to m %&gt;&lt;% if rs.eof then exit for %&gt;WebEdit/&lt;%=rs("zhuang"&amp;i)%&gt;&lt;% if i&lt;&gt;m then %&gt;|&lt;% end if %&gt;&lt;% rs.movenext i=i+1 %&gt; %&gt;&lt;% next %&gt;'
var links='&lt;% rs.MoveFirst %&gt;&lt;% for i=1 to m %&gt;&lt;% if rs.eof then exit for %&gt;class_m_list.asp?id=&lt;%=rs("bookid")%&gt;&lt;% if i&lt;&gt;m then %&gt;|&lt;% end if %&gt;&lt;% rs.movenext i=i+1 %&gt;&lt;% next %&gt;'
var texts='&lt;% rs.MoveFirst %&gt;&lt;% for i=1 to m %&gt;&lt;% if rs.eof then exit for %&gt;&lt;%= rs("bookname")&amp;i %&gt;&lt;% if i&lt;&gt;m then %&gt;|&lt;% end if %&gt;&lt;% i=i+1 %&gt;&lt;% rs.movenext %&gt;&lt;% next %&gt;'

document.write('&lt;object ID="focus_flash" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="[http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,0,0](http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,0,0)" width="'+ focus_width +'" height="'+ swf_height +'"&gt;');
document.write('&lt;param name="allowScriptAccess" value="sameDomain"&gt;&lt;param name="movie" value="pixviewer.swf"&gt;&lt;param name="quality" value="high"&gt;&lt;param name="bgcolor" value="#ECEAEA"&gt;');
document.write('&lt;param name="menu" value="false"&gt;&lt;param name=wmode value="opaque"&gt;');
document.write('&lt;param name="FlashVars" value="pics='+pics+'&amp;links='+links+'&amp;texts='+texts+'&amp;borderwidth='+focus_width+'&amp;borderheight='+focus_height+'&amp;textheight='+text_height+'"&gt;');
document.write('&lt;embed src="pixviewer.swf" wmopaque" FlashVars="pics='+pics+'&amp;links='+links+'&amp;texts='+texts+'&amp;borderwidth='+focus_width+'&amp;borderheight='+focus_height+'&amp;textheight='+text_height+'" menu="false" bgcolor="#ECEAEA" quality="high" width="'+ focus_width +'" height="'+ focus_height +'" allowScriptAccess="sameDomain" type="application/x-shockwave-flash" pluginspage="[http://www.macromedia.com/go/getflashplayer](http://www.macromedia.com/go/getflashplayer)" /&gt;');
document.write('&lt;/object&gt;');
//--&gt;
&lt;/script&gt;&lt;/td&gt;
&lt;td width="391" valign="middle"&gt;

&lt;%

set rs=server.createobject("adodb.recordset")
if leixing="big" then
rs.open "select * from lt_books_m where anclassid="&amp;anclassid&amp;" order by adddate desc",conn,1,1
elseif leixing="small" then
rs.open "select * from lt_books_m where anclassid="&amp;anclassid&amp;" and nclassid="&amp;nclassid&amp;" order by adddate desc",conn,1,1
end if

%&gt;

&lt;table width="100%" height="120" border="0" cellpadding="0" cellspacing="0"&gt;
&lt;tr&gt;
&lt;td width="12%" height="18" align="center"&gt;编号:&lt;/td&gt;
&lt;td width="88%"&gt;&lt;%=rs("bookname") %&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;tr&gt;
&lt;td height="18" align="center"&gt;身高:&lt;/td&gt;
&lt;td&gt;&lt;%=rs("ShenGao") %&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;tr&gt;
&lt;td height="18" align="center"&gt;三围:&lt;/td&gt;
&lt;td&gt;&lt;%=rs("3_Wei") %&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;tr&gt;
&lt;td height="18" align="center"&gt;鞋码:&lt;/td&gt;
&lt;td&gt;&lt;%=rs("Foot") %&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;/table&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;/table&gt;

&lt;%
rs.close
set rs=nothing
%&gt;

</div>