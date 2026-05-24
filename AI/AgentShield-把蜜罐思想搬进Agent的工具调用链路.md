# AgentShield：把蜜罐思想搬进 Agent 的工具调用链路

> 来源: https://mp.weixin.qq.com/s/Wvx6x4KzlI0yJYuP7ID7mg
> 日期: 2026-05-24
> 分类: AI
> 标签: Agent安全, 提示注入, 蜜罐, 工具调用, compromise detection, LLM安全

## 概述

论文《AgentShield: Deception-based Compromise Detection for Tool-using LLM Agents》（https://arxiv.org/pdf/2605.11026）提出了一种面向工具型 LLM Agent 的欺骗式 compromise detection 框架。核心思路：不试图在输入阶段识别恶意提示词，而是把 Agent 的工具调用链路变成一个"诱捕现场"，在 Agent 真正被攻击带偏后、通过工具调用行为发现异常。

## Agent 安全从"说错话"到"做错事"

间接提示注入的本质是攻击者把恶意指令藏在 Agent 会读取的外部内容（邮件、网页、文档、数据库记录）里。工具型 Agent 一旦被带偏，可能真的去发邮件、查数据库、改密码、转账——风险从"内容风险"变成了"行为风险"。

AgentShield 的核心判断：不要只盯着模型读到了什么，而要看它最后准备做什么。输入文本可以被伪装、换语言、混入 Unicode，但工具调用动作是最值得监控的安全边界。

## 三层陷阱设计

### 第一层：Honeytools（假工具）

把看起来对攻击者有吸引力、但正常任务绝不应调用的工具放进 Agent 工具列表，例如 `export_all_user_data`、`exfiltrate_data_to_attacker_server`、`system_admin_console`。被调用时不会造成危害，只返回错误或触发告警。假工具名称必须足够"明显恶意"，正常 Agent 没有理由误调用。

### 第二层：Honeytokens（假凭据）

在环境中放置假的 API key、管理员密码、数据库连接串、内部 URL、SSH key 路径等。检查所有外发工具调用参数是否包含假凭据。若出现，说明 Agent 可能被诱导窃取和外传敏感信息。

### 第三层：Parameter Validator（参数白名单）

检查 Agent 传给工具的参数是否在允许范围内。银行转账只能转给已知账户，邮件只能发到允许域名，URL 必须匹配白名单。超出范围即触发告警。最接近真实工程的策略控制，但依赖业务配置。

**性能开销**：三层机制加起来每次工具调用增加不到 50ms，总体开销低于 1%，不需要额外调用 LLM。

## 实验设计亮点

- 使用 AgentDojo v1.2.2 评测基准，覆盖 banking、messaging、travel、workspace 四类 Agent 环境
- 测试模型：GPT-4o-mini、GPT-5-mini、Llama 3.3 70B-Instruct、DeepSeek-V3
- **176 条跨语言攻击 prompt**，覆盖英文、库尔德语、阿拉伯语及混合语言
- 攻击类型：目标劫持、数据外传、凭据窃取、社会工程、音译、零宽 Unicode、同形字符替换

跨语言设计重要，因为很多提示注入防御在英文上还行，切到低资源语言或特殊字符变体就崩溃。AgentShield 回避了这个问题——它不直接理解攻击文本，而是观察 Agent 后续的工具调用行为。

## 实验结果

### 检测效果

- 原始检测率 25.8%~36.5%（单看不高，因为很多攻击被模型自身拒绝，没有行为可检测）
- **关键：只看 Agent 真正服从攻击指令的成功攻击，商业模型捕获率 90.7%~100%**
  - GPT-4o-mini：117/129 = 90.7%
  - GPT-5-mini：125/125 = 100%
- GPT-4o-mini 485 次正常任务运行零误报（Wilson 95% CI: 0%~0.79%）

### 不同模型触发不同陷阱层

- GPT-4o-mini：参数校验贡献最大
- GPT-5-mini：假工具信号更明显
- Llama 3.3：基础攻击成功率仅 1.8%，样本不足
- DeepSeek-V3：三层都有贡献

**结论**：不能指望单一检测信号在所有模型上稳定有效，必须做多层诱捕。

### 自适应攻击测试

攻击者完全知道 AgentShield 机制，48 条自适应攻击，4 模型 × 3 语言 = 1,728 次运行：

- **商业模型零逃逸**：GPT-4o-mini 43 次成功攻击全被检测，GPT-5-mini 攻击前全部拒绝
- 开源模型少量逃逸：Llama 3.3 有 5 次逃逸（银行账号加空格绕过精确匹配），DeepSeek-V3 有 2 次逃逸（凭据塞进未监控字段）

逃逸原因很工程化：参数规范化不足、字段覆盖率不够、schema 不完整。

## 输入分类器为什么不够

### Spotlighting（分隔标记）

在四个模型上效果不稳定，GPT-4o-mini 上反而让攻击成功率从 10.0% 升到 13.3%。

### 输入级提示注入分类器

- ProtectAI DeBERTa v2：库尔德语攻击召回率高，但正常样本误报率高达 97.5%
- Meta Prompt-Guard-2：误报率低，但几乎抓不到攻击，各语言召回率仅约 1%

AgentShield 不直接判断文本是否恶意，而是看工具调用行为——越复杂的 Agent 场景，越需要把检测点后移到工具调用、权限边界和行为轨迹上。

## 陷阱变成训练标签

假工具触发可以生成高置信 compromise 标签，训练 Random Forest 行为分类器（输入特征为工具调用数量、读写比例、操作模式等行为特征）：

- held-out 测试集：F1 = 0.996，precision = 1.000，recall = 0.991
- 跨模型迁移（GPT 训练 → Llama/DeepSeek 测试）：F1 = 0.990
- 跨语言迁移（英文训练 → 库尔德语/阿拉伯语测试）：F1 = 0.997，误报率 0.1%

局限性：只能覆盖"触发过陷阱的沦陷行为"，攻击完全待在合法工具/参数内则无法自动标注。

## 局限性

- 只能捕捉三类异常：调用可疑工具、触碰假凭据、参数不在白名单
- 攻击者完全待在批准工具、合法参数、可信收件人之内则无法报警
- 实验主要基于 AgentDojo 一个 benchmark
- 误报测试只在 GPT-4o-mini 上完成
- 176 条攻击 prompt 为手工构造
- honeytoken 层在标准测试中未触发

## 工程启示

AgentShield 指向的架构方向：**Agent 安全不能只做输入输出审核，而要进入工具调用控制面**。未来企业 Agent 连接邮件、OA、数据库、代码仓库、云资源、财务系统，每一个工具都是动作出口，安全系统需要在这些出口上做识别、监控和告警。

这和传统安全的 EDR、蜜罐、DLP、API 网关、零信任策略很像，只不过监控对象从人和程序变成了 Agent 的工具调用轨迹。

论文计划将 AgentShield 部署为 **MCP proxy**，在真实 Agent 系统中评估性能——MCP 正在变成 Agent 连接工具和数据源的重要协议层，安全能力进入 MCP proxy 可能形成天然的 Agent Runtime Security 入口。

## 相关页面

- [[大厂打包了 harness，sandbox 公司快死了]] — Agent infra 生态变化，Anthropic/OpenAI 加固 agent 安全基础设施
- [[AmphiLoop — 两栖模式构建Agent]] — Agent 框架设计，工具调用链路安全是部署前提
- [[Claude Code 在大型代码库中的最佳实践]] — Anthropic 官方 Agent 部署模式，Hooks/MCP 等控制面与 AgentShield 互补
- [[6个让Agent技能稳定生效的模式]] — Agent 技能编写模式，安全约束是技能设计的重要维度

---

*由 knowledge-wiki skill 自动收录并翻译整理*
