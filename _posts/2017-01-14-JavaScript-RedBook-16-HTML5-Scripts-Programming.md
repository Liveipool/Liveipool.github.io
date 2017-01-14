---
layout: post
title: JavaScript红皮书学习(16)--HTML5 脚本编程 
date: 2017-01-14
categories: blog
description: JavaScript学习
---

# HTML5 脚本编程       
![16.0.png](http://upload-images.jianshu.io/upload_images/3001083-71187a0ba7a2593e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

## 16.1 原生拖放       
HTML5 以IE的实例为基础指定了拖放规范.       

### 16.1.1 拖放事件       
![16.1.1.png](http://upload-images.jianshu.io/upload_images/3001083-d2a00d08f5ef1bfe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 16.1.2 自定义放置目标       
虽然所有元素都支持放置目标事件,但这些元素默认是不允许放置的.如果拖动元素经过不允许放置的元素,无论用户如何操作的,都不会发生drop事件,而不会发生drop事件我们就无法定义在drop事件发生时会产生怎样的变化.       
把任何元素变成有效的放置目标的方法是:重写dragenter和dragover事件的默认行为:       

``` javascript
	var node1 = document.getElementById("drag");
	node1.addEventListener("dragover", function(event) {
		event.preventDefault();
	})
	node1.addEventListener("dragenter", function(event) {
		event.preventDefault();
	})
```
*firefox上还有个小bug,在拖动图片后,它会自动打开图片的URL,这时还需要在drop事件中加上event.preventDefault().*       

### 16.1.3 dataTransfer 对象        
![16.2.3.png](http://upload-images.jianshu.io/upload_images/3001083-92e7785ec6ab2500.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
*暂时没用到过这个对象*       

### 16.1.4 可拖动       
![16.2.5.png](http://upload-images.jianshu.io/upload_images/3001083-cb9c953d70996c7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

由于元素的放置事件的默认行为是什么都不做,所以要自己重写这个行为,比如,可通过下面这段代码实现一个拖动添加图片:(但这种写法不太好,最好是能够直接复制,但还没学会怎么直接复制图片,应该是要用到dataTransfer对象,以后看看).       

```
	//jade 
	div.m-20
		img(src="/static/pictures/08685344-11d9-426d-a204-ccabbf7049ba-2060x1236.jpeg")
	div#drag.w-500.h-500.m-20(style="border: 1px solid black")

	//JavaScript
	var node1 = document.getElementById("drag");
	var nodeImg = document.getElementsByTagName("img")[0];
	node1.addEventListener("dragover", function(event) {
		console.log("1");
		event.preventDefault();
	})
	node1.addEventListener("dragenter", function(event) {
		console.log("2");
		event.preventDefault();
	})
	node1.addEventListener("drop", function(event) {
		var image = document.createElement("img");
		image.src = "/static/pictures/08685344-11d9-426d-a204-ccabbf7049ba-2060x1236.jpeg";
		node1.appendChild(image);
		event.preventDefault();
	})
```

## 16.2 媒体元素       
HTML5新增了两个与媒体相关的标签,让开发人员不必依赖任何插件就能在网页中嵌入浏览器的音频和视频内容.这两个标签就是**< audio >和< video >**.       
![16.3.png](http://upload-images.jianshu.io/upload_images/3001083-09d4b799b575e113.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
**但是要注意,不同浏览器对这两个标签的支持区别很大,在使用时一定要注意.**       

### 16.2.1 属性       
![16.3.11.png](http://upload-images.jianshu.io/upload_images/3001083-7d20349a8fc2b28c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![16.3.12.png](http://upload-images.jianshu.io/upload_images/3001083-5e8d070c5ea55020.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 16.2.2 事件       
![16.3.2.png](http://upload-images.jianshu.io/upload_images/3001083-c1276200015b9bbf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 16.2.3 检测编解码器的支持情况       
![16.3.41.png](http://upload-images.jianshu.io/upload_images/3001083-a99f9ecbf2235c50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![16.3.42.png](http://upload-images.jianshu.io/upload_images/3001083-6ed36432b0544034.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 16.2.4 Audio类型       
![16.3.51.png](http://upload-images.jianshu.io/upload_images/3001083-10c01277b0b62314.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![16.3.52.png](http://upload-images.jianshu.io/upload_images/3001083-3fd1a797bc9b4330.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

## 16.3 历史状态管理       
历史状态管理是现代Web应用中的一个难点.因为在现代Web应用中,用户的每次操作不一定会打开一个一个全新的页面,因此"后退"和"前进"按钮也就失去了作用,导致用户很难在不同状态间切换.       

要解决这个问题,首选使用**hashchange事件**,通过这个事件,可以知道URL参数什么时候发生了变化,即什么时候该有所反应.而通过状态管理API,能够在不加载新页面的情况下改变浏览器的URL.为此,需要使用history.pushState()方法,该方法可以接收三个参数:1.状态对象. 2.新状态的标题. 3.可选的相对URL.但是,这个方法现在还未被所有浏览器完全支持,如firefox会忽略第二个参数等.       
![16.41.png](http://upload-images.jianshu.io/upload_images/3001083-bec20c03116f8882.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![16.42.png](http://upload-images.jianshu.io/upload_images/3001083-8d31bb807ced42c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![16.43.png](http://upload-images.jianshu.io/upload_images/3001083-0fd0ccae345e4372.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

## 16.4 小结       
![16.5.png](http://upload-images.jianshu.io/upload_images/3001083-28c86eedb100aee5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

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