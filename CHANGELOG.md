# 变更日志

## [v2.4.0] — 2026-04-27

### 同步至 Claude Code v2.1.119

将教程覆盖范围从 Claude Code v2.1.112 → v2.1.119（2026 年 4 月 23 日发布）。
v2.1.120 于 4 月 24 日发布后因回归问题被回滚；客户端
自动回滚至 v2.1.119，该版本仍为稳定目标。

### 新增（英文文档）

- 原生二进制打包说明（v2.1.113）— CLI 现在按平台发布原生二进制文件
- `bfs`/`ugrep` Glob/Grep 替代方案脚注，适用于原生 macOS/Linux 构建（v2.1.117）
- `mcp_tool` 钩子类型及示例（v2.1.118）
- PostToolUse / PostToolUseFailure 输入中的 `duration_ms` 字段（v2.1.119）
- `prUrlTemplate` 设置（v2.1.119）及扩展的 `--from-pr` 提供商列表（GitLab、Bitbucket）
- `cleanupPeriodDays` 扩展范围（检查点 + 任务 + shell 快照 + 备份，v2.1.117）
- 插件市场在每个生命周期事件上强制执行（v2.1.117）及 `hostPattern`/`pathPattern` 正则表达式（v2.1.119）
- 新增环境变量：`DISABLE_UPDATES`、`CLAUDE_CODE_HIDE_CWD`、`CLAUDE_CODE_FORK_SUBAGENT`、`OTEL_LOG_TOOL_DETAILS`、`ENABLE_TOOL_SEARCH` Vertex 可选启用
- 新增斜杠命令：`/btw`、`/theme` 及自定义主题
- `/usage` 规范命令（合并 `/cost` + `/stats`，v2.1.118）
- 分叉子代理（`CLAUDE_CODE_FORK_SUBAGENT=1`，v2.1.117）
- 自动模式 `"$defaults"` 令牌（v2.1.118）
- `wslInheritsWindowsSettings` 托管策略（v2.1.118）
- Vim 可视 / 行可视模式（v2.1.118）
- `claude install [version]` 和 `claude plugin tag` 子命令

### 变更

- 文档托管迁移：`docs.anthropic.com/en/docs/claude-code/*` → `code.claude.com/docs/en/*`
- Opus 4.7 工作量级别：`xhigh` 现为 Claude Code 自 2026-04-16 发布以来的默认值；Opus 4.7 原生上下文窗口确认为 1M（v2.1.117 修复了 `/context` 将其误报为 200K 的问题）
- Pro/Max 订阅者在 Opus 4.6 / Sonnet 4.6 上的默认工作量从 `medium` 提升至 `high`（v2.1.117）
- `STYLE_GUIDE.md` 来源 URL 从 Claude Apps 文章更新为 `code.claude.com/docs/en/changelog`

### 弃用（已跟踪，尚未移除）

- `includeCoAuthoredBy` 设置 → 请使用 `attribution.commit` / `attribution.pr`
- `voiceEnabled` 设置 → 请使用 `voice.enabled`

### 翻译维护者须知

`vi/`、`zh/` 和 `uk/` 本地化目录树由社区维护，可能落后于英文源文件。同步翻译的贡献者应将本次发布中更新的英文文件进行差异比对。

## v2.1.112 — 2026-04-16

### 亮点

- 将所有英文教程与 Claude Code v2.1.112 及新的 Opus 4.7 模型（`claude-opus-4-7`）同步，包括新的 `xhigh` 工作量级别（Opus 4.7 默认，介于 `high` 和 `max` 之间）、两个新的内置斜杠命令（`/ultrareview`、`/less-permission-prompts`）、Max 订阅者在 Opus 4.7 上自动模式不再需要 `--enable-auto-mode`、Windows 上的 PowerShell 工具、"Auto (match terminal)" 主题以及以提示词命名的计划文件。所有 18 个英文文档页脚已更新至 Claude Code v2.1.112。@Luong NGUYEN

### 功能

- 新增完整的乌克兰语（uk）本地化，涵盖所有模块、根目录文档、示例和参考资料 (039dde2) @Evgenij I

### 错误修复

- 修正 pre-tool-check.sh 钩子协议错误 (bce7cf8) @yarlinghe
- 将错误的 Mermaid 示例更改为文本块以通过 CI (b8a7b1f) @Evgenij I
- 修复乌克兰语 claude_concepts_guide.md 目录中的 CP1251 编码问题 (d970cc6) @Evgenij I
- 用完整翻译替换乌克兰语 README 占位内容，修复损坏的锚点 (f6d73e2) @Evgenij I
- 修正所有页脚中的 Claude Code 版本为 2.1.97 (63a1416) @Luong NGUYEN
- 应用 2026-04-09 文档准确性更新 (e015f39) @Luong NGUYEN

### 文档

- 同步至 Claude Code v2.1.112（Opus 4.7、`xhigh` 工作量、`/ultrareview`、`/less-permission-prompts`、PowerShell 工具、自动匹配终端主题）@Luong NGUYEN
- 同步至 Claude Code v2.1.110（TUI、推送通知、会话回顾）(15f0085) @Luong NGUYEN
- 同步至 Claude Code v2.1.101，包含 `/team-onboarding`、`/ultraplan`、Monitor 工具 (2deba3a) @Luong NGUYEN
- 同步越南语文档与英文源文件 (561c6cb) @Thiên Toán
- 更新所有文件的最后更新日期和 Claude Code 版本 (7f2e773) @Luong NGUYEN
- 在语言切换器中添加乌克兰语链接 (9c224ff) @Luong NGUYEN
- 移除贡献者章节 (f07313d) @Luong NGUYEN
- 更新 GitHub 指标至 21,800+ 星标、2,585+ 分支 (4f55374) @Luong NGUYEN

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.3.0...v2.1.112

---

## v2.3.0 — 2026-04-07

### 功能

- 按语言构建和发布 EPUB 产物 (90e9c30) @Thiên Toán
- 在 06-hooks 中添加缺失的 pre-tool-check.sh 钩子 (b511ed1) @JiayuWang
- 在 zh/ 目录中添加中文翻译 (89e89d4) @Luong NGUYEN
- 新增性能优化器子代理和依赖检查钩子 (f53d080) @qk

### 错误修复

- Windows Git Bash 兼容性 + stdin JSON 协议 (2cbb10c) @Luong NGUYEN
- 修正 08-checkpoints 中的 autoCheckpoint 配置文档 (749c79f) @JiayuWang
- 嵌入 SVG 图像而非替换为占位符 (1b16709) @Thiên Toán
- 修复 memory README 中的嵌套代码围栏渲染 (ce24423) @Zhaoshan Duan
- 应用被 squash 合并丢弃的审查修复 (34259ca) @Luong NGUYEN
- 使钩子脚本兼容 Windows Git Bash 并使用 stdin JSON 协议 (107153d) @binyu li

### 文档

- 同步所有教程与最新 Claude Code 文档（2026 年 4 月）(72d3b01) @Luong NGUYEN
- 在语言切换器中添加中文链接 (6cbaa4d) @Luong NGUYEN
- 添加英语和越南语之间的语言切换器 (100c45e) @Luong NGUYEN
- 添加 GitHub #1 趋势徽章 (0ca8c37) @Luong NGUYEN
- 引入 cc-context-stats 用于上下文区域监控 (d41b335) @Luong NGUYEN
- 引入 luongnv89/skills 集合和 luongnv89/asm 技能管理器 (7e3c0b6) @Luong NGUYEN
- 更新 README 统计数据以反映当前 GitHub 指标（5,900+ 星标、690+ 分支）(5001525) @Luong NGUYEN
- 更新 README 统计数据以反映当前 GitHub 指标（3,900+ 星标、460+ 分支）(9cb92d6) @Luong NGUYEN

### 重构

- 用本地 mmdc 渲染替换 Kroki HTTP 依赖 (e76bbe4) @Luong NGUYEN
- 将质量检查前移至 pre-commit，CI 作为第二道关卡 (6d1e0ae) @Luong NGUYEN
- 收窄自动模式权限基线 (2790fb2) @Luong NGUYEN
- 用一次性权限设置脚本替换自动适配钩子 (995a5d6) @Luong NGUYEN

### 其他

- 左移质量门禁 — 将 mypy 添加到 pre-commit，修复 CI 失败 (699fb39) @Luong NGUYEN
- 新增越南语（Tiếng Việt）本地化 (a70777e) @Thiên Toán

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.2.0...v2.3.0

---

## v2.2.0 — 2026-03-26

### 文档

- 将所有教程和参考资料与 Claude Code v2.1.84 同步 (f78c094) @luongnv89
  - 将斜杠命令更新为 55+ 内置 + 5 个捆绑技能，标记 3 个为已弃用
  - 将钩子事件从 18 个扩展到 25 个，添加 `agent` 钩子类型（现为 4 种类型）
  - 在高级功能中添加自动模式、频道、语音听写
  - 添加 `effort`、`shell` 技能前置字段；`initialPrompt`、`disallowedTools` 代理字段
  - 添加 WebSocket MCP 传输、引出（elicitation）、2KB 工具上限
  - 添加插件 LSP 支持、`userConfig`、`${CLAUDE_PLUGIN_DATA}`
  - 更新所有参考文档（CATALOG、QUICK_REFERENCE、LEARNING-ROADMAP、INDEX）
- 将 README 重写为着陆页式结构化指南 (32a0776) @luongnv89

### 错误修复

- 添加缺失的 cSpell 词汇和 README 章节以符合 CI 要求 (93f9d51) @luongnv89
- 将 `Sandboxing` 添加到 cSpell 词典 (b80ce6f) @luongnv89

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### 错误修复

- 移除导致 CI 链接检查失败的无效市场链接 (3fdf0d6) @luongnv89
- 将 `sandboxed` 和 `pycache` 添加到 cSpell 词典 (dc64618) @luongnv89

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### 功能

- 新增自适应学习路径，包含自我评估和课程测验技能 (1ef46cd) @luongnv89
  - `/self-assessment` — 涵盖 10 个功能领域的交互式能力测验，附带个性化学习路径
  - `/lesson-quiz [lesson]` — 每课知识检测，包含 8-10 道针对性题目

### 错误修复

- 更新失效的 URL、弃用项和过时的引用 (8fe4520) @luongnv89
- 修复资源和自我评估技能中的损坏链接 (7a05863) @luongnv89
- 在概念指南中对嵌套代码块使用波浪线围栏 (5f82719) @VikalpP
- 将缺失的单词添加到 cSpell 词典 (8df7572) @luongnv89

### 文档

- 第 5 阶段 QA — 修复文档间的一致性、URL 和术语问题 (00bbe4c) @luongnv89
- 完成第 3-4 阶段 — 新功能覆盖和参考文档更新 (132de29) @luongnv89
- 将 MCPorter 运行时添加到 MCP 上下文膨胀章节 (ef52705) @luongnv89
- 在 6 份指南中添加缺失的命令、功能和设置 (4bc8f15) @luongnv89
- 基于现有仓库惯例添加风格指南 (84141d0) @luongnv89
- 在指南对比表中添加自我评估行 (8fe0c96) @luongnv89
- 将 @VikalpP 添加到贡献者列表（PR #7）(d5b4350) @luongnv89
- 在 README 和学习路线图中添加自我评估和课程测验技能引用 (d5a6106) @luongnv89

### 新贡献者

- @VikalpP 在 #7 中做出了首次贡献

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### 功能

- 将所有文档与 Claude Code 2026 年 2 月新功能同步 (487c96d)
  - 更新 10 个教程目录和 7 份参考文档中的共计 26 个文件
  - 新增 **自动记忆（Auto Memory）** 文档 — 按项目持久化学习内容
  - 新增 **远程控制（Remote Control）**、**Web 会话（Web Sessions）** 和 **桌面应用（Desktop App）** 文档
  - 新增 **代理团队（Agent Teams）** 文档（实验性多代理协作）
  - 新增 **MCP OAuth 2.0**、**工具搜索（Tool Search）** 和 **Claude.ai 连接器（Connectors）** 文档
  - 新增 **持久化记忆（Persistent Memory）** 和子代理 **Worktree 隔离（Worktree Isolation）** 文档
  - 新增 **后台子代理（Background Subagents）**、**任务列表（Task List）**、**提示建议（Prompt Suggestions）** 文档
  - 新增 **沙盒（Sandboxing）** 和 **托管设置（Managed Settings）**（企业版）文档
  - 新增 **HTTP 钩子（HTTP Hooks）** 和 7 个新钩子事件文档
  - 新增 **插件设置（Plugin Settings）**、**LSP 服务器（LSP Servers）** 和市场更新文档
  - 新增 **从检查点摘要（Summarize from Checkpoint）** 回退选项文档
  - 记录 17 个新斜杠命令（`/fork`、`/desktop`、`/teleport`、`/tasks`、`/fast` 等）
  - 记录新 CLI 标志（`--worktree`、`--from-pr`、`--remote`、`--teleport`、`--teammate-mode` 等）
  - 记录用于自动记忆、工作量级别、代理团队等的新环境变量

### 设计

- 将 Logo 重新设计为指南针-方括号标记，采用极简调色板 (20779db)

### 错误修复 / 更正

- 更新模型名称：Sonnet 4.5 → **Sonnet 4.6**，Opus 4.5 → **Opus 4.6**
- 修正权限模式名称：将虚构的 "Unrestricted/Confirm/Read-only" 替换为实际的 `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions`
- 修正钩子事件：移除虚构的 `PreCommit`/`PostCommit`/`PrePush`，添加真实事件（`SubagentStart`、`WorktreeCreate`、`ConfigChange` 等）
- 修正 CLI 语法：将 `claude-code --headless` 替换为 `claude -p`（打印模式）
- 修正检查点命令：将虚构的 `/checkpoint save/list/rewind/diff` 替换为实际的 `Esc+Esc` / `/rewind` 界面
- 修正会话管理：将虚构的 `/session list/new/switch/save` 替换为真实的 `/resume`/`/rename`/`/fork`
- 修正插件清单格式：将 `plugin.yaml` 迁移为 `.claude-plugin/plugin.json`
- 修正 MCP 配置路径：`~/.claude/mcp.json` → `.mcp.json`（项目级）/ `~/.claude.json`（用户级）
- 修正文档 URL：`docs.claude.com` → `docs.anthropic.com`；移除虚构的 `plugins.claude.com`
- 移除多个文件中的虚构配置字段
- 将所有"最后更新"日期更新为 2026 年 2 月

**完整变更日志**：https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
