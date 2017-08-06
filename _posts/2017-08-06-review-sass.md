---
layout: post
title: Sass 基本语法复习
date: 2017-08-06
categories: blog
description: 前端学习
---

# Sass 基本语法复习    
因为明天要举办商汤第一届css王者挑战赛哈哈，最近主要负责的也是产品中组件的开发和集成测试等，没有开发页面，也没怎么写css，因此今天来复习一下sass的基本用法，记录下一些重点    

#### 变量    

```
$primary-color: #000

body
  color: $primary-color
```

#### 嵌套    
用缩进来写就好    

#### @import    

``` scss
// a.sass
body
	width: 100%
```

``` scss
// b.sass

@import a

div
	width: 100%
```

#### Mixins    

``` scss
=border-radius($radius)
  -webkit-border-radius: $radius
  -moz-border-radius:    $radius
  -ms-border-radius:     $radius
  border-radius:         $radius

.box
  +border-radius(10px)
```

比如，写一个添加浏览器前缀的mixins：    

``` scss
=prefix($property, $value)
  -webkit-#{$property}: $value
  -moz-#{property}: $value
  -ms-#{$property}: $value
  #{$property}: $value

div
  +prefix(border-redius, 50px)
```

#### @extend    

``` scss
.message
  border: 1px solid #ccc

.success
  @extend .message
  border-color: green
```

#### &(父选择器)的使用    

``` css
.clearfix:before,
.clearfix:after {
	content:"";
	display:table;
}
.clearfix:after {
	clear:both;
	overflow:hidden;
}
.clearfix {
	*zoom: 1;
}

.mod.on {
    color: green;
}
```

``` scss
$lte-ie: true !default;

.clearfix {
	@if $lte-ie {
		*zoom: 1;
	}
	&:before,
	&:after {
		content: "";
		display: table; 
	}
	&:after {
		clear: both;
		overflow: hidden;
	}
}

.mod {
    &.on{
        color: green;
    }
}
```

#### 注释    
Sass 支持标准的 CSS 多行注释 /* */，以及单行注释 //    

#### 运算    
支持各种基本运算符    

#### 插值语句#{}    

``` scss
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
}
```

#### 变量定义 !default    
可以在变量的结尾添加 !default 给一个未通过 !default 声明赋值的变量赋值，此时，如果变量已经被赋值，不会再被重新赋值，但是如果变量还没有被赋值，则会被赋予新的值。    

``` scss
$content: "First content";
$content: "Second content?" !default;
$new_content: "First time reference" !default;

#main {
  content: $content;
  new-content: $new_content;
}
```
编译为：    

``` scss
#main {
  content: "First content";
  new-content: "First time reference";
}
```

#### @media    
Sass 中 @media 指令与 CSS 中用法一样，只是增加了一点额外的功能：允许其在 CSS 规则中嵌套。如果 @media 嵌套在 CSS 规则内，编译时，@media 将被编译到文件的最外层，包含嵌套的父选择器。这个功能让 @media 用起来更方便，不需要重复使用选择器，也不会打乱 CSS 的书写流程。    

``` scss
.sidebar {
  width: 300px;
  @media screen and (orientation: landscape) {
    width: 500px;
  }
}
```
编译为：    

``` scss
.sidebar {
  width: 300px;
}
@media screen and (orientation: landscape) {
  .sidebar {
    width: 500px;
  }
}  
```

#### 条件和循环    
if(), @if, @for, @each等，暂时没用到过    

#### 动画    
就按照css那样写@keyframes就好，试了一下，区别其实就是sass不用写括号，或者要写成加各浏览器前缀的代码可以写成mixins比较方便。    

#### 参考    
[DOC](http://sass-lang.com/guide)    
[Sass中连体符（&）的运用](http://www.w3cplus.com/preprocessor/use-ampersand-in-selector-name-with-Sass.html)    
[中文文档](https://www.sass.hk/docs/)    