---
title: seajs 使用jquery插件
id: 706
categories:
  - Js
  - web前端
  - 技术专栏
date: 2013-08-06 14:48:42
tags:
---

seajs使用jquery插件。
方法一，将js插件cmd模块化（define封装成seajs模块，返回匿名函数，包含插件的源码）。<!--more-->
[code lang="js"]
define(function(require,exports,moudles){
     return function(jquery){
         (function($) {
             $.fn.pri= function() {
                 alert($(&quot;a&quot;).attr(&quot;href&quot;))
                 // 代码区域。
             };
         })(jquery);
     }

})
[/code]

jquery库在总js文件（调用该插件的文件）中加载。通过require("t1/jquery_pligun")($)来传递jquery变量（$参数） ,保证了jquery在调用js插件模块之前加载
[code lang="js"]
define(function (require, exports, moudles) {
    var $=require(&quot;jquery&quot;)
    require(&quot;t1/jquery_pligun&quot;)($)
    $(document).ready(function () {
        $(&quot;a&quot;).pri()
    })

})
[/code]