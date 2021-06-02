---
title: A Survey on ConnectX NIC
summary: This is an integration of information about Mellanox ConnectX-3, ConnectX-4 and ConnectX-5 NIC.
date: ""
publishDate: "2021-04-26T12:47:00+08:00"
# publishDate: "2021-04-25T11:36:20Z"
lastmod: ""
#publishData: "2021-04-26"

view: 2

comments: true
profile: true
share: false

featured: false
headless: false
draft: true
private: false
---

## ConnectX-5:

{{< figure src="cx5_datacenter_rpc.png" caption="img1" numbered="true" height="75%" width="75%" >}}

8 nodes, each equipped with a dual-port 40 Gbps NIC, connected with an SX1036 switch. Every node sends 16B READ requests to another random node.The size of each connection state is about 375 bytes. The SRAM size on NIC is about 2MB.[1]

{{< figure src="cx5_storm.png" caption="img2" numbered="true" height="75%" width="75%" >}}

Comparation between CX3 and CX5. Page size is 2MB and 4KB. Random 64-Byte READ.


## ConnectX-4:

{{< figure src="cx4_stateless.png" caption="img3" numbered="true" height="75%" width="75%" >}}

## ConnectX-3:

Read latency: 1.7us.[1]

**Reference:**

[1] Datacenter RPCs can be general and fast

[2] Storm: a Fast Transactional Dataplane for Remote Data Structures