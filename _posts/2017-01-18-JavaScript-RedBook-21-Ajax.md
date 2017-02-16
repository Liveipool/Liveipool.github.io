---
layout: post
title: JavaScript红皮书学习(21)--Ajax
date: 2017-01-18
categories: blog
description: JavaScript学习
---


# Ajax     
2005年,Jeses James Garrett发表了一篇文章,题为: [Ajax: A new Approach to Web Applications](http://adaptivepath.org/ideas/ajax-new-approach-web-applications/) . 它在文章中讲道: **Ajax不只是一项技术,它是多项技术的组合.** 而也就是这篇文章,将Web引领向了新时代.     

(Ajax定义的原文):     

> Ajax isn’t a technology. It’s really several technologies, each flourishing in its own right, coming together in powerful new  ways. Ajax incorporates:     
> standards-based presentation using XHTML and CSS;     
> dynamic display and interaction using the Document Object Model;     
> data interchange and manipulation using XML and XSLT;     
> asynchronous data retrieval using XMLHttpRequest;     
> and JavaScript binding everything together.     

(当然,上面的技术是对应Garrett在2005年写文章时的技术,目前已有部分变化.)     

Ajax是对**Asynchronous JavaScript + XML**的简写.这一技术能够向服务器请求额外的数据而无须卸载页面,会带来更好的用户体验.     
传统的Web应用允许用户端填写表单（form），当提交表单时就向网页服务器发送一个请求。服务器接收并处理传来的表单，然后送回一个新的网页，但这个做法浪费了许多带宽，因为在前后两个页面中的大部分HTML码往往是相同的。由于每次应用的沟通都需要向服务器发送请求，应用的回应时间依赖于服务器的回应时间。这导致了用户界面的回应比本机应用慢得多。     
与此不同，AJAX应用可以仅向服务器发送并取回必须的数据，并在客户端采用JavaScript处理来自服务器的回应。因为在服务器和浏览器之间交换的数据大量减少（大约只有原来的5%）[来源请求],服务器回应更快了。同时，很多的处理工作可以在发出请求的客户端机器上完成，因此Web服务器的负荷也减少了。     
Ajax技术的核心是XMLHttpReqeust对象(简称XHR),这是由微软首先引入的一个特性,其他浏览器提供商后来都提供了相同的实现.     
**虽然名字中包含有XML的成分,但Ajax通信与数据格式无关,这种技术就是无须刷新页面即可从服务器取得数据,但不一定是XML数据,实际上,数据格式可以由JSON代替，进一步减少数据量，形成所谓的AJAJ.**     

**Web应用的传统模式和Ajax模式的对比:**     
![21.01.png](http://upload-images.jianshu.io/upload_images/3001083-43c6ee172d72927d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.02.png](http://upload-images.jianshu.io/upload_images/3001083-ba7cb0b3e1725686.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.03.png](http://upload-images.jianshu.io/upload_images/3001083-99c74e43c79f77ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     


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
