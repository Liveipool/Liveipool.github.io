---
layout: post
title: 使用gulp搭建一个简单前端学习框架 
date: 2016-12-15
categories: blog
tags: [前端]
description: 前端自动构建工具--gulp学习笔记
---

# 使用gulp搭建一个简单前端学习框架        
最近使用gulp搭建了一个可以用来进行各种前端练习的简单框架，如果使用一些工具来自动生成较为完整的框架，感觉非常爽和方便，但实际上对于这个框架怎么搭起来的只是略知一二，就算是详细看了官方文档，但也不见得下次自己从0开始手动搭建时就能搭出来。这种东西，真的只有自己踩坑填坑之后才能真正将其弄懂。          
搭建完这个小框架的一个星期后，一直想写一篇blog来记录一下，可几次想提笔都觉得没啥好写的，但心里始终惦记着这事也不舒服，于是在这里来小小总结一下吧，能写多少是多少，毕竟都是自己的亲身体验。          

### 一、安装gulp到项目中          
首先，自然是安装gulp，gulp需要先进行一次全局安装，再在项目中添加进gulp。        
```
$ npm install --global gulp
$ npm install --save-dev gulp
```
接下来创建最为重要的gulpfile.js文件，gulp所有的配置信息都写在这里面：        
```
var gulp = require('gulp');

gulp.task('default', function() {
  // do something
});
```
至此，在命令行输入gulp命令就可以运行项目了，但现在自然是什么输出都没有。        

### 二、使用gulp-connect实现一个简单的服务器           
```
gulp.task('webserver', function() {
	connect.server({
		root: 'tmp',  
		livereload: true
	});
});
```
因为后面会说到，我将所有的文件进行编译或者打包后都放到一个tmp文件夹下，这个tmp文件夹就是完成构建后的完整代码包，可用于拿去和后台代码对接、上线等等。        

![studying-frame目录结构2.png](http://upload-images.jianshu.io/upload_images/3001083-9f0060808562f028.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

![studying-frame目录结构.png](http://upload-images.jianshu.io/upload_images/3001083-c65c46385c112d9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    

这个root属性不设置的话会默认在根目录下，此框架中，即为studying-frame目录下。          
而程序的入口，则默认为root设置的目录下的index.html文件，这个应该也可以更改，具体可查看gulp-connect的文档。            
至此，一个简单的服务器就实现了，只要我们在默认的目录下添加对应的index.html文件，就可以通过gulp命令启动并在浏览器中通过默认的8080端口来看这个index.html文件的效果了，因此，接下来我们需要使tmp目录下有我们需要的index.html文件。        

### 三、添加gulp-sass、gulp-jade等插件          
在这个项目中，内容部分使用jade这个模板引擎，样式部分的代码写在.css或.scss中，因此项目中需要能够处理jade和sass，而要满足这个要求，就要通过安装插件来实现了。而gulp的一个特点就是插件的管理很好，它通过严格的插件指南来确保插件如用户期望的那样简洁高质的工作。通过几条命令就能完成插件的安装了。        

### 四、将各文件预处理或打包到tmp文件夹中          
接下来就是将各文件打包到tmp文件夹中了，这一步较为浅显，看了代码应该就能理解：        
```
gulp.task('sass', function() {
	gulp.src('style.scss')
		.pipe(sass().on('error', sass.logError))
		.pipe(gulp.dest('styles'))
		.pipe(connect.reload());  //用来实现更改自动刷新
});

gulp.task('css', ['sass'], function() {   //['sass']指sass这个task完成之后再执行css这个task。
	gulp.src('styles/*.css')
		.pipe(gulp.dest('tmp/styles'))
		.pipe(connect.reload());
});

gulp.task('js', function() {
	gulp.src('scripts/*.js')
		.pipe(gulp.dest('tmp/scripts'))
		.pipe(connect.reload());
});

//static这一部分应该有更简单的写法，但要求不多所以暂时这么写也没问题
gulp.task('static', function() { 
	gulp.src(['static/pictures/*.jpg', 'static/pictures/*.JPG', 'static/pictures/*.png', 'static/pictures/*.PNG', 'static/pictures/*.gif', 'static/pictures/*.jpeg'])
		.pipe(gulp.dest('tmp/static/pictures'));
});

gulp.task('index', ['inject'], function() {
	gulp.src('*.jade')
		.pipe(jade())
		.pipe(gulp.dest('tmp'))
		.pipe(connect.reload());
});

gulp.task('jade', ['inject'], function() {
	gulp.src('layouts/*.jade')
		.pipe(jade())
		.pipe(gulp.dest('tmp/layouts'))
});
```

### 五、理解一下此项目中的一些文件的作用          
“一些文件”说的是根目录下的style.scss、index.jade、layout.jade文件。           
其中，style.scss是用sass来写一些全局的较为常用的样式代码，方便以后的编程：             
![stylescss.png](http://upload-images.jianshu.io/upload_images/3001083-65992cbee3ce15ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

layout.jade是专门用来装html文件中的所有css、js依赖的文件，这里使用gulp-inject这个插件，创建文件后运行gulp命令后就可以自动注入依赖而不用我们手动注入。        
```
doctype html
html
	head
		meta(charset="utf-8")
		title studying-frame
	//- inject:css
	link(rel="stylesheet", href="/styles/index.css")
	//- css文件会自动注入到inject：css和endinject之间
	//- endinject 

	body
		block content

	
	//- inject:js
	script(src="/scripts/index.js")
	//- js文件会自动注入到inject：js和endinject之间
	//- endinject
```

```
gulp.task('inject', ['sass', 'css', 'js'], function() {
	var target = gulp.src('./layout.jade');
	var sources = gulp.src(['./styles/*.css', './scripts/*.js'], {read: false});
	return target.pipe(inject(sources))
				.pipe(gulp.dest('./'));	
});
```
而单独用一个文件来装这些依赖是通过使用jade的模板继承，来一次写完依赖，其他jade文件通过block content继承这个jade文件就可以使用这些依赖，这样就不需要每个jade文件都去写一遍如头信息、css、js依赖等。（但这样也有个问题，就是所有的jade文件都会注入所有js、css的依赖，这样如果命名有冲突等会造成页面之间的冲突，这个问题在以后的开发中慢慢再想一下。）        
![layoutjade.png](http://upload-images.jianshu.io/upload_images/3001083-5a81551946d0596b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

index.jade：此文件在运行gulp命令后会被编译为index.html文件放在tmp文件夹下，作为项目的最初页面。        
![indexjade.png](http://upload-images.jianshu.io/upload_images/3001083-83e3aec872445238.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   

### 六、实现更改代码自动刷新页面  
这个功能是非常实用的，因为这样就不用更改代码后再手动去刷新页面查看更改。而要实现这个功能也不需要安装新的插件，gulp-connect中有一个livereload属性，我们将它设为true。然后通过一个“watch”  task，在检测到如js文件有改动时就执行“js”这个task，在task中，执行connect.reload（），就实现了自动刷新页面。        
```
gulp.task('webserver', function() {
	connect.server({
		root: 'tmp', 
		livereload: true
	});
});

gulp.task('js', function() {
	gulp.src('scripts/*.js')
		.pipe(gulp.dest('tmp/scripts'))
		.pipe(connect.reload());
});

gulp.task('watch', function() {
	gulp.watch('scripts/*.js', ['js']);
});
```

### 七、通过gulp clean命令删除生成的tmp包           
这个功能也较为简单，主要是通过一个del插件，安装之后再进行一点设置就OK了：        
```
gulp.task('clean', function() {
	return del(['tmp']);
});
```

### 完整的gulpfile.js文件代码和命令行运行截图        
最后，来看看搭建完成后的gulpfile.js文件和命令行运行的截图：        
```
//gulpfile.js
var gulp = require('gulp');
var connect = require('gulp-connect');
var inject = require('gulp-inject');
var del = require('del');  //用来删除文件的
var sass = require('gulp-sass');
var jade = require('gulp-jade');

gulp.task('webserver', function() {
	connect.server({
		root: 'tmp',  //程序入口处，自动为这个文件夹下的Index.html，具体为啥是这个文件可能是gulp默认的，应该可以改
		livereload: true
	});
});

gulp.task('clean', function() {
	return del(['tmp']);
});

gulp.task('sass', function() {
	gulp.src('style.scss')
		.pipe(sass().on('error', sass.logError))
		.pipe(gulp.dest('styles'))
		.pipe(connect.reload());  //用来实现更改自动刷新
});

gulp.task('css', ['sass'], function() {   //['sass']指sass这个task完成之后再执行css这个task。
	gulp.src('styles/*.css')
		.pipe(gulp.dest('tmp/styles'))
		.pipe(connect.reload());
});

gulp.task('js', function() {
	gulp.src('scripts/*.js')
		.pipe(gulp.dest('tmp/scripts'))
		.pipe(connect.reload());
});

//static这一部分应该有更简单的写法，但要求不多所以暂时这么写也没问题
gulp.task('static', function() {
	gulp.src(['static/pictures/*.jpg', 'static/pictures/*.JPG', 'static/pictures/*.png', 'static/pictures/*.PNG', 'static/pictures/*.gif', 'static/pictures/*.jpeg'])
		.pipe(gulp.dest('tmp/static/pictures'));
})

gulp.task('inject', ['sass', 'css', 'js'], function() {
	var target = gulp.src('./layout.jade');
	var sources = gulp.src(['./styles/*.css', './scripts/*.js'], {read: false});
	return target.pipe(inject(sources))
				.pipe(gulp.dest('./'));	
});

gulp.task('index', ['inject'], function() {
	gulp.src('*.jade')
		.pipe(jade())
		.pipe(gulp.dest('tmp'))
		.pipe(connect.reload());
});

gulp.task('jade', ['inject'], function() {
	gulp.src('layouts/*.jade')
		.pipe(jade())
		.pipe(gulp.dest('tmp/layouts'))
		.pipe(connect.reload());
})

gulp.task('watch', function() {
	// gulp.watch(['*.jade','layouts/*.jade'], ['index', 'jade']);
	gulp.watch('*.jade', ['index']);
	gulp.watch('layouts/*.jade', ['jade']);
	gulp.watch('styles/*.css', ['css']);
	gulp.watch('scripts/*.js', ['js']);
	gulp.watch('./style.scss', ['sass']);
});

//在命令行中执行gulp指令时，会执行一下命令，这个default必须要设置
gulp.task('default', ['webserver', 'clean', 'sass', 'css', 'js', 'static', 'inject', 'index', 'jade', 'watch']);
```

![gulpclean.png](http://upload-images.jianshu.io/upload_images/3001083-8f27d4c60650fc52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![gulp.png](http://upload-images.jianshu.io/upload_images/3001083-059429f8940bc7ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
可以看出，这里好多命令执行了几遍，可能是因为我在里面的一些依赖关系没有理清或者多个文件就会有多次inject等的情况，不过这个就需要看更多和gulp相关的文档、文章了，这里只是搭建了一个简单的框架，要想给一个大项目搭建框架，还需要学习得更加深入才行。            
总算是写完了这篇blog，内容不多，就一篇小小的总结，但也是踩过的坑了，以后再遇到相同的问题时，希望能够感觉更加得心印手吧，已是25：05分了，睡觉了～        



