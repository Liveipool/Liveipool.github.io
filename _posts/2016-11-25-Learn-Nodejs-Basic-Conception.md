---
layout: post
title: Node.js学习摘要之基本概念
date: 2016-11-25
categories: blog
tags: [Nodejs]
description: Nodejs学习摘要
---


# Node.js学习摘要之基本概念    
 -“如果成真的话，以后开发Web应用就只需要一种语言了。”       
 - 2009年，RyanDahl在一个Javascript大会上宣布了一项名为Node.js的新技术，而Node以其异步IO、服务器端Javascript的特点为Web开发掀开了新的篇章。    
 - V8是Google为Chrome Web浏览器设计的JavaScript解释器和虚拟机，它运行JavaScript非常快。可以通过V8引擎内嵌到操作系统的集成层，来让Javascript享受到底层操作系统的异步接口，从而实现将其带到服务器端的目的。Node.js快速高效的优点得益于一种叫事件轮询（event loop）的技术，以及其构建于V8之上。     
 - V8做了一件很酷的事，它始终坚定不移地实现最新版本的ECMA标准。Nodejs的核心团队也是如此，只要你安装的是最新版本的Node，你总能使用到最新版本的V8.    
 - Node的REPL（Read-Eval-Print Loop），在终端输入node即可运行。可以简单方便地验证一些Node API是否正确，开发时可常用。   
 - JavaScript是基于原型、面向对象、弱类型的动态脚本语言。    
 - Node应用依托在一个拥有大量共享状态的大进程中。  
![Node采用一个长期运行的进程.png](http://upload-images.jianshu.io/upload_images/3001083-4862bf5ef5b030be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
 - 采用了事件轮询意味着什么呢？从本质上来说，Node会先注册事件，随后不停地询问内核这些事件是否已经分发。当事件分发时，对应的回调函数就会被触发，随后继续执行下去。如果没有事件触发，则继续执行其他代码，直到有新事件时，再去执行对应的回调函数。**即优秀的Node模块应该是非阻塞的，执行任务也最好采用异步的方式**    
 - Node最早期是单线程的，但由于调用堆栈执行非常快，因此也能满足高并发的要求。发展至今，Node应该已经可以支持创建子进程了。  
 - Node中的错误处理是非常重要的，防止整个大进程因为错误而崩溃，并且由于堆栈追踪的堆栈信息显示是从事件轮询开始的，因此每步都要正确进行错误处理的原因里，一旦遗漏，因为上下文丢失就很难追踪了。      
 - 在浏览器中，全局对象指的就是window对象。在window对象上定义的任何内容都可以被全局访问到。比如，setTimeout其实就是window.setTimeout。Node中有两个类似但却各自代表着不同含义的对象：1.global：和window一样，任何global对象上的属性都可以被全局访问到。2.process：所有全局执行上下文中的内容都在process对象中。  
 - Node摒弃了采用定义一堆全局变量的方式，转而引入了一个简单但却无比强大的模块系统，该模块系统有三个核心的全局对象：require、module、exports。  
 - 绝对模块：指Node通过在其内部node_modules查找到的模块，或者Node内置的如fs这样的模块。这时，可以直接通过名字来require这个模块，无需添加路径名如require（‘fs’）。    
 - 相对模块：将require指向一个相对工作目录中的JavaScript文件，如require（‘./moduleA’）。    
暴露API要让模块暴露一个API成为require调用的返回值，就要依靠module和exports这两个全局变量。    
 - 事件：    
![Node事件.png](http://upload-images.jianshu.io/upload_images/3001083-ab30f9823933f734.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
 - Buffer：   
![NodeBuffer.png](http://upload-images.jianshu.io/upload_images/3001083-bc21861bf69b50a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    



