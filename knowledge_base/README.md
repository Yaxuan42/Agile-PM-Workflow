# Agent 时代 PM 知识库 (2026)

> **范围**:2026 年 3 月之后的产品经理工作流与工作范式最佳实践
> **更新日期**:2026-05-10
> **来源**:由 5 个研究 Agent 并行调研、跨 100+ 一手信源(Lenny's Newsletter、Anthropic Engineering、a16z、Latent Space、Hamel Husain、Eugene Yan、TechCrunch、MIT Tech Review、各公司官方博客等)交叉验证整合

---

## 为什么需要这个知识库

进入 2026 年,PM 这个职业正经历自诞生以来最剧烈的重构。Lenny's Newsletter 的 AI 相关内容占比从 2023 年初的 4% 飙升到 2026 Q1 的 67%。Microsoft CPO Aparna Chennapragada 把这场转变压缩为两句话:**"Prompt sets are the new PRDs"** 与 **"NLX is the new UX"**。

旧范式:Discovery → PRD → Design → Build → QA → Launch
新范式:**双循环飞轮** —— 外环是"真实信号 → 错误分析 → 数据集扩充 → 能力规格";内环是"Offline Eval → Shadow → Canary → 生产日志回流"

PM 的核心交付物不再是 PRD 文档,而是 **Eval Set + AGENTS.md + MCP Tool Schema + Capability Card + Golden Dataset** 的组合。Hamel Husain 与 Shreya Shankar 的论断已成共识:**"Evals are the new PRDs"**。

---

## 目录结构

| 章节 | 文件 | 一句话内容 |
|------|------|-----------|
| **00 总览** | `README.md`(本文) | 入口、导航、心法清单 |
| **01 角色重塑** | [`01_role_transformation.md`](./01_role_transformation.md) | PM 从"信息搬运工"到"Agent 编排者"的 8 项新职责、5 项被淘汰的旧职责、邻位新角色族(FDE / Product Builder / Eval Engineer) |
| **02 工作流范式** | [`02_workflow_patterns.md`](./02_workflow_patterns.md) | 双循环飞轮、10 个具体工作流模式、取代 PRD 的工件清单、日/周节奏 |
| **03 工具栈 2026** | [`03_tooling_stack.md`](./03_tooling_stack.md) | 9 大类别完整工具盘点、新人 AI PM "默认入门工具包"、Token 经济学 sidebar |
| **04 评测体系** | [`04_evaluation_metrics.md`](./04_evaluation_metrics.md) | 5 级评估成熟度阶梯、18 个核心指标、Starter Eval Set 模板、6 大常见陷阱 |
| **05 案例研究** | [`05_case_studies.md`](./05_case_studies.md) | Anthropic / Cursor / Linear / Notion / Figma / Lovable / Microsoft / Sierra / Perplexity / Glean / Harvey 等 11 个头部团队的真实运作方式 |
| **06 治理与安全** | [`06_governance_safety.md`](./06_governance_safety.md) | EU AI Act / US AI Action Plan / 中国法规要点、5 个失败复盘、27 项发布前安全清单 |
| **07 行动手册** | [`07_action_playbook.md`](./07_action_playbook.md) | 30/60/90 天落地计划、关键人物语录、推荐阅读路径 |

---

## 10 条核心心法(Cheat Sheet)

如果只能记 10 条,记这些:

1. **Evals are the new PRDs** —— 把"完成"重新定义为"在评估集 X 上达到 Y 分,且 p95 延迟 ≤ Z 秒,单任务成本 ≤ $C"。
2. **Demos before memos** —— PRD 不再先行,先用 Lovable / v0 / Claude Code 出可点击原型,再围绕原型补 spec。
3. **Error Analysis First, then Evaluator** —— Hamel & Shankar 反对纯 EDD;先打开 50-100 条真实 trace 做开放式编码,再写评分函数。
4. **Vibes → Spot-checks → LLM Judge → Online Evals** 是 4 周渐进硬化路径,不要一上来就追求完整 eval 框架。
5. **Pass@k(能力上限) vs Pass^k(一致性)双轨** —— 内部工具用 Pass@k,客户面 Agent 必须看 Pass^k。
6. **只评 Final Output 会让通过率虚高 20-40%** —— Agent 评测必须看完整 Trajectory。
7. **三段式部署** —— Offline Regression → Online Shadow → Production Canary,按 capability 灰度而非按 feature。
8. **PM = Context Manager**(Aman Khan)—— 你的核心资产不是 PRD,而是结构化的 markdown 上下文(`CLAUDE.md` / `AGENTS.md` / Skills)。
9. **最小权限 + 破坏性操作必须 Human-in-the-Loop** —— PocketOS 9 秒删库、Step Finance $40M 损失,共同根因都是 agent 被过度授信。
10. **20-50 条来自真实失败的 starter eval 就足够开局** —— Anthropic 官方建议,小样本下早期变更效应量大。

---

## 适用场景

- **新晋 AI PM 入职**:按 `01 → 04 → 03 → 07` 路径学习,2 周内建立工作模型
- **传统 PM 转型 AI PM**:按 `01 → 05 → 02 → 07` 路径学习,理解差异 → 看真实案例 → 学新流程
- **PM Leader 重构团队**:按 `05 → 01 → 06 → 02` 路径学习,看头部团队组织 → 重画角色 → 设治理 → 落工作流
- **Agent 产品发布前 review**:直接走 `06_governance_safety.md` 的 27 项清单
- **工具选型**:直接查 `03_tooling_stack.md` 的分类对照表

---

## 维护说明

本知识库的每篇文档末尾都列出**资料源**(markdown 链接),便于追溯与更新。建议节奏:

- **每月**:扫一遍 Lenny's Newsletter / Latent Space / Anthropic Engineering / Hamel.dev / Eugene Yan,补充新增工具和实践
- **每季度**:重审 27 项治理清单,跟进 EU AI Act / 中国法规更新
- **每半年**:复盘一次本知识库引用的失败案例,纳入新的 post-mortem
