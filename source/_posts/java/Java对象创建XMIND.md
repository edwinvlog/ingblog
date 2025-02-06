---
title: Java对象创建XMind
date: '2019-10-15 21:00:00'
sticky: 0
keywords: java对象创建
categories:
  - java对象创建
tags:
  - java对象创建
abbrlink: 24306
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gz2qbsgh2gj31hn0u0dnd.jpg'
description:
---

# Java对象的创建

![](https://tva1.sinaimg.cn/large/008i3skNly1gz2qeql9pyj31az0u00xk.jpg)

## 创建

## 对象内存分配

### 方式

- 空闲列表

	- 不规整空间

		- CMS

- 指针碰撞

	- 连续规整空间

		- Serial ParNew

			- 简单高效

				- 带压缩算法的GC收集器使用

- CMS分配方式

	- Linear Allocation Buffer 空闲列表拿到一大块分配缓冲区之后，在它里面仍然可以使用指 针碰撞方式来分配

### 并发安全问题

- 分配动作通过CAS + 失败重试
- 预分配 本地线程分配缓冲(TLAB)， 不够在使用CAS

	- -XX:+/-UseTLAB

##  对象的内存布局

### 对象头

- 存储对象自身的运行时数据

	- 哈希吗
	- GC分代年龄
	- 锁状态
	- 线程持有的锁
	- 偏向线程ID
	- 偏向时间戳

- 类型指针(对象指向它的类型元数据的指针)--不是必须的

	- 作用：确定对象是那个类的实例

### 实例数据

- 各种类型的字段内容(包含从父类中继承的)

	- 存储顺序会 受到虚拟机分配策略参数(-XX:FieldsAllocationStyle参数)和字段在Java源码中定义顺序的影响

		- HotSpot虚拟机默认的分配顺序为longs/doubles、ints、shorts/chars、bytes/booleans、oops(Ordinary Object Pointers，OOPs)

			- 相同宽度的字段总是被分配到一起存 放，在满足这个前提条件的情况下，在父类中定义的变量会出现在子类之前。如果HotSpot虚拟机的 +XX:CompactFields参数值为true(默认就为true)，那子类之中较窄的变量也允许插入父类变量的空 隙之中，以节省出一点点空间

### 对齐填充(非必须)

- 作用：占位符

	- 原由：HotSpot虚拟机的自动内存管理系统要求对象起始地址必须是8字节的整数倍，换句话说就是 任何对象的大小都必须是8字节的整数倍。对象头部分已经被精心设计成正好是8字节的倍数(1倍或者 2倍)，因此，如果对象实例数据部分没有对齐的话，就需要通过对齐填充来补全

## 对象的访问(使用方式)

### 通过reference数据操作堆上的对象

- 句柄

	- 在堆上划分出一块内存作为句柄池，reference中存储的就是对象的句柄地址

		- 句柄中包含对象实例数据与类型数据的具体地址

- 直接指针

	- reference中存储的直接是对象的地址

- 优劣

	- 句柄

		- reference中存储的是稳定句柄地址，在对象被移动(垃圾收集时移动对象是非常普遍的行为)时只会改变句柄中的实例数据指针，而 reference本身不需要被修改

	- 直接指针

		- 速度更快，它节省了一次指针定位的时间开销
		- 对象移动后，reference需要更新