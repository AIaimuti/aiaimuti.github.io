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

# 计算机网络概述

局域网：覆盖范围小，自己买设备，带宽固定，自己维护，100米 带宽10M 100M 1000M（接入层设备和汇聚层设备是不一样的，计算机连交换机100M即可，交换机再往上连交换机就需要更大的带宽，如1000M，其网线或者光纤也需要支持1000M）<br>
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
![](https://img-blog.csdnimg.cn/20200102005246159.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

## 数据的返回：
发送端的数据时分块发送的，请求端和发送端都有缓存
例如：网页上的数据是一块块发送给请求端的，请求端成功接收后会发送反馈信息，此时发送端中的缓存可以删掉了。数据接收完成后，会拼接恢复网页。
![](https://img-blog.csdnimg.cn/20200102005316692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

## OSI参考模型
OSI参考模型共7层
应用层：所有能产生网络流量的程序
表示层：在传输之前是否进行加密 或 压缩 处理，二进制或ASCII码表示
会话层：查木马，看需求端和网站之间的连接
传输层：可靠传输，流量控制，不可靠传输(一个数据包即可，不需要建立会话，例如向DNS查询网站IP地址)
网络层—：负责选择最佳路径，规划IP地址(ipv4和ipv6变化只会影响网络层)
数据链路层：——帧的开始和结束，还有透明传输，差错校验(纠错由传输层解决)
物理层：定义网络设备接口标准，电气标准(电压)，如何在物理链路上传输的更快（网线，光纤）

IPv4和IPv6是网络层不同
应用层和网络层规划时没有分开，应用程序通过IP地址访问，当需要接入新服务器时，可以再添加IP地址，让应用层再访问新的IP地址，避免了改动程序。


## OSI参考模型对网络排错的意义：

每一层都为上一层提供服务，一旦某一层崩了，上面所有层都崩了，所以排查问题从最上层(物理层)排查；

1.物理层故障怎么办？
查看连接状态（如网线等），发送和接收的数据包数值；

2.数据链路层故障怎么办？
Mac地址冲突，2个一样的Mac地址同时访问；ADSL(非对称数字用户线路)欠费；两端的接口网速没有协商一致；计算机连接到其它VLAN(虚拟互联网)中。

3.网络层故障怎么办？
配置错误的IP地址，子网掩码，网关；数据有没有通过各个网关到达指定位置；

4.应用层(合并3层)故障怎么办？
应用程序配置问题
![](https://img-blog.csdnimg.cn/2020010200534545.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
## OSI参考模型和网络安全
1.物理层安全隐患？
别人能私自接入你的网络，应该拔掉不用的网线或接口；

2.数据链路层安全隐患？
ADSL账号密码，VLAN，交换机端口绑定Mac地址

3.网络层安全隐患？
路由器上使用ACL(访问控制列表)控制数据包流量；防火墙设置；

4.应用层安全隐患？
应用程序有没有漏洞；
