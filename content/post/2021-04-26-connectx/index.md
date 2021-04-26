---
title: A Survey on ConnectX NIC
summary: This is an integration of information about Mellanox ConnectX-3, ConnectX-4 and ConnectX-5 NIC.
publishDate: "2021-04-26T03:05:20Z"
#publishData: "2021-04-26"

view: 2

comments: true
profile: true
share: false

headless: true
---

## ConnectX-5:
**Datacenter RPCs can be general and fast:**

{{< figure src="cx5_datacenter_rpc.png" caption="img1" numbered="true" height="75%" width="75%" >}}

8 nodes, each equipped with a dual-port 40 Gbps NIC, connected with an SX1036 switch. Every node sends 16B READ requests to another random node.

The size of each connection state is about 375 bytes. The SRAM size on NIC is about 2MB.

{{< figure src="cx5_storm.png" caption="img2" numbered="true" height="75%" width="75%" >}}

Comparation between CX3 and CX5. Page size is 2MB and 4KB. Random 64-Byte READ.