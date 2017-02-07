---
layout: post
title: CSS 揭秘(2)--背景定位和条纹背景  
date: 2017-02-06
categories: blog
description: CSS学习
---


# 背景定位和条纹背景            

### 背景定位      
现代的背景定位可通过background-position、background-origin、calc()等属性或方法结合来使用产生效果。      
      
#### background-position      
 - background-position 属性设置背景图像的起始位置。      
 - 可能的值：      
 - top left、top center、top right、center left、center center、center right、bottom left、bottom center、bottom right（如果您仅规定了一个关键词，那么第二个值将是"center"。并且注意这种写法和后两种写法的水平垂直顺序不同）      
 - x% y%：	第一个值是水平位置，第二个值是垂直位置。左上角是 0% 0%。右下角是 100% 100%。如果您仅规定了一个值，另一个值将是 50%。      
 - xpos ypos	：第一个值是水平位置，第二个值是垂直位置。左上角是 0 0。单位是像素 (0px 0px) 或任何其他的 CSS 单位。      
 - **可以混合使用 % 或 pos 和 position 值。如： top 30px left 20 px;**      
- 注意，是在CSS3中才能混合使用这些参数值，因此项目若要面向旧版本的浏览器，要设置回退机制。      

#### background-origin      
 - background-origin 属性规定 background-position 属性相对于什么位置来定位。      
 - 可能的取值为： padding-box、border-box、content-box。（默认为padding-box）      

#### calc()方案      
calc()是CSS3中引入的一个函数，可以进行四则运算。      
如我们想把背景图片定位到距离底边10px，距离右边20px的位置，并且我们希望仍以左上角偏移的思路来思考，可以这么实现：      

```
background: url("lallana.png") no-repeat;
background-position: calc(100% - 20px) calc(100% - 10px);  //注意这里的顺序 100%-20是水平偏移量，后面一个是垂直偏移量。
```

**注意在calc()函数内部的 - 和 + 运算符两侧需要各加一个空白符，否则会产生解析错误。这个规则是为了向前兼容，未来，在calc()内部可能会允许使用关键字，而这些关键字可能会带有连字符。**      

### 条纹背景      

#### linear-gradient      
-  用线性渐变创建图像。      
-  参数值见文档[http://www.css88.com/book/css/values/image/linear-gradient().htm](http://www.css88.com/book/css/values/image/linear-gradient%28%29.htm)，以及下面的例子。      

#### repeating-linear-gradient      
 - 用重复的线性渐变创建图像。      
 - 参数值和linear-gradient类似。      

```
div {
	background: linear-gradient(#fb3, #58a);
}
```
![2.51.png](http://upload-images.jianshu.io/upload_images/3001083-7a6c4707ac9b4a4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
div {
	background: linear-gradient(#fb3 40%, #58a 60%);
}
```
![2.52.png](http://upload-images.jianshu.io/upload_images/3001083-f2363780b51d0b6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
div {
	background: linear-gradient(#fb3 50%, #58a 50%);
}
```
![2.53.png](http://upload-images.jianshu.io/upload_images/3001083-ad290f097cbf26c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
即，颜色后面的百分数的意思是，如容器顶部的40%去区域为#fb3纯色，容器底部的40%区域为#58a纯色，容器中间的20%为两种颜色的过度值。      
因此设为50%和50%时，相当于设置了两条巨大的水平条纹。      

```
div {
	background: linear-gradient(#fb3 30%, #58a 30%);
}
```
通过设置百分比还可以创建不等宽的条纹。
![2.54.png](http://upload-images.jianshu.io/upload_images/3001083-e8a03822508659d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

```
div {
	background: linear-gradient(#fb3 50%, #58a 50%);
	background-size: 100% 30px;
}
```
通过设置background-size来实现重复平铺的多条条纹。      
![2.55.png](http://upload-images.jianshu.io/upload_images/3001083-c12d3ecfa106d2a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

由于规范中说到：     
 
> 如果某个色表的位置值比整个列表中在它之前的位置值都要小，则该色标的位置值会被设置为它前面的所有色标值的最大值。      

因此上面的代码也可以写为：      

```
div {
	background: linear-gradient(#fb3 50%, #58a 0);
	background-size: 100% 30px;
}
```

![2.55.png](http://upload-images.jianshu.io/upload_images/3001083-c12d3ecfa106d2a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
要想创建多种颜色的条纹也是类似的道理：      

```
div {
	background: linear-gradient(#fb3 33.3%, #58a 0, #58a 66.6%, yellowgreen 0);
	background-size: 100% 45px;
}
```
![2.56.png](http://upload-images.jianshu.io/upload_images/3001083-b417706ba95a3962.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
可以通过加上参数”to right“来使条纹变为垂直方向（平时默认值为”to bottom“，也就是水平方向）：      

```
div {
	background: linear-gradient(to right, #fb3 50%, #58a 0);
}
```
![2.57.png](http://upload-images.jianshu.io/upload_images/3001083-accd746ba924fae3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
当然，有了水平（0deg）和垂直（90deg），我们也可以通过角度来创建斜向条纹：      

```
div {
	background: linear-gradient(45deg, #fb3 50%, #58a 0);
}
```
![2.58.png](http://upload-images.jianshu.io/upload_images/3001083-bd3c8a221d408ca1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
现在我们若想实现多条条纹平铺的效果，我们可以先用一种写法试验一下：      

```
div {
	background: linear-gradient(45deg, #fb3 50%, #58a 0);
	background-size: 30px 30px;
}
```
![2.59.png](http://upload-images.jianshu.io/upload_images/3001083-af36a355d64c58a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
可以发现，这样是行不通的。我们想要实现想要的效果，需要通过四条条纹而不是两条来实现无缝连接：      

```
div {
	background: linear-gradient(45deg, #fb3 25%, #58a 0, #58a 50%, #fb3 0, #fb3 75%, #58a 0);
	background-size: 30px 30px;
}
```
![2.510.png](http://upload-images.jianshu.io/upload_images/3001083-09ebfcc6bcd76eae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
然而，由于条纹为45度斜向，因此在此时，条纹的宽度实际上为 15除以根号2 px，因此若想要得到想要的条纹宽度，还需要根据角度进行一定计算。      
因为角度和条纹的宽度难以控制，因此这样的创建重复斜向条纹的方法并不太好，幸运的是，除了linear-gradient()和radial-gradient()（圆形式渐进），还各有一个循环式的加强版repeating-linear-gradient()和repeatinr-radial-gradient()。      如：      

```
div {
	background: repeating-linear-gradient(45deg, #fb3, #58a 30px);
}
```
![2.511.png](http://upload-images.jianshu.io/upload_images/3001083-9fc4aa0f3df2046d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
有了这个repeating-*方法，我们就可以随心所欲的改变宽度和角度了。      

```
div {
	background: repeating-linear-gradient(60deg, #fb3, #fb3 15px, #58a 0, #58a 30px);
}
```
![2.512.png](http://upload-images.jianshu.io/upload_images/3001083-606fc1f0df947b64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
在大多数情况下，我们想要的条纹图案并不是由差异极大的几种颜色组成的，这些颜色往往属于统一色系，只是在明度方面有着轻微的差异。比如：      
      
```
div {
	background: repeating-linear-gradient(30deg, #79b, #79b 15px, #58a 0, #58a 30px);
}
```
![2.513.png](http://upload-images.jianshu.io/upload_images/3001083-73b3c3fd50ce214b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
还有一种更好的方法实现上面的效果：不再为某种条纹单独指定颜色，而是把最深的颜色指定为背景色，同时把半透明白色的条纹叠加在背景色之上得到浅色条纹：      

```
div {
	background: #58a;
	background-image: repeating-linear-gradient(30deg, hsla(0, 0%, 100%, .1), hsla(0, 0%, 100%, .1) 15px, transparent 0, transparent 30px);  //transparent是全透明黑色(black)的速记法，即一个类似rgba(0,0,0,0)这样的值。
}
```
![2.513.png](http://upload-images.jianshu.io/upload_images/3001083-73b3c3fd50ce214b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      