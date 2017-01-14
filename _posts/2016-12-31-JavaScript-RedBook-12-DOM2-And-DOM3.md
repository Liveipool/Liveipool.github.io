---
layout: post
title: JavaScript红皮书学习(12)--DOM2 和 DOM3
date: 2016-12-31
categories: blog
description: JavaScript学习
---

# DOM2 和 DOM3      
DOM1级主要定义的是HTML文档的底层结构.DOM2 和 DOM3级则在这个结构的基础上引入了更多的交互能力.为此,DOM2 和 DOM3级分为许多模块(模块之间具有某种关联),分别描述了DOM的某个非常具体的子集,这些模块为:      

![12.0.png](http://upload-images.jianshu.io/upload_images/3001083-746a88d915ac3b4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

DOM2级和3级的目的在于扩展DOM API,同时提供更好的错误处理及特性检测能力."DOM2级核心"没有引入新类型,它只是在DOM1级的基础上通过增加新方法和新属性来增强了既有类型."DOM3级核心"同样增强了既有类型,但也引入了一些新类型.      
下文只是介绍DOM2和DOM3的**样式**,**遍历**和**范围**模块部分的一些有用的扩展(下一部分会介绍**事件**),其余部分参考官方文档.      

## 12.1 样式      

### 12.1.1 访问元素的样式      
![12.2.11.png](http://upload-images.jianshu.io/upload_images/3001083-afc046b9cef30699.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.2.12.png](http://upload-images.jianshu.io/upload_images/3001083-a85da328996138da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**注意,因为以上都是style对象的方法,操作的全都是为元素设置的style属性里写的CSS样式内容,而且有少数的方法浏览器的兼容性不好,在使用时一定要考虑这些问题.**      
![12.2.13.png](http://upload-images.jianshu.io/upload_images/3001083-00fda90be0f794b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
### 12.1.2 操作样式表      
![12.2.21.png](http://upload-images.jianshu.io/upload_images/3001083-150aefdd166bfbeb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.2.22.png](http://upload-images.jianshu.io/upload_images/3001083-f5bfca471f25cfcd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.2.23.png](http://upload-images.jianshu.io/upload_images/3001083-11b8222dd67084d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.2.24.png](http://upload-images.jianshu.io/upload_images/3001083-e762df36e6387c40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 12.1.3 元素大小      
![12.2.31.png](http://upload-images.jianshu.io/upload_images/3001083-e1a6c6323ea3b81b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.2.32.png](http://upload-images.jianshu.io/upload_images/3001083-2cde82609499e5a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.2.33.png](http://upload-images.jianshu.io/upload_images/3001083-bff743f2bc8acb7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**注意,所有这些偏移量属性都是只读的,而且每次访问它们都需要重新计算.因此,应该尽量避免重复访问这些属性.如果需要重复使用其中某些属性的值,可以将它们保存在局部变量中,以提高性能.**      
![12.2.34.png](http://upload-images.jianshu.io/upload_images/3001083-cd7ebc3998f2819d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![12.2.35.png](http://upload-images.jianshu.io/upload_images/3001083-03bfd84c47f4e0b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**注意,这几个属性在不同的浏览器中返回的值很可能会不同.**      
**scrollTop 和 scrollLeft属性都是可以设置的,比如可以这样写一个函数用于检测元素是否位于顶部,如果不是就将其滚回到顶部:**      

``` javascript
function scrollToTop(element) {
	if (element.scrollTop != 0) {
		element.scrollTop = 0;
	}
}
```
**但是,还是要记住这些属性在不同浏览器中返回的值不同,所以使用时要格外注意.**      

**4. 确定元素大小**      
各浏览器还提供了一个getBoundingClientRect()方法.这个方法返回一个对象,对象中可能有多个属性:left, top, right, bottom, width, height(现在firefox还有x和y),在不同的浏览器中的支持也不同,使用时,需要在各浏览器上进行测试.      

## 12.2 遍历      
"DOM2级遍历和范围"模块定义了两个用于辅助完成顺序遍历DOM结构的类型: NodeIterator 和 TreeWalker.这两个类型能够基于给定的起点对DOM结构执行深度优先(depth-first)的遍历操作.      
![12.31.png](http://upload-images.jianshu.io/upload_images/3001083-b806dd3ed171c04b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.32.png](http://upload-images.jianshu.io/upload_images/3001083-baae6cbf407e59e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 12.2.1 NodeIterator      
![12.3.11.png](http://upload-images.jianshu.io/upload_images/3001083-354bf55cb675242a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.3.12.png](http://upload-images.jianshu.io/upload_images/3001083-b363a86556ca3393.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.3.13.png](http://upload-images.jianshu.io/upload_images/3001083-ba317f890d1e2bd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.3.14.png](http://upload-images.jianshu.io/upload_images/3001083-8ec42d6ded12fcca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.3.15.png](http://upload-images.jianshu.io/upload_images/3001083-eddf81db34f0d79e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 12.2.2 TreeWalker      
![12.3.21.png](http://upload-images.jianshu.io/upload_images/3001083-6490104948554cb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

创建TreeWalker对象要使用document.createTreeWalker()方法,它接受的四个参数和document.createNodeIterator()方法相同.      而它和NodeIterator主要的区别在于:      
1. 第三个参数filter可以返回的值更多.      
2. 能够在DOM结构中沿任何方向移动.      
3. 有一个currentNode属性,表示在最近一次遍历中返回的节点,使用它还可以随时修改遍历继续进行的起点.      

总而言之,与NodeIterator想比,TreeWalker类型在遍历DOM时拥有更大的灵活性,但由于IE似乎在9+版本才支持这两个对象,所以暂时使用得还不太多.      

## 12.3 范围      
![12.41.png](http://upload-images.jianshu.io/upload_images/3001083-fccde3f2be2f2aba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 12.3.1 DOM中的范围      
![12.4.11.png](http://upload-images.jianshu.io/upload_images/3001083-2442447d2cef7779.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.12.png](http://upload-images.jianshu.io/upload_images/3001083-10b0b757e5258097.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.13.png](http://upload-images.jianshu.io/upload_images/3001083-e24b29c9b916aa0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.14.png](http://upload-images.jianshu.io/upload_images/3001083-9b3c1ad99113d513.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.15.png](http://upload-images.jianshu.io/upload_images/3001083-178b8fb5c6068ef4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.16.png](http://upload-images.jianshu.io/upload_images/3001083-a1fccbc58fc3f47d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.17.png](http://upload-images.jianshu.io/upload_images/3001083-b9f11381dc2e82f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.18.png](http://upload-images.jianshu.io/upload_images/3001083-a895cff89f9695a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.19.png](http://upload-images.jianshu.io/upload_images/3001083-a95e7f2d0e08866a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.20.png](http://upload-images.jianshu.io/upload_images/3001083-44c008c500410040.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.21.png](http://upload-images.jianshu.io/upload_images/3001083-5df0b81368e2306d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.22.png](http://upload-images.jianshu.io/upload_images/3001083-902e8503af319c1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![12.4.23.png](http://upload-images.jianshu.io/upload_images/3001083-24fac269ff9db7d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 12.3.2 IE8及更早版本中的范围      
这一部分,留到专门检测IE各种bug那一部分来理解.      

## 12.4 小结      
![12.5.png](http://upload-images.jianshu.io/upload_images/3001083-34cd3d38618891dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      


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