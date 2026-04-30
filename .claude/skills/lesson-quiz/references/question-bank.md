# 课程测验 — 题库

每课 10 道题。每道题包含：类别、题目文本、选项（3-4 个）、正确答案、解释和复习章节。

---

## 第 01 课：Slash Commands

### Q1
- **类别**：概念
- **题目**：Claude Code 中有哪四种类型的 slash commands？
- **选项**：A) 内置命令、skills、插件命令、MCP 提示 | B) 内置命令、自定义命令、hook 命令、API 提示 | C) 系统命令、用户命令、插件命令、终端命令 | D) 核心命令、扩展命令、宏命令、脚本命令
- **正确答案**：A
- **解释**：Claude Code 有内置命令（如 /help、/compact）、skills（SKILL.md 文件）、插件命令（以 plugin-name:command 命名空间方式使用）和 MCP 提示（/mcp__server__prompt）。
- **复习**：Slash Commands 类型章节

### Q2
- **类别**：实践
- **题目**：如何将用户提供的所有参数传递给一个 skill？
- **选项**：A) 使用 `${args}` | B) 使用 `$ARGUMENTS` | C) 使用 `$@` | D) 使用 `$INPUT`
- **正确答案**：B
- **解释**：`$ARGUMENTS` 捕获命令名称之后的所有文本。对于位置参数，使用 `$0`、`$1` 等。
- **复习**：参数处理章节

### Q3
- **类别**：概念
- **题目**：当 skill（.claude/skills/name/SKILL.md）和旧版命令（.claude/commands/name.md）同名时，哪个优先？
- **选项**：A) 旧版命令 | B) skill | C) 先创建的那个 | D) Claude 让用户选择
- **正确答案**：B
- **解释**：同名情况下，skills 优先于旧版命令。skill 系统取代了旧的命令系统。
- **复习**：Skill 优先级章节

### Q4
- **类别**：实践
- **题目**：如何将实时 shell 输出注入到 skill 的提示中？
- **选项**：A) 使用 `$(command)` 语法 | B) 使用 `!`command`` （反引号加 !）语法 | C) 使用 `@shell:command` 语法 | D) 使用 `{command}` 语法
- **正确答案**：B
- **解释**：`!`command`` 语法运行一个 shell 命令，并在 Claude 看到之前将其输出注入到 skill 提示中。
- **复习**：动态上下文注入章节

### Q5
- **类别**：概念
- **题目**：skill 的 frontmatter 中 `disable-model-invocation: true` 有什么作用？
- **选项**：A) 完全阻止 skill 运行 | B) 只允许用户调用（Claude 不能自动调用） | C) 从 /help 菜单中隐藏 | D) 禁用 skill 的 AI 处理
- **正确答案**：B
- **解释**：`disable-model-invocation: true` 意味着只有用户可以通过 `/command-name` 触发该命令。Claude 永远不会自动调用它，适用于具有副作用的 skill（如部署）。
- **复习**：调用控制章节

### Q6
- **类别**：实践
- **题目**：你想创建一个只有 Claude 可以自动调用的 skill（从用户的 / 菜单中隐藏）。应该设置哪个 frontmatter 字段？
- **选项**：A) `disable-model-invocation: true` | B) `user-invocable: false` | C) `hidden: true` | D) `auto-only: true`
- **正确答案**：B
- **解释**：`user-invocable: false` 将 skill 从用户的 slash 菜单中隐藏，但允许 Claude 根据上下文自动调用它。
- **复习**：调用控制矩阵

### Q7
- **类别**：实践
- **题目**：名为 "deploy" 的新自定义 skill 的正确目录结构是什么？
- **选项**：A) `.claude/commands/deploy.md` | B) `.claude/skills/deploy/SKILL.md` | C) `.claude/skills/deploy.md` | D) `.claude/deploy/SKILL.md`
- **正确答案**：B
- **解释**：Skills 位于 `.claude/skills/` 下的目录中，目录内包含一个 `SKILL.md` 文件。目录名与命令名匹配。
- **复习**：Skill 类型和位置章节

### Q8
- **类别**：概念
- **题目**：插件命令如何避免与用户命令的名称冲突？
- **选项**：A) 使用 `plugin-name:command-name` 命名空间 | B) 有特殊的 .plugin 扩展名 | C) 以 `p/` 为前缀 | D) 自动覆盖用户命令
- **正确答案**：A
- **解释**：插件命令使用类似 `pr-review:check-security` 的命名空间来避免与独立用户命令的冲突。
- **复习**：插件命令章节

### Q9
- **类别**：实践
- **题目**：你想限制 skill 可以使用的工具。应该添加哪个 frontmatter 字段？
- **选项**：A) `tools: [Read, Grep]` | B) `allowed-tools: [Read, Grep]` | C) `permissions: [Read, Grep]` | D) `restrict-tools: [Read, Grep]`
- **正确答案**：B
- **解释**：SKILL.md frontmatter 中的 `allowed-tools` 字段限定了命令可以调用的工具范围。
- **复习**：Frontmatter 字段参考

### Q10
- **类别**：概念
- **题目**：skill 中的 `@file` 语法用于什么？
- **选项**：A) 导入另一个 skill | B) 引用文件以将其内容包含到提示中 | C) 创建符号链接 | D) 设置文件权限
- **正确答案**：B
- **解释**：skill 中的 `@path/to/file` 语法将引用文件的内容包含到提示中，允许 skill 引入模板或上下文文件。
- **复习**：文件引用章节

---

## 第 02 课：Memory

### Q1
- **类别**：概念
- **题目**：Claude Code 的内存层级有多少层，最高优先级的是什么？
- **选项**：A) 5 层，User Memory 最高 | B) 7 层，Managed Policy 最高 | C) 3 层，Project Memory 最高 | D) 7 层，Auto Memory 最高
- **正确答案**：B
- **解释**：层级有 7 层：Managed Policy > Project Memory > Project Rules > User Memory > User Rules > Local Project Memory > Auto Memory。Managed Policy（由管理员设置）具有最高优先级。
- **复习**：内存层级章节

### Q2
- **类别**：实践
- **题目**：如何在对话中快速添加新规则到内存？
- **选项**：A) 输入 `/memory add "rule text"` | B) 在消息前加 `#` 前缀（例如 `# always use TypeScript`） | C) 输入 `/rule "rule text"` | D) 使用 `@add-memory "rule text"`
- **正确答案**：B
- **解释**：`#` 前缀模式允许在对话中快速添加单条规则。Claude 会询问保存到哪个内存层级。
- **复习**：快速内存更新章节

### Q3
- **类别**：概念
- **题目**：CLAUDE.md 中 `@path/to/file` 导入的最大深度是多少？
- **选项**：A) 3 层 | B) 5 层 | C) 10 层 | D) 无限制
- **正确答案**：B
- **解释**：`@import` 语法支持递归导入，最大深度为 5 层，以防止无限循环。
- **复习**：导入语法章节

### Q4
- **类别**：实践
- **题目**：如何将规则文件限定为仅应用于 `src/api/` 中的文件？
- **选项**：A) 将规则放在 `src/api/CLAUDE.md` 中 | B) 在 `.claude/rules/*.md` 文件中添加 `paths: src/api/**` YAML frontmatter | C) 将文件命名为 `.claude/rules/api.md` | D) 在规则文件中使用 `@scope: src/api`
- **正确答案**：B
- **解释**：`.claude/rules/` 中的文件支持 `paths:` frontmatter 字段，使用 glob 模式将规则限定到特定目录。
- **复习**：路径特定规则章节

### Q5
- **类别**：概念
- **题目**：Auto Memory 的 MEMORY.md 在会话开始时加载多少行？
- **选项**：A) 所有行 | B) 前 100 行 | C) 前 200 行 | D) 前 500 行
- **正确答案**：C
- **解释**：MEMORY.md 的前 200 行在会话开始时自动加载到上下文中。从 MEMORY.md 引用的主题文件按需加载。
- **复习**：Auto Memory 章节

### Q6
- **类别**：实践
- **题目**：你想要不提交到 git 的个人项目偏好设置。应该使用哪个文件？
- **选项**：A) `~/.claude/CLAUDE.md` | B) `CLAUDE.local.md` | C) `.claude/rules/personal.md` | D) `.claude/memory/personal.md`
- **正确答案**：B
- **解释**：项目根目录下的 `CLAUDE.local.md` 用于个人项目特定的偏好设置。它应被 git 忽略。
- **复习**：内存位置比较

### Q7
- **类别**：概念
- **题目**：`/init` 命令做什么？
- **选项**：A) 从零开始初始化新的 Claude Code 项目 | B) 根据你的项目结构生成模板 CLAUDE.md | C) 将所有内存重置为默认值 | D) 创建新会话
- **正确答案**：B
- **解释**：`/init` 分析你的项目并生成带有建议规则和标准的模板 CLAUDE.md。这是一个一次性的引导工具。
- **复习**：/init 命令章节

### Q8
- **类别**：实践
- **题目**：如何完全禁用 Auto Memory？
- **选项**：A) 删除 ~/.claude/projects 目录 | B) 设置 `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1` | C) 在 CLAUDE.md 中添加 `auto-memory: false` | D) 使用 `/memory disable auto`
- **正确答案**：B
- **解释**：设置 `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1` 可禁用自动记忆。值为 `0` 强制启用。未设置 = 默认启用。
- **复习**：Auto Memory 配置章节

### Q9
- **类别**：概念
- **题目**：低优先级的内存层能否覆盖高优先级层的规则？
- **选项**：A) 是的，最近的规则总是优先 | B) 不能，高优先级层始终优先 | C) 是的，如果低优先级层使用 `!important` 标志 | D) 取决于规则类型
- **正确答案**：B
- **解释**：内存优先级从 Managed Policy 向下流动。低优先级层（如 Auto Memory）不能覆盖高优先级层（如 Project Memory）。
- **复习**：内存层级章节

### Q10
- **类别**：实践
- **题目**：你在两个仓库间工作，想让 Claude 同时加载两个仓库的 CLAUDE.md。应该使用什么标志？
- **选项**：A) `--multi-repo` | B) `--add-dir /path/to/other` | C) `--include /path/to/other` | D) `--merge-context /path/to/other`
- **正确答案**：B
- **解释**：`--add-dir` 标志从额外的目录加载 CLAUDE.md，实现多仓库上下文。
- **复习**：额外目录章节

---

## 第 03 课：Skills

### Q1
- **类别**：概念
- **题目**：skill 系统的渐进式披露有哪 3 个层级？
- **选项**：A) 元数据、说明、资源 | B) 名称、正文、附件 | C) 头部、内容、脚本 | D) 摘要、详情、数据
- **正确答案**：A
- **解释**：第 1 级：元数据（约 100 token，始终加载），第 2 级：SKILL.md 正文（<5k token，触发时加载），第 3 级：捆绑资源（scripts/references/assets，按需加载）。
- **复习**：渐进式披露架构章节

### Q2
- **类别**：实践
- **题目**：skill 被 Claude 自动调用的最重要因素是什么？
- **选项**：A) skill 的文件名 | B) frontmatter 中包含使用时机关键词的 `description` 字段 | C) skill 的目录位置 | D) `auto-invoke: true` frontmatter 字段
- **正确答案**：B
- **解释**：Claude 仅根据 `description` 字段决定是否自动调用 skill。它必须包含具体的触发短语和场景。
- **复习**：自动调用章节

### Q3
- **类别**：概念
- **题目**：SKILL.md 文件的建议最大长度是多少？
- **选项**：A) 100 行 | B) 250 行 | C) 500 行 | D) 1000 行
- **正确答案**：C
- **解释**：SKILL.md 应保持在 500 行以内。更大的参考材料应放在 `references/` 子目录文件中。
- **复习**：内容指南章节

### Q4
- **类别**：实践
- **题目**：如何让 skill 在拥有自己上下文的隔离子代理中运行？
- **选项**：A) 在 frontmatter 中设置 `isolation: true` | B) 在 frontmatter 中设置 `context: fork` 和 `agent` 字段 | C) 在 frontmatter 中设置 `subagent: true` | D) 将 skill 放在 `.claude/agents/`
- **正确答案**：B
- **解释**：`context: fork` 在独立的上下文中运行 skill，`agent` 字段指定使用哪种代理类型（例如 `Explore`、`Plan`、自定义代理）。
- **复习**：在子代理中运行 skills 章节

### Q5
- **类别**：概念
- **题目**：分配给 skill 元数据（第 1 级）的大约上下文预算是多少？
- **选项**：A) 上下文窗口的 0.5% | B) 上下文窗口的 1% | C) 上下文窗口的 5% | D) 上下文窗口的 10%
- **正确答案**：B
- **解释**：Skill 元数据占用约 1% 的上下文窗口（回退值：8,000 字符）。可通过 `SLASH_COMMAND_TOOL_CHAR_BUDGET` 配置。
- **复习**：上下文预算章节

### Q6
- **类别**：实践
- **题目**：一个 skill 需要引用大型 API 规范。应该放在哪里？
- **选项**：A) 内联到 SKILL.md 中 | B) 在 skill 目录内的 `references/api-spec.md` 文件中 | C) 在项目的 CLAUDE.md 中 | D) 在单独的 `.claude/rules/` 文件中
- **正确答案**：B
- **解释**：大型参考材料应放在 `references/` 子目录中。Claude 按需加载第 3 级资源，保持 SKILL.md 精简。
- **复习**：支持文件结构章节

### Q7
- **类别**：概念
- **题目**：skill 中参考内容和任务内容有什么区别？
- **选项**：A) 参考内容是只读的，任务内容是读写的 | B) 参考内容向上下文添加知识，任务内容提供分步指令 | C) 参考内容用于文档，任务内容用于代码 | D) 没有区别
- **正确答案**：B
- **解释**：参考内容向 Claude 的上下文添加领域知识（例如品牌指南）。任务内容提供工作流程的可操作分步指令。
- **复习**：Skill 内容类型章节

### Q8
- **类别**：实践
- **题目**：skill 的 frontmatter 中 `name` 字段允许使用哪些字符？
- **选项**：A) 任意字符 | B) 仅小写字母、数字和连字符（最多 64 个字符） | C) 字母和下划线 | D) 仅字母数字
- **正确答案**：B
- **解释**：name 必须是 kebab-case（小写、连字符），最多 64 个字符，且不能包含 "anthropic" 或 "claude"。
- **复习**：SKILL.md 格式章节

### Q9
- **类别**：概念
- **题目**：Claude 搜索 skills 的顺序是什么？
- **选项**：A) 用户 > 项目 > 企业 | B) 企业 > 个人 > 项目（插件使用命名空间） | C) 项目 > 用户 > 企业 | D) 按字母顺序
- **正确答案**：B
- **解释**：优先级顺序是：企业 > 个人 > 项目。插件 skills 使用命名空间（plugin-name:skill）因此不会冲突。
- **复习**：Skill 类型和位置章节

### Q10
- **类别**：实践
- **题目**：如何阻止 Claude 自动调用 skill，同时仍允许用户手动使用？
- **选项**：A) 设置 `user-invocable: false` | B) 设置 `disable-model-invocation: true` | C) 移除 description 字段 | D) 设置 `auto-invoke: false`
- **正确答案**：B
- **解释**：`disable-model-invocation: true` 阻止 Claude 自动调用，但保持 skill 在用户的 `/` 菜单中可用于手动使用。
- **复习**：调用控制章节

---

## 第 04 课：Subagents

### Q1
- **类别**：概念
- **题目**：子代理相比内联对话的主要优势是什么？
- **选项**：A) 它们更快 | B) 它们在独立的、干净的上下文窗口中运行，防止上下文污染 | C) 它们可以使用更多工具 | D) 它们有更好的错误处理
- **正确答案**：B
- **解释**：子代理获得全新的上下文窗口，只接收主代理传递的内容。这防止了主对话被任务特定的细节所污染。
- **复习**：概述章节

### Q2
- **类别**：实践
- **题目**：代理定义的优先级顺序是什么？
- **选项**：A) 项目 > 用户 > CLI | B) CLI > 项目 > 用户 | C) 用户 > 项目 > CLI | D) 它们优先级相同
- **正确答案**：B
- **解释**：CLI 定义的代理（`--agents` 标志）覆盖项目级（`.claude/agents/`），项目级覆盖用户级（`~/.claude/agents/`）。
- **复习**：文件位置章节

### Q3
- **类别**：概念
- **题目**：哪个内置子代理使用 Haiku 模型并针对只读代码库探索进行了优化？
- **选项**：A) general-purpose | B) Plan | C) Explore | D) Bash
- **正确答案**：C
- **解释**：Explore 子代理使用 Haiku 进行快速、只读的代码库探索。它支持三个彻底程度：快速、中等、非常彻底。
- **复习**：内置子代理章节

### Q4
- **类别**：实践
- **题目**：如何限制协调者代理可以生成的子代理？
- **选项**：A) 使用 `allowed-agents:` 字段 | B) 在 `tools` 字段中使用 `Task(agent_name)` 语法 | C) 设置 `spawn-limit: 2` | D) 使用 `restrict-agents: [name1, name2]`
- **正确答案**：B
- **解释**：在 tools 字段中添加 `Task(worker, researcher)` 创建一个允许列表 — 该代理只能生成名为 "worker" 或 "researcher" 的子代理。
- **复习**：限制可生成子代理章节

### Q5
- **类别**：概念
- **题目**：`isolation: worktree` 对子代理做了什么？
- **选项**：A) 在 Docker 容器中运行代理 | B) 给代理自己的 git worktree，使更改不影响主工作树 | C) 阻止代理读取任何文件 | D) 在沙盒中运行代理
- **正确答案**：B
- **解释**：Worktree 隔离创建单独的 git worktree。如果代理未做更改，则自动清理。如果有更改，则返回 worktree 路径和分支。
- **复习**：Worktree 隔离章节

### Q6
- **类别**：实践
- **题目**：如何让子代理在后台运行？
- **选项**：A) 在代理配置中设置 `background: true` | B) 在代理配置中使用 `async: true` | C) 启动后按 Ctrl+D | D) 使用 `--background` CLI 标志
- **正确答案**：A
- **解释**：代理配置中的 `background: true` 使子代理始终作为后台任务运行。用户也可以使用 Ctrl+B 将前台任务发送到后台。
- **复习**：后台子代理章节

### Q7
- **类别**：概念
- **题目**：带有 `project` 范围的 `memory` 字段对子代理做了什么？
- **选项**：A) 提供对项目 CLAUDE.md 的读取权限 | B) 创建一个范围限定为当前项目的持久化内存目录 | C) 共享主代理的对话历史 | D) 加载项目的 git 历史
- **正确答案**：B
- **解释**：`memory` 字段为子代理创建一个持久化目录。`project` 范围意味着内存绑定到当前项目。代理的 MEMORY.md 前 200 行会自动加载。
- **复习**：持久化内存章节

### Q8
- **类别**：实践
- **题目**：如何在子代理的描述中包含鼓励 Claude 自动委派任务的短语？
- **选项**：A) 添加 "priority: high" | B) 在描述中包含 "use PROACTIVELY" 或 "MUST BE USED" | C) 设置 `auto-delegate: true` | D) 添加 "trigger: always"
- **正确答案**：B
- **解释**：在描述中包含 "use PROACTIVELY" 或 "MUST BE USED" 等短语可以强烈鼓励 Claude 自动委派匹配的任务。
- **复习**：自动委派章节

### Q9
- **类别**：概念
- **题目**：子代理的有效 `permissionMode` 值有哪些？
- **选项**：A) read、write、admin | B) default、acceptEdits、bypassPermissions、plan、dontAsk、auto | C) safe、normal、dangerous | D) restricted、standard、elevated
- **正确答案**：B
- **解释**：子代理支持六种权限模式：default（提示所有操作）、acceptEdits（自动接受文件编辑）、bypassPermissions（跳过所有检查）、plan（只读）、dontAsk（自动拒绝除非预先批准）、auto（后台分类器决定）。
- **复习**：配置字段章节

### Q10
- **类别**：实践
- **题目**：如何恢复从上次运行返回了 agentId 的子代理？
- **选项**：A) 使用 `/resume agent-id` | B) 调用 Task 工具时传入带有 agentId 的 `resume` 参数 | C) 使用 `claude -r agent-id` | D) 子代理不能恢复
- **正确答案**：B
- **解释**：子代理可以通过传入带有先前返回的 agentId 的 `resume` 参数来恢复，并保留完整的上下文。
- **复习**：可恢复代理章节

---

## 第 05 课：MCP

### Q1
- **类别**：概念
- **题目**：MCP 的三种传输协议是什么，推荐使用哪种？
- **选项**：A) HTTP（推荐）、Stdio、SSE（已弃用） | B) WebSocket（推荐）、REST、gRPC | C) TCP、UDP、HTTP | D) Stdio（推荐）、HTTP、SSE
- **正确答案**：A
- **解释**：HTTP 推荐用于远程服务器。Stdio 用于本地进程（目前最常用）。SSE 已弃用但仍受支持。
- **复习**：传输协议章节

### Q2
- **类别**：实践
- **题目**：如何通过 CLI 添加 GitHub MCP 服务器？
- **选项**：A) `claude mcp install github` | B) `claude mcp add --transport http github https://api.github.com/mcp` | C) `claude plugin add github-mcp` | D) `claude connect github`
- **正确答案**：B
- **解释**：使用 `claude mcp add` 加上 `--transport` 标志、名称和服务器 URL。对于 stdio：`claude mcp add github -- npx -y @modelcontextprotocol/server-github`。
- **复习**：MCP 配置管理章节

### Q3
- **类别**：概念
- **题目**：当 MCP 工具描述超过上下文窗口的 10% 时会发生什么？
- **选项**：A) 它们被截断 | B) Tool Search 自动启用以动态选择相关工具 | C) Claude 显示错误 | D) 额外的工具被禁用
- **正确答案**：B
- **解释**：当工具超过上下文的 10% 时，MCP Tool Search 自动启用。它要求最低 Sonnet 4 或 Opus 4（不支持 Haiku）。
- **复习**：MCP Tool Search 章节

### Q4
- **类别**：实践
- **题目**：如何在 MCP 配置中使用环境变量回退？
- **选项**：A) `${VAR || "default"}` | B) `${VAR:-default}` | C) `${VAR:default}` | D) `${VAR ? "default"}`
- **正确答案**：B
- **解释**：`${VAR:-default}` 在环境变量未设置时提供回退值。不带回退的 `${VAR}` 如果未设置将报错。
- **复习**：环境变量展开章节

### Q5
- **类别**：概念
- **题目**：MCP 和 Memory 在数据访问方面有什么区别？
- **选项**：A) MCP 更快，Memory 更慢 | B) MCP 用于实时/变化的外部数据，Memory 用于持久化/静态的偏好设置 | C) MCP 用于代码，Memory 用于文本 | D) 它们可以互换
- **正确答案**：B
- **解释**：MCP 连接到实时、变化的外部数据源（API、数据库）。Memory 存储持久化的、静态的项目上下文和偏好设置。
- **复习**：MCP 与 Memory 对比章节

### Q6
- **类别**：实践
- **题目**：团队成员首次遇到项目范围的 `.mcp.json` 时会发生什么？
- **选项**：A) 自动加载 | B) 他们会收到信任项目 MCP 服务器的批准提示 | C) 除非他们通过设置选择启用，否则被忽略 | D) Claude 请求管理员批准
- **正确答案**：B
- **解释**：项目范围的 `.mcp.json` 在每个团队成员首次使用时触发安全批准提示。这是有意为之 — 防止不受信任的 MCP 服务器。
- **复习**：MCP 范围章节

### Q7
- **类别**：概念
- **题目**：`claude mcp serve` 做什么？
- **选项**：A) 启动 MCP 服务器仪表板 | B) 使 Claude Code 本身作为 MCP 服务器供其他应用程序使用 | C) 提供 MCP 文档 | D) 测试 MCP 服务器连接
- **正确答案**：B
- **解释**：`claude mcp serve` 将 Claude Code 变成一个 MCP 服务器，实现多代理编排，其中一个 Claude 实例可以被另一个控制。
- **复习**：Claude 作为 MCP 服务器章节

### Q8
- **类别**：实践
- **题目**：MCP 工具的默认最大输出大小是多少？
- **选项**：A) 5,000 token | B) 10,000 token | C) 25,000 token | D) 50,000 token
- **正确答案**：C
- **解释**：默认最大值为 25,000 token（`MAX_MCP_OUTPUT_TOKENS`）。10k token 时显示警告。磁盘持久化上限为 50k 字符。
- **复习**：MCP 输出限制章节

### Q9
- **类别**：概念
- **题目**：在托管配置中，`allowedMcpServers` 和 `deniedMcpServers` 同时匹配一个服务器时，哪个优先？
- **选项**：A) 允许优先 | B) 拒绝优先 | C) 最后配置的优先 | D) 两者独立应用
- **正确答案**：B
- **解释**：在托管 MCP 配置中，拒绝规则始终优先于允许规则。
- **复习**：托管 MCP 配置章节

### Q10
- **类别**：实践
- **题目**：如何在对话中引用 MCP 资源？
- **选项**：A) 使用 `/mcp resource-name` | B) 使用 `@server-name:protocol://resource/path` 提及语法 | C) 使用 `mcp.get("resource")` | D) 资源自动加载
- **正确答案**：B
- **解释**：MCP 资源通过对话中的 `@server-name:protocol://resource/path` 提及语法访问。
- **复习**：MCP 资源章节

---

## 第 06 课：Hooks

### Q1
- **类别**：概念
- **题目**：Claude Code 中有哪四种类型的 hooks？
- **选项**：A) Pre、Post、Error 和 Filter hooks | B) Command、HTTP、Prompt 和 Agent hooks | C) Before、After、Around 和 Through hooks | D) Input、Output、Filter 和 Transform hooks
- **正确答案**：B
- **解释**：Command hooks 运行 shell 脚本，HTTP hooks 调用 webhook 端点，Prompt hooks 使用单轮 LLM 评估，Agent hooks 使用基于子代理的验证。
- **复习**：Hook 类型章节

### Q2
- **类别**：实践
- **题目**：hook 脚本以退出码 2 退出。会发生什么？
- **选项**：A) 非阻塞警告显示 | B) 阻塞错误 — stderr 作为错误显示给 Claude，工具使用被阻止 | C) Hook 被重试 | D) 会话结束
- **正确答案**：B
- **解释**：退出码 0 = 成功/继续，退出码 2 = 阻塞错误（stderr 作为错误显示），任何其他非零值 = 非阻塞（stderr 仅在详细模式下显示）。
- **复习**：退出码章节

### Q3
- **类别**：概念
- **题目**：PreToolUse hook 通过 stdin 接收哪些 JSON 字段？
- **选项**：A) `tool_name` 和 `tool_output` | B) `session_id`、`tool_name`、`tool_input`、`hook_event_name`、`cwd` 等 | C) 仅 `tool_name` | D) 完整的对话历史
- **正确答案**：B
- **解释**：Hooks 通过 stdin 接收包含以下字段的 JSON 对象：session_id、transcript_path、hook_event_name、tool_name、tool_input、tool_use_id、cwd 和 permission_mode。
- **复习**：JSON 输入结构章节

### Q4
- **类别**：实践
- **题目**：PreToolUse hook 如何在执行前修改工具的输入参数？
- **选项**：A) 在 stderr 上返回修改后的 JSON | B) 在 stdout 上返回带有 `updatedInput` 字段的 JSON（退出码 0） | C) 写入临时文件 | D) Hooks 不能修改输入
- **正确答案**：B
- **解释**：PreToolUse hook 可以在 stdout 上输出带有 `"updatedInput": {...}` 的 JSON（退出码为 0），以在 Claude 使用之前修改工具的参数。
- **复习**：PreToolUse 输出章节

### Q5
- **类别**：概念
- **题目**：哪个 hook 事件支持 `CLAUDE_ENV_FILE` 将环境变量持久化到会话中？
- **选项**：A) PreToolUse | B) UserPromptSubmit | C) SessionStart | D) 所有事件
- **正确答案**：C
- **解释**：只有 SessionStart hooks 可以使用 `CLAUDE_ENV_FILE` 将环境变量持久化到会话中。
- **复习**：SessionStart 章节

### Q6
- **类别**：实践
- **题目**：你想要一个只在 skill 首次加载时运行一次的 hook，而不是每次工具调用时运行。应该添加什么字段？
- **选项**：A) `run-once: true` | B) 组件 hook 定义中的 `once: true` | C) `single: true` | D) `max-runs: 1`
- **正确答案**：B
- **解释**：组件范围的 hooks（在 SKILL.md 或代理 frontmatter 中定义）支持 `once: true` 以仅在首次激活时运行。
- **复习**：组件范围 hooks 章节

### Q7
- **类别**：概念
- **题目**：在子代理的 frontmatter 中定义的 Stop hook 会自动转换为什么？
- **选项**：A) PostToolUse hook | B) SubagentStop hook | C) SessionEnd hook | D) 保持为 Stop hook
- **正确答案**：B
- **解释**：当 Stop hook 放置在子代理的 frontmatter 中时，它会自动转换为 SubagentStop，以便在该特定子代理完成时运行。
- **复习**：组件范围 hooks 章节

### Q8
- **类别**：实践
- **题目**：如何将 hook 匹配到特定服务器的所有 MCP 工具？
- **选项**：A) `matcher: "mcp_github"` | B) `matcher: "mcp__github__.*"`（正则模式） | C) `matcher: "mcp:github:*"` | D) `matcher: "github-mcp"`
- **正确答案**：B
- **解释**：对匹配器使用正则模式。MCP 工具遵循 `mcp__server__tool` 命名约定，所以 `mcp__github__.*` 匹配所有 GitHub MCP 工具。
- **复习**：匹配器模式章节

### Q9
- **类别**：概念
- **题目**：Claude Code 总共支持多少个 hook 事件？
- **选项**：A) 10 | B) 16 | C) 25 | D) 30
- **正确答案**：C
- **解释**：Claude Code 支持 25 个 hook 事件：PreToolUse、PostToolUse、PostToolUseFailure、UserPromptSubmit、Stop、StopFailure、SubagentStop、SubagentStart、PermissionRequest、Notification、PreCompact、PostCompact、SessionStart、SessionEnd、WorktreeCreate、WorktreeRemove、ConfigChange、CwdChanged、FileChanged、TeammateIdle、TaskCompleted、TaskCreated、Elicitation、ElicitationResult、InstructionsLoaded。
- **复习**：Hook 事件表

### Q10
- **类别**：实践
- **题目**：你想调试为什么一个 hook 没有触发。最佳方法是什么？
- **选项**：A) 在 hook 脚本中添加 print 语句 | B) 使用 `--debug` 标志和 `Ctrl+O` 启用详细模式 | C) 检查系统日志 | D) Hooks 没有调试工具
- **正确答案**：B
- **解释**：`--debug` 标志和 `Ctrl+O` 详细模式显示 hook 执行细节，包括哪些 hooks 触发了、它们的输入和输出。
- **复习**：调试章节

---

## 第 07 课：Plugins

### Q1
- **类别**：概念
- **题目**：插件的核心清单文件是什么，它位于哪里？
- **选项**：A) 根目录中的 `plugin.yaml` | B) `.claude-plugin/plugin.json` | C) 带有 "claude" 键的 `package.json` | D) `.claude/plugin.md`
- **正确答案**：B
- **解释**：插件清单位于 `.claude-plugin/plugin.json`，必填字段包括：name、description、version、author。
- **复习**：插件定义结构章节

### Q2
- **类别**：实践
- **题目**：如何在发布前本地测试插件？
- **选项**：A) 使用 `/plugin test ./my-plugin` | B) 使用 `claude --plugin-dir ./my-plugin` | C) 使用 `claude plugin validate ./my-plugin` | D) 复制到 ~/.claude/plugins/
- **正确答案**：B
- **解释**：`--plugin-dir` 标志从本地目录加载插件进行测试。可重复使用以加载多个插件。
- **复习**：测试章节

### Q3
- **类别**：概念
- **题目**：在插件 hooks 和 MCP 配置中，什么环境变量可以引用插件的安装目录？
- **选项**：A) `$PLUGIN_HOME` | B) `${CLAUDE_PLUGIN_ROOT}` | C) `$PLUGIN_DIR` | D) `${CLAUDE_PLUGIN_PATH}`
- **正确答案**：B
- **解释**：`${CLAUDE_PLUGIN_ROOT}` 解析为插件的安装目录，使 hooks 和 MCP 配置中可以使用可移植的路径引用。
- **复习**：插件目录结构章节

### Q4
- **类别**：实践
- **题目**："pr-review" 插件中有一个名为 "check-security" 的命令。用户如何调用它？
- **选项**：A) `/check-security` | B) `/pr-review:check-security` | C) `/plugin pr-review check-security` | D) `/pr-review/check-security`
- **正确答案**：B
- **解释**：插件命令使用 `plugin-name:command-name` 命名空间来避免与用户命令和其他插件的冲突。
- **复习**：插件命令章节

### Q5
- **类别**：概念
- **题目**：插件可以捆绑哪些组件？
- **选项**：A) 仅命令和设置 | B) 命令、代理、skills、hooks、MCP 服务器、LSP 配置、设置、模板、脚本 | C) 仅命令、hooks 和 MCP 服务器 | D) 仅 skills 和代理
- **正确答案**：B
- **解释**：插件可以捆绑：commands/、agents/、skills/、hooks/hooks.json、.mcp.json、.lsp.json、settings.json、templates/、scripts/、docs/、tests/。
- **复习**：插件目录结构章节

### Q6
- **类别**：实践
- **题目**：如何从 GitHub 安装插件？
- **选项**：A) `claude plugin add github:username/repo` | B) `/plugin install github:username/repo` | C) `npm install @claude/username-repo` | D) `git clone` 然后 `claude plugin register`
- **正确答案**：B
- **解释**：使用 `/plugin install github:username/repo` 直接从 GitHub 仓库安装。
- **复习**：安装方式章节

### Q7
- **类别**：概念
- **题目**：插件 settings.json 中的 `agent` 键做什么？
- **选项**：A) 指定认证凭据 | B) 设置插件激活时的主线程代理 | C) 列出可用的子代理 | D) 配置代理权限
- **正确答案**：B
- **解释**：插件 settings.json 中的 `agent` 键指定在插件激活时使用哪个代理定义作为主线程代理。
- **复习**：插件设置章节

### Q8
- **类别**：实践
- **题目**：如何管理插件生命周期（启用/禁用/更新）？
- **选项**：A) 手动编辑配置文件 | B) 使用 `/plugin enable`、`/plugin disable`、`/plugin update plugin-name` | C) 使用 `claude plugin-manager` | D) 重新安装插件
- **正确答案**：B
- **解释**：Claude Code 提供 slash 命令进行完整的生命周期管理：启用、禁用、更新、卸载。
- **复习**：安装方式章节

### Q9
- **类别**：概念
- **题目**：插件相比独立的 skills/hooks/MCP 的主要优势是什么？
- **选项**：A) 插件更快 | B) 一键安装、版本管理、市场分发、捆绑所有组件 | C) 插件有更多权限 | D) 插件可离线工作
- **正确答案**：B
- **解释**：插件将多个组件打包成一个可安装的单元，具有版本管理、市场分发和自动更新功能 — 与独立组件的手动设置相比。
- **复习**：独立组件与插件对比章节

### Q10
- **类别**：实践
- **题目**：插件目录中的 hooks 配置位于哪里？
- **选项**：A) `.claude-plugin/hooks.json` | B) `hooks/hooks.json` | C) `plugin.json` 的 hooks 部分 | D) `.claude/settings.json`
- **正确答案**：B
- **解释**：插件 hooks 在插件目录结构中的 `hooks/hooks.json` 中配置。
- **复习**：插件 hooks 章节

---

## 第 08 课：Checkpoints

### Q1
- **类别**：概念
- **题目**：检查点捕获哪四样东西？
- **选项**：A) Git 提交、分支、标签、暂存 | B) 消息、文件修改、工具使用历史、会话上下文 | C) 代码、测试、日志、配置 | D) 输入、输出、错误、时间
- **正确答案**：B
- **解释**：检查点捕获对话消息、Claude 工具进行的文件修改、工具使用历史和会话上下文。
- **复习**：概述章节

### Q2
- **类别**：实践
- **题目**：如何访问检查点浏览器？
- **选项**：A) 使用 `/checkpoints` 命令 | B) 按 `Esc + Esc`（双击 Escape）或使用 `/rewind` | C) 使用 `/history` 命令 | D) 按 `Ctrl+Z`
- **正确答案**：B
- **解释**：双击 Escape（Esc+Esc）或 `/rewind` 命令打开检查点浏览器以选择恢复点。
- **复习**：访问检查点章节

### Q3
- **类别**：概念
- **题目**：有多少个回退选项，分别是什么？
- **选项**：A) 3 个：撤销、重做、重置 | B) 5 个：恢复代码+对话、恢复对话、恢复代码、从此处摘要、取消 | C) 2 个：完整恢复、部分恢复 | D) 4 个：代码、消息、两者、取消
- **正确答案**：B
- **解释**：5 个选项是：恢复代码和对话（完整回退）、仅恢复对话、仅恢复代码、从此处摘要（压缩）、取消。
- **复习**：回退选项章节

### Q4
- **类别**：实践
- **题目**：你在 Claude Code 中通过 Bash 使用了 `rm -rf temp/`，然后想回退。检查点能恢复这些文件吗？
- **选项**：A) 能，检查点捕获所有内容 | B) 不能，Bash 文件系统操作（rm、mv、cp）不被检查点追踪 | C) 仅当你使用 Edit 工具时可以 | D) 仅当启用了 autoCheckpoint 时可以
- **正确答案**：B
- **解释**：检查点仅追踪由 Claude 工具（Write、Edit）进行的文件更改。Bash 命令（如 rm、mv、cp）在检查点追踪之外运行。
- **复习**：限制章节

### Q5
- **类别**：概念
- **题目**：检查点保留多长时间？
- **选项**：A) 直到会话结束 | B) 7 天 | C) 30 天 | D) 无限期
- **正确答案**：C
- **解释**：检查点跨会话保留最多 30 天，之后自动清理。
- **复习**：检查点持久化章节

### Q6
- **类别**：实践
- **题目**：回退时"从此处摘要"做什么？
- **选项**：A) 从该点删除对话 | B) 将对话压缩为 AI 生成的摘要，同时在记录文件中保留原始内容 | C) 创建更改的项目符号列表 | D) 将对话导出到文件
- **正确答案**：B
- **解释**：摘要将对话压缩为更短的 AI 生成摘要。原始完整文本保留在记录文件中。
- **复习**：摘要选项章节

### Q7
- **类别**：概念
- **题目**：检查点何时自动创建？
- **选项**：A) 每 5 分钟 | B) 每次用户提示时 | C) 仅当你手动保存时 | D) 每次工具使用后
- **正确答案**：B
- **解释**：自动检查点在每次用户提示时创建，捕获 Claude 处理请求之前的状态。
- **复习**：自动检查点章节

### Q8
- **类别**：实践
- **题目**：如何禁用自动检查点创建？
- **选项**：A) 使用 `--no-checkpoints` 标志 | B) 在设置中设置 `autoCheckpoint: false` | C) 删除 checkpoints 目录 | D) 检查点不能禁用
- **正确答案**：B
- **解释**：在配置中设置 `autoCheckpoint: false` 可禁用自动检查点创建（默认为 true）。
- **复习**：配置章节

### Q9
- **类别**：概念
- **题目**：检查点是 git 提交的替代品吗？
- **选项**：A) 是的，它们更强大 | B) 不是，它们是互补的 — 检查点是会话范围的且会过期，git 是永久的且可共享的 | C) 对小项目来说是的 | D) 仅在单人开发中
- **正确答案**：B
- **解释**：检查点是临时的（30 天保留期）、会话范围的，不能共享。Git 提交是永久的、可审计的、可共享的。两者应一起使用。
- **复习**：与 git 集成章节

### Q10
- **类别**：实践
- **题目**：你想比较两种不同的方案。推荐的检查点工作流是什么？
- **选项**：A) 创建两个单独的会话 | B) 在方案 A 前创建检查点，尝试方案 A，回退到检查点，尝试方案 B，比较结果 | C) 改用 git 分支 | D) 没有好的方法来比较方案
- **正确答案**：B
- **解释**：分支策略：在干净状态时创建检查点，尝试方案 A，记录结果，回退到同一检查点，尝试方案 B。比较两种结果。
- **复习**：工作流模式章节

---

## 第 09 课：Advanced Features

### Q1
- **类别**：概念
- **题目**：Claude Code 中有哪六种权限模式？
- **选项**：A) read、write、execute、admin、root、sudo | B) default、acceptEdits、plan、auto、dontAsk、bypassPermissions | C) safe、normal、elevated、admin、unrestricted、god | D) view、edit、run、deploy、full、bypass
- **正确答案**：B
- **解释**：六种模式是：default（提示所有操作）、acceptEdits（自动接受文件编辑）、plan（只读分析）、auto（后台分类器决定）、dontAsk（自动拒绝除非预先批准）、bypassPermissions（跳过所有检查）。
- **复习**：权限模式章节

### Q2
- **类别**：实践
- **题目**：如何激活规划模式？
- **选项**：A) 仅通过 `/plan` 命令 | B) 通过 `/plan`、`Shift+Tab`/`Alt+M`、`--permission-mode plan` 标志或默认配置 | C) 仅通过 `--planning` 标志 | D) 规划始终开启
- **正确答案**：B
- **解释**：规划模式可通过多种方式激活：/plan 命令、Shift+Tab/Alt+M 键盘快捷键、--permission-mode plan CLI 标志或在配置中设为默认。
- **复习**：规划模式章节

### Q3
- **类别**：概念
- **题目**：`opusplan` 模型别名做什么？
- **选项**：A) 所有操作仅使用 Opus | B) 规划阶段使用 Opus，实现阶段使用 Sonnet | C) 使用专门针对规划优化的模型 | D) 自动启用规划模式
- **正确答案**：B
- **解释**：`opusplan` 是一个模型别名，在规划阶段使用 Opus（更高质量的分析），在执行阶段使用 Sonnet（更快的实现）。
- **复习**：规划模式章节

### Q4
- **类别**：实践
- **题目**：如何在会话中切换扩展思考？
- **选项**：A) 输入 `/think` | B) 按 `Option+T`（macOS）或 `Alt+T` | C) 使用 `--thinking` 标志 | D) 它始终启用且无法切换
- **正确答案**：B
- **解释**：Option+T（macOS）或 Alt+T 切换扩展思考。它默认对所有模型启用。Opus 4.6 支持自适应努力级别。
- **复习**：扩展思考章节

### Q5
- **类别**：概念
- **题目**："think" 或 "ultrathink" 是激活增强思考的特殊关键词吗？
- **选项**：A) 是的，它们激活更深层推理 | B) 不是，它们被当作普通提示文本处理，没有特殊行为 | C) 只有 "ultrathink" 是特殊的 | D) 它们仅适用于 Opus
- **正确答案**：B
- **解释**：文档明确说明这些只是普通的提示指令，不是特殊激活关键词。扩展思考通过 Alt+T 切换和环境变量控制。
- **复习**：扩展思考章节

### Q6
- **类别**：实践
- **题目**：如何在 CI/CD 流水线中运行 Claude 并获取结构化 JSON 输出和轮次限制？
- **选项**：A) `claude --ci --json --limit 3` | B) `claude -p --output-format json --max-turns 3 "review code"` | C) `claude --pipeline --format json` | D) `claude run --json --turns 3`
- **正确答案**：B
- **解释**：打印模式（`-p`）配合 `--output-format json` 和 `--max-turns` 是标准的 CI/CD 集成模式。
- **复习**：无头/打印模式章节

### Q7
- **类别**：概念
- **题目**：任务列表功能（Ctrl+T）提供什么？
- **选项**：A) 正在运行的后台进程列表 | B) 一个能在上下文压缩后保留、可通过 `CLAUDE_CODE_TASK_LIST_ID` 共享的持久待办列表 | C) 过去会话的历史 | D) 待处理的工具调用队列
- **正确答案**：B
- **解释**：任务列表（Ctrl+T）在上下文压缩后保持持久，可通过使用 `CLAUDE_CODE_TASK_LIST_ID` 的命名任务目录跨会话共享。
- **复习**：任务列表章节

### Q8
- **类别**：实践
- **题目**：如何在规划模式期间在外部编辑器中编辑计划？
- **选项**：A) 从终端复制粘贴 | B) 按 `Ctrl+G` 在外部编辑器中打开计划 | C) 使用 `/export-plan` 命令 | D) 计划不能在外部编辑
- **正确答案**：B
- **解释**：Ctrl+G 在你配置的外部编辑器中打开当前计划进行修改。
- **复习**：规划模式章节

### Q9
- **类别**：概念
- **题目**：`dontAsk` 和 `bypassPermissions` 模式有什么区别？
- **选项**：A) 它们相同 | B) `dontAsk` 自动拒绝除非预先批准；`bypassPermissions` 完全跳过所有检查 | C) `dontAsk` 用于文件；`bypassPermissions` 用于命令 | D) `bypassPermissions` 更安全
- **正确答案**：B
- **解释**：dontAsk 自动拒绝权限请求，除非匹配预先批准的模式。bypassPermissions 完全跳过所有安全检查 — 不适合日常使用。
- **复习**：权限模式章节

### Q10
- **类别**：实践
- **题目**：如何将 CLI 会话交接到桌面应用？
- **选项**：A) 使用 `/export` 命令 | B) 使用 `/desktop` 命令 | C) 复制会话 ID 并粘贴到应用中 | D) 会话不能在 CLI 和桌面间传输
- **正确答案**：B
- **解释**：`/desktop` 命令将当前 CLI 会话交接到原生桌面应用程序，用于可视化差异查看和多会话管理。
- **复习**：桌面应用章节

---

## 第 10 课：CLI Reference

### Q1
- **类别**：概念
- **题目**：Claude CLI 的两种主要模式是什么？
- **选项**：A) 在线和离线模式 | B) 交互式 REPL（`claude`）和打印模式（`claude -p`） | C) GUI 和终端模式 | D) 单次和批量模式
- **正确答案**：B
- **解释**：交互式 REPL 是默认的对话模式。打印模式（-p）是非交互式的、可脚本化的、可管道化的 — 一次响应后退出。
- **复习**：CLI 架构章节

### Q2
- **类别**：实践
- **题目**：如何将文件通过管道传入 Claude 并获取 JSON 输出？
- **选项**：A) `claude --file error.log --json` | B) `cat error.log | claude -p --output-format json "explain this"` | C) `claude < error.log --format json` | D) `claude -p --input error.log --json`
- **正确答案**：B
- **解释**：通过 stdin 将内容管道传入打印模式（-p），并使用 --output-format json 获取结构化输出。
- **复习**：交互式与打印模式章节

### Q3
- **类别**：概念
- **题目**：`-c` 和 `-r` 标志有什么区别？
- **选项**：A) 两者相同 | B) `-c` 继续最近的会话；`-r` 按名称或 ID 恢复 | C) `-c` 创建新会话；`-r` 恢复会话 | D) `-c` 用于代码；`-r` 用于审查
- **正确答案**：B
- **解释**：`-c/--continue` 恢复最近的对话。`-r/--resume "name"` 按名称或会话 ID 恢复特定会话。
- **复习**：会话管理章节

### Q4
- **类别**：实践
- **题目**：如何保证 Claude 输出符合 schema 的 JSON？
- **选项**：A) 仅使用 `--output-format json` | B) 使用 `--output-format json --json-schema '{"type":"object",...}'` | C) 使用 `--strict-json` 标志 | D) JSON 输出始终符合 schema
- **正确答案**：B
- **解释**：单独使用 `--output-format json` 只能尽力生成 JSON。添加带有 JSON Schema 定义的 `--json-schema` 才能保证输出匹配 schema。
- **复习**：输出和格式章节

### Q5
- **类别**：概念
- **题目**：哪个标志仅在打印模式（-p）中有效，在交互模式中无效？
- **选项**：A) `--model` | B) `--system-prompt-file` | C) `--verbose` | D) `--max-turns`
- **正确答案**：B
- **解释**：`--system-prompt-file` 从文件加载系统提示，但仅在打印模式中有效。对于交互式会话，使用 `--system-prompt`（内联字符串）。
- **复习**：系统提示标志比较表

### Q6
- **类别**：实践
- **题目**：如何将 Claude 限制为仅使用只读工具进行安全审计？
- **选项**：A) `claude --read-only "audit code"` | B) `claude --permission-mode plan --tools "Read,Grep,Glob" "audit code"` | C) `claude --safe-mode "audit code"` | D) `claude --no-write "audit code"`
- **正确答案**：B
- **解释**：将 `--permission-mode plan`（只读分析）与 `--tools`（特定工具白名单）结合使用，将 Claude 限制为仅执行读取操作。
- **复习**：工具和权限管理章节

### Q7
- **类别**：概念
- **题目**：代理定义的优先级顺序是什么？
- **选项**：A) 项目 > 用户 > CLI | B) CLI > 项目 > 用户 | C) 用户 > CLI > 项目 | D) 所有优先级相同
- **正确答案**：B
- **解释**：CLI 定义的代理（--agents 标志）具有最高优先级，然后是项目级（.claude/agents/），然后是用户级（~/.claude/agents/）。
- **复习**：代理配置章节

### Q8
- **类别**：实践
- **题目**：如何分叉现有会话以尝试不同方案而不丢失原始会话？
- **选项**：A) 使用 `/fork` 命令 | B) 使用 `--resume session-name --fork-session "branch name"` | C) 使用 `--clone session-name` | D) 使用 `/branch session-name`
- **正确答案**：B
- **解释**：`--resume` 配合 `--fork-session` 从恢复的会话创建新的独立分支，保留原始对话。
- **复习**：会话管理章节

### Q9
- **类别**：概念
- **题目**：用户已登录时 `claude auth status` 返回什么退出码？
- **选项**：A) 1 | B) 0 | C) 200 | D) 它不返回退出码
- **正确答案**：B
- **解释**：`claude auth status` 在已登录时返回退出码 0，未登录时返回 1。这使其可用于 CI/CD 认证检查的脚本化。
- **复习**：CLI 命令表

### Q10
- **类别**：实践
- **题目**：如何用 Claude 批量处理多个文件？
- **选项**：A) `claude --batch *.md` | B) 使用 for 循环：`for file in *.md; do claude -p "summarize: $(cat $file)" > ${file%.md}.json; done` | C) `claude -p --files *.md "summarize all"` | D) 不支持批量处理
- **正确答案**：B
- **解释**：使用 shell for 循环配合打印模式逐个处理文件。每次调用是独立的，可以产生结构化输出。
- **复习**：批量处理章节
