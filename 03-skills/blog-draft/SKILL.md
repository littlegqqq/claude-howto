---
name: blog-draft
description: 从想法和资源起草博客文章。当用户想写博客文章、从研究中创建内容或起草文章时使用。引导用户完成研究、头脑风暴、大纲制定和带版本控制的迭代起草过程。
---

## 用户输入

```text
$ARGUMENTS
```

你**必须**在继续之前考虑用户输入。用户应提供：
- **想法/主题**：博客文章的主要概念或主题
- **资源**：URL、文件或研究参考（可选但推荐）
- **目标受众**：博客文章是为谁写的（可选）
- **语调/风格**：正式、随意、技术性等（可选）

**重要**：如果用户请求更新**现有博客文章**，跳过步骤 0-8，直接从**步骤 9** 开始。首先阅读现有草稿文件，然后进行迭代过程。

## 执行流程

按顺序执行以下步骤。**不要跳过步骤或在指示需要用户批准的地方未经批准就继续。**

### 步骤 0：创建项目文件夹

1. 使用格式生成文件夹名称：`YYYY-MM-DD-short-topic-name`
   - 使用今天的日期
   - 从主题创建一个简短的、URL 友好的 slug（小写，连字符，最多 5 个单词）

2. 创建文件夹结构：
   ```
   blog-posts/
   └── YYYY-MM-DD-short-topic-name/
       └── resources/
   ```

3. 在继续之前与用户确认文件夹创建。

### 步骤 1：研究与资源收集

1. 在博客文章目录中创建 `resources/` 子文件夹

2. 对于每个提供的资源：
   - **URL**：获取并将关键信息保存到 `resources/` 作为 markdown 文件
   - **文件**：阅读并在 `resources/` 中总结
   - **主题**：使用网络搜索收集最新信息

3. 对于每个资源，在 `resources/` 中创建摘要文件：
   - `resources/source-1-[short-name].md`
   - `resources/source-2-[short-name].md`
   - 等等

4. 每个摘要应包括：
   ```markdown
   # Source: [Title/URL]

   ## Key Points
   - Point 1
   - Point 2

   ## Relevant Quotes/Data
   - Quote or statistic 1
   - Quote or statistic 2

   ## How This Relates to Topic
   Brief explanation of relevance
   ```

5. 向用户展示研究摘要。

### 步骤 2：头脑风暴与澄清

1. 根据想法和研究的资源，展示：
   - 从研究中识别的**主要主题**
   - 博客文章的**潜在角度**
   - 应该涵盖的**关键点**
   - 需要澄清的信息**空白**

2. 提出澄清问题：
   - 你希望读者获得的主要收获是什么？
   - 研究中是否有你想强调的特定要点？
   - 目标长度是多少？（短：500-800 字，中：1000-1500，长：2000+）
   - 有什么要排除的要点吗？

3. **在继续之前等待用户回复。**

### 步骤 3：提出大纲

1. 创建结构化大纲，包括：

   ```markdown
   # Blog Post Outline: [Title]

   ## Meta Information
   - **Target Audience**: [who]
   - **Tone**: [style]
   - **Target Length**: [word count]
   - **Main Takeaway**: [key message]

   ## Proposed Structure

   ### Hook/Introduction
   - Opening hook idea
   - Context setting
   - Thesis statement

   ### Section 1: [Title]
   - Key point A
   - Key point B
   - Supporting evidence from [source]

   ### Section 2: [Title]
   - Key point A
   - Key point B

   [Continue for all sections...]

   ### Conclusion
   - Summary of key points
   - Call to action or final thought

   ## Sources to Cite
   - Source 1
   - Source 2
   ```

2. 向用户展示大纲并**请求批准或修改**。

### 步骤 4：保存已批准的大纲

1. 用户批准大纲后，将其保存到博客文章文件夹中的 `OUTLINE.md`。

2. 确认大纲已保存。

### 步骤 5：提交大纲（如果在 git 仓库中）

1. 检查当前目录是否是 git 仓库。

2. 如果是：
   - 暂存新文件：博客文章文件夹、资源和 OUTLINE.md
   - 创建提交消息：`docs: Add outline for blog post - [topic-name]`
   - 推送到远程

3. 如果不是 git 仓库，跳过此步骤并通知用户。

### 步骤 6：撰写草稿

1. 根据已批准的大纲，撰写完整的博客文章草稿。

2. 严格按照 OUTLINE.md 的结构。

3. 包括：
   - 带有吸引力的引言
   - 清晰的章节标题
   - 来自研究的支持证据和示例
   - 章节之间的平滑过渡
   - 带有要点的有力结论
   - **引用**：所有比较、统计数据、数据点和事实性声明都必须引用原始来源

4. 将草稿保存为博客文章文件夹中的 `draft-v0.1.md`。

5. 格式：
   ```markdown
   # [Blog Post Title]

   *[Optional: subtitle or tagline]*

   [Full content with inline citations...]

   ---

   ## References
   - [1] Source 1 Title - URL or Citation
   - [2] Source 2 Title - URL or Citation
   - [3] Source 3 Title - URL or Citation
   ```

6. **引用要求**：
   - 每个数据点、统计数据或比较都必须有内联引用
   - 使用编号引用 [1]、[2] 等，或命名引用 [来源名称]
   - 将引用链接到末尾的参考文献部分
   - 示例："研究表明 65% 的开发者偏好 TypeScript [1]"
   - 示例："React 在渲染速度上比 Vue 快 20% [React Benchmarks 2024]"

### 步骤 7：提交草稿（如果在 git 仓库中）

1. 检查是否在 git 仓库中。

2. 如果是：
   - 暂存草稿文件
   - 创建提交消息：`docs: Add draft v0.1 for blog post - [topic-name]`
   - 推送到远程

3. 如果不是 git 仓库，跳过并通知用户。

### 步骤 8：提交草稿供审阅

1. 向用户展示草稿内容。

2. 请求反馈：
   - 整体印象？
   - 需要扩展或缩减的章节？
   - 需要调整的语调？
   - 缺失的信息？
   - 具体的编辑或重写？

3. **等待用户回复。**

### 步骤 9：迭代或定稿

**如果用户请求更改：**
1. 记录所有请求的修改
2. 返回步骤 6，进行以下调整：
   - 递增版本号（v0.2、v0.3 等）
   - 纳入所有反馈
   - 保存为 `draft-v[X.Y].md`
   - 重复步骤 7-8

**如果用户批准：**
1. 确认最终草稿版本
2. 如果用户请求，可选择重命名为 `final.md`
3. 总结博客文章创建过程：
   - 创建的版本总数
   - 版本之间的关键变更
   - 最终字数
   - 创建的文件

## 版本跟踪

所有草稿都以递增版本保存：
- `draft-v0.1.md` - 初始草稿
- `draft-v0.2.md` - 第一轮反馈后
- `draft-v0.3.md` - 第二轮反馈后
- 等等

这允许跟踪博客文章的演变过程，并在需要时回退。

## 输出文件结构

```
blog-posts/
└── YYYY-MM-DD-topic-name/
    ├── resources/
    │   ├── source-1-name.md
    │   ├── source-2-name.md
    │   └── ...
    ├── OUTLINE.md
    ├── draft-v0.1.md
    ├── draft-v0.2.md (if iterations)
    └── draft-v0.3.md (if more iterations)
```

## 质量提示

- **吸引力**：以问题、令人惊讶的事实或可共鸣的场景开头
- **流畅性**：每个段落应连接到下一个
- **证据**：用研究数据支持声明
- **引用**：始终为以下内容引用来源：
  - 所有统计数据和数据点（例如，"根据 [来源]，75% 的..."）
  - 产品、服务或方法之间的比较（例如，"X 比 Y 快 2 倍 [来源]"）
  - 关于市场趋势、研究发现或基准的事实性声明
  - 使用格式的内联引用：[来源名称] 或 [作者，年份]
- **语调**：全文保持一致的语调
- **长度**：遵守目标字数
- **可读性**：使用短段落，在适当时使用项目符号
- **行动号召**：以清晰的行动号召或发人深省的问题结尾

## 备注

- 始终在概述的检查点处等待用户批准
- 保留所有草稿版本以供历史参考
- 当提供 URL 时使用网络搜索获取最新信息
- 如果资源不足，请用户提供更多或建议额外研究
- 根据目标受众调整语调（技术性、通用、商业等）
