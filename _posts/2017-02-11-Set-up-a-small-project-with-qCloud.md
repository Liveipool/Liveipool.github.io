---
layout: post
title: 从无到有，在腾讯云服务器上部署一个个人小项目   
date: 2017-02-11
categories: blog
description: 前端学习
---

# 从无到有，在腾讯云服务器上部署一个个人小项目       
春节刚过，各大公司的HR们就已经蠢蠢欲动了，开始到处搜寻实习生。今天正逢元宵节，刚吃了8个汤圆，真是甜过初恋，上实习僧app瞟了一眼，发现就广州开始招前端实习生的就已经有十多家互联网公司了。是的，春招来了。      
**每个人都有对自己的规划，在互联网时代的大浪潮里，我想要上一条靠谱的大船。**        
在整个实习招聘的流程里，简历是非常重要的一环。作为一个前端开发人员，去套一个网上古板的静态模板也没啥意思，但github帐号的唯一一个github pages已经被我写博客用掉了，没办法，去买个服务器和域名吧。         
![1.png](http://upload-images.jianshu.io/upload_images/3001083-018c2ef9686c6e68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)         

相信我，我绝对不是在打广告。            
一听说腾讯云有个学生1元买服务器的活动，我花了二十秒就填写完了腾讯云里的绝大部分账户信息。可后来二十分钟，我却一直卡在“学生认证”页面，我把正确的内容重复填了100遍，它还是一直提示我信息有错，我盯着我的学生证，久久止不住眼泪。         
没办法，硬买吧。      
![2.png](http://upload-images.jianshu.io/upload_images/3001083-c55dd323264f8323.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我毫不犹豫的把所有配置拉到最高。       
看了一看价格，然后默默的选了最低配置，65块一个月。       
结帐时，突然发现我还有30块的代金卷，瞬间气消了一大半，擦了擦眼泪，配置服务器吧。       

**直到现在在写博客的时候，我都还在回想，我当时为何会舍弃图形界面点点鼠标完事的的windows，也不选打代码用的debian和ubuntu，最后去选了一个centOS，只能说万幸的是，至少它也是linux。**       
![3.png](http://upload-images.jianshu.io/upload_images/3001083-fb50b88d25e7b3f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![4.png](http://upload-images.jianshu.io/upload_images/3001083-9f8799bfd7e6e71e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
![5.png](http://upload-images.jianshu.io/upload_images/3001083-868a126e04c97396.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

虽然以前从来没用过centOS，也没配置过服务器，但其实原理大致上也差不多，上网找找教程也是能找到的。
主要思路是先用yum安装一些最基础的gcc g++，以及一些配置文件等等。       
因为我是准备放一个express项目上去，所以安装node，npm，express什么的自然也不能少，装完以后，可以考虑将代码放到服务器上来了。       

#### SSH 、scp

```
登陆：ssh [hostname] -u user
输入密码：*****
```
SSH，简单的来说，通过上面两行代码登陆进某个linux系统之后，就可以像操控自己的机器一样操控它了，不过没有图形界面。windows的话，可以用putty等软件来模仿。       
       
而scp，是简单的传输命令，可以在自己的机器和目标机器间传输文件：       

```
scp /etc/php.ini user@linux.com:/home/user
会将本地的 /etc/php.ini 这个文件 copy 到 linux.com，使用者user的主目录下。执行命令之后需要输入密码，之后就开始传送。

scp user@linux.com:/etc/php.ini /home/user2
将主机linux.com 上的/etc/php.ini文件copy到本地/home/user2目录下
```
**在我这次配置的时候，我只需要使用scp命令将自己电脑上的代码（用的ubuntu16.04）传到服务器上就好了，因此我不需要用SSH登进服务器。**       

传输不是特别快，但毕竟项目不大，总共也没花多久。       
另外的方法的话，git或者ftp应该都是可以的，不过scp确实比较简单。       

#### nohup、forever       
在服务器端接收完代码之后，使用一句npm run start就可以运行这个express项目了（就像开发的时候一样），在浏览器中输入服务器的外网ip：3000就可以看到项目了（3000为项目里设定的端口）。       

但是，这里有个问题，如果我关掉终端或者终止运行这一行命令，应用也就关闭了。作为上线的应用，我肯定还是希望在关掉电脑时，它也能7/24地运行。       

这个问题网上主要有两种解决方法：       
1. nohup：**可以看为no hang up的缩写，在linux中，如果我们希望某个程序一直在后台运行，就可以使用这个命令。**       
2. forever：forever是npm里的一个包，**它能够让nodejs在后台运行，并且能够记录输出和错误日志等，**但我只是想做一个单页面的简历而已，暂时用不上，以后在大项目中可以考虑。       

在加上nohup命令运行后，即使关掉终端，在网页中也还是可以访问应用。       

**只差一步了，我对自己说，喝了几口奶冷静了一下激动的心情。**       
**程序员的成就感有时候就是来得这么简单。**       

#### 购买域名，进行配置       
所谓的最后一步，就是将域名在DNS服务器上配置，和ip地址连接起来。       
腾讯云上整套流程还是比较完善的，我花了20大洋去挑了一个 liveipool.info，感觉用这个网址来做简历还是挺合适的，心里又美滋滋的。       

程序员的购物观念原来也是这么独特。       

![6.png](http://upload-images.jianshu.io/upload_images/3001083-4103034a8d5a0ac1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

NS类型已经由腾讯云解析自动生成好了，这里也不需要设置别名的CNAME，因此只需配置两条A类型的记录就大功告成了。       
剩下就是焦急而漫长的等待了，一边刷着朋友圈一边去ping主机，等待着ping通的那一刻。       
![7.png](http://upload-images.jianshu.io/upload_images/3001083-d5ff486a968cdc40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

**通了！**       
**通了！！**       
**Ping通了！！！**       
 
**终于等到ping通的那一刻，我打开浏览器，搓了搓手，在地址栏输下liveipool.info，用左手将散乱的刘海理了一理，右手点下回车，准备起身庆祝，见证奇迹的时刻。**       
![8.png](http://upload-images.jianshu.io/upload_images/3001083-9cdd31c41d8f21a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       

××××，我大叫了一声四个字母的单词。       
为什么，访问不了？？       

#### F×CK， 反向代理！All I need is U！       
我恍然大悟，输入域名访问的是ip地址的HTTP协议默认的80端口，而我的应用是在3000端口上的，看来现在还不能庆祝。       
我试着去把代码里的3000端口改为80,但发现这样并不可行，会提示80端口已被占用。       

得另寻他法。       

**反向代理，**这四个字出现在了我的眼前，虽然也挺说过，但还不是特别的了解，google了一下，瞬间清晰了。       

**正向代理：**也就是我们平时翻墙用的代理 ,他的工作原理就像一个跳板。简单的说,我是一个用户,我访问不了某网站,但是我能访问一个代理服务器,这个代理服务器呢,他能访问那个我不能访问的网站,**于是我先连上代理服务器,告诉他我需要那个无法访问网站的内容,代理服务器去取回来,然后返回给我。**想要使用正向代理，**客户端必须要进行一些特别的设置。**       
**反向代理：**而反向代理是说，比如客户端通过我的域名liveipool.info访问我的应用，然后在页面上呈现出信息，看起来像是直接的C/S，但实际上，客户端访问的是liveipool.info：80，而如果配置了**反向代理服务器**，**反向代理服务器就将请求转发给配置文件里规定好的那个真正资源所在的服务器，并将从真正资源所在的服务器上取回的资源返回给客户端，而且这一切，客户端毫不知情，也不需要进行任何配置。**       

![9.png](http://upload-images.jianshu.io/upload_images/3001083-b226c5137718158e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
知道原理后就不复杂了，网上有很多教程，一般是用nginx或nat123，我使用nginx在服务器上进行了配置，一切OK！       

又到了见证奇迹的时刻，但我却相对于之前已经平静了不少，我沉稳地在地址栏键下liveipool.info。       

![10.png](http://upload-images.jianshu.io/upload_images/3001083-064ad3efe1666f00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)       
果然，很荣幸，页面正常地呈现在了我的面前。如果说前面是对接近成果的激动，那么现在就是一种胸有成竹的自信。
在开发过程中，我们会遇到各种各样的bug，有时我们会为了解决bug而随便copy几行代码草草了事，但实际上，我们很可能还会遇到相似的问题，而那时，copy代码却不管用了。       
**说到底，在遇到问题时，还是得静下心来，把原理弄懂，原理懂了，才能真正写出健壮且优雅的代码。**       

虽然今天也搞了大半天，但毕竟也是从买服务器开始，第一次用centos系统，第一次配置服务器，然后配置域名等等，还是收益颇丰，接下来就是好好写简历了，努力找到一个好的实习！！       

再过一个小时，英超比赛就要开打了，利物浦对阵热刺，在利物浦七轮不胜的同时，让我大声说一句：“The champions！Tottenham Hotspur!!!， **热刺是冠军！！！**”       

（补充说明，由于链接域名需要几十个工作日的网上备案，因此这次简历就没法使用域名了，直接通过ip地址查看：[123.207.29.33](http://123.207.29.33:3000/)）        