---
layout: post
title: CSS 揭秘(3)--简单的饼图   
date: 2017-02-07
categories: blog
description: CSS学习
---

# 简单的饼图         
饼图在网页中的运用极为普遍，比如简单的统计图表，进度指示器，定时器等。         

基础形状：         

```
div.pie.w-200.h-200.m-200


.pie {
	border-radius: 50%;
	background: yellowgreen;
}
```
![14.1.png](http://upload-images.jianshu.io/upload_images/3001083-443875ccad78d949.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
假设我们现在的需求是一个最简单的饼图，其展示的比率为20%。         
我们现在的思路是：**把圆形的左右两部分指定为对应的两种颜色，然后用伪元素覆盖上去，通过旋转决定露出多大的扇区。**         

```
.pie {
	border-radius: 50%;
	background: yellowgreen;
	background-image: linear-gradient(to right, transparent 50%, #655 0);
}
```
![14.2.png](http://upload-images.jianshu.io/upload_images/3001083-24565edbcc529e3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

```
.pie {
	border-radius: 50%;
	background: yellowgreen;
	background-image: linear-gradient(to right, transparent 50%, #655 0);
}
.pie::before {
	content: '';
	display: block;
	margin-left: 50%;
	height: 100%;
	border: 1px dotted black;
}
```
![14.3.png](http://upload-images.jianshu.io/upload_images/3001083-3441802e63786221.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
可以看到，现在的伪元素选取到了整个饼图的右半部分，只是现在伪元素区域还没设置样式，暂时起不到遮盖的作用。
对于伪元素：         
1. 我们希望它能够遮盖圆形中指定大小的棕色部分，因此应该给它指定绿色背景。这里可以使用background-color： inherit来避免代码的重复。         
2. 我们希望它是围绕着圆形的圆心来旋转的，对它自己来说，这个点就是它左边缘的中心点。因此，我们应该把transform-origin设为0 50%或left。         
3. 我们不希望它呈现出矩形的形状，否则它会突破整个饼图的圆形范围。因此要么给.pie设置overflow：hidden的样式，要么给它合适的border-radius属性来把它变成一个半圆。         

```         
.pie::before {
	content: '';
	display: block;
	margin-left: 50%;
	height: 100%;
	border-radius: 0 100% 100% 0 / 50%;
	background-color: inherit;  // 不能是background：inherit，因为这样渐变背景也会被继承过来。
	transform-origin: left;
	border: 1px dotted black;
}
```
![14.4.png](http://upload-images.jianshu.io/upload_images/3001083-2183cbde9777cff6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
下面，我们就可以通过一个rotate()变形属性来让这个伪元素旋转起来。如果我们要显示出20%的比率，我们可以指定旋转的值为72deg（0.2 × 360）或 .2turn。         

```
.pie::before {
	content: '';
	display: block;
	margin-left: 50%;
	height: 100%;
	border-radius: 0 100% 100% 0 / 50%;
	background-color: inherit;
	transform-origin: left;
	transform: rotate(.2turn);
}
```
下面三张图分别为.2turn、.4turn、.6turn时的效果：         
![14.5.png](http://upload-images.jianshu.io/upload_images/3001083-9d8558830bb9b729.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![14.6.png](http://upload-images.jianshu.io/upload_images/3001083-3161ad70ab3090d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![14.7.png](http://upload-images.jianshu.io/upload_images/3001083-da6529c435d51b5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
可以看到，当比率大于.5turn时就会出现问题！！         

如果把50% ～ 100%的比率看作另外一个问题，我们就会发现，可以使用上述技巧的一个反向版本来实现这个范围内的比率：设置一个棕色的伪元素，让它在0到.5turn范围内旋转：         

```
.pie::before {
	content: '';
	display: block;
	margin-left: 50%;
	height: 100%;
	border-radius: 0 100% 100% 0 / 50%;
	background-color: #655;  // 这里变为了棕色
	transform-origin: left;
	transform: rotate(.1turn);
}
```
下面就是正确的60%饼图：         
![14.8.png](http://upload-images.jianshu.io/upload_images/3001083-e7d616aa2e0bd8b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
由于已经找到了实现任意比率的方法，我们可以用CSS动画来实现一个饼图从0变化到100%的动画，从而得到一个炫酷的进度指示器：         

```
@keyframes spin {
	to {transform: rotate(.5turn);}
}

@keyframes bg {
	50% { background: #655; }
}
.pie {
	border-radius: 50%;
	background: yellowgreen;
	background-image: linear-gradient(to right, transparent 50%, #655 0);
}
.pie::before {
	content: '';
	display: block;
	margin-left: 50%;
	height: 100%;
	border-radius: 0 100% 100% 0 / 50%;
	background-color: inherit;
	transform-origin: left;
	animation: 
		spin 3s linear infinite,
		bg 6s step-end infinite;
}
```
这个动画的效果很棒，但我们要怎样才能制作出多个不同比率的静态饼图呢？         
理想情况下，我们希望可以用这样的方式来书写结构：         

```
<div class="pie">20%<div>
<div class="pie">60%<div>
```
然后就能得到两个饼图，一个20%，一个60%。         
首先，我们需要探索如何用内联样式来实现这个需求。接下来，我们就可以写一小段脚本来解析文本内容并把内联样式添加到元素上去，以实现**代码的优雅性、封装抽象度、可维护性，**以及最重要的一点--**可访问性。**。         

思路为：**利用上面的那个动画，不过将动画设置为暂停状态，然后通过负的动画延时来直接跳至动画中的任意时间点。**         
关于负的延时的官方解释：         

> 一个负的延时值是合法的。与0s的延时类似，它意味着动画会立即开始播放，但会自动前进到延时值的绝对值处，就好象动画过去已经播放了指定的时间一样。因此实际效果就是动画跳过指定时间而从中间开始播放了。         

根据这个原理，我们在animation-delay中设置的值占整个动画持续时间的比率就是所占的比率，即总长6s的动画，animation-delay设为-1.2s就是显示出20%的比率，为了方便设置，后面我们将动画总持续时间设为100s，由于动画是暂停状态，所以修改持续时长也不会有什么副作用。         

动画是作用在伪元素上的，但我们希望最终内联样式可以设置在.pie元素上。不过，由于< div >没有任何动画效果，我们可以用内联样式的方式为其设置animation-delay属性，然后再在伪元素上应用animation-delay：inherit属性。         
综合以上要素，如果要让饼图显示20%和60%，则代码结构为：         

```
// jade
div.pie.w-200.h-200.m-100(style="animation-delay: -20s")
div.pie.w-200.h-200.m-100(style="animation-delay: -60s")

// css
@keyframes spin {
	to {transform: rotate(.5turn);}
}

@keyframes bg {
	50% { background: #655; }
}
.pie {
	border-radius: 50%;
	background: yellowgreen;
	background-image: linear-gradient(to right, transparent 50%, #655 0);
}
.pie::before {
	content: '';
	display: block;
	margin-left: 50%;
	height: 100%;
	border-radius: 0 100% 100% 0 / 50%;
	background-color: inherit;
	transform-origin: left;
	animation: 
		spin 50s linear infinite,
		bg 100s step-end infinite;
	animation-delay: inherit;
	animation-play-state: paused;
}
```
![14.9.png](http://upload-images.jianshu.io/upload_images/3001083-2b853b5cd90101f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

此时，我们就可以再次优化结构，把饼图的比率写到元素的内容中，就像我们最开始所期望的那样。然后通过一段简单的脚本来把animation-delay写到内联样式中：         

```
// jade
div.pie.w-200.h-200.m-100.fs-30() 20%
div.pie.w-200.h-200.m-100.fs-30() 60%

// js
var divs = document.getElementsByTagName("div");
for (var division of divs) {
	var delay = parseFloat(division.textContent);
	division.style.animationDelay = '-' + delay + 's';
}
```
![14.9.png](http://upload-images.jianshu.io/upload_images/3001083-2b853b5cd90101f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
