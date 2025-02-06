---
title: Mongo 创建索引语句
date: '2018 -02-05 21:00:00'
sticky: 0
keywords: mongo  索引
categories:
  - 点滴
  - mongo
tags:
  - mongo
  - 索引
abbrlink: 22330
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gxortiy2d2j318e0mudhg.jpg'
description:
---

#### 1 单个索引
```
db.tf_kd_bus_online_data.createIndex(
{"req_dts":1} , {background:true}
);

```
#### 2  复合索引

```
db.tf_kd_bus_online_data.createIndex(
{ "interface_code":1, "req_dts": 1, "resp_dts": 1, "status":1}
);


```

