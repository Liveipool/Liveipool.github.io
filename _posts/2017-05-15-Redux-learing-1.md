---
layout: post
title: Redux学习（一）--基础概念
date: 2017-05-15
categories: blog
description: 前端学习
---

# Redux学习（一）--基础概念          
React只是一个javasvript类库，只是一个DOM的抽象层，可以看作是MVC中的V，如果想要使用React来搭建大型的应用则需要搭配一个前端框架，自Flux之后，Redux是口碑最好的一个。          

### 设计思想          
Redux 的设计思想很简单，就两句话。          
（1）Web 应用是一个状态机，视图与状态是一一对应的。          
（2）所有的状态，保存在一个对象里面。          

#### store          
Store 就是保存数据的地方，你可以把它看成一个容器。整个应用只能有一个 Store。          
Redux 提供createStore这个函数，用来生成 Store。          

createStore函数接受另一个函数作为参数，返回新生成的 Store 对象。          

#### State          
Store对象包含所有数据。如果想得到某个时点的数据，就要对 Store 生成快照。这种时点的数据集合，就叫做 State。          
当前时刻的 State，可以通过store.getState()拿到。          

Redux 规定， 一个 State 对应一个 View。只要 State 相同，View 就相同。你知道 State，就知道 View 是什么样，反之亦然。          

#### Action          
State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。所以，State 的变化必须是 View 导致的。Action 就是 View 发出的通知，表示 State 应该要发生变化了。          

可以这样理解，Action 描述当前发生的事情。改变 State 的唯一办法，就是使用 Action。它会运送数据到 Store。          

#### store.dispatch()          
store.dispatch()是 View 发出 Action 的唯一方法。          

#### Reducer          
Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。
Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。          

永远不要在 reducer 里做这些操作：          
1. 修改传入参数；          
2. 执行有副作用的操作，如 API 请求和路由跳转；          
3. 调用非纯函数，如 Date.now() 或 Math.random()。          
现在只需要谨记 reducer 一定要保持纯净。只要传入参数相同，返回计算得到的下一个 state 就一定相同。没有特殊情况、没有副作用，没有 API 请求、没有变量修改，单纯执行计算。          

你应该把要做的修改变成一个普通对象，这个对象被叫做 action，而不是直接修改 state。然后编写专门的函数来决定每个 action 如何改变应用的 state，这个函数被叫做 reducer。          

#### 搭配 React          
这里需要再强调一下：Redux 和 React 之间没有关系。Redux 支持 React、Angular、Ember、jQuery 甚至纯 JavaScript。
尽管如此，Redux 还是和 React 和 Deku 这类框架搭配起来用最好，因为这类框架允许你以 state 函数的形式来描述界面，Redux 通过 action 的形式来发起 state 变化。          

#### 一些参考的教程          
 - [React 技术栈系列教程](http://www.ruanyifeng.com/blog/2016/09/react-technology-stack.html)          
 - [Redux 入门教程（一）：基本用法](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)          
 - [Redux 中文文档](http://cn.redux.js.org/docs/basics/ExampleTodoList.html)          
 - [Redux 视频教程](https://egghead.io/courses/getting-started-with-redux)          
