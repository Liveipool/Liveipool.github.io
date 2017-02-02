---
layout: post
title: JavaScript红皮书学习(24)--最佳实践
date: 2017-02-02
categories: blog
description: JavaScript学习
---

# 最佳实践     

### 24.1.1 可维护的代码     
![24.1.11.png](http://upload-images.jianshu.io/upload_images/3001083-f20209bafa38fd37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.1.12.png](http://upload-images.jianshu.io/upload_images/3001083-f301d171f6b00ab3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 24.1.2 解耦应用逻辑 和 事件处理程序     
![24.1.13.png](http://upload-images.jianshu.io/upload_images/3001083-f596c95a87572b4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.1.14.png](http://upload-images.jianshu.io/upload_images/3001083-85629498de43a3ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 24.1.3 使用常量     
![24.1.15.png](http://upload-images.jianshu.io/upload_images/3001083-8ccdd2c23bfb8415.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.1.16.png](http://upload-images.jianshu.io/upload_images/3001083-e0f46b36bb13f235.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 24.2 性能     
![24.2.png](http://upload-images.jianshu.io/upload_images/3001083-79db1c6a22345d17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 24.2.1 注意作用域     
![24.2.11.png](http://upload-images.jianshu.io/upload_images/3001083-e2bc47f8b38cf42d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.2.12.png](http://upload-images.jianshu.io/upload_images/3001083-285f7b7ba175d38d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 24.2.2 选择正确方法     
![24.2.21.png](http://upload-images.jianshu.io/upload_images/3001083-c0d39d27790af940.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.2.22.png](http://upload-images.jianshu.io/upload_images/3001083-2a62af9bd122b487.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

**这里，变量i每次循环之后都会减1。在这个过程中，将终止条件从value.length的O（n）调用简化成了0的O（1）调用。**

![24.2.23.png](http://upload-images.jianshu.io/upload_images/3001083-1797a3ad6cd76eb8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![24.2.24.png](http://upload-images.jianshu.io/upload_images/3001083-ab82dcaaa69c3405.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![24.2.25.png](http://upload-images.jianshu.io/upload_images/3001083-03a13c0c07f7c304.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.2.26.png](http://upload-images.jianshu.io/upload_images/3001083-151b9dc8e49acfc9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 24.2.3 最小化语句数     
![24.2.31.png](http://upload-images.jianshu.io/upload_images/3001083-7d1d1137ee22f814.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.2.32.png](http://upload-images.jianshu.io/upload_images/3001083-9279ecccc3747e08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.2.33.png](http://upload-images.jianshu.io/upload_images/3001083-9c4cdd2728fd6e06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 24.2.4 优化DOM交互     
![24.2.41.png](http://upload-images.jianshu.io/upload_images/3001083-c294debb76a5072d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.2.42.png](http://upload-images.jianshu.io/upload_images/3001083-bc22a74e522ec416.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.2.43.png](http://upload-images.jianshu.io/upload_images/3001083-7787225b877c820d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![24.2.44.png](http://upload-images.jianshu.io/upload_images/3001083-2c62763dabfa0042.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.2.45.png](http://upload-images.jianshu.io/upload_images/3001083-17c65d070fed88ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.2.46.png](http://upload-images.jianshu.io/upload_images/3001083-ffab6fe48050a52c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

```
// 根据前面提到的减值迭代，这样还要快一些：
var images = document.getElementsByTagName("img"),
	i,
	len = images.length;

for (i = len; i > 0; i--) {
	//处理
}
```

## 24.3 部署     
![24.3.png](http://upload-images.jianshu.io/upload_images/3001083-729dd1279d0fced4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 24.3.1 构建过程     
![24.3.11.png](http://upload-images.jianshu.io/upload_images/3001083-0b1c552a98b655e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.3.12.png](http://upload-images.jianshu.io/upload_images/3001083-9cd2f634153eb14b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 24.3.2 压缩     
![24.3.31.png](http://upload-images.jianshu.io/upload_images/3001083-71d1bfe081884dc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![24.3.32.png](http://upload-images.jianshu.io/upload_images/3001083-43df334c1d6f6cdd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 24.4 小结     
![24.4.png](http://upload-images.jianshu.io/upload_images/3001083-34eb66060e106c46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
