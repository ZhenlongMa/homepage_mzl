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



### [视频链接点此](https://www.youtube.com/watch?v=NM11SZu-ABk&t=43s&ab_channel=BorisPismenny)

### [论文链接点此](https://dl.acm.org/doi/pdf/10.1145/3445814.3446732)

## **背景**

在计算机网络五层模型中，第五层协议（Layer 5 Protocol, L5P）包含了众多数据密集型计算，例如加解密、串/并行化、哈希计算和压缩/解压缩等，它们占用了大量的CPU计算时间。并且第五层协议内部也有依赖关系，比如HTTPS协议基于同是L5P的TLS协议，所以数据密集型计算影响绝大多数L5P的性能。如果能将L5P协议进行网卡卸载，一方面可以节省大量CPU资源，另一方面可以提升基于它们的应用整体的性能。

{{< figure src="cpu-clk.png" caption="**NVMe-TCP和TLS数据密集型操作占比**" numbered="true" height="75%" width="75%" >}}

应用层的正常运行需要依赖传输层，因为传输层能保证数据的顺序和可靠性。如图2中，如果忽视传输层，直接对数据包中的负载进行计算，那么就会导致对pkt1中的计算操作作用在pkt2中，导致严重的错误。

{{< figure src="L5P_dependence.jpg" caption="**L5P和传输层的关系**" numbered="true" height="75%" width="75%" >}}

所以如果要在网卡上进行L5P计算卸载，就必须首先将传输层完全卸载到网卡上。目前绝大多数应用都是构建在TCP/IP之上，使用操作系统提供的内核协议栈，因此需要在网卡上加入TCP卸载引擎（TCP Offload Engine, TOE），而一方面TOE逻辑很复杂，要消耗大量的网卡资源，另一方面Linux的内核开发者们出于安全性、灵活性、可扩展性等方面的考虑拒绝在内核中加入TOE的支持。

目前的应用层卸载工作采用了不同的方法避开TCP协议，例如采用RDMA[1]、UDP[2]或自定义传输层[3][4]。但是有的应用层协议是和TCP紧密配合的，例如TLS，脱离TCP协议就完全无法工作。这篇论文提出了可以旁路传输层的应用层计算模式和对这种应用进行卸载的方法。

## **参考文献**

[1] Li, B., Ruan, Z., Xiao, W., Lu, Y., Xiong, Y., Putnam, A., Chen, E. and Zhang, L., 2017, October. Kv-direct: High-performance in-memory key-value store with programmable nic. In Proceedings of the 26th Symposium on Operating Systems Principles (pp. 137-152).

[2] Tokusashi, Y., Matsutani, H. and Zilberman, N., 2018, December. LaKe: the power of in-network computing. In 2018 International Conference on ReConFigurable Computing and FPGAs (ReConFig) (pp. 1-8). IEEE.

[3] Putnam, A., Caulfield, A.M., Chung, E.S., Chiou, D., Constantinides, K., Demme, J., Esmaeilzadeh, H., Fowers, J., Gopal, G.P., Gray, J. and Haselman, M., 2014, June. A reconfigurable fabric for accelerating large-scale datacenter services. In 2014 ACM/IEEE 41st International Symposium on Computer Architecture (ISCA) (pp. 13-24). IEEE.

[4] Lin, J., Patel, K., Stephens, B.E., Sivaraman, A. and Akella, A., 2020. {PANIC}: A High-Performance Programmable {NIC} for Multi-tenant Networks. In 14th {USENIX} Symposium on Operating Systems Design and Implementation ({OSDI} 20) (pp. 243-259).
