---
title: JVM配置参数
date: '2019-12-28 18:33:00'
sticky: 0
keywords: JVM配置参数
categories:
  - JVM配置参数
tags:
  - JVM配置参数
abbrlink: 16463
cover: 'https://tva1.sinaimg.cn/large/008i3skNly1gyum1wlqwcj31cc0u0457.jpg'
description:
---

**机器配置：8核16G**

### 1、启动参数

```java
VM Arguments: jvm_args: 
-Dfile.encoding=UTF-8 
-Dsun.jnu.encoding=UTF-8 
-Djava.io.tmpdir=/tmp 
-Djava.net.preferIPv6Addresses=false 
-Xss512k -Xmx12g -Xms12g 
-XX:MetaspaceSize=512m 
-XX:MaxMetaspaceSize=512m 
-XX:+AlwaysPreTouch 
-XX:ReservedCodeCacheSize=240m 
-XX:+HeapDumpOnOutOfMemoryError 
-XX:+UseG1GC 
-XX:G1HeapRegionSize=4M 
-XX:InitiatingHeapOccupancyPercent=40 
-XX:MaxGCPauseMillis=100 
-XX:+TieredCompilation 
-XX:CICompilerCount=4 
-XX:-UseBiasedLocking 
-XX:+PrintGCDetails 
-XX:+PrintGCDateStamps 
-XX:+PrintGCTimeStamps 
-XX:+PrintAdaptiveSizePolicy 
-XX:+PrintGCApplicationStoppedTime 
-XX:+PrintHeapAtGC 
-XX:+PrintStringTableStatistics 
-XX:+PrintTenuringDistribution 
-Xloggc:/opt/logs/项目名称gc.log 
-XX:+UseGCLogFileRotation 
-XX:NumberOfGCLogFiles=30 
-XX:GCLogFileSize=50M 
-XX:+PrintFlagsFinal 
-XX:ErrorFile=/opt/logs/项目名称/vmerr.log 
-XX:HeapDumpPath=/opt/logs/项目名称/HeapDump 

java_command: ./项目.jar java_class_path (initial): ./项目.jar Launcher Type: SUN_STANDARD
```

### 2、运行时参数

```java
-XX:+AlwaysPreTouch 
-XX:CICompilerCount=4 
-XX:ConcGCThreads=2 
-XX:ErrorFile=/opt/logs/项目名称/vmerr.log 
-XX:G1HeapRegionSize=4194304 
-XX:GCLogFileSize=52428800 
-XX:+HeapDumpOnOutOfMemoryError 
-XX:HeapDumpPath=/opt/logs/项目名称/HeapDump 
-XX:InitialHeapSize=12884901888 
-XX:InitiatingHeapOccupancyPercent=40 
-XX:MarkStackSize=4194304 
-XX:MaxGCPauseMillis=100 
-XX:MaxHeapSize=12884901888 
-XX:MaxMetaspaceSize=536870912 
-XX:MaxNewSize=7730102272 
-XX:MetaspaceSize=536870912 
-XX:MinHeapDeltaBytes=4194304 
-XX:NumberOfGCLogFiles=30 
-XX:+PrintAdaptiveSizePolicy 
-XX:+PrintFlagsFinal 
-XX:+PrintGC 
-XX:+PrintGCApplicationStoppedTime 
-XX:+PrintGCDateStamps 
-XX:+PrintGCDetails 
-XX:+PrintGCTimeStamps 
-XX:+PrintHeapAtGC 
-XX:+PrintStringTableStatistics 
-XX:+PrintTenuringDistribution 
-XX:ReservedCodeCacheSize=251658240 
-XX:ThreadStackSize=512 
-XX:+TieredCompilation 
-XX:-UseBiasedLocking 
-XX:+UseCompressedClassPointers 
-XX:+UseCompressedOops 
-XX:+UseFastUnorderedTimeStamps 
-XX:+UseG1GC 
-XX:+UseGCLogFileRotation
```