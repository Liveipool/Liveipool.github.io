---
layout: post
title: JavaScript红皮书学习(21)--jQuery中的Ajax
date: 2017-01-19
categories: blog
description: JavaScript学习
---

# jQuery中的Ajax     
Ajax全称Asynchronous JavaScript and XML,它并不是指一种单一的技术,而是有机地利用了一系列交互式网页应用相关的技术所形成的结合体.     
看完了原生的Ajax使用,这里来学习一下jQuery中的Ajax的用法.     

## 一. Ajax的优势和不足     
 - 优势:     
 - **不需要插件支持.**     
 - **优秀的用户体验.**     
 - **提高Web程序的性能:** 按需发送内容,无须全部刷新.     
 - **减轻服务器和宽带的附带:** Ajax的工作原理相当于在用户和服务器之间增加了一个中间层,使用户操作与服务器响应异步化.它在客户端创建Ajax引擎,把传统方式下的一些服务器负担的工作转移到客户端,便于客户端资源来处理.     
 - 不足:     
 - **浏览器对XMLHttpRequest对象的支持各有差异.**     
 - **破坏浏览器前进,后退按钮的正常运行.**     


## 二. jQuery中的Ajax方法     
 - jQuery对Ajax操作进行了封装.     
 - 在jQuery中$.ajax()方法属于最底层的方法.     
 - 第2层是load() 和以$.开头的get() 和 post()方法.     
 - 第3层是以$.开头的getScript()和getJson()方法.     

### 二.1 load()方法     
load()方法能载入远程HTML代码并插入DOM中: load(url, [data], [callback]).如:     

``` javascript
$(document).ready(function() {
	$("#next").click(function() {  //next和Text都是index.html中的元素
		$("#Text").load("layouts/next.html");    
	});
});
```

这样点击index.html页面中的next元素就能将next.html页面加载到index.html页面中的Text元素中了.     

对要加载进来的html文档可以进行筛选,这是通过load()方法的URL参数实现的,它的语法为"url selector",URL和选择器间有一个空格.例如只要next.html中class为"test"的内容:     

``` javascript
	$("#Text").load("layouts/next.html .test");    
```

在没有第二个参数时,load()方法默认采用GET方式.反之,则采用POST.     

对于必须要在加载完成后才能继续的操作,load()方法提供了回调函数.(注意,load()方法的函数在请求完成后就会被触发,无论是否成功).     

``` javascript
$(document).ready(function() {
	$("#next").click(function() {  //next和Text都是index.html中的元素
		$("#Text").load("layouts/next.html", function(responseText, textStatus, XMLHttpRequest) {
			console.log("responseText: ", responseText);  //请求返回的内容
			console.log("textStatus: ", textStatus);    //请求状态:success, error, notmodified, timeout四种
			console.log("XMLHttpRequest: ", XMLHttpRequest);  //XMLHttpRequest对象
		});
	});
});
```

### 二.2 $.get()方法     
该方法使用GET方式来进行异步操作.        
它的结构为: $.get(url, [data], [callback], [type]).     
其中,data是发送至服务器的key/value数据,会作为QueryString附加到请求URL中.     
type是服务器端返回的内容的格式,如html,json,text等,默认为html.     
 
如:     

``` javascript
$(document).ready(function() {
	$("#next").click(function() {
		var inputValue = $("input")[0].value;
		$.get("/post", {input: inputValue}, function(data, textStatus) {
			console.log("data: ", data);  //返回的数据,可能是各种格式,根据自己的设置决定
			console.log("textStatus: ", textStatus);
		}); 
	});
});
```

### 二.3 $.post()方法     
 - 与$.get()方法的结构和使用方式都相同,它们的不同在于:     
 - GET请求会将参数跟在URL后进行传递,而POST请求则是作为HTTP消息的实体内容发送给Web服务器.当然,在Ajax请求中,这种区别对用户是不可见的.     
 - GET方式对传输的数据有大小限制(通常是不能大于2KB),而使用POST方式传递的数据量要比GET方式大得多(理论上不受限制).     
 - GET方式请求的数据会被浏览器缓存起来,因此其他人就可以从浏览器的历史记录中读取到这些数据,例如账号和密码等.在某种情况下,GET方式会带来严重的安全性问题,而POST方式相对来说就可以避免这些问题.     

### 二.4 $.getsScript()方法     
有时候,在页面初次加载时就取得所需的全部JavaScript文件是完全没有必要的,可以在需要哪个JavaScript文件时再进行加载:     

``` javascript
$(document).ready(function() {
	$("#next").click(function() {
		$.getScript('test.js', function() {
			console.log("...");
		});
	});
});
```

### 二.5 $.getJSON()方法     
用法类似与$.getScript()     

``` javascript
$(document).ready(function() {
	$("#next").click(function() {
		$.getJSON('test.json', function(data) {
			//data:返回的数据
		});
	});
});
```

还可以通过JSONP形式的回调函数来加载其他网站的JOSN数据,例如从图片网站(http: //Flicker.com)搜索最新的几张汽车的照片并添加到页面中:     

``` javascript
$(document).ready(function() {
	$("#next").click(function() {
		$.getJSON("http://Flicker.com/...", function(data) {
			//data:返回的数据
		});
	});
});
```

> JSONP (JSON with Padding)是一个非官方的协议,它允许在服务器端继承Script tags返回至客户端,通过JavaScript Callback的形式实现跨域访问.由于JSON只是一种含有简单括号结构的纯文本,因此许多通道都可以交换JSON消息.而由于同源策略的限制,开发人员不能在外部服务器进行通信的时候使用XMLHttpRequest.而JSONP是一种可以绕过同源策略的方法,即通过使用JSON与< script >标记相结合的方法,从服务器端直接返回可以执行的JavaScript函数调用或者JavaScript对象.     

### 二.6 $.ajax()方法     
该方法是jQuery最底层的Ajax实现,前面用到的load()等方法都是基于它实现的,可以用它来替代前面的所有方法.     
$.ajax()方法只有一个对象参数,但在这个对象里包含了这个方法所需要的请求设置以及回调函数等信息,参数以key/value的形式存在,所有参数都是可选的,比较多,需要的时候查看官方API: [https://api.jquery.com/jQuery.ajax/](https://api.jquery.com/jQuery.ajax/). 下面给出一个小例子:     

``` javascript
$(document).ready(function() {
	$("#next").click(function() {
		$.ajax({
			type: "GET",
			url: "/lala",
			dataType: "json",
			success: function(data) {
				// ..
			}
		});
	});
});
```

## 三. 序列化元素     
在使用Ajax提交一个表单时,在只有少量字段时,可以将各字段提取出来,但当字段数变多时,需要一种方法来将其序列化以便传输.     
**1.serialize()方法**     
serialize()方法作用于一个jQuery对象,能够将DOM元素内容序列化为字符串,用于Ajax请求,如:     

``` javascript
$.get("/lala", $("#form1").serialize(), function(data, textStatus) {});
```

因为serialize()方法作用于jQuery对象,所以不只是表单能够使用它,其他选择器的元素也能使用它.     

**2.serializeArray()方法**      
它类似于serialize(),但它返回的不是字符串,而是将DOM元素序列化后,返回JSON格式的数据.     

**3.param()方法**     
它是serialize()方法的核心,用来对一个数组或对象按照key/value进行序列化:     

``` javascript
var obj = {a:1, b:2, c:3};
var k = $.param(obj);
alert(k);  //输出a=1&b=2&c=3
```

## 四. jQuery中的Ajax全局事件     
jQuery简化Ajax操作不仅体现在调用Ajax方法和处理响应方面,还体现在对调用Ajax方法的过程中的HTTP请求的控制.通过jQuery提供的一些自定义全局函数,能够为各种与Ajax相关的事件注册回调函数.例如当Ajax请求开始时,会触发ajaxStart()方法的回调函数,结束时有ajaxStop().这些都是全局的方法,因此无论创建它们的代码位于何处,只要有Ajax请求发生时,就会触发它们.     
如在前面远程读取Flicker.com网站中的图片时速度可能会比较慢,可以通过ajaxSrart()和ajaxStop()两个方法的回调函数采用一些用户交互来增加用户体验.如:     

``` javascript
$("#test").ajaxStart(function() {});
```
     
另外还有的方法如:ajaxComplete(callback), ajaxError(callback), ajaxSend(callback)(Ajax请求发送前执行的函数), ajaxSuccess(callback)等等.     
如果想使某个请求不受全局方法,也可以进行设置,具体用法参考网上.     

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
