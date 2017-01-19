---
layout: post
title: JavaScript学习专题之--私有变量和ES5中的模块模式  
date: 2016-12-24
categories: blog
description: JavaScript学习
---


# 私有变量和ES5中的模块模式      

## 私有变量相关概念      
严格来讲,JavaScript中并没有私有成员的概念,所有对象属性都是公有的.不过,倒是有私有变量的概念.**任何在函数中定义的变量,都可以认为是私有变量.**      

**有权访问私有变量和私有函数的公有方法被称为特权方法.**      
有两种在对向上创建特权方法的方式:**第一种是在构造函数中定义特权方法**     

``` javascript
	function myObject() {
		var privateVariable = 10;     //私有变量
		function privateFunction() {  //私有函数
			return false;
		}

		this.publicMethod = function() {    //特权方法
			privateVariable++;
			console.log(privateVariable);
			console.log(privateFunction());
		}
	}

	var object = new myObject();     
	object.publicMethod();           //11  false
```

**第二种是在原型上定义特权方法.**这种方式和在构造函数中定义特权方法的主要区别在于,私有变量是由实例共享的,下面看一下例子:      

``` javascript

	(function() {
		var name = "";

		Person = function(value) {       //这样写在严格模式下会报错
			name = value;
		}

		Person.prototype.getName = function() {
			return name;
		}

		Person.prototype.setName = function(value) {
			name = value;
		}
	})();

	var person1 = new Person("Daniel");
	console.log(person1.getName());      //Daniel
	person1.setName("Nick");
	console.log(person1.getName());      //Nick

	var person2 = new Person("Michael");
	console.log(person2.getName());      //Micheal
	console.log(person1.getName());      //Micheal
```
**注意理解这两种创建特权方法即它们之间的区别.**

## ES5中的模块模式         
在上文中我们了解了与私有变量相关的一些概念,前面的博客中我们也已经详细了解了闭包.接下来在这里要提到的代码模式:*模块模式*就运用了闭包的强大威力.      
随着ES6的出现,JavaScript终于有了一个完善的模块体系,但是在系统的学习ES6的模块之前,我们还是先了解一下截至到ES5, JavaScript是怎样来实现模块的.          

考虑以下的代码:      

``` javascript
function coolModule() {
	var something = "cool";      //私有变量
	var another = [1, 2, 3];

	function doSomething() {     //私有函数
		console.log(something);
	}

	function doAnother() {
		console.log(another.join(","));
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother
	}
}

var foo = coolModule();

foo.doSomething();     //cool
foo.doAnother();       //1,2,3
```
这个模式在JavaScript中被称为模块.最常见的模块实现的方法通常被称为*模块暴露*,这里展示的是其变体.         
我们分析一下上面的代码:首先,coolModule()只是一个全局函数,必须要通过调用它来创建一个模块实例.如果不调用它,内部作用域和doSomething(),doAnother()这两个闭包都无法被创建.            
其次,coolModule()返回一个用对象字面量语法{key: value, ...}来表示的对象.这个返回的对象中含有对**私有函数**而不是**私有变量**的引用.我们保持私有变量是隐藏且私有的状态.可以将这个对象类型的返回值看作本质上是**模块的公共API.**    
这个对象类型的返回值最终被赋值给外部的变量foo,然后就可以通过它来访问API中的属性方法,比如foo.doSomething().         

如果需要更简单的描述,**模块模式需要具备两个必要条件:**           
**1.必须有外部的封闭函数(如coolModule),该函数必须至少被调用一次(每次调用都会创建一个新的模块实例).**      
**2.封闭函数必须返回至少一个私有函数,这样私有函数才能在私有作用域上形成闭包,并且可以访问或者修改私有的状态.**         

上一个实例代码中有一个叫做coolModule()的独立模块创建器,**可以被调用任意多次,每次调用都会创建一个新的模块实例.当只需要一个实例时,可以对这个模块进行简单的改进来实现单例模式.**      

``` javascript
	var foo = (function coolModule() {
		var something = "cool";
		var another = [1, 2, 3];

		function doSomething() {
			console.log(something);
		}

		function doAnother() {
			console.log(another.join(","));
		}

		return {
			doSomething: doSomething,
			doAnother: doAnother
		}
	})();

	foo.doSomething();     //cool
	foo.doAnother();       //1,2,3
```

**模块模式另一个简单但强大的用法是命名将要作为公共API返回的对象:**      

``` javascript
	var foo = (function coolModule(id) {
		function change() {
			publicAPI.identify = identify2;
		}

		function identify1() {
			console.log(id);
		}

		function identify2() {
			console.log(id.toUpperCase());
		}

		return publicAPI = {
			identify: identify1,
			change: change
		}
	})("foo module");

	foo.identify();     //foo module
	foo.change();
	foo.identify();     //FOO MODULE
```
**通过在模块实例的内部保留对公共API对象的内部引用,可以从内部对模块实例进行修改,包扩添加或删除方法和属性,以及修改它们的值.**      

###  "现代"的模块机制      
这个"现代"其实不那么准确,只是代表截止到ES5,体现和ES6模块机制的区别(本文中不讨论ES6的模块机制).      
**现有的大多数模块依赖加载器/管理器本质上都是将这种模块定义封装进一个友好的API.**      这里并不会研究某个具体的库,而是通过一段代码来宏观地体现一下现代模块机制的核心思想:      
      
``` javascript
	var MyModules = (function Manager() {
		var modules = {};

		function define(name, dependencies, fn) {
			for (var i  = 0; i < dependencies.length; i++) {
				dependencies[i] = modules[dependencies[i]];   //把这个dependence的name对应的值取出来.
			}
			modules[name] = fn.apply(fn, dependencies);
		}

		function get(name) {
			return modules[name];
		}

		return {
			define: define,
			get: get
		}
	})();

	MyModules.define("bar", [], function() {
		function hello(who) {
			console.log("let me introduce: " + who);
		}

		return {
			hello: hello
		}
	});

	MyModules.define("foo", ["bar"], function(bar) {
		var anotherPerson = "Nick";

		function anotherHello() {
			return bar.hello(anotherPerson);
		}

		return {
			anotherHello: anotherHello
		}
	});

	var bar = MyModules.get('bar');
	var foo = MyModules.get('foo');

	bar.hello("Liveipool");
	foo.anotherHello();
```

这部分看得较为仓促,感觉有些地方也还没完全理解,配合着书和资料,以后再多看看,多总结一下,这部分一定要完全弄懂才行.      

### 相关文章链接    
 - [Javascript深入学习1——词法结构、类型、值、变量](http://liveipool.com/blog/2016/09/12/learn-javascript-1/)       
 - [JavaScript红皮书学习(1,2)--JavaScript简介](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-1,2-Introduction/)  
 - [JavaScript红皮书学习(3)--基本概念](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-3-BasicConcepts/)   
 - [JavaScript红皮书学习(4)--变量、作用域和内存问题](http://liveipool.com/blog/2016/12/19/JavaScript-RedBook-4-Variable-Scope-and-Memory/)    
 - [JavaScript红皮书学习(5)--引用类型](http://liveipool.com/blog/2016/12/22/JavaScript-RedBook-5-Reference-Type)     
 - [JavaScript学习专题之--作用域](http://liveipool.com/blog/2016/12/22/JavaScript-Scope)   
 - [JavaScript学习专题之--闭包](http://liveipool.com/blog/2016/12/23/JavaScript-Closures)     
 - [JavaScript学习专题之--私有变量和ES5中的模块模式](http://liveipool.com/blog/2016/12/24/JavaScript-Private-Variable-and-ES5Modules)      
 - [JavaScript学习专题之--this](http://liveipool.com/blog/2016/12/25/JavaScript-this)       
 - [JavaScript红皮书学习(8)--BOM](http://liveipool.com/blog/2016/12/25/JavaScript-RedBook-8-BOM)             
 - [JavaScript红皮书学习(6)--面向对象的程序设计](http://liveipool.com/blog/2016/12/27/JavaScript-RedBook-6-Object-Oriented)                  
 - [JavaScript红皮书学习(10)--DOM1](http://liveipool.com/blog/2016/12/31/JavaScript-RedBook-10-DOM1)                  
 - [JavaScript红皮书学习(11)--DOM-Extensions](http://liveipool.com/blog/2016/12/31/JavaScript-RedBook-11-DOM-Extensions)                  
 - [JavaScript红皮书学习(12)--DOM2-And-DOM3](http://liveipool.com/blog/2016/12/31/JavaScript-RedBook-12-DOM2-And-DOM3)                  
 - [JavaScript红皮书学习(13)--事件](http://liveipool.com/blog/2017/01/13/JavaScript-RedBook-13-Event)                  
 - [JavaScript红皮书学习(14)--表单脚本](http://liveipool.com/blog/2017/01/13/JavaScript-RedBook-14-Form)                
 - [JavaScript红皮书学习(15)--使用Canvas绘图](http://liveipool.com/blog/2017/01/14/JavaScript-RedBook-15-Canvas)      
 - [JavaScript红皮书学习(16)--HTML5脚本编程](http://liveipool.com/blog/2017/01/14/JavaScript-RedBook-16-HTML5-Scripts-Programming)           
 - [JavaScript红皮书学习(20)--JSON](http://liveipool.com/blog/2017/01/17/JavaScript-RedBook-20-JSON)      
 - [JavaScript红皮书学习(21)--Ajax](http://liveipool.com/blog/2017/01/18/JavaScript-RedBook-21-Ajax)      
 - [JavaScript红皮书学习(21)--原生Ajax和Comet](http://liveipool.com/blog/2017/01/18/JavaScript-RedBook-21-Primary-Ajax-and-Comet)      
 - [JavaScript红皮书学习(21)--jQuery中的Ajax](http://liveipool.com/blog/2017/01/19/JavaScript-RedBook-21-Ajax-in-jQuery)      
