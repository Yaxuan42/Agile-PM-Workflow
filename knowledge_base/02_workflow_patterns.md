# 第二章 · Agent 时代工作流范式

> 传统 PM 工作流是线性的:**Discovery → PRD → Design → Build → QA → Launch**。
> 进入 Agent 时代,主流工作流已演变为**双循环飞轮(Dual-Loop Flywheel)**。

---

## 2.1 宏观工作流图:双循环飞轮

由 Aakash Gupta 与 Braintrust CEO Ankur Goyal 在 2026 年提出:

```
[外环:Discovery 飞轮]
真实/合成用户信号 → 错误分析 (Error Analysis) → 数据集扩充 → 能力规格 (Capability Spec)
        ↑                                                            ↓
[内环:Eval 飞轮]
                Online Evals (线上评分) ← 部署 ← Offline Evals (离线评分)
                          ↓                                       ↑
                生产日志失败样本 → 加入 Golden Dataset → 修改 Prompt/Tool/Model
```

### 各阶段产出物(Artifacts)

- Discovery → Synthetic User 报告 + Jobs-to-be-Done 信号
- Specification → **Eval Set + Capability Card + AGENTS.md + MCP Tool Schema**(取代 PRD)
- Prototyping → Lovable / v0 / Bolt / Claude Artifacts 生成的可点击原型
- Validation → Eval Harness 报告 + Shadow Traffic 对比
- Shipping → 按 persona 的 Feature Flag + Canary + 自动 Rollback

Eugene Yan 在 *How to Work and Compound with AI*(2026-05-03)概括:
> **"Prompt is temporary. The eval is permanent."**

---

## 2.2 十个具体工作流模式

### 模式 1 · Error-Analysis-First(错误分析优先)
Hamel Husain 与 Shreya Shankar **反对纯 "Eval-Driven Development"**,主张先做错误分析再写评估器。

1. 收集 50-100 条真实生产 trace
2. PM 在 Notebook 中亲自打开式标注(open coding),不预设标签
3. 自下而上聚类失败模式(axial coding)
4. 针对**已发现**的失败写 LLM-as-judge 或代码断言
5. 进入持续 eval 循环

> Teresa Torres(产品发现教练,非 coder)在 Hamel 课程完成的案例被誉为"最佳公开示范"。

### 模式 2 · Data-Task-Scores 三件套(取代 PRD)
Braintrust 的标准范式:每个 AI 功能交付物 = `{Dataset, Task, Scorer}`,顶级团队每天跑 **12.8 个 eval 实验**。

- **Dataset**:合成 + 真实输入对(最少 100-150 条 / 功能)
- **Task**:被测的 prompt / agent / pipeline 配置
- **Scorer**:归一化到 0-1,使用类别评分而非自由数值(A=完整含引用 / B=部分 / C=无答案)

### 模式 3 · "Vibes → Evals" 渐进硬化

| 阶段 | 产出 | 周期 |
|------|------|------|
| Vibes | LGTM 主观判断 | 第 1-2 天 |
| Spot-checks | 5-20 条手工 case | 第 3-7 天 |
| Code/LLM Judge | 自动 scorer | 第 2 周 |
| Production Hardening | Online evals + 阈值阻断 | 第 3 周起 |

来源:Lenny's Newsletter *Beyond Vibe Checks: A PM's Complete Guide to Evals*

### 模式 4 · Spec-First Delegation(规格优先委托)
Eugene Yan 在 *How to Work and Compound with AI* 描述:PM/工程师同时跑 **3-6 个并行 Claude Code 会话**(用 git worktree 隔离),通过 tmux 状态 emoji(⏳ 进行 / 🟢 完成)+ 音频提醒异步监控。瓶颈从"写代码"转为"写清规格 + 快速 review"。

委托模板示例:
> "Given these eval suites, build isolated containers per suite, run full tests with n iterations for confidence intervals, generate report, verify it follows the guide, and slack results."

### 模式 5 · Pair Programmer 漂移检测
Eugene Yan 的"次级会话观察主会话"模式,区分两类 drift:
- **Execution drift**:忽略测试失败、用错指标
- **Direction drift**:偏离原始意图

口诀:"Check for execution drift often and direction drift occasionally."

### 模式 6 · Offline → Shadow → Canary 三段部署
*Building Production Evals for LLM Systems*(Vadim, 2026-02)总结的标准流水线:

1. **Offline Regression** —— Golden 数据集 + CI 集成 + 仪表盘
2. **Online Shadow** —— 镜像生产流量到候选版本(捕捉 golden set 之外的边缘)
3. **Production Canary** —— 自动 rollback 钩入生命周期控制

按 persona 灰度("Cloudflare Flagship"、AWS AppConfig、LaunchDarkly),按 capability 而非按功能滚动。

### 模式 7 · Synthetic Users 前置 Discovery
SyntheticUsers.com 的多代理架构以 OCEAN 人格 + chain-of-feeling 模拟 1000 人级访谈,2026 年与真人**奇偶校验度 85-92%**。

PM 标准流程:
1. 用 Synthetic Users 跑前 80% 发现型研究
2. 校准问题 + 收窄假设
3. 才把真人预算用在高价值访谈
4. 高决策点必须用真人验证

### 模式 8 · Curated Tool Surface(Agent-First 设计)
Stripe / Google / Anthropic 共同原则:**别暴露全部 API,只挑 10-20 个 autonomous-safe operations**。

PM 的新职责:
- 决定哪些 10-20 个操作允许 agent 自主执行
- 写**给模型读**的描述("when to use" 而非 "what it does")
- 在真实 agent 条件下测试 tool selection 准确率
- 设计权限边界(Stripe 模式:所有 mutation 强制 idempotency key)

### 模式 9 · Subagent "Product Trinity"
Claude Code subagent 模式(VoltAgent awesome-claude-code-subagents 仓库):
**Product Manager → UX Designer → Implementation Specialist** 三个 subagent 串行,把传统三人 / 一周的工作压缩到一个下午。

每个 subagent 维护独立 conversation history,主会话只传必要 context。

### 模式 10 · Pass@k vs Pass^k 上线门槛
Anthropic *Demystifying Evals for AI Agents*:
- **Pass@k**:至少 1 次成功 → 内部工具
- **Pass^k**:所有 k 次都成功 → 面向客户的 agent
- **Capability evals**(低初始通过率)vs **Regression evals**(接近 100%)的双轨制
- Capability eval 饱和后退役为 regression suite

---

## 2.3 取代 PRD 的工件清单

| 旧产物 | 新产物 | 工具/格式 |
|--------|--------|-----------|
| PRD 文档 | **Eval Set**(Data-Task-Scorer 三件套) | Braintrust / Langfuse / Maxim AI |
| 验收标准 | **Scoring Function** | LLM-as-judge rubric |
| 用户研究报告 | **Golden Dataset**(150+ 标注样本) | Arize Golden Dataset 模板 |
| 技术规格 | **AGENTS.md** + **MCP Tool Schema** | Linux Foundation Agentic AI Foundation 标准(Anthropic / Google / Microsoft / OpenAI 联合) |
| Sprint 周期 | **Eval Flywheel**(每天 12.8 实验) | Braintrust |
| Wireframe | **Vibe-coded Prototype** | Lovable / v0 / Bolt / Cursor |
| 用户访谈纪要 | **Synthetic User Transcripts** | SyntheticUsers Shuffle v2 |
| 上线 Checklist | **Verification Ladder**(cheap → expensive) | post-edit hooks → unit tests → LLM review |
| 功能文档 | **Capability Card**(含 pass@k 阈值 + scope) | 自定义 markdown |

> **AGENTS.md** 是 2025 末由 Google / OpenAI / Factory / Sourcegraph / Cursor 联合推出的跨平台标准,2026 由 Linux Foundation 下的 **Agentic AI Foundation** 治理。GitHub 对 2500+ 仓库分析得出 6 个核心区块:commands、testing、structure、style、git workflow、boundaries。

---

## 2.4 日 / 周节奏(Rhythms)

### 日常节奏(Daily)

- **9:00 Morning Eval Standup**(10 分钟)
  1. 拉昨日生产 logs
  2. 看失败 case 和 eval 分数
  3. 标记新型失败模式
  4. 加入 offline dataset
- **9:30-12:00 并行委托**:开 3-6 个 Claude Code 会话,用 git worktree 隔离
- **下午**:审 PR + 跑离线 eval 实验(目标 ≥10 次)
- **17:00 Transcript Review**:扫"can you also / did you check / still wrong"的更正模式,识别系统性 gap

### Aman Khan 的"个人 OS" rhythm
所有任务、优先级、context、目标 → 本地 markdown 文件 → Claude Code/Cursor 每天读取。PM 角色重定义为 **"Context Manager"**:每个决定都在思考"什么信息值得捕获、放哪、何时摘要"。

### 周节奏(Weekly)
- **周一**:Eval Review(眼看 transcript,calibrate LLM judge)
- **周三**:Capability Review(决定哪些 capability eval 升级为 regression)
- **周五**:Agent Retro —— 复盘代理失败案例 + 更新 CLAUDE.md / AGENTS.md / skills

### 配置作为复利记忆(Eugene Yan)
- `~/.claude/CLAUDE.md` 全局风格
- `repo-root/CLAUDE.md` 仓库约定
- `project/CLAUDE.md` 领域知识

每条规则只能存在一处,定期重构防 config decay。

---

## 2.5 多代理编排模式

### 框架对照(Gartner 预测:2026 年底 40% 企业部署 agent)

| 框架 | 模型 | 适合场景 |
|------|------|----------|
| **LangGraph** | 有向图 + 条件边 + 持久化 | 生产级、长时运行、人机协同 |
| **CrewAI** | 角色 / 任务 / Manager 范式 | 业务工作流原型、最容易上手 |
| **AutoGen** | 对话式协作 | 研究型多 agent |
| **OpenAI Agents SDK** | 内置安全 + 企业治理(2026-04 更新) | 企业内网部署 |
| **Google ADK + A2A** | Agent2Agent 协议 + 并行 function call | 跨服务编排 |
| **Anthropic Claude Code Subagents** | 独立 context + skills | 工程团队即用 |

### PM 编排的 5 个标准 Agent(Keren Koshman *PM Stack 2026*)

1. **Research & Discovery Agent** — 抓 G2 / Twitter / Reddit / 竞品评论
2. **Analytics & Insights Agent** — 自然语言转 SQL,跳过 BI 排队
3. **Spec & Documentation Agent** — 把要点扩成完整 spec
4. **Rapid Prototyping Agent** — Cursor / Claude Code 出 POC
5. **Internal Communication Agent** — 状态 / 周报 / release notes

### 主流编排范式
- **Hierarchical Planner-Worker**(最常见):中央 planner 拆任务,分派给 specialist worker
- **CrewAI Crew Pattern**:researcher / writer / reviewer 角色
- **Eval Subagent Pattern**(Eugene Yan):单独 subagent 读 transcript 验证 eval 是否跑对

---

## 2.6 命名方法论与框架(2025-2026 涌现)

| 方法论 | 提出者 | 核心 |
|--------|--------|------|
| **Eval-Driven Development (EDD)** | Braintrust / Red Hat | 评分函数 = 接受标准;无评估不发布 |
| **Error-Analysis-First** | Shreya Shankar / Hamel Husain | 反对 EDD 先写 evaluator;从真实失败发起 |
| **PM Thinking Stack(6 层)** | Esteban Forero | Problem → Context → Boundaries → Eval → Acceptance → Production Loop |
| **Learning Velocity > Release Velocity** | Presta AI | 衡量学习速度而非发布速度 |
| **OODA Loop for Vibe Coding** | Vladik Khononov | Observe-Orient-Decide-Act 套到 AI 反馈环 |
| **Pass@k / Pass^k 双轨** | Anthropic | 内部工具 vs 客户面 agent 不同门槛 |
| **Capability vs Regression 双轨** | Anthropic | 能力评估饱和后转入回归套件 |
| **Verification Ladder (Shift-Left)** | Eugene Yan | 从最便宜到最贵的验证排序 |
| **Personal OS(Markdown context)** | Aman Khan | PM = Context Manager |

---

## 2.7 关键洞察总结

1. **PM 的核心技能从"写 PRD"转向"定义 scoring function + 看 transcript"**:Hamel 称 evals 是 PM 头号新技能,Aakash Gupta 全套播客标题就是 *Evals are the new PRD*。
2. **"先建评估器再开发"是反模式**:Shreya Shankar 明确反对纯 EDD,因 LLM 失败面无限,应从真实错误出发。
3. **PRD 不再是单一文档,而是组合 artifact**:Eval Set + AGENTS.md + MCP Tool Schema + Capability Card + Golden Dataset。
4. **Agent-First 意味着设计的"用户"是模型**:tool description 写给 LLM 读,10-20 个安全操作而非全 API。
5. **顶级团队跑 12.8 eval 实验 / 天**(Braintrust 数据),日 standup 围绕生产失败案例。
6. **并行委托是新瓶颈**:Eugene Yan 一人开 3-6 个 Claude Code 会话,瓶颈在"写规格 + review 速度"。
7. **Synthetic Users 不替代真人,但承担前 80% 的发现工作**(2026 奇偶校验 85-92%)。
8. **三段式部署**(Offline Regression → Online Shadow → Canary)成为标准流水线。

---

## 资料源

**核心 PM-Eval 思想:**
- [Aakash Gupta — Evals are the new PRD for AI PMs (Ankur Goyal podcast)](https://www.news.aakashg.com/p/ankur-goyal-podcast)
- [Aakash Gupta — AI PM Learning Roadmap](https://www.aakashg.com/ai-pm-learning-roadmap/)
- [Hamel Husain — The Best Public Example of AI Evals](https://hamelhusain.substack.com/p/the-best-public-example-of-ai-evals)
- [Hamel Husain & Shreya Shankar — LLM Evals FAQ](https://hamel.dev/blog/posts/evals-faq/)
- [Lenny's Newsletter — Beyond Vibe Checks: A PM's Complete Guide to Evals](https://www.lennysnewsletter.com/p/beyond-vibe-checks-a-pms-complete)

**Eugene Yan / Anthropic 工程实践:**
- [Eugene Yan — How to Work and Compound with AI](https://eugeneyan.com/writing/working-with-ai/)
- [Anthropic — Demystifying Evals for AI Agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
- [Anthropic — Claude Code Subagents docs](https://code.claude.com/docs/en/sub-agents)

**Agent-First 设计:**
- [MindStudio — Agent-First Product Design Principles](https://www.mindstudio.ai/blog/how-to-build-agent-first-product-design-principles)
- [AGENTS.md 官方站](https://agents.md/)
- [VoltAgent — awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents)

**PM Stack & 节奏:**
- [Keren Koshman — The Product Manager Stack in 2026](https://medium.com/design-bootcamp/the-product-manager-stack-in-2026-agents-infrastructure-and-how-not-to-fall-behind-559dc7335140)
- [Aman Khan — Building AI Product Sense with a Personal OS](https://amankhan1.substack.com/p/building-ai-product-sense-with-a)
- [Esteban Forero — The PM Thinking Stack](https://www.estebanf.com/product-management/2025/12/05/the-pm-thinking-stack/)

**多代理编排:**
- [LangGraph vs CrewAI vs AutoGen — Gurusup 2026](https://gurusup.com/blog/best-multi-agent-frameworks-2026)
- [DataCamp — CrewAI vs LangGraph vs AutoGen](https://www.datacamp.com/tutorial/crewai-vs-langgraph-vs-autogen)
- [TechCrunch — OpenAI Agents SDK update (Apr 2026)](https://techcrunch.com/2026/04/15/openai-updates-its-agents-sdk-to-help-enterprises-build-safer-more-capable-agents/)

**Synthetic Users / Discovery:**
- [SyntheticUsers — Generative Agent Simulations of 1000 People](https://www.syntheticusers.com/science-posts/generative-agent-simulations-of-1-000-people)

**Eval 基础设施 & 部署:**
- [Vadim's Blog — Building Production Evals (Feb 2026)](https://vadim.blog/2026/02/03/building-production-evals-for-llm-systems)
- [Red Hat — Eval-Driven Development](https://developers.redhat.com/articles/2026/03/23/eval-driven-development-build-evaluate-ai-agents)
- [Braintrust — What is Eval-Driven Development](https://www.braintrust.dev/articles/eval-driven-development)
