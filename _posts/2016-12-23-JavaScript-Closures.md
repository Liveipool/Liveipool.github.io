---
layout: post
title: JavaScript学习专题之--闭包  
date: 2016-12-23
categories: blog
description: JavaScript学习
---

# JavaScript闭包       
看了《JavaScript高级程序设计》和《你不知道的JavaScript》，也看了很多网上的博客，还把MDN上的中英文都各自看了一遍。不得不说，这确实是系统性学习JS以来觉得最难以理解的一个概念，但确实可能也是太想去找到一个看见能觉得豁然开朗的定义了，结果花了很久还在纠结它的定义是啥。有说它是特殊的对象的，有说是一种引用的，但我觉得，就按照官方定义来吧，它就是一个函数，这个函数有它独特的功能。以后无论看到什么样的例子，始终将官方定义记牢，慢慢理解，总是可以吃透的。          

## 闭包的定义          
 - MDN官方定义为: Closures are functions that refer to independent (free) variables (variables that are used locally, but defined in an enclosing scope). In other words, these functions 'remember' the environment in which they were created.      
 - 翻译: 闭包是指那些能够访问独立(自由)变量的函数 (变量在本地使用，但定义在一个封闭的作用域中)。换句话说，这些函数可以“记忆”它被创建时候的环境。      
 - 也就是说:在 JavaScript中，因为内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回（寿命终结）了之后。即当一个函数即便在离开了它的词法作用域(Lexical Scope)的情况下，仍然可以存取它的词法作用域(Lexical Scope)，这个函数就构成了闭包。      
 - 根据定义,也就不难推测,创建闭包的方法为:在一个函数内部创建另一个函数.      

## 三个例子(来自MDN)        

#### 例子一:      

``` javascript
function init() {
  var name = "Mozilla"; // name is a local variable created by init
  function displayName() { // displayName() is the inner function, a closure
    alert(name); // use variable declared in the parent function    
  }
  displayName();    
}
init();       //“Mozilla”
```
**displayName() is the inner function, a closure!**      
函数 init() 创建了一个局部变量 name，然后定义了名为 displayName() 的函数。 displayName() 是一个内部函数——定义于 init() 之内且仅在该函数体内可用。displayName() 没有任何自己的局部变量，然而它可以访问到外部函数的变量，即可以使用父函数中声明的 name 变量。      

#### 例子二:            

``` javascript
function makeFunc() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  return displayName;       //注意没有(),即应该返回的是dispalyName函数的引用而不是它的执行结果
}
var myFunc = makeFunc();
myFunc();                   //“Mozilla”
```

 myFunc 成为了一个闭包。 MDN这里也说了一段话来帮助理解:       **闭包是一种特殊的对象,它由两部分构成：函数，以及创建该函数的环境。环境由闭包创建时在作用域中的任何局部变量组成。**在这个例子中，**myFunc 是一个闭包，由 displayName() 函数和闭包创建时存在的 "Mozilla" 字符串形成。**      

#### 例子三:              

``` javascript
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = makeAdder(5);      //add5: function(y) {return 5 + y}
var add10 = makeAdder(10);    //add10: function(y) {return 10 + y}

console.log(add5(2));  // 7
console.log(add10(2)); // 12
```
从本质上讲，makeAdder 是一个函数工厂 — 创建将指定的值和它的参数求和的函数，在上面的示例中，我们使用函数工厂创建了两个新函数 — 一个将其参数和 5 求和，另一个和 10 求和。      
**add5 和 add10 都是闭包。它们共享相同的函数定义，但是保存了不同的环境。在 add5 的环境中，x 为 5。而在 add10 中，x 则为 10。**      

**在例子二三中,MDN把闭包描述成一个特殊的对象,看起来也有点道理.但还是牢记,闭包是一个函数,毕竟myFunc()和add5()都是一个函数,但是它们在理解上可以看作为一个对象,因为比如myFunc就包含了如displayName()函数和makeFunc()函数的作用域.**      
**在例子一中,虽然displayName()函数写在init()函数内部,但它也是一个闭包,我们可能觉得例子一中的闭包不如例子二三中的明显,但仔细一想,它是完全符合定义的首先.它是一个函数,接着如果把它当作一个特殊的对象来看,它包括了displayName()函数本身和init()函数作用域里的变量name,其实我们经过对比就可以发现,例子一中的displayName()函数和例子二中的myFunc()函数看为对象理解时包含的东西是一样的.**      
**实际上再定睛一看,既然myFunc = makeFunc(), 而makeFunc()返回的是displayName指针, 那么myFunc和displayName就都是指向同一个函数的指针. 虽然前文中我们说myFunc就是一个闭包,但实际上这只是一种方便的说法(我们平时经常将指针和它指向的函数混作一体来说),因为是闭包的其实不是这个指针本身,而是这个指针指向的函数.从myFunc和displayName两个指针都指向同一个函数更可以看出来,真正是闭包的,是它们指向的那个函数.**      



## 闭包的用途      
闭包可以用在许多地方。它的最大用处有两个，**一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。**      
怎么来理解这句话呢？请看下面的代码。      

``` javascript
 var nAdd;
 function func1() {
 	var n = 999;
 	nAdd = function() {
 		n += 1;
 	}

 	function func2() {
 		console.log(n);
 	}
 	return func2;
 }
 var func = func1();
 console.log(typeof func) //function
 func();                  //999
 nAdd();
 func();                  //1000
```
func是一个函数,也是一个闭包,若把它当作那个特殊的对象来看,它包含了func2和func1作用域里定义的变量n.      
从功能角度来看,func闭包读取到了func1内部的变量,也让本应在func1销毁时也销毁的变量n保存在了内存中.      
**另外,根据前三个MDN中的例子可以看出,func2其实也是一个闭包,所以说闭包可能会在一段代码中出现很多次,只需要记住,只要满足定义,这个函数就是一个闭包.**      

## 其他一些例子        

#### 例子四:无论使用何种方式对函数类型的值进行传递,都可以创建闭包.           

``` javascript
function foo() {
	var a = 2;
	function baz() {      //这个函数是个闭包
		console.log(a);
	}
	bar(baz);
}

function bar(fn) {
	fn();     //这个函数也是个闭包
}
foo();   //2
```
无论怎样变来变去,判断一个函数是不是闭包的条件就是看它满不满足定义,不用过多纠结这个函数和其它代码的关系等.      

#### 例子五:      

``` javascript
function wait(message) {
	setTimeout(function timer() {    //timer()函数是一个闭包
		console.log(message)
	}, 3000);
}

wait("Hello Closure!");    //三秒后,输出"Hello CLosure!"
```
**这里可以看到,在wait()函数执行三秒后,它的内部作用域并不会消失,而这里的timer()函数就是一个闭包.**      
**本质上讲,无论何时何地,如果将(访问它们各自词法作用域的)函数当作第一级的值类型并到处传递,你就会看到闭包在这些函数中的应用.在定时器、事件监听器、Ajax请求、跨窗口通信或任何其他的异步(或者同步)请求中,只要使用了回调函数,实际上就是在使用闭包.**      

#### 例子六:循环和闭包      

``` javascript
for (var i = 1; i < 5; i++) {
	setTimeout(function timer() {
		console.log(i);
	}, 1000);
}
```
看到上面的代码后,我大概推测了一下可能的输出结果,不知道你推测的结果是怎样的,反正我推测的,都不对.但还好,经过后来的仔细分析,还是能够得以理解.      
**这段代码的输出为: 1秒后同时输出4个5.**      
分析: 首先,timer()函数是一个闭包(庆幸写blog写到这里已经能够轻松的看出一些闭包了), 因此,这个timer()函数第一次执行的时间即为第一次循环的一秒后,然而在一秒后,整个循环早已运行结束了,而那时timer()函数去读取这个循环里的i值,值自然是5.而这四个5同时输出是因为,每一次循环的1秒后就执行一次timer()函数,而这四次循环,由于数量太少,几乎同时在时间可以忽略的情况下就完成了,因此看起来4个5是同时输出的.      

``` javascript
for (var i = 1; i < 5; i++) {
	setTimeout(function timer() {
		console.log(i);
	}, i*1000);
}
```
再看看这一段稍有改动的代码,情况稍有改动,输出变为了: **1秒后输出了一个5,再隔1秒输出第二个5,保持频率直到4个5都输出出来.**      
想必也是非常好理解的,因为这四次回调函数设定的时间变为了1000,2000,3000,4000毫秒.并且没有变化的是,输出的结果,全是5.      
以上两种情况都只输出5就是因为闭包引起的,那么我们想让循环每一次输出对应的i要怎么做呢?      
**这里我们就要依靠IIFE了,我们需要为每一次循环创建一个单独的作用域,并把每次循环对应的i值放进对应的作用域里,这样以后每次运行的timer()函数就是去找它那次循环对应作用域中的i值而不是for循环的作用域里的i值5了.**      

``` javascript
for (var i = 1; i < 5; i++) {
	(function() {
		setTimeout(function timer() {
			console.log(i);	
		}, i*1000)
	})();
}
```
眨眼一看,我们使用了IIFE,是不是就可以了呢?答案还是不行了,上面这段代码的输出依然是每秒1个5这样输出,这是为什么呢?分析一下,噢,原来我们只是给了每次循环一个作用域,而并没有将那次循环对应的i值传进去.      

``` javascript
for (var i = 1; i < 5; i++) {
	(function(i) {
		setTimeout(function timer() {
			console.log(i);	
		}, i*1000)
	})(i);
}
```
这段代码就是最终的正确代码了,它的输出为: 1秒后输出1,两秒后输出2.....      
如果你能仔细分清这四段代码的细微的区别以及它们为啥会那样输出,那么这部分内容你就算基本掌握了.      

## 使用闭包的注意点      
 - 由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。      
 - 闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。      

## 关于this对象        
在闭包中使用this对象可能会导致一些问题.我们知道,**每个函数在被调用的时候都会自动取得两个特殊变量:this和arguments. 而this对象是在运行时基于函数的执行环境绑定的:在全局函数中,this等于window,而当函数被作为某个对象的方法调用时,this等于那个对象.**      

看下面的例子:      

``` javascript
	var name = "The window";

	var object = {
		name: "My object",

		getNameFunc: function() {
			return function() {
				return this.name;
			};
		}
	};

	var func = object.getNameFunc();
	console.log(func());             //The window
```
这里func是一个闭包,执行func()函数取到的是"The window".这里取到window的原因是func闭包使包含this.name的函数能够在外部被调用,而在外部被调用时,this指向的是window.       那么,如果我们想要取到"My object"应该怎么做呢?      
改为下列代码:      

``` javascript
	var name = "The window";

	var object = {
		name: "My object",

		getNameFunc: function() {
			var that = this;
			return function() {
				return that.name;
			};
		}
	};

	var func = object.getNameFunc();
	console.log(func());         //My object
```
这样,运行func()就可以成功输出"My object"了,这是为什么呢?根据前面讲的原理和代码之间的区别可以看出,我们这里通过一个that变量将当时是指向object中作用域的this指针存下来,在外部func()函数进行调用的时候,即使这时this指针已经指向了window,但that指针依然是指向的object,因此通过that.name可以成功访问到"My object".      
(arguments也有同样的问题,如果想访问作用域中的arguments对象,必须将对该对象的引用保存到另一个变量中,只不过arguments用得不多,我们这里知道原理即可.)           

还有几种特殊的情况,会让this的值意外的改变.虽然可能不会像其中一部分代码一样那么写代码,和闭包关系也不大,但这里说一下有助于理解.           

``` javascript
	var name = "The window";

	var object = {
		name: "My object",

		getName: function() {
			console.log(this.name);
		}
	}

	object.getName();                       //My object
	(object.getName)();                     //My object
	(object.getName = object.getName)();    //The window
```
第一个object.getName()执行的环境为object对象内,所以返回My object.第二排的(object.getName)()和object.getName()几乎没有区别,因此也是My object.但是第三个就有点不同了,它先执行了一个赋值语句,再调用赋值后的结果,这个赋值语句执行的作用域为全局环境下,因此被赋值后的object.getName可以看为全局环境下的一个函数,调用它,this自然指向window.          

## let + 闭包       
还记得例子六中的循环和闭包吗,我们使用IIFE来解决了闭包可能会导致的一些问题,但实际上,随着ES6的出现,此问题有了更加简单的解决方法:let.       

``` javascript
	for (var i = 1; i < 5; i++) {
		let j = i;         //使for循环中的四排代码成为了闭包的块作用域
		setTimeout(function() {
			console.log(j);
		}, j * 1000)
	}
```
上面的代码即可以按照每隔1秒,以此输出1到4.它的原理就是这里let隐式地劫持了for循环里的代码,这四排成为了一个块级作用域.         

``` javascript
	for (let i = 1; i < 5; i++) {
		setTimeout(function() {
			console.log(i);
		}, i * 1000)
	}
```
这样写和上面那段代码也可以有同样的输出,只是let不再写到for循环里去隐式地劫持了.而这样写在循环的头部,实际上**不仅把let绑定到了for循环中的代码的块作用域中,更是将其绑定到了循环的每一次迭代中**,换句话说,可以理解为**变量在循环的过程中不止被声明一次,每次迭代都会声明.随后的每个迭代都会使用上一个迭代结束时的值来初始化这个变量.**          
**随着ES6的出现,一些新的特性会带给Javascript一些新的功能,要是能熟练掌握了这些特性,想必利用JavaScript编程的功力一定能大大增加.**          
    

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