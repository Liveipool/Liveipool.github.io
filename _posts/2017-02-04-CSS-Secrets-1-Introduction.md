---
layout: post
title: CSS 揭秘(1)--引言  
date: 2017-02-04
categories: blog
description: CSS学习
---

# CSS 揭秘(1)--引言      
 - CSS从一门极度简单、功能有限的样式语言，发展成为一项由80多项W3C规范（含草案）所定义的复杂技术，并建立起了独有的开发者生态圈、专属的技术会议、专用的框架和工具链。      
 - [可用来查找html、css、js的各属性、方法的及时有效的浏览器兼容信息的网站：http://caniuse.com](http://caniuse.com/#)      

当不同的浏览器的表现有差异时，即某个属性可能需要一个浏览器前缀的时候，应该使用各种不同的语法，并且通过层叠机制来确保哪条声明最终生效，因此，**应该把标准语法排在最后**，如想要得到一条从黄色到红色的垂直渐变色：      

``` css
background: -moz-linear-gradient(0deg, yellow, red);
background: -o-linear-gradient(0deg, yellow, red);
background: -webkit-linear-gradient(0deg, yellow, red);
background: linear-gradient(0deg, yellow, red);
```

> 标准就像香肠，最好别去看它们是怎么做出来的。      

> W3C并不“生产“标准。实际上，它扮演的是一个论坛的角色：W3C以工作组的方式，把某项技术的相关各方聚集起来，最终由他们来产出标准。当然，W3C并不只是一个观察者，它设定了整个平台的规则，监督整个进程。但这些技术规范（基本上）并不是由W3C的工作人员编写完成的。      

### em      
一个em表示一种特殊字体的大写字母M的高度。在网页上，一个em是网页浏览器的基础文本尺寸的高度，它一般情况下是16px。然而，任何人都可以改变这个基础尺寸的设置，因此1em对于有的人来说可能是16px，但是在其他人的浏览器上可能是24px。换句话说，em是一个相对的度量单位。      
**在适当的情况下使用em，可以使得效果变为响应式。**      

### @supports      

```
@supports (rule)[operator (rule)]* { sRules }
operator：or | and | not
```
rule：指定一条具体的CSS规则，必须使用括号包裹      
operator：使用or 或 and 或 not等操作符指定多条规则。      
说明：检测是否支持某CSS特性，定义支持和不支持flex标准写法的浏览器分别使用不同的规则：      
示例代码：      

``` css
@supports ( display: flex ) {
	body {
		display: flex;
	}
	#aside {
		width: 210px;
	}
	#main {
		flex: auto;
	}
}

@supports not ( display: flex ) {
	#aside {
		float: left;
		width: 210px;
	}
	#main {
		overflow: hidden;
		*zoom: 1;
	}
}
```
我们可以通过类似这样的写法来给不同的终端使用差异化的方案。      
也可以写多重规则来进行过滤：      
示例代码：      

``` css
@supports ( box-shadow: 2px 2px ) or
          ( -moz-box-shadow: 2px 2px ) or
          ( -webkit-box-shadow: 2px 2px ) {
	.demo {
		-moz-box-shadow: 2px 2px 0 rgba(0, 0, 0, .3);
		-webkit-box-shadow: 2px 2px 0 rgba(0, 0, 0, .3);
		box-shadow: 2px 2px 0 rgba(0, 0, 0, .3);
	}
}
```


### currentColor      
currentColor是CSS中有史以来第一个变量。它并没有绑定到某个固定的颜色值，而是一直被解析为color。      
      
### 相信眼睛，而不是数字      
 - 如：同样宽高的正方形看起来比圆形大一些。            
 - 如：设置相同padding但看起来上下的距离和左右不同。      
 - 这些是觉上的错觉在任何形式的视觉设计中都普遍存在，需要通过一些来使效果更好。      