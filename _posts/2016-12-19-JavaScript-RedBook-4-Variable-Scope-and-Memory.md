---
layout: post
title: JavaScript红皮书学习(4)--变量、作用域和内存问题
date: 2016-12-19
categories: blog
tags: [JavaScript]
description: JavaScript学习
---

# 变量、作用域和内存问题     
按照ECMA-262的定义,Javascript的变量与其他语言的变量有很大的区别.      Javascript的变量松散类型的本质,决定了它只是在特定时间用于保存特定值的一个名字而已.      由于不存在定义某个变量必须要保存何种数据类型值的规则,变量的值及其数据类型可以在脚本的生命周期中改变.       

## 4.1 基本类型和引用类型的值       
 - ECMAScript变量可能包含两种不同类型的值：基本类型和引用类型值。       
 - **基本类型值**指简单的数据端，包括：Undefined、Null、Boolean、Number、String。        
 - **引用类型值**指那些可能由多个值构成的**对象**。      

在将一个值赋给变量时，解析器必须确定这个值是基本类型值还是引用类型值。引用类型值是**保存在内存中的对象。**          
**当复制保存着某个对象的某个变量时，操作的是对象的引用。当在为对象添加属性时，操作的是实际的对象。**            
   
``` javascript
var person = new Object();
person.name = "lallana";  //此时操作的是实际的对象
alert(person.name);  //lallana
```

``` javascript
var o1 = {x : 1};
o2 = o1; //此时操作的是对象的引用
alert(o2.x);  //1
```

### 4.1.1 复制变量值        

``` javascript
var num1 = 5;
var num2 = n1;
```
此后，这两个变量可以参与任何操作而不会互相影响。         
![4.1.11.png](http://upload-images.jianshu.io/upload_images/3001083-4dd137b5ab4f2ddb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![4.1.12.png](http://upload-images.jianshu.io/upload_images/3001083-1e7dd9304105b1a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 4.1.2 传递参数       
![4.1.21.png](http://upload-images.jianshu.io/upload_images/3001083-e1e9334bb0713fe7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.1.22.png](http://upload-images.jianshu.io/upload_images/3001083-ed5ef7834da1a653.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.1.23.png](http://upload-images.jianshu.io/upload_images/3001083-c1916d210f7aa0ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
可以看出，person的引用没有变，也就是说在函数内部也无法改变person的引用，从这里可以看出，参数都是按值传递的。    

## 4.2 执行环境和作用域       
- **执行环境**是Javascript中最为重要的一个概念。有时也简称**环境**。     
 - 每个执行环境都有一个与之关联的**变量对象**，环境中定义的所有变量和函数都保存在这个对象中。        
 - 当代码在一个环境中执行时，会创建变量对象的一个**作用域链**。它保证对执行环境有权访问的所有变量和函数的有序访问。       
 - 作用域链的前端始终都是当前执行的代码所在的环境的变量对象。如果这个环境是函数，则将其**活动对象**作为变量对象。     

![4.21.png](http://upload-images.jianshu.io/upload_images/3001083-f0df6582242ccbcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.22.png](http://upload-images.jianshu.io/upload_images/3001083-5f18f74d30b77da0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.23.png](http://upload-images.jianshu.io/upload_images/3001083-8b3ff78528eb51c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.24.png](http://upload-images.jianshu.io/upload_images/3001083-b7819bf53a82428c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 4.2.1 延长作用域链       
虽然执行环境的类型总共只有两种——全局和局部（函数），但还是有其他办法来延长作用域链。这么说是因为有些语句可以用在作用域链的**前端临时增加一个变量对象**，该变量对象会在代码执行后被移除，这两种语句是：       
1. try-catch语句的catch块。     
2. with语句          
这两个语句都会在作用域链前端添加一个变量对象。with语句以后都不建议用了就不说了。catch语句是创建一个新的变量对象，其中包含的是被抛出的错误对象的声明。       

### 4.2.2 没有块级作用域          

![4.2.21.png](http://upload-images.jianshu.io/upload_images/3001083-7b2921800518d343.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)               

**if语句和for语句都没有自己的执行环境。**     

![4.2.22.png](http://upload-images.jianshu.io/upload_images/3001083-4264d58387f4f4b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)          

**函数有自己的执行环境。**        
**在编写Javascript代码的过程中，不声明而直接初始化变量是一个常见的错误做法，因为这样可能导致意外。我们建议在初始化变量之前，一定要先声明，这样就可以避免这类问题。在严格模式下，初始化未经声明的变量就会导致错误。**     

![4.2.23.png](http://upload-images.jianshu.io/upload_images/3001083-315a7abb67d44633.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.2.24.png](http://upload-images.jianshu.io/upload_images/3001083-ec4c975c3da73565.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)              

## 4.3 垃圾收集        
![4.31.png](http://upload-images.jianshu.io/upload_images/3001083-cb0c1956abc1d092.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.32.png](http://upload-images.jianshu.io/upload_images/3001083-b293f59ed203c44d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.33.png](http://upload-images.jianshu.io/upload_images/3001083-f74d9de23145190f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.34.png](http://upload-images.jianshu.io/upload_images/3001083-ed4927228c7b2639.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.35.png](http://upload-images.jianshu.io/upload_images/3001083-a8e0c49fd549a6ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

**局部变量会在它们离开执行环境的时候自动解除引用，而全局变量会一直占用内存，因此尽量少使用全局变量，或在不用后手动清除。**     

## 4.4 小结     
![4.41.png](http://upload-images.jianshu.io/upload_images/3001083-d1e66dff56ee0b4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![4.42.png](http://upload-images.jianshu.io/upload_images/3001083-49ecfa1b17cbabe9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

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