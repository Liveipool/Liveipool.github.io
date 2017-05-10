---
layout: post
title: React学习（一）-- 核心基础概念
date: 2017-05-08
categories: blog
description: 前端学习
---

# React学习（一）-- 核心基础概念         
这周开始实习了，项目中要用到react，下面是一些学习记录。       

#### 组件化         
react是基于组件化的。         

#### JSX语法         
JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。         
XML 有固定的标签开启和闭合的优点。这能让复杂的树更易于阅读，优于方法调用和对象字面量的形式。         
它没有修改 JavaScript 语义。         

#### 重要的方法         

``` javascript
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        Hello, world! I am a CommentBox.
      </div>
    );
  }
});
ReactDOM.render(
  <CommentBox />,
  document.getElementById('content')
);
```

我们在一个 JavaScript 对象中传递一些方法到 React.createClass() 来创建一个新的React组件。这些方法中最重要的是 **render**，该方法返回一颗 React 组件树，这棵树最终将会渲染成 HTML。         

**这个 < div > 标签不是真实的DOM节点；他们是 React div 组件的实例化。你可以把这些看做是React知道如何处理的标记或者是一些数据 。**React 是安全的。我们不生成 HTML 字符串，因此XSS防护是默认特性。         

你没有必要返回基本的 HTML。你可以返回一个你（或者其他人）创建的组件树。这就使 React 组件化：一个可维护前端的关键原则。         

**ReactDOM.render() 实例化根组件，启动框架，注入标记到原始的 DOM 元素中，作为第二个参数提供。**         

ReactDOM 模块暴露了 DOM 相关的方法， 而 React 保有被不同平台的 React 共享的核心工具 （例如 React Native）。         

 ReactDOM.render 通常放在脚本底部。         

#### props         
从父级传来的数据在子组件里作为 '属性' 可供使用。 这些 '属性' 可以通过 this.props 访问。         
在 JSX 中,通过将 JavaScript 表达式放在大括号中（作为属性或者子节点）,你可以把文本或者 React 组件放置到树中。我们以 this.props 的 keys 访问传递给组件的命名属性，以 this.props.children 访问任何嵌套的元素。         

#### state         
props 是不可变的：它们从父级传来并被父级“拥有”。为了实现交互，我们给组件引进了可变的 state。this.state 是组件私有的，可以通过调用 this.setState() 改变它。每当state更新，组件就重新渲染自己。         

**render() 方法被声明为一个带有 this.props 和 this.state 的函数。框架保证了 UI 总是与输入一致。**         

#### Lifting State Up         
**When you want to aggregate data from multiple children or to have two child components communicate with each other, move the state upwards so that it lives in the parent component. The parent can then pass the state back down to the children via props, so that the child components are always in sync with each other and with the parent.**         

#### controlled components         
某个组件绑定了某个事件函数，这个组件就是受控组件。              

#### Functional Components         
In fact, React supports a simpler syntax called functional components for component types like Square that only consist of a render method.          

``` javascript
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

晚上回到宿舍又看了看react，看到了阮一峰老师的入门博客，只能说讲得清楚明了，真的看懂能少走很多弯路：    
[React 入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html)     
[也许，DOM 不是答案](http://www.ruanyifeng.com/blog/2015/02/future-of-dom.html)      
[React 技术栈系列教程](http://www.ruanyifeng.com/blog/2016/09/react-technology-stack.html)      