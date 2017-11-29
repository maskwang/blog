---
title: 详解javascript类继承机制的原理
id: 49
categories:
  - Js
  - 技术专栏
date: 2011-08-21 05:52:39
tags:
---

<div id="blog_text">

本文着重解析javascript类继承机制，让你从底层了解javascript是怎样实现“继承”这一概念的。
<div>                                                                jimichan在javaeye博客发布，转载请说明。
目前 javascript的实现继承方式并不是通过“extend”关键字来实现的，而是通过 constructor function和prototype属性来实现继承。首先我们创建一个animal 类
<div>js 代码</div>
<div></div>
var animal =  function (){   //这就是constructor function 了
this .name = 'pipi';       this .age = 10;         this .height = 0;      }       //建立一个动物的实例      var  a1 =   new  animal ();
构造函数与其他普通函数区别在于，1.构造函数里有 this关键字，2.调用构造函数是使用的new关键字。通过new运算符调用构造函数animal 后，系统就会返回一个对象，这个对象就相当于
<div>js 代码</div>
var  a1 = { name:'pipi' ,age:10,height:0 }    //或者   var  a1 =  new  Object();  a1.name='pipi';  a1.age = 10;  a1.height = 0;
等同这样的方式来产生js对象。

到这里我们知道如何在js中定义一个类了，接下来我们展示如何写一个cat
<div>js 代码</div>
<div></div>
var  cat =  function (){    this .play =  function (){      alert('cat play')     }     }     cat .prototype = new  animal ();     //prototype 属性指向一个对象
var c1 = new cat();

到这里，cat就继承了 animal 对象，类cat的一个实例对象c1拥有属性name,age,height,和方法play了。
那么 prototype起到了一个什么样的作用呢？
prototype就好比一个指针，它指向一个object，这个object就称为子类对象的原型。当cat的对象被创建的时候，由于cat的构造函数拥有prototype属性，那么cat的实例就会间接指向这个原型对象了（说成间接的是因为每个object都有一个 constructor 属性指向它的构造函数）。
那么问题来了，“当我们修改对象 c1 的name属性的时候，会不会修改它prototype的name属性值呢？”，答案是否定的。
接下来详细解析：
1.访问name属性： 首先当我们第一次访问c1.name的属性的时候，我们会得到值“pipi”，这个和我们预料中的一样。但是计算过程你未必知道。
它计算的过程是这样的：第一步：检查c1对象中是否有name属性,找到的话就返回值，没有就跳到第二步，显然没有找到，因为cat的构造函数中没有定义。第二步：当第一步没有找时，去间接访问 prototype对象所指向的object，如果在 prototype对象中 找到的name属性的话，就返回找到的属性值。如果还是没有找到的话，再去递归地寻找 prototype对象的 prototype对象（去找它的爷爷） ，一直到找到name属性或者没有prototype对象为止。如果到最后还是没有找到name属性的话就返回undefined。

2\. 设定name属性：当我们设定c1对象的name属性时，及调用 c1.name= ' new name';  这个过程就简单多了。首先检查是否对象已有该属性，若已存在则修改当前值，若不存在则为该对象新增一个属性并设定当前值。值得一提的是，在设定值的过程中 没有去访问prototype属性。

为了加深理解，我们再看一个 read-write-read 的过程，第一次read的时候，由于自己的对象没有name属性，那么就会返回的原型对象的name属性的值。第二步，写入name的值，同样没发现本身 对象有name属性,那么就在本身对象上新建一个name属性，然后赋值。第三步，再次读取name属性，由于在第二步中已经新建了name属性，此时就 返回在第二步中设定的值。值得一提的是，在这三步中没有改变原型对象的值。

好了，到此详细分析了 javascript对象是如果实现继承的，其实和其他的面向对象语言不一样的是，javascript的继承机制是对象的原型继承而不是类型继承。

呵呵，欢迎看完，有不对的地方欢迎大家讨论！</div>
</div>