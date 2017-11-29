---
title: 初学Python：一个抓取图片脚本
tags:
  - python
id: 738
categories:
  - 编程语言
date: 2014-06-12 07:55:54
---

#!/usr/bin/python
import re
import urllib

def getHtml(url):
    page = urllib.urlopen(url)
    html = page.read()
    return html

def getImg(html):
    reg = r'src="(.*?\.jpg)"'
    imgre = re.compile(reg)
    imglist = re.findall(imgre,html)
    x = 0
    for imgurl in imglist:
        urllib.urlretrieve(imgurl,'%s.jpg' % x)
        x+=1
    return imglist

html = getHtml("http://tieba.baidu.com/p/3099506290")
print getImg(html)