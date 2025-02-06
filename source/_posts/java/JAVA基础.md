---
title: JAVA基础回顾
sticky: 0
keywords: java
categories: java基础
tags: [java基础, java]
abbrlink: 15913
date: 2018-02-05 21:00:00
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gxorzlwn36j31xs0t0q5b.jpg'
description:
top_img:
---


##### 1. 8种基本数据类型

![java8中基本数据类型](https://images.gitee.com/uploads/images/2019/1001/160806_dbece76c_5028606.jpeg "java8中基本数据类型")
##### 2. 面向对象的三大特性
 - 封装
 - 继承
 - 多态
##### 3. ==、hashCode()、 equals()
- ==
  - 本质：是一个比较运算符
  - 作用：比较俩个对象地址是否相等，基本数据类型比较的是值，引用数据类型比较的是内存地址  

- hashCode() 
   - 本质：返回一个int整数值(哈希码/散列码)
   - 作用：确定该对象在哈希表中的索引位置，也就是说hashCode()在散列表(Java集合中本质是散列表的类，如HashMap，Hashtable，HashSet。)中才有用，在其它情况下没用(例如，创建类的单个对象，或者创建类的对象数组等等)。
   - hashcode()定义在JDK的Object.java中，所以Java中的任何类都包含有该方法
   - 使用：比较俩个引用类型对象是否相等时，需要重写该方法
- equals()
   - 作用：判断俩个引用类型对象的地址是否相等
   - 使用：
           1). 类没有重写equals()方法。调用equals()比较该类的俩个对象时，等价与'=='，比较的是俩个对象的地址值。
           2). 类重写equals()方法。比较的是俩个对象的内容(对象的属性值)是否相等。

   - 源码
     `public boolean equals(Object obj) {
          return (this == obj);
      }` 
-  一些特性
   -  **如果两个对象相等，则hashcode一定也是相同的** 
  -  **两个对象有相同的hashcode值，它们也不一定是相等的** 
  -  **因此，equals 方法被覆盖过，则 hashCode 方法也必须被覆盖** 

##### 4. 线程
  - 定义：一个比进程更小的执行单位，一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。
  - 基本状态

    ![Java线程基本状态](https://images.gitee.com/uploads/images/2019/0929/170959_5599a2cf_5028606.png "java线程基本状态")
    
                                    图源《Java 并发编程艺术》4.1.4节

    线程在生命周期中并不是固定处于某一个状态而是随着代码的执行在不同状态之间切换。

    ![Java线程运行状态](https://images.gitee.com/uploads/images/2019/0929/171406_4883b913_5028606.png "java线程运行状态")

                                    图源《Java 并发编程艺术》4.1.4节

    线程创建之后它将处于 NEW（新建） 状态，调用 start() 方法后开始运行，线程这时候处于 READY（可运行） 状态。可运行状态的线程获得了 cpu 
    时间片（timeslice）后就处于 RUNNING（运行） 状态。

     :fa-camera:   _Java将操作系统中的运行和就绪两个状态合并称为运行状态。阻塞状态是线程阻塞在例如synchronized关键字修饰的方法或代码块(获取锁)时的状态，但是阻塞在java.concurrent包中Lock接口的线程状态却是等待状态，因为java.concurrent包中Lock接口对于阻塞的实现均使用了LockSupport类中的相关方法。_ 
    
##### 5.Java中的异常处理

![Java中的异常](https://images.gitee.com/uploads/images/2019/1001/161103_c8b3c9c9_5028606.png "在这里输入图片标题")

Java语言中，所有的异常都有一个共同的祖先java.lang包中的 Throwable类。Throwable： 有两个重要的子类：Exception（异常） 和 Error（错误） ，二者都是 Java 异常处理的重要子类，各自都包含大量子类。

###### 5.1 Error（错误）

Error（错误）是程序无法处理的错误，表示运行应用程序中较严重问题。大多数错误与代码编写者执行的操作无关，而表示代码运行时 JVM（Java 虚拟机）出现的问题。例如，Java虚拟机运行错误（Virtual MachineError），当 JVM 不再有继续执行操作所需的内存资源时，将出现 OutOfMemoryError。这些异常发生时，Java虚拟机（JVM）一般会选择线程终止。

这些错误表示故障发生于虚拟机自身、或者发生在虚拟机试图执行应用时，如Java虚拟机运行错误（Virtual MachineError）、类定义错误（NoClassDefFoundError）等。这些错误是不可查的，因为它们在应用程序的控制和处理能力之 外，而且绝大多数是程序运行时不允许出现的状况。对于设计合理的应用程序来说，即使确实发生了错误，本质上也不应该试图去处理它所引起的异常状况。在 Java中，错误通过Error的子类描述。


###### 5.2 Exception（异常）

Exception（异常）是程序本身可以处理的异常。Exception 类有一个重要的子类 RuntimeException。RuntimeException 异常由Java虚拟机抛出。NullPointerException（要访问的变量没有引用任何对象时，抛出该异常）、ArithmeticException（算术运算异常，一个整数除以0时，抛出该异常）和 ArrayIndexOutOfBoundsException （下标越界异常）。

:fa-camera:  异常和错误的区别：异常能被程序本身处理，错误是无法处理。

###### 5.3 Throwable类常用方法

- public string getMessage():返回异常发生时的详细信息
- public string toString():返回异常发生时的简要描述
- public string getLocalizedMessage():返回异常对象的本地化信息。使用Throwable的子类覆盖这个方法，可以声称本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与getMessage（）返回的结果相同
- public void printStackTrace():在控制台上打印Throwable对象封装的异常信息


##### 6.IO

###### 6.1 Java中IO流的分类

- 按照流的流向分，可以分为输入流和输出流（输入和输出面向的主体程序）；
- 按照操作单元划分，可以划分为字节流和字符流；
- 按照流的角色划分为节点流和处理流。

Java Io流共涉及40多个类，这些类看上去很杂乱，但实际上很有规则，而且彼此之间存在非常紧密的联系， Java I0流的40多个类都是从如下4个抽象类基类中派生出来的。

- InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

按操作方式分类结构图：

![输入图片说明](https://images.gitee.com/uploads/images/2019/1001/162446_95e3a29e_5028606.png "在这里输入图片标题")

按操作对象分类结构图：


![输入图片说明](https://images.gitee.com/uploads/images/2019/1001/162536_27245fc5_5028606.png "在这里输入图片标题")


 :fa-video-camera: BIO、NIO、AIO 区别

-  **BIO (Blocking I/O)** : 同步阻塞I/O模式，数据的读取写入必须阻塞在一个线程内等待其完成。在活动连接数不是特别高（小于单机1000）的情况下，这种模型是比较不错的，可以让每一个连接专注于自己的 I/O 并且编程模型简单，也不用过多考虑系统的过载、限流等问题。线程池本身就是一个天然的漏斗，可以缓冲一些系统处理不了的连接或请求。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。

-  **NIO (New I/O)** : NIO是一种同步非阻塞的I/O模型，在Java 1.4 中引入了NIO框架，对应 java.nio 包，提供了 Channel , Selector，Buffer等抽象。NIO中的N可以理解为Non-blocking，不单纯是New。它支持面向缓冲的，基于通道的I/O操作方法。 NIO提供了与传统BIO模型中的 Socket 和 ServerSocket 相对应的 SocketChannel 和 ServerSocketChannel 两种不同的套接字通道实现,两种通道都支持阻塞和非阻塞两种模式。阻塞模式使用就像传统中的支持一样，比较简单，但是性能和可靠性都不好；非阻塞模式正好与之相反。对于低负载、低并发的应用程序，可以使用同步阻塞I/O来提升开发速率和更好的维护性；对于高负载、高并发的（网络）应用，应使用 NIO 的非阻塞模式来开发。


-  **AIO (Asynchronous I/O)** : AIO 也就是 NIO 2。在 Java 7 中引入了 NIO 的改进版 NIO 2,它是异步非阻塞的IO模型。异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。AIO 是异步IO的缩写，虽然 NIO 在网络操作中，提供了非阻塞的方法，但是 NIO 的 IO 行为还是同步的。对于 NIO 来说，我们的业务线程是在 IO 操作准备好时，得到通知，接着就由这个线程自行进行 IO 操作，IO操作本身是同步的。