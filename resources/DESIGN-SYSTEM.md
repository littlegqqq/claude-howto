# Claude How To - 设计系统

## 视觉识别

### 图标设计理念：指南针与代码括号

Claude How To 图标使用**指南针搭配 `>` 代码括号**来表示代码世界中的导航引导：

```
      N (green)
      ▲
      │
 W ───>─── E     Compass = Guidance/Direction
      │          > Bracket = Code/Terminal/CLI
      ▼
      S (black)
```

这带来了：
- **视觉清晰度**：立即传达"代码导航指南"的含义
- **象征意义**：指南针 = 找到方向；`>` = 代码/终端
- **可扩展性**：从 16px 到 512px 任何尺寸都适用
- **品牌契合**：以极简调色板契合开发者工具美学

---

## 色彩系统

### 调色板

| 颜色 | Hex | RGB | 用途 |
|------|-----|-----|------|
| 黑色（主色） | `#000000` | 0, 0, 0 | 主描边、文本、南针 |
| 白色（背景） | `#FFFFFF` | 255, 255, 255 | 浅色背景 |
| 灰色（辅助） | `#6B7280` | 107, 114, 128 | 次要刻度线、辅助文本 |
| 亮绿色（强调） | `#22C55E` | 34, 197, 94 | 北针、中心点、强调线 |
| 近黑色（深色背景） | `#0A0A0A` | 10, 10, 10 | 深色模式背景 |

### 对比度比率（WCAG）

- 黑色在白色上：**21:1** AAA
- 灰色在白色上：**4.6:1** AA
- 绿色在白色上：**3.2:1**（仅装饰用途，不用于文本）
- 白色在深色上：**19.5:1** AAA

### 强调色规则

**亮绿色（#22C55E）仅限高亮用途：**
- 指南针北针
- 中心圆点
- 强调下划线/边框
- 绝不用作背景色
- 绝不用于正文文本

---

## 排版

### Logo 字体
- **字族**：Inter、SF Pro Display、-apple-system、Segoe UI、sans-serif
- **"Claude"**：42px、字重 700（粗体）、黑色
- **"How-To"**：32px、字重 500（中等）、灰色（#6B7280）
- **副标题**：10px、字重 500、灰色、字间距 1.5px、大写

### 界面字体
- **字族**：Inter、SF Pro、系统字体（sans-serif）
- **字重**：400-600
- **风格**：简洁、易读

---

## 图标细节

### 指南针规格

指南针标识由以下几何元素构成：

```
Element             | Stroke/Fill    | Color
--------------------|----------------|------------------
Outer ring          | 3px stroke     | Black / White (dark mode)
North tick          | 2.5px stroke   | Black / White (dark mode)
Other cardinal ticks| 2px stroke     | Gray / White 50% (dark mode)
Intercardinal ticks | 1.5px stroke   | Gray / White 40% (dark mode)
North needle        | filled polygon | #22C55E (always green)
South needle        | filled polygon | Black / White (dark mode)
> bracket           | 3px stroke     | Black / White (dark mode)
Center dot          | filled circle  | #22C55E (always green)
```

### 尺寸递进

```
16px  → Ring + needles + chevron only (minimal)
32px  → Adds cardinal tick marks
64px  → Adds intercardinal tick marks
128px → Full detail, all elements crisp
256px → Maximum detail, thick strokes
```

---

## 尺寸规范

### Logo 尺寸

- **最小**：200px 宽度（用于网页）
- **推荐**：520px（原始尺寸）
- **最大**：无限制（矢量格式）
- **宽高比**：约 4.3:1（宽:高）

### 图标尺寸

- **最小**：16px（favicon）
- **推荐**：64-256px（应用、头像）
- **最大**：无限制（矢量格式）
- **宽高比**：1:1（正方形）

---

## 间距与对齐

### Logo 间距

```
┌─────────────────────────────────────┐
│                                     │
│        Clear Space Minimum          │
│         (logo height / 2)           │
│                                     │
│    [COMPASS]  Claude                │
│               How-To                │
│                                     │
└─────────────────────────────────────┘
```

### 图标中心点

所有图标在画布中心对齐：
- 256px 画布的 128×128
- 128px 画布的 64×64
- 与其他 UI 元素保持对齐

---

## 无障碍性

### 颜色对比度
- 所有文本满足 WCAG AA（最低 4.5:1）
- 绿色强调色为装饰性，非信息性
- 不依赖红绿色

### 可扩展性
- 矢量格式确保任何尺寸下都清晰
- 几何图形在 16px 下仍可辨识
- 基于可用尺寸的渐进细节

---

## 应用示例

### 网页头部
- 尺寸：520×120px logo
- 文件：`logos/claude-howto-logo.svg`
- 背景：白色或深色（#0A0A0A）
- 内边距：最小 20px

### 应用图标
- 尺寸：256×256px
- 文件：`icons/claude-howto-icon.svg`
- 背景：白色或深色
- 用途：应用快捷方式、头像

### 浏览器 Favicon
- 尺寸：32px（主要）、16px（备选）
- 文件：`favicons/favicon-32.svg`
- 格式：SVG 确保清晰显示

### 社交媒体
- 头像：256×256px 图标
- 横幅：520×120px logo（居中）

### 文档
- 章节头部：Logo 缩放适配
- 段落图标：64×64px favicon
- 行内：32×32px favicon

---

## 文件格式详情

### SVG 结构

所有 SVG 文件采用扁平设计：
- 无渐变（仅纯色）
- 无滤镜效果（无模糊、发光或阴影）
- 干净的描边和填充几何图形
- ViewBox 实现响应式缩放
- 可读的、带注释的代码

### 跨浏览器兼容性

- Chrome/Edge：完全支持
- Firefox：完全支持
- Safari：完全支持
- iOS Safari：完全支持
- 所有现代浏览器：完全支持

---

## 自定义

### 更改强调色

创建使用不同强调色的变体：

1. 将所有 `#22C55E` 替换为你的强调色
2. 确保装饰元素的对比度比率保持在 3:1 以上
3. 保持黑/白/灰结构不变

### 缩放

```css
svg {
  width: 256px;
  height: 256px;
}
```

SVG 通过 viewBox 自动缩放 — 无需变换。

---

## 版本控制

在 git 中跟踪设计变更：
- 正常版本管理 SVG 文件（它们是文本格式）
- 为包含设计变更的版本打标签
- 在提交中包含 DESIGN-SYSTEM.md

---

**最后更新**：2026 年 2 月
**设计系统版本**：3.0
