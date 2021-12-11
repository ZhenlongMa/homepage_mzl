---
title: 谷歌TPU调研
summary: 本文为对谷歌TPU加速器的调研报告

authors:

date: ""
publishDate: "2021-12-11T15:35:00+08:00"
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
slug: TPU-cn

featured: false
headless: false
draft: false
private: false

tags:
- 加速器互连

categories:
- 调研与综述

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

张量处理器（Tensor Processing Unit，TPU）是谷歌针对DNN的推理过程推出的一款DSA。早在2006年谷歌的工程师们就讨论过在数据中心内部署类似的器件，但当时适用于其上的应用很少，部署的成本相对较高，因此搁置了几年。但2013年谷歌预测接下来几年语音搜索会广泛应用，如果用户每天语音搜索的事件达到三分钟，谷歌数据中心的规模需要翻倍。因此谷歌立刻启动了一个使用ASIC进行推理的项目，并期望性能可以达到GPU的十倍以上。15个月后，TPU诞生。

TPU的核心部件是一个乘法阵列，其中有256x256个ALU，每个ALU是一个8位乘加器，组成systolic架构进行矩阵乘运算。TPU不是冯诺依曼架构，其中没有取指环节，而是主机将指令送给TPU上的指令buffer中，其更像是一个浮点处理器而不是GPU。指令类似CISC，每条指令的执行时间（CPI）为10-20个时钟周期。TPU内部通过256位宽线路互连。

{{< figure src="tpu-arch.png" caption="**TPU架构图**" numbered="true" height="50%" width="50%" >}}

## **参考文献**

[1] 计算机体系结构量化研究方法
