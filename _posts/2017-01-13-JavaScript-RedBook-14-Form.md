---
layout: post
title: JavaScript红皮书学习(14)--表单脚本
date: 2017-01-13
categories: blog
description: JavaScript学习
---

# 表单脚本    
![14.0.png](http://upload-images.jianshu.io/upload_images/3001083-dfd5bdf3825c6193.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

## 14.1 表单的基础知识    
![14.1.png](http://upload-images.jianshu.io/upload_images/3001083-044cf7b4d46f5d5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

``` javascript
var firstForm = document.forms[0];    //取得页面中的第一个表单
var myForm = document.forms["form2"]; //取得页面中名叫"form2"的表单
```

### 14.1.1 提交表单    
![14.1.1.png](http://upload-images.jianshu.io/upload_images/3001083-246985ffd8c42cc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**有一个问题在于,用户可能因为多次点击而重复提交表单,这种时候可以通过第一次提交表单后就禁用提交按钮,或者利用onsubmit事件处理程序取消后续的表单提交操作.**    

### 14.1.2 重置表单    
![14.1.2.png](http://upload-images.jianshu.io/upload_images/3001083-58776662c8275642.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 14.1.3 表单字段    
![14.1.31.png](http://upload-images.jianshu.io/upload_images/3001083-be8f2342e0d96107.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![14.1.32.png](http://upload-images.jianshu.io/upload_images/3001083-992e04b274378976.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**如果有多个表单控件都在使用一个name(如单选按钮), 那么就会返回以该name命名的一个NodeList.**    
**通常,在访问form中的元素时,尽量都使用elements.**    

**1.共有的表单字段属性:**    
![14.1.33.png](http://upload-images.jianshu.io/upload_images/3001083-7cc85b61e9ff4665.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
**2.共有的表单字段方法:**    
每个表单字段都有两个方法:focus()和blur().它们是自解释的.(注意这里说的是*方法*)    
![14.1.34.png](http://upload-images.jianshu.io/upload_images/3001083-82da23d83405e84d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

**3.共有的表单字段事件:**(注意这里是*事件*)        
![14.1.35.png](http://upload-images.jianshu.io/upload_images/3001083-84d994bad9e47cb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

## 14.2 文本框脚本    
![14.2.png](http://upload-images.jianshu.io/upload_images/3001083-14d2007d231e1a15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 14.2.1 选择文本        
上述两种文本框都支持select()方法,这个方法用于选择文本框中的所有文本.如        

``` javascript
var textBox = document.forms[0].elements["textBox"];
textBox.select();
```

select()方法对应了select事件,可通过selectionStart和selectionEnd属性来取得选择的文本,如:    

``` javascript
function getSelectText(textBox) {
	return textBox.value.substring(textBox.selectionStart, textBox.selectionEnd);
}
```
还有选择部分文本如setSelectionRange()方法等.    

### 14.2.2 过滤输入    
我们经常会要求用户在文本框中输入特定的数据,或者输入特定格式的数据.由于文本框在默认情况下没有提供多少验证数据的手段,因此必须使用JavaScript来完成此类过滤输入的操作.这里就不详讲,以后在项目中,具体情况具体分析,综合运用事件和DOM手段来实现如屏蔽字符或操作剪贴板等.    

### 14.2.3 自动切换焦点    
使用JavaScript可以从多个方面增强表单字段的易用性.其中,最常见的一种方式就是在用户填写完当前字段时,自动将焦点切换到下一字段.这种功能通常是配合着文本框的maxlength特性来实现.    

### 14.2.4 HTML5约束验证API    
为了在将表单提交到服务器之前验证数据,HTML5新增了一些功能:    
![14.2.41.png](http://upload-images.jianshu.io/upload_images/3001083-363af29918c57d63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![14.2.42.png](http://upload-images.jianshu.io/upload_images/3001083-caa3dc82635c8c38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![14.2.43.png](http://upload-images.jianshu.io/upload_images/3001083-9564f20dd9bd31aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![14.2.44.png](http://upload-images.jianshu.io/upload_images/3001083-437e34c6b1a38d02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![14.2.45.png](http://upload-images.jianshu.io/upload_images/3001083-0128c26759a93770.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![14.2.46.png](http://upload-images.jianshu.io/upload_images/3001083-16bb718ad9bf39fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![14.2.47.png](http://upload-images.jianshu.io/upload_images/3001083-a02b08ca567ac6a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![14.2.48.png](http://upload-images.jianshu.io/upload_images/3001083-e5fdd45acd986d59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

## 14.3 选择框脚本    
选择框是通过< select >和< option >元素创建的.暂时还没怎么用过,用的时候再查API吧.    

## 14.4 表单序列化    
![14.4.png](http://upload-images.jianshu.io/upload_images/3001083-95d7e1c1fde42de9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

## 14.5 小结    
虽然HTML和Web应用自诞生以来已经发生了天翻地覆的变化,但Web表单相对却没有什么改变.使用JavaScript可以增强已有的表单字段,从而创造出新的功能,或者提升表单的易用性.    

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