---
layout:     post
title:      传输层
subtitle:   基础内容
date:       2020-4-19
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - 计算机网络
---

## 5.1 OSI和DoD模型
下图必须背下来。尤其是传输层和网络层的协议。
![](https://img-blog.csdnimg.cn/20200104182238490.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
传输层最大数据包是65535字节，而网络层数据最大只有1480字节。所以需要分段，但是只要分段，就有可能丢包，因为网络层不负责可靠传输。所以要求服务器和客户端保持会话，直到数据传输完成。

->TCP(Transmission Control Protocol)传输控制协议<br>
应用场景：需要将要传输的文件分段传输时；就需要TCP协议来建立会话实现可靠传输；同时也有流量控制功能。(例如QQ传文件)<br>
查看会话 netstat -n<br>
查看建立会话的进程 netstat -nb

->UDP(User Data Protocol)用户数据报协议<br><br>
应用场景：一个数据包就能完成数据通信；不需要建立会话和流量控制；多播/广播；是一种不可靠传输。(例如QQ聊天，屏幕广播)

## 5.2 传输层协议和应用层协议的关系

(1)TCP和UDP协议和不同的端口即可对应一个应用层的协议。注意，53大部分是与UDP相连。<br>
(2)熟知数值一般为0-1023，登记端口号数值1024-49151，客户端口号数值为49152-65535.<br>
(3)常用的应用层协议使用的端口(号)：<br>
http = TCP + 80<br>
Https = TCP + 443<br>
RDP = TCP + 3389<br>
ftp = TCP + 21<br>
共享文件夹 = TCP + 445<br>
SMTP = TCP + 25<br>
POP3 = TCP + 110<br>
telnet = TCP + 23<br>
SQL = TCP + 1433<br>
DNS = UDP + 53<br>
**(注意与4.6 的协议号的区别)**

## 5.3 服务和应用层协议的关系
服务使用TCP或UDP的端口侦听客户端请求；<br>
客户端使用IP地址定位服务器，使用目标端口，定位服务；<br>
可以在服务器网卡上设置只开放必要的端口，实现服务器网络安全。<br>
防火墙是基于网卡的，只打开必要的端口，不必要的端口不允许接收数据，不影响服务的运行和监听。

## 5.3.1 如何在Windows上安装服务
DNS服务<br>
Web服务<br>y
邮件发送SMTP<br>
邮件接受POP3

### 5.3.2 如何查看服务侦听的端口
netstat -a<br>
netstat -an 以数字的形式查看端口<br>
netstat -n 查看建立的会话<br>
netstat -nb 查看建立会话的进程<br>
telnet 192.168.80.100 3389 测试到远程计算机某个端口是否打开


### 5.3.3 如何更改服务使用默认端口
可以迷惑病毒，使系统更加安全。

### 5.3.4 如何设置Windows网络安全
设置本地连接 TCP/IP筛选

## 5.4 传输层功能和端口范围
![](https://img-blog.csdnimg.cn/20200104182348169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

### 5.4.1 传输层协议和网络层协议的主要区别
网络层实现如何把数据包从这个地址(服务器)发送到另一个地址(服务器)。<br>
传输层实现如何让这个应用程序找到对应计算机的应用程序(相对应的应用程序实现逻辑通信)。
![](https://img-blog.csdnimg.cn/20200104182407430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

### 5.4.2 传输层的主要功能
(1)传输层为应用进程之间提供了端到端的逻辑通信(但网络层是为主机之间提供逻辑通信)。<br>
(2)传输层还要对收到的报文进行差错检验。<br>
(3)传输层提供面向连接(TCP)和无连接(UDP)的服务。

### 5.4.3 传输层的端口
![](https://img-blog.csdnimg.cn/20200104182425652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(1)TCP的端口<br>
端口用一个16位端口号进行标志。<br>
端口号只具有本地意义，即端口号只是为了标志本计算机应用层的各进程。在Internet中不同计算机的相同端口号是没有联系的(最好不要有冲突)。

## 5.5 UDP协议
(1)UDP是无连接的，即发送数据之前不需要建立连接。<br>
(2)UDP使用尽最大努力交付，即不保证可靠交付，同时也不使用拥塞控制。<br>
(3)UDP是面向报文的，适合多媒体通信的要求。<br>
(4)UDP支持一对一，一对多，多对一，多对多交互通信。<br>
(5)UDP首部开销小，只有8个字节。
![](https://img-blog.csdnimg.cn/20200104182437891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

### 5.5.1 UDP的首部格式
![](https://img-blog.csdnimg.cn/20200104182453304.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
首部中的长度指的是UDP用户数据报的长度(首部+数据)。<br>
伪首部用于检验和，我的理解是伪首部是IP数据包首部的后部分。
![](https://img-blog.csdnimg.cn/20200104182503206.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

## 5.6 TCP协议
(1)TCP是面向连接的传输层协议。(三次握手)<br>
(2)每一条TCP连接智能有两个端点(endpoint)，每一条TCP连接只能时点对点的(一对一)。<br>
(3)TCP提供可靠交付的服务。(确保不丢包)<br>
(4)TCP提供全双工通信。(因为需要接收端的反馈，例如如果接收端处理不过来，可让发送端慢一点，流量控制)<br>
(5)面向字节流。
![](https://img-blog.csdnimg.cn/20200104182520945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
如果要传输一个比较大的数据，首先一次只会传输一小块，这个数据块的大小是没有规律的。加上数据包数据帧的头，发送给接收端，接收端去掉首部，再次拼接。

### 5.6.1 TCP的连接
(1)TCP把连接作为最基本的抽象。<br>
(2)每一条TCP连接有两个端点。<br>
(3)TCP连接的端点不是主机，不是主机的IP地址，不是应用程序，也不是传输层协议端口，TCP连接的端点叫套接字(socket).<br>
->套接字socket = (IP地址:端口号)<br>
->每一条TCP连接唯一地被通信两端的两个套接字所确定，即：<br>
->TCP连接 ::= {socket1, socket2} = {(IP1:port1), (IP2:port2)}<br>
(4)端口号拼接到IP地址即构成了套接字。

## 5.7 TCP如何实现可靠传输
(1)可靠传输的工作原理——停止等待协议。
![](https://img-blog.csdnimg.cn/20200104182536235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
->在发送完一个分组后，必须暂时保留已发送的分组的副本。<br>
->分组和确认分组都必须进行编号。<br>
->超时计时器的重传时间应当比数据在分组传输的平均往返时间更长一些。<br>
(2)确认丢失和确认迟到
![](https://img-blog.csdnimg.cn/20200104182551725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(3)可靠通信的实现<br>
->使用上述的确认和重传机制，微秒就可以在不可靠的传输网络上实现可靠的通信。<br>
->这种可靠传输的协议常称为自动重传请求ARQ(Automatic Repeat reQuest)。<br>
->ARQ表明重传的请求是自动进行的。接收方不需要请求发送方重传某个出错的分组。<br>
->缺点，信道利用率低。
![](https://img-blog.csdnimg.cn/202001041825590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
->信道利用率U
![](https://img-blog.csdnimg.cn/20200104182617681.png)
(4)流水线传输(发送方)<br>
发送方可连续发送多个分组，不必每发完一个分组就停顿下来等待对方的确认。由于信道上一直有数据不间断的传送，这种传输方式可获得很高的信道利用率。
![](https://img-blog.csdnimg.cn/20200104182630295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(5)连续ARQ协议
![](https://img-blog.csdnimg.cn/20200104182635697.png)
如果1确认收到了，则滑动窗口。
![](https://img-blog.csdnimg.cn/20200104182647647.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
如果12收到了，3没有收到，则滑动窗口会会回溯到3位置，重新发送。<br>
(6)累计确认(接收方)<br>
接收方一般采用累计确认的方式。<br>
优点：容易实现，信道利用率高。<br>
缺点：不能向发送方反映出接收方已经正确收到的所有分组的信息。

## 5.8 TCP报文段的首部格式
![](https://img-blog.csdnimg.cn/20200104182702877.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(1)源端口：2个字节16位。<br>
(2)目的端口：2个字节16位。<br>
(3)序号：当前数据的第一个字节在整个文件中的序号。<br>
(4)确认号ack：接收端发送，提示发送端下一次该发的数据在整个文件中的序号。接收端收到后，会把这个序号之前的数据从缓存中删掉。<br>
(5)数据偏移：当前TCP报文段第多少个字节后是TCP的数据部分了。数据偏移最多表示1111，即15，他最多可以表示15乘以4，即60个字节的偏移量，所以选项+填充最多只能是40个字节。<br>
(6)保留：6位，无作用。<br>
(7)URG：urgent，意思是优先级高，发送端优先发送，而不是在缓存中排队。<br>
(8)ACK：acknowledge，1意味着确认建立了会话。<br>
(9)PSH：1意味着接收端优先读取，而不是在缓存中排队。<br>
(10)RST：reset，1意味着TCP会话出现严重错误，必须释放和重新连接。<br>
(11)SYN：同步。1意味着要发起会话。 SYN攻击，就是采用大量ip地址对目标ip请求对话。<br>
(12)FIN：finish，1意味着释放连接。<br>
(13)窗口：接收端先发，发送端根据接收端的窗口尺寸确定发送端窗口尺寸。<br>
(14)检验和：除了将UDP中的协议号17换成1，其他与UDP一样<br>
(15)紧急指针：只有URG为1才有用，紧急指针指明了需要紧急处理的数据部分。

### 5.8.1 抓包分析P64
(1)
![](https://img-blog.csdnimg.cn/2020010418271350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
第一步，ARP，建立可靠传输<br>
第二步，UDP(DNS同时占用UDP和TCP的53端口)，域名解析<br>
第三步，TCP，识别网关MAC地址

(2)cmd打开控制台如下，当前是建立了2个会话。
![](https://img-blog.csdnimg.cn/20200104182730288.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(3)浅蓝：请求的数据包；深蓝：得到的结果；<br>
192是我方地址；8是服务器地址；
![](https://img-blog.csdnimg.cn/20200104182741822.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

(4)两个SYN是双方确认建立了会话，MSS意思是最大数据包1460字节，web端(219.148.36.48)最多能缓存win=64240字节，我端(192.168.80.63)最多能缓存win=65535字节,如果某一方发了另一方win字节个数据对方都没有确认，那么就暂停。标记为TCP这三行(12,13,14)，不光是建立对话，还协商了一些参数。
![](https://img-blog.csdnimg.cn/20200104182755319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(5)第15行，开始正式通信，HTTP。GET的意思是我要访问这个网站了。白框内写着各个层的数据首部的结构。
![](https://img-blog.csdnimg.cn/20200104182804430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
目标端口Src Port是80，源端口Dst Port是1057，序号Sep是1，确认号Ack是1，数据部分长度是1-203字节。
![](https://img-blog.csdnimg.cn/20200104182816908.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(6)219->192是发送网页信息，192->219是确认反馈。注意，16第一次发送数据和19反馈接收是没有数据len=0。
![](https://img-blog.csdnimg.cn/20200104182826925.png)
从建立会话，到传输数据到确认反馈的一个过程如下，15-19。
![](https://img-blog.csdnimg.cn/2020010418283931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

## 5.9 TCP如何实现可靠传输P67
### 5.9.1 以字节为单位的滑动窗口技术
A的发送窗口是由B的接受窗口长度决定的。<br>
在没有收到B确认收到之前，A不能删掉滑动窗口内的内容。<br>
A可以持续给B发送，直到A的滑动窗口内数据都发了。
![](https://img-blog.csdnimg.cn/20200104182910778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
B收到后给A发确认收到的反馈ACK，序号是下一个应该发送的字节的序号，A收到后，就可以滑动窗口到对应的位置。例如B反馈ACK是7，那么A的滑窗可以移动到7位置，1-6删除。21-26可以发送。
![](https://img-blog.csdnimg.cn/20200104182934629.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
B收到1-6之后，也开始滑窗，B的应用程序可以读取1-6的数据。B的滑窗继续接收。
![](https://img-blog.csdnimg.cn/2020010418294182.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
以上是正常状态下的情况。如果出现丢失情况，例如7-9丢失，此时B反馈的ACK=7.因为10-12收到了，因此B发送SACK(选择性确认)，A只发送7-9.
![](https://img-blog.csdnimg.cn/20200104182958855.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

### 5.9.2 超时重传时间的确定
TCP美发送一个报文段，就对这个报文段设置一次计时器。只要计时器设置的重传时间到了，但是还没有收到数据，那么就重传这一报文段。<br>
RTTs(new) = (1 - alpha) x (RTTs(old)) + alpha x (new RTT样本)<br>
超时重传时间应略大于上面得出的加权平均往返时间RTTs。alpha推荐值是0.125.<br>
这个公式的目的是根据网速和带宽的实时情况调整往返时间。

## 5.10 TCP如何实现流量控制P68
解决通信两端处理时间不一样的问题。通过实时调整滑窗尺寸的大小(尺寸甚至可以是0)来实现流量控制。接收端主动调整滑窗大小，发送端根据接收端发送的报文调整相应的滑窗。发送端也会定时发送报文向接收端确认滑窗信息，避免接收端发送的相关调整滑窗大小的报文丢失带来的影响。

## 5.11 TCP如何避免网络拥塞
(1)出现资源拥塞的条件：对资源需求的总和>可用资源。<br>
(2)拥塞控制是一个全局性的过程，涉及到所有的主机，所有的路由区，以及与降低网络传输性能有关的所有因素。<br>
(3)流量控制往往指在给定的发送端和接收端之间的点对点通信量的控制，它所要做的就是抑制发送端发送数据的速率，以便使接收端来得及接收。

### 5.11.1 拥塞控制起到的作用
红线和绿线之间是数据丢失内容。
![](https://img-blog.csdnimg.cn/20200104183007298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
### 5.11.2 慢开始和拥塞避免
(1)发送方维持 拥塞窗口cwnd(congestion window)<br>
(2)发送方控制拥塞窗口的原则是：<br>
只要网络没有出现拥塞，拥塞窗口就再增大一些，以便把更多的分组发送出去；<br>
只要网络出现拥塞，拥塞窗口就减少一些，以减少注入到网络中的分组数。<br>
(3)慢开始算法的原理
![](https://img-blog.csdnimg.cn/20200104183026234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(4)设置慢开始门限状态变量ssthresh<br>
慢开始门限状态变量ssthresh的用法如下：<br>
当cwnd<ssthresh时，使用慢开始算法；<br>
当cwnd>ssthresh时，停止使用慢开始算法，改用拥塞避免算法；<br>
当cwnd=ssthresh时，使用慢开始算法或拥塞避免算法均可；<br>
(5)拥塞避免算法的思路<br>
让拥塞窗口cwnd缓慢地增大，即每经过一个往返时间RTT就把发送方的拥塞窗口cwnd加1，而不是加倍，使拥塞窗口cwnd按线性规律缓慢增长。<br>
(6)当网络出现拥塞时对策<br>
无论是在慢开始阶段还是在拥塞避免阶段，只要发送方判断网络出现拥塞(其根据就是没有按时收到确认)，就要把慢开始门限ssthresh设置为出现拥塞时的发送方窗口值的一半(但是不能小于2)。<br>
然后把拥塞窗口cwnd重新设置为1，执行慢开始算法。<br>
这样做的目的就是要迅速减少主机发送到网络中的分组数，使得发生拥塞的路由器有足够的时间吧队列中积压的分组处理完毕。<br>
(7)慢开始和拥塞避免算法的实现举例
![](https://img-blog.csdnimg.cn/20200104183041722.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
->拥塞避免并非指完全能够避免拥塞。利用以上的措施要完全避免网络拥塞还是不可能的。<br>
->拥塞避免是说在拥塞避免阶段吧拥塞避免窗口控制为按线性规律增长，使网络比较不容易出现拥塞。

### 5.11.3 快重传和快恢复
快重传算法首先要求接收方每收到一个失序的报文段后就立即发出重复确认，这样做可以让发送方及早知道有报文段没有到达接收方。<br>
当发送端收到连续三个重复的确认时，就执行“乘法减少”算法，即把慢开始门限ssthresh减半，但拥塞窗口cwnd现在不设置为1，而是设置为慢开始门限ssthresh减半后的数值，然后开始执行拥塞避免算法(“加法增大”)，使拥塞窗口缓慢地线性增大。
![](https://img-blog.csdnimg.cn/20200104183058971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

### 5.11.4 发送窗口的实际上限制
取接收方窗口和 拥塞窗口 这两个变量中的较小值。<br>
发送窗口的上限制 = min {rwnd, cwnd}.

## 5.12 TCP传输连接管理
传输连接有三个阶段，即：连接建立，数据传送，连接释放。<br>
TCP连接的建立都是采用客户服务器方式。<br>
主动发起连接建立的应用进程叫做客户(client)。<br>
被动等待连接建立的应用进程叫做服务器(server)。
![](https://img-blog.csdnimg.cn/20200104183113214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
头两次握手除了确定双方都能联通外，还通知了双方的一些端口信息。<br>
第三次握手原因：假如把三次握手改成仅需要两次握手，死锁是可能发生的。作为例子，考虑计算机A和B之间的通信，假定A给B发送一个连接请求分组，B收到了这个分组，并发送了确认应答分组。按照两次握手的协定，B认为连接已经成功地建立了，可以开始发送数据分组。可是，B的应答分组在传输中被丢失的情况下，A将不知道B是否已准备好，A认为连接还未建立成功，将忽略B发来的任何数据分组，这样就形成了死锁。
![](https://img-blog.csdnimg.cn/2020010418312417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
### 5.12.1 TCP连接时三次握手各状态
![](https://img-blog.csdn.net/20180628211256486?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llcWlhbmcxOTkxMDQxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
A(Client客户端):Close-->SYNSENT-->ESTABLISHED<br>
B(Server服务器端)：Close-->LISTEN-->SYNSRCVD-->ESTABLISHED<br>
A-->SYN = 1, seq = x<br>
B-->SYN = 1, ACK = 1, seq = y, ack= x + 1(等于A发送的seq值 + 1)<br>
A-->ACK = 1, seq = x + 1, ack = y + 1(等于B发送的seq值 + 1)
TCP连接时三次握手各状态
### 5.12.2 TCP连接释放
![](https://img-blog.csdn.net/20180628211331451?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llcWlhbmcxOTkxMDQxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![](https://img-blog.csdn.net/20180628211929536?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llcWlhbmcxOTkxMDQxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![](https://img-blog.csdn.net/20180628212000450?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llcWlhbmcxOTkxMDQxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![](https://img-blog.csdn.net/20180628212043623?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llcWlhbmcxOTkxMDQxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![](https://img-blog.csdn.net/20180628212108966?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3llcWlhbmcxOTkxMDQxMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
TCP连接释放: 四次挥手
1. A想要关闭连接, 就发送报文。 发送FIN=1, seq=u(这个U等于前一个报文的最后一个字节+1)。
A-->FIN=1, SEQ=U
2. 这时A进入等待结束状态, B 收到释放连接的报文后, 给予确认, 发送ACK=1,ack=u+1(回应A，seq值 + 1), seq=v。
B-->ACK=1,seq=v,ack=u+1
3. 这时, A处TCP处于FIN-WAIT-1状态, 因为是A提出关闭的, 所以A就没有数据传给B, 但是如果B有数据传给A, A一定要接收。
4. A收到B的确认报文, 整个TCP处于FIN-WAIT-2状态, 就剩下最后一步,等待B也发送释放连接的请求。
5. B发送释放链接报文。 发送FIN=1(结束标识), ACK=1, ack=u+1(回应A，seq值 + 1), SEQ=W。
B-->FIN=1,ACK=1,seq=w,ack=u+1
6. 当A收到B的请求释放连接报文后, 发送ACK=1, ack=w+1(回应B，seq值 + 1),SEQ=U+1，向B进行确认。 发了之后, 进入"时间等待" 状态.  经过时间等待计时器设置的2MSL后, 才算是完全释放了TCP连接。
A-->ACK=1,seq=w+1,ack=u+1
等待计时器是为了防止B服务端没有接受到A的回复而一直开启，这样当B再次发送释放链接报文时，A还可以回复。
7. 除了时间等待计时器, 还有一个保活计时器, 如果客户端突然出现故障, 为了不让服务器端白白等下去, 就设计这个计时器; 一段时间之后，发送一个探测报文段, 若连续发10个都没有回应, 就自动断开连接把。  


参考来源
https://www.bilibili.com/video/av9876107?p=7<br>
https://blog.csdn.net/iwanderu/article/details/103812974    


