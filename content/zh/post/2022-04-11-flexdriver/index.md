---
title: FlexDriver总结
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
draft: true
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

{{< figure src="hw.jpg" caption="**FLD架构图**" numbered="true" height="75%" width="75%" >}}

## **逻辑切分**

网卡驱动以及网络协议栈非常复杂，如果在加速器侧完全实现会占用大量片上面积，所以FLD对驱动逻辑进行了切分，对性能影响严重的数据平面放在加速器上，而逻辑复杂且不严重影响性能的部分仍然放在主机侧，前者包括队列管理、数据buffer分配，而后者包括队列初始化、连接管理。

## **数据压缩**

在数据存储方面，
