---
title: linux中kill -9和kill -15的区别哪里?
id: 433
categories:
  - Linux
  - 技术专栏
date: 2011-11-01 13:52:02
tags:
---

<pre id="best-answer-content">kill -9 pid，是不顾后果的强制终止(如果的你的速度够快，有时候是和ctrl＋c是一样的)
kill -15 pid，是先关闭和其有关的程序，再将其关闭</pre>