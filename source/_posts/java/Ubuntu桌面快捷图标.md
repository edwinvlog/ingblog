---
title: Ubuntu桌面快捷图标
date: '2018-02-05'
sticky: 0
keywords: Ubuntu桌面快捷图标
categories:
  - 点滴
  - ubuntu
  - Linux
tags:
  - Ubuntu桌面快捷图标
abbrlink: 25850
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gxorva4rikj31bz0u075w.jpg'
description:
---

#### 新建文件
```
    touch idea.desktop
```



####  写入下面内容
```
[Desktop Entry]

Name=IntelliJ IDEA

Comment=IntelliJ IDEA

//idea 启动命令路径
Exec=/home/edwin/work/work/soft/idea/bin/idea.sh 

//idea 图标路径
Icon=/home/edwin/work/work/soft/idea/bin/idea.png

Terminal=false

Type=Application

Categories=Developer;
```