# 5分钟学习Hermes-Agent指南

> 来源: https://mp.weixin.qq.com/s?__biz=MzI1NzUwOTY4OA==&mid=2247485104&idx=1&sn=c53bb1971e67a3499b73ac436dbb6a9d
> 日期: 2026-04-10
> 分类: AI
> 标签: Hermes Agent, Nous Research, AI Agent, 自我进化, Skills, MCP, 多平台, 记忆系统

## 一、什么是 Hermes Agent？

Hermes Agent 是由 **Nous Research** 打造的自主 AI Agent，核心理念是**闭环学习**（Closed Learning Loop）：
- 从经验中自动创建 Skills
- 在使用中持续改进技能
- 主动提醒自己持久化知识
- 搜索过往对话获取上下文
- 跨会话不断深化用户了解

### 与同类工具的区别

| 对比项 | Hermes Agent | 典型编码副手 | 聊天机器人 |
|--------|-------------|-------------|-----------|
| 运行位置 | 任意服务器/本地 | 绑定 IDE | 网页/App |
| 学习能力 | 自动创建并改进技能 | 无 | 无 |
| 记忆系统 | 跨会话持久化 | 仅当前会话 | 有限 |
| 多平台 | 15+ 平台 | 仅 IDE | 单一平台 |
| 模型选择 | 完全自由 | 固定 | 固定 |

## 二、核心特性

| 特性 | 说明 |
|------|------|
| 自我进化学习循环 | 自动创建技能、自我改进、跨会话记忆、用户建模 |
| 多平台接入 | CLI、Telegram、Discord、Slack、WhatsApp、Signal 等 15+ 平台 |
| 多模型支持 | Nous Portal、OpenRouter、OpenAI、Anthropic、DeepSeek、通义千问、Kimi 等 |
| 多环境部署 | 本地、Docker、SSH、Daytona、Modal、Singularity 六大后端 |
| 任务调度 | 内置 Cron 调度器，支持自然语言定义定时任务 |
| 子代理委派 | 可并行产生最多 3 个子代理处理复杂任务 |
| MCP 集成 | 连接任意 MCP 服务器扩展工具能力 |
| 语音交互 | CLI 麦克风输入、TTS 语音回复、Discord 语音频道 |

## 三、安装与配置

### 安装
```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
source ~/.bashrc  # 或 source ~/.zshrc
hermes doctor      # 验证安装
```

系统要求：Linux（Ubuntu 20.04+/CentOS 8+/Debian 11+）、macOS 12+、Windows 仅 WSL2。

### 配置
```bash
hermes setup   # 全功能配置向导
hermes model   # 随时切换模型提供商
```

### 支持的模型提供商

| 提供商 | 说明 | 配置方式 |
|--------|------|---------|
| Nous Portal | 官方订阅制，零配置 | OAuth 登录 |
| OpenRouter | 200+ 模型路由 | API Key |
| OpenAI | GPT 系列 | API Key |
| Anthropic | Claude 系列 | API Key / Claude Code 认证 |
| DeepSeek | DeepSeek 系列 | API Key |
| 通义千问 | Qwen 系列 | API Key |
| Kimi/Moonshot | Moonshot 系列 | API Key |
| Hugging Face | 20+ 开源模型 | HF Token |
| GitHub Copilot | GPT-5.x、Claude、Gemini 等 | OAuth |
| 自定义端点 | VLLM、SGLang、Ollama 等 | Base URL + API Key |

## 四、常用命令

### CLI 交互
```bash
hermes                  # 启动交互式 CLI
hermes --continue       # 恢复上次会话
hermes model            # 切换模型
hermes tools            # 管理工具
hermes setup            # 配置向导
hermes doctor           # 诊断问题
hermes update           # 更新版本
```

### 对话内斜杠命令
| 命令 | 功能 |
|------|------|
| `/help` | 显示所有可用命令 |
| `/tools` | 查看当前可用工具 |
| `/model` | 交互式切换模型 |
| `/skills` | 浏览可用技能 |
| `/save` | 保存当前对话 |
| `/new` | 开始新对话 |
| `/retry` | 重试上一次回复 |
| `/undo` | 撤销上一次操作 |
| `/compress` | 压缩当前上下文 |
| `/usage` | 查看 Token 用量 |
| `/insights` | 查看使用洞察 |
| `/personality pirate` | 切换人格（如海盗人格） |

## 五、核心功能详解

### 5.1 工具系统（Tools & Toolsets）
内置 **48 个工具**，分属 40 个工具集，覆盖 Web 搜索、终端执行、文件编辑、记忆管理、子代理委派等。可按平台单独启用/禁用。

### 5.2 技能系统（Skills System）
最独特的功能——按需加载的知识文档，Agent 完成复杂任务后自动创建技能并持续改进。兼容 **agentskills.io** 开放标准。
```bash
hermes skills search kubernetes      # 搜索技能
hermes skills install openai/skills/k8s  # 安装技能
```

### 5.3 记忆系统（Persistent Memory）
- **MEMORY.md**：Agent 学到的知识、经验和工作成果
- **USER.md**：用户偏好、项目、环境等个人化信息
- **FTS5 全文搜索**：搜索过往会话，配合 LLM 摘要
- **Honcho 用户建模**：辨证式建模，越用越懂你

### 5.4 上下文文件
自动发现项目下的 `.hermes.md`、`AGENTS.md`、`CLAUDE.md`、`SOUL.md`、`.cursorrules` 等，塑造 Agent 在项目中的行为。

### 5.5 检查点与回滚
修改文件前自动快照工作目录，出错可用 `/rollback` 安全回滚。

## 六、高级功能

### 6.1 消息平台网关（Gateway）
支持平台：Telegram、Discord、Slack、WhatsApp、Signal、Matrix、Mattermost、Email、SMS、钉钉、飞书、企业微信、BlueBubbles、Home Assistant、Webhook。
```bash
hermes gateway setup   # 交互式配置平台
hermes gateway start   # 启动网关
```

### 6.2 定时任务（Cron）
支持自然语言定义，如"每天早上 9 点检查 Hacker News 的 AI 新闻并通过 Telegram 发送摘要"。

### 6.3 子代理委派
产生独立子代理实例（最多 3 个并行），每个拥有独立上下文、受限工具集和独立终端会话。

### 6.4 代码执行
`execute_code` 工具允许 Agent 编写 Python 脚本并通过 RPC 调用 Hermes 工具，将多步骤工作流压缩为单次 LLM 调用。

### 6.5 MCP 集成
通过 stdio 或 HTTP 连接任意 MCP 服务器：
```yaml
# ~/.hermes/config.yaml
mcp_servers:
  github:
    command: npx
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "ghp_xxx"
```

### 6.6 语音模式
```bash
pip install "hermes-agent[voice]"
pip install faster-whisper  # 可选本地语音识别
```
支持 Edge TTS（免费）、ElevenLabs、OpenAI TTS、MiniMax、NeuTTS 五种提供商。

### 6.7 浏览器自动化
支持 Browserbase 云、Browser Use 云、本地 Chrome CDP、本地 Chromium。

## 七、系统架构

### 分层架构
| 层级 | 核心组件 | 说明 |
|------|---------|------|
| 入口层 | CLI / Gateway / ACP / Batch | 多种交互入口 |
| 核心层 | AIAgent (run_agent.py) | Prompt 构建、Provider 解析、工具分发 |
| 工具层 | 48 工具 + 6 终端后端 | 终端、浏览器、Web、MCP、文件等 |
| 存储层 | SQLite + FTS5 | 会话存储、全文搜索 |

### 数据流程
1. 用户通过 CLI 或消息平台发送消息
2. AIAgent 通过 Prompt Builder 组装系统提示词（含记忆、上下文、技能等）
3. Provider Resolution 解析到对应 LLM 提供商并发送请求
4. LLM 返回结果，如需调用工具则通过 Tool Dispatch 执行
5. 工具结果回传给 LLM，循环直到最终回复
6. 结果返回用户，同时更新记忆

### 关键目录
| 目录/文件 | 说明 |
|----------|------|
| `run_agent.py` | 核心对话循环（~9,200 行） |
| `cli.py` | 交互式终端 UI（~8,500 行） |
| `model_tools.py` | 工具发现、Schema 收集、分发 |
| `agent/` | 提示词构建、上下文压缩、缓存 |
| `tools/` | 工具实现，每个工具一个文件 |
| `gateway/` | 消息平台网关，15 个适配器 |
| `skills/` | 内置技能 |
| `cron/` | 任务调度器 |
| `tests/` | 3,000+ 测试用例 |

## 八、部署方案

| 后端 | 说明 | 适用场景 |
|------|------|---------|
| Local | 本地直接执行 | 开发测试 |
| Docker | 容器化隔离 | 安全沙箱环境 |
| SSH | 远程服务器 | 云端开发 |
| Daytona | Serverless 持久化 | 按需休眠，接近零成本 |
| Modal | Serverless GPU | 需要 GPU 的任务 |
| Singularity | HPC 容器 | 研究/超算环境 |

```bash
hermes config set terminal.backend docker  # 切换后端
```

可部署在 $5/月 VPS 上，Daytona/Modal 后端空闲时自动休眠。

## 九、安全机制

- **命令审批**：危险命令执行前请求确认
- **DM 配对授权**：消息平台用户需授权才能交互
- **容器隔离**：Docker/Singularity 沙箱
- **凭证池**：多 API Key 自动轮转，避免限流
- **检查点回滚**：文件修改前自动快照

## 十、实战案例

### 日报自动化
配置 Telegram Gateway → 启动 → 自然语言定义 Cron 任务 → 每日自动执行

### 多任务并行
直接告诉 Hermes 同时研究多个产品，自动产生 3 个子代理并行执行并汇总。

### 项目上下文
在项目根目录创建 `.hermes.md` 写入规范，Agent 自动加载。

### MCP 集成 GitHub
配置 MCP 后可通过自然语言创建 Issue、查看 PR、管理仓库。

## 相关页面

- [[LLM-Wiki-用LLM构建个人知识库的模式]]

---

*由 knowledge-wiki skill 自动收录整理*
