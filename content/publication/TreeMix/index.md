---
abstract: Data augmentation is an effective approach to tackle over-fitting. Many previous works have proposed different data augmentations strategies for NLP, such as noise injection, word replacement,  back-translation etc. Though effective, they missed one important characteristic of language--compositionality, meaning of a complex expression is built from its sub-parts. Motivated by this, we propose a compositional data augmentation approach for natural language understanding called TreeMix. Specifically, TreeMix leverages constituency parsing tree to decompose sentences into constituent sub-structures and the Mixup data augmentation technique to recombine them to generate new sentences. Compared with previous approaches, TreeMix introduces greater diversity to the samples generated and encourages models to learn compositionality of NLP data. Extensive experiments on text classification and semantic parsing benchmarks demonstrate that TreeMix outperforms current state-of-the-art data augmentation methods.
slides: example
url_pdf: "https://github.com/Magiccircuit/TreeMix/blob/main/ACL_2022_TreeMix.pdf"
publication_types:
  - "1"
authors:
  - Le Zhang
  - Zichao Yang
  - Diyi Yang
author_notes:
publication: In submission to ACL 2022
summary: We introduce a new compositional data augmentation method which encourages models to learn compositionality of NLP data and outperforms current state-of-the-art data augmentation methods on several benchmarks.
url_dataset: ""
url_project: ""
publication_short: ""
url_source: ""
url_video: ""
title: "TreeMix: Compositional Constituency-based Data Augmentation for Natural Language Understanding"
doi: ""
featured: true
tags: [NLP]
projects:
  - TreeMix
image:
  caption: "Illustration of TreeMix for single sentence classification"
  focal_point: ""
  preview_only: false
date: 2021-11-15T09:35:11.276Z
publishDate: preprint
url_code: "https://github.com/Magiccircuit/TreeMix"
---

##  :smile:This work is now in submission to ACL 2022! ! :smile:

# Main Result 

### Full datasets

![image.png](https://i.loli.net/2021/11/16/j4tEyJ5NpfTWIwA.png)

### Partial set results

![sst2_lr.png](https://i.loli.net/2021/11/16/iOStwlsmyh8zC2Z.png)

