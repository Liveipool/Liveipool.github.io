---
layout: post
title: JavaScript红皮书学习(22)--离线应用与客户端存储
date: 2017-02-02
categories: blog
description: JavaScript学习
---

# 离线应用与客户端存储      
![23.0.png](http://upload-images.jianshu.io/upload_images/3001083-e416929c492bcf74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 23.1 离线检测
开发离线应用的第一步是要知道设备是在线还是离线，HTML5为此定义了一个navigator.onLine属性，这个属性值为true表示设备能上网，但这个属性在各浏览器上的支持情况稍有差异，慎用。      
![23.1.png](http://upload-images.jianshu.io/upload_images/3001083-4cecc4c3fb9b6f9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 23.2 应用缓存   
[应用缓存 w3school](http://www.w3school.com.cn/html/html5_app_cache.asp)          
![23.21.png](http://upload-images.jianshu.io/upload_images/3001083-f7d380e30a2c7aab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.22.png](http://upload-images.jianshu.io/upload_images/3001083-0540a07d88b91183.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.23.png](http://upload-images.jianshu.io/upload_images/3001083-53adf2a35c2ea5b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
**现在还没有需要连接远程服务器的项目，在本地没法学习，以后试试。**      

## 23.3 数据存储      
![23.3.png](http://upload-images.jianshu.io/upload_images/3001083-d66f3223ed62e44c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 23.3.1 Cookie      
[Document.cookie: https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie)      
![23.3.11.png](http://upload-images.jianshu.io/upload_images/3001083-30a36c5620c54144.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.12.png](http://upload-images.jianshu.io/upload_images/3001083-aa15ce2e96aa9580.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.13.png](http://upload-images.jianshu.io/upload_images/3001083-750067566e031b99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.14.png](http://upload-images.jianshu.io/upload_images/3001083-43daec9ee56ede6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.15.png](http://upload-images.jianshu.io/upload_images/3001083-b1e953284a4b500f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.16.png](http://upload-images.jianshu.io/upload_images/3001083-c784b1c4a8eb41b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.17.png](http://upload-images.jianshu.io/upload_images/3001083-1ca32ff3aa551e94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.18.png](http://upload-images.jianshu.io/upload_images/3001083-70f16946cacb6de5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.19.png](http://upload-images.jianshu.io/upload_images/3001083-e57364cafd69fe63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.191.png](http://upload-images.jianshu.io/upload_images/3001083-8698e7d98975837c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
**具体实现可查看红皮书或网上，原理类似。**      
![23.3.192.png](http://upload-images.jianshu.io/upload_images/3001083-52500326aeec4c8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 23.3.3 Web存储机制      
![23.3.31.png](http://upload-images.jianshu.io/upload_images/3001083-528224cf38d7046f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.32.png](http://upload-images.jianshu.io/upload_images/3001083-07756eaf241d9d02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.33.png](http://upload-images.jianshu.io/upload_images/3001083-4e3cf8bc5fe4f52c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
**Storage类型只能存储字符串。非字符串的数据在存储之前会被转换成字符串。**      

**sessionStorage 和 localStorage 对象，参考：[http://www.w3school.com.cn/html5/html_5_webstorage.asp](http://www.w3school.com.cn/html5/html_5_webstorage.asp)**      

![23.3.34.png](http://upload-images.jianshu.io/upload_images/3001083-2b128c6a77cf2d5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.36.png](http://upload-images.jianshu.io/upload_images/3001083-238ce60029b4399c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.37.png](http://upload-images.jianshu.io/upload_images/3001083-2ee795f5b86a0131.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.38.png](http://upload-images.jianshu.io/upload_images/3001083-69475ec55fef4f05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.39.png](http://upload-images.jianshu.io/upload_images/3001083-02f4707881721d93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

### 23.3.4 IndexedDB      
![23.3.41.png](http://upload-images.jianshu.io/upload_images/3001083-0f743a29fa32b4c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.42.png](http://upload-images.jianshu.io/upload_images/3001083-99983afa1d10b771.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.43.png](http://upload-images.jianshu.io/upload_images/3001083-ae3feafe47da95b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.44.png](http://upload-images.jianshu.io/upload_images/3001083-935834d413a743ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.45.png](http://upload-images.jianshu.io/upload_images/3001083-0781e5d83084420e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.46.png](http://upload-images.jianshu.io/upload_images/3001083-1ead30daa77d5ef5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.47.png](http://upload-images.jianshu.io/upload_images/3001083-26c167bec5ade53e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.48.png](http://upload-images.jianshu.io/upload_images/3001083-a167a0b4c58300bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.49.png](http://upload-images.jianshu.io/upload_images/3001083-9c374f87b2fa0273.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.491.png](http://upload-images.jianshu.io/upload_images/3001083-2a614480095b973a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.492.png](http://upload-images.jianshu.io/upload_images/3001083-373b2664e0c272df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.493.png](http://upload-images.jianshu.io/upload_images/3001083-d221ad8ae044ace8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.494.png](http://upload-images.jianshu.io/upload_images/3001083-396d87792d6a42f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.495.png](http://upload-images.jianshu.io/upload_images/3001083-aa15e3769c11b071.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.496.png](http://upload-images.jianshu.io/upload_images/3001083-5226b92a5cd87a86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.497.png](http://upload-images.jianshu.io/upload_images/3001083-9425744984316c73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.498.png](http://upload-images.jianshu.io/upload_images/3001083-4d48f46db37d0100.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.3.499.png](http://upload-images.jianshu.io/upload_images/3001083-9457b77b5ae7468b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        
![23.3.4991.png](http://upload-images.jianshu.io/upload_images/3001083-0004123551f37abb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

## 23.4 小结      
![23.41.png](http://upload-images.jianshu.io/upload_images/3001083-7e829a54fd361427.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
![23.42.png](http://upload-images.jianshu.io/upload_images/3001083-cbeb51caee1ccd62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      
      