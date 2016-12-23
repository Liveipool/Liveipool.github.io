---
layout: post
title: JavaScript学习专题之--作用域  
date: 2016-12-22
categories: blog
tags: [JavaScript]
description: JavaScript学习
---

# JavaScript作用域               
 - JS红皮书刚讲完前三章基本的语法后,就开始提到作用域的概念,第七章更是开始大谈闭包,但看了下还是觉得略抽象,正好《你不知道的JavaScript》到了，上卷有两个大部分，第一个部分就是scope & closures，可见这作用域和闭包的重要性，结合着两本书来看，算是基本把它们理解得差不多了，但这肯定只是暂时记忆，需要好好用两个专题来总结一下，以后经常看看.     
 - 《你不知道的JavaScript》关于作用域讲得很清楚的一个地方在于，它不是纯粹的告诉你哪些地方的作用域是嵌套的，哪些声明会提升，它从编译原理的角度讲起，从编译器和引擎的位置出发来告诉你为什么会出现那些看似奇怪的现象。     
 - 几乎所有编程语言最基本的功能之一，就是能够储存变量当中的值，并且能在之后对这个值进行访问或修改。事实上，正是这种储存和访问变量的值的能力将**状态**带给了程序。而变量是储存在哪里的呢？程序又是如何找到它们的？     
 - 这些问题说明**需要一套设计良好的规则来存储变量，并且之后可以方便地找到这些变量**。这套规则被称为**作用域**。        

### 一、编译原理     
尽管通常将JavaScript归类为“动态”或“解释执行”语言，而事实上它是一门编译语言。但与传统的编译语言不同，它不是提前编译的，编译结果也不能在分布式系统中进行移植。统编译语言的流程中，程序中的一段源代码在执行之前会经历三个步骤，统称为“编译”:        

#### 1.分词/词法分析（Tokenizing/Lexing）     
> 这个过程会将由字符组成的字符串分解成（对编程语言来说）有意义的代码块，这些代码块被称为词法单元（token）。例如，考虑程序var a = 2;。这段程序通常会被分解成为下面这些词法单元：var、a、=、2 、;。空格是否会被当作词法单元，取决于空格在这门语言中是否具有意义。     

#### 2.解析/语法分析（Parsing）     
> 这个过程是将词法单元流（数组）转换成一个由元素逐级嵌套所组成的代表了程序语法结构的树。这个树被称为“抽象语法树”（Abstract Syntax Tree，AST）。     
> var a = 2;的抽象语法树中可能会有一个叫作VariableDeclaration的顶级节点，接下来是一个叫作Identifier（它的值是a）的子节点，以及一个叫作AssignmentExpression的子节点。AssignmentExpression节点有一个叫作NumericLiteral（它的值是2）的子节点。     

#### 3.代码生成     
> 将AST转换为可执行代码的过程称被称为代码生成。这个过程与语言、目标平台等息息相关。     
> 抛开具体细节，简单来说就是有某种方法可以将var a = 2;的AST转化为一组机器指令，用来创建一个叫作a的变量（包括分配内存等），并将一个值储存在a中。     

事实上，JavaScript引擎进行编译的步骤和传统的编译语言非常相似，在某些环节可能比预想的要复杂。     
首先，JavaScript引擎不会有大量的（像其他语言编译器那么多的）时间用来进行优化，因为与其他语言不同，JavaScript的编译过程不是发生在构建之前的。     
对于JavaScript来说，大部分情况下编译发生在代码执行前的几微秒的时间内。在我们所要讨论的作用域背后，JavaScript引擎用尽了各种办法（比如JIT，可以延迟编译甚至实施重编译）来保证性能最佳。     
简单地说，任何JavaScript代码片段在执行前都要进行编译（通常就在执行前）。因此，JavaScript编译器首先会对var a = 2;这段程序进行编译，然后做好执行它的准备，并且通常马上就会执行它。     

### 二、理解作用域         
 - 从始至终,有三个概念是至关重要的:     
 - **引擎**: 负责整个JavaScript程序的编译及执行过程.     
 - **编译器**:负责语法分析及代码生成等脏活累活.     
 - **作用域**:负责收集并维护由所有声明的标识符(变量)组成的一系列查询,并实施一套非常严格的规则,确定当前执行的代码对这些标识符的访问权限.     

下面通过一个具体的例子结合着分析:     
如果我们现在遇到一行代码: var a = 2; 上面那个概念是怎么结合着来处理这一行代码的呢?     
1.首先,**编译器** 会将这段程序分解成词法单元.     
2.然后,**编译器**将词法单元解析成一个树结构.     
3.当**编译器**开始进行代码生成时,它对这段程序的处理方式会和预期的有所不同.     
4.如果遇到var a，**编译器**会询问**作用域**是否已经有一个该名称的变量存在于同一个**作用域的集合**中。如果是，**编译器**会忽略该声明，继续进行编译；否则它会要求**作用域**在当前**作用域的集合**中声明一个新的变量，并命名为a。     
5.接下来**编译器**会为**引擎**生成运行时所需的代码，这些代码被用来处理a = 2这个赋值操作。**引擎**运行时会首先询问**作用域**，在当前的**作用域集合**中是否存在一个叫作a的变量。如果是，**引擎**就会使用这个变量；如果不是，**引擎**会继续查找该变量。     
     
#### 1.  LHS和RHS     
**编译器**在编译过程中的第5步中生成了代码,**引擎**执行它时,会通过查找变量a来判断它是否已声明过.查找的过程由**作用域**进行协作,在本例中,**引擎**会为变量a进行LHS查询.     
而查询变量a的具体值的操作为RHS.即,RHS查询与简单地查找某个变量的值别无二致，而LHS查询则是试图找到变量的容器本身，从而可以对其赋值。从这个角度说，RHS并不是真正意义上的“赋值操作的右侧”，更准确地说是“非左侧”。       

考虑下面的程序，其中既有LHS也有RHS引用：     

``` javascript
function foo(a)  {
    console.log( a ); // 2
}
foo( 2 );
```
最后一行foo(..)函数的调用需要对foo进行RHS引用，意味着“去找到foo的值，并把它给我”。并且(..)意味着foo的值需要被执行，因此它最好真的是一个函数类型的值！     
这里还有一个容易被忽略却非常重要的细节:代码中隐式的a = 2操作可能很容易被你忽略掉。这个操作发生在2被当作参数传递给foo(..)函数时，2会被分配给参数a。为了给参数a（隐式地）分配值，需要进行一次LHS查询。     
这里还有对a进行的RHS引用，并且将得到的值传给了console.log(..)。console.log(..)本身也需要一个引用才能执行，因此会对console对象进行RHS查询，并且检查得到的值中是否有一个叫作log的方法。     

#### 2. 作用域嵌套       
前面说到,作用域是根据名称查找变量的一套规则。实际情况中，通常需要同时顾及几个作用域。     
当一个块或函数嵌套在另一个块或函数中时，就发生了作用域的嵌套。因此，在当前作用域中无法找到某个变量时，引擎就会在外层嵌套的作用域中继续查找，直到找到该变量，或抵达最外层的作用域（也就是全局作用域）（浏览器中为window）为止。     
考虑以下代码：     

``` javascript
function foo(a) {
    console.log( a + b );
}

var b = 2;

foo( 2 ); // 4
```
对b进行的RHS引用无法在函数foo内部完成，但可以在上一级作用域（在这个例子中就是全局作用域）中完成。     
![sc1.png](http://upload-images.jianshu.io/upload_images/3001083-71afb13f67cb3bd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### 3. 异常     
为什么区分LHS和RHS是一件重要的事情？     

因为在变量还没有声明（在任何作用域中都无法找到该变量）的情况下，这两种查询的行为是不一样的。     

考虑如下代码：     

``` javascript
function foo(a) {
    console.log( a + b );  //此时RHS查询会失败
    b = a;   //非严格模式下,此时LHS查询失败,会自动在全局作用域中创建一个具有该名称的变量
}
foo( 2 );
```

第一次对b进行RHS查询时是无法找到该变量的。也就是说，这是一个“未声明”的变量，因为在任何相关的作用域中都无法找到它。     

如果RHS查询在所有嵌套的作用域中遍寻不到所需的变量，引擎就会抛出**ReferenceError异常**。值得注意的是，ReferenceError是非常重要的异常类型。     

相较之下，当引擎执行LHS查询时，如果在顶层（全局作用域）中也无法找到目标变量，全局作用域中就会创建一个具有该名称的变量，并将其返还给引擎，前提是程序运行在非“严格模式”下。     

ES5中引入了“严格模式”。同正常模式，或者说宽松/懒惰模式相比，严格模式在行为上有很多不同。其中一个不同的行为是严格模式禁止自动或隐式地创建全局变量。因此，在严格模式中LHS查询失败时，并不会创建并返回一个全局变量，引擎会抛出同RHS查询失败时类似的ReferenceError异常。     

接下来，如果RHS查询找到了一个变量，但是你尝试对这个变量的值进行不合理的操作，比如试图对一个非函数类型的值进行函数调用，或者引用null或undefined类型的值中的属性，那么引擎会抛出另外一种类型的异常，叫作**TypeError**。     

**ReferenceError同作用域判别失败相关，而TypeError则代表作用域判别成功了，但是对结果的操作是非法或不合理的。**     

#### 4. 小结        
 - 作用域是一套规则，用于确定在何处以及如何查找变量（标识符）。如果查找的目的是对变量进行赋值，那么就会使用LHS查询；如果目的是获取变量的值，就会使用RHS查询。     
 - 赋值操作符会导致LHS查询。＝操作符或调用函数时传入参数的操作都会导致关联作用域的赋值操作。     
 - JavaScript引擎首先会在代码执行前对其进行编译，在这个过程中，像var a = 2这样的声明会被分解成两个独立的步骤：     
   - 首先，var a在其作用域中声明新变量。这会在最开始的阶段，也就是代码执行前进行。     
   -  接下来，a = 2会查询（LHS查询）变量a并对其进行赋值。     
 - LHS和RHS查询都会在当前执行作用域中开始，如果有需要（也就是说它们没有找到所需的标识符），就会向上级作用域继续查找目标标识符，这样每次上升一级作用域（一层楼），最后抵达全局作用域（顶层），无论找到或没找到都将停止。     
 - 不成功的RHS引用会导致抛出ReferenceError异常。不成功的LHS引用会导致自动隐式地创建一个全局变量（非严格模式下），该变量使用LHS引用的目标作为标识符，或者抛出ReferenceError异常（严格模式下）。     

### 三、词法作用域        
 - 再次强调，**作用域**是**一套规则**，这套规则用来管理**引擎**如何在当前作用域以及嵌套的子作用域中根据标识符名称进行变量查找。     
 - **作用域共有两种主要的工作模型：**     
 - 一种叫做**动态作用域**，只有极少数编程语言在使用（如Bash脚本，Perl中的一些模式等）     
 - 第二种是被大多数编程语言所采用的**词法作用域**。     

#### 1.词法阶段       
大部分标准语言编译器的第一个工作阶段叫作词法化（也叫单词化）。词法化的过程会对源代码中的字符进行检查，如果是有状态的解析过程，还会赋予单词语义。        
简单地说，词法作用域就是定义在词法阶段的作用域。换句话说，词法作用域是由你在写代码时将变量和块作用域写在哪里来决定的，因此当词法分析器处理代码时会保持作用域不变。（大部分情况下是这样的，后面会介绍一些欺骗词法作用域的方法，这些方法在词法分析器处理过后依然可以修改作用域。事实上，让词法作用域根据词法关系保持书写时的自然关系不变，是一个很好的实践。）     
考虑以下代码：     

``` javascript
function foo(a) {
    var b = a * 2;

    function bar(c) {
        console.log( a, b, c );
    }

    bar( b * 3 );
}

foo( 2 ); // 2, 4, 12
```

![sc2.png](http://upload-images.jianshu.io/upload_images/3001083-6a75c1c61c905f5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)          

**注意，这里所说的气泡是严格包含的。没有任何函数的气泡可以（部分地）同时出现在两个外部作用域的气泡中，就如同没有任何函数可以部分地同时出现在两个父级函数中一样。**        

**查找：**     
作用域气泡的结构和互相之间的位置关系给引擎提供了足够的位置信息，引擎用这些信息来查找标识符的位置。作用域查找会在**找到第一个匹配的标识符时停止**。在多层的嵌套作用域中可以定义同名的标识符，这叫作**“遮蔽效应”**（内部的标识符“遮蔽”了外部的标识符）。抛开遮蔽效应，作用域查找始终从运行时所处的最内部作用域开始，逐级向外或者说向上进行，直到遇见第一个匹配的标识符为止。无论函数在哪里被调用，也无论它如何被调用，它的词法作用域都**只由函数被声明时所处的位置决定**。     
     
#### 2. 欺骗词法      
**欺骗词法作用域会导致性能下降！！尽量别用了，了解一下就好！！**        
JavaScript中有两种机制来实现这个目的： eval()和with。     

##### 2.1 eval()     
JavaScript中的eval()函数可以接受一个字符串为参数，并将其中的内容视为好像在书写时就存在于程序中这个位置的代码。换句话说，可以在你写的代码中用程序生成代码并运行，就好像代码是写在那个位置的一样。     
根据这个原理来理解eval()，它是如何通过代码欺骗和假装成书写时（也就是词法期）代码就在那，来实现修改词法作用域环境的，这个原理就变得清晰易懂了。
在执行eval(.之后的代码时，引擎并不“知道”或“在意”前面的代码是以动态形式插入进来，并对词法作用域的环境进行修改的。引擎只会如往常地进行词法作用域查找。     
考虑以下代码：     

``` javascript
function foo(str, a) {
    eval( str ); // 欺骗！
    console.log( a, b );
}
var b = 2;
foo( "var b = 3;", 1 ); // 1, 3
```

eval(..)调用中的"var b = 3;"这段代码会被当作本来就在那里一样来处理。由于那段代码声明了一个新的变量b**，因此它对已经存在的foo(..)的词法作用域进行了修改**。事实上，和前面提到的原理一样，这段代码实际上在foo(..)内部创建了一个变量b，并遮蔽了外部（全局）作用域中的同名变量。     
当console.log(..)被执行时，会在foo(..)的内部同时找到a和b，但是永远也无法找到外部的b。因此会输出“1, 3”而不是正常情况下会输出的“1, 2”。     
在这个例子中，为了展示的方便和简洁，我们传递进去的“代码”字符串是固定不变的。而在实际情况中，可以非常容易地根据程序逻辑动态地将字符拼接在一起之后再传递进去。eval(..)通常被用来执行动态创建的代码，因为像例子中这样动态地执行一段固定字符所组成的代码，并没有比直接将代码写在那里更有好处。     

 **在严格模式的程序中，eval(..)在运行时有其自己的词法作用域，意味着其中的声明无法修改所在的作用域。**     
     
``` javascript
function foo(str) {
   "use strict";
   eval( str );
   console.log( a ); // ReferenceError: a is not defined
}

foo( "var a = 2");
```

JavaScript中还有其他一些功能效果和eval(..)很相似。setTimeout(..)和setInterval(..)的第一个参数可以是字符串，字符串的内容可以被解释为一段动态生成的函数代码。这些功能已经过时且并不被提倡。**不要使用它们！**     
new Function(..)函数的行为也很类似，最后一个参数可以接受代码字符串，并将其转化为动态生成的函数（前面的参数是这个新生成的函数的形参）。这种构建函数的语法比eval(..)略微安全一些，**但也要尽量避免使用。**     

**在程序中动态生成代码的使用场景非常罕见，因为它所带来的好处无法抵消性能上的损失。**       

##### 2.2 with       
也会在运行时修改或创建新的作用域,影响性能,with在严格模式下已经被完全禁止了,所以了解一下就好,不要再用了.        

##### 2.3 性能       
JavaScript引擎会在编译阶段进行数项的性能优化。其中有些优化依赖于能够根据代码的词法进行静态分析，并预先确定所有变量和函数的定义位置，才能在执行过程中快速找到标识符。     

但如果引擎在代码中发现了eval(..)或with，它只能简单地假设关于标识符位置的判断都是无效的，因为无法在词法分析阶段明确知道eval(..)会接收到什么代码，这些代码会如何对作用域进行修改，也无法知道传递给with用来创建新词法作用域的对象的内容到底是什么。     

最悲观的情况是如果出现了eval(..)或with，所有的优化可能都是无意义的，因此最简单的做法就是完全不做任何优化。     

如果代码中大量使用eval(..)或with，那么运行起来一定会变得非常慢。无论引擎多聪明，试图将这些悲观情况的副作用限制在最小范围内，也无法避免如果没有这些优化，代码会运行得更慢这个事实。        

#### 3. 小结        
词法作用域意味着作用域是由书写代码时函数声明的位置来决定的。编译的词法分析阶段基本能够知道全部标识符在哪里以及是如何声明的，从而能够预测在执行过程中如何对它们进行查找。     
     
JavaScript中有两个机制可以“欺骗”词法作用域：eval(..)和with。前者可以对一段包含一个或多个声明的“代码”字符串进行演算，并借此来修改已经存在的词法作用域（在运行时）。后者本质上是通过将一个对象的引用当作作用域来处理，将对象的属性当作作用域中的标识符来处理，从而创建了一个新的词法作用域（同样是在运行时）。     

这两个机制的副作用是引擎无法在编译时对作用域查找进行优化，因为引擎只能谨慎地认为这样的优化是无效的。使用这其中任何一个机制都将导致代码运行变慢。不要使用它们。     


### 四、函数作用域和块作用域       
     
#### 1. 函数作用域        
考虑下面的代码:      

``` javascript
function foo(a) {
	var b = 2;
	// some codes
	function bar() {
		//...
	}
	var c = 3;
}
```
**JavaScript具有基于函数的作用域,意味着每声明一个函数都会为其自身创建一个类似上文中的气泡.**       
**函数作用域的含义是指,属于这个函数的全部变量都可以在整个函数的范围内使用及复用(事实上在嵌套的作用域中也可以使用).**     

#### 2. 隐藏内部实现       
对函数的传统认知就是先声明一个函数,然后再向里面添加代码.但反过来想也可以带来一些启示:从所写的代码中挑选出一个人以的片段,然后用函数声明对它进行包装,实际上就是把这些代码"隐藏"起来了.       
为什么"隐藏"变量和函数是一个有用的技术?这里要提到一个概念:**最小特权原则**.     
最小特权原则也叫最小授权原则或最小暴露原则.这个原则是指在软件设计中,应该**     最小限度地暴露必要内容,而将其他内容都"隐藏"起来,比如某个模块或对象的API设计.**        
![sc3.png](http://upload-images.jianshu.io/upload_images/3001083-158a9278f94f0aa3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

"隐藏"作用域中的变量和函数还能带来另为一个好处:**可以避免同名标识符之间的冲突**,比如一个经典的错例:     
![sc4.png](http://upload-images.jianshu.io/upload_images/3001083-518a72cf7b2847fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        
![sc5.png](http://upload-images.jianshu.io/upload_images/3001083-dc02c15130da05b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc6.png](http://upload-images.jianshu.io/upload_images/3001083-364e0ac977de31ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### 1.函数作用域(接上文)       
![sc7.png](http://upload-images.jianshu.io/upload_images/3001083-c5644988c33fc299.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc8.png](http://upload-images.jianshu.io/upload_images/3001083-2071fd728e233051.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

##### 1.1 匿名和具名       
![sc9.png](http://upload-images.jianshu.io/upload_images/3001083-70e79e0adfe1fa76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc10.png](http://upload-images.jianshu.io/upload_images/3001083-75e053cb1e63013f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

##### 1.2 立即执行函数(IIFE)     
![sc11.png](http://upload-images.jianshu.io/upload_images/3001083-3a05a5647add009d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc12.png](http://upload-images.jianshu.io/upload_images/3001083-2b0cc80dfd2dbf3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc13.png](http://upload-images.jianshu.io/upload_images/3001083-5f94957397119618.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc14.png](http://upload-images.jianshu.io/upload_images/3001083-157129cf5368e9c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        
**注意最后这里是先运行IIFE,后面的def函数被当作参数传进去,而真正运行函数是def**     

#### 3. 块作用域       
**尽管函数作用域是最常见的作用域单元,当然也是现行大多数JavaScript中最普遍的设计方法,但其他类型的作用域单元也是存在的,并且通过使用其他类型的ufology单元甚至可以实现维护起来更加优秀、简洁的代码.**     
**除JavaScript外的很多编程语言都支持块作用域,但在《JavaScript高级程序设计》第三版中，针对ES5说JavaScript目前是没有块作用域的，但这种说法其实不太严密，就算是截止到ES5，实际上也是有一些块作用域的，只是用得并不多而已：**     

``` javascript
if (true) {
	var a = 1;
}

for (var i = 0; i < 10; i++) {
	//console.log(i);
}

console.log(a);    //1
console.log(i);    //10
```
**可见，看似有块作用域功能的if 和 for都只是空有其表，若不注意，经常会在代码中因此犯下错误。**     
**那么，上面说的ES5中的块作用域是哪些呢？答案是：1.with 2.try/catch。**     

``` javascript
try {
	undefined();  //使用一个非法操作来制造一个异常。
} catch(err) {
	console.log(err);  //输出err
}

console.log(err);    //ReferenceError: err not fount
```

**但其实，相对于块作用域能提供的好处来说，它俩一个是不建议使用，一个是很少用来使用具有块作用域特点的功能，因此，截至到ES5,还是没有很好的块作用域的机制。**     

![sc15.png](http://upload-images.jianshu.io/upload_images/3001083-d95afc18d41e55a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc16.png](http://upload-images.jianshu.io/upload_images/3001083-02c4570fb3314547.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc17.png](http://upload-images.jianshu.io/upload_images/3001083-01a7a74736d86af3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc18.png](http://upload-images.jianshu.io/upload_images/3001083-4a46cd0bd24fd6de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc19.png](http://upload-images.jianshu.io/upload_images/3001083-a1587a676df1bcd7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc20.png](http://upload-images.jianshu.io/upload_images/3001083-7bda7ff263a02789.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

除了let意外，ES6还引入了**const**，同样可以用来创建块作用域变量，但其值是固定的，之后**任何试图修改值的操作都会引起错误。**参考如下代码：     

``` javascript
var foo = true;
if (foo) {
	var a = 2;
	const b = 3;   //包含在if中的块作用域常量

	a = 3;         //正常
	b = 4;         //错误 
}

console.log(a);    //3
console.log(b);    //ReferenceError!
```

#### 3.小结       
![sc21.png](http://upload-images.jianshu.io/upload_images/3001083-baaa96dd412539de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 五、提升       
![sc22.png](http://upload-images.jianshu.io/upload_images/3001083-4c174cabe74e627e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc23.png](http://upload-images.jianshu.io/upload_images/3001083-58e5b4867a16b2bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc24.png](http://upload-images.jianshu.io/upload_images/3001083-684580ec661b181a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc25.png](http://upload-images.jianshu.io/upload_images/3001083-c7db7aea45ec3dd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc26.png](http://upload-images.jianshu.io/upload_images/3001083-56f1a79908d22a09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![sc27.png](http://upload-images.jianshu.io/upload_images/3001083-7f9a61269c490c45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     


我们知道，函数声明和变量声明都会被提升，那么问题来了，它俩是谁先被提升呢？答案是：**函数优先**。     
考虑下面代码：     

``` javascript
foo();    //注意这里会输出1
var foo;
function foo() {
	console.log(1);
}
foo = function() {
	console.log(2);
}
```

在理解为啥上面会输出1之前，我们先看看重复声明会怎样：     

``` javascript
var a = 2;
var a = 3;          //编译器会忽略var，只是做一个赋值操作
console.log(a);     //3
```

可见，重复的声明会被编译器自动忽略掉，在看看上面的代码，根据**函数优先提升**的原则，在**编译器编译后**代码会被提升成这样：     

``` javascript
function foo() {
	console.log(1);
}
var foo;  //产生了重复声明
foo();    //1
foo = function() {
	console.log(2);
}
```

上面的代码中还有一点小错误，那就是产生了重复声明，因此，编译器会忽略掉后面的声明，因此**经过编译器编译后的最终代码，也即引擎看到的代码**应为：     

``` javascript
function foo() {
	console.log(1);
}
foo();    //1
foo = function() {
	console.log(2);
}
```

##### 小结：     

![sc28.png](http://upload-images.jianshu.io/upload_images/3001083-872072afbbd60e3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![sc29.png](http://upload-images.jianshu.io/upload_images/3001083-d8243e48946d68c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     


### 相关文章链接    
 - [Javascript深入学习1——词法结构、类型、值、变量](http://liveipool.com/blog/2016/09/12/learn-javascript-1/)       
 - [JavaScript红皮书学习(1,2)--JavaScript简介](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-1,2-Introduction/)  
 - [JavaScript红皮书学习(3)--基本概念](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-3-BasicConcepts/)   
 - [JavaScript红皮书学习(4)--变量、作用域和内存问题](http://liveipool.com/blog/2016/12/19/JavaScript-RedBook-4-Variable-Scope-and-Memory/)    
 - [JavaScript红皮书学习(5)--引用类型](http://liveipool.com/blog/2016/12/22/JavaScript-RedBook-5-Reference-Type)     
 - [JavaScript学习专题之--作用域](http://liveipool.com/blog/2016/12/22/JavaScript-Scope)   
 - [JavaScript学习专题之--闭包](http://liveipool.com/blog/2016/12/23/JavaScript-Closures)     

