---
name: wechat-article-to-md
description: 抓取微信公众号文章并转换为 Markdown 文档。使用脚本自动获取文章标题、作者、正文内容，保留格式并保存为 .md 文件，支持下载图片到本地（按 magic bytes 修正扩展名）、支持表格转 GFM 管道表格、支持识别样式伪装的大标题（一、背景等）为二级标题。适用场景：用户要求抓取/获取/下载/保存微信公众号文章内容为 Markdown 格式，或给出 mp.weixin.qq.com 链接需要提取内容。支持 Obsidian 模式（使用 -obsidian 参数）。
slug: wechat-article-to-md
displayName: 微信公众号文章转 Markdown
version: 1.0.0
summary: 抓取公众号文章转 Markdown，支持表格（含合并单元格展开）、大标题层级、图片下载与格式归一化、Obsidian 模式。
license: MIT
---

# 微信公众号文章转 Markdown

抓取微信公众号文章并将其转换为 Markdown 文档，支持表格、大标题层级、图片下载与格式归一化、Obsidian 模式。

## 依赖

脚本需要 `requests` + `beautifulsoup4`，已装在隔离 venv：

```bash
PY="C:/Users/Jiazi/.workbuddy/binaries/python/envs/default/Scripts/python.exe"
```

**不要用系统 `python3`**——系统 Python 没装 bs4，会报 `ModuleNotFoundError: bs4`。

## 快速使用

### 普通模式（推荐）
```bash
PY="C:/Users/Jiazi/.workbuddy/binaries/python/envs/default/Scripts/python.exe"
"$PY" scripts/wechat_article_to_md.py <文章URL> [输出目录]
```

### Obsidian 模式
```bash
"$PY" scripts/wechat_article_to_md.py <文章URL> [输出目录] -obsidian
```

## 输出内容

脚本自动提取：

- **标题**：文章主标题
- **作者**：公众号作者名称
- **来源**：原文链接
- **正文**：
  - 标题层级（h1-h6）
  - **大标题提升**：识别「一、背景」「二、xxx」「结语」等样式伪装的彩色标题条，提升为 `##` 二级标题
  - **表格 → GFM 管道表格**（含单元格内加粗/斜体/图片引用）
  - 粗体、斜体
  - 列表（有序/无序）
  - 链接
  - 图片（下载到本地 `images/`，**按 magic bytes 修正真实扩展名**，SVG/GIF 伪装不再存错）
  - 代码块、引用块

## 文件命名

输出文件名自动使用文章标题，非法字符替换为下划线。

## 已知问题

1. ~~作者「未知作者」~~（已修复）：作者提取改为多级 fallback（`<meta name="author">` → `og:article:author` → `#js_author_name` → `a.rich_media_meta_link`），新版文章也能正确提取。
2. ~~不支持合并单元格~~（已支持）：含 `colspan`/`rowspan` 的表格自动展开成规则 GFM 表格（跨行列单元格重复填充），对 AI/RAG 友好。
3. **图片目录会被清空**：脚本运行时会清空目标 `images/` 目录里的旧图片（重新下载前清旧图的既定逻辑）。**别把输出目录指向自己存图的地方**。

## 环境坑（Git Bash）

- 用 `$HOME/...`（=`/c/Users`）当输出目录时，Python 会把 `/c/Users` 解析成 `C:\c\Users`（畸形路径）。**请用 Windows 绝对路径（`C:/Users/...`）**。

## 脚本位置

`scripts/wechat_article_to_md.py`
