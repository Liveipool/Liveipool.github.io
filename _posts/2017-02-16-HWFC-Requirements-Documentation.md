---
layout: post
title: HWFC球队网站项目需求文档   
date: 2017-02-16
categories: blog
description: 前端学习
---

# 需求文档           

**由于石墨文档需要登录才能访问，因此我将HWFC项目已有的需求文档、编码规范、设计图等放到个人博客上以供查看。**           

#### 相关链接               
 - [HWFC球队网站项目需求文档](http://liveipool.com/blog/2017/02/16/HWFC-Requirements-Documentation)                      
 - [HWFC球队网站项目编码规范](http://liveipool.com/blog/2017/02/16/HWFC-Coding-Specification)                  
 - [HWFC球队网站项目设计图、原型图](http://liveipool.com/blog/2017/02/16/HWFC-Design-Diagram)                        

## 一、项目介绍           
Hello World Football Club 官方网站。           
该网站可用来向用户提供HWFC的各种介绍，如球队历史、球队成员、战绩记录等。用户也可以查看HWFC的各种图片视频等，登录后还可以进行点赞、评论。未来可能会开设提供给赞助商的广告版面，以及提供给每届新生的招新报名页面等。           
该项目于2016年12月21日正式启动。           

## 二、项目成员           
李为          tel:  18826072459    email: 123653079@qq.com           
林桢          tel:  15902096442    email:  sysulz19920512@163.com           
欧光文      tel: 13602232650      email:ougwen1235@163.com           
黄潮立      tel：15521188004     email: gavingog@qq.com            
胡弘康      tel:  18819253591    email: codeforsomeone@gmail.com           
邓杰友      tel：15622243216           

## 三、项目需求           
功能需求：（按照各页面来说）           

#### 一、导航栏(navigation-bar 组件1)和底栏(footer 组件2)（v1.0）           
导航栏和底栏所有界面均有。           

导航栏：           
 1. 球队队徽及“HWFC”           
 2. 主页的链接           
 3. 球队简介页面的链接                      
 4. 球队成员页面的链接           
 5. 战绩记录页面的链接           
 6. 登录按钮 （v1.0仅管理员能登录）           
 7. 底栏           
 8. 中文大字 “我在中大，混碗饭吃”           
 9. 英文大字 “Hello World Football Club”           
 10. 版权说明 “@Copyright Hello World Football Club”           

#### 二、主页（home）（V1.0）           
有3个组件：           

**组件3**: 图片专区的链接（v1.0）           
1. 采用轮播的方式，轮播张数为4张。           
2. 点击图片进入图片专区。           

**组件4**: 上为导航栏,下为详细信息（v1.0）           
上部导航栏:           
1. 一个横向导航栏                      
2. 两栏定时轮播,分别为"预告"和"文章","文章"为链接,点击进入文章专区,"预告"不可点.           
3." 预告"变为链接,点击进入预告专区,增加"留言",点击进入留言专区(v2.0)           

下部详细信息:           
预告:           
1. 时间           
2. 内容           
                      
文章：           
1. 标题（如：如何干一名优秀的程序员）           
2. 上传时间（如：2016.10.15）           
3. 作者（如：肥桢）           
4. 摘要（暂定70字，届时视情况可更改）           

**组件5**: 视频专区的链接和两个热点视频（v1.0）           
上为一个链接，点击进入视频专区           
下为两个视频的封面           

留言墙页面的链接及最新留言（添加进组件4里）（v2.0）           
招新报名页面的链接（每年招新时开放）（v3.0）           
快乐足球图片专栏（v3.0）           
广告专区（v3.0）           
  
#### 三、球队简介页面（introduction）（v1.0）           

有5个组件：           
1. 组件6：时间轴，一个事件一个节点，每个事件对应一张图片和一段文字介绍，它们分别在组件7和组件8中呈现。           
2. 组件7：对应事件的图片。           
3. 组件8：对应事件的文字介绍。           
4. 组件11: 点击之后再加载更多内容的组件.           
5. 组件13: 回到顶部组件.           

注意，在时间轴的两侧，组件7和组件8交替呈现，可参看设计图效果。           

#### 四、成员介绍（members）（v1.0）                      
有6个组件:           
1. 组件14:该成员的照片           
2. 组件15:该成员的各属性           
3. 组件16:该成员的自我介绍           
4. 四个组件20:分别是根据 年级 队内职责 出生年份 场上位置对成员数组进行排序,在页面上显示最接近条件的一个.           
5. 组件21:点击显示上一个成员的三部分信息(顺序是按照排序后的)           
6. 组件22:点击显示下一个成员的三部分信息           
  
#### 五、战绩记录（records）（V1.0）           
有6个组件：           
1. 组件6：时间轴，一个事件一个节点，每个事件对应一张图片和一段文字介绍，它们分别在组件7和组件8中呈现。注意,"胜","平","负","夺冠"四种事件的节点图标不同.           
2. 组件7：对应事件的图片。           
3. 组件8：对应事件的文字介绍。           
4. 组件11: 点击之后再加载更多内容的组件.           
5. 组件13: 回到顶部组件.           
6. 三个组件20:分别是根据 年份 赛果 赛事 对时间轴进行排序,最符合要求的排在最顶端.           

#### 六、图片专区（images）（V1.0）           
有4个组件:           
1. 组件9:图片文件夹组件,页面上按照时间呈现八个文件夹.           
2. 组件10:关键字搜索框组件,根据每个文件夹的名字中的关键字进行搜索,搜索结果实时展现(按照时间顺序显示前8个符合要求的搜索结果文件夹,搜索结果不够8个时,后面接普通的按照时间排序的文件夹).           
3. 组件11:点击之后再加载更多内容的组件.           
4. 组件13: 回到顶部组件(点击加载更多后才出现).           
  
图片可评论、点赞。（v2.0）           

#### 七、视频专区（videos）（V1.0）           
有两个组件:           
1. 组件10:关键字搜索框组件,根据每个文件夹的名字中的关键字进行搜索,搜索结果实时展现(即输入关键字时从三个类别选择框变为8个小框，也即按照时间顺序显示前8个符合要求的搜索结果文件夹，清空搜索内容时变回为三个类别选择框）
2. 组件12:视频每个类别一个板块,那个板块做成一个组件.           
  
视频可评论、点赞。（v2.0）           

#### 八、视频专区各部分（如：videos/games）（V1.0）           
有5个组件:           
1. 组件10:关键字搜索框组件,根据每个文件夹的名字中的关键字进行搜索,搜索结果实时展现(按照时间顺序显示前8个符合要求的搜索结果文件夹,搜索结果不够8个时,后面接普通的按照时间排序的文件夹).           
2. 组件11:点击之后再加载更多内容的组件.           
3. 组件13: 回到顶部组件(点击加载更多后才出现).             
4. 组件17: 具体视频组件,视页面情况而定,一页四个或六个           
5. 组件18: 类别选择组件,一个激活,两个灭活,页面上显示激活的类型对应的视频,输入关键字进行搜索时,三个都灭活.           

#### 九、文章专区（essays） （V1.0）           
有4个组件:           
1. 组件10:关键字搜索框组件,根据每个文件夹的名字中的关键字进行搜索,搜索结果实时展现(按照时间顺序显示前8个符合要求的搜索结果文件夹,搜索结果不够8个时,后面接普通的按照时间排序的文件夹).           
2. 组件11:点击之后再加载更多内容的组件.           
3. 组件13: 回到顶部组件(点击加载更多后才出现).           
4. 组件19: 展现文章组件.一个组件用于展现一篇文章.           
   
文章可评论、点赞。（v2.0）           

####  十、管理员页面（administrator）  （V1.0）           
通过管理员帐号登录后进入,具体页面待设计，功能如下:           
1. 能够更改首页的四张图片.           
2. 能够更改首页的两个视频.           
3. 能够更改首页的最新预告.           
4. 能够更改首页的最新文章.           
5. 球队介绍页面中,能够添加一条记录.           
6. 球队介绍页面中,能够删除某条记录.           
7. 球队介绍页面中,能够编辑某条记录的图片和文字简介.           
8. 球队成员页面中,能够添加一个成员.           
9. 球队成员页面中,能够删除某个成员.           
10. 球队成员页面中,能够编辑某个成员的任意资料.           
11. 战绩记录页面中,能够添加一条记录.           
12. 战绩记录页面中,能够删除某条记录.           
13. 战绩记录页面中,能够编辑某条记录的图片和文字简介.           
14. 图片专区中,能够添加一个文件夹.           
15. 图片专区中,能够删除某个文件夹.           
16. 图片专区中,能够删除某个文件夹的某张图片.           
17. 图片专区中,能够编辑某个文件夹的名字.           
18. 视频专区中,能够添加一个视频.           
19. 视频专区中,能够删除某个视频.           
20. 视频专区中,能够编辑某个视频的名字.           
21. 视频专区中,能够编辑某个视频属于哪个类别(比赛,进球或娱乐).           
22. 文章专区中,能够添加一篇文章.           
23. 文章专区中,能够删除某篇文章.           
24. 文章专区中,能够编辑某篇文章.           

#### 十一、预告专区（notices）（V2.0）           
最新预告显示到主页。           
预告专区内的预告按照时间排序。           
可通过关键字进行搜索。           
由管理员进行添加、更改、隐藏、删除预告。             

#### 十二、普通用户的用户系统（V2.0）           
v2.0后提供普通用户用户系统，由管理员分配帐号。           
登录后可进行图片、视频、文章的评论、点赞。           
未登录时为游客状态，可以看到评论，但不能进行评论。（或者不能看到评论？）           

#### 十三、普通用户对图片、视频、文章的评论部分（V2.0）           
待设计。           

#### 十四、积分榜（V2.0）           
联赛积分榜或杯赛对阵表，标明比赛的时间段。            
待设计。           

#### 十五、管理员用户管理、评论管理页面（V2.0）           
待设计。           

#### 十六、留言墙 （V3.0）           
待设计。           

#### 十七、快乐足球图片专栏（V3.0）           
待设计。           

#### 十八、广告区（V3.0）           
待设计。           

#### 十九、招新报名页面（V3.0）                      
待设计。           

## 四、技术选型           
前端使用Vue.js、bootstrap框架，并使用webpack进行打包。           
后端使用Express框架，语言为node.js。           
数据库使用mongodb。           
版本控制采用git，使用github来做为代码仓(https://github.com/Liveipool/HWFC)。           
快速迭代：Docker.           

其中：           
Vue采用最新的Vue2.0，使用vur-router管理路由，vuex管理前端页面组件状态，bootstarp3为前端UI框架，ES2016作为js语言规范，scss为css预处理语言。           

## 五、版本           
1.0：首次发布上线版，包含了：1、主页 2、球队简介 3、球队成员 4、战绩记录 5、图片专区 6、视频专区  7、文章专区 8、管理员用户及其对各页面的操作           

2.0：1、添加进普通用户的用户系统 2、普通用户可以图片、视频、文章进行评论和点赞  3、添加积分榜页面 4、管理员增加对普通用户进行管理以及对各种评论进行管理的功能 5、预告专区           

3.0：1、添加留言墙 2、添加快乐足球专栏 3、添加广告区 4、添加招新报名页面           

PS. 除版本1.0外，后面的皆为一些预想，其中的功能也可以提前或者推后，视具体情况而定。           


