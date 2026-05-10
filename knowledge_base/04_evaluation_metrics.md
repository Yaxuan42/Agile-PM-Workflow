# 第四章 · 评测体系与核心指标

> **PM 对 "done" 的定义已重写**:不再是 "feature 上线",而是 "在评估集 X 上达到 Y 分,且 p95 延迟 ≤ Z 秒,单任务成本 ≤ $C"。

Anthropic 在《Demystifying Evals for AI Agents》中明确指出:**"使 agent 有用的那些能力——自主性、智能性、灵活性——同样使其更难评估。"**

---

## 4.1 评估成熟度阶梯(Eval Maturity Ladder)

综合 Hamel Husain、Shreya Shankar、Anthropic 的框架,可整理为五级阶梯:

| 级别 | 名称 | 特征 | 典型团队画像 |
|------|------|------|------------|
| **L1** | Vibes-only | 创始人在 Slack 截屏 demo;无任何持久化数据 | Pre-PMF 原型 |
| **L2** | Spreadsheet evals | 几十条 prompt + 期望输出;Excel 人工打分 | 早期产品迭代 |
| **L3** | Unit-test 化 | 代码化断言 + 跑在 CI;按 feature 拆 scenario | Hamel 推崇的 Rechat 模式 |
| **L4** | LLM-as-judge + 校准 | 引入 LLM 打分器,20-30% 人工样本持续校准 judge 与人类对齐 | 大多数 Series B+ 创业公司的甜蜜区 |
| **L5** | Production-grade | Online + offline 双轨;shadow + bandit;trajectory replay;漂移监控;自动化红队 | OpenAI / Anthropic / 头部 enterprise |

> Hamel 强调:**"The important part is that these assertions should run fast and cheaply."** L3 的核心不是花哨工具,而是廉价高频。

---

## 4.2 PM 必须掌握的 18 个核心指标

### A. 任务完成类

**1. Task Success Rate(任务成功率)** = `成功完成的任务数 / 总任务数`
Amazon 称之为 "Goal Success Rate",顶层北极星指标。

**2. Pass@k** = `1 - C(n-c, k)/C(n, k)`,其中 c 为 n 次采样中正确数
SWE-bench 等 benchmark 标配。Pass@1 = 首次正确率;Pass@5 = 5 次内能否解决。

**3. Pass^k**(Anthropic 推广)= 全部 k 次试验都成功的概率
衡量**一致性**而非能力上限。Agent 产品上线必须看 Pass^k——用户没有重试耐心。

### B. LLM-as-Judge 类

**4. Faithfulness(忠实度)**:输出是否被提供的上下文支持
客观任务,Eugene Yan 建议直接打分而非 pairwise。

**5. Helpfulness / Harmlessness / Honesty(HHH 三元组)**:Anthropic Constitutional AI 标准
主观维度建议用 pairwise comparison。

**6. Judge-Human Agreement Rate**:衡量 LLM judge 与人工标注的对齐度
GPT-4 在 MT-Bench 开放问题上达 85% 一致率,但在摘要任务仅 0.3-0.6 相关性,**不可跨域复用**。

### C. 成本与延迟(PM 必须直接对账)

**7. Cost per Successful Task** = `总 token 花费 / 成功任务数`
比 cost-per-token 更诚实——把重试、思考 token、失败回滚都摊进去。

**8. Reasoning Token Tax**:思考型模型的隐藏成本
GPT-5.5 Pro high reasoning 的 P50 TTFT 67 秒;Claude Opus 4.7 extended thinking 28 秒。Reasoning tokens 通常按 output 价格计费但用户看不到。

**9. TTFT (Time-To-First-Token) p95**:流式 UX 体感关键
聊天场景 p95 < 0.5-0.6s 体感优秀;> 1s 用户开始流失。P95/P50 比通常 2.1×,最差 3.2×。

**10. Total Task Latency p95**:Agent 端到端时间
Anthropic 建议把 turn count、token usage 一并记录在 trajectory 中。

### D. 工具使用(Agent 特有)

**11. Tool Selection Accuracy**:选了正确工具的比例
需要 ground-truth 标注。

**12. Tool Parameter Accuracy**:参数填充准确率
Amazon 把 multi-turn function call accuracy 单列出来,因 multi-turn 错误会级联放大。

**13. Tool Call Error Rate**:实际触发 API 异常的比例(不同于"选错")

### E. Agent 轨迹质量

**14. Trajectory Quality Score**:评估完整 trace 而非仅终态
Anthropic 实测:**只看终态会让 agent 多通过 20-40% 的测试用例,但回归隐藏在中间步骤**。

**15. Recovery Rate / Plan-Revision Rate**:遇到 tool error 后能否自我纠错并重新规划
τ²-bench 显示从 single-control 切到 dual-control 时 agent 表现急剧下降,沟通协调成为瓶颈。

### F. 安全类

**16. Refusal Rate / False Refusal Rate**
Meta Muse Spark 报告 chat cyber 11.0%、agentic cyber 4.3% 的 false refusal——**过度拒绝同样有害**。

**17. Jailbreak Success Rate**
单轮防护良好的模型在多轮 jailbreak 下成功率仍 > 70%;2026-03 研究显示自动化 jailbreak agent 成功率达 97.14%。Claude 4 Sonnet harm score 仅 2.86%,DeepSeek-V3 高达 90%。

**18. Leakage Rate**
Browser agent 即使底层 LLM 拒绝,仍能在 GPT-4o agent 上达到 100% Attack Success Rate、o1 上 68%——**安全训练在 agent 化后并不自动迁移**。

### G. 北极星指标的演化(DAU 已死?)

传统 DAU/MAU 在 AI 产品中失真——用户可能日活但每次都被 agent 失败劝退。2026 年新北极星候选:

- **Tasks Successfully Completed per Active User (TSC/U)**
- **Time-to-Successful-Outcome**(用户从输入到拿到正确结果的中位时长)
- **Eval-Shaped Business Metric**:Eric Weber 提出 "数据负责人 2026 年的工作不是给 AI 做 dashboard,而是构建一个 eval-shaped metric 来判断 AI 是否在交付商业价值"

---

## 4.3 Starter Eval Set 模板(PM 可直接用)

Anthropic 官方建议:**20-50 个简单任务,全部从真实失败案例中抽取**。Hamel 进一步建议初期"读 trace 直到不再学到新东西为止"。

### 推荐起步配比(共 50 条)

| 类别 | 数量 | 说明 |
|------|------|------|
| **Golden / Happy Path** | 15 | 最高频、最赚钱场景。任何回归立即阻塞发布 |
| **Edge Cases**(罕见输入) | 10 | 长输入、多语言、空字段、矛盾指令 |
| **Adversarial / Red Team** | 10 | Prompt injection、jailbreak、社工话术、tool misuse |
| **Multi-Turn / State** | 8 | 上下文累积、用户改主意、agent 需复用前序结果 |
| **Tool-Use Trajectory** | 5 | 显式 ground-truth 标注 "应调用工具 A 然后 B" |
| **Drift Sentinels** | 2 | 经过多版本验证的 canonical 用例,专门探测模型/数据漂移 |

每条用例至少包含:
```
id, input, expected_output_or_rubric, category, severity,
source_trace_url, last_human_reviewed_at
```

### 在线 + 离线双轨

| 维度 | Offline | Online |
|------|---------|--------|
| 频率 | 每次 commit | 持续 |
| 流量 | 固定 50-500 条 | 真实生产 |
| 工具 | CI 中跑 grader | Shadow mode、Multi-Armed Bandit |
| 用途 | 防回归 | 发现长尾、A/B 优胜 prompt |

> **Shadow Mode**:把请求同时发给 Control 和 Treatment,只展示 Control 给用户——零 UX 风险但成本翻倍
> **Multi-Armed Bandit**:比固定切流的 A/B 平均快 37% 找到优胜变体

---

## 4.4 PM 必读的基准 Benchmark(2026-05 现状)

| 基准 | 测什么 | 2026 年领先成绩 |
|------|--------|---------------|
| **SWE-bench Verified** | 真实 GitHub 仓库代码修复 | Claude Mythos Preview 93.9%、Claude Opus 4.7 (Adaptive) 87.6%、GPT-5.3 Codex 85% |
| **SWE-bench Pro** | 去污染的更难版本 | 同款 Mythos Preview 仅 45.9%——**说明高分有水分** |
| **GAIA** | 通用助手 466 任务,多步浏览+文件解析 | Claude Sonnet 4.5 在 Princeton HAL 上 74.6% |
| **τ²-bench** | 客服 dual-control 仿真(零售/航空/电信) | 单/双 control 表现差距巨大,暴露协作短板 |
| **AgentBench** | 8 类 agent 环境综合 | THUDM 维护 |
| **Humanity's Last Exam** | 2,500 道学科边界题 | 顶级模型仅 ~35%,人类专家 ~90%,**50+ 分鸿沟** |
| **WebArena** | 真实网页 UI 操作 | 与 GAIA 互补 |

> **PM 重要心法**:这 5-7 个基准测的是不同维度,**绝不可压缩成单一排名**。

---

## 4.5 六大常见陷阱与具体案例

### 陷阱 1 · Goodhart's Law on LLM-as-Judge
*"When a measure becomes a target, it ceases to be a good measure."*

**案例**:LMArena/Chatbot Arena 被曝出 Meta、OpenAI、Google、Amazon 在私下批量测试模型版本,只发布最佳成绩——典型的 cherry-pick Goodhart 操作。
**对策**:所有评估 run(含失败)必须落库可审计。

### 陷阱 2 · LLM Judge 的固有偏差
Eugene Yan 总结的三大偏差:
- **Position bias**:gpt-3.5 50% 倾向第一位
- **Verbosity bias**:>90% 偏好更长答案,即使更短的更好
- **Self-enhancement bias**:Claude-v1 给自己输出多 25% 胜率

**对策**:随机化 pairwise 顺序、限制长度、用不同家族模型做交叉 judge。

### 陷阱 3 · Criteria Drift(评估标准会漂)
Shreya Shankar 在 UIST 2024 论文 *Who Validates the Validators* 中正式命名:**用户需要标准来打分,但打分过程本身在塑造标准**——这是个 catch-22。

> "It is impossible to completely determine evaluation criteria prior to human judging."

**对策**:把 eval 设计成**可迭代**的,配 EvalGen 类工具,接受混乱与迭代。

### 陷阱 4 · Benchmark 污染(Contamination)
SWE-bench Verified 上 93.9% 的模型在去污染的 SWE-bench Pro 上掉到 45.9%——训练数据已经吃过测试集。

**对策**:内部 holdout 集必须从未发布到任何公网;季度轮换 sentinel 用例。

### 陷阱 5 · 只评 Final Output,忽略 Trajectory
Anthropic 实测:**只看终态会让 agent 通过率虚高 20-40%**。

> "Agent regressions appear at the interaction level: a model update that changes behavior at step 3 corrupts reasoning at steps 4-8, invisible to single-turn scoring."

### 陷阱 6 · Safety 训练不传递到 Agent
Browser ART 报告 GPT-4o agent 攻击成功率 100%、o1-based agent 68%——**底层模型拒绝有害请求,但 agent 化后照样越狱**。

**对策**:安全 eval 必须在 agent 完整 loop 中重做。

---

## 4.6 关键人物语录

> **"Evals are the new PRDs."**
> — Hamel Husain & Shreya Shankar, Lenny's Newsletter

> **"Always start with error analysis. Don't jump into writing evals."**
> — Hamel Husain & Shreya Shankar, Maven AI Evals 课程

> **"Don't buy fancy LLM tools. Use what you have first."**
> — Hamel Husain

> **"Each model performed poorly on some datasets, suggesting that they're not reliable enough to systematically replace human judgments."**
> — Eugene Yan, *Evaluating the Effectiveness of LLM-Evaluators*

> **"It is impossible to completely determine evaluation criteria prior to human judging."**
> — Shreya Shankar et al., UIST 2024

> **"The capabilities that make agents useful also make them more difficult to evaluate."**
> — Anthropic, *Demystifying Evals for AI Agents*

> **"20-50 simple tasks drawn from real failures is a great start. Early changes have large effect sizes, so small sample sizes suffice."**
> — Anthropic Engineering Blog

> **"Cost per successful task beats cost per token for executive reporting."**
> — Silicon Data, LLM Cost Per Token Guide 2026

---

## 资料源

- [Hamel Husain — Your AI Product Needs Evals](https://hamel.dev/blog/posts/evals/)
- [Hamel Husain — LLM Evals FAQ](https://hamel.dev/blog/posts/evals-faq/)
- [Anthropic — Demystifying Evals for AI Agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
- [Anthropic Cookbook — building_evals.ipynb](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/building_evals.ipynb)
- [Eugene Yan — Evaluating the Effectiveness of LLM-Evaluators](https://eugeneyan.com/writing/llm-evaluators/)
- [Eugene Yan — An LLM-as-Judge Won't Save the Product](https://eugeneyan.com/writing/eval-process/)
- [Shreya Shankar et al. — Who Validates the Validators (UIST 2024)](https://arxiv.org/abs/2404.12272)
- [Lenny's Newsletter — Why AI Evals Are the Hottest New Skill](https://www.lennysnewsletter.com/p/why-ai-evals-are-the-hottest-new-skill)
- [Maven — AI Evals For Engineers & PMs Course](https://maven.com/parlance-labs/evals)
- [AWS ML Blog — Evaluating AI Agents at Amazon](https://aws.amazon.com/blogs/machine-learning/evaluating-ai-agents-real-world-lessons-from-building-agentic-systems-at-amazon/)
- [SWE-bench Official Leaderboards](https://www.swebench.com/)
- [CodeAnt — SWE-bench Leaderboard 2026 Analysis](https://www.codeant.ai/blogs/swe-bench-scores)
- [Rapid Claw — AI Agent Framework Scorecard 2026](https://rapidclaw.dev/blog/ai-agent-benchmarks-2026)
- [Kili Technology — AI Benchmarks 2026](https://kili-technology.com/blog/ai-benchmarks-guide-the-top-evaluations-in-2026-and-why-theyre-not-enough)
- [BenchLM.ai — LLM Speed & Latency Comparison 2026](https://benchlm.ai/llm-speed)
- [Silicon Data — Understanding LLM Cost Per Token (2026)](https://www.silicondata.com/blog/llm-cost-per-token)
- [DeepEval — Tool Correctness Metric](https://deepeval.com/docs/metrics-tool-correctness)
- [TensorZero — Bandits in Your LLM Gateway](https://www.tensorzero.com/blog/bandits-in-your-llm-gateway/)
- [Scale AI — Refusal-Trained LLMs Are Easily Jailbroken As Browser Agents](https://static.scale.com/uploads/6691558a94899f2f65a87a75/browser_art_draft_preview.pdf)
- [Repello AI — AI Jailbreak Prompts](https://repello.ai/blog/understanding-ai-jailbreaking-techniques-and-safeguards-against-prompt-exploits)
- [Meta AI — Muse Spark Safety & Preparedness Report](https://ai.meta.com/static-resource/muse-spark-safety-and-preparedness-report/)
- [Collinear AI — Goodhart's Law in AI Leaderboard Controversy](https://blog.collinear.ai/p/gaming-the-system-goodharts-law-exemplified-in-ai-leaderboard-controversy)
- [Eric Weber — North Star Metrics for AI Data Products](https://ericdataproduct.substack.com/p/north-star-metrics-for-ai-data-products)
