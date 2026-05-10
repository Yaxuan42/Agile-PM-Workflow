# 第十八章 · 新兴前沿(Q3-Q4 2026 / 2027)

> Round 1 + 2 覆盖了"现在"。本章是前瞻边疆 —— 8 大方向 + 量化拐点 + Q3-Q4 必做清单。

---

## 18.1 PM 主导的偏好微调(RFT / DPO / RLAIF)

### 现状

- **OpenAI RFT**:2025-05 在 o4-mini GA,2026-04 扩展到 Global Training(12+ 区域低价节点)和 GPT-4.1 grader
- **Anthropic Model Spec Midtraining (MSM)**:2026-Q1 公开论文,把"模型规范"作为合成中训练数据
- **Microsoft Foundry**:2026-04 把 RFT 引入 Azure OpenAI

### 关键案例

- **Accordance AI(税务)**:RFT 微调 o4-mini → 复杂税务推理 **+39% 精度**,超所有领先闭源模型
- **Ambience Healthcare(ICD-10 编码)**:相对医生基线提升 **12 个百分点**
- **Harvey(法律引用抽取)**:F1 提升 **20%**,与 GPT-4o 同精度但 inference 更快
- **Sierra(客服 agent)**:fine-tune 开源模型 + 专有偏好数据,以"模拟过往真实 ticket 后再灰度"作为标准发布流程

### 量化拐点

- RFT 公开 GA 后,**单条 reward run 起步成本降到 ~$50**
- 2026-Q1,**70% 上市企业 AI 团队报告至少跑过一次 DPO**
- DPO 相比完整 RLHF **节省 40-75% 算力**且不需独立 reward model

### PM 必做(Q3-Q4 2026)

1. **建立"反馈数据"流水线**:日志埋点必须捕获 ≥ 3 类信号 —— 明确点赞/点踩、用户改写后的"对的版本"、agent 执行成功/失败结果
2. **写出第一份 Reward Rubric**:每个核心场景(≤5 个)写成"chosen vs rejected"的判定准则。**这份文档将取代部分 PRD 成为新的产品规约**
3. **Q4 前跑一轮 DPO POC**:Together.ai / Fireworks / OpenAI RFT,预算 < $5k,验证 rubric 是否能产出可度量的提升(首轮目标 +5pp)

### 角色 / 技能转变

PM 从"写需求"转向"**设计 reward function、策划 preference 数据集、审核 grader 输出**"。要懂统计显著性、偏置审核(rater inter-annotator agreement)、reward hacking 监控。

### 风险

- **Reward hacking**:模型学会优化 grader 而非真实用户价值
- **数据隐私**:用户对话被用于 fine-tune,必须在 ToS 与"数据 opt-out"上做明确分层
- **模型漂移**:RFT 后模型能力可能在非目标维度退化

---

## 18.2 记忆架构主流化

### 五大中间件生态

- **Mem0**:自适应个人化,适合 chatbot、客服 agent,"轻库"形态
- **Zep**(Graphiti 引擎):时序知识图谱,每个事实带 validity window,"X 在二月归谁负责"这种实体演化追踪
- **Letta**(前 MemGPT):把记忆变成 agent 状态的一等公民,可编辑 memory blocks
- **Cognee**(GraphRAG):多文档,企业知识库 / 法律 / 研究
- **Cloudflare Agent Memory**:基础设施级,按调用计费

### 应用样本

- **Notion 3.3 Custom Agents**(2026-02-24):自动选择 Claude / GPT / Gemini,运行 24/7,跨 Notion / Slack / Mail / Calendar / Figma / Linear
- **ChatGPT Memory(GPT-5.5 时代)**:cross-conversation context 默认开启,新增 "Memory Sources" 按钮可见性
- **Claude Projects + Managed Agents**:长 session、自动记忆管理,已在 Notion 私测
- **Replit Agent**(2026-02):Background memory compression,接入生产 Deployment 日志

### 学术关键

- arXiv 2603.07670《Memory for Autonomous LLM Agents》提出 write-manage-read 闭环
- arXiv 2604.16548《Mnemonic Sovereignty》专题综述长期记忆攻击面
- arXiv 2601.01885《Agentic Memory (AgeMem)》把记忆操作做成 tool action

### PM 必做

1. **画出产品的"记忆图谱"**:哪些是 session 内 working memory、哪些是 episodic、哪些是 semantic,分别用什么后端、保存多久
2. **设计"记忆 UX"三件套**:**可见(用户能看到 AI 记住了什么)、可编辑(用户能改)、可遗忘(一键删除 + 软删除追溯)**。Simon Willison 2025 那篇 "我不喜欢 ChatGPT 新 memory dossier" 是必读警示
3. **过 GDPR/SOC2 关**:必须能按用户、按时间窗、按 entity 三种维度删除。Zep 的 temporal graph 因此在欧洲企业销售上有结构性优势

### 风险

- **记忆中毒(memory poisoning)**:恶意输入污染长期记忆,Mnemonic Sovereignty 列为头号攻击向量
- **跨用户泄漏**:多租户必须严格 namespace 隔离
- **成本失控**:memory fetch 不当让每次调用变成 N 次 RAG

---

## 18.3 世界模型与具身智能

### 资本拐点(2026-Q1 至 Q2)

- **AMI Labs(Yann LeCun)**:2026-03 关闭 **$1.03B 种子轮 @ $3.5B pre-money**,押注 JEPA。投资人含 Bezos Expeditions、Eric Schmidt、Tim Berners-Lee
- **World Labs(Fei-Fei Li)**:2025-11 发布 **Marble**(首个商业化 world model);2026-02 关闭 $1B,估值 $5B
- **DeepMind Genie 3**(11B 参数自回归 transformer):**720p 24fps 实时可导航世界,且无硬编码物理引擎**;2026-01-29 公开,2-19 对 AI Ultra 用户开放
- **Wayve GAIA-3**(15B 参数)+ Series D **$1.5B**(NVIDIA、Uber、Mercedes-Benz、Nissan、Stellantis)
- **2026-Q1 累计世界模型赛道融资 > $1.3B**

### 量化指标

- **Genie 3 一致性维持几分钟** —— 仍是关键限制
- AMI Labs 公开路线图:2027 年发布工业控制 / 可穿戴 / 机器人专用世界模型
- LeCun 多次声称:**纯 LLM 永远无法达到 AMI(Advanced Machine Intelligence),世界模型是必经之路**

### PM 必做

1. **医疗/教育/工业 PM**:评估"基于交互的合成数据"是否能解决你领域的数据稀缺(用 Genie 3 / Marble 生成训练 / 评测场景)
2. **机器人/仿真公司 PM**:把 world model 列入 **2027 路线图的"核心依赖"**而非"探索"
3. **投资/战略 PM**:识别哪些产品现在的卖点 6 个月后会被 world model 化的工具替代(3D 资产生成、游戏关卡、训练仿真)

### 风险

- 世界模型一致性/因果性远未解决,过度承诺会重蹈 2024 自动驾驶覆辙
- 监管空窗:合成医学影像、合成驾驶场景的合规标准尚未成型

---

## 18.4 Computer-Use Agents 走向成熟

### 2026-Q2 旗舰对比(OSWorld-Verified)

| 模型 | OSWorld-Verified | 备注 |
|------|----------------|------|
| Claude Opus 4.7(4-16 发布) | **78.0%** | 视觉分辨率 3× 提升,XBOW visual-acuity 98.5% |
| GPT-5.5(4-23 发布) | ~76% | 内置 Computer Use |
| GPT-5.4 | 75.0% | **首次超越 72.4% 人类基线** |
| Mythos Preview | 79.6% | 实验性 |
| **2024 Q4 SOTA** | ~38% | 一年内翻倍 |

WebVoyager:CUA 87%、Mariner 83.5%、Computer Use 56%。

### 主要 player

- **Anthropic Computer Use API**(Claude 4.7) + Claude for Chrome
- **OpenAI Operator → Workspace Agents**(2026-04-22 发布)
- **Google Project Mariner**(Gemini 3 驱动)
- **Perplexity Personal Computer**(2026-04-16,Mac mini 常驻 agent)
- **Microsoft Agent 365** + ServiceNow AI Control Tower 联合治理

### 关键事实:可靠性曲线

ServiceNow / Microsoft 的 WAREX 论文证明:**default benchmark 上 80% 成功率的 agent,遇到 DNS 抖动、partial page load 时成功率可掉到 30-40%** —— 真实生产环境的 SLA 远未到位。

a16z 估计 2026 浏览器 agent 市场规模 **$12B,YoY +200%**。

### PM 必做

1. **写出"agent 接管前置条件"**:什么场景允许 agent 自主点击、什么必须人类 approve、什么允许 agent 在 sandbox 内自由探索。**Perplexity Personal Computer 的 "sandbox + auditable + reversible" 是当前最清晰范式**
2. **设计失败模式 UX**:当 agent 卡住时如何"优雅交回控制" —— 当前所有 demo 视频回避的难题
3. **做"网络抖动 / 部分加载"压测**:不要只在 happy path 上跑 OSWorld,**模拟 30% failure rate 下的整体任务完成率,作为 GA 的 release gate**

### 风险

- **prompt injection**:网页里塞恶意指令让 agent 越权操作(已有多起攻击 Mariner / Operator 的公开 demo)
- **审计证据链**:每一步动作必须可溯源、可回滚

---

## 18.5 Voice-First Agents 规模化

### 主要 player

- **OpenAI Realtime API GA**:speech-to-speech 单模型,固定 voice catalogue
- **ElevenLabs v3 + ElevenAgents**:Speech Arena #2,接入 MCP/API 可直接执行 CRM 写入、预约、支付
- **Sesame AI(Sequoia 投资)**:Maya 与 Miles 是 2026 上半年最受关注的"自然度"标杆,对话节奏会因情境调整
- **Vapi**:**62M 通话/月、99.99% SLA**,14+ provider 接入,$0.05/min orchestration 起
- **Retell AI**:HIPAA / SOC2 / GDPR 默认开启,$0.07/min,**医疗与金融服务事实标准**
- **Hippocratic AI**:voice nurse 持续放量,首个能拿到 Joint Commission 类证书的 AI 角色

### 关键工程指标

- **Barge-in 发生率:约 1/5 的电话** —— 无法处理打断的 voice agent 永远不可能像人
- **目标延迟:< 300 ms** end-to-end 才能让对话自然
- 转折点检测:从 VAD 升级到 **turn-taking model**(韵律 + 语义 + pause)

### PM 必做

1. **制定 Voice Persona Spec**:声音(音色 + 速率 + 韵律)、可打断阈值、failure handoff、情绪映射(用户激动时 agent 应降速 / 降音)
2. **跑"打断质量"评测**:不要只测 ASR 准确率,要测 **"用户尝试打断 → agent 在 200ms 内停止 → 接住用户新意图" 的端到端成功率**
3. **合规分层**:医疗、金融、催收的录音同意 / 数据保留 / disclosure 要在产品 onboarding 里写死

### 风险

- 情绪伪装与 "AI 不告诉用户它是 AI" 的合规雷区(加州 SB 1047 / 欧盟 AI Act 都已立法要求披露)
- 噪声环境下的 ASR 退化与跨语言切换

---

## 18.6 多模态 Agent(视觉 + 工具 + 语音)

### 旗舰对比(2026-Q2)

| 基准 | Gemini 3 Pro | GPT-5.5 | Claude Opus 4.7 | Qwen 3.5 Omni |
|------|-------------|---------|----------------|--------------|
| MMMU-Pro | 81.0% | ~81% | ~82% | ~83% |
| Video-MME (long) | **78.4%** | 71.2% | 67.8% | 69.5% |
| DocVQA | 90.8% | 91.5% | **93.0%** | 87.9% |
| 长文档(50p+ PDF) | 领先 5-8 pp | | | |

### 关键产品

- **NVIDIA Nemotron 3 Nano Omni**:单模型统一 vision + audio + language,9× 效率
- **LiveKit Agents**:voice + vision + tool 默认接入
- **Roboflow + Gemini 3 Pro**:视觉任务 SOTA
- **Apple Intelligence Browse**(仍未上线)+ Apple Q2 财报承认 Perplexity 是 Mac 平台首选企业 AI 助手

### PM 必做

1. **重新评估 MMMU-Pro 的指标价值**:基准已饱和,应用层应该转向 **task-specific 评测** —— 你的客户真实截图、真实 PDF、真实视频
2. **视觉的"安全副输入"**:用户上传图片时必须有 **prompt-injection-via-image 的过滤层**(已有多个攻击 PoC)
3. **建立 video-token 成本预算**:1 小时视频 ≈ 数十万 tokens,按月计费产品要算清楚 unit economics

---

## 18.7 Test-Time Compute / 推理时 Scaling

### 三大厂"思考预算"产品形态

- **Anthropic**:adaptive thinking + effort 参数(low / medium / high / max),Opus 4.7 与 Sonnet 4.6 默选 high
- **OpenAI**:`reasoning.effort` ∈ {none, low, medium, high, xhigh},GPT-5.5 默选 medium。Thinking Effort 控件已下放到 ChatGPT 模型选择器
- **Google Gemini 3 Deep Think**:并行思考 —— 同时生成多个假设、并评再合并;**ARC-AGI-2 84.6%、HLE 48.4%(无工具)、Codeforces 3455 Elo**

**定价**:思考 token 按 output 标准计费,**没有溢价 —— 但可以轻易 10-20× cost**。

### PM 必做

1. **决定要不要把"思考强度"暴露给用户**:参考 Cursor 的 "Auto / Slow / Max" 模式,让用户自己拍板成本/质量 trade-off
2. **按场景预设默认 effort**:客服闲聊用 low,合同审查用 high。**这本身就是 PRD 里的一行决策**
3. **建立"thinking ROI"指标**:跟踪 *单次 thinking token 投入 vs 用户满意度提升* —— 很多产品默认开 thinking 但实际无收益

### 风险

- 思考链泄露用户敏感推理(已发现 chain-of-thought 包含用户 PII)
- 长思考引发的"超时 UX 黑洞"(用户等 30s 不知道发生了什么)

---

## 18.8 Agent 生态与市场:分发渠道分化

### 主要分发面板

- **OpenAI Workspace Agents**(2026-04-22):取代 Custom GPTs,60+ 企业集成 + custom MCP servers + 长任务调度
- **Salesforce Agentforce in ChatGPT**(2025-12-22):Agentforce 直接嵌入 ChatGPT
- **Microsoft Agent 365 + ServiceNow AI Control Tower**(Knowledge 2026):跨 AWS / Azure / GCP / SAP / Oracle / Workday 25+ 系统统一治理
- **Claude Skills Marketplace**(2025-12 Skills 规约开放):免费上架,无平台分成;通过 hosted access 间接变现
- **GPT Store**:按美国用户 engagement 付分成
- **AgentExchange、Hugging Face Spaces、Replit Agent Market、LangChain Hub、Vercel Agent Gallery、Cloudflare AI Marketplace** 各占垂直生态

**Skills 规约的胜利信号**:Anthropic 12 月开源、OpenAI 同期采纳、Adobe CX Enterprise 在 2026-04 把它列为标准 —— **成为继 MCP 之后第二个跨厂商标准**。

### PM 必做

1. **决定上架战略**:你的 agent 应该在哪 1-2 个 marketplace 首发?
   - B2B → AgentExchange + Microsoft Agent 365
   - C 端 → GPT Store + Claude Skills
   - 开发者 → LangChain Hub + Vercel Gallery
2. **变现模型选型**:
   - 付费 SKILL 文件**几乎注定失败**(IP 一次性流失)
   - **hosted access(按调用 / 月 / 结果)是当前唯一规模化路径**
   - 托管在 Workers AI / Bedrock 上做 inference markup 是 mid-market 选择
3. **治理与上架审批**:ServiceNow AI Control Tower + Microsoft Agent 365 联合 vetting 流程开始普及,企业上架必须满足 permission/audit 双门槛

### 风险

- **生态绑定**:在 OpenAI Workspace Agents 内深度集成,意味着模型迁移成本结构性上升
- **抽成不确定**:GPT Store 创作者支付公式不透明、Claude Skills 完全无平台分成 —— 商业模型仍在博弈

---

## 18.9 综合判断:PM 角色的三层重构

| 层级 | 2024 PM | 2026-Q4 PM |
|------|---------|-----------|
| **战略** | 选模型、写 PRD | 选模型 × 选记忆栈 × 选分发面板的三维矩阵决策 |
| **执行** | Prompt 工程、RAG 调优 | Reward Rubric 设计 + DPO 数据策展 + Agent UX(接管/失败/记忆) |
| **运营** | A/B test、留存 | Reward hacking 监控 + 记忆漂移审计 + 多 marketplace 分发治理 |

a16z 在 Big Ideas 2026 把 "Agent Employee" 作为头号议题,但比这更根本的判断是:**Agent 的差异化生于数据反馈环、活于记忆架构、死于分发渠道。** 这三件事都不是模型团队能替 PM 做的。

---

## 18.10 三条保持冷静的原则

1. **基准饱和不等于能力到位**:MMMU-Pro 三家在 81-83% 不意味着多模态可以无脑上线,OSWorld 78% 也不等于真实 SLA 可达。生产环境的 robustness 仍是 6-12 个月的差距
2. **世界模型不要过度承诺**:AMI Labs / World Labs 的资金量很大,但 Genie 3 的几分钟一致性和 GAIA-3 的仿真验证范围都还有限。**把它列入 2027 路线图、而不是 2026-Q3 必交付**
3. **记忆是双刃剑**:ChatGPT memory dossier 引发的用户反弹、Mnemonic Sovereignty 论文列出的攻击面,都说明"记忆默认开"在 GDPR/CCPA 时代是高风险默认值。**先把"可见、可改、可删"三件套做扎实再谈个性化**

---

## 18.11 PM 应该问自己的三个问题

1. **我的产品产生的偏好数据,6 个月后能否变成至少 +5pp 的 fine-tune 提升?**
2. **我的 agent 在 30% 网络抖动率下,端到端任务完成率能否守住 60%?**
3. **如果今天 OpenAI 把我用的模型/marketplace 抽成结构改 2×,我的 P&L 是否仍成立?**

**这三个问题决定 2027 谁还活着。**

---

## 资料源

### 一手研究 / 厂商
- [OpenAI RFT use cases](https://platform.openai.com/docs/guides/rft-use-cases)
- [OpenAI Cookbook: SFT vs DPO vs RFT](https://cookbook.openai.com/examples/fine_tuning_direct_preference_optimization_guide)
- [Anthropic Model Spec Midtraining (2026)](https://alignment.anthropic.com/2026/msm/)
- [Sierra: Constellation of models](https://sierra.ai/blog/constellation-of-models)
- [Notion 3.3 Custom Agents (2026-02-24)](https://www.notion.com/releases/2026-02-24)
- [OpenAI Memory and new controls](https://openai.com/index/memory-and-new-controls-for-chatgpt/)
- [OpenAI Workspace Agents announcement](https://venturebeat.com/orchestration/openai-unveils-workspace-agents-a-successor-to-custom-gpts-for-enterprises-that-can-plug-directly-into-slack-salesforce-and-more)
- [Salesforce Agentforce in ChatGPT (Dec 2025)](https://salesforcedevops.net/index.php/2025/12/22/salesforce-launches-agentforce-in-chatgpt-to-head-off-homegrown-mcp-servers/)
- [Anthropic Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7)
- [OpenAI Operator launch](https://openai.com/index/introducing-operator/)
- [DeepMind Genie 3 research](https://deepmind.google/blog/genie-3-a-new-frontier-for-world-models/)
- [Gemini 3 Deep Think](https://deepmind.google/blog/accelerating-mathematical-and-scientific-discovery-with-gemini-deep-think/)
- [World Labs (Marble)](https://www.worldlabs.ai/)
- [AMI Labs](https://amilabs.xyz/)
- [Wayve GAIA-3 launch](https://wayve.ai/press/wayve-launches-gaia3/)
- [Sesame Research: Crossing the Uncanny Valley](https://www.sesame.com/research/crossing_the_uncanny_valley_of_voice)

### 学术
- [arXiv 2603.07670 — Memory for Autonomous LLM Agents](https://arxiv.org/abs/2603.07670)
- [arXiv 2604.16548 — Mnemonic Sovereignty](https://arxiv.org/abs/2604.16548)
- [arXiv 2510.03285 — WAREX: Web Agent Reliability Evaluation](https://arxiv.org/html/2510.03285)
- [DPO original paper (arXiv 2305.18290)](https://arxiv.org/pdf/2305.18290)

### VC 与行业
- [a16z Big Ideas 2026 Part 1](https://a16z.com/newsletter/big-ideas-2026-part-1/)
- [No Priors Ep.144: 2026 AI Forecast](https://www.youtube.com/watch?v=TOsNrV3bXtQ)
- [TechCrunch — AMI Labs raises $1.03B](https://techcrunch.com/2026/03/09/yann-lecuns-ami-labs-raises-1-03-billion-to-build-world-models/)
- [MIT Tech Review — World models 10 things](https://www.technologyreview.com/2026/04/21/1135650/world-models-ai-artificial-intelligence/)
- [Mem0: State of AI Agent Memory 2026](https://mem0.ai/blog/state-of-ai-agent-memory-2026)
- [Simon Willison: I really don't like ChatGPT's new memory dossier](https://simonwillison.net/2025/May/21/chatgpt-new-memory/)

### 工程参考
- [Voice Agent Infrastructure Stack 2026](https://www.digitalapplied.com/blog/voice-agent-infrastructure-stack-2026-reference)
- [Multimodal AI Benchmarks 2026](https://www.digitalapplied.com/blog/multimodal-ai-benchmarks-2026-vision-audio-code)
- [AI Agent Marketplaces 2026](https://www.digitalapplied.com/blog/ai-agent-marketplaces-2026-discovery-distribution)
- [Poly.ai: Barge-in handling](https://poly.ai/blog/barge-in-voice-ai-interruption-handling)
