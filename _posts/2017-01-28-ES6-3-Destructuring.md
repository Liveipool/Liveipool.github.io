---
layout: post
title: ES6 标准入门(3)--变量的解构赋值 
date: 2017-01-28
categories: blog
description: JavaScript学习
---

# 变量的解构赋值     

#### 基本用法     
ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructing）。     

```
//以前，为变量赋值，只能直接指定值。
let a = 1;
let b = 2;
let c = 3;

//ES6允许写成下面这样。
let [a, b, c] = [1, 2, 3];
```

本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。下面是一些使用嵌套数组进行解构的例子。     

```
let [foo, [[bar], baz]] = [1, [[2], 3]];

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []

let [bar, foo] = [1];  //属于不完全解构，但也可以成功
```
如果解构不成功，变量的值就等于undefined。     
**如果等号的右边不是可遍历的结构，将会报错。**如：     

```
let[foo] = 1;   //报错
```

#### 默认值     
解构赋值允许指定默认值。     

```
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```
注意，ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。     

```
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```
上面代码中，如果一个数组成员是null，默认值就不会生效，因为null不严格等于undefined。     

默认值可以引用解构赋值的其他变量，但该变量必须已经声明。     

```
let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // ReferenceError
```

#### 对象的解构赋值     
解构不仅可以用于数组，还可以用于对象。     

```
let { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
```
对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。     

```
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
```
如果变量名与属性名不一致，必须写成下面这样。     

```
var { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"
```
也就是说，对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。     

```
let { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined
```
**上面代码中，foo是匹配的模式，baz才是变量。真正被赋值的是变量baz，而不是模式foo。**     
**注意，采取这种写法时，变量的声明和赋值是一体的。因此要注意不要重复声明。**     

再看一个例子：     
     
```
let obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

let { p: [x, { y }] } = obj;
x // "Hello"
y // "World"
p // Uncaught ReferenceError: p is not defined
```
**这时p是模式，不是变量，因此不会被赋值。**     

对象的解构也可以指定默认值。     

```
var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5
```
默认值生效的条件和数组情况时相同。     

如果要将一个已经声明的变量用于解构赋值，必须非常小心。     

```
// 错误的写法
let x;
{x} = {x: 1};
// SyntaxError: syntax error
```
上面代码的写法会报错，因为JavaScript引擎会将{x}理解成一个代码块，从而发生语法错误。只有不将大括号写在行首，避免JavaScript将其解释为代码块，才能解决这个问题。     

```
// 正确的写法
({x} = {x: 1});
```

#### 字符串的结构赋值     
字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。     

```
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```
类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。     

```
let {length : len} = 'hello';
len // 5
```

#### 数值和布尔值的解构赋值     
解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。     

```
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```
上面代码中，数值和布尔值的包装对象都有toString属性，因此变量s都能取到值。     

解构赋值的规则是，只要等号右边的值不是对象，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。     

```
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```

#### 函数参数的解构赋值     
函数的参数也可以使用解构赋值。     

```
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```

#### 圆括号问题     
解构赋值虽然很方便，但是解析起来并不容易。对于编译器来说，一个式子到底是模式，还是表达式，没有办法从一开始就知道，必须解析到（或解析不到）等号才能知道。     
由此带来的问题是，如果模式中出现圆括号怎么处理。ES6的规则是，只要有可能导致解构的歧义，就不得使用圆括号。
但是，这条规则实际上不那么容易辨别，处理起来相当麻烦。因此，建议只要有可能，就不要在模式中放置圆括号。     

**只有赋值语句的非模式部分，可以使用圆括号。**     

```
[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确
```

## 用途     

#### 交换变量的值     

```
[x, y] = [y, x];
```

#### 从函数返回多个值     

```
function example() {
	return [1, 2, 3];
}

var [x, y, z] = example();
```

#### 函数参数的定义     
解构赋值可以方便地将一组参数与变量名对应起来。     

```
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

#### 提取JSON数据     
解构赋值对提取JSON对象中的数据，尤其有用。     

```
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```

#### 函数参数的默认值     

``` 
function a({x = 1, y = 0}) {
	// ...
}

a({});
```
这样就避免了再写 x = x || 1; 这样的代码。     

