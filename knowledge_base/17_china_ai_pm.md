# 第十七章 · 中国 AI PM 全景

> Round 1 + 2 全部是 Western 视角(Anthropic / OpenAI / Google / Cursor / Sierra / Notion / Vercel)。中国是 2026 年两大 AI 超级生态之一,本章填补这一缺口。

---

## 17.1 双轨格局:开源派 vs 超级应用派

2026 年的中国大模型生态呈现"双轨"结构:
- **开源派**:DeepSeek、Qwen、GLM、Kimi —— 向 Hugging Face 输出全球影响力
- **超级应用派**:豆包、元宝、文心、腾讯混元 —— 绑定大厂 C 端入口与企业云

### 头部模型一览(截至 2026-05)

| 厂商 | 旗舰模型 | 发布日 | 总参/激活 | 上下文 | 价格(百万 tokens) | 开/闭源 |
|------|---------|-------|----------|-------|-------------------|--------|
| DeepSeek | V4-Pro / V4-Flash | 2026-04-24 | 1.6T / 49B(Pro)、280B / 13B(Flash) | 1M | 输入 $0.145、输出 $1.74 | 开源(MIT-like) |
| Alibaba Qwen | Qwen 3.6-Max-Preview、Qwen 3.6-27B 密集 | 2026-04-20、3.5 系列于 2026-02 | 397B/17B(3.5)、27B 密集 | 256K | Qwen-Long 输入 ¥0.0005/千 tokens(~$0.07/M) | 全部开源 |
| Moonshot Kimi | K2.6 | 2026-04-20 | 1T / 32B | 256K | OpenRouter 列示约 $0.55/$2.20 | 开源权重(agent swarm 系统封闭) |
| ByteDance Doubao | Seed 2.0 Pro | 2026-02-14 | MoE | 256K | $0.47/$2.37(GPT-5.2 的 1/4-1/6) | 闭源 |
| Tencent Hunyuan | Hy3 Preview、Hunyuan 3.0 | 2026-04 | 295B / 21B | 256K | 2026-03-13 起涨价:输入 ¥0.004505/千 tokens(**+460%**) | Hy3 Preview 开源 |
| Zhipu | GLM-5(744B/40B) | 2026-02-11 | 744B / 40B | — | GLM-4-Plus ¥50→¥5/百万 tokens;GLM-5.1 +10% | 开源权重 |
| MiniMax | M2.5 | 2026-02 | 高度稀疏 MoE | — | OpenRouter 列示低价 | 开源权重 |
| StepFun(阶跃星辰) | Step 3.5 Flash | 2026-03 | — | — | OpenRouter Top 4 Agent 场景 | 部分开源 |
| Baidu | ERNIE 5.1 | 2026-05-09(5.0 于 2025-11-13) | 5.1 总参约为 5.0 的 1/3 | — | Baidu Cloud Qianfan 内定价 | 闭源 |
| 01.AI(零一万物) | 已停止万亿级训练;改用 DeepSeek 底模做行业模型 | 转型于 2025 | — | — | 私有化部署收费 | — |
| Baichuan | Baichuan-M3(医疗专用) | 2026-01 | — | — | C 端单独定价 | — |

### 关键观察

- **DeepSeek V4 是 2026 年最重要的开源里程碑**:1.6T 参数、SWE-bench Verified 80.6%、LiveCodeBench 93.5、Codeforces 3206 超过 GPT-5.4;**输入价格相比 GPT-5.5 / Claude Opus 4.7 便宜约 7×**。**首个打通华为昇腾 + 英伟达双平台**的国产模型。
- **Qwen 在 Hugging Face 反超 Meta**:Qwen 衍生模型超 20 万个,HF 累计下载突破 10 亿次,**全球开源榜 TOP 10 中占 8 席**。
- **价格反弹信号**:2024-2025 大模型 API 经历 90%-97% 大幅降价,但 **2026 年起出现反转** —— 腾讯混元 3 月 13 日 Hunyuan 2.0 Instruct 输入价上调 **460%**;GLM-5.1 上调 10%;阿里云 AI 算力存储 4 月 18 日全线涨价至 34%。原因:国内日 token 调用量从 2024 年初的 1000 亿飙升到 2026-03 的 **140 万亿**(增长 1400×),推理成本结构反转。
- **"算力主权"问题**:DeepSeek V4 同步适配昇腾,反映中国 PM 必须考虑双栈部署 —— 这是西方 PM 没有的维度。

---

## 17.2 Agent 产品:从 Manus 引爆到大厂入口争夺

### Manus 现象与"中国式 Agent 引爆"

**蝴蝶效应公司**(创始人肖弘、首席科学家季逸超),2025-03-06 邀请制 beta 上线。Demo 视频 20 小时内观看破百万;邀请码灰产从 ¥999 炒至 ¥50,000;闲鱼挂牌成交价过万。

**商业化进程**:
- 上线 **8 个月即年化营收 $100M+**
- 收入 run rate 突破 $125M
- 母公司将总部从武汉/北京迁至**新加坡**(避免美方 Treasury 审查)
- **2025-12-30 被 Meta 收购**(对中国 PM 圈影响深远 —— Manus 团队拒绝多地中国地方政府投资意向以保留出海路径)

### 大厂 Agent 矩阵(2026)

| 平台 | 母公司 | 用户/调用规模 |
|------|-------|--------------|
| **豆包 / Doubao App** | 字节跳动 | **MAU 3.45 亿**(中国第一) |
| **通义千问 App** | 阿里 | MAU 1.66 亿 |
| **DeepSeek App** | DeepSeek | MAU 1.27 亿;月人均 41.7 次 |
| **腾讯元宝** | 腾讯 | 接入 Hunyuan + DeepSeek;2026-01 推出"元宝派"社交群;春节 10 亿元红包 |
| **Coze 2.0 / Coze Space** | 字节 | 智能体已建超 800 万;Coze Studio 开源后 GitHub Star 持续上升 |
| **DingTalk Wukong**(钉钉悟空) | 阿里 | 2026-03-16 AI 钉钉 2.0 发布;服务 2000 万企业用户 |
| **腾讯元器** | 腾讯 | 智能体平台,微信生态分发 |
| **Yuanbao Groups(元宝派)** | 腾讯 | 2026-01-27 内测;多人对话、健身打卡、消息总结 |
| **Genspark Super Agent** | 华人创始团队(北美注册) | ARR 接近 $200M、月活 200 万、日本市场 1500 万次/月 |

### 与西方对比

| 维度 | 西方 | 中国 |
|------|------|------|
| 主入口 | ChatGPT、Claude.ai、独立 web | **微信、钉钉、飞书、企微**(IM 内嵌) |
| 计费 | 月订阅($20、$200) | 免费 C 端 + 企业 API + 私有化部署一次性大单 |
| 引爆方式 | Twitter / Hacker News / 媒体头条 | **小红书、知乎、B 站、微信公众号深度长文 + 邀请码灰产** |
| 地方政府关系 | 不存在 | 多地国资入股、政企客户首选本地厂商 |
| 出海决策 | 默认全球 | 对模型团队是"是否出海决定总部所在地"(Manus 迁新加坡) |

---

## 17.3 代码 Agent:渗透率而非 SWE-bench 才是 KPI

| 工具 | 厂商 | SWE-bench 表现 | 内部渗透 |
|------|------|--------------|---------|
| **通义灵码(Tongyi Lingma)** | 阿里云 | Lingma SWE-GPT-72B:30.20%;7B:18.20% | 阿里云内部约 40% 代码 AI 辅助生成 |
| **MarsCode / TRAE** | 字节 | MarsCode Agent SWE-bench Lite 修复 39.33% | TRAE MAU 100 万+ |
| **CodeBuddy(腾讯云代码助手)** | 腾讯 | — | **腾讯内部 85% 程序员使用,编码时间缩短 40%;90% 开发岗位接入** |
| **文心快码(Baidu Comate)** | 百度 | — | 百度内部 AI 辅助代码占比约 43% |
| **iFlyCode(讯飞星火飞码)** | 科大讯飞 | — | 教育部 18 个 "AI+ 高校" 典型案例;500 所高校落地 |
| **CodeGeeX** | 智谱 | 基于 GLM | 老牌开源 IDE 插件,海外亦有用户 |

### PM 视角的差异

- **客户构成不同**:西方代码 PM 主要服务个人开发者+SaaS 销售(Cursor、Claude Code、Copilot)。**中国代码 PM 主要服务大厂内部 + 政企私有化**(CodeBuddy 90% 内部覆盖说明 "自用驱动外销" 是常态)。
- **KPI 不同**:西方看 ARR、付费转化;**中国大厂看内部渗透率**和 "X% 代码由 AI 生成" 等公司级 OKR。
- **基线挑战**:阿里、腾讯、百度 2026 年初 AI 辅助代码生成比例 40%-43%,**已超 GitHub Copilot 公布的全球水平**。

---

## 17.4 垂直行业 AI(金融、医疗、教育是商业化最深的三条赛道)

### 金融

- **蚂蚁数科**:**Agentar-Fin-R1** 在 FinEval1.0、FinanceIQ 等基准超 DeepSeek-R1 同尺寸模型;100+ 金融场景智能体
- **招商银行**:2025 年大模型日均 token 调用量达 **260 亿**,同比 +10.1×;对公业务的小企业尽调报告 **82% 工作量已由大模型替代**
- **中国平安**:PingAnGPT-Qwen3-32B 在 CNFinBench 综合排名第一(2026-03)
- **行业体量**:2025-2029 金融大模型市场 ¥41.23 亿 → ¥310.44 亿,CAGR 65.65%

### 医疗

- **微医(WeDoctor)**:4 项 AI 算法国家备案;CMB 测评 91.71 分位居榜首;天津 AI 总医院 2024 总收入近 ¥50 亿;**2025 H1 AI 医疗服务收入占总营收 90%+**
- **平安医疗大模型 3.5**:HealthBench Hard 全球评测 **57.27 分获全球最高**,AI 诊疗采纳率 85%,乳腺癌等重症与主任专家一致性 92.5%+
- **百川智能 Baichuan-M3**:王小川已宣布 ToC 严肃医疗产品 2026 年发布、**2027 年 IPO 计划**;¥30 亿现金在手

### 教育

- **作业帮**:题库 10 亿+,Question.AI 周活近 200 万
- **猿辅导**:旗下产品接入 DeepSeek + 自研猿力大模型
- **市场**:2024 中国 AI 大模型市场 ¥294.16 亿,2026 破 ¥700 亿;用户使用场景中"工作 + 学习"占 53.9% + 44.5%

### 客户服务

中国 95%+ 银行、保险、电商客服系统已部署 NLP 模型多年,**2026 是"客服 → Agent 化"爆发年**。微信对话开放平台、阿里云百炼 AppFlow 让任何公众号 10 分钟接入大模型客服。

---

## 17.5 监管框架:PM 必须掌握的"中国合规清单"

### 监管时间轴

| 年月 | 法规/规范 | PM 关键影响 |
|------|---------|-----------|
| 2022-03 | 算法推荐管理规定 → **算法备案** | 服务上线 10 个工作日内通过"算法备案系统"填报 |
| 2023-08-15 | **生成式人工智能服务管理暂行办法** | 具有舆论属性 / 社会动员能力的生成式 AI 必须经属地→国家网信办备案 |
| 2025-03 | 四部门《人工智能生成合成内容标识办法》+ GB/T 强制国标 | **2025-09-01 起强制实施** 显式(视觉/听觉可感知)+ 隐式(文件元数据)双标识 |
| 2025-12-27 | 拟人化互动服务管理办法征求意见稿 | — |
| 2026-01-01 | **新修订《网络安全法》施行** | 第二十条专设 AI 条款;处罚上限:网络运营者 ¥50 → ¥100 万,**关基设施 ¥50 → ¥1000 万(升 20 倍)** |
| 2026-04-10 | 五部门正式公布《人工智能拟人化互动服务管理暂行办法》 | 2026-07-15 施行 |
| **2026-07-15** | 拟人化办法生效 | **禁止向未成年人提供虚拟亲属/伴侣**;禁止过度迎合诱导情感依赖;要求提示老年人安全风险;要求情感操纵防控 |

### 大模型备案流程(PM 必背)

1. **属地网信办预审** → 提交:上线备案表、安全评估报告、模型服务协议、语料标注规则、拦截关键词列表、评估测试题
2. **属地材料审 + 技术测试**(拦截测试是重点)
3. **属地上报中央网信办**
4. **中央复审 + 技术评审**
5. **下发备案号**(不通过则需重新走流程)

**周期**:3-6 个月。**北京已压缩至 2 个月**,截至 2026-02-28 北京备案 216 款居全国第一;全国累计 748 款(截至 2025-12-31)。

### 训练数据合规

- 强制国标 **GB/T 45652-2025**《网络安全技术 生成式人工智能预训练和优化训练数据安全规范》对个人信息须匿名化或去标识化
- 网信办 + 公安开始把"训练数据来源 + 同意 + PIA"纳入执法样本 —— **人脸数据是高压线**
- 企业被要求把 AI 生命周期拆为五阶段,每阶段建立训练数据清单、授权关系、PIA、去标识化记录、供应链尽调

### 与西方对比

- 西方(特别是美国)**无强制备案**,加州 SB-53、欧盟 AI 法案是事后透明度要求
- 中国是**事前许可制 + 事后处罚联动**,PM 必须把"备案 / 内容标识 / 安全评估"做进 PRD,并预留 **2-6 个月监管周期**
- 西方 PM 几乎不考虑 ToG(政府客户),中国 PM 必须把 ToG(政企)当成头部业务,**默认产品形态是私有化部署一体机**

---

## 17.6 PM 招聘市场(2026 春招)

### 整体行情

- 2026 春招 AI 相关岗位**同比激增 14×**
- 字节跳动开放 5000+ 岗位、AI 占近半;阿里春招 AI 占比超 60%;百度 2026 届校招 AI 占比超 90%
- **AI PM 是非技术岗中唯一月薪能稳定到 ¥3 万元**的角色;**字节豆包业务平台 PM 月薪开到 ¥6 万元**
- 地区分布:北京占近 50%,上海 25%+,深圳、杭州为辅

### 必备技能(与西方对比)

| 维度 | 西方 AI PM | 中国 AI PM |
|------|-----------|----------|
| 技术理解 | 评测设计、tool use、prompt eval | **+ 算法备案/合规、内容标识 SOP、私有化部署架构** |
| 用户研究 | 用户访谈、A/B 测试 | **+ 政企客户客户成功(KA 销售陪跑)、地方政府关系** |
| 增长能力 | LTV/CAC、PLG | **+ 微信/抖音/小红书种草、邀请码灰产管理** |
| 节奏 | 双周 sprint | **春节迭代节奏**(CNY 大版本) |
| 工具栈 | Notion、Linear、Figma | **飞书、钉钉、企微 + 腾讯文档为主** |
| 薪资中位数 | $200K-$400K(SF) | ¥30K-¥60K/月(北京/上海/杭州,~$50K-$100K/年) |

### AI 六小虎人才流动

"AI 六小虎"(零一万物、百川、智谱、MiniMax、月之暗面、阶跃星辰)2025-2026 人才动荡明显:MiniMax 产品负责人张川离职;智谱产研中心是裁员重灾区;阶跃星辰 2026-01 完成 **B+ 轮 ¥50 亿融资**,由旷视印奇任董事长;**智谱 2026-01-08 港股 02513 上市,市值两个月破 ¥2500 亿**。

---

## 17.7 独特中国 PM 实践

### 春节迭代节奏

2025-02 DeepSeek-R1 全球出圈、2026-02 GLM-5、MiniMax M2.2、智谱港股上市、Yuanbao 红包 ¥10 亿、Coze 2.0、Seed 2.0 几乎全部集中在春节前后 2 周。

**PM 必须把"春节前提前 2 周冻结,春节后第一周复盘"写进版本计划** —— 这是西方完全没有的节律。

### 微信/钉钉/飞书/企微作为 Agent Surface

- **OpenClaw 主仓 GitHub Star 4 万+**,针对中国 IM 的 openclaw-china 子项目 1.9K star,阿里云、腾讯云、百度云均提供一键部署镜像
- 钉钉 AI 战略以 "AI 普惠 + 业务自动化" 为核心,"AI 魔法棒" 贯穿 17+ 产品 60+ 场景
- 飞书"智能伙伴"基于字节 Seed 模型;企微采开放策略接入 DeepSeek
- 钉钉、飞书 2026 集体转 CLI(命令行/工作流)以更好对接 Agent

### 政企客户 vs 西方 Enterprise

- 西方 PM 卖给企业的是 SaaS 订阅
- 中国卖给政企的是**软硬件一体的"大模型一体机"**(中国信通院已发《大模型一体机应用研究报告 2025》),项目千万级人民币起,竞争激烈但毛利薄
- 智谱已服务 12,000+ 全球企业客户;阿里平头哥真武 810E 已服务国家电网、中科院等 400+ 政企

### 私有化部署是默认形态

- 智谱 2023 上半年就推 GLM-130B 私有化,单价过千万元
- 零一万物 2025 转型 "小而美",专注私有化行业模型
- **影响 PM**:必须考虑昇腾/海光/英伟达三栈适配、客户 IT 部门审计闭环、版本升级周期由远程 OTA 变为现场升级

### 价格战 ROI 与西方完全不同

- 中国大模型 API 自 2024-05 经历**降价 90%-97%** 洗礼
- 但 2026 年起反向涨价(腾讯混元 +460%、智谱 +10%),**PM 必须重新设计企业合同保护性价格条款**
- **火山引擎 2025 营收翻倍至 ¥250 亿**、2026-03 占国内 MaaS 市场 49.2%;豆包日均 token 达 120 万亿(2026-04),是 2025-12 的近 2 倍 —— **用价格战换 token 调用规模是中国独有的飞轮玩法**

---

## 17.8 开源贡献与全球影响力

- **DeepSeek-R1 是 Hugging Face 上评分最高的模型**;中国模型 HF 总下载量已超过美国总和
- **Qwen 系列累计 HF 下载 10 亿+**,衍生模型超 20 万个,**全球第一个突破此规模的开源模型族**
- **全球开源 TOP10 中中国模型占 8 席**;MoE 已成绝对主流

### MCP / AGENTS.md 生态

- 阿里云**百炼**已支持 MCP 服务接入(官方 + 自定义双通道)
- 行业预测 2026-2027 是 MCP 标准化加速期,国内主流大模型完成适配
- Coze Studio、Coze Loop 已开源(字节);**OpenClaw 是中国版 Agent SDK 的 de facto 选择**

### 中国 AI Engineer 社群

- 量子位、机器之心、36Kr、PingWest、Geekpark、人人都是产品经理是中文 PM 主流深度内容平台
- 微信公众号长文("36 氪深度"、"晚点 LatePost"、"硅基立场"等)替代了西方的 Substack,**单篇 10K+ 中文长文是中国 PM 知识传播的核心载体**
- 智源社区、知乎专栏、CSDN AtomGit 是技术 PM 的核心工具圈

---

## 17.9 值得关注的中国创始人 / PM

| 人物 | 公司 | 关键定位 |
|------|------|---------|
| **梁文锋** | DeepSeek | 极少公开;2025-01 国务院专家座谈会发言;2025-04 与黄仁勋会谈芯片;2025-09 论文登《自然》封面;2026-01 入选《企业家》"年度企业家" |
| **杨植麟** | Moonshot Kimi | 2026 GTC 演讲《How We Scaled Kimi K2.5》;**2026 中关村论坛唯一科技代表主旨演讲** |
| **张鹏** | 智谱 | 2026-01-08 港股 02513 挂牌 |
| **王小川** | 百川智能 | 2026-01 宣布"造医生"路线,¥30 亿现金、2027 IPO;2026-03 警告 "Agent 安全问题 2026 集中爆发" |
| **姜大昕** | 阶跃星辰 | 多模态强项;2026 计划赴港 IPO |
| **印奇** | 阶跃星辰董事长 + 千里科技董事长 | 旷视联合创始人 2026-01 入主 |
| **谭待** | 字节火山引擎总裁 | 公开"今年收入翻倍"、定义豆包"普惠"价格策略 |
| **肖弘** | Manus(Butterfly Effect) | 拒绝 Wuhan 公开亮相、低调;张小珺播客深访谈 |
| **季逸超** | Manus 首席科学家 | 混沌学园圆桌深度阐述"Agent 通用能力" |
| **李开复** | 零一万物 | 2025-03 宣布"不再训万亿大模型"、转私有化部署 |

---

## 17.10 给西方读者的"维度差"总结

| 维度 | 西方 PM | 中国 PM |
|------|--------|--------|
| 算法主权 | 几乎无 | 必须考虑昇腾/海光/英伟达三栈 |
| 上线流程 | A/B → grad rollout | **算法备案 + 大模型备案 + 内容标识 + 安全评估** 缺一不可 |
| 用户增长 | App Store + ProductHunt | 微信公众号长文 + 邀请码灰产 + 春节红包大战 |
| 客户类型 | SaaS → SMB → Enterprise | C 端免费 + 政企私有化(数千万级合同) |
| 节奏 | 季度 OKR | **春节大版本 + 季度迭代** |
| 价格策略 | 月订阅、按 token 计费 | 经历 90%+ 降价后 2026 反向涨价 |
| 模型选择 | 单一闭源 / Anthropic + OpenAI 双供 | DeepSeek + Qwen + Doubao 多供,且经常**自训蒸馏小模型** |
| 法规风险 | EU AI Act / SB-53 / FTC | 网信办 / 工信部 / 公安部 / 广电总局 / 市监总局 **五部门联合** |
| 出海决策 | 默认全球 | 创业公司常需迁总部至新加坡/香港规避美方审查 |

---

## 资料源

### 官方与监管
- [国家互联网信息办公室 — 生成式人工智能服务管理暂行办法](https://www.cac.gov.cn/2023-07/13/c_1690898327029107.htm)
- [国家互联网信息办公室 — 人工智能生成合成内容标识办法](https://www.cac.gov.cn/2025-03/14/c_1743654685899683.htm)
- [国家互联网信息办公室 — 人工智能拟人化互动服务管理暂行办法](https://www.cac.gov.cn/2026-04/10/c_1777558395078289.htm)
- [新华网 — 五部门联合公布拟人化互动服务管理暂行办法](https://www.news.cn/law/20260413/d8f87a27897f40de8b3119102e2f83fc/c.html)
- [广州市算法备案、大模型备案及登记指引](https://gxj.gz.gov.cn/attachment/7/7759/7759505/10110285.docx)

### 模型与产品
- [DeepSeek V4 Preview Release(API Docs)](https://api-docs.deepseek.com/news/news260424)
- [InfoQ — DeepSeek V4 重磅开源、首次打通昇腾](https://www.infoq.cn/article/wUUPEzvNajcaVN0k7HPF)
- [VentureBeat — Qwen 3.5 397B-A17 beats trillion-parameter](https://venturebeat.com/technology/alibabas-qwen-3-5-397b-a17-beats-its-larger-trillion-parameter-model-at-a)
- [MarkTechPost — Moonshot Kimi K2.6 with Agent Swarm](https://www.marktechpost.com/2026/04/20/moonshot-ai-releases-kimi-k2-6-with-long-horizon-coding-agent-swarm-scaling-to-300-sub-agents-and-4000-coordinated-steps/)
- [SCMP — Zhipu AI launches GLM-5](https://www.scmp.com/tech/article/3343239/chinas-zhipu-ai-launches-new-major-model-glm-5)
- [TMTPost — ByteDance Volcano Engine major upgrades](https://en.tmtpost.com/post/7645516)
- [PRNewswire — Baidu unveils ERNIE 5.0](https://www.prnewswire.com/news-releases/baidu-unveils-ernie-5-0-and-a-series-of-ai-applications-at-baidu-world-2025--ramps-up-global-push-302614531.html)

### Agent 与平台
- [TechNode — Manus AI agent gains traction](https://technode.com/2025/03/07/chinas-ai-agent-manus-gains-traction-amid-growing-demand-for-autonomous-ai/)
- [MIT Technology Review — Manus has kick-started AI agent boom in China](https://www.technologyreview.com/2025/06/05/1117958/china-ai-agent-boom/)
- [CNBC — Meta acquires Butterfly Effect / Manus](https://www.cnbc.com/2025/12/30/meta-acquires-singapore-ai-agent-firm-manus-china-butterfly-effect-monicai.html)
- [TechNode — Tencent Yuanbao Groups beta](https://technode.com/2026/01/27/ai-gets-social-in-china-tencent-tests-yuanbao-groups-for-ai-powered-social-interaction/)
- [Sacra — Genspark revenue & funding](https://sacra.com/c/genspark/)

### 行业 / 商业
- [QbitAI 量子位 — 蚂蚁数科金融大模型](https://www.qbitai.com/2025/06/298955.html)
- [Vbdata 动脉网 — 2026 年最值得期待的医疗大模型](https://www.vbdata.cn/1519060462)
- [21 经济网 — 2025 招商银行大模型 token 调用 260 亿](https://www.21jingji.com/article/20250423/herald/2b46b142ae89d459a51f0ae2fc5b4420.html)
- [QuestMobile / 新浪科技 — 豆包 2.26 亿月活](https://finance.sina.com.cn/tech/discovery/2026-03-03/doc-inhpspmu4723719.shtml)
- [The Paper 澎湃 — Token 涨价潮警惕"词元垄断"](https://m.thepaper.cn/newsDetail_forward_33027088)
- [Wallstreetcn — 火山引擎 2025 营收 250 亿](https://wallstreetcn.com/articles/3748822)
- [TMTPost — 杨植麟 GTC 2026 演讲完整披露 Kimi 路线图](https://www.tmtpost.com/7919219.html)
- [QbitAI — 王小川:30 亿现金、2027 IPO](https://www.qbitai.com/2026/01/369332.html)
- [人民网 — 北京备案大模型 216 款居全国首位](http://bj.people.com.cn/n2/2026/0301/c14540-41511945.html)
- [White & Case — AI Watch: China regulatory tracker](https://www.whitecase.com/insight-our-thinking/ai-watch-global-regulatory-tracker-china)
- [Junzejun — 合规视角下的算法备案全攻略](https://www.junzejun.com/Publications/162744eb3fbb03-3.html)
- [人人都是产品经理 — 2026 年 AI PM 的下一个战场](https://www.woshipm.com/ai/6353165.html)
