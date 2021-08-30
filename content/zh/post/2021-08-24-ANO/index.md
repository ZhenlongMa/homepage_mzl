---
title: 智能网卡论文笔记：Autonomous NIC Offloads
summary: 论文笔记，越过传输层对应用层进行卸载的方法

authors:

date: ""
publishDate: "2021-08-24T10:08:00+08:00"
lastmod: ""

# View.
#   1 = List
#   2 = Compact
#   3 = Card
view: 2

comments: true
profile: true
share: false

gitment: true
slug: ANO

featured: false
headless: false
draft: false
private: false

tags:
- 智能网卡
- 论文笔记

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

<!-- <center><iframe width="560" height="315" src="https://www.youtube.com/embed/NM11SZu-ABk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center> -->



### [视频链接点此](https://www.youtube.com/watch?v=NM11SZu-ABk&ab_channel=BorisPismenny)

### [论文链接点此](https://dl.acm.org/doi/pdf/10.1145/3445814.3446732)

## **背景**

在计算机网络五层模型中，第五层协议（Layer 5 Protocol, L5P）包含了众多数据密集型计算，例如加解密、串/并行化、哈希计算和压缩/解压缩等，它们占用了大量的CPU计算时间。并且第五层协议内部也有依赖关系，比如HTTPS协议基于同是L5P的TLS协议，所以数据密集型计算影响绝大多数L5P的性能。如果能将L5P协议进行网卡卸载，一方面可以节省大量CPU资源，另一方面可以提升基于它们的应用整体的性能。

{{< figure src="cpu-clk.png" caption="**NVMe-TCP和TLS数据密集型操作占比**" numbered="true" height="50%" width="50%" >}}

应用层的正常运行需要依赖传输层，因为传输层能保证数据的顺序和可靠性。如图2中，如果忽视传输层，直接对数据包中的负载进行计算，那么就会导致对pkt1中的计算操作作用在pkt2中，导致严重的错误。

{{< figure src="L5P_dependence.jpg" caption="**L5P和传输层的关系**" numbered="true" height="75%" width="75%" >}}

所以如果要在网卡上进行L5P计算卸载，就必须首先将传输层完全卸载到网卡上。目前绝大多数应用都是构建在TCP/IP之上，使用操作系统提供的内核协议栈，因此需要在网卡上加入TCP卸载引擎（TCP Offload Engine, TOE），而一方面TOE逻辑很复杂，要消耗大量的网卡资源，另一方面Linux的内核开发者们出于安全性、灵活性、可扩展性等方面的考虑拒绝在内核中加入TOE的支持。

目前的应用层卸载工作采用了不同的方法避开TCP协议，例如采用RDMA（[KV-Direct](https://ring0.me/files/KV-Direct/kv-direct-paper.pdf)）、UDP（[LaKe](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=8641696)）或自定义传输层（[Catapult](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7106407)、[PANIC](https://www.usenix.org/system/files/osdi20-lin.pdf)）。但是有的应用层协议是和TCP紧密配合的，例如TLS，脱离TCP协议就完全无法工作。这篇论文提出了可以旁路传输层的应用层计算模式和对这种应用进行卸载的方法。

## **自主卸载**

论文称这种方法为自主卸载（Autonomous Offload）。能应用这种方法进行卸载的应用，它的计算模式需要[满足一定条件](#约束条件)；并且由于没有TCP保证数据包的顺序，自主卸载需要[正确地处理乱序到达的数据包](#卸载方法)。

### **约束条件**

可以使用自主卸载的应用需要满足四个条件：增量计算、数据量保持、有消息独立的状态以及有未加密的magic pattern。

{{< figure src="accumulative.png" caption="**递增计算**" numbered="true" height="50%" width="50%" >}}

增量计算的含义是消息块在进行计算时只能依靠前一级消息的计算结果，不能依靠后面或者前面好几级的消息。例如图中，data1进行计算时只能使用data0的计算结果，不能使用data2的计算结果；而data2也不能使用data0的计算结果。加这一条的限制是为了避免将整条消息都存储在网卡上，一条消息大小有可能达到GB级，网卡不可能缓存下来。

第二个限制是数据量保持，也就是说N个字节的数据计算完成后仍然是N个字节，否则有可能导致TCP包的数量发生改变，而内核协议栈对此完全无感。

每条消息进行计算时也要有消息独立的状态，也就是说一条消息进行计算的状态要只和当前状态有关，不能与之前和之后的消息相关。原文中这里提到的是要有恒定大小的状态，但我没有理解为什么要大小恒定。

此外，由于应用层有可能是加密运算，第四个约束就是要保证每个消息中有一部分未加密的部分，称为magic pattern，这一部分一般位于包头。这两个约束是为了在遇到乱序包的时候进行状态恢复。

### **TLS**

文中找到了几个符合上述条件的应用，文章重点放在其中的TLS上。

{{< figure src="tls_format.jpg" caption="**TLS数据格式**" numbered="true" height="50%" width="50%" >}}

TLS是一种给TCP数据包进行加密的协议。一个TLS消息格式如图4所示，初始向量是一个随机值，前面五个字段进行一系列认证计算后生成包尾ICV，接收方用作认证。

{{< figure src="tls_calc.png" caption="**TLS计算过程**" numbered="true" height="75%" width="75%" >}}

文章中采用的加密方法是AES-GCM方法。首先使用AES算法对初始向量进行运算生成16B kerstream，每一个keystream对16B的明文进行异或操作，生成16B的密文。第二个16B明文使用的初始向量是第一个16B的初始向量加一，以此类推。最后将全零用AES加密后的结果和包头以及所有密文做累乘得到ICV字段。可以看到这一计算过程是符合增量计算和数据量保持的。

### **卸载方法**

网卡上对每一条L5P消息都维护静态状态和动态状态，静态状态就是指密钥，动态状态包括期望的TCP包序列号、当前包在当前消息中的偏移、当前消息大小和当前消息的IV。

{{< figure src="in_seq.png" caption="**顺序计算过程**" numbered="true" height="75%" width="75%" >}}

顺序计算最简单，所有的数据包都是按顺序发送或接收，网卡在完成对一个TCP包的计算后就将动态状态更新，以进行下一个数据包的计算。

发送端的网卡遇到的另一种情况是重传。在这种情况下发送端的驱动程序识别出这是重传数据包并向L5P发送请求，读取重传数据包所在的状态并以一个特殊描述符的形式发送给网卡，网卡临时将状态恢复到对重传数据包的处理中。




## **参考文献**



