---
title: Exception和Error
date: '2020-01-15 10:40'
sticky: 0
keywords: Exception Error
categories:
  - java异常
  - java基础
tags:
  - java异常
  - java基础
  - Exception
  - Error
abbrlink: 53751
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gy8iuroj06j31ek0u0djc.jpg'
top_img:
description:
---

## 1、比Exception和Error，另外，运行时异常与一般异常有什么区别？

Exception和Error都是继承了Throwable类，在Java中只有Throwable类型的实例才可以被抛出（throw）或者捕获（catch），它是异常处理机制的基本组成类型。

Exception和Error体现了Java平台设计者对不同异常情况的分类。Exception是程序正常运行中，可以预料的意外情况，可能并且应该被捕获，进行相应处理。

Error是指在正常情况下，不大可能出现的情况，绝大部分的Error都会导致程序（比如JVM自身）处于非正常的、不可恢复状态。既然是非正常情况，所以不便于也不需要捕获，常见的比如OutOfMemoryError之类，都是Error的子类。

Exception又分为**可检查**（checked）异常和**不检查**（unchecked）异常，可检查异常在源代码里必须显式地进行捕获处理，这是编译期检查的一部分。前面我介绍的不可查的Error，是Throwable不是Exception。

不检查异常就是所谓的运行时异常，类似 NullPointerException、ArrayIndexOutOfBoundsException之类，通常是可以编码避免的逻辑错误，具体根据需要来判断是否需要捕获，并不会在编译期强制要求。

### 1.1、常见的一些Exception和Error

![](https://tva1.sinaimg.cn/large/008i3skNly1gyj2nw37osj315a0u043e.jpg)

**NoClassDefFoundError和ClassNotFoundException**



























