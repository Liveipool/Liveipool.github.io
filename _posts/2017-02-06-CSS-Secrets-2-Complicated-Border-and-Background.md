---
layout: post
title: CSS 揭秘(2)--复杂的背景图案和连续的图像边框  
date: 2017-02-06
categories: blog
description: CSS学习
---


# 复杂的背景图案和连续的图像边框        

### 复杂的背景图案      

#### 网格      
只使用一个渐变时，能创建的图案并不多。当我们把多个渐变图案组合起来，如把水平和垂直的条纹叠加起来，就可以得到各种样式的网格。      

```
div {
	background: #fff;
	border: 1px solid black;
	background-image: 
		linear-gradient(90deg, rgba(200, 0, 0, .5) 50%, transparent 0),
		linear-gradient(rgba(200, 0, 0, 0.5) 50%, transparent 0);
}
```
![2.6.1.png](http://upload-images.jianshu.io/upload_images/3001083-466a31a442bc9580.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
在某些情况下，我们希望网格中的每个格子的大小可以调整，而网格线条的粗细同时保持固定。这时就需要使用长度而不是百分比：      

```
div {
	background: #58a;
	border: 1px solid black;
	background-image: 
		linear-gradient(90deg, #fff 1px, transparent 0),
		linear-gradient(#fff 1px, transparent 0);
	background-size: 30px 30px;
}
```
![2.6.2.png](http://upload-images.jianshu.io/upload_images/3001083-0cbb9173078a9c5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

#### 波点      
径向渐变同样也是非常实用的，因为它允许我们创建圆形、椭圆。      

```
div {
	background: #655;
	background-image: radial-gradient(tan 30%, transparent 0);
	background-size: 30px, 30px;
}
```
![2.6.3.png](http://upload-images.jianshu.io/upload_images/3001083-79dd7115f5d95cde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 伪随机背景      
> 重复平铺的几何图案很美观，但看起来可能有一些代办。其实自然界中的事物都不是义无限平铺的方式存在的。即使重复，也往往伴随着多样性和随机性。      

现在假设我们想要得到不同颜色和宽度的垂直条纹，并且不让人看出平铺时的接缝：      

```
div {
	background: linear-gradient(90deg, #fb3 15%, #655 0, #655 40%, #ab4 0, #ab4 65%, hsl(20, 40%, 90%) 0);
	background-size: 80px 100%;
}
```
![2.7.1.png](http://upload-images.jianshu.io/upload_images/3001083-b9b457f9c6f19f96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
为了更真实地模拟条纹的随机性，我们可以把这组条纹从一个平面拆散为多个图层：一种颜色作为底色，另三种颜色作为条纹，然后再让条纹以不同的间隔进行重复平铺：      

```
div {
	background: hsl(20, 40%, 90%);
	background-image: 
		linear-gradient(90deg, #fb3 10px, transparent 0),
		linear-gradient(90deg, #ab4 20px, transparent 0),
		linear-gradient(90deg, #655 20px, transparent 0);
	background-size: 80px 100%, 60px 100%, 40px 100%;
}
```
![2.7.2.png](http://upload-images.jianshu.io/upload_images/3001083-855f84ad55375be6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
这样的图片看起来比较有随机的感觉，但实际上，图案每隔240px就会重复一次，240px也就是每个贴片的长度，而这240px，是80px、60px和40px的最小公倍数。      
因此为了使每个贴片的尺寸最大，我们最好将最小公倍数最大化，也就是说，最好选一些互为”相对质数“的数字，如：      

```
div {
	background: hsl(20, 40%, 90%);
	background-image: 
		linear-gradient(90deg, #fb3 10px, transparent 0),
		linear-gradient(90deg, #ab4 20px, transparent 0),
		linear-gradient(90deg, #655 20px, transparent 0);
	background-size: 83px 100%, 61px 100%, 41px 100%;
}
```
![2.7.3.png](http://upload-images.jianshu.io/upload_images/3001083-b4a1cdba9a0478b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
这种通过质数来增加随机真实性的技巧，也被称为”蝉原则“。      

### 连续的图像边框       
有时我们想把一幅图案应用为边框，而不是背景。并且，我们还希望这个元素的尺寸在扩大或缩小时，这幅图片都可以自动延伸并覆盖完整的边框区域。      
说到图案和边框，我们可能会想到border-image，但实际上，并不能通过border-image实现我们想要的效果。      
![2.8.1.png](http://upload-images.jianshu.io/upload_images/3001083-4e2ecc85bc68de42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
border-image的原理基本上是九宫格伸缩法：把图片切成九块，然后把它们应用到元素边框相应的边和角。总的来说，它不够灵活。      

需要实现我们想要的效果，思路为：**在一张背景图上，再叠加一层纯白的实色背景。为了让下层的图片背景透过边框区域显示出来，我们需要给两层背景指定不同background-clip值。最后一个要点在于，我们只能在多重背景的最底层设置背景色，因此需要用一道从白色过渡到白色的CSS渐变来模拟出纯白实色背景的效果：**      

```
div {
	padding: 1em;	
	border: 1em solid transparent;
	background:
		linear-gradient(white, white), 
		url("/static/pictures/08685344-11d9-426d-a204-ccabbf7049ba-2060x1236.jpeg");
	background-size: cover;
	background-clip: padding-box, border-box;
}
```
![2.8.2.png](http://upload-images.jianshu.io/upload_images/3001083-93e015654af62506.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
上面这张图比较接近我们想要的效果，但边框的图片有一种奇怪的拼接效果。原因是：background-origin的默认值是padding-box，因此，图片的显示尺寸不仅取决于padding box的尺寸，而且被放置在了padding box的原点（左上角）。我们看到的实际上就是背景图片以平铺的方式蔓延到border box区域的效果。为了修正这一问题，我们只需把background-origin也设置为border-box就行了：      

```
div {
	padding: 1em;	
	border: 1em solid transparent;
	background:
		linear-gradient(white, white), 
		url("/static/pictures/08685344-11d9-426d-a204-ccabbf7049ba-2060x1236.jpeg");
	background-size: cover;
	background-clip: padding-box, border-box;  //background-clip：规定背景的绘制区域。
	background-origin: border-box; //background-origin：规定背景图片的定位区域。
}
```
![2.8.3.png](http://upload-images.jianshu.io/upload_images/3001083-d5852f9acf275a6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

上面代码还可简写为：      

```
div {
	padding: 1em;	
	border: 1em solid transparent;
	background:
		linear-gradient(white, white) padding-box, 
		url("/static/pictures/08685344-11d9-426d-a204-ccabbf7049ba-2060x1236.jpeg") border-box 0 / cover;
}
``` 
