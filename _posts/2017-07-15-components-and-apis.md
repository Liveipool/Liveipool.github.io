---
layout: post
title: 组件化编程及API设计
date: 2017-07-15
categories: blog
description: 前端学习
---

# 组件化编程及API设计       
最近在写图腾项目中的一个组件，下面是一些觉得值得注意的点。       

#### 组件命名       
组件命名应该遵从以下几点原则       
1. 有意义: 名字不要太详细，也不要太抽象。       
2. 短: 名字最好是2-3个单词。       
3. 可读的:容易让人能读出来以便我们可以更容易的讨论它。       
4. 在我们的项目中尽量使用连字符。       

```
<!-- 建议这样 -->
<app-header></app-header>
<user-list></user-list>
<range-slider></range-slider>

<!-- 避免这样 -->
<btn-group></btn-group> <!-- 足够短但是不容易发音，使用`button-group`代替 -->
<ui-slider></ui-slider> <!-- 所有的组件都是ui元素，所以这样命名无意义 -->
<slider></slider> <!--不是我们适应的风格 -->
```

#### 保证组件模板中的表达式简短       
把复杂的语法移动到methods或者计算属性中，这个在官方文档中也写到了，表达式太长容易表述不明。       

#### 保证组件的props简单       
 - 尽管vue支持通过props传递复杂的object，但是你要尽量保持props传递的数据简单,尽量只传递基本数据类型(strings, numbers, booleans)       
 - props只传递简单类型数据和函数，让我们组件的api看起来更像原生html的属性。       
 - props传递复杂数据类型，让你的组件很难重构，也会造成代码冗余。       

```
<!-- recommended -->
<range-slider
  min="0"
  max="100"
  :on-end="updateResults">
</range-slider>

<!-- avoid -->
<range-slider :config="complexConfigObject"></range-slider>
```

#### 对你组件的props做一些限制       
 - vue 组件中props就是api,健壮且可预测的api让别人更容易使用你的组件       
 - 对props做一些限制保证你的组件正常工作，即使别人没有按照你预想的方式调用你的组件。       
 - 属性设置默认值       
 - 属性设置数据类型校验       
 - 使用组件之前检查props是否存在       

#### 单一职责原则       
保持组件的单一职责很重要       

#### API设计              
 - 命名尽量不要超过三个单词，易读易记       
 - 保持接口的流畅性，即降低映射成本和记忆成本       
 - 保持接口风格的一致性       
 - 永远不修改接口，只扩展它！可扩展性同时会要求接口的职责单一，多职责的接口很难扩展。       

```
// 保持接口风格的一致性
Nightware:
setColor,
letBackGround，
changefontSize，
makedisplay，

dream:
setColor，
setBackground，
setFontSize，
set.........
```

#### 设计思想       
 - 尽量简单       
 - 保持可扩展性       
 - TODOS....       

以上内容只是刚开始学习进行组件开发时上网查到的一些觉得好的点，但这种东西肯定要慢慢在自己的编码过程中体会和总结，因此这篇博客还会根据组件的开发过程进行持续更新...       
