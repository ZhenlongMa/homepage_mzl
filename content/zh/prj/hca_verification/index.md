---
title: RDMA网卡系统级功能验证框架
summary: 本工作对课题组开发的RDMA网卡作RTL功能验证，使用UVM框架判定网卡工作的正确性。
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

categories:

tags:
- 验证
---
[本项目源代码在此](https://github.com/ZhenlongMa/RDMA-NIC-Verification)

## **函谷HCA介绍**
RDMA网卡（简称RNIC）是主机与RDMA网络之间的软硬件交互接口，通过旁路内核从而降低延迟并达到较高带宽。本课题组组开发的RNIC支持最多8192个连接，带宽达到100Gbps（数据位宽256bit，时钟500MHz）。

### **软硬件接口** 
RNIC向主机暴露BAR空间用作配置寄存器和门铃寄存器。

## **验证平台架构设计**
验证平台基于UVM搭建，整体结构图如下：

{{< figure src="framework.png" caption="Verification Framework" numbered="true" height="75%" width="75%" >}}

验证平台模拟了两台主机互相发送数据，两台主机抽象为两个sub_env。

### **Component**

### **主机侧接口**
DUT与主机侧通信采用[Xilinx提供的PCIe IP](https://www.xilinx.com/products/intellectual-property/7_series_gen_3_pci_express.html#tabAnchor-overview)，其接口可参考其文档。

## **激励生成机制**
验证平台根据运行时参数确定本次验证的连接数、数据量等信息。

### **命令行用户接口**
命令行中需要指定的参数及取值范围如下表：

|参数|含义|取值范围|
|:---:|:---:|:---:|
|+HOST_NAME|通信的主机个数|取决于DUT实际情况|
|+PROC_NAME|每台主机的通信进程数量|>=2|
|+RC_QP_NUM|RC类型的QP数量|0-8192，QP总数不能超过8192|
|+UC_QP_NUM|UC类型的QP数量|0-8192，QP总数不能超过8192|
|+UD_QP_NUM|UD类型的QP数量|0-8192，QP总数不能超过8192|
|+DB_NUM|每个QP下发的门铃个数|0-10|
|+WRITE_WQE_NUM|每个门铃中包含的WRITE工作请求个数|0-30，工作请求总数不能超过30|
|+READ_WQE_NUM|每个门铃中包含的READ工作请求个数|0-30，工作请求总数不能超过30|
|+SEND_WQE_NUM|每个门铃中包含的SEND工作请求个数|0-30，工作请求总数不能超过30，SEND数量与RECV相同|
|+RECV_WQE_NUM|每个门铃中包含的RECV工作请求个数|0-30，工作请求总数不能超过30，SEND数量与RECV相同|
|+SG_NUM|每个工作请求中包含的SG Entry数量|1-6|
|+DATA_CNT|每个SG Entry中包含的数据量|不限|
|+DATA_UNIT|上述数据量的单位|B/KB/MB|

一个测试的输入样例如下：

```
./hca_test -uvmDebug +UVM_VERBOSITY=UVM_LOW \
+vpdfile+./case/multiqp_wave/wave4109.vpd \
+UVM_TESTNAME=test_direct_param +HOST_NUM=2 +PROC_NUM=2 \
+RC_QP_NUM=16 +UC_QP_NUM=0 +UD_QP_NUM=0 \
+DB_NUM=1 \
+WRITE_WQE_NUM=0 +READ_WQE_NUM=0 +SEND_WQE_NUM=1 +RECV_WQE_NUM=1 \
+SG_NUM=1 +DATA_CNT=10 +DATA_UNIT=B \
-l ./case/multiqp_log/case4109.log
```


## **地址分配方法**
RDMA网卡工作涉及到三种内存区域：上下文空间、队列空间以及数据空间。上下文空间存储内存地址转换信息、队列状态信息等等对通信进行管理所需要的数据，在实际网卡使用中此部分只能由在内核态访问；队列空间存储工作队列以及完成队列的描述符；数据空间中为待发送的数据或接收缓冲区。

验证平台为上下文分配16TB空间，为每个队列分配8GB数据空间和8MB队列空间。

| 63:48 | 47 | 46:33 | 32:0 |
|:----:|:----:|:----:|:----:|
|进程号|1|QP号|0|

工作队列区域地址格式如下：

| 63:48 | 47:38 | 37:24 | 23:0 |
|:----:|:----:|:----:|:----:|
|进程号|10'b1|QP号|0|

完成事件队列区域地址格式如下：

| 63:48 | 47:33 | 32:20 | 19:0 |
|:----:|:----:|:----:|:----:|
|进程号|15'b1|CQ号|0|

### **正确性判定**
由于硬件支持的连接数量最多达到8192个，并且网卡片上还有上下文信息的cache。如果采用golden model的话很难保证与DUT的请求顺序一致。因此本验证平台将整个通信过程看作黑盒，仅比对源地址和目的地址的数据是否一致。