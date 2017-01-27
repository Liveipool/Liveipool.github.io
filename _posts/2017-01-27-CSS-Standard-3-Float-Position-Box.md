---
layout: post
title: CSS 标准决方案(3)--浮动、定位和盒模型
date: 2017-01-27
categories: blog
description: CSS学习
---

# 盒模型、定位 和 浮动      

## 盒模型      
盒模型是CSS的基石之一，它指定元素如何显示以及如何相互交互。页面上的每个元素被看做一个矩形框，这个框由元素的content、padding、border、margin组成。      
![3.11.png](http://upload-images.jianshu.io/upload_images/3001083-0646a217d438b1bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**当在元素上添加背景时，那么背景会应用于content和padding组成的区域。**            
在CSS中，**width**和**height**指的是内容区域的宽度和高度。      
（IE6及更早的IE版本中，在混杂模式下，是使用的它自己的非标准盒模型，width属性不是内容的宽度，而是内容、内边距和边框的宽度总和。）      

**外边距叠加**      
外边距叠加也是一个重要的概念。外边距的高度等于两个发生叠加的外边距的高度中的较大者。      
![3.12.png](http://upload-images.jianshu.io/upload_images/3001083-166a040ccf6c94d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**注意，外边距叠加是指在两个垂直方向的外边距重合时进行叠加，两个水平的外边距并不会叠加。**      

## 定位      
p、h1或div等元素常常称为**块级元素**，这意味着这些元素显示为一块内容，即“块框”。与之相反，strong和span等元素称为**行内元素**，因为它们的内容显示在行中，即“行内框”。      
![3.2.11.png](http://upload-images.jianshu.io/upload_images/3001083-2e79c93c0c4ce8de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

CSS 2.1也可以把元素的display属性设置为inline-block，这个声明让元素像行内元素一样水平的依次排列。但是，框的内容仍然复合块级框的行为，例如能够显式地设置width、height、垂直外边距和内边距。      

行内框在一行中水平排列，可以通过**内边距**、**边框**和**外边距**来调整它们的水平距离，**width**、**height**、**垂直外边距**不起作用。行高指行框的高度，总是足以容纳它包含的所有行内框。要想修改一个行内框的高度，只能增大内容所占高度，如调大字体大小等。      

下面是一些例子：      

```
span#test4.inline-test.m-100.p-100.w-20.h-20
span#test5.inline-test.m-100.p-100.w-20.h-20

div, span {
	background-color: grey;
	border: 20px solid black;
}
```
![test1.png](http://upload-images.jianshu.io/upload_images/3001083-c01f510051c4e3c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![test2.png](http://upload-images.jianshu.io/upload_images/3001083-f3087762fadd5955.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
span#test4.inline-test.m-100.p-100.w-20.h-20.fs-200 lallna
span#test5.inline-test.m-100.p-100.w-20.h-20.fs-50 wow

div, span {
	background-color: grey;
	border: 20px solid black;
}
```
![test3.png](http://upload-images.jianshu.io/upload_images/3001083-2a3781bfe8c52899.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
span#test4.inline-test.m-10.p-10.w-200.h-200
span#test5.inline-test.m-10.p-10.w-200.h-200
div#test1.block-test.m-20.p-20.w-100.h-100
div#test2.block-test.m-20.p-20.w-100.h-100
div#test3.block-test.m-20.p-20.w-100.h-100

div, span {
	background-color: grey;
	border: 20px solid black;
}
```
![test4.png](http://upload-images.jianshu.io/upload_images/3001083-0ff8e5011264446e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
span#test4.inline-test.m-10.p-10.w-200.h-200
span#test5.inline-test.m-10.p-10.w-200.h-200
div#test1.block-test.m-20.p-20.w-100.h-100
div#test2.block-test.m-20.p-20.w-100.h-100
div#test3.block-test.m-20.p-20.w-100.h-100

div, span {
	background-color: grey;
	border: 20px solid black;
}

.inline-test {
	display: block;
}
```
![test5.png](http://upload-images.jianshu.io/upload_images/3001083-99ac1e24806cde94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
span#test4.inline-test.m-10.p-10.w-200.h-200
span#test5.inline-test.m-10.p-10.w-200.h-200
div#test1.block-test.m-20.p-20.w-100.h-100
div#test2.block-test.m-20.p-20.w-100.h-100
div#test3.block-test.m-20.p-20.w-100.h-100

div, span {
	background-color: grey;
	border: 20px solid black;
}

.block-test {
	display: inline;
}
```
![test6.png](http://upload-images.jianshu.io/upload_images/3001083-805daa8169a9ffcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
span#test4.inline-test.m-10.p-10.w-200.h-200
span#test5.inline-test.m-10.p-10.w-200.h-200
div#test1.block-test.m-20.p-20.w-100.h-100
div#test2.block-test.m-20.p-20.w-100.h-100
div#test3.block-test.m-20.p-20.w-100.h-100

div, span {
	background-color: grey;
	border: 20px solid black;
}

.block-test {
	display: inline-block;
}
```
![test7.png](http://upload-images.jianshu.io/upload_images/3001083-5e643853582f1ba8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 相对定位 和 绝对定位      
 - 相对定位是相对于元素在文档流中的位置。      
 - 绝对定位是相对于距离它最近的已定位的祖先元素，如果不存在已定位的祖先元素，那么相对于初始包含块。      

```
div#test1.block-test.m-10.p-10.w-100.h-100.fs-30 1
div#test2.block-test.m-10.p-10.w-100.h-100.fs-30 2
div#test3.block-test.m-10.p-10.w-100.h-100.fs-30 3

div {
	background-color: grey;
	border: 20px solid black;
	color: #FFFFFF;	
	display: inline-block;
}
```
![test8.png](http://upload-images.jianshu.io/upload_images/3001083-e87a6e5175a5e3d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
div#test1.block-test.m-10.p-10.w-100.h-100.fs-30 1
div#test2.block-test.m-10.p-10.w-100.h-100.fs-30 2
div#test3.block-test.m-10.p-10.w-100.h-100.fs-30 3

div {
	background-color: grey;
	border: 20px solid black;
	color: #FFFFFF;
	display: inline-block;
}

#test2 {
	position: relative;
	left: 70px;
	top: 70px;
}
```
![test9.png](http://upload-images.jianshu.io/upload_images/3001083-238bca148859f76c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**相对定位实际上被看做普通流定位模型的一部分，因为元素的位置是相对于它在普通流中的位置的。**      
**与之相反，绝对定位使元素的位置与文档流无关，因此不占据空间。普通文档流中其他元素的布局就像绝对定位的元素不存在一样。**      

```
div#test1.block-test.m-10.p-10.w-100.h-100.fs-30 1
div#test2.block-test.m-10.p-10.w-100.h-100.fs-30 2
div#test3.block-test.m-10.p-10.w-100.h-100.fs-30 3

div {
	background-color: grey;
	border: 20px solid black;
	color: #FFFFFF;
	display: inline-block;
}

#test2 {
	position: absolute;
	left: 70px;
	top: 70px;
}
```
![test10.png](http://upload-images.jianshu.io/upload_images/3001083-28a772b41d202e47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
div#test1.block-test.m-10.p-10.w-100.h-100.fs-30 1
div#test2.block-test.m-10.p-10.w-100.h-100.fs-30 2
	div#test3.block-test.m-10.p-10.w-100.h-100.fs-30 3

div, span {
	background-color: grey;
	border: 20px solid black;
	color: #FFFFFF;
}
```
![test11.png](http://upload-images.jianshu.io/upload_images/3001083-0983f8748d508a13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
div#test1.block-test.m-10.p-10.w-100.h-100.fs-30 1
div#test2.block-test.m-10.p-10.w-100.h-100.fs-30 2
	div#test3.block-test.m-10.p-10.w-100.h-100.fs-30 3

div, span {
	background-color: grey;
	border: 20px solid black;
	color: #FFFFFF;
}

#test1 {
	position: relative;
}

#test3 {
	position: absolute;
	left: 150px;
	top: 20px;
}
```
![test13.png](http://upload-images.jianshu.io/upload_images/3001083-fc49aa8a020c49d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      


**可以通过z-index属性来控制这些框的叠放次序。z-index为正数，表示离用户更近，为负数表示离用户更远。**      

```
div#test1.block-test.m-10.p-10.w-100.h-100.fs-30 1
div#test2.block-test.m-10.p-10.w-100.h-100.fs-30 2
div#test3.block-test.m-10.p-10.w-100.h-100.fs-30 3

div {
	background-color: grey;
	border: 20px solid black;
	color: #FFFFFF;
	display: inline-block;
}

#test2 {
	position: absolute;
	left: 70px;
	top: 70px;
	z-index: -1;
}
```
![test12.png](http://upload-images.jianshu.io/upload_images/3001083-3d8c36d0de5ffe15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 浮动      
浮动的框可以左右移动，直到它的外边缘碰到包含框或另一个浮动框的边缘。因为浮动框不在文档的普通流中，所以文档的普通流中的块框表现得就像浮动框不存在一样。      
![3.2.41.png](http://upload-images.jianshu.io/upload_images/3001083-2f7b825bb2ef2c21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![3.2.42.png](http://upload-images.jianshu.io/upload_images/3001083-7e47ce5ee8ae01ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![3.2.43.png](http://upload-images.jianshu.io/upload_images/3001083-7d12ae48cc780cc9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

![3.2.44.png](http://upload-images.jianshu.io/upload_images/3001083-d3373d6d248965a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
**要想阻止行框围绕在浮动框的外边，需要对包含这些行框的元素应用clear属性。clear属性的值可以是left、right、both、none，它表示框的哪些边不应该挨着浮动框。**      
![3.2.45.png](http://upload-images.jianshu.io/upload_images/3001083-eeccfb39f4493a9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**在一些情况下，容器不包围浮动元素的时候，需要添加少量的额外标记，如用来进行清理的空div。**      
![3.2.46.png](http://upload-images.jianshu.io/upload_images/3001083-82d7e95e6cc79051.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
div
	div#test1.block-test.m-10.p-10.w-100.h-100.fs-30 1
	div#test4.m-10.p-10 lala
	div#clear   //- 用来进行清理的空div

#test4 {
	float: right;
}

#clear {
	clear: both;
```
**清理前：**      
![3.2.47.png](http://upload-images.jianshu.io/upload_images/3001083-0305bf492ecfc988.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**清理后：**      
![3.2.48.png](http://upload-images.jianshu.io/upload_images/3001083-85c5308609ef9799.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

#### overflow属性      
 - overflow 属性规定当内容溢出元素框时发生的事情。它可能的值为：      
 - visible	默认值。内容不会被修剪，会呈现在元素框之外。      
 - hidden	内容会被修剪，并且其余内容是不可见的。      
 - scroll	内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。      
 - auto	如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。      
 - inherit	规定应该从父元素继承 overflow 属性的值。      
      
如：      
![3.2.49.png](http://upload-images.jianshu.io/upload_images/3001083-50ca679930ac6512.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 小结      
 - 盒模型以及内边距、外边距、宽度和高度如何影响框的尺寸。      
 - 外边距的叠加。      
 - CSS 中的3中格式化模型：普通流、绝对定位和浮动。            
 - 行内框和块框的差异。      
 - 在相对定位的祖先元素中进行绝对定位。      
 - clear、overflow。      
