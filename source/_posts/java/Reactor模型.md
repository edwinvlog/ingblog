---
title: Reactor模型
date: '2019-12-16 13:51'
sticky: 0
keywords: reactor 数据结构 高性能IO
categories:
  - 数据结构
  - 高性能IO
tags:
  - reactor
  - 高性能IO
abbrlink: 28012
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gyk2cu41f1j31hy0u04hg.jpg'
top_img:
description:
---
#### 单线程Reactor模式

一个线程：
单线程：建立连接（Acceptor）、监听accept、read、write事件（Reactor）、处理事件（Handler）都只用一个单线程。

####  多线程Reactor模式
一个线程 + 一个线程池：
单线程：建立连接（Acceptor）和 监听accept、read、write事件（Reactor），复用一个线程。
工作线程池：处理事件（Handler），由一个工作线程池来执行业务逻辑，包括数据就绪后，用户态的数据读写。

#### 主从Reactor模式

三个线程池：
主线程池：建立连接（Acceptor），并且将accept事件注册到从线程池。
从线程池：监听accept、read、write事件（Reactor），包括等待数据就绪时，内核态的数据I读写。
工作线程池：处理事件（Handler），由一个工作线程池来执行业务逻辑，包括数据就绪后，用户态的数据读写

具体的可以参考并发大神 doug lea 关于Reactor的文章。 http://gee.cs.oswego.edu/dl/cpjslides/nio.pdf
再提一点，使用了多路复用，不一定是使用了Reacto模型，Mysql使用了select（为什么不使用epoll，因为Mysql的瓶颈不是网络，是磁盘IO），但是并不是Reactor模型

#### 扩展

nginx：nginx是多进程模型，master进程不处理网络IO，每个Wroker进程是一个独立的单Reacotr单线程模型。
netty：通信绝对的王者，默认是多Reactor，主Reacotr只负责建立连接，然后把建立好的连接给到从Reactor，从Reactor负责IO读写。当然可以专门调整为单Reactor。
kafka：kafka也是多Reactor，但是因为Kafka主要与磁盘IO交互，因此真正的读写数据不是从Reactor处理的，而是有一个worker线程池，专门处理磁盘IO，从Reactor负责网络IO，然后把任务交给worker线程池处理。