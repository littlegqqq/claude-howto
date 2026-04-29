<picture>
  <source media="(prefers-color-scheme: dark)" srcset="logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="logos/claude-howto-logo.svg">
</picture>

# Claude How To - 品牌资产

Claude How To 项目的完整 logo、图标和 favicon 合集。所有资产采用 V3.0 设计：带有代码括号（`>`）符号的指南针，代表代码世界中的导航引导 — 使用黑/白/灰调色板搭配亮绿色（#22C55E）强调色。

## 目录结构

```
resources/
├── logos/
│   ├── claude-howto-logo.svg       # Main logo - Light mode (520×120px)
│   └── claude-howto-logo-dark.svg  # Main logo - Dark mode (520×120px)
├── icons/
│   ├── claude-howto-icon.svg       # App icon - Light mode (256×256px)
│   └── claude-howto-icon-dark.svg  # App icon - Dark mode (256×256px)
└── favicons/
    ├── favicon-16.svg              # Favicon - 16×16px
    ├── favicon-32.svg              # Favicon - 32×32px (primary)
    ├── favicon-64.svg              # Favicon - 64×64px
    ├── favicon-128.svg             # Favicon - 128×128px
    └── favicon-256.svg             # Favicon - 256×256px
```

`assets/logo/` 中的其他资产：
```
assets/logo/
├── logo-full.svg       # Mark + wordmark (horizontal)
├── logo-mark.svg       # Compass symbol only (120×120px)
├── logo-wordmark.svg   # Text only
├── logo-icon.svg       # App icon (512×512, rounded)
├── favicon.svg         # 16×16 optimized
├── logo-white.svg      # White version for dark backgrounds
└── logo-black.svg      # Black monochrome version
```

## 资产概览

### 设计理念（V3.0）

**指南针与代码括号** — 引导与代码的结合：
- **指南针环** = 导航，找到方向
- **北针（绿色）** = 方向，学习之路上的进步
- **南针（黑色）** = 根基，坚实的基础
- **`>` 括号** = 终端提示符，代码，CLI 上下文
- **刻度线** = 精确，结构化学习

### Logo

**文件**：
- `logos/claude-howto-logo.svg`（浅色模式）
- `logos/claude-howto-logo-dark.svg`（深色模式）

**规格**：
- **尺寸**：520×120 px
- **用途**：带文字标识的主要头部/品牌 logo
- **使用场景**：
  - 网站头部
  - README 徽章
  - 营销材料
  - 印刷品
- **格式**：SVG（完全可缩放）
- **模式**：浅色（白色背景）& 深色（#0A0A0A 背景）

### 图标

**文件**：
- `icons/claude-howto-icon.svg`（浅色模式）
- `icons/claude-howto-icon-dark.svg`（深色模式）

**规格**：
- **尺寸**：256×256 px
- **用途**：应用图标、头像、缩略图
- **使用场景**：
  - 应用图标
  - 个人头像
  - 社交媒体缩略图
  - 文档头部
- **格式**：SVG（完全可缩放）
- **模式**：浅色（白色背景）& 深色（#0A0A0A 背景）

**设计元素**：
- 带有基本和中间刻度线的指南针环
- 绿色北针（方向/引导）
- 黑色南针（基础）
- 中心 `>` 代码括号（终端/CLI）
- 绿色中心点强调

### Favicon

为网页使用优化的多尺寸版本：

| 文件 | 尺寸 | DPI | 用途 |
|------|------|-----|------|
| `favicon-16.svg` | 16×16 px | 1x | 浏览器标签页（旧版浏览器） |
| `favicon-32.svg` | 32×32 px | 1x | 标准浏览器 favicon |
| `favicon-64.svg` | 64×64 px | 1x-2x | 高 DPI 显示屏 |
| `favicon-128.svg` | 128×128 px | 2x | Apple touch icon、书签 |
| `favicon-256.svg` | 256×256 px | 4x | 现代浏览器、PWA 图标 |

**优化说明**：
- 16px：最简几何 — 仅环、指针、V 形
- 32px：添加基本刻度线
- 64px+：完整细节，含中间刻度线
- 所有尺寸与主图标保持视觉一致性
- SVG 格式确保在任何尺寸下都清晰显示

## HTML 集成

### 基础 Favicon 设置

```html
<!-- Browser favicon -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">

<!-- Apple touch icon (mobile home screen) -->
<link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">

<!-- PWA & modern browsers -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">
```

### 完整设置

```html
<head>
  <!-- Primary favicon -->
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg" sizes="32x32">
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">

  <!-- Apple touch icon -->
  <link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">

  <!-- PWA icons -->
  <link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">

  <!-- Android -->
  <link rel="shortcut icon" href="/resources/favicons/favicon-256.svg">

  <!-- PWA manifest reference (if using manifest.json) -->
  <meta name="theme-color" content="#000000">
</head>
```

## 调色板

### 主色调
- **黑色**：`#000000`（主文本、描边、南针）
- **白色**：`#FFFFFF`（浅色背景）
- **灰色**：`#6B7280`（辅助文本、次要刻度线）

### 强调色
- **亮绿色**：`#22C55E`（北针、中心点、强调线 — 仅用于高亮，绝不用作背景）

### 深色模式
- **背景**：`#0A0A0A`（近黑色）

### CSS 变量
```css
--color-primary: #000000;
--color-secondary: #6B7280;
--color-accent: #22C55E;
--color-bg-light: #FFFFFF;
--color-bg-dark: #0A0A0A;
```

### Tailwind 配置
```js
colors: {
  brand: {
    primary: '#000000',
    secondary: '#6B7280',
    accent: '#22C55E',
  }
}
```

### 使用指南
- 黑色用于主文本和结构元素
- 灰色用于辅助/支撑元素
- 绿色**仅**用于高亮 — 指针、圆点、强调线
- 绝不将绿色用作背景色
- 保持 WCAG AA 对比度（最低 4.5:1）

## 设计规范

### Logo 使用
- 在白色或深色（#0A0A0A）背景上使用
- 等比例缩放
- 在 logo 周围保留净空间（最小值：logo 高度 / 2）
- 根据背景使用对应的浅色/深色版本

### 图标使用
- 使用标准尺寸：16、32、64、128、256px
- 保持指南针比例
- 等比例缩放

### Favicon 使用
- 根据场景使用合适的尺寸
- 16-32px：浏览器标签页、书签
- 64px：Favicon 站点图标
- 128px+：Apple/Android 主屏幕

## SVG 优化

所有 SVG 文件采用扁平设计，无渐变或滤镜：
- 干净的基于描边的几何图形
- 无嵌入栅格图
- 优化的路径
- 响应式 viewBox

网页优化方法：
```bash
# Compress SVG while maintaining quality
svgo --config='{
  "js2svg": {
    "indent": 2
  },
  "plugins": [
    "convertStyleToAttrs",
    "removeRasterImages"
  ]
}' input.svg -o output.svg
```

## PNG 转换

将 SVG 转换为 PNG 以支持旧版浏览器：

```bash
# Using ImageMagick
convert -density 300 -background none favicon-256.svg favicon-256.png

# Using Inkscape
inkscape -D -z --file=favicon-256.svg --export-png=favicon-256.png
```

## 无障碍性

- 高对比度色彩比率（符合 WCAG AA — 最低 4.5:1）
- 在所有尺寸下都可识别的干净几何图形
- 可缩放矢量格式
- 图标中无文字（文字在文字标识中单独添加）
- 不依赖红绿色来传达含义

## 署名

这些资产是 Claude How To 项目的一部分。

**许可证**：MIT（见项目 LICENSE 文件）

## 版本历史

- **v3.0**（2026 年 2 月）：指南针-括号设计，黑/白/灰 + 绿色强调色调色板
- **v2.0**（2026 年 1 月）：Claude 风格的 12 射线星爆设计，祖母绿调色板
- **v1.0**（2026 年 1 月）：原始六边形渐进式图标设计

---

**最后更新**：2026 年 2 月
**当前版本**：3.0（指南针-括号）
**所有资产**：生产就绪 SVG，完全可缩放，符合 WCAG AA 无障碍标准
