---
title: 智能网卡发展概述
summary: 本文介绍智能网卡、在网计算和DPU的发展历史
date: "2021-06-04T10:42:00+08:00"
publishDate: ""
lastmod: ""

view: 2

comments: true
profile: true
share: false

featured: false
headless: false
draft: true
private: false
---

# 背景

众所周知现在CPU性能的增长速度已经减慢，但是网络带宽还在持续高速增加。为了弥补计算-网络鸿沟，一种思路就是尽可能多地把数据处理功能卸载到网络设备上。也有众多网络设备厂商推出了含有各种卸载功能的网卡，称为智能网卡或者数据处理单元（Data Processing Unit, DPU）。

现在的网卡基本上都支持

所遇到的问题：由于不灵活导致的底层依赖让L5P不方便卸载。

Linux内核不支持TCP卸载，Windows也取消了这一支持。

问题在于哪些应该被卸载、应该用怎样的架构来做卸载。

在英伟达的世界观中，DPU是与CPU和GPU并列的第三个处理器，CPU代表通用处理，GPU代表并行处理，而DPU代表通信密集型处理。一个DPU包含一个多核CPU、一个高性能网络接口和一个可编程硬件加速引擎。DPU中搭载的CPU应该仅仅负责配置控制通路和处理例外[1]。

Fungible Nvidia Pensando Nebulon



# 参考资料
[1] https://blogs.nvidia.com/blog/2020/05/20/whats-a-dpu-data-processing-unit/