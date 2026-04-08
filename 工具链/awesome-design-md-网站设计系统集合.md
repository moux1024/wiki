# awesome-design-md — 网站设计系统 DESIGN.md 集合

> 来源: https://github.com/VoltAgent/awesome-design-md
> 日期: 2026-04-08
> 分类: 工具链
> 标签: DESIGN.md, AI, UI设计, 设计系统, Google Stitch, coding agent

## 概述

由 VoltAgent 维护的精选 DESIGN.md 文件集合，从知名网站提取设计系统，供 AI coding agent 生成一致的 UI。

## DESIGN.md 是什么

[DESIGN.md](https://stitch.withgoogle.com/docs/design-md/overview/) 是 Google Stitch 引入的新概念——纯文本设计系统文档，AI agent 读取后可生成一致的 UI。

它就是一个 markdown 文件，不需要 Figma 导出、JSON schema 或特殊工具。放到项目根目录，任何 AI coding agent 或 Google Stitch 都能立即理解 UI 应有的外观。

| 文件 | 谁读取 | 定义什么 |
|------|--------|----------|
| AGENTS.md | Coding agent | 如何构建项目 |
| DESIGN.md | Design agent | 项目应该长什么样 |

## 文件结构

每个 DESIGN.md 遵循 [Stitch DESIGN.md 格式](https://stitch.withgoogle.com/docs/design-md/format/)，包含扩展章节：

| 章节 | 内容 |
|------|------|
| 1. Visual Theme & Atmosphere | 基调、密度、设计哲学 |
| 2. Color Palette & Roles | 语义名称 + hex + 功能角色 |
| 3. Typography Rules | 字体族、完整层级表 |
| 4. Component Stylings | 按钮、卡片、输入框、导航及各状态 |
| 5. Layout Principles | 间距系统、网格、留白哲学 |
| 6. Depth & Elevation | 阴影系统、表面层级 |
| 7. Do's and Don'ts | 设计护栏和反模式 |
| 8. Responsive Behavior | 断点、触摸目标、折叠策略 |
| 9. Agent Prompt Guide | 快速颜色参考、可直接使用的 prompt |

每个站点还附带 `preview.html`（亮色）和 `preview-dark.html`（暗色）可视化目录。

## 收录站点（58 个）

### AI 平台
- **Claude** — Anthropic AI 助手，暖色赤陶色调，简洁编辑式布局
- **Cohere** — 企业 AI 平台，活力渐变，数据丰富的仪表盘风格
- **ElevenLabs** — AI 语音平台，暗色电影级 UI，音频波形美学
- **Minimax** — AI 模型提供商，大胆暗色界面配霓虹点缀
- **Mistral AI** — 开源 LLM 提供商，法式极简主义，紫色调
- **Ollama** — 本地运行 LLM，终端优先，单色简洁
- **OpenCode AI** — AI 编码平台，开发者暗色主题
- **Replicate** — API 运行 ML 模型，干净白色画布，代码导向
- **RunwayML** — AI 视频生成，电影级暗色 UI，媒体丰富布局
- **Together AI** — 开源 AI 基础设施，技术蓝图风格设计
- **VoltAgent** — AI agent 框架，纯黑画布，翡翠绿点缀，终端原生
- **xAI** — Elon Musk 的 AI 实验室，鲜明单色，未来极简主义

### 开发者工具
- **Cursor** — AI 优先代码编辑器，流畅暗色界面，渐变点缀
- **Expo** — React Native 平台，暗色主题，紧凑字距，代码导向
- **Linear** — 工程师项目管理，超极简，精准，紫色点缀
- **Lovable** — AI 全栈构建器，活泼渐变，友好开发美学
- **Mintlify** — 文档平台，清爽，绿色点缀，阅读优化
- **PostHog** — 产品分析，活泼刺猬品牌，开发者友好暗色 UI
- **Raycast** — 效率启动器，流畅暗色，活力渐变点缀
- **Resend** — 开发者邮件 API，极简暗色主题，等宽字体点缀
- **Sentry** — 错误监控，暗色仪表盘，数据密集，粉紫点缀
- **Supabase** — 开源 Firebase 替代品，暗色翡翠主题，代码优先
- **Superhuman** — 快速邮件客户端，高端暗色 UI，键盘优先，紫色光晕
- **Vercel** — 前端部署平台，黑白精准，Geist 字体
- **Warp** — 现代终端，暗色 IDE 风格，块状命令 UI
- **Zapier** — 自动化平台，温暖橙色，友好插画驱动

### 数据与基础设施
- **ClickHouse** — 快速分析数据库，黄色点缀，技术文档风格
- **Composio** — 工具集成平台，现代暗色配多彩集成图标
- **HashiCorp** — 基础设施自动化，企业级整洁，黑白配色
- **MongoDB** — 文档数据库，绿叶品牌，开发者文档导向
- **Sanity** — Headless CMS，红色点缀，内容优先编辑式布局
- **Stripe** — 支付基础设施，标志性紫色渐变，font-weight-300 优雅

### 设计与协作
- **Airtable** — 电子表格-数据库混合，多彩友好，结构化数据美学
- **Cal.com** — 开源日程，干净中性 UI，开发者导向简洁
- **Clay** — 创意机构，有机形状，柔和渐变，艺术指导布局
- **Figma** — 协作设计工具，活力多色，有趣而专业
- **Framer** — 网站构建器，大胆黑蓝，动效优先，设计导向
- **Intercom** — 客户消息，友好蓝色调，对话式 UI 模式
- **Miro** — 可视化协作，明亮黄色点缀，无限画布美学
- **Notion** — 一体化工作空间，温暖极简，serif 标题，柔和表面
- **Pinterest** — 视觉发现平台，红色点缀，瀑布流网格，图片优先
- **Webflow** — 可视化网页构建器，蓝色点缀，精美营销站美学

### 金融科技
- **Coinbase** — 加密交易所，干净蓝色标识，信任导向，机构感
- **Kraken** — 加密交易平台，紫色点缀暗色 UI，数据密集仪表盘
- **Revolut** — 数字银行，流畅暗色界面，渐变卡片，fintech 精准
- **Wise** — 国际转账，明亮绿色点缀，友好清晰

### 消费品牌
- **Airbnb** — 旅行市场，温暖珊瑚色点缀，摄影驱动，圆润 UI
- **Apple** — 消费电子，高端留白，SF Pro 字体，电影级影像
- **IBM** — 企业技术，Carbon 设计系统，结构化蓝色调
- **NVIDIA** — GPU 计算，绿黑能量，技术力量美学
- **SpaceX** — 航天技术，鲜明黑白，全出血影像，未来感
- **Spotify** — 音乐流媒体，暗色活力绿色，粗体排版，专辑封面驱动
- **Uber** — 出行平台，大胆黑白，紧凑排版，都市活力

### 汽车品牌
- **BMW** — 奢华汽车，暗色高端表面，精准德式工程美学
- **Ferrari** — 奢华汽车，明暗对比编辑风格，Ferrari 红配极致稀疏
- **Lamborghini** — 奢华汽车，纯黑大教堂风格，金色点缀，LamboType 定制字体
- **Renault** — 法系汽车，鲜明极光渐变，NouvelR 专属字体，零圆角按钮
- **Tesla** — 电动汽车，极致减法，电影级全视口摄影，Universal Sans 字体

## 使用方法

1. 复制某个站点的 DESIGN.md 到项目根目录
2. 告诉 AI agent 使用它即可

## 许可

MIT License。所有 DESIGN.md 文件按原样提供，不声称拥有任何网站的视觉标识所有权。

## 相关页面

- [[LLM Wiki — 用 LLM 构建个人知识库的模式]]

---

*由 knowledge-wiki skill 自动收录并翻译整理*
