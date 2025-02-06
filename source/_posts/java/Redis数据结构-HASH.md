---
title: Redis数据结构-Hash
date: '2019-12-18 11:51'
sticky: 0
keywords: reids 数据结构 Hash
categories:
  - 数据结构
  - reids
tags:
  - redis
  - 数据结构
  - Hash
abbrlink: 28527
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gxnpcy28gij31ve0p8dhy.jpg'
top_img:
description:
---

### 1、Redis的hash结构简介

Redis使用**一个全局Hash表来保存所有的键值对具体值的指针(\*key, \*value 不是数据本身)**，从而既满足应用存取Hash结构数据需求，又能提供快速查询功能。

一个哈希表，其实就是一个数组，数组的每个元素称为一个哈希桶。所以，我们常说，一个哈希表是由多个哈希桶组成的，每个哈希桶中保存了键值对数据。

![](https://tva1.sinaimg.cn/large/008i3skNly1gxnphlqdgfj31je0rc77g.jpg)

### 2、hash冲突

![](https://tva1.sinaimg.cn/large/008i3skNly1gxnpiy0hpfj30w80s676p.jpg)

Redis会对哈希表做rehash操作。rehash也就是增加现有的哈希桶数量，让逐渐增多的entry元素能在更多的桶之间分散保存，减少单个桶中的元素数量，从而减少单个桶中的冲突。那具体怎么做呢？

其实，为了使rehash操作更高效，Redis默认使用了两个全局哈希表：哈希表1和哈希表2。一开始，当你刚插入数据时，默认使用哈希表1，此时的哈希表2并没有被分配空间。随着数据逐步增多，Redis开始执行rehash，这个过程分为三步：

1. 给哈希表2分配更大的空间，例如是当前哈希表1大小的两倍；
2. 把哈希表1中的数据重新映射并拷贝到哈希表2中；
3. 释放哈希表1的空间。

这个过程看似简单，但是第二步涉及大量的数据拷贝，如果一次性把哈希表1中的数据都迁移完，会造成Redis线程阻塞，无法服务其他请求。此时，Redis就无法快速访问数据了。

### 3、渐进式rehash

#### 3.1背景(why)

Hash表在执行rehash时，由于Hash表空间扩大，原本映射到某一位置的键可能会被映射到一个新的位置上，因此，很多键就需要从原来的位置拷贝到新的位置。而在键拷贝时，由于Redis主线程无法执行其他请求，所以键拷贝会阻塞主线程，这样就会产生**rehash开销**

#### 3.2描述(what)

上述第二步拷贝数据时，Redis仍然正常处理客户端请求，每处理一个请求时，从哈希表1中的第一个索引位置开始，顺带着将这个索引位置上的所有entries拷贝到哈希表2中；等处理下一个请求时，再顺带拷贝哈希表1中的下一个索引位置的entries

![](https://tva1.sinaimg.cn/large/008i3skNly1gxnppmfhxgj31ee0u0aen.jpg)

具体到代码，它的过程是这样的：

1.  在字典中维持一个索引计数器变量 rehashidx，并将设置为 0，表示 rehash 开始。
2.  在 rehash 期间，客户端每次对字典进行 CRUD 操作时，会将 ht [0] 中 rehashidx 索引上的值 rehash 到 ht [1]，操作完成后 rehashidx+1。
3.  字典操作不断执行，最终在某个时间点，所有的键值对完成 rehash，这时将 rehashidx 设置为 - 1，表示 rehash 完成

#### 3.3 渐进式rehash触发条件(when)

- 服务器目前没有在执行BGSAVE命令或者BGREWRITEAOF命令，并且哈希表的负载因子大于或等于1。
- 服务端目前正在执行BGSAVE命令或者BGREWRITEAOF命令，并且哈希表的负载因子大于或等于5。
- 当哈希表的负载因子小于0.1时，redis会自动开始对哈希表进行缩容操作。

其中哈希表的负载因子可以通过公式：

\# 负载因子 = 哈希表以保存节点数量/哈希表大小 load_factor = ht[0].used / ht[0].size