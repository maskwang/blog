---
title: 关于Http Referer需要注意的一些问题
id: 497
categories:
  - 其它
  - 技术专栏
date: 2012-02-15 00:06:03
tags:
---

&nbsp;&nbsp;&nbsp;&nbsp;对于普通超链接、表单提交、302 跳转，各浏览器均会在请求头附加 Referer 字段信息，而对于 META 元素控制跳转及通过脚本使用 location 对象进行跳转时：

*   在 IE6 IE7 IE8 中，始终不在使用 META 元素控制跳转时附加 Referer 字段到请求头中。在普通页面中，当脚本调用 location 对象进行跳转时也不会附加 Referer 字段信息；
*   在 Firefox 中，始终不在使用 META 元素控制跳转时附加 Referer 字段到请求头中；
*   在 Chrome Safari Opera 中，均会附加 Referer 字段信息。

## 解决方案

若服务端需要获得正确的 Referer 字段信息，则应采用各浏览器均可以附加 Referer 字段信息的方式进行跳转。如，普通超链接、表单提交、HTTP 302 跳转。