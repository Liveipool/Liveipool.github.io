---
layout: post
title: JavaScript红皮书学习(5)--引用类型
date: 2016-12-22
categories: blog
tags: [JavaScript]
description: JavaScript学习
---

# 引用类型       
 - **引用类型的值(对象)**是**引用类型**的一个实例.         
 - 在ECMAScript中,引用类型是一种**数据结构**,用于将数据和功能组织在一起.        
 - 尽管ECMAScript从技术上讲是一门面向对象的语言,但它不具备传统的面向语言所支持的**类**和**接口**等基本结构.         
 - 引用类型有时也被称为**对象定义**,因为它们描述的是一类对象所具有的属性和方法.               
 - 新对象可以通过new操作符后跟一个**构造函数**或通过**字面量表示法**创建.          

## 5.1 Object 类型         
Object是ECMAScript中使用最多的一个类型,创建Object实例的方式有两种:        

``` javascript
var person1 = new Object();  //一是通过new+构造函数创建
person1.age = 29;
person1.name = "Nick";

var person2 = {
	name: "Nick",   //一般是这么写,key不加双引号,value如果是字符串必须得加双引号
	"age": 29,    //key值也可以加双引号,后面是数值的话不用加双引号
	5: true  //这里的数值属性名会自动转换为字符串
}  //二是通过对象字面量表示法

alert(person.name);  //"Nick"
alert(person["name"]);  //"Nick"
```
当一个对象的属性较多时,建议用**对象字面量表示法**来创建.        
一般来说,访问对象属性都是点表示法,这也是很多面向对象语言中通用的语法.不过,JavaScript也可以使用方括号表示法来访问对象的属性.在使用方括号语法时,应该将要访问的属性以字符串的形式放在方括号中,它的主要优势是**可以通过变量来访问属性**,属性名中包含空格的或包含非数字非字母的字符时,通过方括号表示法来访问.        
通常情况下,除非必须使用方括号表示法,一般建议使用点表示法.         

## 5.2 Array 类型         
虽然ECMAScript数组与其他类型语言中的数组都是数据的有序列表,但与其他语言不同的是,ECMAScript数组的每一项可以保存**任何类型**的数据.而且,ECMAScript数组的大小是可以**动态调整**的,即可以随着数据的添加自动增长以容纳新增数据.        

``` javascript
var colors = new Array();      //new + 构造函数
var colors = ["red", "blue"];  //数组字面量表示法

var colors = new Array(20);    //colors.length == 20
var colors = Array(3);         //new 可选
var name = Array("Greg");      //创建一个包含一项,即字符串"Greg"的数组
var names = [];                //创建一个空数

var colors = ["red", "blue", "yellow"];
colors[2] = "black";           //修改第三项
colors[3] = "white";           //添加第四项
alert(colors.length);          //4
colors.length = 3;             //length属性不是只读的.通过设置这个属性,可以从数组的末尾移除项或向数组中添加项.
alert(colors[3]);              //undefined
alert(colors.length);          //3
colors.length = 4;             
alert(colors[3]);              //undefined
alert(colors.length);          //4
colors[colors.length] = "green";  //添加colors[4],"green",colors.length变为5
```   

### 5.2.1 检测数组        
 
``` javascript
alert(Array.isArray(colors));              //true
alert(colors instanceof Array);            //true
alert(colors instanceof Object);           //true
alert(colors instanceof Date);             //false
```

### 5.2.2 转换方法      

``` javascript
	var colors = new Array(7);
	colors[0] = "red";
	colors[1] = "yellow";
	colors[3] = "black";

	console.log(colors.toLocaleString());  //red,yellow,,black,,,
	console.log(colors.toString());        //red,yellow,,black,,,
	console.log(colors.valueOf());         //>["red","yellow",3:"black"]
	console.log(colors);                   //>["red","yellow",3:"black"]
	alert(colors);                         //red,yellow,,black,,,  由于alert要接收字符串参数,所以它会在后台调用toString()方法.   
	console.log(colors.join(','));         //red,yellow,,black,,,  不给join()传任何参数,默认也是逗号
	console.log(colors.join('@'));         //red@yellow@@black@@@
```

### 5.2.3栈方法        
 - ECMAScript数组提供了一种让数组的行为类似于其他数据结构的方法.push()和pop()方法都只发生在栈的顶部.      
 - push()方法可以接收任意数量的参数,把它们逐个添加到数组末尾,并返回**修改后的数组的长度**.      
 - pop()方法则从数组末尾移除最后一项,减少数组的length值,然后返回**移除的项**.      

``` javascript
var colors = new Array();
var count = colors.push("red", "green");
alert(count);                              //2
var item = colors.pop();
alert(item);                               //"green"
```

### 5.2.4 队列方法        
 - 队列在列表的末端添加项,从列表的前端移除项.      
 - push()返回**修改后数组的长度**.      
 - shift()返回**取得的项**.      
 - unshift()在数组**前端**添加任意个项并返回**修改后的数组的长度**.          

### 5.2.5 重排序方法     
 - reverse()直接反转数组项的顺序.      
 - sort()方法按照升序排列数组项,但它会调用每个数组项的toString()转型方法,因此sort()方法实际上进行的是字符串的比较.         

``` javascript
	var nums = [0, 5, 1, 10, 15];
	var strs = ["a", "cc", "va", "ge"];

	var a = strs.reverse();
	console.log(a);                      //>["ge", "va", "cc", "a"]
	console.log(strs);                   //>["ge", "va", "cc", "a"]
	console.log(nums.reverse());         //>[15, 10, 1, 5, 0]
	console.log(nums.sort());            //>[0, 1, 10, 15, 5] 注意这里比较的是字符串

	function compare(value1, value2) {
		if (value1 < value2) {
			return -1;
		} else if (value1 == value2) {
			return 0;
		} else {
			return 1;
		}
	}

	nums = [0, 5, 1, 10, 15];
	strs = ["a", "cc", "va", "ge"];
	console.log(nums.sort(compare));     //>[0, 1, 5, 10, 15]
	console.log(strs.sort(compare));     //>["a", "cc", "ge", "va"]

	//当前提满足参数为数值类型或其valueOf()方法会返回数值类型的对象类型时,compare函数可简写为:
	function compare(value1, value2) {
		return value1 - value2;
	}
```

### 5.2.6 操作方法         
 - concat() 方法可以基于当前数组中的所有项创建一个新数组.具体来说,这个方法会先创建当前数组的一个副本,若参数为空,则直接返回参数,否则,将参数添加到新数组的末尾.      
 - slice() 能够基于当前数组中的一个或多个项创建一个新数组.它可以接受两个参数,即返回项的起始和结束位置.只有一个参数时,返回从该参数指定位置到当前数组末尾的所有项.      
 - splice() 的主要用途是向数组的中部插入项.该方法始终都会返回一个数组,该数组中包含从原始数组中删除的项,(如果没有删除任何项,则返回一个空数组)      

``` javascript
	var colors = ["red", "yellow"];
	colors2 = colors.concat(21, "32", {a: 2});
	console.log(colors2);            //>["red", "yellow", 21, "32", Objetc]
	alert(colors2);                  //red, yellow, 21, 32, [object Object]

	var str = ["a", "b", "c", "d"];
	var str2 = str.slice(1);
	var str3 = str.slice(1, 3);
	var str4 = str.slice(-3, -1);
	alert(str2);    //b, c, d
	alert(str3);    //b, c    不包括str[3]
	alert(str4);    //b, c    和str3相同,即如果slice()方法的参数中有负数,则用数组长度加上该数来确定相应的位置. 
```

![5.2.6.png](http://upload-images.jianshu.io/upload_images/3001083-603ee75b9d31b77b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)          

### 5.2.7 位置方法         
 - indexOf() 两个参数:要查找的项和(可选)查找起点位置的索引,从数组开头向后查找.      
 - lastIndexOf() 参数同上,从数组的末尾开始向前查找.           

``` javascript 
var numbers = [1,2,3,4,5,4,3,2,1]
alert(numbers.indexOf(4));            //3   即number[3]=4
alert(numbers.lastIndexOf(4));        //5   
alert(numbers.lastIndexOf(4, 4));     //3
```   

### 5.2.8 迭代方法   
![5.2.8.png](http://upload-images.jianshu.io/upload_images/3001083-7f6b9d6934b6098e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

### 5.2.9 归并方法    
![5.2.91.png](http://upload-images.jianshu.io/upload_images/3001083-fcce5b0f2463e033.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.2.92.png](http://upload-images.jianshu.io/upload_images/3001083-576f163b6322f3b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)            

## 5.3 Date类型         
 - ECMAScript中的Date类型使用自UTC()Coordinate Universal Time,国际协调时间)1970年1月1日午夜(零时)开始经过的毫秒数来保存日期.          
 - Date.parse()方法接收一个表示日期的字符串参数,然后尝试根据这个字符串返回响应日期的毫秒数.      
 - Date.UTC()      
 - Date.now()      
 - +new Date()      

![5.31.png](http://upload-images.jianshu.io/upload_images/3001083-1e4d57aed6262f12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.32.png](http://upload-images.jianshu.io/upload_images/3001083-201feac8c8d10036.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.33.png](http://upload-images.jianshu.io/upload_images/3001083-7dad2d929f493c39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
``` javascript
	var now = new Date();            //无参数时,新创建的对象自动获得当前日期和时间

	var date1 = new Date();
	var date2 = Date.now();
	var	date3 = +new Date();

	console.log("date1: ", date1);   //date1:  Tue Dec 20 2016 23:50:48 GMT+0800 (CST)
	console.log("date2: ", date2);   //date2:  1482249048596
	console.log("date3: ", date3);   //date3:  1482249048596
```

### 5.3.1 继承的方法        
 - Date类型重写了toLocaleString(), toString(), valueOf()      
 - toLocaleString(): 按照与浏览器设置的地区相适应的格式返回日期和时间.      
 - toString(): 通常返回带有时区信息的日期和时间,其中时间一般以军用时间.      
 - valueOf: 不返回字符串,而是返回日期的毫秒表示.      

### 5.3.2 日期格式化方法        
![5.3.2.png](http://upload-images.jianshu.io/upload_images/3001083-057ba8db52a3e11a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)          

### 5.3.3 日期/时间组件方法      
![5.3.31.png](http://upload-images.jianshu.io/upload_images/3001083-a15956dc3aebb048.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![5.3.32.png](http://upload-images.jianshu.io/upload_images/3001083-278fbfa5e4a8a09d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           

## 5.4 RegExp 类型          
[MDN上对正则表达式的讲解](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)      
 
![5.41.png](http://upload-images.jianshu.io/upload_images/3001083-9998c6aee380ef92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.42.png](http://upload-images.jianshu.io/upload_images/3001083-09ba3b8e077df6fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.43.png](http://upload-images.jianshu.io/upload_images/3001083-099c0b30f3eda8e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           
ECMAScript5明确规定,使用正则表达式字面量必须像直接调用RegExp构造函数一样,每次都创建新的RegExp实例.      

``` javascript
	re = /cat/g;
	console.log(re.test("catastrophe"));  //true
	console.log(re.test("catastrophe"));  //false
```

### 5.4.1 RegExp 实例属性        
![5.4.1.png](http://upload-images.jianshu.io/upload_images/3001083-7cbcffd008c1c1f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 5.4.2 RegExp 实例方法           
 - exec(): 接收一个参数,即要应用模式的字符串,然后返回包含第一个匹配项信息的数组.      
 - test(): 在模式与该参数匹配的情况下返回true.      
![5.4.21.png](http://upload-images.jianshu.io/upload_images/3001083-785eca0f15445806.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.4.22.png](http://upload-images.jianshu.io/upload_images/3001083-b54e8b41b9fe143e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.4.23.png](http://upload-images.jianshu.io/upload_images/3001083-6e9bc67ee84dcb6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      


### 5.4.3 RegExp 构造函数属性        

![5.4.31.png](http://upload-images.jianshu.io/upload_images/3001083-5cd5bbb657690330.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.4.32.png](http://upload-images.jianshu.io/upload_images/3001083-eee00461dc958c90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
      
![5.4.33.png](http://upload-images.jianshu.io/upload_images/3001083-58b78feec4604300.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 5.4.4 模式的局限性      
![5.4.4.png](http://upload-images.jianshu.io/upload_images/3001083-ccee79090774e234.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

##5.5 Function 类型         
每个函数都是Function类型的实例,而且都与其他引用类型一样具有属性和方法.由于函数是对象,因此函数名实际上也是一个指向函数对象的指针,不会与某个函数绑定,如:      

``` javascript
function sum(sum1, sum2) {
	return sum1 + sum2;
}

var anotherSum = sum;
alert(anotherSum(10, 10));   //20

sum = null;
alert(anotherSum(10, 10));   //20
```

**函数有两种声明方式**,一种是**函数声明**语法,一种是**函数表达式**语法,它们有细微的区别,具体的区别以后会讲到.        

``` javascript
function sum(sum1, sum2) {
	return sum1 + sum2;
}                                   // 函数声明 语法

var sum = function(sum1, sum2) {
	return sum1 + sum2;
}                                   // 函数表达式 语法
```

### 5.5.1 没有重载         
前面提到过函数没有重载,在这里详细了解了之后可以知道,因为**函数名是一个指针,指针只能指向一个对象,因此函数没有重载**.      

### 5.5.2 函数声明与函数表达式        
![5.5.21.png](http://upload-images.jianshu.io/upload_images/3001083-f61a22093be378bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.5.22.png](http://upload-images.jianshu.io/upload_images/3001083-fe18a15a4efe998c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 5.5.3 作为值的函数          
因为ECMAScript中的函数名本身就是变量,所以函数也可以作为值来使用.也就是说,不仅可以像传递参数一样把一个函数传递给另一个函数,而且可以将一个函数作为另一个函数的结果返回,如:      

``` javascript
funcrion callSomeFunction(somFunction, someArgument) {
	return someFunction(someArgument);
}

function add10(num) {
	return num + 10;
}

var result1 = callSomeFunction(add10, 10);     //注意这里是add10,不是add10()
alert(result1);            //20
```

**注意,要访问函数的指针而不是执行函数的话,必须去掉函数名后的括号.**        

从一个函数中返回另一个函数,是一种非常有用的技术.比如有一个对象数组,我们想要根据对象属性对数组进行排序.这时就可以定义一个函数,它接收一个属性名,然后根据这个属性名来创建一个比较函数.      

``` javascript
	function createComparisonFunction(propertyName) {
		return function(object1, object2) {
			var value1 = object1[propertyName];
			var value2 = object2[propertyName];

			if (value1 < value2) {
				return -1;
			} else if (value1 == value2) {
				return 0;
			} else {
				return 1;
			}
		}
	}

	var object = [{name: "Nick", age: 29}, {name: "Maggie", age:25}];
	object.sort(createComparisonFunction("name"))
	console.log(object[0].name);
	object.sort(createComparisonFunction("age"))
	console.log(object[0].age);
```

### 5.5.4 函数内部属性        
 - 牢记,函数是一个对象,函数名是指向这个对象的指针.      
 - 在严格模式下,禁用arguments.callee和arguments.caller.      
 - 在严格模式下,禁止this指向全局对象.          

![5.5.41.png](http://upload-images.jianshu.io/upload_images/3001083-94f4d12bc654d5e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.5.42.png](http://upload-images.jianshu.io/upload_images/3001083-5345ea94847aedf6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.5.43.png](http://upload-images.jianshu.io/upload_images/3001083-138f20461b35bc82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.5.44.png](http://upload-images.jianshu.io/upload_images/3001083-36d594bc8d28b1d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 5.5.5 函数属性和方法          
 - 每个函数都包含两个属性: length和prototype.      
 - 对于ECMAScript中的引用类型而言,prototype是保存它们所有实例和方法的真正所在.      
 - 每个函数都包含两个飞机成而来的方法: apply() 和 call(). 这两个方法的用途都是在**特定的作用域**中,实际上等于设置函数体内this对象的值.      
 - 在严格模式下,未指定环境对象而调用函数,则this值不会转型为window,除非明确把函数添加到某个对象或者调用apply()或call(),否则this值将是undefined.         

![5.5.51.png](http://upload-images.jianshu.io/upload_images/3001083-2cacf5cd29da13d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.5.52.png](http://upload-images.jianshu.io/upload_images/3001083-692a0f7f13925aef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.5.53.png](http://upload-images.jianshu.io/upload_images/3001083-578ea591d44c53b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.5.54.png](http://upload-images.jianshu.io/upload_images/3001083-5405067bd06b5995.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.5.55.png](http://upload-images.jianshu.io/upload_images/3001083-b86d33ec0ae9f455.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 5.6 基本包装对象        
![5.61.png](http://upload-images.jianshu.io/upload_images/3001083-55d70d8216af1836.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.62.png](http://upload-images.jianshu.io/upload_images/3001083-11a8a4b5fb97c477.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**其实《权威指南》上说，其实不一定真正有创建那个包装对象的过程。用来当作一种理解就好了。不管如何理解，在后台确实是进行了一些操作的，但是我们看不见。**      

### 5.6.1 Boolean类型        
Boolean类型是布尔值对应的引用类型。Boolean类型的实例重写了valueOf（）方法，返回基本类型值true和false。也重写了toString（）砝码嗯法，返回字符串“true”和“false”。      

``` javascript
var falseObject = new Boolean(false);
var result = falseObject && true;    //布尔表达式中的所有对象都会被转为true，所以falseObject这里会被转为true
alert(result);            //true

var falseValue = false;
result = falseValue && true;
alert(result);            //false

alert(typeof falseObject);  //object
alert(typeof falseValue);   //boolean
alert(falseObject instanceof Boolean);  //true
alert(falseValue instanceof Boolean);   //false
```

### 5.6.2 Number 类型          
 - Number类型也重写了valueOf(),toLocaleString()和toString()方法,重写后的valueOf()      方法返回对象表示的基本类型的数值,另外两各方法则返回字符串形式的数值.给toString()方法传一个参数还可以返回对应进制数值的字符串形式.
 - toFixed(): 按照指定的小数位返回数值的字符串表示.      
 - toExponential(): 返回以指数表示法表示的数值输出结果中的小数位数.      
 - toPrecision(): 根据要处理的数值决定是调用toFixed()还是调用toExponential(),这三个方法都可以通过向上或向下舍入,做到以最准确的形式来表示带有正确小数位的值.      


``` javascript
var num1 = 10;
alert(num1.toFixed(2));    //"10.00"

var num2 = 10.005;
alert(num2.toFixed(2));   //"10.01"

var num3 = 10;
alert(num3.toEcponential(1));  //"1.0e+1"

var num4 = 99;
alert(num4.toPrecision(1));    //"1e+2"
alert(num4.toPrecision(2));    //"99"
alert(num4.toPrecision(3));    //"99.0"

var numberObject = new Number(10);
var numberValue = 10;
alert(typeof numberObject);    //"object"
alert(typeof numberValue);     //"number"
alert(numberObject instanceof Number);  //true
alert(numberValue instanceof Number);   //false 
```

### 5.6.3 String 类型         
 - **valueOf(), toLocaleString(), toString()**都返回对象所表示的基本字符串值.      
 - **字符方法**: **charAt()**和**charCodeAt()**.这两个方法都接收一个参数,即基于0的字符位置.其中charAt()方法以单字符串的形式返回给定位置的那个字符,charCodeAt()是返回字符编码.      
 - **字符串操作方法**:**concat()**,用于将一个或多个字符拼接起来,返回拼接得到的新字符串.还有**slice(),substr(),substring()**,这三个都会返回被操作字符串的一个子字符串,而且都接受一个或两个参数,第一个参数指定字符串的开始位置,第二个参数(在指定的情况下)表示字符串到哪里结束.      
 - **字符串位置方法**: indexOf(), lastIndexOf()都是从一个字符串中搜索给定的子字符串,然后返回子字符串的位置(没找到则返回-1).      
 - **trim()**方法,会创建一个字符串的副本,删除前置及后缀的所有空格,然后返回结果.      
 - **字符串大小写转换方法**:**toLowerCase() , toLocaleLowerCase() , toUpperCase() , toLocaleUpperCase().**
 - **字符串模式匹配方法**:**match(), search()**只接受一个参数,要么是一个正则表达式,要么是一个RegExp对象.      
 - **简单替换字符串的操作: replace()**,接收两个参数,第一个是一个RegExp对象或者一个字符串(这个字符串不会被转换成正则表达式),第二个参数是一个字符串或一个函数.      
 - **将一个字符串拆分为多个字符串:split()**      
 - **localeCompare()**:比较两个字符串,并返回-1, 0 或 1.      
 - **fromCharCode()**:接收一个或多个字符串编码,然后将它们转换成字符串.      

``` javascript
var str = "hello world";
alert(str.charAt(1));         //"e"
alert(str.charCodeAt(1));     //输出"101"

var str2 = "hello ";
var str3 = str2.concat("world");
alert(str3);                  //"hello world"
alert(str2);                  //"hello"

var str4 = "hello wolrd";
alert(str4.indexOf("o"));     //4
alert(str4.lastIndexOf("o")); //7
alert(str4.indexOf("o", 6));  //7

alert(String.fromCharCode(104, 101, 108, 108, 111));  //"hello"
```
![5.6.31.png](http://upload-images.jianshu.io/upload_images/3001083-032671b225501057.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.6.32.png](http://upload-images.jianshu.io/upload_images/3001083-d10cac52da3de287.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.6.33.png](http://upload-images.jianshu.io/upload_images/3001083-3c7aae3baa63110b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.6.34.png](http://upload-images.jianshu.io/upload_images/3001083-742ffbe718fb95f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.6.35.png](http://upload-images.jianshu.io/upload_images/3001083-03cc7325e788735a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.6.36.png](http://upload-images.jianshu.io/upload_images/3001083-6fd78b8d62bf2c86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.6.37.png](http://upload-images.jianshu.io/upload_images/3001083-87a0a0cb4e7a447f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.6.38.png](http://upload-images.jianshu.io/upload_images/3001083-7df9fd14b0257153.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

## 5.7 单体内置对象         
![5.71.png](http://upload-images.jianshu.io/upload_images/3001083-e77a94bc84d5a6a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           

### 5.7.1 Global 对象      
![5.7.1.png](http://upload-images.jianshu.io/upload_images/3001083-3355e7f443f261e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.7.12.png](http://upload-images.jianshu.io/upload_images/3001083-809265f2c1c016d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.7.13.png](http://upload-images.jianshu.io/upload_images/3001083-f76d65c8c3099fda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![5.7.14.png](http://upload-images.jianshu.io/upload_images/3001083-0ecd9971e981312e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 5.7.2 Math 对象        
 - ECMAScript为保存数学公式和信息提供了一个公共位置,即Math对象,其中包含了很多方法.      
 - **min()和max()**,确定一组数值中的最小值和最大值.接受任意数量个参数.      
 - **舍入方法: Math.ceil()**向上舍入,**Math.floor()**向下舍入,**Math.round()**四舍五入.      
 - **random()**:返回大于等于0小于1的一个随机数.      

``` javascript
var max = Math.max(10,13,456,48,10,0);
alert(max);      //456
```

**其他方法:**        
![5.7.2.png](http://upload-images.jianshu.io/upload_images/3001083-44edb7e327702254.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 5.8 小结      
![5.8.png](http://upload-images.jianshu.io/upload_images/3001083-a434ba8fe53ffa06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 相关文章链接    
 - [Javascript深入学习1——词法结构、类型、值、变量](http://liveipool.com/blog/2016/09/12/learn-javascript-1/)       
 - [JavaScript红皮书学习(1,2)--JavaScript简介](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-1,2-Introduction/)  
 - [JavaScript红皮书学习(3)--基本概念](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-3-BasicConcepts/)   
 - [JavaScript红皮书学习(4)--变量、作用域和内存问题](http://liveipool.com/blog/2016/12/19/JavaScript-RedBook-4-Variable-Scope-and-Memory/)    
 - [JavaScript红皮书学习(5)--引用类型](http://liveipool.com/blog/2016/12/22/JavaScript-RedBook-5-Reference-Type)     
 - [JavaScript学习专题之--作用域](http://liveipool.com/blog/2016/12/22/JavaScript-Scope)   
 - [JavaScript学习专题之--闭包](http://liveipool.com/blog/2016/12/23/JavaScript-Closures)     
 - [JavaScript学习专题之--私有变量和ES5中的模块模式](http://liveipool.com/blog/2016/12/23/JavaScript-Private-Variable-and-ES5Modules)          

