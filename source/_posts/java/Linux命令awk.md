---
title: Linux 命令awk
date: '2018-02-05 21:00:00'
sticky: 0
keywords: Linux awk
categories: Linux
tags: Linux
abbrlink: 64458
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gxorskiy2fj31hm0u00tq.jpg'
description:
---

#### 1 简介

    awk是一个报告生成器，它拥有强大的文本格式化的能力，这就是专业的说法。
    
    awk是由Alfred Aho 、Peter Weinberger 和 Brian Kernighan这三个人创造的，awk由这个三个人的姓氏的首个字母组成。
    
    awk早期是在unix上实现的，所以，我们现在在linux的所使用的awk其实是gawk，也就是GNU awk，简称为gawk，awk还有一个版本，New awk，简称为nawk，但是linux中最常用的还是gawk。


    awk其实是一门编程语言，它支持条件判断、数组、循环等功能。所以，我们也可以把awk理解成一个脚本语言解释器。


 grep 、sed、awk被称为linux中的"三剑客"。

  grep 更适合单纯的查找或匹配文本

  sed  更适合编辑匹配到的文本

 awk  更适合格式化文本，对文本进行较复杂格式处理

#### 2 awk命令使用

        awk [选项参数] 'script' var=value file(s)
        或
        awk [选项参数] -f scriptfile var=value file(s)

#####  选项参数说明：

 -  -F fs or --field-separator fs

     指定输入文件折分隔符，fs是一个字符串或者是一个正则表达式，如-F:。

-  -v var=value or --asign var=value
   
     赋值一个用户定义变量。
     



