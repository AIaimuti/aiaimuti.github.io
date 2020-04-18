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
OSI参考模型共7层<br>
应用层：所有能产生网络流量的程序<br>
表示层：在传输之前是否进行加密 或 压缩 处理，二进制或ASCII码表示<br>
会话层：查木马，看需求端和网站之间的连接<br>
传输层：可靠传输，流量控制，不可靠传输(一个数据包即可，不需要建立会话，例如向DNS查询网站IP地址)<br>
网络层—：负责选择最佳路径，规划IP地址(ipv4和ipv6变化只会影响网络层)<br>
数据链路层：——帧的开始和结束，还有透明传输，差错校验(纠错由传输层解决)<br>
物理层：定义网络设备接口标准，电气标准(电压)，如何在物理链路上传输的更快（网线，光纤）

IPv4和IPv6是网络层不同<br><br>
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

封装过程：就像包装快递，上面写上寄件人和收件人地址<br>
![](https://img-blog.csdnimg.cn/20200102005413482.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
解封装过程：就像拆快递，把没有得东西去除，得到物品；数据链路层校验就像，拆快递验货，如果快递内容和快递包装上标识得不一致，就拒收<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/Computer_Network/decode.jpg)
通信过程：1.我们只关心快递能不能到。2.快递运送的方式就像数据运送过程，各种各样<br>
![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/Computer_Network/communication.jpg)

## 计算机网络的性能
1.速率<br>
连接在计算机网络上的主机在数字信道上传送数据位数的速率，也称data rate或bit rate(比特率)，单位是b/s, kb/s, Mb/s, Gb/s。<br>
和正常理解的网速的关系是除以8.

2.带宽<br>
数据通信领域中，数字信道所能传送的最高数据率，单位是b/s, kb/s, Mb/s, Gb/s。常见的是Mpbs。

3.吞吐量<br>
在单位时间内通过某个网络的数据量，单位是b/s, Mb/s。

4.时延<br>
包括发送时延，传播时延，处理时延，排队时延。<br>
发送时延：从数据开始到数据结束用的时间，等于数据块长度(bit)除以信道带宽(bit/s). <br>
传播时延：数据在网线上走的时间。<br>
排队时延：可能有多个数据到达接收点，当前数据的接收需要等待前面的数据接收完，这个等待的时间为排队时延。<br>
另外，数据接收后继续发送时，当前需要发送的数据需要等待前面的数据发送完，这个等待的时间也为排队时延。<br>
处理时延：数据接收后，选择往哪个方向发送，是处理时延。<br>
更快的发送速度意味着波长越短，链路上的数据量更大；更快的传播速度意味着在网线中更快的传播速度。

5.时延X带宽(时延带宽积)<br>
有多少数据正在线路上。

6.往返时间(RTT, Round-Trip Time)<br>
从发送方发送数据开始，到发送方收到接收方确认数据的时间。例如ping一下。

7.利用率
包括信道利用率：有数据通过的时间/总时间<br>
网络利用率：信道利用率的加权平均<br>
网络当前时延D = 网络空闲时时延D0 / (1 - 信道利用率U)

## 参考来源
https://www.bilibili.com/video/av9876107?p=7<br>
https://blog.csdn.net/iwanderu/article/details/103795715
