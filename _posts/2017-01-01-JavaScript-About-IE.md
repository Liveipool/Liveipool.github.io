---
layout: post
title: IE中的那些坑
date: 2017-01-01
categories: blog
description: JavaScript学习
---

# IE中的那些坑      
 - 最近在学习DOM，由于不同浏览器支持DOM的程度不同，所以在这一部分内容中不同的浏览器采用的行为可能区别较大，就在几个主流浏览器上都试了一下。      
 - 以前对浏览器的版本了解不多，但一般还是听说比如IE8，IE9这样的，但看了一下最新的Chrome，Opera，Firefox的版本，真是吓了一跳，我最近下的时候，Chrome的版本是54.几，Firefox是50.1.0，Opera也出到了42.0.2393.94.不知它们仨是真的效率高，不停疯狂迭代，还是只是想把版本号弄高一点让人不明觉厉。      
 - 这三个浏览器使用后的感觉是，Chrome肯定是最接近标准的，但Opera和Chrome也非常像，几乎支持的所有的属性、方法等都相同。Firefox可能在个别属性有偏差，但总的来说，这三个浏览器新版本或者近几年的版本坑都不多。      
 - Safari 在Linux上用不了，Safari5.1.7是最后一个支持windows的版本，16年9月safari都出到版本10了，因此没有Mac所以也没办法试safari了，但大概看了一下官方文档上各属性的支持浏览器，觉得它的坑也不多。      
 - 最坑的当然还是IE，但其实现在已经改善了很多了，IE9将BOM对象和DOM对象都转换成了真正的JavaScript对象，从那以后到13年正式发布的IE11，坑也是减少了很多。      
 - 15年微软又推出了Edge，感觉这个浏览器要励志甩掉以前IE的坏名声。      
 - 总的感觉，随着语言的不断发展，真的是朝着越来越规范的方向在走，而这个规范，包括了W3C制定的各语言的规范以及浏览器新版本对规范的支持程度。      
 - 但是，对现在业界里的行情也不清楚，前段时间看到一个前段程序员吐槽，说现在能完全抛弃IE6就很开心了，不知是真是假，但至少还是有一部分人在用诸如IE8等的浏览器的，所以关于各浏览器的坑还是要有所了解，IE8以前主要的坑是在于它所有的DOM对象都是以COM对象的形式实现的，所以会有很多行为和活动特点不一致的情况，这篇博客就专门在windows下用IE踩一下这些坑。      
 - 我现在的win10系统也装不了IE那些小于等于8的版本了，但还好IE11有一个“仿真”，可以设置采用的文档模式，可以用来模拟运行在IE9， IE8的环境下。      


以下内容基本都是红皮书上所说的关于IE的部分，挨个试一下：      

**1. DOM1级定义了一个Node类型，该接口将由DOM中的所有节点类型实现。这个Node接口在JavaScript中是作为Node类型实现的。除了IE以外，在其他所有浏览器中都可以访问到这个类型。**      
实际上，IE上也是可以访问到的，即使是IE8及以前的版本，比如我输出一个Div节点nodeType、nodeName和nodeValue：      
![1.jpg](http://upload-images.jianshu.io/upload_images/3001083-8ff42f3e3abc31cd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      


**2. IE9以前的版本不会为空白符创建节点。**      

``` javascript
	<div id="lala"> </div>   //中间为一个空格符的div节点

	var node1 = document.getElementById("lala");
	console.log(node1.childNodes);
```
**Chrome下的输出：**      
![2.jpg](http://upload-images.jianshu.io/upload_images/3001083-194e491cc8409f41.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**Edge下的输出：**      
![3.jpg](http://upload-images.jianshu.io/upload_images/3001083-034d9b1e8abe7227.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**IE9下的输出：**      
![4.jpg](http://upload-images.jianshu.io/upload_images/3001083-0cd18541f510954f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**IE8下的输出：**      
![5.jpg](http://upload-images.jianshu.io/upload_images/3001083-80d71c22aad9bd5c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      


**3. IE9以前的版本document.doctype始终等于null。而Chrome，Edge，IE9等都会正常的输出如：< !DOCTYPE html > ，这是因为IE8及以前的版本会将其错误的将其解释为一个注释并当作comment节点。**      

**4. IE7及以前的版本时，不区分ID的大小写，如var div = document.getElementById("lala")中lala和LALA都可以找到该元素。**      

**5. IE7及以前的版本会有有一个怪癖：**      

``` javascript
	<input type="text" name="lala">
	<div id="lala"> </div>

	var node1 = document.getElementById("lala");  //node1找到的是input
```
即要是input的name特性等于指定的ID，就会返回那个input。      

**6. IE6及以前的版本不支持removeAttribute()。**      

**7. 在查看元素的子节点时，IE8及以前版本会和现在的各浏览器不同，如下面这段代码，我们查看ul的childNodes：**      
![7.jpg](http://upload-images.jianshu.io/upload_images/3001083-7f9d9038172d1dd8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

Chrome下输出：      
![6.jpg](http://upload-images.jianshu.io/upload_images/3001083-146c86132c894a54.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

IE9下输出：      
![8.jpg](http://upload-images.jianshu.io/upload_images/3001083-cd1ec6b0231890f4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

IE8下：      
![9.jpg](http://upload-images.jianshu.io/upload_images/3001083-f0cdaf464b2832af.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
可以看见，除了IE8及以前版本只有3个子节点，其余ul都是有7个子节点，3个li节点和4个文本节点。      

**8. 所有IE版本和Edge都不支持matches()方法。**      

**9. 从IE9起才开始支持getElementsByClassName()方法。**      
      
**10. 从IE9起才开始支持document.head属性。**      

**11. IE8及以前的版本的innerHTML属性会将所有标签都转换为大写形式。**      
![10.jpg](http://upload-images.jianshu.io/upload_images/3001083-6638fc9755dddc6d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**12 IE8及更早版本中的范围：**      
虽然IE9支持“DOM范围”，但IE8及之前版本不支持DOM范围。不过，IE8及早期版本支持一种类似的概念：文本范围（text range）。文本范围是IE专有的特性。顾名思义，文本范围处理的主要是文本（不一定是DOM节点）。通过< body > , < button >, < input >, < textarea >等几个元素，可以调用createTextRange()来创建文本范围，如：var range = document.body.createTextRange().      

以上的内容是红皮书里提到的一些IE和其他浏览器不同的地方，但这肯定不是全部，去将官方文档都看一遍也不现实，最好还是养成习惯，以后遇到较新的属性和方法时，去官网上看一看哪些浏览器支持。并且我觉得，就在不远的将来，就不用再考虑IE8了，到了那时，程序员们就能体会到更多前端的乐趣了吧。        



