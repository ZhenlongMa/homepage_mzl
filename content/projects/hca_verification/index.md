---
title: HCA Verification
summary: This article is the introduction for my work of verification of HCA.
#authors:
#- admin

# View.
#   1 = List
#   2 = Compact
#   3 = Card
view: 2

comments: true
profile: true
share: false


# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

## **1. HCA Introduction**
Host Channel Adapter(HCA) is the software/hardware interface between IB network and host. Our HCA - HanGu HCA - supports up to 16K QPs and bandwidth with 100Gbps.

## **2. HCA Verification**
My verification framework is built based on Universal Verification Methodology(UVM).


{{< figure src="framework.jpg" caption="Verification Framework" numbered="true" height=500>}}