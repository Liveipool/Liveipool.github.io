---
layout: post
title: CSS 杂记
date: 2017-01-28
categories: blog
description: CSS学习
---

# CSS 杂记     
刚把w3school上css部分的都又看了一遍（CSS3和参考手册还没看），以下为一些琐碎的点：     

#### background-position     
**background-position可通过关键字（center、left、top、right、bottom）、百分数值、长度值三种方式来进行设置。 **     
![4.11.png](http://upload-images.jianshu.io/upload_images/3001083-0435cab15538c955.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     

#### background     
![4.12.png](http://upload-images.jianshu.io/upload_images/3001083-2cc7294f08deec4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
background-color 不能继承，其默认值是 transparent。transparent 有“透明”之意。也就是说，如果一个元素没有指定背景色，那么背景就是透明的，这样其祖先元素的背景才能可见。     
background-image 也不能继承。事实上，所有背景属性都不能继承。     

#### RGBa     
RGBa是一种同时设置颜色和不透明度的机制。RGB代表红色，绿色和蓝色，a代表alpha透明度。     

#### RGB     
我们可以通过两种方法使用 RGB 值：     

```
p { color: rgb(255,0,0); }
p { color: rgb(100%,0%,0%); }
```
请注意，当使用 RGB 百分比时，即使当值为 0 时也要写百分比符号。但是在其他的情况下就不需要这么做了。比如说，当尺寸为 0 像素时，0 之后不需要使用 px 单位，因为 0 就是 0，无论单位是什么。     

#### 上下文选择器     
通过依据元素在其位置的上下文关系来定义样式，你可以使标记更加简洁。     
在 CSS1 中，通过这种方式来应用规则的选择器被称为上下文选择器 (contextual selectors)，这是由于它们依赖于上下文关系来应用或者避免某项规则。在 CSS2 中，它们称为派生选择器，但是无论你如何称呼它们，它们的作用都是相同的。     

#### 属性选择器     
对带有指定属性的 HTML 元素设置样式。     
可以为拥有指定属性的 HTML 元素设置样式，而不仅限于 class 和 id 属性。     
下面的例子为 title="W3School" 的所有元素设置样式：     

```
[title="W3School"]
{
border:5px solid blue;
}
```
下面的例子为包含指定值的 title 属性的所有元素设置样式。适用于由空格分隔的属性值：     

```
[title~="hello"] { color:red; }
```
属性选择器在为不带有 class 或 id 的表单设置样式时特别有用：     

```
input[type="text"]
{
  width:150px;
  display:block;
}
```
[attribute]	用于选取带有指定属性的元素。        
[attribute=value]	用于选取带有指定属性和值的元素。     
[attribute~=value]	用于选取属性值中包含指定词汇的元素。     
[attribute|=value]	用于选取带有以指定值开头的属性值的元素，该值必须是整个单词。     
[attribute^=value]	匹配属性值以指定值开头的每个元素。     
[attribute$=value]	匹配属性值以指定值结尾的每个元素。     
[attribute*=value]	匹配属性值中包含指定值的每个元素。     
还可以根据多个属性进行选择，只需将属性选择器链接在一起即可。     
例如，为了将同时有 href 和 title 属性的 HTML 超链接的文本设置为红色，可以这样写：     

```
a[href][title] {color:red;}
```


#### text-indent     
CSS 提供了 text-indent 属性，该属性可以方便地实现文本缩进。     
通过使用 text-indent 属性，所有元素的第一行都可以缩进一个给定的长度，甚至该长度可以是负值。     
一般来说，可以为所有块级元素应用 text-indent，但无法将该属性应用于行内元素，图像之类的替换元素上也无法应用 text-indent 属性。不过，如果一个块级元素（比如段落）的首行中有一个图像，它会随该行的其余文本移动。     
如果想把一个行内元素的第一行“缩进”，可以用左内边距或外边距创造这种效果。     
text-indent 可以使用所有长度单位，包括百分比值。     
百分数要相对于缩进元素父元素的宽度。换句话说，如果将缩进值设置为 20%，所影响元素的第一行会缩进其父元素宽度的 20%。     

#### text-align     
 - text-align 是一个基本的属性，它会影响一个元素中的文本行互相之间的对齐方式。     
 - left	把文本排列到左边。默认值：由浏览器决定。     
 - right	把文本排列到右边。     
 - center	把文本排列到中间。     
 - justify	实现两端对齐文本效果。     
 - inherit	规定应该从父元素继承 text-align 属性的值。     

将块级元素或表元素居中，要通过在这些元素上适当地设置左、右外边距来实现。     
您可能会认为 text-align:center 与 < CENTER > 元素的作用一样，但实际上二者大不相同。     
< CENTER > 不仅影响文本，还会把整个元素居中。text-align 不会控制元素的对齐，而只影响内部内容。元素本身不会从一段移到另一端，只是其中的文本受影响。     

#### word-spacing     
word-spacing 属性可以改变字（单词）之间的标准间隔。其默认值 normal 与设置值为 0 是一样的。     
word-spacing 属性接受一个正长度值或负长度值。如果提供一个正长度值，那么字之间的间隔就会增加。为 word-spacing 设置一个负值，会把它拉近。     
letter-spacing	设置字符间距。     

#### text-transform     
text-transform 属性处理文本的大小写。这个属性有 4 个值：     
默认值 none 对文本不做任何改动，将使用源文档中的原有大小写。顾名思义，uppercase 和 lowercase 将文本转换为全大写和全小写字符。最后，capitalize 只对每个单词的首字母大写。     
但感觉用js来变化好一点。     

#### text-decoration          
 - text-decoration 有 5 个值：     
 - none     
 - underline     
 - overline     
 - line-through     
 - blink     

不出所料，underline 会对元素加下划线，就像 HTML 中的 U 元素一样。overline 的作用恰好相反，会在文本的顶端画一个上划线。值 line-through 则在文本中间画一个贯穿线，等价于 HTML 中的 S 和 strike 元素。blink 会让文本闪烁，类似于 Netscape 支持的颇招非议的 blink 标记。     

#### white-space     
 - white-space 属性会影响到用户代理对源文档中的空格、换行和 tab 字符的处理。默认值：normal，空白会被浏览器忽略。     
 - 值	空白符	换行符	自动换行          
 - pre-line	合并	保留	允许     
 - normal	合并	忽略	允许     
 - nowrap	合并	忽略	不允许     
 - pre	保留	保留	不允许     
 - pre-wrap	保留	保留	允许     

#### direction     
direction 属性影响块级元素中文本的书写方向、表中列布局的方向、内容水平填充其元素框的方向、以及两端对齐元素中最后一行的位置。     

#### 链接     
 - 链接的特殊性在于能够根据它们所处的状态来设置它们的样式。     
 - 链接的四种状态：     
 - a:link - 普通的、未被访问的链接     
 - a:visited - 用户已访问的链接     
 - a:hover - 鼠标指针位于链接的上方     
 - a:active - 链接被点击的时刻     
 - 当为链接的不同状态设置样式时，请按照以下次序规则：     
 - a:hover 必须位于 a:link 和 a:visited 之后     
 - a:active 必须位于 a:hover 之后     

#### 列表     
要修改用于列表项的标志类型，可以使用属性 list-style-type：     

```
ul {list-style-type : square}
```
上面的声明把无序列表中的列表项标志设置为方块。     
有时，常规的标志是不够的。你可能想对各标志使用一个图像，这可以利用 list-style-image 属性做到。     
CSS2.1 可以确定标志出现在列表项内容之外还是内容内部。这是利用 list-style-position 完成的。     
为简单起见，可以将以上 3 个列表样式属性合并为一个方便的属性：list-style，就像这样：     

```
li {list-style : url(example.gif) square inside}
```
list-style 的值可以按任何顺序列出，而且这些值都可以忽略。只要提供了一个值，其它的就会填入其默认值。     


#### Table 属性     
 - 属性	描述     
 - border-collapse	设置是否把表格边框合并为单一的边框。     
 - border-spacing	设置分隔单元格边框的距离。     
 - caption-side	设置表格标题的位置。     
 - empty-cells	设置是否显示表格中的空单元格。     
 - table-layout	设置显示单元、行和列的算法。     

#### outline     
 - 轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。     
 - CSS outline 属性规定元素轮廓的样式、颜色和宽度。     
 - 属性	描述     
 - outline	在一个声明中设置所有的轮廓属性。     
 - outline-color	设置轮廓的颜色。     
 - outline-style	设置轮廓的样式。     
 - outline-width	设置轮廓的宽度。     


#### 一切皆为框     
div、h1 或 p 元素常常被称为块级元素。这意味着这些元素显示为一块内容，即“块框”。与之相反，span 和 strong 等元素称为“行内元素”，这是因为它们的内容显示在行中，即“行内框”。     
您可以使用 display 属性改变生成的框的类型。这意味着，通过将 display 属性设置为 block，可以让行内元素（比如 < a > 元素）表现得像块级元素一样。还可以通过把 display 设置为 none，让生成的元素根本没有框。这样的话，该框及其所有内容就不再显示，不占用文档中的空间。     
但是在一种情况下，即使没有进行显式定义，也会创建块级元素。这种情况发生在把一些文本添加到一个块级元素（比如 div）的开头。即使没有把这些文本定义为段落，它也会被当作段落对待：     

```
<div>
some text
<p>Some more text.</p>
</div>
```
在这种情况下，这个框称为无名块框，因为它不与专门定义的元素相关联。     
块级元素的文本行也会发生类似的情况。假设有一个包含三行文本的段落。每行文本形成一个无名框。无法直接对无名块或行框应用样式，因为没有可以应用样式的地方（注意，行框和行内框是两个概念）。但是，这有助于理解在屏幕上看到的所有东西都形成某种框。     

#### position     
 - 通过使用 position 属性，我们可以选择 4 种不同类型的定位，这会影响元素框生成的方式。          
 - position 属性值的含义：          
 - static: 元素框正常生成。块级元素生成一个矩形框，作为文档流的一部分，行内元素则会创建一个或多个行框，置于其父元素中。          
 - relative: 元素框偏移某个距离。元素仍保持其未定位前的形状，它原本所占的空间仍保留。     
 - absolute: 元素框从文档流完全删除，并相对于其包含块定位。包含块可能是文档中的另一个元素或者是初始包含块。元素原先在正常文档流中所占的空间会关闭，就好像元素原来不存在一样。元素定位后生成一个块级框，而不论原来它在正常流中生成何种类型的框。     
 - fixed: 元素框的表现类似于将 position 设置为 absolute，不过其包含块是视窗本身。     
 - 提示：相对定位实际上被看作普通流定位模型的一部分，因为元素的位置相对于它在普通流中的位置。     


#### clip     
 - clip 属性剪裁绝对定位元素。     
 - 当一幅图像的尺寸大于包含它的元素时会发生什么呢？"clip" 属性允许您规定一个元素的可见尺寸，这样此元素就会被修剪并显示为这个形状。     
 - 这个属性用于定义一个剪裁矩形。对于一个绝对定义元素，在这个矩形内的内容才可见。出了这个剪裁区域的内容会根据 overflow 的值来处理。剪裁区域可能比元素的内容区大，也可能比内容区小。     
 - 值	描述     
 - shape	设置元素的形状。唯一合法的形状值是：rect (top, right, bottom, left)     
 - auto	默认值。不应用任何剪裁。     
 - inherit	规定应该从父元素继承 clip 属性的值。     

#### 类名选择器     
通过把两个类选择器链接在一起，仅可以选择同时包含这些类名的元素（类名的顺序不限）。     

```
.important.urgent {background:silver;}
```

#### 伪类     
 - 属性	描述	     
 - :active	向被激活的元素添加样式。	     
 - :focus	向拥有键盘输入焦点的元素添加样式。	     
 - :hover	当鼠标悬浮在元素上方时，向元素添加样式。	     
 - :link	向未被访问的链接添加样式。	     
 - :visited	向已被访问的链接添加样式。	     
 - :first-child	向元素的第一个子元素添加样式。     
 - :lang	向带有指定 lang 属性的元素添加样式。	     

#### 伪元素     
 - 伪元素用于向某些选择器设置特殊效果。     
 - 属性	描述         
 - :first-letter	向文本的第一个字母添加特殊样式。	     
 - :first-line	向文本的首行添加特殊样式。	     
 - :before	在元素之前添加内容。	     
 - :after	在元素之后添加内容。	     

#### 尺寸     
 - CSS 尺寸属性允许你控制元素的高度和宽度。同样，还允许你增加行间距。     
 - 属性	描述     
 - height	设置元素的高度。     
 - line-height	设置行高。     
 - max-height	设置元素的最大高度。     
 - max-width	设置元素的最大宽度。     
 - min-height	设置元素的最小高度。     
 - min-width	设置元素的最小宽度。     
 - width	设置元素的宽度。     

#### 分类     
 - CSS 分类属性允许你控制如何显示元素，设置图像显示于另一元素中的何处，相对于其正常位置来定位元素，使用绝对值来定位元素，以及元素的可见度。     
 - 属性	描述     
 - clear	设置一个元素的侧面是否允许其他的浮动元素。     
 - cursor	规定当指向某元素之上时显示的指针类型。     
 - display	设置是否及如何显示元素。     
 - float	定义元素在哪个方向浮动。     
 - position	把元素放置到一个静态的、相对的、绝对的、或固定的位置中。     
 - visibility	设置元素是否可见或不可见。     
     
#### 媒介类型     
媒介类型(Media Types)允许你定义以何种媒介来提交文档。文档可以被显示在显示器、纸媒介或者听觉浏览器等等。     
某些 CSS 属性仅仅被设计为针对某些媒介。比方说 "voice-family" 属性被设计为针对听觉用户终端。其他的属性可被用于不同的媒介。例如，"font-size" 属性可被用于显示器以及印刷媒介，但是也许会带有不同的值。显示器上面的显示的文档通常会需要比纸媒介文档更大的字号，同时，在显示器上，sans-serif 字体更易阅读，而在纸媒介上，serif 字体更易阅读。     
@media规则     
@media 规则使你有能力在相同的样式表中，使用不同的样式规则来针对不同的媒介。     
下面这个例子中的样式告知浏览器在显示器上显示 14 像素的 Verdana 字体。但是假如页面需要被打印，将使用 10 个像素的 Times 字体。注意：font-weight 被设置为粗体，不论显示器还是纸媒介：     

```
<html>
<head>
<style>
@media screen
{
p.test {font-family:verdana,sans-serif; font-size:14px}
}
@media print
{
p.test {font-family:times,serif; font-size:10px}
}
@media screen,print
{
p.test {font-weight:bold}
}
</style>
</head>
<body>....</body>
</html>
```

##### 不同的媒介类型     
 - 注释：媒介类型名称对大小写不敏感。     
 - 媒介类型	描述     
 - all	用于所有的媒介设备。     
 - aural	用于语音和音频合成器。     
 - braille	用于盲人用点字法触觉回馈设备。     
 - embossed	用于分页的盲人用点字法打印机。     
 - handheld	用于小的手持的设备。     
 - print	用于打印机。     
 - projection	用于方案展示，比如幻灯片。     
 - screen	用于电脑显示器。     
 - tty	用于使用固定密度字母栅格的媒介，比如电传打字机和终端。     
 - tv	用于电视机类型的设备。     

#### margin可以为负值，padding不能为负值。     


