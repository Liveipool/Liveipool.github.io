---
layout: post
title: JavaScript红皮书学习(13)--事件
date: 2017-01-13
categories: blog
description: JavaScript学习
---

# 事件    
![13.0.png](http://upload-images.jianshu.io/upload_images/3001083-cde7503352242c90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

## 13.1 事件流    
![13.11.png](http://upload-images.jianshu.io/upload_images/3001083-5412272772febbb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**也就是说,事件流描述的是从页面中接收事件的顺序.**    

### 13.1.1 事件冒泡    
![13.1.1.png](http://upload-images.jianshu.io/upload_images/3001083-fc0c4bb8bd8a5300.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 13.1.2 事件捕获    
![13.1.21.png](http://upload-images.jianshu.io/upload_images/3001083-58e7c9531c28dd14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.1.22.png](http://upload-images.jianshu.io/upload_images/3001083-f47fb22a90afa910.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 13.1.3 DOM事件流    
![13.1.31.png](http://upload-images.jianshu.io/upload_images/3001083-930e77ee6d1de4de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.1.32.png](http://upload-images.jianshu.io/upload_images/3001083-eaffffe9f3dfae7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**一般来说,尽量在冒泡阶段处理事件!**    

## 13.2 事件处理程序    
 - 事件就是用户或浏览器自身执行的某种动作,如click等.    
 - 响应某个事件的函数就叫做**事件处理程序**(或**事件侦听器**).    
 - 事件处理程序的名字以"on"开头,因此click事件的事件处理程序就是onclick.    

### 13.2.1 HTML事件处理程序    
某个元素支持的每种事件,都可以有一个与相应事件处理程序同名的HTML特性来指定.    

``` 
<input type="button" value="click me" onclick="alert('Clicked')" />
<input type="button" value="click me" onclick="showMessage()" />
```

这样指定事件处理程序具有一些独到之处.首先,这样会创建一个封装着元素属性值的函数.这个函数中有一个局部变量event,也就是**事件对象.**    
![13.2.1.png](http://upload-images.jianshu.io/upload_images/3001083-5cf186323bd90d8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

但是,这样**指定事件处理程序**有几个缺点:     
1. 存在一个时差问题,用户可能在HTML元素一出现在页面上就触发相应的事件,但当时的事件处理程序可能还未具备执行条件.    
2.  不符合SOC的原则,HTML和JavaScript的耦合太紧.    

### 13.2.2 DOM 0级事件处理程序    
通过JavaSctript指定事件处理程序的传统方式,就是将一个函数赋值给一个事件处理程序属性.如:    

``` javascript
var btn = document.getElementById("myBtn");
btn.onclick = function() {
	alert("click");
}
```

![13.2.21.png](http://upload-images.jianshu.io/upload_images/3001083-204bca6c16ff1383.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**以这种方式添加的事件处理程序会在事件流的冒泡阶段被处理.**    

![13.2.22.png](http://upload-images.jianshu.io/upload_images/3001083-40d236ec68d913bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 13.2.3 DOM 2级事件处理程序    
![13.2.31.png](http://upload-images.jianshu.io/upload_images/3001083-14b192668e325102.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

``` javascript
	var btn = document.getElementById("myBtn");
	btn.addEventListener("click", function() {
		console.log(this.id);
	}, false);  //myBtn
```
![13.2.32.png](http://upload-images.jianshu.io/upload_images/3001083-6bc379c1c8de25e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

``` javascript
	var btn = document.getElementById("myBtn");
	btn.addEventListener("click", function() {
		console.log(this.id);
	}, false);  //myBtn
		btn.addEventListener("click", function() {
		console.log("hello world");
	}, true);   //hello world
```
**注意,上面这段代码会按照顺序输出两排,第一排是myBtn,第二排是hello world.**    

![13.2.33.png](http://upload-images.jianshu.io/upload_images/3001083-f64e8950b93bbf6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

```
	var btn = document.getElementById("myBtn");
	btn.addEventListener("click", function() {
		console.log("hello world");
	}, false);   //hello world
	btn.removeEventListener("click", function() {
		console.log("hello world");
	}, false);   //没有用,还是能够输出hello world
```

![13.2.34.png](http://upload-images.jianshu.io/upload_images/3001083-42b815bd634df23f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 13.2.4 IE事件处理程序    
![13.2.4.png](http://upload-images.jianshu.io/upload_images/3001083-187278d290e74e3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**但是,由于只有IE支持这种方法,并且它和DOM方法在一些地方有所不同,所以看一下就好了,别用.**    

### 13.2.5 跨浏览器的事件处理程序    
![13.2.51.png](http://upload-images.jianshu.io/upload_images/3001083-282af1e4e98c94e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.2.52.png](http://upload-images.jianshu.io/upload_images/3001083-ee90fdd69eb13c16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.2.53.png](http://upload-images.jianshu.io/upload_images/3001083-2dc2c4488a01782b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**然而,现代浏览器(IE9, firefox, chrome, opera, safari, edge)都已经支持DOM2事件了,所以这种需要进行能力检测的时候也不是太多,了解一下上面这一段就行了,以后实在需要考虑旧版本浏览器的时候再用.**    

## 13.3 事件对象    

![13.3.png](http://upload-images.jianshu.io/upload_images/3001083-f22f745eb7caa50d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
如下面的代码在chrome, opera中都可以,但在firefox下会报错:    

``` javascript
	btn.addEventListener("click", function() {
		console.log(event.eventPhase);
	}, false);   //2
```
这是因为firefox中的event只能在事件发生的现场使用，需要在调用函数时传入event对象:    

``` javascript
	btn.addEventListener("click", function(event) {
		console.log(event.eventPhase);
	}, false);   //2
```
**以后要使用event时尽量都将event写进函数参数中.**IE中的event使用有所不同,后面会说到.    

### 13.3.1 DOM中的事件对象    
![13.3.11.png](http://upload-images.jianshu.io/upload_images/3001083-3cc7e65da71e5320.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.3.12.png](http://upload-images.jianshu.io/upload_images/3001083-c4018ac6b2e1124d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.3.13.png](http://upload-images.jianshu.io/upload_images/3001083-ff2473ae6351a413.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.3.14.png](http://upload-images.jianshu.io/upload_images/3001083-c3239a3b85893231.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.3.15.png](http://upload-images.jianshu.io/upload_images/3001083-9a8b8a4bb101758d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.3.16.png](http://upload-images.jianshu.io/upload_images/3001083-e19aaa75f34b986c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.3.17.png](http://upload-images.jianshu.io/upload_images/3001083-45b03a3344e25704.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.3.18.png](http://upload-images.jianshu.io/upload_images/3001083-193e370bd3fbae33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**注意,只有在事件处理程序执行期间,event对象才会存在,一旦事件处理程序执行完成,event对象就会被销毁.**    


### 13.3.2 IE中的事件对象    
![13.3.21.png](http://upload-images.jianshu.io/upload_images/3001083-a1fc5a6d78324a65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.3.22.png](http://upload-images.jianshu.io/upload_images/3001083-b9f20901a5b88e10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 13.3.3 跨浏览器的事件对象    
![13.3.31.png](http://upload-images.jianshu.io/upload_images/3001083-738e1eed9849b616.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.3.32.png](http://upload-images.jianshu.io/upload_images/3001083-07aa4ec332af13ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**然而,以上代码也只是红皮书上的一个示例,由于写书至今已经发生了很多变化,所以这段代码也不一定还合适,但最重要的还是要学习这种思想和方法,在要实现一些跨浏览器的功能时,可采用这种模式,并且是重要记住,是进行能力检测,而不是浏览器检测.**    

## 13.4 事件类型    
![13.4.png](http://upload-images.jianshu.io/upload_images/3001083-14e4c69f3e6413c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**这一节的内容有需要使用时就参考官方文档: ** [https://www.w3.org/TR/uievents/](https://www.w3.org/TR/uievents/)    

### 13.4.1 UI事件    
**UI事件指的是那些不一定与用户操作有关的事件,现有的UI事件如下:**    
![13.4.1.png](http://upload-images.jianshu.io/upload_images/3001083-2aa2815d6f6e43bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
**最常用的就是load事件,其他的用得不多.**      

### 13.4.2 焦点事件    
焦点事件会在页面元素获得或时区焦点时触发.利用这些事件与document,.hasFocus()方法以及document.activeElement属性配合,可以知晓用户在页面上的行踪.    
这一类事件中最主要的两个是**focus和blur**,它们都是JavScript早期就得到所有浏览器支持的事件,但它们两个事件**最大的问题是它们不冒泡.**因此,DOM3级事件引入了两个新的事件,它们支持冒泡: **focusin和focusout**.    

### 13.4.3 鼠标与滚轮事件    
![13.4.31.png](http://upload-images.jianshu.io/upload_images/3001083-f8d1289cc81922ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.4.32.png](http://upload-images.jianshu.io/upload_images/3001083-80daa103741f426b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

鼠标事件中还有一类滚轮事件,其中也只有一个mousewheel事件,这个事件跟踪鼠标滚轮,类似于Mac的触控版.(具体使用去mdn查)    
和鼠标事件相关的一些需要了解的内容:    
**1. 客户区坐标位置: **     
鼠标时间都是在浏览器视口中的特定位置上发生的,这个位置信息保存在事件对象的clientX和clientY中.(是相对客户区的坐标,比如上面不会包括那些选项栏的高度)    

**2. 屏幕坐标位置: **    
鼠标事件发生时,不仅会有相对于浏览器窗口的位置,还有一个相对于整个电脑屏幕的位置: screenX和screenY属性.    

**3. 修改键: **    
![13.4.33.png](http://upload-images.jianshu.io/upload_images/3001083-b704fd255ec0629a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**4. 鼠标按钮: **    
![13.4.34.png](http://upload-images.jianshu.io/upload_images/3001083-05957a62803bb24b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**5. detail属性: **    
"DOM2级事件"规范在event对象中还提供了detail属性,用于给出有关事件的更多信息,对于鼠标事件来说,detail中包含了一个数值,表示在给定位置上发生了多少次单击.    

### 13.4.4 键盘与文本事件    
![13.4.41.png](http://upload-images.jianshu.io/upload_images/3001083-5cdbddcdab1e2579.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
**KeyboardEvent.key属性可以得到按下的键的值.**    

### 13.4.5 复合事件    
复合事件(composition event)是DOM3级事件中新添加的一类事件,用于处理IME的输入序列.IME(Input Method Editor, 输入法编辑器)可以让用户输入在物理键盘上找不到的字符.    
*暂时不用深究,知道有个这个东西就好.*    

### 13.4.6 变动事件    
DOM2级的变动(mutation)事件能在DOM中的某一部分发生变化时给出提示,有如下事件:    
![13.4.6.png](http://upload-images.jianshu.io/upload_images/3001083-1af7ff7c5dd6f1c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 13.4.7 HTML5事件    
DOM规范没有涵盖所有浏览器支持的所有时间,很多浏览器出于不同的目的--满足用户需求或解决特殊问题,还实现了一些自定义的事件.(下文只说一个现代浏览器都支持的事件:)    
**hashchange 事件: **    
![13.4.7.png](http://upload-images.jianshu.io/upload_images/3001083-1885b84dd9eb8e7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 13.4.8 设备事件    
![13.4.8.png](http://upload-images.jianshu.io/upload_images/3001083-2e8de6d2f3659afb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

*暂时还没有这方面的开发经历,以后可以在具体项目中试一下.*    

### 13.4.9 触摸与手势事件    
*也需要在以后的项目中进行实践.*    

## 13.5 内存和性能    
![13.5.png](http://upload-images.jianshu.io/upload_images/3001083-2a86e70c4198f71d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 13.5.1 事件委托    
![13.5.12.png](http://upload-images.jianshu.io/upload_images/3001083-db777012949285fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.5.13.png](http://upload-images.jianshu.io/upload_images/3001083-08933debd3821bb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.5.14.png](http://upload-images.jianshu.io/upload_images/3001083-ed5be0aa8bfef584.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.5.15.png](http://upload-images.jianshu.io/upload_images/3001083-1272f671fda30afd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**我在studying-fram里面试着写了一下,为每个li分别添加事件的写法时的处理时间: **    
![13.5.11.png](http://upload-images.jianshu.io/upload_images/3001083-665f5a11381e2d15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**改为事件委托写法之后,整个时间大概少了30ms,由于现在的例子中操作的DOM的数量不多,所以区别不大,但还是能看出有一些区别的,当项目大了之后这个的影响就会很大,因此要学会事件委托这种写法.**    

### 13.5.2 移除事件处理程序    
![13.5.21.png](http://upload-images.jianshu.io/upload_images/3001083-bd9b53eee7d6b32d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![13.5.22.png](http://upload-images.jianshu.io/upload_images/3001083-57ec54a729935806.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

## 13.6 模拟事件    
![13.6.png](http://upload-images.jianshu.io/upload_images/3001083-5076cd8d73e6ed6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**看了之后感觉并不是太好用,也不知道在哪些情况下比较适用,感觉上是在开发时可以用来调试,但有点麻烦不如自己进行事件,了解一下就好.**    

## 13.7 小结    
![13.7.png](http://upload-images.jianshu.io/upload_images/3001083-77c00aecfcdf1e8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

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