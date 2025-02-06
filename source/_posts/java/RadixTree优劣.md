---
title: RadixTree优劣
date: '2019-12-15 13:51'
sticky: 0
keywords: RadixTree 数据结构
categories:
  - 数据结构
  - RadixTree
tags:
  - RadixTree
  - 数据结构
abbrlink: 5043
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gyk2dch74jj31c60u0k0v.jpg'
top_img:
description:
---

## RadixTree优势

- 本质上是前缀树，所以存储有「公共前缀」的数据时，比 B+ 树、跳表节省内存
- 没有公共前缀的数据项，压缩存储，value 用 listpack 存储，也可以节省内存
- 查询复杂度是 O(K)，只与「目标长度」有关，与总数据量无关
- 这种数据结构也经常用在搜索引擎提示、文字自动补全等场景

## RadixTree不足

- 如果数据集公共前缀较少，会导致内存占用多
- 增删节点需要处理其它节点的「分裂、合并」，跳表只需调整前后指针即可
- B+ 树、跳表范围查询友好，直接遍历链表即可，Radix Tree 需遍历树结构
- 实现难度高比 B+ 树、跳表复杂

---

Redis中 Stream 在存消息时，推荐使用默认自动生成的「时间戳+序号」作为消息 ID，不建议自己指定消息 ID，这样才能发挥 Radix Tree 公共前缀的优势。

每种数据结构都是在面对不同问题场景下，才被设计出来的，结合各自场景中的数据特点，使用优势最大的数据结构才是正解。