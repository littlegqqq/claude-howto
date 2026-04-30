---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*)
argument-hint: [message]
description: Create a git commit with context
---

## 上下文

- 当前 git 状态：!`git status`
- 当前 git 差异：!`git diff HEAD`
- 当前分支：!`git branch --show-current`
- 最近的提交记录：!`git log --oneline -10`

## 你的任务

根据以上变更，创建一个 git 提交（commit）。

如果通过参数提供了提交信息，请使用该信息：$ARGUMENTS

否则，请分析变更内容并按照约定式提交（Conventional Commits）格式创建适当的提交信息：
- `feat:` 用于新功能
- `fix:` 用于缺陷修复
- `docs:` 用于文档变更
- `refactor:` 用于代码重构
- `test:` 用于添加测试
- `chore:` 用于维护任务

---
**最后更新**：2026 年 4 月 9 日
