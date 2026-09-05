# qxx.workbuddy01

禁忌的统计学交作业之地

# 概念学习资料生成 Skill 作业

本项目完成了“概念学习资料生成 Skill”的创建、调用结果整理和 Git 版本管理。

## 目录

- `.workbuddy/skills/concept-learning-material-generator/SKILL.md`：项目级 Skill 定义。
- `learning-materials/agent.md`：Agent 学习资料。
- `learning-materials/context.md`：大模型上下文学习资料。
- `learning-materials/skill.md`：Skill 学习资料。
- `SUBMISSION.md`：作业流程记录、检查结果和提交说明。
- `.gitignore`：排除密钥、密码、个人目录、缓存和本地工具状态。

## 使用方式

在 WorkBuddy 中打开本仓库，调用项目级的“概念学习资料生成 Skill”，输入 `Agent`、`大模型的上下文` 或 `Skill`，即可按统一模板生成或更新资料。生成后应检查定义、示例、自测题和答案，再提交 Git。

提交前使用 `git status` 和敏感信息搜索检查变更；不要把 API Key、密码、个人隐私或其他敏感文件加入仓库。
