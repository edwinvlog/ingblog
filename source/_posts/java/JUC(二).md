---
title: JUC(二)
date: '2019-05-14 18:51'
sticky: 0
keywords: juc 多线程 volatitle CAS
categories:
  - juc
  - 多线程
  - CAS
tags:
  - juc
  - 多线程
  - CAS
abbrlink: 46173
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gy1kbxpz2fj31i90u0wj7.jpg'
top_img:
description:
---

## 1、CAS

### 1.1、概述

> 结合JMM线程的工作内存，主内存，副本之类的知识点。CAS主要对副本写回主内存进行了限定要求。
> 主存有一个变量为num，其值为5，称之为A，A指代主存num的值
> Thread1从主内存拷贝副本到自己的工作内存，此时的副本称之为B，B指代副本num = 5
> Thread1将num修改为2019，此时称之为C，C指代副本num = 2019
> Thread1将修改过的num写回主内存时，做如下事情：
> 将B与A进行比较，如果相等，说明在我Thread1一顿猛操作的期间，没有第二个线程动过num，所以我Thread1可以大胆的将新num，也就是C，写回内存；
>
> 如果不相等，说明在我Thread1操作期间，有人先我一步动过了num，把它给改变了，那我之前的一顿操作都是在旧数据的基础上干的，其实就是白干了，所以无法写回内存。

```java
public static void demo1() {
    // 主内存的值
    AtomicInteger atomicInteger = new AtomicInteger(5);
    // 修改成功返回true
    atomicInteger.compareAndSet(5, 2019);
    // 结果是2019
    System.out.println(atomicInteger.get());
    // 修改失败返回false
    atomicInteger.compareAndSet(5, 1024);
    // 结果是2019
    System.out.println(atomicInteger.get());
}
// 源代码
public final boolean compareAndSet(int expect, int update) {
    // this：当前对象
    // valueOffset：内存偏移量
    // expect：期望值，也就是上述的B
    // update：修改后的值，也就是上述的C
    return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
}
```

CAS是一条CPU并发原语，功能是判断内存某个位置的值是否为预期值，是则更新，这个过程是原子的。
CAS并发原语体现在Java语言中就是sun.misc.Unsafe类中的各个方法。调用Unsafe类中的CAS方法，JVM会帮我们实现出CAS汇编指令。这是一种完全依赖于硬件的功能，通过它实现了原子操作。再次强调，由于CAS是一种系统原语，原语属于OS用语范畴，是由若干条指令组成，用语完成某个功能的一个 过程，并且原语的执行必须是连续不能被中断的，所以CAS不会造成数据不一致问题。

## 2、Unsafe类

CAS的核心类，由于Java方法无法直接访问底层系统，需要通过本地（native）方法来访问，Unsafe相当于一个后门，基于该类可以直接操作特定内存的数据。Unsafe类存在于sum.misc包中，其内部方法操作可以像C的指针一样直接操作内存。Java中CAS操作依赖于Unsafe类中的方法。

注意UnSafe类中的方法都是native修饰，也就是说Unsafe类中的方法都直接调用OS底层资源执行响应任务

## 3、自旋锁

```java
// Unsafe类中自旋锁的示例
@HotSpotIntrinsicCandidate
public final int getAndSetInt(Object o, long offset, int newValue) {
    int v;
    do {
        // 从内存偏移量offset的位置获取对象o的数据，相当于副本
        v = getIntVolatile(o, offset);
        // 如果期望值与主存中的真实值不同会一直卡在这，自旋
    } while (!weakCompareAndSetInt(o, offset, v, newValue));
    return v;
}

@HotSpotIntrinsicCandidate
public native int getIntVolatile(Object o, long offset);

@HotSpotIntrinsicCandidate
public final boolean weakCompareAndSetInt(Object o, long offset, int expected, int x) {
  return compareAndSetInt(o, offset, expected, x);
}

@HotSpotIntrinsicCandidate
public final native boolean compareAndSetInt(Object o, long offset, int expected, int x);
```

> @HotSpotIntrinsicCandidate JDK9开始出现的注解，该注解表示此方法在HotSpot虚拟机中**可能**另有更加高效的实现

## 4、CAS的问题

- 一直卡while，CPU负担不小
- ABA问题
- CAS针对一个共享变量，多个不行，只能上锁

## 5、ABA问题

在前面概述中，读完后其实会有一个问题：期望值与主存中的值一致，就一定能够说明没有其他线程动过这个数据吗？
这是不能保证的，Thread1的期望值是A，它再写回主存之前，先比较主存的值是否是A。在比较之前，完全可以有另外的线程对该变量一通操作，最后重新改为A，Thread1就会误认为期间无其他线程动过数据。
这就是所谓的ABA问题

**如何解决ABA问题？**

时间戳原子引用，在比较期望值的同时，也会比较时间戳（版本号），类比数据库中的乐观锁。每次修改数据，数据的版本号都会发生变化，某一线程修改数据，不仅期望值要求一样，期望的版本号也要一致。这样就解决了ABA问题

















