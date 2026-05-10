# 第十四章 · 垂直行业 PM 差异化

> 第五章在案例研究中提到了 Harvey、Glean。本章对 5 大垂直行业(法律 / 金融 / 医疗 / 客服 / 代码)做完整对比 + 与水平 PM 的差异。

**Bessemer 判断**:"垂直 AI 的市值将至少是传统 Vertical SaaS 的 10 倍。" 早期垂直 AI 公司年增速 ~400%,毛利率 65%。

---

## 14.1 Legal 法律:高单价、低容错、专家评估为王

### 头部产品

| 公司 | 估值 / ARR | 关键节点 | 定位 |
|------|-----------|---------|------|
| **Harvey** | **$11B 估值**(2026-03 GIC + Sequoia $200M) | LAB benchmark 2026-05 发布 | 大型律所 + 企业法务 agent 平台 |
| **Hebbia** | 服务全球 40% 头部资管 + 大量 Am Law 100 | 2026-03 与 Seyfarth Shaw 战略合作 | 文档分析 "agent swarm" |
| **EvenUp** | $2B+ 估值,ARR 同比翻倍 | 90% 新销售来自 2025 新品 | 人身伤害(PI)律所 demand letter 自动化 |
| **Spellbook** | $50M Series B + $40M 债权 | 2026-03 RBCx | 中小律所合同起草 |
| **Ironclad** | $150M ARR | — | 合同生命周期管理(CLM) |

### 垂直评估基准:Harvey LAB(2026-05-06 发布)

- **1,200+ agent 任务**,覆盖 **24 个法律执业领域**
- 由 **75,000+ 条专家撰写的 rubric criteria** 评分
- 模仿大型律所"分配-执行-审核"三段式工作流
- 任务为长程:分析复杂客户事项、综合证据、产出风险评估
- Harvey 故意未发布 leaderboard,称"希望与社区一同打磨"

> 这种 rubric-based 评估是**法律 PM 工作的核心**:每个任务必须由 5-10 个执业律师撰写评分准则,PM 要负责设计 rubric template、招募 reviewer pool、做 inter-rater reliability 校准。

### 合规约束(PM 必须遵守的硬红线)

- **ABA Model Rule 1.6(保密义务)**:客户数据不能用于训练通用模型,必须 tenant 隔离
- **Model Rule 5.5(未授权执业 UPL)**:agent 不能给出"法律建议",只能产出"草稿供律师审阅"
- **律师-委托人特权**:审计日志本身可能成为对方律师取证目标 —— **必须设计 privilege-preserving logging**
- **Bar Association 各州指引**:纽约州、加州、佛州 2024-2025 陆续发布 GenAI 律师执业指南,要求律师对 AI 输出承担 "reasonable verification" 义务

### 标志性失败:Mata v. Avianca

2023-06-22,Castel 法官对在 ChatGPT 生成的虚假判例(含虚构航空公司名称、伪造判决引文)上签字提交的 Levidow 律所两位律师处以 **$5,000 罚款**。这是法律 AI PM 的"原罪事件" —— 所有产品都必须解决 "**hallucination = malpractice**" 等式。Harvey 因此构建了 **multi-step citation verification pipeline**,每条引文都必须回链到 Westlaw/Lexis 的真实数据库 ID。

### 关键金句

> "The legal industry is now well past AI as an assistant and officially in the era of legal agents."
> —— Winston Weinberg, Harvey CEO(30 岁)

### 定价

- Harvey:**~$288K/年/律所席位包**(覆盖 ~100 律师,按席位)
- Hebbia:六位数年合同,含 dedicated AI Strategist
- EvenUp:按 demand letter 数量(per-deliverable, $500-$2,000/case)

---

## 14.2 Finance 金融:审计追溯优先、模型风险即生命线

### 头部产品

| 公司 | 估值 / ARR | 关键节点 | 定位 |
|------|-----------|---------|------|
| **Hebbia** | 服务全球 40%+ 头部资管 by AUM | 2026-04 Disclosure 发布 | 投行/PE/资管文档智能 |
| **AlphaSense** | **$200M+ ARR**, $4B+ 估值 | 2026-01 推出 Generative Search agent | 市场情报 + research agent |
| **Rogo** | $2B 估值, $160M Series D(2026-04 Kleiner)、25,000 用户/250+ 机构 | — | 投行 analyst agent |
| **BlackRock Aladdin Copilot** | 服务 **$11T+ AUM** | 2024-09 GA | 投资管理平台内嵌 copilot |
| **Bloomberg GPT** | 内嵌 Terminal | — | 专有金融语料模型 |

### 垂直评估基准:FinanceBench

Patronus AI 2023-11 发布:
- 完整集 **10,231 道题**(公开版抽样 75 道)
- 基于 SEC 10-K / 10-Q / 8-K、earnings call transcripts
- 4 大能力维度:数值推理、信息检索、逻辑推理、世界知识
- **关键发现**:GPT-4-Turbo + RAG 在 150 题样本中**错答或拒答 81%**

### 合规约束

- **SR 11-7(美联储模型风险管理)**:2026-04-17 Fed/FDIC/OCC 发布**修订版**,要求 LLM 同样适用 "shift-left" 治理
  - 全程**统一审计追溯**(数据 + 特征 + 模型 + 监控 + 文档)
  - 关键数据:AI/ML 占大型银行模型库存约 50%,但仅 **26.4%** 金融机构对自身 AI 合规准备度有信心
- **SEC Rule 17a-4**:所有客户通讯必须 immutable 存档 7 年
- **FINRA 21-19**:算法交易 supervision 要求扩展到 LLM 生成的 research

### 金融 PM 与水平 PM 的差异

- 必须设计 **citation-grounded UI**:每个数字必须可点击回到原始 10-K 第几页
- 必须有 **MRM workstream**:与 second/third line of defense(合规、内审)并行迭代
- **不能 A/B test**:Reg FD 要求所有客户得到同等信息,禁止灰度
- **数据 sourcing licensing**:Bloomberg、S&P、Refinitiv 数据 redistribution 条款是合同细节

### 定价

- Hebbia:六位数 enterprise contract(seat × usage)
- AlphaSense:~$15K-$30K/seat/year
- Rogo:tier-based seat + custom workflow
- BlackRock Aladdin Copilot:捆绑在 Aladdin 主合约中(按 AUM 比例)

---

## 14.3 Healthcare 医疗:FDA + HIPAA 双轨监管

### 头部产品

| 公司 | 估值 / ARR | 关键节点 | 定位 |
|------|-----------|---------|------|
| **Abridge** | $5.3B 估值(2025-06 a16z $300M Series E)、**Q1 2025 contracted ARR $117M**;2025 支持 5000 万次问诊 | 客户:Kaiser、Mayo、Hopkins、Duke、UPMC、Yale | 环境式临床记录 |
| **OpenEvidence** | $12B 估值(2026-01 Series D $250M);月处理 1,500 万次 clinical consultation | 2026-03 嵌入 Mount Sinai Epic | 临床证据查询 agent |
| **Hippocratic AI** | $3.5B 估值;**1.15 亿次**临床患者交互、零安全事件 | 50+ 健康系统 / 1,000+ use cases | 患者面对的 voice nurse agent |
| **Nabla** | 通过 AMI Labs $1.03B seed(2026-03)独家拿到 world model 优先权 | — | 法/美双区环境式 scribe |
| **Suki / Rad AI** | Abridge+Ambience+Rad+Nabla 合计 ~$7.7B 估值 | — | Suki:独立诊所;Rad AI:放射科报告 |

### 垂直评估基准

- **MedQA**(USMLE 风格 1,273 测试题):2026-04 leaderboard:o4 Mini High **95.2%**、Gemini 2.5 Pro 94.6%、Claude 3.7 Sonnet 92.3%、GPT-5 95.84%
- **MedHELM**(Stanford CRFM):5 类目 / 22 子类 / **121 临床任务**
- **MedS-Bench**:11 任务类别 + 39 数据集
- **各厂商自建 eval**:Abridge 用真实多专科对话 corpus(55 个专科 / 28 种语言);Hippocratic 用 1,000 名持照护士做"图灵测试"

### 合规约束

- **HIPAA BAA**:2026 §164.308 风险管理条款更新,3 年以上 BAA 必须复审
- **FDA AI/ML Lifecycle Guidance**(2024-12 最终版 + 2025-08 增补):
  - **PCCP(Predetermined Change Control Plan)**:可在不重新提交 marketing application 情况下迭代模型
  - 当前**仅 16.7%** 已批准 ML-enabled device 报备 PCCP —— **巨大合规缺口**
- **州级 Wiretapping 法律**:CA / IL 2025 集体诉讼指控 ambient scribe 未获明确同意构成窃听
- **CMS reimbursement codes**:2025 新增 ambient AI documentation 的 G code

### 医疗 PM 与水平 PM 的差异

- 必须有 **clinical advisory board**(持照 MD + RN)
- **patient-facing 产品**必须先拿 IRB approval 跑临床研究再上线
- **可解释性是产品需求**而非加分项:每条 SOAP note 都要 provenance trace 到具体对话片段
- 与 EHR 集成(Epic、Oracle Cerner)周期 = **6-18 个月**

### 关键金句

> "We are actually going to have 1,000 nurses interacting with our large language model as if they're patients... Only when they think it's safe will we launch it."
> —— Munjal Shah, Hippocratic AI CEO

### 定价

- Abridge:provider seat / 月($200-$400/医生/月)+ 健康系统 enterprise floor
- Hippocratic:**outcome-based / per-call** + 平台 license fee
- OpenEvidence:免费给医生(B2B2C),向药厂 + 健康系统收费
- Nabla:seat + EHR 分发抽成

---

## 14.4 Customer Service 客服:outcome-based pricing 最强样板

### 头部产品

| 公司 | 估值 / ARR | 关键节点 | 定位 |
|------|-----------|---------|------|
| **Sierra** | **$15B 估值**(2026-05 Tiger + GV $950M); ARR 2025-11 $100M → 2026-02 **$150M**;Fortune 50 中 40% 客户 | 推出 Ghostwriter | 通用客服 + 客户体验 agent OS |
| **Decagon** | **$4.5B 估值**(2026-01 $250M);2025-10 **$35M ARR** | 客户:Hertz、Chime、Oura | 全渠道 concierge agent |
| **Cresta** | ~$150K/年 base;**首个 ISO 42001 认证**联络中心 AI | 2026-03 Knowledge Agent | 大型联络中心 agent assist |
| **Forethought** | **被 Zendesk 2026-03 收购** | 中位 $59.5K/年 | 中型 SaaS 客服 |
| **Intercom Fin** | **$0.99 / 已解决会话** | — | 生态内最便宜 outcome 定价 |

### 垂直评估基准:τ-Bench

Sierra 开源的 **τ-bench / τ³-bench**:
- 模拟客户服务多轮对话(航空、零售场景)
- text half-duplex + voice full-duplex
- τ³-Bench 扩展到 **τ-Knowledge**(大批内部文档检索)+ **τ-Voice**(实时语音)
- 4 个 attribute 评估:coherence、repetitiveness、grounding in fact、sentiment
- 还有 **supervisor 模型并行运行** —— "guardrail-as-product" 范式

### 合规约束

- **PCI DSS**:AI 接触卡号场景必须分层 —— keypad input → 隔离 vault → AI agent 看不到 raw PAN
  - 违规罚款:每月 $5,000 - $100,000
  - Sierra 2025 宣布 **"industry-first PCI-compliant agents"**
- **GDPR / CCPA**:每次 escalation 必须保留人工 fallback
- **EU AI Act 高风险分类**:金融服务客服已划入 high-risk 等级

### 标志性反转:Klarna

2024-02,Klarna 与 OpenAI 合作的 chatbot 宣称"做了 700 名全职 agent 的工作"。**2025 CEO Sebastian Siemiatkowski 公开承认走得太远**,重新雇佣人工客服。**这是垂直 AI PM 必须铭记的 "deflection-only" 反面教材。**

另一关键判例:**Moffatt v. Air Canada**(2024-02)—— 加航 chatbot 错误承诺 bereavement fare 退款,法庭判加航**赔偿 $812.02 + 利息**,明确"公司无法以 chatbot 是独立法律实体"为由免责。

### 客服 PM 与水平 PM 的差异

- **指标体系**全行业重写:
  - 传统:CSAT、AHT、FCR
  - AI 时代:**Deflection rate**、**Resolution rate**、**Escalation rate**、**Containment rate**、**Cost per resolution**
- 必须做 **shadow mode 部署**:先并行人类 vs AI 对比 4-8 周再切流量
- **每个客户是独立 instance**:tenant 隔离 + brand voice fine-tuning
- **outcome SLA 写进合同**:例如 95% resolution rate 未达成则按比例退款

### 定价对比

| 厂商 | 模型 | 价格 |
|------|------|------|
| **Intercom Fin** | per resolution | **$0.99** |
| **Zendesk AI** | per automated resolution | $1.50 - $2.00 |
| **Decagon** | platform fee $50K + per-conversation OR per-resolution | per-resolution ~$0.50 |
| **Sierra** | 完全 outcome-based + 大客户定制 | 不公开,per-customer 协商 |
| **Cresta** | 年合同 | ~$150K/year base |
| **Forethought** | 年合同 | ~$59.5K/year median |

### 标杆指标

- **Decagon 平台均值**:80%+ deflection rate、65% 支持成本下降、93% agent quality score

---

## 14.5 Code 代码:工程师即用户、PM 角色被根本性挑战

### 头部产品

| 公司 | 估值 / ARR | 关键节点 | 定位 |
|------|-----------|---------|------|
| **Cursor (Anysphere)** | **$50B 估值**(2026-04 a16z + Thrive);**ARR 0 → $2B 用 3 年**(2026-02);年底预测 $6B+ | **~50 名员工、零 PM、零 PMO** | AI native IDE |
| **Cognition (Devin)** | Devin 2.0 SWE-bench Verified 45.8%;2025-07 收购 Windsurf | — | 异步 SWE agent("junior engineer") |
| **Replit Agent** | Claude 3.7 Sonnet 编排,SWE-bench Lite 第二 | — | 全栈生成 + 部署 |
| **GitHub Copilot Workspace** | 内嵌 GitHub | — | issue→PR 自动化 |
| **Anthropic Claude Code** | 2024-05 发布 | SWE-bench Verified Mythos **93.9%**、Opus 4.7 87.6% | 终端原生 agent |
| **Augment / Codeium-Windsurf** | 部分被 Cognition 收购 | — | 企业代码 search + 补全 |

### 垂直评估基准:SWE-bench

**SWE-bench Verified**(500 道人工核验过的真实 GitHub issue)已成为代码 agent 的"圣杯":
- 2026-05 leaderboard:**Claude Mythos Preview 93.9%**、Claude Opus 4.7 (Adaptive) 87.6%、GPT-5.3 Codex 85.0%、Claude Opus 4.5 80.9%
- **SWE-bench Pro**(Scale AI)更难、更抗污染:Claude Opus 4.5 在 SEAL 板 45.9% 领先
- Cognition Devin 故意只做 single-agent unassisted(**45.8%**),强调 "real and reproducible"

### Cursor 的"无 PM"组织(范式挑战)

- ~50 人公司 / **$2B ARR**(2026-02)→ **史上最高 revenue-per-employee**
- **零 PM、零 PMO、零中层管理** —— 工程师直接对话用户、识别问题、构建解决方案、当天发版
- 创始人 CEO Michael Truell(25 岁,前 Google intern)每日仍写代码

**这对垂直 PM 实践提出根本挑战**:**当用户即是工程师(PM 自己),传统 PRD-driven 流程被绕过。** 但企业销售(IPO 前合规、SOC 2、Procurement)仍需要 GTM PM,Cursor 已开始招聘 enterprise PM。

### 代码 PM 与水平 PM 的差异

- **PM 必须自己写代码**:无法靠 user research 替代亲身使用
- **dogfooding 比一切更重要**:Cursor / Anthropic 内部团队是首批用户
- **eval = 自动化测试集**:SWE-bench / 内部 repo 回归测试 → **可以每天迭代模型**
- **失败的代价是开发者信任**:一个 over-eager refactor PR 可能让用户卸载

### 定价

- Cursor:**$20/月 Pro 个人**、$40/月 Business、企业版定制
- GitHub Copilot:$10-$39/月/seat
- Devin:**$500/月起**(每月 ACU 单元)+ 企业 enterprise tier
- Claude Code:按 token usage(API-based)
- Replit Agent:使用量 + seat 混合

---

## 14.6 横向 vs 垂直 PM:综合差异表

| 维度 | 水平 PM(Notion/Slack) | 垂直 PM(5 大行业平均) |
|------|----------------------|----------------------|
| **用户研究** | 5-10 用户访谈 | 必须聘 ex-从业者作 SME(律师/MD/RN/banker) |
| **评估机制** | A/B test、CSAT | 行业 benchmark + 专家 rubric + 双盲临床/法律审查 |
| **监管/合规** | GDPR、SOC 2 | HIPAA + FDA / SR 11-7 / Bar Rules / PCI DSS / EU AI Act high-risk |
| **失败成本** | churn | 起诉、罚款、患者伤害、bar 调查 |
| **数据 sourcing** | 通用 web | licensed industry data(Bloomberg、Westlaw、SNOMED) |
| **集成周期** | API 即用 | EHR / Aladdin / iManage / Salesforce 6-18 月 |
| **定价** | seat | **outcome-based + platform fee + usage** 三段混合(客服最显著) |
| **发版节奏** | 双周 / 季度 | 每个 model upgrade 必须重跑全 benchmark;FDA/MRM submission 锁版本 |
| **PM 招聘** | 通用 PM 转岗 | **ex-行业从业者 + 技术背景 = 独角** |
| **可解释性** | nice-to-have | 产品级硬性需求 |
| **审计追溯** | log | immutable + privilege-preserving + examiner-traceable |

### 关键洞察

1. **outcome-based pricing 是垂直 AI 的标志**:客服 ($0.99/resolution)、法律(per demand letter)、医疗(per call)已成主流;PM 对每个 transaction 的 unit economics 极度敏感
2. **专家评估流程是垂直 PM 的护城河**:Harvey 75K rubric、Hippocratic 1000 RN 双盲、Patronus FinanceBench —— 这些不是工具,而是组织能力
3. **"PM 必须会做 SME 工作"是新常态**:Cursor 让工程师当 PM、Abridge 创始人本身是医生、EvenUp PM 团队多为 ex-PI 律师
4. **失败案例正在重塑监管**:Mata、Air Canada、Klarna 三起标志性事件已实质改变 PM 必须遵守的合规边界

---

## 资料源

### 法律
- [Harvey Legal Agent Benchmark](https://www.harvey.ai/blog/introducing-harveys-legal-agent-benchmark)
- [Harvey - $11B 融资公告](https://www.harvey.ai/blog/harvey-raises-at-dollar11-billion-valuation-to-scale-agents-across-law-firms-and-enterprises)
- [Sacra - Harvey revenue 数据](https://sacra.com/c/harvey/)
- [Mata v. Avianca Wikipedia](https://en.wikipedia.org/wiki/Mata_v._Avianca,_Inc.)
- [Hebbia 官网](https://www.hebbia.com/)
- [EvenUp Series E $150M](https://www.evenuplaw.com/blog/evenup-2b-valuation/)

### 金融
- [Patronus AI - FinanceBench](https://www.patronus.ai/announcements/patronus-ai-launches-financebench-the-industrys-first-benchmark-for-llm-performance-on-financial-questions)
- [arXiv 2311.11944 - FinanceBench paper](https://arxiv.org/abs/2311.11944)
- [Rogo $160M Series D](https://siliconangle.com/2026/04/29/rogo-raises-160m-speed-financial-analysis-ai-agents/)
- [BlackRock Aladdin Copilot](https://www.blackrock.com/aladdin/solutions/aladdin-copilot)
- [Databricks - SR 11-7 2026 修订指南](https://www.databricks.com/blog/model-risk-management-2026-bankers-guide-revised-interagency-guidance)

### 医疗
- [Abridge Series E $300M](https://www.abridge.com/blog/series-e)
- [Fortune - Abridge Shiv Rao 专访](https://fortune.com/2025/07/02/abridge-ceo-shiv-rao-on-raising-300-million-a-prospective-ipo-and-the-future-of-healthcare/)
- [Hippocratic AI Series C $126M](https://hippocraticai.com/hippocratic-ai-announces-series-c-funding-126-million/)
- [FDA AI/ML guidance](https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-software-medical-device)
- [npj Digital Medicine - AI Scribes 风险](https://www.nature.com/articles/s41746-025-01895-6)

### 客服
- [Sierra $950M / $15B 估值](https://techcrunch.com/2026/05/04/sierra-raises-950m-as-the-race-to-own-enterprise-ai-gets-serious/)
- [Sierra - τ-Bench 介绍](https://sierra.ai/blog/benchmarking-ai-agents)
- [Sierra - PCI compliant agents](https://sierra.ai/blog/payments)
- [Decagon $4.5B 估值](https://techcrunch.com/2026/03/04/decagon-completes-first-tender-offer-at-4-5b-valuation/)
- [Decagon - Pricing the AI Agent Economy](https://decagon.ai/resources/pricing-ai-agents)
- [OpenAI - Klarna case study](https://openai.com/index/klarna/)
- [Moffatt v Air Canada 分析](https://www.mccarthy.ca/en/insights/blogs/techlex/moffatt-v-air-canada-misrepresentation-ai-chatbot)

### 代码
- [Anysphere Wikipedia](https://en.wikipedia.org/wiki/Anysphere)
- [Lenny - Michael Truell 专访](https://www.lennysnewsletter.com/p/the-rise-of-cursor-michael-truell)
- [SWE-bench 官方 leaderboard](https://www.swebench.com/)
- [Lenny - Scott Wu / Devin 专访](https://www.lennysnewsletter.com/p/inside-devin-scott-wu)

### 行业 / 趋势
- [a16z - Big Ideas 2026 Part 1](https://a16z.com/newsletter/big-ideas-2026-part-1/)
- [Bessemer - Building Vertical AI 2026](https://www.bvp.com/assets/uploads/2026/01/BUILDING-VERTICAL-AI_PDF_BESSEMER_VENTURE_PARTNERS_BOOK_JANUARY_2026.pdf)
