---
description: Create comprehensive API documentation from source code
---

# API 文档生成器

通过以下步骤生成 API 文档：

1. 扫描 `/src/api/` 中的所有文件
2. 提取函数签名和 JSDoc 注释
3. 按端点（Endpoint）/模块组织
4. 创建带示例的 Markdown 文档
5. 包含请求/响应 Schema（模式）
6. 添加错误文档

输出格式：
- Markdown 文件，保存至 `/docs/api.md`
- 为所有端点包含 curl 示例
- 添加 TypeScript 类型

---
**最后更新**：2026 年 4 月 9 日
