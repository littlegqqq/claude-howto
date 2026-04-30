<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 功能目录

> Claude Code 所有功能的快速参考指南：命令、智能体、技能、插件和钩子。

**导航**：[命令](#斜杠命令) | [权限模式](#权限模式) | [子智能体](#子智能体) | [技能](#技能) | [插件](#插件) | [MCP 服务器](#mcp-服务器) | [钩子](#钩子) | [记忆文件](#记忆文件) | [新功能](#新功能2026-年-4-月)

---

## 总览

| 功能 | 内置 | 示例 | 合计 | 参考 |
|------|------|------|------|------|
| **斜杠命令** | 60+ | 8 | 68+ | [01-slash-commands/](01-slash-commands/) |
| **子智能体** | 6 | 11 | 17 | [04-subagents/](04-subagents/) |
| **技能** | 5 个内置 | 4 | 9 | [03-skills/](03-skills/) |
| **插件** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCP 服务器** | 1 | 8 | 9 | [05-mcp/](05-mcp/) |
| **钩子** | 28 个事件 | 8 | 8 | [06-hooks/](06-hooks/) |
| **记忆** | 7 种类型 | 3 | 3 | [02-memory/](02-memory/) |
| **合计** | **99** | **45** | **119** | |

---

## 斜杠命令

命令是用户调用的快捷方式，用于执行特定操作。

### 内置命令

| 命令 | 描述 | 使用场景 |
|------|------|----------|
| `/help` | 显示帮助信息 | 入门学习、了解命令 |
| `/btw` | 临时性旁问——不污染主上下文 | 快速的题外问题 |
| `/chrome` | 配置 Chrome 集成 | 浏览器自动化 |
| `/clear` | 清除对话历史 | 重新开始、减少上下文 |
| `/diff` | 交互式差异查看器 | 审查变更 |
| `/config` | 查看/编辑配置 | 自定义行为 |
| `/status` | 显示会话状态 | 检查当前状态 |
| `/agents` | 列出可用智能体 | 查看委派选项 |
| `/skills` | 列出可用技能 | 查看自动调用能力 |
| `/hooks` | 列出已配置的钩子 | 调试自动化流程 |
| `/insights` | 分析会话模式 | 优化会话 |
| `/install-slack-app` | 安装 Claude Slack 应用 | Slack 集成 |
| `/keybindings` | 自定义键盘快捷键 | 按键自定义 |
| `/mcp` | 列出 MCP 服务器 | 检查外部集成 |
| `/memory` | 查看已加载的记忆文件 | 调试上下文加载 |
| `/mobile` | 生成手机二维码 | 移动端访问 |
| `/passes` | 查看使用通行证 | 订阅信息 |
| `/plugin` | 管理插件 | 安装/移除扩展 |
| `/plan` | 进入规划模式 | 复杂实现方案 |
| `/proactive` | `/loop` 的别名（v2.1.105） | 与 `/loop` 相同 |
| `/recap` | 返回会话时显示会话回顾 | 离开后回来，了解之前做了什么 |
| `/rewind` | 回退到检查点 | 撤销更改、探索替代方案 |
| `/checkpoint` | 管理检查点 | 保存/恢复状态 |
| `/cost` | 快捷别名，打开 `/usage` 的费用标签页（v2.1.118+） | 监控支出 |
| `/context` | 显示上下文窗口使用情况 | 管理对话长度 |
| `/export` | 导出对话 | 保存以供参考 |
| `/extra-usage` | 配置额外使用限额 | 速率限制管理 |
| `/feedback` | 提交反馈或错误报告 | 报告问题 |
| `/login` | 使用 Anthropic 账号认证 | 访问功能 |
| `/logout` | 登出 | 切换账号 |
| `/sandbox` | 切换沙盒模式 | 安全执行命令 |
| `/doctor` | 运行诊断 | 排查问题 |
| `/reload-plugins` | 重新加载已安装的插件 | 插件管理 |
| `/release-notes` | 显示发布说明 | 查看新功能 |
| `/remote-control` | 启用远程控制 | 远程访问 |
| `/permissions` | 管理权限 | 控制访问 |
| `/session` | 管理会话 | 多会话工作流 |
| `/rename` | 重命名当前会话 | 整理会话 |
| `/resume` | 恢复上一个会话 | 继续工作 |
| `/todo` | 查看/管理待办列表 | 跟踪任务 |
| `/tui` | 切换全屏 TUI（文本用户界面）模式 | 在全屏终端或 tmux 中无闪烁渲染 |
| `/tasks` | 查看后台任务 | 监控异步操作 |
| `/copy` | 复制上一条响应到剪贴板 | 快速分享输出 |
| `/teleport` | 将会话传送到另一台机器 | 远程继续工作 |
| `/desktop` | 打开 Claude Desktop 应用 | 切换到桌面界面 |
| `/theme` | 更改颜色主题；v2.1.118 新增通过 `~/.claude/themes/<name>.json` 自定义命名主题（插件可附带 `themes/` 目录） | 自定义外观 |
| `/usage` | 使用量/费用/统计的规范命令——将 `/cost` 和 `/stats` 合并为单一的标签页视图（v2.1.118） | 监控配额和费用 |
| `/focus` | 切换焦点视图（无干扰输出显示） | 长任务期间减少视觉干扰 |
| `/fork` | 复刻当前对话 | 探索替代方案 |
| `/stats` | 快捷别名，打开 `/usage` 的统计标签页（v2.1.118+） | 查看会话指标 |
| `/statusline` | 配置状态栏 | 自定义状态显示 |
| `/stickers` | 查看会话贴纸 | 趣味奖励 |
| `/fast` | 切换快速输出模式 | 加快响应速度 |
| `/terminal-setup` | 配置终端集成 | 设置终端功能 |
| `/undo` | `/rewind` 的别名（v2.1.108） | 与 `/rewind` 相同 |
| `/upgrade` | 检查更新 | 版本管理 |
| `/team-onboarding` | 根据项目的 Claude Code 使用情况生成团队成员上手指南 | 新成员入职（v2.1.101） |
| `/ultraplan` | 将规划任务交给 Claude Code 云端会话以规划模式运行 | 重度规划卸载（研究预览，v2.1.91+） |
| `/ultrareview` | 对当前变更运行云端多智能体代码审查 | 合并前跨多个智能体的深度审查（v2.1.112） |
| `/less-permission-prompts` | 扫描会话记录并为常见只读工具建议优先级白名单 | 减少项目中重复的权限提示（v2.1.112） |

### 自定义命令（示例）

| 命令 | 描述 | 使用场景 | 作用域 | 安装方式 |
|------|------|----------|--------|----------|
| `/optimize` | 分析代码并进行优化 | 性能改进 | 项目 | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | 准备拉取请求 | 提交 PR 之前 | 项目 | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | 生成 API 文档 | 编写 API 文档 | 项目 | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | 创建带上下文的 Git 提交 | 提交变更 | 用户 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | 暂存、提交并推送 | 快速部署 | 用户 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | 重构文档结构 | 改善文档 | 项目 | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | 设置 CI/CD 流水线 | 新项目 | 项目 | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | 扩展测试覆盖率 | 改善测试 | 项目 | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **作用域**：`用户` = 个人工作流（`~/.claude/commands/`），`项目` = 团队共享（`.claude/commands/`）

**参考**：[01-slash-commands/](01-slash-commands/) | [官方文档](https://code.claude.com/docs/en/interactive-mode)

**快速安装（所有自定义命令）**：
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## 权限模式

Claude Code 支持 6 种权限模式，用于控制工具使用的授权方式。

| 模式 | 描述 | 使用场景 |
|------|------|----------|
| `default` | 每次工具调用都提示确认 | 标准交互使用 |
| `acceptEdits` | 自动接受文件编辑，其他操作需确认 | 可信的编辑工作流 |
| `plan` | 仅限只读工具，不可写入 | 规划和探索 |
| `auto` | 接受所有工具调用，无需提示 | 完全自主运行（研究预览） |
| `bypassPermissions` | 跳过所有权限检查 | CI/CD、无头环境 |
| `dontAsk` | 跳过需要权限确认的工具 | 非交互式脚本 |

> **注意**：`auto` 模式为研究预览功能（2026 年 3 月）。`bypassPermissions` 仅应在可信的沙盒环境中使用。

**参考**：[官方文档](https://code.claude.com/docs/en/permissions)

---

## 子智能体

具有隔离上下文的专业化 AI 助手，用于处理特定任务。

### 内置子智能体

| 智能体 | 描述 | 可用工具 | 模型 | 使用场景 |
|--------|------|----------|------|----------|
| **general-purpose** | 多步骤任务、研究 | 所有工具 | 继承模型 | 复杂研究、多文件任务 |
| **Plan** | 实现方案规划 | Read、Glob、Grep、Bash | 继承模型 | 架构设计、方案规划 |
| **Explore** | 代码库探索 | Read、Glob、Grep | Haiku 4.5 | 快速搜索、理解代码 |
| **Bash** | 命令执行 | Bash | 继承模型 | Git 操作、终端任务 |
| **statusline-setup** | 状态栏配置 | Bash、Read、Write | Sonnet 4.6 | 配置状态栏显示 |
| **Claude Code Guide** | 帮助和文档 | Read、Glob、Grep | Haiku 4.5 | 获取帮助、学习功能 |

### 子智能体配置字段

| 字段 | 类型 | 描述 |
|------|------|------|
| `name` | string | 智能体标识符 |
| `description` | string | 智能体功能描述 |
| `model` | string | 模型覆盖（如 `haiku-4.5`） |
| `tools` | array | 允许的工具列表 |
| `effort` | string | 推理努力级别（`low`、`medium`、`high`） |
| `initialPrompt` | string | 智能体启动时注入的系统提示词 |
| `disallowedTools` | array | 明确禁止该智能体使用的工具 |

### 自定义子智能体（示例）

| 智能体 | 描述 | 使用场景 | 作用域 | 安装方式 |
|--------|------|----------|--------|----------|
| `code-reviewer` | 全面的代码质量审查 | 代码审查会话 | 项目 | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `code-architect` | 功能架构设计 | 新功能规划 | 项目 | `cp 04-subagents/code-architect.md .claude/agents/` |
| `code-explorer` | 深度代码库分析 | 理解现有功能 | 项目 | `cp 04-subagents/code-explorer.md .claude/agents/` |
| `clean-code-reviewer` | 整洁代码原则审查 | 可维护性审查 | 项目 | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | 测试策略与覆盖率 | 测试规划 | 项目 | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 技术文档编写 | API 文档、指南 | 项目 | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | 安全导向审查 | 安全审计 | 项目 | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | 完整功能实现 | 功能开发 | 项目 | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 根因分析 | 缺陷调查 | 用户 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL 查询、数据分析 | 数据任务 | 用户 | `cp 04-subagents/data-scientist.md .claude/agents/` |
| `performance-optimizer` | 性能分析与调优 | 瓶颈调查 | 项目 | `cp 04-subagents/performance-optimizer.md .claude/agents/` |

> **作用域**：`用户` = 个人（`~/.claude/agents/`），`项目` = 团队共享（`.claude/agents/`）

**参考**：[04-subagents/](04-subagents/) | [官方文档](https://code.claude.com/docs/en/sub-agents)

**快速安装（所有自定义智能体）**：
```bash
cp 04-subagents/*.md .claude/agents/
```

---

## 技能

具有指令、脚本和模板的自动调用能力。

### 示例技能

| 技能 | 描述 | 自动调用时机 | 作用域 | 安装方式 |
|------|------|------------|--------|----------|
| `code-review` | 全面的代码审查 | "审查这段代码"、"检查质量" | 项目 | `cp -r 03-skills/code-review .claude/skills/` |
| `brand-voice` | 品牌一致性检查 | 撰写营销文案时 | 项目 | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 文档生成器 | "生成文档"、"编写 API 文档" | 项目 | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 系统化代码重构（Martin Fowler 方法） | "重构这个"、"整理代码" | 用户 | `cp -r 03-skills/refactor ~/.claude/skills/` |

> **作用域**：`用户` = 个人（`~/.claude/skills/`），`项目` = 团队共享（`.claude/skills/`）

### 技能结构

```
~/.claude/skills/skill-name/
├── SKILL.md          # 技能定义与指令
├── scripts/          # 辅助脚本
└── templates/        # 输出模板
```

### 技能前言字段

技能在 `SKILL.md` 中支持 YAML 前言配置：

| 字段 | 类型 | 描述 |
|------|------|------|
| `name` | string | 技能显示名称 |
| `description` | string | 技能功能描述 |
| `autoInvoke` | array | 自动调用的触发短语 |
| `effort` | string | 推理努力级别（`low`、`medium`、`high`） |
| `shell` | string | 脚本使用的 Shell（`bash`、`zsh`、`sh`） |

**参考**：[03-skills/](03-skills/) | [官方文档](https://code.claude.com/docs/en/skills)

**快速安装（所有技能）**：
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### 内置技能

| 技能 | 描述 | 自动调用时机 |
|------|------|------------|
| `/simplify` | 审查代码质量 | 编写代码后 |
| `/batch` | 对多个文件运行提示词 | 批量操作 |
| `/debug` | 调试失败的测试/错误 | 调试会话 |
| `/loop` | 按间隔运行提示词 | 循环任务 |
| `/claude-api` | 使用 Claude API 构建应用 | API 开发 |

---

## 插件

命令、智能体、MCP 服务器和钩子的捆绑集合。

### 示例插件

| 插件 | 描述 | 组件 | 使用场景 | 作用域 | 安装方式 |
|------|------|------|----------|--------|----------|
| `pr-review` | PR 审查工作流 | 3 个命令、3 个智能体、GitHub MCP | 代码审查 | 项目 | `/plugin install pr-review` |
| `devops-automation` | 部署与监控 | 4 个命令、3 个智能体、K8s MCP | DevOps 任务 | 项目 | `/plugin install devops-automation` |
| `documentation` | 文档生成套件 | 4 个命令、3 个智能体、模板 | 文档编写 | 项目 | `/plugin install documentation` |

> **作用域**：`项目` = 团队共享，`用户` = 个人工作流

### 插件结构

```
.claude-plugin/
├── plugin.json       # 清单文件
├── commands/         # 斜杠命令
├── agents/           # 子智能体
├── skills/           # 技能
├── mcp/              # MCP 配置
├── hooks/            # 钩子脚本
└── scripts/          # 工具脚本
```

**参考**：[07-plugins/](07-plugins/) | [官方文档](https://code.claude.com/docs/en/plugins)

**插件管理命令**：
```bash
/plugin list              # 列出已安装的插件
/plugin install <name>    # 安装插件
/plugin remove <name>     # 移除插件
/plugin update <name>     # 更新插件
```

---

## MCP 服务器

Model Context Protocol 服务器，用于外部工具和 API 访问。

### 常用 MCP 服务器

| 服务器 | 描述 | 使用场景 | 作用域 | 安装方式 |
|--------|------|----------|--------|----------|
| **GitHub** | PR 管理、议题、代码 | GitHub 工作流 | 项目 | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQL 查询、数据访问 | 数据库操作 | 项目 | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 高级文件操作 | 复杂文件任务 | 用户 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | 团队通信 | 通知、更新 | 项目 | 在设置中配置 |
| **Google Docs** | 文档访问 | 文档编辑、审阅 | 项目 | 在设置中配置 |
| **Asana** | 项目管理 | 任务跟踪 | 项目 | 在设置中配置 |
| **Stripe** | 支付数据 | 财务分析 | 项目 | 在设置中配置 |
| **Memory** | 持久化记忆 | 跨会话回忆 | 用户 | 在设置中配置 |
| **Context7** | 库文档 | 最新文档查阅 | 内置 | 内置 |

> **作用域**：`项目` = 团队（`.mcp.json`），`用户` = 个人（`~/.claude.json`），`内置` = 预装

### MCP 配置示例

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**参考**：[05-mcp/](05-mcp/) | [MCP 协议文档](https://modelcontextprotocol.io)

**快速安装（GitHub MCP）**：
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## 钩子

基于事件驱动的自动化，在 Claude Code 事件发生时执行 Shell 命令。

### 钩子事件

| 事件 | 描述 | 触发时机 | 使用场景 |
|------|------|----------|----------|
| `SessionStart` | 会话开始/恢复 | 会话初始化 | 初始化任务 |
| `InstructionsLoaded` | 指令加载完成 | CLAUDE.md 或规则文件加载时 | 自定义指令处理 |
| `UserPromptSubmit` | 提示词处理之前 | 用户发送消息 | 输入验证 |
| `PreToolUse` | 工具执行之前 | 任何工具运行前 | 验证、日志记录 |
| `PermissionRequest` | 权限对话框显示 | 敏感操作前 | 自定义审批流程 |
| `PostToolUse` | 工具执行成功后 | 任何工具完成后 | 格式化、通知 |
| `PostToolUseFailure` | 工具执行失败 | 工具出错后 | 错误处理、日志记录 |
| `Notification` | 发送通知 | Claude 发送通知时 | 外部告警 |
| `SubagentStart` | 子智能体被派生 | 子智能体任务开始 | 初始化子智能体上下文 |
| `SubagentStop` | 子智能体完成 | 子智能体任务结束 | 链式操作 |
| `Stop` | Claude 完成响应 | 响应结束 | 清理、报告 |
| `StopFailure` | API 错误结束回合 | API 错误发生时 | 错误恢复、日志记录 |
| `TeammateIdle` | 队友智能体空闲 | 智能体团队协调 | 分配工作 |
| `TaskCompleted` | 任务标记完成 | 任务完成时 | 任务后处理 |
| `TaskCreated` | 通过 TaskCreate 创建任务 | 新任务创建时 | 任务跟踪、日志记录 |
| `ConfigChange` | 配置更新 | 设置被修改时 | 响应配置变更 |
| `CwdChanged` | 工作目录变更 | 目录改变时 | 目录特定的设置 |
| `FileChanged` | 监控的文件变更 | 文件被修改时 | 文件监控、重新构建 |
| `PreCompact` | 压缩操作之前 | 上下文压缩时 | 状态保存 |
| `PostCompact` | 压缩完成之后 | 压缩结束时 | 压缩后操作 |
| `WorktreeCreate` | 工作树正在创建 | Git 工作树创建时 | 设置工作树环境 |
| `WorktreeRemove` | 工作树正在移除 | Git 工作树移除时 | 清理工作树资源 |
| `Elicitation` | MCP 服务器请求输入 | MCP 请求交互时 | 输入验证 |
| `ElicitationResult` | 用户响应交互请求 | 用户响应时 | 响应处理 |
| `SessionEnd` | 会话终止 | 会话结束时 | 清理、保存状态 |

### 示例钩子

| 钩子 | 描述 | 事件 | 作用域 | 安装方式 |
|------|------|------|--------|----------|
| `validate-bash.py` | 命令验证 | PreToolUse:Bash | 项目 | `cp 06-hooks/validate-bash.py .claude/hooks/` |
| `security-scan.py` | 安全扫描 | PostToolUse:Write | 项目 | `cp 06-hooks/security-scan.py .claude/hooks/` |
| `format-code.sh` | 自动格式化 | PostToolUse:Write | 用户 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.py` | 提示词验证 | UserPromptSubmit | 项目 | `cp 06-hooks/validate-prompt.py .claude/hooks/` |
| `context-tracker.py` | Token 使用量跟踪 | Stop | 用户 | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `pre-commit.sh` | 预提交验证 | PreToolUse:Bash | 项目 | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | 命令日志记录 | PostToolUse:Bash | 用户 | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |
| `dependency-check.sh` | 清单文件变更时的漏洞扫描 | PostToolUse:Write | 项目 | `cp 06-hooks/dependency-check.sh .claude/hooks/` |

> **作用域**：`项目` = 团队（`.claude/settings.json`），`用户` = 个人（`~/.claude/settings.json`）

### 钩子配置

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "~/.claude/hooks/validate-bash.py"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "~/.claude/hooks/format-code.sh"
      }
    ]
  }
}
```

**参考**：[06-hooks/](06-hooks/) | [官方文档](https://code.claude.com/docs/en/hooks)

**快速安装（所有钩子）**：
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## 记忆文件

跨会话自动加载的持久化上下文。

### 记忆类型

| 类型 | 位置 | 作用域 | 使用场景 |
|------|------|--------|----------|
| **托管策略** | 组织托管策略 | 组织 | 强制执行组织范围的标准 |
| **项目** | `./CLAUDE.md` | 项目（团队） | 团队标准、项目上下文 |
| **项目规则** | `.claude/rules/` | 项目（团队） | 模块化项目规则 |
| **用户** | `~/.claude/CLAUDE.md` | 用户（个人） | 个人偏好 |
| **用户规则** | `~/.claude/rules/` | 用户（个人） | 模块化个人规则 |
| **本地** | `./CLAUDE.local.md` | 本地（git 忽略） | 特定机器覆盖（截至 2026 年 3 月官方文档未列出；可能为遗留功能） |
| **自动记忆** | 自动 | 会话 | 自动捕获的洞察和修正 |

> **作用域**：`组织` = 管理员管理，`项目` = 通过 git 与团队共享，`用户` = 个人偏好，`本地` = 不提交，`会话` = 自动管理

**参考**：[02-memory/](02-memory/) | [官方文档](https://code.claude.com/docs/en/memory)

**快速安装**：
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

---

## 新功能（2026 年 4 月）

| 功能 | 描述 | 使用方式 |
|------|------|----------|
| **/focus** | 切换焦点视图，实现无干扰的输出显示（v2.1.110） | 运行 `/focus` 以在长任务期间减少视觉干扰 |
| **/proactive** | `/loop` 的别名——相同的循环任务行为（v2.1.105） | `/proactive` 可与 `/loop` 互换使用 |
| **/recap** | 返回现有会话时显示会话回顾（v2.1.108） | 离开后运行 `/recap` 以获取之前操作的上下文 |
| **/tui** | 切换全屏 TUI（文本用户界面）模式，实现无闪烁渲染（v2.1.110） | 在全屏终端或 tmux 中使用 `/tui` |
| **/undo** | `/rewind` 的别名——回退到上一个检查点（v2.1.108） | `/undo` 可与 `/rewind` 互换使用 |
| **Monitor 工具** | 监控后台命令的 stdout 流并对事件做出响应，替代轮询（v2.1.98+） | 通过[高级功能](09-advanced-features/)使用 Monitor 工具 |
| **/team-onboarding** | 根据项目的 Claude Code 设置自动生成团队成员上手指南（v2.1.101） | 在项目中运行 `/team-onboarding` |
| **Ultraplan 自动创建** | 首次调用 `/ultraplan` 时自动创建云端环境——无需手动设置（v2.1.101） | 使用 `/ultraplan <prompt>` |
| **远程控制** | 通过 API 远程控制 Claude Code 会话 | 使用远程控制 API 以编程方式发送提示词并接收响应 |
| **Web 会话** | 在浏览器环境中运行 Claude Code | 通过 `claude web` 或 Anthropic 控制台访问 |
| **桌面应用** | Claude Code 原生桌面应用程序 | 使用 `/desktop` 或从 Anthropic 官网下载 |
| **智能体团队** | 协调多个智能体协作处理相关任务 | 配置队友智能体进行协作并共享上下文 |
| **任务列表** | 后台任务管理与监控 | 使用 `/tasks` 查看和管理后台操作 |
| **提示词建议** | 上下文感知的命令建议 | 建议会根据当前上下文自动出现 |
| **Git 工作树** | 隔离的 Git 工作树，用于并行开发 | 使用工作树命令进行安全的并行分支工作 |
| **沙盒化** | 隔离的执行环境，保障安全 | 使用 `/sandbox` 切换；在受限环境中运行命令 |
| **MCP OAuth** | MCP 服务器的 OAuth 认证 | 在 MCP 服务器设置中配置 OAuth 凭证以实现安全访问 |
| **MCP 工具搜索** | 动态搜索和发现 MCP 工具 | 使用工具搜索在已连接的服务器中查找可用的 MCP 工具 |
| **定时任务** | 使用 `/loop` 和 cron 工具设置循环任务 | 使用 `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome 集成** | 使用无头 Chromium 进行浏览器自动化 | 使用 `--chrome` 标志或 `/chrome` 命令 |
| **键盘自定义** | 自定义键绑定，支持组合键 | 使用 `/keybindings` 或编辑 `~/.claude/keybindings.json` |
| **Auto 模式** | 完全自主运行，无需权限提示（研究预览） | 使用 `--mode auto` 或 `/permissions auto`；2026 年 3 月 |
| **频道** | 多频道通信（Telegram、Slack 等）（研究预览） | 配置频道插件；2026 年 3 月 |
| **语音输入** | 通过语音输入提示词 | 使用麦克风图标或语音键绑定 |
| **Agent 钩子类型** | 派生子智能体而非运行 Shell 命令的钩子 | 在钩子配置中设置 `"type": "agent"` |
| **Prompt 钩子类型** | 向对话中注入提示词文本的钩子 | 在钩子配置中设置 `"type": "prompt"` |
| **MCP 交互请求** | MCP 服务器可在工具执行期间请求用户输入 | 通过 `Elicitation` 和 `ElicitationResult` 钩子事件处理 |
| **插件 LSP 支持** | 通过插件集成语言服务器协议 | 在 `plugin.json` 中配置 LSP 服务器以获取编辑器功能 |
| **托管 Drop-in 配置** | 组织管理的 Drop-in 配置（v2.1.83） | 由管理员通过托管策略配置；自动应用到所有用户 |

---

## 快速参考矩阵

### 功能选择指南

| 需求 | 推荐功能 | 原因 |
|------|----------|------|
| 快速快捷操作 | 斜杠命令 | 手动触发、立即执行 |
| 持久化上下文 | 记忆 | 自动加载 |
| 复杂自动化 | 技能 | 自动调用 |
| 专业化任务 | 子智能体 | 隔离上下文 |
| 外部数据 | MCP 服务器 | 实时访问 |
| 事件自动化 | 钩子 | 事件触发 |
| 完整解决方案 | 插件 | 一体化捆绑 |

### 安装优先级

| 优先级 | 功能 | 命令 |
|--------|------|------|
| 1. 必备 | 记忆 | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 日常使用 | 斜杠命令 | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 质量保障 | 子智能体 | `cp 04-subagents/*.md .claude/agents/` |
| 4. 自动化 | 钩子 | `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5. 外部集成 | MCP | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 高级功能 | 技能 | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 完整方案 | 插件 | `/plugin install pr-review` |

---

## 一键安装全部

从本仓库安装所有示例：

```bash
# 创建目录
mkdir -p .claude/{commands,agents,skills} ~/.claude/{hooks,skills}

# 安装所有功能
cp 01-slash-commands/*.md .claude/commands/ && \
cp 02-memory/project-CLAUDE.md ./CLAUDE.md && \
cp -r 03-skills/* ~/.claude/skills/ && \
cp 04-subagents/*.md .claude/agents/ && \
cp 06-hooks/*.sh ~/.claude/hooks/ && \
chmod +x ~/.claude/hooks/*.sh
```

---

## 其他资源

- [Claude Code 官方文档](https://code.claude.com/docs/en/overview)
- [MCP 协议规范](https://modelcontextprotocol.io)
- [学习路线图](LEARNING-ROADMAP.md)
- [主 README](README.md)

---

**最后更新**：2026 年 4 月 24 日
**Claude Code 版本**：2.1.119
**来源**：
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/hooks
- https://github.com/anthropics/claude-code/releases/tag/v2.1.118
**兼容模型**：Claude Sonnet 4.6、Claude Opus 4.7、Claude Haiku 4.5