---
layout: post
title: HTML 杂记
date: 2017-02-02
categories: blog
description: HTML学习
---

# HTML 杂记       
今天把w3school上关于HTML的内容浏览了一遍，以下是一些以前印象不太深刻的点。          

### 什么是HTML        
 - HTML 是用来描述网页的一种语言。        
 - HTML 指的是超文本标记语言 (Hyper Text Markup Language)。        
 - HTML 不是一种编程语言，而是一种标记语言 (markup language)。        
 - 标记语言是一套标记标签 (markup tag)。        
 - HTML 使用标记标签来描述网页。        

**注释：开始标签常被称为开放标签（opening tag），结束标签常称为闭合标签（closing tag）。**        

### HTML文档 = 网页        
 - HTML 文档描述网页        
 - HTML 文档包含 HTML 标签和纯文本        
 - HTML 文档也被称为网页        
 - Web 浏览器的作用是读取 HTML 文档，并以网页的形式显示出它们。浏览器不会显示 HTML 标签，而是使用标签来解释页面的内容。        

### HTML5 浏览器支持     
所有现代浏览器都支持 HTML5。     
此外，所有浏览器，不论新旧，都会自动把未识别元素当做行内元素来处理。     
HTML5 定义了八个新的语义 HTML 元素。所有都是块级元素。     
您可以把 CSS display 属性设置为 block，以确保老式浏览器中正确的行为：     

```
header, section, footer, aside, nav, main, article, figure {
    display: block; 
}
```

### 替换文本属性（Alt）        
alt 属性用来为图像定义一串预备的可替换的文本。替换文本属性的值是用户定义的。        

```
<img src="boat.gif" alt="Big Boat">
```
在浏览器无法载入图像时，替换文本属性告诉读者她们失去的信息。此时，浏览器将显示这个替代性的文本而不是图像。为页面上的图像都加上替换文本属性是个好习惯，这样有助于更好的显示信息，并且对于那些使用纯文本浏览器的人来说是非常有用的。        

### iframe        

```
<iframe src="URL"></iframe>
<iframe src="demo_iframe.htm" width="200" height="200"></iframe>
<iframe src="demo_iframe.htm" frameborder="0"></iframe>
```

使用 iframe 作为链接的目标，iframe 可用作链接的目标（target）。        
链接的 target 属性必须引用 iframe 的 name 属性：        

```
<iframe src="demo_iframe.htm" name="iframe_a"></iframe>
<p><a href="http://www.w3school.com.cn" target="iframe_a">W3School.com.cn</a></p>
```

### HTML 实体        
在 HTML 中，某些字符是预留的。        
在 HTML 中不能使用小于号（<）和大于号（>），这是因为浏览器会误认为它们是标签。        
如果希望正确地显示预留字符，我们必须在 HTML 源代码中使用字符实体（character entities）。        
字符实体类似这样：        

```
&entity_name;
// 或者
&#entity_number;
```

[HTML实体 w3school](http://www.w3school.com.cn/html/html_entities.asp)        

### 统一资源定位符 URL                 
 - URL 可以由单词组成，比如 “w3school.com.cn”，或者是因特网协议（IP）地址：192.168.1.253。大多数人在网上冲浪时，会键入网址的域名，因为名称比数字容易记忆。         
 - Uniform Resource Locator。        
 - 当您点击 HTML 页面中的某个链接时，对应的 < a > 标签指向万维网上的一个地址。        
 - 网址，比如 http://www.w3school.com.cn/html/index.asp，遵守以下的语法规则：        
 - scheme://host.domain:port/path/filename， 解释：        
 - scheme - 定义因特网服务的类型。最常见的类型是 http        
 - host - 定义域主机（http 的默认主机是 www）        
 - domain - 定义因特网域名，比如 w3school.com.cn        
 - :port - 定义主机上的端口号（http 的默认端口号是 80）        
 - path - 定义服务器上的路径（如果省略，则文档必须位于网站的根目录中）。        
 - filename - 定义文档/资源的名称        

以下是其中一些最流行的 scheme：        
1. http	超文本传输协议	以 http:// 开头的普通网页。不加密。        
2. https	安全超文本传输协议	安全网页。加密所有信息交换。        
3. ftp	文件传输协议	用于将文件下载或上传至网站。        
4. file	 	您计算机上的文件。        

### < !DOCTYPE > 声明        
Web 世界中存在许多不同的文档。只有了解文档的类型，浏览器才能正确地显示文档。        
HTML 也有多个不同的版本，只有完全明白页面中使用的确切 HTML 版本，浏览器才能完全正确地显示出 HTML 页面。这就是 < !DOCTYPE > 的用处。        
< !DOCTYPE > 不是 HTML 标签。它为浏览器提供一项信息（声明），即 HTML 是用什么版本编写的。        

### HTML5 < datalist > 元素        
 - < datalist>  元素为 < input > 元素规定预定义选项列表。        
 - 用户会在他们输入数据时看到预定义选项的下拉列表。        
 - < input > 元素的 list 属性必须引用 < datalist > 元素的 id 属性。        
 - 实例，通过 < datalist > 设置预定义值的 < input > 元素：        

```
<form action="action_page.php">
<input list="browsers">
<datalist id="browsers">
   <option value="Internet Explorer">
   <option value="Firefox">
   <option value="Chrome">
   <option value="Opera">
   <option value="Safari">
</datalist> 
</form>
```

### 更改为 HTML5 编码        
修改编码信息，从 HTML4：        

```
<meta http-equiv="Content-Type" content="text/html;charset=utf-8">
```
改为 HTML5：        

```
<meta charset="utf-8">
```

### HTML5 支持内联 SVG        
 - SVG 指可伸缩矢量图形 (Scalable Vector Graphics)        
 - SVG 用于定义用于网络的基于矢量的图形        
 - SVG 使用 XML 格式定义图形        
 - SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失        
 - SVG 是万维网联盟的标准        
 - SVG 的优势：        
 - 与其他图像格式相比（比如 JPEG 和 GIF），使用 SVG 的优势在于：        
 - SVG 图像可通过文本编辑器来创建和修改        
 - SVG 图像可被搜索、索引、脚本化或压缩        
 - SVG 是可伸缩的        
 - SVG 图像可在任何的分辨率下被高质量地打印        
 - SVG 可在图像质量不下降的情况下被放大        
 - 浏览器支持：Internet Explorer 9、Firefox、Opera、Chrome 以及 Safari 支持内联 SVG。        

在 HTML5 中，能够将 SVG 元素直接嵌入 HTML 页面中：        
实例：        

```
<!DOCTYPE html>
<html>
<body>

<svg xmlns="http://www.w3.org/2000/svg" version="1.1" height="190">
  <polygon points="100,10 40,180 190,60 10,60 160,180"
  style="fill:lime;stroke:purple;stroke-width:5;fill-rule:evenodd;" />
</svg>

</body>
</html>
```

### HTML 5 Canvas vs. SVG        
 - Canvas 和 SVG 都允许您在浏览器中创建图形，但是它们在根本上是不同的。        
 - SVG 是一种使用 XML 描述 2D 图形的语言。        
 - SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。        
 - 在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。        
 - Canvas 通过 JavaScript 来绘制 2D 图形。        
 - Canvas 是逐像素进行渲染的。        
 - 在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。        

下面列出了 canvas 与 SVG 之间的一些不同之处。        
Canvas：        
1. 依赖分辨率        
2. 不支持事件处理器        
3. 弱的文本渲染能力        
4. 能够以 .png 或 .jpg 格式保存结果图像        
5. 最适合图像密集型的游戏，其中的许多对象会被频繁重绘        

SVG：        
1. 不依赖分辨率        
2. 支持事件处理器        
3. 最适合带有大型渲染区域的应用程序（比如谷歌地图）        
4. 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）        
5. 不适合游戏应用        

### Web worker 是运行在后台的 JavaScript，不会影响页面的性能。        
当在 HTML 页面中执行脚本时，页面是不可响应的，直到脚本已完成。        
Web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 运行在后台。        

在创建 web worker 之前，请检测用户浏览器是否支持它：        

```
if (typeof(Worker) !== "undefined") {
    // 是的！支持 Web worker！
    // 一些代码.....
} else {
    // 抱歉！不支持 Web Worker！
}
```

Web Worker 和 DOM        
由于 web worker 位于外部文件中，它们无法访问下例 JavaScript 对象：        
1. window 对象        
2. document 对象        
3. parent 对象        

### HTML Server-Sent事件                
 - 简称SSE, IE不支持。        
 - Server-Sent 事件允许网页从服务器获得更新。        
 - Server-Sent 事件 - One Way Messaging        
 - Server-Sent 事件指的是网页自动从服务器获得更新。        
 - 以前也可能做到这一点，前提是网页不得不询问是否有可用的更新。通过 Server-Sent 事件，更新能够自动到达。        
 - 例如：Facebook/Twitter 更新、股价更新、新的博文、赛事结果，等等。        