---
layout: post
title:  Vue.js 更改对象中数组不更新视图相关总结
date: 2018-08-22
categories: blog
description: 前端学习
---

# Vue.js 更改对象中数组不更新视图相关总结

最后我想达成的数据结构是这样的：
``` javascript
arrays: [
    {
        anotherArray: ['10'],
        subArray: ['0']
    }
]
```
在实际的需求中，我可能会遇到下面几种情况：

#### 在Data里已经写好结构了，直接更改arrays数组第0项的subArray的值：

``` javascript
FesData () {
    return {
        arrays: [
            {
                anotherArray: ['10'],
                subArray: ['0']
            }
        ]
    };
},
updated () {
    console.log('updated');
},
FesReady () {
    console.log('this.arrays[0]: ', this.arrays[0]);
    console.log('this.arrays[0].anotherArray: ', this.arrays[0].anotherArray);
    console.log('this.arrays[0].subArray: ', this.arrays[0].subArray);
    // 输出：
    // this.arrays[0]:  {subArray: Array(1), __ob__: Observer}
    // this.arrays[0].anotherArray:  ["10", __ob__: Observer]
    // this.arrays[0].subArray:  ["0", __ob__: Observer]

	// 可以看到，从arrays数组本身到它的每一个属性都被Observe了，所以无论使用下面哪种方式去更改它们，都会更新视图
},
methods: {
    change () {
        console.log('1 this.arrays[0].subArray: ', this.arrays[0].subArray);

        // 写法1
        this.arrays.splice(0, 1, Object.assign({}, this.arrays[0], {
            subArray: ['1']
        }));

        // 写法2
        // this.arrays[0].subArray = ['1'];

        // 写法3
        // this.$set(this.arrays[0], 'subArray', ['1']);

        console.log('2 this.arrays[0].subArray: ', this.arrays[0].subArray);

        // 写法1、2、3都可以更新视图，输出
        // 1 this.arrays[0].subArray:  ["0", __ob__: Observer]
        // 2 this.arrays[0].subArray:  ["1", __ob__: Observer]
        // updated
    }
}
```

#### 只在Data中定义了arrays的结构，在后面去动态的给它添加项，然后更改某一项的某个已存在属性

``` javascript
FesData () {
    return {
        arrays: []
    };
},
updated () {
    console.log('updated');
},
FesReady () {
    const obj = {
        anotherArray: ['10'],
        subArray: ['0']
    }
    this.arrays.push(obj);

    console.log('this.arrays[0]: ', this.arrays[0]);
    console.log('this.arrays[0].anotherArray: ', this.arrays[0].anotherArray);
    console.log('this.arrays[0].subArray: ', this.arrays[0].subArray);
    // 输出：
    // this.arrays[0]:  {subArray: Array(1), __ob__: Observer}
    // this.arrays[0].anotherArray:  ["10", __ob__: Observer]
    // this.arrays[0].subArray:  ["0", __ob__: Observer]
    
	// 可以看到，从arrays数组本身到它的每一个属性都被Observe了，所以无论使用下面哪种方式去更改它们，都会更新视图
},
methods: {
    change () {
        console.log('1 this.arrays[0].subArray: ', this.arrays[0].subArray);

        // 写法1
        this.arrays.splice(0, 1, Object.assign({}, this.arrays[0], {
            subArray: ['1']
        }));

        // 写法2
        // this.arrays[0].subArray = ['1'];

        // 写法3
        // this.$set(this.arrays[0], 'subArray', ['1']);

        console.log('2 this.arrays[0].subArray: ', this.arrays[0].subArray);

        // 可以更新视图，输出
        // 1 this.arrays[0].subArray:  ["0", __ob__: Observer]
        // 2 this.arrays[0].subArray:  ["1", __ob__: Observer]
        // updated
    }
}
```

#### 有了arrays的结构，也有一些项的结构了，但要给某些项添加新属性（错误写法）
之前我给某一项添加了一个新属性后，再更改这个新属性，不会更新视图，就是因为我使用的这一种写法：

``` javascript
FesData () {
    return {
        arrays: [
            {
                anotherArray: ['10']
            }
        ]
    };
},
updated () {
    console.log('updated');
},
FesReady () {
	// 这样写就会导致subArray不会被Observe到
    this.arrays[0].subArray = ['0'];

    console.log('this.arrays[0]: ', this.arrays[0]);
    console.log('this.arrays[0].anotherArray: ', this.arrays[0].anotherArray);
    console.log('this.arrays[0].subArray: ', this.arrays[0].subArray);
    // 输出：
    // this.arrays[0]:  {subArray: Array(1), __ob__: Observer}
    // this.arrays[0].anotherArray:  ["10", __ob__: Observer]
    // this.arrays[0].subArray:  ["0"]
    
    // 在这里可以清楚的看到，本身的arrays和array的anotherArray属性都是被Observe的，所以无论使用下面的那种写法来更改arrays或者arrays[0].anotherArray，只要他们有改动，就一定会更新视图
},
methods: {
    change () {
        console.log('1 this.arrays[0].subArray: ', this.arrays[0].subArray);

        // 写法1，可以更新视图
        // this.arrays.splice(0, 1, Object.assign({}, this.arrays[0], {
        //     subArray: ['1']
        // }));

        // 写法2，不能更新视图
        // this.arrays[0].subArray = ['1'];

        // 写法3，不能更新视图
        this.$set(this.arrays[0], 'subArray', ['1']);

        console.log('2 this.arrays[0].subArray: ', this.arrays[0].subArray);

        // 写法1，可以更新视图，输出：
        // 1 this.arrays[0].subArray:  ["0"]
        // 2 this.arrays[0].subArray:  ["1", __ob__: Observer]
        // updated

        // 写法2、3，不能更新视图，输出：
        // 1 this.arrays[0].subArray:  ["0"]
        // 2 this.arrays[0].subArray:  ["1"]

        // 因为this.arrays[0].subArray没有被Observe，所以除了第一种方法可以生效：把arrays[0]这一项整体删掉，再添加进有subArray属性的新对象。其他的两种方法都不会在改变this.arrays[0].subArray的值后更新视图
    }
}
```

#### 有了arrays的结构，也有一些项的结构了，但要给某些项添加新属性（正确写法）
在Vue中，给对象添加新属性时，最好使用[set](https://cn.vuejs.org/v2/api/index.html#vm-set)这种写法，才会被Observe到。

``` javascript
FesData () {
    return {
        arrays: [
            {
                anotherArray: ['10']
            }
        ]
    };
},
updated () {
    console.log('updated');
},
FesReady () {
	// 这样写才会被Observe到
    this.$set(this.arrays[0], 'subArray', ['0']);

    console.log('this.arrays[0]: ', this.arrays[0]);
    console.log('this.arrays[0].anotherArray: ', this.arrays[0].anotherArray);
    console.log('this.arrays[0].subArray: ', this.arrays[0].subArray);
    // 输出：
    // this.arrays[0]:  {subArray: Array(1), __ob__: Observer}
    // this.arrays[0].anotherArray:  ["10", __ob__: Observer]
    // this.arrays[0].subArray:  ["0", __ob__: Observer]
    
},
methods: {
    change () {
        console.log('1 this.arrays[0].subArray: ', this.arrays[0].subArray);

        // 写法1，可以更新视图
        this.arrays.splice(0, 1, Object.assign({}, this.arrays[0], {
            subArray: ['1']
        }));

        // 写法2，可以更新视图
        // this.arrays[0].subArray = ['1'];

        // 写法3，可以更新视图
        // this.$set(this.arrays[0], 'subArray', ['1']);

        console.log('2 this.arrays[0].subArray: ', this.arrays[0].subArray);

        // 写法1、2、3，都可以更新视图，输出：
        // 1 this.arrays[0].subArray:  ["0", __ob__: Observer]
        // 2 this.arrays[0].subArray:  ["1", __ob__: Observer]
        // updated
    }
}
```