# 第五章 · 头部产品团队案例研究

> 11 个 2025-2026 年标志性团队的真实运作方式。每个案例包含:公司、规模、交付节奏、组织方式、关键金句、PM 教训。

---

## 5.1 Anthropic Claude Code — "PM 轻触式管理 + 评估驱动"

- **团队规模**:Claude Code 起步团队为核心三人(Boris Cherny、Sid、Ben),Cat Wu 作为 PM 后期加入
- **覆盖度**:Anthropic 90% 自家代码由 AI 编写,Claude Code 在 GitHub 上每天承包约 13.5 万次公共提交,相当于 4% 公开 commits(截至 2026-02)
- **交付节奏**:Cat Wu 在 Lenny's Podcast (2026-04) 表示,Anthropic 内部研发周期已从"6 个月→1 个月→有时 1 天"

### 组织方式
- 三件套工作流:**Claude.ai**(思考伙伴)+ **Claude Code**(原型)+ **Cowork**(知识工作)
- **Side quests**(自由实验):工程师跳出 roadmap 进行 1-2 天的探索,结果 demo 优先于文档
- **Launch room**:发布前在专门的房间集中红队、性能、UX 审查
- PM 负责 "clearing the path",由模型/工程驱动,**PM 轻触**
- 每次新模型发布时复测旧功能

### 评估实践
- 建立基线 → 写 rubric → 由独立 grader(隔离上下文)打分,避免 agent 自我合理化
- "Dreaming":跨多 agent 沉淀共性错误与偏好,演化为团队记忆

### 关键金句
> "Claude Code is not a product as much as it's a Unix utility." —— Boris Cherny, Latent Space

> "I PM with a pretty light touch... I'm mainly there to clear the path." —— Cat Wu

> "Because you can prototype in an afternoon, wrong bets are cheap." —— Cat Wu, Anthropic Blog

### PM 教训
**尽量做"最简单的事"**——下一个模型来了,复杂的 workaround 都会过期。

---

## 5.2 Cursor (Anysphere) — 50 人无 PM、$2B ARR 的极致工程主导

- **团队规模**:~50 名员工,$2B ARR(2026),服务数百万用户
- **交付节奏**:仅 2026-03 就 ship 了 5 个主要发布;2026-04 推出 Cursor 3(agent-first workspace)

### 组织方式
- **没有 PM**。工程师直接和用户对话、定义问题、当天发布
- 极扁平结构、无审批层级;每个工程师都参与 hiring
- 文化:**极致工程聚焦 + 高速发版**

### PM 教训
在 agent 工具领域,传统 PM 角色被"懂产品的工程师"取代。当模型能力本身就在快速变化时,**决策链越短迭代越快**。

---

## 5.3 Linear — 单 PM 模式 + Agent 一等公民

- **团队规模**:仍以 "craft, taste, focus" 为核心,多年只有 1 名 PM(Karri Saarinen 自任设计 + 战略)

### 交付内容
- 2025-04 推出 **Linear for Agents**(agents 作为一等用户加入 workspace)
- 2026-04-30 Linear Agent 接入 **MCP**(Mission Control Plane),可读 Linear 外的工具和数据

### 关键教训(Karri Saarinen)
- **不要急于把 GenAI 塞到产品里**,等到使用模式真正成熟(Linear 早期对生成式 AI 持保守态度)
- Agents 应当像同事一样被指派 issue,而不是隐藏在按钮后
- PM 数量少,反而保持了产品克制感

---

## 5.4 Notion — 从 AI 助手到 "Custom Agents" 的路径

- **覆盖度**:内部已运行 2,800 个内部 agent

### 交付时间线
- 2025-09-18 **Notion 3.0**:Agents
- 2026-01-20 **Notion 3.2**:移动端 AI、自动模型选择(GPT-5.2、Claude Opus 4.5、Gemini 3)
- 2026-02-24 **Notion 3.3**:Custom Agents(早期测试者已创建 21,000+ 自定义 agent)
- 2026-04-14 **Notion 3.4 Part 2**:AI Autofill + Skills

### PM 学到的关键经验
- **自然语言 agent 构建器**:让用户描述需求,由 AI 生成 agent,PM 不必预设流程
- **Skills 模式**:把团队反复出现的工作流(写周报、按格式重排文档)封装为可复用命令——把"用户重复行为"转为产品组件的高 ROI 模式

---

## 5.5 Figma — 把画布开放给 Agent

- **背景**:Config 2026 主题之一就是"非人类用户"

### 交付内容(2026-03-24 启动 Figma Canvas for AI Agents 公测)
- `generate_figma_design`:把 HTML 转成可编辑 Figma 图层
- `use_figma`:让 agent 直接基于设计系统组件创建/修改设计
- **Skills**(markdown 文件,编码团队设计规范)
- 与 Uber、One North、Edenspiekermann 等共建 9 个示例 skills

### 关键金句
> "Skills teach Claude Code how to work directly in the design canvas, so you can build in a way that stays true to your team's intent and judgment."
> —— Cat Wu, Head of Product, Claude Code, Anthropic

> "Codex can find and use all the important design context in Figma to help us build higher quality products more efficiently."
> —— Ed Bayes, OpenAI Codex Design Lead

### PM 教训
1. 通用 agent 输出在缺乏设计系统上下文时几乎不可用 → **上下文工程重于模型选型**
2. 用 markdown skills 让团队不写插件就能引导 agent 行为
3. 双向桥(设计 ↔ 代码)才是真正的 agentic 价值

---

## 5.6 垂直 AI 三巨头 — Perplexity / Glean / Harvey

### Perplexity
- 2026 发布 **Perplexity Computer**(autonomous AI agent,可填表、跨软件操作)
- 估值 $9B;revenue 因 agent 战略转型同比 +50%
- **教训**:搜索作为 launchpad,扩展到 Comet Browser(2025-10)、Shopping Hub

### Glean
- ARR 9 个月内从 $100M 跃升到 $200M;2025-06 Series F 募资 $150M,估值 $7.2B
- 战略从"企业搜索"转为"模型与企业系统之间的中间件层"
- **三大支柱**:模型可换插(避锁定)、深度连接器、企业级治理

> **Arvind Jain (CEO)**: "You need to build a permissions-aware governance layer and retrieval layer that is able to bring the right information, but knowing who's asking that question so that it filters the information based on their access rights."

### Harvey
- a16z 领投 $150M,估值 $8B
- 2026-05 推出 **Legal Agent Bench**:1,200+ 任务、24 个法律领域、75,000+ 专家 rubric
- **PM 教训**:垂直 agent 必须自建评估基准,因为通用 benchmark 无法覆盖法律细节

---

## 5.7 Microsoft Copilot — 大企业 + Agent 的治理范式

- **组织**:Jared Spataro 任 CMO of AI at Work,主导从 "assistance" 到 "real doing" 的转型

### 交付
- **Copilot Cowork**(与 Anthropic 合作开发,2026-03 推出):跨应用多步工作流编排
- **Microsoft 365 E7**:E5(生产力)+ Entra(身份)+ Copilot(流程内 AI)+ Agent 365(agent 控制平面)
- **Agent 365**:2026-05-01 GA,$15/user/月;提供 "single pane of glass" 管理所有 agent(Copilot Studio、第三方 marketplace、Cowork workflows),消除 shadow AI

### PM 教训
**在企业场景,治理工具就是产品**。把 observe / govern / secure 当成一等功能而非附属。

---

## 5.8 Lovable — 146 人、$400M ARR 的 agent-leveraged 团队

- **创始人**:Anton Osika (CEO)、Ryan Meadows (CRO)

### 团队规模演进
- 2025-07:~45 人,$100M ARR(8 个月达到,号称全球最快)
- 2026-02:$400M ARR
- 2026-03:146 人、单月新增 $100M ARR、**$2.77M ARR/员工**

### 客户与教训
- 扩展到企业客户(Klarna、HubSpot)
- **"vibe coding"作为切入点降低门槛**,再通过 Lovable Agent(2025-07 默认开启)处理多步任务,宣称 91% 错误率下降
- 极小团队 + 高 leverage 的关键是**把"产品本身就是 agent"**

> **Anton Osika**: "The purpose of this brand campaign is to inspire the next generation of builders — non-technical people with great ideas that deserve to come to life."

---

## 5.9 Sierra — Agent PM 与 Agent Engineer 融为一岗

- **背景**:Bret Taylor(前 Salesforce Co-CEO,OpenAI 董事会主席)创立
- **融资**:2026-05 以 $15B 估值完成 $950M 融资
- **APX(Agent Product Expert)项目**:18 个月轮岗,新人前 9 个月当 Agent Engineer,后 9 个月当 Agent PM
- **核心理念**:每个 Agent 都是"为客户从零设计的完全定制产品",非通用解决方案
- **2026-03 推出 Ghostwriter**:an agent that builds agents

### PM 教训
在企业级垂直 agent 场景,**PM 与 Engineer 的边界必须打破**——让同一个人既能写 spec 又能改 prompt、调 tool。

---

## 5.10 Anthropic 双轨架构 — Claude Agent SDK + Managed Agents

- **2026-Q2 推出**:把"agent 团队"作为一等公民组织哲学产品化
- **Claude Agent SDK**(给 platform engineer):底层框架,完全可控
- **Managed Agents**(给 product engineer):上层托管,快速集成
- **跨学科评审**:platform engineering(框架)+ ML engineering(skill registry)+ observability + security(把 service principal 扩展到 Agent)
- **2026-04 与 SpaceX 签 Colossus 1 数据中心协议**:220k GPU、300MW,缓解 Claude API 容量瓶颈

### PM 教训
**双轨产品策略**让 platform 与 product 两类工程师都有合适的入口,降低 Anthropic 自己的客户成功成本。

---

## 5.11 Apollo / Every / Duolingo — "Product Builders" 模式

### Apollo
CPO 公开宣布招 **Product Builders** 而非 Product Managers,要求"先用 Claude Code 做出可工作的产品再说"。

### Every (Dan Shipper)
**15 人团队跑 5 个产品**,做到七位数收入,"已经没人在手写代码"。

### Duolingo
用 AI 把课程产能从 12 年 100 门提升到 **12 个月 150 门**——18 倍效率提升。

### PM 教训
**小团队 + 高 leverage 的杠杆点是 PM 自己也写代码**,而非依赖外部工程师执行。

---

## 资料源

### 头部团队案例
- [Best practices for Claude Code — Anthropic Engineering](https://www.anthropic.com/engineering/claude-code-best-practices)
- [How Anthropic teams use Claude Code (PDF)](https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf)
- [Product Management on the AI Exponential — Claude Blog (Cat Wu et al.)](https://claude.com/blog/product-management-on-the-ai-exponential)
- [Lenny's Newsletter — How Anthropic's product team moves faster (Cat Wu)](https://www.lennysnewsletter.com/p/how-anthropics-product-team-moves)
- [Lenny's Newsletter — Head of Claude Code: What happens after coding is solved (Boris Cherny)](https://www.lennysnewsletter.com/p/head-of-claude-code-what-happens)
- [Latent Space — Claude Code: Anthropic's Agent in Your Terminal](https://www.latent.space/p/claude-code)
- [JobsByCulture — Working at Cursor (Anysphere) in 2026](https://jobsbyculture.com/blog/working-at-cursor-2026)
- [Contrary Research — Cursor Business Breakdown](https://research.contrary.com/company/cursor)
- [Linear for Agents](https://linear.app/agents)
- [Lenny's Newsletter — Inside Linear: Building with taste, craft, and focus (Karri Saarinen)](https://www.lennysnewsletter.com/p/inside-linear-building-with-taste)
- [Notion 3.3: Custom Agents](https://www.notion.com/releases/2026-02-24)
- [Notion 3.4 Part 2](https://www.notion.com/releases/2026-04-14)
- [Notion 3.0: Agents](https://www.notion.com/releases/2025-09-18)
- [Figma Blog — Agents, Meet the Figma Canvas](https://www.figma.com/blog/the-figma-canvas-is-now-open-to-agents/)
- [Figma Blog — Config 2026 Speakers Looking Ahead](https://www.figma.com/blog/config-speakers-looking-ahead-2026/)

### 垂直 AI 三巨头
- [Miracuves — Perplexity Revenue Model 2026](https://miracuves.com/blog/perplexity-revenue-model/)
- [Let's Data Science — Perplexity Shifts to AI Agents](https://letsdatascience.com/news/perplexity-shifts-to-ai-agents-boosts-revenue-a76bce31)
- [Glean — Series F Announcement](https://www.glean.com/blog/glean-series-f-announcement)
- [TechCrunch — Glean is building the layer beneath the interface](https://techcrunch.com/2026/02/15/the-enterprise-ai-land-grab-is-on-glean-is-building-the-layer-beneath-the-interface/)
- [Creator Economy — Arvind Jain on $100M Company in 3 Years](https://creatoreconomy.so/p/100m-company-in-3-years-ai-agents-glean-arvind-jain)
- [Artificial Lawyer — Harvey Launches Legal Agent Bench](https://www.artificiallawyer.com/2026/05/06/harvey-launches-legal-agent-bench/)
- [Harvey — The Brief April 2026](https://www.harvey.ai/blog/the-brief-april-2026)

### Microsoft / Lovable / Sierra
- [Microsoft 365 Blog — Powering Frontier Transformation with Copilot and agents](https://www.microsoft.com/en-us/microsoft-365/blog/2026/03/09/powering-frontier-transformation-with-copilot-and-agents/)
- [Microsoft 365 Blog — Copilot Cowork (May 5, 2026)](https://www.microsoft.com/en-us/microsoft-365/blog/2026/05/05/copilot-cowork-from-conversation-to-action-across-skills-integrations-and-devices/)
- [Fortune — Microsoft debuts Copilot Cowork built with Anthropic's help](https://fortune.com/2026/03/09/microsoft-copilot-cowork-ai-agents-anthropic-e7-m365-saas/)
- [Lovable — $100M ARR & Lovable Agent](https://lovable.dev/blog/agent)
- [TechCrunch — Lovable adds $100M in revenue last month with 146 employees](https://techcrunch.com/2026/03/11/lovable-says-it-added-100m-in-revenue-last-month-alone-with-just-146-employees/)
- [TechCrunch — Sierra raises $950M (May 4, 2026)](https://techcrunch.com/2026/05/04/sierra-raises-950m-as-the-race-to-own-enterprise-ai-gets-serious/)
- [Sierra — APX Program](https://sierra.ai/blog/apx-program)
- [Zylos Research — Claude Agent SDK & Managed Agents architecture](https://zylos.ai/research/2026-04-20-claude-agent-sdk-managed-agents-architecture)
