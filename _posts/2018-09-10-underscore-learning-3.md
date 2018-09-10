---
layout: post
title:  Underscore.js源码学习（二）
date: 2018-08-28
categories: blog
description: 前端学习
---

# Underscore.js源码学习（三）-- 学以致用
之前在 [对 JavaScript 函数式编程的一些想法](http://liveipool.com/blog/2018/09/02/thoughts-about-functional-programming/) 中提到了关于想写一个utils-webank函数库的想法，其实也就是对最近看的Underscore.js源码和函数式编程相关知识进行学以致用。
在具体的自己写函数库的过程中，发现了一些之前在单纯阅读Underscore.js源码时并没有注意到的，却很能体现作者功力的点，或者说是，真正当我自己开始写这样一个库时，才会从简单的看Underscore.js作者是怎么写的，变成思考要怎么写，为什么要这么写。

下面的内容，就是在实现utils-webank函数库中一些印象深刻的点：

#### 通过Object.prototype.toString批量添加类型判断函数
以前在进行类型判断的时候，一直都是typeof或者instanceof，不仅麻烦，而且往往需要多个判断进行组合才能唯一标识一种类型，而实际上：**在JavaScript中,想要判断某个对象值属于哪种内置类型,最靠谱的做法就是通过Object.prototype.toString方法.**具体可参见：[JavaScript：Object.prototype.toString方法的原理【转载】](http://liveipool.com/blog/2018/09/10/object-prototype-tostring/)

因此，参考Underscore.js，可以统一增加这些方法：isArguments, isBoolean, isFunction, isString, isNumber, isObject, isDate, isRegExp, isMath, isError, isMap, isWeakMap, isSet, isWeakSet：
``` javascript
    ['Arguments', 'Boolean', 'String', 'Number', 'Object', 'Function', 'Date', 'RegExp', 'Math','Error', 'Symbol', 'Map', 'WeakMap', 'Set', 'WeakSet'].forEach( (name) => {
        uwb['is' + name] = (obj) => {
            return Object.prototype.toString.call(obj) === '[object ' + name + ']';
        };
    });
```

#### isJSON

``` javascript
/*
    判断变量是否符合JSON格式，（网上的很多简单的写法都不够严密，很多情况涵盖不到，下面是一种比较完整的判断）
    本来在标准中说的JSON格式的也满足上面的'[object JSON]'，但实际的会得到'[object String]'
    输入：需要进行判断的变量
    输出：true or false
*/
uwb.isJSON = (str) => {
	// 在以前写代码的过程中，就算if语句里只有一排，我也会写大括号，觉得可读性更好
	// 而在Underscore.js中，只有一条语句的if语句，作者都没有写大括号，我试着写了一下，感觉也还是可以，反而比较清爽。最重要的是，在写库时，要尽量减少代码，所以这种大括号能省就省了。
    if (!uwb.isString(str)) return false;
    str = str.replace(/\s/g, '').replace(/\n|\r/, '');
    if (/^\{(.*?)\}$/.test(str)) return /"(.*?)":(.*?)/g.test(str);
    if (/^\[(.*?)\]$/.test(str)) {
        return str.replace(/^\[/, '')
        .replace(/\]$/, '')
        .replace(/},{/g, '}\n{')
        .split(/\n/)
        .map(function (s) { return uwb.isJSON(s); })
        .reduce(function (prev, curr) { return !!curr; });
    }
    return false;
}
```

#### isArray
其实isArray函数也可以在上面进行批量添加，因为它也符合'[object Array]'相关规则，但是Underscore.js的作者为了让性能更优，在有原生方法Array.isArray的时候，首先考虑原生方法。

``` javascript
// 首先用局部变量把原生方法存下来
const nativeIsArray = Array.isArray;

// 如果运行环境有原生方法uwb.isArray就是原生方法，如果没有就是后面的函数，通过Object.prototype.toString来验证
uwb.isArray = nativeIsArray || function (obj) {
    return Object.prototype.toString.call(obj) === '[object Array]';
};
```

#### isNull、isUndefined
判断变量是否是null或者undefined，Object.prototype.toString.call(null)也等于'[object Null]'，但是由于首字母n的大小写问题，所以和undefined单独定义

``` javascript
uwb.isNull = (obj) => obj === null;
// void 0比undifined更纯净
uwb.isUndefined = (obj) => obj === void 0;
// obj是null或者undefined，在某些时候使用可以增强可读性
uwb.isNullOrUndefined = (obj) => uwb.isNull(obj) || uwb.isUndefined(obj);
// obj不是null且不是undefined，这个很常用，所以名字短一点好，!isSafe就相当于isNullOrUndefined
uwb.isSafe = (obj) => !(uwb.isNull(obj) || uwb.isUndefined(obj));
```
其实最开始的时候，我只写了isNull、isUndefined、isNullOrUndefined这三个函数，而这三个函数也能满足要求了，但是，在我后来具体的使用过程中，在一些if语句里或者一些判断时，需要的达成的条件是：**如果这个变量不为null或undefined，则执行后面的步骤**。那么在这种情况下（也是最常见的情况），我就得写成!uwb.isNullOrUndefined，感觉太长了，所以直接增加了uwb.isSafe函数，更简洁，更易用。从这里可以看出，好的api的设计，一定是要让使用者觉得便捷的。
那么为什么还要留下isNullOrUndefined这个函数呢？!isSafe也是同样的效果，并且更简洁，这是因为，我们经常需要**在代码的简洁和可读性上进行取舍**，如果过度缩减，有时候会让代码难以阅读。比如在有些时候，我的逻辑是，如果obj是null或者undefined，则执行后续的步骤，那么这里显然用isNullOrUndefined(obj)逻辑更加清晰。

#### 更安全地获取对象的属性值
这样写的好处在于，当obj不是null或者undefined时，就返回obj的key属性。否则就返回void 0，因为如果obj是null或者undefined，obj[key]会报错。这样，最差是得到结果为undefined，但始终不会报错，更加安全，使用者用起来也就不需要每一次都进行判断了。

```
uwb.getObjectValue = (obj, key) => {
    return uwb.isSafe(obj) ? obj[key] : uwb.getUndefined();
};
```

#### 定义一些常用的正则表达式
这个是Underscore.js中没有的，但之前也讲到了，utils-webank和一些传统的函数库的不同在于，它更偏向于实际业务，所以在实际业务中我们往往会有一些常用的正则表达式，虽然常用，但也没谁记得住，每次都得上网查。
所以在utils-webank中，会事先写好一些正则表达式，在需要用时调用get方法就行。
``` javascript
// 写好常用正则表达式
const RegExps = {
    ipReg: new RegExp(/^(((2[0-4][0-9]|25[0-5]|1\d{2}|[1-9]\d|\d)(\.)){1,3}(2[0-4][0-9]|25[0-5]|1\d{2}|[1-9]\d|\d){0,1})$|^(2[0-4][0-9]|25[0-5]|1\d{2}|[1-9]\d|\d){1}$/),
};

// 通过getRegExp方法进行获取
uwb.getRegExp = (key) => RegExps[key];
```

#### 判断对象是否有某个属性（在其本身而不是原型链上）
**hasOwnProperty 方法是 Javascript 中唯一一个处理对象属性而不会往上遍历原型链的。**

``` javascript
// Poisoning Object.prototype
Object.prototype.bar = 1;
const foo = {goo: 2};

foo.bar; // 1
'bar' in foo; // true
foo.hasOwnProperty('bar'); // false
foo.hasOwnProperty('goo'); // true
```

**Javascript 并未将 hasOwnProperty 设为敏感词，这意味着你可以拥有一个命名为 hasOwnProperty 的属性。这个时候你无法再使用本身的 hasOwnProperty 方法来判断属性，所以你需要使用外部的 hasOwnProperty 方法来进行判断。**

``` javascript
var foo = {
    hasOwnProperty: function() {
        return false;
    },
    bar: 'Here be dragons'
};

foo.hasOwnProperty('bar'); // 总会返回false

// 用一个其他的空对象的hasOwnProperty方法进行调用，确保hasOwnProperty方法是纯净的
({}).hasOwnProperty.call(foo, 'bar'); // true

// 也可用原型链顶端的hasOwnProperty方法来调用
Object.prototype.hasOwnProperty.call(foo, 'bar'); // true
```

因此，utils-webank中的hasProperty函数实现：
``` javascript
/*
    判断对象是否有某个属性，并且这个属性是它本身的属性，而不是它的原型链上的
    在JS中可以更改hasOwnProperty的值，所以用一个空对象的hasOwnProperty更稳
    如果Object的键名为number，会被js自动转换为string，Object中的所有键值类型都是字符串
    输入：对象Object obj，属性String property
    输出：true or false
*/
uwb.hasProperty = (obj, property) => {
    return !uwb.isObject(obj) && ({}).hasOwnProperty.call(obj, property);
}
```

#### 批量设置对象的属性
``` javascript
/*
    批量设置对象的属性
    因为经常会有这种情况，在请求接口后，需要用局部变量将一些数据存下来，这时候就要写成这样：
    const tmpObj = { id: response.id, hostname: response.hostname, ip: response.ip, idc: response.idc }
    或者是 const tmpObj = { a: 1, b: 2, c: 3 }
    很麻烦，所以提供一个批量设置对象的属性的方法
    可以简写成:
    uwb.setObjectValues(tmpObj, ['id', 'hostname', 'ip', 'idc'], response);
    uwb.setObjectValues(tmpObj, ['a', 'b', 'c'], [1, 2, 3]);
    输入：对象Object obj，属性Array properties，值value，Function func
    如果value是Object，每一次迭代的值就是value[propertys[index]]，如果是Array，每一次迭代的值就是value[index]
    Function func是如果要使用this.$set方法设置属性时，则在这里将this.$set传进来
    输出：无
*/
uwb.setObjectValues = (obj, properties, value, func) => {
    // obj必须为Object，properties必须为Array
    if (!uwb.isObject(obj) || !uwb.isArray(properties)) return null;
    if (uwb.isObject(value)) {
        properties.forEach( (property) => {
            if (uwb.isFunction(func)) {
                // 使用this.$set方法进行赋值
                func(obj, property, value[property])
            } else {
                obj[property] = value[property];
            }
        });
    } else if (uwb.isArray(value) && value.length >= properties.length) {
        properties.forEach( (property, index) => {
            if (uwb.isFunction(func)) {
                func(obj, property, value[index])
            } else {
                obj[property] = value[index];
            }
        });
    }
};
```

#### 更加完善的Object.keys功能
``` javascript
/*
    更加完善的Object.keys功能
    输入：对象Object obj
    输出：keys数组
*/
uwb.keys = function(obj) {
    // 如果不是对象，直接返回空数组
    if (!uwb.isObject(obj)) return [];
    // 如果支持ES5，也就是有nativeKeys方法，则调用
    if (nativeKeys) return nativeKeys(obj);
    // 否则通过for in语句来实现
    const keys = [];
    for (const key in obj) if (uwb.hasProperty(obj, key)) keys.push(key);
    return keys;
};
```

#### 统一的判断 isEmpty
经常会遇到空字符串、空数组、空对象的方法，每次都重写判断非常麻烦，因此定义了一个可以对各类型的变量进行判断是否为空的函数。

``` javascript
/*
    这些情况将会返回true：1.null 2.undefined 3.数字0 4.NaN 5.空字符串 6.空数组 7.空对象
    输入：需要进行判断的变量
    输出：true or false
*/
uwb.isEmpty = (obj) => {
    if (!uwb.isSafe(obj) || uwb.isNaN(obj) || obj === 0) return true;
    // 如果是数组或者字符串，则根据length的值来判断
    if (uwb.isArray(obj) || uwb.isString(obj)) return obj.length === 0;
    // 如果是Object了，通过它的keys数组的length来判断
    if (uwb.isObject(obj)) return uwb.keys(obj).length === 0;
    return false;
};
```

#### 到底哪些要提取成函数？
无论是在看源码和书，还是在实际写函数库的过程中，我一直都在问自己这个问题，到底哪些功能可以提取成函数，哪些语句用函数式的写法更好？
这也是我觉得函数式编程中一个很核心的问题，在我的理解下：**要进行取舍，不要盲目地将所有语句都写成函数，如果真正有帮助，才提取成函数或使用。**

举两个例子：
```
// 例子一：在字符串text不为空的情况下，将其输出
// 写法一
if (text) console.log(text);
// 写法二
if (uwb.isEmpty(text)) console.log(text);

// 例子二：给对象obj设置属性
const obj = {};
// 写法一
obj.name = 'Liveipool';
// 写法二
uwb.setObjectValue(obj, name, 'Liveipool');
```
显然，第一种写法更简单，虽然我之前定义好了isEmpty和setObjectValue，但是在这里，直接if (text)或者简单的赋值语句已经可以满足要求了，不需要再写成第二种来画蛇添足了。

而在下面这种情况下：
``` javascript
// 如果变量arr是数组类型，且它的长度不为0，则输出

// 写法一
if (arr instanceof Array === true && arr.length !== 0) console.log(arr);

// 写法二
if (uwb.isArray(arr) && !uwb.isEmpty(arr)) console.log(arr);
```
显然第二种函数式的写法，逻辑更清晰，往往代码也更简洁。
所以，具体什么时候使用函数式的写法，一定要看它是否真的能让自己的代码更优，因为函数式编程的目的是让我们的代码更加优雅，如果单纯的为了函数式而函数式，毫无意义。

以上，就是近期学以致用过程中的一些思考和总结。

#### 参考文章
 - [JavaScript:Object.prototype.toString方法的原理](http://www.cnblogs.com/ziyunfei/archive/2012/11/05/2754156.html)
 - [细说 Javascript 对象篇（三） : hasOwnProperty](https://segmentfault.com/a/1190000000480531)

![赞赏码.jpeg](https://upload-images.jianshu.io/upload_images/3001083-f65814d1f594b39c.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     