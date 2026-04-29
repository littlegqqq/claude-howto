<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 高级功能

Claude Code 高级功能的综合指南，包括规划模式、扩展思考、自动模式、后台任务、权限模式、打印模式（非交互式）、会话管理、交互功能、频道、语音输入、远程控制、Web 会话、桌面应用、任务列表、提示建议、Git 工作树、沙箱、托管设置和配置。

## 目录

1. [概述](#overview)
2. [规划模式](#planning-mode)
3. [超级规划 (Cloud Plan Drafting)](#ultraplan-cloud-plan-drafting)
4. [扩展思考](#extended-thinking)
5. [自动模式](#auto-mode)
6. [后台任务](#background-tasks)
7. [监视工具（事件驱动流）](#monitor-tool-event-driven-streams)
8. [定时任务](#scheduled-tasks)
9. [权限模式](#permission-modes)
10. [无头模式](#headless-mode)
11. [会话管理](#session-management)
12. [交互功能](#interactive-features)
13. [TUI 模式（全屏）](#tui-mode-fullscreen)
14. [语音输入](#voice-dictation)
15. [频道](#channels)
16. [Chrome 集成](#chrome-integration)
17. [远程控制](#remote-control)
18. [Web 会话](#web-sessions)
19. [桌面应用](#desktop-app)
20. [任务列表](#task-list)
21. [提示建议](#prompt-suggestions)
22. [Git 工作树](#git-worktrees)
23. [沙箱](#sandboxing)
24. [托管设置（企业版）](#managed-settings-enterprise)
25. [配置和设置](#configuration-and-settings)
26. [智能体团队](#agent-teams)
27. [最佳实践](#best-practices)
28. [其他资源](#additional-resources)

---

## 概述

Claude Code 中的高级功能通过规划、推理、自动化和控制机制扩展了核心能力。这些功能支持复杂开发任务、代码审查、自动化和多会话管理等高级工作流。

**主要高级功能包括：**
- **规划模式**：在编码之前创建详细的实施计划
- **扩展思考**：针对复杂问题的深度推理
- **自动模式**：后台安全分类器在执行前审查每个操作（研究预览版）
- **后台任务**：在不阻塞对话的情况下运行长时间操作
- **权限模式**：控制 Claude 可以执行的操作（`default`、`acceptEdits`、`plan`、`auto`、`dontAsk`、`bypassPermissions`）
- **打印模式**：以非交互方式运行 Claude Code，用于自动化和 CI/CD（`claude -p`）
- **会话管理**：管理多个工作会话
- **交互功能**：键盘快捷键、多行输入和命令历史记录
- **语音输入**：支持 20 种语言 STT 的按住说话语音输入
- **频道**：MCP 服务器将消息推送到正在运行的会话中（研究预览版）
- **远程控制**：从 Claude.ai 或 Claude 应用控制 Claude Code
- **Web 会话**：在浏览器中通过 claude.ai/code 运行 Claude Code
- **桌面应用**：用于可视化差异审查和多会话的独立应用
- **任务列表**：在上下文压缩后仍持久保存的任务跟踪
- **提示建议**：基于上下文的智能命令建议
- **Git 工作树**：用于并行工作的隔离工作树分支
- **沙箱**：操作系统级别的文件系统和网络隔离
- **托管设置**：通过 plist、Registry 或托管文件进行企业部署
- **配置**：使用 JSON 配置文件自定义行为

---

## 规划模式

规划模式允许 Claude 在实施复杂任务之前进行周密思考，创建一个供你审查和批准的详细计划。

### 什么是规划模式？

规划模式是一种两阶段方法：
1. **规划阶段**：Claude 分析任务并创建详细的实施计划
2. **实施阶段**：批准后，Claude 执行该计划

### 何时使用规划模式

✅ 适合使用规划模式的场景：
- 复杂的多文件重构
- 新功能实现
- 架构变更
- 数据库迁移
- 重大 API 重新设计

❌ 不适合使用规划模式的场景：
- 简单的 bug 修复
- 格式化更改
- 单文件编辑
- 快速查询

### 激活规划模式

**斜杠命令**：
```bash
/plan Implement user authentication system
```

**CLI 标志**：
```bash
claude --permission-mode plan
```

**设置为默认值**：
```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

**键盘快捷键**：
- `Shift + Tab` - 切换权限模式（包括 plan）
- `Alt + M` - 切换权限模式（Windows/Linux）

### 规划模式的优势

- **结构化的实施方法**：实施步骤的详细分解
- **审查和批准**：在执行前批准或调整计划
- **风险识别**：在实施前识别潜在问题
- **清晰的阶段**：组织有序的实施阶段和里程碑

### 示例：功能实现

```
User: /plan Add a real-time notification system

Claude: I'll create a plan for implementing real-time notifications.

## Implementation Plan

### Phase 1: Infrastructure Setup
1. Install WebSocket libraries (socket.io)
2. Set up WebSocket server
3. Configure connection management

### Phase 2: Backend Implementation
4. Create notification service
5. Implement notification queue
6. Add database schema for notifications
7. Create API endpoints

### Phase 3: Frontend Integration
8. Set up WebSocket client
9. Create notification UI components
10. Implement notification state management
11. Add notification permissions

### Phase 4: Testing & Deployment
12. Write unit tests
13. Write integration tests
14. Update documentation
15. Deploy to staging

Estimated time: 3-4 hours
Files to modify: 12
New files to create: 8

Ready to proceed? (yes/no/modify plan)

User: yes

Claude: [Begins implementation following the plan]
```

### 计划修改

```
User: Modify the plan - skip the queue for now, we'll add it later

Claude: Updated plan:
[Shows modified plan with queue removed]

User: Looks good, proceed

Claude: [Implements modified plan]
```

### 规划模式配置

规划模式通过 CLI 标志或斜杠命令激活：

```bash
# Activate plan mode via CLI
claude --permission-mode plan

# Or use the /plan slash command inside the REPL
/plan Implement user authentication system
```

**用于规划的模型别名**：使用 `opusplan` 作为模型别名，让 Opus 用于规划、Sonnet 用于执行：

```bash
claude --model opusplan "design and implement the new API"
```

**在外部编辑器中编辑计划**：按 `Ctrl+G` 在外部编辑器中打开当前计划进行详细修改。

> **v2.1.112 更新**：计划文件现在以生成它们的提示命名（而不是随机单词），使其更容易浏览和复用。

---

## 超级规划 (Cloud Plan Drafting)

> **v2.1.101 新功能**：Ultraplan 现在在首次调用时会自动创建 Claude Code on the web 云环境——无需手动设置，无需等待容器预热即可开始起草。

> **注意**：Ultraplan 是研究预览版，需要 Claude Code v2.1.91 或更新版本。

`/ultraplan` 将规划任务从本地 CLI 移交给以规划模式运行的 Claude Code on the web 会话。Claude 在云端起草计划，同时你的终端可以自由地进行其他工作，然后你在浏览器中审查草案，并选择在哪里执行——在同一个云会话中或传送回你的终端。

### 何时使用 Ultraplan

- 你想要比终端更丰富的审查界面：内联评论、表情反应、大纲侧边栏和持久历史记录。
- 你想要在本地继续编码时无需干预地起草——云会话研究仓库并编写计划，而不会阻塞你的 CLI。
- 计划需要在执行前由利益相关者审查——可分享的 Web URL 比粘贴终端滚动输出更好。

### 要求

- Claude Code on the web 账户。
- GitHub 仓库（云会话克隆你的仓库以针对真实代码进行起草）。
- 在 Amazon Bedrock、Google Cloud Vertex AI 或 Microsoft Foundry 上**不可用**。

### 三种启动方式

- **命令**：`/ultraplan <prompt>` — 显式调用。
- **关键词**：在普通提示中的任何位置包含单词 `ultraplan`，Claude 会将请求路由到云端。
- **从本地计划启动**：在 Claude 本地完成计划后，在批准对话框中选择"No, refine with Ultraplan on Claude Code on the web"以将草案移交进行更深入的研究。

### 使用示例

```bash
/ultraplan migrate the auth service from sessions to JWTs
```

Claude 确认后启动云环境（v2.1.101+ 首次运行时自动创建），并返回一个可在浏览器中打开的会话链接。

### 状态指示器

| 状态 | 含义 |
|---|---|
| `◇ ultraplan` | Claude 正在研究你的代码库并起草计划 |
| `◇ ultraplan needs your input` | Claude 有一个澄清问题；打开会话链接来回复 |
| `◆ ultraplan ready` | 计划已准备好在浏览器中审查 |

### 执行选项

计划准备就绪后，你有两条执行路径。在浏览器中批准计划以在同一云会话中执行——Claude 在远程实施更改并从 Web UI 打开拉取请求。或选择"Approve plan and teleport back to terminal"在本地实施。终端传送对话框提供三个选项：

- **Implement here** — 在当前终端会话中运行已批准的计划。
- **Start new session** — 在同一工作目录中打开一个全新会话并在那里实施。
- **Cancel** — 将计划保存到文件中，以便以后继续。

> **警告**：当 ultraplan 启动时，Remote Control 会断开连接。两个功能共享 claude.ai/code 界面，因此一次只能激活一个。

---

## 扩展思考

扩展思考允许 Claude 在提供解决方案之前花更多时间推理复杂问题。

### 什么是扩展思考？

扩展思考是一种深思熟虑的逐步推理过程，其中 Claude：
- 分解复杂问题
- 考虑多种方法
- 评估权衡取舍
- 推理边界情况

### 激活扩展思考

**键盘快捷键**：
- `Option + T`（macOS）/ `Alt + T`（Windows/Linux）- 切换扩展思考

**自动激活**：
- 所有模型默认启用（Opus 4.7、Sonnet 4.6、Haiku 4.5）
- Opus 4.7：自适应推理，支持努力级别：`low`（○）、`medium`（◐）、`high`（●）、`xhigh`（仅 Opus 4.7，自 Opus 4.7 发布以来为 Claude Code 的默认值，2026-04-16）、`max`。Opus 4.6 和 Sonnet 4.6 也支持 `low`、`medium`、`high`、`max`（无 `xhigh`）。Opus 4.7 拥有 1M token 原生上下文窗口（1M 上下文修复在 v2.1.117 中落地——在此之前，`/context` 错误地将 Opus 4.7 按照 200K 窗口计数，导致过早触发自动压缩）。
- Opus 4.6 / Sonnet 4.6 的 Pro/Max 订阅者：默认努力级别在 v2.1.117 中从 `medium` 提升到 `high`。
- 其他模型：固定预算最高 31,999 个 token

**配置方法**：
- 切换：`Alt+T` / `Option+T`，或通过 `/config`
- 查看推理过程：`Ctrl+O`（详细模式）
- 设置努力级别：`/effort` 命令或 `--effort` 标志

**自定义预算**：
```bash
export MAX_THINKING_TOKENS=1024
```

**努力级别**（Opus 4.7、Opus 4.6 和 Sonnet 4.6 支持）：
```bash
export CLAUDE_CODE_EFFORT_LEVEL=xhigh   # low (○), medium (◐), high (●), xhigh (Opus 4.7 only, default), or max
```

**CLI 标志**：
```bash
claude --effort high "complex architectural review"
```

**斜杠命令**：
```
/effort high
```

> **注意：** 在提示中使用关键词"ultrathink"可激活深度推理模式。Opus 4.7、Opus 4.6 和 Sonnet 4.6 支持 `low`、`medium`、`high` 和 `max` 努力级别。`xhigh`（Opus 4.7 默认）仅限 Opus 4.7 使用。

### 扩展思考的优势

- **深度推理**：花更多时间思考复杂问题
- **更好的架构决策**：通过全面评估做出明智的选择
- **边界情况考量**：仔细思考潜在问题和场景
- **彻底的分析**：结构化的方法解决复杂问题

### 示例：架构决策

```
User: /think Should we use microservices or a monolith for our e-commerce platform?

Claude: Let me think through this architectural decision carefully...

<extended_thinking>
## Context Analysis
- Team size: 5 developers
- Expected traffic: 10k daily users initially
- Need to scale to 100k+ users
- Limited DevOps resources
- 6-month timeline to MVP

## Approach 1: Microservices
Pros:
+ Independent scaling
+ Technology flexibility
+ Team independence
+ Fault isolation

Cons:
- Operational complexity
- Network overhead
- Distributed tracing needed
- Requires DevOps expertise
- Slower initial development

## Approach 2: Monolith
Pros:
+ Simpler deployment
+ Faster initial development
+ Easier debugging
+ Lower operational overhead
+ Single codebase

Cons:
- Scaling challenges later
- All-or-nothing deployments
- Technology lock-in
- Potential for tight coupling

## Decision Factors
1. Team size (5 devs) → Favors monolith
2. DevOps resources (limited) → Favors monolith
3. Timeline (6 months) → Favors monolith
4. Future scale (100k users) → Achievable with monolith + caching
5. Complexity budget → Limited

## Recommendation
Start with a **modular monolith**:
- Single deployment unit
- Clear module boundaries
- Clean interfaces between modules
- Prepare for future extraction to microservices if needed

This gives you:
✓ Fast initial development
✓ Low operational complexity
✓ Option to extract services later
✓ Clear architecture

Migration path: When you reach 50k+ users or specific modules need independent scaling, extract those modules into microservices.
</extended_thinking>

**Recommendation**: Implement a modular monolith with clean module boundaries.

This approach balances your current constraints (team size, timeline, DevOps resources) with future scalability needs. You can migrate specific modules to microservices later when there's a clear business need.
```

### 扩展思考配置

扩展思考通过环境变量、键盘快捷键和 CLI 标志进行控制：

```bash
# Set thinking token budget
export MAX_THINKING_TOKENS=16000

# Set effort level (Opus 4.7, Opus 4.6, Sonnet 4.6): low (○), medium (◐), high (●), xhigh (Opus 4.7 only, default), or max
export CLAUDE_CODE_EFFORT_LEVEL=xhigh
```

在会话中使用 `Alt+T` / `Option+T` 切换，使用 `/effort` 设置努力级别，或通过 `/config` 配置。

---

## 自动模式

自动模式是一种研究预览版权限模式（2026 年 3 月），使用后台安全分类器在执行前审查每个操作。它允许 Claude 自主工作，同时阻止危险操作。

### 要求

- **计划**：Team、Enterprise 或 API（不适用于 Pro 或 Max 计划）
- **模型**：Claude Sonnet 4.6 或 Opus 4.7
- **提供商**：仅限 Anthropic API（不支持 Bedrock、Vertex 或 Foundry）
- **分类器**：在 Claude Sonnet 4.6 上运行（会产生额外的 token 成本）

### 启用自动模式

```bash
# Unlock auto mode with CLI flag (no longer required for Max subscribers on Opus 4.7 — access it directly)
claude --enable-auto-mode

# Then cycle to it with Shift+Tab in the REPL
```

> **v2.1.112 更新**：自动模式不再需要 `--enable-auto-mode` 标志。Max 订阅者可直接在 Opus 4.7 上访问它。

或将其设置为默认权限模式：

```bash
claude --permission-mode auto
```

通过配置设置：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 分类器的工作原理

后台分类器使用以下决策顺序评估每个操作：

1. **允许/拒绝规则** -- 首先检查显式权限规则
2. **只读/编辑自动批准** -- 文件读取和编辑自动通过
3. **分类器** -- 后台分类器审查操作
4. **回退** -- 在连续 3 次或总计 20 次阻止后回退为提示用户

### 默认阻止的操作

自动模式默认阻止以下操作：

| 阻止的操作 | 示例 |
|----------------|---------|
| 管道安装脚本 | `curl \| bash` |
| 向外部发送敏感数据 | API 密钥、通过网络发送凭据 |
| 生产环境部署 | 针对生产环境的部署命令 |
| 大规模删除 | 对大型目录执行 `rm -rf` |
| IAM 变更 | 权限和角色修改 |
| 强制推送到 main | `git push --force origin main` |

### 默认允许的操作

| 允许的操作 | 示例 |
|----------------|---------|
| 本地文件操作 | 读取、写入、编辑项目文件 |
| 声明的依赖安装 | 从清单文件执行 `npm install`、`pip install` |
| 只读 HTTP | 使用 `curl` 获取文档 |
| 推送到当前分支 | `git push origin feature-branch` |

### 配置自动模式

**将默认规则打印为 JSON**：
```bash
claude auto-mode defaults
```

**配置受信任的基础设施**，通过企业部署的 `autoMode.environment` 托管设置。这允许管理员定义受信任的 CI/CD 环境、部署目标和基础设施模式。

#### 使用 `"$defaults"` 扩展默认值 (v2.1.118)

自 v2.1.118 起，`autoMode.allow`、`autoMode.soft_deny` 和 `autoMode.environment` 接受 `"$defaults"` 标记，该标记会将你的规则**追加**到内置列表中，而不是替换它。在 v2.1.118 之前，任何用户定义的数组都会静默覆盖内置规则。

**之前（替换内置规则——v2.1.118 之前的行为）：**

```json
{
  "autoMode": {
    "allow": ["Bash(gh pr list:*)"]
  }
}
```

**之后（扩展内置规则——v2.1.118+）：**

```json
{
  "autoMode": {
    "allow": ["$defaults", "Bash(gh pr list:*)"],
    "soft_deny": ["$defaults", "Bash(kubectl delete:*)"],
    "environment": ["$defaults", "trusted-ci.internal"]
  }
}
```

使用 `"$defaults"` 保留已发布的基线规则，同时在其上叠加组织或项目特定的规则。

### 回退行为

当分类器不确定时，自动模式会回退为提示用户：
- 连续 **3 次**分类器阻止后
- 会话中总计 **20 次**分类器阻止后

这确保了当分类器无法自信地批准操作时，用户始终保持控制权。

### 无需 Team 计划即可预设自动模式等效权限

如果你没有 Team 计划或想要一种不使用后台分类器的更简单方法，你可以在 `~/.claude/settings.json` 中预设一组保守的安全权限规则基线。该脚本从只读和本地检查规则开始，然后仅在需要时让你选择启用编辑、测试、本地 git 写入、包安装和 GitHub 写入操作。

**文件：** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# Preview what would be added (no changes written)
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# Apply the conservative baseline
python3 09-advanced-features/setup-auto-mode-permissions.py

# Add more capability only when you need it
python3 09-advanced-features/setup-auto-mode-permissions.py --include-edits --include-tests
python3 09-advanced-features/setup-auto-mode-permissions.py --include-git-write --include-packages
```

该脚本在以下类别中添加规则：

| 类别 | 示例 |
|----------|---------|
| 核心只读工具 | `Read(*)`, `Glob(*)`, `Grep(*)`, `Agent(*)`, `WebSearch(*)`, `WebFetch(*)` |
| 本地检查 | `Bash(git status:*)`, `Bash(git log:*)`, `Bash(git diff:*)`, `Bash(cat:*)` |
| 可选编辑 | `Edit(*)`, `Write(*)`, `NotebookEdit(*)` |
| 可选测试/构建 | `Bash(pytest:*)`, `Bash(python3 -m pytest:*)`, `Bash(cargo test:*)` |
| 可选 git 写入 | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git stash:*)` |
| Git（本地写入） | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git checkout:*)` |
| 包管理器 | `Bash(npm install:*)`, `Bash(pip install:*)`, `Bash(cargo build:*)` |
| 构建和测试 | `Bash(make:*)`, `Bash(pytest:*)`, `Bash(go test:*)` |
| 常用 Shell | `Bash(ls:*)`, `Bash(cat:*)`, `Bash(find:*)`, `Bash(cp:*)`, `Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`, `Bash(gh pr create:*)`, `Bash(gh issue list:*)` |

危险操作（`rm -rf`、`sudo`、强制推送、`DROP TABLE`、`terraform destroy` 等）被故意排除在外。该脚本是幂等的——运行两次不会重复规则。

---

## 后台任务

后台任务允许在不阻塞对话的情况下执行长时间运行的操作。

### 什么是后台任务？

后台任务在你继续工作时异步运行：
- 长时间的测试套件
- 构建过程
- 数据库迁移
- 部署脚本
- 分析工具

**基本用法：**
```bash
User: Run tests in background

Claude: Started task bg-1234

/task list           # Show all tasks
/task status bg-1234 # Check progress
/task show bg-1234   # View output
/task cancel bg-1234 # Cancel task
```

### 启动后台任务

```
User: Run the full test suite in the background

Claude: Starting tests in background (task-id: bg-1234)
You can continue working while tests run.

[You can continue the conversation]

User: Meanwhile, let's refactor the auth module

Claude: [Works on auth module while tests run in background]

[Later, when tests complete]

Claude: 📢 Background task bg-1234 completed:
✅ 245 tests passed
❌ 3 tests failed
View results: /task show bg-1234
```

### 管理后台任务

**列出活跃任务**：
```
User: /task list

Active background tasks:
1. [bg-1234] Running tests (50% complete, 2min remaining)
2. [bg-1235] Building Docker image (25% complete, 8min remaining)
3. [bg-1236] Deploying to staging (90% complete, 30sec remaining)
```

**检查任务状态**：
```
User: /task status bg-1234

Task bg-1234: Running tests
Status: In progress
Progress: 120/245 tests (49%)
Started: 2025-11-08 10:30:15
Estimated completion: 2025-11-08 10:34:22
```

**查看任务输出**：
```
User: /task show bg-1234

[Shows live output from the test run]
```

**取消任务**：
```
User: /task cancel bg-1234

Cancelled background task bg-1234
```

### 示例：并行开发

```
User: Run the build in the background

Claude: Starting build... (task-id: bg-5001)

User: Also run the linter in background

Claude: Starting linter... (task-id: bg-5002)

User: While those run, let's implement the new API endpoint

Claude: [Implements API endpoint while build and linter run]

[10 minutes later]

Claude: 📢 Build completed successfully (bg-5001)
📢 Linter found 12 issues (bg-5002)

User: Show me the linter issues

Claude: [Shows linter output from bg-5002]
```

### 配置

```json
{
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5,
    "notifyOnCompletion": true,
    "autoCleanup": true,
    "logOutput": true
  }
}
```

---

## 监视工具（事件驱动流）

> **v2.1.98 新功能**：监视工具（Monitor tool）让 Claude 监视后台命令的标准输出，并在匹配事件出现时立即响应——替代了等待长时间运行进程时的轮询循环和 `sleep`。

Monitor 附加到任何写入标准输出的 shell 命令。命令的每一行标准输出都成为唤醒会话的通知。Claude 指定命令；运行程序流式传输输出并在事件触发时传递。相关内容请参阅[后台任务](#background-tasks)部分。

### 为什么它很重要

使用 `/loop` 或 `sleep` 轮询每个周期都会消耗一次完整的 API 往返，无论是否有任何变化。Monitor 在命令安静时保持沉默，消耗**零 token**。当事件发生时，Claude 立即响应——不会因等待下一次轮询而延迟发现。对于运行时间超过几分钟的任务，这比轮询循环更便宜也更快。

### 两种常见模式

**流过滤器**监视来自长时间运行源的持续输出。命令持续运行；每个匹配的行都是一个事件。

```bash
tail -f /var/log/app.log | grep --line-buffered "ERROR"
```

**轮询并发出过滤器**定期检查源，仅在发生变化时发出。适用于 API、数据库或任何没有原生流的内容。

```bash
last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
while true; do
  gh api "repos/owner/repo/issues/123/comments?since=$last" || true
  last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
  sleep 30
done
```

### 具体示例

"启动我的开发服务器并监控错误。" Claude 将服务器作为后台任务启动，附加 Monitor 过滤器（`tail -F server.log | grep --line-buffered -E "ERROR|FATAL"`），然后会话进入安静状态。当日志中出现错误行时，Claude 会立即唤醒，读取错误，并做出反应——重启服务器、修复 bug 或向你报告——而无需你去检查。

> **警告**：当管道输出到 `grep` 时，**务必**使用 `grep --line-buffered`。没有它，grep 会以 4KB 块缓冲标准输出，这可能导致在低流量流上延迟数分钟。这是 Monitor 在实践中最常见的故障原因——如果你的过滤器看起来不应该沉默却沉默了，请首先检查 `--line-buffered` 标志。

---

## 定时任务

定时任务让你在定期计划或一次性提醒上自动运行提示。任务是会话范围的——它们在 Claude Code 处于活动状态时运行，并在会话结束时清除。自 v2.1.72+ 起可用。

### `/loop` 命令

```bash
# Explicit interval
/loop 5m check if the deployment finished

# Natural language
/loop check build status every 30 minutes
```

标准的 5 字段 cron 表达式也支持精确调度。

### 一次性提醒

设置只触发一次的特定时间提醒：

```
remind me at 3pm to push the release branch
in 45 minutes, run the integration tests
```

### 管理定时任务

| 工具 | 描述 |
|------|-------------|
| `CronCreate` | 创建新的定时任务 |
| `CronList` | 列出所有活跃的定时任务 |
| `CronDelete` | 删除定时任务 |

**限制和行为**：
- 每个会话最多 **50 个定时任务**
- 会话范围——会话结束时清除
- 循环任务在 **3 天**后自动过期
- 任务仅在 Claude Code 运行时触发——错过的触发不会补执行

### 行为详情

| 方面 | 详情 |
|--------|--------|
| **循环抖动** | 最多为间隔的 10%（最大 15 分钟） |
| **一次性抖动** | 在 :00/:30 边界最多 90 秒 |
| **错过的触发** | 不补执行——如果 Claude Code 未运行则跳过 |
| **持久性** | 不跨重启持久化 |

### 云定时任务

使用 `/schedule` 创建在 Anthropic 基础设施上运行的云定时任务：

```
/schedule daily at 9am run the test suite and report failures
```

云定时任务跨重启持久化，不需要 Claude Code 在本地运行。

### 禁用定时任务

```bash
export CLAUDE_CODE_DISABLE_CRON=1
```

### 示例：监控部署

```
/loop 5m check the deployment status of the staging environment.
        If the deploy succeeded, notify me and stop looping.
        If it failed, show the error logs.
```

> **提示**：定时任务是会话范围的。对于在重启后仍然存在的持久自动化，请改用 CI/CD 管道、GitHub Actions 或桌面应用定时任务。

---

## 权限模式

权限模式控制 Claude 在未经明确批准的情况下可以执行哪些操作。

### 可用的权限模式

| 模式 | 行为 |
|---|---|
| `default` | 仅读取文件；所有其他操作需要提示 |
| `acceptEdits` | 读取和编辑文件；命令需要提示 |
| `plan` | 仅读取文件（研究模式，不编辑） |
| `auto` | 所有操作经后台安全分类器检查（研究预览版） |
| `bypassPermissions` | 所有操作，无权限检查（危险） |
| `dontAsk` | 仅执行预批准的工具；所有其他被拒绝 |

在 CLI 中使用 `Shift+Tab` 循环切换模式。使用 `--permission-mode` 标志或 `permissions.defaultMode` 设置来设定默认值。

### 激活方法

**键盘快捷键**：
```bash
Shift + Tab  # Cycle through all 6 modes
```

**斜杠命令**：
```bash
/plan                  # Enter plan mode
```

**CLI 标志**：
```bash
claude --permission-mode plan
claude --permission-mode auto
```

**设置**：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 权限模式示例

#### 默认模式
Claude 在执行重要操作前请求确认：

```
User: Fix the bug in auth.ts

Claude: I need to modify src/auth.ts to fix the bug.
The change will update the password validation logic.

Approve this change? (yes/no/show)
```

#### 规划模式
在执行前审查实施计划：

```
User: /plan Implement user authentication system

Claude: I'll create a plan for implementing authentication.

## Implementation Plan
[Detailed plan with phases and steps]

Ready to proceed? (yes/no/modify)
```

#### 接受编辑模式
自动接受文件修改：

```
User: acceptEdits
User: Fix the bug in auth.ts

Claude: [Makes changes without asking]
```

### 使用场景

**代码审查**：
```
User: claude --permission-mode plan
User: Review this PR and suggest improvements

Claude: [Reads code, provides feedback, but cannot modify]
```

**结对编程**：
```
User: claude --permission-mode default
User: Let's implement the feature together

Claude: [Asks for approval before each change]
```

**自动化任务**：
```
User: claude --permission-mode acceptEdits
User: Fix all linting issues in the codebase

Claude: [Auto-accepts file edits without asking]
```

---

## 无头模式

打印模式（`claude -p`）允许 Claude Code 在没有交互输入的情况下运行，非常适合自动化和 CI/CD。这是非交互式模式，取代了旧的 `--headless` 标志。

### 什么是打印模式？

打印模式支持：
- 自动化脚本执行
- CI/CD 集成
- 批处理
- 定时任务

### 以打印模式运行（非交互式）

```bash
# Run specific task
claude -p "Run all tests"

# Process piped content
cat error.log | claude -p "Analyze these errors"

# CI/CD integration (GitHub Actions)
- name: AI Code Review
  run: claude -p "Review PR"
```

### 打印模式的更多用法示例

```bash
# Run a specific task with output capture
claude -p "Run all tests and generate coverage report"

# With structured output
claude -p --output-format json "Analyze code quality"

# With input from stdin
echo "Analyze code quality" | claude -p "explain this"
```

### 示例：CI/CD 集成

**GitHub Actions**：
```yaml
# .github/workflows/code-review.yml
name: AI Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Claude Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p --output-format json \
            --max-turns 3 \
            "Review this PR for:
            - Code quality issues
            - Security vulnerabilities
            - Performance concerns
            - Test coverage
            Output results as JSON" > review.json

      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: JSON.stringify(review, null, 2)
            });
```

### 打印模式配置

打印模式（`claude -p`）支持多个自动化标志：

```bash
# Limit autonomous turns
claude -p --max-turns 5 "refactor this module"

# Structured JSON output
claude -p --output-format json "analyze this codebase"

# With schema validation
claude -p --json-schema '{"type":"object","properties":{"issues":{"type":"array"}}}' \
  "find bugs in this code"

# Disable session persistence
claude -p --no-session-persistence "one-off analysis"
```

---

## 会话管理

有效管理多个 Claude Code 会话。

### 会话管理命令

| 命令 | 描述 |
|---------|-------------|
| `/resume` | 通过 ID 或名称恢复对话 |
| `/rename` | 命名当前会话 |
| `/fork` | 将当前会话分叉为新分支 |
| `claude -c` | 继续最近的对话 |
| `claude -r "session"` | 通过名称或 ID 恢复会话 |

### 恢复会话

**继续上次对话**：
```bash
claude -c
```

**恢复已命名的会话**：
```bash
claude -r "auth-refactor" "finish this PR"
```

**重命名当前会话**（在 REPL 内）：
```
/rename auth-refactor
```

### 分叉会话

分叉会话以尝试替代方法，同时不丢失原始内容：

```
/fork
```

或从 CLI：
```bash
claude --resume auth-refactor --fork-session "try OAuth instead"
```

### 会话持久性

会话自动保存并可以恢复：

```bash
# Continue last conversation
claude -c

# Resume specific session by name or ID
claude -r "auth-refactor"

# Resume and fork for experimentation
claude --resume auth-refactor --fork-session "alternative approach"
```

### 会话回顾 (v2.1.108)

当你离开后返回会话时，Claude 可以显示已完成内容的简要回顾。对于禁用遥测的用户（Bedrock、Vertex、Foundry 用户），此功能默认启用。

**控制回顾行为：**

```bash
/recap                                 # manually trigger a recap
/config                                # toggle auto-recap on/off
```

或通过环境变量：
```bash
CLAUDE_CODE_ENABLE_AWAY_SUMMARY=0 claude   # disable recap
CLAUDE_CODE_ENABLE_AWAY_SUMMARY=1 claude   # force enable recap
```

---

## 交互功能

### 键盘快捷键

Claude Code 支持键盘快捷键以提高效率。以下是官方文档中的完整参考：

| 快捷键 | 描述 |
|----------|-------------|
| `Ctrl+C` | 取消当前输入/生成 |
| `Ctrl+D` | 退出 Claude Code |
| `Ctrl+G` | 在外部编辑器中编辑计划 |
| `Ctrl+L` | 清除终端屏幕 |
| `Ctrl+O` | 切换详细输出（查看推理过程） |
| `Ctrl+R` | 反向搜索历史 |
| `Ctrl+T` | 切换任务列表视图 |
| `Ctrl+B` | 后台运行任务 |
| `Esc+Esc` | 回退代码/对话 |
| `Shift+Tab` / `Alt+M` | 切换权限模式 |
| `Option+P` / `Alt+P` | 切换模型 |
| `Option+T` / `Alt+T` | 切换扩展思考 |

**行编辑（标准 readline 快捷键）：**

| 快捷键 | 操作 |
|----------|--------|
| `Ctrl + A` | 移动到行首 |
| `Ctrl + E` | 移动到行尾 |
| `Ctrl + K` | 剪切到行尾 |
| `Ctrl + U` | 剪切到行首 |
| `Ctrl + W` | 向后删除单词 |
| `Ctrl + Y` | 粘贴（yank） |
| `Tab` | 自动补全 |
| `↑ / ↓` | 命令历史 |

### 自定义键绑定

通过运行 `/keybindings` 创建自定义键盘快捷键，这将打开 `~/.claude/keybindings.json` 进行编辑（v2.1.18+）。

**配置格式**：

```json
{
  "$schema": "https://www.schemastore.org/claude-code-keybindings.json",
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+e": "chat:externalEditor",
        "ctrl+u": null,
        "ctrl+k ctrl+s": "chat:stash"
      }
    },
    {
      "context": "Confirmation",
      "bindings": {
        "ctrl+a": "confirmation:yes"
      }
    }
  ]
}
```

将绑定设置为 `null` 可以取消默认快捷键。

### 可用上下文

键绑定的作用域限于特定的 UI 上下文：

| 上下文 | 关键操作 |
|---------|-------------|
| **Chat** | `submit`, `cancel`, `cycleMode`, `modelPicker`, `thinkingToggle`, `undo`, `externalEditor`, `stash`, `imagePaste` |
| **Confirmation** | `yes`, `no`, `previous`, `next`, `nextField`, `cycleMode`, `toggleExplanation` |
| **Global** | `interrupt`, `exit`, `toggleTodos`, `toggleTranscript` |
| **Autocomplete** | `accept`, `dismiss`, `next`, `previous` |
| **HistorySearch** | `search`, `previous`, `next` |
| **Settings** | 上下文特定的设置导航 |
| **Tabs** | 标签页切换和管理 |
| **Help** | 帮助面板导航 |

总共有 18 个上下文，包括 `Transcript`、`Task`、`ThemePicker`、`Attachments`、`Footer`、`MessageSelector`、`DiffDialog`、`ModelPicker` 和 `Select`。

### 组合键支持

键绑定支持组合键序列（多键组合）：

```
"ctrl+k ctrl+s"   → Two-key sequence: press ctrl+k, then ctrl+s
"ctrl+shift+p"    → Simultaneous modifier keys
```

**按键语法**：
- **修饰键**：`ctrl`、`alt`（或 `opt`）、`shift`、`meta`（或 `cmd`）
- **大写字母暗示 Shift**：`K` 等同于 `shift+k`
- **特殊键**：`escape`、`enter`、`return`、`tab`、`space`、`backspace`、`delete`、方向键

### 保留键和冲突键

| 键 | 状态 | 说明 |
|-----|--------|-------|
| `Ctrl+C` | 保留 | 无法重新绑定（中断） |
| `Ctrl+D` | 保留 | 无法重新绑定（退出） |
| `Ctrl+B` | 终端冲突 | tmux 前缀键 |
| `Ctrl+A` | 终端冲突 | GNU Screen 前缀键 |
| `Ctrl+Z` | 终端冲突 | 进程挂起 |

> **提示**：如果快捷键不起作用，请检查是否与终端模拟器或多路复用器冲突。

### Tab 补全

Claude Code 提供智能 Tab 补全：

```
User: /rew<TAB>
→ /rewind

User: /plu<TAB>
→ /plugin

User: /plugin <TAB>
→ /plugin install
→ /plugin enable
→ /plugin disable
```

### 命令历史

访问之前的命令：

```
User: <↑>  # Previous command
User: <↓>  # Next command
User: Ctrl+R  # Search history

(reverse-i-search)`test': run all tests
```

### 多行输入

对于复杂查询，使用多行模式：

```bash
User: \
> Long complex prompt
> spanning multiple lines
> \end
```

**示例：**

```
User: \
> Implement a user authentication system
> with the following requirements:
> - JWT tokens
> - Email verification
> - Password reset
> - 2FA support
> \end

Claude: [Processes the multi-line request]
```

### 内联编辑

在发送前编辑命令：

```
User: Deploy to prodcution<Backspace><Backspace>uction

[Edit in-place before sending]
```

### Vim 模式

启用 Vi/Vim 键绑定用于文本编辑：

**激活**：
- 通过 `/config`（切换"Editor / Vim mode"）或在 `~/.claude/settings.json` 中设置 `editorMode: "vim"` 来启用。独立的 `/vim` 斜杠命令已被移除（参见 [issue #43370](https://github.com/anthropics/claude-code/issues/43370)）；vim 模式现在通过配置驱动。
- 使用 `Esc` 切换到 NORMAL 模式，`i/a/o` 切换到 INSERT 模式，`v` 切换到 VISUAL 模式，`V` 切换到 VISUAL-LINE 模式（v2.1.118+）

**导航键**：
- `h` / `l` - 左移/右移
- `j` / `k` - 下移/上移
- `w` / `b` / `e` - 按单词移动
- `0` / `$` - 移动到行首/行尾
- `gg` / `G` - 跳转到文本开头/结尾

**文本对象**：
- `iw` / `aw` - 内部/周围单词
- `i"` / `a"` - 内部/周围引号字符串
- `i(` / `a(` - 内部/周围括号

**可视模式（v2.1.118+）**：

| 键 | 模式 | 行为 |
|-----|------|----------|
| `v` | Visual | 字符级选择，带视觉反馈；使用移动键扩展 |
| `V` | Visual-line | 行级选择；始终选择整行 |
| `y` | Yank | 复制当前可视选区 |
| `d` / `x` | Delete | 删除当前可视选区 |
| `c` | Change | 删除选区并进入 INSERT 模式 |
| `Esc` | Exit | 返回 NORMAL 模式 |

可视选区在输入框中高亮显示，这样你可以在执行操作符之前确切地看到将被复制、删除或更改的内容。

### Bash 模式

使用 `!` 前缀直接执行 shell 命令：

```bash
! npm test
! git status
! cat src/index.js
```

使用此功能可以在不切换上下文的情况下快速执行命令。

---

## TUI 模式（全屏）

> **v2.1.110 新功能**

TUI（文本用户界面）模式以全屏方式渲染 Claude Code，无闪烁输出——非常适合 tmux 或 iTerm2 分屏等终端多路复用器。

### 启用 TUI 模式

使用 `/tui` 命令切换 TUI 模式或使用 `--tui` 标志启动：

```bash
/tui          # toggle from within a session
claude --tui  # start directly in TUI mode
```

### 配置

| 设置 | 描述 | 默认值 |
|---------|-------------|---------|
| `autoScrollEnabled` | 自动滚动到最新消息 | `true` |

通过 `/config` 或 `settings.json` 禁用自动滚动：

```json
{
  "autoScrollEnabled": false
}
```

### 聚焦视图

`/focus` 命令切换聚焦视图——一种无干扰的显示，只显示最相关的输出。`Ctrl+O` 现在仅在正常和详细转录之间切换（聚焦视图使用 `/focus`）。

---

## 语音输入

语音输入为 Claude Code 提供按住说话的语音输入功能，允许你用语音说出提示而不是打字。

### 激活语音输入

```
/voice
```

### 功能

| 功能 | 描述 |
|---------|-------------|
| **按住说话** | 按住键录音，释放后发送 |
| **20 种语言** | 语音转文字支持 20 种语言 |
| **自定义键绑定** | 通过 `/keybindings` 配置按住说话键 |
| **账户要求** | 需要 Claude.ai 账户进行 STT 处理 |

### 配置

在键绑定文件中自定义按住说话键绑定（`/keybindings`）。语音输入使用你的 Claude.ai 账户进行语音转文字处理。

---

## 频道

频道是研究预览版功能，通过 MCP 服务器将外部服务的事件推送到正在运行的 Claude Code 会话中。来源包括 Telegram、Discord、iMessage 和任意 Webhook，允许 Claude 在不进行轮询的情况下响应实时通知。

### 订阅频道

```bash
# Subscribe to channel plugins at startup
claude --channels discord,telegram

# Subscribe to multiple sources
claude --channels discord,telegram,imessage,webhooks
```

### 支持的集成

| 集成 | 描述 |
|-------------|-------------|
| **Discord** | 在会话中接收和回复 Discord 消息 |
| **Telegram** | 在会话中接收和回复 Telegram 消息 |
| **iMessage** | 在会话中接收 iMessage 通知 |
| **Webhooks** | 从任意 Webhook 来源接收事件 |

### 配置

在启动时使用 `--channels` 标志配置频道。对于企业部署，使用托管设置来控制允许的频道插件：

```json
{
  "allowedChannelPlugins": ["discord", "telegram"]
}
```

`allowedChannelPlugins` 托管设置控制组织中允许的频道插件。

### 工作原理

1. MCP 服务器充当连接外部服务的频道插件
2. 传入的消息和事件被推送到活跃的 Claude Code 会话中
3. Claude 可以在会话上下文中读取和回复消息
4. 频道插件必须通过 `allowedChannelPlugins` 托管设置批准
5. 无需轮询——事件实时推送

---

## Chrome 集成

Chrome 集成将 Claude Code 连接到你的 Chrome 或 Microsoft Edge 浏览器，用于实时网页自动化和调试。这是自 v2.0.73+ 起可用的测试版功能（Edge 支持在 v1.0.36+ 中添加）。

### 启用 Chrome 集成

**启动时**：

```bash
claude --chrome      # Enable Chrome connection
claude --no-chrome   # Disable Chrome connection
```

**在会话中**：

```
/chrome
```

选择"Enabled by default"以在所有未来会话中激活 Chrome 集成。Claude Code 共享你浏览器的登录状态，因此它可以与已认证的 Web 应用交互。

### 功能

| 功能 | 描述 |
|------------|-------------|
| **实时调试** | 读取控制台日志、检查 DOM 元素、实时调试 JavaScript |
| **设计验证** | 将渲染的页面与设计稿进行对比 |
| **表单验证** | 测试表单提交、输入验证和错误处理 |
| **Web 应用测试** | 与已认证的应用交互（Gmail、Google Docs、Notion 等） |
| **数据提取** | 从网页中抓取和处理内容 |
| **会话录制** | 将浏览器交互录制为 GIF 文件 |

### 站点级权限

Chrome 扩展管理每个站点的访问权限。可以随时通过扩展弹出窗口授予或撤销特定站点的访问权限。Claude Code 只与你明确允许的站点交互。

### 工作原理

Claude Code 在一个可见窗口中控制浏览器——你可以实时观看操作发生。当浏览器遇到登录页面或验证码时，Claude 会暂停并等待你手动处理后再继续。

### 已知限制

- **浏览器支持**：仅限 Chrome 和 Edge——不支持 Brave、Arc 和其他 Chromium 浏览器
- **WSL**：在 Windows Subsystem for Linux 中不可用
- **第三方提供商**：不支持 Bedrock、Vertex 或 Foundry API 提供商
- **Service Worker 空闲**：Chrome 扩展的 Service Worker 可能在长时间会话期间进入空闲状态

> **提示**：Chrome 集成是测试版功能。未来版本可能会扩展浏览器支持。

---

## 远程控制

远程控制让你从手机、平板电脑或任何浏览器继续本地运行的 Claude Code 会话。你的本地会话继续在你的机器上运行——不会移动到云端。适用于 Pro、Max、Team 和 Enterprise 计划（v2.1.51+）。

### 启动远程控制

**从 CLI**：

```bash
# Start with default session name
claude remote-control

# Start with a custom name
claude remote-control --name "Auth Refactor"
```

**在会话中**：

```
/remote-control
/remote-control "Auth Refactor"
```

**可用标志**：

| 标志 | 描述 |
|------|-------------|
| `--name "title"` | 自定义会话标题以便识别 |
| `--verbose` | 显示详细连接日志 |
| `--sandbox` | 启用文件系统和网络隔离 |
| `--no-sandbox` | 禁用沙箱（默认） |

### 连接到会话

从另一台设备连接的三种方式：

1. **会话 URL** — 会话启动时打印到终端；在任何浏览器中打开
2. **二维码** — 启动后按 `spacebar` 显示可扫描的二维码
3. **按名称查找** — 在 claude.ai/code 或 Claude 移动应用（iOS/Android）中浏览你的会话

### 安全性

- 你的机器上**不开放入站端口**
- **仅通过 TLS 进行出站 HTTPS** 连接
- **范围受限的凭据** — 多个短期、窄范围的令牌
- **会话隔离** — 每个远程会话都是独立的

### 远程控制 vs Claude Code on the web

| 方面 | 远程控制 | Claude Code on Web |
|--------|---------------|-------------------|
| **执行** | 在你的机器上运行 | 在 Anthropic 云上运行 |
| **本地工具** | 完全访问本地 MCP 服务器、文件和 CLI | 无本地依赖 |
| **使用场景** | 从另一台设备继续本地工作 | 从任何浏览器全新开始 |

### 限制

- 每个 Claude Code 实例一个远程会话
- 终端必须在主机上保持打开
- 如果网络不可达，会话在约 10 分钟后超时

### 使用场景

- 离开桌面时从移动设备或平板电脑控制 Claude Code
- 使用更丰富的 claude.ai UI 同时保持本地工具执行
- 使用完整的本地开发环境进行快速代码审查

### 推送通知 (v2.1.110)

当远程控制处于活动状态且在 `/config` 中启用了"Push when Claude decides"时，Claude 可以向你的手机发送移动推送通知——例如，当长时间任务完成或需要你的输入时。

启用方法：
1. 激活远程控制：`/remote-control` 或 `claude --rc`
2. 打开 `/config` 并启用 **Push when Claude decides**

推送通知需要 Claude 订阅和 Claude 移动应用。

---

## Web 会话

Web 会话允许你直接在浏览器中通过 claude.ai/code 运行 Claude Code，或从 CLI 创建 Web 会话。

### 创建 Web 会话

```bash
# Create a new web session from the CLI
claude --remote "implement the new API endpoints"
```

这会在 claude.ai 上启动一个 Claude Code 会话，你可以从任何浏览器访问。

### 在本地恢复 Web 会话

如果你在 Web 上启动了会话并想在本地继续：

```bash
# Resume a web session in the local terminal
claude --teleport
```

或在交互式 REPL 中：
```
/teleport
```

### 使用场景

- 在一台机器上开始工作，在另一台机器上继续
- 与团队成员分享会话 URL
- 使用 Web UI 进行可视化差异审查，然后切换到终端执行

---

## 桌面应用

Claude Code 桌面应用提供独立应用程序，具有可视化差异审查、并行会话和集成连接器功能。适用于 macOS 和 Windows（Pro、Max、Team 和 Enterprise 计划）。

### 安装

从 [claude.ai](https://claude.ai) 下载适合你平台的版本：
- **macOS**：通用构建（Apple Silicon 和 Intel）
- **Windows**：提供 x64 和 ARM64 安装程序

设置说明请参阅 [Desktop Quickstart](https://code.claude.com/docs/en/desktop-quickstart)。

### 从 CLI 移交

将当前 CLI 会话转移到桌面应用：

```
/desktop
```

### 核心功能

| 功能 | 描述 |
|---------|-------------|
| **差异视图** | 逐文件的可视化审查，支持内联评论；Claude 读取评论并修改 |
| **应用预览** | 自动启动开发服务器，带嵌入式浏览器进行实时验证 |
| **PR 监控** | GitHub CLI 集成，自动修复 CI 失败并在检查通过时自动合并 |
| **并行会话** | 侧边栏中的多个会话，自动 Git 工作树隔离 |
| **定时任务** | 在应用打开时运行的循环任务（每小时、每天、工作日、每周） |
| **富渲染** | 代码、Markdown 和图表渲染，支持语法高亮 |

### 应用预览配置

在 `.claude/launch.json` 中配置开发服务器行为：

```json
{
  "command": "npm run dev",
  "port": 3000,
  "readyPattern": "ready on",
  "persistCookies": true
}
```

### 连接器

连接外部服务以获取更丰富的上下文：

| 连接器 | 功能 |
|-----------|------------|
| **GitHub** | PR 监控、问题跟踪、代码审查 |
| **Slack** | 通知、频道上下文 |
| **Linear** | 问题跟踪、冲刺管理 |
| **Notion** | 文档、知识库访问 |
| **Asana** | 任务管理、项目跟踪 |
| **Calendar** | 日程感知、会议上下文 |

> **注意**：连接器不适用于远程（云）会话。

### 远程和 SSH 会话

- **远程会话**：在 Anthropic 云基础设施上运行；即使应用关闭也能继续。可从 claude.ai/code 或 Claude 移动应用访问
- **SSH 会话**：通过 SSH 连接到远程机器，完全访问远程文件系统和工具。Claude Code 必须安装在远程机器上

### 桌面应用中的权限模式

桌面应用支持与 CLI 相同的 4 种权限模式：

| 模式 | 行为 |
|------|----------|
| **Ask permissions**（默认） | 审查和批准每次编辑和命令 |
| **Auto accept edits** | 文件编辑自动批准；命令需要手动批准 |
| **Plan mode** | 在进行任何更改之前审查方法 |
| **Bypass permissions** | 自动执行（仅限沙箱，管理员控制） |

### 企业功能

- **管理控制台**：控制组织的 Code 标签页访问和权限设置
- **MDM 部署**：在 macOS 上通过 MDM 部署，在 Windows 上通过 MSIX 部署
- **SSO 集成**：要求组织成员使用单点登录
- **托管设置**：集中管理团队配置和模型可用性

---

## 任务列表

任务列表功能提供在上下文压缩（当对话历史被裁剪以适应上下文窗口时）后仍然持久保存的任务跟踪。

### 切换任务列表

在会话中按 `Ctrl+T` 切换任务列表视图的显示和隐藏。

### 持久任务

任务在上下文压缩后持久保存，确保在对话上下文被裁剪时不会丢失长时间运行的工作项。这对于复杂的多步骤实现特别有用。

### 命名任务目录

使用 `CLAUDE_CODE_TASK_LIST_ID` 环境变量创建跨会话共享的命名任务目录：

```bash
export CLAUDE_CODE_TASK_LIST_ID=my-project-sprint-3
```

这允许多个会话共享同一个任务列表，对于团队工作流或多会话项目非常有用。

---

## 提示建议

提示建议根据你的 git 历史和当前对话上下文显示灰色的示例命令。

### 工作原理

- 建议以灰色文本显示在输入提示下方
- 按 `Tab` 接受建议
- 按 `Enter` 接受并立即提交
- 建议是上下文感知的，从 git 历史和对话状态中提取

### 禁用提示建议

```bash
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
```

---

## Git 工作树

Git 工作树允许你在隔离的工作树中启动 Claude Code，支持在不同分支上并行工作，无需暂存或切换。

### 在工作树中启动

```bash
# Start Claude Code in an isolated worktree
claude --worktree
# or
claude -w
```

### 工作树位置

工作树创建在：
```
<repo>/.claude/worktrees/<name>
```

### 单仓库的稀疏检出

使用 `worktree.sparsePaths` 设置在单仓库中执行稀疏检出，减少磁盘使用和克隆时间：

```json
{
  "worktree": {
    "sparsePaths": ["packages/my-package", "shared/"]
  }
}
```

### 工作树工具和钩子

| 项目 | 描述 |
|------|-------------|
| `ExitWorktree` | 退出并清理当前工作树的工具 |
| `WorktreeCreate` | 工作树创建时触发的钩子事件 |
| `WorktreeRemove` | 工作树删除时触发的钩子事件 |

### 自动清理

如果工作树中没有进行任何更改，它会在会话结束时自动清理。

### 使用场景

- 在保持主分支不受影响的情况下在功能分支上工作
- 在不影响工作目录的情况下隔离运行测试
- 在可丢弃的环境中尝试实验性更改
- 在单仓库中稀疏检出特定包以加快启动速度

---

## 沙箱

沙箱为 Claude Code 执行的 Bash 命令提供操作系统级别的文件系统和网络隔离。这是权限规则的补充，提供额外的安全层。

### 启用沙箱

**斜杠命令**：
```
/sandbox
```

**CLI 标志**：
```bash
claude --sandbox       # Enable sandboxing
claude --no-sandbox    # Disable sandboxing
```

### 配置设置

| 设置 | 描述 |
|---------|-------------|
| `sandbox.enabled` | 启用或禁用沙箱 |
| `sandbox.failIfUnavailable` | 如果无法激活沙箱则失败 |
| `sandbox.filesystem.allowWrite` | 允许写入访问的路径 |
| `sandbox.filesystem.allowRead` | 允许读取访问的路径 |
| `sandbox.filesystem.denyRead` | 拒绝读取访问的路径 |
| `sandbox.network.allowedDomains` | Bash 启动的进程允许访问的域名（支持 `*.` 通配符） |
| `sandbox.network.deniedDomains` | 即使 `allowedDomains` 通配符允许也要阻止的域名（v2.1.113+） |
| `sandbox.enableWeakerNetworkIsolation` | 在 macOS 上启用较弱的网络隔离 |

`deniedDomains` 覆盖宽泛通配符的示例（v2.1.113+）：

```json
{
  "sandbox": {
    "network": {
      "allowedDomains": ["*.example.com"],
      "deniedDomains": ["evil.example.com"]
    }
  }
}
```

通配符允许 `example.com` 上的所有流量通过，但 `deniedDomains` 仍然阻止特定命名的主机。

### 配置示例

```json
{
  "sandbox": {
    "enabled": true,
    "failIfUnavailable": true,
    "filesystem": {
      "allowWrite": ["/Users/me/project"],
      "allowRead": ["/Users/me/project", "/usr/local/lib"],
      "denyRead": ["/Users/me/.ssh", "/Users/me/.aws"]
    },
    "enableWeakerNetworkIsolation": true
  }
}
```

### 工作原理

- Bash 命令在受限文件系统访问的沙箱环境中运行
- 网络访问可以被隔离以防止意外的外部连接
- 与权限规则协同工作，提供纵深防御
- 在 macOS 上，使用 `sandbox.enableWeakerNetworkIsolation` 进行网络限制（macOS 上不支持完全网络隔离）

### 使用场景

- 安全运行不受信任或生成的代码
- 防止意外修改项目外的文件
- 在自动化任务期间限制网络访问

---

## 托管设置（企业版）

托管设置使企业管理员能够使用平台原生管理工具在整个组织中部署 Claude Code 配置。

### 部署方法

| 平台 | 方法 | 起始版本 |
|----------|--------|-------|
| macOS | 托管 plist 文件（MDM） | v2.1.51+ |
| Windows | Windows Registry | v2.1.51+ |
| 跨平台 | 托管配置文件 | v2.1.51+ |
| 跨平台 | 托管插件目录（`managed-settings.d/` 目录） | v2.1.83+ |

### 托管插件目录

自 v2.1.83 起，管理员可以将多个托管设置文件部署到 `managed-settings.d/` 目录中。文件按字母顺序合并，允许跨团队的模块化配置：

```
~/.claude/managed-settings.d/
  00-org-defaults.json
  10-team-policies.json
  20-project-overrides.json
```

### 可用的托管设置

| 设置 | 描述 |
|---------|-------------|
| `disableBypassPermissionsMode` | 阻止用户启用绕过权限模式 |
| `availableModels` | 限制用户可以选择的模型 |
| `allowedChannelPlugins` | 控制允许的频道插件 |
| `autoMode.environment` | 为自动模式配置受信任的基础设施 |
| `wslInheritsWindowsSettings` | 仅限 Windows/WSL（v2.1.118+）：当设为 `true` 时，在 WSL 中运行的 Claude Code 继承 Windows 主机的托管设置，使通过 Registry/MDM 部署的企业策略在 Windows 和 WSL shell 中统一应用 |
| 自定义策略 | 组织特定的权限和工具策略 |

### 示例：macOS Plist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>disableBypassPermissionsMode</key>
  <true/>
  <key>availableModels</key>
  <array>
    <string>claude-sonnet-4-6</string>
    <string>claude-haiku-4-5</string>
  </array>
</dict>
</plist>
```

---

## 配置和设置

### 配置文件位置

1. **全局配置**：`~/.claude/config.json`
2. **项目配置**：`./.claude/config.json`
3. **用户配置**：`~/.config/claude-code/settings.json`

### 完整配置示例

**核心高级功能配置：**

```json
{
  "permissions": {
    "mode": "default"
  },
  "hooks": {
    "PreToolUse:Edit": "eslint --fix ${file_path}",
    "PostToolUse:Write": "~/.claude/hooks/security-scan.sh"
  },
  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"]
      }
    }
  }
}
```

**扩展配置示例：**

```json
{
  "permissions": {
    "mode": "default",
    "allowedTools": ["Bash(git log:*)", "Read"],
    "disallowedTools": ["Bash(rm -rf:*)"]
  },

  "hooks": {
    "PreToolUse": [{ "matcher": "Edit", "hooks": ["eslint --fix ${file_path}"] }],
    "PostToolUse": [{ "matcher": "Write", "hooks": ["~/.claude/hooks/security-scan.sh"] }],
    "Stop": [{ "hooks": ["~/.claude/hooks/notify.sh"] }]
  },

  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_TOKEN": "${GITHUB_TOKEN}"
        }
      }
    }
  }
}
```

### 环境变量

使用环境变量覆盖配置：

```bash
# Model selection
export ANTHROPIC_MODEL=claude-opus-4-7
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-7
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-6
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-haiku-4-5

# API configuration
export ANTHROPIC_API_KEY=sk-ant-...

# Thinking configuration
export MAX_THINKING_TOKENS=16000
export CLAUDE_CODE_EFFORT_LEVEL=xhigh   # low, medium, high, xhigh (Opus 4.7 only, default), or max (supported on Opus 4.7, Opus 4.6, Sonnet 4.6)

# Feature toggles
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=true
export CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true
export CLAUDE_CODE_DISABLE_CRON=1
export CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS=true
export CLAUDE_CODE_DISABLE_TERMINAL_TITLE=true
export CLAUDE_CODE_DISABLE_1M_CONTEXT=true
export CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK=true
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
export CLAUDE_CODE_ENABLE_TASKS=true
export CLAUDE_CODE_SIMPLE=true              # Set by --bare flag

# MCP configuration
export MAX_MCP_OUTPUT_TOKENS=50000
export ENABLE_TOOL_SEARCH=true

# Prompt caching
export ENABLE_PROMPT_CACHING_1H=1      # Use 1-hour prompt cache TTL (default is 5 min)

# Task management
export CLAUDE_CODE_TASK_LIST_ID=my-project-tasks

# Agent teams (experimental)
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1

# Subagent and plugin configuration
export CLAUDE_CODE_SUBAGENT_MODEL=sonnet
export CLAUDE_CODE_PLUGIN_SEED_DIR=./my-plugins
export CLAUDE_CODE_NEW_INIT=1

# Subprocess and streaming
export CLAUDE_CODE_SUBPROCESS_ENV_SCRUB="SECRET_KEY,DB_PASSWORD"
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=80
export CLAUDE_STREAM_IDLE_TIMEOUT_MS=30000
export ANTHROPIC_CUSTOM_MODEL_OPTION=my-custom-model
export SLASH_COMMAND_TOOL_CHAR_BUDGET=50000
```

> **v2.1.108**：`ENABLE_PROMPT_CACHING_1H=1` — 使用 1 小时的提示缓存 TTL 而不是默认的 5 分钟 TTL。减少长时间稳定会话中的缓存未命中。

### 配置管理命令

```
User: /config
[Opens interactive configuration menu]
```

`/config` 命令提供交互式菜单来切换设置，例如：
- 扩展思考开/关
- 详细输出
- 权限模式
- 模型选择

### 每项目配置

在项目中创建 `.claude/config.json`：

```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Bash", "hooks": ["npm test && npm run lint"] }]
  },
  "permissions": {
    "mode": "default"
  },
  "mcp": {
    "servers": {
      "project-db": {
        "command": "mcp-postgres",
        "env": {
          "DATABASE_URL": "${PROJECT_DB_URL}"
        }
      }
    }
  }
}
```

---

## 智能体团队

智能体团队是一个实验性功能，允许多个 Claude Code 实例协作完成任务。默认情况下禁用。

### 启用智能体团队

通过环境变量或设置启用：

```bash
# Environment variable
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

或添加到你的设置 JSON 中：

```json
{
  "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
}
```

### 智能体团队的工作原理

- **团队负责人**协调整体任务并将子任务委派给队友
- **队友**独立工作，每个人都有自己的上下文窗口
- **共享任务列表**使团队成员之间能够自协调
- 使用子智能体定义（`.claude/agents/` 或 `--agents` 标志）来定义队友角色和专业化

### 显示模式

智能体团队支持两种显示模式，通过 `--teammate-mode` 标志配置：

| 模式 | 描述 |
|------|-------------|
| `in-process`（默认） | 队友在同一终端进程内运行 |
| `tmux` | 每个队友获得专用的分屏窗格（需要 tmux 或 iTerm2） |
| `auto` | 自动选择最佳显示模式 |

```bash
# Use tmux split panes for teammate display
claude --teammate-mode tmux

# Explicitly use in-process mode
claude --teammate-mode in-process
```

### 使用场景

- 不同队友处理不同模块的大型重构任务
- 并行代码审查和实现
- 跨代码库的协调多文件更改

> **注意**：智能体团队是实验性功能，可能在未来版本中发生变化。完整参考请参阅 [code.claude.com/docs/en/agent-teams](https://code.claude.com/docs/en/agent-teams)。

---

## 最佳实践

### 规划模式
- ✅ 用于复杂的多步骤任务
- ✅ 在批准前审查计划
- ✅ 需要时修改计划
- ❌ 不要用于简单任务

### 扩展思考
- ✅ 用于架构决策
- ✅ 用于复杂问题解决
- ✅ 审查思考过程
- ❌ 不要用于简单查询

### 后台任务
- ✅ 用于长时间运行的操作
- ✅ 监控任务进度
- ✅ 优雅处理任务失败
- ❌ 不要启动太多并发任务

### 权限
- ✅ 使用 `plan` 进行代码审查（只读）
- ✅ 使用 `default` 进行交互式开发
- ✅ 使用 `acceptEdits` 进行自动化工作流
- ✅ 使用 `auto` 进行有安全防护的自主工作
- ❌ 除非绝对必要，不要使用 `bypassPermissions`

### 会话
- ✅ 为不同任务使用单独的会话
- ✅ 保存重要的会话状态
- ✅ 清理旧会话
- ❌ 不要在一个会话中混合不相关的工作

---

## 其他资源

有关 Claude Code 和相关功能的更多信息：

- [Official Interactive Mode Documentation](https://code.claude.com/docs/en/interactive-mode)
- [Official Headless Mode Documentation](https://code.claude.com/docs/en/headless)
- [CLI Reference](https://code.claude.com/docs/en/cli-reference)
- [检查点指南](../08-checkpoints/) - 会话管理和回退
- [斜杠命令](../01-slash-commands/) - 命令参考
- [记忆指南](../02-memory/) - 持久上下文
- [技能指南](../03-skills/) - 自主能力
- [子智能体指南](../04-subagents/) - 委派任务执行
- [MCP 指南](../05-mcp/) - 外部数据访问
- [钩子指南](../06-hooks/) - 事件驱动自动化
- [插件指南](../07-plugins/) - 捆绑扩展
- [Official Scheduled Tasks Documentation](https://code.claude.com/docs/en/scheduled-tasks)
- [Official Chrome Integration Documentation](https://code.claude.com/docs/en/chrome)
- [Official Remote Control Documentation](https://code.claude.com/docs/en/remote-control)
- [Official Keybindings Documentation](https://code.claude.com/docs/en/keybindings)
- [Official Desktop App Documentation](https://code.claude.com/docs/en/desktop)
- [Official Agent Teams Documentation](https://code.claude.com/docs/en/agent-teams)

---

**Last Updated**: April 24, 2026
**Claude Code Version**: 2.1.119
**Sources**:
- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/interactive-mode
- https://code.claude.com/docs/en/settings
- https://www.anthropic.com/news/claude-opus-4-7
- https://github.com/anthropics/claude-code/releases/tag/v2.1.117
- https://github.com/anthropics/claude-code/releases/tag/v2.1.118
**Compatible Models**: Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5
