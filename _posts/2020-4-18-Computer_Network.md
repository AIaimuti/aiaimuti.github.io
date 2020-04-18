---
layout:     post
title:      计算机网络概述
subtitle:   基础内容
date:       2020-4-18
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - 计算机网络
---

## 计算机网络概述

局域网：覆盖范围小，自己买设备，带宽固定，自己维护，100米 带宽10M 100M 1000M（接入层设备和汇聚层设备是不一样的）<br>
广域网：距离远 花钱租带宽<br>
Internet：有ISP，ISP是电信运营商internet service producer，有自己的机房，对网民提供对Internet的连接<br>

全球计算机IP地址同一规划，出厂时有MAC地址全球<br>
计算机网卡有MAC地址，即物理地址；计算机有IP地址

## 网站的访问
输入网址（如 http://XX.com/ ），通过DNS解析，获取对应网址对应IP地址，然后需要从本地向网址对应的IP地址发送请求<br>
数据包：数据+请求端的IP地址+目标网站IP地址；<br>
数据帧：数据包 + 原mac地址 + 目标mac地址<br>
通过不断变化mac地址，来访问不同的路由器，最终到达所需访问的地址。<br>
物理地址决定了下一个路由器的去处，IP地址决定了最终的目的地  
![](https://blog.csdn.net/iwanderu/article/details/103795715)

## 数据的返回：
网页上的数据是一块块发送给请求端的，成功接收后会发送反馈信息，此时链路中的缓存可以删掉了。数据接收完成后，会拼接恢复网页。
![](https://blog.csdn.net/iwanderu/article/details/103795715)
