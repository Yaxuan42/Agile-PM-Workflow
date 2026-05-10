# 第十一章 · Agent UX 模式库

> 9 类可复用的具名 UX 模式 + 真实产品案例 + 反模式 + 10 条 Heuristics。每类都包含:**定义 / 具名产品 / 何时用 / 何时不用 / 反模式**。

2026 上半年 Agent 产品已度过"炫技期"进入"制度化期"。Anthropic Claude Code Plan Mode、Cursor Plan Mode + Checkpoint、Replit Agent App History、Linear for Agents、Notion 3.0 Agents、Slack Agentforce、Microsoft 365 Copilot Agent Registry —— 这些产品在 2025-09 至 2026-05 之间集中落地了一组高度可复用的 UX 模式。

---

## 11.1 模式 1 · Intent Preview / Plan Preview

### 定义
Agent 在"执行任何不可逆动作前",把它对任务的理解、要做的步骤、要碰的文件/工具,作为一份**可读、可改、可拒绝的工件**呈现给用户;用户通过 Approve / Edit / Reject 决定是否进入执行阶段。

### 具名案例
- **Anthropic Claude Code Plan Mode**:`Shift+Tab` 在 default → acceptEdits → plan 三模式间循环;Plan Mode 下 Claude 只研究、不写文件,完成后弹 "Approve and auto-allow / Approve but ask each / Continue discussing"
- **Cursor Plan Mode**:输入框按 `Shift+Tab` 进入,自动生成 `.plan.md` 在编辑器面板打开,用户编辑 to-do、点 **Build** 才进入 Agent Mode;2026 加 "Build in Parallel"
- **GitHub Copilot Workspace**:三层工件 —— Proposed Specification → Plan(文件级行动)→ Diff;每一层都可逐条编辑;"View references" 按钮让用户审计 Agent 选了哪些文件
- **Devin 2.0**:把 Plan 当成"随时间频繁修订的活文档",每次发现新约束都更新 Plan
- **Replit Agent**:在 Build 阶段强制 Plan Preview

### 何时用
- 高代价、不可逆任务:写代码、改数据库、发邮件、调用付费 API
- 用户与 Agent 上下文理解可能错位的任务
- 多步骤(>3 step)且步骤间有依赖

### 何时不用
- 单步、可逆、低代价(改一句话、补全一行代码)
- 高频重复操作 —— 反复审批触发 **Confirmation Fatigue**(2026 头号反模式)
- 实时对话场景(语音、客服),Plan 渲染会破坏节奏

### 反模式
1. **不可编辑的"假计划"**:只展示不让改 = 走流程
2. **Plan 与实际执行不一致**:嘴上说 Plan A,执行 Plan B,且不解释偏离
3. **过密的技术 Plan**:把 30 条 shell 命令糊在一起 = 没有 Plan

---

## 11.2 模式 2 · Streaming + Progress UX

### 定义
Token 边生成边渲染、Tool call 边调用边播报、长任务用"思考中…" + 阶段性进度 checklist 维持用户在场感。**研究显示流式响应在等长延迟下被感知为快 40-60%。**

### 具名案例
- **ChatGPT**:Apps SDK 暴露 `ui/notifications/tool-input` 与 `ui/notifications/tool-result` 让插件渲染工具调用进度卡片
- **Claude.ai / Claude Code**:思考链以 `<thinking>` 折叠块呈现,默认收起;工具调用显示为彩色徽章(Read / Edit / Bash)
- **Perplexity**:三阶段进度 —— Searching → Reading sources → Writing answer
- **Cursor**:Agent 执行时左侧时间线 "Thinking → Reading file X → Editing Y → Running tests",失败 Tool call 红色标记 + 就地重试

### 反模式
1. **永恒 Spinner**:只有"转圈圈"没有阶段说明
2. **Tool call 黑盒**:只说"调用了工具"不说调了什么
3. **流式但无法中断**:边流边写但 Stop 按钮不生效(Zed、Qwen、早期 Cursor 都踩过)

---

## 11.3 模式 3 · Confidence Signaling

### 定义
Agent 在输出旁明确标注"我有多确定"、"来源是哪里"、"哪段是猜的"。包括 inline 引用、置信徽章、refusal 时给出原因、对模糊问题主动反问。

**Anthropic 解释性研究证实**:**"置信度"与"准确率"由模型内不同电路驱动,会失耦** —— 信号必须显式设计,不能只靠模型"自我感觉"。

### 具名案例
- **Perplexity**:每条事实后挂 [1][2][3],hover 弹出来源卡片;但 Perplexity **没有 confidence badge**,这是公认 UX 缺口
- **Claude**:Anthropic 在 system prompt 显式给 Claude "permission to admit uncertainty",输出 "I'm not sure, but…";**hallucination 率因此下降 30%+**
- **Glean**:每条答案均挂可点击源链接,配合 AWARE 框架(Actor Intent / Work Context / Autonomous Guardrails / Real-time Risk / Ecosystem Observability)
- **You.com**:答案分段标注 Source,支持"Cite this paragraph"

### 反模式
1. **假引用**:编造看似存在但 404 的引用 —— Bard/Gemini 早期、ChatGPT 4o 都被记录过
2. **置信度通胀**:对所有回答都说"I'm 95% confident",用户很快忽略
3. **"As an AI, I cannot…"死路**:无原因、无替代、无升级路径

---

## 11.4 模式 4 · Permission UX

### 定义
对 Agent 的工具/范围/数据访问做**细粒度、可观测、可撤销**的授权。

### 具名案例
- **Claude Code 权限系统**:四级 deny → ask → allow,首次匹配生效;Bash 通配符(`Bash(npm test:*)`);`/permissions` 命令打开 UI;弹窗有"Always allow"复选框写入 `~/.claude.json`。**2026-03 Auto Mode 用 Sonnet 4.6 分类器对每次 tool call 实时打分**,自动放行低风险、拦截高风险
- **Cursor Auto-Run / YOLO Mode**:Agents 菜单可选"Run Everything" + 自定义 allowlist/denylist + "禁止删除文件"复选框
- **Microsoft 365 Copilot Agent Registry**:管理员 admin center 内集中管控,可按用户/组授权、按 Security Template 限制
- **Linear for Agents**:Agent 通过 OAuth 安装时显式列出 scope(可读哪些 Issue、可写哪些字段)

### 反模式
1. **Binary Skip-Permissions**:只有"全允许 / 每次问"两档,逼用户选 YOLO 后裸奔
2. **同意疲劳轰炸**:每条 `ls` 都弹窗,用户开始无脑点 Allow —— Rippling 列为威胁 T10 "Overwhelming Human-in-the-Loop"
3. **Scope 不可见**:OAuth 同意页只说"访问您的工作区"

---

## 11.5 模式 5 · Stop / Undo / Rollback

### 定义
覆盖三个时间尺度的"后悔权":**Stop**(执行中立刻打断)、**Undo**(刚完成的一步快速撤销)、**Rollback**(回到几小时前的某个 checkpoint)。

### 具名案例
- **Claude Code Esc 中断 + 注入新指令**:按 Esc 立刻终止当前 turn,可在终止时直接输入新提示词替换上下文;"Rewind" 功能允许回到上一条用户消息之前的工程状态
- **Cursor Checkpoints**:Agent 模式下每次 edit 前自动快照,与 Git 历史隔离;Restore Checkpoint 一键回滚 AI 改动。**已知 bug**:subagent 改动不会被回滚
- **Replit Agent App History**:基于 Git 提交 + Neon 数据库分支的双层快照;每次 Agent 完成"doneness"自动 commit;App History 列出每个版本截图、可选回滚数据库;**最近 7 天可在"Preview Mode"预览旧版本**再决定回滚
- **Devin**:工作在隔离 VM 中,不可逆操作(push、deploy)需用户显式确认

### 反模式
1. **Stop 按钮假动作**:点了没反应(Zed Issue #50592、Claude Code Issue #3455)
2. **中断即全废**:Cursor 早期版本中断会自动撤销所有已完成工作 —— 用户失去半小时进度
3. **Checkpoint 不透明**:Restore 后看不到"将丢失什么",用户不敢点
4. **Rollback 不带数据库**:代码回了,DB schema 还是新版,应用直接崩 —— Replit 通过 Neon Branches 解决

---

## 11.6 模式 6 · Agent-as-Coworker

### 定义
把 Agent 当成 workspace 里的一等公民:有名字、头像、状态(online/busy/offline)、可被 @mention、可被 assign issue、可发评论、可加入项目。**这是 2025 Q4-2026 Q1 最大的 UX 范式迁移。**

### 具名案例
- **Linear for Agents**(2025-05-20 首发,2026-03-24 Linear Agent 自家产品):Agent 是 first-class user,可被 @mention、assign Issue、参与 Project/Document、Slack/Teams 同步出现;**Agent 安装时必须提供短而独特的名字与图标**
- **Slack Agentforce**:Agent 与人类一样能被 @ 进频道、DM、thread;Slack 提供 Agent Directory;**Agent 可主动发起对话**(proactive)
- **Notion 3.0 Agents** + **Notion 3.3 Custom Agents**(2026-02-24):Agent 可被分配"Instructions Page"作为长期记忆,执行 20 分钟级别的多步任务,trigger/schedule 自动唤起
- **GitHub Copilot Workspace / Coding Agent**:Agent 可作为 PR 作者、Issue Assignee、Review Requested 对象

### 反模式
1. **过度拟人化**:头像放真人照、名字叫 "Sarah from Marketing" —— 触发 Uncanny Valley、误导责任归属
2. **Agent 无状态**:头像永远在线但实际在 sleep
3. **责任真空**:Agent 改了文档却没有 audit trail 指向用户配置/触发,出错没人负责

---

## 11.7 模式 7 · Conversational vs Spatial vs Hybrid

### 定义
- **Conversational**:线性对话流,prompt-first(ChatGPT、Claude.ai、Perplexity)
- **Spatial**:画布/编辑器/3D 空间为主,AI 作用于局部对象(v0 Canvas、Lovable Visual Edits、Figma Make、Cursor 主编辑区)
- **Hybrid**:左右分栏,主区域空间式 + 侧边栏会话式(Cursor 右侧 Composer、Lovable 左侧 Chat、Notion 内嵌 AI、ChatGPT Canvas)

NN/g 把它定位为"自 60 年来 UI 第三范式 — Intent-based outcome specification"。

### 何时用各自
- **Conversational**:开放探索、不知道想要什么、研究/学习;低门槛对新手友好
- **Spatial**:产物本身是空间性的(UI、设计稿、地图、白板);用户已知目标,要精确控制
- **Hybrid**:需要"对话指导 + 直接操作"并存的复杂工作流(代码、长文、设计应用)

UX Collective 总结:**Lovable 选左栏因为 AI 是核心引擎,Cursor 选右栏因为 AI 是辅助。**

### 反模式
1. **Chat as Hammer**:把所有 AI 功能塞进对话框
2. **Spatial without Memory**:画布上 AI 只看局部元素不看整页上下文
3. **Hybrid 但 Chat 与 Canvas 状态不同步**:Chat 里说"改了 Header"但 Canvas 没动

---

## 11.8 模式 8 · Error / Refusal UX

### 定义
当 Agent 失败、超时、不会做、拒绝做时,给用户**原因 + 替代路径 + 升级到人**三件套,而不是死路一条。

### 具名案例
- **Anthropic Claude**:对越权请求拒绝时给原因("我无法生成这类内容,因为…")+ 建议替代("但我可以帮你用合法方式达成相似目标:…");**Anthropic 公开承诺减少 sycophancy**(2026-03 "Protecting the well-being of our users")
- **Replit Agent**:报错时主动建议回滚到上一个绿色 Checkpoint
- **Glean**:AWARE 框架的 Real-Time Risk Scoring & Blocking —— 拦截后给"为什么被拦"+ 升级到 IT Admin
- **Zapier AI Drafting**:Workflow 草稿失败时显示哪一步失败、可编辑、可转交人审

### 反模式
1. **"As an AI language model, I cannot…"死路** —— 无原因、无替代、无升级
2. **Sycophantic Failure**:模型不会做却假装做了,瞎编结果
3. **Cryptic Error 503**:把后端错误码暴露给最终用户;应改为 "We're at capacity, try in 30s, or save this prompt for later"
4. **Silent Failure**:Tool call 失败但 Agent 没察觉,继续基于错误结果推理(MindStudio 报告六大失败模式之首)

---

## 11.9 模式 9 · Trust-building Micro-interactions

### 定义
不是单一大功能,而是一组**小的、长期累积的信号**:展示推理过程、给出引用、主动承认不确定、追问 clarifying questions、记住用户偏好。

**Linus Lee 框架**:**trust 与 control 的二元性** —— 给用户更多 control 反过来生成 trust。

### 具名案例
- **Claude Projects / Memory**(2025-2026):跨会话记住项目上下文,在使用记忆时**显式标注**"Based on what you told me last week…"
- **ChatGPT Memory**:有"记住此事"与"忘记此事"快捷操作,设置面板列出所有已记忆条目
- **Notion Agent Instructions Page**:把 Agent 记忆显式做成**用户可读可改的 Notion Page** —— 记忆是工件,不是黑盒
- **Perplexity Follow-ups**:答案下方提供 3-4 个 clarifying / 深挖问题
- **Cursor Apply with Reasoning**:在生成 diff 旁解释 "Because your tests use Vitest, I imported from `vitest` not `jest`"
- **Devin**:遇到模糊点主动反问而非瞎猜,把 "ask before assume" 做成默认行为

### 反模式
1. **隐式记忆**:不告诉用户记了什么,某天突然弹出旧偏好
2. **过度反问**:每个任务都先问 5 个 clarifying questions,变 Slow Bot
3. **"记得太多但不会忘"**:用户已变,Agent 还按半年前偏好走
4. **Empty Reassurance**:每段输出都加 "Great question!" "I'd be happy to help!" —— Anthropic 反复强调要砍掉 sycophancy

---

## 11.10 Agent UX Heuristics 一页纸 Cheat Sheet(10 条)

1. **Plan Before Act**:任何 >3 步或不可逆操作,先输出可读、可改、可拒的 Plan
2. **Stream Thought, Not Just Tokens**:不只流式文字,还要流式播报当前在调哪个工具、读哪个文件、为什么
3. **Confidence ≠ Accuracy**:置信度是单独的设计层,要主动设计 "I don't know" 表达通道
4. **Cite or Die**:信息密集型回答必须挂可点击源,且源不能伪造
5. **Permission Granularity**:别只给 YOLO / Ask Each 两档;deny → ask → allow 三级 + "Always allow this kind" + runtime 风险分类器
6. **Reversible by Default**:Stop 按钮真生效;Undo 不丢已完成工作;Rollback 把代码 + 数据 + 上下文一起带回去
7. **Match Stakes to Friction**:低代价可逆 = toast + undo;高代价不可逆 = Intent Preview 强同意。**Confirmation Fatigue 是头号敌人**
8. **Coworker, Not Mascot**:Agent 应有名字、scope、audit trail,但不要冒充人
9. **Hybrid UI Wins for Productivity**:复杂工作流用 Chat + Canvas 双栏,但二者必须状态同步
10. **Fail Like a Pro**:错误时给原因 + 替代 + 升级到人;砍掉 sycophancy 与 "As an AI…" 死路;主动追问优于瞎猜

---

## 资料源

- [Designing For Agentic AI: Practical UX Patterns — Smashing Magazine](https://www.smashingmagazine.com/2026/02/designing-agentic-ai-practical-ux-patterns/)
- [Identifying Necessary Transparency Moments — Smashing Magazine](https://www.smashingmagazine.com/2026/04/identifying-necessary-transparency-moments-agentic-ai-part1/)
- [Claude Code permission modes](https://code.claude.com/docs/en/permission-modes)
- [Claude Code Auto Mode — Anthropic](https://www.anthropic.com/engineering/claude-code-auto-mode)
- [Cursor Plan Mode Blog](https://cursor.com/blog/plan-mode)
- [Cursor Checkpoints](https://cursor.com/docs/agent/chat/checkpoints)
- [GitHub Copilot Workspace Overview](https://github.com/githubnext/copilot-workspace-user-manual/blob/main/overview.md)
- [Replit Checkpoints and Rollbacks](https://docs.replit.com/replitai/checkpoints-and-rollbacks)
- [Replit Inside the Snapshot Engine](https://blog.replit.com/inside-replits-snapshot-engine)
- [Linear for Agents](https://linear.app/agents)
- [Linear AI Agents Docs](https://linear.app/docs/agents-in-linear)
- [Slack AI Agents](https://slack.com/ai-agents)
- [Introducing Notion 3.0](https://www.notion.com/blog/introducing-notion-3-0)
- [Microsoft 365 Copilot Control System](https://learn.microsoft.com/en-us/copilot/microsoft-365/copilot-control-system/security-governance)
- [Glean AWARE framework](https://www.glean.com/blog/agentic-security-aware)
- [Perplexity Citation-Forward Answers — Unusual.ai](https://www.unusual.ai/blog/perplexity-platform-guide-design-for-citation-forward-answers)
- [NN/g — AI: First New UI Paradigm in 60 Years](https://www.nngroup.com/articles/ai-paradigm/)
- [UX Collective — Where should AI sit in your UI?](https://uxdesign.cc/where-should-ai-sit-in-your-ui-1710a258390e)
- [Latent Space — Karina Nguyen on Agent Reasoning Interface](https://www.latent.space/p/karina)
- [Latent Space — Linus Lee on AI/UX](https://www.latent.space/p/ai-interfaces-and-notion)
- [Anthropic — Protecting well-being of users](https://www.anthropic.com/news/protecting-well-being-of-users)
- [Confirmation Fatigue and the Protocol Gap](https://changkun.de/blog/ideas/human-in-the-loop-agents/)
