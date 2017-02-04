---
layout: post
title: ES6 标准入门(11)--Proxy 和 Reflect  
date: 2017-02-04
categories: blog
description: JavaScript学习
---

# Proxy 和 Reflect      
这一部分的内容不太好总结，内容比较新，也不算太多，阮一峰老师讲得非常详细。全新的内容，有空多看，多打几遍。
先看关于Reflect的内容，再看Proxy;      
[Reflect：http://es6.ruanyifeng.com/#docs/reflect](http://es6.ruanyifeng.com/#docs/reflect)      
[Proxy：http://es6.ruanyifeng.com/#docs/proxy](http://es6.ruanyifeng.com/#docs/proxy)      

```
var proxy = new Proxy(target, handler);
```