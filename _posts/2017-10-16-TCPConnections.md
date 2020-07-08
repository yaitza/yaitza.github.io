---
layout: post
category : 工具
title: TCP Connections Status
tagline: NetWork
tags: NetWork
---




# 1. 序言

&nbsp;&nbsp;&nbsp;&nbsp;最近工作进行性能测试时发现，对应VR设备在与服务端建立连接后，用netstat -ano | grep “Port”，发现在推送不同大小的数据时，分别出现了CLOSE_WAIT和FIN_WAIT的状态，于是就有了此文。此前一直知道有网络连接的几次握手，但是平时工作中没有去具体排查如此底层的东西，概念基本就忘得差不多了。知识也只有在平时使用中积累，然后再具体问题出现时进一步加深印象。

# 2. TCP连接
&nbsp;&nbsp;&nbsp;&nbsp;**TCP（Transmission Control Protocol 传输控制协议）**是一种面向连接的、可靠的、基于字节流的传输层通信协议，由IETF的RFC 793定义。在简化的计算机网络OSI模型中，它完成第四层传输层所指定的功能，用户数据报协议（UDP）是同一层内另一个重要的传输协议。在因特网协议族（Internet protocol suite）中，TCP层是位于IP层之上，应用层之下的中间层。不同主机的应用层之间经常需要可靠的、像管道一样的连接，但是IP层不提供这样的流机制，而是提供不可靠的包交换。  

&nbsp;&nbsp;&nbsp;&nbsp;应用层向TCP层发送用于网间传输的、用8位字节表示的数据流，然后TCP把数据流分区成适当长度的报文段（通常受该计算机连接的网络的数据链路层的最大传输单元（MTU）的限制）。之后TCP把结果包传给IP层，由它来通过网络将包传送给接收端实体的TCP层。TCP为了保证不发生丢包，就给每个包一个序号，同时序号也保证了传送到接收端实体的包的按序接收。然后接收端实体对已成功收到的包发回一个相应的确认（ACK）；如果发送端实体在合理的往返时延（RTT）内未收到确认，那么对应的数据包就被假设为已丢失将会被进行重传。TCP用一个校验和函数来检验数据是否有错误；在发送和接收时都要计算校验和。  

&nbsp;&nbsp;&nbsp;&nbsp;当我们向服务器发送HTTP请求，获取数据、修改信息时，都需要建立TCP连接，包括三次握手，四次分手。为实现数据的可靠传输，TCP要在应用进程间建立传输连接。它是在两个传输用户之间建立一种逻辑联系，使得通信双方都确认对方为自己的传输连接端点。  

## 2.1 建立连接  
&nbsp;&nbsp;&nbsp;&nbsp;建立连接前，服务器端首先被动打开其熟知的端口，对端口进行侦听。当客户端要和服务器端建立连接时，发起一个主动打开端口的请求（该端口一般为临时端口）；然后进入三次握手的过程。  

&nbsp;&nbsp;&nbsp;&nbsp;**Client连接Server**  

&nbsp;&nbsp;&nbsp;&nbsp;当Client端调用socket函数调用时，相当于Client端产生了一个处于Closed状态的套接字。  

&nbsp;&nbsp;&nbsp;&nbsp;(1）第一次握手：Client端又调用connect函数调用，系统为Client随机分配一个端口，连同传入connect中的参数(Server的IP和端口)，这就形成了一个连接四元组，客户端发送一个带SYN标志的TCP报文到服务器。这是三次握手过程中的报文1。connect调用让Client端的socket处于SYN_SENT状态，等待服务器确认；SYN：同步序列编号(Synchronize Sequence Numbers)。  

&nbsp;&nbsp;&nbsp;&nbsp;(2）第二次握手： 服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态。  

&nbsp;&nbsp;&nbsp;&nbsp;(3）第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户器和客务器进入ESTABLISHED状态，完成三次握手。连接已经可以进行读写操作。

&nbsp;&nbsp;&nbsp;&nbsp;一个完整的三次握手也就是：**请求---应答---再次确认**。  

## 2.2 释放连接  

![pic1](https://raw.githubusercontent.com/yaitza/yaitza.github.io/master/_posts/images/Tools/2017-10-16-TCPConnections-1.png)   

&nbsp;&nbsp;&nbsp;&nbsp;（1）客户端A发送一个FIN，用来关闭客户A到服务器B的数据传送。

&nbsp;&nbsp;&nbsp;&nbsp;（2）服务器B收到这个FIN，它发回一个ACK，确认序号为收到的序号加1。和SYN一样，一个FIN将占用一个序号。

&nbsp;&nbsp;&nbsp;&nbsp;（3）服务器B关闭与客户端A的连接，发送一个FIN给客户端A。

&nbsp;&nbsp;&nbsp;&nbsp;（4）客户端A发回ACK报文确认，并将确认序号设置为收到序号加1。

