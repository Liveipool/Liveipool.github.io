---
layout: post
title: Express学习之三大核心--路由、中间件、模板引擎
date: 2016-12-13
categories: blog
tags: [node.js]
description: Express学习
---


# Express学习之三大核心--路由、中间件、模板引擎
Express：高度包容、快速而极简的 Node.js Web 框架。   
鉴于Connect（一个基于HTTP服务器的工具集）基于HTTP模块提供了开发Web应用常用的基础功能，Express基于Connect为构建整个网站以及Web应用提供了更为方便的API。（从V4.x起，Express不再依赖于Connect）    
##### Express是基于nodejs的web开发框架。优点是易上手、高性能、扩展性强。   
 - 易上手：nodejs最初就是为了开发高性能web服务器而被设计出来的，然而相对底层的API会让不少新手望而却步。Express对web开发相关的模块进行了适度的封装，屏蔽了大量复杂繁琐的技术细节，让开发者只需要专注于业务逻辑的开发，极大的降低了入门和学习的成本。   
 - 高性能：Express仅在web应用相关的nodejs模块上进行了适度的封装和扩展，较大程度避免了过度封装导致的性能损耗。    
 - 扩展性强：基于中间件的开发模式，使得Express应用的扩展、模块拆分非常简单，既灵活，扩展性又强。    

##### express主要包含三个核心概念：路由、中间件、模板引擎。    
 - 中间件：可以毫不夸张的说，在express应用中，一切皆中间件。各种应用逻辑，如cookie解析、会话处理、日志记录、权限校验等，都是通过中间件来完成的。      
 - 路由：负责寻址，比如用户发送了个http请求，该定位到哪个资源，就是路由说了算。      
 - 模板引擎：负责视图动态渲染。下面会介绍相关配置，以及如何开发自己的模板引擎。    

### 路由     
路由用于确定应用程序如何响应对特定端点的客户机请求，包含一个 URI（或路径）和一个特定的 HTTP 请求方法（GET、POST 等）。     
每个路由可以具有一个或多个处理程序函数，这些函数在路由匹配时执行。     
路由定义采用此结构：app.METHOD(PATH, HANDLER)       
#####其中：    
 - app 是 express 的实例。     
 - METHOD 是 HTTP 请求方法。       
 - PATH 是服务器上的路径。        
 - HANDLER 是在路由匹配时执行的函数。         
##### 如：    
> var express = require('express');     
>	var app = express();      
>	app.get('/', function (req, res, next) {    
>	  res.send('Hello World!');   
>	});    
>	app.post('/user:id', function (req, res, next) {   
>	  res.send('Got a POST request');   
>	});       
  

##### 也可在一个路由上设置多个回调函数:      
> app.get('/', function (req, res, next) {    
>	  console.log('AAAA');   
>	  next();    
>	}, function (req, res) {   
>		res.send('BBBB');   
>	});   
>	当访问‘/’路径时，就会输出：   
>	AAAA   
>	BBBB      
 

##### 若要针对同一路径指定多个方法，可使用app.route(),以下是使用 app.route() 定义的链式路由处理程序的示例:     
> app.route('/book')    
> .get(function(req, res) {    
>    res.send('Get a random book');    
>  })    
>  .post(function(req, res) {     
>    res.send('Add a book');    
>  })     
>  .put(function(req, res) {     
>    res.send('Update the book');     
>  });      
      

 也可以利用**模块化**的思想，将一个路由处理程序单独分离出来写，最后再安装到主应用程序的路径中（也可以不用单独写在一个文件而写在同一文件内当作路由拆分进行使用）        
 ![使用 express.Router 类来创建可安装的模块化路由处理程序.png](http://upload-images.jianshu.io/upload_images/3001083-177684fe5dfa64d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        

### 中间件           
"一切皆中间件“            
Express 是一个路由和中间件 Web 框架，其自身只具有最低程度的功能：Express 应用程序基本上是一系列中间件函数调用。     
中间件函数能够访问请求对象 (req)、响应对象 (res) 以及应用程序的请求/响应循环中的下一个中间件函数。下一个中间件函数通常由名为 next 的变量来表示。     
##### 中间件函数可以执行以下任务：     
 - 执行任何代码。       
 - 对请求和响应对象进行更改。         
 - 结束请求/响应循环。       
 - 调用堆栈中的下一个中间件函数。            
如果当前中间件函数没有结束请求/响应循环，那么它必须调用 next()（回调方法，当next()被调用时，就进入下一个中间件），以将控制权传递给下一个中间件函数。否则，请求将保持挂起状态。         

##### Express 应用程序可以使用以下类型的中间件：       
 - 应用层中间件   
 - 路由器层中间件        
 - 错误处理中间件（必须提供四个自变量，err，req，res，next）         
 - 内置中间件（如今只有express.static，其他所有中间件均以单独模块的形式提供）        
 - 第三方中间件（通过npm进行安装）        
您可以使用可选安装路径来装入应用层和路由器层中间件（应用层中间件和路由器层中间件基本相同，差异之处在于应用层中间件是绑定到express()，如app.get,而路由层中间件是绑定在express.Router()的实例，如router = express.Router(), router.get） 还可以将一系列中间件函数一起装入，这样会在安装点创建中间件系统的子堆栈。    
![中间件函数调用的元素.png](http://upload-images.jianshu.io/upload_images/3001083-8c9157073a353248.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)          

开发一个自己的中间件：       
![开发一个自己的中间件.png](http://upload-images.jianshu.io/upload_images/3001083-9c482c225ea3e718.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        


### 模板引擎          
![模板引擎1.png](http://upload-images.jianshu.io/upload_images/3001083-68b08c0df7cf6df0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

![模板引擎2.png](http://upload-images.jianshu.io/upload_images/3001083-1aec9c946fcffb42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

而想要使用模板引擎，需要调用Express为response对象提供的render方法，该方法完成如下三件事：         
1. 初始化模板引擎。    
2. 读取视图文件并将其传递给模板引擎。   
3. 获取解析后的HTML页面作为响应发送给客户端。     


还有很多其他方面的内容，这里就不一一列举，具体内容参考官网[expressjs.com](http://expressjs.com) 。       