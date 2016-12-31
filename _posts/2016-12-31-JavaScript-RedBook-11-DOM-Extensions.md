---
layout: post
title: JavaScript红皮书学习(11)--DOM 扩展
date: 2016-12-31
categories: blog
description: JavaScript学习
---

# DOM 扩展       
 - 尽管DOM1级作为API已经非常完善了,但为了实现更多的功能,仍然会有一些标准或专有的扩展出现.2008年之前,浏览器中几乎所有的DOM扩展都是浏览器专有的.此后,W3C着手将一些浏览器专有扩展标准化并写入规范当中.       
 - 以下要提到的功能大部分是已经成为标准的,也有一部分是浏览器专有的,具体哪些是标准哪些是专有我也会说明,但这是会根据时间的推移而变化的,所以最终都还是以标准文档为准[www.w3.org](www.w3.org) .       
 - 对DOM的两个主要的扩展是Selectors API(选择符API)和HTML5.       

## 11.1 选择符API       
![11.1.png](http://upload-images.jianshu.io/upload_images/3001083-21b21d7ca88a2afb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 11.1.1 querySelector()方法       
querySelector()方法接收一个CSS选择符,返回与该模式匹配的**第一个元素**,如果没有找到匹配的元素,返回null.       
![11.1.1.png](http://upload-images.jianshu.io/upload_images/3001083-c5b2c4f85f31257f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 11.1.2 querySelectorAll()方法       
querySelectorAll()方法接收的参数也是一个CSS选择符,但返回的是**所有匹配的元素.**这个方法返回的是一个**NodeList的实例.**       
![11.1.2.png](http://upload-images.jianshu.io/upload_images/3001083-f4b7f8406a426764.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 11.1.3 matches()方法       
![11.1.3.png](http://upload-images.jianshu.io/upload_images/3001083-ed21aef71bbb1a9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
**但是,至今还没有浏览器支持matchesSelector(),但几乎所有现在的浏览器都支持matches()方法,各浏览器通过加一些前缀如msMatchesSelector()或webkitMatchesSelector()方法来使用该功能,但最好就用matches()方法,功能相同,又通用.**       

## 11.2 HTML5       
![11.3.png](http://upload-images.jianshu.io/upload_images/3001083-aed6e520768e97be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 11.2.1 与类相关的扩充       
HTML5新增了很多API,,致力于简化CSS类的用法.       
**1.getElementByClassName()方法:**       
HTML5添加的getElementByClassName()方法是最受人欢迎的一个方法,可以通过document对象及所有HTML元素调用该方法.该方法返回带有指定类的所有元素的NodeList.传入多个类名时,类名的先后顺序不重要.       

**2.classList属性**       
![11.3.11.png](http://upload-images.jianshu.io/upload_images/3001083-a250a36a17388066.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.3.12.png](http://upload-images.jianshu.io/upload_images/3001083-72603ebdea6f0f63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
**至今,只有部分浏览器支持该属性,具体支持请参见[https://developer.mozilla.org/en-US/docs/Web/API/Element/classLis](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)**       

### 11.2.2 焦点管理       
**1. document.activeElment属性**       
![11.3.21.png](http://upload-images.jianshu.io/upload_images/3001083-37eb8d5b653d9d62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**2. document.hasFocus()方法**       

``` javascript
var button = document.getElementById("myButton");
console.log(button.hasFocus());    //...
```
![11.3.22.png](http://upload-images.jianshu.io/upload_images/3001083-c5f82b7d27db296a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 11.2.3 HTMLDocument的变化       
![11.3.31.png](http://upload-images.jianshu.io/upload_images/3001083-436cba540c10efc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**1. readyState属性**       
![11.3.32.png](http://upload-images.jianshu.io/upload_images/3001083-59f34f6e0193da7f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**2. 兼容模式**       
![11.3.33.png](http://upload-images.jianshu.io/upload_images/3001083-7da78daa4e8fdc83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**3. head属性**       
![11.3.34.png](http://upload-images.jianshu.io/upload_images/3001083-5855eed288a0d677.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
**至今,还不是所有在用的浏览器都支持该属性,具体支持查看: [https://developer.mozilla.org/en-US/docs/Web/API/Document/head](https://developer.mozilla.org/en-US/docs/Web/API/Document/head), 如果不可用,就使用getElementByTagName()方法.**       

### 11.2.4 字符集属性       
![11.3.4.png](http://upload-images.jianshu.io/upload_images/3001083-59a4786ec3060139.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**试了一下,在最新的chrome, firefox, opera上都可以有document.charset,但document.defaultCharset都是undefined. 在MDN上也查不到这两个属性,了解一下就行了,应该也不太常用.**       

### 11.2.5 自定义数据属性       
![11.3.5.png](http://upload-images.jianshu.io/upload_images/3001083-d3f59030b962866d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**如果需要给元素添加一些不可见的数据以便进行其他树立,那就要用到自定义数据属性.在跟踪链接或混搭应用中,通过自定义数据属性能方便地知道点击来自页面中的哪个部分.**       

### 11.2.6 插入标记       
![11.3.61.png](http://upload-images.jianshu.io/upload_images/3001083-5bc38dc087942ba0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.3.62.png](http://upload-images.jianshu.io/upload_images/3001083-3c6ed867ed681045.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
如:       

``` javascript
	node1.innerHTML = "Hello & welcome, <b>\"reader\"!</b>"
	console.log(node1.innerHTML);  //Hello &amp; welcome, <b>"reader"!</b>
```
![11.3.63.png](http://upload-images.jianshu.io/upload_images/3001083-93b0f184078f60e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.3.64.png](http://upload-images.jianshu.io/upload_images/3001083-510e5266187bb479.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.3.65.png](http://upload-images.jianshu.io/upload_images/3001083-21bdc310358a205e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.3.66.png](http://upload-images.jianshu.io/upload_images/3001083-60ec42437ae424e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.3.67.png](http://upload-images.jianshu.io/upload_images/3001083-716c1e7fbf762fe7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 11.2.7 scrollIntoView()方法       
![11.3.7.png](http://upload-images.jianshu.io/upload_images/3001083-faa91264bfb2b430.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

## 11.3 专有扩展       
![11.4.png](http://upload-images.jianshu.io/upload_images/3001083-e62a07c5c98de945.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**即便如此,仍然还有大量的专有的DOM扩展还没有成为标准,但这并不代表它们将来不会被写进标准,只是在写本篇博客的时候,它们还是专有功能,只得到了少数浏览器的支持.**       

### 11.3.1 文档模式       
![11.4.11.png](http://upload-images.jianshu.io/upload_images/3001083-e1edc09fc79768ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.4.12.png](http://upload-images.jianshu.io/upload_images/3001083-d467c31eecc225bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 11.3.2 children属性 (	现在已经成了标准属性)       
![11.4.2.png](http://upload-images.jianshu.io/upload_images/3001083-538c4a00742e91c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

支持children属性的浏览器参见: [https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/children](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/children).(IE8及更早版本的children属性中也会包含注释节点,但IE9之后的版本则只返回元素节点)       

### 11.3.3 contains()方法 (现在已经成了标准方法)       
![11.4.31.png](http://upload-images.jianshu.io/upload_images/3001083-e811a88e44112987.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.4.32.png](http://upload-images.jianshu.io/upload_images/3001083-f3678df7b204ffad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.4.33.png](http://upload-images.jianshu.io/upload_images/3001083-f3b62e05afb56514.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 11.3.4 插入文本       
![11.4.41.png](http://upload-images.jianshu.io/upload_images/3001083-e978154b1ca2a96d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
IE, Opera, Safari, Chrome都较早的实现了innerText属性,MDN上说Firefox在版本45之后也实现了这个属性,但我用50.1版本的Firefox使用这属性输出也是undefined.       
和这个属性有一个功能相似的属性: textContent属性,该属性在五大浏览器上较早都实现了.       
![11.4.42.png](http://upload-images.jianshu.io/upload_images/3001083-4813dc63c3c3e2c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**现在innerText已经成为了标准,但outerText并没有成为标准.**       

### 11.3.5 滚动       
![11.4.51.png](http://upload-images.jianshu.io/upload_images/3001083-f76eaa56e749154e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![11.4.52.png](http://upload-images.jianshu.io/upload_images/3001083-34a427f887bfd7b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

由于这些方法是非标准的,支持的浏览器也不全,而所有浏览器都支持的标准方法scrollIntoView()方法基本都能满足要求,所以尽量还是用scrollIntoView()方法.       

## 11.4小结       
 - 虽然DOM为HTML文档交互制定了一系列核心API,但仍然有几个规范对标准的DOM进行了扩展.这些扩展中很多原来是浏览器专有的,但后来称为了事实标准,于是其他浏览器也都提供了相同的实现.主要讲了三部分内容:       
 - Selectors API(标准): 定义了几个方法,让开发人员能够基于CSS选择符从DOM中取得元素.       
 - HTML5(标准): 为标准的DOM定义了很多扩展功能.其中包括在innerHTML属性这样的事实标准基础上提供的标准定义,以及为管理焦点,设置字符集,滚动页面等而规定的扩展API.       
 - 专有扩展(尚未成为标准): 这些标准有它们独特的作用,它们已经被很多浏览器所实现,只是尚未成为标准,但只它们获得了大家的认可,说不定在不久的将来,它们就会被收录到规范的更新版本中.       
