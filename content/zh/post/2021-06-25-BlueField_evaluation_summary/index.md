---
title: 论文总结：Performance Characteristics of the BlueField-2 SmartNIC
summary: 一篇论文阅读总结
date: ""
publishDate: "2021-06-25T20:28:00+08:00"
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

[论文链接](https://arxiv.org/pdf/2105.06619.pdf)

无论是数据中心还是高性能计算领域都在寻求高效处理数据流的方法，因此基于各种架构的智能网卡或DPU陆续问世。其中最著名的应属Mellanox的BlueField。虽然智能网卡几年来陆续推出，但它在保证数据高速传输的基础上还剩余多少计算能力，以及什么样的计算任务适合放在智能网卡上进行，长期以来仍没有确切的答案。加州大学圣克鲁兹分校的Jianshen Liu等人在2021年5月14日的文章使用BlueField-2针对上述两个问题作了评测。

BlueField芯片中集成了8个A72核和一个ConnectX-6网卡以及一系列硬件加速器。它和主机之间有两种模式，既可以看作在同一个网络接口下的两个不同地址的终端设备，也可以使所有发往主机的流量都经过ARM核，这两种模式在DPU初始化的时候进行配置。

实验使用美国国家科学基金会（NSF）的云实验室搭载的DPU，DPU上装有Ubuntu20.04操作系统和英伟达提供的BlueField软件栈，接入云实验室100G以太网。

关于第一个问题，文章


参考资料：

[1] https://www.nextplatform.com/2021/05/24/testing-the-limits-of-the-bluefield-2-smartnic/

[2] Performance Characteristic of the BlueField-2 SmartNIC