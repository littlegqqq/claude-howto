# SKILL

## 概览

Skills 是可以复用、可自动触发的能力包。一个 skill 通常包含 `SKILL.md`、参考文件、脚本和模板。Claude 在合适的场景下会自动加载它。

### 主要优点

- 可复用
- 可渐进加载
- 可以把脚本、模板、说明放在一起
- 适合标准化流程

## Skills 的工作方式：渐进式披露

Skills 不会一次性把所有内容都塞进上下文，而是按需加载。

### 三层加载

1. **只看描述**：先判断这个 skill 是否相关
2. **加载 `SKILL.md`**：读取核心说明
3. **按需加载支持文件**：脚本、模板、参考资料

## skill基本格式：

```markdown
---
name: my-command
description: 这个命令的作用，以及何时使用它
---

# 我的命令

当该命令被触发时，Claude 需要遵循的说明。

1. 第一步
2. 第二步
3. 第三步
```

### Frontmatter 参考

| 字段                         | 作用                                            | 默认值               |
| -------------------------- | --------------------------------------------- | ----------------- |
| `name`(必填)                 | 命令名（会变成 `/name`）                              | 目录名               |
| `description`(必填)          | 简短说明，帮助 Claude 判断何时使用                         | 第一段               |
| `argument-hint`            | 自动补全时显示的参数提示                                  | 无                 |
| `allowed-tools`            | 命令可无权限使用的工具                                   | 继承                |
| `model`                    | 指定要使用的模型                                      | 继承                |
| `disable-model-invocation` | 若为 `true`，只有用户能调用，Claude 不能自动调用               | `false`           |
| `user-invocable`           | 若为 `false`，不会出现在 `/` 菜单中                      | `true`            |
| `context`                  | 设为 `fork` 时，在隔离 subagent 中运行                  | 无                 |
| `agent`                    | `context: fork` 时使用的 agent 类型                 | `general-purpose` |
| `hooks`                    | Skill 范围内的 hooks（PreToolUse、PostToolUse、Stop） | 无                 |

### 参数

命令可以接收参数：

**使用 `$ARGUMENTS` 接收全部参数：**

```yaml
---
name: fix-issue
description: 根据编号修复 GitHub issue
---

按团队编码规范修复 #$ARGUMENTS
```

调用 `/fix-issue 123` 时，`$ARGUMENTS` 会变成 `123`。

**使用 `$0`、`$1` 等接收单个参数：**

```yaml
---
name: review-pr
description: 按优先级审查 PR
---

审查 #$0，优先级为 $1
```

调用 `/review-pr 456 high` 时，`$0="456"`，`$1="high"`。

### 用 Shell 命令注入动态上下文

在 prompt 发送前，可用 `!` 命令先执行 shell 命令：

```yaml
---
name: commit
description: 使用上下文创建 git commit
allowed-tools: Bash(git *)
---

## 上下文

- 当前 git 状态：!`git status`
- 当前 diff：!`git diff HEAD`
- 当前分支：!`git branch --show-current`
- 最近提交：!`git log --oneline -5`

## 你的任务

根据以上变更，创建一个 git commit。
```

### 文件引用

使用 `@` 引用文件内容：

```md
审查 @src/utils/helpers.js 中的实现比较 @src/old-version.js 和 @src/new-version.js
```

## 插件命令

插件可以提供自定义命令：

```
/plugin-name:command-name
```

如果没有命名冲突，也可以直接使用 `/command-name`。

## MCP Prompts 作为命令

MCP servers 可以把 prompt 暴露成 slash command：

```
/mcp__<server-name>__<prompt-name> [arguments]
```

**示例：**

```shell
/mcp__github__list_prs/mcp__github__pr_review 456/mcp__jira__create_issue "Bug title" high
```

### MCP 权限语法

在权限中控制 MCP server 访问：

- `mcp__github` - 访问整个 GitHub MCP server
- `mcp__github__*` - 通配符访问全部工具
- `mcp__github__get_issue` - 访问某个特定工具

## 创建你自己的命令

### Skill 模板（推荐）

创建 `.claude/skills/my-command/SKILL.md`：

```yaml
---
name: my-command
description: 这个命令做什么。用于 [触发条件]。
argument-hint: [可选参数]
allowed-tools: Bash(npm *), Read, Grep
---

# Command Title

## 上下文

- 当前分支：!`git branch --show-current`
- 相关文件：@package.json

## 指令

1. 第一步
2. 第二步，参数：$ARGUMENTS
3. 第三步

## 输出格式

- 如何格式化回复
- 需要包含什么
```

### 仅用户可调用的命令（无自动触发）

对于带副作用、Claude 不应自动触发的命令：

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

## 在 subagent 中运行 Skills

某些 skill 适合在隔离上下文中运行，这样可以降低对主会话的影响。

---

## 最佳实践

### 1. 描述要具体

让 `description` 明确说明这个 skill 在什么时候触发。

### 2. 保持聚焦

一个 skill 解决一个问题，不要什么都塞进去。

### 3. 包含触发词

在描述里加入相关关键词，帮助 Claude 更准确地选择它。

### 4. `SKILL.md` 不要太长

尽量控制在 500 行以内，超过就拆分支持文件。

### 5. 引用支持文件

把重复内容移到脚本或参考文件中，主文件只保留核心说明。

---


