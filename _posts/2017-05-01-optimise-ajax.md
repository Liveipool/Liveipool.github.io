---
layout: post
title: 使用各种方式来优化循环ajax的写法
date: 2017-05-01
categories: blog
description: 前端学习
---

# 使用各种方式来优化循环ajax的写法        
在项目中遇到了一个问题，就是我可能有两个嵌套的ajax，首先要请求第一个，拿回来一个数组，然后再通过循环这个数组的每一项发起ajax请求，这就带来了一个问题，循环发起的ajax请求它们的到达顺序和期望的顺序不一定一致。虽然说可以更改后端代码，直接将这个数组作为请求字段，然后拿到对应数量的数据。不过也还是觉得在这次项目中ajax写得太乱了，所以想用一些方法来进行优化，下面是一些记录。        

我现在忽略了第一次ajax请求，假设我已经拿到了一个数组，现在需要对数组的每一项发起一次ajax请求。        

### 原写法        

``` javascript
// 试验ajax
const dataTest = [5, 18, 150, 165, 177];
for (let i = 0; i < dataTest.length; i++) {
  $.get(global_api.day_times + dataTest[i], function(data, textStatus) {
    console.log(i, ' : ', data);
  });
}
```
这样写的话每次得到的顺序都不一样，即i的输出总是43012之类的。        

### 递归写法        
根据原理，要想多次ajax返回的数据有序，就需要在一次ajax的回调函数里触发下一个，这种方法一种情况是写成回调地狱，另外一种情况就是使用递归的方法：        

``` javascript
const dataTest = [5, 18, 150, 165, 177];
ajaxTest(dataTest, 0);
function ajaxTest(dataTest, i) {
  $.get(global_api.day_times + dataTest[i], function(data, textStatus) {
    console.log(i, ' : ', data);
    if (i < dataTest.length - 1) {
      ajaxTest(dataTest, i + 1);
    }
  });                  
}
```
这样写，就能得到正确的请求顺序对应的返回数据了。        

### promise        
我用promise写了一通，不知道promise能不能写循环ajax，不能的话也只能用递归来写，还不如上面一种方法简单。        

### Generator        
要使用Generator和async都需要先用npm安装babel-pollify，并且在要使用的js文件顶部import "babel-polyfill";                

基本写法:        

``` javascript
// 试验ajax， Generator版
const dataTest = [5, 18, 150, 165, 177];
function* ajaxTest(dataTest) {
  for (let i = 0; i < dataTest.length; i++) {
    yield $.get(global_api.day_times + dataTest[i], function(data, textStatus) {
      console.log(i, ' : ', dataTest[i]);
    })
  }
}

let ajaxGen = ajaxTest(dataTest);
console.log('ajaxGen: ', ajaxGen);
while (ajaxGen.next().done == false) {
  console.log('ajaxGen.next: ', ajaxGen.next());
}
```
但是这种写法写出来五个ajax请求也是无序返回的。        

接下来主要学async。        

### async        
async函数是是 Generator 函数的语法糖。它通过内置执行器自动执行函数并输出最后结果，这样就不用再调用next方法。        

``` javascript
// 试验ajax， async版
const dataTest = [5, 18, 150, 165, 177];
function ajaxTest(dataTest) {
  console.log('dataTest111: ', dataTest);
  $.get(global_api.day_times + dataTest, function(data, textStatus) {
    console.log('dataTest: ', dataTest);
  })
}

let c = async function() {
  await ajaxTest(dataTest[0]);
  await ajaxTest(dataTest[1]);
  await ajaxTest(dataTest[2]);
  await ajaxTest(dataTest[3]);
  await ajaxTest(dataTest[4]);
  console.log('lala');
}

//或
let c = async function() {
  for (let i = 0; i < dataTest.length; i++) {
    await ajaxTest(dataTest[i]);
  }
  console.log('lala');
}      

c();
```
先输出‘lala’，再无序输出五个结果，而dataTest111后面的五个数字的顺序永远是有序的。        

然而，我再仔细看了一下async的教程，发现根本不是上面这么写的。        
应该是：        

``` javascript
// 试验ajax， async版
const dataTest = [5, 18, 150, 165, 177];
function ajaxTest(dataTest) {
  return new Promise((resolve, reject) => {
    $.get(global_api.day_times + dataTest, function(data, textStatus) {
      console.log('dataTest: ', dataTest);
      resolve(data);
    })
  })
}

let c = async function() {
  for (let i = 0; i < dataTest.length; i++) {
    let a = await ajaxTest(dataTest[i]);
    console.log('a: ', a);
  }
  console.log('lala');
}      
c();
```
输出为：        
datatest: 5        
a: object{movieOnShowId: 5....}                
datatest: 18        
a: object{movieOnShowId: 18....}        
datatest: 150        
a: object{movieOnShowId: 150....}        
datatest: 165        
a: object{movieOnShowId: 165....}        
datatest: 177        
a: object{movieOnShowId: 177....}        
lala        

即，在async函数中遇到await就会执行后面的语句并阻塞后面的语句的执行，这条语句一般是一个promise对象，整个语句给前面的变量赋值为resovle函数里的值。        

#### 综上所述        
所以可以看出，要想使循环多次ajax按照顺序返回，除了麻烦的递归写法，最好的就是使用async函数了，并且，async函数的作用远不止于此，并且它是一种趋势，因此要好好学async函数，以后多用它。        

#### 例子，使用async函数优化代码        
学以致用，用async函数来优化下面这部分代码 ：        

``` javascript
$.get(global_api.movie_info + movieId, function(data, textStatus) {
  const movie_info = data;
  let movie_info_length = document.getElementById('movie_info_length');
  movie_info_length.innerHTML = '时长：' + movie_info.length + '分钟';
})
```
上面代码的问题是，耦合太紧了，整段代码先是进行了一次ajax请求，再对DOM进行了操作，谈不上任何的代码复用，以后还要再发起这个ajax请求的时候就还需要写一次这个代码。当然可以说把这个ajax请求写到一个函数里，但因为操作DOM的那一部分内容必须等到ajax异步请求结束才能执行，所以说依然非常的僵硬，于是，我就用async函数来优化，就像写同步的代码一样：        

``` javascript
  // 获取电影信息
  function getMovieInfo() {
    return new Promise((resolve, reject) => {
      $.get(global_api.movie_info + movieId, function(data, textStatus) {
        resolve(data);
      });
    })
  }

  // 将电影各信息添加进DOM里
  function setMovieInfo(movie_info) {
    let movie_info_length = document.getElementById('movie_info_length');
    movie_info_length.innerHTML = '时长：' + movie_info.length + '分钟';
  }

  async function movieInfoPart() {
    let movie_info = await getMovieInfo();
    setMovieInfo(movie_info);
  }

  movieInfoPart();
```
这样写清晰明了，结构好，代码可复用性高，当代码更复杂时，可以摆脱回调地狱。        
不用不知道，一用起来真是感觉好爽，去优化其他的代码了。。。        