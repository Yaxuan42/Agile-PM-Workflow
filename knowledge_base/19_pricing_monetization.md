# 第十九章 · Pricing & Monetization Playbook

> 第三章给了 Token 经济学的内部成本面。本章是**对外定价决策学** —— 7 大模型 + 单位经济 + B2B 合同条款 + 8 节点决策树。

---

## 19.1 为什么 2026 年定价问题"被重新打开"

2025 是"AI adoption at any cost"的一年 —— 大多数企业愿意为 PoC 早期合同付费。**2026 是第一轮大规模续约年**(renewal cycles),买方第一次有了真实 ROI 数据,开始拷问"我究竟买到了什么"。

Bessemer、Sequoia、a16z、Decagon 在 2026 Q1 几乎同时发表"AI Pricing Playbook"长文,结论一致:**席位制(per-seat)正在死亡,但纯结果制(pure outcome)的边界条件非常苛刻;混合模型(平台费 + 可变 + 结果分成)将成为 2026-2027 主流。**

**Salesforce 在 14 个月内换了三次 Agentforce 定价**(per-conversation → per-seat 加价 → Flex Credits);**Devin 从 $500/月 重定价到 $20/月起步**;**Klarna 公开承认 AI 客服降本失败并重新招人** —— 这些都是 PM 应当背诵的"反面教材"。

---

## 19.2 七大主流定价模型对比

| 模型 | 计费单位 | 真实案例 | 典型价格点 | 卖方毛利 | 主要"坑" |
|------|---------|---------|-----------|---------|---------|
| **席位制 Per-seat** | 每用户/月 | Microsoft Copilot $30、Glean $45-50、Harvey $1,200-2,000 | $20-$2,000/seat/月 | 60-80% | 与 AI 工作量解耦;**Copilot 激活率仅 35.8%**;ROI 难证明 |
| **用量制 Per-token / per-credit** | API token、credit | Anthropic $3/$15(Sonnet 4.6)、Cursor $20 credit 池、Lovable 100 credits/$25 | 千 token 级 | 50-65% | 账单不可预测;用户产生"使用焦虑" |
| **任务制 Per-task / per-deliverable** | 每完成任务 | EvenUp(per-case)、Devin $2.25/ACU、Cognition | $0.10-$2,000/task | 55-75% | "任务"定义争议;部分完成怎么算 |
| **结果制 Per-resolution / outcome** | 每成功结果 | Intercom Fin $0.99、Sierra ~$1.50、Zendesk $1.00-1.50 | $0.99-$2/resolution | 60-80% | "解决"的定义是最大争议;**renewal 时定义会被收紧** |
| **Agent 制 Per-agent** | 每数字员工 | ServiceNow AI Agent($10K/年起)、Salesforce Agentforce 原 $2/conversation | $24K-$120K/agent/年 | 65-80% | "代理"边界模糊;买方比较"agent vs 人力" |
| **结果分成 Outcome-share** | % of value | 部分 Sierra 大客户合同、Paid(Manny Medina 新公司) | 5-30% of saved cost | 看场景 | 价值归因复杂;合同周期长 |
| **Freemium + Premium** | 免费 + 订阅 | ChatGPT Plus $20、Perplexity Pro $20、Cursor $20 | $17-$25/月 | 30-60%(消费端) | 转化率 2-3% |

### Bessemer 的"定价成熟度曲线"

四阶段演进:**活动制(counting tokens) → 工作流制(charging for processes) → 结果制(paid for results) → Agent 制(replacing human equivalents)**。

2026 大多数 B2B 玩家在阶段二到三之间,少数(Sierra、Intercom Fin)跳到阶段三。

---

## 19.3 结果制定价的深度解剖

### "结果"如何定义(Resolution Definition Matrix)

| 厂商 | 结果定义 | 谁判定 | 部分解决怎么算 | 客户不满意是否退费 |
|------|---------|-------|--------------|----------------|
| **Intercom Fin** | 客户在 AI 回复后 **24 小时内不重新打开工单** | 系统自动 | 不计 resolution | 默认不退;高级合同可加 SLA |
| **Sierra** | "成功 resolved support / saved cancellation / upsell" | 合同条款定义,可加 CSAT 阈值 | 半费或不计 | 月度对账 |
| **Decagon** | per-conversation 或 per-resolution(二选一);resolution = AI 完整处理无人工介入 | 系统 + 客户抽样审计 | conversation 计费;resolution 不计 | 抽样不满意可申诉 |
| **Zendesk AI** | "Automated Resolution" = 任何被 AI 关闭且未升级的工单 | 系统判定 | 升级到人工不计 | 不退 |
| **EvenUp** | per-case(2025-05 改);之前 per-demand letter(含 unlimited revisions) | 客户验收 | 重写不加价 | 律师拒收可重做 |

### 关键洞察

> **"Resolution definitions are renegotiated at renewal, and if the agent has been performing well, the vendor has data showing how to tighten the definition in their favor"**
> —— OpenNash 评估 Sierra
>
> **续约时定义会被供应商收紧,第二年的真实单价基本不会等于第一年。**

**PM 必须在合同里写死定义并锁定多年价。**

### 争议处理机制

1. **月度对账(Monthly true-up)**:客户在收到账单后 N 天(15-30 天)内对单一 resolution 提出申诉;供应商需举证
2. **抽样审计(Statistical sampling)**:双方 NDA 下的第三方审计权,对 1-2% resolution 抽样复核
3. **CSAT-Gating**:Resolution 只在 CSAT ≥ 4/5(或 NPS 阈值)时才计费 —— Sierra 高端合同里出现
4. **Escalation Refund**:被 AI 处理后又升级到人工的工单 100% 退费 —— 已成 Zendesk 默认条款
5. **Cap & Floor**:年度最低承诺 + 最高消费上限,保护双方

### 典型合同结构

| 条款 | 范围 | 解释 |
|------|------|------|
| **Platform Fee** | $50K-$200K/年(Decagon 起步 $50K) | 不可退费的"门票费" |
| **Minimum Commit** | 月度最少 N resolutions 或 $X | 防止客户用极少量"白嫖";典型 5,000-50,000/月 |
| **Volume Discount Tiers** | Zendesk:1-100 $1.50、101-1000 $1.30、1001-5000 $1.10、5001+ $1.00 | 阶梯下降 |
| **Burst Cap / Annual Cap** | $200K-$1M | 客户最高年度支出 |
| **Renewal Cap** | 续约时单价涨幅 ≤ 10-15% | 防供应商利用定义优势涨价 |
| **Termination on Eval Breach** | 准确率 < 85% 连续 30 天可终止 | **2026 年新出现的"质量门"** |

### 真实合同金额区间

- **Decagon**:合同 $100K-$580K/年;Sacra 估算 $35M ARR(2025-11),同比 +283%
- **Sierra**:~$1.50/resolution,Sacra 估算 $100M ARR(2025-10),同比 +400%
- **Intercom Fin**:8 位数 ARR,季度环比 +393%
- **Glean**:mid-market $100K+/年,企业级 $2-5M+/年;$200M ARR

---

## 19.4 不同行业 Agent 的单位经济

| 行业 | 主流定价 | 典型单价 | 典型合同规模 | 厂商毛利 | PM 给 CFO 的辩护点 |
|------|---------|---------|-------------|---------|------------------|
| **客户服务** | per-resolution | $0.99-$2.00 | $50K-$500K/年 | 70-80% | 1 名客服年成本 $40K,AI 单 resolution $1,replace ratio 30-50% |
| **编码** | per-seat 或 credit | $20-$200/月 | $20-$10K/月/团队 | 50-65% | 开发者节省 20-30% 时间,按 $150K 年薪算每月省 $2,500 |
| **法律** | per-deliverable 或 seat | $300-2000/案件,$1,200-2,000/律师/月 | $288K-$2M/年(Harvey 20-seat 起) | 60-70% | 1 份 demand letter 律师手写 8h × $400/h = $3,200;AI 交付 $300 |
| **金融研究** | per-seat 或 per-research | $1,000-3,000/分析师/月 | $100K-$2M/年 | 60-75% | 分析师节省 15h/周;按 $300K 年薪算每月省 $9K |
| **医疗** | per-encounter 或 seat | $1-$5/encounter;$200-500/医生/月 | $50K-$5M/年 | 50-65% | 医生 1h 临床笔记 = $300 时间成本;AI $3/笔记 |
| **API 平台** | per-token | $0.10-$25/M token | 看体量 | 50-70% | 不需要辩护——按用量付费 |
| **Vertical Agent**(HR、Finance) | per-agent + outcome | $24K-$120K/agent/年 | $100K-$1M | 60-75% | 1 个数字员工 vs 1 个全职员工成本($80K+benefits) |

### PM 给 CFO 的三段式辩护框架

1. **Replace cost**:列出被替代的人力/外包成本(年化 $X)
2. **Avoid cost**:列出避免的工具采购、错误成本、合规罚款(年化 $Y)
3. **Upside revenue**:列出 AI 带来的新增收入(upsell、conversion、retention,年化 $Z)

**合同价格上限 = (X + Y + Z) × 25-40%**(买方愿付的 ROI 倍数:典型 **2.5-4x ROI 是企业 AI 采购的 hurdle rate**)。

### 毛利现实检查

> "AI 应用毛利从 SaaS 的 80-90% 跌到 50-60%,因每次推理产生真实变动成本。"
> —— Bain Capital Ventures

SaaStr 数据:**84% 公司报告 6%+ 毛利侵蚀**。Microsoft Azure 毛利因 AI 工作负载压到 69%。

**PM 应对**:
- 把 prompt caching(Anthropic 90% off)、batch API(50% off)做到 infra 层,组合可省 95% token 成本
- 把高频任务路由到便宜模型(Haiku $1/$5 vs Opus $5/$25)
- **在结果制下,"resolution 定义"本质上是毛利杠杆 —— 定义越严,毛利越高**

---

## 19.5 定价发现过程

| 公司 | 发现方法 | 经验教训 |
|------|---------|---------|
| **Decagon** | 客户访谈定锚 → per-conversation 起步 → 大客户要 per-resolution → 双轨制 | 一次给客户"二选一"反而帮客户做了决策 |
| **Sierra** | Bret Taylor 公开承诺"outcome-based pricing"作为差异化 | 把"outcome"作为品牌话术,但内部其实是按合同一单一议 |
| **Cursor** | Free → $20 Pro 无限 → 2025-06 改 credit pool | $20 unlimited 烧钱,必须改额度制 |
| **Lovable** | 免费 5 credits/day → $25 Pro 100 credits → $50 Business | "giving away product for free has become our most powerful growth strategy" —— Elena Verna |
| **Manus** | 邀请码二级市场倒卖(¥50K-100K)→ $39 Starter / $199 Pro | 用稀缺性做营销,再用阶梯订阅收割 |
| **Devin** | $500/月 团队版 → 失败 → 2025-04 改 Devin 2.0 起步 $20 | "起步价 $500"被市场拒绝,必须把入口压到 $20 |
| **Salesforce Agentforce** | $2/conv → 抱怨 → Per-seat 60% 加价 → 2025-05 Flex Credits $0.10/action | 单一定价模型在多客群下行不通;hybrid 必须 |

### 定价发现 5 步法

1. **MVP 价格锚定**:用最贵合理价上线,便于后续打折(Devin 反例:$500 太高踩穿了"合理"边界)
2. **三套并行 PoC**:同时给同一客户三种报价(per-seat / per-task / per-outcome),观察客户选哪个
3. **WTP 调查 + 反向工程**:Madhavan Ramanujam 框架 —— 直接问 "在什么价格下你会犹豫但仍买,什么价格下你会觉得贵但能接受,什么价格下你会拒绝"
4. **续约信号**:第一批客户续约时若 zero churn 且无议价 → 价格偏低
5. **价格 / 用量解耦实验**:把订阅费和用量费拆开,观察哪一项弹性更高

---

## 19.6 B2B Agent 合同条款(与传统 SaaS 的差异)

| 条款类别 | 传统 SaaS | Agent 产品 2026 |
|---------|----------|----------------|
| SLA Uptime | 99.9% | 99.9% + **accuracy SLA**(连续 30 天准确率 ≥ 85% 否则可终止) |
| 数据使用 | "Vendor may use aggregated metadata" | **明确禁止用客户数据训练 base model**;可选 opt-in 训练专属模型 |
| 模型替换 | 不存在 | **Buyer Notification 条款**:供应商更换底层模型须提前 30-60 天通知,允许客户在新模型 eval 阶段免费试用;若准确率下降 > X% 可解约 |
| Indemnification | 知识产权 + 数据泄露 | + **AI 输出错误造成的第三方损失**(AI 误诊、错误法律建议) |
| Output Liability | 一般免责 | **Bonterms AI Standard Clauses v1.0**:vendor "does not guarantee outputs are accurate";customer 须"independently review" |
| Audit Rights | 财务/安全 | + **Model documentation、training data provenance、incident logs** 审计权 |
| Termination Trigger | 重大违约 | + **Eval threshold breach** |
| Data Exit | 客户数据返还 | + **Agent state、custom prompts、fine-tune artifacts 返还** |

### 五个真实命名条款(必背)

1. **"HITL Carve-out"**(Human-in-the-Loop 免责):供应商不对客户员工显式批准的 agent 行为负责
2. **"Delegation of Authority Breach"**(授权越界):Agent 超出客户设定的 policy guardrails 行动,供应商承担更广 indemnification
3. **"Flex Credit Reallocation"**(Salesforce 模型):客户可在 agent action、座席、用量之间自由分配 credits,无需重新议合同
4. **"Renewal Definition Lock"**(PM 必争):Resolution / Task 的定义在初始合同期内不得变更
5. **"Eval-Triggered Termination"**:客户提供 golden eval set,连续 N 天准确率低于阈值可零违约金解约

### Mayer Brown 趋势判断

2026-02 报告:**Agentic AI 合同正在从 SaaS 模板转向 BPO(业务流程外包)模板**,新增 "service definitions, warranties, outcome-based SLAs, broader indemnification, governance and audit rights, data ownership"。**这是 PM 与法务在 2026 年要联手适应的最大变化。**

---

## 19.7 消费级 Agent 商业化

### ARPU / 转化 / 流失

| 产品 | 价格 | 付费用户量 | ARR | 转化率 | 备注 |
|------|------|----------|-----|-------|------|
| ChatGPT Plus | $20/月 | 15-17M(2026 Q1) | $3.6B-$4B | 2-3% | OpenAI 预测 Plus 从 44M(2025)跌至 9M(2026),靠 ChatGPT Go(广告型)补 112M |
| Claude Pro | $20/月 | 未披露 | Anthropic 总 ARR ~$4-7B | - | - |
| Perplexity Pro | $20/月($40/seat 企业) | - | $500M annualized,同比 +335% | - | 流失率 ChatGPT ~20%;Perplexity 因周用 retention 更强 |
| Cursor | $20 Pro / $200 Ultra | - | $500M+ ARR(估) | - | $20 unlimited 时代亏损;改 credit pool 后改善 |
| Lovable | $25/月 Pro | - | - | 免费层是核心增长 | - |
| v0(Vercel) | $20 Premium | - | - | $5 free credits 用极快 | - |
| Replit | $17 Core | - | - | 2026-02 改 4 套 tier | - |
| Manus | $39 Starter / $199 Pro | - | - | 邀请码二级市场曾达 $7K-13K | - |

### 三大流失驱动

1. **价值同质化**:开源模型/竞品达到 parity
2. **用量焦虑**:credit pool 用完后的负面体验
3. **底层模型涨价的隐形传导**:Anthropic Opus 4.7 新 tokenizer 让相同文本多 35% token,**等价于 27% 涨价(不改 sticker price)**

### 消费端定价的"$20 锚"

ChatGPT Plus 从 2023 上线至今未涨过价。**$20 已成消费 AI 订阅的"心理锚定价"** —— Claude Pro、Perplexity Pro、Cursor、v0、Lovable、Replit 全部围绕 $17-$25。**任何高于 $30 的消费产品必须重新构造价值故事**(Manus $39/$199 是用稀缺性破锚的少数案例)。

---

## 19.8 API / Platform 定价策略

### 主要 API 价格(2026-04)

| 模型 | Input ($/M) | Output ($/M) | Cache | Batch |
|------|-----------|-------------|-------|-------|
| Claude Opus 4.6 | $5.00 | $25.00 | 90% off | 50% |
| Claude Sonnet 4.6 | $3.00 | $15.00 | 90% off | 50% |
| Claude Haiku 4.5 | $1.00 | $5.00 | 90% off | 50% |
| GPT-5.4 | $2.50 | $15.00 | 类似 | 50% |
| GPT Nano | $0.10-$0.20 | - | - | - |
| Gemini | $1.25-$2.50 | $5-$10 | implicit cache | 50% |

**组合后**:cache hit + batch = **95% 节省**(Anthropic 官方)。

### API 供给端 PM 策略

1. **价格阶梯化**:旗舰(Opus)+ 中端(Sonnet)+ 廉价(Haiku/Nano)
2. **Cache 作为留存武器**:Cache 越久价值越大,构建"沉没成本"
3. **Batch 收割可异步工作负载**:50% off 但 24h SLA
4. **Tokenizer 暗涨价**:Opus 4.7 新 tokenizer 多 35% token,sticker 不变但客户账单涨 27%

### API 需求端 PM 策略

1. **多供应商路由**:任务 80% 走廉价,关键任务走旗舰
2. **Prompt 共用前缀 → Cache 命中**:系统提示长到 5K+ token,cache 一次受益千次
3. **Batch 处理非实时任务**:摘要、报告、归档 → 全走 batch
4. **合同里加 "Model Swap" 保护**

---

## 19.9 7 个定价失败案例

| 公司 | 失败原因 | 教训 |
|------|---------|------|
| **Klarna AI 客服** | 2022-2024 用 OpenAI 替代 700 客服 → 2025-05 公开承认 AI "lower quality"、重新招人 | "全面替代"叙事失败;AI 客服需要 graceful escalation |
| **Devin $500** | 起步价过高,无 prosumer 入口 → 2025-04 改 Devin 2.0 $20 起 | 高价产品不能没有低门槛入口;prosumer 是企业销售种子 |
| **Salesforce Agentforce $2/conv** | 中小客户拒绝,企业抱怨"unpredictable" → 数月内三次改价 | 单一定价模型在多客群下行不通;必须 hybrid |
| **Cursor $20 Unlimited** | 早期 unlimited 烧钱 → 2025-06 改 credit pool;用户怒了一波 | "Unlimited" 在 AI 时代是有毒承诺 |
| **Microsoft Copilot $30** | 仅 3.3%(450M M365 用户)付费;**激活率 35.8%,64% 席位休眠** | 默认捆绑式 add-on 转化率低 |
| **EvenUp per-demand $300-$800** | 价格区间太宽且 "token math + extras" 被吐槽 → 2025-05 改 per-case | 透明可预测 > 精准计费 |
| **HubSpot AI Add-on** | 类似 Copilot 的 add-on,Tropic 数据显示采购率 < 20% | Add-on 模型在 2026 让位于 Native AI tier |

### 共同结论

1. **第一版定价大概率是错的** —— Salesforce、Cursor、Devin、EvenUp 全部 12 个月内改过价;PM 应在合同里给自己"重定价权"
2. **极端模型(pure unlimited、pure $500、pure per-conv)都失败** —— 市场要 hybrid
3. **客户对账单不可预测的容忍度极低** —— cap 与 floor 是必须,不是 nice-to-have

---

## 19.10 PM 定价决策树

```
START: 我要给 Agent 产品定价
│
├─ Q1: 我的产品是 API/Platform,还是 App/Agent?
│   ├─ API/Platform → §19.8 用量制 + cache + batch;价格围绕竞品 token 价 ±20%
│   └─ App/Agent → 进入 Q2
│
├─ Q2: 我的买方是企业 (B2B) 还是个人 (B2C)?
│   ├─ B2C → 走 $17-$25 锚定订阅;Free + Pro + Premium 三层;ARPU 目标 $20-$30
│   └─ B2B → 进入 Q3
│
├─ Q3: 我的产品输出是"离散可观测结果"吗?
│   ├─ 是 → Q4(候选:结果制 / 任务制)
│   └─ 否 → Q5(候选:席位制 / 用量制)
│
├─ Q4: 结果是双方都可独立验证的吗?
│   ├─ 是 → 结果制(per-resolution $0.99-$2 + platform fee $50K-$200K + 月度 cap)
│   │     必加条款:resolution 定义锁定、续约涨幅 cap ≤ 15%、HITL carve-out
│   └─ 否 → 任务/Deliverable 制(per-case,含 unlimited revisions 是关键卖点)
│
├─ Q5: 目标客户是否已习惯按"数字员工/Agent"思考?
│   ├─ 是 → Per-agent $24K-$120K/agent/年
│   └─ 否 → Q6
│
├─ Q6: AI 使用量是否高度集中在少数 power users?
│   ├─ 是 → 用量/Credit 制
│   └─ 否(全员覆盖型,类 Copilot)→ Per-seat($20-$50 中端;$200-$2,000 高专业度)
│
├─ Q7: 我能否 commit accuracy / outcome SLA?
│   ├─ 能(>85% accuracy 可保证)→ outcome guarantee 写进合同,溢价 20-30%
│   └─ 不能 → 必须留 HITL carve-out 与 customer verification disclaimer
│
└─ Q8: 第一年合同期满后我能否扛得住 zero-churn 续约?
    ├─ 能 → 当前价偏低,下一轮可涨 10-15%
    └─ 不能(churn > 15%)→ 价格偏高或 ROI 故事弱

END: 上线 + 监控 4 个指标
  - 报价拒绝率(>30% = 定价过高)
  - PoC 转付费率(<20% = ROI 故事弱)
  - 月度账单波动率(>40% = 加 cap)
  - 续约时单价变化(< +10% = 议价权弱)
```

### 7 个常见盲点

1. **Q3 误判**:以为输出离散,客户认为"半解决"也算 → 选了结果制反被申诉
2. **Q4 忽视审计权**:结果定义由供应商单方面判定 → 客户不信任 → 销售周期长
3. **Q5 错把 Copilot 当 Agent**:Microsoft 把 Copilot 卖成 per-seat 但激活率仅 35.8%
4. **Q6 忽视长尾用户**:全员席位但只有 10% 在用 → CFO 续约砍 60%
5. **Q7 没法律审核**:accuracy guarantee 写得过死,触发巨额 indemnification
6. **没有 Renewal Definition Lock**:客户第一年付得开心,第二年被收紧定义
7. **没设 Cap**:客户某月业务暴涨,账单飞天

---

## 19.11 六条 PM 硬记忆

1. **2026 是 renewal 年** —— 第一批结果制合同要重新议价
2. **Hybrid 是终态** —— Platform Fee + 可变(per-task / per-resolution)+ 上下 Cap 的三段式结构
3. **定义即毛利** —— "什么算 resolution" 在合同里的字句价值是百万级别的
4. **消费端锚在 $20** —— 除非有强稀缺性故事(Manus),别越过 $30
5. **API 端的真实战场是 cache + batch,不是 sticker price** —— 95% 成本节省藏在组合优化里
6. **第一版定价大概率是错的** —— 给自己留 12 个月的重定价权

---

## 资料源

- [Decagon — Pricing the AI Agent Economy](https://decagon.ai/resources/pricing-ai-agents)
- [Decagon revenue & funding | Sacra](https://sacra.com/c/decagon/)
- [Sierra — Outcome-based pricing for AI Agents](https://sierra.ai/blog/outcome-based-pricing-for-ai-agents)
- [OpenNash — Sierra AI Pricing](https://opennash.com/blog/sierra-ai-pricing-what-outcome-based-really-costs-and-when/)
- [Bret Taylor of Sierra | Cheeky Pint](https://cheekypint.substack.com/p/bret-taylor-of-sierra-on-ai-agents)
- [Fin AI Agent Pricing | Intercom](https://fin.ai/pricing)
- [Salesforce Agentforce Flexible Pricing](https://www.salesforce.com/news/press-releases/2025/05/15/agentforce-flexible-pricing-news/)
- [SaaStr — Salesforce Now Has 3+ Pricing Models for Agentforce](https://www.saastr.com/salesforce-now-has-3-pricing-models-for-agentforce-and-maybe-right-now-thats-the-way-to-do-it/)
- [Monetizely — The Doomed Evolution of Salesforce's Agentforce Pricing](https://www.getmonetizely.com/blogs/the-doomed-evolution-of-salesforces-agentforce-pricing)
- [EvenUp launches AI Drafts Suite per-case pricing](https://legaltechnology.com/2025/05/15/evenup-launches-ai-drafts-suite-smart-workflow-and-per-case-pricing-model/)
- [Glean Pricing 2026 | Workativ](https://workativ.com/ai-agent/blog/glean-pricing)
- [Harvey AI Pricing 2026](https://www.aivortex.io/legal/ai-tools/harvey-ai-pricing-2026/)
- [Devin 2.0 price slash | VentureBeat](https://venturebeat.com/programming-development/devin-2-0-is-here-cognition-slashes-price-of-ai-software-engineer-to-20-per-month-from-500)
- [Cursor — Clarifying our pricing](https://cursor.com/blog/june-2025-pricing)
- [Lovable Pricing](https://lovable.dev/pricing)
- [Lenny — Elena Verna AI growth playbook 2026](https://www.lennysnewsletter.com/p/the-new-ai-growth-playbook-for-2026-elena-verna)
- [Lenny — Madhavan Ramanujam pricing AI](https://www.lennysnewsletter.com/p/pricing-and-scaling-your-ai-product-madhavan-ramanujam)
- [Bessemer — AI pricing and monetization playbook](https://www.bvp.com/atlas/the-ai-pricing-and-monetization-playbook)
- [Sequoia — Pricing in the AI Era / Manny Medina](https://sequoiacap.com/podcast/pricing-in-the-ai-era-from-inputs-to-outcomes-with-paid-ceo-manny-medina/)
- [Mayer Brown — Contracting for Agentic AI Solutions](https://www.mayerbrown.com/en/insights/publications/2026/02/contracting-for-agentic-ai-solutions-shifting-the-model-from-saas-to-services)
- [Bonterms AI Standard Clauses](https://bonterms.com/forms/ai-standard-clauses-version-1-0)
- [Klarna plans to hire humans again | Fortune](https://fortune.com/2025/05/09/klarna-ai-humans-return-on-investment/)
- [Tropic — SaaS and AI Buying Trends 2025/2026](https://www.tropicapp.io/reports/software-spending-trends-2025)
- [Microsoft Copilot Adoption Statistics 2026](https://www.stackmatix.com/blog/copilot-market-adoption-trends)
- [Bain Capital Ventures — Gross Margin BS Metric](https://baincapitalventures.com/insight/gross-margin-is-a-bs-metric/)
