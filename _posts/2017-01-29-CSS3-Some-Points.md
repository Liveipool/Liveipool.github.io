---
layout: post
title: CSS3 杂记
date: 2017-01-29
categories: blog
description: CSS学习
---

# CSS3 杂记     
昨天把CSS1,2的文档看了一遍,今天把3的看了一遍.但注意,**在使用CSS3的特性时,不确定时要先查阅文档各浏览器对该属性的支持情况.**     

### CSS3 模块     
 - CSS3 被划分为模块。其中最重要的 CSS3 模块包括：     
 - 选择器     
 - 框模型     
 - 背景和边框     
 - 文本效果     
 - 2D/3D 转换     
 - 动画     
 - 多列布局     
 - 用户界面     

### background     
 - 在 CSS3 之前，背景图片的尺寸是由图片的实际尺寸决定的。在 CSS3 中，可以规定背景图片的尺寸，这就允许我们在不同的环境中重复使用背景图片。能够以像素或百分比规定尺寸。如果以百分比规定尺寸，那么尺寸相对于父元素的宽度和高度。      
 - background-origin 属性规定背景图片的定位区域。背景图片可以放置于 content-box、padding-box 或 border-box 区域。     
 - CSS3 允许您为元素使用多个背景图像。     

### 新的文本属性     
 - text-overflow	规定当文本溢出包含元素时发生的事情。	     
 - text-shadow	向文本添加阴影。	     
 - word-break	规定非中日韩文本的换行规则。	     
 - word-wrap	允许对长的不可分割的单词进行分割并换行到下一行。	     

### CSS3 字体 @font-face     
 - 在 CSS3 之前，web 设计师必须使用已在用户计算机上安装好的字体。通过 CSS3，web 设计师可以使用他们喜欢的任意字体。     
 - 当您找到或购买到希望使用的字体时，可将该字体文件存放到 web 服务器上，它会在需要时被自动下载到用户的计算机上。     
 - 您“自己的”的字体是在 CSS3 @font-face 规则中定义的。     
 - Firefox、Chrome、Safari 以及 Opera 支持 .ttf (True Type Fonts) 和 .otf (OpenType Fonts) 类型的字体。     
 - Internet Explorer 9+ 支持新的 @font-face 规则，但是仅支持 .eot 类型的字体 (Embedded OpenType)。     

在新的 @font-face 规则中，您必须首先定义字体的名称（比如 myFirstFont），然后指向该字体文件。     
如需为 HTML 元素使用字体，请通过 font-family 属性来引用字体的名称 (myFirstFont)：     
实例:     

```
<style> 
@font-face
{
font-family: myFirstFont;
src: url('Sansation_Light.ttf'),
     url('Sansation_Light.eot'); /* IE9+ */
}

div
{
font-family:myFirstFont;
}
</style>
```
     
### CSS3 转换     
 - 通过 CSS3 转换，我们能够对元素进行移动、缩放、转动、拉长或拉伸。     
 - 2D转换:      
 - translate(): 移动     
 - rotate(): 旋转     
 - scale(): 缩放     
 - skew(): 翻转     
 - matrix(): 把所有2D方法组合在一起     
 - 3D转换:     
 - rotateX()     
 - rotateY()     

### CSS3 过渡 transition     
 - 通过 CSS3，我们可以在不使用 Flash 动画或 JavaScript 的情况下，当元素从一种样式变换为另一种样式时为元素添加效果。     
 - CSS3 过渡是元素从一种样式逐渐改变为另一种的效果。     
 - 要实现这一点，必须规定两项内容：     
 - 规定您希望把效果添加到哪个 CSS 属性上     
 - 规定效果的时长     

 应用于宽度属性的过渡效果，时长为 2 秒：     

```
div
{
transition: width 2s;
-moz-transition: width 2s;	/* Firefox 4 */
-webkit-transition: width 2s;	/* Safari 和 Chrome */
-o-transition: width 2s;	/* Opera */
}
```


### CSS3 动画 animation       
 - 通过 CSS3，我们能够创建动画，这可以在许多网页中取代动画图片、Flash 动画以及 JavaScript。     
 - 如需在 CSS3 中创建动画，您需要学习 @keyframes 规则。@keyframes 规则用于创建动画。在 @keyframes 中规定某项 CSS 样式，就能创建由当前样式逐渐改为新样式的动画效果。     
 - 当您在 @keyframes 中创建动画时，请把它捆绑到某个选择器，否则不会产生动画效果。     
 - 通过规定至少以下两项 CSS3 动画属性，即可将动画绑定到选择器：     
 - 规定动画的名称     
 - 规定动画的时长     

把 "myfirst" 动画捆绑到 div 元素，时长：5 秒：     
```
div
{
animation: myfirst 5s;
-moz-animation: myfirst 5s;	/* Firefox */
-webkit-animation: myfirst 5s;	/* Safari 和 Chrome */
-o-animation: myfirst 5s;	/* Opera */
}

@keyframes myfirst
{
from {background: red;}
to {background: yellow;}
}

@-moz-keyframes myfirst /* Firefox */
{
from {background: red;}
to {background: yellow;}
}

@-webkit-keyframes myfirst /* Safari 和 Chrome */
{
from {background: red;}
to {background: yellow;}
}

@-o-keyframes myfirst /* Opera */
{
from {background: red;}
to {background: yellow;}
}
```

可以改变任意多的样式任意多的次数。     
请用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"，等同于 0% 和 100%。     
0% 是动画的开始，100% 是动画的完成。为了得到最佳的浏览器支持，您应该始终定义 0% 和 100% 选择器。     
也可以:     

```
@keyframes myfirst
{
0%   {background: red;}
25%  {background: yellow;}
50%  {background: blue;}
100% {background: green;}
}
```

### CSS3 多列     
 - 通过 CSS3，您能够创建多个列来对文本进行布局 - 就像报纸那样！     
 - column-count: 规定元素应该被分隔的列数.     
 - column-gap: 规定列之间的间隔.     
 - column-rule: 设置列之间的宽度、样式和颜色规则.     