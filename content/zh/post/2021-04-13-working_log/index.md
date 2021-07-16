---
title: 工作日志
summary: 每日一百字

authors:

# View.
#   1 = List
#   2 = Compact
#   3 = Card
view: 2

comments: true
profile: true
share: false

gitment: true
slug: working-log

featured: false
headless: false
draft: false
private: false

# Optional header image (relative to `static/img/` folder).
header:
  caption: ""
  image: ""
---
# 2021
## 4月8日
- 完成slave_monitor.
## 4月9日
- 完成博客搭建
- 完成参考模型最初版本.
## 6月8日
- Full Mask和MPW是两种集成电路流片的方式。Full Mask指制造流程中的全部掩模都为某一个设计服务；MPW，全称为Multi Project Wafer指多个项目共享同一个掩模。
## 7月9日
- 读PANIC论文
## 7月16日
### 项目工作：
- 修正验证平台bug，激励在从master driver到参考模型中经过一次复制，而qp上下文等结构体没有被添加进uvm field中，且uvm field不支持结构体，为解决这个问题，item类添加了复制结构体函数。
- query qp获得的结果不匹配，尚未排查到问题。
