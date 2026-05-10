# 第十六章 · Anthropic Claude Code 团队 12 个月垂直作战实录

> 第五章给出了 11 个团队的横向扫描。本章选 **公开材料密度最高的一个团队**(Cat Wu/Boris Cherny/Jenny Wen 各有 6+ 长篇 transcript),从 2024-09 Boris 入职原点纵切到 2026-Q2 的 21 个月。

---

## 16.1 起源:从"音乐播放器命令行小玩具"到 25 亿美元 ARR

**Boris Cherny 入职日期:2024 年 9 月。** 入职第一个月,他在 Anthropic 内部用刚发布的 Claude 3.5 Sonnet 做了一堆原型,最早的是 "用 AppleScript + Claude 写一个能告诉同事我正在听什么音乐" 的命令行工具 —— 纯属好玩。

但当他给这个工具加了 `bash` 和 `edit_file` 两个原始能力后,**它突然变成"日常工作不可或缺"**。这是 Claude Code 的真实起点:2024 年 10 月某一天,一个不到 200 行的 TS 脚本。

### 关键时间点

| 时间 | 事件 |
|------|------|
| **2024-11** | 第一次 dogfooding 内部发布(原型 → 内部 2 个月);核心三人 Boris、Sid(后来负责 to-do lists 和 sub-agents)、Ben |
| **2024-12 至 2025-01** | Anthropic 内部研究员("ants")的 DAU 曲线从平到几乎垂直("near-vertical growth") |
| **2025-02-24** | 公开发布 "research preview",与 Claude 3.7 Sonnet 一起上线 |
| **2025-05** | "full launch"(脱离 research preview),定价 $20/月 Pro、$200/月 Max |
| **2025-08 月底** | 约 $250M ARR run rate |
| **2025-11** | **$1B ARR**,从公开发布算起 9 个月 |
| **2026-02** | $2.5B ARR run rate,约占 Anthropic 总 $14B ARR 的 18% |

> Boris 自己的话(Lenny's Podcast 2026-02):"我从 2025 年 11 月起就再也没手写过一行代码了。"
> 这句话在 STATION F 又重复了一遍。**从那之后他完全靠 Claude Code 编辑 Claude Code。**

---

## 16.2 编制:刻意 "underfund" 的小队

Anthropic 公司层面 2026-05 约 1500 人。但 Claude Code 团队的特点是 Boris 多次强调的 **"underfund teams and give them unlimited tokens"** —— 故意让团队规模小于工作量。

### 截至 2026-05 可点名的核心成员

| 角色 | 姓名 | 职责 |
|------|------|------|
| 创始工程师 / Head | Boris Cherny | 架构、技术方向、产品愿景,2024-09 入职 |
| Head of Product | Cat Wu | 定价、打包、feature launches,2024 年底加入 |
| 工程 | Sid | to-do lists、sub-agents |
| 工程 | Ben | 早期核心三人之一 |
| 工程 | Dixon | hooks、plugins |
| 工程 | Daisy | plugins |
| 工程 | Robert | permission system |
| 工程 | Jeremy | 早期 feedback 频道自动化 bot |
| 工程 | Forrest | 快速 feature 实现 |
| 工程 | Ingo | issue 去重、PR 自动化 |
| 工程 | Conner | Agent Teams evals |
| 工程 | Noah | plugins spec |
| 工程 / 早期内部用户 | Brandon Kurkela | 数据科学背景的早期内部用户 |
| 设计 | Megan | 设计师,自己用 Claude Code 提交 PR |
| 设计 Lead | Jenny Wen | Head of Design(Claude + Cowork),2025 年从 Figma 加入 |
| 设计 | Fiona Fung | LinkedIn 公开 |
| 工程经理 | Fiona | 近期招聘的非 coder 背景 manager |

**估算总人数:截至 2026-Q2 不到 30 人**(包含工程、产品、设计)。

### 招聘哲学

1. **"Designers ship code, engineers make product decisions, PMs build prototypes and evals"**(Cat Wu, claude.com/blog)—— 角色边界刻意模糊
2. **非 coder 背景也招**:Fiona(经理)、Megan(设计师)都用 Claude Code 提交 PR
3. **Boris 的反向跳槽**:他在 2025 年中曾短暂离职去 Cursor,**两周后回到 Anthropic** —— 这件事在 2026-02 Lenny's 那期被提到,作为 "mission alignment" 的极端佐证

---

## 16.3 12 个月作战时间轴(2025-Q3 → 2026-Q2)

### 2025-Q3(Jul-Sep):从工具到平台

- **2025-07**:Claude directory 上线,连接器生态启动;3 个月后增长到 200+ connectors
- **2025-08**:内部 ARR 估算 $250M run rate;Discord/社区版块的 "antfooding" 视频开始外泄
- **2025-09-29(重大里程碑)**:**Claude Code 2.0 + Sonnet 4.5 同步发布**,包含:
  - **Checkpoint 系统**(Esc+Esc / `/rewind`,30 天保留)
  - VS Code 原生扩展、Cursor/Windsurf 兼容
  - **Sub-agents**(Explore subagent 默认 Haiku,节省主上下文)
  - **Hooks** 自动化
  - Sonnet 4.5 在 SWE-bench 上 77% 通过率,被 Boris 描述为 **"the magic moment where it actually works"**

**Q3 抛弃**:Boris 在 Every podcast 提到团队 **"unshipped the LS tool"** —— 把自建的 ls 命令包装移除,改用纯 bash。这奠定了 **"everything is dual use"** 原则。

### 2025-Q4(Oct-Dec):横向扩张与生态

- **2025-10-20**:Claude Code on the Web 上线 —— 浏览器版,跑在 Anthropic 管理的隔离 VM
- **2025-10**:Haiku 4.5 发布,Sonnet 4 级智能 1/3 成本,Claude Code 默认 sub-agent 切到 Haiku
- **2025-11-24**:Opus 4.5 发布;同时 Claude Code 业务订阅启动
- **2025-12-08**:Claude Code in Slack research preview —— @Claude 即可在 Slack 频道里跑完整 coding session
- **2025-12**:业务客户 300,000+,Boris 在 Lenny's 透露 **"Anthropic 内部 70-80% 的 ants 每天用 Claude Code"**

**Q4 抛弃**:vector embedding 索引方案被正式判死。Boris 在 Every podcast 明确说:**"vector embeddings 维护起来太麻烦 —— 重新索引、安全边界、staleness —— agentic search(让模型自己 grep / read)反而更可靠。"** 这个决定从 Q3 苗头到 Q4 形成共识。

### 2026-Q1(Jan-Mar):Agent Teams 与企业化

- **2026-01**:DAU "doubled last month";运行时数据显示 Claude Code 写的代码占公开 GitHub commits 的 **4%**
- **2026-02-05**:Opus 4.6 发布,引入 **Agent Teams** 和 1M context window;**57% 大客户启用 Agent Teams**
- **2026-02-20**:"cybersecurity flash crash" —— CVE-2026-25723 暴露 agent bypass 风险,团队开了 5 天连续 post-mortem
- **2026-02**:业务订阅在 2 个月内**翻 4 倍**
- **2026-03**:Claude Code Channels 在 Discord/Telegram 上线;**Deloitte 宣布部署给 470,000 员工**
- **2026-03-11**:Boris 在巴黎 STATION F 演讲,公开"我从 2025-11 起没手写过代码"

**Q1 模型升级生存能力**:Cat Wu 在 claude.com/blog 写道:"每次新模型发布,我们都重审所有 feature —— 很多功能在新模型上变成多余。" 例如 Sonnet 4.5 上线时,**system prompt 减少了 2000 个 token**,Plan Mode 的"边界"被推远。

### 2026-Q2(Apr - 至今 May 10):roadmap 收敛

- **2026-04-23**:Cat Wu 上 Lenny's,5 点 takeaways 被广泛转载
- **2026-04-30**:O'Reilly Radar "Everyone's an Engineer Now" 文章发布 —— 工程师 **"200% more code than a year ago"**,review 成为新瓶颈
- **2026-05-04 至 09**:6 天里发了 9 个 patch 版本(v2.1.128 → v2.1.138),节奏接近"小时级"
- **2026-05**:Anthropic 总 ARR $30B(4 月),Claude Code 占约 $5B run rate

---

## 16.4 日常仪式:每 5 分钟一次反馈

### 核心仪式:antfooding feedback channel

- 一个内部 Slack 频道,**新消息节奏每 5-10 分钟一条**
- Boris 与 Cat Wu 都把 Slack 通知保留在最高优先级
- Jeremy 写了一个机器人,自动把消息按 sentiment / 频率聚类,每天产出 top issues
- Ingo 后来扩展了一个机器人,**自动从 issue 开 PR** —— Cat 在 Lenny's 说约 **30% 的"小修小补"是机器人独立完成**

### Boris 个人日流程("How Boris Uses Claude Code" 自建网站)

- **5 个 terminal tab**,每个一个独立 git worktree
- 每个 tab 一个 Claude Code session,通常 plan mode 启动
- **每天合并 20-30 个 PR**
- 常用 slash commands:`/commit`、`/PR`、`/feature dev`、`/security review`、`/code review`
- **没有传统 standup —— demos 替代**

### Cat Wu 个人日流程

- `/PR commit`、front-end 测试用 Playwright sub-agent
- 用 Claude.ai 做战略思考、用 Claude Code 做 prototype 和 eval、用 Cowork 做知识管理
- **"我每天 wake up 的第一件事就是把 feedback 频道扫一遍"**

### On-call rotation

**没有传统的 on-call。** Boris 在 Lenny's 说"系统 99% 是云端 API + CLI 客户端,crash 也只是单用户体验问题"。重大事件(如 2026-02-20 CVE)走全员 war room。

### 工具栈(Daily 层)

Slack(核心)、Linear(issue tracking)、Notion(doc,越来越被 Cowork 替代)、**Granola**(meeting notes,AI 自动)、GitHub(代码 + Action 跑 Claude Code 自动化)、**Bun**(compile/test runtime)、**React Ink**(terminal UI)、**MCP**(外部工具协议)、**tmux**(多会话编排)。

---

## 16.5 周仪式:Demo Friday,没有 sprint

Cat Wu 在 claude.com/blog 把团队节奏总结为四 shifts,第一条是 **"Plan in short sprints with side quests"** —— 但她在 Lenny's 进一步解释:所谓 "sprint" 其实是**每周一次的 1 小时同步,不是 Scrum**。

### 周节奏(2026 成熟态)

1. **周一 60 分钟 sync**:Boris + Cat + 工程师们快速过 metrics(DAU、feedback channel sentiment、Top 10 bug);过下周打算 ship 的 features
2. **周二-周四 自由作战**:每个工程师独立选课题("side quests");prototype-first,**demo 替代 stand-up**
3. **周五 Demo Day**:所有人各 5 分钟 demo 本周做的东西,**包括失败的实验**。Cat 在 Lenny's 说这是"团队最重要的仪式,比 retro 更高频更轻量"
4. **每周一次 eval review**:跑当周新 feature 的 eval set,对比上一版

### Retro pattern

**没有固定 retro。** Cat Wu:"我们不开 retro,因为问题在 feedback channel 里实时浮现,不需要等 2 周。" 重大失败(如 2026-02-20 CVE)走 7 天 war room + post-mortem doc 形式。

### 关键细节:Anthropic 给员工 unlimited tokens

这制造了"免费实验环境"。多位重度内部用户单日消耗超 $1,000 等价 token,但**这是被鼓励的**。

---

## 16.6 月/季仪式:OKR 已死,模型版本是节拍器

**Cat Wu 在 Lenny 的 5 takeaways 里第一条**:Anthropic 的产品周期从 **6 个月 → 1 个月 → 1 周 → 1 天**。这意味着**传统季度 OKR 失效**。

### 实际的"季度节奏"是模型节奏

- Sonnet 4.5(2025-09)→ Claude Code 2.0
- Opus 4.5(2025-11)→ business 订阅启动
- Opus 4.6(2026-02)→ Agent Teams

**每次新模型发布前 2 周和后 2 周构成一个"小季度"** —— 团队会重写 system prompt、复测所有 eval、淘汰被新能力覆盖的功能。

### Post-mortem 文化

用 Cowork(自家产品)写共享 doc。最知名的两个:
1. **"Cursor 误投奔事件"**:Boris 自己写过半内部半公开的反思 "Why I came back in two weeks"
2. **2026-02-20 CVE post-mortem**:在 anthropic.com/news 上半公开

---

## 16.7 Eval 体系:从"vibes"到 50-case golden set

Boris 在 Latent Space(2025-Q1)有一句被反复引用的话:**"我们的成功 metric 主要是 vibes"**。但到 2026 年这已经不是事实 —— 只是节奏与传统软件不同。

### Eval 现状(2026-Q2)

1. **Internal evals(不公开)**:每个 feature 一组 hand-crafted eval cases,规模通常 20-100 个 prompt-output pair。"Custom evals" 是 Cat Wu 在 claude.com/blog 用的词
2. **Golden dataset**:根据 TribeAI/claude-evals(第三方按 Anthropic 公开 patterns 做的开源版本),**~50 case 是 Anthropic 官方推荐的 baseline 大小**
3. **Eval 类别**(从公开访谈拼出来):
   - Code generation correctness(最大,~30%)
   - Tool use precision(哪些 bash 命令应该被允许、哪些应该 ask for permission)
   - Long-context recall(在 1M token context 里找信息)
   - Plan quality(Plan Mode 输出的可执行性)
   - Permission classifier accuracy(auto mode 关键)
4. **CI 集成**:每次 PR merge 前在 CI 里跑核心 eval;通过 `claude -p` headless 模式做 batch
5. **Judge prompts**(从泄露的 system prompts 推断):用 Opus 做 judge,Haiku 做 generator,pairwise comparison + rubric scoring 双轨

**Conner 是 Agent Teams 的 eval owner**;**Noah 是 plugins spec 的 eval 责任人**。

**100% 内部测试由 Claude 写**(Boris 在 Every);**100% lint 规则由 Claude 写** —— eval 自身也大部分被 Claude Code 自动维护。

---

## 16.8 工具栈与理由

| 类别 | 工具 | 为什么 |
|------|------|--------|
| Issue tracking | Linear | 速度快、API 友好、Claude Code 能通过 MCP 直接读写 |
| Knowledge | Cowork(自家) | 强制 dogfooding;2025-Q4 起取代 Notion |
| 通信 | Slack(核心 antfooding 频道) | 5-10 分钟一条 feedback 的节奏只有 Slack 能承载 |
| Meeting notes | Granola | AI 自动转录,PM 不再当书记员 |
| Code | GitHub + Actions | 公开仓库 anthropics/claude-code,Action 跑自家 CI |
| Runtime | Bun | 编译速度比 Node 快 4-10 倍,对 CLI 体验关键 |
| UI | React Ink | terminal UI 抽象层 |
| 集成协议 | MCP(自家 spec) | 强制 ecosystem 用同一个协议 |
| Multi-session | tmux + git worktree | 平行作战 |
| Headless / CI | `claude -p` | 整合到 GitHub Action、pre-commit |
| Eval | 自研 + TribeAI/claude-evals 模式 | 50-case golden + judge model |

**理由背后的哲学**(Boris 在 Every):**"Pick the simplest thing that could work"** —— markdown CLAUDE.md 优于知识图谱、agentic grep 优于 vector index、bash 优于自建工具包装、$6/天 PAYG 优于复杂订阅。

---

## 16.9 5 个做对的决定

1. **2024-Q4 选择终端而不是 IDE 插件**。Boris 反复说"如果当时做 VSCode plugin 就死了" —— 终端意味着可脚本化、可与 tmux/cron 组合,Unix 哲学的延续。VS Code 扩展直到 2025-Q3 才作为补充上线。
2. **2025-Q1 Pay-as-you-go 而不是订阅**。**$6/天平均消费定价**,让重度用户合理消费、轻度用户低成本试。Cursor 当时是 $20/月固定,重度用户被迫被补贴轻度用户。Claude Code 反过来。
3. **2025-Q2 拒绝向量数据库,All-in agentic search**。Boris:vector index 的 staleness、re-index 成本、安全边界都是问题;让模型自己 grep + read,**"slow but correct"**。这导致 Claude Code 比 Cursor 慢但 recall 高得多。
4. **2025-Q3 CLAUDE.md 这种 markdown memory,而不是结构化 schema**。一个文件、一种语法、check into git。代价是没有强类型,**收益是任何团队成员(包括非 coder)都能编辑**。
5. **2025-Q4 把 LS tool 等自建工具拆掉,回归裸 bash**。这是反直觉决定 —— 大多数团队倾向"为模型做更安全的封装"。Anthropic 反向认为模型直接用 bash 更可靠,**把权限移到 permission classifier 层**。

---

## 16.10 3 个做错并被废弃的决定(最有价值的部分)

1. **Vector embedding 索引(2025-Q1 → Q2 废弃)**。Boris 公开承认早期试过类似 Cursor 的索引方案。**"维护成本、re-index 时机、stale 问题"加上 enterprise 的 security review 把这条路堵死**,最终改 agentic search。

   > **教训**:在 agent 时代,模型 IO 速度的提升让 RAG 在很多场景下不必要。

2. **自建 LS / Read / Write 等工具的"安全包装层"(2025-Q3 废弃)**。最初 Claude Code 像所有 agent framework 一样,给模型暴露的是 `LS()`、`Read()`、`Edit()` 等抽象 tool。后来 Boris 发现:bash 本身就是这套接口,**模型用 bash 训练数据更多,结果更稳**。**"unshipped the LS tool"** 成了团队内部的隐喻 —— 指代"减法比加法重要"。

3. **Top-down 企业销售 motion(2025-Q1 反转)**。Boris 在 STATION F 透露:他和 Cat 最初的 GTM 辩论里,他倾向走传统大客户销售路径 —— "先签下 Goldman、JPM 这类 anchor logo"。**但实际数据显示 grassroots 个人开发者订阅曲线远比 enterprise 快。** 2025-Q2 决定逆转,先做 Pro/Max 自助订阅,企业版(Team/Enterprise admin controls)拖到 2025-Q4 才补足。**这个决定把 $1B ARR 的时间点从可能的 12 个月压缩到 9 个月。**

**[次要的反向决定]**:Plan Mode 在 2025-Q1 一度被设为默认。后来发现简单任务下 Plan Mode 反而拉低体验,2025-Q2 改为 opt-in(按 Tab 进入)。

---

## 16.11 团队赖以生存的指标

**北极星指标**(公开访谈推断):**weekly active engineers writing >100 lines/day via Claude Code** —— "有效活跃"度量,过滤一次性试用。

**护栏指标**:
- **Claude Code 写出的 PR 在合并后引入 CVE 的比例**(无安全 guidance 时 87% PR 含漏洞,这是被作为护栏跟踪的核心数字)
- **Cost per incremental PR 比例 ≤ 4:1**(自家 production code 实测)
- **Eval set 通过率回归**:每次模型升级前对核心 50-case set 跑通过率,下降即 block
- **Feedback channel 情绪指数**(Jeremy 的机器人聚合)

**可公开的标杆数字**:
- DAU 月增 100%(2025-12 → 2026-01 区间)
- 占公开 GitHub commits 4%(2026-Q1)
- ARR:$1B(2025-11)→ $2.5B(2026-02)
- Anthropic 内部 70-80% ants 日活
- 工程师人均代码产出 +200%
- Sonnet 4.5 在真实 GitHub issue 上 77% 通过率
- 业务客户 300,000+
- Stack Overflow 调查中 46% "most loved",**是 Cursor 19% 的 2.4 倍**

---

## 16.12 已公开的 2026-2027 路线图碎片

通过 Boris、Cat、Jenny 最近访谈拼出:

1. **Agent autonomy 时长从 30 小时 → "days"**(Boris 在 Every 2025-10);Claude 5(未发布,预计 2026-H2)
2. **Cowork 集成深化**:把 Claude Code 与 Cowork 的"项目记忆"打通(Jenny Wen 在 Lenny's)
3. **多 agent 团队(Agent Teams)成为主流**:Opus 4.6 引入,预计 2026-Q3 进入 Pro 默认
4. **Permission classifier 从规则转纯 ML**:auto mode 的 fallback 阈值会被持续放宽
5. **Mobile**:Claude Code 移动端原生客户端在 internal 测试,预计 2026 下半年发布
6. **新的 PM 范式书写**:Cat Wu 暗示要发布"PM on the AI Exponential"系列文章/讲座,预计 AI Engineer Summit 2026 主题演讲

---

## 16.13 综合评论:这个团队为什么独特

回到大问题 —— 其他 AI 团队该从 Claude Code 学什么?

1. **"underfund + unlimited tokens" 是新的资源公式**。少招人、给现有人无限模型预算,比反过来更高 ROI。
2. **Antfooding 是结构性优势,不是文化口号**。Anthropic 之所以能 5 分钟一次 feedback,是因为整个公司 1500 人里有上千个高质量目标用户 —— 这点 Cursor、Sierra、Decagon 复制不了。
3. **模型节奏 = 产品节奏**。OKR/季度计划在 Claude Code 不存在,因为模型 6 周一次升级,迫使所有 feature 重审。**其他团队需要建立类似的"外部节拍器"。**
4. **减法决策被低估**。最强的 5 个决定里,**3 个是"砍东西"**(砍 vector index、砍 LS tool、砍 enterprise sales priority)。
5. **角色边界蒸发是必然**。Megan(设计师)写 PR、Boris(工程)做产品决策、Cat(PM)写 prototype 和 eval —— 这不是噱头,是 60% 时间被 AI 加速后的合理重组。

---

## 资料源

- [Lenny's Newsletter — How Anthropic's product team moves faster than anyone else | Cat Wu (2026-04-23)](https://www.lennysnewsletter.com/p/how-anthropics-product-team-moves)
- [Lenny's Newsletter — Head of Claude Code: What happens after coding is solved | Boris Cherny (2026-02)](https://www.lennysnewsletter.com/p/head-of-claude-code-what-happens)
- [Every / AI & I — How to Use Claude Code Like the People Who Built It (2025-10)](https://every.to/podcast/how-to-use-claude-code-like-the-people-who-built-it)
- [Latent Space — Claude Code: Anthropic's Agent in Your Terminal](https://www.latent.space/p/claude-code)
- [Pragmatic Engineer — How Claude Code is built](https://newsletter.pragmaticengineer.com/p/how-claude-code-is-built)
- [Lenny — The design process is dead | Jenny Wen (2026-03)](https://www.lennysnewsletter.com/p/the-design-process-is-dead)
- [claude.com/blog — Product management on the AI exponential | Cat Wu](https://claude.com/blog/product-management-on-the-ai-exponential)
- [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices)
- [Boris Cherny at STATION F (2026-03-11)](https://stationf.co/news/boris-cherny)
- [GitHub — anthropics/claude-code releases](https://github.com/anthropics/claude-code/releases)
- [Anthropic news — Claude Code 2.0 + Sonnet 4.5 (2025-09-29)](https://www.anthropic.com/news/enabling-claude-code-to-work-more-autonomously)
- [Claude Code on the Web launch (2025-10-20)](https://mlq.ai/news/anthropic-launches-claude-code-on-the-web/)
- [TechCrunch — Claude Code in Slack (2025-12-08)](https://techcrunch.com/2025/12/08/claude-code-is-coming-to-slack-and-thats-a-bigger-deal-than-it-sounds/)
- [TechCrunch — Anthropic releases Opus 4.6 with Agent Teams (2026-02-05)](https://techcrunch.com/2026/02/05/anthropic-releases-opus-4-6-with-agent-teams/)
- [O'Reilly Radar — Everyone's an Engineer Now (2026-04-30)](https://www.oreilly.com/radar/everyones-an-engineer-now/)
- [Stormy AI — Inside Claude Code's GTM Strategy](https://stormy.ai/blog/claude-code-gtm-strategy-anthropic-revenue-2026)
- [How Boris Uses Claude Code(自建网站)](https://howborisusesclaudecode.com/)
- [TribeAI/claude-evals (GitHub)](https://github.com/TribeAI/claude-evals)
