---
title: "Palos: Fair and Flexible Flow Scheduling on RNIC"
authors:
- admin
#- Robert Ford
date: "2024-11-09T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2017-01-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: In *IEEE International Conference on High Performance Computing and Communications*
publication_short: In *HPCC*

abstract: 

In recent years, Remote Direct Memory Access (RDMA) has gained significant attraction within modern hyperscale data centers. However, RNIC fails to provide fine-grained performance isolation among network flows with different traffic patterns which co-exist in multi-tenant data centers and typically have various bandwidth, throughput and latency requirements.In this paper, we reveal that the drawbacks on isolation root in the packet-level flow scheduling mechanism implemented in the RNIC hardware. To solve this problem, we introduce Palos, a fair and flexible flow-scheduling mechanism. In the hardware layer, Palos adopts a data chunk based scheduling mechanism by reconstructing communication descriptors. The data chunk based scheduling diminishes the performance interference between large flows and small flows. Palos configures the scheduler in the software layer using a hierarchical weight setting to enable customized performance policy while preventing the configuration of users from interfering each other. Our experiments demonstrate that Palos provides better performance isolation and performance control flexibility compared with the commodity RDMA NIC and existing optimization framework.

# Summary. An optional shortened abstract.
summary: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis posuere tellus ac convallis placerat. Proin tincidunt magna sed ex sollicitudin condimentum.

tags:
- Source Themes
featured: false

links:
- name: Custom Link
  url: http://example.org
url_pdf: http://eprints.soton.ac.uk/352095/1/Cushen-IMV2013.pdf
url_code: '#'
url_dataset: '#'
url_poster: '#'
url_project: ''
url_slides: ''
url_source: '#'
url_video: '#'

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


Supplementary notes can be added here, including [code and math](https://sourcethemes.com/academic/docs/writing-markdown-latex/).
