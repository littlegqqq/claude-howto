<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude 概念完全指南

一份涵盖 Slash Commands、Subagents、Memory、MCP 协议和 Agent Skills 的综合参考指南，附带表格、图表和实用示例。

---

## 目录

1. [Slash Commands](#slash-commands)
2. [Subagents](#subagents)
3. [Memory](#memory)
4. [MCP 协议](#mcp-协议)
5. [Agent Skills](#agent-skills)
6. [Plugins](#claude-code-plugins)
7. [Hooks](#hooks)
8. [Checkpoints 与 Rewind](#checkpoints-与-rewind)
9. [高级功能](#高级功能)
10. [比较与集成](#比较与集成)

---

## Slash Commands

### 概述

Slash commands 是以 Markdown 文件形式存储的用户调用快捷方式，可由 Claude Code 执行。它们使团队能够标准化常用提示和工作流。

### 架构

```mermaid
graph TD
    A["用户输入: /command-name"] -->|触发| B["搜索 .claude/commands/"]
    B -->|找到| C["command-name.md"]
    C -->|加载| D["Markdown 内容"]
    D -->|执行| E["Claude 处理提示"]
    E -->|返回| F["结果输出到上下文"]
```

### 文件结构

```mermaid
graph LR
    A["项目根目录"] -->|包含| B[".claude/commands/"]
    B -->|包含| C["optimize.md"]
    B -->|包含| D["test.md"]
    B -->|包含| E["docs/"]
    E -->|包含| F["generate-api-docs.md"]
    E -->|包含| G["generate-readme.md"]
```

### 命令组织表

| 位置 | 作用域 | 可用范围 | 使用场景 | Git 跟踪 |
|----------|-------|--------------|----------|-------------|
| `.claude/commands/` | 项目级 | 团队成员 | 团队工作流、共享标准 | ✅ 是 |
| `~/.claude/commands/` | 个人级 | 单个用户 | 跨项目的个人快捷方式 | ❌ 否 |
| 子目录 | 命名空间 | 取决于父目录 | 按类别组织 | ✅ 是 |

### 功能与能力

| 功能 | 示例 | 是否支持 |
|---------|---------|-----------|
| Shell 脚本执行 | `bash scripts/deploy.sh` | ✅ 是 |
| 文件引用 | `@path/to/file.js` | ✅ 是 |
| Bash 集成 | `$(git log --oneline)` | ✅ 是 |
| 参数传递 | `/pr --verbose` | ✅ 是 |
| MCP 命令 | `/mcp__github__list_prs` | ✅ 是 |

### 实用示例

#### 示例 1：代码优化命令

**文件：** `.claude/commands/optimize.md`

```markdown
---
name: Code Optimization
description: Analyze code for performance issues and suggest optimizations
tags: performance, analysis
---

# Code Optimization

Review the provided code for the following issues in order of priority:

1. **Performance bottlenecks** - identify O(n²) operations, inefficient loops
2. **Memory leaks** - find unreleased resources, circular references
3. **Algorithm improvements** - suggest better algorithms or data structures
4. **Caching opportunities** - identify repeated computations
5. **Concurrency issues** - find race conditions or threading problems

Format your response with:
- Issue severity (Critical/High/Medium/Low)
- Location in code
- Explanation
- Recommended fix with code example
```

**用法：**
```bash
# 用户在 Claude Code 中输入
/optimize

# Claude 加载提示并等待代码输入
```

#### 示例 2：Pull Request 辅助命令

**文件：** `.claude/commands/pr.md`

```markdown
---
name: Prepare Pull Request
description: Clean up code, stage changes, and prepare a pull request
tags: git, workflow
---

# Pull Request Preparation Checklist

Before creating a PR, execute these steps:

1. Run linting: `prettier --write .`
2. Run tests: `npm test`
3. Review git diff: `git diff HEAD`
4. Stage changes: `git add .`
5. Create commit message following conventional commits:
   - `fix:` for bug fixes
   - `feat:` for new features
   - `docs:` for documentation
   - `refactor:` for code restructuring
   - `test:` for test additions
   - `chore:` for maintenance

6. Generate PR summary including:
   - What changed
   - Why it changed
   - Testing performed
   - Potential impacts
```

**用法：**
```bash
/pr

# Claude 按照检查清单逐步执行并准备 PR
```

#### 示例 3：层级化文档生成器

**文件：** `.claude/commands/docs/generate-api-docs.md`

```markdown
---
name: Generate API Documentation
description: Create comprehensive API documentation from source code
tags: documentation, api
---

# API Documentation Generator

Generate API documentation by:

1. Scanning all files in `/src/api/`
2. Extracting function signatures and JSDoc comments
3. Organizing by endpoint/module
4. Creating markdown with examples
5. Including request/response schemas
6. Adding error documentation

Output format:
- Markdown file in `/docs/api.md`
- Include curl examples for all endpoints
- Add TypeScript types
```

### 命令生命周期图

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant FS as File System
    participant CLI as Shell/Bash

    User->>Claude: 输入 /optimize
    Claude->>FS: 搜索 .claude/commands/
    FS-->>Claude: 返回 optimize.md
    Claude->>Claude: 加载 Markdown 内容
    Claude->>User: 显示提示上下文
    User->>Claude: 提供待分析的代码
    Claude->>CLI: （可能执行脚本）
    CLI-->>Claude: 结果
    Claude->>User: 返回分析结果
```

### 最佳实践

| ✅ 应该做 | ❌ 不应该做 |
|------|---------|
| 使用清晰的、面向操作的名称 | 为一次性任务创建命令 |
| 在描述中记录触发词 | 在命令中构建复杂逻辑 |
| 保持命令专注于单一任务 | 创建冗余命令 |
| 对项目命令使用版本控制 | 硬编码敏感信息 |
| 在子目录中组织 | 创建冗长的命令列表 |
| 使用简单、可读的提示 | 使用缩写或难以理解的措辞 |

---

## Subagents

### 概述

Subagents 是具有独立上下文窗口和自定义系统提示的专业化 AI 助手。它们能够在保持清晰关注点分离的同时执行委托任务。

### 架构图

```mermaid
graph TB
    User["👤 用户"]
    Main["🎯 主 Agent<br/>（协调者）"]
    Reviewer["🔍 代码审查<br/>Subagent"]
    Tester["✅ 测试工程师<br/>Subagent"]
    Docs["📝 文档编写<br/>Subagent"]

    User -->|请求| Main
    Main -->|委托| Reviewer
    Main -->|委托| Tester
    Main -->|委托| Docs
    Reviewer -->|返回结果| Main
    Tester -->|返回结果| Main
    Docs -->|返回结果| Main
    Main -->|综合输出| User
```

### Subagent 生命周期

```mermaid
sequenceDiagram
    participant User
    participant MainAgent as 主 Agent
    participant CodeReviewer as 代码审查<br/>Subagent
    participant Context as 独立<br/>上下文窗口

    User->>MainAgent: "构建新的认证功能"
    MainAgent->>MainAgent: 分析任务
    MainAgent->>CodeReviewer: "审查此代码"
    CodeReviewer->>Context: 初始化干净的上下文
    Context->>CodeReviewer: 加载审查指令
    CodeReviewer->>CodeReviewer: 执行审查
    CodeReviewer-->>MainAgent: 返回发现
    MainAgent->>MainAgent: 整合结果
    MainAgent-->>User: 提供综合结果
```

### Subagent 配置表

| 配置项 | 类型 | 用途 | 示例 |
|---------------|------|---------|---------|
| `name` | String | Agent 标识符 | `code-reviewer` |
| `description` | String | 用途和触发词 | `Comprehensive code quality analysis` |
| `tools` | List/String | 允许的能力 | `read, grep, diff, lint_runner` |
| `system_prompt` | Markdown | 行为指令 | 自定义指南 |

### 工具访问层级

```mermaid
graph TD
    A["Subagent 配置"] -->|选项 1| B["继承所有工具<br/>来自主线程"]
    A -->|选项 2| C["指定单独工具"]
    B -->|包含| B1["文件操作"]
    B -->|包含| B2["Shell 命令"]
    B -->|包含| B3["MCP 工具"]
    C -->|显式列表| C1["read, grep, diff"]
    C -->|显式列表| C2["Bash(npm:*), Bash(test:*)"]
```

### 实用示例

#### 示例 1：完整的 Subagent 设置

**文件：** `.claude/agents/code-reviewer.md`

```yaml
---
name: code-reviewer
description: Comprehensive code quality and maintainability analysis
tools: read, grep, diff, lint_runner
---

# Code Reviewer Agent

You are an expert code reviewer specializing in:
- Performance optimization
- Security vulnerabilities
- Code maintainability
- Testing coverage
- Design patterns

## Review Priorities (in order)

1. **Security Issues** - Authentication, authorization, data exposure
2. **Performance Problems** - O(n²) operations, memory leaks, inefficient queries
3. **Code Quality** - Readability, naming, documentation
4. **Test Coverage** - Missing tests, edge cases
5. **Design Patterns** - SOLID principles, architecture

## Review Output Format

For each issue:
- **Severity**: Critical / High / Medium / Low
- **Category**: Security / Performance / Quality / Testing / Design
- **Location**: File path and line number
- **Issue Description**: What's wrong and why
- **Suggested Fix**: Code example
- **Impact**: How this affects the system

## Example Review

### Issue: N+1 Query Problem
- **Severity**: High
- **Category**: Performance
- **Location**: src/user-service.ts:45
- **Issue**: Loop executes database query in each iteration
- **Fix**: Use JOIN or batch query
```

**文件：** `.claude/agents/test-engineer.md`

```yaml
---
name: test-engineer
description: Test strategy, coverage analysis, and automated testing
tools: read, write, bash, grep
---

# Test Engineer Agent

You are expert at:
- Writing comprehensive test suites
- Ensuring high code coverage (>80%)
- Testing edge cases and error scenarios
- Performance benchmarking
- Integration testing

## Testing Strategy

1. **Unit Tests** - Individual functions/methods
2. **Integration Tests** - Component interactions
3. **End-to-End Tests** - Complete workflows
4. **Edge Cases** - Boundary conditions
5. **Error Scenarios** - Failure handling

## Test Output Requirements

- Use Jest for JavaScript/TypeScript
- Include setup/teardown for each test
- Mock external dependencies
- Document test purpose
- Include performance assertions when relevant

## Coverage Requirements

- Minimum 80% code coverage
- 100% for critical paths
- Report missing coverage areas
```

**文件：** `.claude/agents/documentation-writer.md`

```yaml
---
name: documentation-writer
description: Technical documentation, API docs, and user guides
tools: read, write, grep
---

# Documentation Writer Agent

You create:
- API documentation with examples
- User guides and tutorials
- Architecture documentation
- Changelog entries
- Code comment improvements

## Documentation Standards

1. **Clarity** - Use simple, clear language
2. **Examples** - Include practical code examples
3. **Completeness** - Cover all parameters and returns
4. **Structure** - Use consistent formatting
5. **Accuracy** - Verify against actual code

## Documentation Sections

### For APIs
- Description
- Parameters (with types)
- Returns (with types)
- Throws (possible errors)
- Examples (curl, JavaScript, Python)
- Related endpoints

### For Features
- Overview
- Prerequisites
- Step-by-step instructions
- Expected outcomes
- Troubleshooting
- Related topics
```

#### 示例 2：Subagent 委托实战

```markdown
# 场景：构建支付功能

## 用户请求
"构建一个安全的支付处理功能，集成 Stripe"

## 主 Agent 流程

1. **规划阶段**
   - 理解需求
   - 确定所需任务
   - 规划架构

2. **委托给代码审查 Subagent**
   - 任务："审查支付处理实现的安全性"
   - 上下文：Auth、API 密钥、令牌处理
   - 审查项：SQL 注入、密钥暴露、HTTPS 强制

3. **委托给测试工程师 Subagent**
   - 任务："为支付流程创建全面测试"
   - 上下文：成功场景、失败场景、边缘情况
   - 创建测试：有效支付、卡片被拒、网络故障、Webhook

4. **委托给文档编写 Subagent**
   - 任务："记录支付 API 端点"
   - 上下文：请求/响应 Schema
   - 产出：包含 curl 示例和错误码的 API 文档

5. **综合**
   - 主 Agent 收集所有输出
   - 整合发现
   - 向用户返回完整解决方案
```

#### 示例 3：工具权限范围控制

**限制性设置 - 仅限特定命令**

```yaml
---
name: secure-reviewer
description: Security-focused code review with minimal permissions
tools: read, grep
---

# Secure Code Reviewer

Reviews code for security vulnerabilities only.

This agent:
- ✅ Reads files to analyze
- ✅ Searches for patterns
- ❌ Cannot execute code
- ❌ Cannot modify files
- ❌ Cannot run tests

This ensures the reviewer doesn't accidentally break anything.
```

**扩展设置 - 用于实现的所有工具**

```yaml
---
name: implementation-agent
description: Full implementation capabilities for feature development
tools: read, write, bash, grep, edit, glob
---

# Implementation Agent

Builds features from specifications.

This agent:
- ✅ Reads specifications
- ✅ Writes new code files
- ✅ Runs build commands
- ✅ Searches codebase
- ✅ Edits existing files
- ✅ Finds files matching patterns

Full capabilities for independent feature development.
```

### Subagent 上下文管理

```mermaid
graph TB
    A["主 Agent 上下文<br/>50,000 tokens"]
    B["Subagent 1 上下文<br/>20,000 tokens"]
    C["Subagent 2 上下文<br/>20,000 tokens"]
    D["Subagent 3 上下文<br/>20,000 tokens"]

    A -->|全新状态| B
    A -->|全新状态| C
    A -->|全新状态| D

    B -->|仅返回结果| A
    C -->|仅返回结果| A
    D -->|仅返回结果| A

    style A fill:#e1f5ff
    style B fill:#fff9c4
    style C fill:#fff9c4
    style D fill:#fff9c4
```

### 何时使用 Subagents

| 场景 | 使用 Subagent | 原因 |
|----------|--------------|-----|
| 包含多步骤的复杂功能 | ✅ 是 | 分离关注点，防止上下文污染 |
| 快速代码审查 | ❌ 否 | 不必要的开销 |
| 并行任务执行 | ✅ 是 | 每个 Subagent 有自己的上下文 |
| 需要专业知识 | ✅ 是 | 自定义系统提示 |
| 长时间运行的分析 | ✅ 是 | 防止主上下文耗尽 |
| 单一任务 | ❌ 否 | 不必要地增加延迟 |

### Agent Teams

Agent Teams 协调多个 Agent 协作完成相关任务。与一次委托给一个 Subagent 不同，Agent Teams 允许主 Agent 编排一组 Agent 进行协作、共享中间结果，并朝着共同目标努力。这对于全栈功能开发等大规模任务非常有用，例如前端 Agent、后端 Agent 和测试 Agent 并行工作。

---

## Memory

### 概述

Memory 使 Claude 能够跨会话和对话保留上下文。它以两种形式存在：claude.ai 中的自动综合，以及 Claude Code 中基于文件系统的 CLAUDE.md。

### Memory 架构

```mermaid
graph TB
    A["Claude 会话"]
    B["用户输入"]
    C["Memory 系统"]
    D["Memory 存储"]

    B -->|用户提供信息| C
    C -->|每 24 小时综合| D
    D -->|自动加载| A
    A -->|使用上下文| C
```

### Claude Code 中的 Memory 层级（7 层）

Claude Code 从 7 个层级加载 Memory，按优先级从高到低排列：

```mermaid
graph TD
    A["1. 托管策略<br/>企业管理员策略"] --> B["2. 项目 Memory<br/>./CLAUDE.md"]
    B --> C["3. 项目规则<br/>.claude/rules/*.md"]
    C --> D["4. 用户 Memory<br/>~/.claude/CLAUDE.md"]
    D --> E["5. 用户规则<br/>~/.claude/rules/*.md"]
    E --> F["6. 本地 Memory<br/>.claude/local/CLAUDE.md"]
    F --> G["7. 自动 Memory<br/>自动捕获的偏好"]

    style A fill:#fce4ec,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#e1f5fe,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
    style G fill:#fff3e0,stroke:#333,color:#333
```

### Memory 位置表

| 层级 | 位置 | 作用域 | 优先级 | 是否共享 | 最适用于 |
|------|----------|-------|----------|--------|----------|
| 1. 托管策略 | 企业管理员 | 组织 | 最高 | 所有组织用户 | 合规、安全策略 |
| 2. 项目 | `./CLAUDE.md` | 项目 | 高 | 团队（Git） | 团队标准、架构 |
| 3. 项目规则 | `.claude/rules/*.md` | 项目 | 高 | 团队（Git） | 模块化项目规范 |
| 4. 用户 | `~/.claude/CLAUDE.md` | 个人 | 中 | 个人 | 个人偏好 |
| 5. 用户规则 | `~/.claude/rules/*.md` | 个人 | 中 | 个人 | 个人规则模块 |
| 6. 本地 | `.claude/local/CLAUDE.md` | 本地 | 低 | 不共享 | 机器特定设置 |
| 7. 自动 Memory | 自动 | 会话 | 最低 | 个人 | 学习到的偏好、模式 |

### 自动 Memory

自动 Memory 会自动捕获在会话中观察到的用户偏好和模式。Claude 从你的交互中学习并记住：

- 编码风格偏好
- 你经常做的修正
- 框架和工具选择
- 沟通风格偏好

自动 Memory 在后台工作，不需要手动配置。

### Memory 更新生命周期

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Editor as 文件系统
    participant Memory as CLAUDE.md

    User->>Claude: "记住：使用 async/await"
    Claude->>User: "哪个 Memory 文件？"
    User->>Claude: "项目 Memory"
    Claude->>Editor: 打开 ~/.claude/settings.json
    Claude->>Memory: 写入 ./CLAUDE.md
    Memory-->>Claude: 文件已保存
    Claude->>Claude: 加载更新后的 Memory
    Claude-->>User: "Memory 已保存！"
```

### 实用示例

#### 示例 1：项目 Memory 结构

**文件：** `./CLAUDE.md`

```markdown
# Project Configuration

## Project Overview
- **Name**: E-commerce Platform
- **Tech Stack**: Node.js, PostgreSQL, React 18, Docker
- **Team Size**: 5 developers
- **Deadline**: Q4 2025

## Architecture
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## Development Standards

### Code Style
- Use Prettier for formatting
- Use ESLint with airbnb config
- Maximum line length: 100 characters
- Use 2-space indentation

### Naming Conventions
- **Files**: kebab-case (user-controller.js)
- **Classes**: PascalCase (UserService)
- **Functions/Variables**: camelCase (getUserById)
- **Constants**: UPPER_SNAKE_CASE (API_BASE_URL)
- **Database Tables**: snake_case (user_accounts)

### Git Workflow
- Branch names: `feature/description` or `fix/description`
- Commit messages: Follow conventional commits
- PR required before merge
- All CI/CD checks must pass
- Minimum 1 approval required

### Testing Requirements
- Minimum 80% code coverage
- All critical paths must have tests
- Use Jest for unit tests
- Use Cypress for E2E tests
- Test filenames: `*.test.ts` or `*.spec.ts`

### API Standards
- RESTful endpoints only
- JSON request/response
- Use HTTP status codes correctly
- Version API endpoints: `/api/v1/`
- Document all endpoints with examples

### Database
- Use migrations for schema changes
- Never hardcode credentials
- Use connection pooling
- Enable query logging in development
- Regular backups required

### Deployment
- Docker-based deployment
- Kubernetes orchestration
- Blue-green deployment strategy
- Automatic rollback on failure
- Database migrations run before deploy

## Common Commands

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start development server |
| `npm test` | Run test suite |
| `npm run lint` | Check code style |
| `npm run build` | Build for production |
| `npm run migrate` | Run database migrations |

## Team Contacts
- Tech Lead: Sarah Chen (@sarah.chen)
- Product Manager: Mike Johnson (@mike.j)
- DevOps: Alex Kim (@alex.k)

## Known Issues & Workarounds
- PostgreSQL connection pooling limited to 20 during peak hours
- Workaround: Implement query queuing
- Safari 14 compatibility issues with async generators
- Workaround: Use Babel transpiler

## Related Projects
- Analytics Dashboard: `/projects/analytics`
- Mobile App: `/projects/mobile`
- Admin Panel: `/projects/admin`
```

#### 示例 2：目录级 Memory

**文件：** `./src/api/CLAUDE.md`

~~~~markdown
# API Module Standards

This file overrides root CLAUDE.md for everything in /src/api/

## API-Specific Standards

### Request Validation
- Use Zod for schema validation
- Always validate input
- Return 400 with validation errors
- Include field-level error details

### Authentication
- All endpoints require JWT token
- Token in Authorization header
- Token expires after 24 hours
- Implement refresh token mechanism

### Response Format

All responses must follow this structure:

```json
{
  "success": true,
  "data": { /* actual data */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```

### Error responses:
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "User message",
    "details": { /* field errors */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### Pagination
- Use cursor-based pagination (not offset)
- Include `hasMore` boolean
- Limit max page size to 100
- Default page size: 20

### Rate Limiting
- 1000 requests per hour for authenticated users
- 100 requests per hour for public endpoints
- Return 429 when exceeded
- Include retry-after header

### Caching
- Use Redis for session caching
- Cache duration: 5 minutes default
- Invalidate on write operations
- Tag cache keys with resource type
~~~~

#### 示例 3：个人 Memory

**文件：** `~/.claude/CLAUDE.md`

~~~~markdown
# My Development Preferences

## About Me
- **Experience Level**: 8 years full-stack development
- **Preferred Languages**: TypeScript, Python
- **Communication Style**: Direct, with examples
- **Learning Style**: Visual diagrams with code

## Code Preferences

### Error Handling
I prefer explicit error handling with try-catch blocks and meaningful error messages.
Avoid generic errors. Always log errors for debugging.

### Comments
Use comments for WHY, not WHAT. Code should be self-documenting.
Comments should explain business logic or non-obvious decisions.

### Testing
I prefer TDD (test-driven development).
Write tests first, then implementation.
Focus on behavior, not implementation details.

### Architecture
I prefer modular, loosely-coupled design.
Use dependency injection for testability.
Separate concerns (Controllers, Services, Repositories).

## Debugging Preferences
- Use console.log with prefix: `[DEBUG]`
- Include context: function name, relevant variables
- Use stack traces when available
- Always include timestamps in logs

## Communication
- Explain complex concepts with diagrams
- Show concrete examples before explaining theory
- Include before/after code snippets
- Summarize key points at the end

## Project Organization
I organize my projects as:
```
project/
  ├── src/
  │   ├── api/
  │   ├── services/
  │   ├── models/
  │   └── utils/
  ├── tests/
  ├── docs/
  └── docker/
```

## Tooling
- **IDE**: VS Code with vim keybindings
- **Terminal**: Zsh with Oh-My-Zsh
- **Format**: Prettier (100 char line length)
- **Linter**: ESLint with airbnb config
- **Test Framework**: Jest with React Testing Library
~~~~

#### 示例 4：会话中的 Memory 更新

**会话交互：**

```markdown
User: Remember that I prefer using React hooks instead of class components
     for all new components.

Claude: I'm adding that to your memory. Which memory file should this go in?
        1. Project memory (./CLAUDE.md)
        2. Personal memory (~/.claude/CLAUDE.md)

User: Project memory

Claude: ✅ Memory saved!

Added to ./CLAUDE.md:
---

### Component Development
- Use functional components with React Hooks
- Prefer hooks over class components
- Custom hooks for reusable logic
- Use useCallback for event handlers
- Use useMemo for expensive computations
```

### Claude Web/Desktop 中的 Memory

#### Memory 综合时间线

```mermaid
graph LR
    A["第 1 天：用户<br/>对话"] -->|24 小时| B["第 2 天：Memory<br/>综合"]
    B -->|自动| C["Memory 更新<br/>已总结"]
    C -->|加载到| D["第 2-N 天：<br/>新对话"]
    D -->|添加到| E["Memory"]
    E -->|24 小时后| F["Memory 刷新"]
```

**Memory 综合示例：**

```markdown
## Claude's Memory of User

### Professional Background
- Senior full-stack developer with 8 years experience
- Focus on TypeScript/Node.js backends and React frontends
- Active open source contributor
- Interested in AI and machine learning

### Project Context
- Currently building e-commerce platform
- Tech stack: Node.js, PostgreSQL, React 18, Docker
- Working with team of 5 developers
- Using CI/CD and blue-green deployments

### Communication Preferences
- Prefers direct, concise explanations
- Likes visual diagrams and examples
- Appreciates code snippets
- Explains business logic in comments

### Current Goals
- Improve API performance
- Increase test coverage to 90%
- Implement caching strategy
- Document architecture
```

### Memory 功能对比

| 功能 | Claude Web/Desktop | Claude Code (CLAUDE.md) |
|---------|-------------------|------------------------|
| 自动综合 | ✅ 每 24 小时 | ❌ 手动 |
| 跨项目 | ✅ 共享 | ❌ 项目特定 |
| 团队访问 | ✅ 共享项目 | ✅ Git 跟踪 |
| 可搜索 | ✅ 内置 | ✅ 通过 `/memory` |
| 可编辑 | ✅ 对话中编辑 | ✅ 直接编辑文件 |
| 导入/导出 | ✅ 是 | ✅ 复制/粘贴 |
| 持久性 | ✅ 24 小时以上 | ✅ 无限期 |

---

## MCP 协议

### 概述

MCP（Model Context Protocol）是一种标准化方式，使 Claude 能够访问外部工具、API 和实时数据源。与 Memory 不同，MCP 提供对变化数据的实时访问。

### MCP 架构

```mermaid
graph TB
    A["Claude"]
    B["MCP Server"]
    C["外部服务"]

    A -->|请求: list_issues| B
    B -->|查询| C
    C -->|数据| B
    B -->|响应| A

    A -->|请求: create_issue| B
    B -->|操作| C
    C -->|结果| B
    B -->|响应| A
```

### MCP 生态系统

```mermaid
graph TB
    A["Claude"] -->|MCP| B["Filesystem<br/>MCP Server"]
    A -->|MCP| C["GitHub<br/>MCP Server"]
    A -->|MCP| D["Database<br/>MCP Server"]
    A -->|MCP| E["Slack<br/>MCP Server"]
    A -->|MCP| F["Google Docs<br/>MCP Server"]

    B -->|文件 I/O| G["本地文件"]
    C -->|API| H["GitHub 仓库"]
    D -->|查询| I["PostgreSQL/MySQL"]
    E -->|消息| J["Slack 工作区"]
    F -->|文档| K["Google Drive"]
```

### MCP 设置流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as 配置文件
    participant Service as 外部服务

    User->>Claude: 输入 /mcp
    Claude->>Claude: 列出可用的 MCP 服务器
    Claude->>User: 显示选项
    User->>Claude: 选择 GitHub MCP
    Claude->>Config: 更新配置
    Config->>Claude: 激活连接
    Claude->>Service: 测试连接
    Service-->>Claude: 认证成功
    Claude->>User: ✅ MCP 已连接！
```

### 可用 MCP 服务器表

| MCP 服务器 | 用途 | 常用工具 | 认证方式 | 实时 |
|------------|---------|--------------|------|-----------|
| **Filesystem** | 文件操作 | read, write, delete | 操作系统权限 | ✅ 是 |
| **GitHub** | 仓库管理 | list_prs, create_issue, push | OAuth | ✅ 是 |
| **Slack** | 团队沟通 | send_message, list_channels | Token | ✅ 是 |
| **Database** | SQL 查询 | query, insert, update | 凭证 | ✅ 是 |
| **Google Docs** | 文档访问 | read, write, share | OAuth | ✅ 是 |
| **Asana** | 项目管理 | create_task, update_status | API Key | ✅ 是 |
| **Stripe** | 支付数据 | list_charges, create_invoice | API Key | ✅ 是 |
| **Memory** | 持久化存储 | store, retrieve, delete | 本地 | ❌ 否 |

### 实用示例

#### 示例 1：GitHub MCP 配置

**文件：** `.mcp.json`（项目范围）或 `~/.claude.json`（用户范围）

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**可用的 GitHub MCP 工具：**

~~~~markdown
# GitHub MCP Tools

## Pull Request Management
- `list_prs` - List all PRs in repository
- `get_pr` - Get PR details including diff
- `create_pr` - Create new PR
- `update_pr` - Update PR description/title
- `merge_pr` - Merge PR to main branch
- `review_pr` - Add review comments

Example request:
```
/mcp__github__get_pr 456

# Returns:
Title: Add dark mode support
Author: @alice
Description: Implements dark theme using CSS variables
Status: OPEN
Reviewers: @bob, @charlie
```

## Issue Management
- `list_issues` - List all issues
- `get_issue` - Get issue details
- `create_issue` - Create new issue
- `close_issue` - Close issue
- `add_comment` - Add comment to issue

## Repository Information
- `get_repo_info` - Repository details
- `list_files` - File tree structure
- `get_file_content` - Read file contents
- `search_code` - Search across codebase

## Commit Operations
- `list_commits` - Commit history
- `get_commit` - Specific commit details
- `create_commit` - Create new commit
~~~~

#### 示例 2：Database MCP 设置

**配置：**

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    }
  }
}
```

**使用示例：**

```markdown
User: Fetch all users with more than 10 orders

Claude: I'll query your database to find that information.

# Using MCP database tool:
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# Results:
- Alice: 15 orders
- Bob: 12 orders
- Charlie: 11 orders
```

#### 示例 3：多 MCP 工作流

**场景：每日报告生成**

```markdown
# Daily Report Workflow using Multiple MCPs

## Setup
1. GitHub MCP - fetch PR metrics
2. Database MCP - query sales data
3. Slack MCP - post report
4. Filesystem MCP - save report

## Workflow

### Step 1: Fetch GitHub Data
/mcp__github__list_prs completed:true last:7days

Output:
- Total PRs: 42
- Average merge time: 2.3 hours
- Review turnaround: 1.1 hours

### Step 2: Query Database
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

Output:
- Sales: 247
- Revenue: $12,450

### Step 3: Generate Report
Combine data into HTML report

### Step 4: Save to Filesystem
Write report.html to /reports/

### Step 5: Post to Slack
Send summary to #daily-reports channel

Final Output:
✅ Report generated and posted
📊 47 PRs merged this week
💰 $12,450 in daily sales
```

#### 示例 4：Filesystem MCP 操作

**配置：**

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    }
  }
}
```

**可用操作：**

| 操作 | 命令 | 用途 |
|-----------|---------|---------|
| 列出文件 | `ls ~/projects` | 显示目录内容 |
| 读取文件 | `cat src/main.ts` | 读取文件内容 |
| 写入文件 | `create docs/api.md` | 创建新文件 |
| 编辑文件 | `edit src/app.ts` | 修改文件 |
| 搜索 | `grep "async function"` | 在文件中搜索 |
| 删除 | `rm old-file.js` | 删除文件 |

### MCP vs Memory：决策矩阵

```mermaid
graph TD
    A["需要外部数据？"]
    A -->|否| B["使用 Memory"]
    A -->|是| C["数据是否频繁变化？"]
    C -->|否/很少| B
    C -->|是/经常| D["使用 MCP"]

    B -->|存储| E["偏好<br/>上下文<br/>历史"]
    D -->|访问| F["实时 API<br/>数据库<br/>服务"]

    style B fill:#e1f5ff
    style D fill:#fff9c4
```

### 请求/响应模式

```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCP Server
    participant DB as Database

    App->>MCP: 请求: "SELECT * FROM users WHERE id=1"
    MCP->>DB: 执行查询
    DB-->>MCP: 结果集
    MCP-->>App: 返回解析后的数据
    App->>App: 处理结果
    App->>App: 继续任务

    Note over MCP,DB: 实时访问<br/>无缓存
```

---

## Agent Skills

### 概述

Agent Skills 是打包为文件夹的可重用、模型调用的能力，包含指令、脚本和资源。Claude 自动检测并使用相关的 Skills。

### Skill 架构

```mermaid
graph TB
    A["Skill 目录"]
    B["SKILL.md"]
    C["YAML 元数据"]
    D["指令"]
    E["脚本"]
    F["模板"]

    A --> B
    B --> C
    B --> D
    E --> A
    F --> A
```

### Skill 加载流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude
    participant System as 系统
    participant Skill as Skill

    User->>Claude: "创建 Excel 报告"
    Claude->>System: 扫描可用 Skills
    System->>System: 加载 Skill 元数据
    Claude->>Claude: 将用户请求匹配到 Skills
    Claude->>Skill: 加载 xlsx skill SKILL.md
    Skill-->>Claude: 返回指令 + 工具
    Claude->>Claude: 执行 Skill
    Claude->>User: 生成 Excel 文件
```

### Skill 类型与位置表

| 类型 | 位置 | 作用域 | 是否共享 | 同步方式 | 最适用于 |
|------|----------|-------|--------|------|----------|
| 预构建 | 内置 | 全局 | 所有用户 | 自动 | 文档创建 |
| 个人 | `~/.claude/skills/` | 个人 | 否 | 手动 | 个人自动化 |
| 项目 | `.claude/skills/` | 团队 | 是 | Git | 团队标准 |
| 插件 | 通过插件安装 | 不定 | 视情况 | 自动 | 集成功能 |

### 预构建 Skills

```mermaid
graph TB
    A["预构建 Skills"]
    B["PowerPoint (pptx)"]
    C["Excel (xlsx)"]
    D["Word (docx)"]
    E["PDF"]

    A --> B
    A --> C
    A --> D
    A --> E

    B --> B1["创建演示文稿"]
    B --> B2["编辑幻灯片"]
    C --> C1["创建电子表格"]
    C --> C2["分析数据"]
    D --> D1["创建文档"]
    D --> D2["格式化文本"]
    E --> E1["生成 PDF"]
    E --> E2["填写表单"]
```

### 内置 Skills

Claude Code 现在包含 5 个开箱即用的内置 Skills：

| Skill | 命令 | 用途 |
|-------|---------|---------|
| **Simplify** | `/simplify` | 简化复杂代码或解释 |
| **Batch** | `/batch` | 跨多个文件或项目运行操作 |
| **Debug** | `/debug` | 系统化调试问题并找出根本原因 |
| **Loop** | `/loop` | 按定时器调度重复任务 |
| **Claude API** | `/claude-api` | 直接与 Anthropic API 交互 |

这些内置 Skills 始终可用，无需安装或配置。

### 实用示例

#### 示例 1：自定义代码审查 Skill

**目录结构：**

```
~/.claude/skills/code-review/
├── SKILL.md
├── templates/
│   ├── review-checklist.md
│   └── finding-template.md
└── scripts/
    ├── analyze-metrics.py
    └── compare-complexity.py
```

**文件：** `~/.claude/skills/code-review/SKILL.md`

```yaml
---
name: Code Review Specialist
description: Comprehensive code review with security, performance, and quality analysis
version: "1.0.0"
tags:
  - code-review
  - quality
  - security
when_to_use: When users ask to review code, analyze code quality, or evaluate pull requests
effort: high
shell: bash
---

# Code Review Skill

This skill provides comprehensive code review capabilities focusing on:

1. **Security Analysis**
   - Authentication/authorization issues
   - Data exposure risks
   - Injection vulnerabilities
   - Cryptographic weaknesses
   - Sensitive data logging

2. **Performance Review**
   - Algorithm efficiency (Big O analysis)
   - Memory optimization
   - Database query optimization
   - Caching opportunities
   - Concurrency issues

3. **Code Quality**
   - SOLID principles
   - Design patterns
   - Naming conventions
   - Documentation
   - Test coverage

4. **Maintainability**
   - Code readability
   - Function size (should be < 50 lines)
   - Cyclomatic complexity
   - Dependency management
   - Type safety

## Review Template

For each piece of code reviewed, provide:

### Summary
- Overall quality assessment (1-5)
- Key findings count
- Recommended priority areas

### Critical Issues (if any)
- **Issue**: Clear description
- **Location**: File and line number
- **Impact**: Why this matters
- **Severity**: Critical/High/Medium
- **Fix**: Code example

### Findings by Category

#### Security (if issues found)
List security vulnerabilities with examples

#### Performance (if issues found)
List performance problems with complexity analysis

#### Quality (if issues found)
List code quality issues with refactoring suggestions

#### Maintainability (if issues found)
List maintainability problems with improvements
```
## Python 脚本：analyze-metrics.py

```python
#!/usr/bin/env python3
import re
import sys

def analyze_code_metrics(code):
    """Analyze code for common metrics."""

    # Count functions
    functions = len(re.findall(r'^def\s+\w+', code, re.MULTILINE))

    # Count classes
    classes = len(re.findall(r'^class\s+\w+', code, re.MULTILINE))

    # Average line length
    lines = code.split('\n')
    avg_length = sum(len(l) for l in lines) / len(lines) if lines else 0

    # Estimate complexity
    complexity = len(re.findall(r'\b(if|elif|else|for|while|and|or)\b', code))

    return {
        'functions': functions,
        'classes': classes,
        'avg_line_length': avg_length,
        'complexity_score': complexity
    }

if __name__ == '__main__':
    with open(sys.argv[1], 'r') as f:
        code = f.read()
    metrics = analyze_code_metrics(code)
    for key, value in metrics.items():
        print(f"{key}: {value:.2f}")
```

## Python 脚本：compare-complexity.py

```python
#!/usr/bin/env python3
"""
Compare cyclomatic complexity of code before and after changes.
Helps identify if refactoring actually simplifies code structure.
"""

import re
import sys
from typing import Dict, Tuple

class ComplexityAnalyzer:
    """Analyze code complexity metrics."""

    def __init__(self, code: str):
        self.code = code
        self.lines = code.split('\n')

    def calculate_cyclomatic_complexity(self) -> int:
        """
        Calculate cyclomatic complexity using McCabe's method.
        Count decision points: if, elif, else, for, while, except, and, or
        """
        complexity = 1  # Base complexity

        # Count decision points
        decision_patterns = [
            r'\bif\b',
            r'\belif\b',
            r'\bfor\b',
            r'\bwhile\b',
            r'\bexcept\b',
            r'\band\b(?!$)',
            r'\bor\b(?!$)'
        ]

        for pattern in decision_patterns:
            matches = re.findall(pattern, self.code)
            complexity += len(matches)

        return complexity

    def calculate_cognitive_complexity(self) -> int:
        """
        Calculate cognitive complexity - how hard is it to understand?
        Based on nesting depth and control flow.
        """
        cognitive = 0
        nesting_depth = 0

        for line in self.lines:
            # Track nesting depth
            if re.search(r'^\s*(if|for|while|def|class|try)\b', line):
                nesting_depth += 1
                cognitive += nesting_depth
            elif re.search(r'^\s*(elif|else|except|finally)\b', line):
                cognitive += nesting_depth

            # Reduce nesting when unindenting
            if line and not line[0].isspace():
                nesting_depth = 0

        return cognitive

    def calculate_maintainability_index(self) -> float:
        """
        Maintainability Index ranges from 0-100.
        > 85: Excellent
        > 65: Good
        > 50: Fair
        < 50: Poor
        """
        lines = len(self.lines)
        cyclomatic = self.calculate_cyclomatic_complexity()
        cognitive = self.calculate_cognitive_complexity()

        # Simplified MI calculation
        mi = 171 - 5.2 * (cyclomatic / lines) - 0.23 * (cognitive) - 16.2 * (lines / 1000)

        return max(0, min(100, mi))

    def get_complexity_report(self) -> Dict:
        """Generate comprehensive complexity report."""
        return {
            'cyclomatic_complexity': self.calculate_cyclomatic_complexity(),
            'cognitive_complexity': self.calculate_cognitive_complexity(),
            'maintainability_index': round(self.calculate_maintainability_index(), 2),
            'lines_of_code': len(self.lines),
            'avg_line_length': round(sum(len(l) for l in self.lines) / len(self.lines), 2) if self.lines else 0
        }


def compare_files(before_file: str, after_file: str) -> None:
    """Compare complexity metrics between two code versions."""

    with open(before_file, 'r') as f:
        before_code = f.read()

    with open(after_file, 'r') as f:
        after_code = f.read()

    before_analyzer = ComplexityAnalyzer(before_code)
    after_analyzer = ComplexityAnalyzer(after_code)

    before_metrics = before_analyzer.get_complexity_report()
    after_metrics = after_analyzer.get_complexity_report()

    print("=" * 60)
    print("CODE COMPLEXITY COMPARISON")
    print("=" * 60)

    print("\nBEFORE:")
    print(f"  Cyclomatic Complexity:    {before_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {before_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {before_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {before_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {before_metrics['avg_line_length']}")

    print("\nAFTER:")
    print(f"  Cyclomatic Complexity:    {after_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {after_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {after_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {after_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {after_metrics['avg_line_length']}")

    print("\nCHANGES:")
    cyclomatic_change = after_metrics['cyclomatic_complexity'] - before_metrics['cyclomatic_complexity']
    cognitive_change = after_metrics['cognitive_complexity'] - before_metrics['cognitive_complexity']
    mi_change = after_metrics['maintainability_index'] - before_metrics['maintainability_index']
    loc_change = after_metrics['lines_of_code'] - before_metrics['lines_of_code']

    print(f"  Cyclomatic Complexity:    {cyclomatic_change:+d}")
    print(f"  Cognitive Complexity:     {cognitive_change:+d}")
    print(f"  Maintainability Index:    {mi_change:+.2f}")
    print(f"  Lines of Code:            {loc_change:+d}")

    print("\nASSESSMENT:")
    if mi_change > 0:
        print("  ✅ Code is MORE maintainable")
    elif mi_change < 0:
        print("  ⚠️  Code is LESS maintainable")
    else:
        print("  ➡️  Maintainability unchanged")

    if cyclomatic_change < 0:
        print("  ✅ Complexity DECREASED")
    elif cyclomatic_change > 0:
        print("  ⚠️  Complexity INCREASED")
    else:
        print("  ➡️  Complexity unchanged")

    print("=" * 60)


if __name__ == '__main__':
    if len(sys.argv) != 3:
        print("Usage: python compare-complexity.py <before_file> <after_file>")
        sys.exit(1)

    compare_files(sys.argv[1], sys.argv[2])
```

## 模板：review-checklist.md

```markdown
# Code Review Checklist

## Security Checklist
- [ ] No hardcoded credentials or secrets
- [ ] Input validation on all user inputs
- [ ] SQL injection prevention (parameterized queries)
- [ ] CSRF protection on state-changing operations
- [ ] XSS prevention with proper escaping
- [ ] Authentication checks on protected endpoints
- [ ] Authorization checks on resources
- [ ] Secure password hashing (bcrypt, argon2)
- [ ] No sensitive data in logs
- [ ] HTTPS enforced

## Performance Checklist
- [ ] No N+1 queries
- [ ] Appropriate use of indexes
- [ ] Caching implemented where beneficial
- [ ] No blocking operations on main thread
- [ ] Async/await used correctly
- [ ] Large datasets paginated
- [ ] Database connections pooled
- [ ] Regular expressions optimized
- [ ] No unnecessary object creation
- [ ] Memory leaks prevented

## Quality Checklist
- [ ] Functions < 50 lines
- [ ] Clear variable naming
- [ ] No duplicate code
- [ ] Proper error handling
- [ ] Comments explain WHY, not WHAT
- [ ] No console.logs in production
- [ ] Type checking (TypeScript/JSDoc)
- [ ] SOLID principles followed
- [ ] Design patterns applied correctly
- [ ] Self-documenting code

## Testing Checklist
- [ ] Unit tests written
- [ ] Edge cases covered
- [ ] Error scenarios tested
- [ ] Integration tests present
- [ ] Coverage > 80%
- [ ] No flaky tests
- [ ] Mock external dependencies
- [ ] Clear test names
```

## 模板：finding-template.md

~~~~markdown
# Code Review Finding Template

Use this template when documenting each issue found during code review.

---

## Issue: [TITLE]

### Severity
- [ ] Critical (blocks deployment)
- [ ] High (should fix before merge)
- [ ] Medium (should fix soon)
- [ ] Low (nice to have)

### Category
- [ ] Security
- [ ] Performance
- [ ] Code Quality
- [ ] Maintainability
- [ ] Testing
- [ ] Design Pattern
- [ ] Documentation

### Location
**File:** `src/components/UserCard.tsx`
**Lines:** 45-52

**Function/Method:** `renderUserDetails()`

### Issue Description

**What:** Describe what the issue is.

**Why it matters:** Explain the impact and why this needs to be fixed.

**Current behavior:** Show the problematic code or behavior.

**Expected behavior:** Describe what should happen instead.

### Code Example

#### Current (Problematic)

```typescript
// Shows the N+1 query problem
const users = fetchUsers();
users.forEach(user => {
  const posts = fetchUserPosts(user.id); // Query per user!
  renderUserPosts(posts);
});
```

#### Suggested Fix

```typescript
// Optimized with JOIN query
const usersWithPosts = fetchUsersWithPosts();
usersWithPosts.forEach(({ user, posts }) => {
  renderUserPosts(posts);
});
```

### Impact Analysis

| Aspect | Impact | Severity |
|--------|--------|----------|
| Performance | 100+ queries for 20 users | High |
| User Experience | Slow page load | High |
| Scalability | Breaks at scale | Critical |
| Maintainability | Hard to debug | Medium |

### Related Issues

- Similar issue in `AdminUserList.tsx` line 120
- Related PR: #456
- Related issue: #789

### Additional Resources

- [N+1 Query Problem](https://en.wikipedia.org/wiki/N%2B1_problem)
- [Database Join Documentation](https://docs.example.com/joins)

### Reviewer Notes

- This is a common pattern in this codebase
- Consider adding this to the code style guide
- Might be worth creating a helper function

### Author Response (for feedback)

*To be filled by the code author:*

- [ ] Fix implemented in commit: `abc123`
- [ ] Fix status: Complete / In Progress / Needs Discussion
- [ ] Questions or concerns: (describe)

---

## Finding Statistics (for Reviewer)

When reviewing multiple findings, track:

- **Total Issues Found:** X
- **Critical:** X
- **High:** X
- **Medium:** X
- **Low:** X

**Recommendation:** ✅ Approve / ⚠️ Request Changes / 🔄 Needs Discussion

**Overall Code Quality:** 1-5 stars
~~~~

## 使用示例

~~~~markdown
User: Review this React component code

Claude: I'll conduct a comprehensive code review using my Code Review Skill.

[Loads SKILL.md]
[Analyzes code against checklist]
[Runs Python metrics script]
[Applies templates]

## Code Review Results

### Summary
Quality Score: 3.5/5
Key Findings: 7 items
Priority Areas: Performance optimization, error handling

### Critical Issues: 1

**Issue**: N+1 Query Problem
**Location**: UserComponent.tsx, lines 45-52
**Impact**: Renders 100+ database queries for list of 20 users
**Severity**: High
**Fix**:
```javascript
// Before: N+1 queries
const users = fetchUsers();
users.forEach(user => fetchUserPosts(user.id)); // 20+ queries

// After: Single query with JOIN
const users = fetchUsersWithPosts(); // 1 query
```

### Performance Findings
- Missing pagination on large lists
- Recommendation: Use React.memo() for items
- Database query: Could be optimized with index

### Quality Findings
- Function in line 20 is 127 lines (max: 50)
- Missing error boundary
- Props should have TypeScript types
~~~~

#### 示例 2：品牌语调 Skill

**目录结构：**

```
.claude/skills/brand-voice/
├── SKILL.md
├── brand-guidelines.md
├── tone-examples.md
└── templates/
    ├── email-template.txt
    ├── social-post-template.txt
    └── blog-post-template.md
```

**文件：** `.claude/skills/brand-voice/SKILL.md`

```yaml
---
name: Brand Voice Consistency
description: Ensure all communication matches brand voice and tone guidelines
tags:
  - brand
  - writing
  - consistency
when_to_use: When creating marketing copy, customer communications, or public-facing content
---

# Brand Voice Skill

## Overview
This skill ensures all communications maintain consistent brand voice, tone, and messaging.

## Brand Identity

### Mission
Help teams automate their development workflows with AI

### Values
- **Simplicity**: Make complex things simple
- **Reliability**: Rock-solid execution
- **Empowerment**: Enable human creativity

### Tone of Voice
- **Friendly but professional** - approachable without being casual
- **Clear and concise** - avoid jargon, explain technical concepts simply
- **Confident** - we know what we're doing
- **Empathetic** - understand user needs and pain points

## Writing Guidelines

### Do's ✅
- Use "you" when addressing readers
- Use active voice: "Claude generates reports" not "Reports are generated by Claude"
- Start with value proposition
- Use concrete examples
- Keep sentences under 20 words
- Use lists for clarity
- Include calls-to-action

### Don'ts ❌
- Don't use corporate jargon
- Don't patronize or oversimplify
- Don't use "we believe" or "we think"
- Don't use ALL CAPS except for emphasis
- Don't create walls of text
- Don't assume technical knowledge

## Vocabulary

### ✅ Preferred Terms
- Claude (not "the Claude AI")
- Code generation (not "auto-coding")
- Agent (not "bot")
- Streamline (not "revolutionize")
- Integrate (not "synergize")

### ❌ Avoid Terms
- "Cutting-edge" (overused)
- "Game-changer" (vague)
- "Leverage" (corporate-speak)
- "Utilize" (use "use")
- "Paradigm shift" (unclear)
```
## 示例

### ✅ 好的示例
"Claude automates your code review process. Instead of manually checking each PR, Claude reviews security, performance, and quality—saving your team hours every week."

Why it works: Clear value, specific benefits, action-oriented

### ❌ 不好的示例
"Claude leverages cutting-edge AI to provide comprehensive software development solutions."

Why it doesn't work: Vague, corporate jargon, no specific value

## 模板：Email

```
Subject: [Clear, benefit-driven subject]

Hi [Name],

[Opening: What's the value for them]

[Body: How it works / What they'll get]

[Specific example or benefit]

[Call to action: Clear next step]

Best regards,
[Name]
```

## 模板：Social Media

```
[Hook: Grab attention in first line]
[2-3 lines: Value or interesting fact]
[Call to action: Link, question, or engagement]
[Emoji: 1-2 max for visual interest]
```

## 文件：tone-examples.md
```
Exciting announcement:
"Save 8 hours per week on code reviews. Claude reviews your PRs automatically."

Empathetic support:
"We know deployments can be stressful. Claude handles testing so you don't have to worry."

Confident product feature:
"Claude doesn't just suggest code. It understands your architecture and maintains consistency."

Educational blog post:
"Let's explore how agents improve code review workflows. Here's what we learned..."
```

#### 示例 3：文档生成器 Skill

**文件：** `.claude/skills/doc-generator/SKILL.md`

~~~~yaml
---
name: API Documentation Generator
description: Generate comprehensive, accurate API documentation from source code
version: "1.0.0"
tags:
  - documentation
  - api
  - automation
when_to_use: When creating or updating API documentation
---

# API Documentation Generator Skill

## Generates

- OpenAPI/Swagger specifications
- API endpoint documentation
- SDK usage examples
- Integration guides
- Error code references
- Authentication guides

## Documentation Structure

### For Each Endpoint

```markdown
## GET /api/v1/users/:id

### Description
Brief explanation of what this endpoint does

### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| id | string | Yes | User ID |

### Response

**200 Success**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-15T10:30:00Z"
}
```

**404 Not Found**
```json
{
  "error": "USER_NOT_FOUND",
  "message": "User does not exist"
}
```

### Examples

**cURL**
```bash
curl -X GET "https://api.example.com/api/v1/users/usr_123" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**JavaScript**
```javascript
const user = await fetch('/api/v1/users/usr_123', {
  headers: { 'Authorization': 'Bearer token' }
}).then(r => r.json());
```

**Python**
```python
response = requests.get(
    'https://api.example.com/api/v1/users/usr_123',
    headers={'Authorization': 'Bearer token'}
)
user = response.json()
```

## Python Script: generate-docs.py

```python
#!/usr/bin/env python3
import ast
import json
from typing import Dict, List

class APIDocExtractor(ast.NodeVisitor):
    """Extract API documentation from Python source code."""

    def __init__(self):
        self.endpoints = []

    def visit_FunctionDef(self, node):
        """Extract function documentation."""
        if node.name.startswith('get_') or node.name.startswith('post_'):
            doc = ast.get_docstring(node)
            endpoint = {
                'name': node.name,
                'docstring': doc,
                'params': [arg.arg for arg in node.args.args],
                'returns': self._extract_return_type(node)
            }
            self.endpoints.append(endpoint)
        self.generic_visit(node)

    def _extract_return_type(self, node):
        """Extract return type from function annotation."""
        if node.returns:
            return ast.unparse(node.returns)
        return "Any"

def generate_markdown_docs(endpoints: List[Dict]) -> str:
    """Generate markdown documentation from endpoints."""
    docs = "# API Documentation\n\n"

    for endpoint in endpoints:
        docs += f"## {endpoint['name']}\n\n"
        docs += f"{endpoint['docstring']}\n\n"
        docs += f"**Parameters**: {', '.join(endpoint['params'])}\n\n"
        docs += f"**Returns**: {endpoint['returns']}\n\n"
        docs += "---\n\n"

    return docs

if __name__ == '__main__':
    import sys
    with open(sys.argv[1], 'r') as f:
        tree = ast.parse(f.read())

    extractor = APIDocExtractor()
    extractor.visit(tree)

    markdown = generate_markdown_docs(extractor.endpoints)
    print(markdown)
~~~~
### Skill 发现与调用

```mermaid
graph TD
    A["用户请求"] --> B["Claude 分析"]
    B -->|扫描| C["可用 Skills"]
    C -->|元数据检查| D["Skill 描述匹配？"]
    D -->|是| E["加载 SKILL.md"]
    D -->|否| F["尝试下一个 Skill"]
    F -->|还有更多？| D
    F -->|没有了| G["使用通用知识"]
    E --> H["提取指令"]
    H --> I["执行 Skill"]
    I --> J["返回结果"]
```

### Skill 与其他功能对比

```mermaid
graph TB
    A["扩展 Claude"]
    B["Slash Commands"]
    C["Subagents"]
    D["Memory"]
    E["MCP"]
    F["Skills"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F

    B -->|用户调用| G["快速快捷方式"]
    C -->|自动委托| H["隔离上下文"]
    D -->|持久化| I["跨会话上下文"]
    E -->|实时| J["外部数据访问"]
    F -->|自动调用| K["自主执行"]
```

---

## Claude Code Plugins

### 概述

Claude Code Plugins 是捆绑的自定义集合（Slash Commands、Subagents、MCP 服务器和 Hooks），可通过单一命令安装。它们代表了最高级别的扩展机制——将多个功能组合成连贯的、可共享的包。

### 架构

```mermaid
graph TB
    A["Plugin"]
    B["Slash Commands"]
    C["Subagents"]
    D["MCP Servers"]
    E["Hooks"]
    F["配置"]

    A -->|捆绑| B
    A -->|捆绑| C
    A -->|捆绑| D
    A -->|捆绑| E
    A -->|捆绑| F
```

### Plugin 加载流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Plugin as Plugin Marketplace
    participant Install as 安装
    participant SlashCmds as Slash Commands
    participant Subagents
    participant MCPServers as MCP Servers
    participant Hooks
    participant Tools as 已配置工具

    User->>Claude: /plugin install pr-review
    Claude->>Plugin: 下载 Plugin 清单
    Plugin-->>Claude: 返回 Plugin 定义
    Claude->>Install: 提取组件
    Install->>SlashCmds: 配置
    Install->>Subagents: 配置
    Install->>MCPServers: 配置
    Install->>Hooks: 配置
    SlashCmds-->>Tools: 就绪
    Subagents-->>Tools: 就绪
    MCPServers-->>Tools: 就绪
    Hooks-->>Tools: 就绪
    Tools-->>Claude: Plugin 已安装 ✅
```

### Plugin 类型与分发

| 类型 | 作用域 | 是否共享 | 授权方 | 示例 |
|------|-------|--------|-----------|----------|
| 官方 | 全局 | 所有用户 | Anthropic | PR Review, Security Guidance |
| 社区 | 公开 | 所有用户 | 社区 | DevOps, Data Science |
| 组织 | 内部 | 团队成员 | 公司 | 内部标准、工具 |
| 个人 | 个人 | 单用户 | 开发者 | 自定义工作流 |

### Plugin 定义结构

```yaml
---
name: plugin-name
version: "1.0.0"
description: "What this plugin does"
author: "Your Name"
license: MIT

# Plugin metadata
tags:
  - category
  - use-case

# Requirements
requires:
  - claude-code: ">=1.0.0"

# Components bundled
components:
  - type: commands
    path: commands/
  - type: agents
    path: agents/
  - type: mcp
    path: mcp/
  - type: hooks
    path: hooks/

# Configuration
config:
  auto_load: true
  enabled_by_default: true
---
```

### Plugin 结构

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── task-1.md
│   ├── task-2.md
│   └── workflows/
├── agents/
│   ├── specialist-1.md
│   ├── specialist-2.md
│   └── configs/
├── skills/
│   ├── skill-1.md
│   └── skill-2.md
├── hooks/
│   └── hooks.json
├── .mcp.json
├── .lsp.json
├── settings.json
├── templates/
│   └── issue-template.md
├── scripts/
│   ├── helper-1.sh
│   └── helper-2.py
├── docs/
│   ├── README.md
│   └── USAGE.md
└── tests/
    └── plugin.test.js
```

### 实用示例

#### 示例 1：PR Review Plugin

**文件：** `.claude-plugin/plugin.json`

```json
{
  "name": "pr-review",
  "version": "1.0.0",
  "description": "Complete PR review workflow with security, testing, and docs",
  "author": {
    "name": "Anthropic"
  },
  "license": "MIT"
}
```

**文件：** `commands/review-pr.md`

```markdown
---
name: Review PR
description: Start comprehensive PR review with security and testing checks
---

# PR Review

This command initiates a complete pull request review including:

1. Security analysis
2. Test coverage verification
3. Documentation updates
4. Code quality checks
5. Performance impact assessment
```

**文件：** `agents/security-reviewer.md`

```yaml
---
name: security-reviewer
description: Security-focused code review
tools: read, grep, diff
---

# Security Reviewer

Specializes in finding security vulnerabilities:
- Authentication/authorization issues
- Data exposure
- Injection attacks
- Secure configuration
```

**安装：**

```bash
/plugin install pr-review

# Result:
# ✅ 3 slash commands installed
# ✅ 3 subagents configured
# ✅ 2 MCP servers connected
# ✅ 4 hooks registered
# ✅ Ready to use!
```

#### 示例 2：DevOps Plugin

**组件：**

```
devops-automation/
├── commands/
│   ├── deploy.md
│   ├── rollback.md
│   ├── status.md
│   └── incident.md
├── agents/
│   ├── deployment-specialist.md
│   ├── incident-commander.md
│   └── alert-analyzer.md
├── mcp/
│   ├── github-config.json
│   ├── kubernetes-config.json
│   └── prometheus-config.json
├── hooks/
│   ├── pre-deploy.js
│   ├── post-deploy.js
│   └── on-error.js
└── scripts/
    ├── deploy.sh
    ├── rollback.sh
    └── health-check.sh
```

#### 示例 3：文档 Plugin

**捆绑组件：**

```
documentation/
├── commands/
│   ├── generate-api-docs.md
│   ├── generate-readme.md
│   ├── sync-docs.md
│   └── validate-docs.md
├── agents/
│   ├── api-documenter.md
│   ├── code-commentator.md
│   └── example-generator.md
├── mcp/
│   ├── github-docs-config.json
│   └── slack-announce-config.json
└── templates/
    ├── api-endpoint.md
    ├── function-docs.md
    └── adr-template.md
```

### Plugin Marketplace

```mermaid
graph TB
    A["Plugin Marketplace"]
    B["官方<br/>Anthropic"]
    C["社区<br/>Marketplace"]
    D["企业<br/>Registry"]

    A --> B
    A --> C
    A --> D

    B -->|类别| B1["开发"]
    B -->|类别| B2["DevOps"]
    B -->|类别| B3["文档"]

    C -->|搜索| C1["DevOps 自动化"]
    C -->|搜索| C2["移动开发"]
    C -->|搜索| C3["数据科学"]

    D -->|内部| D1["公司标准"]
    D -->|内部| D2["遗留系统"]
    D -->|内部| D3["合规"]
```

### Plugin 安装与生命周期

```mermaid
graph LR
    A["发现"] -->|浏览| B["Marketplace"]
    B -->|选择| C["Plugin 页面"]
    C -->|查看| D["组件"]
    D -->|安装| E["/plugin install"]
    E -->|提取| F["配置"]
    F -->|激活| G["使用"]
    G -->|检查| H["更新"]
    H -->|可用| G
    G -->|完成| I["禁用"]
    I -->|稍后| J["启用"]
    J -->|恢复| G
```

### Plugin 功能对比

| 功能 | Slash Command | Skill | Subagent | Plugin |
|---------|---------------|-------|----------|--------|
| **安装方式** | 手动复制 | 手动复制 | 手动配置 | 一条命令 |
| **设置时间** | 5 分钟 | 10 分钟 | 15 分钟 | 2 分钟 |
| **捆绑** | 单文件 | 单文件 | 单文件 | 多个 |
| **版本控制** | 手动 | 手动 | 手动 | 自动 |
| **团队共享** | 复制文件 | 复制文件 | 复制文件 | 安装 ID |
| **更新** | 手动 | 手动 | 手动 | 自动可用 |
| **依赖** | 无 | 无 | 无 | 可能包含 |
| **Marketplace** | 无 | 无 | 无 | 有 |
| **分发** | 仓库 | 仓库 | 仓库 | Marketplace |

### Plugin 使用场景

| 使用场景 | 建议 | 原因 |
|----------|-----------------|-----|
| **团队入职** | ✅ 使用 Plugin | 即时设置，所有配置就绪 |
| **框架设置** | ✅ 使用 Plugin | 捆绑框架特定命令 |
| **企业标准** | ✅ 使用 Plugin | 集中分发，版本控制 |
| **快速任务自动化** | ❌ 使用 Command | 过度复杂 |
| **单一领域专业** | ❌ 使用 Skill | 太重，改用 Skill |
| **专业分析** | ❌ 使用 Subagent | 手动创建或使用 Skill |
| **实时数据访问** | ❌ 使用 MCP | 独立使用，不要捆绑 |

### 何时创建 Plugin

```mermaid
graph TD
    A["是否应该创建 Plugin？"]
    A -->|需要多个组件| B{"多个命令<br/>或 Subagents<br/>或 MCPs？"}
    B -->|是| C["✅ 创建 Plugin"]
    B -->|否| D["使用单独功能"]
    A -->|团队工作流| E{"需要与<br/>团队共享？"}
    E -->|是| C
    E -->|否| F["保持为本地设置"]
    A -->|复杂设置| G{"需要自动<br/>配置？"}
    G -->|是| C
    G -->|否| D
```

### 发布 Plugin

**发布步骤：**

1. 创建包含所有组件的 Plugin 结构
2. 编写 `.claude-plugin/plugin.json` 清单
3. 创建 `README.md` 文档
4. 使用 `/plugin install ./my-plugin` 在本地测试
5. 提交到 Plugin Marketplace
6. 接受审查和批准
7. 发布到 Marketplace
8. 用户可以通过一条命令安装

**提交示例：**

~~~~markdown
# PR Review Plugin

## Description
Complete PR review workflow with security, testing, and documentation checks.

## What's Included
- 3 slash commands for different review types
- 3 specialized subagents
- GitHub and CodeQL MCP integration
- Automated security scanning hooks

## Installation
```bash
/plugin install pr-review
```

## Features
✅ Security analysis
✅ Test coverage checking
✅ Documentation verification
✅ Code quality assessment
✅ Performance impact analysis

## Usage
```bash
/review-pr
/check-security
/check-tests
```

## Requirements
- Claude Code 1.0+
- GitHub access
- CodeQL (optional)
~~~~

### Plugin 与手动配置对比

**手动设置（2 小时以上）：**
- 逐个安装 Slash Commands
- 单独创建 Subagents
- 分别配置 MCP
- 手动设置 Hooks
- 记录所有内容
- 与团队共享（期望他们正确配置）

**使用 Plugin（2 分钟）：**
```bash
/plugin install pr-review
# ✅ 一切已安装并配置
# ✅ 立即可用
# ✅ 团队可以复现完全相同的设置
```

---

## 比较与集成

### 功能对比矩阵

| 功能 | 调用方式 | 持久性 | 作用域 | 使用场景 |
|---------|-----------|------------|-------|----------|
| **Slash Commands** | 手动 (`/cmd`) | 仅会话 | 单个命令 | 快速快捷方式 |
| **Subagents** | 自动委托 | 隔离上下文 | 专项任务 | 任务分配 |
| **Memory** | 自动加载 | 跨会话 | 用户/团队上下文 | 长期学习 |
| **MCP Protocol** | 自动查询 | 实时外部 | 实时数据访问 | 动态信息 |
| **Skills** | 自动调用 | 基于文件系统 | 可重用专业能力 | 自动化工作流 |

### 交互时间线

```mermaid
graph LR
    A["会话开始"] -->|加载| B["Memory (CLAUDE.md)"]
    B -->|发现| C["可用 Skills"]
    C -->|注册| D["Slash Commands"]
    D -->|连接| E["MCP Servers"]
    E -->|就绪| F["用户交互"]

    F -->|输入 /cmd| G["Slash Command"]
    F -->|请求| H["Skill 自动调用"]
    F -->|查询| I["MCP 数据"]
    F -->|复杂任务| J["委托给 Subagent"]

    G -->|使用| B
    H -->|使用| B
    I -->|使用| B
    J -->|使用| B
```

### 实际集成示例：客户支持自动化

#### 架构

```mermaid
graph TB
    User["客户邮件"] -->|接收| Router["支持路由器"]

    Router -->|分析| Memory["Memory<br/>客户历史"]
    Router -->|查找| MCP1["MCP: 客户数据库<br/>历史工单"]
    Router -->|检查| MCP2["MCP: Slack<br/>团队状态"]

    Router -->|路由复杂问题| Sub1["Subagent: 技术支持<br/>上下文: 技术问题"]
    Router -->|路由简单问题| Sub2["Subagent: 账单<br/>上下文: 支付问题"]
    Router -->|路由紧急问题| Sub3["Subagent: 升级处理<br/>上下文: 优先处理"]

    Sub1 -->|格式化| Skill1["Skill: 回复生成器<br/>保持品牌语调"]
    Sub2 -->|格式化| Skill2["Skill: 回复生成器"]
    Sub3 -->|格式化| Skill3["Skill: 回复生成器"]

    Skill1 -->|生成| Output["格式化回复"]
    Skill2 -->|生成| Output
    Skill3 -->|生成| Output

    Output -->|发布| MCP3["MCP: Slack<br/>通知团队"]
    Output -->|发送| Reply["客户回复"]
```

#### 请求流程

```markdown
## Customer Support Request Flow

### 1. Incoming Email
"I'm getting error 500 when trying to upload files. This is blocking my workflow!"

### 2. Memory Lookup
- Loads CLAUDE.md with support standards
- Checks customer history: VIP customer, 3rd incident this month

### 3. MCP Queries
- GitHub MCP: List open issues (finds related bug report)
- Database MCP: Check system status (no outages reported)
- Slack MCP: Check if engineering is aware

### 4. Skill Detection & Loading
- Request matches "Technical Support" skill
- Loads support response template from Skill

### 5. Subagent Delegation
- Routes to Tech Support Subagent
- Provides context: customer history, error details, known issues
- Subagent has full access to: read, bash, grep tools

### 6. Subagent Processing
Tech Support Subagent:
- Searches codebase for 500 error in file upload
- Finds recent change in commit 8f4a2c
- Creates workaround documentation

### 7. Skill Execution
Response Generator Skill:
- Uses Brand Voice guidelines
- Formats response with empathy
- Includes workaround steps
- Links to related documentation

### 8. MCP Output
- Posts update to #support Slack channel
- Tags engineering team
- Updates ticket in Jira MCP

### 9. Response
Customer receives:
- Empathetic acknowledgment
- Explanation of cause
- Immediate workaround
- Timeline for permanent fix
- Link to related issues
```

### 完整功能编排

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Memory as Memory<br/>CLAUDE.md
    participant MCP as MCP Servers
    participant Skills as Skills
    participant SubAgent as Subagents

    User->>Claude: 请求: "构建认证系统"
    Claude->>Memory: 加载项目标准
    Memory-->>Claude: 认证标准、团队实践
    Claude->>MCP: 在 GitHub 上查询类似实现
    MCP-->>Claude: 代码示例、最佳实践
    Claude->>Skills: 检测匹配的 Skills
    Skills-->>Claude: Security Review Skill + Testing Skill
    Claude->>SubAgent: 委托实现
    SubAgent->>SubAgent: 构建功能
    Claude->>Skills: 应用 Security Review Skill
    Skills-->>Claude: 安全检查清单结果
    Claude->>SubAgent: 委托测试
    SubAgent-->>Claude: 测试结果
    Claude->>User: 完整系统已交付
```

### 何时使用各功能

```mermaid
graph TD
    A["新任务"] --> B{任务类型？}

    B -->|重复工作流| C["Slash Command"]
    B -->|需要实时数据| D["MCP Protocol"]
    B -->|下次需要记住| E["Memory"]
    B -->|专项子任务| F["Subagent"]
    B -->|领域特定工作| G["Skill"]

    C --> C1["✅ 团队快捷方式"]
    D --> D1["✅ 实时 API 访问"]
    E --> E1["✅ 持久化上下文"]
    F --> F1["✅ 并行执行"]
    G --> G1["✅ 自动调用专业能力"]
```

### 选择决策树

```mermaid
graph TD
    Start["需要扩展 Claude？"]

    Start -->|快速重复任务| A{"手动还是自动？"}
    A -->|手动| B["Slash Command"]
    A -->|自动| C["Skill"]

    Start -->|需要外部数据| D{"是否实时？"}
    D -->|是| E["MCP Protocol"]
    D -->|否/跨会话| F["Memory"]

    Start -->|复杂项目| G{"多个角色？"}
    G -->|是| H["Subagents"]
    G -->|否| I["Skills + Memory"]

    Start -->|长期上下文| J["Memory"]
    Start -->|团队工作流| K["Slash Command +<br/>Memory"]
    Start -->|全面自动化| L["Skills +<br/>Subagents +<br/>MCP"]
```

---

## 总结表

| 方面 | Slash Commands | Subagents | Memory | MCP | Skills | Plugins |
|--------|---|---|---|---|---|---|
| **设置难度** | 简单 | 中等 | 简单 | 中等 | 中等 | 简单 |
| **学习曲线** | 低 | 中 | 低 | 中 | 中 | 低 |
| **团队收益** | 高 | 高 | 中 | 高 | 高 | 非常高 |
| **自动化程度** | 低 | 高 | 中 | 高 | 高 | 非常高 |
| **上下文管理** | 单会话 | 隔离 | 持久化 | 实时 | 持久化 | 所有功能 |
| **维护负担** | 低 | 中 | 低 | 中 | 中 | 低 |
| **可扩展性** | 好 | 优秀 | 好 | 优秀 | 优秀 | 优秀 |
| **可共享性** | 一般 | 一般 | 好 | 好 | 好 | 优秀 |
| **版本控制** | 手动 | 手动 | 手动 | 手动 | 手动 | 自动 |
| **安装方式** | 手动复制 | 手动配置 | 不适用 | 手动配置 | 手动复制 | 一条命令 |

---

## 快速入门指南

### 第 1 周：从简单开始
- 为常见任务创建 2-3 个 Slash Commands
- 在设置中启用 Memory
- 在 CLAUDE.md 中记录团队标准

### 第 2 周：添加实时访问
- 设置 1 个 MCP（GitHub 或 Database）
- 使用 `/mcp` 进行配置
- 在工作流中查询实时数据

### 第 3 周：分配工作
- 为特定角色创建第一个 Subagent
- 使用 `/agents` 命令
- 用简单任务测试委托

### 第 4 周：全面自动化
- 为重复自动化创建第一个 Skill
- 使用 Skill Marketplace 或构建自定义 Skill
- 组合所有功能实现完整工作流

### 持续进行
- 每月审查和更新 Memory
- 在新模式出现时添加新 Skills
- 优化 MCP 查询
- 改进 Subagent 提示

---

## Hooks

### 概述

Hooks 是事件驱动的 Shell 命令，在 Claude Code 事件发生时自动执行。它们无需手动干预即可实现自动化、验证和自定义工作流。

### Hook 事件

Claude Code 支持 **28 个 Hook 事件**，涵盖五种 Hook 类型（command、http、mcp_tool、prompt、agent）：

| Hook 事件 | 触发时机 | 使用场景 |
|------------|---------|-----------|
| **SessionStart** | 会话开始/恢复/清除/压缩 | 环境设置、初始化 |
| **InstructionsLoaded** | CLAUDE.md 或规则文件被加载 | 验证、转换、增强 |
| **UserPromptSubmit** | 用户提交提示 | 输入验证、提示过滤 |
| **PreToolUse** | 任何工具运行前 | 验证、审批门控、日志 |
| **PermissionRequest** | 显示权限对话框 | 自动批准/拒绝流程 |
| **PostToolUse** | 工具成功执行后 | 自动格式化、通知、清理 |
| **PostToolUseFailure** | 工具执行失败 | 错误处理、日志 |
| **Notification** | 发送通知 | 警报、外部集成 |
| **SubagentStart** | Subagent 被创建 | 上下文注入、初始化 |
| **SubagentStop** | Subagent 完成 | 结果验证、日志 |
| **Stop** | Claude 完成响应 | 摘要生成、清理任务 |
| **StopFailure** | API 错误结束回合 | 错误恢复、日志 |
| **TeammateIdle** | Agent Team 队友空闲 | 工作分配、协调 |
| **TaskCompleted** | 任务被标记为完成 | 任务后处理 |
| **TaskCreated** | 通过 TaskCreate 创建任务 | 任务跟踪、日志 |
| **ConfigChange** | 配置文件变更 | 验证、传播 |
| **CwdChanged** | 工作目录变更 | 目录特定设置 |
| **FileChanged** | 被监视的文件变更 | 文件监控、重新构建触发 |
| **PreCompact** | 上下文压缩前 | 状态保存 |
| **PostCompact** | 压缩完成后 | 压缩后操作 |
| **WorktreeCreate** | 工作树正在创建 | 环境设置、依赖安装 |
| **WorktreeRemove** | 工作树正在删除 | 清理、资源释放 |
| **Elicitation** | MCP 服务器请求用户输入 | 输入验证 |
| **ElicitationResult** | 用户响应 Elicitation | 响应处理 |
| **SessionEnd** | 会话终止 | 清理、最终日志 |

### 常用 Hooks

Hooks 在 `~/.claude/settings.json`（用户级）或 `.claude/settings.json`（项目级）中配置：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "eslint $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

### Hook 环境变量

- `$CLAUDE_FILE_PATH` - 正在编辑/写入的文件路径
- `$CLAUDE_TOOL_NAME` - 正在使用的工具名称
- `$CLAUDE_SESSION_ID` - 当前会话标识符
- `$CLAUDE_PROJECT_DIR` - 项目目录路径

### 最佳实践

✅ **应该做：**
- 保持 Hooks 快速执行（< 1 秒）
- 使用 Hooks 进行验证和自动化
- 优雅地处理错误
- 使用绝对路径

❌ **不应该做：**
- 使 Hooks 具有交互性
- 将 Hooks 用于长时间运行的任务
- 硬编码凭证

**参见**：[06-hooks/](06-hooks/) 获取详细示例

---

## Checkpoints 与 Rewind

### 概述

Checkpoints 允许你保存对话状态并回退到先前的节点，从而支持安全的实验和多种方法的探索。

### 关键概念

| 概念 | 描述 |
|---------|-------------|
| **Checkpoint** | 对话状态的快照，包括消息、文件和上下文 |
| **Rewind** | 回退到先前的 Checkpoint，丢弃后续更改 |
| **Branch Point** | 从该 Checkpoint 探索多种方法 |

### 访问 Checkpoints

Checkpoints 随每次用户提示自动创建。要回退：

```bash
# 按两次 Esc 打开 Checkpoint 浏览器
Esc + Esc

# 或使用 /rewind 命令
/rewind
```

选择 Checkpoint 时，有五个选项：
1. **恢复代码和对话** -- 将两者都回退到该节点
2. **恢复对话** -- 回退消息，保留当前代码
3. **恢复代码** -- 回退文件，保留对话
4. **从此处总结** -- 将对话压缩为摘要
5. **取消** -- 取消操作

### 使用场景

| 场景 | 工作流 |
|----------|----------|
| **探索方法** | 保存 → 尝试 A → 保存 → 回退 → 尝试 B → 比较 |
| **安全重构** | 保存 → 重构 → 测试 → 如果失败：回退 |
| **A/B 测试** | 保存 → 设计 A → 保存 → 回退 → 设计 B → 比较 |
| **错误恢复** | 发现问题 → 回退到最后的正常状态 |

### 配置

```json
{
  "autoCheckpoint": true
}
```

**参见**：[08-checkpoints/](08-checkpoints/) 获取详细示例

---

## 高级功能

### 规划模式

在编码前创建详细的实施计划。

**激活：**
```bash
/plan Implement user authentication system
```

**优势：**
- 包含时间估算的清晰路线图
- 风险评估
- 系统化任务分解
- 审查和修改的机会

### 扩展思考

用于复杂问题的深度推理。

**激活：**
- 在会话中使用 `Alt+T`（macOS 上为 `Option+T`）切换
- 设置 `MAX_THINKING_TOKENS` 环境变量进行编程控制

```bash
# 通过环境变量启用扩展思考
export MAX_THINKING_TOKENS=50000
claude -p "Should we use microservices or monolith?"
```

**优势：**
- 全面分析利弊权衡
- 更好的架构决策
- 考虑边缘情况
- 系统化评估

### 后台任务

运行长时间操作而不阻塞对话。

**用法：**
```bash
User: Run tests in background

Claude: Started task bg-1234

/task list           # 显示所有任务
/task status bg-1234 # 检查进度
/task show bg-1234   # 查看输出
/task cancel bg-1234 # 取消任务
```

### 权限模式

控制 Claude 可以做什么。

| 模式 | 描述 | 使用场景 |
|------|-------------|----------|
| **default** | 标准权限，对敏感操作进行提示 | 一般开发 |
| **acceptEdits** | 自动接受文件编辑而无需确认 | 可信编辑工作流 |
| **plan** | 仅分析和规划，不修改文件 | 代码审查、架构规划 |
| **auto** | 自动批准安全操作，仅对有风险操作提示 | 平衡自主性与安全性 |
| **dontAsk** | 执行所有操作而无需确认提示 | 经验丰富的用户、自动化 |
| **bypassPermissions** | 完全不受限访问，无安全检查 | CI/CD 管道、可信脚本 |

**用法：**
```bash
claude --permission-mode plan          # 只读分析
claude --permission-mode acceptEdits   # 自动接受编辑
claude --permission-mode auto          # 自动批准安全操作
claude --permission-mode dontAsk       # 无确认提示
```

### 无头模式（Print 模式）

使用 `-p`（print）标志运行 Claude Code 而无需交互式输入，用于自动化和 CI/CD。

**用法：**
```bash
# 运行特定任务
claude -p "Run all tests"

# 管道输入进行分析
cat error.log | claude -p "explain this error"

# CI/CD 集成（GitHub Actions）
- name: AI Code Review
  run: claude -p "Review PR changes and report issues"

# JSON 输出用于脚本
claude -p --output-format json "list all functions in src/"
```

### 定时任务

使用 `/loop` 命令按计划运行重复任务。

**用法：**
```bash
/loop every 30m "Run tests and report failures"
/loop every 2h "Check for dependency updates"
/loop every 1d "Generate daily summary of code changes"
```

定时任务在后台运行，完成后报告结果。它们适用于持续监控、定期检查和自动化维护工作流。

### Chrome 集成

Claude Code 可以与 Chrome 浏览器集成以执行 Web 自动化任务。这支持在开发工作流中直接导航网页、填写表单、截取屏幕截图和从网站提取数据等功能。

### 会话管理

管理多个工作会话。

**命令：**
```bash
/resume                # 恢复先前的对话
/rename "Feature"      # 命名当前会话
/fork                  # 分支为新会话
claude -c              # 继续最近的对话
claude -r "Feature"    # 按名称/ID 恢复会话
```

### 交互功能

**键盘快捷键：**
- `Ctrl + R` - 搜索命令历史
- `Tab` - 自动完成
- `↑ / ↓` - 命令历史
- `Ctrl + L` - 清屏

**多行输入：**
```bash
User: \
> Long complex prompt
> spanning multiple lines
> \end
```

### 配置

完整配置示例：

```json
{
  "planning": {
    "autoEnter": true,
    "requireApproval": true
  },
  "extendedThinking": {
    "enabled": true,
    "showThinkingProcess": true
  },
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5
  },
  "permissions": {
    "mode": "default"
  }
}
```

**参见**：[09-advanced-features/](09-advanced-features/) 获取综合指南

---

## 模型与推理强度

Claude Code 支持三个模型，具有自适应推理强度：

| 模型 | 上下文窗口 | 强度等级 | 默认强度（Claude Code） |
|-------|----------------|---------------|------------------------------|
| Claude Opus 4.7 | 1M tokens（原生） | `low`、`medium`、`high`、`xhigh`、`max` | `xhigh`（自 Opus 4.7 发布以来，2026-04-16） |
| Claude Sonnet 4.6 | 1M tokens | `low`、`medium`、`high`、`max` | `high` 用于 Pro/Max 订阅者（在 v2.1.117 中从 `medium` 提升） |
| Claude Haiku 4.5 | 200K tokens | `low`、`medium`、`high` | `medium` |

> **注意**：v2.1.117 修复了一个 bug，之前 Opus 4.7 会话将 `/context` 按 200K 而非原生 1M 窗口计算——请升级到 v2.1.117 或更高版本以实际获得 Opus 4.7 的 1M 上下文。

> **注意**：`/cost` 和 `/stats` 在 v2.1.118 中合并为 `/usage`。`/usage` 现在是具有成本/统计等选项卡的规范命令；`/cost` 和 `/stats` 仍作为快捷别名保留，打开对应的选项卡。

## 资源

- [Claude Code 文档](https://code.claude.com/docs/en/overview)
- [Claude Code 更新日志](https://code.claude.com/docs/en/changelog)
- [MCP GitHub Servers](https://github.com/modelcontextprotocol/servers)
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

---

*最后更新：2026 年 4 月 24 日*
*适用于 Claude Haiku 4.5、Sonnet 4.6 和 Opus 4.7*
*现已包含：Hooks、Checkpoints、规划模式、扩展思考、后台任务、权限模式（6 种模式）、无头模式、会话管理、自动 Memory、Agent Teams、定时任务、Chrome 集成、Channels、语音听写和内置 Skills*

---
**最后更新**：2026 年 4 月 24 日
**Claude Code 版本**：2.1.119
**来源**：
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/hooks
- https://www.anthropic.com/news/claude-opus-4-7
- https://github.com/anthropics/claude-code/releases/tag/v2.1.117
**兼容模型**：Claude Sonnet 4.6、Claude Opus 4.7、Claude Haiku 4.5
