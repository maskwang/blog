---
title: 设为首页 添加到收藏夹（兼容火狐和ie）
id: 142
categories:
  - Html
  - 技术专栏
date: 2011-08-22 23:01:42
tags:
---

<div id="blog_text">
<div>
<div>Js代码 [![复制代码](http://lym6520.javaeye.com/images/icon_copy.gif)](http://lym6520.javaeye.com/blog/342887# "复制代码")</div>
</div>

1.  <span style="color: #008200;">//添加到收藏夹 </span>
2.  **<span style="color: #7f0055;">function</span>** addBookmark(url, title) {
3.  **<span style="color: #7f0055;">if</span>** (window.sidebar) {
4.  window.sidebar.addPanel(title, url,<span style="color: #0000ff;">""</span>);
5.  } **<span style="color: #7f0055;">else</span>** **<span style="color: #7f0055;">if</span>** (document.all) {
6.  **<span style="color: #7f0055;">var</span>** external = window.external;
7.  external.AddFavorite(url, title);
8.  } **<span style="color: #7f0055;">else</span>** **<span style="color: #7f0055;">if</span>** (window.opera &amp;&amp; window.print) {
9.  **<span style="color: #7f0055;">return</span>** **<span style="color: #7f0055;">true</span>**;
10.  }
11.  }
12.13.  <span style="color: #008200;">//设为首页 </span>
14.  **<span style="color: #7f0055;">function</span>** setHomepage() {
15.  **<span style="color: #7f0055;">var</span>** lan = window.location;
16.  **<span style="color: #7f0055;">if</span>** (document.all) {
17.  document.body.style.behavior = <span style="color: #0000ff;">'url(#default#homepage)'</span>;
18.  **<span style="color: #7f0055;">var</span>** body = document.body;
19.  body.setHomePage(lan.href);
20.  } **<span style="color: #7f0055;">else</span>** **<span style="color: #7f0055;">if</span>** (window.sidebar) {
21.  **<span style="color: #7f0055;">if</span>** (window.netscape) {
22.  **<span style="color: #7f0055;">try</span>** {
23.  netscape.security.PrivilegeManager.enablePrivilege(<span style="color: #0000ff;">"UniversalXPConnect"</span>);
24.  } **<span style="color: #7f0055;">catch</span>** (e) {
25.  alert(<span style="color: #0000ff;">"该操作被浏览器拒绝，如果想启用该功能，请在地址栏内输入 about:config,然后将项 signed.applets.codebase_principal_support 值该为true"</span>);
26.  }
27.  }
28.  **<span style="color: #7f0055;">var</span>** prefs = Components.classes[<span style="color: #0000ff;">'@mozilla.org/preferences-service;1'</span>].getService(Components. interfaces.nsIPrefBranch);
29.  prefs.setCharPref(<span style="color: #0000ff;">'browser.startup.homepage'</span>, lan.href);
30.  }
31.  }
</div>
&nbsp;