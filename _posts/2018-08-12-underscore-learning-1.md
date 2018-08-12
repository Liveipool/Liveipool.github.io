---
layout: post
title:  Underscore.js源码学习（一）
date: 2018-08-12
categories: blog
description: 前端学习
---

# Underscore.js源码学习（一）
最近刚接手了一两个项目，代码量也算不小，直到今天一个下雨的周六，才能安安静静空出来学习。今天看了两部分令我印象深刻的内容：一是Steve Jobs在2007年iPhone发布会时的精彩表演，即使到科技如此发达的今天，也还是和当时的现场观众一样，赞叹他带给我们的惊喜。接着开始看Underscore.js的源码，配合着一些注释和文章，频频惊叹于其作者Jeremy Ashkenas在十年前就能写出如此优雅健壮的代码，确实对自己写的代码进行了一些反思。    

### 它是什么？
要理解 Underscore.js 的源码和优雅，首先要清楚它是一个什么东西：它是一个 JavaScript 工具库，提供了一整套函数式编程的实用功能，但它没有扩展任何 JavaScript 内置对象。 它弥补了 jQuery 没有实现的功能。    
它一共提供了100多个函数，但它是一个非常轻量级的库，代码总共只有2000行不到，压缩后只有5.7kb。    

### 它干了什么事？

``` javascript
// 它使代码变得更加易读
_.isEmpty({});
// true

// 它使代码更容易编写
_.flatten([0, 1], [2, 3], [4, 5]);
// [0, 1, 2, 3, 4, 5]

// 它提供了原生所没有的方法：
_.range(5);
// [0, 1, 2, 3, 4]

// 它自身甚至可以作为模板引擎
_.template('<p><%= text %></p>', {text: 'Hello WeBank!'});
// <p>Hello WeBank!</p>
```

### 重要特点及方法

#### 作用域包裹
与其他第三库一样，它通过立即执行函数来包裹自己的业务逻辑。一般而言，这些库的立即执行函数主要有以下目的：   
1. 避免全局污染：所有库的逻辑，库所定义和使用的变量全部被封装到了该函数的作用域中。    
2. 隐私保护：但凡在立即执行函数中声明的函数、变量等，除非是自己想暴露，否则绝不可能在外部获得。    

``` javascript
(function(){
  // ...
})()
```
所以，当我们写自己的库的时候，也可以考虑在最外层包裹上一个立即执行函数。既不受外部影响，也不给外部舔麻烦。   

#### _ 对象
underscore 有下划线的意思，所以 underscore 通过一个下划线变量 _ 来标识自身。        
**_ 是一个函数对象**，所有的 api 都会被挂载到这个到对象上：   

``` javascript
// 源码定义
var _ = function (obj) {
  // 如果 obj 已经是 `_` 函数的实例，则直接返回 obj
  if (obj instanceof _) return obj;
  // 如果不是 `_` 函数的实例,则调用 new 运算符，返回实例化的对象
  if (!(this instanceof _)) return new _(obj);
  // 将 obj 赋值给 this._wrapped 属性
  this._wrapped = obj;
};
```

#### 保留用户的 _
默认情况下，underscore 对象 _ 会覆盖全局对象上同名的 _ 属性。但是，underscore 会保存之前已经存在的 _ 属性：   

``` javascript
// 保存之前已经存在的 _ 属性
var previousUnderscore = root._;
```

当用户已经在全局对象上绑定了 _ 对象时，可以通过 underscore 提供的 noConflict 函数来重命名 underscore 对象，将用户在全局定义的 _ 还给用户：   

``` javascript
var underscore = _.noConflict();
```

看到 noConflict 的源码实现，我们发现，在其内部，将会恢复原来全局对象上的 _：   

``` javascript
// 返回一个 underscore 对象，把_所有权交还给原来的拥有者
_.noConflict = function () {
    // 回复原来的_指代的对象
    root._ = previousUnderscore;
    // 返回 underscore 对象
    return this;
};
```

看到这里就不由得感叹，虽然看起来都是一些很简单的代码，但这体现出作者的思维是多么的清晰和严密，并且能够让代码更简单本身就是一种功力。   

#### 执行环境判断
Underscore.js既能够服务于浏览器，又能够服务于诸如 node.js 所搭建的服务端，它的对象 _ 将会依托于当前的所处环境，挂载到不同的全局空间当中（浏览器的全局对象是 window，node 的全局对象是 global）。       
下面代码展示了 underscore 是如何判断自己所处环境的，这个判断逻辑也为我们自己想要撰写前后端通用的库的时候提供了帮助：      

``` javascript
// self, global和this
var root = typeof self == 'object' && self.self === self && self || typeof global == 'object' && global.global === global && global || this;
```

##### window or self 
在上面的代码中，我们没有看到 window 对象的引用，其实，在浏览器环境下，self 保存的就是当前 window 对象的引用。那么相比较于使用 window，使用 self 有什么优势呢？    
MDN上的解释：The Window.self read-only property returns the window itself, as a WindowProxy. It can be used with dot notation on a window object (that is, window.self) or standalone (self). The advantage of the standalone notation is that a similar notation exists for non-window contexts, such as in Web Workers.    
概括来说，就是 self 还能用于一些不具有window的上下文环境中，比如 Web Workers。所以，为了服务于更多场景，underscore 选择了更加通用的 self 对象。    

#### 局部变量的妙用
Underscore.js本身也依赖了不少 js 的原生方法，但它会通过局部变量来保存一些他经常用到的方法或者属性，这样做的好处有如下几点：   
 - 在后续使用到这些方法或者属性时，避免了冗长的代码书写。   
 - 在进行代码压缩时，局部变量push也比Array.prototype.push长度更短，能够缩小文件大小。    
 - 减少了对象成员的访问深度，如Array.prototype.push --> push, 这样做能带来一定的性能提升。    

这也提醒了我，在我们写代码时，从作用域的角度考虑，查询局部变量肯定比查询全局变量性能开销要小很多，以后可以多学学这种写法，比如在函数作用域里，用得多的全局变量用局部变量来存或代替。   

``` javascript
var ArrayProto = Array.prototype;
var ObjProto = Object.prototype;
var SymbolProto = typeof Symbol !== 'undefined' ? Symbol.prototype : null;

var push = ArrayProto.push;
var toString = ObjProto.toString;
```

#### _.each：写看得懂的循环（迭代）

``` javascript
// 我们写代码时，可能就是这么写的
var letters = ['a', 'b', 'c'];
for(var i = 0; i < letters.length; i++) {
  console.log('letter: ' + letters[i]);
}

// 它封装的_.each方法，虽然在这种情况下并不会减少代码量，但从一定角度看，它用迭代的方式实现了循环，在这个角度看，代码逻辑更加清晰
var letters = ['a', 'b', 'c'];
_.each(letters, function(letter, index, letters) {
  // 回调函数的三个参数，当前letter、当前索引index、迭代的对象letters，
  console.log('letter: ' + letter);
});
```

另外，_.each方法除了能支持Array，**还能支持Object**，这个挺实用的（Array.forEach不能支持Object）：    

```  javascript
var obj = {
  name: 'wxj',
  age: 13,
  sex: 'male'
};

_.each(obj, function(value, key, obj){
  console.log('['+key+'] is:', value);
});

// name's value is: wxj
// ...
// ...
```

_.each实现的源码：   

``` javascript
_.each = _.forEach = function(obj, iteratee, context) {
  // 迭代器iteratee和回调函数cb、optimizeCb都是Underscore.js源码中很重要的概念和方法，但我还没有理解得很清晰，后面再多看几遍
  iteratee = optimizeCb(iteratee, context);
  var i, length;
  // 通过它自己写的isArrayLike方法判断是否是Array
  if (isArrayLike(obj)) {
    for (i = 0, length = obj.length; i < length; i++) {
      iteratee(obj[i], i, obj);
    }
  } else {
    // 否则就是对象
    var keys = _.keys(obj);
    // 以后我们要是想写Object的遍历又没有用第三方库，可以这样自己来实现
    for (i = 0, length = keys.length; i < length; i++) {
      iteratee(obj[keys[i]], keys[i], obj);
    }
  }
  return obj;
};
```

#### void 0
在Underscore中我看到经常出现了void 0这样的代码，其实以前也看到过，但没有很深入去了解过，在这里也记录一下：   

void 运算符能对给定的表达式进行求值，**然后返回 undefined。**也就是说，void 后面你随便跟上一个表达式，返回的都是 undefined，都能完美代替 undefined！那么，这其中最短的毫无疑问就是 void 0 了。其实用 void 1，void (1+1)，void (0) 等等，都是一样的效果。   

但是，为什么要代替undefined呢，直接用undefined不好吗？原因有几个：   
 - undefined是可以被更改的，特别是在老一点的浏览器中，是可以给undefined赋值的，虽然在 ES5 中全局对象undefined已经是一个只读属性了，但是在局部作用域中，undefined还是可以被重写的。**而void 是不能被重写的。**   
 - 用 void 0 代替 undefined 能节省不少字节的大小，事实上，不少 JavaScript 压缩工具在压缩过程中，正是将 undefined 用 void 0 代替掉了。   

一般用法：   

``` html
<a href="javascript:void(0);">
  这个链接点击之后不会做任何事情，如果去掉 void()，
  点击之后整个页面会被替换成一个字符 0。
</a>

<p> 在chrome中即使<a href="javascript:0;">也没变化，但在firefox中会变成一个字符串0 </p>

<a href="javascript:void(document.body.style.backgroundColor='green');">
  点击这个链接会让页面背景变成绿色。
</a>
```

### 大致讲一下lambda-view的使用体验  
在之前的总结文章中记录了一个辅助源码阅读的工具：[lambda-view](https://github.com/Jianru-Lin/lambda-view)，在这次试着用了一下，确实使用是很方便的，npm全局装一下，通过一个lv命令就可以用了。它的主要思想是把代码从最高的层级然后一层层展开慢慢引导我们看代码，如最开始是这样的：   

``` javascript
(function () {...})()
```
这样可以看到整个Underscore.js代码就是一个立即执行函数。点击上面大括号中的省略号，可以展开下一层，如：   

``` javascript
(function () {
  var _ = function (obj) {...};
})()
```

以此类推，一层层展开，让我们能从宏观到具体地看代码，在以后一些复杂的项目中可能确实会有用处，但Underscore.js的源码本来就只有一个文件了，结果也比较清晰，在编辑器上看还能快捷键跳到对应的函数所在的地方，所以Underscore.js的源码在编辑器上看就足够了。   

### 一些想法
Underscore.js诞生于2009年，在2011年6月出来的ECMAScript5.1标准中，增加了很多方法，如数组的forEach、filter函数，有可能就是参考了Underscore.js，社区觉得好用的方法，就把它们加进了标准中。  

在 [You Might Not Need Underscore](https://www.reindex.io/blog/you-might-not-need-underscore/) 这篇文章中也讲到，随着ES5.1、ES2015、ES2016...的出现，标准的ECMAScript有了更多更强大的功能，如：   

``` javascript
// Underscore.js VS ES标准
_.each(array, iteratee)
array.forEach(iteratee)

_.map(array, iteratee)
array.map(iteratee)

_.includes(array, element)
array.includes(element)

...
```

确实，我的观点和作者一样，在现在，使用标准的ES的语法已经能实现很多我们以前只能通过hack或第三方库才能实现的功能，Underscore.js和Lodash这种库可能后面用到的地方越来越少。   
但是，这并不表示他们不值得学习了，历史的车轮总是滚滚向前，就像我们现在去看2007年的iPhone一样，和现在的iPhoneX比起来，功能和样子都那么Old school，但一定是在老iPhone的积累和沉淀下才慢慢发展到了现在。    
不可否认的是，现在ECMAScript标准中的很多方法等都是受到了Underscore.js的启发，所以阅读Underscore.js的源码可以了解到最初的这些优秀方法是怎么样的，又如何发展到了现在，现在的标准方法和以前的比又有什么样的优化呢，如果真的懂了这些，也算是很有收获了。   
所以，就算是只有1500-2000行源码的Underscore.js，也不是一两天就能快速读完的，后面有时间再继续深入学习，下一篇文章见！   

#### 学习资料
- [英文文档](https://underscorejs.org/)
- [中文文档](http://www.bootcss.com/p/underscore/)
- [源码地址](https://github.com/jashkenas/underscore)
- [undersercore 源码分析](https://legacy.gitbook.com/book/yoyoyohamapi/undersercore-analysis/details)：讲得比较详细，上文中的有一些段落就是出自这篇文章里面，确实很清晰，先看了源码再来看这个，本来没理解的地方就都理解了。
- [带有注释的源码（如果觉得英文实在看不下去可以google翻译）](http://www.bootcss.com/p/underscore/docs/underscore.html)
- [1.8.3版本的有一些中文注释的源码](https://github.com/hanzichi/underscore-analysis/blob/master/underscore-1.8.3.js/underscore-1.8.3-analysis.js)

#### 参考文档
 - [Underscore.js 入门教程](http://web.jobbole.com/87537/)
 - [为什么用「void 0」代替「undefined」](https://github.com/hanzichi/underscore-analysis/issues/1)
 - [【 js 基础 】 深浅拷贝](https://zhuanlan.zhihu.com/p/26282765)
 - [【 js 性能优化】underscore throttle 与 debounce 节流](https://zhuanlan.zhihu.com/p/26054718)  
 - [一些相关学习文章](https://github.com/hanzichi/underscore-analysis)
 - [You Might Not Need Underscore](https://www.reindex.io/blog/you-might-not-need-underscore/)
 - [You don't (may not) need Lodash/Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore)
