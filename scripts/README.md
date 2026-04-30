<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# EPUB 构建脚本

从 Claude How-To markdown 文件构建 EPUB 电子书。

## 功能特性

- 按文件夹结构组织章节（01-slash-commands、02-memory 等）
- 通过 Kroki.io API 将 Mermaid 图表渲染为 PNG 图片
- 异步并发获取 - 并行渲染所有图表
- 从项目 logo 生成封面图片
- 将内部 markdown 链接转换为 EPUB 章节引用
- 严格错误模式 - 任何图表无法渲染时构建失败

## 环境要求

- Python 3.10+
- [uv](https://github.com/astral-sh/uv)
- 用于 Mermaid 图表渲染的网络连接

## 快速开始

```bash
# Simplest way - uv handles everything
uv run scripts/build_epub.py
```

## 开发环境设置

```bash
# Create virtual environment
uv venv

# Activate and install dependencies
source .venv/bin/activate
uv pip install -r requirements-dev.txt

# Run tests
pytest scripts/tests/ -v

# Run the script
python scripts/build_epub.py
```

## 命令行选项

```
usage: build_epub.py [-h] [--root ROOT] [--output OUTPUT] [--verbose]
                     [--timeout TIMEOUT] [--max-concurrent MAX_CONCURRENT]

options:
  -h, --help            show this help message and exit
  --root, -r ROOT       Root directory (default: repo root)
  --output, -o OUTPUT   Output path (default: claude-howto-guide.epub)
  --verbose, -v         Enable verbose logging
  --timeout TIMEOUT     API timeout in seconds (default: 30)
  --max-concurrent N    Max concurrent requests (default: 10)
```

## 示例

```bash
# Build with verbose output
uv run scripts/build_epub.py --verbose

# Custom output location
uv run scripts/build_epub.py --output ~/Desktop/claude-guide.epub

# Limit concurrent requests (if rate-limited)
uv run scripts/build_epub.py --max-concurrent 5
```

## 输出

在仓库根目录创建 `claude-howto-guide.epub`。

EPUB 包含：
- 带项目 logo 的封面图片
- 带嵌套章节的目录
- 所有 markdown 内容转换为 EPUB 兼容的 HTML
- Mermaid 图表渲染为 PNG 图片

## 运行测试

```bash
# With virtual environment
source .venv/bin/activate
pytest scripts/tests/ -v

# Or with uv directly
uv run --with pytest --with pytest-asyncio \
    --with ebooklib --with markdown --with beautifulsoup4 \
    --with httpx --with pillow --with tenacity \
    pytest scripts/tests/ -v
```

## 依赖

通过 PEP 723 内联脚本元数据管理：

| 包 | 用途 |
|---|------|
| `ebooklib` | EPUB 生成 |
| `markdown` | Markdown 到 HTML 转换 |
| `beautifulsoup4` | HTML 解析 |
| `httpx` | 异步 HTTP 客户端 |
| `pillow` | 封面图片生成 |
| `tenacity` | 重试逻辑 |

## 故障排除

**构建因网络错误失败**：检查网络连接和 Kroki.io 状态。尝试 `--timeout 60`。

**速率限制**：使用 `--max-concurrent 3` 减少并发请求数。

**缺少 logo**：如果找不到 `claude-howto-logo.png`，脚本会生成纯文本封面。
