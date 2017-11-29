---
title: 简单配置.htaccess就可以实现的10个功能
id: 513
categories:
  - Apache
  - Web服务器
  - 技术专栏
date: 2012-02-17 15:24:51
tags:
---

#### 1\. 反盗链

那些盗用了你的内容，还不愿意自己存储图片的网站是很常见的。你可以通过以下配置来放置别人盗用你的图片：
<div id="highlighter_787580">
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`RewriteBase /`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`2`</td>
<td>`RewriteCond %{HTTP_REFERER} !^$`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`3`</td>
<td>`RewriteCond %{HTTP_REFERER} !^http:``//(www.)?yoursite.com/.*$ [NC]`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`4`</td>
<td>`RewriteRule .(gif|jpg|swf|flv|png)$ /feed/ [R=302,L]`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

#### 2\. 防止目录浏览

有时候目录浏览是有用的，但大部分情况会有安全问题。为了让你的网站更安全，你可以通过htaccess文件来禁用这个功能：
<div id="highlighter_700870">
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`Options All -Indexes`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

#### 3\. SEO友好的301永久重定向

这一招是我常用的。每次我更改网站URL结构的时候，我都会做301重定向：
<div id="highlighter_29449">
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`Redirect 301 http:``//www.yoursite.com/article.html[http://www.yoursite.com/archives/article](http://www.yoursite.com/archives/article)`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

#### 4\. 显示个性化的 404 错误页面

当用户访问了一个不存在的页面的时候，网页服务器会显示“404 file not found”错误。有很多CMS可以让你设置自定义的错误页面，但最简单的方法是更改htaccess：
<div id="highlighter_907263">
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`ErrorDocument 404 /404.html`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

#### 5\. 设置目录的默认页面

假如你需要为不同的目录设置不同的默认页面，你可以很容易的通过 .htaccess 实现：
<div id="highlighter_730404">
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`DirectoryIndex about.html`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

#### 6\. 基于referer来限制网站访问

站长通常不会限制网站访问，但是当你发现有一些网站尽给你带来垃圾流量的话，你就应该屏蔽他们：
<div id="highlighter_377843">
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`&lt;IfModule mod_rewrite.c&gt;`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`2`</td>
<td>`RewriteEngine on  RewriteCond %{HTTP_REFERER} spamteam.com [NC,OR]`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`3`</td>
<td>`RewriteCond %{HTTP_REFERER} trollteam.com [NC,OR]`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`4`</td>
<td>`RewriteRule .* – [F]`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`5`</td>
<td>`&lt;/ifModule&gt;`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

#### 7\. 限制PHP上传文件大小

这招在共享空间的服务器上很有用，可以让我的用户上传更大的文件。第一个是设置最大的上传文件大小，第二个是设置最大的POST请求大小，第三个PHP脚本最长的执行时间，最后一个是脚本解析上传文件的最长时间：
<div id="highlighter_841361">
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`php_value upload_max_filesize 20M`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`2`</td>
<td>`php_value post_max_size 20M`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`3`</td>
<td>`php_value max_execution_time 200`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`4`</td>
<td>`php_value max_input_time 200`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

#### 8\. 压缩文件

你可以通过压缩文件来减少网络流量，页面装载时间：
<div id="highlighter_214682">
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`AddOutputFilterByType DEFLATE text/plain`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`2`</td>
<td>`AddOutputFilterByType DEFLATE text/html`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`3`</td>
<td>`AddOutputFilterByType DEFLATE text/xml`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`4`</td>
<td>`AddOutputFilterByType DEFLATE text/css`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`5`</td>
<td>`AddOutputFilterByType DEFLATE application/xml`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`6`</td>
<td>`AddOutputFilterByType DEFLATE application/xhtml+xml`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`7`</td>
<td>`AddOutputFilterByType DEFLATE application/rss+xml`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`8`</td>
<td>`AddOutputFilterByType DEFLATE application/javascript`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`9`</td>
<td>`AddOutputFilterByType DEFLATE application/x-javascript`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

#### 9\. 缓存文件

这一点还需要解释吗？
<div id="highlighter_31543">
<div>
<div>[view source](http://www.nowamagic.net/librarys/veda/detail/1400#viewSource "view source")
<div><object id="highlighter_31543_clipboard" width="16" height="16" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="src" value="http://www.nowamagic.net/includes/syntaxhighlighter_2.1.364/scripts/clipboard.swf" /><param name="title" value="copy to clipboard" /><param name="allowscriptaccess" value="always" /><param name="wmode" value="transparent" /><param name="flashvars" value="highlighterId=highlighter_31543" /><param name="menu" value="false" /><embed id="highlighter_31543_clipboard" width="16" height="16" type="application/x-shockwave-flash" src="http://www.nowamagic.net/includes/syntaxhighlighter_2.1.364/scripts/clipboard.swf" title="copy to clipboard" allowscriptaccess="always" wmode="transparent" flashvars="highlighterId=highlighter_31543" menu="false" /></object></div>
[print](http://www.nowamagic.net/librarys/veda/detail/1400#printSource "print")[?](http://www.nowamagic.net/librarys/veda/detail/1400#about "?")</div>
</div>
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`&lt;FilesMatch ``".(flv|gif|jpg|jpeg|png|ico|swf|js|css|pdf)$"``&gt;`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`2`</td>
<td>`Header set Cache-Control ``"max-age=2592000"`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`3`</td>
<td>`&lt;/FilesMatch&gt;`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>

#### 10\. 添加尾部的反斜杠

我并不确定，但是很多文章，很多人都说添加尾部反斜杠有益于SEO：
<div id="highlighter_343169">
<div>
<div>
<table>
<tbody>
<tr>
<td>`1`</td>
<td>`&lt;IfModule mod_rewrite.c&gt;`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`2`</td>
<td>`RewriteCond %{REQUEST_URI} /+[^\.]+$`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`3`</td>
<td>`RewriteRule ^(.+[^/])$ %{REQUEST_URI}/ [R=301,L]`</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>`4`</td>
<td>`&lt;/IfModule&gt;`</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>