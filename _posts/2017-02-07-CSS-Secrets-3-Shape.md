---
layout: post
title: CSS 揭秘(3)--椭圆、平行四边形、菱形   
date: 2017-02-07
categories: blog
description: CSS学习
---

# 椭圆、平行四边形、菱形         

### 自适应的椭圆         
通常我们希望，形状能够根据其内容自动调整并适应，如宽高相等时，就显示为一个圆;如果宽高不等，就显示为一个椭圆。         

#### border-radius         
 - border-radius 属性是一个简写属性，用于设置四个 border-*-radius 属性。         
 - 参数可通过长度或百分比来设置：         

```
// 顺序为 左上，右上，右下，左下
border-radius: 10px, 20px, 30px, 40px;
border-radius: 10%, 20%, 30%, 40%;
border-radius: 30px;  //四个位置都为30px
// 当为正方形时，border-radius达到长宽的一半，如（width = 100px， border-radius = 50px或50%），就变为圆形。
```

有了灵活的border-radius属性，我们可以通过一排代码就实现上面想要的效果（宽高相等时为圆，宽高不等时为椭圆）：         

```
div {
	background: #fb3;
	border: 1px solid black;
	border-radius: 50%;
}
```
![9.1.png](http://upload-images.jianshu.io/upload_images/3001083-f3db2bdb57118b6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

#### 半椭圆         
**border-radius有一个鲜为人知的真相：它可以单独指定水平半径和垂直半径，中间用一个（/）分隔这两个值即可。**         

```
border-radius: 100px / 75px;
```
我们可以利用分开写水平、垂直半径的写法来实现一个半椭圆形状。         
**还需要知道的是，border-radius是另外四个子属性的缩写，因此border-radius后面跟的参数也可以进行省略，即：如果只提供了三个值，则第四个值和第二个值相等;如果只提供了两个值，则第三个值和第一个相等。**         

```
border-radius: 10px / 5px 20px;
// 等同于
border-radius: 10px 10px 10px 10px / 5px 20px 5px 20px;
```
要想实现半椭圆，思路如下：         
1. 这个形状是垂直对称的，因此左上和右上角的半径值应该相同，左下和右下也相同。         
2. 顶部边缘没有平直的部分（也就是说，整个定变都是曲线），这意味着左上角和右上角的半径之和应该等于整个形状的宽度。         
3. 垂直方向上，顶部的两个圆角占据了整个元素的高度，而底部完全没有任何圆角，因此在垂直方向上border-radius的合理值为 100% 100% 0 0。         
4. 既然垂直方向上右下角和左下角的值为0,那么它们水平方向上的半径值也就无所谓了。         
         
因此最终代码为：         

```
div {
	background: #fb3;
	border: 1px solid black;
	border-radius: 50% / 100% 100% 0 0;
}
```
![9.2.png](http://upload-images.jianshu.io/upload_images/3001083-db669fa0f6a7a5fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

#### 四分之一圆角         
类似的，其中一个角的水平和垂直半径值都需要是100%，而其他三个角都不能设为圆角。         

```
div {
	background: #fb3;
	border: 1px solid black;
	border-radius: 100% 0 0 0;
}
```
![9.3.png](http://upload-images.jianshu.io/upload_images/3001083-7b5ec945cc23c3bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

### 平行四边形         

```
div {
	background: #fb3;
	transform: skewX(-45deg);
}
```
![10.1.png](http://upload-images.jianshu.io/upload_images/3001083-311e5e23ce8a9314.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
但是，这导致它的内容也发生了斜向变形，这很不好看，而且难读。有没有办法只让容器的形状倾斜，而保持其内容不变呢？         

一种思路就是**把所有样式（背景、边框等）应用到伪元素上，然后再对伪元素进行变形。**因为我们的内容并不是包含在伪元素内的，所以内容并不会受到变形的影响。         
读懂下面的代码：         

```
div {
	position: relative;
}

div::before {
	content: '';
	position: absolute;
	top: 0;
	right: 0;
	bottom: 0;
	left: 0;
	z-index: -1;
	background: #fb3;
	transform: skew(45deg);
}
```
![10.2.png](http://upload-images.jianshu.io/upload_images/3001083-3e25364a982b3369.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
这个技巧不仅可以用在skew()时， 还适用于其他任何变形样式，当我们想变形一个元素而不想变形它的内容时就可以用到它。         

### 菱形图片         
我们有时会想要将一张方形的图片显示为一张菱形图片的效果：         
![11.1.png](http://upload-images.jianshu.io/upload_images/3001083-218dadccd893c47d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![11.3.png](http://upload-images.jianshu.io/upload_images/3001083-59f9f0cb269aa1f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

```
div.w-342.h-342.m-200
	img.w-340.h-340(src="/static/pictures/08685344-11d9-426d-a204-ccabbf7049ba-2060x1236.jpeg") 
```

我们可能会想到使用rotate()方法，并将div和img分别以相反方向旋转45度，来试下效果：         

```
div {
	border: 1px solid red;
	transform: rotate(45deg);
	overflow: hidden;
}

div > img {
	max-width: 100%;
	transform: rotate(-45deg);
}
```
![11.2.png](http://upload-images.jianshu.io/upload_images/3001083-ed60605996b3ac09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

产生这种情况的问题在于max-width：100%这条声明，100%会被解析为div的边长，而实际上，我们想让图片的宽度与容器的对角线长度相等，而不是与边长长度。         
因此一种办法是我们将max-width再扩大1.42倍，即我们想将其设为142%。但这样并不科学，因为max-width很可能上限为100%。所以我们**需要用另外一种方式来将图片扩大1.42倍，这里就可以用到scale()了：**         

```
div {
	border: 1px solid red;
	transform: rotate(45deg);
	overflow: hidden;
}

div > img {
	max-width: 100%;
	transform: rotate(-45deg) scale(1.42);
}
```
![11.3.png](http://upload-images.jianshu.io/upload_images/3001083-59f9f0cb269aa1f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
上面的方法确实可以奏效，但它基本上就是一个hack。这个方法需要一层额外的HTML标签，这不够简洁，代码本身也不够直观。它也不够健壮，因为如果我们要处理一张非正方形的图片，就没办法使用上面这种方法了。         

有一个现代的666的 **clip-path** 属性，让我们可以轻松地将元素裁剪为我们想要的任何形状，而且较为简便，可读性好，唯一的劣处在于，至今浏览器的支持还不是太完善。         
![11.4.png](http://upload-images.jianshu.io/upload_images/3001083-ebc24e803a8440e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

而且它能够轻松地实现炫酷的动画，下面这段代码运行起来就相当简洁和炫酷（在Chrome上可以试效果）：         

```
img {
	clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);
	transition: 1s clip-path;
}

img:hover {
	clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
}
```
![11.5.png](http://upload-images.jianshu.io/upload_images/3001083-641a883e4a36c3e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         