# Claudian — Obsidian 中嵌入 AI 编码 Agent 的插件

> 来源: https://github.com/YishenTu/claudian
> 日期: 2026-04-24
> 分类: 工具链
> 标签: Obsidian, Claude Code, Codex, AI Agent, MCP, 插件, 编码助手, 知识管理

![Claudian Preview](https://raw.githubusercontent.com/YishenTu/claudian/main/Preview.png)

一款 Obsidian 插件，将 AI 编码 Agent（Claude Code、Codex 及更多即将支持的后端）直接嵌入你的 Vault 中。你的 Vault 就成为 Agent 的工作目录——文件读写、搜索、bash 命令和多步骤工作流开箱即用。

## 核心功能

### 行内编辑（Inline Edit）

选中文本或从光标位置出发，配合快捷键直接在笔记中进行编辑，支持词级别的 diff 预览。

### 斜杠命令与 Skills

输入 `/` 或 `$` 调用可复用的 prompt 模板，或来自用户级和 Vault 级范围的 Skills。

### @mention

输入 `@` 可以引用任何你希望 Agent 处理的对象：Vault 文件、子 Agent、MCP 服务器，或外部目录中的文件。

### Plan 模式

通过 `Shift+Tab` 切换。Agent 会先探索和设计，再实施，最后呈现一个计划供你审批。

### Instruction 模式（`#`）

从聊天输入框添加精细化的自定义指令。

### MCP 服务器

通过 Model Context Protocol（stdio、SSE、HTTP）连接外部工具。Claude 在应用内管理 Vault MCP；Codex 使用自己的 CLI 管理的 MCP 配置。

### 多标签页与对话管理

支持多个聊天标签页、对话历史、分支（fork）、恢复（resume）和压缩（compact）。

## 系统要求

- **Claude 后端**：需安装 [Claude Code CLI](https://code.claude.com/docs/en/overview)（推荐原生安装）。需要 Claude 订阅/API 或兼容提供商（[Openrouter](https://openrouter.ai/docs/guides/guides/claude-code-integration)、[Kimi](https://platform.moonshot.ai/docs/guide/agent-support) 等）。
- **Codex 后端**（可选）：需安装 [Codex CLI](https://github.com/openai/codex)。
- Obsidian v1.4.5+
- 仅桌面端（macOS、Linux、Windows）

## 安装方式

### 从 GitHub Release 安装（推荐）

1. 从 [最新 Release](https://github.com/YishenTu/claudian/releases/latest) 下载 `main.js`、`manifest.json` 和 `styles.css`
2. 在 Vault 的 plugins 目录中创建 `claudian` 文件夹：`/path/to/vault/.obsidian/plugins/claudian/`
3. 将下载的文件复制到 `claudian` 文件夹
4. 在 Obsidian 中启用插件：设置 → 社区插件 → 启用 "Claudian"

### 使用 BRAT 安装

[BRAT](https://github.com/TfTHacker/obsidian42-brat) 支持从 GitHub 直接安装并自动更新插件。

1. 安装 BRAT 插件
2. 在 BRAT 设置中点击 "Add Beta plugin"
3. 输入仓库地址：`https://github.com/YishenTu/claudian`
4. BRAT 会自动安装并检测更新

### 从源码安装（开发用）

```bash
cd /path/to/vault/.obsidian/plugins
git clone https://github.com/YishenTu/claudian.git
cd claudian
npm install
npm run build
```

## 隐私与数据使用

- **发送到 API**：你的输入、附件、图片和工具调用输出。默认发送至 Anthropic（Claude）或 OpenAI（Codex），可通过环境变量配置。
- **本地存储**：Claudian 设置和会话元数据存储在 `vault/.claudian/`；Claude 后端文件在 `vault/.claude/`；对话记录在 `~/.claude/projects/`（Claude）和 `~/.codex/sessions/`（Codex）。
- **无遥测**：除了你配置的 API 提供商外，没有任何追踪。

## 常见问题

### Claude CLI 找不到

遇到 `spawn claude ENOENT` 或 `Claude CLI not found`，插件无法自动检测 Claude 安装路径。常见于使用 Node 版本管理器（nvm、fnm、volta）的情况。

**解决方案**：找到你的 CLI 路径，在 设置 → 高级 → Claude CLI path 中设置。

| 平台 | 命令 | 示例路径 |
|------|------|---------|
| macOS/Linux | `which claude` | `/Users/you/.volta/bin/claude` |
| Windows (原生) | `where.exe claude` | `C:\Users\you\AppData\Local\Claude\claude.exe` |
| Windows (npm) | `npm root -g` | `{root}\@anthropic-ai\claude-code\cli.js` |

> 注意：Windows 上避免使用 `.cmd` 包装器，请使用 `claude.exe` 或 `cli.js`。

## 项目架构

```
src/
├── main.ts                      # 插件入口
├── app/                         # 共享默认值和插件级存储
├── core/                        # Provider 中立的运行时、注册表和类型契约
│   ├── runtime/                 # ChatRuntime 接口和审批类型
│   ├── providers/               # Provider 注册表和工作区服务
│   ├── security/                # 审批工具
│   └── ...                      # commands, mcp, prompt, storage, tools, types
├── providers/
│   ├── claude/                  # Claude SDK 适配器、prompt 编码、存储、MCP、插件
│   └── codex/                   # Codex app-server 适配器、JSON-RPC 传输、JSONL 历史
├── features/
│   ├── chat/                    # 侧边栏聊天：标签页、控制器、渲染器
│   ├── inline-edit/             # 行内编辑模态框和 Provider 支持的编辑服务
│   └── settings/                # 带 Provider 标签页的设置界面
├── shared/                      # 可复用 UI 组件和模态框
├── i18n/                        # 国际化（10 种语言）
├── utils/                       # 跨领域工具
└── style/                       # 模块化 CSS
```

## 开源协议

MIT License

## 相关页面

- [[Hermes-Agent自我进化AI-Agent指南]]
- [[AmphiLoop-两栖模式构建Agent]]
- [[LLM-Wiki-用LLM构建个人知识库的模式]]

---

*由 knowledge-wiki skill 自动收录并翻译整理*
