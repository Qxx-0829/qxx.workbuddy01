# LLM Wiki（LLM 维基 / LLM Wiki Agent）讲解

> 本资料基于已安装到当前目录的 `llm-wiki-agent/`（README.md 与 CLAUDE.md）整理。
> 以当前工具版本为准。

## 学习目标

学完后应当能够：

1. **解释** LLM Wiki 是什么、它要解决什么痛点。
2. **区分** LLM Wiki 与传统 RAG 的关键差异。
3. **描述** `raw/`、`wiki/`、`graph/` 三层结构与 `ingest / query / lint / graph` 工作流。
4. **应用** 在自己的资料上跑通一次「丢文档 → 吸收 → 提问」。

## 核心问题

- 它到底是什么？
- 为什么有了 RAG 还需要它？
- 知识是怎么组织、怎么积累的？
- 我能用它做什么？

## 一句话定义

**LLM Wiki** 是一种「让 AI 代理把零散资料（论文、笔记、PDF…）逐步整理成一份结构化、互相链接、可持续积累的知识库（wiki）」的方法/工具范式。它最完整的开源实现是 **SamurAIGPT/llm-wiki-agent**——一个面向 Claude Code / Codex / Gemini CLI 的 *agent skill*（把操作规程写成 `CLAUDE.md`/`AGENTS.md`/`GEMINI.md`，agent 读后自动按流程维护 wiki）。

## 它解决什么问题

传统笔记/知识工具有个普遍痛点：**资料越存越多，但永远是「一堆你自己以后得去搜的笔记/PDF」**。你存了却没读、读了也难串联。

LLM Wiki 让 AI 替你做「阅读 → 抽取 → 结构化 → 交叉引用 → 矛盾标注」的脏活，最终产出一份**会随资料增加而越来越丰富**的 wiki——而不是一个你再也不会打开的文件夹。

## 核心机制

### 组成部分（目录布局）

| 目录/文件 | 作用 | 由谁负责 |
|---|---|---|
| `raw/` | 原始资料（不可变，不要改） | 用户 |
| `wiki/` | AI 生成的结构化知识层 | AI（agent） |
| `wiki/index.md` | 所有页面的目录，每次 ingest 更新 | AI |
| `wiki/overview.md` | 跨所有来源的「活合成」概览 | AI |
| `wiki/sources/` | 每个源文档一页摘要 | AI |
| `wiki/entities/` | 人物、公司、项目、产品 | AI 自动建 |
| `wiki/concepts/` | 想法、框架、方法、理论 | AI 自动建 |
| `wiki/syntheses/` | 已保存的提问答案 | AI（按需） |
| `graph/` | 知识图谱数据 `graph.json` + 可交互 `graph.html` | AI 生成 |
| `CLAUDE.md` / `AGENTS.md` / `GEMINI.md` | 给 agent 的 schema 与操作规程 | 用户可改 |
| `tools/` | 独立 Python 脚本（health / lint / build_graph） | — |

每个 wiki 页带 YAML frontmatter（`type: source|entity|concept|synthesis`、`tags`、`sources`、`last_updated`），并用 `[[PageName]]` wikilink 互相链接。

### 工作过程（以一次 `ingest` 为例）

1. 把源文件丢进 `raw/`
2. 触发 `ingest`：AI 通读 → 写 `wiki/sources/<slug>.md` → 更新 `index.md` / `overview.md`
3. 自动**建/更新** `entities` 与 `concepts` 页，并在第 8 步**标注与已有内容的矛盾**
4. 追加 `wiki/log.md`（可 grep 的时间线）
5. 之后可 `query`（带 `[[wikilink]]` 引用的答案，可存为 syntheses）、`lint`（找孤儿页/断链/矛盾/缺口）、`build graph`

### 结果

一份带 frontmatter、互相 `[[链接]]` 的纯 Markdown wiki；可用 **Obsidian** 浏览、用 **git** 做版本控制。
技术栈：**NetworkX + Louvain + vis.js**，纯本地、无数据库、无服务器。

## 应用案例

**场景**：连续几周深挖「注意力机制 / Transformer」。

- **输入**：把 `attention-is-all-you-need.md`、`llama2.md`、`rag-survey.md` 放进 `raw/papers/`
- **过程**：依次 `/wiki-ingest` 三篇 → AI 自动生成实体页（Meta AI、Google Brain）与概念页（Attention、RLHF、Context Window），并在 ingestion 时标记矛盾
- **结果**：`/wiki-query "有哪些降低幻觉的主流方法？"` 得到带引用的综合答案；`/wiki-lint` 提示「缺少 mixture-of-experts 资料，建议补 Mixtral 论文」
- **适用条件**：需要长期、跨多源、要交叉引用与矛盾检查的研究 / 阅读 / 个人知识管理

## 与相邻概念的区别（对比 RAG）

| 维度 | RAG | LLM Wiki Agent |
|---|---|---|
| 知识来源 | 每次查询重新从原始 chunk 检索+生成 | 一次性编译成结构化 wiki 页并持续保持最新 |
| 检索单元 | 原始文本片段 | 结构化 wiki 页面 |
| 交叉引用 | 无 | 预先建好的 `[[wikilink]]` |
| 矛盾发现 | 查询时才可能暴露（甚至不暴露） | **ingest 时**就被标记 |
| 知识积累 | 无（每次从零） | 每多一个源，wiki 更丰富 |

**联系**：两者都用 LLM 理解文本；LLM Wiki 可视为「把知识**预先编译**成 wiki」而非「每次现查」。
**选择依据**：追求极低延迟、资料极少且一次性问答 → RAG 更轻；追求长期积累、复杂推理、可审计 → LLM Wiki 更合适。

## 常见误区与边界

- **误区 1**：以为它要 API key / 装 Python 才能用。→ 实际上作为 agent skill 在 Claude Code 里打开仓库即可，**无需 key**；只有独立 `tools/` 脚本需要 `ANTHROPIC_API_KEY`。
- **误区 2**：以为它是数据库系统。→ 其实一切都是本地 Markdown 文件。
- **边界**：它依赖 agent（Claude Code / Codex / Gemini CLI）执行 schema；在其它环境（如 WorkBuddy）需按 `CLAUDE.md` 流程手动操作，slash 命令不会自动加载。
- **多格式摄入**：非 `.md` 文件（pdf/docx/pptx/xlsx…）靠 `markitdown` 在 ingest 时自动转 Markdown；需要该依赖才支持。

## 记忆要点

> 「丢进去 → AI 整理成 wiki → 越用越聪明」

- 三层：`raw`（原料）/ `wiki`（成品）/ `graph`（关系图）
- 四动作：`ingest` / `query` / `lint` / `graph`
- 对比 RAG 关键词：**编译一次 vs 每次重查**、**预建链接 vs 无链接**、**ingest 标矛盾 vs 查询才暴露**

## 自测题

1. `raw/` 和 `wiki/` 分别由谁负责、能否修改？
2. 说出 LLM Wiki 相比 RAG 的至少三点区别。
3. `ingest` 一个与现有结论矛盾的源文件时，系统会怎么做？
4. 知识图谱 `graph.html` 的「确定性边」和「语义推断边」分别指什么？
5. 为什么这份 wiki 适合用 Obsidian 浏览？

## 参考答案

1. `raw/` 是用户放入的**不可变**原始资料，不应修改；`wiki/` 完全由 AI（agent）维护，可随每次 ingest 更新。
2. ① 编译一次持续更新 vs 每次查询重算；② 预建 `[[wikilink]]` 交叉引用 vs 无引用；③ 矛盾在 ingest 时标记 vs 查询时才可能暴露；④ 每源让 wiki 更丰富 vs 无积累。
3. 在 ingest 流程中**标注与已有 wiki 内容的矛盾**，写入 source 页的 `Contradictions` 段并提示用户，而不是等到查询时才发现。
4. **确定性边（EXTRACTED）**：解析所有 `[[wikilink]]` 得到；**语义推断边（INFERRED）**：agent 推断 wikilink 未捕获的隐含关系，带置信度（或标记为 AMBIGUOUS）。
5. 因为所有页面保持一致的 `[[wikilinks]]`，Obsidian 能天然渲染可点击交叉引用并生成图谱视图，是浏览该 wiki 的推荐工具。

## 参考来源

- 仓库 README：`llm-wiki-agent/README.md`（已安装到当前工作目录）
- Schema / 工作流：`llm-wiki-agent/CLAUDE.md`
- 官方仓库：https://github.com/SamurAIGPT/llm-wiki-agent
- 思想源头：Andrej Karpathy 提出的「用 LLM 增量构建个人 wiki 替代 RAG」；灵感可追溯至 Vannevar Bush 的 Memex（1945）
