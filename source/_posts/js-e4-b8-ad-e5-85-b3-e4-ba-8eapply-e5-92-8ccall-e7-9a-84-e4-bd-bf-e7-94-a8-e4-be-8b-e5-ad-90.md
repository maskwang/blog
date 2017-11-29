---
title: JS中关于apply和call的使用例子
id: 672
categories:
  - Js
  - web前端
  - 技术专栏
date: 2012-12-09 19:09:10
tags:
---

[code lang="js"]
function Person(name,age) {
 this.name = name;
 this.age = age;
 this.sayHello = function() {
 alert('Hello:' + this.name + this.age);
 }
 }
 function Print() {
 this.funcName = 'Print';
 this.show = function() {
 var msg = [];
 for (var key in this) {
 if (typeof(this[key]) != 'function') {
 msg.push([key, ':', this[key]].join(&quot;--&quot;));
 }
 }
 alert(msg.join(&quot;|&quot;));
 }
 }
 function Student(name, age, grade, school) {
 Person.apply(this, arguments);
 Print.apply(this, arguments);
 this.grade = grade;
 this.school = school;
 }

var p1 = new Person('jake', 10);
 p1.sayHello();

var s1 = new Student('tom', 13, 6, '清华小学');
 s1.show();
 s1.sayHello();
 alert(s1.funcName);

//++++
 function add(a, b) {
 alert(a + b);
 }

function sub(a, b) {
 alert(a - b);
 }

add.call(sub, 3, 1);

[/code]