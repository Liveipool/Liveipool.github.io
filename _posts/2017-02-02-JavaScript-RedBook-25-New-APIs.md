---
layout: post
title: JavaScript红皮书学习(25)--新兴的API
date: 2017-02-02
categories: blog
description: JavaScript学习
---

# 新兴的API      
**微软是ms，Chrome和Safari是webkit， firefox是moz。**      

## 25.2 Page Visibility API      
![25.2.png](http://upload-images.jianshu.io/upload_images/3001083-06b8d26d4582d29c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

就现在来说，document.visibilityState的属性只有visible和hidden被浏览器广泛支持，prerender和unload还未被完全支持。      

## 25.3 Geolocation API      
![25.31.png](http://upload-images.jianshu.io/upload_images/3001083-a87f1ca2d59b78cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.32.png](http://upload-images.jianshu.io/upload_images/3001083-6fc3535364fe452c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.33.png](http://upload-images.jianshu.io/upload_images/3001083-a3db7adc6465f099.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.34.png](http://upload-images.jianshu.io/upload_images/3001083-ac055d2554406003.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 25.4 File API      
![25.4.01.png](http://upload-images.jianshu.io/upload_images/3001083-ce90a89e93b8ca5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.02.png](http://upload-images.jianshu.io/upload_images/3001083-81f625d05cb5b064.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 25.4.1 FileReader 类型      
![25.4.11.png](http://upload-images.jianshu.io/upload_images/3001083-f37ca37481887720.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.12.png](http://upload-images.jianshu.io/upload_images/3001083-acef607502763d59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.13.png](http://upload-images.jianshu.io/upload_images/3001083-8ab35998a01af781.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.14.png](http://upload-images.jianshu.io/upload_images/3001083-350ddcf00fb5eff5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.15.png](http://upload-images.jianshu.io/upload_images/3001083-c3bb8edda946e2b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 25.4.2 读取部分内容      
![25.4.21.png](http://upload-images.jianshu.io/upload_images/3001083-69e39a9fecc76a07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.22.png](http://upload-images.jianshu.io/upload_images/3001083-77e0c39adf12a4d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 25.4.3 对象URL      
![25.4.31.png](http://upload-images.jianshu.io/upload_images/3001083-2adfc5219d549495.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.32.png](http://upload-images.jianshu.io/upload_images/3001083-dc59230ecac2c516.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.33.png](http://upload-images.jianshu.io/upload_images/3001083-5f97916e53838968.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.34.png](http://upload-images.jianshu.io/upload_images/3001083-f8e56e3586833dfc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 25.4.4 读取拖放的文件      
![25.4.41.png](http://upload-images.jianshu.io/upload_images/3001083-5be1e1b4e64fa96d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.42.png](http://upload-images.jianshu.io/upload_images/3001083-fc7a4bf756117bcf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 25.4.5 使用XHR上传文件      
![25.4.51.png](http://upload-images.jianshu.io/upload_images/3001083-e152f1541e424f6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.4.52.png](http://upload-images.jianshu.io/upload_images/3001083-1afc787d5e529468.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

**使用FormData上传文件，服务器端就好像是接收到了常规的表单数据一样，一切按部就班地处理即可。**      

## 25.5 Web 计时      
![25.51.png](http://upload-images.jianshu.io/upload_images/3001083-557631f48174b091.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.52.png](http://upload-images.jianshu.io/upload_images/3001083-b8be4c2eb35f5fc2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.53.png](http://upload-images.jianshu.io/upload_images/3001083-59133a5fd5ac996a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 25.6 Web Workers      
![25.6.png](http://upload-images.jianshu.io/upload_images/3001083-afb4bd917dccfcd5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 25.6.1 使用Worker      
![25.6.1.png](http://upload-images.jianshu.io/upload_images/3001083-3abe77d0d827dc59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.6.11.png](http://upload-images.jianshu.io/upload_images/3001083-f6fd1fb73de8ac94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 25.6.2 Worker 全局作用域      
![25.6.21.png](http://upload-images.jianshu.io/upload_images/3001083-be726e6b9455f1dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.6.22.png](http://upload-images.jianshu.io/upload_images/3001083-2d66ca809f6ca52f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.6.23.png](http://upload-images.jianshu.io/upload_images/3001083-cbcda7c3f7fdf3fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 25.6.3 包含其他脚本      
![25.6.31.png](http://upload-images.jianshu.io/upload_images/3001083-96cf1f902bbeda65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![25.6.32.png](http://upload-images.jianshu.io/upload_images/3001083-4fa3eb40de64a995.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 25.7 小结      
![25.7.png](http://upload-images.jianshu.io/upload_images/3001083-efa13ca4cc6091a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      