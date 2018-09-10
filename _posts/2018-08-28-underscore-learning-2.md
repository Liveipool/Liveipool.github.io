---
layout: post
title:  Underscore.js源码学习（二）
date: 2018-08-28
categories: blog
description: 前端学习
---

# Underscore.js源码学习（二）
在第一篇文章中讲的是 underscore 的功能以及一些核心特点之类的，并且也谈到了对于 underscore 使用的一些想法。     
这一篇文章则是对于上一篇的补充，更多的是根据源码和别人的解读，对 underscore 的各函数进行分析，为什么要这样写，这样写的好处在哪里。    

#### 使用迭代，而不是循环   
在函数式编程里，更推荐使用迭代：   

``` javascript
// 这样写是确实能减少代码量的，并且逻辑能清晰，就算没有注释，也更容易一眼看出在干什么
var results = _.map([1,2,3],function(elem){
  return elem*2;
}); // => [2,4,6]

// 而不是循环：
var results = [];
var elems = [1,2,3];
for(var i=0,length=elems.length;i<length;i++) {
  results.push(elems[i]*2);
} // => [2,4,6]

// 其实我在实际开发中循环用的还是多一些，但是毕竟还是有后来ES5规范了的forEach、map等函数，所以能使用节省时间的时候是要学会灵活使用的
```

#### iteratee
对于一个迭代来说，他至少由如下两个部分构成：   
1. 被迭代集合     
2. 当前迭代过程    

**在 underscore 中，当前迭代过程是一个函数，它被称为 iteratee（被迭代者），他将对当前的迭代元素进行处理。这个iteratee非常重要，在underscore中会一直出现。**   

我们可以根据\_.map 的实现来理解iteratee和cb函数等：   
```  javascript
// map函数的作用是遍历数组或者对象的每个元素，将结果保存到新的数组中并返回
_.map = _.collect = function (obj, iteratee, context) {
	// 这个iteratee可能是函数，对象，甚至是字符串，会通过cb函数将其统一处理为一个函数。
    iteratee = cb(iteratee, context);
    // 这里看似一句很短的赋值语句，实际上深入去看会觉得真是巧妙，我觉得换我来写至少得写一个if判断
    // 这里的逻辑是这样，首先判断obj是不是数组，如果是数组，因为这里是短路表达式，keys直接等于false，那么下面的length的值就成了这个数组的length
	// 而如果obj是对象，则keys是等于_.keys(obj)的返回值，_.keys的作用是返回obj对象的keys组成的数组，这时下面的length则是obj对象的keys的length
	// 再次感叹，就两个赋值语句，我觉得我一个if判断都不一定能写完
    var keys = !isArrayLike(obj) && _.keys(obj),
	    // 如果 obj 为对象，则 length 为 obj对象的keys的length
        // 如果 obj 为数组，则 length 为 这个数组的length
        length = (keys || obj).length,
        results = Array(length); // 定长初始化数组
    for (var index = 0; index < length; index++) {
	    // 如果 obj 为对象，则 currentKey 为对象键值 key
	    // 如果 obj 为数组，则 currentKey 为 index 值
        var currentKey = keys ? keys[index] : index;
        results[index] = iteratee(obj[currentKey], currentKey, obj);
    }
    return results;
};
```

#### cb函数
**我们传递给的\_.map 的第二个参数就是一个 iteratee，他可能是函数，对象，甚至是字符串，underscore会将其统一处理为一个函数。这个处理统一由函数cb来完成：**
``` javascript
// cb 将根据不同情况来为我们的迭代创建一个迭代过程 iteratee，服务于每轮迭代：
var cb = function (value, context, argCount) {
    // 是否用自定义的iteratee
    if (_.iteratee !== builtinIteratee) return _.iteratee(value, context);
    // 下面是根据value的不同情况来进行不同的处理，传进来的value可能为空、函数、对象、字面量

    // 没有传入iteratee或传入了一个null，则iteratee的行为只是返回当前迭代元素自身比如：var results = _.map([1,2,3]); => results：[1,2,3]
    // 在underscore中，很多特殊值的判断都是写的value == null，这是因为 == 会自动做类型转换，null == undefined是true的。现在都是写===了，所以需要写成 value === null || value === undefined;
    if (value == null) return _.identity;
    // 如果传入 value 是一个函数，那么通过内置函数 optimizeCb 对其进行优化
    if (_.isFunction(value)) return optimizeCb(value, context, argCount);
    // 如果 value 传入的是一个对象，那么返回的 iteratee（_.matcher）的目的是想要知道当前被迭代元素是否匹配给定的这个对象
    if (_.isObject(value)) return _.matcher(value);
    // 如果以上情况都不是， 那么传入的 value 会是一个字面量（直接量），他指示了一个对象的属性 key，返回的 iteratee（_.property）将用来获得该属性对应的值
    return _.property(value);
};
```

``` javascript
  // 上面的_.identity就是自身的意思，其实直接返回自身也行，因为这个函数也就是这个作用，不过因为整体都是函数式编程的逻辑，所以这里的identity最准确的说法是返回自身的函数
  _.identity = function(value) {
    return value;
  };
```

### optimizeCb函数
上面提到的optimizeCb函数，就是优化回调的意思，所以它是一个对最终返回的iteratee进行优化的过程：
``` javascript
// func 待优化回调函数
// context 执行上下文
// argCount 参数个数
var optimizeCb = function(func, context, argCount) {
  // 一定要保证回调的执行上下文存在
  if (context === void 0) return func;
  // 通过argCount来判断，根据参数个数分情况进行优化
  switch (argCount == null ? 3 : argCount) {
    case 1: return function(value) {
      return func.call(context, value);
    };
    case 2: return function(value, other) {
      return func.call(context, value, other);
    };
    case 3: return function(value, index, collection) {
      return func.call(context, value, index, collection);
    };
    case 4: return function(accumulator, value, index, collection) {
      return func.call(context, accumulator, value, index, collection);
    };
  }

  // 其实不用上面的switch语句，直接执行下面的return语句，最后返回的结果是一样的
  // 不这样做的原因是 call 比 apply 快很多，这也就是能进行优化的根本原因
  // .apply 在运行前要对作为参数的数组进行一系列检验和深拷贝，.call 则没有这些步骤
  return function() {
    return func.apply(context, arguments);
  };
};
```
#### Function的length属性
原来函数.length会返回它的参数个数，以前都忽略了这个。

``` javascript
function a() {}
// a.length === 0 true
function a(b, c) {}
// a.length === 2 true
```

#### Array.prototype.slice.call
在underscore中，看到了一些如Array.prototype.slice.call这样的用法，slice方法和call方法都很清楚，但它们合起来使用的方式以前没怎么看见过：
 - array.slice(start, [end]) ，截取数组的一部分。
 - func.call(thisArg, arg1, arg2, ...)，其具有一个指定的this值和可选的参数

那么Array.prototype.slice.call(arguments,1)，这句话的意思就是说把调用方法的参数截取出来。 如：

``` javascript
function test(a,b,c,d) { 
  var arg = Array.prototype.slice.call(arguments,1); 
} 
  test("a","b","c","d"); //b,c,d
```

#### 对象和原型
在 [创建对象的正确姿势](https://yoyoyohamapi.gitbooks.io/undersercore-analysis/content/base/%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%AD%A3%E7%A1%AE%E5%A7%BF%E5%8A%BF.html) 这一篇文章中，作者讲到了关于JavaScript中的对象和原型的内容，也讲了在underscore中是如何创建对象的。

其实，以前的《JavaScript高级程序设计》和《你不知道的JS》里有讲到，我自己也总结过不少：
 - [原型–更深入的学习和总结](http://liveipool.com/blog/2017/02/21/Prototype/)
 - [面向对象和面向类](http://liveipool.com/blog/2017/02/21/Object-Orient-and-Class-Orient/)
 - [面向对象的程序设计](http://liveipool.com/blog/2016/12/27/JavaScript-RedBook-6-Object-Oriented/)

这一块的内容是需要进行理解的，当时也是花了很多时间去看，后来感觉也还是较为完整的理解了。
**但是，在后来实际的开发过程中，其实很少去关心对象、原型之类的问题，往往直接以字面量的形式声明并使用就行了，继承机制则是更少用到。在underscore中，作者定义了\_的prototype对象，并给它定义了一些方法，所以我觉得我很少使用这一块内容的原因也是我没有真正写过这样的库。但我也想知道，除了写这种库会有继承、原型之类的操作，还有写什么JS代码的时候会用到这些东西？在普通的应用和业务开发时，有没有这一块相关内容能提升代码和应用质量的？希望有经验的大佬们可以在评论中大致讲解一下。**

#### invoke函数
作用时对数组或者对象中的每个元素都调用传入的method参数的这个方法

``` javascript
  // 用法
  _.invoke([[5, 1, 7], [3, 2, 1]], 'sort');
  // => [[1, 5, 7], [1, 2, 3]]
```

``` javascript
  // 实现
  // 返回调用后的结果
  // method参数之后的参数会被当做参数传入method方法中
  _.invoke = function(obj, method) {
    // 获取method参数之后的参数，存下来后面传入method方法中
    var args = slice.call(arguments, 2);

    // 判断 method 是不是函数
    var isFunc = _.isFunction(method);

    // 用 map 方法对数组或者对象每个元素调用method方法
    return _.map(obj, function(value) {
      // 如果 method 不是函数，则可能是 obj 的 key 值，而 obj[method] 可能为函数
      var func = isFunc ? method : value[method];
      return func == null ? func : func.apply(value, args);
    });
  };
```

#### max函数
作用是寻找数组中的最大元素或者对象中的最大 value 值。

``` javascript
  // 如果有 iteratee 参数，则返回的是每个元素经过该函数迭代后的值的最大值
  _.max = function(obj, iteratee, context) {
    var result = -Infinity, lastComputed = -Infinity,
        value, computed;

    // 单纯地寻找最值
    if (iteratee == null && obj != null) {
      // _.values的作用是将一个对象的所有 values 值放入数组中，并返回该数组
      // 所以obj如果是值要么就是数组本身要么就是对象的values组成的数组
      obj = isArrayLike(obj) ? obj : _.values(obj);
 
      // 如果是数组，则寻找数组中最大元素
      // 如果是对象，则寻找最大 value 值
      for (var i = 0; i < obj.length; i++) {
        value = obj[i];
        if (value > result) {
          result = value;
        }
      }
    } else {
      // iteratee有值，需要对每个元素进行一次iteratee函数的洗礼
      iteratee = cb(iteratee, context);

      // result 保存最后返回的结果
      // lastComputed 保存计算过程中出现的最值
      _.each(obj, function(value, index, list) {
        // 经过迭代函数后的值
        computed = iteratee(value, index, list);
        // 这个判断的意思是指，如果computed是-Infinity，则无法比较它和lastComputed的大小，再进行赋值没意思，如果computed不为-Infinity则进行赋值操作
        if (computed > lastComputed || computed === -Infinity && result === -Infinity) {
          result = value;
          lastComputed = computed;
        }
      });
    }
    return result;
  };
```

#### intersection函数
作用是找数组的交集

``` javascript
  _.intersection([1, 2, 3, 1], [101, 2, 1, 10, 1], [2, 1, 1])
  // => [1, 2]
```

``` javascript
  // 寻找几个数组中共有的元素
  _.intersection = function(array) {
    // 结果数组
    var result = [];

    // 传入的参数（数组）个数
    var argsLength = arguments.length;

     // 遍历第一个数组的元素
    for (var i = 0, length = getLength(array); i < length; i++) {
      var item = array[i];

      // 如果 result[] 中已经有 item 元素了，continue，跳过这一次迭代，但是不跳出循环
      // 即 array 中出现了相同的元素
      // 返回的 result[] 其实是个 "集合"（是去重的）
      if (_.contains(result, item)) continue;

      // 判断其他参数数组中是否都有 item 这个元素
      for (var j = 1; j < argsLength; j++) {
        if (!_.contains(arguments[j], item))
          break;
      }

      // 遍历其他参数数组完毕
      // j === argsLength 说明其他参数数组中都有 item 元素
      // 则将其放入 result[] 中
      if (j === argsLength) result.push(item);
    }

    return result;
  };
```

#### 一些想法
**函数式编程**，是underscore中从头到尾贯彻的原则，无论是从代码的复用还是可读性上来说都是能看到好处的。就比如说identity这个例子：  

``` javascript
  _.identity = function(value) {
    return value;
  };
```

其实说起来，每次直接返回自身反而代码还少一些，但是这样函数式的写法既可以统一写法，还可以在思维上变得更加清晰。设想一下，每一次的变量名可能是不同的，如果每一次返回自身时都要看自己变量名叫什么，和不用想直接调用identity函数，当然是后者更难出错。  

另外，还能看到这样的一些函数，都是体现函数式编程好处的一些功能，只需要进行简单的一些封装就能实现复用。  

``` javascript
_.union = function() {
  return _.uniq(flatten(arguments, true, true));
};

_.zip = function() {
  return _.unzip(arguments);
};
```

最后还有如\_.isFunction，\_.isObject这样的函数，在整个代码中多次被使用，节省了很多时间。  
不过确实，现在很多情况下不会使用纯函数式编程，但是里面的一些好的思想是完全值得学习的，要是以后在开发的时候，能想到今天看的一些内容，也算是学有所获了吧。

#### 学习资料
- [英文文档](https://underscorejs.org/)
- [中文文档](http://www.bootcss.com/p/underscore/)
- [源码地址](https://github.com/jashkenas/underscore)
- [undersercore 源码分析](https://legacy.gitbook.com/book/yoyoyohamapi/undersercore-analysis/details)：讲得比较详细，上文中的有一些段落就是出自这篇文章里面，确实很清晰，先看了源码再来看这个，本来没理解的地方就都理解了。
- [带有注释的源码（如果觉得英文实在看不下去可以google翻译）](http://www.bootcss.com/p/underscore/docs/underscore.html)
- [1.8.3版本的有一些中文注释的源码](https://github.com/hanzichi/underscore-analysis/blob/master/underscore-1.8.3.js/underscore-1.8.3-analysis.js)

#### 参考文章
[Array.prototype.slice.call()方法详解](https://blog.csdn.net/i10630226/article/details/49702375)   

![赞赏码.jpeg](https://upload-images.jianshu.io/upload_images/3001083-f65814d1f594b39c.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
