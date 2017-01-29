---
layout: post
title: ES6 标准入门(2)--let 和 const命令
date: 2017-01-27
categories: blog
description: JavaScript学习
---

# let 和 const 命令    

### let命令    
**基本用法：**    
ES6新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。    

``` javascript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

**不存在变量提升：**    
let不像var那样会发生**变量提升**现象。所以，变量一定要在声明后使用，否则报错。    

``` javascript
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

**暂时性死区：**    
ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。    

总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。    

```
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```
暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。    

**不允许重复声明：**    
let不允许在相同作用域内重复声明同一个变量。    

另外，下面的代码也会报错，与var的行为不同。     

```
// 不报错
var x = x;

// 报错,也是因为暂时性死区,在变量x的声明语句还没有执行完成前，就去取x的值.
let x = x;
// ReferenceError: x is not defined
```

### 块级作用域    
在ES5中：    

```
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = "hello world";
  }
}

f(); // undefined
```
上面代码中，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。    

**let为JavaScript增加了块级作用域。**块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。    

ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。    

``` 
function f() { console.log('I am outside!'); }
(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());
```
上面代码在 ES5 中运行，会得到“I am inside!”，因为在if内声明的函数f会被提升到函数头部，实际运行的代码如下。    

```
// ES5版本
function f() { console.log('I am outside!'); }
(function () {
  function f() { console.log('I am inside!'); }
  if (false) {
  }
  f();
}());
```

ES6 的运行结果就完全不一样了，会得到“I am outside!”。因为块级作用域内声明的函数类似于let，对作用域之外没有影响，实际运行的代码如下。    

```
// ES6版本
function f() { console.log('I am outside!'); }
(function () {
  f();
}());
```

试了下，上面的代码在最新版本的Chrome、Opera和Firefox中都会报错，因为实际运行的是下面的代码（浏览器的实现可以不遵守上面讲的约定）：    

```
// ES6的浏览器环境
function f() { console.log('I am outside!'); }
(function () {
  var f = undefined;
  if (false) {
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```

考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。    

### const 命令    

```
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```
const一旦声明变量，就必须立即初始化，不能留到以后赋值。    

```
const foo;
// SyntaxError: Missing initializer in const declaration
```
const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。    
const声明的常量，也与let一样不可重复声明。    

**对于复合类型的变量，变量名不指向数据，而是指向数据所在的地址。const命令只是保证变量名指向的地址不变，并不保证该地址的数据不变，所以将一个对象声明为常量必须非常小心。**    
因此，对象本身是可变的：    

```
const a = [];
a.push('Hello'); // 可执行
a.length = 0;    // 可执行
a = ['Dave'];    // 报错
```
如果真的想将对象冻结，应该使用Object.freeze方法。    

```
const foo = Object.freeze({});
// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```

**ES5只有两种声明变量的方法：var命令和function命令。ES6除了添加let和const命令，后面章节还会提到，另外两种声明变量的方法：import命令和class命令。所以，ES6一共有6种声明变量的方法。**    

### 全局对象的属性    
全局对象，在浏览器环境指的是window对象，在Node指的是global对象。ES5之中，顶层对象的属性与全局变量是等价的。    

```
window.a = 1;
a // 1

a = 2;
window.a // 2
```
上面代码中，全局对象的属性赋值与全局变量的赋值，是同一件事。    

全局对象的属性与全局变量挂钩，被认为是JavaScript语言最大的设计败笔之一。这样的设计带来了几个很大的问题，首先是没法在编译时就报出变量未声明的错误，只有运行时才能知道（因为全局变量可能是全局对象的属性创造的，而属性的创造是动态的）；其次，程序员很容易不知不觉地就创建了全局变量（比如打字出错）；最后，全局对象的属性是到处可以读写的，这非常不利于模块化编程。另一方面，window对象有实体含义，指的是浏览器的窗口对象，全局对象是一个有实体含义的对象，也是不合适的。    

ES6为了改变这一点，一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是全局对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于全局对象的属性。也就是说，从ES6开始，全局变量将逐步与顶层对象的属性脱钩。      

```
var a = 1;
// 如果在Node的REPL环境，可以写成global.a
// 或者采用通用方法，写成this.a
window.a // 1

let b = 1;
window.b // undefined
```