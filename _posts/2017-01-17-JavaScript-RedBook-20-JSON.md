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