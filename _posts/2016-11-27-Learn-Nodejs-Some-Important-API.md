---
layout: post
title: Node学习摘要之一些重要的API 
date: 2016-11-27
categories: blog
tags: [Nodejs]
description: Nodejs学习摘要
---

# Node学习摘要之一些重要的API  
 - 处理进程（stdio）的stdin和stdout， 以及一些关于文件系统（fs）的API。  
 - fs模块是唯一一个同时提供同步和异步API的模块。    

理解什么是流（stream）：
![Node stream1.png](http://upload-images.jianshu.io/upload_images/3001083-f3819912143fa6de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![Node stream2.png](http://upload-images.jianshu.io/upload_images/3001083-83907ebeb0547be0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 一些API：      
 - process.argv：包含了所有Node程序运行时的参数值。    
 - process.cwd：获取当前的工作目录。    
 - process.env：可用来轻松访问shell环境下的变量。比如，一个最常见的环境变量就是NODE_ENV，该变量用来控制程序是运行在开发模式下还是产品模式下。    
 - process.exit：用于退出程序，最好使用退出代码1, process.exit（1）。    
 - process.on：事件分发。        
 - 信号：进程和操作系统进行通信的其中一种方式就是通过信号。比如要想让进程终止，可以发送SIGKILL信号：process.on('SIGKILL', function(){ //信号已收到 }) ;      

对fs的一些深入理解：    
![Node fs1.png](http://upload-images.jianshu.io/upload_images/3001083-ecde37ac9e78dfee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![Node fs2.png](http://upload-images.jianshu.io/upload_images/3001083-8f5a1b9766b079f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![Node fs3.png](http://upload-images.jianshu.io/upload_images/3001083-7c1b09e32652447a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   

### TCP    
 - 大部分时候，连接如MySQL等的数据库以及与数据库进行通信使用的都是TCP套接字。Node HTTP服务器是构建于Node TCP服务器之上的。从编程的角度来说，也就是Node中的http.Server继承自net.Server（net是TCP模块）。   
 - 要是用Node来写一个TCP服务器，就完全没必要去考虑TCP复杂的内部实现了。只要考虑连接以及往套接字中写数据即可。   
 - TCP对字符以及字符编码是完全不之的，不同的编码会导致传输的字节数不同。所以TCP允许数据以ASCⅡ字符（每个字符一个字节）或者Unicode（每个字符四个字节）进行传输。正是因为对消息格式没有严格的约束，使得TCP有很好的灵活性。  
 - 书上讲解了一个简单的基于TCP的聊天程序的写法。（创建了一个TCP服务器，可通过telnet在终端建立一个TCP连接以加入聊天室），代码如下：    
![Node tcp-chat.png](http://upload-images.jianshu.io/upload_images/3001083-5e7cb87dc88ef6b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### HTTP   
 - HTTP协议构建在请求和响应的概念上，对应在Node.js中就是由http.ServerRequest和http.ServerResponse这两个构造器构造出来的对象。   
 - HTTP在请求和响应消息前使用投信系（header）来描述不同的消息内容：   
![Node header1.png](http://upload-images.jianshu.io/upload_images/3001083-870d54d1bc5baa19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![Node header2.png](http://upload-images.jianshu.io/upload_images/3001083-3dc1dd77bca2434a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![Node header3.png](http://upload-images.jianshu.io/upload_images/3001083-aa41f4df198c319e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

Transfer-Encoding头信息的默认值是chunked（分块），主要的原因是Node天生的异步机制，这样响应就可以逐步产生。    
![Node header4.png](http://upload-images.jianshu.io/upload_images/3001083-ae6925eee3558286.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![Node header5.png](http://upload-images.jianshu.io/upload_images/3001083-0e066afe850698f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   

对于TCP服务器和HTTP服务器，它们都调用了createServer方法，并且当客户端连入时会执行一个回调函数。但它们有个本质的区别，即回调函数中对象的类型。在net服务器中，是个连接（connection）对象，而在HTTP服务器中，是请求和响应对象。   
这一章节下面的内容讲了构建一个简单的表单提交Web服务器，讲述了如何在表单提交时传递编码过的数据，以及如何将它们解析为JavaScript中的数据结构。还介绍了用于浏览器和服务器之间消息传递的编码，以及Node提供了哪些工具来对数据进行解析和处理等等。
