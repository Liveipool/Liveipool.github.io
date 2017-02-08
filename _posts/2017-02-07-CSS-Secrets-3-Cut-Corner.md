---
layout: post
title: CSS 揭秘(3)--切角效果、梯形标签页   
date: 2017-02-07
categories: blog
description: CSS学习
---

# 切角效果、梯形标签页         

### 切角效果         
切角是当下非常流行的一种设计风格。         

第一种方案是**无所不能的CSS渐变。**以右下角为例。利用渐变的一大特性：渐变可以接收一个角度（如45deg）作为方向，并且色标的位置可以是绝对的长度值。         
因此，我们把一个透明色标放在切角处，然后在相同位置设置另一个色标，并且把它的颜色设置为我们想要的背景色，代码如下：         

```
div {
	background: #58a;
	background: linear-gradient(-45deg, transparent 80px, #58a 0);
}
```
![12.1.png](http://upload-images.jianshu.io/upload_images/3001083-2ea2f6d2c9f055bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
注意，第一行声明并不是必需的，加上它是将其作为**回退机制**，如果某些浏览器不支持渐变，那么第二行声明会被丢弃，而此时我们至少还能得到一个简单的纯色背景。         

现在，我想要在左下角再切一个角，于是我就再加上一行类似的代码：         

```
div {
	background: 
		linear-gradient(-45deg, transparent 40px, #58a 0),
		linear-gradient(45deg, transparent 40px, #655 0);
}
```
![12.2.png](http://upload-images.jianshu.io/upload_images/3001083-0153639c144d09c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
可显然，没有得到想要的结果，这是因为**默认情况下，这两层渐变都会填满整个元素，因此它们会相互覆盖。需要让它们都缩小一些，于是我们使用background-size让每层渐变分别只占据整个元素一半的面积。**         

```
div {
	background: 
		linear-gradient(-45deg, transparent 40px, #58a 0),
		linear-gradient(45deg, transparent 40px, #655 0);
	background-size: 50% 100%;  //意思为每个背景水平方向上占总长的50%，垂直方向上占100%。
}
```
![12.3.png](http://upload-images.jianshu.io/upload_images/3001083-b2261e38f2e04c6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
现在的效果依然和想要的结果有所不同，是因为由于background-repeat未被关掉，因此每层渐变图案各自平铺了两次，仍然相互覆盖，应该这么修改代码：         

```
div {
	background: 
		linear-gradient(-45deg, transparent 40px, #58a 0) right,
		linear-gradient(45deg, transparent 40px, #655 0) left;
	background-size: 50% 100%;
	background-repeat: no-repeat;
}
```
![12.4.png](http://upload-images.jianshu.io/upload_images/3001083-a7200ea4e1349d16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
上面这张图就是我们想要的效果了。         

因此，四角切角：         
         
```
div {
	background: 
		linear-gradient(135deg, transparent 40px, #58a 0) top left,
		linear-gradient(45deg, transparent 40px, #58a 0) bottom left,
		linear-gradient(-45deg, transparent 40px, #58a 0) bottom right,
		linear-gradient(-135deg, transparent 40px, #58a 0) top right;
	background-size: 50% 50%;
	background-repeat: no-repeat;
}
```
![12.5.png](http://upload-images.jianshu.io/upload_images/3001083-1ad7c8fbb6d69c65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
**弧形切角（内凹圆角）：**         

```
div {
	background: 
		radial-gradient(circle at top left, transparent 40px, #58a 0) top left,
		radial-gradient(circle at bottom left, transparent 40px, #58a 0) bottom left,
		radial-gradient(circle at bottom right, transparent 40px, #58a 0) bottom right,
		radial-gradient(circle at top right, transparent 40px, #58a 0) top right;
	background-size: 50% 50%;
	background-repeat: no-repeat;
}
```
![12.6.png](http://upload-images.jianshu.io/upload_images/3001083-a070bc821e8edbb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
同样，以后还可以使用clip-path来实现相近的结果。         

### 梯形标签页         
一直以来，梯形都是众所周知的难以生成的形状，有人通过仅有的before和after两个伪元素来在一个矩形前后各加一个三角形来实现梯形，但这种做法不值得称赞。         

一种解决方案是：在三维世界中，一个矩形看起来就像一个梯形一样，我们可以根据这个思路通过3D旋转来实现：         

```
div {
	color: #fff;
	background: #58a;
	transform: perspective(.5em) rotateX(5deg);
}
```
![13.1.png](http://upload-images.jianshu.io/upload_images/3001083-59c9103af8ae175a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
但是，熟悉的问题来了，上面的文字也变形了：对元素使用了3D变形之后，其内部的变形效应是“不可逆转”的（在2D变形的体系之下，内部的逆向变形可以抵消外部的变形效应）。         
因此，我们又可以使用伪元素来解决这个问题：         

```
div {
	color: #fff;	
	position: relative;
	display: inline-block;
	padding: .5em 1em .35em;
}
div::before {
	content: '';
	position: absolute;
	top: 0px;
	right: 0; 
	bottom: 0; 
	left: 0px;
	background: #58a;
	transform: perspective(.5em) rotateX(5deg);
	z-index: -1;
}
```
![13.2.png](http://upload-images.jianshu.io/upload_images/3001083-6ac1101b8b13bccc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

由于3D旋转是以自身的中心线为轴在空间上旋转，因此可能会导致它的尺寸难以进行控制，我们可以通过将transform-origin设为bottom来解决这个问题，即将轴定为元素的bottom，在头脑中想象一下，这样的旋转更好控制。         
旋转还会带来一个问题，就是会看起来比原来的大小要小一点，这时候我们再设置一个scale属性来弥补这种变化：         

```
div {
	color: #fff;	
	position: relative;
	display: inline-block;
	padding: .5em 1em .35em;
}
div::before {
	content: '';
	position: absolute;
	top: 0px;
	right: 0; 
	bottom: 0; 
	left: 0px;
	background: #58a;
	transform: scale(1.7) perspective(.5em) rotateX(5deg);  //注意变化
	z-index: -1;
	transform-origin: bottom;  //注意变化
}
```
![13.3.png](http://upload-images.jianshu.io/upload_images/3001083-e9e0e84206a63d47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         