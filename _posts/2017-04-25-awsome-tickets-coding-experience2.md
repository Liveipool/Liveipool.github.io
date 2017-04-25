---
layout: post
title: 系统分析与设计大作业--电影订票系统编程总结（二）
date: 2017-04-25
categories: blog
description: 前端学习
---

# 系统分析与设计大作业--电影订票系统编程总结（二）           
最近一个星期也抽了几个整天来做这个系统的第二轮迭代，有三个页面，逻辑还是有些复杂，代码量也比较大，打完这次迭代也遇到了一些问题，是时候总结一些遇到的知识点了。           
首先，先放一下现在已经做好的四个页面的截图：           
![首页.png](http://upload-images.jianshu.io/upload_images/3001083-7b15375e5da030dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           
![选择影院.png](http://upload-images.jianshu.io/upload_images/3001083-83f488f447cf92c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           
![选择场次.png](http://upload-images.jianshu.io/upload_images/3001083-4cbedded9e921a55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           
![选择座位.png](http://upload-images.jianshu.io/upload_images/3001083-c76d2020bb0e0315.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           

#### gulp-watch           
在第一轮迭代时，在linux上没啥问题，可在windows上，出现了gulp无限运行页面一直刷新的情况，开始我还以为是因为glob语法的‘**’在windows上有问题，而这次偶然发现了实际上是我watch这个gulp.task写得有问题，以index.jade为例：           

``` javascript
gulp.task('index', function() {
	gulp.src('index.jade')
		.pipe(jade())
		.pipe(gulp.dest('tmp'))
		.pipe(connect.reload());
});

// 无限刷新
gulp.task('watch', function() {
	gulp.watch('*.jade', ['index']);
});

// 正确
gulp.task('watch', function() {
	gulp.watch('index.jade', ['index']);
});
```

开始会出现无限刷新的原因是gulp.watch中的src和后面对应的task中的src要写得完全一样才行，不然就会出现莫名其妙的无限刷新bug。

#### 设置padding防止出现滚动条时页面跳动           
![出现滚动条.png](http://upload-images.jianshu.io/upload_images/3001083-f2a62f5e1189b411.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           
 在一些时候，当页面的内容较多时，会出现滚动条的情况，如果是内容宽度固定两边居中的布局就会产生左右跳动的情况，这样看起来会很不舒服。网上说到了两种方法，一种是当滚动条经常都会出现时，通过设置overflow或者overflow-y来让滚动条一直出现在页面上，但这样感觉很僵硬，滚动条看起来也很丑。           
 另外一种方式是说，对整体内容的父元素设置一个padding或对整体内容设置一个margin。在我的系统中，因为整体内容的左右margin都设为auto了，所以只能对父元素设置一个padding，这个padding就应该是滚动条的宽度，那么这个宽度是多少呢？           

两条语句搞定：           

``` css
  width: calc(100vw - 100% + 1024px);
  padding-left: calc(100vw - 100%); 
```

这里是用到了css的计算属性calc，IE10及以上的浏览器都是支持的，但vw单位，IE10和11只是部分支持，所以这种方案在IE上不一定能完全支持。           
所以还是多设置一个width: 1024px;作为回退机制比较好。           
那么这个calc(100vw - 100%)是多宽呢？100vw就是整个浏览器的宽度，而100%是浏览器内容的宽度，不包括可能出现的滚动条，所以如果无滚动条时，padding-left就是0,有滚动条时，padding-left就是滚动条的宽度，而因为width也跟着增加了一个padding-left的宽度，所以在这种情况下，出现了滚动条内容也不会左右移动，解决了跳动的问题。           

#### overflow、overflow-x、overflow-y           
overflow有很多个属性，默认的是visible，但它的属性值scroll和auto稍微有点像，在使用的过程中感觉还没彻底理解清楚，有时候会有一些问题，这个再多用用以后再详细总结一下。           

#### ajax请求无序到达           
在这次的项目中，用到了很多次ajax请求，比如选择日期和场次页面，先点击某个日期，拿到一个数组，对应的这个日期内的各个排期，再通过这个数组中的每一项分别去请求一条排期。           
那这样就出现了一个问题，比如我通过一个循环对数组中的五个排期分别请求数据，在它们请求成功的回调函数里将它们插进DOM里，但是由于它们请求成功的顺序每一次都可能不一样，即这五个排期是无序到达并无序插入到DOM中的。现在比如我想通过场次时间将它们排序，就只有在它们都插入到DOM中之后再将它们排序，我现在也就是这样做的，但我觉得这样还不够优雅，应该是在等到所以数据都拿到之后将它们排序再插入到DOM中。这样就需要ajax请求后的状态能够控制得更精细。在ES6之前，要想让异步函数顺序执行，只能写成callback hell，ES6和ES7中规范了promise和async函数等，大致看了一下，觉得async函数可能可以解决这个问题，后面有空了看看，看能不能用async函数来改进现在的写法。           

#### IE中任何版本都不能使用Array.from           
Array.from是ES6中对数组的扩展，但IE现在任何版本都不支持。           
虽然我在项目中没有直接用到Array.from，但因为我添加进了babel进行转码，所以当我在用扩展运算符...时，代码被转成了使用Array.from，这样的话在IE中跑不了，所以要想适配IE，就没办法用扩展运算符...了。            
这确实有点尴尬，为啥用了babel转码得到的代码还有部分是ES6中的属性，不太懂，这个后面还可能会遇到，再注意。           

#### :active伪元素                      
这个元素以前一直没太注意过到底是什么效果，这次了解到了，就比如一个按钮，我给他设置以下属性：           

``` css
#somebutton:active {
	background-color: red;
}
```
在对这个按钮进行点击时，点下去的瞬间，背景会变为红色，点击完之后，背景的红色又会消失。这就相当于是鼠标的类似keydown和keyup事件的效果。           

#### 加上一个小三角形           
![选择座位，更改场次.png](http://upload-images.jianshu.io/upload_images/3001083-25de2bf55ca6c9b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)           
更改场次的弹出框，给它设一个大的z-index值，展开它时就会遮住下面的内容。           
弹出框上面的那个小三角形，以前在《CSS揭秘》上看到过教学，但那是通过使用弹出框的伪元素:before来做的，但我这次在用:before来做时，不知道为何，这个小三角形始终都无法出现在框外，即出现在框外的内容会自动被切掉，然而我已经设置了很高的在z-index值，出现这样的情况确实有点奇怪。           
于是我换了种思路，我用另外一个div元素来做成一个旋转了45度的小正方形，挡住一部分就成了三角形，定位的话，也比较灵活。           

``` css
#movie_info_change_show_time_dialog_triangle {
  border: 1px solid #ECE7E7;
  background: #FAFAFA;
  position: absolute;
  top: 300px;
  left: 85px;
  padding: 0;
  width: 17px;
  height: 17px;
  z-index: 1;
  transform: rotate(45deg);
  border-bottom: 0;
  border-right: 0;
}
```

#### spinner           
现在的话，由于页面上的很多数据都是动态加载的，所以如果网络不好可能交互就会很差，准备在可能会等待数据的地方加一些spinner进行提示，这个和后面改进代码的时候一起做吧。           
