---
layout: post
title: JavaScript红皮书学习(3)--基本概念
date: 2016-12-14
categories: blog
tags: [JavaScript]
description: JavaScript学习
---

# 基本概念      

## 3.1 语法     

### 3.1.1 区分大小写     
ECMAScript中的一切（变量、函数名和操作符都区分大小写），如：函数名不能使用typeof，因为它是一个关键字，但typeOf完全可以作为一个函数名。      

### 3.1.2 标识符       
 - 标识符是指变量、函数、属性的名字，或者函数的参数。标识符可以是按照下列格式规则组合起来的一个或多个字符。     
 - 第一个字符必须是一个字母、下划线或一个美元符号($)。     
 - 其它字符可以是字母、下划线、美元符号或者数字。     
按照惯例，ECMAScript标识符采用驼峰大小写格式，这不是一种规定，但为了与 ECMAScript内置的函数和对象命名格式保持一致，可以将其当作是一种最佳实践。        

### 3.1.4 严格模式        
![3.1.4.png](http://upload-images.jianshu.io/upload_images/3001083-7f0ee6d840d29c93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

##### 严格模式主要有以下限制      
 - 变量必须声明后再使用     
 - 函数的参数不能有同名属性，否则报错     
 - 不能使用with语句     
 - 不能对只读属性赋值，否则报错     
 - 不能使用前缀0表示八进制数，否则报错     
 - 不能删除不可删除的属性，否则报错     
 - 不能删除变量delete prop，会报错，只能删除属性delete global[prop]     
 - eval不会在它的外层作用域引入变量     
 - eval和arguments不能被重新赋值     
 - arguments不会自动反映函数参数的变化     
 - 不能使用arguments.callee     
 - 不能使用arguments.caller     
 - 禁止this指向全局对象          
 - 不能使用fn.caller和fn.arguments获取函数调用的堆栈     
 - 增加了保留字（比如protected、static和interface）     

## 3.3 变量       
 - ECMAScript的变量是**松散类型**的，所谓松散类型就是可以用来保存任何类型的数据。换句话说，每个变量仅仅是一个用于保存值的占位符而已。       
 - 在严格模式下，不能定义名为eval或arguments的变量，否则会导致语法错误。            

## 3.4 数据类型       
ECMAScript有五种**基本类型**：Undefined、Null、Boolean、Number、String，和一种**引用类型**：Object。ECMAScript不支持任何创建自定义类型的机制，而所有值最终都将是上述6种数据类型之一。       

### 3.4.1 typeof操作符       
 - typeof是一个**操作符**而不是函数，因此它后面的括号不是必需的，如 typeof（3）和 typeof 20 ，都是正确的。        
 - 有时候，typeof操作符会返回一些令人迷惑但技术上却正确的值。比如，typeof null会返回“object”，因为特殊值null被认为是一个空的对象的引用。     
 - 从技术上讲，虽然typeof 一个函数会返回“function”， 但函数在ECMAScript中是对象，而不是一种单独的数据类型。使用typeof可以区分函数和其他对象。        

### 3.4.5 Number类型       
 - Number类型使用IEEE754格式来表示整数和浮点数值。         
 - 八进制的字面值的第一位必须是零。     
 - 八进制字面量在严格模式下是无效的，会导致支持该模式的JavaScript引擎抛出错误。     
 - 在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。     
 - 关于浮点数值计算会产生舍入误差的问题，有一点需要明确：这时使用基于IEEE754数值的浮点计算的通病，ECMAScript并非独此一家，其他使用相同数据格式的语言也存在这个问题。       
 - parseInt（）函数可将字符串转为数值。它会忽略字符串前面的空格，直到找到第一个非空格字符，如果第一个字符不是数字字符或者符号，它将返回NaN。     

```
var a = 070 //八进制的56
var b = 0xA //十六进制的10
a + b = 66  //十进制的66

var floatNum = 3.125e7  //等于31250000

0.1 + 0.2 = 0.30000000004

var num1 = parseInt("")      //NaN
var num2 = parseInt(22.5)    //22
var num3 = parseInt("070")   //56
var num4 = parseInt("A", 16) //10
var num5 = parseInt("10", 10)//10
var num5 = parseInt("10", 8) //8
```

### 3.4.6 String类型       
 - 因为ECMAScript中字符串是不可变类型的，也就是说，**字符串一旦创建，他们的值就不能改变**。要改变某个变量保存的字符串，**首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量**。如：`var a = "aa"; a = a + "bb"; alert(a); //输出aabb`，但实际上现在的a和以前的a已经不是同一个了。要记住：**原始值是无法更改的，任何方法都无法更改一个原始值。**    

## 3.5 操作符       

### 3.5.1 一元操作符  
```
var o = {
	valueOf: function() {
		return -1;
	}
}
o--;  //-2

var o = {
	valueOf: function() {
		return -1;
	}
}
o = -o;  //1

var b = false;
b = +b; //0
var s = "1.1";
s = +s; //变为数值1.1
```

### 3.5.2 位操作符       

![3.5.21.png](http://upload-images.jianshu.io/upload_images/3001083-fc2be71fcdb3b2c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.5.22.png](http://upload-images.jianshu.io/upload_images/3001083-1f3871203dc8c7ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

1. 按位非（NOT， ～）：本质：操作数的负值减一。     
2. 按位与（AND，&）     
3. 按位或（OR，| ）     
4. 按位异或（XOR， ^）     
5. 左移（ < < ）     
6. 有符号的右移 （ > > ）：用符号位来填充     
7. 无符号的右移 （ > > > ）：移动时以0来填充空位，不像有符号的右移是用符号位的值来填充空位的。       

![3.5.23.png](http://upload-images.jianshu.io/upload_images/3001083-d1f0af47241a3a7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

```
var num1 = 25    //二进制000000000000000000000000000011001
var num2 = ~num1 //二进制111111111111111111111111111100110
alert(num2) //-26

var num3 = 3 //0000 0000 0000 0000 0000 0000 0000 0011
var num4 = 5 //0000 0000 0000 0000 0000 0000 0000 0101
var num5 = 7 //0000 0000 0000 0000 0000 0000 0000 0111
num3 & num4  //1
num3 & num5  //3
num3 | num4  //7
num3 ^ num4  //6

2 << 5       //64,即从0000 0000 0000 0000 0000 0000 0000 0010 变为了 0000 0000 0000 0000 0000 0000 0100 0000
2 << 29      //1073741824,  0100 0000 0000 0000 0000 0000 0000 0000
2 << 30      //-2147483648, 1000 0000 0000 0000 0000 0000 0000 0000
2 << 31      //0,           0000 0000 0000 0000 0000 0000 0000 0000  
64 >> 5      //2
```

### 3.5.3 布尔操作符        

#### 逻辑非（!）        
可以应用于ECMAScript中的任何值。逻辑非操作符可以用于将一个值转换为与其对应的布尔值。而同时使用两个逻辑非操作符，实际上就会模拟Boolean（）转型函数的行为：       
![3.5.31.png](http://upload-images.jianshu.io/upload_images/3001083-2b78a156824d472b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### 逻辑与（&&）：短路操作符         

![3.5.32.png](http://upload-images.jianshu.io/upload_images/3001083-84d01b82bc7a2d5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.5.33.png](http://upload-images.jianshu.io/upload_images/3001083-6cdcc120dcbfb309.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### 逻辑或 （||）：短路操作符       
![3.5.34.png](http://upload-images.jianshu.io/upload_images/3001083-fcb33ba89129c2b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.5.35.png](http://upload-images.jianshu.io/upload_images/3001083-082591a0e70db1a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 3.5.4 乘性操作符       
![3.5.41.png](http://upload-images.jianshu.io/upload_images/3001083-a32e4f5dfaf3cf0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.5.42.png](http://upload-images.jianshu.io/upload_images/3001083-12c3a698da7d65ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.5.43.png](http://upload-images.jianshu.io/upload_images/3001083-d0d7bf83f0b7a865.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        

### 3.5.5 加性操作符       
![3.5.51.png](http://upload-images.jianshu.io/upload_images/3001083-772eaf9eb03cd4f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.5.52.png](http://upload-images.jianshu.io/upload_images/3001083-26a77b7d1e4ec774.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.5.53.png](http://upload-images.jianshu.io/upload_images/3001083-94778d959636934c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.5.54.png](http://upload-images.jianshu.io/upload_images/3001083-826e724d68ef8da9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.5.55.png](http://upload-images.jianshu.io/upload_images/3001083-a5481681304b3b5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

### 3.5.6 关系操作符       
![3.5.6.png](http://upload-images.jianshu.io/upload_images/3001083-2aa8a5e924d9162a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

## 3.6 语句     

### 3.6.5 for-in语句       
![3.6.5.png](http://upload-images.jianshu.io/upload_images/3001083-ed3cf2918490d792.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        

### 3.6.7 break和continue语句       
![3.6.71.png](http://upload-images.jianshu.io/upload_images/3001083-ab0e95107f1b6692.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.6.72.png](http://upload-images.jianshu.io/upload_images/3001083-d611b83f78cc4086.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.6.73.png](http://upload-images.jianshu.io/upload_images/3001083-999f99f747cca153.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 3.7 函数       

### 3.7.1 理解参数       
![3.7.11.png](http://upload-images.jianshu.io/upload_images/3001083-ed43b13f76e1bdb6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.7.12.png](http://upload-images.jianshu.io/upload_images/3001083-5848b6875f1f13a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.7.13.png](http://upload-images.jianshu.io/upload_images/3001083-f41d295f221211b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![3.7.14.png](http://upload-images.jianshu.io/upload_images/3001083-d75a061c0d5fdc24.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 3.7.2 没有重载     
![3.7.2.png](http://upload-images.jianshu.io/upload_images/3001083-45922c54d078f44c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 3.8 小结       
![3.8.png](http://upload-images.jianshu.io/upload_images/3001083-50161b2c8a4300ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     


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