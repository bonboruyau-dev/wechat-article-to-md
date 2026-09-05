# 微信公众号文章转 Markdown · wechat-article-to-md

> 一键把微信公众号文章抓取为干净的 Markdown：自动提取标题 / 作者 / 来源，下载图片，表格转 GFM，识别「一、背景」等伪装大标题，支持 Obsidian 双链。适合知识库归档、RAG 语料清洗、内容二次创作。
>
> Turn any WeChat Official Account article into clean Markdown in one command.

[English version](#english)

---

## ✨ 功能特性 / Features

- 📄 **自动提取** 标题、作者、来源链接（多级 fallback，新样式文章也能识别）
- 🖼️ **图片下载** 到本地 `images/`，按 magic bytes 修正真实扩展名（SVG/GIF 伪装不再存错）
- 📊 **表格 → GFM 管道表格**，含 `colspan`/`rowspan` 合并单元格自动展开（对 AI / RAG 友好）
- 🅷 **大标题层级还原**：识别「一、背景」「二、xxx」「结语」等样式伪装的彩色标题条，提升为 `##`
- 📝 **完整 Markdown**：标题 / 列表 / 代码块 / 引用 / 粗斜体 / 链接 全部保留
- 🗃️ **Obsidian 模式**：图片引用转 `![[filename.png]]`，适配双链与全文搜索
- 🤖 **跨平台 Agent Skill**：纯 Python 脚本，无框架依赖，WorkBuddy / Claude Code / Codex 均可直接调用

## 🚀 快速开始 / Quick Start

```bash
# 1. 安装依赖（Python 3.10+）
pip install -r requirements.txt

# 2. 抓取文章 → 输出到当前目录（默认 images/ 同目录）
python scripts/wechat_article_to_md.py "https://mp.weixin.qq.com/s/xxxxxx"

# 3. 指定输出目录
python scripts/wechat_article_to_md.py "https://mp.weixin.qq.com/s/xxxxxx" ./output

# 4. Obsidian 模式（图片引用转 ![[...]]）
python scripts/wechat_article_to_md.py "https://mp.weixin.qq.com/s/xxxxxx" ./vault -obsidian
```

> **WorkBuddy 用户**：技能已内置隔离 venv，直接让 Agent 调用即可，无需手动装依赖、无需记忆路径。

## 🧩 跨平台安装 / Multi-Platform Setup

本技能是标准 **Agent Skill**（`SKILL.md` + 纯 Python 脚本），三端通用：

| 平台 | 安装方式 | 依赖 |
|---|---|---|
| **WorkBuddy** | 从 SkillHub 一键导入，或放入 `~/.workbuddy/skills/` | 已内置隔离 venv，无需手动装 |
| **Claude Code** | 放入 `~/.claude/skills/`（全局）或项目 `.claude/skills/` | `pip install -r requirements.txt` |
| **Codex** | 放入 `~/.codex/skills/`（全局）或项目 `.codex/skills/` | `pip install -r requirements.txt` |

脚本零框架依赖，任何能跑 Python 3.10+ 的环境都能执行；`SKILL.md` 遵循 Anthropic Agent Skills 开放格式，三端均可识别。

## 📖 输出示例 / Example

```markdown
# AI Agent 应用精细化评测：评测体系设计与工程实践

**作者**: 砚东
**来源**: https://mp.weixin.qq.com/s/5Tvv8g20CybjbT0a7iUfHw

---

![](images/AI Agent 应用精细化评测_001.png)

### 1.1 从“能用”到“好用”的距离

近年来，大模型驱动的 AI Agent 在各行各业加速落地……
```

## 📂 目录结构 / Structure

```
wechat-article-to-md/
├── SKILL.md                       # 技能元数据 + Agent 调用说明（WorkBuddy 读取）
├── README.md                      # 本文件
├── requirements.txt               # requests + beautifulsoup4
├── LICENSE                        # MIT
└── scripts/
    └── wechat_article_to_md.py    # 主转换器
```

## ⚠️ 注意事项 / Notes

- **图片目录会被清空**：脚本运行时会清空目标 `images/` 下的旧图片（重新下载前清旧图的既定逻辑）。别把输出目录指向自己存图的地方。
- **微信视频**无法直接下载，脚本会在视频位置添加提示标记（到原文观看）。
- **部分文章**可能有访问频率限制，稍后重试即可。
- **合并单元格**：已展开为规则 GFM 网格（跨行列单元格重复填充），便于模型消费。

## 🔗 配套 / Related

- **HTML → 飞书文档（html-to-feishu-doc）**：与本技能串联，形成「抓取公众号 → 落盘飞书」闭环。

## 🤝 贡献 / Contributing

欢迎提交 Issue 与 Pull Request。PR 请保证：

1. `python -m py_compile scripts/*.py` 通过
2. 新增能力补充到 `SKILL.md` 的「输出内容 / 已知问题」
3. README 与 SKILL.md 同步更新

## 📄 许可证 / License

[MIT](./LICENSE)

---

## English

A zero-framework Python script that converts WeChat Official Account articles into clean Markdown.

**Highlights**

- Extracts title, author, source URL
- Downloads images locally, fixes real extension via magic bytes
- Converts tables to GFM (merged cells expanded)
- Promotes styled fake headings (一、背景 … 结语) to `##`
- Preserves headings / lists / code / quotes / bold / links
- Obsidian mode (`-obsidian`) for `![[wikilinks]]`

**Usage**

```bash
pip install -r requirements.txt
python scripts/wechat_article_to_md.py "<article_url>" [output_dir] [-obsidian]
```

**Keywords**: wechat, weixin, official-account, markdown, converter, html-to-markdown, obsidian, rag, knowledge-base, scraper, skill, agent-skills, workbuddy, claude-code, codex
