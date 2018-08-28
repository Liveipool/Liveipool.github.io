---
layout: post
title: 优秀前端源码的学习和总结
date: 2018-08-01
categories: blog
description: 前端学习
---

# 优秀前端源码的学习和总结
最近导师开始让我阅读源码写总结文章，我觉得这是一个我以前忽略掉的好习惯，或者说以前也在某些地方看到说要看源码，但也总是没有坚持下去。看源码能了解到前端生态圈里最牛的人都在干些什么事情，他们优秀的原因是什么，他们有些什么好的想法。   
当然，看源码不能盲目的一头栽进去，我觉得看之前，得清楚要看什么、怎么看，能学到什么。    
以下就是一些学习过程中的总结，将会随着学习的进度进行持续的更新和总结，欢迎大家指出问题，提出建议，有哪些不准确的地方也请多多包涵，一起进行完善。

## 一、读哪些？

#### [underscore.js](https://github.com/jashkenas/underscore)
一个JavaScript实用库，提供了一整套工具方法，github上有带注释的源码，一千多行。学习一下封装那些常用api的最佳实践。Underscore封装了 100 多个有用的方法，耦合度低，非常适合逐个方法阅读。从中，不仅可以学到用 void 0 代替 undefined 避免 undefined 被重写等一些小技巧 ，也可以学到变量类型判断、函数节流&函数去抖等常用的方法，还可以学到很多浏览器兼容的 hack，更可以学到作者的整体设计思路以及 API 设计的原理。
它是很好的函数式编程（Functional Programming）的体现。看完它后，紧接着看《javascript函数式编程》这本书。

相关链接：
 - **2018年8月11日补充，自己的源码阅读总结：[Underscore.js源码学习（一）](http://liveipool.com/blog/2018/08/12/underscore-learning-1/)，上面写了自己读源码的一些总结以及整理的一些优质参考资料。**    
 - **2018年8月28日补充，自己的源码阅读总计：[Underscore.js源码学习（二）](http://liveipool.com/blog/2018/08/28/underscore-learning-2/)**   

#### [Backbone.js](https://github.com/jashkenas/backbone)
两千多行的MVC框架。它里面大量设计模式的灵活运用，以及令人赞叹的处理技巧，还是非常值得学习。

相关链接：
- [backbone源码解读](https://github.com/aircloud/backboneAnalyze)
- [【 js 基础 】【 源码学习 】源码设计 （更新了backbone分析）](http://www.cnblogs.com/lijiayi/p/sourcecode.html)
- [【 js 基础 】【 源码学习 】backbone 源码阅读（一）](https://zhuanlan.zhihu.com/p/28334551)
- [【 js 基础 】【 源码学习 】backbone 源码阅读（二）控制反转](https://zhuanlan.zhihu.com/p/28343769)

#### [Spine.js](http://spine.github.io/)
Spine.js与backbone类似。可以在看之前先看看本《基于MVC的JavaScript Web富应用开发》。看完后写一个自己的MVC框架。

#### [Prototype.js](http://prototypejs.org/)
Prototype JavaScript Framework是由Sam Stephenson所开发的JavaScript框架。它提供了完整的Ajax框架及其它的工具。它的实现完全包含在一个单一的prototype.js文件中。

#### [lodash](https://lodash.com/)
A modern JavaScript utility library delivering modularity, performance & extras.

相关链接：
- [Why Use Lodash When ES6 Is Available](http://shzhangji.com/blog/2017/03/13/why-use-lodash-when-es6-is-available/)
- [10 Lodash Features You Can Replace with ES6](https://www.sitepoint.com/lodash-features-replace-es6/)

#### [jQuery](https://jquery.com/)
虽然写得好，但内容太多也不能全看，等看到jQuery再具体的分析总结.

相关链接：
- [jQuery- v1.10.2 源码解读](https://github.com/chokcoco/jQuery-)
- [jQuery v3.1.1 源码解读](https://github.com/songjinzhong/JQuerySource)
- [Aaron 的 jQuery源码分析系列](http://www.cnblogs.com/aaronjs/p/3279314.html)
- [一扬的jQuery-1.7.1 源码阅读笔记](http://www.cnblogs.com/yiyang/p/4525544.html)
- [Yon don't need jQuery](https://github.com/nefe/You-Dont-Need-jQuery/blob/master/README.zh-CN.md) ：能学到很多关于如何用现代浏览器原生 API 来实现 jQuery 实现的功能。


#### [Zepto.js](https://github.com/madrobby/zepto)
Zepto is a minimalist JavaScript library for modern browsers with a largely jQuery-compatible API. If you use jQuery, you already know how to use Zepto.
代码1800多行，与jQuery有着类似的api。看完后，写自己的类jQuery库。

相关链接：
- [Zepto：【 js 基础 】 setTimeout(fn, 0) 的作用](https://zhuanlan.zhihu.com/p/26962590)
- [Zepto对象思想与源码分析](https://www.kancloud.cn/wangfupeng/zepto-design-srouce/173682)

#### [Vue.js](https://cn.vuejs.org/index.html)
相关链接：
- [vue成长系列文章](https://github.com/xiaofuzi/deep-in-vue)：从源码的角度看vue的成长历程。
- [vue-router 源码分析-整体流程](https://github.com/dwqs/blog/issues/53):vue-router 也是简单好用, 功能强大. 本文将结合 Vue.js 来分析 vue-router 的整体流程.
- [从vue-cli源码学习如何写模板](https://github.com/dwqs/blog/issues/56):vue-cli 是 vuejs 官方提供的基于 vuejs 的项目脚手架工具, 可以很快的帮助 vuejs 开发者搭建一个 startup 项目, 免去环境配置的繁琐, 开箱即用. 看下 vue-cli 的实现.
- [一个研究vue源码的博客](https://github.com/youngwind/blog)

#### [Vuex](https://vuex.vuejs.org/zh/guide/)
Vuex是一个专为Vue服务，用于管理页面数据状态、提供统一数据操作的生态系统。它集中于Model层，规定所有的数据操作必须通过 action - mutation - state change 的流程来进行，再结合Vue的数据视图双向绑定特性来实现页面的展示更新。统一的页面状态管理以及操作处理，可以让复杂的组件交互变得简单清晰，同时可在调试模式下进行时光机般的倒退前进操作，查看数据改变过程，使code debug更加方便。

相关链接：
- [Vuex 源码解析](https://juejin.im/post/5962c13c6fb9a06b9e11a6a9)

#### [React](https://reactjs.org/)
React的源码适合有函数式编程经验的人看。

#### [Redux](https://redux.js.org/)
Redux 的源码非常的精炼，短短几百行代码，却提供了强大的功能。

相关链接：
- [redux 源码解读](https://segmentfault.com/a/1190000003791415)：redux并不局限于flux与react。redux 自身保持简洁以便适配各种场景，让社区发展出各种 redux-* 中间件或者插件，从而形成它自己的生态系统。

#### [Angular](https://angularjs.org/)    
    

#### [gulp](https://www.gulpjs.com.cn/)
基于流的自动化构建工具。

#### [webpack](https://webpack.js.org/)
现代 JavaScript 应用程序的静态模块打包器。

#### [Node.js](https://nodejs.org/zh-cn/)
如果对 node.js 感兴趣，可以看 express 和 connect 的代码，可以了解到 HTTP 服务端的工作方式，服务端 MVC 模型，路由分发等等很有价值的工程知识。

相关链接：
 - [【 js 模块加载 】深入学习模块化加载（node.js 模块源码）](https://zhuanlan.zhihu.com/p/26477995)

#### [echarts](http://echarts.baidu.com/index.html)   
    

#### 处理canavs的库
- [yi.js](https://github.com/wy-ei/YI)
- [CamanJS](http://camanjs.com/)
- [glfx/js](http://evanw.github.io/glfx.js/)
- [grafi.js](https://github.com/grafijs/grafi)
- [Filtrr2](https://github.com/alexmic/filtrr/tree/master/filtrr2)

#### [MobX](https://cn.mobx.js.org/)
简单、可扩展的状态管理

#### [async](https://caolan.github.io/async/)
Async is a utility module which provides straight-forward, powerful functions for working with asynchronous JavaScript. 

#### [JavaScript Patterns Collection](http://shichuan.github.io/javascript-patterns/) 
一些 JavaScript 模式的介绍，以及实现。

### UI框架

#### [Normalize.css](https://github.com/Alsiso/normalize-zh)
Normalize-zh.css 是根据对 Normalize.css 的源码分析后，经过学习与整理，将源代码中的英文注释文档翻译为中文版本，方便国内的开发者学习和使用。

相关链接：
-  [normalize.css](https://github.com/necolas/normalize.css/blob/master/normalize.css)： 能了解到各不同浏览器的默认样式之间的差异以及某些浏览器对某些元素设置的“奇怪”默认样式。

#### [Bootstrap](http://www.bootcss.com/)
纯手写CSS时会遇到各种各样的问题，看看Bootstrap是怎么解决这些问题的。

相关链接：
- [Bootstrap 的 JS 插件](https://github.com/twbs/bootstrap/tree/master/js)：Bootstrap 的 JS 插件都是基于 jQuery 的，每个插件也就一两百行代码。

#### [element-ui](http://element-cn.eleme.io/#/zh-CN)
很优秀的设计。

#### [Snippents](https://css-tricks.com/snippets/)
几行代码片段实现一些简单常见的功能。

#### [Animate.css](https://github.com/daneden/animate.css/blob/master/animate.css)
能了解常见的动画效果名称，以及如何 CSS 来写动画。

#### [fastclick](https://github.com/ftlabs/fastclick)
学习一波手势与事件

## 二、怎么读？
看到很多人都说，一定要**带着问题去读**，比如：**这个库／框架实现了一个什么东西，它是怎么实现的？为什么其他库不能支持这个功能，而它能支持？**只有带着问题，读着才有目的性，才能真正学到东西。

1. 在准备看一个开源项目源码的时候先去熟悉下这个项目的背景、功能以及相应的API。这步为了理解整个项目的功能做准备，也是为了后面重点看哪些模块做准备。
2. 查看项目的README.md文件。有些项目会在将设计文档和架构图放到md文件上，这样会让你宏观上对一些概念有些认识。
3. 了解目录结构，知道各个模块／文件夹的基本功能。
4. 从一些重要文件和入口文件开始，如package.json。
5. 当你深入读到某一块源码不理解，先试着在网上找找有没有相关的资料学习下，实在找不到可以先标记下放着，等你看到后面再回过头来看不理解的地方或者会茅塞顿开。
6. 在你阅读的过程中，如果发现一些代码片段很精妙的可以记录一下，或者尝试下有没有更好的方式去实现。
7. 当你不了解某个模块的作用时，你可以去看看测试用例，特别是单元测试(unit)，测试用例包含对输入输出的校验，从这里可以快速了解到模块相关的作用。
8. 要学会给项目打断点，在边读边运行项目源码的过程中，通过断点输出当前执行的堆栈信息对你理解项目也是有很大的帮助。

## 三、能学到什么？
 - 技巧
 - 设计原则
 - 待补充...

**学习项目源码往往周期比较长，最好的就是能够将项目Fork下来，慢慢去研究、记录看源码过程中的一些心得，在阅读的过程中不断加上自己的注释和理解，到最后看完再回过头来能说出它的架构和设计，阅读源码的终究目标是了解项目，最终能实现它。**

## 四、一些阅读工具
这些工具只是先看到记录下来，在后面进行阅读的时候具体使用再去评价是否好用，但工具这种东西，的确也是因人而异，找到最适合自己的工具就好。

 -  [lambda-view](https://github.com/Jianru-Lin/lambda-view)：provides you a better way to READ JavaScript source code.
 - [Octotree](https://chrome.google.com/webstore/detail/octotree/bkhaagjahfmjljalopjnoealnfndnagc?hl=en-US)：浏览器扩展，在github上面显示代码树，对你了解整个项目结构有着非常大的帮助。
 - [webstorm](https://www.jetbrains.com/webstorm/)，工具比较全面，比如支持typescript、babel等等一些前端经常用到的工具。
 - [SourceTree](https://www.sourcetreeapp.com/)，git可视化客户端工具，如果你喜欢从第一个commit开始看，那么这个工具对你比较有帮助，看看快速了解到每个版本提交的内容以及源码变更记录。
 - [高效阅读Github源代码](https://zhuanlan.zhihu.com/p/35778751)

## 五、参考文档
- [前端工程师的入门与进阶](https://shenbao.github.io/2017/04/22/justjavac-live/)
- [作为一名前端开发人员，有哪些值得一读的js代码？](https://www.zhihu.com/question/27471576)
- [优秀前端源码解读汇总](https://www.jianshu.com/p/19ae438e1a1c)
- [高效阅读Github源代码](https://zhuanlan.zhihu.com/p/35778751)
- [阅读前端项目源码的正确姿势](https://juejin.im/post/5aa8be9f5188253640011f2a)
- [Web 前端菜鸟源码阅读列表](http://www.imooc.com/article/4901)
- [统一回复《怎么学 JavaScript？》](https://juejin.im/entry/57ea104d816dfa005efb3e15)
- [JavaScript 有哪些优雅的框架值得学习？](https://segmentfault.com/q/1010000000670612)
- [阅读大型 JavaScript 源码时有什么好用的工具？](https://www.zhihu.com/question/25490540/answer/30883710)
- [研究学习javascript框架或jquery插件的正确姿势是什么？](https://www.zhihu.com/question/38621456/answer/77255054)
- [哪些 JavaScript 框架（库）的源代码最值得阅读和学习？](https://www.zhihu.com/question/19793863)
- [6 款 Javascript 的图像处理库](https://juejin.im/post/58c0edac0ce4630054592a78)
