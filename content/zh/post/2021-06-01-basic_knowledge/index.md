---
title: 基础知识
summary: 此处记录日常学习工作中积累的基础背景知识
date: ""
publishDate: "2021-06-01T14:33:0011:34:00+08:00"
lastmod: ""

view: 2

comments: true
profile: true
share: false

featured: false
headless: false
draft: false
private: false
---

Virtual Protocol Interconnect：Mellanox提出的一种架构，在VPI网卡或交换机上同一个端口上能在IB和以太网语义之间切换。

目前RDMA广泛使用的实现方式有三种：InfiniBand，RoCE，iWARP。这三种实现方式底层的物理层和链路层不同，但向上层提供了相同的API，即verbs