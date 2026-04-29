<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 检查点与回退

检查点允许你保存对话状态并回退到 Claude Code 会话中的先前时间点。这对于探索不同方案、从错误中恢复或比较替代解决方案非常有价值。

## 概述

检查点允许你保存对话状态并回退到先前时间点，从而实现安全的实验和多种方案的探索。它们是对话状态的快照，包括：
- 所有交换的消息
- 所做的文件修改
- 工具使用历史
- 会话上下文

在探索不同方案、从错误中恢复或比较替代解决方案时，检查点非常有价值。

## 核心概念

| 概念 | 描述 |
|---------|-------------|
| **检查点（Checkpoint）** | 对话状态的快照，包括消息、文件和上下文 |
| **回退（Rewind）** | 返回到先前的检查点，丢弃后续更改 |
| **分支点（Branch Point）** | 从该检查点出发探索多种方案 |

## 访问检查点

你可以通过两种主要方式访问和管理检查点：

### 使用键盘快捷键
按两次 `Esc`（`Esc` + `Esc`）打开检查点界面并浏览已保存的检查点。

### 使用斜杠命令
使用 `/rewind` 命令（别名：`/checkpoint`）快速访问：

```bash
# Open rewind interface
/rewind

# Or use the alias
/checkpoint
```

## 回退选项

当你执行回退时，会看到一个包含五个选项的菜单：

1. **恢复代码和对话** -- 将文件和消息都恢复到该检查点
2. **恢复对话** -- 仅回退消息，保持当前代码不变
3. **恢复代码** -- 仅恢复文件更改，保留完整的对话历史
4. **从此处总结** -- 将从该时间点开始的对话压缩为 AI 生成的摘要，释放上下文窗口空间。选定时间点之前的消息保持不变。磁盘上的文件不会更改。原始消息保留在会话记录中。你可以选择性地提供指令，让摘要聚焦于特定主题。
5. **取消** -- 取消并返回当前状态

> **注意**：恢复对话或总结后，选定消息的原始提示会恢复到输入框中，以便你重新发送或编辑。

## 自动检查点

Claude Code 会自动为你创建检查点：

- **每次用户提示** - 每次用户输入都会创建一个新的检查点
- **持久化** - 检查点在会话之间持久保存
- **自动清理** - 检查点在 30 天后自动清理

这意味着你可以随时回退到对话中的任何先前时间点，无论是几分钟前还是几天前。

## 使用场景

| 场景 | 工作流程 |
|----------|----------|
| **探索方案** | 保存 → 尝试 A → 保存 → 回退 → 尝试 B → 比较 |
| **安全重构** | 保存 → 重构 → 测试 → 如果失败：回退 |
| **A/B 测试** | 保存 → 设计 A → 保存 → 回退 → 设计 B → 比较 |
| **错误恢复** | 发现问题 → 回退到上一个正常状态 |

## 使用检查点

### 查看和回退

按两次 `Esc` 或使用 `/rewind` 打开检查点浏览器。你会看到所有可用检查点及其时间戳的列表。选择任意检查点即可回退到该状态。

### 检查点详情

每个检查点显示：
- 创建时间戳
- 被修改的文件
- 对话中的消息数量
- 使用的工具

## 实际示例

### 示例 1：探索不同方案

```
User: Let's add a caching layer to the API

Claude: I'll add Redis caching to your API endpoints...
[Makes changes at checkpoint A]

User: Actually, let's try in-memory caching instead

Claude: I'll rewind to explore a different approach...
[User presses Esc+Esc and rewinds to checkpoint A]
[Implements in-memory caching at checkpoint B]

User: Now I can compare both approaches
```

### 示例 2：从错误中恢复

```
User: Refactor the authentication module to use JWT

Claude: I'll refactor the authentication module...
[Makes extensive changes]

User: Wait, that broke the OAuth integration. Let's go back.

Claude: I'll help you rewind to before the refactoring...
[User presses Esc+Esc and selects the checkpoint before the refactor]

User: Let's try a more conservative approach this time
```

### 示例 3：安全实验

```
User: Let's try rewriting this in a functional style
[Creates checkpoint before experiment]

Claude: [Makes experimental changes]

User: The tests are failing. Let's rewind.
[User presses Esc+Esc and rewinds to the checkpoint]

Claude: I've rewound the changes. Let's try a different approach.
```

### 示例 4：分支方案

```
User: I want to compare two database designs
[Takes note of checkpoint - call it "Start"]

Claude: I'll create the first design...
[Implements Schema A]

User: Now let me go back and try the second approach
[User presses Esc+Esc and rewinds to "Start"]

Claude: Now I'll implement Schema B...
[Implements Schema B]

User: Great! Now I have both schemas to choose from
```

## 检查点保留

Claude Code 自动管理你的检查点：

- 检查点随每次用户提示自动创建
- 旧检查点保留最多 30 天
- 检查点会自动清理以防止存储无限增长

## 工作流程模式

### 探索的分支策略

探索多种方案时：

```
1. Start with initial implementation → Checkpoint A
2. Try Approach 1 → Checkpoint B
3. Rewind to Checkpoint A
4. Try Approach 2 → Checkpoint C
5. Compare results from B and C
6. Choose best approach and continue
```

### 安全重构模式

进行重大更改时：

```
1. Current state → Checkpoint (auto)
2. Start refactoring
3. Run tests
4. If tests pass → Continue working
5. If tests fail → Rewind and try different approach
```

## 最佳实践

由于检查点是自动创建的，你可以专注于工作而不必担心手动保存状态。但请记住以下实践：

### 有效使用检查点

✅ **应该做的：**
- 回退前查看可用的检查点
- 当你想探索不同方向时使用回退
- 保留检查点以比较不同方案
- 了解每个回退选项的作用（恢复代码和对话、恢复对话、恢复代码或总结）

❌ **不应该做的：**
- 仅依赖检查点来保存代码
- 期望检查点跟踪外部文件系统更改
- 将检查点作为 git 提交的替代品

## 配置

检查点是 Claude Code 的内置默认功能，无需任何配置即可启用。每次用户提示都会自动创建一个检查点。

唯一与检查点相关的设置是 `cleanupPeriodDays`，它控制会话和检查点的保留时间：

```json
{
  "cleanupPeriodDays": 30
}
```

- `cleanupPeriodDays`：保留会话历史和检查点的天数（默认值：`30`）

> **v2.1.117 更新**：`cleanupPeriodDays` 现在管理四个磁盘缓存的保留策略，而不仅仅是检查点：
>
> - 会话检查点
> - `~/.claude/tasks/` — 持久化任务列表
> - `~/.claude/shell-snapshots/` — 捕获的 shell 环境快照
> - `~/.claude/backups/` — 滚动设置 / CLAUDE.md 备份
>
> 单一设置现在可以在相同天数后统一清理所有四个目录。

## 限制

检查点有以下限制：

- **Bash 命令更改不被跟踪** - 文件系统上的 `rm`、`mv`、`cp` 等操作不会被检查点捕获
- **外部更改不被跟踪** - 在 Claude Code 之外进行的更改（在编辑器、终端等中）不会被捕获
- **不能替代版本控制** - 对代码库的永久性、可审计的更改请使用 git

## 故障排除

### 检查点缺失

**问题**：找不到预期的检查点

**解决方案**：
- 检查检查点是否已被清除
- 检查磁盘空间
- 确保 `cleanupPeriodDays` 设置得足够大（默认：30 天）

### 回退失败

**问题**：无法回退到检查点

**解决方案**：
- 确保没有未提交的更改产生冲突
- 检查检查点是否损坏
- 尝试回退到其他检查点

## 与 Git 的集成

检查点是 git 的补充（而非替代）：

| 特性 | Git | 检查点 |
|---------|-----|-------------|
| 范围 | 文件系统 | 对话 + 文件 |
| 持久性 | 永久 | 基于会话 |
| 粒度 | 提交 | 任意时间点 |
| 速度 | 较慢 | 即时 |
| 共享 | 是 | 有限 |

结合使用两者：
1. 使用检查点进行快速实验
2. 使用 git 提交确定的更改
3. 在 git 操作前创建检查点
4. 将成功的检查点状态提交到 git

## 快速入门指南

### 基本工作流程

1. **正常工作** - Claude Code 自动创建检查点
2. **想要回退？** - 按两次 `Esc` 或使用 `/rewind`
3. **选择检查点** - 从列表中选择要回退的检查点
4. **选择恢复内容** - 从恢复代码和对话、恢复对话、恢复代码、从此处总结或取消中选择
5. **继续工作** - 你已回到该时间点

### 键盘快捷键

- **`Esc` + `Esc`** - 打开检查点浏览器
- **`/rewind`** - 访问检查点的另一种方式
- **`/checkpoint`** - `/rewind` 的别名

## 何时应该回退：上下文监控

检查点让你可以回退——但你怎么知道*何时*应该回退？随着对话增长，Claude 的上下文窗口会被填满，模型质量会悄然下降。你可能在不知不觉中使用了一个"半盲"模型来生成代码。

**[cc-context-stats](https://github.com/luongnv89/cc-context-stats)** 通过在 Claude Code 状态栏中添加实时**上下文区域**来解决这个问题。它跟踪你在上下文窗口中的位置——从 **Plan**（绿色，安全地规划和编码）到 **Code**（黄色，避免开始新计划）再到 **Dump**（橙色，完成并回退）。当你看到区域变化时，就知道是时候创建检查点并重新开始，而不是在输出质量下降的情况下继续推进。

## 相关概念

- **[高级功能](../09-advanced-features/)** - 规划模式和其他高级功能
- **[记忆管理](../02-memory/)** - 管理对话历史和上下文
- **[斜杠命令](../01-slash-commands/)** - 用户调用的快捷方式
- **[钩子](../06-hooks/)** - 事件驱动的自动化
- **[插件](../07-plugins/)** - 捆绑扩展包

## 附加资源

- [官方检查点文档](https://code.claude.com/docs/en/checkpointing)
- [高级功能指南](../09-advanced-features/) - 扩展思考和其他功能

## 总结

检查点是 Claude Code 的一项自动功能，让你可以安全地探索不同方案而不必担心丢失工作。每次用户提示都会自动创建一个新的检查点，因此你可以回退到会话中的任何先前时间点。

主要优势：
- 无所畏惧地尝试多种方案
- 快速从错误中恢复
- 并排比较不同的解决方案
- 安全地与版本控制系统集成

请记住：检查点不能替代 git。使用检查点进行快速实验，使用 git 进行永久性代码更改。

---

**Last Updated**: April 24, 2026
**Claude Code Version**: 2.1.119
**Sources**:
- https://code.claude.com/docs/en/checkpointing
- https://code.claude.com/docs/en/settings
- https://github.com/anthropics/claude-code/releases/tag/v2.1.117
**Compatible Models**: Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5
