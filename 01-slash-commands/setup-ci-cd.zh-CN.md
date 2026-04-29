---
name: Setup CI/CD Pipeline
description: Implement pre-commit hooks and GitHub Actions for quality assurance
tags: ci-cd, devops, automation
---

# 设置 CI/CD 流水线

实现全面的 DevOps 质量门禁（Quality Gates），适配项目类型：

1. **分析项目**：检测编程语言、框架、构建系统和现有工具链
2. **配置 pre-commit 钩子**，使用与语言匹配的工具：
   - 格式化：Prettier/Black/gofmt/rustfmt 等
   - 代码检查（Linting）：ESLint/Ruff/golangci-lint/Clippy 等
   - 安全扫描：Bandit/gosec/cargo-audit/npm audit 等
   - 类型检查：TypeScript/mypy/flow（如适用）
   - 测试：运行相关测试套件
3. **创建 GitHub Actions 工作流**（`.github/workflows/`）：
   - 在 push/PR 时镜像 pre-commit 检查
   - 多版本/多平台矩阵（如适用）
   - 构建和测试验证
   - 部署步骤（如需要）
4. **验证流水线**：本地测试，创建测试 PR，确认所有检查通过

使用免费/开源工具。尊重现有配置。保持执行速度。

---
**最后更新**：2026 年 4 月 9 日
