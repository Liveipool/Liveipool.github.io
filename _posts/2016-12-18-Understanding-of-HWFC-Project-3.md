---
layout: post
title: HWFC项目包理解（三）-- 三个 webpack.conf.js 文件   
date: 2016-12-18
categories: blog
tags: [前端]
description: HWFC项目包理解
---

# 三个 webpack.conf.js 文件      

> Webpack在执行的时候，除了在命令行传入参数，还可以通过指定的配置文件来执行。默认情况下，会搜索当前目录的webpack.config.js文件，这个文件是一个 node.js 模块，返回一个 json 格式的配置信息对象，或者通过 --config 选项来指定配置文件。     
> 这个文件非常重要，在HWFC项目包中，有一个基础的webpack.base.conf.js文件，还有两个对应不同环境的文件,一个是"development"的webpack.dev.conf.js,一个是"production"的webpack.prod.conf.js,这两个文件通过webpack-merge插件合并webpack.base.conf.js,成为两种环境下利用webpack打包时的完整配置文件,这些文件里包含了整个打包流程的各种配置信息。     
> 下面,我们还是结合代码和注释来理解.     

## 各文链接     
 - [HWFC项目包理解（一）-- 从目录结构和package.json开始 ](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-1/)      
 - [HWFC项目理解（二）-- webpack-dev-middleware and webpack-hot-middleware](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-2/)        
 - [HWFC项目包理解（三）-- 三个 webpack.conf.js 文件](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-3/)   
 - [HWFC项目包理解（四）-- Utils 和 Config/](http://liveipool.com/blog/2016/12/18/Understanding-of-HWFC-Project-4/)               

## webpack.base.conf.js        

``` javascript
//webpack.base.conf.js
var path = require('path')
var config = require('../config')
var utils = require('./utils')
//这个projectRoot console出来正好是HWFC项目根目录，__dirname是返回当前工作环境的绝对路径
var projectRoot = path.resolve(__dirname, '../')
//process.env：可用来轻松访问shell环境下的变量。比如，一个最常见的环境变量就是NODE_ENV，该变量用来控制程序是运行在开发模式下还是产品模式下。
var env = process.env.NODE_ENV

//如果env是‘development’， useCssSourceMap = false || false
//如果env是‘production’， useCssSourceMap = false || true
//即production时才使用CssSourceMap
var cssSourceMapDev = (env === 'development' && config.dev.cssSourceMap)
var cssSourceMapProd = (env === 'production' && config.build.productionSourceMap)
var useCssSourceMap = cssSourceMapDev || cssSourceMapProd

module.exports = {
  //程序入口处，这里的entry传入的是一个对象，因此会创建多个输入包文件(虽然这个项目中是一个,但和传的是一个单文件有模式上的区别)。
  entry: {
    app: './src/main.js'
  },
  //输出的文件的各属性
  output: {
    //assetsRoot值为HWFC/dist/
    path: config.build.assetsRoot,
    //publicPath这个属性是用在开发时和运行时可能有些静态文件的url不同的情况，
    //如开发是通过‘./logo.png’来找这张图，而上线后是通过‘https://someCDN/logo.png’来引用这张图的，
    //这种时候就需要到时候url通过这个publicPath属性来生成对应的路径。
    //不过production和development两种情况下assetsPublicPath的值都是‘/’，这个以后具体体会到功能后再来看看。
    publicPath: process.env.NODE_ENV === 'production' ? config.build.assetsPublicPath : config.dev.assetsPublicPath,
    //当有多个入口点时，应该使用替换符来为每个文件命一个名字，这里的[name]会被chunk的名字替换掉。
    //注意: 这个output并没有生成dist文件夹中的js文件,这个[name].js文件是生成到什么地方去的???
    filename: '[name].js'
  },
  resolve: {
    //require时省略扩展名，如require('module')，不需要module.js。
    extensions: ['', '.js', '.vue', '.scss'],
    //webpack没有在resolve.root 或者 resolve.modulesDirectories找到的模块的一个目录（或者目录绝对路径的数组）。
    fallback: [path.join(__dirname, '../node_modules')],
    //别名，可以直接使用别名来代表设定的路径或其它。
    alias: {
      'src': path.resolve(__dirname, '../src'),
      'assets': path.resolve(__dirname, '../src/assets'),
      'components': path.resolve(__dirname, '../src/components')
    }
  },
  //module.loaders解析了他们应用相关的资源，这意味着他们不需要解析配置过的文件。
  //如果你用npm安装loaders，node_modules文件夹不在资源文件夹的父目录中，webpack就找不到这个loader。
  //你需要把node_modules文件夹的绝对路径添加到resolveLoader.fallback这个选项中。 
  //像 resolve 但是是对于loaders.
  resolveLoader: {
    fallback: [path.join(__dirname, '../node_modules')]
  },
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
      {
        //json-loader，解析json文件
        test: /\.json$/,
        loader: 'json'
      },
      {
        //图片打包需要url-loader，将10000字节以下的图片自动转化为base64的编码，大于这个大小的保存为单独的文件
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url',
        query: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        //woff2、eot、ttf、otf是一些字体，他们的打包参照上面图片的打包
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url',
        query: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  },
  vue: {
    //vue-loader会返回一堆loaders，用于处理.vue文件，这个utils文件中的cssLoaders()函数的具体作用在utils文件中来分析.
    loaders: utils.cssLoaders({ sourceMap: useCssSourceMap }),
    //自动添加前缀
    postcss: [
      require('autoprefixer')({
        browsers: ['last 2 versions']
      })
    ]
  }
}
```    


**其中有一些要素值得单独拿出来一说:**:     

### entry, output       
![entry.png](http://upload-images.jianshu.io/upload_images/3001083-065a081a6fbe5ea3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![output1.png](http://upload-images.jianshu.io/upload_images/3001083-ebe32d873c13caad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![output2.png](http://upload-images.jianshu.io/upload_images/3001083-167c3819616b2c08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![output3.png](http://upload-images.jianshu.io/upload_images/3001083-b2352d1d0eaa5816.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![output4.png](http://upload-images.jianshu.io/upload_images/3001083-964ae6666cafc447.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### module     
![moduleloaders.png](http://upload-images.jianshu.io/upload_images/3001083-7e1edc30814dbc6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### url-loader     
加载图片需要url-loader，有些图片会作为一个单独的文件，但有些会被转为base64,这个是通过loader中的limit属性来设置的，通过limit属性设置的大小，如果文件比这个数值小，就会被转为base64,如果比这个数值大，就会通过一个单独的文件来保存。     
比如我现在设置的limit为10000,即10000字节，而我现在的两张图片，一张拉拉纳的，27350字节，大于10000。另一张是Vue的logo，只有6849字节，因此，根据原理，logo那张图会被转为base64,拉拉纳的这张图会作为一个单独的文件保存着。     

![base64 1.png](http://upload-images.jianshu.io/upload_images/3001083-bfd0be961f00268f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![base64 2.png](http://upload-images.jianshu.io/upload_images/3001083-66897f0a91277b8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

可以看到，经过打包后的dist文件夹里，只有拉拉纳的这张图，而怎么判断logo那张图是被转为base64了呢？     
![base64 4.png](http://upload-images.jianshu.io/upload_images/3001083-03c88dc29854227d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

从打包后的一个js文件里面可以看到，拉拉纳那张图还是作为一个单独文件来用，而logo已经被转为base64了。     
![base 64 3.png](http://upload-images.jianshu.io/upload_images/3001083-ade3c08484bd7278.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### autoprefixer       

```
  vue: {
    loaders: utils.cssLoaders({ sourceMap: useCssSourceMap }),
    postcss: [
      require('autoprefixer')({
        browsers: ['last 2 versions']
      })
    ]
  }
```
在webpack.base.conf.js中，开始不懂其中的postcss是啥，后来发现它是和autoprefixer配合着用的，用来给css自动添加各种前缀，但我看了下package.json里，只有 autoprefixer没有postcss，后来试了一下，发现安装autoprefixer的时候会自动安装postcss：       
![autoprefixer1.png](http://upload-images.jianshu.io/upload_images/3001083-7bca94d281aea9ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

下面是autoprefixer的github上的写法和功能介绍，比较清晰：        
在webpack中的用法：     
![autoprefixer2.png](http://upload-images.jianshu.io/upload_images/3001083-a0f09223e5c930b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
功能：     
![autoprefixer3.png](http://upload-images.jianshu.io/upload_images/3001083-e17708b8ab82cdc6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## webpack.dev.conf.js       

``` javascript
// webpack.dev.conf.js 
var config = require('../config')
var webpack = require('webpack')
//见下文webpack-merge的详细介绍
var merge = require('webpack-merge')
var utils = require('./utils')
//把webpack.base.conf.js中的设置拿过来准备合并,是一个对象
var baseWebpackConfig = require('./webpack.base.conf')
//用来生成index.html文件,并自动注入js的依赖到dist/index.html中body的尾部.
var HtmlWebpackPlugin = require('html-webpack-plugin')

// add hot-reload related code to entry chunks
Object.keys(baseWebpackConfig.entry).forEach(function (name) {
  baseWebpackConfig.entry[name] = ['./build/dev-client'].concat(baseWebpackConfig.entry[name])
})

module.exports = merge(baseWebpackConfig, {
  module: {
    loaders: utils.styleLoaders({ sourceMap: config.dev.cssSourceMap })
  },

  //devtool：选择开发工具来提高debug效率.
  //eval-source-map is faster for development
  //eval-source-map就是把 eval 的 sourceURL 换成了完整 souremap 信息的 DataUrl
  devtool: '#eval-source-map',
  plugins: [
    //The DefinePlugin allows you to create global constants which can be configured at compile time. 
    //This can be very useful for allowing different behaviour between development builds and release builds. 
    //For example, you might use a global constant to determine whether logging takes place; 
    //perhaps you perform logging in your development build but not in the release build. 
    new webpack.DefinePlugin({
      'process.env': config.dev.env
    }),
    //Assign the module and chunk ids by occurrence count. Ids that are used often get lower (shorter) ids. 
    //This make ids predictable, reduces total file size and is recommended.
    //简短地介绍一下OccurrenceOrderPlugin部份，您应该知道webapck会给编译好的程式码片段一个id以用来辨别。
    //透过这个plugin 可以让webpack 在id 的分派上优化并保持一致性。
    new webpack.optimize.OccurenceOrderPlugin(),
    //热替换
    new webpack.HotModuleReplacementPlugin(),
    //用来跳过编译时出错的代码并记录，使编译后运行时的包不会发生错误
    new webpack.NoErrorsPlugin(),
    
    //在dev中,并未创建一个新的index.html文件,因为根目录下已经有一个index.html了,从运行后根目录下的index.html中没有自动inject就可以看出.
    //这里用这个插件是为了在使用webpack打包后的文件里能有一个index.html入口文件
    //这些打包后的文件再被webpack-dev-middleware装为一个bundle用来传递到Express服务器端,其中必须要有那个index.html入口文件
    //可以试着将下面这个插件注释掉,注释掉之后运行再打开8080端口就显示不出页面了/
    new HtmlWebpackPlugin({
      //filename: The file to write the HTML to. Defaults to index.html. 
      //You can specify a subdirectory here too (eg: assets/admin.html).
      filename: 'index.html',
      //Webpack require path to the template. 使用模板引擎时才用
      template: 'index.html',
      //inject: true | 'head' | 'body' | false Inject all assets into the given template or templateContent
      //When passing true or 'body' all javascript resources will be placed at the bottom of the body element. 
      //'head' will place the scripts in the head element.
      //这里即用来自动注入js依赖到index.html中body的尾部.
      inject: true
    })
  ]
})

```

### webpack-merge        
![webpack-merge1.png](http://upload-images.jianshu.io/upload_images/3001083-99d1fa2278c4572d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

![webpack-merge2.png](http://upload-images.jianshu.io/upload_images/3001083-b9e37524a6c02458.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)          

### html-webpack-plugin       
This is a webpack plugin that simplifies creation of HTML files to serve your webpack bundles. This is especially useful for webpack bundles that include a hash in the filename which changes every compilation. You can either let the plugin generate an HTML file for you, supply your own template using lodash templates or use your own loader.         

![html-webpack-plugin1.png](http://upload-images.jianshu.io/upload_images/3001083-81b5b067666d2570.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

虽然是ugly的,但还是可以看出所有js的依赖都自动注入到了index.html文件中     
![html-webpack-plugin2.png](http://upload-images.jianshu.io/upload_images/3001083-0297608985ff4631.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### devtool     
![devtool.png](http://upload-images.jianshu.io/upload_images/3001083-ae40a419712b7870.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## webpack.prod.conf.js       

``` javascript
// webpack.prod.conf.js
var path = require('path')
var config = require('../config')
var utils = require('./utils')
var webpack = require('webpack')
var merge = require('webpack-merge')
var baseWebpackConfig = require('./webpack.base.conf')
// 这个ExtractTextPlugin插件的详细功能参见博客后面的介绍.
var ExtractTextPlugin = require('extract-text-webpack-plugin')
var HtmlWebpackPlugin = require('html-webpack-plugin')
var env = config.build.env

var webpackConfig = merge(baseWebpackConfig, {
  module: {
    loaders: utils.styleLoaders了({ sourceMap: config.build.productionSourceMap, extract: true })
  },

  //source-map 这是最原始的 source-map 实现方式，其实现是打包代码同时创建一个新的 sourcemap 文件
  //并在打包文件的末尾添加 //# sourceURL 注释行告诉 JS 引擎文件在哪儿
  devtool: config.build.productionSourceMap ? '#source-map' : false,
  output: {
    //path值为HWFC/dist
    path: config.build.assetsRoot,
    //这里的[name]会被chunk的名字替换掉，[chunkhash]会被chunk的hash替换掉
    //filename的值经过函数运行后是如static/js/[name].[chunkhash].js之类的路径
    //path + filename 即为文件的最终的完整路径.
    filename: utils.assetsPath('js/[name].[chunkhash].js'),
    //非入口的chunk的文件名，作为一个相对路径放到output.path里。
    //[id]被chunk的id替换掉
    chunkFilename: utils.assetsPath('js/[id].[chunkhash].js')
  },
  vue: {
    loaders: utils.cssLoaders({
      sourceMap: config.build.productionSourceMap,
      extract: true
    })
  },
  plugins: [

    new webpack.DefinePlugin({
      'process.env': env
    }),
    // 压缩代码
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      }
    }),
    new webpack.optimize.OccurrenceOrderPlugin(),

    //[contenthash]: a hash of the content of the extracted file
    //这个utils.assetsPath('css/[name].[contenthash].css')的返回值为一个字符串:'static/css/[name].[contenthash].css'
    //将所有提取出来的样式代码放到static/css/[name].[contenthash].css这个文件中
    new ExtractTextPlugin(utils.assetsPath('css/[name].[contenthash].css')),
    
    // 创建dist/index.html文件
    // 注意index.html文件是在这里通过这个插件才生成的,output生成的只有部分js文件
    // 注意webpack.dev.conf.js和webpack.prod.conf.js中这两个插件使用后的区别.
    // webpack.dev.conf.js中并没有在项目中创建一个新的index.html文件,
    // 只是在使用webpack打包后的文件里生成一个index.html入口文件
    // 这些打包后的文件再被webpack-dev-middleware装为一个bundle用来传递到Express服务器端,其中必须要有那个index.html入口文件.
    // 而webpack.prod.conf.js中只是在项目的HWFC/dist目录下生成了一个index.html文件,
    // 它暂时不用和服务器端代码连接,只是先生成好,以后连接到服务器端代码后作为项目的入口启动文件.
    new HtmlWebpackPlugin({
      //filename值为dist/index.html
      filename: config.build.index,
      template: 'index.html',
      inject: true,
      //Pass a html-minifier options object to minify the output.
      minify: {
        //消除掉html中标签外的注释
        //如:<!-- some comment --><p>blah</p> minify后就是: <p>blah</p>
        //如:<script type="text/javascript"><!-- some comment --></script> minify后保持原状
        removeComments: true,
        //消除掉标签中的空格,不会消除标签里的.
        //如:<a>lala</a>  <p> la la la </la>,minify后为:<a>lala</a><p> la la la </la>
        collapseWhitespace: true,
        //在某些可以的情况下,去除掉不必要的引号
        //如:<p class="foo-bar" id="moo" title="blah blah">foo</p>
        //minify后结果为:<p class=foo-bar id=moo title="blah blah">foo</p>
        removeAttributeQuotes: true
      },
      // Allows to control how chunks should be sorted before they are included to the html. 
      chunksSortMode: 'dependency'
    }),
    
    // split vendor js into its own file
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
      // 设置一个依赖被引用超过多少次就提取出来
      minChunks: function (module, count) {
        // any required modules inside node_modules are extracted to vendor
        return (
          module.resource &&
          /\.js$/.test(module.resource) &&
          module.resource.indexOf(
            path.join(__dirname, '../node_modules')
          ) === 0
        )
      }
    }),
    // extract webpack runtime and module manifest to its own file in order to
    // prevent vendor hash from being updated whenever app bundle is updated
    new webpack.optimize.CommonsChunkPlugin({
      name: 'manifest',
      chunks: ['vendor']
    })
  ]
})

//一个压缩文件的插件,现在值为false,暂时未用到.可在config/index.js文件中进行设置.
if (config.build.productionGzip) {
  var CompressionWebpackPlugin = require('compression-webpack-plugin')

  webpackConfig.plugins.push(
    new CompressionWebpackPlugin({
      asset: '[path].gz[query]',
      algorithm: 'gzip',
      test: new RegExp(
        '\\.(' +
        config.build.productionGzipExtensions.join('|') +
        ')$'
      ),
      threshold: 10240,
      minRatio: 0.8
    })
  )
}

module.exports = webpackConfig

```


![dist目录.png](http://upload-images.jianshu.io/upload_images/3001083-e573fe703cd527e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)          

### 从dist文件夹的结构来看:         
 - index.html是webpack.prod.conf.js中的 HtmlWebpackPlugin部分生成的.     
 - index.html中的所有css的依赖是通过extract-text-webpack-plugin插件注入的,所有的的js依赖是通过html-webpack-plugin注入的.     
 - app.×××.js是webpack.prod.conf.js中的output里配置产生的:filename: utils.assetsPath('js/[name].[chunkhash].js').     
 - app.×××.css是webpack.prod.conf.js中的new ExtractTextPlugin(utils.assetsPath('css/[name].[contenthash].css'))生成的.     
 - manifest和vendor是通过webpack.prod.conf.js中的CommonsChunkPlugin产生的.     
 - 所有的map文件是sourceMap的产物.     
     
### extract-text-webpack-plugin       
如果一开始我们的样式就被动态地注入到页面里，导致页面加载完成前，样式会闪烁。因此,我们最好将所有的样式代码,打包到一个单独的css文件里.用一句话概括,这个插件的功能就是将样式抽取出来作为独立的css文件,并通过< link >添加到html代码中.       
看一个简单的配置例子:       

```
var ExtractTextPlugin = require("extract-text-webpack-plugin");
module.exports = {
    // ...
    module: {
        loaders: [
            {test: /\.css$/, loader: ExtractTextPlugin.extract("style-loader", "css-loader")},
            {test: /\.(jpg|png|svg)$/, loader: "url?limit=8192"},
        ]
    },
    plugins: [
        new ExtractTextPlugin("[name].css")
    ]
}
```

ExtractTextPlugin的extract方法有两个参数，第一个参数是经过编译后通过style-loader单独提取出文件来，而第二个参数就是用来编译代码的loader.style-loader和css-loader在这个插件中总是结合着使用的,只需记住,它们共同的作用就是将所有样式代码提取出来并放到一个单独的css文件里.     

总的来说,一遍看下来的感觉是,webpack确实非常的专业,它有很多很棒的特点,它的模块化、打包都做得确实非常的不错,因此使用它的人很多,文档、插件也非常的完善.         
根据上文中的几个代码文件加上注释,相信也能将三个文件各部分基本掌握了,但其中我们可以看到经常有用到两个require进来的模块: var config = require('../config') 和 var utils = require('./utils'),这两个模块具体包含了些什么内容呢,我将在下一篇博客中写道.     