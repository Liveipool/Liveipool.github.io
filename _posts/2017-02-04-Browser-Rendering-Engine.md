---
layout: post
title: 浏览器内核  
date: 2017-02-04
categories: blog
description: 前端学习
---

# 浏览器内核      
 - 浏览器最重要或者说核心的部分是“Rendering Engine”，可大概译为“**渲染引擎**”，不过我们一般习惯将之称为“**浏览器内核**”，负责对网页语法的**解释**（如HTML、JavaScript）并**渲染**（显示）网页。       
 - 所以，通常所谓的**浏览器内核**也就是浏览器所采用的**渲染引擎**，它决定了浏览器**如何显示网页的内容以及页面的格式信息**。      
 - 不同的浏览器内核对网页编写语法的解释也有不同，**因此同一网页在不同的内核的浏览器里的渲染（显示）效果也可能不同，这也是网页编写者需要在不同内核的浏览器中测试网页显示效果的原因。**      

### Trident（IE内核）      
 - 该内核程序在1997年的IE4中首次被采用，是微软在Mosaic代码的基础之上修改而来的，并沿用到现在的IE11，被普遍称作”IE内核”。      
 - Trident实际上是一款开放的内核，其接口内核设计的相当成熟，因此才有许多采用IE内核而非IE的浏览器**(壳浏览器)**涌现。      
 - IE从版本11开始，初步支持WebGL技术。IE8的JavaScript引擎是Jscript，IE9开始用Chakra，这两个版本区别很大，Chakra无论是速度和标准化方面都很出色。      
 - Chakra是一个由微软为其Internet Explorer 9及以上版本和Microsoft Edge等网页浏览器开发的**JavaScript引擎**。其特色是，它在一个独立的CPU核心上即时编译脚本，与浏览器并行。该引擎也能够访问电脑的图形处理器(GPU)，特别是对 3D 图形和视频的情况。            
 - 部分**壳浏览器**的新版本是“双核”甚至是“多核”，其中一个内核是Trident，然后再增加一个其他内核。国内的厂商一般把其他内核叫做“高速浏览模式”，而Trident则是“兼容浏览模式”，用户可以来回切换。      

### Gecko（Firefox内核）      
Netscape6开始采用的内核，后来的Mozilla FireFox(火狐浏览器) 也采用了该内核，Gecko的特点是代码完全公开，其可开发程度很高。因为这是个开源内核，因此受到许多人的青睐，Gecko内核的浏览器也很多，这也是Gecko内核虽然年轻但市场占有率能够迅速提高的重要原因。      

### Webkit（Safari内核）      
它是苹果公司自己的开源内核，也是苹果的Safari浏览器使用的内核。       
Google Chrome（版本28以前）也使用Webkit作为内核(在脚本理解方面，Chrome使用自己研发的V8引擎)。      

### Blink（Chrome和Opera内核）      
 - Blink是一个由Google和Opera Software开发的浏览器内核，在Chrome（28及往后版本）、Opera（15及往后版本）浏览器中使用。      
 - 2013年4月3日，谷歌在Chromium Blog上发表博客，称将与苹果的开源浏览器核心Webkit分道扬镳，在Chromium项目中研发**Blink渲染引擎**（即浏览器内核），内置于Chrome浏览器之中。      
 - 苹果在Safari中采用Webkit核心，并于2005年将Webkit公开为开源软件，谷歌当时采用苹果的Webkit核心打造了Chrome浏览器，Opera也宣布称将会转向Webkit核心。但是谷歌宣布此举后，Opera表示将会跟随谷歌采用其Blink浏览器核心，同时参与了Blink的开发。      

### Chromium      
 - Chromium是一个**开源项目**，也是一个由Google主导开发的**网页浏览器**。      
 - Chrome是闭源的，很多浏览器说是基于Chromium的，但不会说是基于Chrome的。      
 - Chromium的开发可能早自2006年即开始，设计思想基于简单、高速、稳定、安全等理念，**在架构上使用了Apple发展出来的WebKit排版引擎、Safari的部份源代码与Firefox的成果**，**并采用Google独家开发出的V8引擎以提升解译JavaScript的效率**，而且设计了“沙盒”、“黑名单”、“无痕浏览”等功能来实现稳定与安全的网页浏览环境。      
 - Chromium是Google为发展自家的浏览器Google Chrome而开启的计划，**所以Chromium相当于Chrome的工程版或称实验版**。      
 - 新功能先在chromium出现，chromium有很多开发者共同的去改进，谷歌收集改进后会发布安装包，也就是chromium，然后会将chromium的东西更新到chrome中。      
 - 即，在chrome内的更新有一个过程：      
 - Chromium（未验证bug）：Chromium的更新速度很快，每隔数小时即有新的开发版本。      
 - Chrome dev（大问题已验证）：基于最新的Chromium Build，经常每周就更新推出新功能；与Beta版十分相似，但稳定性较差，不适合公共使用。      
 - Chrome beta（小问题已验证）：基于Chrome dev；按月更新；崩溃等重大故障较少发生，功能比dev更加完善。      
 - Chrome（稳定版）      

### 浏览器前缀      
 - Chrome， Safari：-webkit-      
 - Firefox： -moz-      
 - IE：-ms-      
 - Opera（早期）：-o-      