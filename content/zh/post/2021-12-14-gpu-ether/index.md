---
title: 论文笔记：GPU-Ether
summary: 本文为论文笔记

authors:

date: ""
publishDate: "2021-12-14T17:17:00+08:00"
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
slug: GPU-Ether-cn

featured: false
headless: false
draft: false
private: false

tags:
- 加速器互连

categories:
- 论文笔记

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

AI应用越来越广泛，其对算力的需求越来越高，因此GPU之间的高效互连是一个很重要的研究方向。以往对GPU的通信优化都需要专用网卡硬件的支持，例如HCA、SNIC或者FPGA等，要在大规模数据中心实现这些并不现实，本文实现了一种仅在软件层面进行修改从而让GPU能通过P2P直接与商用以太网卡进行连接的方法。

