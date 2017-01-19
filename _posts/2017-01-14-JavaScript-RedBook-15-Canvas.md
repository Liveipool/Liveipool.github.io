---
layout: post
title: JavaScript红皮书学习(15)--使用Canvas绘图
date: 2017-01-14
categories: blog
description: JavaScript学习
---

# 使用Canvas绘图       
![15.0.png](http://upload-images.jianshu.io/upload_images/3001083-0026b6248134a88a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

与浏览器环境中的其他组件类似,< canvas >由几组API构成.除了具备基本绘图能力的2D上下文,< canvas >还建议了一个名为WebGL的3D上下文.       

## 15.1 基本用法       
要使用< canvas >元素,必须先设置其width和height属性,指定可以绘图的区域大小.如:       

``` 
<canvas id="myCanvas" width="300" height="300"></canvas>
```

要在这块画布(canvas)上绘画,需要取得绘画上下文.而取得绘图上下文对象的引用,需要调用getContext()方法并传入上下文的名字.传入"2d",就可以取得2D上下文对象:       

``` javascript
	var myCanvas = document.getElementById("myCanvas");
	var context = myCanvas.getContext("2d");
```

使用toDataURL()方法,可以导出在< canvas >元素绘制的图像.这个方法接收两个可选参数,第一个是图像的MIME类型格式,默认为"image/png",第二个是一个0-1间的数字,用于在类型为"image/jpeg"时设置生成的图像的质量.    
   
``` javascript
	//toDataURL(type, encoderOptions);
	var myCanvas = document.getElementById("myCanvas");
	var imageURI = myCanvas.toDataURL();
	var image = document.createElement("img");
	image.src = imageURI;
	document.body.appendChild(image);
```

## 15.2 2D上下文       
![15.2.png](http://upload-images.jianshu.io/upload_images/3001083-9b205f5f5ccb2f6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 15.2.1 填充和描边       
 - fillStyle 属性       
 - strokeStyle 属性       
![15.2.11.png](http://upload-images.jianshu.io/upload_images/3001083-34777151143fcad9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.12.png](http://upload-images.jianshu.io/upload_images/3001083-2f316e53f2936cdd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

``` javascript
	var myCanvas = document.getElementById("myCanvas");
	var context = myCanvas.getContext("2d");
	context.fillStyle = "#3399FF";
	context.strokeStyle = "red";
```
**然后,所有涉及填充和描边的操作都将使用这两个样式,直至重新设置这两个值.**       

### 15.2.2 绘制矩形       
 - fillRect()       
 - strokeRect()       
 - clearRect()       
![15.2.21.png](http://upload-images.jianshu.io/upload_images/3001083-f18005f06701f861.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.22.png](http://upload-images.jianshu.io/upload_images/3001083-3d1cff3d110a841f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.23.png](http://upload-images.jianshu.io/upload_images/3001083-2d1ce9c9d6be6725.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.24.png](http://upload-images.jianshu.io/upload_images/3001083-ec5b8cb75453e11f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.25.png](http://upload-images.jianshu.io/upload_images/3001083-f0c88b3dc1637cb4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.26.png](http://upload-images.jianshu.io/upload_images/3001083-91308001835c98af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.27.png](http://upload-images.jianshu.io/upload_images/3001083-69c1a8944d7af17c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.28.png](http://upload-images.jianshu.io/upload_images/3001083-3f8c06ab1599b87e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.29.png](http://upload-images.jianshu.io/upload_images/3001083-1e138458fd008fcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 15.2.3 绘制路径       
 - beginPath()       
 - closePath()       
 - fill(), stroke()       
 - clip()       
 - isPointInPath()       
![15.2.31.png](http://upload-images.jianshu.io/upload_images/3001083-64c7bc15e246537b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.32.png](http://upload-images.jianshu.io/upload_images/3001083-5c6a0c75d5bfb914.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**但上面这些方法还没完全理解,比如控制点具体是什么效果,怎样确定当前游标位置等,下面通过绘制一个不带数字的时钟表盘来熟悉其中一些方法:**       
![15.2.33.png](http://upload-images.jianshu.io/upload_images/3001083-419518360edbbb39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.34.png](http://upload-images.jianshu.io/upload_images/3001083-7b8aa5f68853275c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.35.png](http://upload-images.jianshu.io/upload_images/3001083-3f456808bb33e212.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 15.2.4 绘制文本       
 - fillText()       
 - strokeText()       
 - measureText()       
![15.2.41.png](http://upload-images.jianshu.io/upload_images/3001083-b0b497daad68b25b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.42.png](http://upload-images.jianshu.io/upload_images/3001083-fb0b4d464ee89821.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.43.png](http://upload-images.jianshu.io/upload_images/3001083-0c85fe73d4cde0f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.44.png](http://upload-images.jianshu.io/upload_images/3001083-1acbe9614052ed50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.45.png](http://upload-images.jianshu.io/upload_images/3001083-22f822c82d4204db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.46.png](http://upload-images.jianshu.io/upload_images/3001083-e361e950361184fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 15.2.5 变换       
![15.2.51.png](http://upload-images.jianshu.io/upload_images/3001083-69c7af2c55dc6f97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.52.png](http://upload-images.jianshu.io/upload_images/3001083-3c60175fbf2f2092.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.53.png](http://upload-images.jianshu.io/upload_images/3001083-48123a6c88f8cbda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**值得注意的是,save()方法保存的只是对绘图上下文的设置和变换,不会保存绘图上下文的内容.**       

### 15.2.6 绘制图像       
 - drawImage()       
![15.2.61.png](http://upload-images.jianshu.io/upload_images/3001083-78ec791c8350a1d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.62.png](http://upload-images.jianshu.io/upload_images/3001083-c5920fa7dc4d0114.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.63.png](http://upload-images.jianshu.io/upload_images/3001083-d6979a8ac0b8a2cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.64.png](http://upload-images.jianshu.io/upload_images/3001083-2114ae5679ac3d61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**结合使用drawImage()和其他方法,可以对图像进行各种基本操作.而操作的结果可以通过toDataURL()方法获得.注意,本节讨论的都是2D绘图上下文,但toDataURL()都是Canvas对象的方法,不是上下文对象的方法,上下文对象也是Canvas对象中的一个对象.**       

### 15.2.7 阴影       
![15.2.71.png](http://upload-images.jianshu.io/upload_images/3001083-5d91b5afe1aee615.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**不同浏览器对阴影的支持有一些差异,在实际使用时一定要在各浏览器上进行试验.**       

### 15.2.8 渐变       
 - 渐变由CanvasGradient实例表示.       
 - createLinearGradient()       
 - createRadialGradient()       
![15.2.81.png](http://upload-images.jianshu.io/upload_images/3001083-e414d967bcf8029d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.82.png](http://upload-images.jianshu.io/upload_images/3001083-2b2b56512b34c0bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.83.png](http://upload-images.jianshu.io/upload_images/3001083-3a96f838f9520ca2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.84.png](http://upload-images.jianshu.io/upload_images/3001083-cee732618a35dd4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.85.png](http://upload-images.jianshu.io/upload_images/3001083-13c0c30496909b56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 15.2.9 模式       
 - createPattern()       
![15.2.91.png](http://upload-images.jianshu.io/upload_images/3001083-fa84a515b0897f15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.92.png](http://upload-images.jianshu.io/upload_images/3001083-5d225ef3f3bba697.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 15.2.10 使用图像数据       
 - getImageData()       
 - putImageData()       
![15.2.101.png](http://upload-images.jianshu.io/upload_images/3001083-cda5bdcb22bb0ea6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.102.png](http://upload-images.jianshu.io/upload_images/3001083-a9498ce427ae41d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**数组中的每个元素的值都介于0到255之间(包括0和255).能够直接访问到原始图像数据,就能够以各种方式来操作这些数据.**       
**在修改了imageData对象可,可以调用putImageData()方法把图像绘制到画布上.**       

### 15.2.11 合成       
 - 还有两个会应用到2D上下文中所有绘制操作的属性       
 - globalAlpha属性:指定所有绘制的透明度,默认为0.       
 - globalCompositionOperation属性:表示后绘制的图形怎样与先绘制的图形结合.       
![15.2.111.png](http://upload-images.jianshu.io/upload_images/3001083-e9e4f467e3e36ecf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.112.png](http://upload-images.jianshu.io/upload_images/3001083-8bbd09b9be3ff617.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.2.113.png](http://upload-images.jianshu.io/upload_images/3001083-d1f0ae70f5067e22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

## 15.3 WebGL       
![15.3.png](http://upload-images.jianshu.io/upload_images/3001083-144e9f77e7acd1e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**大概看了一下,略难,而且只是单纯的看书做点例子也掌握不了什么,需要具体到需要使用时去针对性地学习.**       

## 15.4 小结       
![15.41.png](http://upload-images.jianshu.io/upload_images/3001083-f6026194f2560fb6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![15.42.png](http://upload-images.jianshu.io/upload_images/3001083-ad3e96e870557928.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

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
