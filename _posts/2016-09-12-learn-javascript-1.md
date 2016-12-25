---
layout: post
title: Javascript深入学习1——词法结构、类型、值、变量
date: 2016-09-12
categories: blog
tags: [前端]
description: Javascript学习1
---

# Javascript深入学习——词法结构、类型、值、变量  

无论使用什么框架，无论使用什么新的方法，在web应用开发时，Javascript都是必须得透彻掌握的一门语言，这门语言的重要性不言而喻。  
在一段时间的学习和一些具体应用的开发后，深切的感觉自己对一些Javascript的基础知识掌握的并不牢固，于是便翻起一本厚厚的《Javascript权威指南》。  
最近大家都在说“神奇是有毒的”，随着学习的不断深入，愈发能理解这句话。但之所以经常能感受到神奇，就是因为基础不好，很多东西并没有深入理解过，导致看到一件神奇的事情时只有空赞叹而没有如何实现的想法。  
在这里，我向大家分享一些我学到的一些觉得应该注意的地方，希望能有所帮助。  

**一、关于这门语言的基本介绍及一些历史**  
Javascript是一门高端的、动态的、弱类型的编程语言，适合面向对象和函数式的编程风格。它是由发展初期的网景(Netscape)公司创建。”Javascirpt”是Oracle公司的注册商标，用来特指网景(现在的Mozilla)对这门语言的实现。  
网景将这门语言作为标准提交给了ECMA—由于商标的冲突，这门语言的标准版本改了一个丑陋的名字”ECMAScript”。但几乎所有人都将这门语言叫做”Javascript”,我们也只是用”ECMAScript”来指代语言标准。  

**二、Javascript的数据类型的两种分类方法**  
第一种分法：Javascript的数据类型分为:原始类型(primitive type)和对象类型(object type)。  
原始类型包括数字、字符串、布尔值以及两个特殊的原始值:null和undefined，这两个特殊值通常代表了各自特殊类型的唯一的成员。  
而Javascript除了以上说的五个原始类型之外的就是对象了。对象是属性的集合，每个属性都由”名/值对”构成。数组、函数都是特殊的对象。  
  
第二种分法：将Javascript的数据类型分为可变(mutable)类型和不可变(immutable)类型，可变类型的值是可以修改的。  
对象属于可变类型：Javascript可以改变对象属性的值。  
数字、布尔值、null和undefined都是不可变的：修改一个数值的内容本身就说不通。字符串可以看成由字符组成的数组，你可能会认为它是可变的。然而在Javascript中，字符串是不可变的:可以访问字符串任意位置的文本，但Javascript并未提供修改已知字符串的文本内容的方法。  

**三、算术运算时的一些特殊情况**  
Javascript中的算术运算在一处(上溢overflow)、下溢(underflow)或被零整除时不会报错:  
1：当数字运算结果超过了所能表示的数字上限或负数的值超过了负数表示范围(溢出)，结果是一个特殊的无穷(infinity)值，在Javascript中表示为Infinity和-Infinity。无穷值的行为特性和我们所期望的是一致的，基于它们的加减乘除运算结果依然是无穷值。  
2：下溢(underflow)是当运算结果无限接近于零并比Javascript所能表示的最小值还小的时候发生的一种情况。这种情况下，结果返回0或-0。  
-0值比较特殊，它和0在严格相等判断下都是相等的：     

![image2016-7-16 1-1-48.png](http://upload-images.jianshu.io/upload_images/3001083-6e5e09caaf3eed8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

（正零和负零的值相等，但正无穷大和负无穷大不相等）  
3：被零整除在Javascript中并不报错：它只是简单的返回Infinity或-Infinity。但有一个例外，零除以零是没有意义的，这时将会返回NaN。以及无穷除以无穷，给任意负数作开方运算或者算术运算符与不是数字或无法转换为数字的操作数一起使用时都将返回NaN。  
![](http://hcp.sysu.edu.cn/wiki/download/attachments/20611443/image2016-7-16%201%3A6%3A31.png?version=1&modificationDate=1468603965134&api=v2)  
NaN这个值有点特殊：它和任何值都不相等，包括自身。即不能通过x === NaN来判断变量x是否是NaN,应当使用isNaN()函数或x !== x来判断。  
![](http://hcp.sysu.edu.cn/wiki/download/attachments/20611443/image2016-7-16%201%3A7%3A3.png?version=1&modificationDate=1468603965128&api=v2)    

**四、关于“==” 和 “===”**  
或许大家已经了解过，或许还没有，“==”和“===”这两个运算符到底有什么区别？  
以前只是囫囵吞枣地看过，不过用起来也还是不太说得出到底，浑浑噩噩就用了，于是特地去了解了一下：  
“==”叫相等运算符，“===”叫作严格相等运算符或恒等运算符，它们最本质的区别在于，**判断相等时是否允许做类型转换**。“==”允许在判断中进行类型转换后再判断，而“===”在比较过程中不允许有任何的类型转换。  
举个最简单的例子：  
![](http://hcp.sysu.edu.cn/wiki/download/attachments/20611443/image2016-7-16%201%3A17%3A5.png?version=1&modificationDate=1468603965122&api=v2)  
一目了然，“==”运算时，字符串“1”转换为了数字1，而“===”时，“1”依然是字符串。  
 
**五、二进制表示法**  
Javascript采用IEEE-754浮点数表示法（几乎所有现代编程语言所采用），这是一种二进制表示法，可以精确地表示分数，如1/2，1/8，1/1024。但二进制浮点数表示法并不能精确的表示类似0.1这样简单的数字。  
让我们看看下面这段有毒的代码：  
![](http://hcp.sysu.edu.cn/wiki/download/attachments/20611443/image2016-7-16%201%3A21%3A16.png?version=1&modificationDate=1468603965115&api=v2)  
这里x = 0.3 - 0.2，y = 0.2 - 0.1 而x和y居然不相等，console后可以看出，在Javascript的真实运行环境中，0.2 - 0.1运行得结果0.1，0.3 - 0.2 = 0.不知道多少个9还有个8.  
由于舍入误差，0.3和0.2之间的近似差值并不等于0.2和0.1之间的近似差值。其实，这个问题不只是在Javascript中才会出现，在任何使用二进制浮点数的编程语言中都会有这个问题。但虽然有误差，这种计算结果已经可以胜任大多数的计算任务，这个问题也只有在比较两个值是否相等的时候才会出现。  
 
**六、关于null和undefined**  
null和undefined有很多绕口的概念，以下几个点是我觉得最常用的几个点，有兴趣的可以再深入了解这两个值的其他概念：  
null是关键字 undefined不是  

![image2016-7-16 1-27-41.png](http://upload-images.jianshu.io/upload_images/3001083-e2afc8da5bd95ca0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
想说的都在截图里~  
 
 
**七、“包装对象”**  
Javascript对象是一种复合值：它是属性或已命名值的集合。当属性值是一个函数的时候，其称为方法。通过如o.m()来调用对象o中的方法。  
而我们看见字符串也有相同的属性和方法：  
var s = “hello world”;  
var word = s.substring(s.indexOf(“ ”)+1, s.length);  
 
字符串既然不是对象，为什么会有属性呢？是因为只要引用了字符串s的属性，Javascript会将字符串通过调用new String(s)的方式转换成对象，这个对象继承了字符串的方法，并被用来处理属性的引用。一旦属性引用结束，这个新创建的临时对象就会销毁（其实实际上并不一定创建或销毁这个临时对象，然而整个过程看起来是这样，可以帮助理解）。  
数字和布尔值也类似。  
看看下面这段代码：  

![image2016-7-16 1-30-45.png](http://upload-images.jianshu.io/upload_images/3001083-0383b1eeb498d7d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

可以看出第二行代码”s.len“创建一个临时字符串对象，并给其len属性赋值为4，随即销毁这个对象。第三行”var t=s.len“通过原始的字符串值创建一个新字符串对象并尝试读取len属性，这len属性自然不存在。  
修改只是发生在临时对象身上，而这个临时对象并未继续保留下来，**存取字符串、数字、布尔值的属性时创建的临时对象称为包装对象。**  
通常，包装对象只是被看做是一种实现细节，而不用特别关注。由于字符串、数字、布尔值的属性都是只读的，并且不能给它们定义新属性，因此包装对象是有别于对象的。  
 
还有一点需要注意的是，如在通过Number()等构造函数来显式创建包装对象时，Javascript会在必要时将包装对象转换成原始值，但包装对象和原始值还是有不同的。  

![](http://upload-images.jianshu.io/upload_images/3001083-b66e3e111fbace8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  


**八、不可变的原始值和可变的对象的引用**  
Javascript中的原始值（undefined、null、字符串、数字和布尔值）与对象(包括数组和函数)有根本的区别。**原始值是不可更改的：任何方法都无法改变一个原始值！**  
例如，字符串中的所有方法看上去都是返回了一个修改后的字符串，**实际上返回的是一个新的字符串**：  

![](http://upload-images.jianshu.io/upload_images/3001083-2db6bba2b35760d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

 
而对象就是可变的：  

![](http://upload-images.jianshu.io/upload_images/3001083-4241ff8d174d8dd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

 
又是一个相当有毒的来了：对象的比较！  
**对象的比较并非值的比较**：即使两个对象包含同样的属性及相同的值，它们也是不相等的。各个索引完全相等的两个数组也不相等。  

![](http://upload-images.jianshu.io/upload_images/3001083-6b28d79944216de4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

![](http://upload-images.jianshu.io/upload_images/3001083-547f75227ec8f4fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
 
**两个单独的对象，永不相等！**  
 
通常将对象称为引用类型(reference type)， 依照术语的叫法，对象值都是引用，对象的比较均是引用的比较，这也就解释了为啥上面的对象o、p不相等，因为它们的引用不同。当且仅当它们引用同一个基对象时，它们才相等：  

![](http://upload-images.jianshu.io/upload_images/3001083-531235d409c3b4b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

从代码中可以看出，将对象赋值给一个变量，仅仅是赋的引用值，对象本身并没有复制一次。如果想要得到一个对象的副本，必须显式地复制对象的每个属性。  
 
**九、显式类型转换**  
尽管Javascript可以自动做许多类型转换，但有时仍需要做显式转换，或者说为了使代码变得清晰易读而做显式转换。  
显式类型转换最简单的方法是使用Boolean(), Number(), String(), Object()等函数。  
而以下三行代码是我看到的觉得比较简单实用但以前忽视掉的，在此记录一下：  
x + ""     //等价于String(x).    
+x         //等价于Number(x).    
!!x         //等价于Boolean(x).  
 
**十、动态类型和静态类型**  
在最开始，我说过Javascript是一种高端的、动态的、弱类型的编程语言，但以前其实一直不太能完全说出动态语言和静态语言的区别，而在这里也算详细了解了一下：编程语言分为动态（类型）语言和静态（类型）语言，动态类型语言是指在运行期间去做数据类型的检查，也就是说，在用动态类型的语言编程时，不用给任何变量指定数据类型，该语言会在第一次赋值给变量时，在内部将数据类型记录下来。Python、Ruby、Javascript就是典型的动态类型语言。而静态类型语言则恰好相反，它的数据类型是在编译期间检查的，也就是说在写程序时要声明所有变量的数据类型，C、C++是静态类型语言的典型代表。  
 
**神奇是有毒的，但这样的毒未尝不是一件好事，它让你迫切地想去解毒，想去解决这个问题**。  
**我感觉学习就是一个不断踩坑再不断填坑的过程，有些坑可能第一次偷懒迈过去了，但谁又说得准以后就不会碰到相同的坑了呢。**  
**所以，打好基础才是硬道理，不断学习，让“神奇”的东西越来越少。**   
**甚至，说不定有一天，能自己去创造一些“神奇”。**     


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
 - [JavaScript红皮书学习(5)--BOM](http://liveipool.com/blog/2016/12/25/JavaScript-RedBook-8-BOM)             


