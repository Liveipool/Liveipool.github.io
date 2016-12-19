---
layout: post
title: HWFC项目包理解（四）-- Utils 和 Config/      
date: 2016-12-18
categories: blog
tags: [前端]
description: HWFC项目包理解
---      

# Utils 和 Config/     

> 在上文中提到了,有两个模块经常被使用到, 它们是Utils.js文件和config文件夹,config文件夹下还有几个js文件,它们也是一些模块,而config在项目中被作为一个整体的环境配置模块来使用,接下来我们通过代码和注释来分析.     


## 各文链接     
 - [HWFC项目包理解（一）-- 从目录结构和package.json开始 ](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-1/)      
 - [HWFC项目包理解（二）-- webpack-dev-middleware and webpack-hot-middleware](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-2/)        
 - [HWFC项目包理解（三）-- 三个 webpack.conf.js 文件](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-3/)   
 - [HWFC项目包理解（四）-- Utils 和 Config/](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-4/)


## utils.js     

``` javascript
// utils.js
var path = require('path')
var config = require('../config')
var ExtractTextPlugin = require('extract-text-webpack-plugin')

//这个函数的作用是用来返回进行打包处理后的文件在dist文件夹下的完整相对路径
//如static/js/[name].[chunkhash].js
//这个路径再加上各webpack.conf.js文件中的output下设置的path根路径如:HWFC/dist即为最终文件的完整路径.
//以后打包放到某个服务器上时要改动也只需要改动path属性,不用改动里面的各文件相对于根目录的路径.
exports.assetsPath = function (_path) {
  var assetsSubDirectory = process.env.NODE_ENV === 'production' 
    ? config.build.assetsSubDirectory : config.dev.assetsSubDirectory
    //config.build.assetsSubDirectory 和 config.dev.assetsSubDirectory的值相同,为'static'

  //总是以posix兼容的方式交互
  //posix是可移植作業系統接口（英语：Portable Operating System Interface of UNIX，縮寫為POSIX）
  //是IEEE为要在各种UNIX操作系统上运行软件，而定义API的一系列互相关联的标准的总称
  return path.posix.join(assetsSubDirectory, _path)

  // console.log(path.posix.join(assetsSubDirectory, _path)):

  //npm run dev:
  // static/img/[name].[hash:7].[ext]
  // static/fonts/[name].[hash:7].[ext]

  //npm run build:
  // static/img/[name].[hash:7].[ext]
  // static/fonts/[name].[hash:7].[ext]
  // static/js/[name].[chunkhash].js
  // static/js/[id].[chunkhash].js
  // static/css/[name].[contenthash].css
}

//这个函数的返回值为一个对象,这个对象里是各loaders的名值对
//如果是npm run build时,这个函数含有提取所有样式代码的作用.(extract部分)
exports.cssLoaders = function (options) {
  // 运行npm run dev时,cssLoaders会执行两次
  // 在webpack.base.conf.js中直接执行一次,在webpack.dev.conf.js中通过styleLoaders函数再执行一次.
  // 运行npm run build时,cssLoaders会执行三次
  // 在webpack.base.conf.js中直接执行一次,在webpack.prod.conf.js中直接执行一次,通过styleLoaders函数再执行一次.

  // 传进来的options有两种可能的取值,console.log("options: ", options):
  // 运行npm run dev,即process.env.NODE_ENV === 'development'时
  // options:  { sourceMap: false }
  // 运行npm run build,即process.env.NODE_ENV === 'production'时
  // options:  { sourceMap: true, extract: true }
  options = options || {}

  // loaders是一个数组,在下面的return里
  function generateLoaders (loaders) {
    // map()函数是对数组中的每一想运行给定函数,返回每次函数调用结果组成的数组,
    // map()是ES5中数组的5个迭代方法之一,因此sourceLoader也应该是一个数组
    var sourceLoader = loaders.map(function (loader) {
      var extraParamChar
      // 如果该字符串中有问号,进进入这个if
      if (/\?/.test(loader)) {
        // 把问号替换为 '-loader?',
        // 如下文的sass?indentedSyntax输出会变为sas-loader?indentedSyntax
        loader = loader.replace(/\?/, '-loader?')
        extraParamChar = '&'
      } else {
        loader = loader + '-loader'
        extraParamChar = '?'
      }
      return loader + (options.sourceMap ? extraParamChar + 'sourceMap' : '')
    }).join('!')
    // console.log("sourceLoader: ", sourceLoader):
    // npm run dev 时结果为:
    // 如: sourceLoader:  css-loader
    // 如: sourceLoader:  css-loader!less-loader
    // 如: sourceLoader:  css-loader!sass-loader?indentedSyntax
    // ...
    // npm run build 时结果为:
    // 如: sourceLoader:  css-loader?sourceMap
    // 如: sourceLoader:  css-loader?sourceMap!less-loader?sourceMap
    // 如: sourceLoader:  css-loader?sourceMap!sass-loader?indentedSyntax&sourceMap
    // 如: sourceLoader:  css-loader?sourceMap!stylus-loader?sourceMap
    // ...

    if (options.extract) {
      // process.env.NODE_ENV === 'production'
      // style-loader 和后面的一大堆如css-loader总是结合使用的
      // 这一步是将所有的样式代码提取出来准备放到一个css文件里,
      // 而这个文件的路径、名称等实在webpack.prod.conf.js中的new ExtractTextPlugin()里定义的.
      return ExtractTextPlugin.extract('vue-style-loader', sourceLoader)
    } else {
      // process.env.NODE_ENV === 'development'
      return ['vue-style-loader', sourceLoader].join('!')
    }
  }

  // 下面这个return即为整个cssLoaders函数的返回值
  return {
    css: generateLoaders(['css']),
    postcss: generateLoaders(['css']),
    less: generateLoaders(['css', 'less']),
    sass: generateLoaders(['css', 'sass?indentedSyntax']),
    scss: generateLoaders(['css', 'sass']),
    stylus: generateLoaders(['css', 'stylus']),
    styl: generateLoaders(['css', 'stylus'])
  }
  // npm run dev时, 返回这个对象:
  // { css:     vue-style-loader!css-loader,
  // postcss: vue-style-loader!css-loader,
  // less:    vue-style-loader!css-loader!less-loader,
  // sass:    vue-style-loader!css-loader!sass-loader?indentedSyntax,
  // scss:    vue-style-loader!css-loader!sass-loader,
  // stylus:  vue-style-loader!css-loader!stylus-loader,
  // styl:    vue-style-loader!css-loader!stylus-loader }

  //npm run build时, 返回这个对象:
  // { css:     /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap,
  // postcss: /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap,
  // less:    /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!less-loader?sourceMap,
  // sass:    /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!sass-loader?indentedSyntax&sourceMap,
  // scss:    /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!sass-loader?sourceMap,
  // stylus:  /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!stylus-loader?sourceMap,
  // styl:    /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!stylus-loader?sourceMap }
}

// 这个函数的返回值为一个数组,里面有很多个loader.
// 这些loaders对应的放到webpack.dev.conf.js或webpack.prod.conf.js文件的module.loaders中.
// 这些loaders可以用来将.vue文件里的样式部分代码生成到外面对立的css文件中
exports.styleLoaders = function (options) {
  // console.log("options: ", options);
  var output = []
  var loaders = exports.cssLoaders(options)
  // console.log("loaders: ", loaders);
  for (var extension in loaders) {
    // console.log("extension: ", extension);
    var loader = loaders[extension]
    // console.log("loader: ", loader);
    output.push({
      test: new RegExp('\\.' + extension + '$'),
      loader: loader
    })
  }
  // console.log("output: ", output);
  return output
}
//在两种环境下,上面五个console的值为:
//npm run dev:
// options:  { sourceMap: false }

// loaders:  { css: 'vue-style-loader!css-loader',
//   postcss: 'vue-style-loader!css-loader',
//   less: 'vue-style-loader!css-loader!less-loader',
//   sass: 'vue-style-loader!css-loader!sass-loader?indentedSyntax',
//   scss: 'vue-style-loader!css-loader!sass-loader',
//   stylus: 'vue-style-loader!css-loader!stylus-loader',
//   styl: 'vue-style-loader!css-loader!stylus-loader' }

// extension:  css
// loader:  vue-style-loader!css-loader
// extension:  postcss
// loader:  vue-style-loader!css-loader
// extension:  less
// loader:  vue-style-loader!css-loader!less-loader
// extension:  sass
// loader:  vue-style-loader!css-loader!sass-loader?indentedSyntax
// extension:  scss
// loader:  vue-style-loader!css-loader!sass-loader
// extension:  stylus
// loader:  vue-style-loader!css-loader!stylus-loader
// extension:  styl
// loader:  vue-style-loader!css-loader!stylus-loader

// output:  [ { test: /\.css$/, loader: 'vue-style-loader!css-loader' },
//   { test: /\.postcss$/, loader: 'vue-style-loader!css-loader' },
//   { test: /\.less$/,
//     loader: 'vue-style-loader!css-loader!less-loader' },
//   { test: /\.sass$/,
//     loader: 'vue-style-loader!css-loader!sass-loader?indentedSyntax' },
//   { test: /\.scss$/,
//     loader: 'vue-style-loader!css-loader!sass-loader' },
//   { test: /\.stylus$/,
//     loader: 'vue-style-loader!css-loader!stylus-loader' },
//   { test: /\.styl$/,
//     loader: 'vue-style-loader!css-loader!stylus-loader' } ]

// npm run build
// options:  { sourceMap: true, extract: true }

// loaders:  { css: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap',
//   postcss: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap',
//   less: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!less-loader?sourceMap',
//   sass: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!sass-loader?indentedSyntax&sourceMap',
//   scss: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!sass-loader?sourceMap',
//   stylus: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!stylus-loader?sourceMap',
//   styl: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!stylus-loader?sourceMap' }

// extension:  css
// loader:  /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap
// extension:  postcss
// loader:  /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap
// extension:  less
// loader:  /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!less-loader?sourceMap
// extension:  sass
// loader:  /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!sass-loader?indentedSyntax&sourceMap
// extension:  scss
// loader:  /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!sass-loader?sourceMap
// extension:  stylus
// loader:  /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!stylus-loader?sourceMap
// extension:  styl
// loader:  /home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!stylus-loader?sourceMap

// output:  [ { test: /\.css$/,
//     loader: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap' },
//   { test: /\.postcss$/,
//     loader: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap' },
//   { test: /\.less$/,
//     loader: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!less-loader?sourceMap' },
//   { test: /\.sass$/,
//     loader: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!sass-loader?indentedSyntax&sourceMap' },
//   { test: /\.scss$/,
//     loader: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!sass-loader?sourceMap' },
//   { test: /\.stylus$/,
//     loader: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!stylus-loader?sourceMap' },
//   { test: /\.styl$/,
//     loader: '/home/liveipool/Desktop/HWFC/node_modules/extract-text-webpack-plugin/loader.js?{"omit":1,"extract":true,"remove":true}!vue-style-loader!css-loader?sourceMap!stylus-loader?sourceMap' } ]
```   

## config/   

### dev.env.js    

``` javascript
// dev.env.js   
var merge = require('webpack-merge')
var prodEnv = require('./prod.env')

module.exports = merge(prodEnv, {
  NODE_ENV: '"development"'
})

```   

### prod.env.js   

``` javascript
// prod.env.js   
module.exports = {
  NODE_ENV: '"production"'
}
```  

### index.js     


``` javascript
// index.js  
// see http://vuejs-templates.github.io/webpack for documentation.
var path = require('path')

module.exports = {
  build: {
    //即”production“
    env: require('./prod.env'), 
    index: path.resolve(__dirname, '../dist/index.html'),
    //即HWFC/dist/
    assetsRoot: path.resolve(__dirname, '../dist'),  
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    productionSourceMap: true,
    
    // Gzip off by default as many popular static hosts such as
    // Surge or Netlify already gzip all static assets for you.
    // Before setting to `true`, make sure to:
    // npm install --save-dev compression-webpack-plugin
    productionGzip: false,
    productionGzipExtensions: ['js', 'css']
  },
  dev: {
    //即"development"
    env: require('./dev.env'),  
    port: 8080,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {},
    // CSS Sourcemaps off by default because relative paths are "buggy"
    // with this option, according to the CSS-Loader README
    // (https://github.com/webpack/css-loader#sourcemaps)
    // In our experience, they generally work as expected,
    // just be aware of this issue when enabling this option.
    cssSourceMap: false
  }
}
```  
 
就文件而言,它们的作用不难理解,utils模块里是一些函数或插件的具体使用配置,config模块主要是存放dev和prod两种环境下的一些基本配置.    
而具体看到文件中的代码,有一些还是需要花点时间来理解的,我也都写到注释中了,希望能起到帮忙理解的作用.      
到这里,从目录结构和package.json开始,再说到如何配置服务端代码,再讲了webpack的配置代码,这里又补充讲了一些剩余主要文件的作用.整个项目也是从整体到细节理了一遍了,不说对项目百分百吃透,但对项目结构、原理、思路、技术都有了一定程度的理解，相信这还是会对以后的开发产生一定的帮助.   
拖了这么久的项目，得好好写一波了。  