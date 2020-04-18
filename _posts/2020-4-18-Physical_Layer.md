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

# 2 物理层
## 2.1 物理层定义的标准
物理层解决如何在链接各种计算机的传输媒体上传输数据比特流，而不是指具体的传输媒体。<br>
物理层的主要任务：确定传输媒体的接口的一些特性，包括，<br>
机械特性：接口形状，大小，引线数量<br>
电气特性：例如规定电压范围(-5V-5V)<br>
功能特性：例如规定-5V上0，+5V是1<br>
过程特性：也称规程特性，规定建立连接时各个相关部件的工作步骤

## 2.2 数据通信基础知识
![](https://img-blog.csdnimg.cn/20200102124548980.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
### 2.2.1 通信的目的是传送消息
数据(data)：运送消息的实体。<br>
信号(signal)：数据的电气或电磁的表现。<br>
->模拟信号：代表信息的参数的取值是连续的。<br>
->数字信号：代表信息的参数的取值是离散的。<br>
码元(code)：在使用时间域的波形表示数字信号时，代表不同离散数值的基本波形。<br>
->在数字通信中常常用时间间隔相同的符号表示一个二进制数字，这样的时间间隔内的信号称为二进制码元。<br>
->这个间隔称为码元长度，1 code可以携带n bit信息量(此时电压会有多个取值，例如1，2，…,7V，此时携带3Bit信息量)。
### 2.2.2 信道的概念
信道一般表示向一个方向传输信息的媒体。所以通信线路往往包含一条发送信息的信道和一条接收信息的信道。<br>
->单向通信(单工通信)——只能有一个方向的通信，没有反向交互。<br>
->双向交替通信(半双工通信)——通信的双方都可以发送信息，但是不能双方同时发送或接收。<br>
->双向同时通信(全双工通信)——通信的双发可以同时发送和接收。
### 2.2.3 什么是基带(baseband)和带通(base pass)信号？
基带信号(基本频率信号)——来自信源的信号，例如计算机输出的代表各种文字或图像文件的数据信号都属于基带信号。基带信号就是发出的直接表达了要传输的信息的信号，比如说我们说话的声波。<br>
带通信号——把基带信号经过载波调制后，把信号的频率范围迁移到较高的频段以便在信道中传输(即仅在一段频率范围内能够通过信道)。<br>
因此在传输距离较近时，采用基带传输方式(衰减不大，信号内容不会变化)。传输距离较远时，采用带通传输方式，例如从计算机到监视器，打印机等外设的信号。
![](https://img-blog.csdnimg.cn/20200102124938625.png)
### 2.2.4 几种基本的调制方法
从基带信号到带通信号，涉及到调幅(AM)，调频(FM)，调相(PM)。
![](https://img-blog.csdnimg.cn/20200102125052514.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
### 2.2.5 常用编码方式
单极性不归零编码：只使用一个电压值，高电平表示1，低电平表示0.
双极性不归零编码：用幅值相等的正负电平表示二进制数1和0.
单极性归零编码：发送码1时高电平在整个码元期间只持续一段时间，其余时间返回零电平。
双极性归零编码：正负零三个电平，信号本事携带同步信息。
![](https://img-blog.csdnimg.cn/202001021251409.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

曼彻斯特编码：单极性编码的缺点是没有办法区分此时是没有信号，还是有信号，但是信号是0.
->这种编码方式是bit中间有信号，低-高跳转表示0，高-低跳转表示1，一个时钟周期只可以表示一个bit，并且必须通过两次采样才能得到一个bit。它能携带时钟信号，而且能区分此时是没有信号还是信号为0.
![](https://img-blog.csdnimg.cn/20200102125200692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

差分曼彻斯特编码：抗干扰能力比曼彻斯特编码更强。bit与bit之间有信号跳变，表示下一个bit为0，bit与bit之间没有信号跳变，表示下一个bit为1。
![](https://img-blog.csdnimg.cn/20200102125343549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

### 2.2.6 信道极限容量
信道极限容量取决于失真的大小，当失真过大时，接收信号无法还原发送信号。
![](https://img-blog.csdnimg.cn/2020010212540244.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

### 2.2.7 奈氏(Nyquist)准则
在理想条件下(无噪声/干扰/失真)，为了避免码间串扰，码元的传输速率是有上限的。如果信道的频带越宽(信号高频分量越多)，则用更高的速率传输码元也不会出现码间串扰。

### 2.2.8 信噪比和香农(Shannon)公式
信道的极限信息传输速率C(带宽受限且有Gauss白噪声干扰时的信道极限且无差错时的信息传输速率)可表示为，
C = W log2(1 + S / N) b/s
->W是信道的带宽(Hz)；
->S是信道内所传信号的平均功率；
->N是信道内的Gauss噪声的功率。
可以发现，减少速度和增大功率能提高准确度。
信道的带宽或信道中的信噪比(S/N)越大，则信息的极限传输速率C就越高；
只要信息传输速率小于信道的极限传输速率C，就一定能实现某种无差错传输；
若带宽W或信噪比(S/N)没有上限，则极限传输速率C也没有上限(虽然不可能)；
实际上，信道能达到的最高传输速率要比C低不少；

### 2.2.9 奈氏(Nyquist)准则和香农公式的应用范围
![](https://img-blog.csdnimg.cn/20200102125508584.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
## 2.3 物理层下面的传输介质
### 2.3.1 导向传输媒体
导向传输媒体中，电磁波沿着固体媒体传播。
双绞线：
->屏蔽/非屏蔽双绞线(STP/UTP)
![](https://img-blog.csdnimg.cn/2020010212553584.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
同轴电缆：
->50欧姆同轴电缆(基带同轴电缆)——用于数字传输，多用于基带传输；
->75欧姆同轴电缆(宽带同轴电缆)——用于模拟传输，多用于带通传输；
![](https://img-blog.csdnimg.cn/20200102125556959.png)

网线：
->直通线——双绞线夹线顺序两端一致(1白橙2橙3白绿4蓝5白蓝6绿7白棕8棕，口诀“橙白橙/绿白蓝/蓝白绿/棕白棕”)，这是100M的标准(568B)，也称正线/标准线/直通线。当用于10M和100M时，下图只用了1，3，2，6线。对于1000M的网络，8根线都会用上。
![](https://img-blog.csdnimg.cn/20200102125618389.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

直通线应用最广，这种类型的以太网电缆用来实现以下连接：主机到交换机/集线器，路由器到交换机/集线器。
->交叉线——一般不同设备连接用直通线，同类设备用交叉线。也用于集线器到交换机，路由器到主机连接。
![](https://img-blog.csdnimg.cn/20200102125705746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
注意，现在网卡能够自动协商，所以交叉线和直通线已经无所谓了，连错了也没关系。

光纤：
![](https://img-blog.csdnimg.cn/2020010212572797.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
单模光纤——只能传输一种电磁波；直径小；用于有线电视网络，传播特性好，带宽可达10GHz，可以在一根光纤中传输60套PAL-D电视节目。
![](https://img-blog.csdnimg.cn/20200102125745955.png)
多模光纤——能传输多种电磁波；直径大；
![](https://img-blog.csdnimg.cn/20200102125755384.png)
### 2.3.2 非导向传输媒体
非导向传输媒体指自由空间，其中的电磁波传输称为无线传输。
无线传输的频段很广。
短波通信主要是靠电离层的反射，但短波信道的通信质量较差；
微波在空间主要是直线传播(延迟大)——地面微波接力通信；微信通信。
电信领域使用的电磁波的频谱如下，
![](https://img-blog.csdnimg.cn/20200102125822870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
### 2.3.3 物理层设备
集线器：
工作特点是在网络中只起到信号放大和重发作用，目的是扩大网络的传输范围，而不具备信号的定向传送能力。最大传输距离是100m；集线器是一个大的冲突域(意思是某个时间点只能是2台设备进行通信)。现在很少用集线器了，它不安全，一般用交换机。
![](https://img-blog.csdnimg.cn/2020010212590985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
