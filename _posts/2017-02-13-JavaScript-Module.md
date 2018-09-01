---
layout: post
title: JavaScript 模块化   
date: 2017-02-13
categories: blog
description: 前端学习
---

# JavaScript 模块化          
模块化的需求是规模的产物，当web page进化到web application，浏览器端处理的逻辑越来越复杂，展现的样式和动画越来多，对于工程的要求也就越来越高。于是模块化的需求也就产生了。模块化的意义：          
1. **模块的复用，降低开发成本和维护成本**          
2. **模块单独开发，方便分工合作**          
3. **模块化遵循标准，方便自动化依赖管理，代码优化，部署**          

JavaScript长久以来被认为是简单的脚本语言，实际上情况早就发生来变化，在最新版的ECMA-262（ES6）文档中强调**JavaScript是通用编程语言而不是脚本语言。**脚本语言，比如shell并不是用来完成复杂功能的，只是用来做一些自动化控制，是不需要模块化的。而用于构建复杂系统通用编程语言（比如Java）一般都有模块的实现。          

#### ES6以前的模块化          
讲解为什么需要模块化以及ES6以前的模块化实现：[阮一峰的博客：Javascript模块化编程（一）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)          

在ES6的Module出来以前，主要有三种模块书写规范：CommonJS、AMD、CMD，分别用于在服务端和浏览器端组织不同的模块、管理模块之间的依赖性。          
[浅析JS模块规范：AMD，CMD，CommonJS](http://www.jianshu.com/p/09ffac7a3b2c)          

### CommonJS          
node.js的模块系统，就是参照CommonJS规范实现的。在CommonJS中，有一个全局性方法require()，用于加载模块。CommonJS适用于服务端。          
[CommonJS规范](http://javascript.ruanyifeng.com/nodejs/module.html#toc4)          

### AMD          
[阮一峰的博客：Javascript模块化编程（二）：AMD规范](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)          
下面介绍一个常用的Javascript库，它实现了AMD规范：require.js。          

#### require.js          
[阮一峰的博客：Javascript模块化编程（三）：require.js的用法](http://www.ruanyifeng.com/blog/2012/11/require_js.html)          

### CMD          
CMD (Common Module Definition), 是seajs推崇的规范，CMD是**依赖就近**，**用的时候再require。**它写起来是这样的：          

```
define(function(require, exports, module) {
   var clock = require('clock');
   clock.start();
});
```

### CommonJS、AMD、CMD的区别          
 - CommonJS规范加载模块是**同步**的，也就是说，只有加载完成，才能执行后面的操作。用于**服务器端**。          
 - AMD、CMD规范则是**异步**加载模块，允许指定回调函数。用于**浏览器端**。          
 - 由于Node.js主要用于服务器编程，**模块文件一般都已经存在于本地硬盘，可以同步加载完成，等待时间就是硬盘的读取时间，所以CommonJS规范比较适用。**但是，对于浏览器，这却是一个大问题，因为模块都放在服务器端，等待时间取决于网速的快慢，可能要等很长时间。因此浏览器端一般采用AMD规范.          
 - AMD和CMD最大的区别是对依赖模块的执行时机处理不同。AMD**依赖前置**，js可以方便知道依赖模块是谁，立即加载；而CMD**就近依赖**，需要使用把模块变为字符串解析一遍才知道依赖了那些模块，这也是很多人诟病CMD的一点，牺牲性能来带来开发的便利性，实际上解析模块用的时间短到可以忽略。          
 - require.js采用AMD规范，sea.js采用CMD规范。          

```
// CMD
define(function(require, exports, module) {
var a = require('./a')
a.doSomething()
// 此处略去 100 行
var b = require('./b') // 依赖可以就近书写
b.doSomething()
// ... 
})

// AMD 默认推荐的是
define(['./a', './b'], function(a, b) { // 依赖必须一开始就写好
a.doSomething()
// 此处略去 100 行
b.doSomething()
...
}) 
```

### 动态加载和静态加载          
在看ES6之前我们先看模块加载的两种方式：          
1. **静态加载:在编译阶段进行,把所有需要的依赖打包到一个文件中**          
2. **动态加载:在运行时加载依赖**          

那么ES6采用何种加载机制:          
ES6 模块的设计思想，是**尽量的静态化**，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。**CommonJS 和 AMD 模块，都只能在运行时确定这些东西。**比如，CommonJS 模块就是对象，输入时必须查找对象属性。          
并且，为了使功能更加强大，ES6 Module现在有一提案，即加入import()函数，这个函数可以进行动态加载，并返回一个Promise对象。有了它就可以更好的代替CommonJS了。   

### ES6 Module          
[ES6标准入门：Module](http://es6.ruanyifeng.com/#docs/module)          

### require和import          
ES6标准发布后，Module成为标准，标准的使用是一export指令导出接口，以import引入模块。但在我们一贯的node模块中，我们采用的是CommonJS规范，使用require来引入模块 ，使用module.exports导出接口。                    

#### require时代的模块          
node编程中最重要的思想之一就是模块，而正是这个思想，让JavaScript的大规模工程成为可能。模块化编程在js界流行，也是基于此，随后在浏览器端，requirejs和seajs之类的工具包也出现了，可以说在对应规范下，require统治了ES6之前的所有模块化编程，即使现在，在ES6 module被完全实现之前，还是这样。          

#### ES6中的module          
ES6发布的module并没有直接采用CommonJS，甚至连require都没有采用，也就是说**require仍然只是node的一个私有的全局方法，**module.exports也只是node私有的一个全局变量属性，跟标准半毛钱关系都没有。          

#### 该用require还是import？          
require的使用非常简单，它相当于module.exports的传送门，module.exports后面的内容是什么，require的结果就是什么，对象、数字、字符串、函数……再把require的结果赋值给某个变量，相当于把require和module.exports进行平行空间的位置重叠。          

而且require理论上可以运用在代码的任何地方，甚至不需要赋值给某个变量之后再使用，比如：          

```
require('./a')(); // a模块是一个函数，立即执行a模块函数
var data = require('./a').data; // a模块导出的是一个对象
var a = require('./a')[0]; // a模块导出的是一个数组
```
**但是import则不同，它是编译时的（require是运行时的），它必须放在文件最顶层，而且使用格式也是确定的，不容置疑。**它不会将整个模块运行后赋值给某个变量，而是只选择import的接口进行编译，这样在**性能上比require好很多**。          
从理解上，**require是赋值过程，import是解构过程，**当然，require也可以将结果解构赋值给一组变量，但是import在遇到default时，和require则完全不同：           

```
var $ = require('jquery'); 和 import $ from 'jquery' 是完全不同的两种概念。
```
虽然就现在来说，还是可以在一些项目中看到require的身影，但是在不久的将来，市场一定会被import给霸占。所以，尽早熟练import的写法，将标准的ES6 Module为项目的第一选择。          

> **ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为服务器和浏览器通用的模块解决方案。**          


> 确实，各种模块和书写规范层出不穷，希望ES6能尽快终止这场混战。          

![赞赏码.jpeg](https://upload-images.jianshu.io/upload_images/3001083-f65814d1f594b39c.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
