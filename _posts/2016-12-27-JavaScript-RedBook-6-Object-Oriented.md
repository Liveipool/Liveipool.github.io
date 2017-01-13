---
layout: post
title: JavaScript红皮书学习(6)--面向对象的程序设计
date: 2016-12-27
categories: blog
description: JavaScript学习
---

# 面向对象的程序设计      
对象一直是各种编程语言中的重要概念,面向对象也一直是一个非常热点的话题,身边的程序员常常抱怨找不到对象,可见对象不仅重要,也非常地难以彻底掌握.      
面向对象(Object-Oriented, OO)的语言有一个标志,那就是它们都有类的概念,而通过类可以创建任意多个具有相同属性的方法的对象.ECMAScript中没有类的概念,因此它的对象也与基于类的语言中的对象有所不同.      
ECMA-262把对象定义为:"无序属性的集合,其属性可以包含基本值,对象或者函数.",严格来讲,这就相当于说对象是一组没有特定顺序的值.对象的每个属性或方法都有一个名字,而每个名字都映射到一个值.正因为这样,我们可以把ECMAScript的对象想象成散列表:无非就是一组名值对,其中值可以是数据或函数.      

## 6.1 理解对象        
创建对象有两种方法,第一种是创建一个Object的实例,然后再为它添加属性和方法:      

``` javascript
var person = new Object();
person.name = "liveipool";
```

第二种是现在使用得最多的对象字面量模式:      

``` javascript
var person = {
	name: "liveipool",
	age: 29
}
```

### 6.1.1 属性类型       
ECMA-262第5版在定义只有内部才用的特性(attribute)时,描述了属性(property)的各种特征,ECMA-262定义这些特性是为了实现JavaScript引擎用的,因此**在JavaScript中不能直接访问它们.**为了表示特性是内部值,该规范把它们放在了[[]]中,例如[[Enumerable]].      
ECMAScript中有两种属性:数据属性和访问器属性.      

**数据属性:**      
数据属性包含一个数据值的位置.在这个位置可以读取和写入值.数据属性有4个描述其行为的特征:      
1. [[Configureable]]: 表示能否通过但delete删除属性从而重新定义属性,默认为true.      
2. [[Enumerable]]: 表示能否通过for-in循环返回属性.默认为true.      
3. [[Writable]]: 表示能否修改属性的值.默认为true.      
4. [[Value]]: 包含这个属性的数据值.默认为undefined.      
要修改属性默认的值,必须使用ECMAScript5的Object.defineProperty()方法.这个方法接收三个参数: 属性所在的对象,属性的名字和一个描述符对象.      

``` javascript
	var person = {};

	Object.defineProperty(person, "name", {
		configurable: false,
		writable: false,         //当不设置除value外的几个属性,这三个值都会变为false
		value: "liveipool"
	});

	console.log(person.name);   //liveipool
	person.name = "Nick";       //严格模式下会报错,不能对只读属性赋值
	console.log(person.name);   //liveipool
	delete person.name;
	console.log(person.name);   //liveipool

	Object.defineProperty(person, "name", {
		configurable: true      //上面configurable属性设为false之后这里再设为true就会报错.
	});
```

**访问器属性:**      
访问器属性不包含数据值.      
1. [[Configurable]]: 同上.      
2. [[Enumerable]]: 同上.      
3. [[Get]]: 在读取属性时调用的函数,默认为undefined.      
4. [[Set]]: 在写入属性时调用的函数.默认为undefined.      

``` javascript
	var book = {
		_year: 2004,
		edition: 1
	};

	Object.defineProperty(book, "year", {
		get: function() {
			return this._year;
		},
		set: function(newValue) {
			if (newValue > 2004) {
				this._year = newValue;
				this.edition += newValue - 2004;
			}
		}  //不一定要同时指定getter和setter.只指定getter意味着不能写.
	});

	book.year = 2005;
	console.log(book.edition);   //2
```

### 6.1.2 定义多个属性      
![6.1.21.png](http://upload-images.jianshu.io/upload_images/3001083-96e820fb755b27f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.1.22.png](http://upload-images.jianshu.io/upload_images/3001083-fe8c1ce46353935f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 6.2 创建对象        
 - 除了最基本的使用构造函数和对象字面量的方法可以用来创建单个对象,还有七种常见的模式,可用来创建更多的对象:      
 - 工厂模式      
 - 构造函数模式      
 - 原型模式      
 - 组合使用构造函数模式和原型模式      
 - 动态原型模式      
 - 寄生构造函数模式      
 - 稳妥构造函数模式      

### 6.2.1 工厂模式      
工厂模式是软件工程领域一种广为人知的设计模式,**这种模式抽象了创建具体对象的过程.**考虑到ECMAScript无法创建类,开发人员就发明了一种函数,用函数来**封装**以特定接口创建对象的细节:      

``` javascript
function createPerson(name, age, job) {
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	return o;
}
var person1 = createPerson("liveipool", 18, "student");
```

### 6.2.2 构造函数模式      
除了像Object, Array这种原生构造函数在运行时会自动出现在执行环境中.也可以创建自定义的构造函数,从而定义自定义对象类型的属性和方法:      

``` javascript
function Person(name, age, job) {
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = function() {
		console.log(this.name);
	}
}
var person1 = new Person("liveipool", 18, "student");
person1.sayName();   //"liveipool"

Person("Nick", 28, "teacher");   //添加到window
window.sayName();    //"Nick"
```
要创建Person的新实例,必须使用new操作符.以这种方式调用构造函数实际上会经历以下4个步骤:      
1. 创建一个新对象;      
2. 将构造函数的作用域赋给新对象(因此this就指向了这个新对象);      
3. 执行构造函数中的代码(为这个新对象添加属性);      
4. 返回新对象      

以这种模式创建的实例会有一个constructor属性,该属性指向Person.      

``` javascript
console.log(person1.constructor === Person);    //true
console.log(person1 instanceof Object);         //true
console.log(person1 instanceof Person);         //true
```

**使用构造函数的一个确定在于,每个方法都要在每个实例上重新创建一遍,因此不同实例上的同名函数也是不相等的.这样的情况确实显得有点累赘,为了解决这个问题,我们引入了原型模式.**      

### 6.2.3 原型模式      
 - 要想理解清楚这一部分,首先要牢记三个重要概念: **构造函数(constructor)**、**原型对象(prototype)**、**实例**.      
 - 我们创建的每个函数都有一个**prototype(原型)属性**,这个属性是一个**指针**,指向**原型对象**.      
 - 默认情况下,每个**原型对象**也都有一个**constructor指针**,它指向**构造函数**.      
 - 每个**实例**都有一个**[[Prototype]]指针**,指向**原型对象**.      
![6.2.3.png](http://upload-images.jianshu.io/upload_images/3001083-ad676ed44eef0a35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

``` javascript
	function Person() {}

	Person.prototype.name = "Nick";
	Person.prototype.age = 29;
	Person.prototype.sayName = function() {
		console.log(this.name);
	}

	var person1 = new Person();
	person1.sayName();           //"Nick"
	var person2 = new Person();
	person2.sayName();           //"Nick"
	console.log(person1.sayName === person2.sayName); //true

	console.log(Person.prototype.constructor === Person);  //true
	console.log(person1.__proto__);  
	console.log(Person.prototype); 
    /*上面两行都输出一个对象: {
		age: 29,
		>constructor: function Person(),      //>都表示还有展开内容
		name: "Nick",
		>sayName: function(),
		>__proto__: object
    } */

    console.log(Person.prototype.isPrototypeOf(person1));   //true
    console.log(Object.getPrototypeOf(person1) === Person.prototype);  //true

    person1.name = "liveipool";     //不能通过对象实例重写原型中的值
    person1.sayName();    //"liveipool"
    person2.sayName();	  //"Nick"
    console.log(person1.hasOwnProperty("name"));  //true hasOwnProperty可以检测该属性是否是在实例中
    console.log("name" in person1);   //true in操作符会在只要通过对象能够访问给定属性时返回true
    delete person1.name;  //使用delete操作符可以删除 实例属性,这样可以取消掉实例属性对原型中的属性的遮蔽,从而能够重新访问到原型中的属性
    person1.sayName();    //"Nick"
    console.log(person1.hasOwnProperty("name"));  //false
    console.log("name" in person1);   //true
    
    //可以结合hasOwnProperty()和in操作符实现一个hasPrototypeProperty()方法.
    function hasPrototypeProperty(obj, name) {
    	return !obj.hasOwnProperty(name) && (name in obj);
    }
	//还可以使用for-in循环返回所有能够通过对象访问的,可枚举的(enumerable)属性,实例中的和原型中的都包括在内.

	//Object.keys方法,接收一个对象为参数,返回一个包含所有可枚举属性的字符串数组.
    var keys1 = Object.keys(Person.prototype);
    console.log(keys1);  //"name", "age", "sayName"
    var keys2 = Object.keys(person1);
    console.log(keys2);  //"name"
    //getOwnPropertyNames返回所有实例属性,无论是否可枚举
    var keys3 = Object.getOwnPropertyNames(Person.prototype);
    console.log(keys3);  //"constructor", "name", "age", "sayName"
    var keys4 = Object.getOwnPropertyNames(person1);
    console.log(keys4);  //"name"
```
虽然在脚本中我们没有标准的形式访问各实例的[[Prototype]]属性,但FireFox, Safari和Chrome在每个对象上都支持一个属性**__proto__**.不过要切忌的是,这个连接存在于**实例(person1)**与**构造函数的原型对象(Person.prototype)**之间,不是存在于实例与构造函数之间.      
还要注意,person1和person2中都不包含属性和方法,它们的属性和方法都是通过向上搜索来实现的.      

为了减少不必要的输入,也为了从视觉上更好地封装原型的功能,更常见的做法是用一个包含所有属性和方法的对象字面量来重写整个原型对象.      

``` javascript
function() {}

Person.prototype = {
	name: "liveipool",
	age: 28,
	sayName: function() {
		console.log(this.name);
	}
}
```
**但是,这样将Person.prototye设置为等于一个以对象字面量形式创建的新对象,会使constructor属性不再指向Person了.**      

``` javascript
var friend = new Person();
alert(friend instanceof Object);  //true
alert(friend instanceof Person);  //true
alert(friend.constructor === Person);  //false
alert(friend.constructor === Object);  //true
```
尽管instanceof操作符还能返回正确的结果,但通过constructor已经无法确定对象的类型了.      
如果实在需要constructor属性,可以显式地进行设置:      


``` javascript
function() {}

Person.prototype = {
	constructor: Person,  //但是这种方式重设constructor会导致它的[[Enumerable]]特性被设置为true,而默认情况下,原生的constructor是不可枚举的.
	name: "liveipool",
	age: 28,
	sayName: function() {
		console.log(this.name);
	}
}
```
此外,还值得注意的是:      
1. 原型具有动态性,我们对原型对象所做的任何修改都能够立即从实例上出来.(不要使用对象字面量形式重写整个原型对象,会出现问题,因为那样相当于切断了构造函数和最初原型之间的联系).      
2. 原型模式的重要性不仅体现在创建自定义类型方面,所有原生的引用类型也都是采用这种模式创建的.      
3. 原型模式也有缺点,因为它的所有属性是被很多实例共享的,因此对属性的更改等都会影响所有的实例.      

### 6.2.4 组合使用构造函数模式和原型模式      
为了弥补原型模式的缺点,我们可以**组合使用构造函数模式和原型模式**, 这也是创建自定义类型的最常见方法.      
构造函数模式用于定义实例属性,而原型模式用于定义方法和共享的属性.这样,**每个实例都会有自己的一份实例属性的副本,但同时又共享着对方法的引用,最大限度地节省了内存.**      
另外,这种混成模式还支持向构造函数传递参数:可谓是集两种模式之长.      

``` javascript
    function Person(name, age) {
    	this.name = name;
    	this.age = age;
    }

    Person.prototype = {
    	sayName: function() {
    		console.log(this.name);
    	}
    }

    var person1 = new Person("liveipool", 32);
    person1.sayName();
```

这种构造函数和原型混成的模式,是目前ECMAScript中使用最广泛,认同度最高的一种创建自定义类型的方法.可以说,这是用来定义引用类型的一种默认模式.      

### 6.2.5 动态原型模式      
动态原型模式把所有的信息都封装在了构造函数中,而通过在构造函数中初始化原型(仅在必要的情况下),又保持了组合使用构造函数和原型的优点.换句话说,可以通过检查某个应该存在的方法是否有效,来决定是否需要初始化原型.      

``` javascript
    function Person(name, age) {
    	this.name = name;
    	this.age = age;
    	// this.sayName = function() {
    		// console.log("hello");
    	// }  //这个函数未被注释时最后输出hello
    	if (typeof this.sayName != "function") {
    		Person.prototype.sayName = function() {
    			console.log(this.name);
    		}
    	}
    }

    var person1 = new Person("liveipool", 23);
    person1.sayName();  //"liveipool"
```

### 6.2.6 寄生构造函数模式      
当前述的几种模式都不适用的情况下,可以使用寄生(parasitic)构造函数模式.这种模式的基本思想是创建一个函数,该函数的作用仅仅是封装创建对象的代码,然后再返回新创建的对象.      

```javascript
function Person(name, age) {
	var o = new Object();
	o.name = name;
	o.age = age;
	o.sayName = function() {
		console.log(this.name);
	}
	return o;
}
```

这个模式可以在特殊的情况下为对象创建构造函数.假设我们想创建一个具有额外方法的特殊数组.由于不能直接修改Array构造函数,因此可以使用这个模式.      

``` javascript
function SpecialArray() {
	var values = new Array();
	//添加值
	values.push.apply(values, arguments);
	//添加方法
	values.toPipedString = function() {
		return this.join('|');
	};
	return values;
}

var colors = new SpecialArray("red", "blue", "green");
alert(colors.toPipedString());  //"red|blue|green"
console.log(colors instanceof SpecialArray);  //false
```
使用寄生构造函数模式返回的对象与构造函数或者与构造函数的原型属性之间没有关系.也就是说,构造函数返回的对象与在构造函数外部创建的对象没有什么不同.      
因此,不太建议使用这种模式.      

### 6.2.7 稳妥构造函数模式      
![6.2.71.png](http://upload-images.jianshu.io/upload_images/3001083-a79c0cf547ecadff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![6.2.72.png](http://upload-images.jianshu.io/upload_images/3001083-33ce061c04f7ff3d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 6.3 继承      
继承是OO语言中的一个最为人津津乐道的概念.许多OO语言都支持两种继承方式: **接口继承**和**实现继承**.**接口继承只能继承方法签名,而实现继承则继承实际的方法.**      
因为ECMAScript中的函数没有签名,因此ECMAScript无法实现接口继承,只能支持实现继承,而实现继承主要是依靠**原型链**来实现的.      

### 6.3.1 原型链      
![6.3.11.png](http://upload-images.jianshu.io/upload_images/3001083-e1a2a4725be804e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.3.12.png](http://upload-images.jianshu.io/upload_images/3001083-113b9779bfebf4d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.3.13.png](http://upload-images.jianshu.io/upload_images/3001083-fd53d7c2c5447c7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**和上一节的原型模式类似,原型链主要有两个问题:**      
1. 牵一发而动全身,更改原型的属性会影响到所有实例.      
2. 在创建子类型的实例时,不能向超类型的构造函数中传递参数.也就是说没有办法在不影响所有对象实例的情况下,给超类型的构造函数传递参数.      

因此,很少单独使用原型链.      

### 6.3.2 借用构造函数
![6.3.21.png](http://upload-images.jianshu.io/upload_images/3001083-6e0b4d9c89e9c913.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**现在,每个SubType的实例都有它们自己colors属性的副本了.**
![6.3.22.png](http://upload-images.jianshu.io/upload_images/3001083-2a4fa993bbb916da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6.3.3 组合继承      
组合继承(combination inheritance),有时候也叫伪经典继承,指的是将原型链和借用构造函数的技术组合到一块.**使用原型链实现对原型属性和方法的继承,而通过借用构造函数来实现对实例属性的继承.**      

``` javascript
	function SuperType(name) {
		this.name = name;
		this.colors = ["red", "blue", "green"];
	}

	SuperType.prototype.sayName = function() {
		console.log(this.name);
	}

	function SubType(name, age) {
		SuperType.call(this, name);
		this.age = age;
	}

	SubType.prototype = new SuperType();
	SubType.prototype.constructor = SubType; //SubType.prototype.constructor赋值前等于SuperType
	SubType.prototype.sayAge = function() {
		console.log(this.age);
	}

	var instance1 = new SubType("liveipool", 20);
	instance1.colors.push("black");
	console.log(instance1.colors);    //"red", "blue", "green", "black"
	instance1.sayName();              //"liveipool"
	instance1.sayAge();               //20

	var instance2 = new SubType("Nick", 27);
	console.log(instance2.colors);    //"red", "blue", "green"
	instance2.sayName();              //"Nick"
	instance2.sayAge();               //27
```

### 6.3.4 原型式继承        
![6.3.41.png](http://upload-images.jianshu.io/upload_images/3001083-d8d3da6b8a9e78bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.3.42.png](http://upload-images.jianshu.io/upload_images/3001083-95d4877a4cd8bb96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.3.43.png](http://upload-images.jianshu.io/upload_images/3001083-15e4a4ae7a94e0c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.3.44.png](http://upload-images.jianshu.io/upload_images/3001083-bca1b2bf67c09daf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 6.3.5 寄生式继承      
![6.3.5.png](http://upload-images.jianshu.io/upload_images/3001083-ba71d84f6ee15e43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 6.3.6 寄生组合式继承      
![6.3.61.png](http://upload-images.jianshu.io/upload_images/3001083-6f2f05ebd0413b45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.3.62.png](http://upload-images.jianshu.io/upload_images/3001083-dfb0d7c4f40da8c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.3.63.png](http://upload-images.jianshu.io/upload_images/3001083-056c0b230619d558.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.3.64.png](http://upload-images.jianshu.io/upload_images/3001083-01057e2d9b8939cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![6.3.65.png](http://upload-images.jianshu.io/upload_images/3001083-a8b14cd1d0945a67.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      


## 6.4 小结      
![6.4.png](http://upload-images.jianshu.io/upload_images/3001083-c7c5ce86c97ef3d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

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