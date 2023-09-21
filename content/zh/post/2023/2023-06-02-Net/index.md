---
title: The Convergence of Hyperscale Data Center and High-Performance Computing Networks论文总结
summary: 本文为论文总结

authors:

date: ""
publishDate: "2023-08-10T15:04:00+08:00"
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
slug: convergence-hyperscale-cn

featured: false
headless: false
draft: true
private: false

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

本文是ETH的一位体系结构研究者Torsten Hoefler的一篇综述性文章。

我们往往将数据中心和高性能计算看做两个不同种类的环境，两者除了都是大规模集群这一点外似乎并没有什么共同点。此文从设计、运维、编程等多方面全面介绍了HPC和数据中心网络差别。

{{< figure src="Capture.jpg" caption="**数据中心网络与HPC网络**" numbered="true" height="75%" width="75%" >}}