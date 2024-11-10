---
title: RDMA网卡系统级功能验证框架
summary: 本工作对课题组开发的RDMA网卡作系统级功能验证，使用UVM框架判定网卡硬件代码的正确性。
date: ""
publishDate: "2021-06-03T14:33:00+08:00"
lastmod: ""
# authors:
# - admin

# View.
#   1 = List
#   2 = Compact
#   3 = Card
view: 2

comments: true
profile: true
share: false

featured: false
headless: false
draft: false
private: false

gitment: true
slug: HCA-verification-cn

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""

links:
- name: GitHub
  url: https://github.com/ZhenlongMa/RDMA_NIC_Verification

categories:

tags:
- 验证
---
[本项目源代码在此](https://github.com/ZhenlongMa/RDMA-NIC-Verification)

## **函谷RNIC简介**
RDMA网卡（简称RNIC）是主机与RDMA网络之间的软硬件交互接口，通过旁路内核从而降低延迟并达到较高带宽。本课题组开发的RNIC支持最多8192个连接，带宽达到100Gbps（数据位宽256bit，时钟500MHz），软件层支持基础verbs。

## **验证平台架构设计**
验证平台基于UVM搭建，整体结构图如下：

{{< figure src="framework.png" caption="验证平台主体架构" numbered="true" height="100%" width="100%" >}}

验证平台的待测模块（Design Under Test， DUT）为两个相互直连的网卡，验证平台中两台主机抽象为两个sub_env，其中各有一个主动方（master agent）、一个被动方（slave agent）和一个内存模型。

验证激励由顶层文件通过不同的sequence发送给master端。

由于硬件支持的连接数量最多达到8192个，并且网卡内部还有上下文和地址转换的cache，输出行为无法预测，因此本验证平台将整个通信过程看作黑盒，在收集足够的完成事件后比对源地址和目的地址的数据是否一致。

### **主机侧接口**
DUT与主机侧通信采用[Xilinx提供的PCIe IP](https://www.xilinx.com/products/intellectual-property/7_series_gen_3_pci_express.html#tabAnchor-overview)，其接口可参考其文档。验证平台模拟了PCIe的行为。

## **激励生成机制**
验证平台运行前用户通过输入参数确定本次验证的连接数、服务类型、操作类型、数据量等信息。验证平台的基类模拟了ib_verbs库中的主要函数，例如create_mr、create_qp、modify_qp等。


### **地址分配方法**
RDMA网卡工作涉及到三种内存区域：上下文空间、队列空间以及数据空间。上下文空间存储内存地址转换信息、队列状态信息等等对通信进行管理所需要的数据，在实际网卡使用中此部分只能由在内核态访问或由RNIC访问；队列空间存储工作队列以及完成队列的描述符；数据空间中为待发送的数据或接收缓冲区，此两者由用户访问。

为方便管理，验证平台采用静态方式分配内存地址，即每一个工作队列及其负责的数据的地址按固定规则指定，而非在分配内存空间时以某个算法动态分配。

验证平台为上下文分配16TB空间，为每个队列分配8GB数据空间和8MB队列空间。

数据区域地址格式如下：

| 63:48 | 47 | 46:33 | 32:0 |
|:----:|:----:|:----:|:----:|
|进程号|1|QP号|0|

工作队列区域地址格式如下：

| 63:48 | 47:38 | 37:24 | 23:0 |
|:----:|:----:|:----:|:----:|
|进程号|10'b1|QP号|0|

## **工作过程**

To Do