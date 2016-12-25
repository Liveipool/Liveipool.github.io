---
layout: post
title: JavaScript学习专题之--this  
date: 2016-12-25
categories: blog
description: JavaScript学习
---

# This is a this      
this关键字是JavaScript中最复杂的机制之一.他是一个很特别的关键字,**被自动定义到所有函数的作用域中.**    

## 1. 为什么要用this    
对比下面两段代码.    

``` javascript
	function identify() {
		return this.name;
	}

	function speak() {
		console.log(this);            //speak.call(me)时为object{name:"Liveipool"}
		var greeting = "Hello, I'm " +  identify.call(this);
		console.log(greeting);
	}

	var me = {
		name: "Liveipool"
	}

	var you = {
		name: "Daniel"
	}
	console.log(this);                        //Window{....}
	console.log(identify.call(me));           //Liveipool
	console.log(identify.call(you));          //Daniel

	speak.call(me);                  //Hello, I'm Liveipool
	speak.call(you);                 //Hello, I'm Daniel
```

``` javascript
	function identify(context) {
		return context.name;
	}

	function speak(context) {
		var greeting = "Hello, I'm " + identify(context);
		console.log(greeting);
	}

	var me = {
		name: "Liveipool"
	}

	var you = {
		name: "Daniel"
	}

	console.log(identify(me));        //Liveipool
	speak(you);                       //Hello, I'm Daniel
```
可以看出,虽然输出结果都是一样的,但是如果不使用this,就需要给identify()和speak()显式地传入一个上下文对象.随着代码中使用的模式越来越复杂,显式传递上下文对象就会让代码变得越来越混乱,使用this就不会这样.**this提供了一种更优雅的方式来隐式'传递'一个对象引用,可以用来将API设计得更加简洁并且易于复用.**    

## 2.误解    
如果没有深入理解过this,很可能会被它的名字误解.而常见的误解一般有两种:    

#### 2.1指向自身    
人们很容易把this理解成自身,这就是典型的根据this的英文意思来理解的.    

考虑下面代码:    

``` javascript
	function foo(num) {
		console.log("foo: " + num);
		console.log("count: " + foo.count);
		//记录foo被调用的次数
		this.count++;
	}

	foo.count = 0;

	for (var i = 1; i < 5; i++) {
		foo(i);
		//foo: 1  count: 0
		//foo: 2  count: 0
		//foo: 3  count: 0
		//foo: 4  count: 0
	}

	//foo 被调用了多少次?
	console.log(foo.count);   //0
	console.log(count);		  //NaN
	console.log(window.count);//NaN
```
显然,这样是无法得到想要的结果的.在执行foo.count = 0时,的确向函数对象foo添加了一个属性count.但是**函数内部代码this.count中的this并不是指向那个函数对象,**所以虽然属性名相同,根对象却并不相同,因此出现了这样的输出结果.    
那么每次循环增加的count是哪个count呢?实际上,**this.count++这行代码在无意中创建了一个全局变量count,**并且没有给他初始化值,因此undefined++就变成了NaN.    

说回代码,其实只要进行简单的修改就能得到我们想要的结果:    

``` javascript
	function foo(num) {
		console.log("foo: " + num);
		//记录foo被调用的次数
		data.count++;
	}

	var data = {
		count: 0
	}

	for (var i = 1; i < 5; i++) {
		foo(i);
		//foo: 1
		//foo: 2
		//foo: 3
		//foo: 4
	}

	//foo 被调用了多少次?
	console.log(data.count);   //4
	console.log(count); //报错
```
这种方式是使用的词法作用域,虽然它可以解决这个问题,但却和这篇博客的主题优点跑偏了,不能说使用this出现了问题就不用了,我们得找到问题并解决它.    

解决办法:    

``` javascript
	function foo(num) {
		console.log("foo: " + num);
		console.log("count: " + foo.count);
		//记录foo被调用的次数
		//注意, 此时this确实指向foo
		this.count++;
	}

	foo.count = 0;

	for (var i = 1; i < 5; i++) {
		//使用call()可以确保this指向函数对象foo本身
		foo.call(foo, i);
		//foo: 1  count: 0
		//foo: 2  count: 1
		//foo: 3  count: 2
		//foo: 4  count: 3
	}

	//foo 被调用了多少次?
	console.log(foo.count);   //4
	console.log(count);		  //报错
```
实际上就更改了一排代码,使用了foo.call()就解决了问题,通过这三段代码的对比,应该可以感觉到this的一些特性,要是还是觉得有点疑惑,下文中会有详细的原理解释.    

#### 2.2它的作用域           
this指向函数的作用域较为复杂,不同情况下指向的作用域可能不同,很难给出具体的定义.但要记住,不要像下面一样想要把this和词法作用域的查找混合起来写代码:    

``` javascript
	function foo() {
		var a = 2;
		this.bar();
	}

	function bar() {
		console.log(this.a);
	}

	foo();   //这样是肯定得不到正确的值的
```

## 3.this到底是什么    
**this是在运行时绑定的,不是在编写时绑定的,它的上下文取决于函数调用时的各种条件.this的绑定和函数声明的位置没有任何关系,只取决于函数的调用方式.**    
**当一个函数被调用时,会创建一个活动记录(有时候也称为执行上下文).这个记录会包含函数在哪里被调用(调用栈)、函数的调用方式、传入的参数等信息.**    
**this就是这个记录的一个属性,会在函数执行的过程中用到.**       

## 4.调用位置      
调用位置就是函数在代码中被调用的位置(而不是声明的位置).    
寻找调用位置最重要的是要**分析调用栈(为了到达当前执行位置所调用的所有函数)**,而我们要找的**调用位置**就在当前正在执行的函数的**前一个调用中**.    

``` javascript
function baz() {
	//当前调用栈是: baz
	//因此,当前调用位置是全局作用域
	bar()； //bar()的调用位置
}

function bar() {
	//当前调用栈是baz --> bar
	//因此,当前调用位置在baz中
	foo()；//foo()的调用位置
}

function() foo() {
	//当前调用栈是 baz --> bar --> foo
	//因此,当前调用位置在bar中
}

baz()； //baz的调用位置
```
可以通过浏览器的开发者工具查看调用栈(设置debugger;)    
![sc100.png](http://upload-images.jianshu.io/upload_images/3001083-e332fd92ad5659f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

## 5.绑定规则      
首先,我们找到了调用位置,接下来,我们就需要判断需要应用**四条绑定规则**中的哪一条.       

#### 5.1 默认绑定    
最常用的调用类型:独立函数调用.可以把这条规则看作是无法应用其他规则时的默认规则.    

``` javascript
function foo() {
	console.log(this.a);
}

var a = 2;
foo(); //2 非严格模式下
```
例中,函数调用时应用了this的默认绑定,因此this指向全局变量.    
在代码中,如果函数是直接使用不带任何修饰的函数引用进行调用的,因此只能使用默认绑定,无法应用其他规则.    

#### 5.2 隐式绑定     
这一条规则考虑的是调用位置是否有上下文对象,或者说是否被某个对象拥有或包含.    

``` javascript
function foo() {
	console.log(this.a);
}

var obj = { 
	a: 2,
	foo: foo
};

obj.foo();     //2	
```
注意,**obj对象中只是有了foo()函数的一个引用,foo()函数还是属于全局.**    
**当foo()被调用时,它的前面加上了对obj的引用,因此,调用位置会使用obj上下文来引用函数.当函数引用有上下文对象时,隐式绑定规则会把函数调用中的this绑定到这个上下文对象.**    
换句话说,就是现在有一个foo()函数,foo和obj.foo是两个对这个函数的引用,使用obj.foo引用调用foo()函数时,obj成了foo()函数的上下文对象,隐式绑定规则就会把this指针绑定到obj对象上.    

**注意,当对象属性引用链中只有上一层或者说最后一层在调用位置中起作用:**    

``` javascript
function foo() {
	console.log(this.a);
}
var obj2 = {
	a: 42,
	foo: foo
}
var obj1 = {
	a: 2,
	obj2: obj2
}
obj1.obj2.foo();  //42
```

然而,隐式绑定有一个很常见的问题:**被隐式绑定的函数会丢失绑定对象,也就是说它会应用默认绑定规则,从而把this绑定到全局对象或者undefined(TypeError)上,取决于是否是严格模式.**    

``` javascript
	function foo() {
		console.log(this.a);
	}

	var obj = {
		a:2,
		foo: foo
	}

	var bar = obj.foo;
	var a = "global";
	bar();    //global
```
虽然看起来bar是obj.foo的一个引用,但实际上,bar还是指向foo()函数的一个引用,因此现在bar()就成了一个不带任何修饰的函数调用,因此会采用默认绑定.    

回调函数丢失this绑定也是非常常见的:    

``` javascript
	function foo() {
		console.log(this.a);
	}

	var obj = {
		a:2,
		foo: foo
	}
	var a = "global";
	serTimeout(obj.foo, 1000); //global
```
无论是哪种情况,this的改变都是意想不到的,实际上无法控制回调函数的执行方式,因此就没有办法控制调用位置以得到期望的绑定.要想解决这个问题,可以通过**固定this**来实现.    

#### 5.3 显式绑定    
在分析隐式绑定时,我们必须在一个对象内部包含一个指向函数的属性,并通过这个属性间接引用函数,从而把this隐式的绑定到这个对象上.    
而显式绑定可以使我们在不想在对象内部包含函数引用时,可以在某个对象上强制调用函数.    

``` javascript
function foo() {
	console.log(this.a);
}

var obj = {
	a: 2
};

foo.call(obj);  //2
```
只是,简单的显式绑定依然无法解决丢失绑定的问题.要想解决问题,需要使用显式绑定的一个变种:**硬绑定**:    

``` javascript
function foo() {
	console.log(this.a);
}

var obj = {
	a: 2
};

var bar = function() {
	foo.call(obj);
};   // 硬绑定

bar();   //2
setTimeout(bar, 1000);  //2

//硬绑定的bar不可能再修改它的this
bar.call(window);   //2
```

**由于硬绑定是一种非常常见的模式,所以ES5提供了内置的方法Function.prototype.bind:**    

``` javascript
function foo(num) {
	console.log(this.a, num);
	return this.a + num;
}

var obj = {
	a:2
};

var bar = foo.bind(obj);
var b = bar(3);   //2 3
console.log(b);   //5
```

**另外,第三方库的许多函数,以及JavaScript语言和宿主环境中许多新的内置函数,都提供了一个可选的参数,通常被称为"上下文"(context),其作用和bind()一样,确保我们的回调函数使用正确的this.如:**    

``` javascript
function foo(el) {
	console.log(el, this.greet);
}

var obj = {
	greet: "hello"
};

//调用foo()时把this绑定到obj
[1,2,3].forEach(foo, obj); //1 hello 2 hello 3 hello
```
**实际上,这些函数就是通过call()或apply()实现了显示绑定,但可以简化我们的代码编写.**    

#### 5.4 new绑定       
首先需要知道,JavaScript中的new的机制实际上和面向类的语言完全不同.在JavaScript中,构造函数只是一些使用new操作符时被调用的函数.它们不会属于某个类,也不会实例化一个类.实际上,他们只是被new操作符调用的普通函数而已. 因此,**实际上并不存在所谓的"构造函数",只有对函数的"构造调用".**    
使用new来构造调用时,会自动执行下面的操作:    
1. 创建(或者说构造一个全新的对象).    
2. 这个新对象会被执行[[Prototype]]连接.    
3. 这个新对象会被绑定到函数调用的this.    
4. 如果函数没有返回其他对象,那么new表达式中的函数调用会自动返回这个新对象.    

因此,使用new进行绑定:    

``` javascript
function foo(a) {
	this.a = a;
}
var bar = new foo(2);
console.log(bar.a);  //2

var baz = new foo(3);
console.log(baz.a); //3
```
使用new来调用foo()时,我们构造了一个新对象bar并把它绑定到foo()调用中的this上.    

## 6. 优先级    
现在我们知道了调用位置和四种绑定规则,那么如果在某个调用位置可以应用多条绑定规则应该怎么判断呢?这就需要知道四种规则的优先级了:    
**new > 显示绑定 > 隐式绑定 > 默认绑定.** 参见下面例子:    

``` javascript
	function foo() {
		console.log(this.a);
	}

	var obj1 = {
		a: 2,
		foo: foo
	};

	var obj2 = {
		a: 4,
		foo: foo
	};

	obj1.foo();    //2 隐式绑定
	obj2.foo();    //4 隐式绑定

	obj1.foo.call(obj2);   //4 显式绑定 > 隐式绑定
	obj2.foo.call(obj1);   //2 显式绑定 > 隐式绑定
```

``` javascript
	function foo(num) {
		this.a = num;
	}

	var obj = { };

	var bar = foo.bind(obj);    //foo中的this被显式绑定(硬绑定)绑到了obj上
	bar(2);                     //给obj.a赋值2
	console.log(obj.a);         //2 

	var another = new bar(3);   //本来绑在obj上的this被new绑到了another上,并给another.a赋值为3
	console.log(obj.a);         //2 后面没有对它进行操作了
	console.log(another.a);     //3 
	//可见, new >  显式绑定
```

因此,总结一下,我们现在可以根据优先级来判断在某个调用位置应用的是哪条规则.可以根据下面的顺序来判断:    
1. 函数是否在new中调用(new绑定)?如果是的话,this绑定的就是新的对象: var bar  = new foo().    
2. 函数是否通过call, apply(显式绑定)或者硬绑定调用?如果是的话,this绑定的是指定的对象:var bar = foo.call(obj2)    
3. 函数是否在某个上下文中调用(隐式绑定)?如果是的话,this绑定的是那个上下文对象:var bar = obj1.foo()    
4. 如果都不是的话,使用默认绑定.如果在严格模式下,就绑定到undefined(会报错),否则绑定到全局对象: var bar = foo().    

但是,凡事总有例外,这里的绑定规则,也有例外的情况.    

## 7. 绑定例外    

#### 7.1 被忽略的this    
当我们把null或undefined作为this的绑定对象传入call,apply或者bind时,这两个值在调用时会被忽略,因此这种情况下应用的实际上是默认绑定规则:    

``` javascript
functon foo() {
	console.log(this.a);
}

var a = 2;
foo.call(null); //2
```

什么情况下会传入null呢?    
一种非常常见的做法是使用apply()来"展开"一个数组,并当作参数传入一个函数.类似地,bind()可以对参数进行柯里化(指把接收多个参数的函数变为一次接收一个参数.):    

``` javascript
function foo(a, b) {
	console.log(a + " , " + b);
}
//把数组"展开"成参数
foo.apply(null, [2, 3]);

//进行柯里化
var bar = foo.bind(null, 2);
bar(3);   //2 , 3
```  
即如果函数并不关心this的话,但仍需传入一个占位值,这时null是一个不错的选择.     
但是,这种方式可能会产生一些副作用,可能会把某些确实使用了this的函数中的this绑定到了全局对象.    

有一种相对来说更安全的方法:**传入一个空对象,因为任何对于this的使用都会被限制在这个空对象中,不会对全局对象产生影响.**    

``` javascript
var emptyObject = {};
//或
var emptyObject = Object.create(null);    //不会创建Object.prototype这个委托,所以更空一点

foo.apply(emptyObject, [2, 3]);
```

#### 7.2 间接引用    

``` javascript
function foo() {
	console.log(this.a);
}

var a = 2;
var o = {a: 3, foo: foo};
var p = {a: 4};

o.foo();
(p.foo = o.foo)(); //2
```
赋值表达式p.foo = o.foo的返回值是目标函数的引用,因此调用位置是foo()而不是p.foo()或者o.foo().因此这里执行的实际上是默认绑定而不是隐式绑定.    

#### 7.4 软绑定      
需要写一个softBind函数,暂时用不上,理解原理就好:**如果可以给默认绑定指定一个全局对象和undefined以外的值,那就可以实现和硬绑定相同的效果,同时保留隐式绑定或者显示绑定修改this的能力.**    


BTW, 我们之前介绍的四条规则已经可以包含所有正常的函数,可是ES6中的箭头函数并不遵守这些规则.以上的内容是ES5中对this比较详细的学习了,至于以后的内容,就要留到以后再来仔细研究了.    

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
 - [JavaScript红皮书学习(5)--BOM](http://liveipool.com/blog/2016/12/25/JavaScript-RedBook-8-BOM)             


