# PM-to-Builder Knowledge Base

> **一个让"信息搬运型 PM"转型为"Builder PM / Agent PM"的完整知识库 + 工作流 + 技能插件**
> Agile PM Workflow · Knowledge Base v2 · 2026

> 🖼️ **[一页可视化导览图 →](./knowledge_base/infographic.html)** · 把 21 章 200+ 信源压缩到一屏的展览级 HTML(12 个 Panel,含交互式学习路径选择器、时间线、失败墙、定价决策树)。在浏览器里直接打开。

---

## 🎯 为什么需要这个项目

Meta / Google 前 CPO **Nikhyl Singhal** 在 2026 年公开预言:

> **"未来 12-24 个月将是 PM 历史上最混乱的时期 —— 公司会裁掉数千个'信息搬运工'PM,再重新招聘数千个 AI-first、技能完全不同、薪酬更高的 Builder。"**

Microsoft CPO **Aparna Chennapragada** 用两句话定义了新范式:

> **"Prompt sets are the new PRDs. NLX is the new UX."**

Hamel Husain & Shreya Shankar 把它再压缩为一句:

> **"Evals are the new PRDs."**

PM 这个职业正分化为两条路:
- **Information Mover**(信息搬运工)—— 靠协调、对齐、写文档存活 · **2026-2027 加速消失**
- **Builder PM / Agent PM**(建造者)—— 会用 AI 直接产出代码、原型、Agent 与 Eval · **薪资 $350K-$700K,且需求 +75%**

**本项目要做的事**:把"信息搬运 PM → Builder PM"的转型路径,做成一个从模板到技能到知识体系的完整工具包。

---

## 📦 项目三大支柱

```
PM-to-Builder Knowledge Base
│
├─ 1. 工作流模板  →  上手 (今天就能用)
│    pm_workflow_template/
│    对话式需求采集 → 详细 PRD → HTML 原型 → iframe 沙盒切片 → 版本管理
│
├─ 2. AI IDE Skill 插件  →  自动化 (一键开启)
│    agile-pm-workflow_skill/
│    Trae / Cursor / Claude Code / Codex / Gemini CLI 通用
│
└─ 3. Builder PM 知识库 v2  →  深度学习 (21 章 + 200 信源)
     knowledge_base/
     战略 / 工程 / 战术 三层完整体系
```

---

## 🌟 第一支柱 · 工作流模板(pm_workflow_template/)

打破传统 "先想清楚再写长篇文档" 模式,采用 **启发式对话 → 详细初版 PRD → 原型视觉化验证 → 逻辑细节补全** 的 7 步敏捷迭代。

### 核心特性

- 💬 **告别填表,对话式采集**:AI 主动评估 7 维度并温和追问,降低认知门槛
- 🎨 **原型先行验证**:复杂逻辑前先让 AI 产出高保真 HTML 原型,"看图说话"发现漏洞
- 🔗 **PRD 与原型双向联动**:同步迭代,所想即所见
- 🧩 **iframe 沙盒切片**:最终 PRD 中嵌入可交互原型,左边规则、右边真实界面
- 📂 **自动化项目初始化**:一键生成 `prd/ prototype/ flowcharts/ annex/` 标准目录
- 📈 **版本切换器**:物理隔离(v1.0 → v1.1 复制)+ PRD 右上角下拉切换

### 在任何 AI 助手里手动使用

1. 打开 `pm_workflow_template/pm_workflow_definition.md`,把全部内容发给 AI 助手
2. 附上最原始的想法(如:"给装修工人做打卡小程序,能拍照就行")
3. 跟随 AI 引导:**需求采集 → 目录架构 → 详细 v1 PRD → HTML 原型 → 流程图 → 内嵌原型最终版 PRD**

详见 [`pm_workflow_template/README.md`](./pm_workflow_template/README.md)。

---

## 🛠️ 第二支柱 · AI IDE Skill 插件(agile-pm-workflow_skill/)

已封装好的专属 Skill,Trae / Cursor / Claude Code / Codex CLI / Gemini CLI 都支持,输入 `/agile-pm-workflow` 即可一键开启。

### 安装

1. `git clone` 本仓库到本地
2. 找到 `agile-pm-workflow_skill` 文件夹
3. 复制到你的 AI 助手全局技能目录:
   - **Trae / Cursor**:Windows `C:\Users\你的用户名\.trae\skills\agile-pm-workflow`;Mac/Linux `~/.trae/skills/agile-pm-workflow`(或 `.cursor/skills/`)
   - **Claude Code**:`~/.claude/skills/`
   - **Gemini CLI**:`~/.gemini/skills/`
   *(没有 `skills` 文件夹请手动新建)*
4. 重启 AI IDE

### 使用

在对话框输入 `/agile-pm-workflow`,然后描述新项目想法。AI 自动建标准目录并引导你产出专业 PRD。

---

## 📚 第三支柱 · Builder PM 知识库 v2(knowledge_base/)

由 **3 轮 18 个并行研究 Agent 调研、跨 200+ 一手信源(Lenny's Newsletter / Anthropic Engineering / a16z / Latent Space / Hamel Husain / Eugene Yan / 五部门规章 / Maven 课程 / arXiv 等)交叉验证整合**而成的 21 章 Builder PM 完整体系。

### 三层结构

#### Part 1 · 战略基础(7 章 · Round 1)

| # | 章节 | 一句话 |
|---|------|-------|
| 01 | [角色重塑](./knowledge_base/01_role_transformation.md) | 8 项新职责、5 项被淘汰、FDE / Product Builder / Eval Engineer 新角色族 |
| 02 | [工作流范式](./knowledge_base/02_workflow_patterns.md) | 双循环飞轮、10 个工作流模式、取代 PRD 的工件清单 |
| 03 | [工具栈 2026](./knowledge_base/03_tooling_stack.md) | 9 大类工具盘点、$210/月入门工具包、Token 经济 |
| 04 | [评测体系](./knowledge_base/04_evaluation_metrics.md) | 5 级成熟度阶梯、18 个核心指标、Starter Eval Set、6 大陷阱 |
| 05 | [案例研究](./knowledge_base/05_case_studies.md) | Anthropic / Cursor / Linear / Notion / Lovable / Sierra 等 11 个团队 |
| 06 | [治理与安全](./knowledge_base/06_governance_safety.md) | 5 个失败复盘 + 全球三大监管 + 27 项发布前清单 |
| 07 | [行动手册](./knowledge_base/07_action_playbook.md) | 30/60/90 天落地 + 推荐阅读路径 |

#### Part 2 · 工程级深潜(8 章 · Round 2)

| # | 章节 | 一句话 |
|---|------|-------|
| 08 | [Eval Cookbook 工程级](./knowledge_base/08_eval_cookbook.md) | 10 个生产 prompt + 5 个开源 eval suite 解剖 + CI yaml |
| 09 | [多 Agent 架构](./knowledge_base/09_multi_agent_architecture.md) | LangGraph / Subagents / OpenAI SDK 真实代码 + Durable Execution + Memory + A2A |
| 10 | [协议生态](./knowledge_base/10_protocol_ecosystem.md) | MCP 2025-11-25 完整规格 + Skills frontmatter + AGENTS.md 6 区块 + AAIF |
| 11 | [Agent UX 模式库](./knowledge_base/11_agent_ux_patterns.md) | 9 类具名模式 + 真实产品案例 + 反模式 + 10 条 Heuristics |
| 12 | [红队与安全](./knowledge_base/12_red_team_safety.md) | Promptfoo / PyRIT / Garak 实战代码 + CaMeL + 5 层沙箱 + 三标准合规映射 |
| 13 | [Token 经济](./knowledge_base/13_token_economics.md) | 缓存 / Batch / Cascade / Loop budget 代码 + 5 类 Agent 成本基准 |
| 14 | [垂直行业 PM](./knowledge_base/14_vertical_pm.md) | Legal / Finance / Healthcare / Customer Service / Code 差异化 |
| 15 | [Career Playbook](./knowledge_base/15_career_playbook.md) | 13 门课程 + 18 道面试题 + 12 个 portfolio + 两份 12 周计划 |

#### Part 3 · 战术级 Playbook(5 章 · Round 3)

| # | 章节 | 一句话 |
|---|------|-------|
| 16 | [Anthropic Claude Code 案例](./knowledge_base/16_anthropic_claude_code_case.md) | 12 个月垂直作战实录:团队、季度时间线、日/周/月仪式、3 个废弃决定 |
| 17 | [中国 AI PM 全景](./knowledge_base/17_china_ai_pm.md) | 双轨格局 + Manus + 大厂矩阵 + 五部门监管 + 春节迭代 + 政企私有化 |
| 18 | [新兴前沿](./knowledge_base/18_emerging_frontiers.md) | RL by PMs / Memory / 世界模型 / Computer-Use / Voice / Q3-Q4 2026 必做 |
| 19 | [Pricing & Monetization](./knowledge_base/19_pricing_monetization.md) | 7 大定价模型 + 单位经济 + 5 个真实命名条款 + 8 节点决策树 |
| 20 | [PM 工件模板包](./knowledge_base/20_artifact_templates.md) | 12 个开箱即用模板(Agent PRD / Capability Card / Eval Set / AGENTS.md / Runbook 等) |

完整索引见 [`knowledge_base/README.md`](./knowledge_base/README.md)。

---

## 🚦 我应该怎么开始?(按角色推荐路径)

### 我是新晋 AI PM(转岗 ≤ 6 个月)
1. 装 Skill 插件,跑一遍工作流 ship 第一个原型
2. 知识库读:`01 → 04 → 08 → 03 → 13 → 15 → 20 → 07`
3. 30 天目标:用 Template 20 的 Agent PRD 写出自己第一份;用 Eval Set 模板跑 50 条 starter set

### 我是传统 PM 想转型(资深,5-10 年经验)
1. 先读 `01 角色重塑` —— 决定是走 Builder 还是 CPO/战略路线
2. 读 `16 Anthropic Claude Code 一年全程` —— 看 Builder PM 真实工作日常
3. 路径:`16 → 02 → 11 → 14 → 19 → 20 → 07`
4. 90 天目标:跑完 [Career Playbook](./knowledge_base/15_career_playbook.md) 里的 12 周转型计划

### 我是 PM Leader / CPO 想改造团队
1. 读 `05 案例研究` + `16 Anthropic` 看头部组织
2. 路径:`05 → 16 → 01 → 06 → 12 → 19 → 02 → 18`
3. 关键动作:把团队成员按 "Information Mover vs Builder" 分类;1-2 岗重定义为 Agent PM;设 Eval Engineer

### 我要发布 Agent 产品(已开发完)
**直接走** `06(27 项清单) → 12(红队工程) → 20(Template 7 + 8 + 12)`
**违 4 件套(Intent Preview / Stop Button / Audit Trail / 异常检测)之一即阻断上线**

### 我在中国市场做 AI 产品
**强烈推荐先读** `17(中国全景) → 14(垂直行业) → 19(定价反弹) → 06(合规)`,**否则会被西方视角误导**

---

## 🧭 10 条 Builder PM 心法(Cheat Sheet)

1. **Evals are the new PRDs** —— 把"完成"重新定义为"评估集 X 上达 Y 分,p95 延迟 ≤ Z 秒,$/task ≤ $C"
2. **Demos before memos** —— PRD 不先行,先 Lovable / v0 / Claude Code 出可点击原型,再补 spec
3. **Error Analysis First, then Evaluator** —— Hamel & Shankar 反对纯 EDD;先打开 50-100 条 trace 做开放式编码,再写 scorer
4. **Vibes → Spot-checks → LLM Judge → Online Evals** 是 4 周渐进硬化路径
5. **Pass@k(能力上限) vs Pass^k(一致性)双轨** —— 客户面 Agent 必须看 Pass^k
6. **只评 Final Output 会让通过率虚高 20-40%** —— Agent 评测必须看完整 Trajectory
7. **三段式部署** —— Offline Regression → Online Shadow → Production Canary,按 capability 灰度
8. **PM = Context Manager** —— 你的核心资产不是 PRD,而是结构化 markdown 上下文(`CLAUDE.md` / `AGENTS.md` / Skills)
9. **最小权限 + 破坏性操作必须 Human-in-the-Loop** —— PocketOS 9 秒删库、Step Finance $40M 共同根因都是 agent 被过度授信
10. **20-50 条来自真实失败的 starter eval 就足够开局** —— Anthropic 官方建议

---

## 🏷️ 适合谁?

- ✅ **传统 PM 想拿 $300K+ AI PM offer**:走 Part 1 → Part 2 → Part 3,12 周转型
- ✅ **新晋 AI PM 刚入职**:工作流 + Skill 立即上手,知识库随用随查
- ✅ **PM Leader 想重画团队岗位**:Part 1(01,05)+ Part 3(16)是组织重构的剧本
- ✅ **工程师跨界做 Builder PM**:Career Playbook 里有专门的"工程师 → AI PM 12 周计划"
- ✅ **AI Agent 创业团队**:Pricing(19)+ Templates(20)+ 安全清单(06,12)是发布前必备
- ✅ **中国市场 PM**:Part 3(17)填补西方视角缺失,合规 + 政企 + 春节迭代全覆盖

---

## 📅 维护与贡献

知识库 v2 已稳定。维护节奏:

- **每月**:扫 Lenny's Newsletter / Latent Space / Anthropic Engineering / Hamel.dev / Eugene Yan,补新工具与新实践
- **每季度**:重审 27 项治理清单,跟进 EU AI Act / 中国法规;更新 Token 价格表(Ch 13)
- **每半年**:复盘新增失败案例,纳入 Ch 6 + Ch 12;重做 Ch 16 Claude Code 案例
- **每年**:重写 Ch 1 角色定义、Ch 17 中国全景、Ch 18 前沿(迭代最快的三章)

每章末尾列出资料源 markdown 链接,可独立追溯。

---

> 💡 **进阶推荐**:让生成的 HTML 原型达到专业级设计效果,强烈建议搭配 [Impeccable Skills](https://impeccable.style/)(前端设计专家指令集)使用。
