<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 斜杠命令

## 概述

斜杠命令是在交互会话中控制 Claude 行为的快捷方式。它们有几种类型：

- **内置命令**: Claude Code 提供的命令（`/help`、`/clear`、`/model`）
- **技能**: 用户定义的命令，创建为 `SKILL.md` 文件（`/optimize`、`/pr`）
- **插件命令**: 已安装插件的命令（`/frontend-design:frontend-design`）
- **MCP 提示**: MCP 服务器的命令（`/mcp__github__list_prs`）

> **注意**: 自定义斜杠命令已合并到技能中。`.claude/commands/` 中的文件仍然有效，但技能（`.claude/skills/`）是现在推荐的方法。两者都创建 `/command-name` 快捷方式。请参阅[技能指南](../03-skills/)获取完整参考。

## 内置命令参考

内置命令是常见操作的快捷方式。有 **60+ 个内置命令**和 **5 个捆绑技能**可用。在 Claude Code 中输入 `/` 查看完整列表，或输入 `/` 后跟任何字母进行筛选。

| 命令 | 用途 |
|------|------|
| `/add-dir <path>` | 添加工作目录 |
| `/agents` | 管理代理配置 |
| `/branch [name]` | 将对话分支到新会话（别名：`/fork`）。注意：`/fork` 在 v2.1.77 中更名为 `/branch` |
| `/btw <question>` | 在 Claude 处理主任务时提出临时附带问题；不会污染主对话上下文 |
| `/chrome` | 配置 Chrome 浏览器集成 |
| `/clear` | 清除对话（别名：`/reset`、`/new`） |
| `/color [color\|default]` | 设置提示栏颜色 |
| `/compact [instructions]` | 压缩对话，可选择性地聚焦指令 |
| `/config` | 打开设置（别名：`/settings`） |
| `/context` | 以彩色网格可视化上下文使用情况 |
| `/copy [N]` | 将助手响应复制到剪贴板；`w` 写入文件 |
| `/cost` | `/usage` 的快捷别名——打开费用标签页（v2.1.118+） |
| `/desktop` | 在桌面应用中继续（别名：`/app`） |
| `/diff` | 未提交更改的交互式 diff 查看器 |
| `/doctor` | 诊断安装健康状况——可在 Claude 响应时打开；显示状态图标；按 `f` 自动修复问题（v2.1.116 增强） |
| `/effort [low\|medium\|high\|xhigh\|max\|auto]` | 通过交互式方向键滑块设置努力级别。级别：`low` → `medium` → `high` → `xhigh`（v2.1.111 新增）→ `max`。默认为 Opus 4.7 上的 `xhigh`；`max` 需要 Opus 4.7 |
| `/exit` | 退出 REPL（别名：`/quit`） |
| `/export [filename]` | 将当前对话导出到文件或剪贴板 |
| `/extra-usage` | 配置速率限制的额外用量 |
| `/fast [on\|off]` | 切换快速模式 |
| `/feedback` | 提交反馈（别名：`/bug`） |
| `/focus` | 切换聚焦视图（v2.1.110 添加；替代 `Ctrl+O` 的聚焦切换） |
| `/help` | 显示帮助 |
| `/hooks` | 查看钩子配置 |
| `/ide` | 管理 IDE 集成 |
| `/init` | 初始化 `CLAUDE.md`。设置 `CLAUDE_CODE_NEW_INIT=1` 使用交互式流程 |
| `/insights` | 生成会话分析报告 |
| `/install-github-app` | 设置 GitHub Actions 应用 |
| `/install-slack-app` | 安装 Slack 应用 |
| `/keybindings` | 打开键绑定配置 |
| `/less-permission-prompts` | 分析最近的 Bash/MCP 工具调用，将优先级允许列表添加到 `.claude/settings.json` 以减少权限提示（v2.1.111 添加） |
| `/login` | 切换 Anthropic 账户 |
| `/logout` | 退出 Anthropic 账户 |
| `/mcp` | 管理 MCP 服务器和 OAuth |
| `/memory` | 编辑 `CLAUDE.md`，切换自动记忆 |
| `/mobile` | 移动应用 QR 码（别名：`/ios`、`/android`） |
| `/model [model]` | 用左右方向键选择模型和努力级别 |
| `/passes` | 分享 Claude Code 免费周 |
| `/permissions` | 查看/更新权限（别名：`/allowed-tools`） |
| `/plan [description]` | 进入规划模式 |
| `/plugin` | 管理插件 |
| `/proactive` | `/loop` 的别名（v2.1.105 添加） |
| `/powerup` | 通过带动画演示的交互式课程发现功能 |
| `/privacy-settings` | 隐私设置（仅 Pro/Max） |
| `/release-notes` | 查看更新日志 |
| `/recap` | 返回会话时显示会话回顾/摘要（v2.1.108 添加） |
| `/reload-plugins` | 重新加载活动插件 |
| `/remote-control` | 从 claude.ai 远程控制（别名：`/rc`） |
| `/remote-env` | 配置默认远程环境 |
| `/rename [name]` | 重命名会话 |
| `/resume [session]` | 恢复对话（别名：`/continue`） |
| `/review` | **已弃用** — 请安装 `code-review` 插件 |
| `/rewind` | 回退对话和/或代码（别名：`/checkpoint`） |
| `/sandbox` | 切换沙盒模式 |
| `/schedule [description]` | 创建/管理云端定时任务 |
| `/security-review` | 分析分支的安全漏洞 |
| `/skills` | 列出可用技能 |
| `/stats` | `/usage` 的快捷别名——打开统计标签页（每日使用量、会话、连续天数）（v2.1.118+） |
| `/stickers` | 订购 Claude Code 贴纸 |
| `/status` | 显示版本、模型、账户 |
| `/statusline` | 配置状态行 |
| `/tasks` | 列出/管理后台任务 |
| `/team-onboarding` | 从项目的 Claude Code 设置生成队友上手指南（v2.1.101 新增） |
| `/terminal-setup` | 配置终端键绑定 |
| `/theme` | 打开主题选择器/管理自定义主题（v2.1.118）。通过 `~/.claude/themes/<name>.json` 中的 JSON 定义自定义主题 |
| `/tui` | 切换全屏 TUI（文本用户界面）模式，带无闪烁渲染（v2.1.110 添加） |
| `/ultraplan <prompt>` | 在 ultraplan 会话中起草计划，在浏览器中审查 |
| `/ultrareview` | 基于云的综合代码审查，带多代理分析（v2.1.111 添加） |
| `/undo` | `/rewind` 的别名（v2.1.108 添加） |
| `/upgrade` | 打开升级页面获取更高计划层级 |
| `/usage` | 规范用量仪表板（v2.1.118）——组合计划用量限制、速率限制、费用和每日会话统计。`/cost` 和 `/stats` 是打开特定标签页的快捷别名 |
| `/voice` | 切换按键说话语音听写 |

### 捆绑技能

这些技能随 Claude Code 一起提供，像斜杠命令一样调用：

| 技能 | 用途 |
|------|------|
| `/batch <instruction>` | 使用工作树编排大规模并行更改 |
| `/claude-api` | 加载项目语言的 Claude API 参考 |
| `/debug [description]` | 启用调试日志 |
| `/loop [interval] <prompt>` | 按间隔重复运行提示 |
| `/simplify [focus]` | 审查已更改文件的代码质量 |

### 已弃用命令

| 命令 | 状态 |
|------|------|
| `/review` | 已弃用——被 `code-review` 插件替代 |
| `/output-style` | 自 v2.1.73 弃用 |
| `/fork` | 更名为 `/branch`（别名仍有效，v2.1.77） |
| `/pr-comments` | 在 v2.1.91 中移除——直接要求 Claude 查看 PR 评论 |
| `/vim` | 在 v2.1.92 中移除——使用 /config → 编辑器模式 |

### 最近更改

- `/fork` 更名为 `/branch`，`/fork` 保留为别名（v2.1.77）
- `/output-style` 已弃用（v2.1.73）
- `/review` 已弃用，替代为 `code-review` 插件
- `/effort` 命令添加了 `max` 级别，需要 Opus 4.7（最初仅限 Opus 4.6）
- `/voice` 命令添加了按键说话语音听写
- `/schedule` 命令添加了创建/管理定时任务
- `/color` 命令添加了提示栏自定义
- /pr-comments 在 v2.1.91 中移除——直接要求 Claude 查看 PR 评论
- /vim 在 v2.1.92 中移除——使用 /config → 编辑器模式
- /ultraplan 添加了基于浏览器的计划审查和执行
- /powerup 添加了交互式功能课程
- /sandbox 添加了沙盒模式切换
- `/model` 选择器现在显示人类可读的标签（例如 "Sonnet 4.6"）而非原始模型 ID
- `/resume` 支持 `/continue` 别名
- MCP 提示可作为 `/mcp__<server>__<prompt>` 命令使用（参见 [MCP 提示作为命令](#mcp-提示作为命令)）
- `/team-onboarding` 添加了自动生成队友上手指南（v2.1.101）
- `/tui` 命令添加了无闪烁全屏 TUI 渲染（v2.1.110）
- `/focus` 命令添加了聚焦视图切换；`Ctrl+O` 现在仅切换详细记录（v2.1.110）
- `/recap` 命令添加了手动触发会话上下文回顾（v2.1.108）
- `/undo` 添加为 `/rewind` 的别名（v2.1.108）
- `/proactive` 添加为 `/loop` 的别名（v2.1.105）
- `/effort` 获得交互式方向键滑块和新的 `xhigh` 级别（介于 `high` 和 `max` 之间）；Opus 4.7 计划的默认努力提升到 `xhigh`（v2.1.111）
- `/ultrareview` 添加了基于云的综合多代理代码审查（v2.1.111）
- `/less-permission-prompts` 添加了分析 Bash/MCP 工具调用并通过 `.claude/settings.json` 中的允许列表减少权限提示（v2.1.111）
- 自动模式不再需要 Max 订阅者在 Opus 4.7 上使用 `--enable-auto-mode` 标志（v2.1.112）

### `/team-onboarding` — 队友上手指南

> **v2.1.101 新增**

使用 `/team-onboarding` 从项目的本地 Claude Code 使用情况生成队友上手指南。该命令检查你的 `CLAUDE.md`、已安装的技能、子代理、钩子和最近的工作流，然后生成一份入职文档，帮助新开发者快速上手。

这是一个内置命令——无需安装。

**使用:**

```bash
claude /team-onboarding
```

生成的指南总结了：

- 来自 [`CLAUDE.md`](../02-memory/README.md) 的项目目的和关键约定
- 可用的[技能](../03-skills/README.md)及其自动调用时机
- 已配置的[子代理](../04-subagents/README.md)及其职责
- 在常见事件上运行的[钩子](../06-hooks/README.md)
- 新人应该了解的常见工作流

**可用性:** 在 Claude Code v2.1.101（2026年4月11日）中发布。

## 自定义命令（现为技能）

自定义斜杠命令已**合并到技能中**。两种方法都创建可以用 `/command-name` 调用的命令：

| 方法 | 位置 | 状态 |
|------|------|------|
| **技能（推荐）** | `.claude/skills/<name>/SKILL.md` | 当前标准 |
| **旧命令** | `.claude/commands/<name>.md` | 仍然有效 |

如果技能和命令同名，**技能优先**。例如，当 `.claude/commands/review.md` 和 `.claude/skills/review/SKILL.md` 同时存在时，使用技能版本。

### 迁移路径

你现有的 `.claude/commands/` 文件无需更改即可继续工作。要迁移到技能：

**之前（命令）:**
```
.claude/commands/optimize.md
```

**之后（技能）:**
```
.claude/skills/optimize/SKILL.md
```

### 为什么选择技能？

技能相比旧命令提供了额外功能：

- **目录结构**: 捆绑脚本、模板和参考文件
- **自动调用**: Claude 可以在相关时自动触发技能
- **调用控制**: 选择用户、Claude 或两者都可以调用
- **子代理执行**: 使用 `context: fork` 在隔离上下文中运行技能
- **渐进式展示**: 仅在需要时加载额外文件

### 将自定义命令创建为技能

创建一个包含 `SKILL.md` 文件的目录：

```bash
mkdir -p .claude/skills/my-command
```

**文件:** `.claude/skills/my-command/SKILL.md`

```yaml
---
name: my-command
description: 此命令的功能以及何时使用它
---

# 我的命令

Claude 在调用此命令时应遵循的指令。

1. 第一步
2. 第二步
3. 第三步
```

### Frontmatter 参考

| 字段 | 用途 | 默认值 |
|------|------|--------|
| `name` | 命令名称（变成 `/name`） | 目录名 |
| `description` | 简短描述（帮助 Claude 知道何时使用） | 第一段 |
| `argument-hint` | 自动补全的预期参数 | 无 |
| `allowed-tools` | 命令可以无需权限使用的工具 | 继承 |
| `model` | 使用的特定模型 | 继承 |
| `disable-model-invocation` | 如果为 `true`，仅用户可调用（Claude 不能） | `false` |
| `user-invocable` | 如果为 `false`，从 `/` 菜单中隐藏 | `true` |
| `context` | 设为 `fork` 在隔离子代理中运行 | 无 |
| `agent` | 使用 `context: fork` 时的代理类型 | `general-purpose` |
| `hooks` | 技能范围的钩子（PreToolUse、PostToolUse、Stop） | 无 |

### 参数

命令可以接收参数：

**所有参数用 `$ARGUMENTS`:**

```yaml
---
name: fix-issue
description: 按编号修复 GitHub issue
---

按照我们的编码标准修复 issue #$ARGUMENTS
```

使用: `/fix-issue 123` → `$ARGUMENTS` 变为 "123"

**单个参数用 `$0`、`$1` 等:**

```yaml
---
name: review-pr
description: 按优先级审查 PR
---

以优先级 $1 审查 PR #$0
```

使用: `/review-pr 456 high` → `$0`="456"，`$1`="high"

### 使用 Shell 命令的动态上下文

使用 `!`command`` 在提示前执行 bash 命令：

```yaml
---
name: commit
description: 带上下文创建 git commit
allowed-tools: Bash(git *)
---

## 上下文

- 当前 git 状态: !`git status`
- 当前 git diff: !`git diff HEAD`
- 当前分支: !`git branch --show-current`
- 最近提交: !`git log --oneline -5`

## 你的任务

根据上述更改，创建一个 git commit。
```

### 文件引用

使用 `@` 包含文件内容：

```markdown
审查 @src/utils/helpers.js 中的实现
比较 @src/old-version.js 和 @src/new-version.js
```

## 插件命令

插件可以提供自定义命令：

```
/plugin-name:command-name
```

或者当没有命名冲突时简单使用 `/command-name`。

**示例:**
```bash
/frontend-design:frontend-design
/commit-commands:commit
```

## MCP 提示作为命令

MCP 服务器可以将提示暴露为斜杠命令：

```
/mcp__<server-name>__<prompt-name> [arguments]
```

**示例:**
```bash
/mcp__github__list_prs
/mcp__github__pr_review 456
/mcp__jira__create_issue "Bug title" high
```

### MCP 权限语法

在权限中控制 MCP 服务器访问：

- `mcp__github` - 访问整个 GitHub MCP 服务器
- `mcp__github__*` - 通配符访问所有工具
- `mcp__github__get_issue` - 特定工具访问

## 命令架构

```mermaid
graph TD
    A["用户输入: /command-name"] --> B{"命令类型?"}
    B -->|内置| C["执行内置命令"]
    B -->|技能| D["加载 SKILL.md"]
    B -->|插件| E["加载插件命令"]
    B -->|MCP| F["执行 MCP 提示"]

    D --> G["解析 Frontmatter"]
    G --> H["替换变量"]
    H --> I["执行 Shell 命令"]
    I --> J["发送给 Claude"]
    J --> K["返回结果"]
```

## 命令生命周期

```mermaid
sequenceDiagram
    participant User as 用户
    participant Claude as Claude Code
    participant FS as 文件系统
    participant CLI as Shell/Bash

    User->>Claude: 输入 /optimize
    Claude->>FS: 搜索 .claude/skills/ 和 .claude/commands/
    FS-->>Claude: 返回 optimize/SKILL.md
    Claude->>Claude: 解析 frontmatter
    Claude->>CLI: 执行 !`command` 替换
    CLI-->>Claude: 命令输出
    Claude->>Claude: 替换 $ARGUMENTS
    Claude->>User: 处理提示
    Claude->>User: 返回结果
```

## 本文件夹中的可用命令

这些示例命令可以作为技能或旧命令安装。

### 1. `/optimize` - 代码优化

分析代码的性能问题、内存泄漏和优化机会。

**使用:**
```
/optimize
[粘贴你的代码]
```

### 2. `/pr` - Pull Request 准备

引导完成 PR 准备清单，包括代码检查、测试和提交格式化。

**使用:**
```
/pr
```

**截图:**
![/pr](pr-slash-command.png)

### 3. `/generate-api-docs` - API 文档生成器

从源代码生成综合 API 文档。

**使用:**
```
/generate-api-docs
```

### 4. `/commit` - 带上下文的 Git Commit

使用仓库的动态上下文创建 git commit。

**使用:**
```
/commit [可选消息]
```

### 5. `/push-all` - 暂存、提交和推送

暂存所有更改、创建提交并推送到远程，带安全检查。

**使用:**
```
/push-all
```

**安全检查:**
- 密钥: `.env*`、`*.key`、`*.pem`、`credentials.json`
- API 密钥: 检测真实密钥与占位符
- 大文件: 未使用 Git LFS 的 `>10MB` 文件
- 构建产物: `node_modules/`、`dist/`、`__pycache__/`

### 6. `/doc-refactor` - 文档重构

重构项目文档结构以提高清晰度和可访问性。

**使用:**
```
/doc-refactor
```

### 7. `/setup-ci-cd` - CI/CD 流水线设置

实现 pre-commit 钩子和 GitHub Actions 用于质量保证。

**使用:**
```
/setup-ci-cd
```

### 8. `/unit-test-expand` - 测试覆盖率扩展

通过针对未测试的分支和边界情况来增加测试覆盖率。

**使用:**
```
/unit-test-expand
```

## 安装

### 作为技能（推荐）

复制到你的技能目录：

```bash
# 创建技能目录
mkdir -p .claude/skills

# 为每个命令文件创建技能目录
for cmd in optimize pr commit; do
  mkdir -p .claude/skills/$cmd
  cp 01-slash-commands/$cmd.md .claude/skills/$cmd/SKILL.md
done
```

### 作为旧命令

复制到你的命令目录：

```bash
# 项目范围（团队）
mkdir -p .claude/commands
cp 01-slash-commands/*.md .claude/commands/

# 个人使用
mkdir -p ~/.claude/commands
cp 01-slash-commands/*.md ~/.claude/commands/
```

## 创建你自己的命令

### 技能模板（推荐）

创建 `.claude/skills/my-command/SKILL.md`：

```yaml
---
name: my-command
description: 此命令的功能。在 [触发条件] 时使用。
argument-hint: [optional-args]
allowed-tools: Bash(npm *), Read, Grep
---

# 命令标题

## 上下文

- 当前分支: !`git branch --show-current`
- 相关文件: @package.json

## 指令

1. 第一步
2. 带参数的第二步: $ARGUMENTS
3. 第三步

## 输出格式

- 如何格式化响应
- 包含什么内容
```

### 仅用户命令（无自动调用）

对于有副作用的命令，Claude 不应自动触发：

```yaml
---
name: deploy
description: 部署到生产环境
disable-model-invocation: true
allowed-tools: Bash(npm *), Bash(git *)
---

将应用部署到生产环境：

1. 运行测试
2. 构建应用
3. 推送到部署目标
4. 验证部署
```

## 最佳实践

| 应该做 | 不应该做 |
|--------|---------|
| 使用清晰、面向操作的名称 | 为一次性任务创建命令 |
| 包含带触发条件的 `description` | 在命令中构建复杂逻辑 |
| 保持命令专注于单一任务 | 硬编码敏感信息 |
| 对有副作用的命令使用 `disable-model-invocation` | 跳过 description 字段 |
| 使用 `!` 前缀获取动态上下文 | 假设 Claude 知道当前状态 |
| 在技能目录中组织相关文件 | 把所有东西放在一个文件中 |

## 故障排除

### 命令未找到

**解决方案:**
- 检查文件是否在 `.claude/skills/<name>/SKILL.md` 或 `.claude/commands/<name>.md`
- 验证 frontmatter 中的 `name` 字段匹配预期的命令名
- 重启 Claude Code 会话
- 运行 `/help` 查看可用命令

### 命令未按预期执行

**解决方案:**
- 添加更具体的指令
- 在技能文件中包含示例
- 如果使用 bash 命令，检查 `allowed-tools`
- 先用简单输入测试

### 技能与命令冲突

如果同名的两者都存在，**技能优先**。删除一个或重命名。

## 相关指南

- **[技能](../03-skills/)** - 技能的完整参考（自动调用的能力）
- **[记忆](../02-memory/)** - 使用 CLAUDE.md 的持久化上下文
- **[子代理](../04-subagents/)** - 委托的 AI 代理
- **[插件](../07-plugins/)** - 捆绑的命令集合
- **[钩子](../06-hooks/)** - 事件驱动的自动化

## 额外资源

- [官方交互模式文档](https://code.claude.com/docs/en/interactive-mode) - 内置命令参考
- [官方技能文档](https://code.claude.com/docs/en/skills) - 完整技能参考
- [CLI 参考](https://code.claude.com/docs/en/cli-reference) - 命令行选项
