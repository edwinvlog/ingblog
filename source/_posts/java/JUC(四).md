---
title: JUC(四)
date: '2019-05-14 22:50'
sticky: 0
keywords: juc 多线程 线程池
categories:
  - juc
  - 多线程
  - 线程池
tags:
  - juc
  - 多线程
  - 线程池
abbrlink: 45571
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gybzistx9zj317t0u0tcg.jpg'
top_img:
description:
---

> 在学习面向对象OOP时，肯定听过一句话，java皆对象，要什么对象就new什么对象。
> 之后学习spring时，发现通过DI依赖注入后，便不再手动new对象了。
> 同理，在之前javase学习Thread时，手动new Thread，麻烦，另外gc也需要去不断收回，耗性能。
> 那么，我们能不能将所需要的线程提前创建好，并且可以反复利用呢？
> 线程池来了

## 1、简介

线程池的工作主要是控制运行线程的数量，处理过程中将任务放入队列，然后在线程创建后启动这些任务，如果线程数量超过了最大数量，超出数量的线程排队等待，等待其他线程执行完毕。

**优势-主要特点是：线程复用；控制最大并发数；管理线程**

- 降低资源消耗：通过复用已创建的线程降低线程的创建和销毁造成的消耗
- 提高响应速度：任务到达时，任务可以不需要等待线程创建，即可立即执行
- 提高线程的可管理性：线程是稀缺资源，如果无限制创建，会消耗系统资源，降低系统稳定性，使用线程池可以进行统一分配管理，调优和监控

## 2、线程池创建

| 线程池种类                                   | 说明                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| Executors.newCachedThreadPool()              | 一池多线程带缓存，执行很多短期异步的小程序或者负载较轻的服务器 |
| Executors.newFixedThreadPool()               | 一池固定线程，执行长期任务，性能好                           |
| Executors.newSingleThreadScheduledExecutor() | 一池一线程，一个任务一个任务执行的场景                       |

Executors这个类中，定义Executor、ExecutorService、ScheduledExecutorService、ThreadFactory和Callable类的工厂和实用方法

```java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                               new LinkedBlockingQueue<Runnable>());
}

public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}

public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                               new LinkedBlockingQueue<Runnable>()));
}
```

## 3、七个参数

![](https://tva1.sinaimg.cn/large/008i3skNly1gy8ik5qf23j31640u0gqh.jpg)

- maximumPoolSize：最大线程数 
- keepAliveTime：多余的空闲线程存活时间。当前线程池数量超过corePoolSize时，当空闲时间达到KeepAliveTime时，多余空闲线程会被销毁直到只剩下corePoolSize个线程为止
- unit：存活时间的单位 - workQueue：底层使用的阻塞队列，被提交但是还未执行的任务会保存在这里
- threadFactory：线程工厂，表示生成线程池中工作线程的线程工厂，用于创建线程，一般默认即可 
- handler：拒绝策略（有4种），当队列满了，并且工作线程大于等于线程池的最大线程数，应该如何处理多出来的线程

## 4、四种拒绝策略

![](https://tva1.sinaimg.cn/large/008i3skNly1gy8iqg3fctj31g60mqn1x.jpg)

- AbortPolicy（默认的拒绝策略）：直接抛出RejectedExecutionException异常阻止系统正常运行
- CallerRunsPolicy：“调用者运行”一种调节机制，该策略既不抛弃任务，也不会抛异常，而是将某些任务回退给调用者，从而降低新任务的流量
- DiscardOldestPolicy：抛弃队列中等待最久的任务，然后将当前任务加入队列中尝试再次提交
- DiscardPolicy：直接丢弃任务，不处理不抛异常。如果任务允许丢失，这是最好的一种