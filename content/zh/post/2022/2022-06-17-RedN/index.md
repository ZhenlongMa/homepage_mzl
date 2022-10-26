---
title: RedN论文笔记
summary: 本文为论文笔记

authors:

date: "2022-06-17T20:04:00+08:00"
publishDate: "2022-06-17T20:04:00+08:00"
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
slug: RedN-cn

featured: false
headless: false
draft: true
private: false

tags:
- 加速器互连

categories:
- 论文笔记

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""

# type: wowchemycms
# private: true
# outputs:
#   - wowchemycms_config
#   - HTML
---

智能网卡这一方向最近非常热门，因为用户普遍都希望硬件能替代软件执行尽可能多的任务，从而让主机将更多资源用于应用处理。

智能网卡的实现多种多样，有的基于FPGA，有的基于SoC等等，但无一例外的都是专用网卡，如果要进行计算卸载则需要替换掉当前应用的通用网卡，高昂的成本不是每个公司都能承担的，否则就只能将卸载工作限于网卡提供的几种操作。那么有没有办法采用通用网卡进行更灵活的计算卸载呢？2022年发表在NSDI上的文章[RDMA is Turing complete, we just did not know it yet!](https://www.usenix.org/system/files/nsdi22spring_prepub_reda.pdf)给出了一种方式，即采用ConnectX提供的现有机制，将其改造成一个图灵完备的机器。

## **Cross-Channel Communication**
[Mellanox的编程框架](https://docs.nvidia.com/networking/pages/viewpage.action?pageId=34256548&exitMobile=true)中提供了被称为Cross-Channel Communication的机制（下文简称3C），这种机制提供了多个QP之间的同步服务。用户可以向支持3C的QP下发wait和enable两种描述符。

### **WAIT描述符**
在正常的RDMA网卡工作过程中，