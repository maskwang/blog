---
title: js中prototype用法
id: 508
categories:
  - Js
  - web前端
  - 技术专栏
date: 2012-02-15 11:09:52
tags:
---

prototype 是在 IE 4 及其以后版本引入的一个针对于某一类的对象的方法，而且特殊的地方便在于：它是一个给类的对象添加方法的方法！这一点可能听起来会有点乱，别急，下面我便通过实例对这一特殊的方法作已下讲解：

　　首先，我们要先了解一下类的概念，JavaScript 本身是一种面向对象的语言，它所涉及的元素根据其属性的不同都依附于某一个特定的类。我们所常见的类包括：数组变量(Array)、逻辑变量(Boolean)、日期变量(Date)、结构变量(Function)、数值变量(Number)、对象变量(Object)、字符串变量(String) 等，而相关的类的方法，也是程序员经常用到的（在这里要区分一下类的注意和属性发方法），例如数组的push方法、日期的get系列方法、字符串的split方法等等，

　　但是在实际的编程过程中不知道有没有感觉到现有方法的不足？prototype 方法应运而生！下面，将通过实例由浅入深讲解 prototype 的具体使用方法：

1、最简单的例子，了解 prototype：
(1) Number.add(num)：作用，数字相加
实现方法：Number.prototype.add = function(num){return(this+num);}
试验：alert((3).add(15)) -> 显示 18

(2) Boolean.rev(): 作用，布尔变量取反
实现方法：Boolean.prototype.rev = function(){return(!this);}
试验：alert((true).rev()) -> 显示 false

是不是很简单？这一节仅仅是告诉读者又这么一种方法，这种方法是这样运用的。

2、已有方法的实现和增强，初识 prototype：
(1) Array.push(new_element)
　　作用：在数组末尾加入一个新的元素
　　实现方法：
　　Array.prototype.push = function(new_element){
         this[this.length]=new_element;
         return this.length;
     }
　　让我们进一步来增强他，让他可以一次增加多个元素！
　　实现方法：
　　Array.prototype.pushPro = function() {
         var currentLength = this.length;
         for (var i = 0; i < arguments.length; i++) {
             this[currentLength + i] = arguments[i];
         }
         return this.length;
     }
　　应该不难看懂吧？以此类推，你可以考虑一下如何通过增强 Array.pop 来实现删除任意位置，任意多个元素（具体代码就不再细说了）

(2) String.length
　　作用：这实际上是 String 类的一个属性，但是由于 JavaScript 将全角、半角均视为是一个字符，在一些实际运用中可能会造成一定的问题，现在我们通过 prototype 来弥补这部不足。
　　实现方法：
　　String.prototype.cnLength = function(){
         var arr=this.match(/[^\x00-\xff]/ig);
         return this.length+(arr==null?0:arr.length);
     }
　　试验：alert("EaseWe空间Spaces".cnLength()) -> 显示 16
　　这里用到了一些正则表达式的方法和全角字符的编码原理，由于属于另两个比较大的类别，本文不加说明，请参考相关材料。

3、新功能的实现，深入 prototype：在实际编程中所用到的肯定不只是已有方法的增强，更多的实行的功能的要求，下面我就举两个用 prototype 解决实际问题的例子：
(1) String.left()
　　问题：用过 vb 的应该都知道left函数，从字符串左边取 n 个字符，但是不足是将全角、半角均视为是一个字符，造成在中英文混排的版面中不能截取等长的字符串
　　作用：从字符串左边截取 n 个字符，并支持全角半角字符的区分
　　实现方法：
　　String.prototype.left = function(num,mode){
         if(!/\d+/.test(num))return(this);
         var str = this.substr(0,num);
         if(!mode) return str;
         var n = str.Tlength() - str.length;
         num = num - parseInt(n/2);
         return this.substr(0,num);
     }
　　试验：
     alert("EaseWe空间Spaces".left(8)) -> 显示 EaseWe空间
     alert("EaseWe空间Spaces".left(8,true)) -> 显示 EaseWe空
　　本方法用到了上面所提到的String.Tlength()方法，自定义方法之间也能组合出一些不错的新方法呀！

(2) Date.DayDiff()
　　作用：计算出两个日期型变量的间隔时间（年、月、日、周）
　　实现方法：
　　Date.prototype.DayDiff = function(cDate,mode){
         try{
             cDate.getYear();
         }catch(e){
             return(0);
         }
         var base =60*60*24*1000;
         var result = Math.abs(this - cDate);
         switch(mode){
             case "y":
                 result/=base*365;
                 break;
             case "m":
                 result/=base*365/12;
                 break;
             case "w":
                 result/=base*7;
                 break;
             default:
                 result/=base;
                 break;
         }
         return(Math.floor(result));
     }
　　试验：alert((new Date()).DayDiff((new Date(2002,0,1)))) -> 显示 329
     alert((new Date()).DayDiff((new Date(2002,0,1)),"m")) -> 显示 10
　　当然，也可以进一步扩充，得出响应的小时、分钟，甚至是秒。

(3) Number.fact()
　　作用：某一数字的阶乘
　　实现方法：
　　Number.prototype.fact=function(){
         var num = Math.floor(this);
         if(num<0)return NaN;
         if(num==0 || num==1)
             return 1;
         else
             return (num*(num-1).fact());
     }
　　试验：alert((4).fact()) -> 显示 24
　　这个方法主要是说明了递归的方法在 prototype 方法中也是可行的！ 