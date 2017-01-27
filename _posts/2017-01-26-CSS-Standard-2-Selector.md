---
layout: post
title: CSS 标准决方案(2)--为样式找到应用目标
date: 2017-01-26
categories: blog
description: CSS学习
---

# 为样式找到应用目标     
在CSS中，执行这一任务的样式规则部分称为**选择器（selector）**。     

最常用的选择器类型是**类型选择器**和**后代选择器**。     
**类型选择器：**用来寻找特定类型的元素，比如段落或标题元素，只需指定希望应用样式的元素的名称。类型选择器有时也成为*元素选择器*。     

``` css
p {color: black;}
h1 {font-weight: bold;}
```

**后代选择器：**用来寻找特定元素或元素组的后代。后代选择器由两个选择器之间的空格表示。
如：只缩进是块引用的后代的段落元素，其他所有段落不受影响。     

``` css
blockquote p {padding-left: 2em};
```

**类型选择器**和**后代选择器**适用于哪些范围广的一般样式。要想寻找更特定的元素，可以使用**ID选择器**和**类选择器**。     
当在文档中添加了许多不必要的类时，可考虑将一个类或ID应用于它们的祖先，并通过后代选择器定位它们。     

### 伪类     
有时候，我们需要根据文档结构之外的其他条件对元素应用样式，例如表单元素或链接的状态，这要使用伪类选择器来完成。如：     

``` css
a:link, a:visited {color:black;}
tr:hover, div:focus, a:active {color:red;} 
```

：link和：visited称为链接伪类，只能应用于锚元素。：hover、：active和：focus称为动态伪类，理论上可以应用于任何元素。     

通过把伪类连接在以其，可以创建更复杂的行为，比如在已访问链接和未访问链接上实现不同的鼠标悬停效果：     

``` css
a:visited:hover {color: olive;}
```

### 高级选择器     
CSS 2.1和CSS 3有其他许多有用的选择器。     
如：**子选择器**就是一个高级选择器，后代选择器选择一个元素所有的后代，而子选择器只选择元素的直接后代，即*子元素*。     

``` 
<ul>
	<li>会变红</li>
	<li>
		<ul>
			<li>不会变红</li>
		</ul>
	</li>
</ul>

ul>li {color:red;}
```

*IE7中有一个小bug，如果父元素和子元素之间有HTML注释，就会出问题。*     

**相邻同胞选择器：**用于定位同一个父元素下某个元素之后的元素，如：     

``` css
h2 + p {color: red;}
```

### 层叠和特殊性     
![2.3.3.png](http://upload-images.jianshu.io/upload_images/3001083-6dddbe3633f91841.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

**特殊性：**     
![2.3.31.png](http://upload-images.jianshu.io/upload_images/3001083-85090a190b264dc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![2.3.32.png](http://upload-images.jianshu.io/upload_images/3001083-a4b6e9ec11c3effc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![2.3.33.png](http://upload-images.jianshu.io/upload_images/3001083-de761a8934ae9547.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

**在编写CSS时特殊性非常有用，因为它可以对一般元素应用一般样式，然后在更特殊的元素上覆盖它们。**     

### 继承     
**即：应用样式的元素的后代会继承样式的某些属性。**     

如果在主题上设置字号，你会注意到页面上的任何标题都没有采用这个样式。你可能会认为标题没有继承文本字号。但是，实际上是浏览器的默认样式表设置了标题字号。直接应用于元素的任何样式总会覆盖继承而来的样式，这是因为继承而来的样式的特殊性为空。     

正如恰当使用层叠可以简化CSS，恰当地使用继承也可以减少代码中选择器的数量和复杂性。但是，如果大量元素继承各种样式，那么判断样式的来源就会变得困难（还是可以通过开发者工具来查看）。     

### 对文档应用样式     
![2.4.11.png](http://upload-images.jianshu.io/upload_images/3001083-cbf1c631fe4acc2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

**注意，根据浏览器基准测试已经表明，导入样式表比链接样式表慢。**     
![2.4.12.png](http://upload-images.jianshu.io/upload_images/3001083-59f7e29546637159.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![2.4.13.png](http://upload-images.jianshu.io/upload_images/3001083-c4d61bc943cecdb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![2.4.14.png](http://upload-images.jianshu.io/upload_images/3001083-69621ecb3d7fd04e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

