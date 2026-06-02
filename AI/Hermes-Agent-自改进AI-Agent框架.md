# Hermes Agent — 自改进 AI Agent 框架（架构深度解析）

> 来源: https://deepwiki.com/nousresearch/hermes-agent
> 日期: 2026-05-11
> 分类: AI
> 标签: AI Agent, Agent框架, NousResearch, Hermes, 工具调用, 持久记忆, Skills系统, MCP, ACP

## 概述

Hermes Agent 是 Nous Research 构建的自改进 AI Agent 框架。它提供了稳健的对话循环、工具调用、持久记忆、Agent 自创技能（Skills），以及跨多种界面的部署能力——包括 CLI、消息平台（Telegram、Discord、WhatsApp 等）和通过 Agent Client Protocol (ACP) 的编辑器集成。系统支持任何 OpenAI 兼容的 LLM 提供商，命令可在本地、容器化或云执行环境中运行。

## 三层架构

Hermes Agent 采用**三层架构**，将用户界面、核心 Agent 逻辑和执行后端分离。它通过结构化注册表将"自然语言空间"（用户意图）桥接到"代码实体空间"（工具执行）。

| 层级 | 职责 | 组件 |
|------|------|------|
| 用户界面层 | CLI / Gateway / ACP / Web UI | 多平台适配器 |
| 核心逻辑层 | 对话循环、工具编排、上下文管理、记忆 | AIAgent 类 |
| 执行后端层 | 命令执行环境抽象 | Local / Docker / SSH / Modal / Daytona |

## 运行模式

| 模式 | 入口 | 用途 | 会话持久化 |
|------|------|------|-----------|
| CLI | `cli.py` | 带 TUI 的交互式终端会话 | `~/.hermes/sessions/` |
| Gateway | `gateway/run.py` | 消息平台（Telegram、Discord 等） | `~/.hermes/sessions/` |
| ACP | `acp_adapter/entry.py` | 编辑器集成（VS Code、Zed） | 客户端管理 |
| Web UI | `hermes_cli/web_server.py` | 浏览器仪表盘和聊天 | `~/.hermes/sessions/` |

## 核心组件：AIAgent 类

`run_agent.py` 中的 AIAgent 类是整个系统的编排中心，管理对话循环。

### 五大核心职责

**1. LLM 通信**
- 抽象化与多种 LLM 提供商的交互（OpenRouter、Anthropic、OpenAI）
- 支持 OpenAI 风格和原生 Anthropic 消息 API 协议
- 内置速率限制、重试和退避机制

**2. 工具编排**
- 启动时通过集中式工具注册表（`get_tool_definitions()`）发现工具
- 所有工具调用通过 `handle_function_call()` 统一调度

**3. 上下文管理**
- 动态构建对话上下文
- 使用 `SOUL.md` 中的 persona 和 Skills 系统提示词构建 prompt
- 当 token 用量接近限制时，自动触发 `ContextCompressor` 进行上下文压缩

**4. 会话状态与记忆**
- 对话记录到 SQLite SessionDB
- 集成本地记忆文件（MEMORY.md、USER.md）和通过 Honcho 系统的 AI 原生记忆

**5. 迭代预算**
- `IterationBudget` 机制跨线程/子代理追踪信用用量，防止无限循环

## 工具与环境系统

工具在运行时发现并注册供 LLM 使用。执行环境被抽象化（Local、Docker、SSH、Modal、Daytona 等），在终端设置中配置。

工具系统通过 `model_tools.py` 提供定义，覆盖：
- 终端访问和文件操作
- Web 浏览和视觉工具
- 进程管理和安全/命令审批
- 代码执行和 MCP 工具
- 子代理委派

## 记忆与学习：闭环学习系统

### Skills 系统
Agent 可以**创建新的 Python 工具（Skills）**并通过 `skill_manage` 工具管理它们。这是 Hermes 最独特的特性——Agent 能在完成任务后自动创建技能，并在后续使用中持续改进。

### 持久记忆
- **MEMORY.md**：长期事实存储
- **USER.md**：用户画像
- 由 `agent/memory_manager.py` 管理

### Honcho 集成
提供 AI 原生记忆和用户建模能力，支持跨会话回溯和辨证式查询（dialectic queries），使 Agent 越用越了解用户。

## 配置体系

所有配置位于 HERMES_HOME 目录（默认 `~/.hermes/`）：

| 文件 | 说明 |
|------|------|
| `config.yaml` | 主设置（模型、终端后端、工具集） |
| `.env` | 密钥和 API Key |
| `SOUL.md` | Agent 身份/人格定义 |
| `sessions.db` | SQLite 会话存储（含 FTS5 搜索） |

## 子系统全景

| 子系统 | 说明 |
|--------|------|
| 工具系统 | 工具注册表、工具集、终端/文件操作、进程管理、安全审批、Web/浏览器/视觉工具、代码执行、MCP 工具、子代理委派 |
| 执行环境 | 环境抽象（Local、Docker、SSH、Modal、Daytona） |
| 消息网关 | 网关架构、平台适配器（Telegram、Discord、WhatsApp 等）、会话/媒体管理、安全与配对 |
| Skills 系统 | 技能管理与安全、Skills Hub（社区共享） |
| 批处理 | 批处理运行器、工具集分发、数据生成与轨迹 |
| 高级特性 | 上下文压缩、Provider 运行时解析、Cron/定时任务、诊断工具、RL 训练环境、ACP 服务器与 IDE 集成、插件与记忆提供商 |
| 语音与 TTS | 语音模式、TTS 与转录 |

## 关键设计决策

1. **单体单进程设计**（遵循 Ralph Loop 哲学）——避免多 Agent 交互的非确定性复杂性
2. **Agent 自创并改进工具**——真正的"闭环学习"
3. **任意 OpenAI 兼容 LLM**——不受特定提供商绑定
4. **多执行环境**——本地、容器化、云端灵活切换
5. **SQLite + FTS5 会话持久化**——轻量且高效的全文搜索

## 与已有 Hermes Agent 页面的关系

本篇侧重**系统架构和设计理念**的深度解析（基于 DeepWiki 自动文档化），与 [[Hermes-Agent自我进化AI-Agent指南]]（侧重功能介绍和实用指南）互为补充。建议阅读顺序：先用指南了解功能，再看本篇理解架构。

## 相关页面

- [[Hermes-Agent自我进化AI-Agent指南]] — 同一项目的实用指南，侧重功能介绍、安装配置和实战案例
- [[AmphiLoop-两栖模式构建Agent]] — 同为 Agent 框架，Hermes 侧重单体闭环自我进化，AmphiLoop 侧重 Workflow/Agent 两栖模式切换
- [[Claudian-Obsidian中嵌入AI编码Agent的插件]] — 将 Claude Code/Codex 等 Agent 嵌入 Obsidian，与 Hermes 的 ACP 编辑器集成思路相似
- [[everything-is-a-ralph-loop]] — Hermes 的单体单进程设计直接受 Ralph Loop 哲学影响
- [[6个让Agent技能稳定生效的模式]] — Hermes 的 Skills 自创机制是这些技能编写模式的进阶实践
- [[LLM-Wiki-用LLM构建个人知识库的模式]] — Hermes 的 MEMORY.md + Honcho 记忆系统与 LLM Wiki 的知识管理思想相通
- [[卡片式对话的协议方案探索和思考]] — Hermes 的 ACP 适配器与卡片式交互协议同属 Agent 交互界面设计领域
- [[Claude-Code在大型代码库中的最佳实践]] — Claude Code 的 Skills/Plugins/LSP/Subagent 体系与 Hermes 的 Skills 自创机制同属 Agent 工具化设计，两篇可互为参考
- [[女娲-skill-认知蒸馏工具]] — 同属 Agent Skills 协议生态，女娲将人类认知蒸馏为 Skill，Hermes 让 Agent 自动创建 Skill，两者方向不同

---

*由 knowledge-wiki skill 自动收录并翻译整理*
