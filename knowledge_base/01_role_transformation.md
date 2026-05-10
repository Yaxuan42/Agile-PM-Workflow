# 第一章 · PM 角色重塑

> 2026 年的产品经理(PM)正在经历该职业自诞生以来最剧烈的重构。核心转变可以用一句话概括:**PM 正从"信息搬运工与需求文档作者"(information mover / spec writer),转变为"Agent 编排者、评测设计者与结果所有者"(agent orchestrator / eval designer / outcome owner)。**

---

## 1.1 核心论点

Meta / Google 前 CPO **Nikhyl Singhal** 在 Lenny's Podcast(2026-04)中把 PM 群体劈成两半:

- **Information Mover**(信息搬运工):靠协调、对齐、写文档存活
- **Builder**(建造者):会用 AI 直接产出代码、原型与 Agent

他预测"未来 12-24 个月将是 PM 历史上最混乱的时期,公司会裁掉数千 PM,然后重新招聘数千个 AI-first、技能完全不同、薪酬更高的人"。

Microsoft CPO **Aparna Chennapragada** 提出业界引用最广的两句:

> **"Prompt sets are the new PRDs."**
> **"NLX (Natural Language Experience) is the new UX."**
> **"如果你不在用 AI 做原型,你就做错了。"**

David Haberlah 对 638 位 PM 实践者声音的元分析(2026-03)显示:Lenny's Newsletter 的 AI 相关内容占比从 2023 年初的 4% 飙升至 2026 Q1 的 67%,标志着 AI 已从专题话题变为 PM 讨论的主导镜头。

---

## 1.2 PM 的 8 项新核心职责

### 1. Agent 能力域定义与 Skill / Tool 设计
PM 决定一个 Agent 应该"会做什么、不会做什么":定义 skill 包、MCP 工具调用边界、子 Agent 拆分。
Sierra 的 APX 项目(Bret Taylor 主导)把 Agent PM 定义为"为每个客户从零设计 Agent",每个 Agent 都被视作"一个完全定制的产品"。

### 2. Eval 套件设计与持续运营
区分初/资深 AI PM 的最重要技能。PM 亲自做 error analysis、对真实对话做 open coding、设定 offline tests、A/B 实验、edge-case eval 与漂移监控,把 eval 嵌入 CI/CD 门禁。
Arize 创始人 **Aman Khan** 标志性引言:
> "Prompts may make headlines, but evals quietly decide whether your product thrives or dies."

### 3. 成本 / 延迟预算管理
PM 现在拥有 token 预算、per-request 成本上限、端到端延迟 SLO。
*CIO* 杂志(2026-04)指出"企业 AI Agent 真实 TCO 被低估 40-60%,LLM 调用占总开销 40-60%";同样任务 agent 形态比 chatbot 多消耗 5-30 倍 token。PM 必须从第一天起就部署缓存、跟踪 cost-per-task、设置 iteration cap。

### 4. AI-Native 原型与 Vibe Coding
"Demos before memos"。PM 用 Claude Code、Claude Design(2026-04 Anthropic 推出,基于 Opus 4.7)、n8n、Lovable 等工具,数小时产出可点击原型替代 PRD。
HeyGen 创始人 Joshua Xu 警告:
> "Demo value isn't user value. Building a cool AI demo doesn't mean we have a product customers love."

### 5. 概率系统设计与 NLX 对话编排
PM 必须为"用户行为不可预测 × LLM 输出不可预测"的双重不确定性设计。
Aishwarya Naresh Reganti 概括:
> "You don't know how the user might behave with your product, and you also don't know how the LLM might respond."

### 6. Agent 安全与治理评审
PM 跨 platform engineering(框架)、ML engineering(skill registry)、observability、security(把 service principal 扩展到 Agent)四个学科做安全评审。Anthropic 与摩根大通 Jamie Dimon 合作把"Agent 治理"列为企业部署门票。

### 7. 多 Agent 编排与工作分解
2026 是单 Agent 让位给协同多 Agent 系统的元年。MIT Technology Review 4 月称之为 "Orchestration Era"。PM 的工作变成"管真人团队":写清晰 spec、做工作分解、做产出验证。
Aakash Gupta 的关键观点:
> "真正在 2026 出货多 agent 流的 PM,并不在做完全自治系统,而是在编排可预测部分,只在必要处用 AI。"

### 8. 结果(Outcome)所有权与 "Goal Vector" 委派
PM 不再交付"执行路径",而是定义"Goal Vector"——清晰可量化的结果指标——让 Agent 自决路径。
Marty Cagan 在 2026 年的反复强调:
> "You celebrate when you actually solve the problem... product teams are about outcomes, they're not about output."

---

## 1.3 正在被淘汰(Deprecated)的 5 项旧职责

1. **多年路线图与季度规划文档** —— OpenAI 前 CPO Kevin Weil(2026-04 已离职)指出"季度/年度计划撑不过三个月",规划被 3-6 个月滚动 bet 替代。

2. **精修过的 PRD 与高保真静态 Mockup** —— 设计师 Jenny Wen 报告 mockup/prototype 占设计工作的比例从 60-70% 降至 30-40%。Alloy.app(2026-02)直白:"当 Agent 输出原型,你直接跳到验证。"

3. **跨团队协调与对齐会议** —— Singhal 所说"information mover" PM 被淘汰的核心原因。AI 已能跨 Linear / Slack / Notion 自动同步状态。

4. **手工竞品分析与市场调研报告** —— 被深度研究 Agent(Perplexity Computer、Claude Research、Glean Agent)取代。

5. **特性工厂思维** —— Airtable CEO Howie Liu 在 2026 把整个组织重构为"快思考 / 慢思考"两个组,公开质疑"不重塑的老牌玩家是否能活下来"。

---

## 1.4 组织结构与团队比例的变化

### PM:Engineer:Designer 的真实数据(2026 年初,Lenny Rachitsky 报告)

- 全球 PM 公开岗位 **超过 7,300 个**,较 2023 年最低点上涨 75%,2026 年内再涨 20%——"PM 岗位是三年来最高水平"
- 工程师岗位 **6.7 万+(全球)、2.6 万(美国)**
- 设计师岗位 **约 5,700 个**——自 2023 年起几乎零增长
- **PM:设计师比例已翻转为 1.27×**(过去是设计师更紧俏)

### "更少 PM,但更资深"模式

> "我们 18 个月做到 5,000 万美元 ARR,刚开始招第一个 PM。五年前,我们这个体量已经会有 10 个 PM 团队。"
> —— 一位创始人对 Singhal 的原话

具体公司案例:
- **Anthropic 增长团队** 反扩招 PM,因 Claude Code 让工程师以 3 倍体量出货,"PM 层接不住了"
- **Sierra**:18 个月轮岗的 APX 项目,新人前 9 个月当 Agent Engineer、后 9 个月当 Agent PM,把工程与产品融为一岗。Sierra 在 2026-05 以 $15B 估值完成 $950M 融资
- **Apollo**:CPO 公开宣布招 **"Product Builders"** 而非 Product Managers
- **Every (Dan Shipper)**:15 人团队跑 5 个产品,做到七位数收入,"已经没人在手写代码"
- **Duolingo**:用 AI 把课程产能从 12 年 100 门提升到 12 个月 150 门
- **Linear**:招聘 Senior/Staff Product Engineer (AI),JD 明确"舒适地在没有重 PM overhead 的环境工作"——把传统 PM 工作并入 Product Engineer 角色
- **Coinbase**:2026 年裁约 700 人,Brian Armstrong 公开归因为"AI 工具让更小、更扁平的团队更高效"
- **Salesforce**:2026 年裁员"少于 1,000 人",显式包含 product management 与 Agentforce AI 部门

---

## 1.5 邻位新角色族

| 新角色 | 雇主样本 | 关键差异 |
|---|---|---|
| **AI / Agent PM** | Sierra、Scale、Anthropic | 拥有 eval、agent 行为、客户定制 |
| **Forward Deployed Engineer (FDE)** | Palantir、OpenAI、Anthropic、Salesforce(承诺招 1,000 人)、Ramp、Rippling、Intercom、Cohere、Databricks | 客户发现 + scoping(传统 PM 工作)+ 部署。2025-01 至 09 岗位 +800%,2026 再 +800%,平均薪酬 $238k,Staff 级 $630k+ |
| **Product Builder / Product Engineer** | Apollo、Linear、Vercel、Conductor | 先动手做、再写 spec |
| **Eval Engineer** | Arize、OpenAI、Anthropic | 专做 eval 框架与监控 |
| **Professional Vibe Coder** | Lovable | 全部时间放在 "good judgment, clarity, quality, taste" |

---

## 1.6 高管层视角

**Aparna Chennapragada(Microsoft CPO)**:
> "PM 角色没有死,它在演化为'品味制定者(tastemaker)与编辑者(editor)'。AI 让世界充斥着创意与原型,PM 的工作从过程管理(AI 能做)转向编辑判断与品味制定。"
> "如果一个产品没有编辑判断,就会变成一个 Frankenstein 产品。"

**Mike Krieger(Anthropic CPO)**:在与 Sarah Guo、Kevin Weil 的对谈中强调 Anthropic 内部把"agent 团队"作为一等公民组织单元;Anthropic Q2 2026 推出双轨架构——Claude Agent SDK(给 platform engineer)+ Managed Agents(给 product engineer)——直接把这种组织哲学产品化。

**Nikhyl Singhal**:
> "公司将裁掉 30,000 人,再重新招进 8,000 人——全部 AI-first。"
> "一堂课不会让你变成 Builder。你必须有那种痴迷,只能选一边。"

**Bret Taylor(Sierra CEO,前 Salesforce Co-CEO,OpenAI 董事会主席)**:把 PM 与 Engineer 的边界彻底打散——APX 项目让同一个人在 18 个月内先后扮演两种角色。

---

## 1.7 给读者的实操结论

1. **把"写 PRD"重新定义为"做原型 + 写 prompt set + 设计 eval"**。Aparna 的 "Prompt sets are the new PRDs" 应该成为团队规范。
2. **每个 AI 功能必须配套 eval set + 成本/延迟预算 + 安全评审**——三件套缺一不可。
3. **学一个 Agent 框架并真的用它出货**(n8n 入门 → Claude Code 生产),不是为了取代工程师,而是为了拥有"什么时候该用 Agent / 什么时候该用普通 workflow"的判断力。
4. **如果你是"信息搬运工"型 PM,12-24 个月内必须切换轨道**:要么走 Builder PM / Agent PM / FDE 路线,要么走更高阶的产品战略 / CPO 路线。最危险的是中间地带。
5. **关注组织信号**:招聘 JD 出现 "Product Builder"、"Agent PM"、"Forward Deployed Engineer"、"Product Engineer (AI)" 的公司,代表它们已经在重画 PM 的边界。

---

## 资料源

- [The Skip — The PM Career Framework for AI (Nikhyl Singhal)](https://theskip.substack.com/p/the-pm-career-framework-for-ai-how)
- [Lenny's Newsletter — Why half of product managers are in trouble (Singhal)](https://www.lennysnewsletter.com/p/why-half-of-product-managers-are-in-trouble)
- [Lenny's Newsletter — Microsoft CPO Aparna Chennapragada on AI](https://www.lennysnewsletter.com/p/microsoft-cpo-on-ai)
- [Lenny's Newsletter — State of the product job market in early 2026](https://www.lennysnewsletter.com/p/state-of-the-product-job-market-in-ee9)
- [Medium — Haberlah: 638 Practitioner Voices on PM's AI Transformation](https://medium.com/@haberlah/what-638-practitioner-voices-reveal-about-pms-ai-transformation-7d2fd16be10d)
- [Aakash Gupta — How to become a Builder PM in 2026](https://www.aakashg.com/how-to-become-a-builder-pm/)
- [Sierra — APX Program](https://sierra.ai/blog/apx-program)
- [TechCrunch — Sierra raises $950M (May 2026)](https://techcrunch.com/2026/05/04/sierra-raises-950m-as-the-race-to-own-enterprise-ai-gets-serious/)
- [TechCrunch — Anthropic launches Claude Design (Apr 2026)](https://techcrunch.com/2026/04/17/anthropic-launches-claude-design-a-new-product-for-creating-quick-visuals/)
- [TechCrunch — Kevin Weil exits OpenAI (Apr 2026)](https://techcrunch.com/2026/04/17/kevin-weil-and-bill-peebles-exit-openai-as-company-continues-to-shed-side-quests/)
- [Fortune — Anthropic deepens Wall Street push (May 2026)](https://fortune.com/2026/05/05/anthropic-wall-street-financial-services-agents-jamie-dimon/)
- [Zylos Research — Claude Agent SDK & Managed Agents architecture](https://zylos.ai/research/2026-04-20-claude-agent-sdk-managed-agents-architecture)
- [MIT Technology Review — Agent Orchestration Era (Apr 2026)](https://www.technologyreview.com/2026/04/21/1135654/agent-orchestration-ai-artificial-intelligence/)
- [CIO — How to get AI agent budgets right in 2026](https://www.cio.com/article/4099548/how-to-get-ai-agent-budgets-right-in-2026.html)
- [Linear Careers — Senior/Staff Product Engineer, AI](https://linear.app/careers/b4a7764e-c680-4bdf-9956-dc78f2ca94d5)
- [Gigged.AI — The Forward Deployed Engineer 2026](https://gigged.ai/the-forward-deployed-engineer-2026s-hottest-job-title/)
- [KORE1 — Tech Layoffs 2026](https://www.kore1.com/tech-layoffs-2026/)
