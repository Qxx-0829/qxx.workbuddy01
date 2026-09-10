# Wiki Log

Append-only chronological record of all operations.

Format: `## [YYYY-MM-DD] <operation> | <title>`

Parse recent entries: `grep "^## \[" wiki/log.md | tail -10`

---

## [2026-09-09] ingest | LLM Wiki 概念学习

- 源文档：`raw/llm-wiki-concept.html`
- 生成：`wiki/sources/llm-wiki-concept.md`
- 新建概念页：`wiki/concepts/LLM-Wiki.md`、`wiki/concepts/RAG.md`、`wiki/concepts/Obsidian.md`
- 更新：`wiki/index.md`、`wiki/overview.md`
- 备注：将此前生成的概念学习材料（原 `LLM-Wiki-讲解.md`）转为 HTML 并正式加入 llm wiki
