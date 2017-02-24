---
layout: post
title: JavaScript红皮书学习(8)--BOM
date: 2016-12-25
categories: blog
description: JavaScript学习
---

# BOOOOOOOOM     
ECMAScript是JavaScript的核心，但如果要在**Web浏览器中使用JavaScript**，那么BOM（浏览器对象模型 Browser Object Module）则无疑才是真正的核心。BOM提供了很多对象，**用于访问浏览器的功能**，这些功能与任何网页的内容无关。       
多年来，缺少规范倒是BOM既有意思又有问题，因为浏览器提供商会按照各自的想法去随意扩展它。于是，浏览器之间共有的对象就成为了事实上的标准。这些对象在浏览器中得以存在，很大程度上是由于它们提供了浏览器的互操作性。W3C为了把浏览器中的JavaScript最基本的部分标准化，已经把BOM的主要方面纳入了HTML5的规范中。     

## 8.1 window对象        
BOM的核心对象是window，它表示**浏览器的一个实例**。         
在浏览器中，window对象有双重角色，它既是**通过JavaScript访问浏览器的一个接口**，又是**ECMAScript规定的Global对象。**     这意味着在网页中定义的任何一个对象、变量和函数，都以window作为其Global对象。       

### 8.1.1 全局作用域       
由于window对象同时扮演着ECMAScript中Global对象的角色,因此所有在全局作用域中声明的变量、函数都会变成window对象的属性和方法。      

``` javascript
var age = 29;
function say() {
	consolo.log(this.age);
}

alert(window.age);   //29
sayAge();            //29
window.sayAge();     //29
```

抛开全局变量会称为window对象的属性不谈，定义全局变量与在window对象上直接定义属性还是有一点区别：**全局变量不能通过delete操作符删除，而直接在window对象上的定义的属性可以。**     

``` javascript
var age = 29;
window.color = "red";
delete window.age;
delete window.color;

alert(window.age);     //29
alert(window.color);   //undefinded
```

刚才是用var语句添加的window属性有一个名为[[Configurable]]的特性，这个特性的值被设置为false，因此这样定义的属性不可以通过delete操作符删除。        
另外，尝试未声明的变量会抛出错误，但是通过查询window对项，可以知道某个可能未声明的变量是否存在。     

``` javascript
console.log(newValue);         //报错
console.log(window.newValue);  //undefined
```

### 8.1.2 窗口位置       
  有四个表示位置的属性，screenX、screenLeft、screenTop、screenY，它们在不同的浏览器上受支持的程度不同，值也不太确定，用处不大。     
  还有两个移动位置的方法，moveTo()和moveBy(),但基本被浏览器禁用了.     

### 8.1.3 窗口大小      
 - 跨浏览器确定一个窗口的大小不是一个简单的事.在主流的IE9+、FireFox、Safari、Opera、Chrome有六个属性可以显示浏览器的相关大小信息，可不同的属性在不同浏览器上值也不同，属性有：innerWidth、innerHeight、outerWidth、outerHeight（这四个都是window的属性）、document.documentElement.clientHeight、document.documentElement.clientWidth。     
 - 另外还有两个方法说可以调整浏览器窗口的大小，resizeTo()resizeBy()很多浏览器都将它们禁用了。     
 - innerWidth：浏览器视口（viewport）宽度，如果存在垂直滚动条则包括它。不属于标准。            
 - outerHeight：  获取整个浏览器窗口的高度，包括侧边栏（如果存在）、窗口镶边和窗口调正边框。只读，没有默认值。不属于标准。          
 - clientHeight: 返回**元素**内部的高度(单位像素)，包含内边距，但不包括水平滚动条、边框和外边距。clientHeight 可以通过 CSS height + CSS padding - 水平滚动条高度 (如果存在)来计算。暂不属于标准，在CSSDOM的草案中。          

### 8.1.4 导航和打开窗口       
使用window.open()方法可以导航到一个特定的URL,也可以打开一个新的浏览器窗口.这个方法可以接收四个参数:1.要加载的URL 2.窗口目标 3.一个特性字符串 4.一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值.       
这种方法很可能会被浏览器带的一些扩展程序屏蔽掉.试了一下,Chrome还是可以用这个方法的.     

### 8.1.5 间歇调用和超时调用     
![8.1.6.png](http://upload-images.jianshu.io/upload_images/3001083-3bb119fdb6ebdc81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

间歇调用与超时调用相似,只不过它会按照指定的时间间隔重复执行代码,直至间歇调用被取消或者页面被卸载.  
间歇调用也可以用超时调用来实现:     

![8.1.62.png](http://upload-images.jianshu.io/upload_images/3001083-d0b4531af0fd810e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 8.1.6 系统对话框        
![8.1.71.png](http://upload-images.jianshu.io/upload_images/3001083-cbb5b9f9a8bec4a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![8.1.72.png](http://upload-images.jianshu.io/upload_images/3001083-4cc4bc3da4e069ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![8.1.73.png](http://upload-images.jianshu.io/upload_images/3001083-c03007149d9fd703.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     


## 8.2 location对象
![8.21.png](http://upload-images.jianshu.io/upload_images/3001083-339a159f89c2fe1b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![8.22.png](http://upload-images.jianshu.io/upload_images/3001083-65dcdf73a81200c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![8.2.2.png](http://upload-images.jianshu.io/upload_images/3001083-43a540e095cc39fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![8.2.22.png](http://upload-images.jianshu.io/upload_images/3001083-afe8ec6e3266d422.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![8.2.23.png](http://upload-images.jianshu.io/upload_images/3001083-eae6a9c3b83d0f99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 8.3 navigator 对象     
![8.3.png](http://upload-images.jianshu.io/upload_images/3001083-a6fc747f0a8436a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 8.4 history 对象     
![8.51.png](http://upload-images.jianshu.io/upload_images/3001083-d1f6206c4c9166c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![8.52.png](http://upload-images.jianshu.io/upload_images/3001083-fa242c17f897864a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
*还有一个window.screen对象,不过确实作用不大,就不说了.*     

## 8.5 小结
![8.6.png](http://upload-images.jianshu.io/upload_images/3001083-35bad8a3c45dd730.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

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
