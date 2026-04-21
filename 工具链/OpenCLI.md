# OpenCLI

> 来源: https://github.com/jackwener/opencli
> 日期: 2026-04-21
> 分类: 工具链
> 标签: cli, ai-agent, 浏览器自动化, 工具链, npm, electron

## 概述

OpenCLI 是一个通用的 CLI Hub 和 AI 原生运行时，能将任何网站、Electron 应用或本地工具转化为标准化的命令行接口。专为 AI Agent 设计，通过统一的 AGENT.md 集成，让 Agent 能够发现、学习和无缝执行工具。

**核心能力**：
- **网站 → CLI**：90+ 预置适配器，将网站转化为确定性 CLI 命令
- **浏览器自动化**：AI Agent 通过 Chrome 登录会话操作任意网站
- **桌面应用控制**：通过 CDP 驱动 Electron 应用（Cursor、ChatGPT、Notion 等）
- **CLI Hub**：统一发现和代理执行本地工具（gh、docker 等）
- **零 LLM 成本**：运行时不消耗 token，完全确定性输出

## 安装与使用

```bash
# 安装
npm install -g @jackwener/opencli

# 验证
opencli doctor

# 示例：查看热搜
opencli hackernews top --limit 5
opencli bilibili hot --limit 5
```

需要配合 Chrome 浏览器扩展（Browser Bridge Extension）使用。

## 支持的平台（部分）

| 平台 | 主要命令 |
|------|---------|
| 小红书 | search, note, comments, feed, download |
| B站 | hot, search, ranking, download, dynamic |
| Twitter/X | trending, timeline, bookmarks, post |
| Reddit | hot, search, subreddit, comment |
| 知乎 | hot, search, question, answer |
| HackerNews | top, new, show |
| Amazon | bestsellers, search, product |
| Spotify | play, pause, search, queue |
| Gemini | new, ask, deep-research |

完整列表含 90+ 适配器，还支持 YouTube、Pixiv、1688、豆瓣等平台。

## AI Agent 集成

```bash
# 安装 skill 到 AI Agent（Claude Code, Cursor 等）
npx skills add jackwener/opencli
```

提供三个 skill：
- **opencli-adapter-author**：实时操作网站或编写新适配器
- **opencli-autofix**：修复损坏的适配器
- **smart-search**：搜索已有 OpenCLI 能力

## 桌面应用控制

通过 CDP 直接控制 Electron 应用：Cursor IDE、OpenAI Codex、ChatGPT、Notion、Discord 等。

## 输出格式

所有命令支持 `--format` 切换：table（默认）、json、yaml、md、csv。

## 相关技术

- Chrome DevTools Protocol (CDP)
- Browser Extension Bridge
- Electron 应用自动化
- Node.js >= 21

## 相关页面

- [[]]
