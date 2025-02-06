---
title: Synchronized原理分析(一)
date: '2020-12-28 18:33:00'
sticky: 0
keywords: Synchronized
categories:
  - 多线程 锁
tags:
  - Synchronized 多线程 锁
abbrlink: 51789
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gyumy4gfwdj31dg0u075t.jpg'
description:
---

synchronized 对应的内存间交互操作为：lock 和 unlock，在虚拟机实现上对应的字节码指令为 **monitorenter 和 monitorexit**。

**synchronized 关键字底层原理属于 JVM 层面。**

## 1、synchronized同步语句块

```java
public class SynchronizedClass(){
		public void method(){
			synchronized(this){
				System.out.println("MY SYNCHRONIZED METHOD...")
			}
		}
}
```

通过 JDK 自带的 javap 命令查看 SynchronizedClass 类的相关**字节码**信息：首先切换到类的对应目录执行 **javac** SynchronizedClass.java 命令生成**编译后的 .class 文件**，然后执行 **javap -c -s -v -l SynchronizedClass.class** 反编译。

```java
public class javase.thread.SynchronizedClass {
  public javase.thread.SynchronizedClass();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."":()V
       4: return
  public void method();
    Code:
       0: aload_0
       1: dup
       2: astore_1
       3: monitorenter
       4: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
       7: ldc           #3                  // String synchronized 代码块
       9: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      12: aload_1
      13: monitorexit
      14: goto          22
      17: astore_2
      18: aload_1
      19: monitorexit
      20: aload_2
      21: athrow
      22: return
    Exception table:
       from    to  target type
           4    14    17   any
          17    20    17   any
}
```

从上面反编译结果可以看出：

**synchronized 同步语句块的实现使用的是 monitorenter 和 monitorexit 指令，其中 monitorenter 指令指向同步代码块的开始位置，monitorexit 指令则指明同步代码块的结束位置。** 当执行 monitorenter 指令时，线程试图**获取锁**也就是获取 **monitor**(monitor 对象存在于每个 Java 对象的**对象头**中，synchronized 锁便是通过这种方式获取锁的，也是为什么 **Java 中任意对象可以作为锁**的原因) 的持有权。当计数器为 0 则可以**成功获取**，获取后将**锁计数器**设为 1 也就是加 1。相应的在执行 **monitorexit** 指令后，将**锁计数器设为 0**，表明锁被释放。如果获取对象锁失败，那当前线程就要阻塞等待，直到锁被另外一个线程释放为止。如果有可重入的情况，锁计数器会持续**增加**。

## 2、synchronized 修饰方法

```java
public class SynchronizedDemo2 {
	public synchronized void method() {
		System.out.println("synchronized 方法");
	}
}
```

```java
{
  public javase.thread.SynchronizedDemo2();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."":()V
         4: return
      LineNumberTable:
        line 3: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   Ljavase/thread/SynchronizedDemo2;

  public static synchronized void method();
    descriptor: ()V
    flags: (0x0029) ACC_PUBLIC, ACC_STATIC, ACC_SYNCHRONIZED
    Code:
      stack=2, locals=0, args_size=0
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #3                  // String synchronized 方法
         5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 6: 0
        line 7: 8
}
```

synchronized 修饰的方法并没有 monitorenter 指令和 monitorexit 指令，取得代之的确实是 **ACC_SYNCHRONIZED** 标识，该标识指明了该方法是一个**同步方法**（注意：要出现这个标识，javap 指令必须加 -v 参数，不然显示不完全），JVM 通过该 ACC_SYNCHRONIZED 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。

总结：Java 虚拟机中的 synchronized 是基于进入和退出 monitor 对象实现的，同步分为**显式同步和隐式同步**，同步**代码块**代表着**显式同步**，指的是有明确的 monitorenter 和 monitorexit 指令。**同步方法**代表着**隐式同步**，同步方法是由**方法调用指令**读取运行时常量池中方法的 **ACC_SYNCHRONIZED** 标志来隐式实现的。