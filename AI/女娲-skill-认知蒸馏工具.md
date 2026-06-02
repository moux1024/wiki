# 女娲.skill — 认知蒸馏工具：让名人帮你思考

> 来源: https://github.com/alchaincyf/nuwa-skill
> 日期: 2026-06-02
> 分类: AI
> 标签: agent-skills, 认知蒸馏, prompt-engineering, openclaw, claude-code, 心智模型

## 概述

女娲（nuwa-skill）是一个基于 [Agent Skills 协议](https://agentskills.io) 的 AI 认知蒸馏工具。输入一个名字，它自动完成调研、提炼、验证全流程，将名人的**认知操作系统**蒸馏为可被 AI Agent 加载的 Skill 文件。

已蒸馏 12 位人物（乔布斯、马斯克、芒格、费曼、Naval、Paul Graham、Karpathy、Ilya Sutskever 等），生成的 Skill 可在 Claude Code、Codex、Cursor、OpenClaw 等 50+ 兼容 runtime 中运行。

安装：
```bash
npx skills add alchaincyf/nuwa-skill
```

## 核心思想：蒸馏认知操作系统

这不是角色扮演或复读名人语录。女娲提取的是驱动不同判断的**认知框架**——同一个问题，芒格用逆向思维分析，马斯克用渐近极限法分析，乔布斯用「聚焦即说不」分析。

每个蒸馏 Skill 包含五个层次：

| 层次 | 说明 |
|------|------|
| 怎么说话 | 表达 DNA——语气、节奏、用词偏好 |
| 怎么想 | 心智模型、认知框架 |
| 怎么判断 | 决策启发式 |
| 什么不做 | 反模式、价值观底线 |
| 知道局限 | 诚实边界 |

**诚实边界**是亮点：每个 Skill 明确标注蒸馏不了的直觉、捕捉不了的突变、公开表达不等于真实想法。一个不告诉你局限在哪的 Skill 不值得信任。

## 工作原理：四步蒸馏流程

**1. 六路并行采集**——著作、播客/访谈、社交媒体、批评者视角、决策记录、人生时间线，6 个 Agent 同时跑。

**2. 三重验证提炼**——一个观点要被收录为心智模型，必须同时满足：跨 2+ 个领域出现过（有普遍性）、能推断对新问题的立场（有预测力）、不是所有聪明人都会这么想（有排他性）。

**3. 构建 Skill**——3-7 个心智模型 + 5-10 条决策启发式 + 表达 DNA + 价值观与反模式 + 诚实边界，写入 SKILL.md。

**4. 质量验证**——用 3 个此人公开回答过的问题测试方向一致性，再用 1 个他没讨论过的问题测试适度不确定性（不应斩钉截铁）。

## 已蒸馏人物

| 人物 | 领域 |
|------|------|
| Paul Graham | 创业/写作/产品/人生哲学 |
| 张一鸣 | 产品/组织/全球化/人才 |
| Karpathy | AI/工程/教育/开源 |
| Ilya Sutskever | AI 安全/scaling/研究品味 |
| MrBeast | 内容创造/YouTube 方法论 |
| 特朗普 | 谈判/权力/传播/行为预判 |
| 乔布斯 | 产品/设计/战略 |
| 马斯克 | 工程/成本/第一性原理 |
| 芒格 | 投资/多元思维/逆向思考 |
| 费曼 | 学习/教学/科学思维 |
| 纳瓦尔 | 财富/杠杆/人生哲学 |
| 塔勒布 | 风险/反脆弱/不确定性 |

另有主题 Skill：X 导师（X/Twitter 运营全栈）。

## 仓库结构

```
nuwa-skill/
├── SKILL.md                      # 女娲本体
├── references/
│   ├── extraction-framework.md   # 提炼方法论
│   └── skill-template.md         # 生成 Skill 的模板
└── examples/                     # 13 个人物 + 1 个主题
```

## 相关页面

- [[6个让Agent技能稳定生效的模式]] — 女娲生成的 Skill 直接应用这些编写模式：明确的触发条件、指令语气、输出格式、边界声明
- [[Hermes-Agent自我进化AI-Agent指南]] — Hermes 的技能学习机制与女娲的认知蒸馏同属 Agent Skills 协议生态，前者侧重 Agent 自动创建技能，后者侧重将人类认知蒸馏为技能
- [[Hermes-Agent-自改进AI-Agent框架]] — Hermes 的 Skills 自创机制与女娲的认知蒸馏是 Agent Skills 协议的两种不同应用方向
- [[Claude-Code在大型代码库中的最佳实践]] — Claude Code 的 Skills 体系（渐进式披露、按需加载）是女娲 Skill 的运行环境之一
- [[everything-is-a-ralph-loop]] — 女娲的蒸馏流程本身遵循迭代循环范式：采集→验证→构建→验证→迭代

---

*由 knowledge-wiki skill 自动收录并翻译整理*
