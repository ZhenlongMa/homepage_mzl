---
title: FlexDriver总结（草稿）
summary: 本文为论文总结

authors:

date: ""
publishDate: "2022-04-11T09:35:00+08:00"
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
slug: flexdriver-cn

featured: false
headless: false
draft: false
private: false

tags:
- 加速器互连
- 智能网卡

categories:
- 论文笔记

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

加速器互连和在网计算共同面对的问题是加速单元和网络设备之间如何高效交互。为了降低CPU的开销以及提高性能，文章计划用加速器控制网卡，即加速器实现网卡驱动，因此给项目起名叫FlexDriver，简称FLD。NVidia在本文中针对此问题提出将网卡的控制逻辑植入加速器中，并在逻辑切分、数据结构压缩和软件抽象三个方面做了优化工作。

{{< figure src="sw.jpg" caption="**FLD软件架构图**" numbered="true" height="75%" width="75%" >}}

## **逻辑切分**

网卡驱动以及网络协议栈非常复杂，如果在加速器侧完全实现会占用大量片上面积，所以FLD对驱动逻辑进行了切分，对性能影响严重的数据平面放在加速器上，而逻辑复杂且不严重影响性能的部分仍然放在主机侧，前者包括队列管理、数据buffer分配，而后者包括队列初始化、连接管理。

## **内存设计**

FLD为了避免与其他外设以及加速器应用竞争内存带宽的问题，将通信控制相关的数据存储在FLD专用的片上内存中。经计算，如果直接将这些内容存储在片上，需要85.3MB空间，可以说是十分巨大的开销了，相比之下Innova-2提供的片上存储空间也才10.05MB。
 
 {{< figure src="hw.jpg" caption="**FLD硬件架构图**" numbered="true" height="75%" width="75%" >}}

 因此文中提出用压缩的形式存储这些数据，在需要使用时进行实时恢复。

 为了减少描述符ring buffer的大小，FLD采用哈希表存储描述符，将PCIe端传来的读写请求映射为一系列描述符。