---
layout: post
title: window.onload 函数引发的血案
date: 2017-02-24
categories: blog
description: JavaScript学习
---

# window.onload 函数引发的血案           
以前一直以为在window.onload里直接定义的变量或者函数就是全局变量或者说是顶层对象window的属性和方法，但今天突然间发现其实并不是这样，故事的经过请听我娓娓道来。           

在复习this的时候，我正在试验第一种情况，即**独立函数调用**：           

```
window.onload = function() {
	function foo() {
		console.log(this.a);
	}
	var a = 2;
	// let a = 2;
	foo();  // undefined
}
```
按理说，由于在调用foo()函数时，应用的是this的**默认绑定**，即**this会指向顶层对象window**，那么就应该读出全局变量 a的值为2,但无论是用var还是let，调用函数输出的结果都是undefined。           

看了几分钟都难以理解，从任何角度来想原理都没错，于是我试着输出了一下window，发现它里面并没有a这个变量，所以说，在这里**声明的变量a并不是全局变量！**           

接着我把var或者let去掉，直接不声明a就给它赋值，这样它会默认保存在顶层对象window中（在非严格模式下才敢这么干，只是试一试，正常写代码时要杜绝这样）：           

```
window.onload = function() {
	function foo() {
		console.log(this.a);
	}
	a = 2;
	foo();  // 2
}
```
这次没出意外，调用foo函数就输出了2。那么说明，在window.onload里直接声明的变量并不是全局变量，那它们去哪了呢？           

思考了一下，不管是不是全局变量，变量a和函数foo()所处的环境应该是相同的，于是我试着去找了一下foo()函数的调用栈：           

```
window.onload = function() {
	function foo() {
		console.log(this.a);
	}
	let a = 2;
	debugger;
	foo();  // undefined
}
```
Chrome：           
![debugger.png](http://upload-images.jianshu.io/upload_images/3001083-f8d7a5aa2c7fae0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           
在上图右边的Scope中可以清晰的看出，this确实指向的是window，但a和foo都被保存到了Local中，而这个Local是什么东西就有点不太清楚了，我想可能是不同浏览器的不同实现，于是再去Firefox上测试一下：           

FireFox：           
![debugger2.png](http://upload-images.jianshu.io/upload_images/3001083-2fa9e6590ea1b93e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           

看到Firefox里面的“Function 域”，我才恍然大悟，原来我以前一直将眼光局限在window.onload = function() {}这个函数里面的内容，而忽略了这只是全局环境下的一个函数。           

```
window.onload = function() {
	var a = 2;
	function foo() {
		console.log(a);
	}
	foo();   // 2 
}
```
而之所以上面的foo()函数可以访问到变量a，并不是因为变量a和foo函数都属于全局环境，而是这里的**foo函数是一个闭包而已**。           
我以前一直没有注意到这个地方，以为这些变量a或者函数foo()什么的都是全局环境下的，如今才恍然大悟它们只是window.onload这个函数作用域里的局部变量，自然前面说的执行**默认规则的this指向顶层对象window时找不到一个局部变量a。**           
虽然在以前的编程过程中一直也没有什么大的问题，但其实这还是一个很严重的误解，幸亏今天察觉有所不对深入探究了一下，不然还不知到要懵到什么时候。           
所以说，只要window.onload函数里还有函数（一般来说都会有）形成了闭包，那么window.onload函数里的所有变量和方法都一直会保存在内存中，所以以后使用window.onload函数时一定要注意这个问题，不然继续忽视，以后很可能会导致很严重的性能问题。           

接下来，让我们再把目光转向前面的“this”复习中，这次我到全局环境下去验证**默认绑定规则：**           

```
window.onload = function() {
}
function foo() {
	console.log(this.a);
}
var a = 2;
foo();   // 2 
```
没啥毛病了，正确输出了2。           

我突然一想，既然var都快被废弃了，还是用const或者let吧:           

```
window.onload = function() {
}
function foo() {
	console.log(this.a);
}
const a = 2;
// let a = 2;
foo();   // undefined 
```
这下尴尬了，无论我是用let还是const，都会输出undefined。           
说明**就算在全局环境下，使用let和const声明的变量也不是顶层对象window的属性。**           
           
查看调用栈：           
![debugger3.png](http://upload-images.jianshu.io/upload_images/3001083-714039a3254330ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           
![debugger4.png](http://upload-images.jianshu.io/upload_images/3001083-d4f7c5bbf6e8d0a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           

虽然看不懂Chrome里的那个Script作用域和Firefox里的Block域，但可以肯定的是，**let和const声明的全局变量虽然是全局变量，但它们并不会变成顶层对象window的属性**，这其实是在ES6中有明确规定的：           

> 顶层对象，在浏览器环境指的是window对象，在Node指的是global对象。ES5之中，顶层对象的属性与全局变量是等价的。           

```
window.a = 1;
a // 1

a = 2;
window.a // 2
```

> 上面代码中，顶层对象的属性赋值与全局变量的赋值，是同一件事。           

> 顶层对象的属性与全局变量挂钩，被认为是JavaScript语言最大的设计败笔之一。这样的设计带来了几个很大的问题，首先是没法在编译时就报出变量未声明的错误，只有运行时才能知道（因为全局变量可能是顶层对象的属性创造的，而属性的创造是动态的）；其次，程序员很容易不知不觉地就创建了全局变量（比如打字出错）；最后，顶层对象的属性是到处可以读写的，这非常不利于模块化编程。另一方面，window对象有实体含义，指的是浏览器的窗口对象，顶层对象是一个有实体含义的对象，也是不合适的。           

> ES6为了改变这一点，一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。也就是说，从ES6开始，全局变量将逐步与顶层对象的属性脱钩。           

这样以来，所有问题都迎刃而解了。           

确实，直接操作全局变量或顶层变量本来就不是一个好的习惯，我只是想要测试this指向window时会出现的情况，突然引出了一些没有完全理解清晰的问题。           
学习本来就是不断踩坑和填坑的过程，把坑填上了，心情也愉快多了。           

对了，另外多说一句：        

```
function foo() {
	console.log(this.a);
}

let a = "lala";
foo();  // undefined
```
foo中的this指向window，而window中没有a属性，所以是undefined。           

```
"use strict";
function foo() {
	console.log(this.a);
}

let a = "lala";
foo();  // Uncaught TypeError: Cannot read property 'a' of undefined
```
在严格模式下，this禁止指向window，因此在调用foo时，this为undefined，所以会报错：Uncaught TypeError: Cannot read property 'a' of undefined。           