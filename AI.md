---
title: AI
date: 2024-08-06 11:36:14
tags:
---

# RAG

RAG（Retrieval-Augmented Generation）

1. 检索阶段（Retrieval）：从一个大规模的知识库或文档集合中检索出相关信息。

2. 生成阶段（Generation）：LLM检索到的信息作为上下文

![3](https://www.luxiangdong.com/images/wanjue1/3.png)

- 首先，会从用户给定的文档、图片等资源中提取内容；
- 然后通过chunking（可以认为是将连续的文本分成一个个小块）进行切割，再使用embedding过程变成向量数据存入向量数据库或elasticsearch等载体中。另外就是结合这些非结构化文件所附带的元数据（时间、文件名、作者、副标题、文件类型等）对进行索引（高级索引可以是树结构、图结构等）创建；
- 当RAG接受到用户的提问内容，会先将内容通过embedding转化为向量数据，然后和在前面建立的索引中进行相似度匹配，比如从百万的chunks中找出匹配度较高的100个。然后再将这100个chunk进行更耗时也更精准的重排序（使用交叉熵校验的Rerank算法），将最相关的top3结果找到；
- 最后RAG会将用户的问题、经过技术处理的top3的chunks，还有prompt，一起送入大语言模型，让它生成最终的答案。

## 检索模型

将文档转成向量，来进行相似度比较

1. 基于密集向量的检索模型：DPR（通过对比学习进行训练，针对问答任务进行了优化），BERT-based Models （能够生成上下文感知的嵌入，在处理复杂查询时表现优异）
1. 基于稀疏向量的检索模型：利用文档中关键词的稀疏表示进行检索，典型的如TF-IDF（通过计算术语在文档和整个语料库中的频率来表示文档，适用于规模小的文本库）和BM25（基于词频和逆文档率）
1. 混合模型（es最近的版本新的特性）

# fun call

AI模型解析用户输入，识别出需要调用的函数

# 知识图谱

知识图谱是一种数据结构，通常以节点和边的形式表示实体及其关系。每个节点代表一个实体（如人、地点、事物等），而边则表示实体之间的关系（如“工作于”，“位于”等）。

目的: 知识图谱的目的是以结构化的方式存储和管理知识，允许通过实体及其关系进行复杂的查询和推理。

技术实现: 知识图谱通常基于图数据库实现，支持语义查询，可以处理复杂的关联查询和推理。

# 向量数据库

就是一个向量和一个chunk

## metadata

document的元信息：名称、来源、上次更新日期、所有者或任何其他相关详细信息。

# 参考

https://torchv.com/blog/torchv-action-002/#%E6%A6%82%E8%BF%B0
