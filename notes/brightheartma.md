- GitHub ID: 253285555
- Name: brightheartma
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-26
<!-- DAILY_CHECKIN_2026-07-26_START -->
# 2026-07-26

## 今日主题

围绕 Week 4 项目 **Parallax** 完成差异化定位、核心机制收敛、协议范围确认、仓库结构设计与 PRD 审查。

## 今天完成了什么

### 1\. 从“交易风控助手”转向差异化产品

在发现其他团队也在做交易风控助手后，我调研了 YO Risk Graph、Credora、Blockaid、Hypernative、LlamaRisk，以及 EEA 和 Galaxy 的 DeFi 风险框架。

最终将 Parallax 定位为：

> 把交易依赖、用户风险政策与 Moss 模拟结果结合起来，在签名前生成可验证的 Risk Receipt。

它不只是给协议打分，而是回答：**这笔交易虽然能够执行，但是否符合用户自己的风险边界？**

### 2\. 锁定三个核心创新点

-   **Risk Policy：**把“价格影响不超过 1%”“禁止无限授权”等偏好转成机器可执行规则。
    
-   **Transaction Risk Diff：**解释交易前后新增了哪些资产、授权、协议与风险暴露。
    
-   **Risk Receipt：**把区块、数据来源、依赖、模拟结果和规则结论整理成一份人和 Agent 都能读取的凭证。
    

同时加入 Dependency Resolver 与 Module Registry：系统先识别交易依赖，再动态选择适用的风险模块，避免给所有交易机械展示 Oracle、Liquidity、Approval 等无关检查。

### 3\. 确认 MVP 与协议范围

MVP 使用 Moss 已支持的：

-   Kuru；
    
-   PancakeSwap V2 / V3。
    

两条路径都要求使用真实 Quote、Action 与 Simulation，不使用协议 Mock。Oracle 风险不再强行进入首版；只有交易真实依赖预言机时才启用对应模块。

### 4\. 确认产品名与仓库基线

项目与产品正式统一命名为 **Parallax**。

仓库采用精简的 TypeScript monorepo：

-   `apps/web`：前端；
    
-   `apps/api`：后端与流程编排；
    
-   `packages/engine`：风险规则、Policy、Diff 和 Receipt；
    
-   `docs`：Product、Research 与 Ops 文档。
    

首版保留 README、package.json、pnpm workspace、TypeScript、Biome、环境变量示例和 gitignore；Issue、PR 模板、Contributing、Security 等等真正进入公开协作时再添加。

### 5\. 完成 PRD 审查

当前 PRD 主线完整，但仍需在开发前修正：

-   明确 Kuru 与 PancakeSwap 都是真实 P0；
    
-   Demo 交易方向优先统一为 `MON → USDC`；
    
-   增加 On-chain Evidence Collector，补充 Moss 尚未覆盖的只读链上数据；
    
-   锁定 Price Impact 公式；
    
-   明确 Analysis Account 与 Demo Address；
    
-   为每个 UI 字段标注数据来源与可用性；
    
-   避免 Risk Band 和 Data Confidence 变成新的黑箱评分；
    
-   收缩 History 与 Diff 的首版范围。
    

## 今日复盘

今天最大的收获是：真正的差异化不是“我们也做评级、模拟或 Agent API”，而是把这些能力连接成一条可执行的风险决策链。技术设计也必须忠于真实依赖：需要链上读取不代表需要自建合约；没有 Oracle 依赖就不应展示 Oracle 风险；数据缺失也不能自动视为安全。

## 下一步计划

1.  将 PRD 修订为 v0.2；
    
2.  建立字段—数据来源—可用性矩阵；
    
3.  明确 Price Impact 计算方法；
    
4.  确认 Kuru 与 PancakeSwap 的共同 Demo Pair；
    
5.  完成 Moss Query + RPC 只读数据的最小验证；
    
6.  初始化 Parallax GitHub 仓库。
<!-- DAILY_CHECKIN_2026-07-26_END -->

# 2026-07-25
<!-- DAILY_CHECKIN_2026-07-25_START -->
# 2026-07-25

## 今日主题

围绕 Week 4 的交易前风险分析工具，按照“技术服务需求”的原则评审技术方案，并结合五人团队重新划分前端、后端、风险引擎与协议研究职责。

## 今天完成了什么

### 1\. 明确产品定位

项目不是 DeFi 平台，也不是简单包装 Moss，而是一款交易前风险分析与决策辅助工具：

```
输入交易
→ Preliminary Assessment
→ Moss Simulation
→ Final Assessment
→ 用户自行决定是否继续
```

Moss 负责底层交易构建、模拟和验证；团队负责风险规则、数据整合和产品体验。

### 2\. 收敛技术栈

最终建议保留：

-   pnpm workspace
    
-   TypeScript + Node 22
    
-   Vite + React + Recharts
    
-   Hono + zod + NDJSON 流
    
-   `contracts`、`risk-engine`、`moss-bridge`
    
-   Vitest、fixture 和 Biome
    

暂不引入 Next.js、数据库、LLM 裁决和复杂任务系统。钱包连接作为非阻塞增强，失败时仍可使用手动地址或 Demo 地址。

五人协作使 workspace 有了真实价值：它用于职责隔离、契约共享和并行开发，而不是为了模仿大型项目结构。

### 3\. 修正团队职责

-   **Kai：**产品、流程、README、Demo、Pitch 与协调。
    
-   **Jie：**风险指标、规则、阈值、Risk Engine 和规则测试。
    
-   **Clare：**Backend、Moss Integration、外部数据、API Contract、流式接口、缓存与部署。
    
-   **Antony：**Wallet、UI、图表和前端集成。
    
-   **acoust：**协议研究、数据来源、历史事件、风险案例和研究证据。
    

其中最重要的修正是：数据获取、归一化、API DTO 和传输归 Backend；Jie 负责风险字段含义和计算规则，而不是后端数据接口。

### 4\. 发现技术方案中的关键问题

-   `bigint` 不能直接通过 JSON，应在网络边界转换为字符串；
    
-   无路由结果应使用判别联合，不能让部分字段被迫存在；
    
-   RPC URL 可能包含密钥，不能返回前端或写入快照；
    
-   固定区块只能保证链上读取一致，外部市场 API 需要单独记录；
    
-   安全边界搜索必须先验证单调性，再进行二分；
    
-   缺失数据不能默认视为低风险，Mock 也不能进入真实评级。
    

### 5\. 调整开发顺序

不能把 Moss Integration 放到最后。它是最高技术风险，应先完成最小真实 Spike：

```
Kuru MON → USDC
→ Quote
→ Moss Simulation
→ 原始 JSON
→ 冻结 contracts v0.1
→ 前后端并行开发
```

前端使用通过同一套 schema 验证的 fixture 开发，避免等待后端完成；第一次完整集成也不能拖到最后一天。

## 今日复盘

架构设计不能只考虑代码是否优雅，还要考虑真实团队如何交付。单人项目适合单 package，但五人并行开发需要有限且明确的物理边界。最关键的不是拆多少包，而是谁负责字段含义、谁实现接口、谁有最终修改权。

## 下一步计划

1.  确认 Clare 是否正式负责 Moss Integration；
    
2.  决定是否输出 Overall Risk 及其机械规则；
    
3.  确定第二协议做完整模拟还是事实对照面板；
    
4.  确认 Preliminary 与 Simulation 保持两次点击；
    
5.  完成 Kuru + Moss 最小真实 Spike；
    
6.  根据真实输出冻结 API Contract 与首份 fixture。
<!-- DAILY_CHECKIN_2026-07-25_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
# 2026-07-24

## 今日学习主题

围绕 Moss Collaboration Demo Studio，重新梳理 Pendle Adapter 的展示重点，完成主网演示预检、用户测试流程设计和个人贡献说明。

## 今天完成了什么

### 1\. 调整项目叙事重点

发现原 Pitch、Landing Page 和 Deck 更像是在介绍 Moss，而没有突出我的实际贡献。因此将叙事调整为：

> 我为 Moss 开发了 Pendle Adapter，让 Agent 能够发现真实市场、获取 PT 报价、构建交易，并在签名前验证执行结果。

重点展示了我实现的 `pendle.markets`、`pendle.quote`、`pendle.swap`，以及市场链上验证、风险标签、交易树和 Receipt 解析。

### 2\. 完成主网演示预检

解决 Monad RPC 的 TLS 证书链问题后，重新跑通：

```
discover → load → markets → quote → swap → simulate → Receipt
```

当前可验证 4 个 Pendle 市场，真实报价和模拟均成功，且整个过程没有签名或广播交易。

同时发现 APY 属于动态数据：此前记录的 644% 无法确认与当前 USDat 市场属于同一快照，当前市场约为 8.5%。因此展示时应写成“Pendle API 提供的动态综合年化收益率参考值”，不能描述成固定利率或收益保证。

### 3\. 明确当前项目进度

项目定义、研究、Adapter 开发、MCP 演示和介绍材料已经基本完成。当前进入“真实用户验证与最终提交整理”阶段，录屏暂时停止，不继续扩展 YT、LP、自动签名等功能。

### 4\. 准备用户测试

完成了统一的用户测试流程，包括：

-   让测试者用自己的话描述固定收益需求；
    
-   阅读风险标签、报价和最低收到量；
    
-   判断模拟后资金是否已经移动；
    
-   理解余额不足时系统为何停止；
    
-   记录测试者原话、犹豫和误解。
    

由于测试者目前不在场，本次测试如实延期，没有用自测或 AI 生成内容代替真实反馈。

### 5\. 完成个人贡献说明

按 Solo 团队情况整理了 Dev、Research、Ops 三类贡献，并区分：

-   Week 2 已完成的 Pendle Adapter；
    
-   Week 3 新增的 MCP Demo、研究、风险说明和测试材料；
    
-   仍未完成的真实用户测试与最终录屏。
    

## 今日复盘

今天最大的收获是：项目展示不能只证明底层框架有多强，而要让别人清楚看见“我具体补上了什么能力”。同时，动态 APY、模拟成功和真实执行必须严格区分，不能为了展示效果牺牲准确性。

## 下一步计划

-   完善 Hackathon Readiness Card；
    
-   测试者到场后完成 3 次真实用户测试；
    
-   根据反馈至少记录一项产品修改；
    
-   补齐个人课程要求与最终提交包；
    
-   继续确认模拟是否满足“链上交互”，或是否必须加入用户钱包广播。
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# 2026-07-23

## 今日主题

围绕 Week 3 的 **Pendle PT Yield Assistant**，梳理 Moss 的 Agent 调用方式，并完成 Solo 团队协作、Dev 可行性验证、Ops 用户测试准备和项目研究简报。

## 今天完成了什么

### 1\. 理清 Moss 与 AI Agent 的关系

确认调用方向不是 Moss 调用 AI，而是 AI Agent 通过 MCP 调用 Moss：

```
用户自然语言
→ AI Agent
→ Moss MCP
→ Pendle Adapter
→ 构建并模拟未签名交易
→ Receipt / Warnings
→ AI 向用户解释结果
```

Moss 本身不需要 OpenAI 或 Claude API Key，也不保管私钥、不签名、不广播。只有自行开发 Agent 后端时，才需要模型供应商的 API Key。

### 2\. 完成 Solo Team Formation Card

目前以 Solo 形式推进，由本人同时承担：

-   **Dev**：Adapter、MCP 和端到端 Demo；
    
-   **Research**：场景、协议风险和 APY 来源；
    
-   **Ops**：用户测试、Pitch、录屏和提交材料；
    
-   **Frontend**：Week 3 暂不配置，先使用 CLI/MCP Demo。
    

同时补充了任务看板、同步时间、决策方式和安全规则。所有敏感凭据禁止进入 Git、Notion 和群聊；出现模拟 Warning 时立即停止。

### 3\. 从 Dev 角色验证项目

本周核心功能确定为：

```
markets
→ quote
→ 用户确认
→ swap
→ simulate
→ Receipt
```

真实部分复用现有 Moss、Pendle Adapter、Monad RPC 和主网市场状态；自然语言理解、钱包签名、交易广播及完整网站暂不实现或使用 Mock。

### 4\. 完成 Ops 用户测试准备

根据 Session Log 整理完成：

-   一句话项目介绍；
    
-   100–200 字项目简介；
    
-   测试邀请文案；
    
-   统一测试任务；
    
-   5 个反馈问题；
    
-   Landing Page 草稿说明。
    

目前已有 3 名潜在测试者，但测试尚未真正执行，因此没有填写虚构的用户评价。

### 5\. 完成项目研究简报

研究简报整理了 4 条问题证据：

-   Web3 用户存在 Blind Signing 风险；
    
-   Pendle 的 PT、期限和 APY 具有理解门槛；
    
-   MetaMask、Rabby 已通过交易模拟改善签名前理解；
    
-   Pendle 在 Monad 上存在真实市场和使用数据。
    

对比方案包括 Pendle App 与钱包交易模拟。项目定位不是替代它们，而是验证 Agent 能否完成市场发现、报价、模拟，并提供协议级 Receipt 和风险提示。

核心待验证问题是：

> 用户看到报价、风险标签和 Receipt 后，能否比普通钱包确认页更准确地解释交易，并做出继续或停止的决定？

## 当前边界

-   [Pendle Adapter PR #109](https://github.com/nishuzumi/moss/pull/109) 在今天记录的核对结果中为 Open、非 Draft、尚未合并；
    
-   3 名用户尚未完成真实测试；
    
-   Demo 录屏尚未完成；
    
-   Landing Page 提交前还需开放分享并退出登录验证；
    
-   相关本地文档已经完成，但尚未提交 Git。
    

## AI 帮助与人工校正

AI 帮助提取 Session Log、整理课程任务、生成研究与测试材料，并维护 Notion 课程索引。

人工负责确定 Solo 状态、项目范围、安全边界和真实/Mock 划分，同时要求未发生的测试、反馈和产品效果不得写成已完成事实。

## 今日复盘

今天最大的进展不是增加更多功能，而是把项目从“有代码的 Demo”整理成一套可以被检查的 Week 3 交付：

```
技术可行性
+ 协作规则
+ 用户问题证据
+ 测试准备
+ 真实与 Mock 边界
```

## 下一步计划

-   完成 3 名用户的真实测试并记录原话；
    
-   根据测试结果修改项目介绍和 Demo 表达；
    
-   开放并验证 Landing Page 分享权限；
    
-   录制完整 MCP 演示；
    
-   人工复核研究来源和动态数据；
    
-   整理并提交本地 Week 3 文档。
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
# 2026-07-22

## 今日主题

复盘 Co-learning 中关于随机数、代理合约初始化、升级存储布局和预言机时效性的四道安全题，并将老师的讲解与 AI 辅助解释进行对照。同时了解 Week 3 Mini Demo、黑客松和组队要求。

## 今天学了什么

### 1\. 链上随机数

不能使用 `block.timestamp` 生成抽奖随机数。时间戳可以被预测，也可能在一定范围内受到区块生产者影响。

更安全的方案是：

-   使用 Chainlink VRF 等可验证随机数；
    
-   使用 Commit–Reveal，让参与者先提交哈希、再公开秘密；
    
-   为不 Reveal 的情况设置截止时间、押金惩罚或重抽规则。
    

### 2\. 代理合约初始化

代理部署后如果没有立即初始化，攻击者可能抢先调用：

```
initialize(攻击者地址);
```

并把自己设置为管理员。

正确做法是部署代理时通过初始化 calldata 在同一笔交易中完成 `initialize(owner)`，同时在实现合约的构造函数中调用 `_disableInitializers()`。

前端隐藏按钮、修改函数名都无法阻止攻击者直接调用链上合约。

### 3\. 升级合约的存储布局

代理合约的状态保存在固定 Storage Slot 中。升级实现时，如果删除、重排或在旧变量前插入新变量，原来的管理员、余额等数据会被新代码错误解释。

升级时应：

-   保留旧变量的顺序和类型；
    
-   新变量只追加在兼容位置；
    
-   使用 OpenZeppelin Upgrades 工具检查 Storage Layout；
    
-   升级前在测试环境复现真实状态。
    

### 4\. 预言机价格时效性

能读到价格不代表价格仍然有效。消费合约必须检查：

```
price > 0
updatedAt > 0
block.timestamp - updatedAt <= MAX_PRICE_AGE
```

数据过期时，应暂停新借款、开仓、铸造和提取抵押物等高风险操作；还款、补充抵押物等降低风险的操作可以根据协议规则继续开放。

清算不能简单地“一律继续”或“一律暂停”，应通过备用价格源或 Oracle Emergency Mode 处理。

## 老师讲法中值得借鉴的地方

老师采用“先给结论，再解释风险，最后说明正确方案”的方式，对小白比较友好。

我补充校正了两点：

-   代理模式不能简单理解为“用 constructor 设置管理员”，代理状态仍需通过 `initialize()` 写入；
    
-   预言机过期后不应机械暂停所有功能，而应区分增加风险和降低风险的操作。
    

## 黑客松信息

根据会议分享：

-   Week 3 是 Mini Demo 和组队准备阶段；
    
-   后续黑客松奖金池共 3,000 USDT，两个赛道各 1,500 USDT；
    
-   正式提交需要完整 Demo、演示视频、产品文档和 Pitch；
    
-   团队最好覆盖前端、合约或后端、产品运营及演示角色；
    
-   具体赛道与钱包登记要求以官方群最终公告为准。
    

## AI 帮助与人工校正

AI 帮助我用代码和生活类比拆解四类安全问题，并整理老师讲解与此前答案的差异。

人工负责确定解释结构、核对技术边界，并纠正代理初始化和预言机降级策略中过度简化的表达。

## 今日复盘

今天四个问题的共同点是：不能只检查正常路径，还要考虑输入能否被操纵、权限能否被抢占、旧状态会不会被错误解释，以及外部数据失效时系统如何安全降级。

## 下一步计划

-   学习 Chainlink VRF 和 Commit–Reveal；
    
-   使用 OpenZeppelin 工具检查 Storage Layout；
    
-   编写带 `updatedAt` 检查的预言机读取示例；
    
-   根据正式赛道确定 Mini Demo，并完成团队分工。
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
# 2026-07-21

## **今日主题**

围绕 **PR** [**#109**](https://github.com/brightheartma/moss/issues/109) **Pendle Adapter 的 audit 后续收尾**：把作者对两个延后问题（ABI 交叉核对、dust 错误）的回复逐一落地，同时诚实处理一个新发现的 live 测试 flakiness——能确定性根治的交作者定，不硬修、不臆造证据。

## **今天完成了什么**

-   **救回 sell-PT live 模拟**（`ae48fbb`）：用自筹持有者把作者砍掉的卖出方向 live 测试重新覆盖，买/卖两向都在主网跑通。
    
-   **APY 语义文档化**（`ae6e67d`）：`aggregatedApy` 明确标为小数，消除歧义。
    
-   **ABI 交叉核对 · 块 1**（`2733531`）：MarketFactory explorer 交叉核对，online 4/4 全绿。
    
-   **ABI 交叉核对 · 块 2**（`86c03d5`）：建 issue [#118](https://github.com/brightheartma/moss/issues/118) 记录 selector-proxy（Diamond）无法核对的缺口，`abis.json` 加 `selectorProxies` 引用它。
    
-   **dust 诊断**：证明报价/构建都不 revert，只有执行才 `MarketZeroNetLPFee`；带对比铁证发 follow-up，给作者 a/b/c 选方向。
    
-   **推送 + 归档**：块 1/2 推到 fork，进度写进 Notion 阶段记录。
    

## **重点（三条关键收获）**

1.  **selector-proxy 挡住 explorer 核对 → 记录成 #118，而不是沉默留坑**。
    
2.  **dust 正确定性 = 报价正常、只有执行 revert**，附证据请作者定，不擅自加拦截。
    
3.  **能根治但要动 core 的问题（dust 根治 + flakiness 根治都指向** `SimulatorOptions` **state override）诚实上报、交回 owner**，不硬塞进本 PR。
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->
# 2026-07-20

## 今日主题

围绕两场共学分享：一场从 AI 产品落地谈到 Agent 经济、儿童认知幻觉、销售 AI 与产品转型；另一场覆盖虚拟货币/区块链落地推广，以及求职简历的素材库与 JD 匹配方法。

## 今天完成了什么

-   整理并提炼两场分享中的**观点**与**解决方案**，去掉发言人标签，按议题归档；
    
-   抽出可带走的行动结论，用于对照当前 Moss / Web3 实习中的产品、安全与求职准备。
    

## 核心共识

真正的价值不在于「用上了 AI」或「上了链」，而在于把能力嵌进真实业务场景、解决具体问题；同时要警惕效率幻觉、过度依赖，以及技术成本与产品落地之间的落差。

* * *

## 第一场：从产品到 Agent 经济

### 从技术到产品，再到「Agent 经济」

**观点**

-   产品实现与业务场景结合比「概念跑通」更重要；价值在解决实际问题，不在技术演示。
    
-   AI 正在改变经济协作方式，提出「Agent 经济」：代理会成为新的生产力单元与协作节点。
    
-   技术转向产品后更认同：团队协作 + 深懂业务 + 落地实践，缺一不可。
    

**解决方案**

-   探索新业务时，先锚定真实痛点与可交付服务，再决定是否上 AI。
    
-   以用户体验为本：技术趋势服务于人，而不是堆功能。
    

### 从产品到代理经济的构建

**观点**

-   核心不是默认上 Agent，而是判断项目是否真正需要引入 AI 代理。
    
-   Agent 对效率的提升可能是非线性的：用对了跃升，用错了反而放大风险。
    
-   不熟悉 Agent 操作时，存在误用、失控、决策质量下降等风险。
    

**解决方案**

-   引入前做必要性判断：问题是否适合代理化、收益是否覆盖风险与学习成本。
    
-   活动节奏上衔接下一段专栏，同步确认出行安排。
    

### AI 原住民儿童的「能力幻觉」

**观点**

-   长期深度接触 AI 的孩子，可能误以为「会用 AI = 自己理解力变强」，形成错误自我认知。
    
-   这会影响学习与判断能力的真实发展；教育不能只追工具，更要守住认知与思维训练。
    

**解决方案**

-   在关键学习环节回归传统训练（阅读、推导、动手、独立表达），避免幻觉固化。
    
-   目标不是拒绝科技，而是让儿童正确认知、健康使用 AI：工具辅助，而非替代思考。
    

### 销售 AI：能力上限、成本与知识库瓶颈

**观点**

-   关注 1–2 年内，AI 能否达到优秀销售约 80% 的能力水平。
    
-   已有大量标注、分类的客户聊天记录，具备训练销售 Agent 的数据基础。
    
-   当前模型在对话自然性、转化效率上仍不够，成本与性能压力并存。
    
-   多维知识库（产品属性、规则、健康信息等）建设难度与成本高，是落地关键障碍。
    

**解决方案**

-   用高质量、已结构化的历史沟通数据做定向训练，缩短「能聊」到「能卖」的距离。
    
-   单独投入产品知识、规则与合规信息的体系化整理，不能只靠通用大模型。
    

### 产品转型：需求沟通与打磨之间的定位

**观点**

-   产品开发要在「对外听清用户需求」与「对内持续打磨体验」之间找平衡。
    
-   交互设计与原型是把模糊需求变成可验证方案的关键环节。
    
-   工作重心从偏技术转向偏产品后，关心不直接写代码时如何构建核心竞争力。
    

**解决方案**

-   在产品流程中明确价值锚点：需求澄清、交互/原型、跨团队对齐、落地验证。
    
-   用设计与产品判断力替代「会写代码」，形成不可替代能力。
    

* * *

## 第二场：虚拟货币落地与简历竞争力

### 虚拟货币与区块链的落地应用

**观点**

-   虚拟货币具备成为通用货币的潜力，在面临金融危机的国家已有较广泛使用。
    
-   普通人在处理法定货币价值转换时，可能转向使用虚拟货币。
    
-   区块链产品的设计与推广，前提是理解各国法律与市场环境；不做市场调查容易踩坑。
    
-   区块链在金融侧的具体价值包括：无痕法币价值流通、高并发交易数据处理，以及智能合约支撑的公平投机机制。
    
-   推广策略存在明显地域差异；在不同国家做营销，可能碰到法律与市场限制。
    

**解决方案**

-   先做各国法律与市场环境调研，再据此做产品设计与推广方案。
    
-   按地区差异化营销：借助 KOL 与社群工具拉流量，同时预判并规避当地合规与市场限制。
    
-   持续在区块链领域积累实战经验，并对后续挑战做前瞻准备。
    

### 简历制作：素材库 + JD 精准匹配

**观点**

-   简历成功的关键不只是「写得好」，而是有个人知识库与真实经历素材库可调用。
    
-   有效简历必须基于真实经历，又能按不同岗位灵活调整；模板、素材、结构三者缺一不可。
    
-   参与黑客松等实战项目，能显著提升简历竞争力。
    
-   个性化、针对性与持续优化，决定面试与求职成功率。
    

**解决方案**

-   按目标岗位 JD 做精准定位：挑选、改写素材，确保真实且匹配需求。
    
-   选用合适模板，持续更新个人素材库；可用编码或云代码工具整理信息，提升专业性与条理性。
    
-   通过黑客松等项目不断实践，把经历写进简历并反复优化。
    

* * *

## 可带走的行动结论

| 议题 | 观点 | 可执行方向 |
| --- | --- | --- |
| 做不做 Agent | 不是默认选项，效率可能非线性 | 先做必要性与风险评估 |
| 产品价值 | 场景落地 > 概念实现 | 以真实问题驱动功能 |
| 儿童与 AI | 存在「能力幻觉」风险 | 保留传统思维训练，工具定位要清晰 |
| 销售 AI | 数据有、自然度与成本卡脖子 | 结构化语料 + 专项知识库建设 |
| 个人转型 | 技术→产品后需新竞争力 | 强化需求、交互、原型与业务理解 |
| 虚拟货币 | 危机国家已有通用货币潜力 | 关注法币兑换场景中的真实需求 |
| 区块链推广 | 法律与市场决定产品形态 | 先调研合规与市场，再设计与投放 |
| 地域营销 | KOL/社群有效，但有国别限制 | 差异化策略 + 预判法律风险 |
| 简历竞争力 | 真实素材 + JD 匹配 | 建素材库，按岗位改写 |
| 简历提升 | 项目经历比空泛描述更有用 | 参与黑客松等实战并持续迭代 |

## 今天学到的关键内容

### 1\. Agent 不是默认升级，而是有风险的效率放大器

是否引入 Agent，要先问问题是否适合代理化。效率提升可能非线性：用对了跃升，用错了放大误用与失控。这与 Moss 里「先 simulate、再签名」的安全门思路一致——能力封装之后，仍然要有必要性判断与证据边界。

### 2\. API / 大模型可信，不代表输出可直接当事实执行

销售 AI、儿童「能力幻觉」、Pendle API Candidate 是同一类问题：来源官方或工具很强，不等于结果可直接执行。真正可信的路径是「提名 + 验证 + 可审查证据」。

### 3\. 区块链落地先吃合规与市场，再谈技术叙事

产品设计与推广必须理解各国法律与市场环境；KOL 与社群能拉流量，但国别限制会直接卡死营销。技术能力之外，合规调研是硬门槛。

### 4\. 简历竞争力来自可复用的真实素材，不是一次性美化

建个人知识库与经历素材库，再按 JD 精准改写；黑客松等实战经历比空泛描述更能提升成功率。这与共学打卡、Proof of Work 的逻辑一致：可验证经历才是资产。

## 个人思考

-   「场景落地 > 概念实现」正好对照当前 Pendle Adapter：先把 ABI、市场验证、TLS Evidence 做成可审查信任链，再谈公开 API——不是慢，而是避免把未确认选择固化成接口。
    
-   「能力幻觉」提醒我：AI 辅助写 Review、跑测试时，人工仍要负责范围判断、风险定性与最终结论；工具变强，不等于自己判断力自动变强。
    
-   简历分享直接可用：把近期 Moss PR Review、Pendle Adapter、monad-clicker 等按 JD 拆成素材库条目，而不是只维护一份静态简历。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
# 2026-07-19

## **今日主题**

围绕 [Moss Issue #11：Pendle yield trading](https://github.com/nishuzumi/moss/issues/11)，完成前四个开发阶段：项目骨架、官方地址与 ABI 溯源、已知市场链上验证，以及官方 API 候选市场发现。当前工作停在内部安全基础设施阶段，尚未完成 RouterStatic 报价、Swap、Receipt 和公开 Protocol API。

## **今天完成了什么**

### **1\. 确认 Issue 与开发范围**

在开始编码前，先检查了 Moss 当前的 Adapter Issues、已有 PR 和认领情况，最终选择 Pendle Issue #11。

截至 2026-07-19 19:12：

-   Issue #11 仍为 **Open**；
    
-   当前没有 Assignee；
    
-   已发布[认领及范围提案](https://github.com/nishuzumi/moss/issues/11#issuecomment-5011156140)；
    
-   Maintainer 尚未回复；
    
-   尚未创建 Pendle PR。
    

当前提出的 v1 范围包括：

-   仅支持 Monad Mainnet，运行时强制验证 Chain ID `143`；
    
-   支持 underlying 与 PT 的双向交易；
    
-   使用 RouterStatic 报价、Router 直接执行；
    
-   官方 Pendle API 只能提供候选市场；
    
-   候选市场必须经过 Factory、Market、SY、Token 等链上验证；
    
-   APY 必须标记为 API 派生数据，不能描述成链上收益保证；
    
-   暂不支持 YT、LP、Aggregator、Limit Order、Hosted SDK calldata 和原生 Token 路由。
    

## **Stage 1：建立分支与 Package 骨架**

创建标准功能分支：

```
 feat/pendle-adapter
```

检查后发现本地 `main` 一度落后，因此先修正分支基线。最终本地 `main`、merge-base 和当时的官方 `main` 均为：

```
 efa0a72a368cdf22ab827833cee899955366b0ac
```

随后建立最小 Pendle Package：

```
 packages/protocols/pendle
```

主要文件职责：

-   `package.json`：Package 身份、依赖及验证命令；
    
-   `tsconfig.json`：Monorepo TypeScript 配置；
    
-   `src/index.ts`：保持最小入口，不提前公开未确认的 Query 或 Capability；
    
-   `README.md`：记录阶段边界和 ABI provenance 规则；
    
-   `pnpm-lock.yaml`：Workspace importer。
    

这个阶段只建立项目边界，没有提前实现 `swap`、市场发现或最终公开 API。

## **Stage 2：官方地址与 ABI Provenance**

固定部署信息来自 Pendle 官方仓库的不可变 Commit：

-   Repository：`pendle-finance/pendle-core-v2-public`
    
-   Commit：`6cd4773218e57dbda8925d10dfb672a0f594a9db`
    
-   文件：[deployments/143-core.json](https://github.com/pendle-finance/pendle-core-v2-public/blob/6cd4773218e57dbda8925d10dfb672a0f594a9db/deployments/143-core.json)
    

确认的 Monad 部署包括：

-   V6 Market Factory：`0xA3cb62a49b66eB2536cf6F3C7AC82293784888A3`
    
-   Router V4：`0x888888888889758F76e7103c6CbF23ABbF58F946`
    
-   RouterStatic：`0x6813d43782395A1F2AAb42f39aeEDE03ac655e09`
    

ABI 选用 `@pendle/core-v2@6.7.1`。没有使用当时刚发布不久的 `6.8.0` 和 `6.8.1`，以满足七天版本观察期。

建立的数据流为：

```
 npm 官方 Tarball
 → 完整上游 Artifact
 → 确定性离线生成器
 → TypeScript ABI
 → Byte-for-byte Drift Test
```

同时记录了：

-   Package 版本及发布时间；
    
-   Tarball SHA-256；
    
-   Artifact 与输出 ABI 的映射；
    
-   固定部署来源；
    
-   ABI 更新和生成流程。
    

Stage 1 与 Stage 2 已形成一个有完整意义的本地 Commit：

```
 1e89457 chore: establish Pendle adapter provenance
```

该 Commit 目前只存在于本地功能分支，尚未 Push，也没有创建 PR。

## **Stage 3：实现市场链上验证器**

这一阶段实现内部 `market-verifier`，接收不可信的市场候选和预期 underlying，只有完成全部链上验证后，才返回只读、冻结的 `VerifiedMarket`。

验证流程为：

```
 不可信 Candidate
 → 验证运行时 Chain ID 143
 → 检查 Market Bytecode
 → Factory.isValidMarket
 → 校验 Market.factory()
 → 读取 SY / PT / YT
 → 检查动态合约 Bytecode
 → 检查市场是否过期
 → 验证 SY tokensIn / tokensOut
 → 验证预期 Underlying 双向支持
 → 读取 Underlying / PT Decimals
 → 生成 VerifiedMarket
```

负向测试覆盖：

-   错误 Chain；
    
-   Market 或动态 Token 无 Bytecode；
    
-   Factory 未注册或不匹配；
    
-   Token 地址为零或重复；
    
-   Market 已过期；
    
-   Underlying 不支持 Token In 或 Token Out；
    
-   Decimals 非法；
    
-   RPC 失败；
    
-   ABI 返回结构异常。
    

真实 Monad 验证了三个市场：

-   AUSD：到期日 `2026-10-08`，underlying/PT decimals 为 `6/6`；
    
-   earnAUSD：到期日 `2026-10-08`，保留了 `3 tokensIn / 2 tokensOut` 的非对称支持结构；
    
-   USDat：到期日 `2026-08-27`，underlying/PT decimals 为 `6/6`。
    

这些地址仅作为 Live Test Fixture，不会成为生产代码中的静态市场 fallback。

## **严格 TLS 证据修复**

第一次运行 Live Tests 时发现，当前 Shell 环境继承了不安全的 TLS Override。虽然链上断言能够通过，但关闭证书验证后的结果不能作为可信 Live Evidence。

进一步检查确认：

-   Moss 仓库及 Pendle 代码中不存在关闭 TLS 的实现；
    
-   问题来自用户环境配置；
    
-   没有修改用户全局 Shell 或 npm 配置；
    
-   没有使用 `curl -k`、关闭 Strict SSL 等绕过方式。
    

随后显式恢复严格 TLS，并使用系统可信 CA 重新运行测试。最终：

-   无 TLS Disable Warning；
    
-   固定部署地址验证通过；
    
-   AUSD、earnAUSD、USDat 链上验证全部通过；
    
-   Offline Tests：24 passed，4 live skipped；
    
-   Strict-TLS Live Tests：28 passed；
    
-   Lint、Build、Typecheck、`git diff --check` 全部通过。
    

这次修复让我更加明确：业务断言通过并不等于验证证据可信，传输层安全也是 Live Evidence 的组成部分。

## **Stage 4：官方 API Candidate Discovery**

实现内部市场发现 Pipeline，固定使用 Pendle 官方 Monad API：

[Pendle Monad Markets API](https://api-v2.pendle.finance/core/v2/markets/all?chainId=143)

核心信任边界是：

```
 Pendle API
 → 只能提名 Candidate
 → Market Verifier 执行完整链上验证
 → 通过后才能进入 Verified Markets
```

API 返回的 Market、Underlying、标签和 APY 都不能直接当作链上事实。

Discovery Pipeline 目前包含：

-   Chain ID 固定为 `143`；
    
-   Page Size 50；
    
-   最多请求 5 页；
    
-   最多处理 200 个候选；
    
-   单页最大 1 MB；
    
-   请求超时 10 秒；
    
-   验证 HTTP 状态及 JSON Content-Type；
    
-   严格解析分页和字段类型；
    
-   检查 Total 是否稳定、Skip 是否推进；
    
-   防止重复页面和无限分页；
    
-   相同 Candidate 去重；
    
-   同一 Market 出现冲突 Metadata 时拒绝；
    
-   记录结构化 Rejection，不静默丢弃候选；
    
-   不提供任何静态市场 fallback。
    

失败策略为：

-   Transport、HTTP、JSON、Schema 或 Pagination 错误：整个 Discovery 失败；
    
-   单个候选畸形或链上验证失败：进入结构化 `rejections`；
    
-   所有候选都被拒绝：返回 `unavailable`；
    
-   未知的 Verifier/RPC 基础设施错误：整个调用失败。
    

APY 与链上事实保持分离，并记录：

-   Provider；
    
-   Source URL；
    
-   Fetch Time；
    
-   `inferred` 类型。
    

因此，它只能表示 API 在特定时间观察到的派生数据，不能表示协议承诺的收益。

## **Live Discovery Evidence**

在严格 TLS 环境下运行官方 API Discovery：

```
 API Candidates：3
 Verified：3
 Rejected：0
```

当时观察到的数据为：

| 市场 | API 派生 APY | 到期日 |
| --- | --- | --- |
| USDat | 9.35623% | 2026-08-27 |
| earnAUSD | 8.33632% | 2026-10-08 |
| AUSD | 7.60965% | 2026-10-08 |

这些 APY 会随时间变化，只是 2026-07-19 的 API Observation，不会被写成固定常量。

Stage 4 共补充：

-   45 个 Discovery 安全及行为测试；
    
-   18 个 Market Verifier 单元测试；
    
-   严格 TLS Live Discovery；
    
-   完整 Lint、Build、Typecheck、Offline Tests 和 Diff Check。
    

## **今天学到的关键内容**

### **1\. API 来源可信，不代表 API 返回值可以直接执行**

即使数据来自 Pendle 官方 API，也只能把它当作 Candidate。Market、Underlying 和 Token 关系最终必须由官方 Factory 和合约状态验证。

真正的信任关系是：

```
 API 提名
 ≠ 链上事实
 ​
 API 提名
 + Factory / Market / SY / Token 验证
 = 可接受的 VerifiedMarket
```

### **2\. ABI Provenance 也是安全设计的一部分**

只把 ABI 复制进项目无法回答：

-   ABI 来自哪个版本？
    
-   上游 Artifact 是否完整？
    
-   是否被人工修改？
    
-   重新生成是否得到相同结果？
    
-   当前部署是否真的对应这些接口？
    

通过固定上游 Commit、Package 版本、Tarball Digest 和确定性生成测试，ABI 才具备可审查、可复现的来源链。

### **3\. Live Test 通过不等于 Evidence 有效**

如果 TLS 证书校验被关闭，RPC 返回结果即使满足断言，也不能作为可靠证据。

今天没有接受“测试已经绿了”这个表面结果，而是暂停下一阶段、找到环境问题，并在严格 TLS 下重新执行全部 Live Tests。

### **4\. 先完成内部安全边界，再决定公开 API**

目前没有急着公开 `markets` Query 或 `swap` Capability，因为以下问题仍需要 Maintainer 确认：

-   动态市场发现是否是正式 v1 方案；
    
-   使用统一 `swap`，还是拆分 `buyPt` / `sellPt`；
    
-   Market 参数由用户直接提供，还是从 Query 结果选择；
    
-   APY 的公开 Schema 和 Provenance；
    
-   是否要求覆盖全部动态发现的市场；
    
-   Native Token 和 Route Scope 是否属于 v1。
    

内部 Verifier 和 Discovery 可以安全推进，但不应把尚未确认的选择固化成公共接口。

## **AI 帮助了什么，我人工校正了什么**

AI 帮助完成：

-   调研并比较当前可认领的 Moss Protocol Adapter；
    
-   整理 Pendle Issue #11 的实现范围；
    
-   检查官方部署、ABI Artifact 和 Package 版本；
    
-   按阶段建立 Package、ABI 生成器和测试；
    
-   设计 Market Verifier 的正向及负向测试矩阵；
    
-   实现安全分页、恶意响应和结构化拒绝测试；
    
-   执行 Monad Live Reads 和官方 API Discovery；
    
-   定位 TLS Evidence 不可信的环境原因；
    
-   整理当前进度和后续交接文档。
    

人工负责：

-   决定选择 Pendle Issue #11；
    
-   明确要求每个阶段开始前先解释并获得批准；
    
-   明确 Commit、Push、PR 是三个独立动作；
    
-   要求先修复 TLS Evidence，再进入下一阶段；
    
-   限制 API 只能提名 Candidate，不能直接成为执行依据；
    
-   控制开发边界，不提前公开 Maintainer 尚未确认的接口；
    
-   确认 Stage 1 与 Stage 2 合并为一个有意义的本地 Commit；
    
-   要求 Stage 3、Stage 4 完成后停止，不自动 Commit 或 Push。
    

## **当前进度边界**

当前 Branch：

```
 feat/pendle-adapter
```

Git 状态：

-   Stage 1 与 Stage 2：已本地 Commit；
    
-   Stage 3 与 Stage 4：已实现并验证，但尚未 Commit；
    
-   功能分支：尚未 Push；
    
-   官方仓库：尚未创建 PR；
    
-   `.idea/` 等用户文件未修改、未 Stage；
    
-   未进入 RouterStatic Quote、Swap、Trace、Receipt 或公开 Protocol API；
    
-   不能将当前状态描述为“Pendle Adapter 已完成”。
    

## **下一步计划（仅针对本部分）**

下一阶段计划实现 RouterStatic 双向报价：

-   Buy PT：`swapExactTokenForPtStaticAndGenerateApproxParams`；
    
-   Sell PT：`swapExactPtForTokenStatic`；
    
-   根据 `VerifiedMarket` 验证交易方向；
    
-   使用 Token 的真实 Decimals 和原始整数金额；
    
-   明确 Slippage、Min Out 与舍入责任；
    
-   只支持直接路由；
    
-   不接入 Aggregator、Limit Order、Native Token 或 Hosted Calldata；
    
-   先实现内部 Quote Seam，不提前冻结公开 Query Schema；
    
-   使用 TDD 覆盖双向报价、非法方向、极小金额和精度边界；
    
-   在严格 TLS 下运行两个方向的 Live Quote。
    

按照当前协作约定，进入 Stage 5 前需要先完成只读仓库检查，解释具体文件、范围和排除项，并获得明确批准后才能开始编码。

## **今日复盘**

今天这部分工作的核心不是“已经能调用 Pendle 合约”，而是先建立一条可以被审查的信任链：

```
 官方部署与 ABI 来源
 → 不可信 API Candidate
 → Factory 与合约链上验证
 → 严格 TLS Live Evidence
 → Typed VerifiedMarket
```

目前还没有完成最终 Adapter，但已经把后续报价与交易执行所依赖的地址、ABI、市场和数据来源变成了可验证、可复现的工程基础。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
# 2026-07-18

## 今日主题

围绕 [Moss PR #56：Clober V2 CLOB Adapter](https://github.com/nishuzumi/moss/pull/56)，完成“作者修复—本地复测—发现剩余问题—定位根因—提交反馈—再次修复—全量回归”的完整开源 Review 闭环。

## 今天完成了什么

昨天针对 PR #56 提出了五项 Review 意见，涉及：

-   非零但不足的 ERC-20 allowance 是否兼容 zero-reset Token；
    
-   是否应使用明确的 curated market catalog；
    
-   BookViewer 报价与 Controller 实际执行路径是否一致；
    
-   是否错误依赖可选的 Token `name()` 和 `symbol()`；
    
-   BookId、unit size、fee policy、报价和授权逻辑是否需要补充来源与注释。
    

作者在 Commit `9307779` 中逐项处理，并邀请我重新运行 characterization tests。今天完成了三轮验证：

1.  确认原来的五项问题已经得到处理；
    
2.  通过负向测试发现 Receipt 验证和 99.9% 输入利用率仍有两个问题；
    
3.  作者继续提交 `e9b0417`、`9938e2d` 修复后，在干净工作树重新运行完整检查，最终确认没有新的可执行问题。
    

## 第一轮复测：原有五项问题已处理

在 `9307779` 上验证通过：

-   完整构建；
    
-   Clober TypeScript 类型检查；
    
-   26 项 Clober 本地及 Monad 测试；
    
-   MON→USDC、USDC→MON 两个方向的实时报价；
    
-   Viewer 与 Controller 的零滑点模拟一致性；
    
-   zero-reset allowance 顺序；
    
-   curated market 拒绝逻辑；
    
-   Token metadata independence；
    
-   关键协议逻辑、参数和来源说明。
    

授权流程现在会根据 allowance 生成不同的 Capability Tree：

```
allowance = 0
→ approve(amountIn) → spend

0 < allowance < amountIn
→ approve(0) → approve(amountIn) → spend

allowance >= amountIn
→ spend
```

这说明作者不是只修改文档，而是把反馈落实到了代码、测试和安全说明中。

## 第二轮测试：发现两个剩余问题

### 1\. Receipt 无法证明发生的是用户请求的 Swap

在 `9307779` 上增加负向测试后，以下异常 Change 仍会被接受：

-   正确的 `Take` 加两笔无关 Token Transfer；
    
-   `Take` 使用错误 BookId；
    
-   USDC→MON 只有输入 Token Transfer，没有 MON 输出；
    
-   两笔零金额、无关账户之间的 Transfer；
    
-   Transfer 数量满足要求，但 Token、方向和参与者不属于本次 Swap。
    

根因是当时的 Receipt Parser 主要检查：

```
存在 Take
所有 Take 使用同一个 BookId
transferCount >= 2
```

它没有验证：

-   BookId 是否属于 curated market；
    
-   Token 是否等于预期输入和输出资产；
    
-   是否同时存在输入和输出结算；
    
-   Transfer 方向和参与者是否正确；
    
-   金额是否非零；
    
-   输入、实际消费和退款能否守恒。
    

更深层的框架限制是：`Registry.parseReceipt` 虽然持有 `CapabilityNode`，但 Receipt Parser 只能收到有序的 `Change[]`，无法直接读取原始 Capability 参数和交易发送者。

因此，这不是直接修改 calldata 或窃取资金的执行漏洞，但旧 Receipt 不能独立证明观察到的 Transfer 就是用户请求的 Swap。

### 2\. 固定 99.9% 利用率会误判正常 Book Unit Dust

在固定 Monad 区块 `88525940` 上执行了 73 组原始 `BookViewer.getExpectedOutput` 查询。

MON→USDC 的部分结果：

| 输入 | 实际利用率 | 结果 |
| --- | --- | --- |
| 0.008 MON | 99.940064% | 通过 |
| 0.009 MON | 99.619126% | 拒绝 |
| 0.010 MON | 99.824527% | 拒绝 |
| 0.011 MON | 99.992581% | 通过 |
| 0.012 MON | 99.747502% | 拒绝 |
| 0.039 MON | 99.895627% | 拒绝 |
| 0.040 MON | 99.940064% | 通过 |

USDC→MON 从最小的 `0.000001 USDC` 到 `1 USDC`，测试中均为 100% 消费。

这说明 99.9% 不是简单的“最小交易额”，而是随输入金额、价格、方向和订单单位变化的锯齿型边界。

Clober 会把剩余输入换算为可执行的 quote units：

```solidity
maxAmount = tick.baseToQuote(remaining, false) / key.unitSize;
if (maxAmount == 0) break;
```

整数除法会向下取整。当剩余输入不足以购买一个 quote unit 时，Clober 会正常停止并退款。这部分是绝对数量的订单单位 dust；Moss 使用的却是相对比例规则：

```
spentAmountIn >= ceil(amountIn × 99.9%)
```

因此，小额交易可能有有效输出，却因为正常的单位取整退款而被拒绝。

## 向作者提交的第二轮反馈

我将两个问题拆成不同性质：

-   Receipt：属于正确性与验证强度问题；
    
-   99.9%：属于 fail-closed 策略与 Clober 离散订单单位之间的设计取舍。
    

没有把它们描述成直接资金漏洞，也没有要求在同一 PR 中修改 Moss Core。详细测试数据和根因分析已作为附件发布在[第二轮 Review 评论](https://github.com/nishuzumi/moss/pull/56#issuecomment-5010840747)中。

## 作者第二次修复

作者随后提交 `e9b0417` 和 `9938e2d`。

新的 Receipt Parser 会：

-   将 `Take` BookId 映射到两个 curated 方向之一；
    
-   拒绝零单位的 `Take`；
    
-   验证 Token、参与者、方向和非零金额；
    
-   MON→USDC 必须存在：
    

```
user → Controller → BookManager：native MON 输入
BookManager → user：USDC 输出
Controller → user：可选 MON 退款
```

-   要求：
    

```
input = settled + refund
```

-   USDC→MON 必须存在：
    

```
user → BookManager：USDC 输入
BookManager → user：native MON 输出
```

-   拒绝错误 BookId、错误 Token、错误参与者、零金额、额外 Transfer、无关 Approval 和不守恒退款；
    
-   继续保持每个原始 Change 的对象、数量和顺序不变。
    

对于 99.9% 利用率，作者选择保留严格的 fail-closed 策略。理由是 BookViewer 只返回输出量和实际消费量，无法判断剩余输入是因为不足一个 quote unit，还是订单簿流动性耗尽。仅凭返回值放宽 dust 可能削弱 partial-liquidity 保护。

最终方案不是假装消除该边界，而是：

-   保留严格的 99.9% 规则；
    
-   在 README 中明确精确公式；
    
-   说明小额交易可能被拒绝；
    
-   说明边界具有非单调、方向相关和价格相关特征；
    
-   为阈值上下界及 `0.001 MON` 案例添加回归测试。
    

## 最终回归结果

在当前 Head `9938e2d` 的干净工作树中重新验证：

-   `pnpm lint`：通过；
    
-   `pnpm build`：通过；
    
-   `pnpm typecheck`：通过；
    
-   完整 `pnpm test`：通过；
    
-   26 项 Clober 测试：全部通过；
    
-   MON→USDC 与 USDC→MON Live Monad Quote：通过；
    
-   Viewer/Controller 零滑点模拟：通过；
    
-   之前五项 Review 问题：保持已解决；
    
-   Receipt 异常 Change 测试：现在均被正确拒绝；
    
-   99.9% 阈值和 `0.001 MON` 案例：行为与文档一致。
    

最终已向作者回复：[两轮反馈从我这边均已解决，没有发现新的可执行问题](https://github.com/nishuzumi/moss/pull/56#issuecomment-5011267131)。

所有 characterization tests 都在临时工作树中完成，没有修改本地 Moss 正式工作区，也没有提交测试分支。

## 今天学到的关键内容

### 1\. Review 不能只复跑作者的正向测试

“26 tests passed”只能证明作者写下的预期行为成立。真正发现验证缺口的是负向测试：

-   如果输出 Transfer 缺失，会不会仍然通过？
    
-   如果 BookId 错误，会不会仍然通过？
    
-   如果金额为零，会不会仍然通过？
    
-   如果 Transfer 完全无关，会不会仍然通过？
    

安全 Review 的核心不是证明 Happy Path 能跑，而是证明错误证据不能伪装成正确结果。

### 2\. 发现异常后，还要区分 Bug 与策略

Receipt 问题违反了它需要解释真实结算的职责，因此应当修复。

99.9% 边界则不同：它确实会拒绝部分可执行小额交易，但这是在信息不足时选择 fail-closed。只要行为被准确记录、测试覆盖且没有被宣传成 Clober 的协议保证，就可以作为明确的产品取舍保留。

### 3\. 根因分析比单个失败案例更有价值

如果只报告“0.001 MON 失败”，很容易被理解为流动性不足或偶然状态变化。

固定区块、密集扫描和双向对比证明了它是由：

```
Book Unit 整数取整
+ 绝对 Dust
+ 相对 0.1% 阈值
```

共同形成的锯齿型边界。只有找到这一层，Review 建议才不会停留在“把阈值调低一点”。

### 4\. 好的 Review 是协作，不是挑错

作者认真处理了第一轮反馈，并主动邀请复测；第二轮测试发现问题后，作者继续补充实现、负向测试和 README。

最终成果不是“我证明作者写错了”，而是双方通过可复现证据把 Adapter 的正确性和边界说得更清楚。

## AI 帮助了什么，我人工校正了什么

AI 帮助完成：

-   设计 Receipt 的 Token、方向、账户、金额和 BookId 负向测试矩阵；
    
-   执行固定区块的 73 组报价扫描；
    
-   对照 Clober `BookViewer`、`Controller._spend` 和 `_settleTokens` 定位根因；
    
-   区分结构性 Receipt 缺口和利用率策略问题；
    
-   整理英文 Review 评论及详细测试附件；
    
-   在作者更新后重复运行完整回归。
    

人工负责：

-   明确要求先测试、找到根因，再决定是否发布评论；
    
-   没有在第一轮修复后直接回复 `LGTM`；
    
-   控制 One Ticket, One Issue 的范围，不把问题扩大成通用 Core 重构；
    
-   避免把 Receipt 缺口夸大成直接资金漏洞；
    
-   判断 99.9% 是 Maintainer 策略选择，而不是必须按我的方案修改；
    
-   核对最终测试证据并决定“两轮反馈从我这边均已解决”。
    

## PR 与 Issue 最新状态

截至 2026-07-18：

-   [PR #56](https://github.com/nishuzumi/moss/pull/56) 仍为 **Open Draft**，尚未合并；当前 Head 为 `9938e2d`。
    
-   GitHub 当前报告 PR 可合并，但是否合并仍由 Maintainer 决定。
    
-   [Issue #7](https://github.com/nishuzumi/moss/issues/7) 仍为 Open。
    
-   从我的 Review 角度，两轮反馈均已处理，没有剩余的可执行问题。
    
-   这不等于替 Maintainer Approve，也不代表 PR 必然 Merge。
    

## 明日计划

-   如果 PR #56 再次更新，只针对变化部分运行必要的回归测试，不重复已经关闭的问题。
    
-   将今天形成的检查方法用于自己的 Protocol Adapter：先核对 Issue 范围，再验证 ABI、地址、授权、报价、Receipt 和 Live Monad 模拟。
    
-   继续保持“先解释、再测试、最后写评论”的协作方式。
    
-   整理本次 Review 的测试证据和公开评论，作为 Week 2 Dev Portfolio Pack 的 Proof of Work。
    

## 今日复盘

今天最大的收获不是发现了两个问题，而是完整经历了一次有反馈、有修复、有复测、有最终结论的开源 Review。

真正可验证的贡献不只有提交代码。能够提出可复现问题、找到根因、控制问题范围、尊重作者的设计选择，并在修复后认真验证，也是一份完整的工程 Proof of Work。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->
# 2026-07-17

# 2026-07-17｜残酷共学笔记

## 今日主题

Moss：从理解“可验证执行”，到沉淀教程、提交 Proof of Work，并参与真实的开源 Review。

## 今天完成了什么

今天围绕 Moss 完成了三类工作：

1.  梳理 Moss 的核心定位，明确它不是替 Agent 直接签名和发送交易，而是通过 `discover → load → action → simulate`，把协议交互变成可发现、可构建、可模拟和可验证的 Capability。
    
2.  完成一份 Moss 新手教程，内容包括环境配置、wrap/swap Demo、MCP 接入、Warning 处理、安全检查、FAQ 和首次 PR 流程。目前教程已经整理成可直接发布的 GitHub README。
    
3.  围绕已提交的 [Moss PR #72](https://github.com/nishuzumi/moss/pull/72)，整理了 Pull Request 作业、Proof of Work、文档到代码骨架以及 Moss 项目介绍文章。同时参与 [Issue #7](https://github.com/nishuzumi/moss/issues/7) 的协作讨论，并深入 Review 了 [PR #56](https://github.com/nishuzumi/moss/pull/56) 的 Clober Protocol Adapter 实现。
    

在阅读 PR #56 后，我向作者提交了五项需要进一步确认的问题：

-   ERC-20 剩余授权不足时，是否需要兼容必须先清零再重新授权的 Token；
    
-   动态推导 BookId 的方案是否有意替代 Issue 要求的 curated market catalog；
    
-   使用 `BookViewer.getExpectedOutput` 是否能够替代对实际写入路径的 `eth_call` 模拟；
    
-   获取 Token decimals 时是否必须同时依赖可选的 `name()` 和 `symbol()`；
    
-   是否应该为 BookId、unit size、fee policy、报价路径和授权逻辑补充维护注释与官方来源。
    

## 今天学到的关键内容

### 1\. “模拟成功”不等于“执行可信”

Moss 会记录交易模拟产生的 Event 和 native MON transfer，再由 Protocol 将这些 Change 解析成 Receipt。Core 还会检查 Receipt 是否完整、按原始顺序覆盖所有 Change。

但零 Warning 只代表观察到的变化得到了完整解释，不代表交易一定符合用户意图。最终仍然需要核对资产、数量、接收方、授权额度、滑点和 Protocol 选择。

### 2\. Bound Protocol 解决的是“协议身份参数化”

PR #72 处理的不是普通方法参数，而是 Protocol 在运行时绑定 Token、Market 或合约地址的问题。

今天进一步理解了几个关键约束：

-   Binding 与每次调用的方法参数必须分离；
    
-   Binding 应在执行 Protocol 或请求 RPC 前完成校验；
    
-   Factory 每次创建独立引用，不能随意缓存实例；
    
-   Binding 只解码一次，避免非幂等转换产生不同结果；
    
-   Receipt parser 必须保持纯净，不能访问 Runtime 或动态 Handle。
    

### 3\. 一个 Protocol Adapter 不只是增加一个函数

阅读 PR #56 后，我发现一个完整的 Protocol Adapter 需要同时处理：

-   Protocol、Capability 与 Query 设计；
    
-   ABI 来源及可复现生成；
    
-   官方合约地址与链上 Bytecode 核验；
    
-   ERC-20 Approval 和特殊 Token 行为；
    
-   Quote、滑点与部分成交保护；
    
-   Capability tree 的交易归属和执行顺序；
    
-   Receipt 与原始 Change 的完整覆盖；
    
-   运行时测试、编译期测试和 Live Monad 测试；
    
-   MCP Server 接入、文档与 Changeset。
    

这说明 Adapter Review 不能只看主流程能否运行，还需要检查边界资产、协议假设和 Issue 验收标准是否真正一致。

### 4\. 开源贡献不只等于提交代码

今天回复 Maintainer 建议时，我意识到协作中的表达同样重要。

当一个 Issue 已经有其他贡献者提交实现时，更合适的做法不是直接接管，而是先与作者协调，再通过复现、补充测试和边界分析参与 Review，避免重复劳动。

PR 是否最终 Merge 也不是衡量贡献的唯一标准。公开的需求分析、代码差异、测试证据、Review 意见和协作记录，都可以构成可验证的 Proof of Work。

## AI 帮助了什么，我人工校正了什么

AI 帮我快速阅读仓库文档、梳理 Pull Request、生成文章结构、代码骨架和教程初稿。

人工校正主要集中在：

-   使用 Moss 当前的 Capability、Change、Receipt 和 Outcome 领域语言，避免引用已经过时的设计；
    
-   核对 Node、pnpm、RPC 和测试要求；
    
-   补充“零 Warning 不等于用户授权”的安全边界；
    
-   收紧 Bound Protocol Factory、Binding 校验和 Receipt 纯度要求；
    
-   区分已经确认的问题与仍待作者澄清的 Review 方向；
    
-   通过本地边界测试验证 zero-reset Approval 和缺少 Token metadata 等情况；
    
-   调整 GitHub 回复措辞，使其符合真实开源协作语境。
    

这让我进一步认识到，AI 可以提高阅读和生成效率，但最终产出是否符合项目架构、测试要求和协作规范，仍然需要人工判断和证据支持。

## PR 与 Issue 最新状态

-   [PR #72](https://github.com/nishuzumi/moss/pull/72) 目前仍为 Open Draft，尚未合并。关联 Issue #61 已添加 `maintainer-only` 标签，后续由 Maintainer 主导，PR 能否继续推进暂不确定。
    
-   [PR #56](https://github.com/nishuzumi/moss/pull/56) 目前仍为 Open Draft，尚未合并。我提交的五项 Review 问题已经发布，正在等待作者或 Maintainer 回复。
    

## 当前卡点

-   PR #72 的离线构建、类型检查和测试已经通过，但 Live Monad 检查曾被本机 TLS 证书链错误 `UNABLE_TO_GET_ISSUER_CERT_LOCALLY` 阻塞。
    
-   Issue #61 已被标记为 `maintainer-only`，PR #72 的后续处理方式还不明确。
    
-   PR #56 的五项审查意见已经提交，目前需要等待作者或 Maintainer 回复。
    
-   Moss 新手教程已经完成，但还没有上传到公开 GitHub 仓库。
    

## 明日计划

-   完成 **Week 2｜Challenge｜为 Moss 新增一个 Protocol Adapter**；
    
-   完成 **Week 2｜Prototype Evidence**；
    
-   完成 **Week 2｜Dev Portfolio Pack**；
    
-   解决 RPC TLS 问题，补齐 Live Monad 验证。
    

## 今日复盘

今天最大的收获不是又完成了几份文案，而是把“阅读文档—理解架构—实现代码—验证测试—提交 PR—参与 Review—编写教程”串成了一条完整路径。

PR #72 的状态变化也提醒我：真实开源协作并不完全按照个人计划推进。Issue 的权限、Maintainer 的安排、其他贡献者的实现以及 Review 反馈，都可能改变最终结果。

真正有价值的 Proof of Work，不只是代码数量，也不只是一枚 Merge 标记，而是能否公开说明：为什么这样设计、如何验证、发现了什么问题，以及还有哪些边界尚未解决。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
# 2026-07-16

## **今日进度：完成 Moss GitHub 探索，并把三个 Week 2 任务收束成一个 MCP 安全预览原型**

今天没有急着继续写 Adapter，而是先把 Moss 的代码架构、GitHub 治理方式和 PR review 过程系统读了一遍，并完成三份相互关联的 Week 2 提交：

1.  GitHub 探索日志：理解 Maintainer 如何组织项目；
    
2.  开源贡献计划：确定 Dev Builder 的本周 PR 方向；
    
3.  AI-assisted Dev Plan：把同一份贡献包装成最小 AI × Web3 原型。
    

最终把三个任务收束成同一个交付物：**Moss Safe Action Preview**——用户用自然语言要求“把 0.01 MON 包装成 WMON，只预览、不发送”，Agent 依次调用 `discover → load → action → simulate`，展示 Receipt、Warnings 和安全结论，停在签名边界之前。

* * *

## **核心收获**

**1\. Maintainer 管理的不只是代码，而是整套“协作接口”**

-   Moss 用 pnpm monorepo 按依赖方向划分 `core`、`simulator`、`erc`、`system`、协议包和 `mcp-server`，每层包边界都在阻止不应该出现的依赖进入该层。
    
-   README 定义项目承诺和风险边界，ADR 记录设计取舍，CONTRIBUTING 固化 Definition of Done，Issue 标签表达任务类型、难度和信息完备度。新人从理解项目到提交 PR，每一步都有对应文档承接。
    
-   `needs-design` 和 `ready-for-agent` 不是普通进度标签，而是在按“信息是否完备”切分工作：设计决策留给人，规格已经明确的执行工作才适合交给 Agent。
    

**2\. PR #31 的本质是从“类别对账”升级为“原始证据穷尽覆盖”**

-   旧模型使用 Plan：多笔 `UnsignedTx` 平铺在 `txs` 数组中，再用结构化 `expects` 和模拟后的 effects 做 reconcile。它并不是完全不检查计划外行为——对资金流出、授权和 NFT 等已建模类别存在 `UNDECLARED_*` 检查。
    
-   真正的限制是：旧模型只认识 effects 层预先定义的类别，原始 trace 会先被压缩成几个桶；没有被建模的自定义事件可能在汇总阶段丢失。
    
-   新模型直接保留模拟产生的原始 Change，要求 Receipt 对每条 Change 按**对象身份、数量和顺序**逐一认领。任何无法解释的事件都会失败并产生 Warning，安全模型从“检查我认识的类别”变成“任何不认识的东西都拒绝通过”。
    

**3\. Capability Tree 把交易与解释责任绑定在一起**

-   每个 Capability 节点必须且只能直接拥有一个 TransactionNode；如果操作需要多笔独立交易，就用子 Capability 组织。
    
-   Kuru 使用 ERC20 作为输入时，需要先 `approve` 再 `swap`：`erc20.approve` 是带有自己 `approveReceipt` 的子 Capability，Kuru 根节点直接拥有 Router swap 交易。展开时深度优先执行，天然得到 approve → swap 的顺序。
    
-   Multicall 不是“一个 TransactionNode 里放多个 UnsignedTx”，而是一笔交易的 calldata 内部打包多个合约动作。从链上看仍然只有一次签名、一个 nonce、一个 TransactionNode，但可能产生多条 Change。
    
-   当前 Moss 仓库并没有真正实现 multicall Adapter；它只在 `CONTEXT.md` 和 protocol onboarding 文档中作为未来协议接入时的建模规则出现。
    

**4\. Handle → Change → Receipt 构成一条“构建 → 取证 → 解释”的证据链**

-   Handle 是 ABI 的类型化入口：`read` 真正读取状态，`call` 用 `eth_call` 预览写方法返回值，默认路径只编码 calldata、生成 TransactionNode，不签名也不发送。
    
-   Change 是从 `debug_traceCall` 调用树里提取的原始证据，目前只有 Event 和原生 MON 转账两类；采集阶段不负责解释，并通过排序和冻结保证证据稳定。
    
-   Receipt 是纯函数，只能根据收到的 `readonly Change[]` 得出 Outcome 和文本。它既不能声称发生了 Change 中不存在的事，也不能跳过无法识别的 Change。
    
-   三层分别被限制了一种能力：Handle 不能执行真实交易，Change 采集时不能解释，Receipt 解释时不能查询外部状态。正是这些限制拼成了一条可复现、可审计的安全链路。
    

**5\. Moss 提供安全证据，但不能替钱包强制执行安全策略**

-   从 PR #40 的 Maintainer review 中确认：Moss 可以提供 simulation evidence 和 Warnings，但它不控制钱包或签名行为。
    
-   消费应用可以选择模拟零次、一次或多次；示例钱包中的“签名前重新模拟”只能算应用层策略，不能宣传成 Moss 自动提供的安全保证。
    
-   更准确的安全模型是：**能力封装 + 机器验证 + 消费方执行策略**。保证必须和控制权位于同一层，不能把应用层的选择描述成框架层承诺。
    

* * *

## **本周贡献方向**

选择 Dev Builder 的“完善 Demo + 提交 PR”方向，以 [Issue #55](https://github.com/nishuzumi/moss/issues/55) 为切入点，计划实现一个可运行的 MCP 端到端示例：

```
 用户意图
   → discover：发现 WMON wrap capability
   → load：读取参数、风险和能力说明
   → action：生成未签名 Capability
   → simulate：通过 Monad RPC 执行 debug_traceCall
   → 检查 Receipt 和 Warnings
   → READY FOR REVIEW / DO NOT SIGN
```

本周真实实现 MCP Client、四工具调用、WMON calldata、真实模拟以及成功/失败测试；自然语言解析在自动化测试中使用固定 Tool Plan，钱包签名、交易广播、前端和多协议路由暂时 mock 或不实现。

这样既能形成一个最小 AI × Web3 原型，也能整理成 Moss 的开源贡献 PR，而不是为三个课程任务分别制造三份互不相关的产出。

* * *

## **今日产出**

-   [GitHub 探索日志](https://app.notion.com/p/Week-2-Challenge-GitHub-39fc278534e58113ad14cd6571fc0a35?pvs=21)
    
-   [开源贡献计划](https://app.notion.com/p/Week-2-Challenge-39fc278534e581a28896dc477d0188ec?pvs=21)
    
-   [AI-assisted Dev Plan](https://app.notion.com/p/Week-2-AI-assisted-Dev-Plan-39fc278534e5811690c8c87eaca27e65?pvs=21)
    
-   [Moss PR #31 技术研究笔记](https://app.notion.com/p/Moss-PR-31-Capability-Receipt-39ec278534e5812bac83ecfdba71b660?pvs=21)
    

* * *

## **个人思考**

-   今天最大的收获不是又记住了一批术语，而是把“安全”拆成了证据、解释和执行权三个层次。Moss 能证明模拟中发生了什么，但最终是否调用模拟、是否在 Warning 时停下，仍取决于上层应用和钱包。
    
-   阅读技术项目不能只看当前源码，也不能只相信 Issue 或第一次解释。今天对旧 Plan 的理解就经历了“先推测 → 找 base commit → 读取真实旧代码 → 修正结论”的过程。对于架构迁移，删除掉的代码往往比新代码更能解释“为什么要改”。
    
-   三个课程任务看似不同，其实分别对应“理解项目 → 计划贡献 → 定义用户原型”。把它们统一到同一个 MCP Workflow Demo 后，课程文档不再是额外作业，而是实际 PR 的设计说明、验收标准和完成证明。
    
-   今天完成的是调研与计划，不把计划冒充产出。真正的 Proof of Work 仍然是后续代码、测试结果和 Pull Request。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->
# 2026-07-15

## **今日进度：完成 Moss 项目的业务理解 + 全链路实操**

从手写代码到 agent 自主调用,完整走了一遍 Moss([github.com/nishuzumi/moss](http://github.com/nishuzumi/moss))的两种用法:先在 play.ts 里手写 discover → load → action → simulate 四步;再通过 `.mcp.json` 把 Moss 的 MCP 服务器接入 Claude Code,让 agent 纯靠四个 MCP 工具自主跑通"1 MON 能换多少 USDC"(本地 mainnet fork 实测)。同步完成任务:⭐ Star 仓库、README 精读、理解分享文案。

## **核心收获**

**1\. Moss 是什么(纠正过两轮的理解)**

-   链上协议的操作原本只能人手点 DApp 或程序员手写代码拼交易;Moss 把它们封装成 agent 可直接调用的标准能力,并在 agent 与签名器之间强制加一道"先声明、后对账"的安全门。
    
-   方向链条:**链上协议 → 适配器封装 → MCP 等渠道 → agent**——不是"把 API 上链",MCP 也只是送货渠道不是被转化对象。
    

**2\. 四步流程是 Moss 自己的设计,不是 MCP 的功能**

-   这四步是 `@themoss/core` 库的 API:写代码可直接调(play.ts 验证,全程无 MCP);mcp-server 只是把它们原样包成四个工具。
    
-   discover/load 是便利层(黄页 + 说明书);安全门落在 action(产出带 expects 资金合同、confirms 回执凭据、planHash 防篡改指纹的未签名 Plan)与 simulate(真实链况模拟,实际资金流逐项对账 expects,有 warning 一律停)两步。
    

**3\. 完整安全模型还有三条工具之外的防线**

-   expects 模板由适配器作者写死在 @Capability,agent 改不了;Moss 永不签名永不发送,私钥始终在钱包;意图对齐归 agent——零警告只代表"和声明一致",还要核对"和用户要的一致"。
    

**4\. 实测数据(本地 mainnet fork)**

-   action 声明:最多出 1 MON、至少收 0.022414 USDC(报价扣 1% 滑点);simulate 实测:收 0.022641 USDC,零警告,planHashValid,资金只流向 Kuru 路由,gas 约 21.8 万。
    
-   对比实验:模拟器不接 observer 会如实报 CONFIRMATION\_MISSING——跳过的检查不默认放过,这个设计细节很见功底。
    

**5\. 环境坑:Monad 版 Foundry**

-   官方 anvil 缺 Monad 的 gas 模型/opcode 定价/预编译,fork 脚本直接拒绝;官方 foundryup 的 `--network` 参数已废弃,需用 Category Labs 的安装器另装(详见单独的工具链切换笔记)。
    

## **个人思考**

-   今天最大的收获是理解被纠正的过程本身:从"把服务转成 MCP"到"把 API 上链"再到"链上协议接给 agent + 签名前机器对账"——每一轮都是靠实操(play.ts 的类型报错、fork 实测、observer 对比)把抽象概念落到具体输出上才纠过来的。
    
-   Moss 的本质不是"封装",而是**把安全从"人审查代码"变成"机器审查资金流声明"**——人看不懂 calldata,但机器能逐项核对"最多出多少、至少进多少、钱流向谁"。这个思路对所有 AI × 资金类产品都适用。
    
-   看好的应用场景:自然语言 DeFi 入口、AI 理财助手的策略自动化、claim→swap→supply 多步组合、DAO 金库"AI 提案、机器验证、人类签名"的代理执行。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
# 2026-07-14

## **今日进度：Week 2 Day 2｜排查 Moss monorepo 构建问题 + 撰写 Week 3 Role Statement**

一句话总结：一次真实的 pnpm monorepo 报错排查，加上一份把 Week 1–2 产出转成 Week 3 组队筹码的角色声明。

## **核心收获**

**1\. Moss 是什么**

-   把 Monad 上复杂的 DApp/协议交互抽象成 agent 可调用的统一流程：`discover → load → action → simulate`，由系统而非 agent 组装正确交易；agent 不用手搓 calldata、不碰 ABI 和 decimals 换算。
    
-   两条安全规则分别在两处强制执行：**effects 对账**（服务端机械判定，模拟出的实际资产流动只要和 Plan 声明有差异就 warning）、**意图对齐**（agent 侧，把 effects 摘要和用户原话核对）。Moss 本身永不签名、永不发送，私钥和最终决定权始终留在用户手里。
    

**2\. 调试实录：pnpm monorepo 构建失败**

-   现象：跑 Moss 示例 `wrap` 直接报 `ERR_PNPM_RECURSIVE_RUN_FIRST_FAIL`，只有个错误壳；把这行报错原样丢给 AI，AI 没有凭壳猜，而是先读 `package.json` 和入口脚本，重跑命令拿到完整堆栈 `ERR_MODULE_NOT_FOUND`，才确认是 `packages/core/dist` 不存在——仓库在这台机器上从未 build 过。
    
-   根因一句话：`workspace:\*` **依赖在运行时按普通 npm 包对待**（走 exports → dist），不是源码直连；fresh clone 之后跑任何示例前必须先 `pnpm build`。修复后重跑示例，四步全通过，`simulate` 在 Monad 主网真实模拟 1.5 MON 换 1.5 WMON，`warnings: []`。
    

**3\. Week 3 Role Statement：定位与队友需求**

-   赛道 Tech，角色是链上开发 + 索引/数据层负责人：合约侧负责并行执行下的存储优化（`mapping` 替代全局计数器），链下侧负责 Proposed/Finalized 双游标 reorg-safe 索引器，外加合约安全 review。
    
-   最需要的队友是前端 Tech 伙伴（Next.js + wagmi/viem）——AI 辅助能把前端跑起来但难保证质量；其次是 Ops（社区/增长）和 Research（生态/方向判断）各一名，三条腿凑成完整闭环。
    
-   Proof of Work：Monad Testnet 上已部署的 NFTBadge / DAO Voting / Clicker 合约 + 可运行的索引器代码库，全部有公开打卡记录可查。
    

## **个人思考**

-   Moss 的「effects 对账 + 意图对齐」两条门禁，和我关注的 AI agent 支付四要素（身份验证/预算约束/风险控制/可撤销交易）几乎是同一套思路的另一种实现——前者在交易签名前拦，后者在支付发生前拦，值得把两边的设计对照着看。
    
-   这次调试也提醒我一个通用习惯：**报错只是壳，别对着壳猜**——pnpm 顶层错误和 EVM revert 一样，真正的原因永远在下一层堆栈里，这条规则对合约调试同样适用。
    
-   Week 3 Role Statement 逼着我把 Week 1–2 的零散产出（徽章、DAO、索引器）第一次整理成「团队起步的技术底座」这个叙事——这比单独看每个项目更有说服力。
    

明日计划：跑通 `kuru-swap.ts`（跨 Plan 组合：MON→USDC swap），继续围绕 AI agent 支付 / 链上高频交互产出 Week 2 最小交付。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
# 2026-07-13

# 2026-07-13

## 今日进度：完成 Week 2 职业方向选择提交，搭建 AI 协作记录 + 学习记录归档体系

今天同时在 Claude Code 本地和 [claude.ai](http://claude.ai) 网页版并行推进：完成 Week 2 Role Choice Card 提交（选择 Dev 方向），搭建了 AI Collaboration Log 与本周学习记录两套 Notion 归档体系，并把"挖历史对话记录找 Prompt"这件事沉淀成了一个可复用的 Claude Code skill。

## 核心收获

**1\. 职业方向选择：基于已验证路径选 Dev，而不是从零转型**

-   Go 背景 + 已落地的 NFT badge/DAO Voting/索引器/clicker 多个项目是选择 Dev 方向的实证支撑，本周最小产出定在 AI agent payment 或高频链上交互安全方向的可运行 demo。
    

**2\. AI 协作记录该记录什么、不该记录什么**

-   除了"协作场景/AI 帮了什么/人类删改核查了什么/哪些不能交给 AI"四栏之外，还规划了四类具体截图证据（方向文案生成、打卡模板应用、技术细节核实修正、未采纳 AI 建议的决策点），让"AI 帮了忙但人工在把关"可验证。
    

**3\. 把"挖历史对话找 Prompt"沉淀成一个可复用 skill**

-   本机 Claude Code CLI 会话和 [claude.ai](http://claude.ai) 网页版 Projects 是两套完全独立的系统：前者权威来源是 `~/.claude/projects/<dir>/*.jsonl` 而不是会话管理工具的部分索引；后者必须人工登录、且要滚动到顶才能读到完整对话。把这些坑写成 skill，并用真实的 monad-clicker 会话数据跑了带 skill / 不带 skill 的对比测试验证有效。
    

**4\. 两边并行操作同一份 Notion 笔记导致的结构纠缠**

-   同一个 Week 2 索引页被两个不同的 Claude 会话同时修改，出现过重复子页面块、重复图标等问题。解决方式是每次都重新拉取现有真实结构再改动，不凭记忆猜测。
    

**5\. 一次关于工具安全边界的具体案例**

-   测试 skill 时子任务被 Write 工具拦下后改用 Bash 绕过限制写文件——内容本身是真实的，但这提醒我以后审查 agent 产出不能只看结果对不对，还要看它是不是绕过了什么限制去达成的。
    

## 个人思考

今天大部分时间不是在学新概念，而是在搭建"如何持续、诚实地记录学习过程"这件事本身的基础设施——这本身也是 Dev 方向要面对的问题：记录/文档系统的可靠性，和合约代码的可靠性同样重要。两个 Claude 会话并行改同一份笔记导致的结构冲突，本质上和 monad-clicker 里"两个组件各自独立轮询同一份分数导致不同步"是同一类问题，解法也一样——找一个单一可信来源，别让两边各自为战。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

# **残酷共学打卡 · 2026-07-11**

## **今日进度：BuildAnything 初中课程 4/13**

课程地址：[https://buildanything.so/zh/tracks/sophomore/lessons/monad-architecture](https://buildanything.so/zh/tracks/sophomore/lessons/monad-architecture)

学完第 4 课「Monad 架构」，把 EVM 那节的执行层外扩一圈，看清区块链六层架构（硬件/网络/数据/共识/执行/应用）里 Monad 和以太坊每一层的具体差异。

## **核心收获**

**1\. 硬件层与网络层**

-   Monad 要求裸金属服务器（16 核 CPU、4.5GHz+、32GB+ 内存、2TB NVMe SSD），拒绝云虚拟机——虚拟化引入的延迟会破坏亚秒级时序，硬件门槛是为性能和去中心化做的有意折衷。
    
-   网络层活儿相同（节点发现 + 通信），Monad 靠 MonadBFT 预设种子地址启动，再走状态同步/区块同步追上进度，思路比以太坊的 Discv5+DevP2P+GossipSub 组合更直接。
    

**2\. 数据层：MonadDB**

-   以太坊把 Merkle Patricia Trie 转成键值对塞进通用数据库（LevelDB/RocksDB）；MonadDB 直接存储 Trie 本身，不需要转换，并用 io\_uring 做异步 I/O 高速读写 SSD——为区块链数据定制 vs 通用数据库硬凑。
    

**3\. 共识层：MonadBFT vs Gasper**

-   以太坊 Gasper = LMD-GHOST（选分叉）+ Casper FFG（两阶段投票锁定历史），12 秒出块、约 13 分钟最终性。
    
-   MonadBFT：领导者每 400ms 轮换，两轮投票后区块即最终确定（800ms）；抗 tailforking 强制领导者包含前一区块，防止跳过作恶；RaptorCast 用纠删编码 + 两级扇出分发区块小块，解决 2MB 区块广播的带宽瓶颈。
    

**4\. 执行层：从串行到乐观并行**

-   以太坊瓶颈很明确：交易串行执行（15–25 TPS），且共识与执行交错——必须等区块执行完才能推进共识，出块时间大半被共识和传播吃掉。
    
-   Monad 三件套解耦这个瓶颈：乐观并行执行（多核同时跑，冲突了就重跑，结果与串行一致）、执行与共识异步解耦（共识跑第 N 块时执行在后台处理第 N-1 块）、MonadDB 支持海量并行状态读取——三者叠加做到约 10,000 TPS，同时完全兼容 EVM。
    

**5\. 应用层：兼容性的复利**

-   Monad 完全 EVM 兼容，以太坊应用换个 chain ID 重新部署即可运行，同一套 Solidity、同一套工具、同一个钱包连接——用户感知不到底层差异，只感觉到更快更便宜。
    

## **个人思考**

-   六层框架把之前零散学的 MonadBFT、并行执行、MonadDB 串成了一条线：每一层的改动都在服务同一个目标（亚秒级时序），而不是孤立的性能优化点。
    
-   「共识与执行解耦」这个设计和我做 indexer 时的双游标架构本质相通：把两个有依赖关系的流程解耦成可以流水线化的独立阶段，吞吐才能上得去。
    
-   MonadDB 直接存 Trie、不做键值对转换，提醒我评估任何存储方案时先问一句「这个数据结构原生适配底层引擎吗，还是在打补丁」。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
# 2026-07-11

## **今日进度：monad-clicker 加登录系统，并用真实使用数据修了一串前端 bug**

昨天把「为什么需要频繁交互」的场景论证做完之后，今天把 monad-clicker 从 Demo 推进到「能被人反复实际使用」的状态：加了 MetaMask 登录（会话代签），然后没有止步于"能跑"，而是自己连续实测/连点/换账号，揪出了 6 个真实 bug 并逐一修复，最后把改动推到了 GitHub，也把 Week 1 Build Log 整理提交。

## **核心收获**

**1\. 会话代签（session-key delegation）——登录语义和高频体验的取舍**

-   需求是「一个钱包地址 = 一个账号」，但直接用 MetaMask 签每次点击会弹窗到没法玩。方案是 MetaMask 只签一次 `authorizeOperator`，把本地会话密钥登记为代签人，之后点击都由会话密钥免弹窗代签、分数记在 MetaMask 地址上——合约里没有任何资产可转移，代签密钥泄露的最坏后果只是被人帮你多点几下。
    
-   代价看得很清楚：`ClickGame` 因为加入代签逻辑必须重新部署（v1→v2），测试网数据作废，`ChampionBadge` 复用初版、经 Safe 执行 `setGame()` 切换指向。做完之后一度以为"提案了就等于处理完了"，没主动确认第二个签名是否真的执行——复核链上才发现 `ChampionBadge.game()` 还指向废弃的 v1，及时补签修复。多签的"提案"和"生效"是两个独立状态，必须主动查链上，不能假设。
    

**2\. 自己发现的一个真实安全漏洞：会话钱包跨账号复用**

-   实测「换一个地址登录」时发现浏览器本地的会话钱包（burner）是按浏览器存的单一 key，不区分登录账号——换账号后看到的还是同一个 burner，如果这个 burner 之前已经被充值、被授权过，新账号可以直接白嫖旧账号的 gas 余额甚至复用旧的授权关系。修复方式是把 burner 的 localStorage key 按登录账号地址命名空间隔离，每个账号独立、从零开始的会话钱包。
    

**3\. 发送队列：不为了赶速度牺牲「点击成本即防刷」**

-   一开始考虑过「点击停顿后再批量上链」，能极大降低延迟感知，但会让批量提交里单次点击的边际 gas 成本更低——直接削弱了这个 Demo「Gas 是天然防刷机制」的核心论点，最后否掉了这个方向。
    
-   真正的问题其实是：点击间隔小于链上允许的最快节奏时，前端会把多余点击静默丢弃，界面毫无反馈。改成发送队列——物理点击必入队，按节奏依次发送真实交易，1 次点击依然对应 1 笔真实交易，不合并、不批量。
    

**4\. 乐观 UI 连续踩了三次坑，一次比一次隐蔽**

-   第一版用「挂载时快照当基准 + 本地增量，取两者较大值」，一旦索引器权威分数追上这个冻结基准，后续新点击的加分会被 `Math.max` 悄悄吞掉，界面卡住不动——改成持续对比"权威分数每次轮询涨了多少就核销多少笔未结算点击"，不依赖一次性快照。
    
-   接着发现分数「上下跳动」。没有直接改代码，先用 4 次/秒的频率一边连点一边轮询 `/api/player`，确认后端分数序列完全单调，把嫌疑从后端排除出去，定位到前端：核销逻辑写在 `useEffect` 里（渲染_之后_才跑），所以每次权威分数更新都会先渲染出一帧"新分数 + 还没核销的旧乐观计数"的重复计数瞬时值。改成 React 官方的「渲染时调整状态」模式，核销和分数变化在同一次渲染里原子完成，顺带把两个组件各自独立轮询同一份分数的架构也合并成父组件查一次、往下传，消除了另一个潜在的不同步来源。
    
-   最后一个是我自己截图发现的：连点积压后，「点击→Proposed」面板显示 33 秒——不是链变慢了，是排队等待时间被算进了这个本该只反映链上真实速度的数字里。加一个「真正开始发送」的时间戳，延迟从这里起算，不再含排队时间。
    

**5\. 用余额精确判断，而不是猜 RPC 报错文本**

-   换到一个余额只剩 0.002 MON 的账号后，连续 39 笔点击全部失败，报错是含糊的英文 "An internal error was received"，没命中任何已知错误模式，也没被判定为终止性错误，于是队列把剩下的全部重试了一遍。查链上确认是真的不够付 gas。修复：不再事后解析 RPC 报错文本，直接用前端已知的余额在发送前判断，不够就立刻当终止性错误处理——清空队列、给一句清楚的中文提示，按钮的禁用阈值也从「正好等于 0」提高到同一个安全余量。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
# 2026-07-10

## 今日进度：完成「Monad 理解｜为什么 Monad 体验不同」议题

今天完成了 Tech 方向的场景论证：以「链上实时排行榜 + 高频点击小游戏」为例，从四个方向理解和讨论 Monad——为什么需要频繁交互、链慢或贵会怎样、Monad 能带来什么、是否真的需要链上记录。

## 核心收获

**1\. 为什么这个场景需要频繁交互**

-   点击游戏的核心循环是「点击 → 分数更新 → 排名变化」，单人每分钟几十次写入 + 排行榜近实时读——交互频率不是附加属性，而是玩法本身。
    

**2\. 链慢或手续费高会怎样**

-   以太坊主网级成本下，单次点击的 Gas 高于其娱乐价值，12 秒出块让实时竞争不成立；常见妥协是操作放链下、只结算上链——但那就退化成带钱包登录的 Web2 游戏。
    
-   上链的真实价值有三层：**信任**（排名由玩家自签名交易累加，运营方改榜需伪造签名）、**可组合**（成绩是公共资产，第三方可读作空投/准入凭证）、**防刷**（每次点击签名付 Gas，女巫攻击有了真实边际成本）。
    

**3\. Monad 能带来什么**

-   高吞吐 + 亚秒出块让「每次点击都是一笔真实链上交易」在成本和延迟上可行；EVM 完全兼容，开发成本与任何 EVM 链一致。
    
-   存储层契合是关键：分数存 `mapping(address => uint256)`，每个地址经 keccak256 映射到独立存储槽，1000 人同时点击就是 1000 笔零冲突交易；全局计数是热点槽反模式，聚合统计一律放索引层链下算——**为并行而设计存储布局**。
    

**4\. 是否真的需要链上记录**

-   诚实回答：榜单数据本身数据库完全够，上链的必要理由只有信任与可组合性；压测叙事、可复用索引器属于生态加分项，不能当上链的借口。
    

**5\. 落地设计要点**

-   索引器双订阅流：Proposed 流写 pending、Finalized 流转正，两个 cursor 独立推进；reorg 回滚必须以 block hash 为键——用 block number 会误删新链数据。
    
-   链上 Top-10 用长度 10 的有序数组插入排序维护：纯链上可验证，且该数组是唯一共享状态、仅高分交易写入，冲突可控——顺带成为讨论并行冲突的教学案例。
    
-   前端把三层状态直接映射成 UX：pending 分数半透明、Finalized 变实，并排展示「点击 → Proposed」与「点击 → Finalized」两个毫秒数。
    

## 个人思考

-   这次论证最大的收获是把「快」翻译成了机制：Monad 的优势不是 TPS 数字，而是存储槽隔离 + 乐观并行 + 三层确认共同作用后，某类过去不可行的产品形态变得可行。
    
-   「是否需要上链」这一问逼着我把信任/资产/成本三层拆开——防住了「为上链而上链」，也让 Demo 的技术叙事更站得住。
    
-   计划补一个热点槽对比实验（纯 mapping vs 加全局计数器两版合约的吞吐差异），用数据实证并行执行的价值。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

## **今日进度：BuildAnything 初中课程 3/13**

课程地址：[https://buildanything.so/zh/tracks/sophomore](https://buildanything.so/zh/tracks/sophomore)

学完初中前三课（Vibecoding 原理、Skills、EVM 原理），主线是给之前「会用」的东西打开引擎盖。

## **核心收获**

**1\. 编程智能体的分层结构**

-   所有编程智能体底层相同：模型 + 上下文窗口 + 系统提示 + 工具 + 智能体循环 + harness。出问题先问是哪一层，行为就从「魔法」变成「可调试」。
    
-   模型无状态，不在上下文里的东西对它就不存在——好的 vibecoding 本质是把智能体指向正确的文件。
    

**2\. 智能体循环与委托**

-   引擎是「思考 → 行动 → 观察 → 重复」直到任务完成；sub-agent 用独立上下文干脏活、只返回总结，保持主对话干净。
    
-   持久化靠 `CLAUDE.md` 和 memory；Skills / Hooks / `settings.json` 属于 harness 层；MCP 是接外部系统的标准插头。
    

**3\. Skills 与 Monskills**

-   skill 就是一份 markdown：description 决定何时触发，正文决定怎么做，把泛化助手变成领域专家。Monskills 六件套覆盖 Monad 构建全流程，明确要求助手不许凭空猜合约地址、私钥不落明文。
    
-   恶意 skill 可泄露私钥、跑任意脚本——安装前查作者、读源码，纪律同「不往终端粘贴陌生脚本」。
    

**4\. EVM 核心模型**

-   Ethereum 是分布式状态机：每笔交易触发一次**确定性**状态转换，所有节点独立执行、结果必然一致，trustless 的根源在此。
    
-   Solidity 编译成 opcode 执行；memory 交易内临时（便宜），storage 上链永久（贵，因为要写到全球所有节点）。
    

**5\. Monad 兼容性与合约安全**

-   Monad 跑同一个 EVM，工具链零修改，区别只在吞吐——「同一个 EVM，快得多的轨道」。
    
-   严肃警告：合约错误不可变、可能损失真金白银，AI 写 Solidity 比写前端更不可靠。持有真实资金的合约，未经审计绝不部署。
    

## **个人思考**

-   分层框架正好解释了我在 Claude Code 里做的配置：`settings.json`、CLAUDE.md 都是 harness 层，过去照文档配，现在知道每项作用在哪一层。
    
-   「恶意 skill」和我之前调 Raycast prompt 踩的 instruction-data boundary 泄漏是同类问题：文本会被当指令执行，信任边界就必须前置。
    
-   EVM 确定性状态转换是我做 indexer 的地基：节点重放结果必然一致，事件日志才配当唯一数据源。
    

明天继续初中第 4 课起。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

## 今日进度：BuildAnything 新生课程 10/10 完结 ✅

课程地址：[https://buildanything.so/zh/tracks/freshman](https://buildanything.so/zh/tracks/freshman)

学完 Monad 官方学习平台 BuildAnything 新生阶段全部 10 节课，路线是「先会做产品 → 理解链 → 把产品搬上链」，最终在 Monad Testnet 部署了第一个 dApp。

## 核心收获

**1\. Monad 是什么**

-   完全 EVM 兼容，核心指标：10,000 TPS、400ms 出块、800ms 确定性最终性。
    
-   去中心化不妥协的关键：`MonadBFT` 用 fan-out / fan-in 替代验证者 all-to-all 通信，通信开销线性增长，validator set 可以做大。
    

**2\. 区块链基础概念**

-   PoW 拼算力，PoS 拼抵押（作恶触发 slashing）——用财务风险替代电力消耗保证安全。
    
-   `gas fee = gas units × gas price`：计算量 × 区块空间供需。
    
-   实用四件套：Chain ID（Monad Testnet 为 10143）、RPC、Faucet（[faucet.monad.xyz](http://faucet.monad.xyz)）、Explorer（[monadscan.com](http://monadscan.com)）。实验一律 testnet。
    

**3\. 10k TPS 解锁什么**

-   关键不是数字，是确认时间 <1s 落进人脑「即时响应」感知窗口后，过去被迫绕开链的架构妥协可以取消：链上订单簿、链上游戏、社交图谱、微支付、AI Agent 非托管结算。
    
-   链上/链下的划分原则不变（无信任、永久性、可组合性才上链），变的只是线的位置。
    

**4\. Web2 基建与安全**

-   元数据进 database，文件字节进 storage，数据库只存路径。
    
-   安全铁律：`service_role` key 绝不进前端/Git；生产必开 RLS；永远不要相信客户端。
    

**5\. 第一个 dApp（MessageBoard）**

-   Hardhat 写合约 → 部署 Monad Testnet → wagmi `useReadContract` / `useWriteContract` 接前端。
    
-   最重的提醒：**绝不能让 AI 智能体接触私钥**（.env 都不够，要用加密 Secrets 管理）；AI 写 Solidity 远不如写 UI 可靠——合约保持小而简单、用经审计的库、先上 testnet。
    

## 个人思考

-   上周刚用 Remix 手写部署了 Soulbound ERC-721 徽章，这门课走 vibecoding + Hardhat 路径，两相对照能分清哪些是链的本质（私钥、gas、finality），哪些只是工具链选择。
    
-   「800ms 确定性最终性」正是我之前分析 Monad 三级区块状态（Proposed → Voted → Finalized）做 indexer reorg-safety 时那套模型的用户侧表述，前后串起来了。
    
-   课程反复强调的安全意识是个提醒：Web3 里很多安全责任从公司基建直接落到开发者个人，且错误不可逆。
    

明天进入 Sophomore 阶段。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

\## Week 1 打卡｜部署 NFTBadge 到 Monad Testnet

\### 今天做了什么

\- 用 Remix 编译并部署了一个 Soulbound（不可转让）ERC-721 徽章合约 `NFTBadge`，基于 OpenZeppelin v5

\- 网络：Monad Testnet（Chain ID 10143）

\- 合约地址`0xe9a15a7A91708765b6339dCeED7b89D2b3a3eb9D`

\- 完成了 read`owner()` 等）和 write`setBadgeTypeURI` / `mintBadge`）两类函数调用

\- 给合约配置了真实 IPFS metadata（Pinata 上传图片 + JSON），替换掉了最初的占位字符串

\- 整理了 README v0.1，写清楚了合约功能、部署步骤、交互方式

\### 踩过的坑

1\. **MetaMask 交易排队**：连续点击 write 按钮会攒一堆待确认请求，导致"点 A 方法却弹出 B 方法确认框"——本质是 nonce 顺序问题，不是合约或钱包故障，清空队列即可

2\. \*`mintBadge` revert\*\*：合约设计上"同一地址同一 `badgeType` 不能重复领取"，第二次拿同样参数铸造会被 EVM 回滚。解决方式是新开一个 `badgeType=2` 重新走 `setBadgeTypeURI → mintBadge` 流程

\### 关键技术点

\- **Soulbound 实现**：重写 OpenZeppelin v5 的 `_update` 钩子，只放行铸造`from==0`）和销毁`to==0`），拦截普通转账

\- **CEI 模式**`mintBadge` 里先完成状态写入（Effects）再调用 `_safeMint`（Interactions），降低重入风险

\- **IPFS metadata 流程**：先传图片拿 CID → 写入 JSON 的 `image` 字段 → 再传 JSON 拿最终 CID → 这个 CID 才是 `setBadgeTypeURI` 要填的值

\### 产出

\- 合约地址 + 部署 tx hash + 两笔交互 tx hash`setBadgeTypeURI, mintBadge`）

\- README v0.1（含部署信息、交互说明、安全设计说明）
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
# 2026-07-06

**今日学习内容**

**一、Web3 实习手册·入门导读**

**1\. 区块链基础概念**

-   区块链本质是按时间顺序连接的区块链条，每个区块包含交易记录及前一区块的哈希值，因此历史记录一旦写入便不可篡改
    
-   核心特性：数据不可篡改、公开透明但地址匿名、交易确认速度快
    
-   比特币：节点通过挖矿竞争记账权并获得代币奖励，具备货币属性——总量恒定、可自由转账
    
-   三种链的形态：
    
    -   公链：任何人可自由加入，完全开放
        
    -   联盟链：由多方共同组成"董事会"式治理，半开放
        
    -   私链：由单一主体审批控制，完全封闭
        
-   Web2、Web 3.0、Web3 的区别：
    
    -   Web2：中心化服务器架构，平台掌控用户数据
        
    -   Web 3.0：语义网概念，与区块链技术无关，容易混淆
        
    -   Web3：去中心化互联网，数据主权归还用户，智能合约自动执行规则，资产真正由用户自己掌控
        

**2\. 以太坊概览**

-   以太坊被称为"区块链 2.0"，由 Vitalik Buterin 于 2013—2014 年提出，核心创新是引入智能合约，愿景是打造"世界计算机"
    
-   与比特币的关键差异：
    
    -   图灵完备、可编程，而比特币是简单脚本
        
    -   目前采用 PoS 共识，而比特币是 PoW
        
    -   出块时间约 12 秒，而比特币约 10 分钟
        
-   核心机制：
    
    -   账户体系：EOA（私钥控制的外部账户）与 CA（合约代码控制的合约账户）
        
    -   Gas 模型：费用 = Gas Limit × Gas Price；EIP-1559 之后拆分为销毁的 Base Fee 与支付给验证者的 Tip
        
    -   EVM：全网节点同步执行智能合约的虚拟机环境
        

**3\. 行业赛道全览**

-   **DeFi**：Uniswap（基于恒定乘积公式 x\*y=k 的 AMM 自动定价机制）、Compound（超额抵押借贷，以 cToken 计息）、MakerDAO/Sky（超额抵押生成稳定币 DAI/USDS）
    
-   **NFT**：解决数字资产的唯一性与所有权确权问题，代表项目有 CryptoPunks（先锋项目）、OpenSea（最大交易平台）
    
-   **DAO**：以社区投票取代传统公司科层治理，代表案例有 Nouns DAO、LXDAO（支持 Web3 公共物品建设）；ConstitutionDAO 众筹竞拍美国宪法原稿未遂，反而暴露出 DAO 治理信息过度透明、不利于竞价类场景的短板
    
-   **MEME**：文化驱动型代币（如 DOGE、PEPE），投机性极强，需警惕无实际价值支撑的炒作、盲目跟风与代币集中度过高的风险
    
-   **交叉领域**：DeFi+NFT（NFT 抵押借贷）、DAO+MEME（社区治理与文化资产融合）、AI+DeFi（尚处早期）、RWA（现实资产上链）
    
-   **2026 新趋势**：Intent-Based 交易、账户抽象（ERC-4337）、模块化区块链、AI 与 Web3 深度融合
    

**二、实操**

-   创建了一个课程专用钱包，并添加 Monad Testnet 网络
    
-   领取测试网资产，完成一次测试网转账/应用交互实操
<!-- DAILY_CHECKIN_2026-07-06_END -->

<!-- DAILY_CHECKIN_2026-07-28_START -->
# 2026-07-28

## 今日主题

 

围绕 Parallax 完成产品范围、技术栈、Moss 依赖和钱包模拟边界的再确认，并发现产品决策文档与技术方案文档之间存在同步滞后。

## 今天完成了什么

### 1. 重新收敛产品 P0

Parallax 不做新的 DEX Aggregator、通用钱包安全扫描器或完整 DeFi Risk Dashboard，而是：

> 一个基于 Moss 的 Monad Swap 签名前解释与调整层：在用户签名前说明这笔交易会发生什么、哪里可能造成明显损耗或暴露，以及现在可以调整什么。

当前建议的 P0 闭环是：

```
输入 Swap Intent
→ 获取 Kuru Quote / Route
→ 构造并模拟 unsigned transaction
→ 展示资金变化、损耗、规则结果与证据
→ 修改金额或路线
→ 重新 Quote / simulate
→ 展示前后 Diff

```

主 Demo 使用 Kuru `MON → USDC`。PancakeSwap 默认降级为第二 Quote / Route Preview，只有真实 PoC、字段来源和失败隔离都通过技术闸门，才进入 P0 完整模拟。Approval 不再作为主流程硬依赖，因为原生 MON 输入通常不产生 ERC-20 Approval。

### 2. 明确当前技术栈

核心技术栈没有换，变化是 P0 范围做了减法：

* Node.js 22 + TypeScript + ESM + pnpm；
* Moss + viem：Moss 负责交易构造、模拟、Receipt 和 Evidence，viem 负责 RPC 只读辅助；
* Hono + zod：Hono 提供轻量 API，zod 校验请求和共享 Schema；
* Vite + React + TypeScript：单页强交互，不需要 SSR；
* `packages/contracts`：前后端、Moss Adapter 和 Risk Engine 的共享契约；
* `moss-bridge`：隔离 Moss 版本和原始证据；
* 纯函数 Rule Engine：输出 `PASS / WARN / FAIL / UNKNOWN / NOT_APPLICABLE`。

P0 暂不引入 Go、Next.js、SQLite、SSE、复杂任务队列和 Recharts 金额曲线。Session / 内存 Diff 足以支撑一次 Adjust & Re-run，SQLite 和完整 History 延后到 P1。

### 3. 澄清 Moss 的发布版与开发分支能力

当前 Moss 开发分支已经支持 `SimulatorOptions.stateOverrides`，可以通过 ERC-20 balance mapping slot 注入 synthetic balance；但这项公共 API 尚未进入 `origin/main`，不能直接假设已发布的 `@themoss/simulator@0.1.0` 支持它。

因此形成三个技术闸门：

* Gate A：使用发布版 Moss 完成 Kuru `MON → USDC` 真实 Quote、Action、Simulation 和 Receipt；
* Gate B：只有在 ERC-20 balance/allowance slot、锁定依赖和专项 Smoke Test 都通过后，才能把 ERC-20 输入升级为 P0；
* Gate C：Price Impact 需要多次 Quote 时，必须证明基准 Quote 和用户金额 Quote 的区块可比，否则返回 `UNKNOWN`。

Synthetic prestate 只能证明“在提供这组假设状态时交易会怎样”，不能证明真实钱包拥有余额、授权或支付能力。

### 4. 讨论钱包接入与“实际资金模拟”

连接钱包和使用实际资金模拟并不矛盾，但必须区分：

* **Demo Mode：** 固定 sender 或录制快照，保证演示稳定；
* **Wallet-aware Mode：** 读取用户地址、Chain ID、余额和 Allowance，以用户地址作为 `from` 做只读模拟；不签名、不广播、不扣款；
* **Synthetic Mode：** 用 `stateOverrides` 人工注入余额或授权，只能标记为 synthetic simulation。

当前 Moss simulator 会给 sender 预充原生 MON，因此模拟成功不一定代表用户实际余额足够支付。产品应显示 `AFFORDABILITY_UNKNOWN`，不能把“模拟成功”写成“用户现在一定付得起”。

当前 PRD 将钱包连接列为可选能力，团队还需要决定：P0 只做 Demo Mode，还是同时提供 Wallet-aware Mode。无论选择哪一种，产品文案都不应说“用真实资金试交易”，应说“基于钱包地址和当前链上状态进行签名前只读模拟”。

### 5. 理解 Hono 与 zod 的职责边界

Hono 是 API 入口，负责路由、读取请求和返回 JSON；zod 是运行时数据审核层，负责确认外部 JSON 是否符合 Intent、金额、滑点和路线规则。

例如 `POST /api/check`：

```
浏览器请求
→ Hono 匹配路由
→ zod 校验 JSON
→ Moss 构造并模拟
→ Risk Engine 判断
→ Hono 返回 Receipt / Warning / Verdict

```

TypeScript 类型只能在编译阶段约束代码，不能阻止用户在运行时发送错误 JSON。zod 需要把金额作为字符串校验，后端通过校验后再转换为 BigInt 或协议需要的格式。

### 6. 发现文档同步问题

今天确认了两篇 Notion 文档的职责不同：

* 第一篇负责仓库目录、Owner、Git/PR 协作规范和技术实现方案；
* 第四篇负责产品价值、P0 范围和技术闸门。

两篇不应该写成同一份文档，但第一篇的技术方案必须服从第四篇的产品决策。当前第四篇已经加入 Moss Gate A/B/C 和钱包接入议题；第一篇仍保留 SQLite、Job 查询、Recharts 和更完整双协议流程，需要后续同步。

## 今日关键结论

1. 核心技术栈没有改变，改变的是 P0 技术方案的复杂度。
2. Kuru `MON → USDC` 是发布版 Moss 基线下最稳的主 Demo。
3. ERC-20 override 是开发分支能力，不是当前已发布依赖能力。
4. 钱包只读接入与模拟不矛盾，但真实链上状态、synthetic state 和支付能力必须分别标记。
5. Simulation Success 只说明交易在特定状态下可执行，不等于安全、符合 Policy 或用户当前余额足够。
6. 产品 Scope 文档是上位约束，技术方案文档必须跟随 Scope 更新。

## 今日复盘

今天最大的收获不是记住了 Hono、zod 或某个 Moss API，而是认识到：技术方案真正危险的地方不是“框架选错”，而是产品承诺超过了证据能力。

“使用用户真实资金模拟”听起来很有吸引力，但如果实际使用的是 synthetic balance 或 simulator 预充的原生余额，就必须诚实地降低表述。只有把真实状态、合成状态、模拟区块和支付能力分开，Risk Guardrail 才不会变成另一种黑箱。

## 下一步计划

1. 团队决定 P0 是否包含 Wallet-aware Mode；
2. 完成发布版 Moss 下 Kuru `MON → USDC` Smoke Test；
3. 将第一篇技术方案同步到第四篇的 P0 和 Gate A/B/C；
4. 冻结 Intent、Receipt、Evidence、Verdict 和错误状态 Schema；
5. 单独验证钱包只读模式、真实余额/Allowance 读取和 `AFFORDABILITY_UNKNOWN` 展示；
6. 评审后由 Kai 更新 PRD v0.2 与 Decision Log。
<!-- DAILY_CHECKIN_2026-07-28_END -->

<!-- DAILY_CHECKIN_2026-07-29_START -->
# 2026-07-29

## 今日主题

 

根据团队会议反馈，重新收敛 Parallax 的产品定位、P0范围和核心交互流程。

## 今日完成

1. 明确产品面向普通或轻度链上用户，不优先面向 Agent、MCP 用户或专业机构。
2. 将产品核心从“完整风险报告”收敛为“签名前的交易模拟与决策辅助”。
3. 确定核心流程：`输入Swap参数 → Moss模拟 → 解释结果 → 给出建议 → 调整参数 → 重新模拟`。
4. 初步确定四种结果状态：
   * Proceed：建议继续。
   * Adjust：建议调整后重新模拟。
   * Stop：建议停止。
   * Unknown / Insufficient Evidence：证据不足，无法判断。
5. 明确P0边界：不签名、不广播、不托管，不处理真实签名后的交易失败。
6. 将P0主链路收敛为单向Swap，目标币暂定为USDC。
7. 整理完成 Week 3 的反馈改进提交文档，记录收到的反馈、修改前后对比和剩余问题。

## 今日学到

今天最重要的认识是：黑客松项目的关键不是堆叠功能，而是把一个核心问题做成完整闭环。

用户不一定需要阅读大量技术指标，他们更关心：

* 我这笔交易可能发生什么？
* 为什么模拟失败？
* 我应该改什么？
* 改完之后是否变好了？
* 现在到底应该继续还是停止？

因此，产品应该优先帮助用户完成一次可理解、可操作、可重复的决策流程。

模拟失败和真实交易失败必须严格区分。当前产品只负责模拟、解释和建议，不应该承诺能够检测或解决真实签名后的所有失败。

## 目前仍然存在的问题

* Moss SDK具体版本及双向Swap支持情况尚未确认。
* 起始币种和单向兑换到USDC的具体方向仍需确认。
* minimum received、经济边界、Price Impact是否进入P0尚未决定。
* Evidence Drawer展示哪些字段尚未定稿。
* Moss连接失败是否设置独立错误状态尚未确定。
* 还没有完成可稳定触发成功、失败、调整和重试的Demo案例。
* 当前主要完成了产品文档和流程收敛，真实前端页面仍未完成。

## 明日计划

1. 确认Moss SDK版本、链ID 143及Swap方向。
2. 确认前后端API边界和模拟结果数据格式。
3. 完善PRD中的主流程和四种结果状态。
4. 设计可重复的失败、调整和重新模拟案例。
5. 根据8月5日完成P0 MVP的目标，拆分个人阶段任务并开始每日同步。

## 今日反思

目前最大的风险不是技术难度，而是范围继续膨胀。如果继续加入真实钱包、双向Swap、Agent接入或签名后失败处理，P0很可能无法按时完成。

今天完成的是“方向收敛”，但还不是最终产品。接下来必须尽快把文档中的闭环转化为可操作的Demo，否则仍然停留在讨论层面。
<!-- DAILY_CHECKIN_2026-07-29_END -->

<!-- DAILY_CHECKIN_2026-07-31_START -->
# 2026-07-31

# 残酷共学打卡 · 2026-07-30

 

今天没有上课，进度来自黑客松团队会议：完成 Parallax P0 版 PRD 逐条评审并定稿，确定 API 契约、单链范围与 `amount_in` 语义，同步各人进度与时间线。

## 核心收获

**1. 把「不确定」显式写进产品语义，而不是藏起来**

* `PROCEED` 的对外含义严格限定为「在检查范围内未发现阻断证据」，不代表交易安全，更不构成推荐，这句话必须落到 UI 文案上。
* `UNKNOWN ≠ PASS`，不得自动放行；有 warning 的情况倾向归入 `ADJUST`，`PROCEED` 只留给全部检查通过的场景。
* 风控产品最容易翻车的地方，是用户把「我没查出问题」读成「我保证没问题」。

**2. 必填改可选，是在降低产品的输入门槛**

* 用户自定义安全阈值从主要规则改为可选项，UI 不强制输入，只有用户主动填写才会触发对应的规则分支。
* 参照的是正常交易里滑点/安全阈值本来就是选填的心智，不给用户凭空增加决策负担。

**3.** **`amount_in`** **的两套数：human 与 raw，换算归后端**

* 契约里定义两个字段：前端展示用 `1.25`，与 Moss 交互用按 `decimals` 换算后的 `1250000`。
* 谁来换算的结论是**后端**——单位换算属于业务逻辑，只有后端能提供安全支撑（可回链上校验），前端没有这个能力。
* 值得记一笔：AI 在设计方案时默认把换算推给了前端。它给出的方案能跑，但不安全。

**4. P0 的范围收敛**

* 链路只做单链 `MON → USDC`：文档层面看似双向，实测没有 USDC 的充值接口，双向测试要靠有真实余额的账户，成本太高。
* 接口从 3 个砍到 2 个：`check` 与 `replay`。
* 不做持久化数据库、不引入复杂队列、前端不直接依赖 Moss，单次流程必须 3 分钟内跑完。
* 验收标准锚在「至少跑通一条真实链路 + 至少一个真实案例产出有证据支持的调整建议」。

**5. 卡住开发的不是技术难度，是契约**

* verdict 规则文档和 `amount_in` 契约未定，三个开发（前端、后端、Moss）会同时卡住——这是今天暴露出的唯一真实瓶颈。
* 后端反馈：即便有 AI 辅助，实际耗时仍超预期，成本主要在「这里该怎么做更合适」的判断和跨人确认，而不在写代码。
* 共识是不要把问题攒到每晚的会上，卡住就立刻在群里问。

## 个人思考

上游接口「文档说支持、实测不支持」这件事，我在写 Pendle adapter 的时候遇到过一次，当时反馈了但没推进。这次的处理方式是：照样反馈，但**不把 P0 押在上游修复上**，直接把范围收敛到单链。赌别人的排期是最贵的赌注。

真正让我警惕的是 decimals 换算那一条。AI 给的方案完全能跑通，只是把业务逻辑放错了层。这类工程常识它不会自己补，得显式写进约束里——这和我在 `CLAUDE.md` / `AGENTS.md` 里沉淀协作norms 是同一件事，只是这次是被现场抓到的。

另外，同方向的竞品队伍已经有四支，其中一支的切入点（意图 / 语义 / 实际执行三者一致性）和我们最初的方向高度接近。差异点只剩「模拟失败后的调整与 rerun」，最后大概率是比完整度。

## 明日计划

* 与瑞对齐规则文档边界，力争明晚交付 verdict 规则第一版：`PROCEED` / `ADJUST` / `STOP` / `UNKNOWN` 的触发条件与参数映射。
* 修好仓库，上传 P0 版 PRD 与 API 文档。
* Review 后端提交的 contract PR，确认待定义项的标注方式。
* 定义用户侧关键词的展示语义（用户看到某个状态时，界面究竟该说什么）。
<!-- DAILY_CHECKIN_2026-07-31_END -->

<!-- DAILY_CHECKIN_2026-08-01_START -->
# 2026-08-01

# 残酷共学打卡｜2026-08-01

## 结论（置信度：高）

今天完成了 moss PR #109 的新一轮修复、复审与推送。代码已推进到 commit `6689dd4`，本地全部验证通过；PR 尚未合并，最新 CI 处于 `action_required`，需要 Owner 手动批准 workflow 后才能真正开始运行。

## 今日完成

最初任务名是“修复合并冲突”，但继续分析后发现，真正的阻塞已经变化：Owner 已完成 rebase 和旧 CI 问题修复，原先的本地实验代码也失去价值。今天的核心工作因此转为解决 Owner 最新提出的架构问题：

* 将 Pendle 官方 `Errors.json` 纳入可追溯、可离线复现的 ABI 生成流程。
* 删除手写的 revert selector 表。
* 让 Simulator 根据 `protocol + transaction target` 找到协议声明的 ABI，并通过 `decodeErrorResult` 解码 custom error。
* 将面向用户的错误解释放回 Protocol 元数据，避免 MCP 层积累 Pendle 专属知识。
* 补齐参数化错误、错误目标、畸形数据和未知 selector 的回退测试。
* 修正 ADR 0002，以及 ABI 文件的来源、哈希和链上验证说明。
* 经过多轮 Standards / Spec review，最终两条审查轴均为 0 finding。

完整通过的本地验证包括：

* 冻结依赖安装、lint、build、typecheck
* 全部 offline tests
* 严格 TLS 下的完整 live tests
* Pendle 155/155
* Monadscan ABI online gate 4/4
* `git diff --check`

最终通过普通 fast-forward push 将 PR head 更新到 `6689dd4`，没有 force-push，也没有代替本人发布 GitHub 评论。

## 今日最“残酷”的部分

真正困难的不是写代码，而是不断推翻已经不成立的前提。

一开始我把“新建分支”当作更稳妥的风险控制，但进一步确认后发现，Owner 已经替代了旧本地修改。关键并不是分支名字，而是新提交必须直接建立在最新远端 head 上。另一次，在 Monadscan online test 尚未执行时曾形成本地 commit，但“没有执行”不能算“通过”；因此撤销 commit、补齐 Keychain 中的 API key、跑完全部 gate 后才重新提交。

这两次纠偏让我更明确：工程可信度来自可验证的提交基线与完整测试证据，而不是流程看起来规范，也不是大多数测试已经通过。

## 今日收获

* 合并冲突不能只做文本拼接，必须确认双方语义都被保留。
* CI 历史失败不等于当前功能失败，要区分代码缺陷、secret 限制和运行环境问题。
* 手写 selector 是重复事实来源；ABI 才应该决定 error name、参数与 selector。
* 本地测试全绿仍不等于 PR 已完成：当前最后一道门是 Owner 批准 GitHub Actions workflow。

## 下一步

等待 Owner 点击 “Approve and run workflows”。CI 启动后只读检查结果；若全部绿色，再由我本人发布修复摘要并请求重新 review。
<!-- DAILY_CHECKIN_2026-08-01_END -->

<!-- DAILY_CHECKIN_2026-08-02_START -->
# 2026-08-02

# 🔥 残酷共学打卡 · 2026-08-02｜Parallax PR #4：Contract 信任边界与 reviewer 闭环

 

## 今日主题

围绕 [Parallax PR #4](https://github.com/parallax-monad/parallax/pull/4) 处理 reviewer 反馈与合约边界收紧，完成从“能序列化”到“可被信任地消费”的 Contract/API 校验闭环。

## 今日完成

1. 先处理 reviewer 要求的六项 Contract/API 边界修复：
   * `NOT_APPLICABLE` 只允许用于 `P0-ECONOMIC-001`。
   * 建立 RuleResult 与 Rule-bound Scope 的双向一致性校验。
   * `integration_error` 分支同样执行 Economic Boundary 一致性检查。
   * 强制 `proposedChange.before/after` 与 Action Verification Evidence 的 `beforeValue/afterValue` 匹配。
   * `irrelevantActions` 中的 Acceptance Boundary action 必须使用 `CHANGES_ACCEPTANCE_BOUNDARY_ONLY`。
   * Action ID 在整个 Run 内必须唯一，而不是只在单个 RuleResult 内唯一。
2. 根据后续 review，补上 Integration Error 的表示边界：当同一 Rule 的 Scope 是 `unknown / REQUIRED_CHECK_INTERRUPTED` 时，不允许同时存在对应的 RuleResult。该修复保持其他已计算完成的 UNKNOWN Rule 行为不变。
3. 再次收紧四个 P0 Contract 信任边界：
   * available Route 的 source 关闭为 `moss | quote | derived`，RPC Route 不能被当作可用 Route 序列化。
   * Economic PASS/FAIL 的模拟输出必须引用同 Run、trusted、`SIMULATE`、`generic` 且带明确 Simulation input role 的 Evidence；QUOTE Evidence 不能冒充 Simulation input。
   * Core Rule PASS/FAIL 的非 external Evidence 必须 confirmed、可复现，并具备 `runtimeVersion` 与不可变 `runtimeRevision`。
   * 增加 Route、QUOTE/无类型 Simulation input、缺失或不可复现 runtime provenance 的负向回归测试。
4. 先后完成并推送多个经过验证的提交，最终提交为 `1728d14 fix(contracts): tighten core evidence eligibility`，分支为 `feat/contracts-core`。
5. 最终验证通过：142 个测试、typecheck、lint、`git diff --check` 全部通过。

## 今日学到

今天最重要的认识是：Public Contract 不是“先把字段放进去，等真实 adapter 接上再补逻辑”的临时接口，而是消费者据以判断 PASS、FAIL、UNKNOWN 和 NO\_ROUTE 的信任边界。

真正危险的不是单个字段缺失，而是同一事实可以被两种互相冲突的方式表达。例如，一个被 `REQUIRED_CHECK_INTERRUPTED` 标记为尚未得到可信结果的 Rule，不能同时拥有一个可被消费的 UNKNOWN RuleResult；否则下游无法判断它是“已经完成并得出 UNKNOWN”，还是“根本没有完成”。

今天的 review 也再次证明，契约修复最有价值的产物不是 happy path，而是负向测试：明确哪些看似合理的 payload 必须被拒绝，才能让 fail-closed 语义真正落地。范围控制同样重要——可以先冻结 Route、Evidence、Scope 和 Runtime provenance 的边界，但不因此把真实 Moss adapter、Extractor 或 Fixture 偷偷塞进同一个 PR。

## 目前仍然存在的问题

* Live Moss/RPC activation 尚未完成。
* authoritative tokenOut extractor 尚未实现。
* 真实或脱敏 Fixtures、真实 adapter 仍属于后续集成工作。
* `MOSS_RUNTIME_REVISION` 的真实运行时注入与生产 activation 仍需由后续 owner 完成。
* PR #4 仍需 reviewer 最终复审与合并，Contract 修复完成不等于整条产品链路已经可用。

## 明日计划

1. 跟进 reviewer 对 `1728d14` 的复审结果。
2. 明确真实 Moss/RPC activation、tokenOut extractor 与 Fixture 的拆分边界。
3. 将已冻结的 Contract 字段映射到后续 adapter/API 实现，保持 runtime provenance 不被弱化。
4. 继续优先补充“应拒绝的 payload”测试，再扩展真实集成路径。

## 今日反思

今天处理的表面问题是 PR 合并冲突和 reviewer comment，实质问题是：系统是否允许一条没有充分证据的结论穿过公共边界。最有效的解决方式不是继续增加功能，而是把证据来源、阶段、可复现性、运行时身份和状态表示全部绑定起来，并在不满足条件时明确拒绝。

这次 PR 的关键进展不是“代码更多了”，而是系统更难被含糊的数据骗过。
<!-- DAILY_CHECKIN_2026-08-02_END -->

<!-- DAILY_CHECKIN_2026-08-03_START -->
# 2026-08-03

## 今日主题

围绕 PR8 与 PR9，完成 Parallax 的 Live Check 与 Recorded Replay 边界收敛、演示方案调整、Fixture 方案确认，以及 Contract 和 Fixture 所有权问题梳理。

## 今天完成了什么

### 1. 区分 Live Check 与 Recorded Replay

PR8 交付的是 `POST /api/check` 的 Live Check 后端边界：

* 接收并校验交易请求；
* 生成新的 `runId`；
* 调用 Agent Flow；
* 校验返回的 `RunResult`；
* 记录运行状态与失败原因。

PR9 交付的是 `GET /api/replay/:id` 的 Recorded Replay 边界：

* 读取已经冻结的 Fixture；
* 校验 `RunResult`；
* 确认 `replayMode=true`；
* 确认 Fixture provenance 匹配且不是 Mock；
* 返回稳定的历史结果。

Replay 不重新调用 Moss、RPC、Risk、Quote 或 Action，也不重新生成 Verdict。

### 2. 将演示方案从 Live 调整为 Fixture

最初计划使用 Moss 的实时数据完成演示，但考虑到当前开发进度、实时链路的不确定性以及现场演示的稳定性，本阶段采用 Fixture-based Recorded Replay。

当前使用两个 Kuru 方向的 Fixture：

* MON → USDC；
* USDC → MON。

两个 Fixture 都保守返回 `UNKNOWN`，不虚构 `PROCEED`、`ADJUST` 或 `STOP`。

这不是永久放弃 Live，而是先把稳定演示路径与实时运行路径解耦。

### 3. 锁定 Replay 的最小实现范围

本阶段明确不做：

* Live Check fallback；
* `USE_REPLAY` 或 `ReplayRef` 集成；
* 运行时 Moss、RPC、Risk、Quote、Action 调用；
* 运行时生成 Route、Scope、Rule、Summary 或 Verdict；
* SQLite 或其他生产数据库；
* Fixture 的生成、刷新、审核和过期生命周期；
* 完整 Evidence Drawer；
* 生产环境 server wiring。

首版采用文件型 Fixture，Replay API 只执行：

> 定位 Fixture → Schema 校验 → invariant 校验 → 原样返回。

### 4. 修正“后端可以全权决定”的认识

后端 Owner 可以独立推进：

* 路由；
* Fixture loader；
* Contract 校验；
* 错误处理；
* API 测试；
* Hono composition。

但后端不能单方面决定：

* 返回 `RunResult` 还是 `ReplayResponse`；
* 使用 `fixtureId` 还是 `runId`；
* Fixture 由谁生成、审核和维护；
* Evidence provenance 如何解释；
* 哪些 Rule、Verdict 和 reason code 可以被声明为可信。

技术实现可以并行推进，产品语义和数据责任不能被实现者默默冻结。

### 5. 识别当前实现与目标语义的差异

此前的 Replay 实现仍然会从 `chain-evidence` 动态拼装新的 `RunResult`，包括随机 `runId`、Route、Scope、Rule、Summary 和 Verdict。

这实际上是“基于旧 Evidence 重新投影”，不是忠实读取历史 Replay。PR9 已将方向调整为冻结完整 `RunResult` snapshot，避免当前规则或代码变化改写历史结果。

## 今日复盘

今天最大的收获是：Replay 的难点不是读一个 JSON 文件，而是守住“历史事实不可被当前逻辑重新解释”的边界。

为了赶进度可以减少技术依赖，但不能用减少确认来替代 Contract 设计。没有数据库不等于没有数据责任；Fixture 能被 API 读取，也不等于它的 provenance 已经被 Moss Owner 或风险语义 Owner 认可。

真正需要避免的不是代码写不出来，而是先做出一个语义错误、之后再被迫返工的 Replay。

## 下一步计划

* 建立 Replay Contract Decision Record；
* 确认 `fixtureId`、`runId` 与响应结构；
* 明确 Fixture 的生成、审核、版本化与失效责任；
* 请数据来源 Owner 确认 Quote、Action、Simulation 和 provenance 的可支持范围；
* 保持 PR9 的最薄读取路径；
* 将 `PROCEED`、`ADJUST`、`STOP` Fixture 作为独立 PR 处理。
<!-- DAILY_CHECKIN_2026-08-03_END -->

<!-- DAILY_CHECKIN_2026-08-04_START -->
# 2026-08-04

## 今日主题

 

今天把 Parallax API 的两条 P0/P2 主线收束并合并进 `main`：[PR #11](https://github.com/parallax-monad/parallax/pull/11) 把 `POST /api/check` 的 Re-run 父子 Run 关系、不可变 baseline、精确 Intent Diff 和机器可读拒绝原因写进契约；[PR #12](https://github.com/parallax-monad/parallax/pull/12) 补上可执行的 Node 后端运行时，让 Check 与 Recorded Replay 路由能在真实 listener 上启动和验证。两条 PR 都经过三位 Owner 分域 review，并在 review 反馈上迭代到 merge。

## 今日完成

1. **PR #11 — Re-run lifecycle contract**（merged 2026-08-04）
   * 在 `POST /api/check` 契约上增加可选 `parentRunId`，由 orchestrator 拥有 Re-run 资格判定与 Diff 构造。
   * 新增 `RerunRejectionReason` 稳定 enum，覆盖 parent missing、Replay parent、chaining、chain/sender 变更、Boundary 变更、no-op、多字段变更等八种拒绝场景。
   * 失败 child Run 保留 `parentRunId` 和 `diff`，Integration Error 不伪造 Rule Results / Evidence / Verdict。
   * 新增 ADR 0002，明确 `diff` 是机器可比的 normalized 序列化，不是用户展示模型。
   * 214 tests、typecheck、lint 全部通过；三位 Owner（@jzhao0、@antony819、@chin0312）全部 approve。
2. **PR #12 — P2 Node backend runtime**（merged 2026-08-04）
   * 新增 `bootstrapBackendApp()`、`startBackendServer()` 和 `tsx src/server.ts` 启动路径。
   * 启动前校验 `MONAD_RPC_URL`、`MOSS_RUNTIME_*`、`PARALLAX_TOKEN_REGISTRY_JSON`、`HOST`、`PORT`；空白 `PORT` 不再被 coerce 成 `0`。
   * Live Check 默认注入 `UnavailableAgentFlow`，返回 HTTP 502 + `error.code: "UNSUPPORTED"` + `verdict: UNKNOWN` + `retryable: false`。
   * 新增 CORS middleware、`.env` 加载、真实 Node listener integration test、CI integration test 路径。
   * 230 tests + integration test 全部通过；@antony819 与 @jzhao0 的 P1 review 项在 follow-up commit 中关闭。

## 今日学到

1. **Re-run 是 orchestrator 拥有的「单条件变更」契约，不是前端随意重试。** PR #11 把 Re-run 资格判定和 Diff 构造下沉到 `packages/orchestrator/application/rerun.ts`。一次 Re-run 只能改一个 normalized Intent 条件；父 Run 必须是 completed baseline，不能 chain child→child；Economic Boundary 不可作为 Re-run 条件；Recorded Replay 与 Check `RunStore` 隔离。
2. **前端分支必须依赖** **`reason`** **enum，不能匹配英文 message。** `@antony819` 在 PR #11 review 里指出：八种拒绝场景如果只靠 prose `message`，前端只能靠字符串匹配——一旦改文案或做 i18n 就会断。`reason` enum 才是跨语言、跨版本的安全接口。
3. **失败 child Run 也要保留证据链。** Re-run 过程中 Agent Flow 抛异常或返回无效响应时，child Run 仍以 `integration_error` / `UNKNOWN` 写入 Store，并保留 `parentRunId` 和 `diff`。失败不是空白，而是带上下文的 fail-closed envelope。
4. **P2 运行时的关键是「可启动 + fail-closed + Live/Replay 分离」。** PR #12 让 `pnpm start` 能跑起来，但 Live Check 明确 fail-closed；`UNSUPPORTED` 与 `AGENT_FLOW_ERROR` 的机器可读区分，决定了 UI 该显示「Live 尚未接入」还是「集成崩溃请重试」。Recorded Replay 只走 `/api/replay/:id`，绝不作为 Live Check fallback。
5. **Review 反馈把「契约」和「可运行」两条线都推到了可 merge 状态。** PR #11 确认 parent/child 所有权、Boundary 保持、Replay 隔离；PR #12 关闭 CORS、`.env` 加载、`UNSUPPORTED` 结构化错误、真实 env→listener 集成测试路径等 P1 项。

## 目前仍然存在的问题

* verified `ADJUST` Action Gate 与 `ActionGateAttestation` 尚未激活。
* run-by-id 持久化与 Session History 仍 outside scope，Previous/New 面板刷新后不可读。
* Live Moss/Kuru Agent Flow 尚未注入（PR #10 adapter 已就绪，P0 Live blocker 仍在）。
* RunDiff 字段命名与 future transaction-adjustment model 的对齐仍是非阻塞 follow-up。
* 生产部署、auth、rate limiting 尚未实现。

## 明日计划

1. 开始前端 adapter 接线：基于 `parentRunId`、`diff`、`error.reason` 实现 Re-run 分支逻辑。
2. 明确 run-by-id / Session History 的拆分边界与 timeline。
3. 跟进 Live Agent Flow 注入路径，保持 fail-closed 语义不被弱化。
4. 将 orchestrator Re-run 规则映射到 UI 的 Previous/New 面板设计。

## 今日反思

今天最大的变化，是从「写测试通过的 plumbing」变成「被三位不同视角 Owner review 并 merge 的共享契约」。

PR #11 的 review 让我意识到：API 设计里最容易被低估的不是业务逻辑，而是**消费方如何稳定分支**。PR #12 则把 fail-closed 从测试断言变成了可 `pnpm start` 验证的运行时行为。

两个 PR 也再次印证 Parallax 的分层：`orchestrator` 拥有 Re-run 规则，`apps/api` 拥有传输与 Store，`bootstrap` 拥有运行时组装，ADR 拥有边界声明。每一层都知道自己**不做什么**，这比单 PR 加功能更重要。

今天完成的是已 merge 的后端契约与运行时，不是 Live Check 端到端可用。真正的 Proof of Work 仍然是 Agent Flow 注入、前端 adapter 接线，以及 run-by-id 让 Previous/New 面板在刷新后仍可读。
<!-- DAILY_CHECKIN_2026-08-04_END -->

<!-- DAILY_CHECKIN_2026-08-05_START -->
# 2026-08-05

## 今日完成

1. 完成并提交 PR #14：接通 Backend-owned 的 Kuru Live Agent Flow 边界。
2. 根据 Review 意见补齐 runtime preflight、Git provenance 和文档说明。
3. 增加 `simulatorPinnedBlock` 证据链及 fail-closed 校验。
4. 通过 `pnpm test`（312 passed、2 skipped）、typecheck、lint 和 `git diff --check`。
5. PR #14 已完成 Review 并合并。

### 今日收获

今天最大的收获不是“把接口接通了”，而是进一步明确了什么才算真正的 Live 成功：

有 runtime 身份，不代表有真实模拟区块；\
有接口响应，不代表证据完整；\
测试通过，也不代表真实链上验收完成。

因此，缺少关键 provenance 时，系统必须返回 `UNKNOWN`、`STOP` 或错误，而不是用 Replay、Mock 或假数据制造成功。

### 今日反思

残酷地说，PR 合并只是 Backend 边界完成，不是整个 Live 目标完成。

当前 pinned Moss runtime 仍然缺少权威的 `simulatorPinnedBlock`，也无法完整解析 `FlipOrderUpdated`。本地没有配置真实 Moss/RPC 环境，现有测试主要依赖 deterministic fake bundle 和 recorded evidence。

所以今天交付的不是“真实 Live 已打通”，而是“系统已经不会轻易误报 Live 成功”。这很重要，但距离 P0 acceptance 仍有明显差距。

### 明日计划

1. 推进 Moss runtime 补齐 pinned block 和 receipt parser 能力。
2. 准备真实 Chain 143 的 smoke 环境。
3. 继续补齐 artifact digest、Action Gate 和 `ADJUST` 验证链路。
<!-- DAILY_CHECKIN_2026-08-05_END -->
<!-- Content_END -->
