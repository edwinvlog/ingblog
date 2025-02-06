---
title: Collection集合
date: '2019-09-10 18:22'
sticky: 0
keywords: java基础 collection
categories:
  - java基础
  - collection
tags:
  - collection
  - java基础
abbrlink: 6832
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gxov4kyyjdj31cq0u0780.jpg'
top_img:
description:
---

## 1、Java 为什么要有集合？

首先，java 是一门面向对象语言，操作对象是我们的日常。既然操作就需要有东西把对象存储起来。于是容器就应运而生，初学者接触到的第一个容器就是数组，但这远远不够，根据不同的对象以及不同的业务，我们需要用到不同的容器。比如，不想要重复对象，我们就会想到用 set 容器，想要对象有序我们会用 List 。不管是 List、Set。他们都会有共性， 而 java 就根据这些共性，给我们提供了 Collection 集合。

## 2、Collection接口框架图

![](https://tva1.sinaimg.cn/large/008i3skNly1gxov7dsbymj31im0u0di3.jpg)

由上面的 Collection 接口框架图，我们可以知道 Collection 是 List、Set、Queue 的父接口，看到这里，你们可能会问，Map 哪去了？其实，Java 中的容器，包括 Collection 和 Map ，Map 是另外一个体系。

## 3、Collection的方法

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gxov906kmlj30mg0tmmzy.jpg" style="zoom:40%;" />

Collection 接口定义了以上待实现的方法。比如：

1. size() 计算容器长度

2. isEmpty() 是否为空

3. contains() 是否包含某个对象

4. containsAll() 是否包含另一个集合的所有对象

5. iterator() 上层接口 iterable 的方法，用于生成迭代对象，遍历对象

6. add() 添加一个对象

7. add() 添加另一个集合的所有对象

8. remove() 移除一个对象

9. removeAll() 移除所有对象

10. toArray() 把集合转换成数组

11. retainAll() 是否与另一个集合有交集





































