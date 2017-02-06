---
layout: post
title: CSS 揭秘(2)--半透明边框和多重边框  
date: 2017-02-06
categories: blog
description: CSS学习
---

# 半透明边框和多重边框            

### 回退机制      
回退机制是指在某项技术的浏览器支持情况并不那么良好时,可以做到平稳退化,这样可以确保网站不会在低版本的浏览器中挂掉.      

### HSLA(H,S,L,A)      
 - H：Hue(色调)。0(或360)表示红色，120表示绿色，240表示蓝色，也可取其他数值来指定颜色。取值为：0 - 360      
 - S：Saturation(饱和度)。取值为：0.0% - 100.0%      
 - L：Lightness(亮度)。取值为：0.0% - 100.0%      
 - A：Alpha透明度。取值0~1之间。      

### 半透明边框      
假设我们想给一个容器添加一层白色背景和一个半透明白色边框,我们可能会这么写:      

```
div.w-200.h-200.m-20 lalala

body {
	background-color: #3399FF !important;
}

div {
	border:10px solid hsla(0, 0%, 100%, .5);
	background: white;
}
```
但是出来的效果是这样:      
![2.11 半透明边框.png](http://upload-images.jianshu.io/upload_images/3001083-81156c89d9dfca12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
**这是因为,默认情况下,背景会延伸到边框所在的区域下层,因此边框区域的白色背景会从透明的边框中透出来导致看起来就是白色边框.**      

在CSS3,新增了background-clip属性,通过这个属性,我们可以让背景只在某个区域中,我们将其设定在padding-box中即可看出效果:      

```
// ...
div {
	border:10px solid hsla(0, 0%, 100%, .5);
	background: white;
	background-clip: padding-box;
}
```
![2.12.png](http://upload-images.jianshu.io/upload_images/3001083-c1b26dc4e5d06659.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 多重边框      
**可以通过 box-shadow 或者 outline 来实现多重边框.**      

#### box-shadow方案      
 - box-shadow 属性向框添加一个或多个阴影。      
 - box-shadow: h-shadow v-shadow blur spread color inset;      
 - h-shadow	必需。水平阴影的位置。允许负值。      
 - v-shadow	必需。垂直阴影的位置。允许负值。      
 - blur	可选。模糊距离。      
 - spread	可选。阴影的尺寸。      
 - color	可选。阴影的颜色。请参阅 CSS 颜色值。      
 - inset	可选。将外部阴影 (outset) 改为内部阴影。      

可通过box-shadow的第四个spread属性,让投影面积增大或减小.一个正值的扩张半径加上两个为零的偏移量以及为零的模糊值,得到的"投影"其实就像一道实线边框:      

```
div {
	background-color: yellowgreen;
	box-shadow: 0 0 0 10px #655;
}
```
![2.21.png](http://upload-images.jianshu.io/upload_images/3001083-c67f121cbe60b2c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
**box-shadow的好处在于,它支持逗号分隔语法,我们可以创建任意数量的投影,以此来实现多重边框:**      

```
div {
	background-color: yellowgreen;
	box-shadow: 0 0 0 10px #655, 0 0 0 15px deeppink;
}
```
![2.22.png](http://upload-images.jianshu.io/upload_images/3001083-046d916a334c7903.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
需要注意的是,投影的行为跟边框不完全一致,因为它不会影响布局,而且也不会受到box-sizing属性的影响.不过,还是可以通过内边距或外边距（这取决于投影是outset还是inset）来模拟处边框所需要占据的空间。      
还有一点，上述方法所创建出的假“边框”出现在元素的外圈。他们并不会响应鼠标事件，比如悬停或点击。      

#### outline方案      
 - outline （轮廓）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。      
 - 可以按顺序设置如下属性（省略其中的某个值也是可以的）：      
 - outline-width      
 - outline-style                  
 - outline-color       

因此多重边框还可以通过下面这种方法来创建：      

```
div {
	background-color: yellowgreen;
	border: #655 solid 10px; 
	outline: solid 5px deeppink;
}
```
![2.22.png](http://upload-images.jianshu.io/upload_images/3001083-046d916a334c7903.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

在使用outline时，有一个地方需要注意：**边框不一定会贴合border-radius产生的圆角（见下面的例子）。这种行为也被CSS工作组认为是一个bug，因此未来可能会改为贴合border-radius圆角。**      
若想填补圆角和outline间的空隙，可通过box-shadow来实现，不过box-shadow的尺寸大小等需要进行计算。      

```
div {
	background-color: yellowgreen;
	border-radius: 30%;	
	outline: deeppink solid 5px;
}
```
![2.23.png](http://upload-images.jianshu.io/upload_images/3001083-78e7a24bb1322041.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      