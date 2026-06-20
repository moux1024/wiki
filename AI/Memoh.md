# Memoh — 为每个 AI Agent 提供专属云计算机

> 来源: https://github.com/memohai/Memoh
> 日期: 2026-06-20
> 分类: AI
> 标签: Agent, 多Agent平台, 容器化, 长期记忆, MCP, Browser Use, Computer Use, 自托管, Go

## 概述

Memoh 是一个开源的多 Agent 平台。每个 Agent 都拥有自己的云计算机——一个专用容器，配备文件系统、桌面环境、浏览器、网络和长期记忆。即使你的笔记本电脑合上，Agent 仍然保持在线 24/7 运行。

支持通过 Telegram、Discord、Lark、微信、Web UI 等渠道与 Agent 交互。Agent 能够跨会话和平台记忆上下文，驱动浏览器，调用 MCP 工具，并运行定时任务。可以为自己运行一个，为每个团队成员分配一个，或者批量部署。

## 快速开始

### Memoh Cloud
即将上线——零配置的云端 Agent。

### 部署到服务器
在自有基础设施上自托管完整技术栈：

```bash
curl -fsSL https://memoh.sh | sh
```

### 桌面客户端
支持 macOS、Windows 和 Linux 的原生客户端。

## 核心优势

- **每个 Agent 一台计算机**：独立容器，拥有自己的文件系统、网络、桌面和浏览器
- **多用户、多机器人**：可以为自己运行一个，为每个家庭成员部署一个，或在一台机器上运行集群
- **轻量化**：可在边缘设备上运行。推理在云端，数据留在本地

## 功能特性

### 核心功能
- **多机器人 & 多用户**：多个机器人可私聊、群聊或互相通信，支持跨平台身份绑定
- **容器化工作区**：每个机器人在独立容器中运行，拥有专属文件系统、网络、工具和桌面
- **内置记忆**：开箱即用的跨会话、跨平台长期记忆，也支持 Mem0、OpenViking 作为替代方案
- **10+ 渠道**：支持 Telegram、Discord、Lark、微信、QQ、Email 等

### Agent 能力
- **MCP**：连接外部工具服务器，每个机器人独立管理自己的连接
- **Browser Use**：在容器内驱动浏览器
- **Computer Use**：操作容器桌面，执行 GUI 工作流
- **Skills & Supermarket**：模块化技能系统，可从 Supermarket 安装精选模板，支持委派给子 Agent
- **Automation**：定时任务和周期性心跳检测

### 记忆系统
内置完全自托管的记忆引擎。每个机器人都能记住你告诉过它的事情，跨会话、跨天、跨平台。也支持 Mem0 和 OpenViking 作为即插即用的替代方案。

## 子项目

- **Twilight AI** — 一个轻量级、地道的 Go 语言 AI SDK，灵感来自 Vercel AI SDK。Provider 无关（支持 OpenAI、Anthropic、Google），拥有一流的 streaming、tool calling、MCP 和 embeddings 支持。

## 许可证

AGPLv3

## 相关链接

- 官网: https://memoh.ai
- 文档: https://docs.memoh.ai
- Telegram: https://t.me/memohai
- Supermarket: https://github.com/memohai/supermarket

## 相关页面

- [[AmphiLoop — 两栖模式构建Agent]] — 同为 Agent 平台框架，Memoh 侧重容器化多 Agent 部署，AmphiLoop 侧重 Workflow/Agent 两栖模式切换
- [[Hermes-Agent — 自改进AI Agent框架]] — 同为 Agent 框架，Hermes 侧重单体闭环自我进化，Memoh 侧重多 Agent 容器化部署
- [[Hermes-Agent自我进化AI-Agent指南]] — Hermes 的技能学习机制与 Memoh 的 Skills & Supermarket 模块化技能系统思路相似
- [[LLM Wiki — 用LLM构建个人知识库的模式]] — Memoh 内置长期记忆引擎，与 LLM Wiki 知识管理模式互补
- [[Claudian-Obsidian中嵌入AI编码Agent的插件]] — 同为将 Agent 嵌入特定环境的方案，Claudian 嵌入 Obsidian，Memoh 嵌入容器
- [[大厂打包了 harness，sandbox 公司快死了]] — Memoh 的容器化 Agent 工作区是 sandbox 模式的实践之一

---

*由 knowledge-wiki skill 自动收录并翻译整理*
