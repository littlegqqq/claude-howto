# 快速入门 - 品牌资产

## 将资产复制到你的项目

```bash
# Copy all resources to your web project
cp -r resources/ /path/to/your/website/

# Or just the favicons for web
cp resources/favicons/* /path/to/your/website/public/
```

## 添加到 HTML（复制粘贴即可）

```html
<!-- Favicons -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg" sizes="32x32">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">
<link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">
<meta name="theme-color" content="#000000">
```

## 在 Markdown/文档中使用

```markdown
# Claude How To

![Claude How To Logo](resources/logos/claude-howto-logo.svg)

![Icon](resources/icons/claude-howto-icon.svg)
```

## 推荐尺寸

| 用途 | 尺寸 | 文件 |
|------|------|------|
| 网站头部 | 520×120 | `logos/claude-howto-logo.svg` |
| 应用图标 | 256×256 | `icons/claude-howto-icon.svg` |
| 浏览器标签页 | 32×32 | `favicons/favicon-32.svg` |
| 手机主屏幕 | 128×128 | `favicons/favicon-128.svg` |
| 桌面应用 | 256×256 | `favicons/favicon-256.svg` |
| 小头像 | 64×64 | `favicons/favicon-64.svg` |

## 颜色值

```css
/* Use these in your CSS */
--color-primary: #000000;
--color-secondary: #6B7280;
--color-accent: #22C55E;
--color-bg-light: #FFFFFF;
--color-bg-dark: #0A0A0A;
```

## 图标设计含义

**指南针与代码括号**：
- 指南针环 = 导航，结构化学习路径
- 绿色北针 = 方向，进步，引导
- 黑色南针 = 根基，坚实基础
- `>` 括号 = 终端提示符，代码，CLI 上下文
- 刻度线 = 精确，结构化步骤

这象征着"在清晰的引导下找到代码之路"。

## 各场景推荐用法

### 网站
- **头部**：Logo (`logos/claude-howto-logo.svg`)
- **Favicon**：32px (`favicons/favicon-32.svg`)
- **社交预览**：图标 (`icons/claude-howto-icon.svg`)

### GitHub
- **README 徽章**：图标 (`icons/claude-howto-icon.svg`)，64-128px
- **仓库头像**：图标 (`icons/claude-howto-icon.svg`)

### 社交媒体
- **头像**：图标 (`icons/claude-howto-icon.svg`)
- **横幅**：Logo (`logos/claude-howto-logo.svg`)
- **缩略图**：图标 256×256px

### 文档
- **章节头部**：Logo 或图标（缩放适配）
- **导航图标**：Favicon (32-64px)

---

完整文档请参阅 [README.md](README.md)。
