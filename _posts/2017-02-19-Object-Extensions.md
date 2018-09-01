---
layout: post
title: 对象，一些补充的知识点  
date: 2017-02-19
categories: blog
description: JavaScript学习
---

# 对象，一些补充的知识点      

#### 为什么typeof null 会是object           
原理是这样的，不同的对象在底层都表示为二进制，在JavaScript中二进制前三位都为0的话会被判断为object类型，null的二进制表示是全0,自然前三位也为0。     

#### 值的存储方式     
对象里的值看似是在对象内部，但这只是它的表现形式。在引擎内部，这些值一般并不会存在对象容器内部。存储在对象容器内部的是这些属性的名称，他们就像指针（从技术角度来说就是引用）一样，指向这些值真正的存储位置。
在访问某个属性的值时，.a语法通常被称为“属性访问”，[’a‘]语法通常被称为“键访问”，它们访问的是同一个位置。这两种语法的主要区别在于，点（.）操作符要求属性名满足标识符的命名规范，而[“...”]语法可以接收任意UTF-8/Unicode字符串作为属性名。     

#### [object Object]     

```
var a={a:1,b:"str1"};
alert(a.toString());
```
就会显示结果：[object Object]，这是什么意思呢？
1. a是一个对象的实例。      
2. toString() 方法默认返回  "[object objectname]"      
3. 因此，小写的是方法返回的值的格式中默认的，大写的是对象的类型的名字Object     

#### 为数组添加值

```
var arr = ['a', 'b', 'c'];
arr.d = 'd';
console.log(arr);  //["a", "b", "c", d: "d"]
arr['4'] = 'e';
console.log(arr);  //["a", "b", "c", 4: "e", d: "d"]
arr[3] = 'f';
console.log(arr);  //["a", "b", "c", "f", "e", d: "d"]
```

#### 浅复制和深复制
浅复制是指改变复制出的新对象的值也会同时改变初始对象的值，因为浅复制实际上只是增加了一个该值的引用，因此复制出的对象的某个属性和初始对象的相同属性都是引用，指向的是同一个值。
而深复制是指，复制出的属性和值会出现在内存中的另外的地方，虽然复制出的对象和初始对象的某个相同属性依然都是引用，但它们指向的是内存中的不同的地方，但这两个地方里包含的值相同。
Object.assign()是浅赋值。

![赞赏码.jpeg](https://upload-images.jianshu.io/upload_images/3001083-f65814d1f594b39c.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
