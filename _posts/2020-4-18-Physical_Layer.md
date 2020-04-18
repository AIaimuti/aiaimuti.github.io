---
layout:     post
title:      物理层
subtitle:   基础内容
date:       2020-4-18
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - 计算机网络
---

# 物理层
## 物理层定义的标准
物理层解决如何在链接各种计算机的传输媒体上传输数据比特流，而不是指具体的传输媒体。<br>
物理层的主要任务：确定传输媒体的接口的一些特性，包括，<br>
机械特性：接口形状，大小，引线数量<br>
电气特性：例如规定电压范围(-5V-5V)<br>
功能特性：例如规定-5V上0，+5V是1<br>
过程特性：也称规程特性，规定建立连接时各个相关部件的工作步骤

## 数据通信基础知识
![](https://img-blog.csdnimg.cn/20200102124548980.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
### 通信的目的是传送消息
数据(data)：运送消息的实体。<br>
信号(signal)：数据的电气或电磁的表现。<br>
->模拟信号：代表信息的参数的取值是连续的。<br>
->数字信号：代表信息的参数的取值是离散的。<br>
码元(code)：在使用时间域的波形表示数字信号时，代表不同离散数值的基本波形。<br>
->在数字通信中常常用时间间隔相同的符号表示一个二进制数字，这样的时间间隔内的信号称为二进制码元。<br>
->这个间隔称为码元长度，1 code可以携带n bit信息量(此时电压会有多个取值，例如1，2，…,7V，而不是只有2个取值)。
