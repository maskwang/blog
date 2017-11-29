---
title: 详解php生成缩略图类
id: 107
categories:
  - Php
  - 技术专栏
date: 2011-08-22 22:45:42
tags:
---

<div id="blog_text">**<span style="color: darkgreen;">php生成缩略图类，支持自定义高和宽。还支持按高和宽截图。
目前最好用的一个类。</span>**

案例：
<div>

#### 

[code language=php]&lt;?php
class resizeimage
{
//图片类型
var $type;
//实际宽度
var $width;
//实际高度
var $height;
//改变后的宽度
var $resize_width;
//改变后的高度
var $resize_height;
//是否裁图
var $cut;
//源图象
var $srcimg;
//目标图象地址
var $dstimg;
//临时创建的图象
var $im;
function resizeimage($img, $wid, $hei,$c,$dstpath)
{
$this-&gt;srcimg = $img;
$this-&gt;resize_width = $wid;
$this-&gt;resize_height = $hei;
$this-&gt;cut = $c;
//图片的类型

$this-&gt;type = strtolower(substr(strrchr($this-&gt;srcimg,"."),1));
//初始化图象
$this-&gt;initi_img();
//目标图象地址
$this -&gt; dst_img($dstpath);
//--
$this-&gt;width = imagesx($this-&gt;im);
$this-&gt;height = imagesy($this-&gt;im);
//生成图象
$this-&gt;newimg();
ImageDestroy ($this-&gt;im);
}
function newimg()
{
//改变后的图象的比例
$resize_ratio = ($this-&gt;resize_width)/($this-&gt;resize_height);
//实际图象的比例
$ratio = ($this-&gt;width)/($this-&gt;height);
if(($this-&gt;cut)=="1")
//裁图
{
if($ratio&gt;=$resize_ratio)
//高度优先
{
$newimg = imagecreatetruecolor($this-&gt;resize_width,$this-&gt;resize_height);
imagecopyresampled($newimg, $this-&gt;im, 0, 0, 0, 0, $this-&gt;resize_width,$this-&gt;resize_height, (($this-&gt;height)*$resize_ratio), $this-&gt;height);
ImageJpeg ($newimg,$this-&gt;dstimg);
}
if($ratio&lt;$resize_ratio)
//宽度优先
{
$newimg = imagecreatetruecolor($this-&gt;resize_width,$this-&gt;resize_height);
imagecopyresampled($newimg, $this-&gt;im, 0, 0, 0, 0, $this-&gt;resize_width, $this-&gt;resize_height, $this-&gt;width, (($this-&gt;width)/$resize_ratio));
ImageJpeg ($newimg,$this-&gt;dstimg);
}
}
else
//不裁图
{
if($ratio&gt;=$resize_ratio)
{
$newimg = imagecreatetruecolor($this-&gt;resize_width,($this-&gt;resize_width)/$ratio);
imagecopyresampled($newimg, $this-&gt;im, 0, 0, 0, 0, $this-&gt;resize_width, ($this-&gt;resize_width)/$ratio, $this-&gt;width, $this-&gt;height);
ImageJpeg ($newimg,$this-&gt;dstimg);
}
if($ratio&lt;$resize_ratio)
{
$newimg = imagecreatetruecolor(($this-&gt;resize_height)*$ratio,$this-&gt;resize_height);
imagecopyresampled($newimg, $this-&gt;im, 0, 0, 0, 0, ($this-&gt;resize_height)*$ratio, $this-&gt;resize_height, $this-&gt;width, $this-&gt;height);
ImageJpeg ($newimg,$this-&gt;dstimg);
}
}
}
//初始化图象
function initi_img()
{
if($this-&gt;type=="jpg")
{
$this-&gt;im = imagecreatefromjpeg($this-&gt;srcimg);
}
if($this-&gt;type=="gif")
{
$this-&gt;im = imagecreatefromgif($this-&gt;srcimg);
}
if($this-&gt;type=="png")
{
$this-&gt;im = imagecreatefrompng($this-&gt;srcimg);
}
}
//图象目标地址
function dst_img($dstpath)
{
$full_length  = strlen($this-&gt;srcimg);
$type_length  = strlen($this-&gt;type);
$name_length  = $full_length-$type_length;

$name         = substr($this-&gt;srcimg,0,$name_length-1);
$this-&gt;dstimg = $dstpath;

//echo $this-&gt;dstimg;
}
}

$resizeimage = new resizeimage("11.jpg", "200", "150", "1","17.jpg");
?&gt;  [/code]</div>
</div>