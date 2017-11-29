---
title: shell-方便截取月份和日期
id: 480
categories:
  - Linux
  - 技术专栏
date: 2012-02-02 11:32:13
tags:
---

&nbsp;

比如：
日期：2007-02-28，年度不固定。
如何截取为02-28？

d=2007-02-28
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td id="postmessage_7497535">echo ${d#*-}</td>
</tr>
</tbody>
</table>
如何截取为28？

d=2007-02-28
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td id="postmessage_7497535">echo ${d##*-}</td>
</tr>
</tbody>
</table>