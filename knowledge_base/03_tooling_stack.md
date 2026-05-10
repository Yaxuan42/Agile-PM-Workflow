# 第三章 · 2026 PM 工具栈

> a16z 的 Martin Casado 指出:所有现存工具(IDE、Figma、Excel)都是"执行工具",而 Agent 时代真正稀缺的是"探索工具"——这定义了 2026 年 PM 新栈的核心方向。

Lenny Rachitsky 2026-04 大型调研:PM 在 AI 上获得最大 ROI 的三个场景是 **撰写 PRD(21.5%)**、**生成原型(19.8%)**、**改善沟通(18.5%)**;而用户研究(4.7%)和路线图构思(1.1%)仍滞后。**生产端工具已成熟,思考端还是蓝海。**

---

## 3.1 九大类别工具盘点

### 1. Discovery / 用户研究合成
- **Dovetail**:大企业研究 repo 标准,但 2026-03 的 search-first redesign 砍掉了自定义 home/feed,引发部分团队迁移
- **Kraftful**(已被 Amplitude 收购):VoC 分析嵌入产品分析栈
- **BuildBetter / Marvin / Condens / Aurelius**:AI-native 替代品,主打 "customer intelligence system"
- **UserCall / Sleekplan Intelligence**:Agent 主动跟用户做电话访谈并自动合成主题
- *用例*:50 个 Gong 通话录音 → 自动产出 themes / quotes / Jira tickets

### 2. Spec / PRD
**传统派**:Notion AI、Confluence AI、ChatPRD(PM 专用 PRD copilot)、Figma AI PRD Generator

**Eval-as-spec 派**:Braintrust 在《Evals are the New PRD》中明确提出新循环:**Problem → Eval → Hillclimb → Ship**。每周节奏:周一看 trace、周二写 eval case、周三跑套件、周四数据决策、周五加速

**Spec-Driven Development**:GitHub Spec Kit(93k+ stars,4 阶段:Specify / Plan / Tasks / Implement)、BMAD-METHOD v6.6(2026-04-29 发布,46.7k stars,12+ 角色 agent 含 Preston PM agent)、Kiro

> *用例*:AI feature 的 acceptance criteria 不再是"应该有用",而是"100 条 test case 通过率 ≥ 92%"

### 3. Prototyping(Figma 已被分流)
- **v0 (Vercel)**:UI 质量与设计工作流之王,2026-02 加 Git 集成、VS Code 风格编辑器、数据库连接、agentic workflow
- **Lovable**:Lovable Cloud(Supabase 后端)让非技术 PM 一站式做出"准生产"应用
- **Bolt.new**:StackBlitz WebContainer,浏览器内全栈,Claude Sonnet 驱动
- **Claude Artifacts / Claude Design**:Anthropic 2026-04 推出的 Claude Design 解决了关键缺口——从现有 codebase 抽取 design system 并直通 Claude Code 落地
- **Magic Patterns**:组件级别原型

> 实战常用组合:**v0(组件) → Lovable(整体应用) → Claude Code(生产清理)**
> 注意:这类工具仅能做到 production 的 ~70%,最后 30% 仍需工程团队介入

### 4. Agent Build
- **Anthropic Claude Agent SDK**:2026-04-18 Opus 4.7 GA,引入 effort controls(standard/high/maximum)和 task budgets,同价 $5/$25 per MTok。Tool-use-first 架构,sub-agent 作为 tool 调用,安全护栏在模型层
- **OpenAI Agents SDK**:2026-04-15 大更新,sandbox + harness 隔离环境;显式 handoff;三层 guardrails 并行运行
- **LangGraph**:2026 年初在 GitHub stars 超过 CrewAI,graph-based state machine、内置持久化、time-travel debugging,long-horizon + HIL 场景的事实标准
- **CrewAI**:role-based crews 适合非工程师推理业务流程,但抽象较重、agent 中途 replan 时控制流不透明
- **Mastra**:TypeScript-first,原生 Vercel 部署,把 agents/workflows/memory/evals/observability 打包
- **Vercel AI SDK 5/6**:`stopWhen`、streaming、tool-calling + v5 的 SSE streaming 和语音支持
- **Google Cloud Next 2026**:Vertex AI 改名为 Gemini Enterprise Agent Platform,并入 Agentspace

### 5. Evaluation
- **Braintrust**:eval-first 设计,免费层 1M spans + 10K eval runs,**无 per-seat 收费**,CI/CD 阻断不达标 PR。Stripe / Anthropic 工程师创建,开发体验最强
- **Langfuse**:开源标杆,Docker 30 分钟自部署,OpenTelemetry 兼容;适合需要数据控制权和成本敏感的团队
- **LangSmith**:LangChain/LangGraph 用户的零配置选择;Plus $39/seat/月含 10K traces,超出 $5/10K(14天) 或 $45/10K(400天)——traffic 高时成本指数级上涨
- **Promptfoo**:已被 OpenAI 收购仍 MIT 开源;CLI + YAML 系统化测试,OpenAI 与 Anthropic 内部都用
- **Inspect AI**(UK AISI):benchmark 评测、computer-use 任务标准
- **Patronus**:多模态 LLM-as-judge、幻觉检测、FinanceBench,金融/医疗合规首选
- **HoneyHive**:复杂多步 agent 调试和 tool-use attribution

### 6. Observability(2026 分裂三派)
- **传统 APM 加 LLM 标签**:Datadog LLM Observability(MCP client tracing 最强)、New Relic AI Monitor
- **AI-native tracing**:Langfuse、LangSmith、Arize Phoenix(local-first、Jupyter 友好,自带 drift detection 和 embeddings 分析)、Laminar
- **AI 网关**:Helicone(**注意:2026-03-03 起 maintenance mode,新项目不建议**)、Portkey

### 7. Experimentation / Rollout
- **Statsig**:AI copilot-like features 嵌入产品工作流;sequential testing、CUPED、switchback;可对 AI config 变更跑自动 benchmark + Release Pipelines + webhooks 阻断不安全 rollout
- **LaunchDarkly**:企业最稳选择
- **GrowthBook**:开源,成本敏感团队优选
- *关键 2026 用例*:把"prompt 版本"作为 feature flag,用 A/B 跑 prompt + model 组合实验

### 8. Project / Roadmap
- **Linear**:速度与键盘流,AI 自动 triage、打标签、预测时间线。PM 主流选择
- **Jira + Atlassian Intelligence**(Premium $18.30/seat/月):企业合规场景仍不可替代
- **Height**:原生 AI agent 自动维护项目状态
- **Plane**:开源 Linear/Jira 替代
- **Factory.ai AI Project Manager**:AI agent 直接管理 sprint(实验性)

### 9. Knowledge / RAG / 内部
- **Glean**:估值 $7.2B,600+ 企业客户,hybrid retrieval(lexical + vector + 知识图谱)。2026-05 重新定位为 "proactive AI coworker"
- **Notion AI**:小规模 + actively maintained 内容效果好,企业级缺乏 verification/freshness 强制
- **Cursor / Cursor Tab**:PM 也在用——Lenny 调研 7.7% PM 把 Cursor 列入工具栈
- **Microsoft Copilot Studio**:2026-05-05 发布 Agent 365 中央化管理 + Copilot Chat 对话层

### 10. PM 专属 AI 工具(2025-2026 新生)
- **ChatPRD**:PM 专用 PRD copilot,Lenny Insider 标配
- **BuildBetter**:通话录音 → 行动洞察的 Voice-of-Customer agent
- **Lindy**:PM 个人 AI 助理,跨日历/邮件/Slack 自动化
- **Pit**(2026-05 由 a16z 领投 €13.6M 退出隐身):以 SaaS 形式提供 "AI product team"
- **Granola**:Mac-native 会议记录,2026 发布 Spaces 团队工作区 + 个人/企业 API;动作项可直推 Asana/Notion/Monday
- **Otter.ai**:跨平台 + Zoom 集成

---

## 3.2 新人 AI PM "默认入门工具包"

| 用途 | 默认工具 | 月成本起步 |
|---|---|---|
| 日常对话/写作 | Claude Opus 4.7 + ChatGPT | $20 + $20 |
| 会议记录 | Granola | $18 |
| 用户洞察 | BuildBetter 或 Kraftful | $50-200 |
| PRD 撰写 | ChatPRD + Notion AI | $19 + $10 |
| 原型 | v0 + Lovable | $20 + $25 |
| Agent 实验 | Claude Code + Vercel AI SDK | $20 |
| Evals | Braintrust 免费层 | $0 |
| 项目管理 | Linear | $10 |
| 实验/发布 | Statsig 免费层 | $0 |
| 分析 | PostHog 免费层(1M events) | $0 |

**合计起步约 $210/月**,个人即可承担。

---

## 3.3 成本与 Token 经济(PM 必懂的 sidebar)

### 模型价格(2026-04)
| 模型 | $/MTok 输入 | $/MTok 输出 |
|---|---|---|
| GPT-4.1 Nano | $0.10 | $0.40 |
| Claude Haiku 4.5 | $1 | $5 |
| Gemini 1.5 Flash | $0.075 | $0.30 |
| DeepSeek V3 | $0.14 | $0.28 |
| GPT-4.1 | $5 | $15 |
| o3 reasoning | $15 | $60 |
| Claude Opus 4.7 | $5 | $25 |

### 关键经验事实
- 2025 年初到 2026 年初,LLM API 单价**整体下降约 80%**
- **输出 token 价格通常是输入的 4 倍**(中位数),多 step agent 这个比例更猛
- 同样任务,**agent 形态比 chatbot 多消耗 5-30 倍 token**;规划型 agent 与单次调用之间最高 70× 价差
- 一个生产 agent 跑 2000 conv/天、每 conv 1500 token,月 token 量 ~9000 万
- Reasoning token tax 不可忽视:GPT-5.5 Pro high reasoning 的 P50 TTFT 67 秒;Claude Opus 4.7 extended thinking 28 秒;Gemini 3 Pro Deep Think high 52 秒

### Eval run 成本参考
- 一次 500-case Braintrust eval 跑 GPT-4.1 mini 约 $1-3
- 同样跑 Opus 4.7 约 $30-80

> **PM 应当为每个 AI feature 预算 eval 成本而非只看推理成本。**

### PM 必须建立的预算控制
- **Iteration cap**(agent loop 最大次数)
- **Workflow-level token budget**
- **Retry limit**
- **Cost-per-task 度量**(优于 cost-per-token)
- 利用 OpenAI prompt caching(最高 90% off cached input)和 Batch API(50% off)

### 24/7 agent 月成本带宽
$49(托管 flat)→ $200+(自托管 + 变量)

---

## 3.4 PM 应当掌握的高频 Skills / Commands / Rules

- **Claude Code Skills**(SKILL.md + YAML frontmatter):`/plugin install pm-skills@claude-code-skills` 内含 Senior PM agent + 12 templates + 30+ frameworks(discovery、SaaS metrics、AI product craft)
- **Subagents**:长任务(code review、competitive analysis)放进 subagent 隔离 context;短交互保留主 agent
- **Cursor Rules**:4 类型(Always / Auto Attached / Agent Requested / Manual),PM 常用于约束 PRD 输出格式
- **MCP Servers**:BuildBetter 评出 2026 PM 必备 7 个 MCP(Linear、Notion、Figma、Slack、PostHog、GitHub、Granola)
- **常见 skills**:`/loop`(定时检查 PR/deploy)、`/review`、`/security-review`、`/init`、`session-start-hook`(Claude Code on the web 上跑测试与 lint)

---

## 3.5 最近 6 个月改变格局的产品事件

1. **2026-04-18 Claude Opus 4.7 GA** — 引入 effort control 和 task budget,PM 可在 PRD 中显式指定"思考强度"
2. **2026-04-15 OpenAI Agents SDK 大更新** — sandbox + harness,TypeScript 支持随后跟进
3. **2026-04 Anthropic 与 SpaceX 签 Colossus 1 数据中心协议** — 220k GPU、300MW,Claude API 容量瓶颈缓解
4. **2026-04 Claude Design 发布** — 直接对标 v0/Lovable,从现有 codebase 抽 design system
5. **2026-04 BMAD v6.6** — 把 PM/Dev/QA 都 agent 化,开源 spec-driven 团队成型
6. **2026-05-05 Microsoft Copilot Cowork + Agent 365** — 把 agent 当成"工作的操作系统层"
7. **2026-04 Cloud Next:Vertex AI → Gemini Enterprise Agent Platform** — Google 合并所有 agent 资产
8. **2026-03 Helicone 进入 maintenance mode** — 该赛道大洗牌信号
9. **2026-03 Dovetail redesign** — 推动一波团队迁移到 AI-native 替代品
10. **Kraftful 被 Amplitude 收购** — VoC 与产品分析正式融合
11. **2026-05 Granola Series C** — 推出 Spaces 和 Enterprise API

---

## 3.6 真采用 vs 炒作

**真在采用**:Lovable(8.7%)、Cursor(7.7%)、Granola、ChatPRD、Linear、Braintrust(CI 阻断 + 免费层)、Claude Code Skills、v0 个人组件、Notion AI 内部 Q&A

**炒作 > 实际**:
- "PM 完全被 AI 替代"——Lenny 调研显示 AI 在策略和路线图仅 1.1%
- "agent 自己想 idea"——a16z 直言 "the ideas are bland, derivative, and lack the spark"
- 端到端"7-agent 创业团队"——demo 惊艳但生产可靠性不足
- 给企业 GA 的"voice-of-customer agent 自动跑访谈"——技术可行,但合规与质量审核还没跟上

**真正改变 PM 工作方式的两件事**:
1. Eval 成为新的 PRD 单元
2. 原型成本降到 $0,使 PM 可以"先做后说"

---

## 资料源

- [Lenny's Newsletter — AI tools are overdelivering](https://www.lennysnewsletter.com/p/ai-tools-are-overdelivering-results)
- [Lenny's Newsletter — Why LinkedIn is replacing PMs with full-stack builders](https://www.lennysnewsletter.com/p/why-linkedin-is-replacing-pms)
- [a16z — Notes on AI Apps in 2026](https://a16z.com/notes-on-ai-apps-in-2026/)
- [Braintrust — Evals are the new PRD](https://www.braintrust.dev/blog/evals-are-the-new-prd)
- [Braintrust — Langfuse alternatives 2026](https://www.braintrust.dev/articles/langfuse-alternatives-2026)
- [Braintrust — LangSmith alternatives 2026](https://www.braintrust.dev/articles/langsmith-alternatives-2026)
- [Latitude — Best AI Agent Evaluation Platforms 2026](https://latitude.so/blog/best-ai-agent-evaluation-platforms-2026-comprehensive-comparison)
- [Laminar — Top 6 Agent Observability Platforms 2026](https://laminar.sh/article/2026-04-23-top-6-agent-observability-platforms)
- [QubitTool — 2026 AI Agent Framework Showdown](https://qubittool.com/blog/ai-agent-framework-comparison-2026)
- [BSWEN — Anthropic Agent SDK vs OpenAI Agents SDK vs Vercel AI SDK](https://docs.bswen.com/blog/2026-04-29-agent-sdk-comparison-anthropic-openai-vercel/)
- [Speakeasy — Agent framework comparison](https://www.speakeasy.com/blog/ai-agent-framework-comparison)
- [OpenAI — The next evolution of the Agents SDK](https://openai.com/index/the-next-evolution-of-the-agents-sdk/)
- [Vercel — AI SDK 5](https://vercel.com/blog/ai-sdk-5)
- [Vercel — AI SDK 6](https://vercel.com/blog/ai-sdk-6)
- [GitHub Blog — Claude Opus 4.7 GA](https://github.blog/changelog/2026-04-16-claude-opus-4-7-is-generally-available/)
- [Anna Arteeva — Choosing your AI prototyping stack](https://annaarteeva.medium.com/choosing-your-ai-prototyping-stack-lovable-v0-bolt-replit-cursor-magic-patterns-compared-9a5194f163e9)
- [Aurora Designs — Claude Design analysis](https://aurora-designs.ca/blog/claude-design-anthropic-labs/)
- [Statsig — GrowthBook vs LaunchDarkly](https://www.statsig.com/perspectives/growthbook-launchdarkly-feature-flags-comparison)
- [AI Productivity — Linear vs Jira 2026](https://aiproductivity.ai/blog/linear-vs-jira-2026/)
- [BuildBetter — Best MCP servers for PMs 2026](https://blog.buildbetter.ai/best-mcp-servers-for-product-managers-in-2026-top-7-tools-ranked/)
- [Silicon Data — LLM Cost Per Token Guide 2026](https://www.silicondata.com/blog/llm-cost-per-token)
- [Iternal — AI Token Usage Guide 2026](https://iternal.ai/token-usage-guide)
- [MarkTechPost — 9 Best AI Tools for Spec-Driven Development 2026](https://www.marktechpost.com/2026/05/08/9-best-ai-tools-for-spec-driven-development-in-2026-kiro-bmad-gsd-and-more-compare/)
- [ChatPRD](https://www.chatprd.ai/)
