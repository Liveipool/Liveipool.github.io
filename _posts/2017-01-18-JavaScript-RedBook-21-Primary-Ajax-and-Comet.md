---
layout: post
title: JavaScript红皮书学习(21)--原生 Ajax 和 Comet
date: 2017-01-18
categories: blog
description: JavaScript学习
---


# 原生 Ajax 和 Comet    
 
## 21.1 XMLHttpRequest对象     
IE7+, Firefox, Opera, Chrome和Safari都支持原生的XHR对象,在这些浏览器中创建XHR对象要像下面这样使用XMLHttpRequest构造函数.     

``` javascript
var xhr = new XMLHttpRequest();
```

### 21.1.1 XHR的用法     
**在使用XHR时,要调用的第一个方法是open(),它接收3个参数:1.要发送的请求的类型(get, post等). 2.请求的URL. 3.表示是否异步发送的布尔值. 如:**     

``` javascript
xhr.open("get", "/get", false);
```

需要注意的两点是:**1. URL是相对于执行代码的当前页面(当然也可以使用绝对路径). 2.调用open方法并不会真正发送请求,只是启动一个请求以备发送.**     
因为现在同步发送已被弃用了,所以最好都用异步发送请求:     

``` javascript
		var date1 = +new Date();
		console.log("date1: ", date1);
		var xhr = new XMLHttpRequest();
		xhr.open("get", "/get", true);
		xhr.send(null);
		xhr.addEventListener("readystatechange", function() {
			if (xhr.readyState == 0) {
				console.log("0 未初始化");
			} else if (xhr.readyState == 1) {
				console.log("1 已启动,还未调用send");
			} else if (xhr.readyState == 2) {
				console.log("2 已调用send,还未接收到响应");
			} else if (xhr.readyState == 3) {
				console.log("3 已接受到部分响应数据");
			} else if (xhr.readyState == 4) {
				console.log("4 已接受到所有响应数据");
				console.log("xhr.status: ", xhr.status);
				console.log("xhr.statusText: ", xhr.statusText);
				console.log("xhr.responseText: ", xhr.responseText);
				var date2 = +new Date();
				console.log("date1: ", date2);
			}
		});
```

这里的send方法必须要接收一个参数,即要作为请求主体发送的数据.如果不需要通过请求主体发送数据,则要传入null.调用send()之后,请求就会被分派到服务器.     
在收到响应后,响应的数据会自动填充XHR对象的属性:     
1. responseText: 作为响应主体被返回的文本.     
2. status: 响应的HTTP状态.     
3. statusText: HTTP状态的说明.     

**在接收到响应后,第一步是检查status属性,以确定响应已经成功返回.一般来说,可以将HTTP状态码为200作为成功的标志.此时,responseText属性的内容已经就绪.状态代码为307表示请求的资源并没有被修改,可以直接使用浏览器中缓存的版本,当然,也意味着响应是有效的.**     
上面那段代码的运行截图:     
![21.1.13.png](http://upload-images.jianshu.io/upload_images/3001083-f349257bbfac1bf9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

在异步发送请求是,我们可以根据readyState属性来监测当前活动阶段,这个属性的取值如下:
1. "0": 未初始化,尚未调用open()方法.     
2. "1": 启动.已调用open()方法,但尚未调用send()方法.     
3. "2": 发送.已调用send()方法,尚未接收到响应.     
4. "3": 接收到部分响应数据.     
5. "4": 已经接收到全部响应数据,而且已经可以在客户端使用了.     

**readyState的属性值每次变化时,都会触发一次readystatechange事件.**     

另外,在接收到响应前还可以调用xhr.abort()方法来取消异步请求.调用这个方法后,XHR对象就会停止触发事件,而且也不再允许访问任何与响应有关的对象属性.在终止请求之后,还应该对XHR对象进行解引用的操作,即xhr = null;     

### 21.1.2 HTTP头部信息     
![21.1.2.png](http://upload-images.jianshu.io/upload_images/3001083-0e47a155ef33ff85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

``` javascript
		xhr.open("get", "/get", true);
		xhr.setRequestHeader("MyHeader", "MyValue");
		xhr.send(null);
```

调用XHR对象的getResponseHeader()方法并传入头部字段名称,可以取得相应的响应头部信息.而调用getAllResponseHeaders()可以取得一个包含所有头部信息的长字符串.     

### 21.1.3 GET请求     
![21.1.3.png](http://upload-images.jianshu.io/upload_images/3001083-a62598153546d2a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

``` javascript
var xhr = new XMLHttpRequest();
xhr.open("get", "/get", true);
function addURLParam(url, name, value) {
	url += (url.indexOf("?") == -1 ? "?" : "&");
	url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
	return url
};
var url = "/get";
url = addURLParam(url, "name", "Nick");
url = addURLParam(url, "year", 27);
console.log(url); ///get?name=Nick&year=27
```

### 21.1.4 POST请求     
![21.1.4.png](http://upload-images.jianshu.io/upload_images/3001083-5956e0e2184dbbbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

```
var xhr = new XMLHttpRequest();
xhr.open("post", "/post", true);
```
接下来,向send()方法中传入某些数据.     

## 21.2 XMLHttpRequest 2级     
![21.2.png](http://upload-images.jianshu.io/upload_images/3001083-9b61bc62ee5e49cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.2.1 FormData     
![21.2.1.png](http://upload-images.jianshu.io/upload_images/3001083-688591ec00f8bae8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

[MDN FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)     

### 21.2.2 超时设定     
![21.2.21.png](http://upload-images.jianshu.io/upload_images/3001083-dfec2e1f82b1e2d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

``` javascript
		xhr.open("get", "/post", true);
		xhr.timeout = 1;
		xhr.addEventListener("timeout", function() {
			console.log("timeout!");
		});
		xhr.send(null);
```


## 21.3 进度事件     
![21.3.png](http://upload-images.jianshu.io/upload_images/3001083-b7cd9c4210896de1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

**注意,就现在来说,上面的一些事件还不是所以浏览器都完全支持的,使用时要先进行能力检测.**     

### 21.3.1 load事件     
可用来代替readystatechange事件.     

### 21.3.2 progress事件     
![21.3.21.png](http://upload-images.jianshu.io/upload_images/3001083-f4f0d0cafed563f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.3.22.png](http://upload-images.jianshu.io/upload_images/3001083-d6f98810a2d25d75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 21.4 跨源资源共享(CORS)  
当一个资源请求一个其它域名的资源时会发起一个跨域HTTP请求(cross-origin HTTP request)。比如说请求另外一个域名下的某张图片。          
但是这样的跨源请求会有安全问题，所以浏览器都会将跨源请求发回的资源进行拦截。            
但为了使Web应用能更强大，W3C推荐了一种新的机制，即跨源资源共享（Cross-Origin Resource Sharing (CORS)）。这种机制让Web应用服务器能支持跨站访问控制，从而可以安全地进行跨站数据传输。               
CORS背后的基本思想是，使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。         
![21.4.png](http://upload-images.jianshu.io/upload_images/3001083-11bb98ae4bf746d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.4.1 IE对 CORS 的实现     
![21.4.11.png](http://upload-images.jianshu.io/upload_images/3001083-66a657d0b1888438.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.4.12.png](http://upload-images.jianshu.io/upload_images/3001083-475e3ec410387760.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.4.13.png](http://upload-images.jianshu.io/upload_images/3001083-3977cf6236eeddab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.4.14.png](http://upload-images.jianshu.io/upload_images/3001083-7aa1421f39d92546.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.4.2 其他浏览器对 CORS 的实现     
![21.4.2.png](http://upload-images.jianshu.io/upload_images/3001083-55bdbac29bff9ecc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.4.3 Preflighted Reqeusts     
![21.4.31.png](http://upload-images.jianshu.io/upload_images/3001083-a5c5e03494841ec0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.4.32.png](http://upload-images.jianshu.io/upload_images/3001083-c2aacb47c2e73f7f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.4.4 带凭据的请求     
![21.4.4.png](http://upload-images.jianshu.io/upload_images/3001083-0008b9f8254718f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.4.5 跨浏览器的CORS     
![21.4.51.png](http://upload-images.jianshu.io/upload_images/3001083-f18094aae27b6cca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.4.52.png](http://upload-images.jianshu.io/upload_images/3001083-2d60f1df9a89fee3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 21.5 其他跨域技术     
![21.5.png](http://upload-images.jianshu.io/upload_images/3001083-f91b0fa092965cd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.5.1 图像Ping     
![21.5.1.png](http://upload-images.jianshu.io/upload_images/3001083-b3976d10da69fdde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.5.2 JSONP     
![21.5.21.png](http://upload-images.jianshu.io/upload_images/3001083-e84d736089d9bb46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.5.22.png](http://upload-images.jianshu.io/upload_images/3001083-7eb7c4bb8978a200.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.5.23.png](http://upload-images.jianshu.io/upload_images/3001083-ace44095f57c841d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.5.3 Comet     
 - **Ajax是一种从页面向服务器请求数据的技术.**     
 - **Comet是一种服务器向页面推送数据的技术.**     
 - Comet能够让信息近乎实时地被推送到页面上,非常适合处理体育比赛的分数和股票价格等.     

![21.5.31.png](http://upload-images.jianshu.io/upload_images/3001083-915124df7b9a3423.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.5.32.png](http://upload-images.jianshu.io/upload_images/3001083-56886a0b4f82bb64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

**第二种流行的Comet实现是HTTP流.流不同于上述两种轮询,因为它在页面的整个生命周期内只使用一个HTTP连接.具体来说,就是浏览器向服务器发送一个请求,而服务器保持连接打开,然后周期性地向浏览器发送数据.**     

### 21.5.4 服务器发送事件     
![21.5.4.png](http://upload-images.jianshu.io/upload_images/3001083-44d3a65ae138a18c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.5.5 Web Socket  
WebSocket protocol 是HTML5一种新的协议。它实现了浏览器与服务器全双工通信(full-duple)。一开始的握手需要借助HTTP请求完成。      
在浏览器中通过http仅能实现单向的通信,comet可以一定程度上模拟双向通信,但效率较低.HTML5定义了WebSocket协议，能更好的节省服务器资源和带宽并达到实时通讯。在支持HTML5的浏览器上 ，可以使用WebSocket。但在如IE8等不完全支持HTML5的浏览器上，还是使用comet来模拟。     
现很多网站为了实现即时通讯，所用的技术都是轮询(polling)。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP request，然后由服务器返回最新的数据给客户端的浏览器。这种传统的HTTP request 的模式带来很明显的缺点 – 浏览器需要不断的向服务器发出请求，然而HTTP request 的header是非常长的，里面包含的有用数据可能只是一个很小的值，这样会占用很多的带宽。           
而使用Comet和Ajax来模拟双向通信，虽然可达到全双工通信，但依然需要发出请求。      
在 WebSocket API，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。在此WebSocket 协议中，为我们实现即时服务带来了两大好处：         
1. Header：互相沟通的Header是很小的-大概只有 2 Bytes           
2. Server Push：服务器的推送，服务器不再被动的接收到浏览器的request之后才返回数据，而是在有新数据时就主动推送给浏览器。     

也就是说，在Comet的长轮询实现中，浏览器需要不断发送请求，Comet的HTTP流实现中，虽然是服务器周期性的发送数据，浏览器不需要请求，但这两种方式每次发送都还是有很大的HTTP头部文件，WebSocket既可以使浏览器不需要一直请求，也能在有数据时及时发送给浏览器，并且每次发送的数据只有很小的头部。      

Socket.io对WebSocket进行了封装的，既可写在客户端，又可写在服务端的用于实时通信、跨平台的开源框架。它除了支持WebSocket通讯协议外，还支持许多种轮询（Polling）机制以及其它实时通信方式，并封装成了通用的接口，并且在服务端实现了这些实时机制的相应代码。       
下面有两篇文章，讲述了Websocket和Socket.io：[http://www.cnblogs.com/xiezhengcai/p/3957314.html](http://www.cnblogs.com/xiezhengcai/p/3957314.html)、
[http://blog.csdn.net/csdnhaoren13/article/details/51034565](http://blog.csdn.net/csdnhaoren13/article/details/51034565)                   
![21.5.51.png](http://upload-images.jianshu.io/upload_images/3001083-f1819eaf9cc6f8f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
**安全性的问题.**     
![21.5.52.png](http://upload-images.jianshu.io/upload_images/3001083-c1c3094a930e4b3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.5.53.png](http://upload-images.jianshu.io/upload_images/3001083-3a1e7b2fd786b693.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.5.54.png](http://upload-images.jianshu.io/upload_images/3001083-11e43aa63d1f885d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.5.55.png](http://upload-images.jianshu.io/upload_images/3001083-8caef6a1c4934b3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.5.56.png](http://upload-images.jianshu.io/upload_images/3001083-ed515b596da640e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

### 21.5.6 SSE 与 Web Sockets     
![21.5.6.png](http://upload-images.jianshu.io/upload_images/3001083-f655ee274eb8f09d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 21.6 安全     
![21.61.png](http://upload-images.jianshu.io/upload_images/3001083-8ab1ab9a10e64d21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.62.png](http://upload-images.jianshu.io/upload_images/3001083-7efeba7588f3d212.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

## 21.7 小结     
![21.71.png](http://upload-images.jianshu.io/upload_images/3001083-5846c50bf21403d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
![21.72.png](http://upload-images.jianshu.io/upload_images/3001083-95ecbb8e51e038f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     


### 相关文章链接    
 - [Javascript深入学习1——词法结构、类型、值、变量](http://liveipool.com/blog/2016/09/12/learn-javascript-1/)       
 - [JavaScript红皮书学习(1,2)--JavaScript简介](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-1,2-Introduction/)  
 - [JavaScript红皮书学习(3)--基本概念](http://liveipool.com/blog/2016/12/14/JavaScript-RedBook-3-BasicConcepts/)   
 - [JavaScript红皮书学习(4)--变量、作用域和内存问题](http://liveipool.com/blog/2016/12/19/JavaScript-RedBook-4-Variable-Scope-and-Memory/)    
 - [JavaScript红皮书学习(5)--引用类型](http://liveipool.com/blog/2016/12/22/JavaScript-RedBook-5-Reference-Type)     
 - [JavaScript学习专题之--作用域](http://liveipool.com/blog/2016/12/22/JavaScript-Scope)   
 - [JavaScript学习专题之--闭包](http://liveipool.com/blog/2016/12/23/JavaScript-Closures)     
 - [JavaScript学习专题之--私有变量和ES5中的模块模式](http://liveipool.com/blog/2016/12/24/JavaScript-Private-Variable-and-ES5Modules)      
 - [JavaScript学习专题之--this](http://liveipool.com/blog/2016/12/25/JavaScript-this)       
 - [JavaScript红皮书学习(8)--BOM](http://liveipool.com/blog/2016/12/25/JavaScript-RedBook-8-BOM)             
 - [JavaScript红皮书学习(6)--面向对象的程序设计](http://liveipool.com/blog/2016/12/27/JavaScript-RedBook-6-Object-Oriented)                  
 - [JavaScript红皮书学习(10)--DOM1](http://liveipool.com/blog/2016/12/31/JavaScript-RedBook-10-DOM1)                  
 - [JavaScript红皮书学习(11)--DOM-Extensions](http://liveipool.com/blog/2016/12/31/JavaScript-RedBook-11-DOM-Extensions)                  
 - [JavaScript红皮书学习(12)--DOM2-And-DOM3](http://liveipool.com/blog/2016/12/31/JavaScript-RedBook-12-DOM2-And-DOM3)                  
 - [JavaScript红皮书学习(13)--事件](http://liveipool.com/blog/2017/01/13/JavaScript-RedBook-13-Event)                  
 - [JavaScript红皮书学习(14)--表单脚本](http://liveipool.com/blog/2017/01/13/JavaScript-RedBook-14-Form)                
 - [JavaScript红皮书学习(15)--使用Canvas绘图](http://liveipool.com/blog/2017/01/14/JavaScript-RedBook-15-Canvas)      
 - [JavaScript红皮书学习(16)--HTML5脚本编程](http://liveipool.com/blog/2017/01/14/JavaScript-RedBook-16-HTML5-Scripts-Programming)           
 - [JavaScript红皮书学习(20)--JSON](http://liveipool.com/blog/2017/01/17/JavaScript-RedBook-20-JSON)      
 - [JavaScript红皮书学习(21)--Ajax](http://liveipool.com/blog/2017/01/18/JavaScript-RedBook-21-Ajax)      
 - [JavaScript红皮书学习(21)--原生Ajax和Comet](http://liveipool.com/blog/2017/01/18/JavaScript-RedBook-21-Primary-Ajax-and-Comet)      
 - [JavaScript红皮书学习(21)--jQuery中的Ajax](http://liveipool.com/blog/2017/01/19/JavaScript-RedBook-21-Ajax-in-jQuery)      
