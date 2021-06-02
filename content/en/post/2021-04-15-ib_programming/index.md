---
title: InfiniBand Programming Interface
summary: This article introduces programming interface provided by Mallanox.

# View.
#   1 = List
#   2 = Compact
#   3 = Card
view: 2

comments: true
profile: true
share: false

featured: false
draft: false
private: false

headless: true # temp invisible

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---

## 1. Introduction
There are three network technology supporting RDMA: InfiniBand, RoCE and iWARP, all of which use common API - called verbs - defined by Mallanox.

## 2. Programming Interface
Libibverbs is the standard communication library provided by Mallanox. Its path is `libibverbs-1.2.1/src/verb.c`.

The source code of IB programming interface can be devided into three parts: communication library, user-level driver and kernel-level driver.