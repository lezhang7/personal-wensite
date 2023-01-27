---
abstract: Although sequence-to-sequence models often achieve good performance in semantic parsing for i.i.d. data, their performance is still inferior in compositional generalization. Several data augmentation methods have been proposed to alleviate this problem. However, prior work only leveraged  superficial grammar or rules for data augmentation, which resulted in limited improvement. We propose to use subtree substitution for compositional data augmentation, where we consider subtrees with similar semantic functions as exchangeable. Our experiments showed that such augmented data led to significantly better performance on Scan and GeoQuery, and reached new SOTA on compositional split of GeoQuery.
url_pdf: "https://aclanthology.org/2022.naacl-main.12/"
publication_types:
  - "1"
authors:
  - Jingfeng Yang
  - Le Zhang
  - Diyi Yang

publication: NAACL 2022 (poster)

url_dataset: ""
url_project: ""
publication_short: ""
url_source: ""
url_video: ""

url_poster: ""
title: "SUBS: Subtree Substitution for Compositional Semantic Parsing"
doi: ""
featured: true
tags: [NLP]
projects:
  - SUBS
image:
  caption: "Illustration of SUBS"
  focal_point: ""
  preview_only: false
date: 2022-05-03T09:35:11.276Z
publishDate: preprint
url_code: "https://github.com/SALT-NLP/SUBS"
---

# Method

![exmf](https://s2.loli.net/2022/01/16/qGBThMefWFDz7I4.png)

<img src="https://s2.loli.net/2022/01/16/WPovegECJb1Uyjt.png" alt="image-20220116223344109" style="zoom: 67%;" />

# Main Result 

### datasets

<img src="https://s2.loli.net/2022/01/16/KQjWpVyhtEYrvPU.png" alt="image-20220116223308390" style="zoom:80%;" />



<img src="https://s2.loli.net/2022/01/16/eTDu3BhzrPtYyHN.png" alt="image-20220116223202195" style="zoom: 80%;" />

<img src="https://s2.loli.net/2022/01/16/RLKutDZNcOSiBAw.png" alt="image-20220116223214043" style="zoom: 80%;" />




