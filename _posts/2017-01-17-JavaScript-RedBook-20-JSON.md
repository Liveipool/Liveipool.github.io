---
layout: post
title: JavaScript红皮书学习(20)--JSON
date: 2017-01-17
categories: blog
description: JavaScript学习
---

# JSON     
![20.0.png](http://upload-images.jianshu.io/upload_images/3001083-a967e1874c135cb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 20.1 语法     
![20.1.png](http://upload-images.jianshu.io/upload_images/3001083-332c0ec109f906dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 20.1.1 简单值     
JavaScript字符串与JSON字符串最大的区别在于,JSON字符串必须使用双引号(单引号会导致语法错误).     

### 20.1.2 对象     
![20.1.2.png](http://upload-images.jianshu.io/upload_images/3001083-621378eb902c2859.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 20.1.3 数组     
JSON数组采用的就是JavaScript中的数组字面量形式.如:     

``` json
{
	"title": "Hello",
	"authors": [
		"Nick",
		"Marry",
		"Joey"
	],
	year: 2015
}
```

## 20.2 解析与序列化     
 JSON之所以流行,拥有与JavaScript类似的语法并不是全部原因.更重要的一个原因是,**可以把JSON数据结构解析为JavaScript对象.**     
 
### 20.2.1 JSON对象     
 - 早期的JSON解析器基本上就是使用JavaScript的eval()函数.     
 - ECMAScript5对解析JSON的行为进行了规范,定义了全局对象JSON. JSON对象有两个方法.     
 - JSON.stringify()可以把JavaScript对象**序列化**为JSON字符串.     
 - JSON.parse()可以把JSON字符串**解析**为原生JavaScript值.     

如:     

``` javascript
	var book = {
		title: "JavaScript",
		authors: ["Nick"],
		year: 2015
	}

	var bookText = JSON.stringify(book);
	console.log(bookText); //{"title":"JavaScript","authors":["Nick"],"year":2015}
```
默认情况下,JSON.stringify输出的JSON字符串不包含任何空格字符或缩进.     
**在序列化JavaScript对象时,所有函数及原型成员都会被有意忽略,不体现在结果中.此外,值为undefined的任何属性也都会被跳过.结果中最终都是值为JSON数据类型的实例属性.**     

将JSON字符串直接传递给JSON.parse()就可以得到相应的JavaScript值,如:     

``` javascript
var bookCopy = JSON.parse(bookText);
```

注意,虽然book与bookCopy具有相同的属性,但它们是两个独立的,没有任何关系的对象.     

### 20.2.2 序列化选项     
 - JSON.stringify()除了要序列化的JavaScript对象外,还可以接收另外两个参数,这两个参数用于指定以不同的方式序列化JavaScript对象.     
 - 第二个参数是过滤器,可以是一个数组,也可以是一个函数.     
 - 第三个参数是一个选项,表示是否在JSON字符串中保留缩进.     

**1. 过滤结果:**     
如果过滤器参数是数组,那么JSON.stringify()的结果中将只包含数组中列出的属性.     

``` javascript
	var book = {
		title: "JavaScript",
		authors: ["Nick"],
		year: 2015
	}

	var bookText = JSON.stringify(book, ["title", "authors"]);
	console.log(bookText); //{"title":"JavaScript","authors":["Nick"]}
```

如果传入的是函数,函数会接收两个参数,属性(键)名和属性值.根据属性(键)名可以知道应该如何处理要序列化的对象中的属性.属性名只能是字符串.     
为了改变序列化对象的结果,函数返回的值就是相应键的值.不过要注意,如果函数返回了undefined,那么相应的属性会被忽略.     

``` javascript
	var book = {
		title: "JavaScript",
		authors: ["Nick"],
		year: 2015
	}

	var bookText = JSON.stringify(book, function(key, value) {
		switch(key) {
			case "title":
				return undefined;
			case "authors": 
				return 500;
			default:
				return value;
		}
	});
	console.log(bookText);   //{"authors":500,"year":2015}
```

**2. 字符串缩进:**     
JSON.stringify()方法的第三个参数用于控制结果中的缩进和空白符.如果这个参数是一个数值,那它表示的是每个级别缩进的空格数.如果缩进参数是一个字符串而非数值,则这个字符串将在JSON字符串中被用作缩进字符(不再使用空格).如:     

``` javascript
	var book = {
		title: "JavaScript",
		authors: ["Nick"],
		year: 2015
	}

	var bookText = JSON.stringify(book, null, " - -");
	console.log(bookText);
	// {
	//  - -"title": "JavaScript",
	//  - -"authors": [
	//  - - - -"Nick"
	//  - -],
	//  - -"year": 2015
	// }

	bookText = JSON.stringify(book, null, 4);
	console.log(bookText);
	// {
	//     "title": "JavaScript",
	//     "authors": [
	//         "Nick"
	//     ],
	//     "year": 2015
	// }
```

**3. toJSON()方法:**     
![20.2.21.png](http://upload-images.jianshu.io/upload_images/3001083-f5f4c29525146945.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![20.2.22.png](http://upload-images.jianshu.io/upload_images/3001083-07a0837adae24cb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 20.2.3 解析选项     
![20.2.31.png](http://upload-images.jianshu.io/upload_images/3001083-e8955488703ad771.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![20.2.32.png](http://upload-images.jianshu.io/upload_images/3001083-2f8851a881c3ade9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
     
## 20.3 小结     
![20.3.png](http://upload-images.jianshu.io/upload_images/3001083-4328956a950b7dc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

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
