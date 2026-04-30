# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在本仓库中工作时提供指导。

## 项目概述

Claude How To 是一个 Claude Code 功能的教程仓库。这是**文档即代码**——主要产出是按编号学习模块组织的 markdown 文件，而非可执行应用程序。

**架构**：每个模块（01-10）覆盖一个特定的 Claude Code 功能，包含可复制粘贴的模板、Mermaid 图表和示例。构建系统验证文档质量并生成 EPUB 电子书。

## 常用命令

### 提交前质量检查

所有文档在提交前必须通过四项质量检查（这些通过 pre-commit 钩子自动运行）：

```bash
# 安装 pre-commit 钩子（每次提交时运行）
pre-commit install

# 手动运行所有检查
pre-commit run --all-files
```

四项检查分别是：
1. **markdown-lint** — 通过 `markdownlint` 检查 Markdown 结构和格式
2. **cross-references** — 内部链接、锚点、代码围栏语法（Python 脚本）
3. **mermaid-syntax** — 验证所有 Mermaid 图表能正确解析（Python 脚本）
4. **link-check** — 外部 URL 可达（Python 脚本）
5. **build-epub** — EPUB 生成无错误（针对 `.md` 变更）

### 开发环境设置

```bash
# 安装 uv（Python 包管理器）
pip install uv

# 创建虚拟环境并安装 Python 依赖
uv venv
source .venv/bin/activate
uv pip install -r scripts/requirements-dev.txt

# 安装 Node.js 工具（markdown 检查器和 Mermaid 验证器）
npm install -g markdownlint-cli
npm install -g @mermaid-js/mermaid-cli

# 安装 pre-commit 钩子
uv pip install pre-commit
pre-commit install
```

### 测试

`scripts/` 中的 Python 脚本有单元测试：

```bash
# 运行所有测试
pytest scripts/tests/ -v

# 运行并生成覆盖率
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 运行特定测试
pytest scripts/tests/test_build_epub.py -v
```

### 代码质量

```bash
# 检查和格式化 Python 代码
ruff check scripts/
ruff format scripts/

# 安全扫描
bandit -c scripts/pyproject.toml -r scripts/ --exclude scripts/tests/

# 类型检查
mypy scripts/ --ignore-missing-imports
```

### EPUB 构建

```bash
# 生成电子书（通过 Kroki.io API 渲染 Mermaid 图表）
uv run scripts/build_epub.py

# 带选项
uv run scripts/build_epub.py --verbose --output custom-name.epub --max-concurrent 5
```

## 目录结构

```
├── 01-slash-commands/      # 用户调用的快捷命令
├── 02-memory/              # 持久化上下文示例
├── 03-skills/              # 可复用能力
├── 04-subagents/           # 专业 AI 助手
├── 05-mcp/                 # 模型上下文协议示例
├── 06-hooks/               # 事件驱动自动化
├── 07-plugins/             # 捆绑功能
├── 08-checkpoints/         # 会话快照
├── 09-advanced-features/   # 规划、思考、后台运行
├── 10-cli/                 # CLI 参考
├── scripts/
│   ├── build_epub.py           # EPUB 生成器（通过 Kroki API 渲染 Mermaid）
│   ├── check_cross_references.py   # 验证内部链接
│   ├── check_links.py          # 检查外部 URL
│   ├── check_mermaid.py        # 验证 Mermaid 语法
│   └── tests/                  # 脚本的单元测试
├── .pre-commit-config.yaml    # 质量检查定义
└── README.md               # 主指南（也是模块索引）
```

## 内容指南

### 模块结构
每个编号文件夹遵循以下模式：
- **README.md** — 功能概述及示例
- **示例文件** — 可复制粘贴的模板（命令用 `.md`，配置用 `.json`，钩子用 `.sh`）
- 文件按功能复杂度和依赖关系组织

### Mermaid 图表
- 所有图表必须能成功解析（由 pre-commit 钩子检查）
- EPUB 构建通过 Kroki.io API 渲染图表（需要网络）
- 使用 Mermaid 绘制流程图、序列图和架构图

### 交叉引用
- 内部链接使用相对路径（如 `(01-slash-commands/README.md)`）
- 代码围栏必须指定语言（如 ` ```bash `、` ```python `）
- 锚点链接使用 `#heading-name` 格式

### 链接验证
- 外部 URL 必须可达（由 pre-commit 钩子检查）
- 避免链接到短暂内容
- 尽可能使用永久链接

## 关键架构要点

1. **编号文件夹表示学习顺序** — 01-10 的前缀代表学习 Claude Code 功能的推荐顺序。这个编号是有意为之的；不要按字母顺序重新组织。

2. **脚本是工具，不是产品** — `scripts/` 中的 Python 脚本支持文档质量和 EPUB 生成。实际内容在编号模块文件夹中。

3. **Pre-commit 是守门人** — 所有四项质量检查必须在 PR 被接受前通过。CI 管道运行相同的检查作为第二道关。

4. **Mermaid 渲染需要网络** — EPUB 构建调用 Kroki.io API 来渲染图表。此处的构建失败通常是网络问题或无效的 Mermaid 语法。

5. **这是教程，不是库** — 添加内容时，专注于清晰的解释、可复制粘贴的示例和可视化图表。价值在于教授概念，而非提供可复用代码。

## 提交约定

遵循约定式提交格式：
- `feat(slash-commands): Add API documentation generator`
- `docs(memory): Improve personal preferences example`
- `fix(README): Correct table of contents link`
- `refactor(hooks): Simplify hook configuration examples`

范围应尽可能匹配文件夹名称。
