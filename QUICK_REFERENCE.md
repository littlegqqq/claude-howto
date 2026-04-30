<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 示例 - 快速参考卡

## 🚀 快速安装命令

### 斜杠命令
```bash
# 安装全部
cp 01-slash-commands/*.md .claude/commands/

# 安装特定命令
cp 01-slash-commands/optimize.md .claude/commands/
```

### Memory
```bash
# 项目记忆
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 个人记忆
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

### Skills
```bash
# 个人技能
cp -r 03-skills/code-review ~/.claude/skills/

# 项目技能
cp -r 03-skills/code-review .claude/skills/
```

### Subagents
```bash
# 安装全部
cp 04-subagents/*.md .claude/agents/

# 安装特定子代理
cp 04-subagents/code-reviewer.md .claude/agents/
```

### MCP
```bash
# 设置凭证
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 安装配置（项目作用域）
cp 05-mcp/github-mcp.json .mcp.json

# 或用户作用域：添加到 ~/.claude.json
```

### Hooks
```bash
# 安装钩子
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 在设置中配置（~/.claude/settings.json）
```

### Plugins
```bash
# 从示例安装（如果已发布）
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

### Checkpoints
```bash
# 检查点在每次用户提示时自动创建
# 要回退，按两次 Esc 或使用：
/rewind

# 然后选择：恢复代码和对话、恢复对话、
# 恢复代码、从此处总结，或取消
```

### 高级功能
```bash
# 在设置中配置（.claude/settings.json）
# 参见 09-advanced-features/config-examples.json

# 规划模式
/plan Task description

# 权限模式（使用 --permission-mode 标志）
# default        - 对危险操作请求批准
# acceptEdits    - 自动接受文件编辑，其他操作请求批准
# plan           - 只读分析，不进行修改
# dontAsk        - 接受所有操作，除了危险操作
# auto           - 后台分类器自动决定权限
# bypassPermissions - 接受所有操作（需要 --dangerously-skip-permissions）

# 会话管理
/resume                # 恢复之前的对话
/rename "name"         # 为当前会话命名
/fork                  # 分叉当前会话
claude -c              # 继续最近的对话
claude -r "session"    # 通过名称/ID 恢复会话
```

---

## 📋 功能速查表

| 功能 | 安装路径 | 使用方式 |
|------|----------|----------|
| **斜杠命令 (55+)** | `.claude/commands/*.md` | `/command-name` |
| **Memory** | `./CLAUDE.md` | 自动加载 |
| **Skills** | `.claude/skills/*/SKILL.md` | 自动调用 |
| **Subagents** | `.claude/agents/*.md` | 自动委派 |
| **MCP** | `.mcp.json`（项目）或 `~/.claude.json`（用户） | `/mcp__server__action` |
| **Hooks (28 个事件)** | `~/.claude/hooks/*.sh` | 事件触发（5 种类型） |
| **Plugins** | 通过 `/plugin install` | 打包所有功能 |
| **Checkpoints** | 内置 | `Esc+Esc` 或 `/rewind` |
| **规划模式** | 内置 | `/plan <task>` |
| **权限模式 (6)** | 内置 | `--allowedTools`、`--permission-mode` |
| **会话** | 内置 | `/session <command>` |
| **后台任务** | 内置 | 在后台运行 |
| **远程控制** | 内置 | WebSocket API |
| **Web 会话** | 内置 | `claude web` |
| **Git Worktrees** | 内置 | `/worktree` |
| **自动记忆** | 内置 | 自动保存到 CLAUDE.md |
| **任务列表** | 内置 | `/task list` |
| **内置技能 (5)** | 内置 | `/simplify`、`/loop`、`/claude-api`、`/voice`、`/browse` |

---

## 🎯 常见使用场景

### 代码审查
```bash
# 方法一：斜杠命令
cp 01-slash-commands/optimize.md .claude/commands/
# 使用：/optimize

# 方法二：子代理
cp 04-subagents/code-reviewer.md .claude/agents/
# 使用：自动委派

# 方法三：技能
cp -r 03-skills/code-review ~/.claude/skills/
# 使用：自动调用

# 方法四：插件（最佳方案）
/plugin install pr-review
# 使用：/review-pr
```

### 文档编写
```bash
# 斜杠命令
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# 子代理
cp 04-subagents/documentation-writer.md .claude/agents/

# 技能
cp -r 03-skills/doc-generator ~/.claude/skills/

# 插件（完整解决方案）
/plugin install documentation
```

### DevOps
```bash
# 完整插件
/plugin install devops-automation

# 命令：/deploy、/rollback、/status、/incident
```

### 团队标准
```bash
# 项目记忆
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 为你的团队编辑
vim CLAUDE.md
```

### 自动化与钩子
```bash
# 安装钩子（28 个事件，5 种类型：command、http、mcp_tool、prompt、agent）
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 示例：
# - 预提交测试：pre-commit.sh
# - 自动格式化代码：format-code.sh
# - 安全扫描：security-scan.sh

# Auto Mode 用于完全自主的工作流
claude --enable-auto-mode -p "Refactor and test the auth module"
# 或在交互模式中使用 Shift+Tab 切换模式
```

### 安全重构
```bash
# 检查点在每次提示前自动创建
# 尝试重构
# 如果成功：继续
# 如果失败：按 Esc+Esc 或使用 /rewind 回退
```

### 复杂实现
```bash
# 使用规划模式
/plan Implement user authentication system

# Claude 创建详细计划
# 审查并批准
# Claude 系统性地实现
```

### CI/CD 集成
```bash
# 以无头模式运行（非交互式）
claude -p "Run all tests and generate report"

# 使用权限模式用于 CI
claude -p "Run tests" --permission-mode dontAsk

# 使用 Auto Mode 用于完全自主的 CI 任务
claude --enable-auto-mode -p "Run tests and fix failures"

# 使用钩子进行自动化
# 参见 09-advanced-features/README.md
```

### 学习与实验
```bash
# 使用 plan 模式进行安全分析
claude --permission-mode plan

# 安全地实验 - 检查点会自动创建
# 如果需要回退：按 Esc+Esc 或使用 /rewind
```

### Agent Teams
```bash
# 启用代理团队
export CLAUDE_AGENT_TEAMS=1

# 或在 settings.json 中
{ "agentTeams": { "enabled": true } }

# 开始使用："Implement feature X using a team approach"
```

### 定时任务
```bash
# 每 5 分钟运行一个命令
/loop 5m /check-status

# 一次性提醒
/loop 30m "remind me to check the deploy"
```

---

## 📁 文件位置参考

```
你的项目/
├── .claude/
│   ├── commands/              # 斜杠命令存放于此
│   ├── agents/                # 子代理存放于此
│   ├── skills/                # 项目技能存放于此
│   └── settings.json          # 项目设置（钩子等）
├── .mcp.json                  # MCP 配置（项目作用域）
├── CLAUDE.md                  # 项目记忆
└── src/
    └── api/
        └── CLAUDE.md          # 目录级别的记忆

用户主目录/
├── .claude/
│   ├── commands/              # 个人命令
│   ├── agents/                # 个人代理
│   ├── skills/                # 个人技能
│   ├── hooks/                 # 钩子脚本
│   ├── settings.json          # 用户设置
│   ├── managed-settings.d/    # 托管设置（企业/组织）
│   └── CLAUDE.md              # 个人记忆
└── .claude.json               # 个人 MCP 配置（用户作用域）
```

---

## 🔍 查找示例

### 按类别
- **斜杠命令**：`01-slash-commands/`
- **Memory**：`02-memory/`
- **Skills**：`03-skills/`
- **Subagents**：`04-subagents/`
- **MCP**：`05-mcp/`
- **Hooks**：`06-hooks/`
- **Plugins**：`07-plugins/`
- **Checkpoints**：`08-checkpoints/`
- **高级功能**：`09-advanced-features/`
- **CLI**：`10-cli/`

### 按使用场景
- **性能优化**：`01-slash-commands/optimize.md`
- **安全审查**：`04-subagents/secure-reviewer.md`
- **测试**：`04-subagents/test-engineer.md`
- **文档**：`03-skills/doc-generator/`
- **DevOps**：`07-plugins/devops-automation/`

### 按复杂度
- **简单**：斜杠命令
- **中等**：子代理、Memory
- **高级**：Skills、Hooks
- **完整方案**：Plugins

---

## 🎓 学习路径

### 第 1 天
```bash
# 阅读概览
cat README.md

# 安装一个命令
cp 01-slash-commands/optimize.md .claude/commands/

# 试用
/optimize
```

### 第 2-3 天
```bash
# 设置记忆
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
vim CLAUDE.md

# 安装子代理
cp 04-subagents/code-reviewer.md .claude/agents/
```

### 第 4-5 天
```bash
# 设置 MCP
export GITHUB_TOKEN="your_token"
cp 05-mcp/github-mcp.json .mcp.json

# 尝试 MCP 命令
/mcp__github__list_prs
```

### 第 2 周
```bash
# 安装技能
cp -r 03-skills/code-review ~/.claude/skills/

# 让它自动调用
# 只需说："Review this code for issues"
```

### 第 3 周及以后
```bash
# 安装完整插件
/plugin install pr-review

# 使用打包的功能
/review-pr
/check-security
/check-tests
```

---

## 新功能（2026 年 3 月）

| 功能 | 描述 | 使用方式 |
|------|------|----------|
| **Auto Mode** | 带后台分类器的完全自主操作 | `--enable-auto-mode` 标志，`Shift+Tab` 切换模式 |
| **Channels** | Discord 和 Telegram 集成 | `--channels` 标志，Discord/Telegram 机器人 |
| **语音听写** | 对 Claude 说出命令和上下文 | `/voice` 命令 |
| **Hooks (28 个事件)** | 扩展的钩子系统，5 种类型 | command、http、mcp_tool、prompt、agent 钩子类型 |
| **MCP Elicitation** | MCP 服务器可在运行时请求用户输入 | 当服务器需要澄清时自动提示 |
| **Plugin LSP** | 插件的语言服务器协议支持 | `userConfig`、`${CLAUDE_PLUGIN_DATA}` 变量 |
| **远程控制** | 通过 WebSocket API 控制 Claude Code | `claude --remote` 用于外部集成 |
| **Web 会话** | 基于浏览器的 Claude Code 界面 | `claude web` 启动 |
| **桌面应用** | 原生桌面应用程序 | 从 claude.ai/download 下载 |
| **任务列表** | 管理后台任务 | `/task list`、`/task status <id>` |
| **自动记忆** | 从对话中自动保存记忆 | Claude 自动将关键上下文保存到 CLAUDE.md |
| **Git Worktrees** | 用于并行开发的隔离工作区 | `/worktree` 创建隔离工作区 |
| **模型选择** | 在 Sonnet 4.6、Opus 4.7 和 Haiku 4.5 之间切换 | `/model` 或 `--model` 标志 |
| **Agent Teams** | 协调多个代理完成任务 | 通过 `CLAUDE_AGENT_TEAMS=1` 环境变量启用 |
| **定时任务** | 使用 `/loop` 创建周期性任务 | `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome 集成** | 浏览器自动化 | `--chrome` 标志或 `/chrome` 命令 |
| **键盘自定义** | 自定义按键绑定 | `/keybindings` 命令 |

---

## 技巧与窍门

### 自定义
- 先按原样使用示例
- 根据需要进行修改
- 分享给团队前先测试
- 对配置进行版本控制

### 最佳实践
- 使用 Memory 管理团队标准
- 使用 Plugins 实现完整工作流
- 使用 Subagents 处理复杂任务
- 使用斜杠命令处理快速任务

### 故障排除
```bash
# 检查文件位置
ls -la .claude/commands/
ls -la .claude/agents/

# 验证 YAML 语法
head -20 .claude/agents/code-reviewer.md

# 测试 MCP 连接
echo $GITHUB_TOKEN
```

---

## 📊 功能矩阵

| 需求 | 使用方案 | 示例 |
|------|----------|------|
| 快捷操作 | 斜杠命令 (55+) | `01-slash-commands/optimize.md` |
| 团队标准 | Memory | `02-memory/project-CLAUDE.md` |
| 自动工作流 | Skill | `03-skills/code-review/` |
| 专门任务 | Subagent | `04-subagents/code-reviewer.md` |
| 外部数据 | MCP（+ Elicitation） | `05-mcp/github-mcp.json` |
| 事件自动化 | Hook（28 个事件，5 种类型） | `06-hooks/pre-commit.sh` |
| 完整解决方案 | Plugin（+ LSP 支持） | `07-plugins/pr-review/` |
| 安全实验 | Checkpoint | `08-checkpoints/checkpoint-examples.md` |
| 完全自主 | Auto Mode | `--enable-auto-mode` 或 `Shift+Tab` |
| 聊天集成 | Channels | `--channels`（Discord、Telegram） |
| CI/CD 流水线 | CLI | `10-cli/README.md` |

---

## 🔗 快速链接

- **主指南**：`README.md`
- **完整索引**：`INDEX.md`
- **摘要**：`EXAMPLES_SUMMARY.md`
- **原始指南**：`claude_concepts_guide.md`

---

## 📞 常见问题

**问：我应该使用哪个功能？**
答：从斜杠命令开始，根据需要添加其他功能。

**问：可以混合使用多个功能吗？**
答：当然可以！它们可以协同工作。Memory + Commands + MCP = 强大组合。

**问：如何与团队共享？**
答：将 `.claude/` 目录提交到 Git。

**问：密钥怎么处理？**
答：使用环境变量，绝不硬编码。

**问：可以修改示例吗？**
答：当然可以！它们是用来自定义的模板。

---

## ✅ 入门清单

新手入门清单：

- [ ] 阅读 `README.md`
- [ ] 安装 1 个斜杠命令
- [ ] 试用该命令
- [ ] 创建项目 `CLAUDE.md`
- [ ] 安装 1 个子代理
- [ ] 设置 1 个 MCP 集成
- [ ] 安装 1 个技能
- [ ] 尝试一个完整插件
- [ ] 根据需求自定义
- [ ] 分享给团队

---

**快速开始**：`cat README.md`

**完整索引**：`cat INDEX.md`

**本参考卡**：随时备用，方便快速查阅！

---
**最后更新**：2026 年 4 月 24 日
**Claude Code 版本**：2.1.119
**来源**：
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/commands
- https://github.com/anthropics/claude-code/releases/tag/v2.1.119
**兼容模型**：Claude Sonnet 4.6、Claude Opus 4.7、Claude Haiku 4.5
