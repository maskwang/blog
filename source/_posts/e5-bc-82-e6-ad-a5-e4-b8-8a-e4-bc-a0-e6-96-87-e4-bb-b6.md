---
title: 异步上传文件
id: 585
categories:
  - Js
  - web前端
  - 技术专栏
date: 2012-08-13 14:53:28
tags:
---

upload.html
upload -- mask

[code lang="js"]
&lt;script type=&quot;text/javascript&quot; src=&quot;http://static1.pplive.cn/zone/pub/js/jquery-1.4.0.min.js&quot;&gt;&lt;/script&gt;

&lt;script type=&quot;text/javascript&quot;&gt;
$(function(){
var form = $(&quot;#upload-form&quot;);
form.one('submit', function(){
//此处动态插入iframe元素
var seed = Math.floor(Math.random() * 1000);
var id = &quot;uploader-frame-&quot; + seed;
var callback = &quot;uploader-cb&quot; + seed;
var iframe = $('&lt;iframe id=&quot;'+id+'&quot; name=&quot;'+id+'&quot; style=&quot;display:none;&quot;&gt;');
var url = form.attr('action');
form.attr('target', id).append(iframe).attr('action', url+'?iframe='+callback);

window[callback] = function(data){
console.log('received callback:', data);
iframe.remove(); //removing iframe
form.removeAttr('target');
form.attr('action', url);
window[callback] = undefined; //removing callback
};
});
})
&gt;&lt;/script&gt;
&lt;form id=&quot;upload-form&quot; action=&quot;upload.php&quot; method=&quot;post&quot; enctype=&quot;multipart/form-data&quot;&gt;　　　　&lt;input id=&quot;upload&quot; type=&quot;file&quot; name=&quot;upload&quot; /&gt;
&lt;input type=&quot;submit&quot; value=&quot;Upload&quot; /&gt; 　　&lt;/form&gt; &lt;!--more--&gt;
[/code]

服务端

[code lang="php"]
&lt;script type=&quot;text/javascript&quot;&gt;
window.top.window['&lt;?php echo $_GET['iframe'] ;?&gt;']('成功啦~');
&lt;/script&gt;
[/code]