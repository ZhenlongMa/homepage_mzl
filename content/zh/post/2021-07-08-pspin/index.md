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
draft: true
private: false

tags:
- 智能网卡
- 论文笔记

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

2017年ETH的Hoefler团队提出了一种基于RDMA的在网计算的编程模型[sPIN](https://classes.cs.uoregon.edu/18S/cis631/Documents/spin.pdf)，2021年此团队使用verilog实现了与此编程模型相对接的硬件[PsPIN](https://arxiv.org/pdf/2010.03536.pdf)，这项工作目前尚未发表，仍然收录在Arxiv中。

## **背景**

在网计算（In-Network Computing）有四个方面的优势：减轻CPU负担、降低延迟、提高吞吐量以及减少资源利用。

目前在网计算的工作可以由四个维度进行衡量：位置、可编程性、计算粒度和可用性。位置可以分为计算逻辑在网络设备的数据路径上、在网络设备但不在数据路径上以及在主机上。在可编程性方面，有的工作支持的自定义操作包括读取数据包头和负载、访问网卡和主机的内存以及发起新的网络请求，而有的工作仅支持预定义的有限操作。这些操作的粒度分为整条消息或单个数据包。可用性是指注册自定义操作是否需要更高级别的权限、服务中断等。下图为几种在网计算工作在这四个维度上的总结。

{{< figure src="four-dimension.jpg" caption="**几种在网计算工作总结**" numbered="true" height="100%" width="100%" >}}

## **sPIN**

sPIN将RDMA语义扩展，允许用户自定义简单的包处理任务，这些任务称为handler。handler在主机上交叉编译。对每一种消息需要分别对包头、负载和包尾定义三种handler，消息描述符中含有需要进行的handler的信息。

// sPIN介绍待细化

## **PsPIN硬件架构**

PsPIN基于PULP使用RISC-V作为在网计算的处理单元。下图为PsPIN的硬件架构图。每一个集群（cluster）中包含多个HPU，一个HPU是一个32位单发射顺序RISC-V核。图中共有四个集群，每一个集群中有8个HPU。L2 handler memory中存储handler需要的数据，program memory中存储handler程序，packet buffer存储需要处理的数据包。前两者都准备好后，主机向网卡配置运行上下文，其中包含数据包计算条件和内存信息。

{{< figure src="arch.jpg" caption="**PsPIN架构**" numbered="true" height="75%" width="75%" >}}

### **PsPIN Unit**

inbound engine处理网络接收到的数据包，对数据包进行判断，如果满足计算条件则转存在PsPIN Unit中的packet buffer中，并向包调度器（Packet Scheduler）发送一个包含数据包指针以及运行上下文的处理请求（Handler Execution Request, HER），之后由包调度器选择特定的集群进行计算。为了尽量避免同一条消息的数据包调度到不同集群从而引发跨集群访存，每一条消息指定一个首位集群（home cluster），包调度器优先将此消息调度至首位集群中，如果首位集群的L1没有足够的空间容纳数据包，则会调度至负载最少的集群。

{{< figure src="control-path.jpg" caption="**PsPIN Unit架构**" numbered="true" height="75%" width="75%" >}}

### **RISC-V集群**

包调度器选择一个特定的集群后，此集群通过DMA将数据包从L2读取到L1中，从而能够单周期访存。

{{< figure src="cluster.jpg" caption="**集群内部架构**" numbered="true" height="75%" width="75%" >}}

## **评测**

