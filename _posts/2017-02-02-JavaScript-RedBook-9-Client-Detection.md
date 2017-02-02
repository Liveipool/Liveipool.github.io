---
layout: post
title: JavaScript红皮书学习(9)--客户端检测
date: 2017-02-02
categories: blog
description: JavaScript学习
---

# 客户端检测      
![9.0.png](http://upload-images.jianshu.io/upload_images/3001083-94ec13d7b32f3233.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
**在实际开发中，应将能力检测作为下一步解决方案的一句，而不是用它来判断用户使用的是什么浏览器！**      

## 9.1 能力检测      
![9.11.png](http://upload-images.jianshu.io/upload_images/3001083-12514c0293153d42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![9.12.png](http://upload-images.jianshu.io/upload_images/3001083-3636d018c006b941.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
            
### 9.1.1 更可靠的能力检测      
![9.1.11.png](http://upload-images.jianshu.io/upload_images/3001083-a9012be24ea4ff5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![9.1.12.png](http://upload-images.jianshu.io/upload_images/3001083-a976632a3d5ce335.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![9.1.13.png](http://upload-images.jianshu.io/upload_images/3001083-e97b6eadd8656cf7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 9.1.2 能力检测，而不是浏览器检测      
![9.1.21.png](http://upload-images.jianshu.io/upload_images/3001083-d5fa3dbab56d2f36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
**以上例子展示了两个检测：一个检测浏览器是否支持Netscape风格的插件;另一个检测浏览器是否具备DOM1级所规定的能力。得到的布尔值可以在以后继续使用，从而节省重新检测能力的时间。**      
**再次强调，在实际开发中，应将能力检测作为确定下一步解决方案的依据，而不是用它来判断用户使用的是什么浏览器。**      
      
## 9.2 怪癖检测      
![9.21.png](http://upload-images.jianshu.io/upload_images/3001083-9d9f54dcc7866f19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![9.22.png](http://upload-images.jianshu.io/upload_images/3001083-c342e28cacccd723.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![9.23.png](http://upload-images.jianshu.io/upload_images/3001083-e0091c5b8715e7c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 9.3 用户代理检测      
![9.31.png](http://upload-images.jianshu.io/upload_images/3001083-a1cad7d831e7eb4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 9.4 小结      
![9.4.png](http://upload-images.jianshu.io/upload_images/3001083-28845ed6368a0d93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      