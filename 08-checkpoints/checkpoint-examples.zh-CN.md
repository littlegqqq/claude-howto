# 检查点示例

在 Claude Code 中有效使用检查点的实际示例。

注意：检查点会在每次用户提示时自动创建，无需手动保存。要回退，请按两次 `Esc`（`Esc+Esc`）或使用 `/rewind` 打开检查点浏览器。

## 示例 1：数据库迁移

### 场景
你需要从 MySQL 迁移到 PostgreSQL，但想要分别探索直接迁移和渐进式双写两种方案。

### 工作流程

```
User: 让我们实现从 MySQL 到 PostgreSQL 的直接迁移
      [自动创建检查点 A]

Claude: 我来实现直接迁移...
[修改数据库驱动，更新查询语句，修改数据库结构]

User: 运行测试
      [自动创建检查点 B]

Claude: 正在运行测试...
❌ 15 个测试失败 - 连接池问题

User: 这个方案有问题。让我们改用双写模式。
[用户按 Esc+Esc，选择检查点 A，选择 "Restore code and conversation"]

User: 现在实现双写模式，同时写入两个数据库

Claude: 我来实现双写迁移...
[实现双写层和渐进式迁移策略]

User: 运行测试

Claude: 正在运行测试...
✅ 所有测试通过

User: 双写方案明显更安全。让我们提交这个。
```

## 示例 2：性能优化

### 场景
需要尝试多种性能优化策略。

### 工作流程

```
User: 让我们优化 API 响应时间。当前：450ms
      [自动创建 "Baseline" 检查点]

Claude: 我来添加数据库查询缓存...
[实现 Redis 缓存]

User: 运行性能测试

Claude: 结果：平均响应时间 280ms（提升 38%）

User: 让我试试另一种方案。
[用户按 Esc+Esc，选择 "Baseline" 检查点，选择 "Restore code and conversation"]

User: 尝试数据库查询优化

Claude: 我来优化数据库查询...
[添加索引，优化 N+1 查询]

User: 运行性能测试

Claude: 结果：平均响应时间 180ms（提升 60%）

User: 让我再试试第三种方案。
[用户按 Esc+Esc，选择 "Baseline" 检查点，选择 "Restore code and conversation"]

User: 尝试响应压缩和 CDN

Claude: 我来添加压缩和 CDN...
[实现 gzip，配置 CDN]

User: 运行性能测试

Claude: 结果：平均响应时间 320ms（提升 29%）

User: 查询优化效果最好。让我回去把它和缓存结合起来。
[用户按 Esc+Esc，选择查询优化之后的检查点，选择 "Restore code and conversation"]

User: 现在在这些优化后的查询基础上添加 Redis 缓存

Claude: [在优化后的查询基础上添加 Redis 缓存]

User: 运行性能测试

Claude: 结果：平均响应时间 95ms（提升 79%）🎉

User: 完美！这是最佳方案。让我们提交这个。
```

## 示例 3：UI/UX 迭代

### 场景
尝试仪表盘的不同布局方案。

### 工作流程

```
User: 让我们用侧边栏布局重新设计仪表盘
      [自动创建 "Start" 检查点]

Claude: [实现侧边栏布局]

User: 给我看截图

Claude: [展示截图]

User: 侧边栏感觉太拥挤了。让我们试试顶部导航。
[用户按 Esc+Esc，选择 "Start" 检查点，选择 "Restore code and conversation"]

User: 尝试顶部导航布局

Claude: [实现顶部导航]

User: 给我看截图

Claude: [展示截图]

User: 好一些了，但让我们试试卡片网格布局
[用户按 Esc+Esc，选择 "Start" 检查点，选择 "Restore code and conversation"]

User: 尝试卡片网格布局

Claude: [实现卡片网格]

User: 给我看截图

Claude: [展示截图]

User: 很棒！但能把顶部导航和卡片网格结合起来吗？
[用户按 Esc+Esc，选择卡片网格之后的检查点，选择 "Restore code and conversation"]

User: 在卡片网格布局上添加顶部导航

Claude: [在卡片网格上添加顶部导航]

User: 完美！这就是最终设计。
```

## 示例 4：调试会话

### 场景
通过多个假设追踪一个棘手的 bug。

### 工作流程

```
User: 应用程序存在内存泄漏
      [自动创建 "Before debugging" 检查点]

Claude: 让我来调查...
[分析代码]

User: 我觉得问题出在事件监听器上。让我们修复它们。

Claude: [修复事件监听器清理逻辑]

User: 运行内存分析器

Claude: 仍然存在内存泄漏 - 事件监听器不是问题所在

User: 让我回去试试不同的假设。
[用户按 Esc+Esc，选择 "Before debugging" 检查点，选择 "Restore code and conversation"]

User: 也许是数据库连接的问题？

Claude: [实现连接池修复]

User: 运行内存分析器

Claude: 仍然在泄漏 - 也不是连接的问题

User: 让我回去换个角度试试。
[用户按 Esc+Esc，选择 "Before debugging" 检查点，选择 "Restore code and conversation"]

User: 让我们检查循环引用

Claude: [找到并修复缓存中的循环引用]

User: 运行内存分析器

Claude: ✅ 内存泄漏已修复！缓存层中的循环引用就是罪魁祸首。

User: 太好了！让我们提交这个修复。
```

## 示例 5：API 设计演进

### 场景
通过多次迭代设计 REST API。

### 工作流程

```
User: 为我们的博客平台创建一个 REST API
      [自动创建 "Before API design" 检查点]

Claude: [创建基本的 CRUD API]

User: 这太简单了。让我们添加分页和过滤

Claude: [添加分页和过滤]

User: 我们需要更好的资源关联

Claude: [实现 HATEOAS 链接]

User: 其实，让我们试试 GraphQL 来替代 REST
[用户按 Esc+Esc，选择 "Before API design" 检查点，选择 "Restore code and conversation"]

User: 改为我们的博客平台实现一个 GraphQL API

Claude: [实现 GraphQL API]

User: 我觉得 REST 更适合我们的场景。
[用户按 Esc+Esc，选择 HATEOAS 之后的检查点，选择 "Restore code and conversation"]

User: 让我们确定使用 REST 方案。
```

## 示例 6：配置管理

### 场景
测试不同的配置管理方案。

### 工作流程

```
User: 将配置移到环境变量中
      [自动创建 "Original config" 检查点]

Claude: [实现环境变量配置]

User: 在开发环境测试

Claude: 本地运行正常，但生产环境部署出现问题

User: 让我试试另一种方案。
[用户按 Esc+Esc，选择 "Original config" 检查点，选择 "Restore code and conversation"]

User: 尝试带验证的配置文件方案

Claude: [实现带 JSON schema 验证的 YAML 配置]

User: 在开发环境测试

Claude: ✅ 运行良好，验证错误提示也很好

User: 能否同时支持用环境变量存储敏感值？

Claude: [为密钥添加环境变量覆盖]

User: 测试部署

Claude: ✅ 所有环境运行正常

User: 完美！这已经可以上线了。
```

## 示例 7：测试策略

### 场景
实现全面的测试方案。

### 工作流程

```
User: 为认证模块添加单元测试
      [自动创建检查点]

Claude: [添加 Jest 单元测试]

User: 运行测试

Claude: 45 个测试通过，78% 覆盖率

User: 也添加集成测试

Claude: [添加集成测试]

User: 运行测试

Claude: 89 个测试通过，92% 覆盖率，但测试很慢（3 分钟）

User: 测试太慢了。让我们优化一下。

Claude: [优化测试设置，添加并行执行]

User: 运行测试

Claude: 89 个测试通过，92% 覆盖率，35 秒 ✅

User: 很好！现在为关键路径添加 E2E 测试

Claude: [添加 Playwright E2E 测试]

User: 运行所有测试

Claude: 112 个测试通过，94% 覆盖率，2 分钟

User: 覆盖率和速度的完美平衡！
```

## 示例 8：从检查点使用摘要功能

### 场景
在一次漫长的调试会话后，你想要压缩对话同时保留上下文。

### 工作流程

```
User: [经过 20 多条消息的调试和探索之后]

[用户按 Esc+Esc，选择一个早期的检查点，选择 "Summarize from here"]
[可选择提供说明："Focus on what we tried and what worked"]

Claude: [从该检查点开始生成对话摘要]
[原始消息保留在记录中]
[摘要替换可见对话，减少上下文窗口占用]

User: 现在让我们继续使用有效的方案。
```

## 关键要点

1. **检查点是自动的**：每次用户提示都会创建检查点——无需手动保存
2. **使用 Esc+Esc 或 /rewind**：这是访问检查点浏览器的两种方式
3. **选择正确的恢复选项**：根据需要恢复代码、对话、两者都恢复，或生成摘要
4. **不要害怕实验**：检查点让你可以安全地尝试激进的改动
5. **与 git 结合使用**：使用检查点进行探索，使用 git 保存最终成果
6. **为长会话生成摘要**：使用 "Summarize from here" 让对话保持可管理

---
**Last Updated**: April 9, 2026
