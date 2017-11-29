---
title: Yii 引入js css jquery 执行操作
id: 37
categories:
  - Yii
  - 技术专栏
date: 2011-08-21 05:47:44
tags:
---

<div id="blog_text">
<div>

**在布局中引用通用到js，或者css：**

&lt;?php Yii::app()-&gt;clientScript-&gt;registerCoreScript('jquery');?&gt;  //注意这个将会插到&lt;title&gt;&lt;/title&gt;标签上..所以title标签要放在head文档顶部防止.jquery没 有第一个加入

**在view中引用单独的js，css这样做：**

$cs=Yii::app()-&gt;clientScript;

//引入本地站点

$cs-&gt;registerScriptFile(Yii::app()-&gt;baseUrl . '/js/star-rating/jquery.rating.pack.js', CClientScript::POS_HEAD);
$cs-&gt;registerScriptFile(Yii::app()-&gt;baseUrl . '/js/star-rating/jquery.MetaData.js', CClientScript::POS_HEAD);
//引入css文件

$cs-&gt;registerCssFile(Yii::app()-&gt;baseUrl . '/js/star-rating/jquery.rating.css');

//引入外面网址
<div>
<div>

$cs-&gt;registerScriptFile('http://ajax.googleapis.com/ajax/libs/jqueryui/1.7.2/jquery-ui.min.js');

&nbsp;

$css=Yii::app()-&gt;getAssetManager()-&gt;publish(dirname(__FILE__)."/aa.css");

</div>
</div>
</div>
</div>