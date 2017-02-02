---
layout: post
title: JavaScript红皮书学习(22)--高级技巧
date: 2017-02-02
categories: blog
description: JavaScript学习
---

# 高级技巧      

## 22.1 防篡改对象      
![22.2.png](http://upload-images.jianshu.io/upload_images/3001083-f658fffedde2fc10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 22.1.1 不可扩展对象      
默认情况下，所有对象都是可扩展的。      
![22.2.1.png](http://upload-images.jianshu.io/upload_images/3001083-555b71cfe04c22fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 22.1.2 密封的对象      
![22.2.2.png](http://upload-images.jianshu.io/upload_images/3001083-97039b72ac171621.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 22.1.3 冻结的对象      
![22.2.3.png](http://upload-images.jianshu.io/upload_images/3001083-06b94fc81d375f98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 22.2 高级定时器      
JavaScript是运行于单线程的环境中的，执行时机是不能保证的，因为在页面的生命周期中，不同事件可能有其他代码在控制JavaScript进程。      
在JavaScript中没有任何代码是立刻执行的，但一旦进程空闲则尽快执行。      

### 22.2.2 Yielding Processes      
![22.3.21.png](http://upload-images.jianshu.io/upload_images/3001083-2896a37b0913fb2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![22.3.22.png](http://upload-images.jianshu.io/upload_images/3001083-d0e135ad1222fa6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![22.3.23.png](http://upload-images.jianshu.io/upload_images/3001083-dda838cf087aa351.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
**一旦某个函数需要花50ms以上的时间完成，那么最好看看能否将任务分割成一系列可以使用定时器的小任务。**      

### 22.2.3 函数节流      
![22.3.3.png](http://upload-images.jianshu.io/upload_images/3001083-ce1dea5201f79cc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
      
## 22.3 观察者模式      
![22.4.png](http://upload-images.jianshu.io/upload_images/3001083-affa2dd95b3507c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 22.4 小结      
![22.61.png](http://upload-images.jianshu.io/upload_images/3001083-bbacaee4245db558.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![22.62.png](http://upload-images.jianshu.io/upload_images/3001083-d9a63c83c9036f8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      


