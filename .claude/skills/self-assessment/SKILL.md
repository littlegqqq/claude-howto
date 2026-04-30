---
name: self-assessment
version: 2.3.0
description: 全面的 Claude Code 自我评估和学习路径顾问。运行覆盖 10 个功能领域的多类别测验，生成详细的技能档案（包含每主题得分），识别具体缺口，并生成带有优先排序下一步的个性化学习路径。当被要求"评估我的水平"、"参加测验"、"确定我的水平"、"我该从哪开始"、"我接下来该学什么"、"检查我的技能"、"技能检查"或"提升水平"时使用。
---

# 自我评估与学习路径顾问

全面的交互式评估，评测 Claude Code 在 10 个功能领域的熟练程度，识别具体技能缺口，并生成个性化学习路径助你提升。

## 使用说明

### 第 1 步：欢迎 & 选择评估模式

向用户呈现评估深度选择：

使用 AskUserQuestion 提供以下选项：
- **快速评估** — "8 道题，约 2 分钟。确定你的整体水平（初级/中级/高级）并给出学习路径。"
- **深度评估** — "5 个类别的详细问题，约 5 分钟。给出每主题技能得分，识别具体缺口，并构建优先学习路径。"

如果用户选择**快速评估**，转到第 2A 步。
如果用户选择**深度评估**，转到第 2B 步。

---

### 第 2A 步：快速评估

呈现两道多选题（AskUserQuestion 每题最多支持 4 个选项）：

**问题 1**（标题："Basics"）：
"第 1/2 部分：你已经具备以下哪些 Claude Code 技能？"
选项：
1. "启动 Claude Code 并对话" — 我能运行 `claude` 并与之交互
2. "创建/编辑了 CLAUDE.md" — 我已设置项目或用户记忆
3. "使用过 3 个以上 slash commands" — 例如 /help、/compact、/model、/clear
4. "创建了自定义命令/skill" — 编写过 SKILL.md 或自定义命令文件

**问题 2**（标题："Advanced"）：
"第 2/2 部分：你具备以下哪些高级技能？"
选项：
1. "配置过 MCP 服务器" — 例如 GitHub、数据库或其他外部数据源
2. "设置过 hooks" — 在 ~/.claude/settings.json 中配置了 hooks
3. "创建/使用过子代理" — 使用 .claude/agents/ 进行任务委派
4. "使用过打印模式（claude -p）" — 使用 `claude -p` 进行非交互式或 CI/CD 使用

**评分：**
- 总计 0-2 项 = 第 1 级：初级
- 总计 3-5 项 = 第 2 级：中级
- 总计 6-8 项 = 第 3 级：高级

带着等级结果转到第 3 步，列出未勾选的具体项目作为缺口。

---

### 第 2B 步：深度评估

分 5 轮呈现问题，每轮一次 AskUserQuestion 调用。每轮涵盖 2 个相关功能领域。所有轮次使用多选。

**重要**：AskUserQuestion 每题最多支持 4 个选项。每轮恰好有 1 道题包含 4 个选项，覆盖 2 个主题（每主题 2 个选项）。

---

**第 1 轮 — Slash Commands & Memory**（标题："Commands"）

"你做过以下哪些？选择所有适用项。"
选项：
1. "创建了自定义 slash command 或 skill" — 编写了带有 frontmatter 的 SKILL.md 文件，或创建了 .claude/commands/ 文件
2. "在命令中使用了动态上下文" — 使用了 `$ARGUMENTS`、`$0`/`$1`、反引号 `!command` 语法或 skill/命令文件中的 `@file` 引用
3. "设置了项目 + 个人记忆" — 创建了项目 CLAUDE.md 和个人 ~/.claude/CLAUDE.md（或 CLAUDE.local.md）
4. "使用了记忆层级功能" — 理解 7 级优先级顺序，使用了 .claude/rules/ 目录、路径特定规则或 @import 语法

**第 1 轮评分：**
- 选项 1-2 映射到 **Slash Commands**（0-2 分）
- 选项 3-4 映射到 **Memory**（0-2 分）

---

**第 2 轮 — Skills & Hooks**（标题："Automation"）

"你做过以下哪些？选择所有适用项。"
选项：
1. "安装并使用了自动调用的 skill" — 一个根据描述自动触发的 skill，无需手动 /command 调用
2. "控制了 skill 调用行为" — 在 SKILL.md frontmatter 中使用了 `disable-model-invocation`、`user-invocable` 或 `context: fork` 配合 agent 字段
3. "设置了 PreToolUse 或 PostToolUse hook" — 配置了在工具执行前/后运行的 hook（例如命令验证器、自动格式化器）
4. "使用了高级 hook 功能" — 配置了 prompt 类型 hooks、SKILL.md 中的组件范围 hooks、HTTP hooks 或带有自定义 JSON 输出（updatedInput、systemMessage）的 hooks

**第 2 轮评分：**
- 选项 1-2 映射到 **Skills**（0-2 分）
- 选项 3-4 映射到 **Hooks**（0-2 分）

---

**第 3 轮 — MCP & Subagents**（标题："Integration"）

"你做过以下哪些？选择所有适用项。"
选项：
1. "连接了 MCP 服务器并使用了其工具" — 例如 GitHub MCP 用于 PR/issues、数据库 MCP 用于查询或任何外部数据源
2. "使用了高级 MCP 功能" — 项目范围 .mcp.json、OAuth 认证、使用 @mentions 的 MCP 资源、Tool Search 或 `claude mcp serve`
3. "创建或配置了自定义子代理" — 在 .claude/agents/ 中定义了带有自定义工具、模型或权限的代理
4. "使用了高级子代理功能" — Worktree 隔离、持久化代理记忆、使用 Ctrl+B 的后台任务、使用 `Task(agent_name)` 的代理允许列表或代理团队

**第 3 轮评分：**
- 选项 1-2 映射到 **MCP**（0-2 分）
- 选项 3-4 映射到 **Subagents**（0-2 分）

---

**第 4 轮 — Checkpoints & Advanced Features**（标题："Power User"）

"你做过以下哪些？选择所有适用项。"
选项：
1. "使用检查点进行安全实验" — 创建了检查点，使用了 Esc+Esc 或 /rewind，恢复了代码和/或对话，或使用了摘要选项
2. "使用了规划模式或扩展思考" — 通过 /plan、Shift+Tab 或 --permission-mode plan 激活了规划；使用 Alt+T/Option+T 切换了扩展思考
3. "配置了权限模式" — 通过 CLI 标志、键盘快捷键或设置使用了 acceptEdits、plan、dontAsk 或 bypassPermissions 模式
4. "使用了远程/桌面/Web 功能" — 使用了 `claude remote-control`、`claude --remote`、`/teleport`、`/desktop` 或使用 `claude -w` 的 worktrees

**第 4 轮评分：**
- 选项 1 映射到 **Checkpoints**（0-1 分）
- 选项 2-4 映射到 **Advanced Features**（0-3 分，上限 2 分）

---

**第 5 轮 — Plugins & CLI**（标题："Mastery"）

"你做过以下哪些？选择所有适用项。"
选项：
1. "安装或创建了插件" — 使用了市场上的捆绑插件，或创建了带有 plugin.json 清单的 .claude-plugin/ 目录
2. "使用了插件高级功能" — 插件 hooks、插件 MCP 服务器、LSP 配置、插件命名空间命令或 --plugin-dir 标志用于测试
3. "在脚本或 CI/CD 中使用了打印模式" — 使用了 `claude -p` 配合 --output-format json、--max-turns、管道输入或集成到 GitHub Actions/CI 流水线
4. "使用了高级 CLI 功能" — 会话恢复（-c/-r）、--agents 标志、--json-schema 用于结构化输出、--fallback-model、--from-pr 或批量处理循环

**第 5 轮评分：**
- 选项 1-2 映射到 **Plugins**（0-2 分）
- 选项 3-4 映射到 **CLI**（0-2 分）

---

### 第 3 步：计算并呈现结果

#### 3A：快速评估

统计总选择数并确定等级。然后呈现：

```markdown
## Claude Code 技能评估结果

### 你的等级：[第 1 级：初级 / 第 2 级：中级 / 第 3 级：高级]

你勾选了 **N/8** 项。

[基于等级的一句话激励摘要]

### 你的技能档案

| 领域 | 状态 |
|------|------|
| 基础 CLI 和对话 | [已掌握/缺口] |
| CLAUDE.md 和 Memory | [已掌握/缺口] |
| Slash Commands（内置） | [已掌握/缺口] |
| 自定义命令和 Skills | [已掌握/缺口] |
| MCP 服务器 | [已掌握/缺口] |
| Hooks | [已掌握/缺口] |
| Subagents | [已掌握/缺口] |
| 打印模式和 CI/CD | [已掌握/缺口] |

### 识别的缺口

[对于每个未勾选的项目，提供一行描述需要学习什么和教程链接]

### 你的个性化学习路径

[输出对应等级的学习路径 — 见第 4 步]
```

#### 3B：深度评估

从 5 轮中计算每主题得分。每个主题获得 0-2 分。然后呈现：

```markdown
## Claude Code 技能评估结果

### 整体等级：[第 1 级 / 第 2 级 / 第 3 级]

**总分：N/20 分**

[一句话激励摘要]

### 你的技能档案

| 功能领域 | 得分 | 掌握程度 | 状态 |
|---------|------|---------|------|
| Slash Commands | N/2 | [无/基础/熟练] | [学习/复习/已掌握] |
| Memory | N/2 | [无/基础/熟练] | [学习/复习/已掌握] |
| Skills | N/2 | [无/基础/熟练] | [学习/复习/已掌握] |
| Hooks | N/2 | [无/基础/熟练] | [学习/复习/已掌握] |
| MCP | N/2 | [无/基础/熟练] | [学习/复习/已掌握] |
| Subagents | N/2 | [无/基础/熟练] | [学习/复习/已掌握] |
| Checkpoints | N/1 | [无/熟练] | [学习/已掌握] |
| Advanced Features | N/2 | [无/基础/熟练] | [学习/复习/已掌握] |
| Plugins | N/2 | [无/基础/熟练] | [学习/复习/已掌握] |
| CLI | N/2 | [无/基础/熟练] | [学习/复习/已掌握] |

**掌握程度说明：** 0 = 无，1 = 基础，2 = 熟练

### 优势领域
[列出得分 2/2 的主题 — 这些已掌握]

### 优先缺口（下一步学习）
[列出得分 0 的主题 — 这些需要优先关注，按依赖顺序排列]

### 复习领域
[列出得分 1/2 的主题 — 基础已知但高级功能尚未使用]

### 你的个性化学习路径

[输出针对缺口的学习路径 — 见第 4 步]
```

**深度评估的整体等级计算：**
- 总分 0-6 分 = 第 1 级：初级
- 总分 7-13 分 = 第 2 级：中级
- 总分 14-20 分 = 第 3 级：高级

---

### 第 4 步：生成个性化学习路径

根据评估结果，生成针对用户缺口的特定学习路径。不要只重复通用的等级路径 — 要根据实际情况调整。

#### 路径生成规则

1. **跳过已掌握的主题**：如果某主题得分 2/2，不要将其包含在路径中。
2. **按依赖顺序排列优先级**：Slash Commands 在 Skills 之前，Memory 在 Subagents 之前，等等。依赖顺序为：
   - Slash Commands（无依赖）-> Skills（依赖 Slash Commands）
   - Memory（无依赖）-> Subagents（依赖 Memory）
   - CLI 基础（无依赖）-> CLI 精通（依赖所有）
   - Checkpoints（无依赖）
   - Hooks（依赖 Slash Commands）
   - MCP（无依赖）-> Plugins（依赖 MCP、Skills、Hooks）
   - Advanced Features（依赖所有前述内容）
3. **对于得分 1/2 的主题**：建议"深入学习" — 链接到他们缺失的具体高级章节。
4. **估计时间**：仅累计他们需要学习/复习的主题。
5. **分阶段组织**：将剩余主题组织为每阶段 2-3 个主题的逻辑分组。

#### 路径输出格式

```markdown
### 你的个性化学习路径

**预计时间**：约 N 小时（已根据你当前技能调整）

#### 第 1 阶段：[阶段名称]（约 N 小时）
[仅当他们在这些领域有缺口时]

**[主题名称]** — [从零开始学习 / 深入学习高级功能]
- 教程：[教程目录链接]
- 重点关注：[他们需要的具体章节/概念]
- 关键练习：[一个具体的练习]
- 完成标志：[具体的成功标准]

**[主题名称]** — …

---

#### 第 2 阶段：[阶段名称]（约 N 小时）
…

---

### 推荐实践项目

基于你的缺口，尝试以下实际练习来巩固学习：

1. **[项目名称]**：[结合 2-3 个缺口主题的一句话描述]
2. **[项目名称]**：[一句话描述]
3. **[项目名称]**：[一句话描述]
```

#### 主题特定建议

当某主题为缺口时，使用以下具体建议：

**Slash Commands（得分 0）**：
- 教程：[01-slash-commands/](../../../01-slash-commands/)
- 重点关注：内置命令参考、创建你的第一个 SKILL.md、`$ARGUMENTS` 语法
- 关键练习：创建一个 `/optimize` 命令并测试它
- 完成标志：你能创建带有参数和动态上下文的自定义 skill

**Slash Commands（得分 1 — 复习）**：
- 重点关注：使用 `!`backtick`` 语法的动态上下文、`@file` 引用、`disable-model-invocation` 与 `user-invocable` 的调用控制
- 完成标志：你能创建注入实时命令输出并控制自身调用行为的 skill

**Memory（得分 0）**：
- 教程：[02-memory/](../../../02-memory/)
- 重点关注：创建 CLAUDE.md、`/init` 和 `/memory` 命令、`#` 前缀快速更新
- 关键练习：创建一个包含你编码标准的项目 CLAUDE.md
- 完成标志：Claude 能在会话间记住你的偏好

**Memory（得分 1 — 复习）**：
- 重点关注：7 级层级和优先级顺序、带有路径特定规则的 .claude/rules/ 目录、`@import` 语法（最大深度 5）、Auto Memory MEMORY.md（200 行限制）
- 完成标志：你为不同目录建立了模块化规则并理解完整层级

**Skills（得分 0）**：
- 教程：[03-skills/](../../../03-skills/)
- 重点关注：SKILL.md 格式、通过 description 字段实现自动调用、渐进式披露（3 个加载级别）
- 关键练习：安装 code-review skill 并验证它自动触发
- 完成标志：skill 能根据对话上下文自动激活

**Skills（得分 1 — 复习）**：
- 重点关注：使用 `agent` 字段的 `context: fork` 实现子代理执行、`disable-model-invocation` 与 `user-invocable`、2% 上下文预算、捆绑资源（scripts/、references/、assets/）
- 完成标志：你能创建在具有 fork 上下文的子代理中运行的 skill

**Hooks（得分 0）**：
- 教程：[06-hooks/](../../../06-hooks/)
- 重点关注：配置结构（matcher + hooks 数组）、PreToolUse/PostToolUse 事件、退出码（0=成功，2=阻塞）、JSON 输入/输出格式
- 关键练习：创建一个验证 Bash 命令的 PreToolUse hook
- 完成标志：hook 能在执行前阻止危险命令

**Hooks（得分 1 — 复习）**：
- 重点关注：全部 25 个 hook 事件（包括 PostToolUseFailure、StopFailure、TaskCreated、CwdChanged、FileChanged、PostCompact、Elicitation、ElicitationResult）、4 种 hook 类型（command、http、prompt、agent）、SKILL.md frontmatter 中的组件范围 hooks、带有 allowedEnvVars 的 HTTP hooks、用于 SessionStart/CwdChanged/FileChanged 的 `CLAUDE_ENV_FILE`
- 完成标志：你能创建基于 prompt 的 Stop hook 和 skill 中的组件范围 hook

**MCP（得分 0）**：
- 教程：[05-mcp/](../../../05-mcp/)
- 重点关注：`claude mcp add` 命令、传输类型（推荐 HTTP）、GitHub MCP 设置、环境变量展开
- 关键练习：添加 GitHub MCP 服务器并查询 PR
- 完成标志：你能通过 MCP 从外部服务查询实时数据

**MCP（得分 1 — 复习）**：
- 重点关注：项目范围 .mcp.json（需要团队审批）、OAuth 2.0 认证、使用 `@server:resource` 提及的 MCP 资源、Tool Search（ENABLE_TOOL_SEARCH）、`claude mcp serve`、输出限制（10k/25k/50k）
- 完成标志：你有项目 .mcp.json 并理解 Tool Search 自动模式

**Subagents（得分 0）**：
- 教程：[04-subagents/](../../../04-subagents/)
- 重点关注：代理文件格式（.claude/agents/*.md）、内置代理（general-purpose、Plan、Explore）、tools/model/permissionMode 配置
- 关键练习：创建一个 code-reviewer 子代理并测试委派
- 完成标志：Claude 将代码审查委派给你的自定义代理

**Subagents（得分 1 — 复习）**：
- 重点关注：Worktree 隔离（`isolation: worktree`）、持久化代理记忆（带有范围的 `memory` 字段）、后台代理（Ctrl+B/Ctrl+F）、使用 `Task(agent_name)` 的代理允许列表、代理团队（`--teammate-mode`）
- 完成标志：你有一个在 worktree 隔离中运行的带有持久化记忆的子代理

**Checkpoints（得分 0）**：
- 教程：[08-checkpoints/](../../../08-checkpoints/)
- 重点关注：Esc+Esc 和 /rewind 访问、5 个回退选项（恢复代码+对话、恢复对话、恢复代码、摘要、取消）、限制（bash 文件系统操作不被追踪）
- 关键练习：进行实验性更改，然后回退恢复
- 完成标志：你能自信地进行实验，因为知道可以回退

**Advanced Features（得分 0）**：
- 教程：[09-advanced-features/](../../../09-advanced-features/)
- 重点关注：规划模式（/plan 或 Shift+Tab）、权限模式（5 种类型）、扩展思考（Alt+T 切换）
- 关键练习：使用规划模式设计一个功能，然后实现它
- 完成标志：你能流畅地在规划和实现模式之间切换

**Advanced Features（得分 1 — 复习）**：
- 重点关注：远程控制（`claude remote-control`）、Web 会话（`claude --remote`）、桌面交接（`/desktop`）、worktrees（`claude -w`）、任务列表（Ctrl+T）、企业托管设置
- 完成标志：你能在 CLI、Web 和桌面之间交接会话

**Plugins（得分 0）**：
- 教程：[07-plugins/](../../../07-plugins/)
- 重点关注：插件结构（.claude-plugin/plugin.json）、插件捆绑内容（命令、代理、MCP、hooks、设置）、从市场安装
- 关键练习：安装一个插件并探索其组件
- 完成标志：你理解何时使用插件与独立组件

**Plugins（得分 1 — 复习）**：
- 重点关注：创建 plugin.json 清单、插件 hooks（hooks/hooks.json）、LSP 配置（.lsp.json）、`${CLAUDE_PLUGIN_ROOT}` 变量、用于测试的 --plugin-dir、市场发布
- 完成标志：你能为团队创建和测试插件

**CLI（得分 0）**：
- 教程：[10-cli/](../../../10-cli/)
- 重点关注：交互式与打印模式、`claude -p` 配合管道、`--output-format json`、会话管理（-c/-r）
- 关键练习：将文件通过管道传入 `claude -p` 并获取 JSON 输出
- 完成标志：你能在脚本中非交互式地使用 Claude

**CLI（得分 1 — 复习）**：
- 重点关注：带有 JSON 配置的 --agents 标志、用于结构化输出的 --json-schema、--fallback-model、--from-pr、--strict-mcp-config、使用 for 循环的批量处理、`claude mcp serve`
- 完成标志：你有一个使用 Claude 配合结构化 JSON 输出的 CI/CD 脚本

---

### 第 5 步：提供后续操作

呈现结果后，询问用户接下来想做什么：

使用 AskUserQuestion 提供以下选项：
- **开始学习** — "帮我立即开始学习路径中的第一个主题"
- **深入了解缺口** — "详细解释我的一个缺口领域，让我在这里学习"
- **实践项目** — "设置一个覆盖我缺口领域的实践项目"
- **重新评估** — "我想重新参加测验（可能选另一种模式）"

如果选择**开始学习**：读取第一个缺口教程的 README.md 并引导用户完成第一个练习。
如果选择**深入了解缺口**：询问哪个缺口主题，然后读取相关教程 README.md 并配合示例解释关键概念。
如果选择**实践项目**：设计一个结合用户 2-3 个缺口主题的小项目，包含具体步骤。
如果选择**重新评估**：返回第 1 步。

## 错误处理

### 用户在某轮未选择任何项目
将该轮主题视为 0 分。继续下一轮。

### 用户在所有轮次都未选择任何项目
分配第 1 级：初级。鼓励从头开始。输出完整的第 1 级路径。

### 用户想重新测评
从第 1 步重新开始，进行全新评估。

### 用户不同意其等级
承认他们的偏好。询问他们认为自己属于哪个等级。呈现所选等级的路径，并对可能遗漏的主题进行先决条件检查。

### 用户询问特定主题
如果用户在评估期间说了类似"告诉我关于 hooks"或"我想学习 MCP"的话，记下它。呈现结果后，无论得分如何都在学习路径中突出该主题。

## 验证

### 触发测试用例

**应当触发：**
- "assess my level"
- "take the quiz"
- "find my level"
- "where should I start"
- "what level am I"
- "learning path quiz"
- "self-assessment"
- "what should I learn next"
- "check my skills"
- "skill check"
- "level up"
- "how good am I at Claude Code"
- "evaluate my Claude Code knowledge"

**不应触发：**
- "review my code"
- "create a skill"
- "help me with MCP"
- "explain slash commands"
- "what is a checkpoint"
