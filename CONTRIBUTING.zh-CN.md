<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# 贡献指南 — Claude How To

感谢您有兴趣为本项目做出贡献！本指南将帮助您了解如何有效地参与贡献。

## 关于本项目

Claude How To 是一份以可视化和示例驱动的 Claude Code 指南。我们提供：
- **Mermaid 图表**，解释各功能的工作原理
- **可直接用于生产环境的模板**，开箱即用
- **真实场景示例**，附带上下文和最佳实践
- **渐进式学习路径**，从入门到高级

## 贡献类型

### 1. 新增示例或模板
为现有功能（斜杠命令、技能、钩子等）添加示例：
- 可直接复制粘贴的代码
- 清晰的工作原理说明
- 使用场景和优势
- 故障排查提示

### 2. 文档改进
- 澄清令人困惑的章节
- 修复拼写和语法错误
- 补充缺失的信息
- 改进代码示例

### 3. 功能指南
为 Claude Code 新功能创建指南：
- 分步教程
- 架构图
- 常见模式与反模式
- 真实工作流程

### 4. 问题报告
报告您遇到的问题：
- 描述预期行为
- 描述实际行为
- 包含复现步骤
- 附上相关的 Claude Code 版本和操作系统信息

### 5. 反馈与建议
帮助改进本指南：
- 建议更好的解释方式
- 指出覆盖范围的不足
- 推荐新增章节或重新组织结构

## 快速开始

### 1. Fork 并克隆
```bash
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto
```

### 2. 创建分支
使用描述性的分支名称：
```bash
git checkout -b add/feature-name
git checkout -b fix/issue-description
git checkout -b docs/improvement-area
```

### 3. 配置开发环境

Pre-commit 钩子会在每次提交前本地运行与 CI 相同的检查。所有四项检查必须全部通过，PR 才会被接受。

**必需依赖：**

```bash
# Python 工具链（uv 是本项目的包管理器）
pip install uv
uv venv
source .venv/bin/activate
uv pip install -r scripts/requirements-dev.txt

# Markdown 检查工具（Node.js）
npm install -g markdownlint-cli

# Mermaid 图表验证工具（Node.js）
npm install -g @mermaid-js/mermaid-cli

# 安装 pre-commit 并激活钩子
uv pip install pre-commit
pre-commit install
```

**验证配置：**

```bash
pre-commit run --all-files
```

每次提交时运行的钩子如下：

| 钩子 | 检查内容 |
|------|---------|
| `markdown-lint` | Markdown 格式和结构 |
| `cross-references` | 相对链接、锚点、代码围栏 |
| `mermaid-syntax` | 所有 ` ```mermaid ` 代码块能正确解析 |
| `link-check` | 外部 URL 可达 |
| `build-epub` | EPUB 生成无错误（针对 `.md` 文件变更） |

## 目录结构

```
├── 01-slash-commands/      # 用户调用的快捷命令
├── 02-memory/              # 持久化上下文示例
├── 03-skills/              # 可复用能力
├── 04-subagents/           # 专用 AI 助手
├── 05-mcp/                 # Model Context Protocol 示例
├── 06-hooks/               # 事件驱动的自动化
├── 07-plugins/             # 捆绑功能
├── 08-checkpoints/         # 会话快照
├── 09-advanced-features/   # 规划、思考、后台运行
├── 10-cli/                 # CLI 参考
├── scripts/                # 构建和工具脚本
└── README.md               # 主指南
```

## 如何贡献示例

### 添加斜杠命令
1. 在 `01-slash-commands/` 中创建一个 `.md` 文件
2. 包含：
   - 功能的清晰描述
   - 使用场景
   - 安装说明
   - 使用示例
   - 自定义提示
3. 更新 `01-slash-commands/README.md`

### 添加技能
1. 在 `03-skills/` 中创建一个目录
2. 包含：
   - `SKILL.md` — 主文档
   - `scripts/` — 辅助脚本（如需要）
   - `templates/` — 提示词模板
   - README 中的使用示例
3. 更新 `03-skills/README.md`

### 添加子代理
1. 在 `04-subagents/` 中创建一个 `.md` 文件
2. 包含：
   - 代理的用途和能力
   - 系统提示词结构
   - 示例用例
   - 集成示例
3. 更新 `04-subagents/README.md`

### 添加 MCP 配置
1. 在 `05-mcp/` 中创建一个 `.json` 文件
2. 包含：
   - 配置说明
   - 所需环境变量
   - 配置步骤
   - 使用示例
3. 更新 `05-mcp/README.md`

### 添加钩子
1. 在 `06-hooks/` 中创建一个 `.sh` 文件
2. 包含：
   - Shebang 行和描述
   - 解释逻辑的清晰注释
   - 错误处理
   - 安全注意事项
3. 更新 `06-hooks/README.md`

## 编写规范

### Markdown 风格
- 使用清晰的标题（H2 用于章节，H3 用于子章节）
- 段落保持简短精炼
- 列表使用项目符号
- 代码块需指定语言
- 章节之间添加空行

### 代码示例
- 确保示例可直接复制粘贴
- 为非显而易见的逻辑添加注释
- 提供简单和高级两种版本
- 展示真实使用场景
- 标注潜在问题

### 文档
- 解释"为什么"而不仅仅是"是什么"
- 列出前置条件
- 添加故障排查章节
- 链接到相关主题
- 保持对初学者友好

### JSON/YAML
- 使用统一的缩进（2 或 4 个空格）
- 添加注释解释配置项
- 包含验证示例

### 图表
- 尽量使用 Mermaid
- 图表保持简洁易读
- 在图表下方添加说明
- 链接到相关章节

## 提交规范

遵循约定式提交格式：
```
type(scope): description

[可选的正文]
```

类型：
- `feat`：新功能或示例
- `fix`：问题修复或更正
- `docs`：文档变更
- `refactor`：代码重构
- `style`：格式变更
- `test`：测试的新增或修改
- `chore`：构建、依赖等

示例：
```
feat(slash-commands): Add API documentation generator
docs(memory): Improve personal preferences example
fix(README): Correct table of contents link
docs(skills): Add comprehensive code review skill
```

## 提交前的检查

### 清单
- [ ] 代码遵循项目的风格和规范
- [ ] 新示例包含清晰的文档
- [ ] 已更新 README 文件（本地和根目录的）
- [ ] 未包含敏感信息（API 密钥、凭据等）
- [ ] 示例已测试且可正常运行
- [ ] 链接已验证且正确
- [ ] 文件具有正确的权限（脚本具有可执行权限）
- [ ] 提交信息清晰且具有描述性

### 本地测试
```bash
# 运行所有 pre-commit 检查（与 CI 检查一致）
pre-commit run --all-files

# 检查您的变更
git diff
```

## Pull Request 流程

1. **创建带有清晰描述的 PR**：
   - 本次添加/修复了什么？
   - 为什么需要？
   - 相关的 issue（如有）

2. **包含相关详情**：
   - 新功能？包含使用场景
   - 文档？说明改进之处
   - 示例？展示修改前后对比

3. **关联 issue**：
   - 使用 `Closes #123` 自动关闭相关 issue

4. **耐心等待审核**：
   - 维护者可能会提出改进建议
   - 根据反馈进行迭代
   - 最终决定权归维护者所有

## 代码审核流程

审核人员将检查：
- **准确性**：是否按描述正常工作？
- **质量**：是否达到生产级别？
- **一致性**：是否遵循项目规范？
- **文档**：是否清晰完整？
- **安全性**：是否存在漏洞？

## 报告问题

### 问题报告
请包含：
- Claude Code 版本
- 操作系统
- 复现步骤
- 预期行为
- 实际行为
- 截图（如适用）

### 功能请求
请包含：
- 要解决的使用场景或问题
- 建议的解决方案
- 您考虑过的替代方案
- 其他背景信息

### 文档问题
请包含：
- 哪些内容令人困惑或缺失
- 建议的改进方向
- 示例或参考资料

## 项目策略

### 敏感信息
- 切勿提交 API 密钥、令牌或凭据
- 在示例中使用占位值
- 为配置文件提供 `.env.example`
- 记录所需的环境变量

### 代码质量
- 保持示例聚焦且易读
- 避免过度工程化
- 为非显而易见的逻辑添加注释
- 提交前充分测试

### 知识产权
- 原创内容归作者所有
- 本项目使用教育许可
- 尊重现有版权
- 必要时注明出处

## 获取帮助

- **提问**：在 GitHub Issues 中发起讨论
- **常规帮助**：查看现有文档
- **开发帮助**：参考类似示例
- **代码审核**：在 PR 中标记维护者

## 致谢

贡献者将在以下位置获得认可：
- README.md 贡献者章节
- GitHub 贡献者页面
- 提交历史

## 安全

在贡献示例和文档时，请遵循安全编码实践：

- **切勿硬编码密钥或 API 密钥** — 使用环境变量
- **警示安全隐患** — 标注潜在风险
- **使用安全的默认配置** — 默认启用安全功能
- **验证输入** — 展示正确的输入验证和清理方式
- **包含安全说明** — 记录安全注意事项

有关安全问题，请参阅 [SECURITY.md](SECURITY.md) 了解我们的漏洞报告流程。

## 行为准则

我们致力于营造一个友好包容的社区环境。请阅读 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) 了解完整的社区规范。

简要来说：
- 尊重他人，包容多元
- 优雅地接受反馈
- 帮助他人学习和成长
- 避免骚扰或歧视行为
- 向维护者报告问题

所有贡献者都应遵守本准则，以善意和尊重对待彼此。

## 许可证

通过为本项目做出贡献，您同意您的贡献将按照 MIT 许可证进行授权。详情请参阅 [LICENSE](LICENSE) 文件。

## 有问题？

- 查看 [README](README.md)
- 阅读 [LEARNING-ROADMAP.md](LEARNING-ROADMAP.md)
- 参考现有示例
- 发起 issue 进行讨论

感谢您的贡献！🙏

---
**最后更新**：2026 年 4 月 9 日
