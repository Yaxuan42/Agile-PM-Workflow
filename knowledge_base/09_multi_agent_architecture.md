# 第九章 · 多 Agent 生产架构

> 第二章给出了"双循环飞轮 + 框架对照表"的概念骨架。本章是工程级深潜:**LangGraph / Claude Subagents / OpenAI Agents SDK 三大栈生产代码 + State 持久化 + Failure Recovery + Memory 架构 + OTel GenAI 可观测性 + A2A 协议**。

---

## 9.1 LangGraph 在生产环境

### 真实部署规模

| 公司 | 规模 | 关键架构 |
|------|------|---------|
| **Klarna** | 8500 万活跃用户、250 万对话、相当于 700 名全职员工 | LangGraph + LangSmith,refund 节点前 `interrupt()` 给人工 |
| **Replit** | 数百万开发者 | manager / editor / verifier 三角色,verifier 频繁回到用户 |
| **Uber Genie** | 154 Slack 频道、7 万+ 问题、节省 1.3 万工程小时 | EAg-RAG + LangGraph 顺序流,48.9% helpfulness |

### Postgres Checkpointer 生产配置(关键坑)

```python
from langgraph.checkpoint.postgres.aio import AsyncPostgresSaver
from psycopg_pool import AsyncConnectionPool

# prepare_threshold=0 兼容 PgBouncer transaction 模式
# autocommit=True 避免 LLM 调用期间持有行锁
pool = AsyncConnectionPool(
    conninfo="postgresql://user:pwd@host/langgraph",
    kwargs={"autocommit": True, "prepare_threshold": 0},
    max_size=20,
)

async with AsyncPostgresSaver(pool) as checkpointer:
    await checkpointer.setup()
    graph = builder.compile(checkpointer=checkpointer)
```

**生产坑点**:20 节点 graph × 10,000 次/天 = 每天 20 万行 checkpoint。**几周后是几千万行。** 必须按 `thread_id + created_at` 建分区表 + nightly cleanup job。

### Human-in-the-Loop + Time-Travel(Klarna 退款核心)

```python
from langgraph.types import Command, interrupt

def review_refund(state) -> Command:
    # 节点会重启! 不要在 interrupt 之前做副作用
    decision = interrupt({
        "question": "Approve refund?",
        "amount": state["refund_amount"],
    })
    return Command(goto="execute" if decision else "cancel")

# 第一次:走到 interrupt 暂停
config = {"configurable": {"thread_id": "klarna-tx-9f2c"}}
graph.invoke({"refund_amount": 250.0}, config=config)

# 人工审核后 resume
graph.invoke(Command(resume=True), config=config)

# Time-travel:回到任意 checkpoint
history = list(graph.get_state_history(config))
before_review = next(s for s in history if s.next == ("review_refund",))
graph.invoke(new_input, config=before_review.config)
```

`get_state_history` 是 LangSmith Studio 的底层 —— 让"为什么这次失败"变成可索引的因果链。

---

## 9.2 Claude Code Subagents

### SKILL.md / Subagent 标准格式

```markdown
---
name: security-auditor
description: Use proactively for vulnerability detection on PRs, OWASP/CWE mapping. Triggered on diff containing auth/crypto/sql.
tools: Read, Grep, Glob          # 只读,禁止破坏性
model: opus                       # 高复杂度推理
---

# Role
你是资深安全审计员,输出按 CWE 编号分类...
```

**2026 关键 frontmatter**:`name`、`description`、`tools`、`model`、`argument-hint`、`disable-model-invocation`、`user-invocable`、`allowed-tools`、`context: fork`、`agent`、`hooks`。

`context: fork` 让 subagent 跑在 isolated context(独立 token 预算、独立 tool log),避免污染主线对话。

### 五个高质量 Subagent 解剖

| Subagent | 工具策略 | 模型 | 设计要点 |
|----------|---------|------|---------|
| **code-reviewer** | `Read, Grep, Glob` | sonnet | 严格只读,markdown checklist |
| **security-auditor** | `Read, Grep, Glob` | opus | 慢思考、CWE 编号化 |
| **backend-developer** | `Read, Write, Edit, Bash, Glob, Grep` | sonnet | 全写权限 + hook 强制 lint |
| **documentation-engineer** | `Read, Write, Edit, Glob, Grep, WebFetch, WebSearch` | haiku | 廉价 + 联网,写大量字数 |
| **devops-engineer** | `Read, Write, Edit, Bash, Glob, Grep` | sonnet | Bash 受限于 settings.json allowlist |

### Fork-Join 组合模式(Anthropic 推荐)

```
main agent (Sonnet)
   ├─► /research-planner    (context:fork, opus)  → plan.md
   ├─► /web-researcher × N  (context:fork, haiku, 并行)
   │     ↓ 各自写到 /tmp/research-{i}.md
   ├─► /critic              (context:fork, opus)  → 找漏洞
   └─► /writer              (context:main)        → 整合输出
```

每个 fork 独享 context,主线只看到 final markdown,避免 "context rot"。这正是 Claude 4.7 1M context 仍倾向 fork 的原因 —— 成本/信噪比远优于把所有 raw 内容塞一个会话。

---

## 9.3 OpenAI Agents SDK 生产模式

### Handoffs 配 input_filter(关键)

```python
from agents import Agent, handoff
from agents.extensions import handoff_filters

triage_agent = Agent(
    name="Triage",
    handoffs=[
        billing_agent,
        handoff(
            agent=refund_agent,
            on_handoff=on_escalate,
            input_type=EscalationData,
            input_filter=handoff_filters.remove_all_tools,  # 切换 agent 前清空工具历史
            tool_name_override="escalate_to_refund",
        ),
    ],
)
```

`input_filter` 在生产里非常关键:默认整个 message history 都会传给被切换的 agent,但例如把 PII 工具调用结果带过去会泄漏,**必须用 filter 裁剪**。

### Guardrails 并行执行

```python
@input_guardrail
async def jailbreak_guardrail(ctx, agent, input):
    res = await Runner.run(jailbreak_agent, input, context=ctx.context)
    return GuardrailFunctionOutput(
        output_info=res.final_output,
        tripwire_triggered=res.final_output.is_jailbreak,
    )

main_agent = Agent(
    name="Support",
    input_guardrails=[jailbreak_guardrail],   # 默认与主 agent 并行
)
```

**并行模式**(`run_in_parallel=True`,默认):guardrail 与主 agent 同时启动以降低延迟。
**串行模式**(`run_in_parallel=False`):用于强校验场景(KYC、医疗),保证未通过时 0 token 损耗。

---

## 9.4 状态机 vs 对话式 vs 角色式

```
            Q: 下一步是否需要确定性 routing?
        YES (合规/审批) │ NO (创意/研究)
                        │
          ┌─────────────┴─────────────┐
          ▼                            ▼
   State Machine                 Q: 任务可静态分解为角色?
   (LangGraph)                YES │           │ NO
   - 节点显式 / 边显式           ▼           ▼
   - 可审计                   Role-Based   Conversational
   - 适合: 退款、KYC、ETL      (CrewAI)     (AutoGen GroupChat)
                              静态层级       动态 selector
                              报告流水线     debate / 自洽编程
```

**真实迁移案例**:
- Klarna 早期 AutoGen → LangGraph,因 refund 必须 deterministic interrupt
- 上市分析公司 CrewAI → LangGraph,因需要 cyclical self-correction
- R&D 团队反向:LangGraph → AutoGen,因研究讨论的 routing 用代码写不出来

---

## 9.5 失败恢复:Durable Execution

LLM agent 失败模式与传统服务不同:网络抖动、模型 schema drift、tool API 限流、用户中断都可能在多步骤工作流中间发生。**2026 共识**:把 agent 包到 durable execution engine,把 LLM/tool 调用作为 idempotent activities。

### Temporal + Pydantic AI 模式

```python
from pydantic_ai import Agent
from pydantic_ai.durable_exec.temporal import TemporalAgent
from temporalio import workflow

dispatch_agent  = Agent(model="anthropic:claude-sonnet-4-7", system_prompt="...")
research_agent  = Agent(model="anthropic:claude-opus-4-7",   system_prompt="...")

# TemporalAgent:把所有 LLM/tool 调用变成 Activity (单次执行 + 重试)
temporal_dispatcher = TemporalAgent(dispatch_agent)
temporal_researcher = TemporalAgent(research_agent)

@workflow.defn
class DinnerBotWorkflow:
    @workflow.run
    async def run(self, user_message: str):
        dispatch = await temporal_dispatcher.run(user_message)
        if isinstance(dispatch.output, NoResponse):
            return None
        research = await temporal_researcher.run(dispatch.output)
        return research.output.recommendations
```

Crash 后 worker 重启时,Temporal 从 Event History **确定性重放**到上次成功 activity 之后。**LLM 输出已持久化,绝不重复调用 OpenAI/Anthropic API**(节省 token,避免双花)。

### Inngest TypeScript 版本

```typescript
async function research({ event, step }) {
  const plan = await step.run("plan", () =>
    llm.complete({ prompt: `Plan research: ${event.data.q}` }));

  const hits = await step.run("search", () =>
    searchApi.query(plan.terms),
    { retries: 5, backoff: { type: "exponential", initial: "1s", max: "5m" }});

  // 7 天审批超时 (durable wait)
  const approval = await step.waitForEvent("approval", {
    event: "content/approved", match: "data.draftId", timeout: "7d",
  });

  return await step.run("publish", () => cms.publish(plan, hits, approval));
}
```

**关键**:`step.run("plan", ...)` 的命名是 idempotency key —— 重启后同名 step 直接返回 cached result,不重跑 LLM。

---

## 9.6 长寿命 Agent 的内存架构

### 内存类型 × 厂商定位

| 类型 | 内容 | 推荐方案 |
|------|------|---------|
| **Working** | 当前对话、scratchpad | LangGraph state / Letta core |
| **Procedural** | system prompt / SOP | Skill files / 提示词模板 |
| **Semantic** | 用户偏好、事实 | Mem0 (vector+graph+kv 三层) |
| **Episodic** | 过往交互流水 | Zep temporal KG (时间戳关系) |
| **Document** | 多文档结构化知识 | Cognee GraphRAG |

**LongMemEval 基准 (2026)**:**Zep 63.8%** > Mem0 49.0%(GPT-4o)。

### Letta(前 MemGPT)的核心抽象

LLM context = "RAM",外部 store = "disk",agent 自己用 tool 把数据 page 进 page 出:

```python
from letta_client import Letta

client = Letta(base_url="http://localhost:8283")
agent = client.agents.create(
    memory_blocks=[
        {"label": "human",   "value": "User is a senior PM in Shanghai."},
        {"label": "persona", "value": "You are a polite project planner."},
    ],
    model="anthropic/claude-sonnet-4-7",
)

# 模型可调用 core_memory_append / archival_memory_search 等内置工具
client.agents.messages.create(
    agent_id=agent.id,
    messages=[{"role": "user", "content": "记住我团队有 8 个工程师"}],
)
```

**Letta 把 working memory 控制器化**,agent 自己感知 "context 快满了" 并主动 evict —— 这是 stateful agent 的核心抽象。

---

## 9.7 多 Agent 可观测性:OpenTelemetry GenAI

### 关键属性 (gen_ai.*)

```text
gen_ai.operation.name        ∈ {create_agent, invoke_agent, invoke_workflow, execute_tool}
gen_ai.agent.id / name / version / description
gen_ai.workflow.name         (multi-agent 工作流名)
gen_ai.conversation.id       (跨 agent / 跨 trace 串联 ← 核心 join key)
gen_ai.provider.name         ∈ {openai, anthropic, aws.bedrock, ...}
gen_ai.request.model
gen_ai.input.messages / output.messages   (敏感: 默认脱敏)
gen_ai.usage.input_tokens / output_tokens
gen_ai.usage.cache_read.input_tokens     (prompt cache 命中)
gen_ai.tool.definitions / tool.call.id
gen_ai.response.finish_reasons
```

### Multi-agent Span 结构示例

```text
Trace: conversation.id = conv-9f2c
└── invoke_workflow "klarna_refund_flow"          (INTERNAL)
    ├── invoke_agent "Triage"                     (INTERNAL)
    │   └── chat anthropic/claude-sonnet-4-7      (CLIENT, 1820 in / 220 out)
    ├── invoke_agent "RefundAnalyzer"             (INTERNAL)
    │   ├── execute_tool "get_transaction"        (INTERNAL)
    │   └── chat anthropic/claude-opus-4-7        (CLIENT, 4100 in / 600 out)
    └── invoke_agent "HumanReview" (interrupted)  (INTERNAL, 12.4h)
```

**`gen_ai.conversation.id` 是关键 join key** —— 把 N 个 agent 调用、跨服务 trace 串成一条用户视角的 thread。

### 自动注入(OpenLLMetry)

```python
from opentelemetry.instrumentation.langchain import LangchainInstrumentor
from opentelemetry.instrumentation.openai import OpenAIInstrumentor
from openinference.instrumentation.crewai import CrewAIInstrumentor

LangchainInstrumentor().instrument()
OpenAIInstrumentor().instrument()
CrewAIInstrumentor().instrument()
```

---

## 9.8 A2A 协议:跨厂商代理协作

**MCP**:agent → tool。
**A2A**:agent → agent。

到 2026-05,A2A 已被 **150+ 组织采用**(Tyson Foods、Gordon Food Service 在供应链场景生产部署),Azure AI Foundry 与 Amazon Bedrock AgentCore 原生支持。

### Agent Card(`/.well-known/agent-card`)

```json
{
  "id": "agent-recruiter-001",
  "name": "Sourcing Agent",
  "serviceEndpoint": "https://recruit.example.com/a2a",
  "capabilities": { "streaming": true, "pushNotifications": true },
  "skills": [
    {
      "id": "find_candidates",
      "name": "Find Candidates",
      "description": "Source candidates by job description",
      "inputSchema": { "type": "object", "properties": { "jd": { "type": "string" } } },
      "outputSchema": { "type": "array" }
    }
  ],
  "securitySchemes": { "bearerAuth": { "type": "http", "scheme": "bearer" } },
  "version": "1.0.0"
}
```

### 三种更新通道

- **Polling**(getTask)
- **Streaming**(SSE / `subscribeToTask`)
- **Push Notifications**(Webhook,客户端可暂时离线)

### 经典使用场景

- **招聘流水线**:Hiring manager Agent → 多个职位平台 Agent → 面试调度 Agent → 背调 Agent,跨厂商串端到端
- **供应链规划**:Tyson Foods 用 A2A 串联仓储/运输/采购的多供应商 Agent
- **CRM × ITSM**:Salesforce Agentforce 与 ServiceNow AI Agent 通过 A2A 互通

---

## 9.9 8 条工程铁律

1. **状态必须可持久化**:LangGraph Postgres checkpointer 或 Temporal Event History,二选一不能没有
2. **Subagent 用 fork context**:避免 token 污染,主线只看 final markdown
3. **Handoff 必须配 input_filter**:默认带全历史会泄漏 PII
4. **范式选择是工程决定**:合规走 state machine,研究走 conversational,流水线走 role-based
5. **Durable execution 不是奢侈品**:所有 LLM/tool 调用包进 idempotent activity
6. **Memory 分层**:working = state graph,semantic = Mem0/Letta,episodic = Zep
7. **OTel GenAI 是统一语言**:用 `gen_ai.conversation.id` join 跨 agent 跨服务
8. **A2A + MCP 是 2026 互操作底座**:内部用 MCP,跨边界用 A2A,避开自定义协议

---

## 资料源

- [LangGraph Persistence Docs](https://docs.langchain.com/oss/python/langgraph/persistence)
- [LangGraph Interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts)
- [Klarna LangGraph Case Study](https://www.langchain.com/blog/customers-klarna)
- [Replit Agent Case Study](https://www.langchain.com/breakoutagents/replit)
- [Uber Genie Enhanced Agentic-RAG](https://www.uber.com/blog/enhanced-agentic-rag/)
- [Awesome Claude Code Subagents (VoltAgent)](https://github.com/VoltAgent/awesome-claude-code-subagents)
- [Awesome Claude Skills (Composio)](https://github.com/ComposioHQ/awesome-claude-skills)
- [Claude Code Sub-agents Docs](https://code.claude.com/docs/en/sub-agents)
- [OpenAI Agents SDK Handoffs](https://openai.github.io/openai-agents-python/handoffs/)
- [OpenAI Agents SDK Guardrails](https://openai.github.io/openai-agents-python/guardrails/)
- [Temporal — Build Durable AI Agents with Pydantic AI](https://temporal.io/blog/build-durable-ai-agents-pydantic-ai-and-temporal)
- [Inngest — Durable Execution for AI Agents](https://www.inngest.com/blog/durable-execution-key-to-harnessing-ai-agents)
- [Letta GitHub](https://github.com/letta-ai/letta)
- [Mem0 — State of AI Agent Memory 2026](https://mem0.ai/blog/state-of-ai-agent-memory-2026)
- [Mem0 vs Letta vs Zep vs Cognee 2026](https://explore.n1n.ai/blog/ai-agent-memory-comparison-2026-mem0-zep-letta-cognee-2026-04-23)
- [OpenTelemetry GenAI Agent Spans](https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-agent-spans/)
- [A2A Protocol Spec](https://a2a-protocol.org/latest/)
- [Google A2A Announcement](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)
- [A2A 1.0 at 150+ Orgs (Linux Foundation)](https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year)
