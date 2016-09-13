---
layout: post
title: 基于Socket通信
date: 2016-09-14
categories: blog
tags: [网络基础]
description: 网络基础
---

# 基于Socket通信
网络通信是一种进程间通信（IPC：Inter-Process Communication），要求位于不同网络节点不同进程的通信双方必须遵循统一的通信协议方可实现。基于套接字（Socket）通信是应用在不同节点上的**进程间通信的典型方法。**  

Socket 是从电话通信中借用的一个术语，Socket的英文原义是“孔”或“插座”。**socket算不上一个协议，他是应用层和传输层之间的一个抽象层，是一个编程接口。它对应的是传输层以上的东西。**  一个Socket代表了逻辑上的一条通信“通道”的一个方向的最端点。而实际上讲，Socket是物理网络地址和逻辑端口号的一个集合，而这个集合可以向另外一个位置的与他具有相同定义的Socket进行数据传输。因为Socket是由机器地址和端口号来区分/识别的，那么在一个特定的计算机网络上，每一个Socket都是以此方式被唯一识别的。这就使得应用程序可以唯一地去定位网络上的另外一个位置的Socket。对于同一台机器上的两个Socket，他们是完全具备彼此间进行通信的可能的；在这种情况下，两个Socket具有相同的主机地址，但是他们拥有不同的端口号。

### 基于Socket API 开发的基本原理
Socket API是TCP/IP网络的API。Socket API最早作为伯克利（Berkeley）UNIX操作系统的程序库，出现于20世纪80年代早期，用于提供IPC（IPC：Inter-Process Communication）通信。目前，所有主流操作系统都支持Socket API，在BSD、Linux等基于UNIX的系统中，Socket API都是操作系统内核的一部分。在MSDOS、Windows等个人计算机操作系统也是以程序库的形式提供Socket API（其中在Windows系统中，Socket API被称作Winsock）。   
JAVA语言在设计之初也考虑到网络编程，也将Socket API作为语言核心类的一部分。
Socket API的概念模型：  
![Socket API的概念模型.png](http://upload-images.jianshu.io/upload_images/3001083-88cb4bc5f3cb3e67.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  


Socket API 提供给用户一种处理通信的方法，使得相关进程可以存在于横跨网络的不同的工作站上。Socket类型有二种方式：  
1. 流式Socket(基于TCP)，它提供进程之间的逻辑连接，并且支持可靠的数据交换。  
2. 数据报Socket(基于UDP)，它是无连接的并且不可靠。  

### 一、流式Socket
流式 Socket 所完成的通信是一种基于连接的通信，即在通信开始之前先由通信双方确认身份并建立一条专用的虚拟连接通道，然后它们通过这条通道传送数据信息进行通信，当通信结束时再将原先所建立的连接拆除。  这个过程中，Server 端首先在某端口提供一个监听Client 请求的监听服务并处于监听状态，当Client 端向该Server 的这个端口提出服务请求时，Server 端和Client 端就建立了一个连接和一条传输数据的通道，当通信结束时，这个连接通道将被同时拆除。
流式Socket连接控制方式：  
![流式Socket连接控制方式.png](http://upload-images.jianshu.io/upload_images/3001083-c51f17d448da18aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   
 

在 Java 程序中使用socket 进行通信可支持在一台主机上运行的单个服务程序为多个不同的客户程序提供服务。为实现这一目标，服务程序将选定一个固定的端口号对外发布服务客户程序则必须先按约定的主机与端口号向服务程序发送一个要求建立连接的请求（这些请求称为连接请求），申请建立一个到服务程序的连接。  
使用流式Socket的通信方式：  
![使用流式Socket的通信方式.png](http://upload-images.jianshu.io/upload_images/3001083-334dc2e7d6593f45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  


服务程序在收到某一客户程序的连接请求后，**并不是利用对外发布的那个端口号建立与该客户程序的连接，而是另外分配一个新的端口号建立与客户程序之间的连接，原端口号仍用于监听其他客户程序的连接请求。**这一通信模式保证了服务程序对外公布的端口号仅用于处理客户程序要求建立连接的请求，不会因为该端口因长期处理客户程序的服务请求而导致其他客户程序的阻塞。

针对这一流式Socket 通信模式，java.net 程序包将基于TCP 通信的socket 封装为两个类：**类Socket 表达了一个用于建立TCP 连接的socket，该socket 既可由客户程序使用，也可由服务程序使用；类ServerSocket 则是一个服务端专门监听客户程序连接请求的socket 的抽象，仅在服务程序中使用。**  

#### 基于流式 socket 通信的客户程序:
1. 通过指定主机（主机名或InetAddress 的实例）和端口号构造一个socket.  
2. 然后调用Socket 类的getInputStream()和getOutputStream()分别打开与该socket 关联的输入流和输出流.  
3. 依照服务程序约定的协议读取输入流或写入输出流.  
4. 依次关闭输入∕输出流和socket.
流式Socket通信过程：  
![流式Socket通信过程.png](http://upload-images.jianshu.io/upload_images/3001083-89d032a6ff6a2731.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  


#### 基于流式 socket 通信的服务程序:  
负责监听对外发布的端口号，该端口专用于处理客户程序的连接请求。因而服务程序:  
1. 通过指定监听的端口号创建一个ServerSocket 实例.  
2. 调用该实例的accept()方法(调用accept()方法会引起阻塞，直至有一个客户程序发送连接请求到服务程
序所监听的端口).  
3. 服务程序收到连接请求后将分配一个新端口号建立与客户程序的连接并返回该连接的一个socket.  
4. 服务程序调用该socket 的getInputStream() 和getOutputStream()方法获取与客户程序的连接相关联的输入流和输出流，并依照预先约定的协议读输入流或写输出流.  
5. 完成所有通信后，服务程序必须依次关闭所有输入流和输出流、所有已建立连接的socket、以及专用于监听的socket。

不同客户程序的编程复杂度主要体现在如何依照客户程序和服务程序双方约定的协议读∕写并处理数据.(代码就不放上来了)   


### 二、数据报Socket
基本原理：  
UDP采用数据报进行通信，数据报是否可到达目标、以什么次序到达目标、到达目标时内容是否依然正确等均是未经校验的。因而UDP是一种不可靠的点对点通信，适合对通信性能要求高、但通信可靠性要求低的应用，并可支持广播和多播通信方式。与基于TCP的通信类似，基于UDP的单播通信是将数据报从一个发送方传输给单个接收方。

#### 面向连接数据报UDP  

一般来说，很少用数据报Socket 实现面向连接通信，因为此API 提供的连接非常简单，通常难以满足应用要求。但我们可以从这个例子中可以看出“分层”思想的体现。
面向连接数据报Socket：  
![面向连接数据报Socket.png](http://upload-images.jianshu.io/upload_images/3001083-15e8986c368b49aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  


#### 面向无连接数据报UDP  
UDP与TCP协议之间的不同在于UDP不是一种基于稳定连接的通讯协议，UDP协议使用数据报式套接字，UDP在数据传输之前不需要先建立连接，UDP没有组装和重传请求的功能，并不保证接收方能够接收到该数据包，也不保证接收方所接收到的数据和发送方所发送的数据在内容和顺序上是完全一致的。其主要工作是将应用程序传输过来的数据分块交给网络层，确认接受到分组信息。
无连接数据报Socket：  
![无连接数据报Socket.png](http://upload-images.jianshu.io/upload_images/3001083-de6c850b221e914c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)   


java.net程序包为实现UDP单播通信主要提供了两个类：  
1. 类DatagramPacket代表一个被传送的UDP数据报，该类封装了被传送数据报的内容、源主机与端口号、目标主机与端口号等信息。  
2. 类DatagramSocket代表一个用于传送UDP数据报的UDPsocket。  

在Java.net包中，DatagramPacket类用于实现数据报的接收和发送、读取报文信息等。在下图建立的模型中，使用UDP数据报协议，以收发数据报作为通信方式。每个数据报文有独立的源地址和目的地址，服务器和客户端不建立连接，通信的内容数据报短信息的形式实现，但传输时不能保证对方一定能收到，也不能保证收到的报文次序。  
数据报Socket通信过程：  

![数据报Socket通信过程.png](http://upload-images.jianshu.io/upload_images/3001083-40c513af5f5eaa42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  


同样，代码就不放上来了，这一部分篇幅挺大，原理需要好好理解。