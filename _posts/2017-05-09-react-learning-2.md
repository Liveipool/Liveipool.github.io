---
layout: post
title: React学习（二）-- 更多核心概念 
date: 2017-05-09
categories: blog
description: 前端学习
---

# React学习（二）-- 更多核心概念           
React 是为了解决一个问题：构建随着时间数据不断变化的大规模应用程序。           

仅仅只要表达出你的应用程序在任一个时间点应该呈现的样子，然后当底层的数据变了，React 会自动处理所有用户界面的更新。           
数据变化后，React 概念上与点击“刷新”按钮类似，但仅会更新变化的部分。           

#### React isn't an MVC framework.           
React is a library for building composable user interfaces. It encourages the creation of reusable UI components which present data that changes over time.           

#### 如何划分组件？           
但是你如何知道什么东西应该是独立的组件？只需在你创建一个函数或者对象时，根据是否使用过相同技术来做决定。一种这样的技术是单一功能原则（single responsibility principle），也就是一个组件在理想情况下只做一件事情。如果它最终增长了，它就应该被分解为更小的组件。           

#### props vs state           
**在React中，尽量避免直接操作DOM，应该操作数据，数据一般有两种形式：props和state**           
State 仅仅是为互动性，也就是随时间变化的数据所预留的。           
**要正确的构建你的 app，你首先需要思考你的 app 需要的可变 state 的最小组**。           
让我们逐个检查出哪一个是state，只需要简单地问以下三个问题:           
1.它是通过props从父级传递来的吗？如果是，它可能不是 state。           
2.它随时间变化吗？如果不是,它可能不是 state。           
3.你能基于其他任何组件里的 state 或者 props 计算出它吗？如果是,它可能不是state.           
记住：React 总是在组件层级中单向数据流动的。可能不能立刻明白哪些组件应该拥有哪些 state。 这对于新手在理解上经常是最具挑战的一部分， 所以跟着这几步来弄明白它：           

对于你的应用里每一个数据块：           
1.确定哪些组件要基于 state 来渲染内容。           
2.找到一个共同的拥有者组件（在所有需要这个state组件的层次之上，找出共有的单一组件）。           
3.要么是共同拥有者，要么是其他在层级里更高级的组件应该拥有这个state。           
4.如果你不能找到一个组件让其可以有意义地拥有这个 state，可以简单地创建一个新的组件 hold 住这个state，并把它添加到比共同拥有者组件更高的层级上。           

尝试把尽可能多的组件无状态化。 这样做能隔离 state，把它放到最合理的地方，也能减少冗余，同时易于解释程序运作过程。           
常用的模式是创建多个只负责渲染数据的无状态（stateless）组件，在它们的上层创建一个有状态（stateful）组件并把它的状态通过 props 传给子级。这个有状态的组件封装了所有用户的交互逻辑，而这些无状态组件则负责声明式地渲染数据。           

#### 在同一个函数中多次调用setState只会render一次，因为react相当于对这个函数进行了一层封装。           
           
#### 构建方向           
你可以自顶向下或自底向上的构建。也就是说，你可以既从较高的层级也可以从较低的层级开始构建组件。在较简单的例子里，通常自顶向下要容易一些，然而在更大的项目上，自底向上地构建更容易，并且更方便伴随着构建写测试。           

#### 再看看生命周期           
组件的生命周期分成三个状态：           
Mounting（挂载）：已插入真实 DOM           
Updating（更新）：正在被重新渲染           
Unmounting（卸载）：已移出真实 DOM           
React 为每个状态都提供了两种处理函数，will 函数在进入状态之前调用，did 函数在进入状态之后调用，三种状态共计五种处理函数。           
componentWillMount()           
componentDidMount()           
componentWillUpdate(object nextProps, object nextState)           
componentDidUpdate(object prevProps, object prevState)           
componentWillUnmount()           
**这五个函数都只能在对应的阶段进行执行，并不能干扰阶段的变化**           

getInitialState(): object 在组件挂载前被调用。有状态组件(Stateful components) 应该实现此函数并返回初始state的数据。    

此外，React 还提供两种特殊状态的处理函数。           
componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用，**可用来进行老数据和新收到的props数据进行一些判断。**           
shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用，**常用来优化React代码，它不同于componentWillUpdate，它执行的时间相同，但它可以通过返回值true或者false来决定是否需要重新render，比较有用。**           


##### 已挂载的方法     
Mounted 复合组件同样支持以下方法:     
component.forceUpdate() 可以在任何已挂载的组件上调用，在你知道某些深处的组件状态在未使用 this.setState() 就被改变了时。     
这个没太懂？    