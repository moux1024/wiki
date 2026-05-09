# 大厂打包了 harness，sandbox 公司快死了

> 来源: http://xhslink.com/o/Aan5SY0DE6T (小红书)
> 日期: 2026-04-28
> 分类: AI
> 标签: Agent, Anthropic, OpenAI, Sandbox, Agent Infra, 创业

2026 年 4 月 8 日和 15 日，Anthropic 发了 Managed Agents，OpenAI Agents SDK 加了 9 个 sandbox provider。这一周 agent infra 的接口定义权基本被两家拿走。

## 背景

我硅谷一个做 agent infra 的朋友，3 月前就从这个赛道撤退了，现在还在探索下一步方向。当时他给我的理由很短：

> agent 是大厂头部集中的游戏。

我当时没太理解。直到 4 月初这两周，Anthropic 和 OpenAI 一周内出招，我看明白他说的事。

## 4 月这两周到底发生了什么

**2026 年 2 月：** OpenAI 给 Responses API 加了 `container_auto`，托管 Debian 12 容器，预装 Python / Node / Java / Go / Ruby。

**4 月 8 日：** Anthropic 发布 **Managed Agents** = 把 brain / harness / sandbox 三件套全托管。

**4 月 15 日：** OpenAI Agents SDK 加 native sandbox harness，一口气接入 9 个 sandbox provider，再配 Codex 风格的 filesystem tools 和 `apply_patch`。

两家做了同一件事：**把 agent harness 这一层全部打包。**

## "打包"长这样

OpenAI 自己画的"打包"动作对比图：

**左边（你自己拼 agent）：**
- Tool integrations
- Components
- Agent loop
- 你自己写 SDK、接 tool、做 sandbox

**右边（SDK 包圆）：**
- Built-in Tools
- Agent Loop
- Tool Registry
- 开发者只剩配置

### Anthropic 的方案：把整套托管掉

Managed Agents 把一个 agent 拆成三个抽象：

1. **Session**：一条 append-only 的事件日志，所有发生过的事都记在这。
2. **Harness**：调 Claude 加路由 tool call 的循环。
3. **Sandbox**：执行环境，统一接口 `execute(name, input) → string`，下面挂容器、手机、模拟器都行。

**关键变化在架构：**

旧版本三件套塞在一个容器里，需要小心维护，挂掉一个 session 全没。新版本 harness 把 sandbox 当 tool 调，容器死了就是一次 tool call 报错，Claude 自己决定要不要重试、挂了重新起一个。

**代价收益：** P50 TTFT 降 60%，P95 降 90%。

原因很简单：旧架构每个 session 都付完整容器启动开销；新架构 inference 立刻起跑，容器按需 Provision。

> "We're opinionated about the shape of these interfaces, not about what runs behind them."

只定义接口，下面跑什么由他们随时换。

### OpenAI 的方案：把 sandbox 给别人卷

2 月的 `container_auto` 已经把"hosted Debian 12 容器"这个能力做出来。后来又加了 Agent Skills（SKILL.md manifest 加文件 bundle）和完整 shell tool。

4 月 15 日的 Agents SDK 升级是这条路线的关键一步：**9 家 sandbox provider 全部接入 SDK：**

- Unix-local / Docker
- Blaxel / Cloudflare / Daytona
- E2B / Modal / Runloop / Vercel

要本地、要云、要哪家都行，OpenAI 不挑。

表面看是开放，实际是 **commoditize** = 9 家 partner 互相卷价格，OpenAI 只收 token 钱。

**架构对比：**

- **旧架构**：harness / agent / tools / filesystem 全塞 sandbox 一个容器
- **新架构**：harness 从 compute 拆出来

OpenAI 列出 5 家 sandbox（E2B / Cloudflare / Vercel / Modal + 自家），Secrets 留在 harness 侧，sandbox 拿不到。Runs anywhere: Temporal / AWS / Azure。

## 包圆 vs 抽水：二十年前的剧本

Joel Spolsky 2002 年写过一篇 **Strategy Letter V**，核心一句话：

> **互补品越商品化，主品越值钱。**

- 微软当年把 PC 硬件商品化（让 OEM 互相卷），Windows 利润最大化。
- IBM 推 Linux 让操作系统商品化，自己卖咨询和服务器。
- **OpenAI 用同样的剧本：主品 = token，互补品 = sandbox。** 把 sandbox 卷成商品，token 才更值钱。

**Anthropic 走包圆路线：** brain + harness + sandbox 全部我来跑，垂直整合换 latency 和集成度。

他们做的是同一件事：**抢 agent infra 的接口定义权。** 谁定义了 session、harness、sandbox 这三个抽象，谁就拥有下一代 agent 应用的入口。

## 为什么 sandbox 公司可能撑不过 18 个月

18 个月这个时间推断有三个原因：

1. **降低延迟需要垂直整合。** Lab 把 inference 和 sandbox 放在同一张网里能拿到 P50 -60%、P95 -90%。E2B 在 AWS、Modal 在自己机房，跨网调用永远追不上。

2. **经济模型只有 lab 能玩。** Lab 用 token margin 补贴 runtime，第三方 sandbox 必须直接收容器钱。一份 token 的钱 vs token 加 sandbox 双份钱，价格永远不可能赢。

3. **Distribution。** Claude Code / Codex CLI 默认用自家 runtime。开发者第一次接触就被锁定。

三个加在一起，**E2B / Modal / Daytona / Runloop / Blaxel 大概率撑不过 18 个月。** 最好的出路是被 lab 收编做 acquihire。

## 分层来看

**受冲击：** sandbox-as-a-service（E2B / Modal / Daytona / Runloop / Blaxel），他们卖的就是被 lab 内置的能力。

**不受影响：** AI workflow（Dify / hAI8h 等），在更高的业务编排层，agent runtime 商品化对他们是利好。

**有空间的几条：**

- **Multi-model framework**：OpenAI 和 Anthropic 必然分裂，"跨 GPT 和 Claude"是真需求
- **企业部署 + 安全合规**：SOC2 不是 lab 特权
- **垂直应用**：法律/医疗/金融 agent，比如澳洲跑出来的诊所 AI 助手 Heidi
- **本地部署 / 隐私敏感场景**

## 创业公司应该站哪一层

Agent infra 这一战已经是大厂游戏，没有创业公司的位置。

真正的空白市场在 **workflow 那一层**——Dify / hAI8h 那种把 agent 能力包成业务流程的公司，护城河是场景理解和数据集成，不是 runtime 能力。

给中小企业落地 AI 时，客户要的从来不是"用了什么 sandbox"，是"我这个流程能不能省人"。理解场景和业务比理解 infra 重要得多。

> Agent infra 越商品化，做 workflow 的人越值钱。

## 参考资料

- [Anthropic Engineering — Managed Agents (2026-04-08)](https://www.anthropic.com/engineering/managed-agents)
- [OpenAI Engineering — Equipping the Responses API with a computer environment (2026-02)](https://openai.com/index/equip-responses-api-computer-environment)
- [VentureBeat — OpenAI Agents SDK + agent skills + complete terminal shell (2026-04-15)](https://venturebeat.com/ai/openai-agents-sdk/)
- [Joel Spolsky — Strategy Letter V: Commoditize Your Complement (2002)](https://www.joelonsoftware.com/2002/06/12/strategy-letter-v/)

## 相关页面

- [[AmphiLoop — 两栖模式构建Agent]]

---

*由 knowledge-wiki skill 自动收录整理（原文为小红书截图，经 OCR 提取并校正）*
