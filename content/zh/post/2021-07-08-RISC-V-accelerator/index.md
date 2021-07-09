---
title: 论文笔记：A RISC-V In-Network Accelerator for Flexible High-Performance Low-Power Packet Processing
summary: 论文笔记，将RISV-V核作为DPU处理单元的工作

authors:

date: ""
publishDate: "2021-07-09T16:08:00+08:00"
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
slug: PsPIN-cn

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

2017年ETH的Hoefler团队提出了一种基于RDMA的在网计算的编程模型sPIN，2021年此团队使用verilog实现了与此编程模型相对接的硬件PsPIN。

## 背景

在网计算（In-Network Computing）有四个方面的优势：