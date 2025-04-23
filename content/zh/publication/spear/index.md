---
title: "Toward Scalable RDMA through Resource Prefetching"
authors:
- admin
- 康宁
- 杨帆
- 洪重阳
- 许晶
- 元国军
- 张佩珩
- 王展
- 孙凝晖
#- Robert Ford
date: "2025-04-23T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2017-01-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: In *IEEE International Conference on High Performance Computing and Communications*
publication_short: In *HPCC*

abstract: RDMA network is being widely deployed in data centers, high-performance computing, and AI clusters. By ofﬂoading the network processing protocol stack to hardware, RDMA bypasses the operating system kernel, thereby enabling high performance and low CPU overhead. However, the protocol processing demands substantial communication resources, and due to the limited hardware resources, commercial NICs (Network Interface Cards) experience a signiﬁcant number of cache misses in large-scale connection scenarios. This results in performance degradation, indicating that RDMA lacks scalability. In this paper, we ﬁrst analyze the characteristics of resource access in RDMA. Based on these characteristics, we propose a resource access prediction and prefetching mechanism in the hardware, which preemptively fetches the resources required by the protocol processing pipeline to the on-chip cache. This mechanism increases the NIC’s cache hit ratio. Evaluation results demonstrate that our approach improves throughput by 125% and reduces latency by 17.9% under large-scale communication scenarios.

# Summary. An optional shortened abstract.
summary: 为了解决RDMA在大规模连接场景下因缓存未命中率高而导致性能下降、可扩展性不足的问题，本文分析了RDMA的资源访问特征。基于此，提出一种硬件级的资源访问预测与预取机制，通过预先将协议处理所需资源加载到片上缓存中，有效提升了缓存命中率和系统性能。

# This paper addresses RDMA’s limitations in isolating network flows with diverse performance needs in multi-tenant data centers. The proposed solution, Palos, improves performance isolation by using a data chunk-based scheduling mechanism at the hardware level, reducing interference between large and small flows. A hierarchical, weight-based scheduler configuration in the software layer further allows customized, interference-free performance policies. Experiments show that Palos provides better isolation and flexibility than current RDMA NICs.

tags:
- Source Themes
featured: false
# gitment: true
# slug: palos-cn

links:
- name: GitHub
  url: https://github.com/ZhenlongMa/RNIC_Simulator_Scheduler
- name: PDF
  url: ../files/spear.pdf
# url_pdf: http://eprints.soton.ac.uk/352095/1/Cushen-IMV2013.pdf
# url_pdf: ''
# url_code: '#'
# url_dataset: '#'
# url_poster: '#'
# url_project: ''
# url_slides: ''
# url_source: '#'
# url_video: '#'

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/pLCdAaMFLTE)'
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects:
- internal-project

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides:
---


<!-- Supplementary notes can be added here, including [code and math](https://sourcethemes.com/academic/docs/writing-markdown-latex/). -->
