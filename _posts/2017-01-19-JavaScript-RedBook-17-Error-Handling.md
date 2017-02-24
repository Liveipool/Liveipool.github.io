---
layout: post
title: JavaScript红皮书学习(17)--错误处理与调试
date: 2017-01-19
categories: blog
description: JavaScript学习
---

# 错误处理与调试     

### 17.1.1 try-catch语句     
如果try块中的任何代码发生了错误,就会立即退出代码执行过程,然后接着执行catch块.此时,catch块会接收到一个包含错误信息的对象.     
**而finnally语句则是,一经使用,则一定会执行.**     

#### 错误类型     
 - 执行代码期间可能会发生的错误有多种类型.每种错误都有对应的错误类型,而当错误发生时,就会抛出相应类型的错误对象.ECMA-262定义了下列7种错误类型:     
 - **Error**: Error是基类型,其他错误类型都继承自该类型.因此,所有错误类型共享了一组相同的属性.Error类型的错误很少见,如果有也是浏览器抛出的,这个基类型的主要目的是供开发人员抛出自定义错误.     
 - **EvalError**: 使用eval()函数发生异常时被抛出.     
 - **RangeError**: 数值超出相应范围时触发.     
 - **ReferenceError**: 通常是找不到对象的情况下触发.     
 - **SyntaxError**: 把语法错误的JavaScript字符串传入eval()函数时.     
 - **TypeError**: 在变量中保存着意外的类型时,或者访问不存在的方法时.     
 - **URIError**: 在使用encodeURI()或decodeURI(),而URI格式不正确时出现.很少见,因为这两个函数容错率很高.     

### 17.1.2 抛出错误     
![17.2.2.png](http://upload-images.jianshu.io/upload_images/3001083-d7e579a64bfd9333.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

还有一个console.assert()方法可以在需要进行如if判断时输出错误信息: [https://developer.mozilla.org/en-US/docs/Web/API/console/assert](https://developer.mozilla.org/en-US/docs/Web/API/console/assert).     
**建议在开发JavaScript代码的过程中,重点关注函数和可能导致函数执行失败的因素.良好的错误处理机制应该可以确保代码中只发生自己抛出的代码.**     


### 17.1.3 错误(error)对象     
任何没有通过try-catch处理的错误都会触发window对象的error事件.在任何Web浏览器中,onerror事件处理程序都不会创建event对象,但它可以接收三个参数: 错误消息,错误所在的URL和行号.     
![17.2.3.png](http://upload-images.jianshu.io/upload_images/3001083-0c45d3637f0dbf28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

**如下面这段代码及点击按钮之后的输出所示:**     
![17.2.32.png](http://upload-images.jianshu.io/upload_images/3001083-4edd06c386b76d06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![17.2.31.png](http://upload-images.jianshu.io/upload_images/3001083-57dd9e195cca1048.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 17.1.4 常见的错误类型     
由于JavaScript是松散类型的,而且也不会验证函数的参数,因此错误只会在代码运行期间出现.一般来说,需要关注三种错误:
**1. 类型转换错误.**     
通常发生在使用某个操作符时.     

**2. 数据类型错误.**     
主要是在将预料之外的值传递给函数的情况下:     

``` javascript
//不安全的函数,任何非字符串值的参数都会导致错误
function getQueryString(url) {
	var pos = url.indexOf("?");
	// ...
}
```
在上面这种情况下,最好是在使用indexOf()方法之前先检查参数是否为一个字符串类型的值.     
**大体上来讲,基本类型的值应该使用typeof来检测,而对象的值则应该使用instanceof来检测.在自己的项目中如果有统一的规范还好,但如果是在写一些面向公众的API时一定要执行类型检查,以确保函数始终能够正常地执行.**     

**3. 通信错误**     
如在使用Ajax向服务器发送数据时,未使用encodeURIComponent()对数据进行编码而URL不正确的情况.     

### 17.1.5 区分致命错误和非致命错误     
任何错误处理策略中最重要的一个部分,就是确定错误是否致命.     
     
非致命错误:     
1. 不影响用户的主要任务.     
2. 只影响页面的一部分.     
3. 可以恢复.     
4. 重复相同操作可以消除错误.     

致命错误:     
1. 应用程序根本无法继续进行.     
2. 错误明显影响了用户的主要操作.     
3. 会导致其他连带错误.     

### 17.1.6 把错误记录到服务器     
**开发Web应用程序过程中的一种常见的做法,就是集中保存错误日志,以便查找重要错误的原因.**     
![17.2.71.png](http://upload-images.jianshu.io/upload_images/3001083-e1ecca00d98508ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![17.2.72.png](http://upload-images.jianshu.io/upload_images/3001083-bb2483c506537d97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 17.2 小结     
错误处理对于今天复杂的Web应用程序开发而言至关重要.不能提前预测到可能发生的错误,不能提前采取恢复策略,可能导致较差的用户体验,最终引发用户不满.多数浏览器在默认情况下都不会向用户报告错误,因此在开发和调试期间需要启用浏览器的错误报告功能.然而,在投入运行的产品代码中,则不应该再有诸如此类的错误报告出现.     
下面是几种避免浏览器响应JavaScript错误的方法:     
1. 在可能发生错误的地方使用try-catch语句,这样还有机会以适当的方式对错误给出响应,而不必沿用浏览器处理错误的机制.     
2. 使用window.onerror事件处理程序,这种方式可以接受try-catch不能处理的所有错误.     
3. 另外,对任何Web应用程序都应该分析可能的错误来源,并指定处理错误的方案.     
4. 需要明确什么是致命错误,什么是非致命错误.     
5. 分析代码,以判断最可能发生的错误.主要原因可能如下:类型转换, 为充分检测数据类型, 发送给服务器或从服务器接收到的数据有错误.     
