# Checkpoints 与 Rewind

## 核心概念

| 概念               | 说明                        |
| ---------------- | ------------------------- |
| **Checkpoint**   | 保存消息、文件和上下文的对话快照          |
| **Rewind**       | 回到之前的 checkpoint，并丢弃之后的更改 |
| **Branch Point** | 从同一个 checkpoint 出发，探索多个方案 |

## 访问 Checkpoints

你可以通过两种主要方式访问和管理 checkpoints：

### 使用键盘快捷键

按两次 `Esc`（`Esc` + `Esc`）打开 checkpoint 界面并浏览已保存的 checkpoints。

### 使用 Slash Command

使用 `/rewind` 命令（别名：`/checkpoint`）快速进入：

```shell
# 打开 rewind 界面
/rewind

# 或者使用别名
/checkpoint
```

## ## Rewind 选项

回退时，你会看到一个包含五个选项的菜单：

1. **恢复代码和对话** - 把文件和消息都恢复到那个 checkpoint
2. **恢复对话** - 只回退消息，保留当前代码不变
3. **恢复代码** - 只回退文件修改，保留完整对话历史
4. **从这里开始总结** - 把从这里往后的对话压缩成 AI 生成的摘要，而不是直接丢弃。原始消息仍会保留在记录中。你也可以额外指定摘要应聚焦哪些主题。
5. **算了** - 取消并返回当前状态

## 自动 Checkpoints

Claude Code 会自动为你创建 checkpoints：

- **每次用户提示** - 每次用户输入都会创建一个新 checkpoint
- **持久保存** - checkpoints 会跨会话保留
- **自动清理** - 30 天后自动清理旧 checkpoints
