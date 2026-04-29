<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../../resources/logos/claude-howto-logo.svg">
</picture>

# 文档插件

为你的项目提供全面的文档生成和维护功能。

## 功能特性

✅ API 文档生成
✅ README 创建和更新
✅ 文档同步
✅ 代码注释改进
✅ 示例生成

## 安装

```bash
/plugin install documentation
```

## 包含内容

### 斜杠命令
- `/generate-api-docs` - 生成 API 文档
- `/generate-readme` - 创建或更新 README
- `/sync-docs` - 将文档与代码变更同步
- `/validate-docs` - 验证文档

### 子代理
- `api-documenter` - API 文档专家
- `code-commentator` - 代码注释改进
- `example-generator` - 代码示例创建

### 模板
- `api-endpoint.md` - API 端点文档模板
- `function-docs.md` - 函数文档模板
- `adr-template.md` - 架构决策记录模板

### MCP 服务器
- GitHub 集成用于文档同步

## 使用方法

### 生成 API 文档
```
/generate-api-docs
```

### 创建 README
```
/generate-readme
```

### 同步文档
```
/sync-docs
```

### 验证文档
```
/validate-docs
```

## 系统要求

- Claude Code 1.0+
- GitHub 访问权限（可选）

## 工作流示例

```
User: /generate-api-docs

Claude:
1. 扫描 /src/api/ 中的所有 API 端点
2. 委托给 api-documenter 子代理
3. 提取函数签名和 JSDoc
4. 按模块/端点组织
5. 使用 api-endpoint.md 模板
6. 生成全面的 markdown 文档
7. 包含 curl、JavaScript 和 Python 示例

Result:
✅ API documentation generated
📄 Files created:
   - docs/api/users.md
   - docs/api/auth.md
   - docs/api/products.md
📊 Coverage: 23/23 endpoints documented
```

## 模板用法

### API 端点模板
用于记录 REST API 端点并提供完整示例。

### 函数文档模板
用于记录单个函数/方法。

### ADR 模板
用于记录架构决策。

## 配置

设置 GitHub 令牌用于文档同步：
```bash
export GITHUB_TOKEN="your_github_token"
```

## 最佳实践

- 让文档靠近代码
- 随代码变更更新文档
- 包含实用示例
- 定期验证
- 使用模板保持一致性

---

**Last Updated**: April 24, 2026
**Claude Code Version**: 2.1.119
**Sources**:
- https://code.claude.com/docs/en/plugins
- https://github.com/anthropics/claude-code/releases/tag/v2.1.119
**Compatible Models**: Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5
