---
layout: post
title: HWFC项目包理解（一）-- 从目录结构和package.json开始 
date: 2016-12-18
categories: blog
tags: [前端]
description: HWFC项目包理解
---

# 从目录结构和package.json开始     
> 这个项目是通过vue-cli + webpack自动构建的,但构建一时爽,要是不趁着项目规模还不大,把源码都读一遍弄懂,以后可能会出现项目变得臃肿或者想要改良却因为对项目本身的结构或者一些文件不熟悉而难以下手.只有把源码都看完一遍,弄懂每个文件的含义、作用,以后才能在想要进行修改的时候觉得更加得心应手.        
   
> 几天看下来,对webpack也算是有了一个基本的理解,但还是感觉webpack的很多特性功能都还没看到,它之所以这么火,肯定还是有它强大的地方,加之无论是commonjs、AMD还是ES6的模块化处理,感觉现在还是只懂个表面,还没实际运用体会过,整个模块化的流程结构,或者是为什么要将某一块分出来,它到了哪些地方等等.模块化是一个相当好的概念,也是一种趋势,在以后做项目的过程中慢慢加体会.          

> 下面就开始对整个项目进行分析,由于篇幅较长,我将其分为了多个部分,可以通过上面的链接进行查看. 这些分析主要针对的是后端部分，整个项目是怎么搭建起来的，用的是什么技术，使用了哪些插件，有些什么功能等.而前端部分，即src文件夹下，阅读vue的文档后应该就能理解.当然,这几篇博客只是在一个较短时间内阅读代码的理解,可能有些地方还有待商榷,这就需要在后面的学习中继续研究了.接下来正式开始对项目包的分析.           

> 首先,从目录结构和package.json开始.     

#### 相关链接               
 - [HWFC球队网站项目需求文档](http://liveipool.com/blog/2017/02/16/HWFC-Requirements-Documentation)                      
 - [HWFC球队网站项目编码规范](http://liveipool.com/blog/2017/02/16/HWFC-Coding-Specification)                  
 - [HWFC球队网站项目设计图、原型图](http://liveipool.com/blog/2017/02/16/HWFC-Design-Diagram)                      
 - [HWFC项目包理解（一）-- 从目录结构和package.json开始 ](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-1/)      
 - [HWFC项目包理解（二）-- webpack-dev-middleware and webpack-hot-middleware](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-2/)        
 - [HWFC项目包理解（三）-- 三个 webpack.conf.js 文件](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-3/)   
 - [HWFC项目包理解（四）-- Utils 和 Config/](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-4/)           

## 目录结构     
- build/ ——————————— 构建项目的各配置文件     
  - build.js ———————— "production" 时的服务器代码     
  - check-version.js ———— 检测node和npm的版本(这两个的版本在项目中有范围要求)     
  - dev-client.js —————— 主要用在"development"时订阅更新,实现热替换     
  - dev-server.js —————— "development" 时的服务器代码     
  - utils.js  ————————— 一些重点函数的效用     
  - webpack.base.conf.js —— webpack的基本配置文件     
  - webpack.dev.conf.js ——— "development" 时webpack的针对性配置     
  - webpack.prod.conf.js —— "production" 时webpack的针对性配置     
- config/ ——————————— 两种运行环境的一些配置     
  - dev.env.js ——————— "development" 时的运行环境     
  - index.js ———————— 两种运行环境的一些配置信息     
  - prod.env.js ——————— "production" 时的运行环境     
- node_modules/       
- src/ ————————————— 前端代码文件     
  - components/  —————— 放置项目的通用界面框架模板里的一些组件     
  - pages/ ————————— 放置各具体功能页面，每个页面单独一个文件夹，如：pages/404/， pages/Home/     
  - Main.vue ———————— 项目的通用界面框架模板     
  - main.js ————————— 创建项目启动文件index.html里的app Vue实例     
  - routes.js ———————— 记录路由信息     
- static/———————————— 存放一些静态资源文件      
  - assets/ ————————— 放置图片     
  - .gitkeep ————————— 若static文件夹为空，git会自动忽略掉，加了这个文件防止static文件夹被忽略掉     
- .babelrc ——————————  babel的配置文件     
- .editorconfig ———————— 用于帮助开发者在不同编辑器和IDE之间定义和维护一致的代码风格     
- .gitignore —————————— git会忽略其中设置的那些文件     
- index.html —————————— 开发时的启动页面     
- package.json ————————— 项目的配置信息      

以上的各文件只采用了一两句话概括,不懂没有关系,只是先有个大概的印象,后面会对各文件进行详细的讲解.          

## package.json      
package.json: 在实际的项目中，json文件是不能有注释的，这里是放到博客上方便理解而添加的注释     

``` javascript
//package.json
{
  "name": "hwfc",
  "version": "1.0.0",
  "description": "official website of HWFC",
  "author": "liveipool",
  //如果这个属性被设置为true，npm将拒绝发布它，这是为了防止一个私有模块被无意间发布出去。
  "private": true,
  // scripts指定了运行脚本命令的npm命令行缩写，比如npm run dev，就是node build/dev-server.js的简写。
  "scripts": {
    "dev": "node build/dev-server.js",
    "build": "node build/build.js"
  },
  //dependenceies字段指定了项目运行时所依赖的模块
  "dependencies": {
    "vue": "^2.0.1"
  },
  //devDependencies指定项目开发时所需要的模块
  "devDependencies": {
    //css中自动添加兼容各浏览器的前缀
    "autoprefixer": "^6.4.0",
    //关于babel的都可以暂时理解为都是ES6的编译器或叫转换器,转为ES5
    "babel-core": "^6.0.0",
    "babel-loader": "^6.0.0",
    "babel-plugin-transform-runtime": "^6.0.0",
    "babel-preset-es2015": "^6.0.0",
    "babel-preset-stage-2": "^6.0.0",
    "babel-register": "^6.0.0",
    //Terminal string styling done right. Much color.用来使终端中的提示变得有色彩,增强用户体验.
    "chalk": "^1.1.3",
    //handle fallback for HTML5 history API.暂时没有直接用到,有空可以了解下.
    "connect-history-api-fallback": "^1.1.0",
    //配合style-loader、extract-text-webpack-plugin来使用,将所有样式代码提取到一个单独的css文件并通过<link>标签插入到html文件中.
    "css-loader": "^0.25.0",
    //polyfill是web开发中的一句黑话,它的准确意思是:用于实现浏览器并不支持的原生API的代码。
    //而这个插件是:A browser polyfill for W3C EventSource (http://www.w3.org/TR/eventsource/)
    //Provide polyfill to support EventSource in browser where it is not available.
    "eventsource-polyfill": "^0.9.6",
    //后端node.js框架
    "express": "^4.13.3",
    //配合style-loader、css-loader来使用,将所有样式代码提取到一个单独的css文件并通过<link>标签插入到html文件中.
    "extract-text-webpack-plugin": "^1.0.1",
    //file loader module for webpack,项目中还未直接用到
    "file-loader": "^0.9.0",
    //Implementation of Function.prototype.bind。项目中还未直接用到
    "function-bind": "^1.0.2",
    //用来生成index.html文件,并自动注入js的依赖到dist/index.html中body的尾部.
    "html-webpack-plugin": "^2.8.1",
    //Node.js proxying made simple. Configure proxy middleware with ease for connect, express, browser-sync and many more.
    "http-proxy-middleware": "^0.17.2",
    //用来解析json文件
    "json-loader": "^0.5.4",
    //Node-sass is a library that provides binding for Node.js to LibSass, the C version of the popular stylesheet preprocessor, Sass.
    //It allows you to natively compile .scss files to css at incredible speed and automatically via a connect middleware.
    "node-sass": "^3.13.0",
    //A better node-open. Opens stuff like websites, files, executables. Cross-platform.
    //这个项目中用来在开发运行npm run dev时自动打开浏览器.
    "opn": "^4.0.2",
    //Elegant terminal spinner
    "ora": "^0.3.0",
    //The sass-loader requires node-sass and webpack as peerDependency. 
    "sass-loader": "^4.0.2",
    //The semantic version parser used by npm.用在check-version文件中.
    "semver": "^5.3.0",
    //ShellJS is a portable (Windows/Linux/OS X) implementation of Unix shell commands on top of the Node.js API.
    //在build.js中使用
    "shelljs": "^0.7.4",
    //解析图片或字体等
    "url-loader": "^0.5.7",
    //vue-loader可以将.vue文件转换为webpack包
    //因此经常通过vue + vue-loader + webpack来构建项目,它们融合得很好
    "vue-loader": "^9.4.0",
    //等同于style-loader,配合css-loader、extract-text-webpack-plugin来使用,将所有样式代码提取到一个单独的css文件并通过<link>标签插入到html文件中.
    "vue-style-loader": "^1.0.0",
    //当然要用
    "webpack": "^1.13.2",
    //下面两个包名字中有middleware,是因为它们是作为express中间件使用
    // a simple wrapper middleware for webpack. It serves the files emitted from webpack over a connect server.
    //对webpack一个简单的包装，它可以通过连接服务器服务那些从webpack发射出来的文件
    "webpack-dev-middleware": "^1.8.3",
    //只能配合webpack-dev-middleware使用,以实现更改后自动刷新页面(热替换)的中间件
    "webpack-hot-middleware": "^2.12.2",
    //webpack-merge provides a merge function that concatenates arrays and merges objects creating a new object. 
    "webpack-merge": "^0.14.1"
  },
  //engines字段指明的是项目所需的node.js版本范围
  "engines": {
    "node": ">= 4.0.0",
    "npm": ">= 3.0.0"
  }
  //此项目中没写，但经常可以看见的main字段，main字段指定了加载的入口文件。
  //意思是，如果我们的项目模块名为hwfc，那么其他人安装了这个模块并require（‘hwfc’）就可以使用这个模块。
  //而require返回的内容就是main属性指定的文件中 module.exports指向的对象。这个字段的默认值是模块根目录下的index.js.
}
```

## .babelrc       

```
  module: {
    loaders: [
      {
        //vue-loader，解析.vue文件
        test: /\.vue$/,
        loader: 'vue'
      },
      {
        //babel-loader，转化ES6的语法 
        test: /\.js$/,
        loader: 'babel',
        include: projectRoot,
        exclude: /node_modules/
      },
```

在webpack.conf.js中设置moule.loaders的时候，如果不给babel-loader指定query，也可以在项目根目录下新建一个.babelrc文件，把内容设置成如下，以达到同样的效果。因为babel默认会去读.babelrc这个文件。       

![babelrc.png](http://upload-images.jianshu.io/upload_images/3001083-dad1cfeed254126b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        

## .editorconfig       
EditorConfig是一套用于统一代码格式的解决方案，很多项目都有用到，比如Bootstrap、jQuery、Underscore和Ruby等等。       
EditorConfig可以帮助开发者在不同的编辑器和IDE之间定义和维护一致的代码风格。EditorConfig包含一个用于定义代码格式的文件和一批编辑器插件，这些插件可以让编辑器读取配置文件并依此格式化代码。EditorConfig的配置文件十分易读，并且可以很好的在VCS（Version Control System）下工作。        
当打开一个文件时，EditorConfig插件会在打开文件的目录和其每一级父目录查找.editorconfig文件，直到  有一个配置文件root=true。     
EditorConfig配置文件从上往下读取，并且路径最近的文件最后被读取。匹配的配置属性按照属性应用在代码上，所以最接近代码文件的属性优先级最高。      

## .gitkeep       
一句话说,git会忽略里面为空的目录,所以要想保留那个目录,一般在其中添加一个.gitkeep文件.因为项目里最初的static文件夹为空,所以需要加一个.gitkeep	     

## 两个命令       
接着再来看看两个命令运行后terminal里的输出,有个印象     

npm run dev (node build/dev-server.js):      
![npm run dev.png](http://upload-images.jianshu.io/upload_images/3001083-e1bee6925e527168.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
接下来就可以在浏览器中的8080端口看到对应的效果了.     

npm run build (node build/build.js): 使用webpack打包好的所有前端资源，可用来拿去和一个后台服务端代码对接     
![npm run build.png](http://upload-images.jianshu.io/upload_images/3001083-04cf6ff9881ec0ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
 
打包后(npm run build)所有的文件会生成在根目录下的dist文件夹下：     
![dist目录.png](http://upload-images.jianshu.io/upload_images/3001083-e573fe703cd527e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)        

dist文件夹的位置config/index.js文件里设定的：     
![这是在config.png](http://upload-images.jianshu.io/upload_images/3001083-9e01cf95d5ea1769.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     


以上内容包括了目录结构及一些概括解释,以及package.json文件代码及里面一些配置信息的解释,再看了一下两个启动命令运行后的效果.       
接下来,我们从npm run dev的角度,来看是怎样通过这个命令启动服务器运行项目的.     