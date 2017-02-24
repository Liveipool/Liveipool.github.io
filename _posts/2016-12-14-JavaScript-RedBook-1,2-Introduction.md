---
layout: post
title: JavaScript红皮书学习(1,2)--JavaScript简介
date: 2016-12-14
categories: blog
tags: [JavaScript]
description: JavaScript学习
---

# JavaScript简介    

## 1. JavaScript简介  
JavaScript诞生于1995年，当时，它的主要目的是处理以前由服务器语言（如perl）负责的一些输入验证操作，因为在JavaScript问世之前，必须把表单数据发送到服务器端才能确定用户是否正确填写了表单。  
如今，JavaScript的用途变得非常强大，它具备了与浏览器窗口及其内容等几乎所有方面交互的能力。   

### 1.1 JavaScript简史  
![1.1.png](http://upload-images.jianshu.io/upload_images/3001083-d143caa96218073c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   

概括来说，就是1995年Netscape公司为了给自己的Navigator 2浏览器开发一门脚本语言，创造了JavaScript，它本来是叫LiveScript，但由于想沾上当时火热的Java的光，便改名为JavaScript。Netscape公司在Navigator3中发布了JavaScript1.1。由于当时微软也在IE3中加入了名为JScript的JavaScript实现，大家担心不同的JavaScript实现会使这门语言走向分裂，Netscape公司将JavaScript1.1提交给ECMA，ECMA经过数月完成了ECMA262--定义了一种名为ECMAScript的新脚本语言的标准。至此以后，各浏览器中的JavaScript实现都是基于ECMAScript的。    
即，**ECMAScript是标准，JavaScript是一种实现**。 

### 1.2 JavaScript实现  
![1.21.png](http://upload-images.jianshu.io/upload_images/3001083-08ddd958d7ddb04b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![1.22.png](http://upload-images.jianshu.io/upload_images/3001083-db9697b8ca87819d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![1.23.png](http://upload-images.jianshu.io/upload_images/3001083-b3944fa53aae94cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![1.24.png](http://upload-images.jianshu.io/upload_images/3001083-810f0f4adba3947a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![1.25.png](http://upload-images.jianshu.io/upload_images/3001083-ebcbe4a6ea101576.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
ECMAScript 1：1997年06月 首版
ECMAScript 2：1998年06月 格式修正，以使得其形式与ISO/IEC16262国际标准一致
ECMAScript 3：1999年12月 强大的正则表达式，更好的文字链处理，新的控制指令，异常处理，错误定义更加明确，数输出的格式化及其它改变

**2015年6月,ES6(也叫ES2015)正式发布了.**  

#### 1.2.2 文档对象模型（Document Object Model， DOM）   
![1.2.2.png](http://upload-images.jianshu.io/upload_images/3001083-a6a6db373b2c15a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   

![1.2.22.png](http://upload-images.jianshu.io/upload_images/3001083-d554509310f86591.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        

![1.2.23.png](http://upload-images.jianshu.io/upload_images/3001083-d83e6521085e1b18.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

![1.2.24.png](http://upload-images.jianshu.io/upload_images/3001083-8abf21ee30de6ddb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)            

#### 1.2.3 浏览器对象模型（Browser Object Model， BOM）  
![1.2.3.png](http://upload-images.jianshu.io/upload_images/3001083-3c2afc39e54639d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 1.3 JavaScript版本    
![1.3.png](http://upload-images.jianshu.io/upload_images/3001083-8865cef104063dae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
**值得注意的是，只有Netscape/Mozilla浏览器才遵循这种编号模式。例如，IE的JScript就采用了另一种版本命名方案。因此，在大多数浏览器提及对JavaScript的支持情况时，一般都以ECMAScript兼容性和对DOM的支持情况为准。**   

### 1.4 小结   
JavaScript是一种专为网页交互而设计的脚本语言，由下列三个不同的部分组成：   
1. ECMAScript，由ECMA-262定义，提供核心语言功能。    
2. 文档对象模型（DOM），提供访问和操作网页内容的方法和接口。   
3. 浏览器对象模型（BOM），提供与浏览器交互的方法和接口。     

## 2. 在HTML中使用JavaScript   

### 2.1 < script >元素    
向HTML页面中插入JavaScript的主要方法，就是使用< script >元素，这个元素有6个属性：   
1. async：可选。目的是不让页面等待JavaScript代码的下载和执行，从而异步加载页面其他内容。   
2. charset：可选。   
3. defer：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。  
4. language：已废弃。  
5. src：可选。表示包含要执行代码的外部文件。  
6. type：可选。表示编写代码使用的脚本语言的内容类型（也称为MIME类型）。虽然text/javascript和text/   ecmascript都已经不被推荐使用，但人们一直依赖使用的都还是text/javascript。实际上，**服务器在传送JavaScript文件时使用的MIME类型通常是application/x-javascript。**考虑到约定俗成和最大限度的浏览器兼容性，目前type属性的值依旧还是text/javascript。不过，这个属性不是必需的，不指定的话，默认也是text/javascript。  

*一般来说，只用第5、6个属性。*     

#### 2.1.1 标签的位置   
在文档的< head >元素中包含所有的JavaScript文件，意味着必须等到全部JavaScript代码都被下载、解析和执行完成以后，才能开始呈现页面的内容（浏览器在遇到< body >标签时才开始呈现内容）。对于哪些需要很多JavaScript代码的页面来说，这无疑会导致浏览器在呈现页面时出现明显的延迟，而延迟期间的浏览器窗口中将是一片空白。为了避免这个问题，现代Web应用程序一般都把全部JavaScript引用放在< body >元素中页面内容的后面。    

### 2.3 文档模式   
IE5.5引入了文档模式的概念，而这个概念是通过使用文档类型（doctype）切换实现的。最初的两种文档模式是：*混杂模式*和*标准模式*，混杂模式会让IE的行为与（包含非标准特性）IE5相同，而标准模式则让IE的行为更接近标准行为。 
HTML5标准模式：  

    <!-- HTML 5 -->     
    <!DOCTYPE html>     


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
