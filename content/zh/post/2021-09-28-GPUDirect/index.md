---
title: GPUDirect笔记
summary: 本文章记录GPUDirect的学习

authors:

date: ""
publishDate: "2021-09-28T16:37:00+08:00"
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
slug: GPUDirect-cn

featured: false
headless: false
draft: true
private: false

tags:
- 加速器互连
- 论文笔记

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

GPUDirect实现的困难有两个：主机和其他外设不能直接访问GPU内存，以及目前GPU的编程模型中没有直接通信相关的原语。
GPUDirect RDMA的实现分为两种：一种使用GPU虚拟地址，另一种基于MMIO。

在没有GPUDirect的情况下，GPU-A要向GPU-B通信需要将数据写入主机内存中属于自己的bufferA中，主机将数据拷贝到属于GPU-B的bufferB中，再发给GPU-B。CUDA4开始引入GPUDirect，

cudaMalloc是在UVA空间中创建内存。

### **GPU地址管理**

### **MMIO**

## **参考资料**
[1] [Direct Communication Methods for Distributed GPUs](https://archiv.ub.uni-heidelberg.de/volltextserver/18623/1/Dissertation_LenaOden.pdf)

[2] [nvidia gpu中的Unified Memory](https://binbinmeng.wordpress.com/2018/10/19/nvidia-gpu%E4%B8%AD%E7%9A%84unified-memory/)