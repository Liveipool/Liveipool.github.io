---
layout: post
title: React学习（五）-- Diff 算法 
date: 2017-05-10
categories: blog
description: 前端学习
---

# React学习（五）-- Diff 算法          
组件并不是真实的 DOM 节点，而是存在于内存之中的一种数据结构，叫做虚拟 DOM （virtual DOM）。只有当它插入文档以后，才会变成真实的 DOM 。根据 React 的设计，所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，这种算法叫做 DOM diff ，它可以极大提高网页的性能表现。          

假设有下面一段代码：          

```
var MyComponent = React.createClass({ 
  render: function() { 
    if (this.props.first) { 
      return <div className="first"><span>A Span</span></div>; 
    } else { 
      return <div className="second"><p>A Paragraph</p></div>; 
    } 
  } 
});
```

React is going to use this representation to try to find the minimum number of steps to go from the previous render to the next. For example, if we mount < MyComponent first={true} / >, replace it with < MyComponent first={false} / >, then unmount it, here are the DOM instructions that result:          

None to first：          
Create node: < div className="first" >< span >A Span< /span >< /div >          

First to second：          
Replace attribute: className="first" by className="second"          
Replace node: < span >A Span< /span > by < p >A Paragraph< /p >          

Second to none：          
Remove node: < div className="second" >< p >A Paragraph< /p >< /div >          

有点难，多看一下下面这篇文章：[React’s diff algorithm](https://calendar.perfplanet.com/2013/diff/)          