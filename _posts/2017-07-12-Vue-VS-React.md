---
layout: post
title: Vue的MVVM和数据双向绑定及其和React单向数据流的比较
date: 2017-07-12
categories: blog
description: 前端学习
---

# Vue的MVVM和数据双向绑定及其和React单向数据流的比较     
**Vue.js是以数据驱动和组件化的思想构建的。Vue并未完全遵循MVVM模式，但无疑是参考了MVVM的思想，我觉得我们也可以按照MVVM来对其进行理解。**     

#### 数据驱动     
Vue.js是**数据驱动**的，我们无需手动操作DOM。Vue.js2.0和React类似，都是采用的virtual DOM的思想，通过数据驱动视图发生变化。     
而为什么这些流行的框架都是在避免操作DOM也无须赘述，阮一峰老师的文章写得很好：[也许，DOM 不是答案](http://www.ruanyifeng.com/blog/2015/02/future-of-dom.html)     

#### Vue 的MVVM模型     
下图描述了Vue的MVVM模式（Model-View-ViewModel）以及在Vue中ViewModel是如何和View以及Model进行交互的。     

![MVVM结构.jpg](http://upload-images.jianshu.io/upload_images/3001083-37ba2a880dfbdd76.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### 双向绑定     
而说到ViewModel和View以及Model的交互，就一定要提到Vue的另外一个核心概念了：双向数据绑定。     
ViewModel是Vue.js的核心，**它是一个Vue实例**。Vue实例是作用于某一个HTML元素上的，这个元素可以是HTML的body元素，也可以是指定了id的某个元素。     

当创建了ViewModel后，双向绑定是如何达成的呢？     
首先，我们将上图中的DOM Listeners和Data Bindings看作两个工具，它们是实现双向绑定的关键。     
1. 从View侧看，ViewModel中的DOM Listeners工具会帮我们监测页面上DOM元素的变化，如果有变化，则更改Model中的数据；     
2. 从Model侧看，当我们更新Model中的数据时，Data Bindings工具会帮我们更新页面中的DOM元素。     

具体的代码实现：     

访问器属性是对象中的一种特殊属性，它不能直接在对象中设置，而必须通过 defineProperty() 方法单独定义。     

```
const obj = { };

// 为obj定义一个名为 hello 的访问器属性

Object.defineProperty(obj, "hello", {
	get: function () {return sth},
	set: function (val) {/* do sth */}
});

obj.hello // 可以像普通属性一样读取访问器属性
// 访问器属性的"值"比较特殊，读取或设置访问器属性的值，实际上是调用其内部特性：get和set函数。

obj.hello // 读取属性，就是调用get函数并返回get函数的返回值

obj.hello = "abc" // 为属性赋值，就是调用set函数，赋值其实是传参
```

一个简单的实现双向绑定的例子，实现文本框随输入文字的变化：     

``` javascript
<input type="text" id="a">
<span id="b"></span>

<script>
const obj={};
Object.defineProperty(obj, 'hello', {
	set: function (newVal) {
		document.getElementById('a').value = newVal;
		document.getElementById('b').innderHTML = newVal;
	}
});

document.addEventListener('keyup', function(e) {
	obj.hello = e.target.value;
});
</script>
```

而以上是Vue对于双向绑定原理的具体实现，而我们在写Vue代码时，是写成下面这样的。     

```
// helloworld.vue

// view
<template>
	<div id="app">
		<input v-model="message" type="text">
	    <div> {{message}} </div>
	</div>
</template>

<script>
// Model
const message = 'Hello World!';

// 创建一个 Vue 实例或 "ViewModel"
// 它连接 View 与 Model
new Vue({
	el: '#app',
	data () {
		return {
			message,
		};
	}
})
</script>
```

也就是说，那些复杂的工作、具体的双向绑定的原理代码，都是Vue帮我们写好了，我们需要做的就是根据Vue的语法来写就好。     

#### Vue的双向绑定 VS React的单向数据流动     
前段时间在公司做项目用了一段时间的React，虽然它和Vue都是一种虚拟DOM的概念，但我觉得它们最大的区别还是在数据流上。     
Vue的数据是双向绑定的，用户操作之后会改变数据层里的数据，数据层里的数据改变也会导致页面上显示的数据的变化。     
而React的数据是单向流动的，在一个组件里，数据存在于state里，如果它的子组件想要使用它的数据，只能通过它从props传到子组件中。并且，父组件是无法访问到子组件的数据的。如果子组件中的数据需要共用，再使用状态提升等方法。     
而就算是使用了Redux之后，Redux的store作为Model层，React作为View层，看起来是更改View层的数据就会改动到Model层的数据，而改动Model层的数据，View层显示的数据也会变化，那么是否就是说在使用了Redux之后，React也采用的是双向数据绑定的原理呢？     
根据我的理解，实际上不是双向的。让我们再来细细分析一下Vue的双向绑定，比如一个View层的输入框和数据层的数据，它们是通过一个ViewModel间接连接在一起的，更改任意一方都会触发ViewModel里的函数更改另外一方。     
而View层的React中的数据和Model层的Redux中的store里的数据，实际上并没有直接或间接的联系。store里的数据更改都是通过dispatch action来实现的，意思就是说，在View层有一个输入框，对其进行输入，如果不进行dispatch action，Model层中的store里的数据是不会变化的，因此可以说是导致Model层中数据变化的本质是dispatch的action导致的变化，和View层并没有直接意义上的联系，不对输入框进行输入也是可以通过dispatch action来改变store里的值从而改变输入框的值。     
而Redux也就是一种单向数据流的思想，所有的数据都在Redux下的store里，store里的数据有变化时再将数据传到View层中的React里进行重新渲染显示出新的数据。     
因此我觉得，Vue和React的数据流还是有本质上的区别的。 

#### vue和angularjs1和react的比较     
 - 都是数据驱动的     
 - AngularJS的api设计较为复杂，学习成本较高，react相对来说也比较高，因为它有一整套技术栈。     
 - react中全都是组件，主要分为了展示组件和功能组件，但angular有service、directive、component，显得有点点乱，不知用哪个最好，service像功能组件，directive像展示组件。在 Vue 中指令和组件分得更清晰。指令只封装 DOM 操作，而组件代表一个自给自足的独立单元 —— 有自己的视图和数据逻辑。在 AngularJS 中两者有不少相混的地方。     
 - 在 AngularJS 中，当 watcher 越来越多时会变得越来越慢，因为作用域内的每一次变化，所有 watcher 都要重新计算。并且，如果一些 watcher 触发另一个更新，脏检查循环（digest cycle）可能要运行多次。AngularJS 用户常常要使用深奥的技术，以解决脏检查循环的问题。有时没有简单的办法来优化有大量 watcher 的作用域。Vue 则根本没有这个问题，因为它使用基于依赖追踪的观察系统并且异步队列更新，所有的数据变化都是独立触发，除非它们之间有明确的依赖关系。react类似，并且react检测数据变化的diff算法也很优秀。     
 - angularjs1有个较为重要的概念是依赖注入，依赖注入式实现控制反转的一种软件设计模式，即将被依赖的对象传递给依赖者，而不需要依赖者自己去创建和查找所需对象。而vue和react不是这样。可能这就是为什么感觉vue和react的模块化做得更好的原因。     
 - react和vue都是有虚拟DOM的概念的，angularjs不是。     
 - react和vue都是一个库,它们将注意力集中保持在核心库，它们不是像angularjs一样的一个完整的框架，若还需要复杂的如路由或状态管理等功能时还需要redux和vuex等其他库。     
 - react有利于seo。         

#### 参考文献     
[Vue.js——60分钟快速入门](http://www.cnblogs.com/keepfool/p/5619070.html)     
