---
title: CSS filter滤镜的用法
id: 146
categories:
  - Css
  - 技术专栏
date: 2011-08-22 23:02:31
tags:
---

语法：

**filter : **_filter _

参数：

_filter : _　要使用的滤镜效果。多个滤镜之间用空格隔开。

说明：

设置或检索对象所应用的滤镜效果。
要使用该属性，对象必须具有[height](mk:@MSITStore:D:%20ascode%20%E8%B5%84%E6%96%99%20%E6%96%B0%E7%9A%84%E5%BC%80%E5%A7%8B%20css%20CSS2.CHM::/css2/c_height.html)，[width](mk:@MSITStore:D:%20ascode%20%E8%B5%84%E6%96%99%20%E6%96%B0%E7%9A%84%E5%BC%80%E5%A7%8B%20css%20CSS2.CHM::/css2/c_width.html)，[position](mk:@MSITStore:D:%20ascode%20%E8%B5%84%E6%96%99%20%E6%96%B0%E7%9A%84%E5%BC%80%E5%A7%8B%20css%20CSS2.CHM::/css2/c_position.html)三个属性中的一个。
滤镜的机制是可扩展的。可以开发和使用第三方滤镜。
该属性在MAC平台上不可用。
对应的脚本特性为**filter**。

示例：

div { width:200px; filter:blur(strength=50) flipv() ; }
img { filter: invert(); }

**filter:filtername(parameters)　即 filter:滤镜名称（参数）**
<table width="500" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="28%">
<div align="center">**滤镜效果**</div></td>
<td width="72%">
<div align="center">**功能描述**</div></td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Alpha **</span></td>
<td width="72%">**设置不同的透明度变化效果**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Blur**</span></td>
<td width="72%">**建立模糊效果**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**DropShadow**</span></td>
<td width="72%">**建立一种偏移的影象轮廓，即投射阴影**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**FlipH**</span></td>
<td width="72%">**水平翻转**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**FlipV**</span></td>
<td width="72%">**垂直翻转**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Glow**</span></td>
<td width="72%">**为对象的边界增加色彩光效**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Gray**</span></td>
<td width="72%">**将图片以灰度形式显示**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Invert**</span></td>
<td width="72%">**将色彩、饱和度以及亮度值完全反转,类似底片效果**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Light**</span></td>
<td width="72%">**在一个对象上进行灯光投影**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Mask**</span></td>
<td width="72%">**为一个对象建立彩色透明遮罩**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Shadow**</span></td>
<td width="72%">**为对象建立轮廓的影效果**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Wave**</span></td>
<td width="72%">**在X轴和Y轴方向利用正弦波打乱图片**</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="28%"><span style="color: #000000;">**Xray**</span></td>
<td width="72%">**只显示对象的轮廓**</td>
</tr>
</tbody>
</table>
**<span style="color: #ac0000;">
具体的应用</span>**有两种方法：

1、 先定义好CSS ，再在页面中需要的对象上使用预先定义好的CSS，实际上CSS的设置对话框里已经预先将这些滤镜的语法设置好了，我们只需填上适合的具体参数即可：

2、直接在支持CSS滤镜效果的HTML控件元素上编写CSS filter代码。

a.**Alpha 滤镜

**　　"Alpha"属性是把一个目标元素与背景混合。设计者可以指定数值来控制混合的程度。这种“与背景混合”通俗地说就是一个元素的透明度。通过指定坐标，可以指定各种不同范围的透明度。
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="23%">
<div align="center">**Alpha 滤镜语法**</div></td>
<td width="77%">
<div align="left">{FILTER：ALPHA(opacity=opacity,finishopacity=finishopacity,
style=style,startx=startx,
starty=starty,finishx=finishx,finishy=finishy)}</div></td>
</tr>
</tbody>
</table>
参数含义分别如下：
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="23%">
<div align="center">**参数 **</div></td>
<td width="77%">
<div align="center">**说明**</div></td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">opacity</td>
<td width="77%">透明度。默认的范围是从0 到 100，他们其实是百分比的形式。也就是说，0代表完全透明，100代表完全不透明。</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">finishopacity</td>
<td width="77%">是一个可选参数，如果想要设置渐变的透明效果，就可以使用他们来指定结束时的透明度。范围也是0 到 100。</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">style</td>
<td width="77%">指定透明区域的形状特征：
0 代表统一形状
1 代表线形
2 代表放射状
3 代表矩形</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">startx</td>
<td width="77%">渐变透明效果开始处的 X坐标。</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">starty</td>
<td width="77%">渐变透明效果开始处的 Y坐标。</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">finishx</td>
<td width="77%">渐变透明效果结束处的 X坐标。</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">finishy</td>
<td width="77%">渐变透明效果结束处的 Y坐标。</td>
</tr>
</tbody>
</table>
b.**Blur 滤镜**　　用手指在一幅尚未干透的画面迅速划过时，画面就会变得模糊。”Blur"就是产生同样的模糊效果。
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="22%">
<div align="center">**<strong>Blur滤镜**语法</strong></div></td>
<td width="78%">
<div align="left">HTML：{filter:blur(add=add,direction=direction,
strength=strength)}
Script语言： [oblurfilter=] object.filters.blur</div></td>
</tr>
</tbody>
</table>
参数含义分别如下：
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="23%">
<div align="center">**参数 **</div></td>
<td width="77%">
<div align="center">**说明**</div></td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">add</td>
<td width="77%">它指定图片是否被改变成印象派的模糊效果。模糊效果是按顺时针的方向进行的，
这是一个布尔值:ture （默认）或false</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">direction</td>
<td width="77%">该参数用来设置模糊的方向。
0度代表垂直向上，每45度为一个单位，默认值是向左的270度</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">strength</td>
<td width="77%">只能使用整数来指定，代表有多少像素的宽度将受到模糊影响，默认是5个像素。</td>
</tr>
</tbody>
</table>
c.**DropShadow 滤镜**

“DropShaow"，顾名思义就是添加对象的阴影效果。其工作原理是建立一个偏移量，加上色彩。
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="31%">
<div align="center">**DropShadow 滤镜****语法**</div></td>
<td width="69%">
<div align="left">{filter:dropshadow
(color=color,offx=ofx,offy=offy,positive=positive)}</div></td>
</tr>
</tbody>
</table>
参数含义如下：
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="23%">
<div align="center">**参数 **</div></td>
<td width="77%">
<div align="center">**说明**</div></td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">Color</td>
<td width="77%">代表投射阴影的颜色</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">offx</td>
<td width="77%">X方向阴影的偏移量</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">offy</td>
<td width="77%">Y方向阴影的偏移量</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">Positive</td>
<td width="77%">布尔值
如果为TRUE（非0），就为任何的非透明像素建立可见的投影
如果为FASLE（0），就为透明的像素部分建立透明效果</td>
</tr>
</tbody>
</table>
d.**FlipH, FlipV 滤镜

**　　FlipH 滤镜实现水平反转** **
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="29%">
<div align="center">**<strong>FlipH 滤镜**语法</strong></div></td>
<td width="71%">
<div align="left">{filter:filph}</div></td>
</tr>
</tbody>
</table>
FlipV 滤镜实现垂直反转** **
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="30%">
<div align="center">**<strong>FlipV 滤镜**语法</strong></div></td>
<td width="70%">
<div align="left">{filter:filpv}</div></td>
</tr>
</tbody>
</table>
e.**FlipH, FlipV 滤镜

**　　FlipH 滤镜实现水平反转** **
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="29%">
<div align="center">**<strong>FlipH 滤镜**语法</strong></div></td>
<td width="71%">
<div align="left">{filter:filph}</div></td>
</tr>
</tbody>
</table>
FlipV 滤镜实现垂直反转** **
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="30%">
<div align="center">**<strong>FlipV 滤镜**语法</strong></div></td>
<td width="70%">
<div align="left">{filter:filpv}</div></td>
</tr>
</tbody>
</table>
f.**Glow 滤镜

**　　对一个对象使用"glow"属性后，这个对象的边缘就会产生类似发光的效果。
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="31%">
<div align="center">**Glow 滤镜****语法**</div></td>
<td width="69%">
<div align="left">{filter:glow(color=color,strength)}</div></td>
</tr>
</tbody>
</table>
参数含义如下：
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="23%">
<div align="center">**参数 **</div></td>
<td width="77%">
<div align="center">**说明**</div></td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">Color</td>
<td width="77%">指定发光的颜色</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">STRENGTH</td>
<td width="77%">强度，值为1到255之间的任何整数，指定发光色力度和范围。</td>
</tr>
</tbody>
</table>
g.**Gray ,Invert,Xray 滤镜

**　　使用Gray滤镜可以把一张图片变成灰度图，语法很简单：
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="31%">
<div align="center">**Gray 滤镜****语法**</div></td>
<td width="69%">
<div align="left">{filter:gray}</div></td>
</tr>
</tbody>
</table>
h.**Gray ,Invert,Xray 滤镜

**　　使用Gray滤镜可以把一张图片变成灰度图，语法很简单：
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="31%">
<div align="center">**Gray 滤镜****语法**</div></td>
<td width="69%">
<div align="left">{filter:gray}</div></td>
</tr>
</tbody>
</table>
i.**Mask 滤镜
**
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="31%">
<div align="center">**Mask 滤镜****语法**</div></td>
<td width="69%">
<div align="left">{filter:mask(color=color)}</div></td>
</tr>
</tbody>
</table>
使用"MASK"属性可以为对象建立一个覆盖于表面的膜，其效果就象戴着有色眼镜看物体一样 。
j.

**Light 滤镜
**
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="31%">
<div align="center">**Light 滤镜****语法**</div></td>
<td width="69%">
<div align="left">{filter:light}</div></td>
</tr>
</tbody>
</table>
这个属性模拟光源的投射效果。一旦为对象定义了“LIGHT"滤镜属性，那么就可以调用它的“方法(Method)"来设置或者改变属性。“LIGHT"可用的方法有：
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="23%">
<div align="center">**参数 **</div></td>
<td width="77%">
<div align="center">**说明**</div></td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">AddAmbient</td>
<td width="77%">加入包围的光源</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">AddCone</td>
<td width="77%">加入锥形光源</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">AddPoint</td>
<td width="77%">加入点光源</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">Changcolor</td>
<td width="77%">改变光的颜色</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">Changstrength</td>
<td width="77%">改变光源的强度</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%" height="18">Clear</td>
<td width="77%" height="18">清除所有的光源</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%" height="18">MoveLight</td>
<td width="77%" height="18">移动光源</td>
</tr>
</tbody>
</table>
k.**Shadow 滤镜
**
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="31%">
<div align="center">**Shadow 滤镜****
语法**</div></td>
<td width="69%">
<div align="left">{filter:shadow(color=color,direction=direction)}</div></td>
</tr>
</tbody>
</table>
利用“Shadow”属性可以在指定的方向建立物体的投影，COLOR是投影色，DIRECTION是设置投影的方向。其中0度代表垂直向上，然后每45度为一个单位。它的默认值是向左的270度。
l.

**Wave 滤镜**
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="31%">
<div align="center">**Wave 滤镜****
语法**</div></td>
<td width="69%">
<div align="left">{filter:wave(add=add,freq=freq,
lightstrength=strength,
phase=phase,strength=strength)}</div></td>
</tr>
</tbody>
</table>
&nbsp;
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="23%">
<div align="center">**参数 **</div></td>
<td width="77%">
<div align="center">**说明**</div></td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">wave</td>
<td width="77%">把对象按垂直的波形样式打乱。
默认是 TRUE（非0）</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">ADD</td>
<td width="77%">是否要把对象按照波形样式打乱</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">FREQ</td>
<td width="77%">波纹的频率，也就是指定在对象上一共需要产生多少个完整的波纹</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">LIGHTSTRENGTH</td>
<td width="77%">可以对于波纹增强光影的效果，范围0----100</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">PHASE</td>
<td width="77%">设置正弦波的偏移量</td>
</tr>
<tr bgcolor="#ffffff">
<td align="center" width="23%">STRENGTH</td>
<td width="77%">振幅大小</td>
</tr>
</tbody>
</table>
m.**Gray ,Invert,Xray 滤镜

**　　使用Gray滤镜可以把一张图片变成灰度图，语法很简单：
<table width="90%" border="0" cellspacing="1" cellpadding="1" align="center" bgcolor="#bbbbbb">
<tbody>
<tr bgcolor="#cccccc">
<td align="center" width="31%">
<div align="center">**Gray 滤镜****语法**</div></td>
<td width="69%">
<div align="left">{filter:gray}</div></td>
</tr>
</tbody>
</table>