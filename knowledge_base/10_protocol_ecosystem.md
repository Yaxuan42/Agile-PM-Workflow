# 第十章 · 协议生态(MCP + Skills + AGENTS.md + A2A)

> 2026 年的 Agent 协议层已统一在 Linux Foundation 旗下 **Agentic AI Foundation (AAIF)** 治理。Anthropic、OpenAI、Google 罕见同台。本章给出**完整规格摘录 + 代码 + 选型决策矩阵**。

---

## 10.1 MCP 完整规格(2025-11-25 版)

### 历史与定位

Model Context Protocol(MCP)由 Anthropic 2024-11 开源,**2025-12-09 随 Linux Foundation AAIF 成立被捐赠托管**。截至 2026-05:**累计安装 9700 万次,生态 18,000+ 服务器**。最新规格版本 **2025-11-25**(社区称"周年版本"),GitHub 仓库以 TypeScript 为单一真相源,自动生成 JSON Schema。

### 三角色架构

- **Host**:用户直接交互的 LLM 应用(Claude Desktop、Cursor、VS Code)
- **Client**:Host 内为每个 Server 创建的隔离会话连接
- **Server**:暴露能力的进程,通过 stdio 或 HTTP 与 Client 通信

设计灵感来自 **Language Server Protocol (LSP)** —— 把"AI 工具集成"做成像"编辑器加语言"那样的可插拔标准。

### JSON-RPC 2.0 消息层

```json
// Request — 必有 id,id MUST NOT 为 null
{ "jsonrpc": "2.0", "id": 1, "method": "tools/call",
  "params": { "name": "search_issues", "arguments": { "query": "P0" } } }

// Result Response
{ "jsonrpc": "2.0", "id": 1, "result": { "content": [...] } }

// Error Response
{ "jsonrpc": "2.0", "id": 1, "error": { "code": -32602, "message": "Invalid params" } }

// Notification — 没有 id,接收方 MUST NOT 回复
{ "jsonrpc": "2.0", "method": "notifications/initialized" }
```

握手:Client 发 `initialize` → Server 返回 capabilities → Client 发 `notifications/initialized` → 才能调用 `tools/list`、`tools/call` 等业务方法。

### 三大原语

| 原语 | 作用 | 触发方式 |
|------|------|---------|
| **Resources** | 模型可读的数据源(文件、DB 行、URL) | 由用户/Host 选择附加到上下文 |
| **Tools** | 模型可调用的函数 | 由模型自主决策调用 |
| **Prompts** | 预定义的工作流模板 | 由用户从菜单触发 |

反向(Client 提供给 Server):**Sampling**(Server 让 Client 的 LLM 帮忙推理)、**Roots**(Server 询问可操作的文件根目录)、**Elicitation**(Server 反向请求用户补充信息)。

### 传输层:Streamable HTTP 取代 SSE

2025-03-26 规格已废弃纯 SSE,改用 **Streamable HTTP**:单个 `/mcp` HTTP 端点,POST 写入 + GET 升级为 SSE 流。2025-11-25 进一步要求支持**无状态会话**,使 MCP 服务器能水平扩展。

### 2025-11-25 周年版本五大新增

1. **Async Tasks**:tools/call 可返回 task id,客户端轮询 / 订阅完成
2. **Enhanced Sampling with tool calls**:Server 让 Client 的 LLM 在 sampling 中再次调用 tools,实现 Server 端代理循环
3. **Elicitation**:正式纳入规格,Server 可向用户索要参数
4. **Client ID Metadata Documents (CIMD)**:取代 Dynamic Client Registration。Client 把自身身份发布为 URL+JSON 文档,Authorization Server 按需 fetch
5. **Extensions System**:正式承认协议会有非核心扩展,enterprise 相关能力大多以 extension 落地

### 主流 MCP 服务器(2026-05)

| 类别 | 代表服务器 |
|------|----------|
| 项目管理 | Linear、Jira、Atlassian、Asana |
| 知识库 | Notion、Confluence、Obsidian |
| 通讯 | Slack、Discord、Microsoft Teams |
| 代码 | GitHub、GitLab、BitBucket |
| 数据 | Postgres、Supabase、Snowflake、BigQuery |
| 可观测 | Sentry、Datadog、Grafana |
| 基础设施 | Cloudflare、AWS、Docker Hub |
| 销售 | Salesforce、HubSpot |

42/50 主要被工程类 Agent 调用;企业类(HubSpot、Salesforce)增长最快。

---

## 10.2 构建 MCP Server:生产级模板

### 工具 Schema 设计原则

- 用 **Zod / Pydantic** 而不是手写 JSON Schema
- `description` 字段写**何时使用**,不是"做什么"——LLM 调度依赖触发语境
- 必填参数尽量少,可空字段用 `default` 注入

```typescript
import { z } from "zod";

export const getMemorySchema = z.object({
  workspace_id: z.string().describe("Target workspace identifier"),
  memory_id:    z.string().describe("Memory document ID"),
});
```

### OAuth 2.1 + Resource Indicator

MCP 规格把 MCP Server 定位为**纯 Resource Server**:不签发 token,只验证。生产模式:OAuth 2.1(强制 PKCE、移除 implicit flow)+ **RFC 8707 Resource Indicator**(把 token 绑定到具体 Server URL,防止跨服务器重放)。

```typescript
const tokenMiddleware = requireBearerAuth({
  requiredScopes: ["default"],
  resourceMetadataUrl: new URL(OAUTH_ISSUER_URL).toString(),
  verifier: {
    verifyAccessToken: async (token) => {
      const claims = await introspect(token);
      return {
        token,
        clientId: claims.client_id,
        scopes:   claims.scope,
        extra:    { userId: claims.user_id },
      };
    },
  },
});
app.use("/mcp", tokenMiddleware, mcpRoutes);
```

### Python FastMCP 对照

```python
from fastmcp import FastMCP
from pydantic import BaseModel

mcp = FastMCP("memory-server")

class GetMemory(BaseModel):
    workspace_id: str
    memory_id: str

@mcp.tool()
async def get_memory(args: GetMemory, ctx) -> dict:
    user = ctx.auth.user_id
    if not await has_access(user, args.workspace_id):
        raise PermissionError("Workspace access denied")
    doc = await db.get(args.workspace_id, args.memory_id)
    return doc.to_dict()

if __name__ == "__main__":
    mcp.run(transport="streamable-http", port=8080)
```

### 五个值得拆解的生产 MCP 服务器

| 服务器 | 看点 |
|--------|------|
| `github/github-mcp-server` | 官方 Go 实现,OAuth + GitHub Apps 双模式,工具命名规范 |
| `cloudflare/mcp-server-cloudflare` | Workers + Durable Object,边缘 + 远程优先 |
| `modelcontextprotocol/servers`(filesystem/postgres/memory) | 三大入门 reference,权限边界与 root 协商 |
| `supabase/mcp-server-supabase` | Edge Function + Postgres,RLS 与 MCP scopes 对齐 |
| `linear/mcp-server` | GraphQL + cursor 分页 |

---

## 10.3 Anthropic Agent Skills(SKILL.md)

### 三层渐进披露

| Level | 何时加载 | Token 成本 | 内容 |
|-------|---------|-----------|------|
| **1 元数据** | 启动时常驻 | ~100 tokens / Skill | YAML frontmatter `name`+`description` |
| **2 主体说明** | 触发时由 bash 读入 | <5k tokens | SKILL.md 正文 |
| **3 资源 / 脚本** | 按需读取或执行 | 几乎不计 | scripts/、references/、assets/ |

**关键**:Level 3 的 Python 脚本由 Claude 通过 bash 运行,**脚本源码不进入上下文**,只输出结果消耗 token。这是 Skills 与 prompt 工程的本质差别。

### 完整 Frontmatter 规格

```yaml
---
name: pdf-processing            # 必填; ≤64 字符; 仅小写字母、数字、连字符
                                #       禁止 "anthropic" / "claude" 与 XML 标签
description: |                   # 必填; ≤1024 字符; 必须同时含"做什么" + "何时使用"
  Extract text and tables from PDF files, fill forms, merge documents.
  Use when working with PDF files or when the user mentions PDFs, forms,
  or document extraction.

# 实验字段
allowed-tools: Read Grep Bash(git:*) Bash(jq:*)
model: sonnet                    # opus | sonnet | haiku
license: Apache-2.0
compatibility: [anthropic-claude-code>=2.0]
---
```

`allowed-tools` 是当前最有价值的实验字段:把"工具白名单"显式化,可使用 `Bash(git:*)` 这样的 glob。**一个声称只处理 Word 文档的 Skill 若申请 Bash 全权,就是明显的红旗。**

### Skills 与 Subagents 组合

- **Subagent**(`.claude/agents/*.md`):**独立上下文窗口**,适合长流程隔离
- **Skill**(`.claude/skills/*/SKILL.md`):**主对话上下文里加载**,适合点对点能力

模型路由原则:`opus`=安全审计/架构;`sonnet`=日常开发;`haiku`=docs/搜索。

### 三个表面分发

| 表面 | 自定义 Skill 共享范围 | 网络访问 |
|------|---------------------|---------|
| Claude.ai | 个人;无组织级管理 | 取决于设置 |
| Claude API | Workspace 全员 | 默认无网络 |
| Claude Code | `~/.claude/skills/` 或 `.claude/skills/`,可经 Plugin 分发 | 全网络 |

**跨表面不会自动同步** —— PM 部署 Skill 时最容易踩的坑。

---

## 10.4 AGENTS.md 标准:2,500 仓库实证

GitHub Blog 分析 2500+ 仓库,结论:**导致 Agent 失败的不是技术,而是含糊**。

### 6 大核心区(实证归纳)

| # | 段落 | 反模式 | 正模式 |
|---|------|-------|-------|
| 1 | **Commands** | 只列工具名(`pytest`) | 给可执行命令含 flag(`pytest -v --maxfail=1`) |
| 2 | **Project Knowledge / Stack** | "a React project" | "React 18 + TypeScript + Vite + Tailwind" 含版本 |
| 3 | **Code Style with Examples** | 三段散文规则 | 1 段真实代码示例 |
| 4 | **Testing Standards** | 无验证机制 | 提交前必须 `npm test` 通过 |
| 5 | **Boundaries(三档)** | 只说"小心" | ✅ Always / ⚠️ Ask First / 🚫 Never |
| 6 | **Persona / Role** | "helpful coding assistant" | "You are an expert technical writer for this project" |

> **原文核心**:"One real code snippet showing your style beats three paragraphs describing it."

### 工具支持矩阵(60,000+ 项目)

OpenAI Codex、Google Jules、Google Gemini CLI、Cursor、VS Code、Zed、Warp、Aider、goose、Factory、Devin、Junie、Semgrep、GitHub Copilot、UiPath、RooCode、Windsurf、Amp、Ona、Augment Code、Kilo Code、Phoenix、opencode。

**Monorepo 规则**:"最近的那一个 AGENTS.md 优先"。

### 实证:AGENTS.md 在 eval 中击败 Skills

Vercel 2026-04 评测在 Next.js 16 全新 API(`connection()`、`cacheLife()`、`forbidden()`)上对照:

| 配置 | 通过率 |
|------|-------|
| Baseline(无文档) | 53% |
| Skill(默认描述) | 53%(模型 56% 不触发) |
| Skill(显式提示) | 79% |
| **AGENTS.md 文档索引** | **100%** |

**胜出原因**:常驻系统提示、不需要触发决策、不会出现"该不该现在调用 Skill"的次序问题。

**Vercel 实践指引**:**通用框架知识用 AGENTS.md;垂直、动作型工作流(版本迁移、表单填写)用 Skill。**

---

## 10.5 Agentic AI Foundation(AAIF)治理

**2025-12-09 Linux Foundation 宣布 AAIF 成立**,三个奠基项目:**MCP(Anthropic) + goose(Block) + AGENTS.md(OpenAI)**。

### 会员构成(2026-05)

| 级别 | 数量 | 代表 |
|------|------|------|
| Platinum | 8 | AWS、Anthropic、Block、Bloomberg、Cloudflare、Google、Microsoft、OpenAI |
| Gold | 18 | Cisco、Datadog、Docker、IBM、JetBrains、Okta、Oracle、Salesforce、SAP、Snowflake、Shopify、Twilio、Temporal |
| Silver | 22 | Hugging Face、Pydantic、Zapier、WorkOS、Solo.io、Uber |

到 2026-04 新增 97 个会员,**总数突破 146**。

### MCP 2026 路线图(David Soria Parra)

1. **Transport Evolution & Scalability**:Streamable HTTP 无状态化、`.well-known/mcp/server-card.json` 标准化
2. **Agent Communication**:基于实验性 Tasks 原语,补齐重试语义、过期策略
3. **Governance Maturation**:建立 contributor ladder,SEP 评审权下放给 Working Groups
4. **Enterprise Readiness**:审计追溯、SSO 鉴权、Gateway 行为、配置可移植性 —— **主要以 extensions 落地**而非核心规格

---

## 10.6 A2A 协议(完整内容见第九章 §9.8)

**MCP**:agent ↔ tool, opaque, 同进程/本地。
**A2A**:agent ↔ agent, 任务委托, 跨厂商。

A2A 强调 "opaque" —— Agent B 不暴露内部实现,只暴露 **Agent Card + JSON-RPC 方法集**,相当于 agent 间的 OpenAPI。

### JSON-RPC 方法(v1.0)

```text
SendMessage              -- 同步发任务
SendStreamingMessage     -- SSE 流式
GetTask / ListTasks      -- 任务查询
CancelTask
SubscribeToTask          -- 订阅任务事件 (push/SSE)
TaskPushNotificationConfig.{Set,Get,List,Delete}
GetExtendedAgentCard     -- 鉴权后的扩展能力
```

### 真协同案例

Klarna 把退款 agent 暴露为 A2A endpoint,让合作银行的 agent 直接对接,**而不是写定制 webhook**。

---

## 10.7 选型决策:MCP vs Tool-Calling vs A2A vs 自研 RPC

```
┌────────────────────────────────────────────────────────┐
│                       LLM 应用                          │
│  ┌──────────────────────────────────────────────────┐  │
│  │ 上下文 / 文件级:AGENTS.md(常驻)、Skill(按需) │  │
│  └──────────────────────────────────────────────────┘  │
└────────────┬──────────────────────────────┬────────────┘
             │  MCP(agent → tool)         │  A2A(agent → agent)
             ▼                              ▼
   ┌──────────────────┐           ┌──────────────────┐
   │  GitHub / Slack  │           │  Sales Agent /   │
   │  Postgres / ...  │           │  Recruiter Agent │
   └──────────────────┘           └──────────────────┘
```

### PM 决策矩阵

| 问题 | 推荐方案 |
|------|---------|
| 单一 Agent 调用 ≤5 个内部 API,且只在自有产品里 | **原生 tool-calling** |
| Agent 需要被多种 IDE / 客户端复用 | **MCP Server** |
| 接入 Linear、Jira、GitHub | **MCP Server**(用现成 9000+ 之一) |
| 多组织/厂商 Agent 互相委托任务 | **A2A** |
| 跨长时间(几小时-几天)、需 webhook 回调 | **A2A**(Tasks + Push Notifications) |
| 高吞吐内部微服务调用,不需要 LLM 协商 | **gRPC / 自研 RPC** |
| 给 Agent 提供"项目知识"/"团队规范" | **AGENTS.md**(常驻)+ Skills(动作型) |
| 给 Agent 提供"领域专长包"(Excel、PDF、品牌设计) | **Skills** |

### Vercel 实测最佳栈(2026-04)

```
仓库根目录
├── AGENTS.md           ← 框架知识、命令、style、boundaries
├── .claude/skills/
│   └── nextjs-16-migration/SKILL.md   ← 动作型工作流
├── .mcp.json           ← Linear / GitHub / Postgres MCP servers
└── .agents/agents.json ← 内部 Agent 互通(若启用 A2A)
```

### 风险清单

| 风险 | 缓解 |
|------|------|
| Skill 触发率低(评测显示 56% 漏触发) | 改用 AGENTS.md 或写死前置 prompt |
| MCP Server 跨租户授权疏漏 | Resource Indicator 锁定 token + workspace HOC |
| Agent Card 泄漏内部能力 | extendedAgentCard 仅在认证后返回 |
| 协议版本碎片化 | A2A 用 `A2A-Version` header;MCP 用 `initialize` 协商 |
| Skill 来自不可信源 | 审 SKILL.md + scripts;只用 anthropics/skills 与官方厂商提供的 |

---

## 资料源

- [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
- [MCP GitHub Repo](https://github.com/modelcontextprotocol/modelcontextprotocol)
- [MCP Authorization Tutorial](https://modelcontextprotocol.io/docs/tutorials/security/authorization)
- [MCP Next Version Update](https://modelcontextprotocol.info/blog/mcp-next-version-update/)
- [MCP 2026 Roadmap (David Soria Parra)](https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/)
- [50 Most Popular MCP Servers (mcpmanager.ai)](https://mcpmanager.ai/blog/most-popular-mcp-servers/)
- [Portal One — Production MCP Server with OAuth & TypeScript](https://portal.one/blog/mcp-server-with-oauth-typescript/)
- [Agent Skills Overview (platform.claude.com)](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- [anthropics/skills GitHub](https://github.com/anthropics/skills)
- [VoltAgent — awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills)
- [GitHub Blog — How to write a great agents.md (2,500 repos)](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/)
- [agents.md 官方站](https://agents.md/)
- [Vercel — AGENTS.md Outperforms Skills in Our Agent Evals](https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals)
- [vercel/next.js · canary/AGENTS.md](https://github.com/vercel/next.js/blob/canary/AGENTS.md)
- [Linux Foundation 公告:AAIF 成立](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation)
- [Agent2Agent Protocol 官网](https://a2a-protocol.org/latest/)
- [Google A2A 公告](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)
