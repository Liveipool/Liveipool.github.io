---
layout: post
title: 原型--更深入的学习和总结
date: 2017-02-21
categories: blog
description: JavaScript学习
---

# 原型--更深入的学习和总结         
在以前看红皮书时，也学习总结过原型的概念，上面的内容讲得较为的清晰和简洁，虽然易懂，但可能还有一些具体的细节没有深入讲到，今天通过《你不知道的JavaScript》来深入学习和总结一下。         

#### 首先复习一下最基本的概念         
 - 牢记三个重要概念: **构造函数(constructor)**、**原型对象(Prototype)**、**实例**.               
 - 我们创建的每个函数都有一个**prototype(原型)属性**,这个属性是一个**指针**,指向**原型对象**.               
 - 默认情况下,每个**原型对象**也都有一个**constructor指针**,它指向**构造函数**.               
 - 每个**实例**都有一个**[[Prototype]]指针**,指向**原型对象**.               
![6.2.3.png](http://upload-images.jianshu.io/upload_images/3001083-ad676ed44eef0a35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)               

## [[Prototype]]         
每个实例都有一个[[Prototype]]指针，指向原型对象。         

```
var myObject = {
	a:2
};

myObject.a;  // 2
```
当我们试图引用对象的属性时，如myObject.a，就会触发[[Get]]操作。对于默认的[[Get]]操作来说，第一步是检查对象本身是否有这个属性，如果有那就使用它。         
**但是如果a不在myObject中，就需要使用对象的[[Prototype]]链了。[[Get]]操作会一直找到[[Prototype]]链的尽头，直到找到为止。**         

#### Object.prototype         
但哪里是[[Prototype]]的尽头呢？所有普通的[[Prototype]]链最终都会指向内置的**Object.prototype**。         
因为所有的内置对象都“源于”（或者说把[[Prototype]]链的顶端设置为）这个Object.prototype对象，所以它包含JavaScript中许多通用的功能（如toString(), valueOf()等）。         

#### 属性设置和屏蔽         
**给一个对象设置属性并不仅仅是添加一个新属性或者修改已有的属性值。**         
比如下面这段代码的执行过程：         

```
myObject.foo = "bar";
```
 1. 如果myObject对象中包含名为foo的普通数据访问属性，这条赋值语句**只会修改已有的属性值**。         
 2. 如果foo不是直接存在与myObject中，[[Prototype]]链就会被遍历，类似[[Get]]操作。**如果原型链上找不到foo，foo就会被直接添加到myObject上。**         
 3. 然而，如果foo存在于原型链上层，这个赋值语句就可能有下面几种情况：         
   - 如果在[[Prototype]]链上层存在名为foo的**普通数据访问属性并且没有被标记为只读（writable：false）**，那就会直接在myObject中添加一个名为foo的新属性，它是**屏蔽属性**。         
   - 如果在[[Prototype]]链上层存在foo，但它被标记为**只读**，若在严格模式下会报错，若不在，这条赋值语句会被忽略。总之不会发生屏蔽。         
   - 如果在[[Prototype]]链上层存在foo并且它是一个setter，那就会调用这个setter。foo不会被添加到myObject，也不会重新定义foo这个setter。         

有几个值得注意的地方：         
1. **屏蔽属性**是指如果foo既出现在myObject中也出现在myObject的[[Prototype]]链上层，那么就会发生屏蔽。myObject中包含的foo属性会屏蔽原型链上层的所有foo属性。         
2. 如果希望在上面说的最后两种小情况中也屏蔽foo，不能用=操作符来赋值，而是使用Object.defineProperty()来向myObject添加foo。         
3. 在上面的某些情况中，myObject对象竟会因为**原型链上的某个对象中有一个只读foo就不能包含foo属性**，这看起来有点奇怪但却是正常的。         

#### 隐式屏蔽         

```
var firstObject = {
	a: 2
};
var secondObject = Object.create(firstObject);

firstObject.a; //2
secondObject.a;  //2
firstObject.hasOwnProperty('a');  //true
secondObject.hasOwnProperty('a');  //false

secondObject++;  //隐式屏蔽

firstObject.a; //2
secondObject.a;  //3

secondObject.hasOwnProperty('a');  //true
```

####  Object.create()         
Object.create() 方法创建一个拥有指定原型和若干个指定属性的对象。         

```
Object.create(proto, [ propertiesObject ])
```
proto：一个对象，作为新创建对象的原型。或者为 null。         
propertiesObject：可选。该参数对象是一组属性与值。         
**ES5通过Object.create()方法来规范化了原型式继承。**         

Object.create(null)会创建一个拥有空[[Prototype]]链接的对象，这个对象无法进行委托。这种特殊的对象常被称作“字典”，因为它们不会受到原型链的干扰，非常适合用来存储数据。         

**ES6中添加了辅助函数Object.setPrototypeOf()，可以用标准并且可靠的方法来修改关联。可在某些场合代替Object.create()使用：**         

```
//两种把Bar.prototype关联到Foo.prototype的方法：

//ES6之前需要抛弃默认的Bar.prototyoe
Bar.prototype = Object.create(Foo.prototype);

//ES6开始可以直接修改现有的Bar.prototype
Object.setPrototypeOf(Bar.prototype, Foo.prototype);
```

#### __ proto __         
在ES5中，要想获取一个对象的[[Prototype]]链，标准的方法是：         

```
Object.getPrototypeOf(a);
```

绝大多数浏览器也支持一种非标准（**在ES6中成了标准**）的方法来访问内部[[Prototype]]属性：          

```
a.__proto__ === Foo.prototype;  //true
```
和constructor属性一样， __ proto __ 实际上并不存在于a中，而是存在于内置的Object.prototype中。         

它看起来很像一个属性，但实际上它更像一个getter/setter，它的实现大致如下：         

```
Object.defineProperty(Object.prototype, "__proto__", {
	get: function() {
		return Object.getPrototypeOf(this);
	},
	set: function(o) {
		//ES6中的setPrototypeOf方法
		Object.setPrototypeOf(this, o);
		return o;
	}
});
```

## “类”函数         
多年依赖，**JavaScript中一直有种奇怪的行为：模仿类。**         

```
function foo() {}
foo.prototype;  //
``` 
在面向类的语言中，或者说有真正的类的语言中，**类可以被复制多次**，就像用模具制作东西一样。         
**但是在JavaScript中，并没有类似的复制机制。你不能创建一个类的多个实例，只能创建多个对象，它们的[[Prototype]]关联的是同一个对象。**         

看下面的例子：         

```
function foo() {}
var a = new foo();
Object.getPrototypeOf(a) === foo.prototype; //true
```
调用new foo()时会执行以下四步操作：         
1. 创建一个全新的对象。         
2. **将这个新对象内部的[[Prototype]]链接到foo.prototype指向的对象。**         
3. 这个新对象会绑定到函数调用的this。         
4. 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。         

在这里我们主要看第二点，最后我们并没有初始化一个类，实际上我们也并没有从“类”中复制任何行为到一个对象中，只是让两个对象相互关联了起来。         

实际上，绝大多数JavaScript开发者不知道的秘密是，new foo()这个函数调用实际上并没有直接创建关联，这个关联只是一个意外的副作用。new foo()只是间接完成了我们的目标：一个关联到其他对象的新对象。         

#### 关于名称         
在JavaScript开发中，我们经常会提到一种**原型继承**机制，它常常被视为动态语言版的**类继承。**         
但是，《你不知道的JS》的作者认为：**JavaScript中的“继承”和类继承其实有很大的差别，这个容易混淆的组合术语“原型继承”（以及使用其它面向类的术语比如“类”，“构造函数”，“实例”，“多态”等等）严重影响了大家对JavaScript机制真实原理的理解。**         

**继承**意味着**复制操作**，**JavaScript（默认）并不会复制对象属性。**
相反，JavaScript会在两个对象之间**创建一个关联**，这样一个对象就可以通过**委托**访问另一个对象的属性和函数，因此，**委托**这个术语可以**更加准确地描述JavaScript中对象的关联机制。**
         
## “构造函数”         
看下面这段代码：         

```
function Foo() {}
Foo.prototype.constructor === Foo;  //true
var a = new Foo();
a.constructor === Foo; //true
```
**(实际上a本身并没有.constructor属性。而且，虽然a.constructor确实指向Foo函数，但是这个属性并不是表示a由Foo“构造”，这个.constructor属性实际上是在Foo.prototype中的。)**         

为什么我们大多数人会将Foo看为一个“类”呢？一是因为是用了new，而是看起来是执行了类的构造函数方法Foo()。         
但终究JavaScript中是没有类的。         
**那如何来理解所谓的“构造函数呢？”**         

实际上，上面的Foo()和其他普通函数没有任何区别。函数本身并不是构造函数，然而，当你在普通的函数调用前面加上new关键字后，就会把这个函数调用变为一个**“构造函数调用”**。         
举例来说：         

```
function NothingSpecial() {
	console.log("lala");
}

var a = new NothingSpecial(); //lala
a; //NothingSpecial {}
```
换句话说，在JavaScript中对于“构造函数”的最准确的解释是，**所有带new的函数调用。**         
**函数不是构造函数，但是当且仅当使用new时，函数调用会变成“构造函数调用”。**         

#### a.constructor         
多说几句上文中提到的a本身并没有constructor这个属性。考虑下面的代码：         

```
function Foo() {}
Foo.prototype = {}
var a1 = new Foo();
a1.constructor === Foo;  //false
a1.constructor === Object;  //true
```
这段代码和上文唯一的不同在于它创建了一个新的原型对象Foo.prototype。这个新的原型对象没有constructor属性（默认情况下是有的。）         
在读取a.constructor时，a并没有constructor属性，所以它会委托[[Prototype]]链上的Foo.prototype，然而这次也找不到constructor属性了，所以会继续委托给委托链顶端的Object.prototype。这个对象有constructor属性，因此得到了上面的结果。         
         
在以前的学习中也学到过，使用字面量的方式创建原型对象会使constructor属性丢失，若想让它拥有constructor属性，需要通过defineProperty来定义：         

```
Object.defineProperty(Foo.prototype, "constructor", {
	enumerable: false,
	writable: true,
	configurable: true,
	value: Foo //让constructor指向Foo
})
```

**因此可以看出，a.constructor不一定会指向默认的函数引用，它非常的不可靠且不安全，应该尽量避免。**         

## 小结         
 - 如果要访问对象中并不存在的一个属性，[[Get]]操作就会查找对象内部[[Prototype]]关联的对象。这个关联关系实际上定义了一条“原型链”。         
 - 所有普通对象都有内置的Object.prototype，指向原型链的顶端。         
 - 关联两个对象最常用的方法是使用new关键词进行函数调用，在调用的四个步骤中会创建一个关联其他对象的新对象。         
 - 使用new调用函数时会把新对象的.prototype属性关联到“其他对象”。带new的函数调用通常被称为“构造函数调用”，尽管它们实际上和传统面向类语言中的类构造函数不一样。         
 - 虽然这些JavaScript机制和传统面向类语言中的“类初始化”和“类继承”很相似，但是JavaScript中的机制有一个核心区别，那就是**不会进行复制**，对象之间是通过内部的[[Prototype]]链相关联的。         
 - 处于各种原因，以“继承”结尾的术语（包括“原型继承”）和其他面向对象的术语都无法帮助你理解JavaScript的真实机制。         
 - 相比之下，**“委托”**是一个更合适的术语，因为对象之间的关系**不是复制而是委托。**         
 - 关于“委托”的更详细的了解等到仔细看完《你不知道的JS》第五章再来详细总结。         