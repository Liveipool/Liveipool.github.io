---
layout: post
title: AngularJS
date: 2017-02-23
categories: blog
description: 前端学习
---

# AngularJS        
在去年暑假，曾使用过AngularJS 1 来写过My-Achievements 项目，而去年的9月也发布了Angular 2，Angular 2 是基于TypeScript的。并且Google准备在2017年3月发布下一个版本（不是3）：Angular4，具体的原因就不去深究了，毕竟也没有ES4。另外还有一个稍微有点疑惑的地方，只有在AngularJS 1 的名字中有 **JS**，而后面的版本，都是说的**Angular 2**和**Angular 4**。        
在这篇博客中，主要是来复习一下AngularJS 1, 并稍微了解一下Angular 2 的一些变化。        

## AngularJS 1        
 - 我们知道HTML是一种创建网页的静态标记语言，它很基础，但不失灵活，然而它自身并不提供对灵活性的具体实现，所以我们要创建具有可用性及赏心悦目的Web应用就需要使用其他语言与之结合去操控它的DOM、标签、标签属性、标签层级结构实现样式变换、动态数据变换、动态元素变换等，比如结合CSS和JavaScript语言。        
 - 但是越多的结合就意味着功能实现越复杂，我们需要写大量的代码去实现类似数据绑定、动态展现、远程服务请求等功能，所以jQuery这类的JS库、ember这类的框架应用而生。前者让我们在完成某些单一功能时调用它封装好的方法，从而减少代码量。后者让我们可以遵循它的规范去填充它设计好的代码结构，从而逐步完成完整的应用功能。        
 - 而AngularJS是JS库和框架的结合，它诞生于2009年，由Misko Hevery 等人创建，后为Google所收购并发扬光大。AngularJS通过为开发者呈现一个更高层次的抽象来简化应用的开发，提供了构建一个CRUD应用可能用到的全部内容包括：自定义HTML标签、事件绑定、数据绑定、基本模板标识符、表单验证、路由、深度链接、组件重用、依赖注入等。        

#### AngularJS 1 部分基础教程        
[AngularJS 1 部分教程](https://hairui219.gitbooks.io/learning_angular/content/zh/chapter04_2.html)        

### AngularJS： MV*        
开始一直以为Angular是标准的MVC，但后来又有人说是MVVM，而官方Github上关于AngularJS是否是MVC的解释是：        
**AngularJS works on MV*, short for Model-View-Whatever. The Whatever is AngularJS's way of telling that you may create any kind of linking between the Model and the View here.**        
**Unlike other frameworks in any programming language, where MVC, the three separate components, each one has to be written and then connected by the programmer, AngularJS helps the programmer by asking him/her to just create these and everything else will be taken care of by AngularJS.**        
因此，说Angular是MVC和MVVM都不恰当。        

### AngularJS的特性        
AngularJS有着诸多的特性，但最为核心的是其Scope概念、MVC模式、路由、模块化、依赖注入、指令系统、双向数据绑定、Service、Provider等。        
[AngularJS的特性](http://www.devtalking.com/articles/angularjs1x-guide/)        
完整的文章参上，下面为一些摘录：        

#### 模块化        
我们先从AngularJS的模块化特性说起。什么是模块？既应用程序中不同功能的容器。在AngularJS中，我们可以按业务功能的不同将实现划分为不同的模块，这些模块可以在一个JS文件中，也可以将它们放在不同的JS文件中，既一个JS文件为一个模块。        

定义一个模块很简单，在JS文件中声明一个模块：        

```
var mianModule = angular.module("mianModule", []);
```
第二个参数为一个数组，该参数的含义为当前定义的模块所依赖的模块，如果有依赖模块则传入包含模块名称的数组，若无依赖则传入空数组。        

#### 服务（Services）        
AngularJS中一个重要的概念是服务，这个服务的概念比较宽泛，比如一个常量值也算做一个服务，既提供一个不可变值的服务。变量、对象、函数都算做是服务。在AngularJS中内置了好几十个服务，这些内置的服务都以 $ 符号开头，比如         

```
$scope、$http、$log、$timeout、$interval
```
等等，从字面意思都不难理解它们的作用，更多的内置服务可以去AngularJS官网查看API文档。        

AngularJS中的服务有两个主要特点：        
1. 延迟加载，当应用中的其他组建使用服务时才会实例化。        
2. 单例，在应用的整个生命周期中，一个服务只存在一份实例，所以服务一般用来共享可复用的代码逻辑或者数据。        

除了内置的服务，我们还可以创建自己的服务：        
**一般来说，一些常用的功能就应该被写为一个service以备复用，比如一个分页功能就写成一个service，要用的时候放进对应的controller就可以直接用：**        
![services1.png](http://upload-images.jianshu.io/upload_images/3001083-19c0465b94a3f623.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        
![services2.png](http://upload-images.jianshu.io/upload_images/3001083-a0f64d21228ab8df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        

注意，directive（指令）也是被单独写出来复用，但它们之间还是有所区别，下文中会有关于指令的介绍。        

#### 指令（Directive）        
指令是AngularJS中另一个主要的特性，指令的作用可以用一句话描述，就是可以给HTML元素赋予特殊或自定义的行为，比如监听事件、视图模板代理等。在上文中我们使用过的ng-app、ng-controller、ng-model就是AngularJS中的指令。        

在AngularJS中，指令有四种表现形式，既标签形式、标签属性形式、标签class名称形式、注释形式：        

```
<my-dir></my-dir>
<span my-dir="exp"></span>
<!-- directive: my-dir exp -->
<span class="my-dir: exp;"></span>
```
一般情况下，推荐使用标签形式和标签属性形式。        

所以，directive可以说相当于是一个**视图模板**，比如看在MA项目中的应用：        
![directives1.png](http://upload-images.jianshu.io/upload_images/3001083-cd2c397f09019eb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        
![directives2.png](http://upload-images.jianshu.io/upload_images/3001083-c6b6375cd1efa184.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        

**需要使用某个模板时，如stu-hw-chart时，就把需要的数据传进这个模板里，就可以渲染出有对应数据的页面了。**         
这样也就相当于是重用了这个模板，利用它可以生成视图相同但数据不同的页面。        

#### 组件（Component）        
在AngularJS 1.5中新加了组件的概念，它的意图是希望从AngularJS 1.x向AngularJS 2.0迁移时能更加平顺，AngularJS团队也提倡使用组件化模式开发Web应用。那么组建是什么呢？其实组件就是指令的一种特性形式，它规避了一些指令中晦涩难理解的东西，比如compile函数，link函数，scope，restrict等，所以组件的目的就是能让我们更为傻瓜式的创建指令，能更好的遵循组件化的开发模式，提高性能以及更容易向AngularJS 2.0迁移。        
但是，不像Vue中组件的意义很明显，Angular中有directive又有component，它俩区分度不是太高，容易让人混淆，在MA项目中，我们使用的是directives。        

#### 术语: 
 - 模板template：包含了Angular特殊扩展标记的HTML代码
 - 指令directive：扩展的HTML代码，自定义的标签、属性等等
 - 模型model：保存在js中，用来和用户交互的数据
 - 作用域scope：模型数据在HTML页面中产生作用的范围
 - 表达式expression：AngularJS在HTML页面中可以运算的语法
 - 编译器compiler：被用来解释HTML代码中的Angular代码
 - 过滤器filter：对HTML页面中输出的数据进行指定格式展示
 - 视图view：统称用户看到的HTML视图页面
 - 数据绑定data binding：JS中特定的数据和HTML页面上的数据的关联关系
 - 控制器controller：给视图view提供功能支持的东东
 - 依赖注入dependency injection：Angular自动创建对象并传递对象的一种方式
 - 注入器 injector：专门用来实现依赖注入（DI）的容器
 - 模块module：Angular用来对项目进行最高层次封装的东东
 - 服务service：Angular用来对视图view提供业务功能支持的东东
 - 组件component：用于对网页的公共部分进行封装重用的Angular代码，通常会包含模板、指令、服务等等

#### 数据双向绑定和脏值检测       
这篇文章讲得很好：[理解数据绑定过程](http://www.angularjs.cn/A0a6)     
截一些关键的图防止原文不见了：     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-c3c701809716c3ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-b505f1794a28b5f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-30f703454bdb5e0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-53850f2db38ab3d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-526b45a01c72eee5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-b773fafc79c588be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)            

## Angular 2        
至于Angular 2 的一些新特性，下次脏值检测的时候再一起看吧。        