---
layout: post
title: 关于React优化的一些相关知识点
date: 2017-09-08
categories: blog
description: 前端学习
---

# 关于React优化的一些相关知识点     

#### Virtual Dom     
Virtual Dom算是React的一个重大的特色，因为Facebook宣称由于Virtual Dom的帮助，React能够达到很好的性能。是的，Facebook说的没错，但只说了一半，它说漏的一半是：“除非你能正确的采用一系列优化手段”。     

**缺点**：每次 state 更改，render 函数都要生成完整的虚拟 DOM，哪怕 state 改动很小，render函数也会完整计算一遍。如果 render 函数很复杂，这个过程就会白白浪费很多计算资源。这是因为 ReactJS 收到的新旧两个虚拟 DOM 之间相互独立，ReactJS 并不知道数据源发生了什么操作，只能根据新旧两个虚拟 DOM 来猜测需要执行的操作。自动的猜测算法既不准又慢，必须要前端开发者手动提供 key 属性、shouldComponentUpdate 方法、componentDidUpdate 方法或者 componentWillUpdate 等方法才能帮助 ReactJS 框架猜对。     

#### 两个优化的时间点     
1. 首次渲染挂载时。     
2. 更新时。     

首屏时间优化方式不多，可以尝试用React Server Render (又称Isomorphic)去提高效率。     
其他的优化方式主要是优化更新时。     

![image.png](http://upload-images.jianshu.io/upload_images/3001083-c18ba3e7022d2b16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### shouldComponentUpdate     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-7ef99f107ebde904.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### setState     
 - 请慎用setState，因其容易导致重新渲染。     
 - 请只传递component需要的props。     
 - 这里还有要注意的是，当你调用 setState 的时候，React.js 并不会马上修改 state。而是把这个对象放到一个更新队列里面，稍后才会从队列当中把新的状态提取出来合并到 state 当中，然后再触发组件更新。(setState是异步的，进行合并之后再更新state，起到优化的作用。)     

#### Immutablejs     
Immutablejs本身就能生成不可变数据，这样就不需要开发者自己去做数据深拷贝，可以直接拿prevProps/prevState和nextProps/nextState来比较。     
Immutable本身还提供了数据的比较方法，这样开发者也不用自己去写数据深比较的方法。     

![image.png](http://upload-images.jianshu.io/upload_images/3001083-7ba62cd444f8bfd7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-6c273a36698dbc93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-d064210a0daf9847.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-c1a66a3a40adef74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-5401a7e10f2a31e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-4b34129ea7272aac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

Immutable 缺点：     
1. 需要学习新的 API     
2. 增加了资源文件大小     
3. 容易与原生对象混淆     

#### diff算法     
这篇文章讲得很清晰：[深入浅出React（四）：虚拟DOM Diff算法解析](http://www.infoq.com/cn/articles/react-dom-diff)     

#### 纯函数     
多写纯函数：纯函数是指不依赖于且不改变它作用域之外的变量状态的函数 ，纯函数有三大原则构成：     
1. 给定相同的输入，就一定会有相同的输出     
2. 运行过程中没有任何副作用     
3. 没有额外的状态依赖     

无论是在什么语言当中，尽可能的使用纯函数去实现程序需求都是不错的方式。因为其简介、超级独立的特点， 它容易在代码中移动、重构、重新组织，让程序更灵活，更适应将来的改变。在实现任何产品需求中，我们尽可能的将逻辑代码与业务代码分离，将状态函数和纯函数分离开来，做到低耦合都是很好的编程习惯。     

![image.png](http://upload-images.jianshu.io/upload_images/3001083-056c401bd8a68feb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-77b460fadf121a34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![image.png](http://upload-images.jianshu.io/upload_images/3001083-8126f4a23a940179.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### key     
 - 我们知道，React 元素可以具有一个特殊的属性 key，这个属性不是给用户自己用的，而是给 React 自己用的。如果我们动态地创建 React 元素，而且 React 元素内包含数量或顺序不确定的子元素时，我们就需要提供 key 这个特殊的属性。     
 - react利用key来识别组件，它是一种身份标识标识，就像我们的身份证用来辨识一个人一样。每个key对应一个组件，相同的key react认为是同一个组件，这样后续相同的key对应组件都不会被创建。     
 - 那么，为什么只有数组中的元素需要有唯一的 key，而其他的元素则不需要呢？答案是：React 有能力辨别出，更新前后元素的对应关系。（其实也就是说那些元素会有一个自动生成的key）     
 - 初学 React 时还容易产生另一个困惑，那就是为什么 JSX 不支持 if 表达式来有选择地输出（不能这样：{if(yes){ <div {...props}/> }}），而必须采用三元运算符来完成这项工作（必须这样：{yes ? <div {...props}/>} : null）。那是因为，React 需要一个 null 去占住那个元素本来的位置。     

![image.png](http://upload-images.jianshu.io/upload_images/3001083-afc4eef45030311e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### 参考文章     
[React 组件性能优化](https://www.manster.me/?p=656)     
[React 实践心得：key 属性的原理和用法](http://taobaofed.org/blog/2016/08/24/react-key/)     
[React之key详解](http://www.cnblogs.com/wonyun/p/6743988.html)     