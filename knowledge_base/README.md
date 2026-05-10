# Agent 时代 PM 知识库 v2 (2026)

> **范围**:2026 年 3 月之后的产品经理工作流与工作范式最佳实践
> **更新日期**:2026-05-10
> **版本**:v2(经 3 轮 18 个研究 Agent 调研整合,跨 200+ 一手信源交叉验证)
> **来源**:Lenny's Newsletter / Anthropic Engineering / a16z / Latent Space / Hamel Husain / Eugene Yan / TechCrunch / MIT Tech Review / 各公司官方博客 / 中国信通院 / 五部门规章 / Maven 课程 / arXiv / Levels.fyi 等

---

## 为什么需要这个知识库

进入 2026 年,PM 这个职业正经历自诞生以来最剧烈的重构。Lenny's Newsletter 的 AI 相关内容占比从 2023 年初的 4% 飙升到 2026 Q1 的 67%。Microsoft CPO Aparna Chennapragada 把这场转变压缩为两句话:**"Prompt sets are the new PRDs"** 与 **"NLX is the new UX"**。

旧范式:Discovery → PRD → Design → Build → QA → Launch
新范式:**双循环飞轮** —— 外环是"真实信号 → 错误分析 → 数据集扩充 → 能力规格";内环是"Offline Eval → Shadow → Canary → 生产日志回流"

PM 的核心交付物不再是 PRD 文档,而是 **Eval Set + AGENTS.md + MCP Tool Schema + Capability Card + Golden Dataset** 的组合。Hamel Husain 与 Shreya Shankar 的论断已成共识:**"Evals are the new PRDs"**。

---

## v2 vs v1

**v1(初版,7 章)**:战略骨架 —— 角色 / 工作流 / 工具 / 评测 / 案例 / 治理 / 行动手册

**v2(本版,21 章)**:在 v1 基础上 **新增 13 章工程级深潜与战术级 Playbook**,覆盖:Eval Cookbook、多 Agent 架构、协议生态、UX 模式库、红队工程、Token 经济、垂直行业、Career Playbook、一年全程深度复盘、中国 AI PM 全景、新兴前沿、Pricing 决策学、12 个可填模板。

---

## 目录结构

### Part 1 · 战略基础(v1 章节)

| 章节 | 文件 | 一句话 |
|------|------|-------|
| **00 总览** | `README.md`(本文) | 入口、导航、心法清单 |
| **01 角色重塑** | [`01_role_transformation.md`](./01_role_transformation.md) | PM 从"信息搬运工"到"Agent 编排者"的 8 项新职责、5 项被淘汰、邻位新角色族(FDE / Product Builder / Eval Engineer) |
| **02 工作流范式** | [`02_workflow_patterns.md`](./02_workflow_patterns.md) | 双循环飞轮、10 个工作流模式、取代 PRD 的工件清单、日/周节奏 |
| **03 工具栈 2026** | [`03_tooling_stack.md`](./03_tooling_stack.md) | 9 大类别完整盘点、新人 AI PM 入门工具包、Token 经济学 sidebar |
| **04 评测体系** | [`04_evaluation_metrics.md`](./04_evaluation_metrics.md) | 5 级成熟度阶梯、18 个核心指标、Starter Eval Set、6 大陷阱 |
| **05 案例研究** | [`05_case_studies.md`](./05_case_studies.md) | Anthropic / Cursor / Linear / Notion / Figma / Lovable / Microsoft / Sierra 等 11 个团队 |
| **06 治理与安全** | [`06_governance_safety.md`](./06_governance_safety.md) | 5 个失败复盘 + 全球三大监管框架 + 27 项发布前清单 |
| **07 行动手册** | [`07_action_playbook.md`](./07_action_playbook.md) | 30/60/90 天落地 + 推荐阅读路径 + 信源订阅清单 |

### Part 2 · 工程级深潜(v2 新增 · Round 2 调研)

| 章节 | 文件 | 一句话 |
|------|------|-------|
| **08 Eval Cookbook 工程级** | [`08_eval_cookbook.md`](./08_eval_cookbook.md) | Hamel & Shankar 完整方法论 + 10 个生产 prompt + 5 个开源 eval suite 解剖 + CI yaml + Calibration Loop + 5 个 post-mortem |
| **09 多 Agent 架构** | [`09_multi_agent_architecture.md`](./09_multi_agent_architecture.md) | LangGraph / Subagents / OpenAI SDK 真实生产代码 + Durable Execution + Memory + OTel GenAI + A2A |
| **10 协议生态** | [`10_protocol_ecosystem.md`](./10_protocol_ecosystem.md) | MCP 2025-11-25 完整规格 + Skills frontmatter + AGENTS.md 6 区块 + AAIF 治理 + 选型决策矩阵 |
| **11 Agent UX 模式库** | [`11_agent_ux_patterns.md`](./11_agent_ux_patterns.md) | 9 类具名模式 + 真实产品案例 + 反模式 + 10 条 Heuristics |
| **12 红队与安全** | [`12_red_team_safety.md`](./12_red_team_safety.md) | Promptfoo / PyRIT / Garak 实战代码 + CaMeL + 5 层沙箱 + 三标准合规映射 |
| **13 Token 经济** | [`13_token_economics.md`](./13_token_economics.md) | 缓存 / Batch / Cascade / Loop budget 代码 + 5 类典型 Agent 成本基准 + 价格战预测 |
| **14 垂直行业 PM** | [`14_vertical_pm.md`](./14_vertical_pm.md) | Legal / Finance / Healthcare / Customer Service / Code 五大 vertical 的 PM 差异化打法 |
| **15 Career Playbook** | [`15_career_playbook.md`](./15_career_playbook.md) | 13 门头部课程 + 18 道真实面试题 + 12 个 portfolio + 两份 12 周计划 + 薪资数据 |

### Part 3 · 战术级 Playbook(v2 新增 · Round 3 调研)

| 章节 | 文件 | 一句话 |
|------|------|-------|
| **16 Anthropic Claude Code 案例** | [`16_anthropic_claude_code_case.md`](./16_anthropic_claude_code_case.md) | 12 个月垂直作战实录:团队、季度时间线、日/周/月仪式、3 个废弃决定、路线图碎片 |
| **17 中国 AI PM 全景** | [`17_china_ai_pm.md`](./17_china_ai_pm.md) | 双轨格局 + Manus + 大厂 Agent 矩阵 + 五部门监管时间轴 + 春节迭代 + 政企私有化 + 价格反弹 |
| **18 新兴前沿** | [`18_emerging_frontiers.md`](./18_emerging_frontiers.md) | RL by PMs / Memory / 世界模型 / Computer-Use / Voice / 多模态 / Test-Time Compute / Marketplaces — Q3-Q4 2026 必做清单 |
| **19 Pricing & Monetization** | [`19_pricing_monetization.md`](./19_pricing_monetization.md) | 7 大定价模型 + 单位经济 + 5 个真实命名条款 + 8 节点决策树 + 7 失败案例 |
| **20 PM 工件模板包** | [`20_artifact_templates.md`](./20_artifact_templates.md) | 12 个开箱即用模板:Agent PRD / Capability Card / Eval Set / AGENTS.md / SKILL.md / MCP Spec / Safety Review / Runbook / Migration / OKR / Interview / Post-mortem |

---

## 10 条核心心法(Cheat Sheet)

如果只能记 10 条,记这些:

1. **Evals are the new PRDs** —— 把"完成"重新定义为"在评估集 X 上达到 Y 分,且 p95 延迟 ≤ Z 秒,单任务成本 ≤ $C"
2. **Demos before memos** —— PRD 不再先行,先用 Lovable / v0 / Claude Code 出可点击原型,再围绕原型补 spec
3. **Error Analysis First, then Evaluator** —— Hamel & Shankar 反对纯 EDD;先打开 50-100 条真实 trace 做开放式编码,再写评分函数
4. **Vibes → Spot-checks → LLM Judge → Online Evals** 是 4 周渐进硬化路径
5. **Pass@k(能力上限) vs Pass^k(一致性)双轨** —— 内部工具用 Pass@k,客户面 Agent 必须看 Pass^k
6. **只评 Final Output 会让通过率虚高 20-40%** —— Agent 评测必须看完整 Trajectory
7. **三段式部署** —— Offline Regression → Online Shadow → Production Canary,按 capability 灰度而非按 feature
8. **PM = Context Manager**(Aman Khan)—— 你的核心资产不是 PRD,而是结构化的 markdown 上下文(`CLAUDE.md` / `AGENTS.md` / Skills)
9. **最小权限 + 破坏性操作必须 Human-in-the-Loop** —— PocketOS 9 秒删库、Step Finance $40M 损失,共同根因都是 agent 被过度授信
10. **20-50 条来自真实失败的 starter eval 就足够开局** —— Anthropic 官方建议

---

## 适用场景与推荐路径

### 新晋 AI PM 入职(转岗 ≤ 6 个月)
**学习路径**:`01 → 04 → 08 → 03 → 13 → 15 → 20 → 07`
理解角色 → 学评测 → 工程级 eval → 装工具 → 算成本 → Career 路径 → 拿可填模板 → 落地

### 传统 PM 转型 AI PM(资深 PM,5-10 年经验)
**学习路径**:`01 → 16 → 02 → 11 → 14 → 19 → 20 → 07`
理解迁移 → 看 Anthropic 全过程 → 学新工作流 → 学 UX 模式 → 看垂直差异 → 学定价 → 拿模板 → 行动

### PM Leader / CPO 重构团队
**学习路径**:`05 → 16 → 01 → 06 → 12 → 19 → 02 → 18`
看头部组织 → 看 Anthropic 一年全程 → 重画岗位 → 设治理 → 红队工程 → 重构定价 → 改造工作流 → 看前沿

### Agent 产品发布前 review
**直接走** `06 (27 项清单) → 12 (红队工程) → 20 (Template 7 + 8 + 12)`

### 工具选型
直接查 `03 (工具栈) + 09 (多 Agent) + 10 (协议) + 13 (成本)`

### 中国市场 PM
**强烈推荐先读** `17 (中国全景) → 14 (垂直行业) → 19 (定价反弹) → 06 (合规)`,再读其余章节;**否则会被西方视角误导**

---

## 维护说明

每篇文档末尾都列出**资料源**(markdown 链接),便于追溯与更新。

### 维护节奏

- **每月**:扫一遍 Lenny's Newsletter / Latent Space / Anthropic Engineering / Hamel.dev / Eugene Yan,补充新增工具和实践
- **每季度**:重审 27 项治理清单,跟进 EU AI Act / 中国法规更新;更新 Token 价格表(Ch 13)
- **每半年**:复盘新增失败案例,纳入 Ch 6 + Ch 12;重做 Ch 16 的 Claude Code 案例(把上一季度的进展加进去)
- **每年**:重写 Ch 1 角色定义、Ch 17 中国全景、Ch 18 前沿 —— 这些章节迭代速度最快

### 调研方法

本知识库由 3 轮 18 个并行研究 Agent 调研整合而成:

- **Round 1**(5 个 Agent):战略层 wide scan —— 角色 / 工作流 / 工具 / 评测 / 案例治理
- **Round 2**(8 个 Agent):工程级 deep dive —— Eval / 多 Agent / 协议 / UX / 安全 / Token / 垂直 / Career
- **Round 3**(5 个 Agent):战术级 Playbook —— Claude Code 案例 / 中国 / 前沿 / Pricing / 模板

每个 Agent 平均深读 25-50 个一手信源,跨 100+ 不同来源(包括 GitHub 仓库源码 / 厂商规格文档 / 学术论文 / 法规原文 / 一线 PM 公开访谈),交叉验证。
