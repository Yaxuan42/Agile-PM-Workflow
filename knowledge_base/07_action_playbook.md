# 第七章 · 行动手册(30/60/90 天落地计划)

> 本章是把前 6 章浓缩成可执行的落地路径。无论你是新晋 AI PM、传统 PM 转型,还是 PM Leader 改造团队,都可以从这里直接抄。

---

## 7.1 个人 PM 的 30/60/90 天计划

### Day 1-30:建立基线

**工具**(参见 [第 3 章](./03_tooling_stack.md)):
- 装齐"默认入门工具包"(Claude Opus 4.7 + Granola + ChatPRD + v0 + Lovable + Linear + Braintrust 免费层),起步预算 ~$210/月
- 在本地建 `~/.claude/CLAUDE.md` + 项目级 `CLAUDE.md` + `AGENTS.md`(三层 context)

**评估**(参见 [第 4 章](./04_evaluation_metrics.md)):
- 与工程师对齐 **50 条 starter eval set**(15 golden + 10 edge + 10 adversarial + 8 multi-turn + 5 tool-use + 2 sentinel)
- 接入 trace 日志(Langfuse / Braintrust 任选)
- 立"30 分钟法则":每周 30 分钟人工读 trace(Hamel & Shankar 推荐)

**节奏**(参见 [第 2 章](./02_workflow_patterns.md)):
- 每天上午 10 分钟 Morning Eval Standup:看昨日生产 logs + 失败 case + 标记新失败模式
- 周五 Agent Retro:复盘 + 更新 CLAUDE.md / Skills

### Day 30-60:Eval 上线 + 成本对账

- 上线 **LLM-as-judge**,维持 20-30% 人工校准
- 定义 **cost-per-successful-task** 与 **p95 latency** 两个非功能 SLO
- 建立 token 预算控制:iteration cap、retry limit、workflow-level token budget
- 选 1-2 个 public benchmark(τ-bench 或 GAIA 子集)做对外标杆
- 开始用 git worktree 做并行委托(目标:同时跑 2-3 个 Claude Code 会话)

### Day 60-90:生产硬化 + 安全合规

- 上 **Shadow Mode**:镜像生产流量到候选版本
- 引入 **trajectory 评分**(不只看终态)
- 跑一遍 [第 6 章](./06_governance_safety.md) 的 **27 项发布前安全清单**
- 制定季度红队节奏(jailbreak / leakage / prompt injection)
- 撰写 EU AI Act / 中国法规合规摘要(若产品涉及)

---

## 7.2 PM Leader 的团队改造路径

### 第 1 个月:盘点
1. 把团队成员按 Singhal 框架分类:**Information Mover** vs **Builder**
2. 评估当前产物:有多少团队还在写传统 PRD?Eval 集存在吗?
3. 看 7 个反模式信号(参见 [6.4 节](./06_governance_safety.md#64-pm-应当熟知的反模式信号))

### 第 2-3 个月:重画角色
1. 把 1-2 个 PM 岗位重定义为 **Agent PM** 或 **Product Builder**——JD 中明确"必须用 Claude Code 出货"
2. 建立 **Eval Engineer** 职能(可以是工程师兼任)
3. 与法务/合规同事建立 **Agentic AI Ethics Council**

### 第 4-6 个月:文化与节奏
1. 把 Sprint 周期换成 **Eval Flywheel**(目标:每周 ≥ 5 个 eval 实验)
2. 把发布门禁挂到 eval 上(Braintrust/LangSmith 的 CI 阻断)
3. 推行 "Demos before memos" 文化:每个新提案必须先有可点击原型

---

## 7.3 关键人物语录精选(可作团队 Slack 置顶)

> **"Evals are the new PRDs."**
> —— Hamel Husain & Shreya Shankar

> **"Prompt sets are the new PRDs. NLX is the new UX."**
> —— Aparna Chennapragada (Microsoft CPO)

> **"PM with a pretty light touch... I'm mainly there to clear the path."**
> —— Cat Wu (Anthropic, Head of Product, Claude Code)

> **"Because you can prototype in an afternoon, wrong bets are cheap."**
> —— Cat Wu, Anthropic Blog

> **"You celebrate when you actually solve the problem... product teams are about outcomes, they're not about output."**
> —— Marty Cagan

> **"Prompt is temporary. The eval is permanent."**
> —— Eugene Yan

> **"Demo value isn't user value."**
> —— Joshua Xu (HeyGen)

> **"Always start with error analysis. Don't jump into writing evals."**
> —— Hamel Husain & Shreya Shankar

> **"Prompts may make headlines, but evals quietly decide whether your product thrives or dies."**
> —— Aman Khan (Arize)

> **"Cost per successful task beats cost per token."**
> —— Silicon Data

---

## 7.4 推荐阅读路径(按角色)

### 新晋 AI PM(转岗 ≤ 6 个月)
1. [01 角色重塑](./01_role_transformation.md) → 理解 PM 这个职业的迁移
2. [04 评测体系](./04_evaluation_metrics.md) → 学会"新的 PRD"
3. [03 工具栈](./03_tooling_stack.md) → 装好工具
4. [07 行动手册](./07_action_playbook.md) → 第一个月的清单

### 传统 PM 转型(资深 PM,5-10 年经验)
1. [01 角色重塑](./01_role_transformation.md) → 看清楚要去哪里
2. [05 案例研究](./05_case_studies.md) → 看 Anthropic / Linear 怎么做
3. [02 工作流范式](./02_workflow_patterns.md) → 学新流程
4. [07 行动手册](./07_action_playbook.md) → 90 天落地

### PM Leader / CPO
1. [05 案例研究](./05_case_studies.md) → 头部团队组织样本
2. [01 角色重塑](./01_role_transformation.md) → 重画岗位族
3. [06 治理与安全](./06_governance_safety.md) → 治理框架
4. [02 工作流范式](./02_workflow_patterns.md) → 团队节奏改造

### Agent 产品发布前
1. **直接走** [06 治理与安全](./06_governance_safety.md) **的 27 项清单**
2. 配合 [04 评测体系](./04_evaluation_metrics.md) 检查 Pass^k 阈值

---

## 7.5 高质量信源订阅清单

**每周必读**:
- [Lenny's Newsletter](https://www.lennysnewsletter.com/) — PM 行业动态主阵地
- [Hamel.dev](https://hamel.dev/) — Eval 实践
- [Eugene Yan](https://eugeneyan.com/) — LLM 系统设计
- [Anthropic Engineering](https://www.anthropic.com/engineering) — 一手实践

**每月必扫**:
- [Latent Space Podcast](https://www.latent.space/) — 技术深度对谈
- [a16z Newsletter](https://a16z.com/newsletter/) — 行业资本视角
- [Aakash Gupta — News](https://www.news.aakashg.com/) — AI PM 专题
- [Aman Khan — Substack](https://amankhan1.substack.com/) — Eval 工程主义
- [TechCrunch AI](https://techcrunch.com/category/artificial-intelligence/) — 公司公告

**每季度必看**:
- [SWE-bench Leaderboards](https://www.swebench.com/) — Agent 编码能力
- [EU AI Act 官方](https://artificialintelligenceact.eu/) — 合规更新
- [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework) — 治理框架
- [Anthropic Blog](https://www.anthropic.com/news) / [OpenAI Blog](https://openai.com/news/) / [Google AI Blog](https://blog.google/technology/ai/) — 模型与 SDK 更新

---

## 7.6 维护本知识库的建议节奏

- **每月**:扫一遍核心信源,补充新工具与新实践
- **每季度**:重审 27 项治理清单 + 跟进监管更新
- **每半年**:复盘新增失败案例,纳入 [第 6 章](./06_governance_safety.md)
- **每年**:重写 [第 1 章](./01_role_transformation.md) 的角色定义——这个职业还在快速演化
