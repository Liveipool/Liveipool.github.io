---
layout: post
title: JavaScript红皮书学习(10)--DOM1
date: 2016-12-31
categories: blog
description: JavaScript学习
---

# DOM1         
DOM(文档对象模型)是针对HTML文档的一个API. DOM描绘了一个层次化的节点树,允许开发人员添加,移除和修改页面的某一部分.DOM脱胎于Netscape及微软公司创始的DHTML(动态HTML),现在它已经成为表现和操作页面标记的真正的跨平台,语言中立的方式.         
1998年10月DOM1级规范成为W3C的推荐标准,为基本的文档结构及查询提供了接口.IE, Firefox, Safari, Chrome和Opera都非常完善地实现了DOM.         

注意,巨坑的IE在IE8及以前的版本中,它所有的DOM对象都是以COM对象的形式实现的.这意味着那些版本的IE中的DOM对象与原生JavaScript对象的行为或活动特点并不一致.这个差异较多需要好好总结并实际操作(IE9开始把BOM和DOM对象都转换成了真正的JavaScript对象).         

## 10.1 节点层次         
DOM可以将任何HTML文档描绘成一个由多层节点构成的结构.         
如:         
![10.11.png](http://upload-images.jianshu.io/upload_images/3001083-ee60d75646ece4cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.12.png](http://upload-images.jianshu.io/upload_images/3001083-6d3d62c2ae718995.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

即:         
1. Document节点称为文档节点,是每个文档的根节点,通过document来访问.         
2. Element html节点称为文档元素,通过document.documentElement来访问.         
3. Element head节点通过document.head来访问.         
4. Element body节点通过document.body来访问.         

### 10.1.1 Node类型         
JavaScript中的所有节点类型都继承自Node类型,因此所有节点类型都共享着相同的基本属性和方法.         
每个节点都有一各nodeType属性,用于表明节点的类型.节点类型由在Node类型中定义的下列12个数值常量来表示,任何节点类型必居其一:         

![10.1.11.png](http://upload-images.jianshu.io/upload_images/3001083-bbd0c5b2b3e772ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.12.png](http://upload-images.jianshu.io/upload_images/3001083-59708b101c32bb0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

(第四种节点,Node.CDATA_SECTION_NODE)是只针对基于XML的文档的,可忽略)         
通过比较上面的常量,很容易确定节点的类型(为了确保跨浏览器兼容,最好还是将nodeType属性与数字进行比较):         

``` javascript
if (someNode.nodeType == 1) {
	alert("this node is an element");
}
```
并不是所有节点类型都受到Web浏览器的支持.开发人员最常用的就是元素和文本节点.         

![10.1.13.png](http://upload-images.jianshu.io/upload_images/3001083-84132c0dfdb0aed9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.14.png](http://upload-images.jianshu.io/upload_images/3001083-e3f1dd1a4847646a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

**注意这个NodeList对象,很重要,需要记住它是动态的!**还要注意length属性表示的是访问NodeList的那一刻,其中包含的节点数量.

![10.1.15.png](http://upload-images.jianshu.io/upload_images/3001083-e127d03829e8e9cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.16.png](http://upload-images.jianshu.io/upload_images/3001083-5374ae81204711da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.17.png](http://upload-images.jianshu.io/upload_images/3001083-cb7a43759dd56a53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

**操作节点一共有四种基本方法:**         
1. 上面提到的appendChild().         
2. insertBefore(). 两个参数:要插入的节点和作为参照的节点.         
3. replaceChild(). 两个参数:新的节点和要被替换掉的节点.         
4. removeChild(). 一个参数:想要移除的节点.         
通过replaceChild()和removeChild()方法移除的节点**仍然为文档所有**,只不过在文档中已经没有了自己的位置.         
这四个方法操作的都是某个节点的字节点,也就是说,要使用这几个方法必须先取得父节点.另外并不是所有类型的节点都可以有字节点,如果在不支持字节点的节点上调用了这些方法,将会导致错误发生.         

![10.1.18.png](http://upload-images.jianshu.io/upload_images/3001083-ae22d33cc929048c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.19.png](http://upload-images.jianshu.io/upload_images/3001083-a26e2ffef471cb06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.20.png](http://upload-images.jianshu.io/upload_images/3001083-8f426c1a42cac6d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

### 10.1.2 Document类型         
![10.1.21.png](http://upload-images.jianshu.io/upload_images/3001083-4413b96b3f158f5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

通过document对象,不仅可以取得与页面有关的信息,还能操作页面的外观及其底层结构.         

**1.文档的子节点:**         
1. document.html         
2. document.head         
3. document.body         
4. document.doctype         
5. document.comment         
6. ...         
它们其中的一些节点(如4, 5)在老版本的一些浏览器上的支持会有区别,这个遇到时去查看文档.         

**2.文档信息:**         
作为HTMLDocument的一个实例,document对象还有一些标准的Document对象没有的属性.         
这些属性提供了document对象所表现的网页的一些信息.有:         
1. document.title         
2. document.URL: 包含页面完整的URL         
3. document.domai: 包含页面的域名         
4. document.referrer: 保存着链接到当前页面的那个页面的URL         
2,3,4属性都存在与请求的HTTP头部,只不过是通过这些属性让我们能够在JavaScript中访问它们而已,一般来说这四个属性都只是用来读的,需要改时再去查看具体的用法.         

**3.查找元素**         
说到最常见的DOM应用,恐怕就要数取得特定的某个或某组元素的引用,然后再执行一些操作了.取得元素的操作可以使用document对象的几个方法来完成:         
1. getElementById(): 若未找到,则返回null,注意接收的参数id必须与页面中的元素的id特性(attribute)严格匹配,包括大小写.         
2. getElementsByTagName().         
3. getElementsByName().         

getElementsByTagName()方法接收一个参数:要取得的元素的标签名,而返回的是包含零或多个元素的NodeList.在HTML文档中,这个方法会返回一个HTMLCollection对象,作为一个"动态集合",该对象与NodeList非常相似.         
如: var images = document.getElementsByTagName("img");   取得页面中所有的< img >元素,并返回一个HTMLCollection对象.访问这个对象中的属性的方法和NodeList相同,item()或[].         

![10.1.22.png](http://upload-images.jianshu.io/upload_images/3001083-b4d04033f6b84565.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.23.png](http://upload-images.jianshu.io/upload_images/3001083-d072962721027b28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.24.png](http://upload-images.jianshu.io/upload_images/3001083-f45ac884e1fa82b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

**4.特殊集合**         
![10.1.25.png](http://upload-images.jianshu.io/upload_images/3001083-6887684508ed9a93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

**5.DOM一致性检测**         
![10.1.26.png](http://upload-images.jianshu.io/upload_images/3001083-aab12b8e15643857.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

由于使用hasFeature()在有些情况下得到的结果会有偏差,所以建议还是使用**能力检测**.         

**6.文档写入**         
1. write()         
2. writeln()         
3. open()         
4. close()         

write()和writeln()方法都接收一个字符串参数,即要写入到输出流中的文本(可以是html语句也可以是外部文件的引用),write()会原样写入,writeln()会自动在字符串的末尾添加一个换行符.在页面被加载的过程中,可以使用这两个方法向页面中动态地加入内容.         
open和close()分别用于打开和关闭网页的输出流.         

### 10.1.3 Element类型         
![10.1.31.png](http://upload-images.jianshu.io/upload_images/3001083-ff2ca4a794c514d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.32.png](http://upload-images.jianshu.io/upload_images/3001083-f8439241ea0fb16f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

getAttribute()用得不多,一般来说,在取得自定义特性值的情况下,才会使用getAttribute()方法.         

![10.1.33.png](http://upload-images.jianshu.io/upload_images/3001083-5a0b35755dbe24a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

**5.创建元素**         
使用document.createElement()方法可以创建新元素.这个方法只接受一个参数,即要创建的元素的标签名.
如: var div = document.createElement("div");         

![10.1.34.png](http://upload-images.jianshu.io/upload_images/3001083-377af49f9453251b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.35.png](http://upload-images.jianshu.io/upload_images/3001083-e798db1d64b67615.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.36.png](http://upload-images.jianshu.io/upload_images/3001083-bf25df6946151347.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

### 10.1.4 Text类型         
![10.1.41.png](http://upload-images.jianshu.io/upload_images/3001083-d99b35793cd2f9eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

1. **创建文本节点**: 可以使用document.createTextNode()创建新文本节点.一般来说,一个元素只有一个文本子节点,但有些情况下也可以包含多个文本子节点.         
2. **规范化文本节点**: DOM文档中存在相邻的通报文本节点易导致混乱,于是催生了一个能将相邻文本节点合并的方法.这个方法是由Node类型定义的(因而在所有节点类型中都存在)名叫normalize().结果节点的nodeValue等于将合并前每个文本节点的nodeValue值拼接起来的值.         
![10.1.42.png](http://upload-images.jianshu.io/upload_images/3001083-a3c5a0fab6ed1695.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

3. **分割文本节点**:Text类型提供了一个作用与normalize()相反的方法:splitText().         
![10.1.43.png](http://upload-images.jianshu.io/upload_images/3001083-2f925bf52345c697.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         


### 10.1.5 Comment类型         
![10.1.51.png](http://upload-images.jianshu.io/upload_images/3001083-523b993113d2b769.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

### 10.1.6 DocumentType类型         
DocumentType类型在Web浏览器中并不常用.         
![10.1.71.png](http://upload-images.jianshu.io/upload_images/3001083-6c12ecb134e75844.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
在DOM1级中,DocumentType对象不能动态创建,而至能通过解析文档代码的方式来创建.支持它的浏览器会把DocumentType对象保存在document.doctype中.         

### 10.1.7 DocumentFragment类型         
![10.1.81.png](http://upload-images.jianshu.io/upload_images/3001083-ad5c62c6e5fa8379.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.1.82.png](http://upload-images.jianshu.io/upload_images/3001083-436380a58776b29a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

### 10.1.8 Attr类型         
![10.1.91.png](http://upload-images.jianshu.io/upload_images/3001083-2fe108e251a39869.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

**尽管它们也是节点,但特性却不被认为是DOM文档树中的一部分.开发人员最常使用的是getAttribute()、setAttribute()、removeAttribute()方法,很少直接引用特性节点.**         

![10.1.92.png](http://upload-images.jianshu.io/upload_images/3001083-afd3c0788ee65ff3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

## 10.2 DOM操作技术           
很多时候,DOM操作都比较简明,因此用JavaScript生成哪些通常原本是用HTML代码生成的内容并不麻烦.不过,也有一些时候,操作DOM并不像表面上看起来那样简单.由于浏览器中充斥着隐藏的陷阱和不兼容问题,用JavaScript代码处理DOM的某些部分要比处理其他部分更复杂一些.         

### 10.2.1 操作表格         
为了方便创建表格,HTML DOM 为< table > , < tbody >和< tr >元素添加了一些属性和方法.         

![10.2.31.png](http://upload-images.jianshu.io/upload_images/3001083-3151f5efa1aa4c70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.2.32.png](http://upload-images.jianshu.io/upload_images/3001083-f7f5a16c27aa6168.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

比如我们要实现下面这个表格:         
![10.2.33.png](http://upload-images.jianshu.io/upload_images/3001083-963fada70c213448.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

``` javascript
	var fragment = document.createDocumentFragment();
	var table = document.createElement("table");
	table.border = 1;
	table.width = "100%";
	fragment.appendChild(table);

	var tbody = document.createElement("tbody");
	table.appendChild(tbody);

	tbody.insertRow(0);
	tbody.rows[0].insertCell(0);
	tbody.rows[0].cells[0].appendChild(document.createTextNode("Cell 1,1"));
	tbody.rows[0].insertCell(1);
	tbody.rows[0].cells[1].appendChild(document.createTextNode("Cell 2,1"));

	tbody.insertRow(1);
	tbody.rows[1].insertCell(0);
	tbody.rows[1].cells[0].appendChild(document.createTextNode("Cell 1,2"));
	tbody.rows[1].insertCell(1);
	tbody.rows[1].cells[1].appendChild(document.createTextNode("Cell 2,2"));

	console.log(fragment);
	document.body.appendChild(fragment);  //最后再把fragment加到页面里就可以不用频繁操作DOM
```

### 10.2.2 使用NodeList         
![10.2.41.png](http://upload-images.jianshu.io/upload_images/3001083-9de9dd1e4c344829.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         
![10.2.42.png](http://upload-images.jianshu.io/upload_images/3001083-03a023661984693d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

(要解决这个问题,可以在第一次时用一个变量将divs.length存起来)         

**一般来说,应该尽量减少访问NodeList的次数.因为每次访问NodeList,都会运行一次基于文档的查询(这也是为什么每次取到的NodeList.length不一样的愿意).所以,可以考虑将NodeList中取得的值缓存起来.**         

## 10.3 小结           
 - DOM是语言中立的API,用于访问和操作HTML文档.DOM1级将HTML文档形象地看作一个层次化的节点树,可以使用JavaScript来操作者各节点树,进而改变底层文档的外观和结构.         
 - DOM由各种节点构成.         
 - 最基本的节点类型是Node,用于抽象地表示文档中一个独立的部分.所有其他类型都继承自Node.         
 - Document类型表示整个文档,是一组分层节点的根节点.在JavaScript中,document对象是Document的一个实例.使用document对象,有很多种方式可以查询和取得节点.         
 - Element节点表示文档中的所有HTML元素,可以用来操作这些元素的内容和特性.         
 - 另外还有一些节点类型,分别表示文本内容,注释,文档类型和文档片段等.         

**理解DOM的关键,是理解DOM对性能的影响.DOM操作往往是JavaScript程序中开销最大的部分,而因访问NodeList导致的问题最多.NodeList对象都是"动态的",这就意味着每次访问NodeList对象,都会运行一次查询.有鉴于此,最好的办法就是尽量减少DOM操作.**         



### 相关文章链接    
 - [Javascript深入学习1——词法结构、类型、值、变量](http://liveipool.com/blog/2016/09/12/learn-javascript-1/)       
 - [JavaScript红皮书学习(1,2)--JavaScript简介](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-1,2-Introduction/)  
 - [JavaScript红皮书学习(3)--基本概念](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-3-BasicConcepts/)   
 - [JavaScript红皮书学习(4)--变量、作用域和内存问题](http://liveipool.com/blog/2016/12/19/JavaScript-RedBook-4-Variable-Scope-and-Memory/)    
 - [JavaScript红皮书学习(5)--引用类型](http://liveipool.com/blog/2016/12/22/JavaScript-RedBook-5-Reference-Type)     
 - [JavaScript学习专题之--作用域](http://liveipool.com/blog/2016/12/22/JavaScript-Scope)   
 - [JavaScript学习专题之--闭包](http://liveipool.com/blog/2016/12/23/JavaScript-Closures)     
 - [JavaScript学习专题之--私有变量和ES5中的模块模式](http://liveipool.com/blog/2016/12/24/JavaScript-Private-Variable-and-ES5Modules)      
 - [JavaScript学习专题之--this](http://liveipool.com/blog/2016/12/25/JavaScript-this)       
 - [JavaScript红皮书学习(8)--BOM](http://liveipool.com/blog/2016/12/25/JavaScript-RedBook-8-BOM)             
 - [JavaScript红皮书学习(6)--面向对象的程序设计](http://liveipool.com/blog/2016/12/27/JavaScript-RedBook-6-Object-Oriented)                  
 - [JavaScript红皮书学习(10)--DOM1](http://liveipool.com/blog/2016/12/31/JavaScript-RedBook-10-DOM1)                  
 - [JavaScript红皮书学习(11)--DOM-Extensions](http://liveipool.com/blog/2016/12/31/JavaScript-RedBook-11-DOM-Extensions)                  
 - [JavaScript红皮书学习(12)--DOM2-And-DOM3](http://liveipool.com/blog/2016/12/31/JavaScript-RedBook-12-DOM2-And-DOM3)                  
 - [JavaScript红皮书学习(13)--事件](http://liveipool.com/blog/2017/01/13/JavaScript-RedBook-13-Event)                  