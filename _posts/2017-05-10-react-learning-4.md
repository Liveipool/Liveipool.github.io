---
layout: post
title: React学习（四）-- 事件 
date: 2017-05-10
categories: blog
description: 前端学习
---

# React学习（四）-- 事件              

#### 事件处理与合成事件（Synthetic Events）          
React 里只需把事件处理器（event handler）以骆峰命名（camelCased）形式当作组件的 props 传入即可，就像使用普通 HTML 那样。React 内部创建一套合成事件系统来使所有事件在 IE8 和以上浏览器表现一致。也就是说，React 知道如何冒泡和捕获事件，而且你的事件处理器接收到的 events 参数与 W3C 规范 一致，无论你使用哪种浏览器。          

#### 幕后原理：自动绑定（Autobinding）和事件代理（Event Delegation）          
在幕后，React 做了一些操作来让代码高效运行且易于理解。          
          
自动绑定: 在 JavaScript 里创建回调的时候，为了保证 this 的正确性，一般都需要显式地绑定方法到它的实例上。在 React 中，所有方法被自动绑定到了它的组件实例上（除非使用ES6的class符号）。React 还缓存这些绑定方法，所以 CPU 和内存都是非常高效。而且还能减少打字！          

事件代理: React 实际并没有把事件处理器绑定到节点本身。当 React 启动的时候，它在最外层使用唯一一个事件监听器处理所有事件。当组件被加载和卸载时，只是在内部映射里添加或删除事件处理器。当事件触发，React 根据映射来决定如何分发。当映射里没有事件处理函数时，会当作空操作处理。参考 David Walsh 很棒的文章 了解这样做高效的原因。          

#### 分原生事件与合成事件          
我们在 componentDidMount 方法里面通过 addEventListener 绑定的事件就是浏览器原生事件，使用原生事件的时候注意在 componentWillUnmount 解除绑定 removeEventListener，所有通过 JSX 这种方式绑定的事件都是绑定到“合成事件”。 “合成事件”会以事件委托（event delegation）的方式绑定到组件最上层，并且在组件卸载（unmount）的时候自动销毁绑定的事件。          

有点难，多看看这篇文章：[React事件初探](http://imweb.io/topic/5774e361af96c5e776f1f5cd)          
