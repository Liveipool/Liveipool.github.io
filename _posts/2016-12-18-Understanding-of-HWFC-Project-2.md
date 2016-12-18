---
layout: post
title: HWFC项目理解（二）-- webpack-dev-middleware and webpack-hot-middleware   
date: 2016-12-18
categories: blog
tags: [前端]
description: HWFC项目包理解
---

# HWFC项目理解（二）-- webpack-dev-middleware and webpack-hot-middleware  

## 各文链接     
 - [HWFC项目包理解（一）-- 从目录结构和package.json开始 ](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-1/)      
 - [HWFC项目理解（二）-- webpack-dev-middleware and webpack-hot-middleware](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-2/)        
 - [HWFC项目包理解（三）-- 三个 webpack.conf.js 文件](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-3/)   
 - [HWFC项目包理解（四）-- Utils 和 Config/](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-4/)          

## webpack-dev-server     
要理解这两个中间件,首先需要了解webpack-dev-server:       
The webpack-dev-server is a little Node.js Express server, which uses the webpack-dev-middleware to serve a webpack bundle. It also has a little runtime which is connected to the server via Sock.js.     
简单的说,webpack-dev-server是一个静态资源服务器，只用于开发环境。      
它可以轻松地实现自动刷新:     
The webpack-dev-server supports multiple modes to automatic refresh the page:     
1. Iframe mode (page is embedded in an iframe and reloaded on change)     
2. Inline mode (a small webpack-dev-server client entry is added to the bundle which refresh the page on change)     

Each mode also supports Hot Module Replacement. In Hot Module replacement, the bundle is notified that a change happened (rather than a full page reload). A Hot Module Replacement runtime could then load the updated modules and inject them into a running app.        

#### HMR热交换       
Hot Module Replacement (HRM) 又称热替换，功能就是在程式运行中交换，移除，增加模组且不会使页面重新载入。
它是怎么运作的呢?     
webpack 在bundle 中即我们的js 里加入了一个小型的HMR 执行环境(HMR runtime)，在编译过程中这个runtime 会在我们的app 中运行。
![bundle server结构图.png](http://upload-images.jianshu.io/upload_images/3001083-a8030763a998066f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
当App 程式开始执行(就是载入bundle) HMR runtime 执行环境就会启用，接下来程式就会要求HMR runtime 帮我们检查是否需要更新。HMR 会帮我们下载更新然后通知App 程式有哪些更新可用。     

然而,虽然webpack-dev-server能够实现热替换,并且它也有一定的特性,但总的来说,功能还是比较局限,只适合于做纯前端开发时的一个静态资源服务器.而当我们要做比如HWFC这样的一个全栈开发的项目的时候,应该怎么做呢?     
HWFC项目中的做法是使用Express这个后端node.js框架,这里对Express就不再赘述,那么怎样使用Express来结合webpack实现一个功能齐全的全栈开发环境呢?       
**这里就需要使用到webpack-dev-middleware和webpack-hot-middleware了.**     

## webpack-dev-middleware     
npm上的解释是: It's a simple wrapper middleware for webpack. It serves the files emitted from webpack over a connect server.     
官网上的解释是:The webpack-dev-middleware is a small middleware for a connect-based middleware stack. It uses webpack to compile assets in-memory and serve them. When a compilation is running every request to the served webpack assets is blocked until we have a stable bundle.     
其实就是说,它是一个处理静态资源的middleware。将webpack打包好的文件装成一个bundle,代码进行了更改后,它又产生一个新的bundle,并且舍弃掉旧的bundle.     

前面说的webpack-dev-server，实际上是一个小型的只有部分功能的Express服务器，它也是用webpack-dev-middleware来处理webpack编译后的输出。     

webpack-dev-middleware有两种模式:     
watch mode (default): The compiler recompiles on file change.     
lazy mode: The compiler compiles on every request to the entry point.     

It has a few advantages over bundling it as files:     
1. No files are written to disk, it handle the files in memory     
2. If files changed in watch mode, the middleware no longer serves the old bundle, but delays requests until the compiling has finished. You don't have to wait before refreshing the page after a file modification.     

## webpack-hot-middleware     
npm上的解释: Webpack hot reloading using only webpack-dev-middleware. This allows you to add hot reloading into an existing server without webpack-dev-server.     

我们都知道webpack-dev-server有提供一种Hot Module Replacement/Hot Reloading热替换的功能。在使用webpack-dev-server的时候我们会在webpack.config.js加入new webpack.HotModuleReplacementPlugin()设定来启用。     

而webpack-hot-middleware是搭配给webpack-dev-middleware用的,就是让我们在一般的server上加上热替换的功能.     
而这两个中间件具体是怎样使用的呢?这就要通过代码来看了,具体在HWFC项目中,是在dev-server.js和dev-client.js中配置编写的.     

## dev-server.js  和 dev-client.js
还记得项目开发时的启动命令npm run dev吗,它其实是node build/dev-server的简写,也就是运行dev-server.js文件,而这个文件的功能、解释等,我也写在了注释中.         

``` javascript
//dev-server.js
//执行这个函数来检查node和npm的版本是否正确
require('./check-versions')()
var config = require('../config')
if (!process.env.NODE_ENV) process.env.NODE_ENV = JSON.parse(config.dev.env.NODE_ENV)
//Node.js中的Path对象，用于处理目录的对象，提高开发效率。
var path = require('path')
var express = require('express')
var webpack = require('webpack')
//A better node-open. Opens stuff like websites, files, executables. Cross-platform.
var opn = require('opn')
//Node.js proxying made simple. Configure proxy middleware with ease for connect, express, browser-sync and many more.
var proxyMiddleware = require('http-proxy-middleware')
var webpackConfig = require('./webpack.dev.conf')

// default port where dev server listens for incoming traffic
var port = process.env.PORT || config.dev.port

// Define HTTP proxies to your custom API backend
// 值为 {}
var proxyTable = config.dev.proxyTable

//创建一个express实例
var app = express()

//webpack的角色就是我们的编译器,compiler是webpack的一个实例
var compiler = webpack(webpackConfig)

//重点来了，我们有了express服务器，有了编译核心webpack，接着我们需要wrapper 来打包webpack 将其合进express 的middleware stack 中。
var devMiddleware = require('webpack-dev-middleware')(compiler, {
  //The path where to bind the middleware to the server.
  //In most cases this equals the webpack configuration option output.publicPath
  //publicPath就是我们想要存取前端bundle 的网址，路径，位置。
  publicPath: webpackConfig.output.publicPath,
  stats: {
    colors: true,
    chunks: false
  }
})

//然后我们要再加上webpack-hot-middleware 使其具备热替换的功能。
//reuire出来的是一个函数,这行代码加上下面的app.use(hotMiddleware)相当于:
//var WebpackHotMiddleware = require('webpack-hot-middleware')
//app.use(WebpackHotMiddleware(compiler))
var hotMiddleware = require('webpack-hot-middleware')(compiler)

//要看懂这几段代码,需要知道两个重要的概念,compiler和compilation
//compiler编译器物件代表一个完整设定的webpack环境。这个物件在webpack发动之后就会被建置，而且只会建置一次。
//然后它会配置所有可以操作的设定包含loaders, plugins。
//当我们套用一个plugin时这个plugin会收到compiler的参考透过存取这个參考,reference就可以取得webpack环境.
//compilation是指编译后的编译结果，在运行webpack dev middleware期间每当档案发生异动就会产生一个新的compilation也就是产生新的编译结果。
//这个编译结果包含的讯息包含module模组的状态，编译后的资源档，发生异动的档案，被观察的相依套件等。
//这个编译结果物件也提供一些执行callback的机会让我们可以在过程中客制一些自己想要的行为。

// force page reload when html-webpack-plugin template changes
// webpack插件,监听html文件改变事件
compiler.plugin('compilation', function (compilation) {
  compilation.plugin('html-webpack-plugin-after-emit', function (data, cb) {
    // 发布事件
    hotMiddleware.publish({ action: 'reload' })
    cb()
  })
})

// 暂时没有用到,proxyTable现在是{},以后可以具体理解使用
Object.keys(proxyTable).forEach(function (context) {
  var options = proxyTable[context]
  if (typeof options === 'string') {
    options = { target: options }
  }
  app.use(proxyMiddleware(context, options))
})

// handle fallback for HTML5 history API
app.use(require('connect-history-api-fallback')())

// serve webpack bundle output
//重点来了，我们有了伺服器express，有了编译核心webpack，接着我们需要wrapper 来打包webpack 将其合进express 的middleware stack 中。
app.use(devMiddleware)

// enable hot-reload and state-preserving
// compilation error display
//然后我们要再加上webpack-hot-middleware 使其具备热替换的功能。
app.use(hotMiddleware)

// serve pure static assets
var staticPath = path.posix.join(config.dev.assetsPublicPath, config.dev.assetsSubDirectory)
app.use(staticPath, express.static('./static'))

module.exports = app.listen(port, function (err) {
  if (err) {
    console.log(err)
    return
  }
  var uri = 'http://localhost:' + port
  console.log('Listening at ' + uri + '\n')

  // when env is testing, don't need open it
  if (process.env.NODE_ENV !== 'testing') {
    //这是用来在运行npm run dev时自动打开浏览器的,可以注释掉试试,它就不会再自动打开了.
    opn(uri)
  }
})
```

``` javascript
// dev-client.js
/* eslint-disable */
require('eventsource-polyfill')
var hotClient = require('webpack-hot-middleware/client?noInfo=true&reload=true')

// 订阅事件,这里订阅的是dev-server.js文件里的hotMiddleware.publish({ action: 'reload' })
hotClient.subscribe(function (event) {
  if (event.action === 'reload') {
    window.location.reload()
  }
})
```
几乎所有的介绍都在注释中了,而用一两句话来概括这部分内容: **在HWFC项目中,我们使用Express来写一个可以有强大功能的后台服务器,而webpack-dev-middleware+ webpack-hot-middleware即可让我们用Express来实现一个有热替换功能的webpack开发服务器。**        
讲到这里,我们似乎已经具备开发的一些基本功能了,但这个vue-cli + webpack的构建的项目,自然也少不了核心的webpack,下一篇我们就将重点介绍一下webpack.     


