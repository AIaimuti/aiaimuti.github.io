---
layout:     post
title:      应用层
subtitle:   基础内容
date:       2020-4-19
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - 计算机网络
---

## 6.1 域名协议DNS(Domain Name System)
### 6.1.1 作用
把域名解析为IP地址。例如打得开QQ(可以直接用IP地址登陆)，打不开网络可能是域名问题。<br>
8.8.8.8 是google的DNS服务器，222.222.222.222是电信的DNS服务器，能够用来帮忙解析域名地址。

### 6.1.2 域名
根 .<br>
顶级域名 com, edu, net,cn,org,gov<br>
二级域名 91xueit, inhe，例如91xueit.com是全球唯一的域名。<br>
三级域名 dba，例如www.dba.91xueit.com
![](https://img-blog.csdnimg.cn/20200105224546868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
Ping或nslookup能得到IP地址。或者直接输入网址。
![](https://img-blog.csdnimg.cn/20200105224559915.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

## 6.2 动态主机配置系统DHCP(同一个网段)
(1)静态IP地址
![](https://img-blog.csdnimg.cn/20200105224616347.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(2)动态IP地址
![](https://img-blog.csdnimg.cn/20200105224628673.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(3)DHCP客户端请求IP地址的过程<br>
我的理解这是一个动态IP地址的请求和分配过程。客户机在网中发广播请求地址，目标IP是4个255，目标MAC地址是全FF。DHCP收到后，会从地址池里选一个给客户机。如果网络中有2个DHCP服务端且都给了反馈，此时计算机还需要返回一个数据通知他选择了谁提供的地址。另一个DHCP客户端发现没要他的，于是收回他提供的。从这个角度看，网络中不能有太多的DHCP服务端。
![](https://img-blog.csdnimg.cn/20200105224643189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(4)DHCP服务器的安装<br>
必须是固定的IP地址。释放命令为ipconfig /release<br>
(5)DHCP跨网段配置IP地址<br>
需要DHCP配IP地址的计算机，需要在他所在的网络的路由器的网关上配置IPhelper address，再访问到DHCP。

## 6.3 文件传输协议FTP(File Transfer Protocol)
### 6.3.1 FTP使用的两个TCP连接
![](https://img-blog.csdnimg.cn/20200105224653853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
FTP的连接方式包括控制连接和数据连接。每上传/下载一个文件就会建立一个会话。<br>
控制连接：标准端口为21，用于发送FTP命令信息。<br>
数据连接：标准端口为20，用于上传/下载数据。

### 6.3.2 FTP数据连接(上图的TCP数据连接)
涉及到主动模式和被动模式<br>
(1)主动模式：FTP客户端告诉FTP服务器(的21端口)它在使用什么端口侦听，然后FTP服务器(的20端口)主动地和FTP客户端的这个端口建立连接。
![](https://img-blog.csdnimg.cn/20200105224711725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
(2)被动模式：FTP服务器端打开在指定范围内的某个新的端口(例如1089端口)并且进行侦听，被动地等待客户端发起连接。
![](https://img-blog.csdnimg.cn/2020010522472251.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
综上，在有防火墙的情况下，如果使用被动模式，防火墙不得不开很多端口来监听。因此应该使用主动模式来进行数据连接，并在防火墙开21和20端口。

### 6.3.3 FTP传输模式
文本模式：ASCII模式，以文本序列传输数据。<br>
二进制模式：Binary模式，以二进制序列传输数据。

## 6.4 远程终端协议telnet
默认使用TDP23端口。

## 6.5 远程桌面协议RDP(Remote Desktop Protocol)
## 6.6 超文本传输协议HTTP(Hyper Text Transfer Protocol)
通过这个协议访问网站。
### 6.6.1 HTTP和HTTPS的区别，以及HTTPS有什么缺点
HTTP协议和HTTPS协议区别如下：<br>
1）HTTP协议是以明文的方式在网络中传输数据，而HTTPS协议传输的数据则是经过TLS加密后的，HTTPS具有更高的安全性<br>
2）HTTPS在TCP三次握手阶段之后，还需要进行SSL 的handshake，协商加密使用的对称加密密钥<br>
3）HTTPS协议需要服务端申请证书，浏览器端安装对应的根证书<br>
4）HTTP协议端口是80，HTTPS协议端口是443

HTTPS优点：<br>
HTTPS传输数据过程中使用密钥进行加密，所以安全性更高<br>
HTTPS协议可以认证用户和服务器，确保数据发送到正确的用户和服务器

HTTPS缺点：<br>
HTTPS握手阶段延时较高：由于在进行HTTP会话之前还需要进行SSL握手，因此HTTPS协议握手阶段延时增加<br>
HTTPS部署成本高：一方面HTTPS协议需要使用证书来验证自身的安全性，所以需要购买CA证书；<br>
另一方面由于采用HTTPS协议需要进行加解密的计算，占用CPU资源较多，需要的服务器配置或数目高
### HTTP返回码
HTTP协议的响应报文由状态行、响应头部和响应包体组成，其响应状态码总体描述如下：
1xx：指示信息--表示请求已接收，继续处理。<br>
2xx：成功--表示请求已被成功接收、理解、接受。<br>
3xx：重定向--要完成请求必须进行更进一步的操作。<br>
4xx：客户端错误--请求有语法错误或请求无法实现。<br>
5xx：服务器端错误--服务器未能实现合法的请求。<br>

常见状态代码、状态描述的详细说明如下。
200 OK：客户端请求成功。<br>
206 partial content服务器已经正确处理部分GET请求，实现断点续传或同时分片下载，该请求必须包含Range请求头来指示客户端期望得到的范围<br>
300 multiple choices（可选重定向）:被请求的资源有一系列可供选择的反馈信息，由浏览器/用户自行选择其中一个。<br>
301  moved permanently（永久重定向）：该资源已被永久移动到新位置，将来任何对该资源的访问都要使用本响应返回的若干个URI之一。
302 move temporarily(临时重定向)：请求的资源现在临时从不同的URI中获得，<br>
304：not modified :如果客户端发送一个待条件的GET请求并且该请求以经被允许，而文档内容未被改变，则返回304,该响应不包含包体（即可直接使用缓存）。<br>
403 Forbidden：服务器收到请求，但是拒绝提供服务。

t Found：请求资源不存在，举个例子：输入了错误的URL。

### 6.6.1 万维网www(World Wide Web)
(1)www提供分布式服务
![](https://img-blog.csdnimg.cn/20200105224801732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)

(2)统一资源定位系统URL(Uniform Resource Locator)
有点像地址。
![](https://img-blog.csdnimg.cn/20200105224813275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
网站的标识有多种方式，不同的端口，不同的IP地址，不同的域名(主机头)来区分。

### 6.6.2 使用web代理服务器访问网站
内网其它主机不能通过路由访问Internet，只能由web代理(在同一个网段里)访问外网，然后内网其它主机访问这个web代理来实现上网功能。如果web代理在外网，反而速度更慢。<br>
(1)节省内网访问Internet的带宽(便于访问内容变化不大的网站)。<br>
(2)绕过防火墙访问外网。例如访问某个禁止的网站A，但是假如通过在与A同一个网络中的web代理，就可以访问该网站。<br>
(3)通过web代理避免被跟踪。

## 6.7 电子邮件(SMTP, POP3, IMAP)
SMTP发, POP3和IMAP是收。
![](https://img-blog.csdnimg.cn/20200105224823180.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l3YW5kZXJ1,size_16,color_FFFFFF,t_70)
参考文献
https://www.bilibili.com/video/av9876107?p=7<br>
https://blog.csdn.net/iwanderu/article/details/103812980

