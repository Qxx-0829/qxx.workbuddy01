---
title: "RAG"
type: concept
tags: [methodology, retrieval]
sources: [llm-wiki-concept]
last_updated: 2026-09-09
---

# RAG（检索增强生成，Retrieval-Augmented Generation）

一种在每次查询时，从原始文本片段检索相关信息并交给 LLM 生成答案的范式。

## 与 LLM Wiki 的区别

- 每次查询重新从原始 chunk 检索，不积累知识
- 无交叉引用
- 矛盾往往在查询时才暴露（甚至不暴露）

## 关联

- [[LLM Wiki]] — 对比参见 [[LLM Wiki 概念学习]]
- [[Obsidian]]
