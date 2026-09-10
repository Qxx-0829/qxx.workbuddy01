---
title: "LLM Wiki 概念学习"
type: source
tags: [learning, methodology]
date: 2026-09-09
source_file: raw/llm-wiki-concept.html
---

# LLM Wiki 概念学习

## Summary

一份面向初学者的 LLM Wiki 概念学习资料（由 `raw/llm-wiki-concept.html` 摄取整理而来）。核心结论：LLM Wiki 是「让 AI 代理把零散资料逐步整理成结构化、互链接、可持续积累的知识库」的方法/工具范式；最完整开源实现为 SamurAIGPT/llm-wiki-agent。

## Key Claims

- LLM Wiki 用 `raw/`（原料）→ `wiki/`（成品）→ `graph/`（关系图）三层结构组织知识。
- 四个核心动作：`ingest`（吸收）、`query`（提问）、`lint`（体检）、`graph`（图谱）。
- 相比 RAG：编译一次持续更新、预建 `[[wikilink]]` 交叉引用、矛盾在 ingest 时标记、每源让 wiki 更丰富。
- 一切都是本地 Markdown 文件；技术栈为 NetworkX + Louvain + vis.js，无服务器无数据库。

## Key Quotes

> "Most knowledge tools make you search your own notes. This one reads everything you've collected and writes a structured wiki that compounds over time." — llm-wiki-agent README

## Connections

- [[LLM Wiki]] — 本资料所讲解的核心概念/工具
- [[RAG]] — 相邻概念，LLM Wiki 用于替代或补足它
- [[Obsidian]] — 推荐用于浏览该 wiki 的编辑器

## Contradictions

- 无（首篇源文档，暂未与其它来源冲突）
