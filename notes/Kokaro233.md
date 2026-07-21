---
timezone: UTC+8
---

# Caro

**GitHub ID:** Kokaro233

**Telegram:** 

## Self-introduction

我是fintech在读～希望在这次有所收获

## Notes

<!-- Content_START -->
# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
今日此静坐
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->

今天也是努力学习的一天喵
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->


# Kintsu Adapter 技术复盘

## Kintsu Adapter：从 Intent 到可验证 Stake Receipt

今天为 Moss 实现并提交了 Kintsu 流动性质押协议适配器。这个 PR 的重点不是简单封装一次合约调用，而是建立一条完整的 Agent 执行链路：

```
自然语言 Stake 意图
  → 参数验证与单位转换
  → 链上份额报价
  → 带滑点保护的未签名交易
  → debug_traceCall 模拟
  → 链上 Change 解析
  → 可验证的结构化 Receipt
```

相关链接：

-   Pull Request：[nishuzumi/moss#96](https://github.com/nishuzumi/moss/pull/96)
    
-   Onboarding Issue：[nishuzumi/moss#94](https://github.com/nishuzumi/moss/issues/94)
    
-   GitHub Profile：[Kokaro233](https://github.com/Kokaro233)
    
-   Package：`@themoss/protocol-kintsu`
    
-   Commit：`1adcaa2 feat(protocols): add Kintsu staking adapter`
    

### 1\. Protocol Surface

Kintsu 接收原生 MON，并铸造代表质押份额的 sMON。Adapter 注册为 `staking` 类协议，绑定 Monad 主网的 StakedMonad Proxy：

```
0xA3227C5969757783154C60bF0bC1944180ed81B9
```

对 Agent 暴露的 surface 为：

| 类型 | 方法 | 语义 |
| --- | --- | --- |
| Capability | stake | 存入 MON，向 receiver 铸造 sMON |
| Query | convertToShares | MON 数量 → 预计 sMON 份额 |
| Query | convertToAssets | sMON 份额 → 当前 MON 价值 |
| Query | balanceOf | 查询地址的 sMON 余额 |

这里刻意将只读报价与状态变更分开。三个 Query 返回 JSON-safe 数据，不产生交易；只有 `stake` 构建 Capability transaction。

### 2\. 参数模型与数值边界

`stake` 接收三个业务参数：

```
{
  amount: PositiveDecimalString,
  receiver: Address,
  slippageBps: BasisPoints.max(5000).default(50)
}
```

`amount` 使用人类可读的十进制字符串，进入 Adapter 后通过 `parseUnits(amount, 18)` 转换为链上整数。这样避免 JavaScript 浮点数参与资产计算。

Kintsu 的 `deposit` 参数使用 `uint96`，因此 Adapter 额外执行：

```
0 < amount ≤ 2^96 - 1
```

这层限制不能只依赖 ABI encoder。提前抛出 `ParameterError`，可以让 Agent 在交易构建阶段得到明确错误，而不是在更晚的编码或 RPC 阶段失败。

滑点使用 basis points：

```
1 bps = 0.01%
默认值 = 50 bps = 0.5%
最大值 = 5000 bps = 50%
```

### 3\. Stake Transaction Construction

Adapter 不直接把 `amount` 塞进 `deposit`。它先读取合约当前兑换率：

```
quotedShares = convertToShares(amount)
```

然后计算最低可接受份额：

```
minShares = floor(quotedShares × (10000 - slippageBps) / 10000)
```

最终生成一笔未签名交易：

```
deposit(minShares, receiver, { value: amount })
```

以测试中的离线报价为例：

```
amount          = 2 MON
quotedShares    = 1.8 sMON
slippage        = 50 bps
minShares       = 1.791 sMON
transaction.to  = Kintsu sMON Proxy
transaction.value = 2 MON
```

需要显式拒绝两个边界：

-   `quotedShares === 0`：协议报价无效，不能继续构建交易；
    
-   `minShares === 0`：整数除法后最低份额失去保护意义。
    

Capability 标记了 `fundOut` 与 `priceImpact` 风险，使上层 Agent 能知道该操作会转出原生资产，并受到链上兑换率影响。

### 4\. Receipt 的验证不变量

交易可以成功执行，不等于它一定符合 Agent 描述的意图。Kintsu Receipt 因此不是把日志格式化成一句话，而是对 simulation trace 建立证据约束。

一次有效的 Stake 至少需要三类 Change：

```
NativeTransfer：account → Kintsu，value = deposited MON
ERC20 Transfer：zero address → receiver，value = minted sMON
Deposit：staker = receiver，shares = minted sMON，value = deposited MON
```

Receipt parser 验证以下不变量：

```
nativeTransfer.to == KINTSU_SMON_ADDRESS
Transfer.from      == ZERO_ADDRESS
Transfer.to        == Deposit.staker
Transfer.value     == Deposit.shares
NativeTransfer.value == Deposit.value
```

因此，最终 outcome 不是根据某一个事件猜出来的，而是由资金流、ERC-20 mint 和协议 Deposit 三份证据共同确定：

```
type KintsuStakeOutcome = {
  operation: "stake";
  account: Address;
  receiver: Address;
  monAmount: string;
  sMonShares: string;
};
```

解析器还拒绝：

-   缺失 native transfer、mint 或 Deposit；
    
-   出现多个 native transfer 或多个 mint；
    
-   Change 来自非 Kintsu 合约地址；
    
-   无法由官方 ABI 严格解码的事件；
    
-   mint 数量、接收地址与 Deposit 不一致；
    
-   MON 转账数量与 Deposit 记录不一致；
    
-   不在允许集合内的额外协议事件。
    

可选的 `VirtualSharesSnapshot` 会被单独覆盖，但只接受一次。每一个输入 Change 都必须映射到 ReceiptChange，保证 simulation evidence 被完整消费，而不是只挑选有利事件生成结论。

### 5\. 复用 ERC20 Receipt，而不是重复实现

sMON mint 本质上是标准 ERC-20 `Transfer`：

```
Transfer(0x000...000, receiver, shares)
```

Kintsu Adapter 只负责识别该事件与 Deposit 的协议关系；具体 ERC-20 Change 的展示交给已注册的 `ERC20.changesReceipt`。这种 composition 避免协议包复制 ERC-20 解析逻辑，也让不同 Adapter 输出一致的 token evidence。

### 6\. ABI Provenance 与可复现生成

ABI 是 Adapter 与合约之间的类型边界。为了避免使用无法审计的手写片段，本次 vendored 完整 artifact 来自：

```
package: @water-cooler-studios/monad-contracts-core
version: 2.2.0
tarball SHA-256:
ff5532ff6daabc7d1bec798aa18485df3aa28ff6c6a67bb946ec2dae8f8688da
```

部署地址同时与 Monad 官方协议清单和 Kintsu artifact 中的 deployment 信息交叉确认。

包内区分了三层：

```
abis-src/StakedMonad.artifact   原始上游 artifact
abis-src/VENDOR.json            包版本、hash、vendor 日期
src/abis/staked-monad.ts        生成后的 typed ABI
```

`gen-abis` 可以完全离线地从 vendored artifact 重建 TypeScript ABI；`update-abis` 用于未来重新拉取和校验官方包。这样 review 不只检查生成结果，也可以追溯输入和生成路径。

### 7\. Verification Matrix

测试按不同风险层拆分：

| 验证层 | 覆盖内容 |
| --- | --- |
| Schema | amount、receiver、slippage 的类型、默认值与上限 |
| Transaction | to、value、function selector、minShares、receiver |
| Query | share quote、asset quote、balance 的 JSON-safe 输出 |
| Receipt positive | 四个有序 Change 被完整解析为 outcome |
| Receipt negative | 缺失 mint、错误 mint 数量、证据不一致时拒绝 |
| ABI surface | 所需函数与事件存在于生成 ABI |
| Type fixture | ProtocolRef、Handle 和 Receipt method 的编译期约束 |
| Mainnet E2E | bytecode、symbol、decimals 与只读报价 |
| Simulation E2E | 0.01 MON traceCall 无 warning，Receipt 完整覆盖 |
| Integration | MCP Server composition root 注册 Kintsu |

主网 E2E 不发送真实交易。它通过 Monad RPC 读取已部署 bytecode、合约 metadata，并使用 `debug_traceCall` 模拟 unsigned transaction。测试断言：

```
halted   == undefined
warnings == []
receipt.operation == "stake"
receipt.monAmount == "10000000000000000"
```

提交前完成了 lint、build、typecheck、offline test 和包含 Monad mainnet simulation 的完整测试。

### 8\. 为什么没有实现 Unstake

Kintsu 的退出不是 `unstake(amount)` 这种单交易语义，而是异步状态机：

```
requestUnlock → 等待 batch processing → redeem
```

如果在第一版中把它伪装成同步 Capability，会产生两个问题：

1.  Agent 可能把“已提交解锁请求”错误解释为“已经收到 MON”；
    
2.  Receipt 无法在同一次 simulation 中证明未来的 redeem 结果。
    

因此 PR #96 只实现原子化、可在当前状态下完整模拟和验证的 Stake。未来的 Unstake 更适合建模为多个 Capability，并显式暴露 pending 状态和可赎回条件。

### 9\. MCP Integration

协议包完成后还需要进入 Agent 的发现路径。MCP Server composition root 增加：

```
import * as kintsu from "@themoss/protocol-kintsu";

protocols: [system, erc, kuru, kintsu]
```

这一步使 Registry 可以发现 `kintsu/stake` 和三个 Query，并通过 MCP tools 将 schema 暴露给外部 Agent。如果只新增 package 而不注册 composition root，代码虽然存在，但运行中的 Agent 无法加载该能力。

### 10\. 本次贡献的技术结论

这个 Adapter 的核心并不是一行 `deposit()`，而是把协议语义压缩成一组可验证约束：

```
输入层：强类型参数 + uint96 边界
报价层：on-chain convertToShares
保护层：basis-point minimum shares
执行层：unsigned payable deposit transaction
证据层：native transfer + ERC20 mint + Deposit
解释层：exhaustive Receipt outcome
集成层：Registry + MCP discovery
```

PR #96 目前已提交并等待维护者 review。后续如果协议退出能力进入 scope，需要先设计异步生命周期和跨交易 Receipt 语义，而不是简单增加一个 `unstake` 方法。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->




# Week 2｜Role Log

## 一、本周完成的事情

本周我完成了一个运行在 Monad Testnet 上的链上抽签小游戏 **Monad Omikuji**。

目前已经实现：

-   链上抽签与七种签运结果
    
-   钱包连接及 Monad 网络切换
    
-   智能合约部署与抽签记录上链
    
-   邮箱 6 位验证码登录
    
-   Supabase 用户与签文数据库
    
-   钱包绑定、签文认领和跨设备同步
    
-   手机端与桌面端适配
    
-   签文收藏与分享
    
-   每个钱包每日抽签次数限制
    

项目不只是一个前端页面，而是打通了前端、钱包、智能合约、登录、数据库和线上部署的完整流程。

## 二、项目链接

-   线上体验：[https://monad-omikuji.vercel.app/](https://monad-omikuji.vercel.app/)
    
-   GitHub：[https://github.com/Kokaro233/monad-omikuji](https://github.com/Kokaro233/monad-omikuji)
    
-   测试网合约：[Monad Explorer](https://testnet.monadexplorer.com/address/0x4be10ce76e9698978afa2414a2b65b8ed771823b)
    

项目目前已通过 TypeScript 类型检查、7 项单元测试、生产环境构建和 Solidity 合约编译检查。

## 三、本周使用的工具

-   React、TypeScript、Vite：前端页面和交互
    
-   Solidity、Hardhat：智能合约开发与部署
    
-   wagmi、viem、RainbowKit：钱包及链上交互
    
-   Supabase：邮箱验证码、用户系统和数据库
    
-   Vercel：网站部署
    
-   GitHub：代码管理和公开作品记录
    
-   AI Agent：需求拆解、代码生成、排错、测试和重构
    

## 四、Prompt 记录

本周比较重要的 Prompt 思路包括：

1.  “把链上抽签小游戏拆分成前端、钱包、合约、登录、数据库、部署和测试模块，并为每个模块设计验收标准。”
    
2.  “使用 Supabase 实现邮箱 6 位验证码登录，同时检查前端流程、邮件模板、验证码有效期和错误提示。”
    
3.  “验证用户提交的抽签交易哈希，服务端重新读取 Monad 交易回执和 FortuneDrawn 事件，通过后再把签文写入数据库。”
    
4.  “检查合约是否存在权限控制、重入、tx.origin、未检查 call 返回值、状态机和随机数安全问题。”
    
5.  “修改完成后运行类型检查、测试、生产构建和合约编译，不要只根据页面效果判断是否完成。”
    

## 五、遇到的问题与解决过程

### 1\. 邮箱只收到登录链接，没有数字验证码

原因是本地配置不会自动修改 Supabase 云端邮件模板。

后来我在 Supabase 邮件模板中加入 `{{ .Token }}`，才完成了“获取验证码—输入验证码—验证登录”的完整流程。

### 2\. 钱包、网络和数据库状态不同步

移动端切换网络、游客记录、钱包记录和云端签文之间存在状态差异。

我重新区分了游客数据、钱包数据和经过链上验证的数据，并增加抽签前置检查和跨设备同步逻辑。

### 3\. 真实交易失败时容易被误判为成功

为了方便展示，项目存在 Demo 模式，但真实链上交易失败时不能自动显示为模拟成功。

因此我明确区分了 Demo 模式和真实模式，真实交易失败时必须显示错误。

### 4\. 生产构建包体积较大

当前项目已经可以正常构建，但钱包及多链依赖让主包体积偏大。这不会阻塞本周提交，但下一步需要通过动态加载和代码分割进行优化。

## 六、课程学习记录

### 运营与作品集

运营分享让我认识到，Web3 的账号、项目、推文、活动数据和公开贡献，本身就是作品集。

相比只在简历里写“会开发”，一个可以访问的 Demo、一段持续的 GitHub 提交记录和一个测试网合约更有说服力。

运营也不只是写文案，而是围绕用户、活动、数据和增长推动项目发展。未来我也希望为项目记录访问量、钱包连接率、抽签完成率、登录率、收藏率和分享率。

### Web3 安全

以前我认为 Web3 安全主要是不要泄露助记词、不要被诈骗。学习后发现，安全还包括：

-   不随便进行 Unlimited Approval
    
-   免费签名也可能包含恶意授权
    
-   Disconnect Wallet 不等于取消链上授权
    
-   合约必须进行权限控制
    
-   使用 `msg.sender` 而不是 `tx.origin`
    
-   遵循 Checks–Effects–Interactions
    
-   检查低级 `call` 的返回值
    
-   使用状态机保证业务状态互斥
    

这些内容也影响了我的项目设计。例如私钥和 Supabase `service_role` 密钥不能放进前端变量；客户端提交的签运结果不能直接相信，必须重新验证链上交易回执。

### AI 开发闭环

Cooper 老师的分享让我认识到，0→1 的重点是先做出可以验证核心价值的 Demo，1→100 才是把 Demo 变成稳定、安全、可维护的产品。

AI 可以帮助生成页面、接口、数据库和测试，但不能替代人的需求判断、验证和架构设计。比较合理的流程应该是：

**明确需求 → 拆分任务 → AI 执行 → 测试验证 → 检查结构 → 重构 → 继续迭代**

我也认识到 TDD 和 SDD 对 AI Coding 很重要。测试可以告诉 AI 什么叫“正确”，规范则可以防止 AI 在需求不清楚时直接生成错误实现。

## 七、我分享的认识

我把 Vibe Coding 能力分成了六个阶段：

1.  静态页面
    
2.  交互式前端
    
3.  全栈应用
    
4.  产品级应用
    
5.  工程化系统
    
6.  分布式系统
    

我的核心判断是：

**级别越高，AI 越像协作工程师，而不是人的替代者。**

AI 可以生成代码、配置和测试，但随着系统复杂度增加，人必须承担更多需求判断、数据设计、安全、性能、架构和线上问题处理责任。

按照这条学习路径，我本周的链上抽签项目已经从交互式前端进入了“全栈应用—产品级应用”的阶段，因为它包含数据库、登录、权限、钱包和真实用户数据。下一步需要继续补足自动化测试、监控、性能和安全，逐渐向工程化系统发展。

## 八、职业方向判断变化

本周之前，我觉得开发、运营和研究是三个比较独立的方向。

现在我的判断是：

-   我的主要方向是 **Developer**
    
-   运营能力可以帮助我表达项目价值、获取用户并进行数据复盘
    
-   研究能力可以帮助我理解协议、安全和底层架构
    

因此，我希望成为一名既能开发，又理解产品、运营和安全的复合型 Web3 Builder。

## 九、下一步计划

1.  补充登录、钱包绑定和抽签流程的自动化测试。
    
2.  继续检查 Supabase RLS、密钥和服务端验证流程。
    
3.  优化生产包体积与移动端体验。
    
4.  为项目建立访问、钱包连接、抽签和分享数据指标。
    
5.  在 X 或小红书发布项目复盘，积累公开 Proof of Work。
    
6.  跑通 MOS 示例，阅读 ADR，并寻找适合认领的 Issue。
    
7.  研究可验证随机数方案，理解测试网 Demo 与生产合约的区别。
    

## 十、本周总结

本周最大的收获，不只是完成了一个链上抽签小游戏，而是第一次把前端、智能合约、钱包、登录、数据库和部署连接成一个可以实际访问的系统。

我也开始意识到：AI 能够大幅提高开发速度，但真正决定项目质量的，仍然是人的需求描述、任务拆解、测试验证、安全意识和系统设计能力。

这次项目进一步确认了我的职业方向——以开发者为主线，同时持续补充运营表达、协议研究和安全能力。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->






今天给大家分享了：

| Level | 项目类型 | 难度 | 需要掌握的技术 | AI 能帮什么 | 人必须解决的问题 | 典型项目 | 应用 / 工具 | 官网 | 配置需求 |
| L1 静态页面 | 展示型网站 | ⭐ | HTML、CSS、JavaScript、Tailwind | 生成页面代码、修改样式、生成动画 | 审美、需求描述、简单修改 | 个人主页、Landing Page、宣传页 | Vercel | https://vercel.com | ✅ 需要配置（部署、域名、环境变量） |
| L2 交互式前端 | 有状态的Web App | ⭐⭐ | React/Vue、组件、Router、State、API 调用 | 生成组件、处理交互逻辑、调用接口 | 理解数据流、解决Bug、组织代码 | Chatbot、小工具、Dashboard | React / Vue | https://react.devhttps://vuejs.org | ⚙️ 本地配置（安装依赖、项目环境） |
| L3 全栈应用 | 前后端+数据库 | ⭐⭐⭐ | Node/Python 后端、REST API、SQL、数据库设计、CRUD | 生成接口、数据库操作、基础业务逻辑 | 设计数据结构、排查前后端问题 | 博客、笔记系统、CMS | Supabase | https://supabase.com | ✅ 需要配置（创建数据库、连接API、环境变量） |
| L4 产品级应用 | 多用户真实产品 | ⭐⭐⭐⭐ | Auth、JWT/OAuth、权限系统、文件存储、支付、第三方服务 | 快速生成登录、支付接入、业务功能 | 安全设计、用户隔离、异常处理 | SaaS、电商、社交平台 | Supabase AuthStripeOAuthRBAC | https://supabase.com/authhttps://stripe.comhttps://oauth.net | ✅ 需要配置（账号系统、API Key、权限规则、Webhook） |
| L5 工程化系统 | 可长期运行的软件 | ⭐⭐⭐⭐⭐ | Docker、Linux、CI/CD、Testing、Logging、Monitoring、Redis、消息队列 | 自动写配置、生成测试、辅助排错 | 架构设计、性能、安全、线上问题处理 | 企业系统、高流量应用 | DockerGitHub ActionsAWSGrafana | https://www.docker.comhttps://github.com/features/actionshttps://aws.amazon.comhttps://grafana.com | ⚙️+✅ 需要配置（服务器、部署流水线、监控系统） |
| L6 AI 协同开发 | AI 作为主要编码者 | ⭐⭐⭐⭐⭐⭐ | Cursor/Claude Code/Codex、Agent Workflow、代码审查、自动测试、Git 工作流 | 完成大量开发任务、重构代码、生成测试 | 定义架构、拆任务、审核AI 输出、防止项目失控 | 复杂 AI 产品、长期迭代项目 | CursorClaude CodeCodexGitHub Copilot | https://cursor.comhttps://www.anthropic.com/claude-codehttps://openai.com/codexhttps://github.com/features/copilot | ⚙️ 本地配置（安装工具、账号、模型权限） |
| L7 AI 软件工厂 | AI 自动生产软件 | ⭐⭐⭐⭐⭐⭐⭐ | Multi-Agent、自动部署、自动监控、自动评估、自动优化系统 | 从需求到代码到运维自动完成 | 目标制定、安全控制、系统治理 | AI 开发平台、自主运营系统 | LangGraphCrewAIAutoGenAI Agent 平台 | https://langchain-ai.github.io/langgraph/https://www.crewai.comhttps://microsoft.github.io/autogen/ | ✅ 需要配置（Agent 工作流、模型、工具链） |
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->







今天学习了运营向的小知识，还对智能合约有了一定的了解～

* * *

# **Week 2｜运营方向分享会笔记（LXDAO）**

## **一、Web3 为什么要运营 Twitter？**

老师认为，在 Web3 领域，**Twitter（X）更像是一张公开的职业名片**。

相比小红书，Twitter 更适合沉淀自己的技术、项目和思考过程，也是很多项目方和招聘方了解一个人的第一入口。

建议：

-   持续记录学习过程
    
-   分享项目、研究和思考
    
-   多参考往届学员的 Twitter
    
-   可以搜索「Web3 实习计划」等关键词寻找优秀案例
    

小红书则更适合分享学习经验、成长经历等内容。

* * *

## **二、账号本身就是作品集（Proof of Work）**

训练营很多宣传内容都是由学员完成，例如：

-   分享会宣传文案
    
-   活动主推文
    
-   社群公告
    
-   Twitter 推文
    
-   公众号文章
    

发布后官方会标注作者，因此这些内容未来都可以作为自己的作品集。

运营的不只是账号，更是在不断积累自己的 **Proof of Work**。

* * *

## **三、AI 已经贯穿整个工作流**

老师目前几乎所有工作都会使用 AI，包括：

-   文案
    
-   Proposal
    
-   调研
    
-   主持稿
    
-   活动策划
    
-   图片制作
    
-   社区内容
    

常用组合：

-   GPT：生成内容、整理思路
    
-   Claude：交叉验证、补充
    
-   Twitter AI：搜索最新社交媒体信息
    

如果遇到陌生问题，会同时询问多个模型，再根据自己的判断整合。

AI 负责提高效率，人负责决定方向。

* * *

## **四、社区内部 AI Agent**

LXDAO 社区内部已经部署了自己的 AI Agent，并连接到 Notion。

因此在 Telegram 中直接 @Agent，就可以完成很多工作，例如：

-   写活动文案
    
-   写 Proposal
    
-   整理会议内容
    
-   修改 Notion 页面
    
-   查询历史活动资料
    

相比每次重新向 GPT 解释背景，这种内部 Agent 已经拥有团队历史知识，因此生成内容更加准确。

* * *

## **五、运营到底负责什么？**

运营并不仅仅是写文案。

老师介绍，目前运营工作包括：

-   活动策划
    
-   社区维护
    
-   嘉宾沟通
    
-   Builder 对接
    
-   Proposal
    
-   主持稿
    
-   分享会流程
    
-   Hackathon 策划
    
-   数据统计
    
-   活动复盘
    
-   社媒运营
    

本质上就是推动整个项目持续向前。

* * *

## **六、活动内容会持续调整**

课程内容并不是开始就全部确定。

例如最近有同学提出：

希望邀请 Web3 法律方向嘉宾。

运营团队便开始联系新的分享嘉宾，并调整下一周课程。

整个训练营会不断根据 Builder 的反馈优化内容。

* * *

## **七、运营最终都会回到数据**

老师提到，每场活动都会统计数据，例如：

基础数据：

-   Twitter 浏览量
    
-   Twitter 粉丝增长
    
-   TG 群人数
    
-   微信群人数
    
-   公众号阅读量
    

活动数据：

-   报名人数
    
-   到场人数
    
-   Builder 数量
    
-   Hackathon 提交项目数
    
-   Offer 数量
    
-   最终就业人数
    

这些数据既用于复盘，也能证明活动效果。

* * *

## **八、数据也是自己的作品集**

运营最终需要用数据证明价值。

例如：

-   一篇推文获得 12000 次阅读
    
-   一场分享会报名 800 人
    
-   一场 Hackathon 收到 100 个项目
    

相比一句”我会运营”，这些数据更有说服力。

* * *

## **九、如何统计用户来源？**

目前最简单的方法是在报名表增加渠道统计，例如：

你是通过什么渠道了解到本次活动？

可选项包括：

-   Twitter
    
-   Telegram
    
-   小红书
    
-   微信群
    
-   朋友推荐
    
-   其他
    

如果以后规模更大，还可以配合邀请码、不同推广链接等方式统计转化。

* * *

## **十、Web3 常用协作工具**

老师建议尽快熟悉以下工具：

-   Twitter（X）
    
-   Telegram
    
-   Zoom
    
-   Google Meet
    
-   Notion
    
-   Google Drive
    
-   Gmail
    

这些基本已经成为 Web3 团队默认的办公环境。

* * *

## **十一、运营可以细分很多方向**

运营并不是单一岗位，而是包括很多不同方向，例如：

-   内容运营
    
-   社区运营
    
-   活动运营
    
-   社媒运营
    
-   用户运营
    
-   渠道运营
    
-   BD（商务合作）
    

虽然工作不同，但核心都是围绕用户、活动和增长展开。

* * *

## **十二、技术 + 运营，更具竞争力**

老师最后提到，未来如果既懂技术，又懂运营，会比只会其中一个方向更有优势。

因为既能够理解开发，也能够理解产品和用户，在团队中的价值通常会更高。

* * *

# **Week 2｜Web3 事故急救室（Yoona）**

## **一、助记词 = 钱包最高权限**

钱包助记词和私钥永远不能告诉任何人，包括所谓的官方客服。

只要别人拿到助记词，就可以在自己的设备恢复你的钱包，直接控制全部资产。

**记住：**

-   官方不会索要助记词
    
-   助记词 ≠ 登录验证码
    
-   助记词 = 钱包所有权
    

* * *

## **二、助记词泄露后要立即换钱包**

即使发错群后马上撤回，也不能继续使用原来的钱包。

因为你无法确定是否已经有人保存了图片。

正确做法：

-   新建钱包
    
-   尽快把资产全部转走
    
-   放弃旧钱包
    

不是等资产丢了再处理，而是默认这个钱包已经不安全。

* * *

## **三、不要无限授权（Unlimited Approval）**

很多空投网站会要求：

授权 Unlimited USDC

这种授权意味着：

以后它可以持续使用你的代币额度。

正确做法：

-   先确认网站真实性
    
-   只授权需要的额度
    
-   不使用 Unlimited Approval
    

原则：

**需要多少，授权多少。**

* * *

## **四、免费签名也可能有风险**

很多网站提示：

仅验证身份，不消耗 Gas。

但实际上签名内容可能包含恶意授权。

如果看不懂签名内容：

直接拒绝。

不是所有不用 Gas 的操作都是安全的。

* * *

## **五、地址投毒（Address Poisoning）**

攻击者会制造：

开头一样、结尾一样的钱包地址。

如果直接复制历史记录里的地址，很容易转错。

正确流程：

-   完整核对地址
    
-   大额转账前先小额测试
    
-   不要只看前后几位
    

* * *

## **六、断开钱包 ≠ 取消授权**

很多人误以为：

Disconnect Wallet 就等于取消授权。

实际上：

钱包断开只是断开当前网页连接。

真正的授权仍然保存在链上。

正确做法：

进入授权管理页面（Approval Checker）

撤销对应 Token 的授权。

* * *

# **合约安全**

* * *

## **七、Mint 函数必须做权限控制**

如果任何人都可以调用：

```
mint(address,uint256)
```

那么任何人都可以无限增发代币。

解决方法：

-   onlyOwner
    
-   Access Control
    
-   Mint 上限
    
-   状态控制
    
-   多签管理
    

前端隐藏按钮没有意义。

因为别人可以直接调用链上函数。

* * *

## **八、重入攻击（Reentrancy）**

错误流程：

```
先转账
↓
再清余额
```

攻击者可以：

收到钱后再次调用退款函数。

于是：

第一次余额还没清零。

第二次又继续退款。

正确流程：

```
先更新余额
↓
再转账
```

即：

Checks → Effects → Interactions

必要时再加入：

Reentrancy Guard。

* * *

## **九、不要用 tx.origin 做权限判断**

错误：

```
tx.origin == owner
```

因为：

攻击者可以诱导管理员调用恶意合约。

最终：

恶意合约仍然可以借用管理员身份。

正确方式：

```
msg.sender
```

再配合：

-   Access Control
    
-   Role 权限管理
    

只判断当前调用者是谁。

* * *

## **十、call() 一定要检查返回值**

低级调用：

```
call(...)
```

会返回：

```
success
```

如果：

调用失败，

但是没有检查 success，

系统仍然可能继续执行后面的逻辑。

例如：

订单已经失败，

页面却显示：

已完成。

所以必须：

```
检查 success
失败立即处理
```

* * *

## **十一、众筹状态必须互斥**

众筹达到目标后：

应该进入：

```
Success
```

此时：

项目方可以提款。

用户不能继续退款。

如果没有状态限制：

项目方还没提款，

用户先退款，

余额又跌回目标以下。

整个合约逻辑就乱掉了。

因此应该增加状态：

```
Funding
↓

Success
↓

Paid
```

不同状态：

只允许对应函数执行。

* * *

# **今天记住的几个关键词**

```
助记词永远不能泄露
↓

Unlimited Approval 不要随便授权

↓

Disconnect ≠ Cancel Approval

↓

Mint 必须权限控制

↓

先改状态，再转账（Reentrancy）

↓

msg.sender > tx.origin

↓

call() 必须检查 success

↓

状态机（State Machine）保证业务互斥
```

* * *

## **今天最大的收获**

虽然前半部分主要是钱包安全，但后半部分第一次系统接触到了智能合约安全。

以前觉得 Web3 安全就是”不要被骗”，现在发现真正的安全还包括**权限控制、状态管理、重入攻击、低级调用检查等大量智能合约设计问题**。

这些问题很多都不是用户操作导致，而是开发者设计合约时留下的漏洞，因此以后如果自己写合约，也需要从一开始就考虑安全性，而不是最后再补。
<!-- DAILY_CHECKIN_2026-07-16_END -->
<!-- Content_END -->
