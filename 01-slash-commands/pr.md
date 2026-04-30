---
description: Clean up code, stage changes, and prepare a pull request
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(npm test:*), Bash(npm run lint:*)
---

# Pull Request 准备清单

在创建 PR 之前，请执行以下步骤：

1. 运行代码检查：`prettier --write .`
2. 运行测试：`npm test`
3. 查看 git 差异：`git diff HEAD`
4. 暂存变更：`git add .`
5. 按照约定式提交（Conventional Commits）格式创建提交信息：
   - `fix:` 用于缺陷修复
   - `feat:` 用于新功能
   - `docs:` 用于文档
   - `refactor:` 用于代码重构
   - `test:` 用于添加测试
   - `chore:` 用于维护任务

6. 生成 PR 摘要，包含：
   - 变更了什么
   - 为什么变更
   - 执行了哪些测试
   - 潜在影响

---
**最后更新**：2026 年 4 月 9 日
