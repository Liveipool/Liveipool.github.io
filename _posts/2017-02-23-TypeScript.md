---
layout: post
title: TypeScript
date: 2017-02-23
categories: blog
description: 前端学习
---

# TypeScript            
TypeScript，经常会听说这个名字，但也没怎么具体了解过，在这里大致了解一下。        

TypeScript是JavaScript类型的**超集**，它可以编译成纯JavaScript，它使用.ts作为文件扩展名。        
TypeScript可以在任何浏览器、任何计算机和任何操作系统上运行，并且是开源的。        
        
#### 始于JavaScript，归于JavaScript        
TypeScript从JavaScript开发者所熟悉的语法和语义开始。使用现有的JavaScript代码，包括流行的JavaScript库，并从JavaScript代码中调用TypeScript代码。        
TypeScript可以**编译**出纯净、 简洁的JavaScript代码，并且可以运行在任何浏览器上、Node.js环境中和任何支持ECMAScript 3（或更高版本）的JavaScript引擎中。        

#### 强大的工具构建 大型应用程序        
允许JavaScript开发者在开发JavaScript应用程序时使用高效的开发工具和常用操作比如**静态检查**和**代码重构**。        

#### 先进的 JavaScript			
TypeScript提供最新的和不断发展的JavaScript特性，包括那些来自2015年的ECMAScript和未来的提案中的特性。
这些特性为高可信应用程序开发时是可用的，但是会被编译成简洁的ECMAScript3（或更新版本）的JavaScript。

#### 类型注解        
类型注解在TypeScript中是记录函数或变量约束的简便方法。在这个示例中，我们想要在调用greeter函数时传入一个字符串类型参数。我们可以尝试在调用greeter函数时变为传入一个数组：        

````
function greeter(person: string) {
    return "Hello, " + person;
}
var user = [0, 1, 2];
document.body.innerHTML = greeter(user);
```

在命令行中运行TypeScript编译器：        

```
tsc greeter.ts
```
重新编译，将看到一个错误：        

```
greeter.ts(7,26): Supplied parameters do not match any signature of call target
```

#### 接口        
让我们进一步开发我们的demo。 在这里我们使用一个接口，它描述了具有firstName和lastName字段的对象。在TypeScript中，如果两个类型其内部结构兼容，那么这两种类型兼容。这使我们实现一个接口，仅仅只需必要的结构形状，而不必有明确的implements子句。        

```
interface Person {
    firstName: string;
    lastName: string;
}
function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}
var user = { firstName: "Jane", lastName: "User" };
document.body.innerHTML = greeter(user);
```

#### 类        
最后，让我们最后一次使用类来继续开发demo。TypeScript支持新的JavaScript特性，像基于类的面向对象编程的支持。
在这里，我们创建一个具有构造函数和一些公共字段的Student类。注意：**类和接口的良好配合使用，决定一个程序员的抽象水平**。        
此外，在构造函数参数中使用public是一种简写形式，它将自动创建具有该名称的属性。        

```
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```
再次运行tsc greeter.ts，你将看到生成的JavaScript代码和以前的一样。TypeScript中的类只是对于经常在JavaScript中使用了相同的基于原型的面向对象的简写。        

#### TypeScript的特点        
 - 现在看来，它最大的特点就是**它是JavaScript的超集，并且添加了可选的静态类型和基于类的面向对象编程。**         
 - 它是**强类型**的语言，而JavaScript是弱类型的，所以TypeScript可以帮助我们检查是否有类型不正确的低级错误。而它名字中的Type也说明了它最大的作用是可以进行**类型检查**。        
 - 在TypeScript中，可通过class、constructor、interface、inheritance、module等来简化我们的代码。        
 - **限制作用域**：方法可以通过public/priviate/protected来限制，变量可以通过let来限制。        

基于以上的特点，TypeScript可以让我们像真正写一个强类型的语言来编写JavaScript应用。还可以通过IDE或命令行编辑器来检查基本的错误。        
[关于强类型和弱类型的讲解](https://www.zhihu.com/question/19918532)        
![TypeScript.png](http://upload-images.jianshu.io/upload_images/3001083-6a704ce9789c756f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        
 
上面的内容就是大致浏览了一下TypeScript官网上的内容，距离要透彻掌握它还很远很远，这个需要在未来某个需要使用它的项目中深入研究并实践才能真正理解到它独到的地方。        
