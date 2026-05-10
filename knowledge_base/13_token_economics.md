# 第十三章 · Token 经济与成本优化

> 第三章给出了模型价格表 + 入门工具包成本。本章是工程级深潜:**真实 ROI 数据 + 缓存 / Batch / Cascade / Loop budget 代码 + 5 类典型 Agent 的成本基准**。

---

## 13.1 Prompt Caching 深度剖析

### 三大厂商定价矩阵(2026 Q2)

| 厂商 | 缓存写入 | 缓存读取 | TTL | 最低 Token 门槛 | 自动/显式 |
|------|---------|---------|-----|---------------|----------|
| Anthropic (5min TTL) | 1.25× 输入价 | 0.10× 输入价 (-90%) | 5 分钟 | 1024 tokens | 显式 (`cache_control`) |
| Anthropic (1h TTL, 2026 新增) | 2.0× 输入价 | 0.10× 输入价 (-90%) | 60 分钟 | 1024 tokens | 显式 |
| OpenAI | 0(免费) | 0.50× 输入价 (-50%) | 5-60 分钟 | 1024 tokens | **自动** |
| Google Gemini 2.5+ 隐式 | 0(免费) | 0.10× 输入价 (-90%) | 不保证 | 1024-2048 tokens | **自动** |
| Google Gemini 显式 | 标准价 + $4.50/MTok·小时(Pro) | 0.10× 输入价 (-90%) | 自定义 | 32,768 tokens | 显式 |

### 真实生产 ROI

- **DEV.to RCA 案例**:Anthropic prompt caching 让 RCA 总成本降 **90%**,月支出从 ~$8,000 → ~$800
- **Cursor Composer 2(2026-03)**:单次请求平均 **390K tokens,88% 命中缓存**,单价从 $0.50/MTok 降到 $0.05/MTok 实际负载;一次请求成本 $0.40 → $0.19(-52%)
- **Helicone 边缘缓存**:10M 请求/月工作负载,纯 L2 节省 39%;L1+L2 叠加节省 54%
- **Claude Code Max 计划**:API 原价 $15,000 的工作量在 Max 套餐下 $100/月(-93%),核心机制就是缓存命中

### 缓存键设计三铁律

```python
import anthropic
client = anthropic.Anthropic()

# 黄金法则:static-first, dynamic-last
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    system=[
        # 块1:静态规则手册(10K tokens,命中 90% 折扣)
        {"type": "text", "text": SYSTEM_PROMPT_30K_TOKENS,
         "cache_control": {"type": "ephemeral", "ttl": "1h"}},
    ],
    tools=TOOLS_DEFINITION,  # 保持顺序稳定
    messages=[
        # 块2:长期对话历史(也可缓存)
        {"role": "user", "content": [
            {"type": "text", "text": HISTORICAL_TURNS,
             "cache_control": {"type": "ephemeral"}},
        ]},
        # 块3:当轮动态输入(不缓存)
        {"role": "user", "content": current_user_query},
    ],
)
print(f"cache_read={response.usage.cache_read_input_tokens}")
print(f"cache_creation={response.usage.cache_creation_input_tokens}")
```

**铁律**:
1. **前缀完全匹配**:单字符差异即 miss。JSON 键序、空格、tool 定义顺序都必须稳定
2. **breakpoint 放在静态尾部**:最多 4 个 `cache_control` 断点
3. **TTL 选择公式**:两次访问间隔 < 5min → 用 5min;间隔 5-60min 且会复用 → 用 1h

---

## 13.2 Batch API 经济学(用 24 小时换 50%)

| 厂商 | 折扣 | 单批上限 | 文件上限 | SLA |
|------|------|---------|---------|-----|
| OpenAI Batch | 50%(输入+输出) | 50,000 请求 | 200 MB | 24h,多数 1-6h 完成 |
| Anthropic Message Batches | 50% | 10,000 请求 | 100 MB | 24h |
| Google Vertex Batch | 50% | 不公开 | — | 24h |

### 折扣可叠加(关键发现)

Anthropic 明确允许 **Batch × Cache 叠加**:
- Sonnet 4.6 标准输入:$3.00/MTok
- + Batch (-50%):$1.50/MTok
- + Cache hit (-90%):$0.05/MTok → **整体 95% 折扣**

### 何时启用 Batch(决策表)

| 工作负载 | 适合 Batch? | 月节省 |
|---------|-----------|-------|
| 实时客服对话 | 否 | — |
| 夜间 eval pipeline(10K 测试用例) | 是 | -50% |
| 每周内容审核(百万条 UGC) | 是 | -50% |
| 训练数据生成(合成 prompt-completion 对) | 是 | -50% |
| 月度报表 / 日志分类 | 是 | -50% |
| 法律文件批量摘要(1000 合同/月) | 是 | -50% |

**经验数据**(TokenMix):当 ≥50% 工作负载可走 Batch,**月 API 账单降低 35-48%**。

---

## 13.3 上下文工程(算法削 Token)

| 技术 | Token 削减率 | 质量损失 | 实现复杂度 | 适用场景 |
|------|-----------|---------|-----------|---------|
| 滑动窗口(固定 N 轮) | 40-70% | 高(丢失早期事实) | 低 | 闲聊机器人 |
| 递归摘要(running summary) | 60-85% | 中(关键事实可保留) | 中 | 长对话客服 |
| LLMLingua token 级压缩 | 20× 压缩 | 低(perplexity-guided) | 中 | RAG 前置 |
| StreamingLLM(attention sinks) | 50-80% | 低 | 高 | 流式生成 |
| 分层递归摘要(chunk→段→章) | 90%+ | 中 | 高 | 10M+ token 文档 |
| Datalake 交互式 schema 抓取 | 87% | 0%(accuracy ↑) | 高 | text-to-SQL |

### 实测数字(production)

- **Eugene Yan 模式**:Anthropic 内部"对话历史保短" + 显式 caching = 客服 bot 单会话成本下降 5×
- **Cursor 代币浪费分析**:42 次 agent run **70% token 是浪费** —— 读多余文件、失败重试、冗长 tool 输出;**87% token 用于"找代码"而非"写代码"**
- **text-to-SQL 列描述权衡**:3,000 tokens prompt → 50% accuracy;7,000 tokens(加列描述)→ 65%;6,500 tokens(加示例值)→ 持平。**每多投 1000 token 约 +2.1pp 准确率**,到 5000 tokens 边际收益接近零

---

## 13.4 模型路由 / Cascade(30-85% 节省的核心引擎)

### 主流路由器横评

| 路由器 | 类型 | 节省 vs 全 GPT-4 | 性能保留 | 备注 |
|-------|------|----------------|---------|------|
| RouteLLM(LMSYS 开源) | 学习型偏好路由 | **-85%** | 95% MT-Bench | 比商用便宜 40%+ |
| Martian | 商用 | -50%(50% 调用 GPT-4) | ~95% | 闭源 |
| NotDiamond | 商用 | -25-40% | 高 | 倾向选大模型,RouterArena 第 12 |
| Unify AI | 商用 | -55%(45.6% 调用 GPT-4) | ~95% | RouteLLM 更高效 |
| Portkey Gateway | 路由+缓存+observability | 30-60% | 工作流可配 | 企业首选 |

### Cascade 代码模式(生产可用)

```python
async def cascading_inference(query: str) -> str:
    # 第 1 档:DeepSeek V4 Flash ($0.14/$0.28 per MTok)
    cheap_resp = await call_model("deepseek-v4-flash", query, log_probs=True)
    confidence = score_confidence(cheap_resp)
    if confidence > 0.85:
        return cheap_resp.text  # 70% 流量在此返回

    # 第 2 档:Claude Sonnet 4.6 ($3/$15)
    mid_resp = await call_model("claude-sonnet-4-6", query)
    if validate_with_tests(mid_resp):
        return mid_resp.text  # 25% 流量在此

    # 第 3 档:Claude Opus 4.7 / GPT-5.4(顶级模型)
    return await call_model("claude-opus-4-7", query).text  # 5% 流量
```

WizardCoder cascade 实测:MBPP 问题先送 7B(3 solutions × 6 test pairs = 18 对),pass 率 < 阈值 0.5 时升 13B,再升 34B。**节约 45-85% inference cost,保留 95% 质量。**

---

## 13.5 推理 Token 管理(可能差 10× 成本)

### Anthropic adaptive thinking(Opus 4.6+ / Sonnet 4.6+)

- Thinking token 按 **output 价计费**(Sonnet 4.6: $15/MTok)
- 4K thinking + 500 output ≈ **9× 普通 answer 的成本**
- 单次 thinking call 可耗 3-10× 普通 completion 的 token
- Effort levels:`low / medium / high / max`,`adaptive` 让模型自决策

**何时该用 extended thinking?**
- ✅ 数学、多步逻辑、代码设计/debug → 显著提升
- ❌ 事实查询、格式化、分类 → **零增益,纯增本**

### OpenAI o3/o4-mini effort levels(ARC-AGI 实测)

| 模型 | effort | ARC-AGI-1 分数 | 成本相对 |
|------|--------|--------------|---------|
| o3 | low | 41% | 1× |
| o3 | medium | 53% | ~3× |
| o3 | high | (未公开) | ~10× |
| o4-mini | low | 21% | 0.1× |
| o4-mini | medium | 41% | ~0.3× |

**o3 定价**: $10/$40/MTok;**o4-mini**: $1.10/$4.40 —— **近 10× 差价**。

### 边际质量陷阱案例

某客户在 RAG QA 任务上做 A/B:
- Sonnet 4.6 default: $0.012/query, accuracy 87%
- Sonnet 4.6 thinking budget=4096: $0.108/query (**9×**), accuracy 89% (+2pp)
- **结论**:**ROI 严重不划算**,仅在 <2% 复杂查询需升级 thinking

---

## 13.6 Agent Loop 预算工程

### 三层 hard guardrails

```python
result = await agent.run(
    prompt=user_query,
    max_turns=20,                # ① 迭代上限
    max_total_cost_usd=2.50,    # ② 财务断路器
    max_tokens_total=500_000,   # ③ token 硬顶
    timeout_s=180,
)
if result.subtype == "error_max_budget_usd":
    log_alert(result.session_id)
```

Claude Code SDK 返回 `error_max_turns` 或 `error_max_budget_usd` 错误子类型,便于熔断。

### 渐进式压力提示(Hermes Agent 模式)

- 默认 `maxIterations=20`
- 剩余 10 轮:注入 CAUTION —— "Start wrapping up"
- 剩余 3 轮:注入 WARNING —— "You MUST respond with your final answer NOW. Do not call any more tools."

### 重试策略矩阵

| HTTP 状态 | 动作 | 退避 |
|---------|------|------|
| 429, 500, 502, 503, 504 | 重试 | 指数退避+jitter |
| 401, 403, 422 | **立即停止** | — |
| 408 (timeout) | 重试 1 次 | 固定 |

### SLO 最佳实践

- Booking workflow:`max_tool_calls=15` 是合理上限。>15 几乎都是 agent confused
- Claude Code Max plan:5h 滚动窗口 + 7 天周 ceiling 双层节流
- 严肃 agent 工作,预算 $60-200/月/用户

---

## 13.7 五类典型 Agent 真实成本基准

> 假设:Claude Sonnet 4.6 ($3/$15) + 80% cache hit。

| Agent 类型 | Input/Output tokens/任务 | 工具调用 | 单任务成本 | 10K 用户/月 | 100K 用户/月 | 1M 用户/月 |
|----------|------------------------|---------|-----------|-----------|------------|----------|
| **客服(Sierra/Decagon style)** | 3,150 in / 400 out (3,550 总) | 2-5 | $0.012 | $1,200 | $12,000 | $120,000 |
| **Code agent(Cursor/CC)** | 390K (88% cached) / 5K out | 10-30 | $0.19 | $19,000 | $190,000 | $1.9M |
| **研究(Perplexity Deep)** | 5K in / 2K out + 30 searches | 30-50 | $0.41 | $41,000 | $410,000 | $4.1M |
| **Text-to-SQL** | 6,500 in / 200 out | 1-3 | $0.039 | $3,900 | $39,000 | $390,000 |
| **Document processing(10K 合同)** | 10K in / 500 out per doc | 0 | $0.038 | $380 | $3,800 | $38,000 |

> **商业基准对照**:**Decagon 年合同 $95K-$590K**;**Sierra 年起 $150K** —— 客服厂商定价毛利空间巨大,**纯 token 成本仅占售价的 1-5%**。

### 复杂 agent 成本爆炸警告

- 简单 tool-calling agent: 5K-15K tokens/任务
- 复杂多 agent 系统: **200K-1M+ tokens/任务**(5-30×)
- Reflexion 循环 10 轮 = **50× 单次 linear cost**
- 软件工程修复 issue 任务 unconstrained agent: **$5-8/任务**

---

## 13.8 价格战轨迹与 Q3/Q4 2026 预测

### 季度价格历史(顶配模型 $/MTok 输出)

| 季度 | OpenAI 旗舰 | Anthropic 旗舰 | Google 旗舰 |
|------|----------|--------------|----------|
| 2024 Q1 | GPT-4 $60 | Claude 3 Opus $75 | Gemini 1.0 Ultra $21 |
| 2024 Q2 | GPT-4o $15 | Claude 3.5 Sonnet $15 | Gemini 1.5 Pro $10.5 |
| 2024 Q4 | GPT-4o $10 | Claude 3.5 Sonnet $15 | Gemini 1.5 Pro $5 |
| 2025 Q2 | GPT-4.1 $8 | Claude Sonnet 4 $15 | Gemini 2.5 Pro $10 |
| 2025 Q4 | GPT-5 $10 | Claude Opus 4.5 $75 | Gemini 3 Pro $12 |
| 2026 Q2 | GPT-5.2/5.4 $14 | Claude Sonnet 4.6 $15 / Opus 4.7 $75 | Gemini 3.1 Pro $12 |

### 关键轨迹观察(Epoch AI)

- 2024.1 之后年降速从 50× 加速到 **200× 中位**,最快 benchmark 达 **900×/年**
- GPT-4 级别能力的获取价:3 年内下降 90%+,目前同质能力 < $5/MTok
- 客户级 DeepSeek V4 Flash:$0.14/$0.28 —— **比 2024 GPT-4 便宜 200×**

### Q3-Q4 2026 预测

1. **入门级模型 < $0.05/MTok 输入**:Q4 GPT-5 mini-equivalent 将打破此阈值
2. **顶级模型价格僵持**:旗舰输出仍在 $10-$75 区间 —— **前沿能力的算力稀缺仍存**
3. **Caching 与 Batch 折扣会扩大**:预计 Anthropic Q3 推出 "L2 持久缓存",TTL > 24 小时(写入费 ~3×,读取 -95%)
4. **路由层成为标准**:到 2026 Q4 预计 < 30% 生产请求直接调用单一供应商,主流走 gateway(Portkey/Helicone/LiteLLM)

### 单位经济学含义

- **客服 SaaS**:单会话成本 3 年内 $0.50 → $0.012 (-97%),**毛利率从 30% 升到 75%**
- **Code agent**:仍是高成本陷阱。即便 Composer 2 大幅降本,单付费用户月成本仍 $50-150。**$20/月"无限套餐"几乎 100% 亏本**
- **研究 agent**:Perplexity Deep Research $0.41-$1.32/查询 —— 若用户每天 >5 次,**$20/月套餐结构性亏损**,必须按查询计费或限频

---

## 13.9 技术选型决策树

```
单任务成本 > $0.10?
├─ 是 → 启用 cascade 路由 (-50-85%)
│        + 强制 prompt caching (-90% on 静态前缀)
│        + 评估能否走 Batch (-50%)
├─ 否 → 是否实时?
        ├─ 否 → Batch API 直接拿 50%
        └─ 是 → caching + 上下文剪枝即可

延迟敏感 (P99 < 2s)?
├─ 是 → 关闭 extended thinking, effort=low
└─ 否 → 复杂任务上 adaptive thinking

每日 > 10M tokens?
├─ 是 → 必须加 observability (Helicone/Langfuse)
│        必须设 agent loop budget 硬顶
│        必须 cascade routing
└─ 否 → 单层 caching + 滑动窗口足够
```

---

## 资料源

- [Anthropic Prompt Caching Docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [Anthropic Pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- [Anthropic Batch Processing](https://platform.claude.com/docs/en/build-with-claude/batch-processing)
- [Anthropic Adaptive Thinking](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking)
- [Anthropic API Pricing 2026 Guide — Finout](https://www.finout.io/blog/anthropic-api-pricing)
- [Save 90% on Claude API Costs — DEV.to](https://dev.to/stklen/how-to-save-90-on-claude-api-costs-3-official-techniques-3d4n)
- [OpenAI Prompt Caching](https://openai.com/index/api-prompt-caching/)
- [OpenAI Batch API](https://developers.openai.com/api/docs/guides/batch)
- [Gemini Context Caching Docs](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/context-cache/context-cache-overview)
- [Eugene Yan — Patterns for LLM-based Systems](https://eugeneyan.com/writing/llm-patterns/)
- [Silicon Data — LLM Cost Per Token 2026](https://www.silicondata.com/blog/llm-cost-per-token)
- [Helicone — Effective LLM Caching](https://www.helicone.ai/blog/effective-llm-caching)
- [Epoch AI — LLM Inference Price Trends](https://epoch.ai/data-insights/llm-inference-price-trends)
- [Cursor Composer 2 Cache Economy — DEV.to](https://dev.to/toyama0919/cursor-composer-2-the-cache-economy-behind-a-10x-cheaper-coding-agent-15cj)
- [Cursor Composer 2 Cost Analysis — Vantage](https://www.vantage.sh/blog/cursor-composer-2)
- [7 Practical Ways to Reduce Claude Code Token Usage — KDnuggets](https://www.kdnuggets.com/7-practical-ways-to-reduce-claude-code-token-usage)
- [Claude Code Agent Loop Docs](https://code.claude.com/docs/en/agent-sdk/agent-loop)
- [RouteLLM — LMSYS Blog](https://www.lmsys.org/blog/2024-07-01-routellm/)
- [Speculative Cascades — Google Research](https://research.google/blog/speculative-cascades-a-hybrid-approach-for-smarter-faster-llm-inference/)
- [DeepSeek V4 Pricing 2026 — DeepInfra](https://deepinfra.com/blog/deepseek-v4-pro-pricing-guide-2026-providers-cost-analysis)
- [Gartner — LLM Inference 90% cheaper by 2030](https://www.gartner.com/en/newsroom/press-releases/2026-03-25-gartner-predicts-that-by-2030-performing-inference-on-an-llm-with-1-trillion-parameters-will-cost-genai-providers-over-90-percent-less-than-in-2025)
- [Decagon Pricing the AI Agent Economy](https://decagon.ai/resources/pricing-ai-agents)
- [Sierra AI Pricing 2026 — Quiq](https://quiq.com/blog/sierra-ai-pricing/)
