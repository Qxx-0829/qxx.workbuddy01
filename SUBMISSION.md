# 作业完成说明

## 流程记录

1. 创建本地 Git 仓库并准备项目目录。
2. 在 `.workbuddy/skills/concept-learning-material-generator/SKILL.md` 创建项目级 Skill。
3. 使用该 Skill 的统一流程整理三个概念：Agent、大模型的上下文、Skill。
4. 检查生成内容：每份资料均包含学习目标、核心问题、定义、机制、应用案例、概念对比、误区、记忆要点、自测题、参考答案和参考来源。
5. 人工核对概念表述、案例边界和来源链接，避免整段照搬 AI 输出或使用伪造来源。
6. 添加 `.gitignore`，排除 API Key、密码、个人目录、缓存和本地工具状态。
7. 将 Skill、学习资料和说明提交到本地 Git 仓库。
8. 配置 GitHub 远程仓库后 push。

## 推送前检查清单

- [ ] GitHub 远程地址已设置为自己的仓库地址。
- [ ] `git status` 显示工作区干净。
- [ ] `git log` 能看到本次作业提交。
- [ ] `git push -u origin main` 成功。
- [ ] 在 GitHub 网页确认 `.workbuddy/skills/` 和 `learning-materials/` 已上传。
- [ ] 使用敏感信息模式检查文件内容和 Git diff。