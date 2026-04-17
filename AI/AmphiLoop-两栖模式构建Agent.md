# 万字长文！两栖模式构建Agent，与OpenClaw/Hermes不一样的解法——开源AmphiLoop

> 来源: https://mp.weixin.qq.com/s/KKHWWzJqeKR1fF6EsXheLQ
> 日期: 2026-04-17
> 分类: AI
> 标签: Agent, Amphiflow, Workflow, 两栖模式, 自动化, Claude Code, Bridgic, AmphiLoop

## 概述

AmphiLoop（Amphibious Loop，两栖循环）是一套全新的 AI 智能体构建方法论、技术栈和工具链，由张铁蕾开源。核心理念是**将 Workflow 的确定性与 Agent 的灵活性结合**，通过运行时自动切换两种模式来构建稳定高效的自动化任务。

GitHub: https://github.com/bitsky-tech/AmphiLoop/

## 核心问题分析

### AI 的不确定性困境

- AI 技术基于概率，充满不确定性
- 错误累积效应导致长程任务成功率骤降
- 当前 Agent（龙虾）模式存在三大痛点：**安全**、**稳定**、**成本**

### 任务分类

| 类型 | 特点 | 适合模式 |
|------|------|---------|
| 确定性任务 | 步骤清晰、结果可预测 | Workflow |
| 目标明确但路径不清晰 | 知道要什么，但怎么做需要探索 | Agent |
| 目标大致明确 | 需要多次迭代明确需求 | Agent |
| 混合型任务 | 部分确定、部分不确定 | **两栖模式** |

## 两种执行模式

### Workflow 模式

- **确定性代码驱动**
- 优势：稳定、省 token、可预测
- 劣势：不灵活，遇到异常就卡住

### Agent 模式

- **LLM 驱动的 agentic loop**
- 优势：灵活、能自主决策
- 劣势：不确定性、token 消耗大、可能出错

## AmphiLoop 核心方案

### TASK.md — 任务的自然语言描述

- 用自然语言（而非代码）描述任务
- 可维护、可迭代、人类可读
- 作为代码生成的输入源

### 「探路 - 编码 - 验证」循环

1. **探路**：Agent 模式探索任务执行路径
2. **编码**：将探索成果固化为确定性 Workflow 代码
3. **验证**：验证产物正确性
4. 循环迭代直到任务完成

### 产物：双模式并存

- 产物同时包含 **Workflow 模式** 和 **Agent 模式**
- 正常情况走 Workflow（快、省、稳）
- 出错时自动切换到 Agent 模式自主决策

### Amphiflow 程序

运行时基于环境变化**自动切换模式**：
- 正常路径 → Workflow 模式执行
- 遇到异常/未知情况 → 自动切换 Agent 模式
- Agent 解决问题后 → 回到 Workflow 模式

## 设计思想

1. **善用自主性，隔离随机性，获得确定性** — AI 的自主性用在探索阶段，执行阶段保持确定性
2. **决策（Decision）与执行（Execution）解耦** — 探索决策和稳定执行是两个独立过程
3. **可管理的自然语言任务描述** — TASK.md 作为人机协作的桥梁
4. **理论上最省 token 的 AI 方案** — 大部分时间走 Workflow，仅在异常时调用 LLM

## 技术栈

| 组件 | 功能 |
|------|------|
| bridgic-amphibious | 两栖特性（Workflow ↔ Agent 切换） |
| bridgic-core | 底层编排引擎 |
| bridgic-browser | 浏览器自动化 |

## 与同类方案的区别

### vs OpenClaw

- **OpenClaw**：适合即时性任务，实时响应、对话驱动
- **AmphiLoop**：适合长程、routine 执行的自动化任务

### vs Hermes Agent

- **Hermes**：侧重自我进化、技能学习、跨会话记忆
- **AmphiLoop**：侧重模式切换、确定性执行、任务自动化

## 相关页面

- [[Hermes-Agent自我进化AI-Agent指南]] — 同为 Agent 框架，但设计理念不同：Hermes 侧重自我进化，AmphiLoop 侧重两栖模式切换
- [[LLM-Wiki-用LLM构建个人知识库的模式]] — TASK.md 的自然语言任务描述与 LLM Wiki 的知识管理思想有相通之处

---

*由 knowledge-wiki skill 自动收录整理*
