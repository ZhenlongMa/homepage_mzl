---
title: 谷歌TPU调研
summary: 本文为对谷歌TPU加速器的调研报告，本文待完成

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

张量处理器（Tensor Processing Unit，TPU）是谷歌针对DNN的推理过程推出的一款DSA。早在2006年谷歌的工程师们就讨论过在数据中心内部署类似的器件，但当时适用于其上的应用很少，部署的成本相对较高，因此搁置了几年。但2013年谷歌预测接下来几年语音搜索会广泛应用，如果用户每天语音搜索的时间达到三分钟，谷歌数据中心的规模需要翻倍。因此谷歌立刻启动了一个使用ASIC进行推理的项目，并期望性能可以达到GPU的十倍以上。15个月后，TPU诞生。

## **v1**  
TPUv1在2015年第二季度部署，仅支持推理过程，也就是说只能执行一个成熟的DNN模型。核心部件是一个乘法阵列，其中有256x256个ALU，每个ALU是一个8位乘加器，组成systolic架构进行矩阵乘运算，每个时钟周期可以进行25万次运算。  
乘法阵列分别与两个存储器相连，与数据存储器之间为高带宽，与参数存储器之间为低带宽，因为推理过程中参数不需要改变。  
第一代TPU不是冯诺依曼架构，其中没有取指环节，而是主机将指令送给TPU上的指令buffer中，其更像是一个浮点处理器而不是GPU。指令类似CISC，每条指令的执行时间（CPI）为10-20个时钟周期。TPU内部通过256位宽线路互连。  
{{< figure src="tpu-arch.png" caption="**TPUv1架构图**" numbered="true" height="100%" width="100%" >}}  
 

## **v2**  
TPUv1没有面向用户开放，仅仅被谷歌公司自己用作搜索加速。出于对训练加速的需求，2017年谷歌又部署了TPUv2。TPUv2作了许多升级，因为训练过程相对于推理过程更加复杂，对TPU的要求更高，表现在以下方面：  

- 推理过程一般包含10<sup>9</sup>次浮点运算操作，而谷歌的训练过程中包含了10<sup>19</sup>~10<sup>22</sup>次浮点操作，相差十个数量级以上；  
- 需要更大内存；  
- 需要更宽的操作数以提高精度；  
- 相比于推理过程可以让各个部分各自为战，训练过程需要多个芯片之间进行频繁的参数交互，通信要求更高。  

TPUv2的改进包括：将乘法器作为向量单元的一个协处理器；将两个buffer合并为一个；增加互连功能；每一个芯片内包含两个TPU核。新架构可以看作以向量单元为中心的架构。  
{{< figure src="v2.jpg" caption="**TPUv2架构图**" numbered="true" height="100%" width="100%" >}}  
TPUv2的一个VLIW语句为322位，包含两个标量操作、四个向量操作、两个矩阵操作、一个杂项操作和六个立即数。  
一个芯片中有四个片间链路和两个片内链路。片上链路与一个片上路由器连接，四条片间链路组成了一个2D torus网络，以契合机器学习的通信模型例如all-reduce等。片间链路带宽为500Gbps。通过这套网络，一个TPU系统最多支持256个TPU芯片协同工作。  

## **v3**  
TPUv3在2018年第四季度部署，在架构上并没有做过多调整，只是性能参数上有所提升，例如提高了矩阵乘单元的密度、提高时钟频率等等。  
{{< figure src="param_table.jpg" caption="**TPU主要参数**" numbered="true" height="100%" width="100%" >}}  


## **参考文献**
[1] 计算机体系结构量化研究方法  
[2] [In-Datacenter Performance Analysis of a Tensor Processing Unit](https://dl.acm.org/doi/10.1145/3079856.3080246)  
[3] The Design Process  