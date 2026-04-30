---
name: Expand Unit Tests
description: Increase test coverage by targeting untested branches and edge cases
tags: testing, coverage, unit-tests
---

# 扩展单元测试

扩展现有的单元测试，以适配项目的测试框架：

1. **分析覆盖率**：运行覆盖率报告，识别未测试的分支、边界情况和低覆盖率区域
2. **识别差距**：审查代码中的逻辑分支、错误路径、边界条件、空值/空输入
3. **编写测试**，使用项目的测试框架：
   - Jest/Vitest/Mocha（JavaScript/TypeScript）
   - pytest/unittest（Python）
   - Go testing/testify（Go）
   - Rust test framework（Rust）
4. **针对特定场景**：
   - 错误处理和异常
   - 边界值（最小值/最大值、空值、null）
   - 边界情况和极端情况
   - 状态转换和副作用
5. **验证改进**：再次运行覆盖率报告，确认覆盖率有可衡量的提升

仅展示新的测试代码块。遵循现有的测试模式和命名规范。

---
**最后更新**：2026 年 4 月 9 日
