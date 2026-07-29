- GitHub ID: 189445912
- Name: Kokaro233
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-27
<!-- DAILY_CHECKIN_2026-07-27_START -->
# 2026-07-27

复习期末中——推进了黑客松进度
<!-- DAILY_CHECKIN_2026-07-27_END -->

# 2026-07-25
<!-- DAILY_CHECKIN_2026-07-25_START -->
# 2026-07-25

非常的好！！！
<!-- DAILY_CHECKIN_2026-07-25_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
# 2026-07-24

今天完成了黑客松的demo
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# 2026-07-23

今天也在好好学习！
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
# 2026-07-22

非常好非常好
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
# 2026-07-21

今日此静坐
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->
# 2026-07-20

今天也是努力学习的一天喵
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
# 2026-07-19

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
# 2026-07-18

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
# 2026-07-17

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
# 2026-07-16

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

<!-- DAILY_CHECKIN_2026-07-06_START -->
# 2026-07-06

**区块链实操笔记：钱包、测试币、转账、合约部署**

-   **任务一：创建钱包** — 安装 **MetaMask** 浏览器插件，设置密码并保存私钥，添加 Mode 测试网网络，通过水龙头领取 5 个测试币
    
-   **任务二：首笔交易** — 从谷歌浏览器钱包向火狐浏览器钱包转账 1 个测试币，在区块链浏览器查询交易哈希，确认余额从 5 变为 6
    
-   **任务三：智能合约** — 用 ChatGPT 生成留言板合约，导入 Remix 自动编译，连接 MetaMask 部署至 Mode 测试网，成功写入并查询留言数据
    

**一、整体流程**

```text
安装 MetaMask
↓
创建钱包
↓
保存助记词 / 私钥
↓
添加 Mode Testnet 测试网
↓
复制钱包地址
↓
去 Faucet 领取测试币
↓
用区块链浏览器查询钱包余额
↓
进行第一笔测试转账
↓
复制交易 Hash
↓
在区块链浏览器查看交易详情
↓
用 AI 生成 Solidity 留言板合约
↓
打开 Remix
↓
创建 .sol 文件
↓
粘贴代码
↓
编译合约
↓
连接 MetaMask
↓
部署合约
↓
调用函数进行留言
↓
查看留言记录和交易 Hash
```

**二、准备钱包**

**工具：MetaMask**

操作顺序：

```text
浏览器打开插件商店
↓
搜索 MetaMask
↓
安装扩展程序
↓
创建新钱包
↓
设置密码
↓
保存助记词 / 私钥
↓
进入钱包首页
```

重点：

| 项目 | 作用 |
| --- | --- |
| 钱包地址 / 公钥 | 接收测试币、查询交易、连接合约 |
| 私钥 / 助记词 | 控制钱包资产，不能泄露 |
| 密码 | 打开本机 MetaMask 用 |

注意点：

```text
钱包地址可以复制给别人
私钥和助记词不能发给任何人
测试钱包也建议单独创建，不要用主钱包
```

**三、添加测试网络**

**网络：Mode Testnet**

操作顺序：

```text
打开 MetaMask
↓
点击网络选择
↓
添加网络
↓
选择 / 搜索 Mode Testnet
↓
确认添加
↓
切换到 Mode Testnet
```

如果钱包里默认没有 Mode Testnet，就需要手动添加网络。

添加成功后，MetaMask 顶部网络名称应显示为：

```text
Mode Testnet
```

这一步的作用：

```text
让钱包连接到测试链
后面的测试币、转账、合约部署都在这个测试网完成
```

**四、领取测试币**

**工具：Faucet 水龙头**

操作顺序：

```text
打开 Mode Faucet 页面
↓
复制 MetaMask 钱包地址
↓
粘贴到 Faucet 输入框
↓
点击领取
↓
等待到账
↓
回到 MetaMask 查看余额
```

关键词：

| 词 | 意思 |
| --- | --- |
| Faucet | 免费领取测试币的网站 |
| Test Token | 测试币，不是真钱 |
| Mode Testnet ETH | Mode 测试网上使用的测试币 |

注意点：

```text
Faucet 一般有限制
可能一天只能领一次
测试币只能用于测试，不能当真钱使用
```

**五、用区块链浏览器查钱包**

**工具：Mode Block Explorer**

操作顺序：

```text
复制钱包地址
↓
打开 Mode 区块链浏览器
↓
把钱包地址粘贴到搜索框
↓
查看钱包余额和交易记录
```

能看到的信息：

| 信息 | 作用 |
| --- | --- |
| Balance | 钱包余额 |
| Transactions | 交易记录 |
| From | 发送方 |
| To | 接收方 |
| Value | 交易金额 |
| Gas Fee | 手续费 |
| Status | 成功 / 失败 |

这一步的意义：

```text
确认测试币是否到账
确认钱包是否已经在测试网上产生记录
```

**六、完成第一笔测试转账**

操作顺序：

```text
准备两个钱包地址
↓
打开 MetaMask
↓
点击 Send
↓
粘贴接收方钱包地址
↓
输入测试币数量
↓
确认 Gas Fee
↓
点击确认发送
↓
等待交易成功
```

发送成功后：

```text
复制交易 Hash
↓
打开区块链浏览器
↓
搜索交易 Hash
↓
查看交易详情
```

交易详情重点看：

| 字段 | 看什么 |
| --- | --- |
| Transaction Hash | 交易唯一编号 |
| Status | 是否成功 |
| From | 哪个地址发出 |
| To | 哪个地址收到 |
| Value | 转了多少测试币 |
| Gas Fee | 花了多少手续费 |
| Timestamp | 交易时间 |

实操理解：

```text
转账不是只改余额
而是在链上生成一条永久记录
交易 Hash 就是这条记录的编号
```

**七、Gas Fee 操作理解**

每次链上操作都可能需要 Gas。

需要 Gas 的操作：

```text
转账
部署智能合约
写入留言
修改链上数据
调用需要改变状态的函数
```

通常不需要 Gas 的操作：

```text
单纯查询余额
读取留言
查看交易记录
查看合约数据
```

Gas Fee 公式：

```text
Gas Fee = Gas Used × Gas Price
```

实操判断：

```text
操作越复杂
写入的数据越多
Gas 可能越高
```

比如留言板：

```text
留言越长
链上需要存的数据越多
Gas 可能更高
```

**八、用 AI 生成智能合约**

任务：生成一个简单留言板合约。

可以给 AI 的 prompt：

```text
Generate a simple Solidity smart contract for a message board.
The contract should allow users to post messages, store the sender address, message content, and timestamp, and allow users to read all messages.
Use Solidity version ^0.8.0.
```

生成后需要保存的是：

```text
Solidity 合约代码
文件后缀 .sol
```

合约功能一般包括：

| 功能 | 作用 |
| --- | --- |
| postMessage | 写入留言 |
| getMessage | 查询单条留言 |
| getAllMessages | 查询全部留言 |
| messageCount | 查看留言数量 |

**九、打开 Remix 部署合约**

**工具：Remix IDE**

操作顺序：

```text
打开 Remix
↓
进入 File Explorer
↓
创建新文件
↓
文件名：MessageBoard.sol
↓
粘贴 AI 生成的 Solidity 代码
↓
保存
```

编译：

```text
点击 Solidity Compiler
↓
选择对应 Solidity 版本
↓
开启 Auto Compile
↓
或点击 Compile MessageBoard.sol
↓
确认没有红色报错
```

部署：

```text
点击 Deploy & Run Transactions
↓
Environment 选择 Injected Provider - MetaMask
↓
MetaMask 弹出连接确认
↓
确认连接
↓
确认网络是 Mode Testnet
↓
点击 Deploy
↓
MetaMask 弹出交易确认
↓
确认 Gas Fee
↓
点击 Confirm
↓
等待部署成功
```

部署成功后会出现：

```text
Deployed Contracts
```

这说明合约已经部署到测试网上。

**十、和合约交互**

留言操作：

```text
展开 Deployed Contracts
↓
找到 postMessage / addMessage 函数
↓
输入留言内容
↓
点击 transact
↓
MetaMask 弹出确认
↓
确认 Gas Fee
↓
点击 Confirm
↓
等待交易成功
```

查询留言：

```text
点击 getAllMessages
或输入 index 查询 getMessage
```

如果第一条留言编号是 0：

```text
第一条留言：0
第二条留言：1
第三条留言：2
```

原因：

```text
数组通常从 0 开始计数
```

留言成功后，可以看到：

| 数据 | 意思 |
| --- | --- |
| sender | 留言的钱包地址 |
| message | 留言内容 |
| timestamp | 留言时间 |
| index | 留言编号 |

**十一、查看合约交互记录**

留言之后也会产生交易 Hash。

操作顺序：

```text
复制交易 Hash
↓
打开区块链浏览器
↓
搜索 Hash
↓
查看交易详情
```

重点看：

```text
Status 是否成功
From 是哪个钱包
To 是哪个合约地址
Gas Fee 花了多少
Input Data / Logs 是否有合约交互记录
```

这里和普通转账不同：

```text
普通转账：From 钱包 → To 钱包
合约交互：From 钱包 → To 合约地址
```

如果留言没有转币：

```text
Value 可能是 0
但 Gas Fee 仍然会产生
```

因为链上写入数据本身就要成本。

**十二、实操检查清单**

完成这节后，应该留下这些东西：

| 检查项 | 是否完成 |
| --- | --- |
| MetaMask 钱包创建成功 |   |
| Mode Testnet 添加成功 |   |
| 测试币领取成功 |   |
| 钱包地址能在浏览器查到 |   |
| 完成一笔测试转账 |   |
| 保存转账 Hash |   |
| 能看懂 From / To / Value / Gas Fee |   |
| AI 生成 Solidity 合约 |   |
| Remix 编译成功 |   |
| MetaMask 成功连接 Remix |   |
| 合约部署成功 |   |
| 留言函数调用成功 |   |
| 留言交易 Hash 可查询 |   |
| 能读取留言内容 |   |

**十三、这节实操的本质**

```text
钱包负责身份
测试币负责支付操作成本
测试网负责练习
转账负责产生第一条链上记录
区块链浏览器负责查询记录
智能合约负责执行链上程序
Remix 负责编译和部署合约
Gas Fee 负责支付链上操作成本
Transaction Hash 负责定位每一笔链上操作
```

最重要的实操逻辑：

```text
只要是写入区块链的操作
通常都要钱包确认
通常都会消耗 Gas
通常都会生成 Transaction Hash
通常都能在区块链浏览器查到
```

* * *

* * *

* * *

**（以下笔记从notion粘贴）**

**海老师分享会**

**blog制作：**

“让他（ai）去学习一下怎么做个人网站，然后会把我的简历丢给他，然后他就会给我做一版框架出来，然后再根据我的想法来跟他慢慢提。就比如说。我是目前是我，当我现在这个个人网站，它其实当时最开始的时候，什么都没有的，就是。黑字，白底黑子，什么都没有，然后是让他一步一步来，按照我的想法来做一个，其实现在都还没优化好这边文字，这些内容还没有优化。”

**ai工作流：**

“cloud code，它适合小白去写代码，因为它可以。  
你只需要给他一个想法，他能把整体的框架给你跑出来，但是你让他去做细节的东西。  
Cloud是不如codex codex，它就适合去做一些细节的，你给它举指定具体的任务，他去完成。  
然后gemini呢，他就比较适合做ui。  
我先喝口水了。  
就比较适合做ui，然后我就会让他们三个。  
放到一起，放到一个工作流里面放到一个workflow里面，然后让。codex和cloud code，他们两个相互来调用，就比如说这个项目。  
因为有他们会有一个问题的一个形式。然后比如说这个。  
我这根据这些问题，我会判断这个。  
开发的复杂程度，然后复杂程度比较高的。  
就是比较繁琐，就会让Claudecode去做，然后一些比较细节方面就会让。就会让codex去，然后一些ui设计上面就会让它gemini去。”

当下web3，base **新加坡** 会大于 香港

to-do list

1.  去github收藏xiaohai老师的“三部曲”
    
2.  对这个profile的blog的前端风格设计很感兴趣呀
    
3.  它的my cube -ai工作流我也是很感兴趣等会去看看
    
    [截屏2026-07-06 19.18.00.png](https://us04file-paa.zoom.us/file/NzsNXPNAS_mPjDebOpfuJg?filename=%E6%88%AA%E5%B1%8F2026-07-06%2019.18.00.png&jwt=eyJhbGciOiJFUzI1NiIsImsiOiJ2dC8rcFVJKyJ9.eyJpaWMiOiJ1czA0Iiwib3JpIjoibHlueC1pbnRlcmFjdGlvbiIsImhkaWciOmZhbHNlLCJkaWciOiI3YTI0ZmE4NDhlNzQwYTNhYTNlMGMwNWZmYWZkNjkyZWMxOWFjNGM0YjUzZTRlNTBhODI1N2U4MzM0YmM5MzcyIiwiaXNzIjoiZmlsZSIsImF1ZCI6InpmcyIsImV4cCI6MTc4MzMzODc3NywiaWF0IjoxNzgzMzM3ODc3fQ.ppc7H28CrRo2o6m_bYt_G14y1I_Yzb8uSH_HjELm-P7xmE85A-0HIMnfPCA7eEIkDlvsG2GaRikTd3rjx7Bpmg&response-cache-control=public%2C%20max-age%3D7776000%2C%20immutable&response-no-vary-search=key-order%2C%20params%3D%28%22jwt%22%20%22Policy%22%20%22Signature%22%20%22Key-Pair-Id%22%20%22verify%22%29&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly91czA0ZmlsZS1wYWEuem9vbS51cy9maWxlL056c05YUE5BU19tUGpEZWJPcGZ1Smc~ZmlsZW5hbWU9JUU2JTg4JUFBJUU1JUIxJThGMjAyNi0wNy0wNiUyMDE5LjE4LjAwLnBuZyZqd3Q9ZXlKaGJHY2lPaUpGVXpJMU5pSXNJbXNpT2lKMmRDOHJjRlZKS3lKOS5leUpwYVdNaU9pSjFjekEwSWl3aWIzSnBJam9pYkhsdWVDMXBiblJsY21GamRHbHZiaUlzSW1oa2FXY2lPbVpoYkhObExDSmthV2NpT2lJM1lUSTBabUU0TkRobE56UXdZVE5oWVRObE1HTXdOV1ptWVdaa05qa3laV014T1dGak5HTTBZalV6WlRSbE5UQmhPREkxTjJVNE16TTBZbU01TXpjeUlpd2lhWE56SWpvaVptbHNaU0lzSW1GMVpDSTZJbnBtY3lJc0ltVjRjQ0k2TVRjNE16TXpPRGMzTnl3aWFXRjBJam94Tnpnek16TTNPRGMzZlEucHBjN0gyOENyUm8ybzZtX2JZdF9HMTR5MUlfWXpiOHVTSF9IakVMbS1QN3htRTg1QS0wSElNbmZQQ0E3ZUVJa0RsdnNHMkdhUmlrVGQzcmp4N0JwbWcmcmVzcG9uc2UtY2FjaGUtY29udHJvbD1wdWJsaWMlMkMlMjBtYXgtYWdlJTNENzc3NjAwMCUyQyUyMGltbXV0YWJsZSZyZXNwb25zZS1uby12YXJ5LXNlYXJjaD1rZXktb3JkZXIlMkMlMjBwYXJhbXMlM0QlMjglMjJqd3QlMjIlMjAlMjJQb2xpY3klMjIlMjAlMjJTaWduYXR1cmUlMjIlMjAlMjJLZXktUGFpci1JZCUyMiUyMCUyMnZlcmlmeSUyMiUyOSIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc4MzMzODc3N319fV19&Signature=GOll07ugzW-anpIOTllbFBybr-zDA9bdfm35UjEzIiyAIgBNZa1mcKjbLsNsHDRzfDorr~dPGHS3XSfjzlGoHP0DT3fHudabNy8fMnwjrrSO8OHGDIEN01G5X628uhcUzEyxOPXhlrbMWJw9iC68YbC6zzSILQQxdgyeofTMtS7-E-ftVdkEaheFBGbvJeHfuSTf2gLrZk2Dji6B76-u1BgUKzrFdumFVEOuZ1rc2uiojZWhxGTUNMFQt7NzOkslf2NdaatMBNZyyoNXclgDnoWlNAUIIrN1DWqU1kwhmjvzP0aRiO6IWrfo0fJ1BI8f38NVqsxTsY3EvDtJvbfrMQ__&Key-Pair-Id=KL18RPQB3R725)
    
4.  关注一下他的x
    
    [截屏2026-07-06 19.20.36.png](https://us04file-paa.zoom.us/file/ITE5aeuLTpiP8G2Joon38w?filename=%E6%88%AA%E5%B1%8F2026-07-06%2019.20.36.png&jwt=eyJrIjoidnQvK3BVSSsiLCJhbGciOiJFUzI1NiJ9.eyJpYXQiOjE3ODMzMzc4NzcsImhkaWciOmZhbHNlLCJpaWMiOiJ1czA0IiwiZXhwIjoxNzgzMzM4Nzc3LCJhdWQiOiJ6ZnMiLCJpc3MiOiJmaWxlIiwiZGlnIjoiMWU4Yjk2MmQ3YWVlMzY1MTJiZmNlMGE5YmI0ZGY4YWM2MjBjYjdhM2NhNjk5ZjA4NjMwOWEzNGJhNTgxYmU0NSIsIm9yaSI6Imx5bngtaW50ZXJhY3Rpb24ifQ.HBY1WnKyT8LlUOVDgKvDZ5nD1X9iGJX98FJWFCKDXZ8LrPmT-thrLl7T9cT8bOmqMSHDbTlM48q7pE6lxl5XjA&response-cache-control=public%2C%20max-age%3D7776000%2C%20immutable&response-no-vary-search=key-order%2C%20params%3D%28%22jwt%22%20%22Policy%22%20%22Signature%22%20%22Key-Pair-Id%22%20%22verify%22%29&Policy=eyJTdGF0ZW1lbnQiOlt7IkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc4MzMzODc3N319LCJSZXNvdXJjZSI6Imh0dHBzOi8vdXMwNGZpbGUtcGFhLnpvb20udXMvZmlsZS9JVEU1YWV1TFRwaVA4RzJKb29uMzh3P2ZpbGVuYW1lPSVFNiU4OCVBQSVFNSVCMSU4RjIwMjYtMDctMDYlMjAxOS4yMC4zNi5wbmcmand0PWV5SnJJam9pZG5RdkszQlZTU3NpTENKaGJHY2lPaUpGVXpJMU5pSjkuZXlKcFlYUWlPakUzT0RNek16YzROemNzSW1oa2FXY2lPbVpoYkhObExDSnBhV01pT2lKMWN6QTBJaXdpWlhod0lqb3hOemd6TXpNNE56YzNMQ0poZFdRaU9pSjZabk1pTENKcGMzTWlPaUptYVd4bElpd2laR2xuSWpvaU1XVTRZamsyTW1RM1lXVmxNelkxTVRKaVptTmxNR0U1WW1JMFpHWTRZV00yTWpCallqZGhNMk5oTmprNVpqQTROak13T1dFek5HSmhOVGd4WW1VME5TSXNJbTl5YVNJNklteDVibmd0YVc1MFpYSmhZM1JwYjI0aWZRLkhCWTFXbkt5VDhMbFVPVkRnS3ZEWjVuRDFYOWlHSlg5OEZKV0ZDS0RYWjhMclBtVC10aHJMbDdUOWNUOGJPbXFNU0hEYlRsTTQ4cTdwRTZseGw1WGpBJnJlc3BvbnNlLWNhY2hlLWNvbnRyb2w9cHVibGljJTJDJTIwbWF4LWFnZSUzRDc3NzYwMDAlMkMlMjBpbW11dGFibGUmcmVzcG9uc2Utbm8tdmFyeS1zZWFyY2g9a2V5LW9yZGVyJTJDJTIwcGFyYW1zJTNEJTI4JTIyand0JTIyJTIwJTIyUG9saWN5JTIyJTIwJTIyU2lnbmF0dXJlJTIyJTIwJTIyS2V5LVBhaXItSWQlMjIlMjAlMjJ2ZXJpZnklMjIlMjkifV19&Signature=SdYnb6w3IPaxgTbfdTQEH2YYPiy2ugfTxUwmUfnxLP~jF4LYqaBVLJSPpz1jfOnRBm6zwTMiZreKp-DBtMXAMg6w-DzYlra2ehHKtJNFdYBpaQB7jF1INs-Mko08kSpCxMCGpCO0W0lCM9qsj3gRDNTz1RQ78WI~sjBGhp6PVlPTjLg5TMJDawCsa1naWyguHNeN8oPtJbx~ckIcdLnxNQmv3FYjxYSHSpICpmF1Nq644U82jeS~XRSS0p0tFN35u2ahKGigBbu~gMTYeS6jhsg~-paOuJl~7wId3uoBWGLCuVTeN~24sF5IMGSAu1sOWre24R7sJB7Hiy3dBsYvBg__&Key-Pair-Id=KL18RPQB3R725)
    
5.  SVP Chain- AI链
    
    [截屏2026-07-06 19.21.54.png](https://us04file-paa.zoom.us/file/huDtt0rhQSKhXTZ_Wg6lyg?filename=%E6%88%AA%E5%B1%8F2026-07-06%2019.21.54.png&jwt=eyJhbGciOiJFUzI1NiIsImsiOiJ2dC8rcFVJKyJ9.eyJpc3MiOiJmaWxlIiwiYXVkIjoiemZzIiwiZGlnIjoiZjUzYWMxZThkZGJhNDczNmU5MDg0MjJkNmExNGVhODIwMDQzYThhNjNkZGI5NTE0YWZkNmIyMzcwZjA3NjQ3YyIsImlpYyI6InVzMDQiLCJoZGlnIjpmYWxzZSwiZXhwIjoxNzgzMzM4Nzc3LCJvcmkiOiJseW54LWludGVyYWN0aW9uIiwiaWF0IjoxNzgzMzM3ODc3fQ.oMzi0cMCwiuzwVP_ClhtFqasnAValnBGlV_qaQ-QRdOCETt0h5DyuZWK3F8Ho976kv8LWDaJG1Jln-JtF1jigA&response-cache-control=public%2C%20max-age%3D7776000%2C%20immutable&response-no-vary-search=key-order%2C%20params%3D%28%22jwt%22%20%22Policy%22%20%22Signature%22%20%22Key-Pair-Id%22%20%22verify%22%29&Policy=eyJTdGF0ZW1lbnQiOlt7IkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc4MzMzODc3N319LCJSZXNvdXJjZSI6Imh0dHBzOi8vdXMwNGZpbGUtcGFhLnpvb20udXMvZmlsZS9odUR0dDByaFFTS2hYVFpfV2c2bHlnP2ZpbGVuYW1lPSVFNiU4OCVBQSVFNSVCMSU4RjIwMjYtMDctMDYlMjAxOS4yMS41NC5wbmcmand0PWV5SmhiR2NpT2lKRlV6STFOaUlzSW1zaU9pSjJkQzhyY0ZWSkt5SjkuZXlKcGMzTWlPaUptYVd4bElpd2lZWFZrSWpvaWVtWnpJaXdpWkdsbklqb2laalV6WVdNeFpUaGtaR0poTkRjek5tVTVNRGcwTWpKa05tRXhOR1ZoT0RJd01EUXpZVGhoTmpOa1pHSTVOVEUwWVdaa05tSXlNemN3WmpBM05qUTNZeUlzSW1scFl5STZJblZ6TURRaUxDSm9aR2xuSWpwbVlXeHpaU3dpWlhod0lqb3hOemd6TXpNNE56YzNMQ0p2Y21raU9pSnNlVzU0TFdsdWRHVnlZV04wYVc5dUlpd2lhV0YwSWpveE56Z3pNek0zT0RjM2ZRLm9NemkwY01Dd2l1endWUF9DbGh0RnFhc25BVmFsbkJHbFZfcWFRLVFSZE9DRVR0MGg1RHl1WldLM0Y4SG85NzZrdjhMV0RhSkcxSmxuLUp0RjFqaWdBJnJlc3BvbnNlLWNhY2hlLWNvbnRyb2w9cHVibGljJTJDJTIwbWF4LWFnZSUzRDc3NzYwMDAlMkMlMjBpbW11dGFibGUmcmVzcG9uc2Utbm8tdmFyeS1zZWFyY2g9a2V5LW9yZGVyJTJDJTIwcGFyYW1zJTNEJTI4JTIyand0JTIyJTIwJTIyUG9saWN5JTIyJTIwJTIyU2lnbmF0dXJlJTIyJTIwJTIyS2V5LVBhaXItSWQlMjIlMjAlMjJ2ZXJpZnklMjIlMjkifV19&Signature=M41Ds~y5nTA3pFPLNLLRWmYz-VVTRLGc1YmIR3XtY6AQOAdAipepoLlZlrMp7Bs7HZ27lawmryZxDxzJ01zypX0~sB~zdx~~MTbVRCkTll3bhIPgSVaXan62QXaxhQ5LvJy8UE9njhdVlhAWVcyc2Vef7wcDSEifWo9vtQyAWtPERnQUkQOZrpnF8pN8uTNvJOjutOl5ax5NXbahx57aq8wnqgUP32-ebbXJsQTYX0zsqKFUx-9XGR8C7z6nhCxiF7Fw95eno9AYLtyCsJSZxEudn145vz2CkoMMp99hFjQtI8wGElpjBIKZCoN3KBQYI-RF8RCzmcijsfnPYyaoIg__&Key-Pair-Id=KL18RPQB3R725)
    

### 新知识点：

以前了解不太具体不太清楚的

-   分布式网络
    
    一个区块链网络中有非常多的节点（矿机）来记账，每个节点都会记录完整的、相同的区块链信息！
    
    ### [**有了分布式网络之后，区块链有了新特性**](https://web3intern.xyz/zh/blockchain-basic/#%E6%9C%89%E4%BA%86%E5%88%86%E5%B8%83%E5%BC%8F%E7%BD%91%E7%BB%9C%E4%B9%8B%E5%90%8E-%E5%8C%BA%E5%9D%97%E9%93%BE%E6%9C%89%E4%BA%86%E6%96%B0%E7%89%B9%E6%80%A7)
    
    **去中心化**
    
    区块链网络通常分布在全球，每个节点都将会存储一份相同的区块链数据。没有人能够控制全部的节点，因此这份区块链数据将会一直存在。
    
    **真正的不可篡改**
    
    区块链网络通常分布在全球，一个人控制大部分节点几乎不可能，因此即便有人修改了部分节点的区块链数据，只要被修改记录的节点不超过 51%，这个改动将不会被认可。
    
    ![image.png](attachment:209316a4-f938-4d36-b4aa-f06199977c47:image.png)
-   比特币
    
    -   **节点可以得到奖励**
        
    
    网络节点服务提供商（以下简称为网络服务提供商）可以得到奖励。不同的网络服务提供商可以得到不同的代币奖励，比如：比特币.
    
    -   **比特币具备了货币属性**
        
    
    根据比特币的设计，它仅有有限的供应量，而且可以自由转账。因此具备了货币的特性，成为了加密货币。
    
    -   **比特币缺点**
        
        匿名和自由有利也有弊。弊端在于难以追踪和限制，所以经常被黑产利用，进行洗钱等违法犯罪活动。由于每打包一个区块需要约 10 分钟，因此也会影响交易的实时性。每个区块的存储数据也是有限的。不过，越来越多的新区块链技术正在优化和解决这些问题。
        
-   区块链怎么运营起来
    
    ### [**一条区块链如何运行起来呢？**](https://web3intern.xyz/zh/blockchain-basic/#%E4%B8%80%E6%9D%A1%E5%8C%BA%E5%9D%97%E9%93%BE%E5%A6%82%E4%BD%95%E8%BF%90%E8%A1%8C%E8%B5%B7%E6%9D%A5%E5%91%A2)
    
    区块链生态系统的运行包含以下几个关键步骤：
    
    1.  **用户发起交易**：用户通过钱包应用发起转账、智能合约调用等操作
        
    2.  **交易广播**：交易信息被广播到整个网络中的各个节点
        
    3.  **节点验证**：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）
        
    4.  **打包成块**：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块
        
    5.  **链接上链**：新区块被添加到区块链上，更新全网的账本状态
        
    6.  **奖励发放**：成功出块的节点可获得协议奖励（如区块奖励/质押奖励）和交易手续费（或其中一部分）
        
    
    区块链生态系统运行流程
    
    ![image.png](attachment:ac42806b-93ed-46a0-8c8b-afd04e8c00cd:image.png)
-   **公链、私链、联盟链**
    
    # [**四、公链 vs 私链 vs 联盟链**](https://web3intern.xyz/zh/blockchain-basic/#%E5%9B%9B%E3%80%81%E5%85%AC%E9%93%BE-vs-%E7%A7%81%E9%93%BE-vs-%E8%81%94%E7%9B%9F%E9%93%BE)
    
    区块链根据访问权限与治理模式，大致可分为三类。按照去中心化程度从高到低排列。
    
    公链、联盟链、私链对比图
    
    ![公链、联盟链、私链对比图](https://web3intern.xyz/assets/different-chains-BJzFHow9.jpg)
    
    公链、联盟链、私链对比图
    
    ### [**1\. 公链（Public Blockchain） = 公共公园**](https://web3intern.xyz/zh/blockchain-basic/#_1-%E5%85%AC%E9%93%BE-public-blockchain-%E5%85%AC%E5%85%B1%E5%85%AC%E5%9B%AD)
    
    想象一个 **完全开放的公园**，任何人都可以自由进入、散步、拍照、甚至参与公园的维护（比如修剪草坪、清理垃圾）。公园里没有管理员，所有规则由大家共同制定。
    
    -   **成为节点的方法**：
        
        -   **无需申请**：任何人只要带着工具（比如手机、电脑）就能加入公园，成为维护者（节点）。
            
        -   **自由进出**：你可以随时离开或回来，没人会拦你。
            
    -   **共同管理数据的模式**：
        
        -   **所有人可见**：公园里的所有活动（比如谁修剪了草坪、谁清理了垃圾）都会被公开记录在公告栏上，所有人都能看到。
            
        -   **去中心化决策**：如果公园需要修路，大家会投票决定（共识机制），不需要某个领导拍板。
            
        -   **缺点**：因为人太多，决策效率低（交易确认慢），维护成本高（比如电费、人力）。
            
    
    ### [**2\. 联盟链（Consortium Blockchain） = 多公司联合的董事会**](https://web3intern.xyz/zh/blockchain-basic/#_2-%E8%81%94%E7%9B%9F%E9%93%BE-consortium-blockchain-%E5%A4%9A%E5%85%AC%E5%8F%B8%E8%81%94%E5%90%88%E7%9A%84%E8%91%A3%E4%BA%8B%E4%BC%9A)
    
    想象一个由 **多家公司**（比如银行、物流公司）组成的 **董事会**，他们共同管理一个共享数据库（比如客户信用信息）。只有这些公司才能参与决策，外人不能随便加入。
    
    -   **成为节点的方法**：
        
        -   **需要邀请或申请**：如果你想加入董事会，必须得到其中一家公司的认可（比如你是某家银行的合作伙伴）。
            
        -   **权限分级**：董事会成员可能分为两类：
            
            -   **决策者**（联盟核心成员）：比如银行 A、银行 B，可以修改数据库规则。
                
            -   **观察者**（联盟普通成员）：比如物流公司，只能查看数据但不能修改。
                
    -   **共同管理数据的模式**：
        
        -   **半公开数据**：数据库里的信息（比如客户信用评分）只有董事会成员能看到，外人无法访问。
            
        -   **联合决策**：如果要修改数据库规则（比如增加新字段），需要董事会成员投票通过。
            
        -   **优点**：效率比公链高（因为成员少），隐私比公链好（数据不对外公开），但不如私链灵活（需要多方协调）。
            
    
    ### [**3\. 私链（Private Blockchain） = 私人俱乐部**](https://web3intern.xyz/zh/blockchain-basic/#_3-%E7%A7%81%E9%93%BE-private-blockchain-%E7%A7%81%E4%BA%BA%E4%BF%B1%E4%B9%90%E9%83%A8)
    
    想象一个 **私人俱乐部**，只有会员才能进入。俱乐部的老板（比如 CEO）完全控制规则，比如谁可以加入、谁能查看账本（会员消费记录）。
    
    -   **成为节点的方法**：
        
        -   **严格审批**：想加入俱乐部？必须经过老板批准（比如交会员费、填写申请表）。
            
        -   **固定成员**：一旦成为会员，你的权限由老板决定（比如能否查看账本、能否修改规则）。
            
    -   **共同管理数据的模式**：
        
        -   **数据完全私有**：账本只对会员开放，外人无法查看（比如你的消费记录只有你和老板知道）。
            
        -   **老板说了算**：如果俱乐部要改规则（比如涨价），老板可以直接决定，不需要投票。
            
        -   **优点**：效率极高（因为只有少数人参与），隐私极强（数据不对外公开），但缺乏公链的透明性。
            
    
    ### [**4. 总结对比**](https://web3intern.xyz/zh/blockchain-basic/#_4-%E6%80%BB%E7%BB%93%E5%AF%B9%E6%AF%94)
    
    | 区块链类型 | 节点加入方式 | 数据可见性 | 管理模式 | 适合场景 |
    | --- | --- | --- | --- | --- |
    | 公链 | 任何人自由加入 | 所有人可见 | 去中心化（大家投票） | 加密货币、公共存证 |
    | 联盟链 | 需联盟成员邀请/审批 | 仅联盟成员可见 | 多中心化（董事会决策） | 供应链、金融协作 |
    | 私链 | 由老板严格审批 | 仅内部成员可见 | 中心化（老板说了算） | 企业内部管理、审计 |
    
-   Web3 vs **Web3.0** vs Web2
    
    # [**四、Web3 vs Web 3.0 vs Web2 的范式革命**](https://web3intern.xyz/zh/blockchain-basic/#%E5%9B%9B%E3%80%81web3-vs-web-3-0-vs-web2-%E7%9A%84%E8%8C%83%E5%BC%8F%E9%9D%A9%E5%91%BD)
    
    ### [**1\. Web2（当前互联网）**](https://web3intern.xyz/zh/blockchain-basic/#_1-web2-%E5%BD%93%E5%89%8D%E4%BA%92%E8%81%94%E7%BD%91)
    
    **核心特征：**
    
    -   **中心化控制**：数据存储在科技巨头的服务器（如 Google、Facebook）
        
    -   **用户角色**：内容生产者，但不拥有数据
        
    -   **商业模式**：广告驱动，平台抽取佣金
        
    -   **典型应用**：微信、抖音、亚马逊
        
    
    **比喻**：
    
    就像租房子，你可以装饰（发内容），但房东（平台）随时能收回钥匙（封号）。
    
    ### [**2\. Web 3.0（语义网）**](https://web3intern.xyz/zh/blockchain-basic/#_2-web-3-0-%E8%AF%AD%E4%B9%89%E7%BD%91)
    
    **核心特征：**
    
    -   **语义标记**：使用 RDF、OWL 等标准描述数据关系
        
    -   **结构化数据**：信息按照标准格式组织，便于机器理解
        
    -   **知识图谱**：构建实体间的语义关系网络
        
    -   **典型技术**：本体工程、语义查询语言（SPARQL）、链接数据
        
    
    **关键区别**：
    
    -   ❌ **不是区块链技术**，而是传统互联网的数据组织升级
        
    -   ❌ **主要不依赖 AI**，而是通过标准化数据格式实现
        
    -   ✅ 与 Web3 可结合（语义标记 + 区块链存储）
        
    
    **比喻**：
    
    像把图书馆的每本书都贴上详细标签（作者、主题、关联书籍），让图书管理员能快速找到相关资料。
    
    ### [**3\. Web3（去中心化互联网）**](https://web3intern.xyz/zh/blockchain-basic/#_3-web3-%E5%8E%BB%E4%B8%AD%E5%BF%83%E5%8C%96%E4%BA%92%E8%81%94%E7%BD%91)
    
    **核心特征：**
    
    -   **数据主权归用户**：用区块链存储身份和资产
        
    -   **无需信任中介**：智能合约自动执行规则
        
    -   **核心组件**：
        
    -   **典型应用**：MetaMask、Uniswap、ENS
        
    
    **核心创新**：
    
    -   **真正拥有数字资产**：你的 NFT 头像、游戏道具真正属于你，平台无法删除或收回
        
    -   **金融服务无门槛**：无需银行卡，用手机钱包就能借贷、理财、交易
        
    -   **应用可自由组合**：一个 DeFi 协议的流动性可以被其他应用直接调用，就像搭积木
        
    -   **内容永不消失**：文章、图片存储在分布式网络，不会因为平台关闭而丢失
        
    
    **比喻**：
    
    像自己买地盖房（数据自托管），用智能合约管理水电费（自动结算）。
    
    ### [**4. 对比矩阵**](https://web3intern.xyz/zh/blockchain-basic/#_4-%E5%AF%B9%E6%AF%94%E7%9F%A9%E9%98%B5)
    
    | 维度 | Web2 | Web 3.0 | Web3 |
    | --- | --- | --- | --- |
    | 控制权 | 平台垄断 | 部分开放 | 用户自治 |
    | 数据存储 | 中心服务器 | 混合存储 | 区块链 / IPFS |
    | 支付系统 | 信用卡 / 支付宝 | 集成支付 | 加密货币 |
    | 典型技术 | JavaScript | RDF / OWL | 智能合约 |
    | 代表企业 | 腾讯 / 阿里 | W3C / DBpedia | Uniswap / ConsenSys |
    
    ### [**5. 常见误解澄清**](https://web3intern.xyz/zh/blockchain-basic/#_5-%E5%B8%B8%E8%A7%81%E8%AF%AF%E8%A7%A3%E6%BE%84%E6%B8%85)
    
    1.  **Web3 ≠ Web 3.0**
        
        -   Web3 是**区块链驱动**的革命
            
        -   Web 3.0 是**语义网技术驱动**的数据组织升级
            
    2.  **Web3 不是万能的**
        
        -   优势：金融、产权、隐私场景
            
        -   劣势：不适合高频社交（如微博）
            
    3.  **渐进式过渡**
        
        -   **Web2.5 案例**：Reddit 社区积分（链上积分+传统界面）
            
    
    ### [**6. 技术栈对比**](https://web3intern.xyz/zh/blockchain-basic/#_6-%E6%8A%80%E6%9C%AF%E6%A0%88%E5%AF%B9%E6%AF%94)
    
    **Web2 开发**：
    
    ```bash
    React + Node.js + MySQL
    ```
    
    **Web3 开发**：
    
    ```bash
    React + Ethers.js + Solidity + IPFS
    ```
    
    **Web 3.0 开发**：
    
    ```bash
    Python + RDFLib + SPARQL
    ```
    
    ### [**7. 该关注哪个？**](https://web3intern.xyz/zh/blockchain-basic/#_7-%E8%AF%A5%E5%85%B3%E6%B3%A8%E5%93%AA%E4%B8%AA)
    
    -   想参与去中心化金融？→ **学 Web3**（Solidity / Rust）
        
    -   想构建知识图谱和语义搜索？→ **学 Web 3.0**（RDF / OWL）
        
    -   想快速就业？→ **Web2 仍是主流市场**
        

* * *

### 以太坊补充

-   以太坊Ethereum和Bitcoin 区别差异
    
    # [**二、Ethereum 与 Bitcoin 的差异**](https://web3intern.xyz/zh/overview-of-ethereum/#%E4%BA%8C%E3%80%81ethereum-%E4%B8%8E-bitcoin-%E7%9A%84%E5%B7%AE%E5%BC%82)
    
    尽管以太坊和比特币均基于区块链技术，但两者的设计目标、功能和技术路线存在显著差异：
    
    | 维度 | 比特币（Bitcoin） | 以太坊（Ethereum） |
    | --- | --- | --- |
    | 目标与定位 | 去中心化的数字货币，强调安全、稳定和稀缺性（总量 2100 万枚） | 去中心化平台，支持智能合约和 Dapps，定位为“区块链 2.0” |
    | 编程能力 | 脚本语言有限，仅支持简单的交易验证逻辑 | 图灵完备的编程语言（如 Solidity），可开发复杂智能合约 |
    | 共识机制 | 工作量证明（PoW），矿工通过算力竞争记账权 | 从 PoW 转向权益证明（PoS），通过 The Merge 实现能源效率优化 |
    | 交易速度 | 每 10 分钟生成一个区块，交易确认较慢 | 区块时间约 12 秒，交易确认更快，适合高频应用 |
    | 经济模型 | 总量固定，强调抗通胀属性 | 供应灵活，通过 EIP-1559 等机制可能呈现通缩趋势 |
    
    以太坊的灵活性使其能够支持更多应用场景，例如 DeFi（借贷、交易）、NFT（数字艺术品）、DAO（去中心化自治组织）等，而比特币则更专注于作为“数字黄金”存储价值。
    
-   以太坊发展
    
    ![image.png](attachment:4c08cb37-f6b3-4d5a-b876-d65e5669d5cb:image.png)

[https://web3intern.xyz/zh/overview-of-ethereum/#二、ethereum-与-bitcoin-的差异](https://web3intern.xyz/zh/overview-of-ethereum/#%E4%BA%8C%E3%80%81ethereum-%E4%B8%8E-bitcoin-%E7%9A%84%E5%B7%AE%E5%BC%82)

⬆️ 值得看一看
<!-- DAILY_CHECKIN_2026-07-06_END -->

<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

# 3.1

# **FluxA / AI Agent Payment 学习笔记**

<aside>

uxA skill:  
[https://fluxapay.xyz/skill.md](https://fluxapay.xyz/skill.md)

Agent Wallet:  
[https://agentwallet.fluxapay.xyz/](https://agentwallet.fluxapay.xyz/)

联系邮箱:  
[jackxiao@fluxapay.xyz](mailto:jackxiao@fluxapay.xyz)

实习 / 内推:  
团队有 intern 机会，可以发邮件问，也可以后续等训练营内推。  
建议提升方向：更加提升 engineering 能力。

</aside>

* * *

## **一、这节课到底在讲什么？**

这节课讲的是：

**AI Agent 未来要真正帮人做事，就一定会涉及“花钱、收钱、结算、风控、合规”这些金融问题。FluxA 做的是给 AI Agent 使用的支付基础设施。**

它和我的 FinTech 专业关联非常大，因为它不是单纯 Web3 技术，也不是单纯 AI 工具，而是把几个东西连在一起：

```
AI Agent
↓
支付系统
↓
钱包 / 稳定币 / 法币充值
↓
风控 / 身份 / 授权
↓
结算 / 撤销 / 审计
↓
商户 / API / MCP / 数据服务
```

所以这节课的核心不是“怎么让 AI 聊天”，而是：

**当 AI Agent 真的开始执行任务时，怎么让它安全、高效、可控地花钱。**

* * *

## **二、为什么 Agent Payment 是一个问题？**

### **1\. 传统支付逻辑太简单**

传统商业逻辑通常是：

```
我给你钱
你给我货
交易结束
```

比如：

```
买衣服
买咖啡
订阅会员
线上购物
```

这类交易通常是一次性、明确金额、明确商户、明确买家身份。

### **2\. Agent 的支付逻辑不一样**

AI Agent 做任务时，不一定是一次性付款。

例如让 Agent 写一份研究报告，它可能需要：

```
调用大模型 API
购买付费数据库资料
访问某个付费网站
下载图片或报告
调用另一个 Agent 的能力
使用某个 MCP / API 工具
最后生成完整结果
```

也就是说，Agent 完成一个任务，背后可能是一整串动作。

老师提到这个过程可以理解为：

```
Intention Discovery
```

也就是：

Agent 先理解用户意图，再拆解任务，再调用工具，再完成支付和执行。

### **3\. 这里会出现大量小微支付**

Agent 调用 API 或数据时，可能每次只花：

```
0.001
0.01
0.1
```

这种金额对传统支付体系不友好，因为：

```
手续费可能比支付金额还贵
银行卡 / 支付卡体系不适合高频小额调用
传统结算可能 T+1 / T+3，不够快
每一步都让人手动确认，体验会很差
```

所以老师讲的重点是：

**Agent economy 需要一种更适合小额、高频、自动化、可审计的支付体系。**

* * *

## **三、Agent 为什么不能直接用传统支付？**

### **1\. Agent 没有自然人身份**

传统金融系统里，开银行账户通常需要：

```
身份证
手机号
实名信息
KYC
银行账户
```

但 AI Agent 本身没有身份证，也不是自然人。

所以问题是：

```
Agent 怎么拥有支付身份？
Agent 怎么被识别？
Agent 怎么收钱？
Agent 怎么花钱？
Agent 花错钱怎么处理？
```

### **2\. Agent 不能无限制花钱**

AI Agent 可能会产生幻觉，也可能理解错任务。

如果它只是回答错一句话，问题还不大。

但如果涉及支付，问题就很严重。

比如：

```
错误调用昂贵 API
重复下单
买错东西
把预算花光
访问错误商户
付给错误对象
```

所以 Agent Payment 必须有：

```
预算限制
权限范围
风控规则
交易审计
可撤销机制
人工兜底
```

这和 FinTech 里的风控逻辑非常接近。

* * *

## **四、FluxA 想解决什么？**

### **1\. 核心定位**

FluxA 想做的是：

**一套让 AI Agent 可以自动、安全地收款、付款、结算的支付基础设施。**

它不是单纯的钱包，也不是单纯的 API，而是一整套体系。

可以理解为：

```
Agent Wallet
+
Agent Payment Protocol
+
Risk Control Engine
+
Settlement Layer
+
Developer Monetization Platform
```

### **2\. 目标用户**

老师提到的场景大概包括：

```
Agent to Agent
Agent to Merchant
Agent to Tool / API / MCP
Agent to User
Agent social transfer
Developer monetization
```

也就是不只是人给 Agent 付款，而是：

```
Agent 给 Agent 付费
Agent 给商户付费
Agent 调用工具并按次付费
Agent 帮用户订票 / 购买服务
开发者把自己的 API / MCP 上架赚钱
```

* * *

## **五、Agent Payment 的几类场景**

### **1\. Agent to Agent**

一个 Agent 调用另一个 Agent 的能力。

例如：

```
研究 Agent 调用数据 Agent
旅行 Agent 调用订票 Agent
写作 Agent 调用图片 Agent
```

如果被调用的 Agent 提供了有价值的能力，就可以按次收费。

这对应：

```
Agent 之间的点对点交易
```

### **2\. Agent to Merchant**

Agent 代表用户向商户购买服务。

比如：

```
订酒店
订机票
购买衣服
订阅服务
交水电煤费
买演出票
```

这更像 To B / 商户支付场景。

这里的问题是：

```
商户怎么识别 Agent？
商户怎么收 Agent 的钱？
Agent 怎么证明支付有效？
支付失败怎么处理？
```

### **3\. Agent to Tool / API / MCP**

很多开发者会把自己的能力做成：

```
API
MCP
数据接口
AI service
专业知识服务
自动化工具
```

Agent 需要调用这些能力时，更适合：

```
按需付费
按次付费
微支付
```

而不是每个工具都买一个月订阅。

这也是老师反复强调微支付的原因。

### **4\. Agent Social Transfer**

Agent 之间也可以直接转账或做社交支付。

比如老师提到春节红包场景：

```
用户给 Agent 指令
Agent 去检查谁发红包
Agent 自动抢红包
甚至根据题目条件抢红包
```

这属于更生活化、社交化的 agent payment use case。

* * *

## **六、FluxA 的产品结构**

老师大概把 FluxA 的支付体系拆成几个核心部分：

```
Identity
Budget
Policy / Risk Control
Auditability
Revocability
Settlement
Wallet
Payment Link
Agent Card
Monetization
```

### **1\. Identity：身份**

Agent 需要一个身份，否则它无法参与支付系统。

这里的身份不一定是传统身份证，而可能是：

```
Agent ID
钱包地址
Google 登录绑定
平台账号
链上身份
法币账户凭证
```

老师提到目前形态偏区块链钱包，同时也支持法币充值。

### **2\. Budget：预算**

人不能让 Agent 无限花钱，所以必须给它设预算。

比如：

```
最多花 50 美元
只能用于订票
只能调用某几个 API
只能在某个时间段内使用
只能单次消费
```

这和企业费用管理、信用卡额度、支付限额很像。

### **3\. Policy / Risk Control：规则和风控**

因为 Agent 可能乱花钱，所以需要风控规则。

比如：

```
单笔限额
总预算限额
商户白名单
用途限制
异常交易拦截
高风险交易人工确认
```

这部分非常 FinTech。

传统金融里也有：

```
反欺诈
反洗钱
KYC
KYB
交易监控
异常检测
风控模型
```

Agent Payment 只是把这些问题搬到了 AI Agent 场景里。

### **4\. Auditability：可审计**

每一笔交易都应该清楚记录：

```
谁发起
为什么发起
付给谁
付了多少
用了哪个工具
什么时候发生
是否成功
是否可撤销
```

支付场景不能是黑箱。

老师后面也提到，Agent 支付过程不是像大模型涌现能力那样完全看不懂。支付链路必须清晰，否则无法做金融。

### **5\. Revocability：可撤销**

用户对某笔交易不满意时，应该有撤销或争议处理机制。

这类似传统支付里的：

```
退款
撤销
chargeback
dispute
风控冻结
```

Agent Payment 不能只做“付款成功”，还要考虑：

```
付款错了怎么办？
Agent 执行错了怎么办？
服务没交付怎么办？
用户反悔怎么办？
```

### **6\. Settlement：结算**

老师提到一个重要逻辑：

不一定每一笔小额交易都马上单独结算，可以在一个预算或 mandate 下，最后集中结算。

这样可以解决：

```
小额支付太碎
每笔都确认效率低
手续费过高
结算太慢
体验不流畅
```

这和很多支付 / 清算系统里的批量结算逻辑很像。

* * *

## **七、Agent Wallet 是什么？**

### **1\. 不是普通用户钱包**

普通钱包，比如 MetaMask，是给人用的。

Agent Wallet 是给 Agent 用的，但不能完全放任 Agent 控制。

所以它更像：

```
人和 Agent 共管的钱包
```

也就是：

```
人设定权限和预算
Agent 在范围内自动执行
系统负责风控和记录
```

### **2\. 为什么不能每笔都让人确认？**

如果 Agent 每次调用 API 都要人点确认，体验会很差。

比如写研究报告时：

```
调用 API 0.01
买数据 0.05
下载报告 0.1
再调用模型 0.02
再查资料 0.03
```

如果每一步都弹窗让人确认，Agent 就不再像 24 小时助手，而是变成人类手动操作工具。

所以更合理的是：

```
用户提前设定预算和规则
Agent 在规则内自动执行
系统记录和审计每一步
异常时才让人介入
```

### **3\. 大白话比喻**

Agent Wallet 像是：

给助理一张有限额、限定用途、可随时冻结的工作卡。

不是把银行卡密码直接给助理，而是说：

```
你最多花 50 美元
只能用于订票
只能今天用
每笔记录都要留下
出问题可以撤销
```

* * *

## **八、Agent Card 是什么？**

老师提到他们在探索 Agent Card，甚至和 Visa 做合作。

### **1\. 一次性 Agent Card**

一种模式是一次性卡。

比如：

```
用户让 Agent 去订一张机票
系统生成一张只限 450 美元的一次性卡
Agent 用这张卡完成付款
用完后卡失效
```

好处：

```
防止 Agent 乱花钱
降低盗刷风险
限制用途和金额
适合单次任务
```

### **2\. 持续性 Agent Card**

未来和 Visa 合作可能会探索更长期、持续使用的 Agent Card。

但这会涉及更多：

```
合规
支付牌照
发卡方合作
KYC / KYB
风控
传统金融机构合作
```

所以这不是简单做一个 Web3 钱包就能完成的。

* * *

## **九、法币、稳定币和 Base / USDC**

老师提到目前主要支付是在：

```
Base 上的 USDC
```

原因大概是：

```
USDC 合规性较强
Base 交易量大
支付场景相对成熟
Coinbase 有传统金融资源
手续费和效率适合 agent payment
```

这里非常 FinTech，因为它涉及：

```
稳定币支付
链上结算
合规稳定币
法币充值
支付牌照
传统金融机构合作
```

### **1\. 法币充值**

老师提到他们支持法币充值。

逻辑大概是：

```
用户通过 Stripe 付款
平台收到法币
转换成平台 credential / credits
用户或 Agent 使用这些额度
```

这更像平台积分或余额系统，不完全等同于链上稳定币自由转账。

### **2\. 稳定币支付**

稳定币更适合跨境、小额、高频、链上支付场景。

比如：

```
USDC
Base
Agent wallet
链上结算
```

### **3\. 数字人民币讨论**

有人问到数字人民币。

老师的判断大概是：

```
数字人民币未来也可能是方向
但国内微信 / 支付宝已经非常方便
普通用户缺少迁移动力
人民币国际化又涉及更大的政治和市场问题
```

所以支付工具能不能普及，不只是技术问题，更是：

```
用户需求
市场习惯
监管
政治经济环境
金融基础设施
```

* * *

## **十、x402 / HTTP 402 / Agent Payment Protocol**

老师提到一个类似 `x402` 的协议。

可以先这样理解：

它是围绕“Payment Required / 需要付费访问资源”这个逻辑，为 Agent 调用服务时支付而设计的协议思路。

它的目标是让 Agent 可以：

```
发现一个付费资源
知道价格
发起支付
完成访问
整个过程尽量自动化
```

也就是：

```
Agent 找到 API / MCP / 数据服务
↓
发现需要付费
↓
使用钱包或支付协议完成付款
↓
获得访问权限
```

这和传统人类付款不同，因为 Agent 不应该每次都跳转网页、填卡号、输入验证码。

* * *

## **十一、Monetize：让开发者的能力可以被 Agent 购买**

这是很重要的一块。

老师提到很多独立开发者、公司、OPC / 一人公司，会做出很多：

```
API
MCP
AI service
数据能力
专业知识服务
自动化工具
```

但问题是：

```
怎么被 Agent 发现？
怎么被调用？
怎么按次收费？
怎么持续赚钱？
```

FluxA 想做的是：

```
让开发者把自己的能力上架
变成 Agent 可以发现、调用、付费的 skill
平台帮忙做支付和结算
```

这就是所谓：

```
Monetize
```

### **大白话理解**

过去开发者做了一个 API，要自己解决：

```
官网
文档
登录
计费
支付
结算
推广
```

现在如果接入 FluxA 的体系，可能变成：

```
把 API / MCP 上架
Agent 能发现它
Agent 按次调用
平台自动计费
开发者获得收入
```

这个很像：

```
Agent 时代的 App Store / API Marketplace / Payment Layer
```

* * *

## **十二、Payment Link 和 Direct Transfer**

除了 Agent Wallet 和 Monetize，老师还提到不同收付款方式。

### **1\. Payment Link**

适合电商或金额不固定的支付场景。

比如：

```
买衣服
订酒店
买演出票
购买一次服务
```

商户生成 payment link，付款方点击后 approve。

它适合：

```
金额每次不一样
交易对象比较明确
用户或 Agent 需要完成一次付款
```

### **2\. Direct Transfer**

也可以直接根据 Agent ID 或钱包地址做转账。

这适合：

```
Agent to Agent
用户给 Agent 转账
Agent 给另一个 Agent 付款
简单点对点支付
```

* * *

## **十三、Use Cases 案例**

### **1\. 春节 Agent 抢红包**

用户给 Agent 指令：

```
今天有哪些人发红包？
帮我去抢。
```

甚至可以加条件：

```
答对题才能抢
比如“床前明月光”的下一句
```

这个案例体现的是：

```
Agent social payment
Agent 执行社交支付任务
Agent 和红包 / 任务 / 游戏机制结合
```

它很生活化，也说明 Agent Payment 不只是严肃金融，也可以进入社交场景。

### **2\. 百度智能云合作**

老师提到和百度智能云合作，重点是：

```
很多 AI 服务 / API / MCP 可以上架
FluxA 帮助这些能力 monetization
Agent 可以发现并调用这些能力
开发者获得收入
```

这和平台经济很像。

### **3\. Agent 完成任务后获得收益**

有些平台会让 Agent 完成任务。

任务完成后，Agent 可以获得对应 payment。

这意味着未来可能有：

```
Agent work platform
Agent task marketplace
Agent earning system
```

### **4\. Agent 订票 / 订演出**

老师提到和大麦相关的票务场景。

Agent 可以帮助用户：

```
订票
抢票
安排日程
完成付款
```

这非常接近日常生活，也说明 Agent Payment 未来可能进入：

```
旅游
票务
生活服务
订阅
电商
```

### **5\. 付费调研报告**

老师提到一个 VC / research service 的例子。

用户或 Agent 可以付费调用某个调研能力，获得：

```
公司研究
行业分析
数据报告
投资相关资料
```

这和金融、咨询、投研非常相关。

* * *

## **十四、为什么这和 FinTech 专业高度相关？**

这节课其实几乎就是 FinTech 的新形态。

### **1\. 支付 Payment**

核心就是：

```
谁付钱
付给谁
付多少
什么时候付
怎么确认
怎么结算
失败怎么办
```

这是支付系统最基础的问题。

### **2\. 风控 Risk Control**

Agent Payment 必须解决：

```
乱花钱
错误支付
欺诈支付
异常交易
预算超限
支付撤销
可审计
```

这和传统 FinTech 风控高度一致。

### **3\. 合规 Compliance**

未来 Agent Payment 一定会涉及：

```
KYC
KYB
AML
支付牌照
发卡合作
稳定币合规
法币出入金
商户审核
```

老师也提到，真正走向主流时，一定要和传统支付公司、金融机构合作。

### **4\. 稳定币和链上结算**

Base / USDC 的使用说明：

```
稳定币可能成为 Agent Payment 的重要结算媒介
链上支付适合高频、小额、跨境、自动化场景
```

这正好是 FinTech + Web3 的交叉点。

### **5\. 商业模式**

FluxA 不只是技术，它还在解决：

```
开发者怎么赚钱
API 怎么收费
MCP 怎么 monetization
商户怎么接入 Agent 支付
Agent 怎么购买服务
```

这就是金融基础设施 + 平台商业模式。

* * *

## **十五、和昨天链上实操的关系**

昨天做的是更底层的新手链上操作：

```
创建钱包
领取测试币
转账
查看 transaction hash
部署智能合约
调用合约函数
```

这些是理解今天内容的基础。因为今天讲的 Agent Payment 里面也会反复出现：

```
钱包
链上账户
稳定币
交易
支付确认
交易记录
结算
合约 / 协议
```

昨天的实操更像是“我第一次理解链上交易怎么发生”；今天这节课则是在讲“这些链上支付能力怎么被包装成真正的金融基础设施”。

* * *

## **十六、这节课最重要的逻辑链**

可以用这一条线理解整节课：

```
AI Agent 会越来越能执行任务
↓
执行任务一定会涉及调用工具、购买数据、订阅服务、买东西
↓
这些行为都涉及付款
↓
传统支付不适合 Agent 的高频、小额、自动化需求
↓
所以需要 Agent Payment Infrastructure
↓
这套基础设施必须包括身份、钱包、预算、风控、审计、撤销、结算和合规
↓
FluxA 正在做这个方向
```

* * *

## **十七、我第一次知道 / 值得记住的点**

### **1\. Agent 没有身份证，所以不能直接接入传统银行体系**

传统金融假设付款主体是人或企业。

但 Agent 既不是自然人，也不是传统公司账户。

所以 Agent 需要新的支付身份。

### **2\. Agent Payment 不能只做“让 AI 花钱”**

核心不是让 AI 随便付款，而是：

```
在预算和规则内自动付款
每笔可审计
异常可撤销
风险可控制
```

### **3\. 微支付是 Agent 场景的关键**

因为 Agent 调用服务可能是大量小额支付。

传统支付手续费太高，不适合这种场景。

### **4\. Agent Wallet 更像“受控工作卡”**

不是把钱完全交给 AI，而是：

```
设额度
设用途
设规则
允许自动执行
保留审计和撤销
```

### **5\. 稳定币可能是 Agent Payment 的重要底层**

老师提到目前主要用 Base 上的 USDC。

这说明稳定币在 AI Agent 支付里可能很重要。

### **6\. FinTech 背景很有优势**

因为这个方向不只是写代码，也需要理解：

```
支付
风控
合规
清算
结算
商户
金融机构合作
用户信任
```

这正好是 Accounting and FinTech 能切入的地方。

* * *

## **十八、对我自己的启发**

这节课让我感觉，Web3 和 FinTech 不只是“炒币”或“写智能合约”。

更现实的方向可能是：

```
Agent payment
Stablecoin payment
On-chain settlement
FinTech risk control
AI commerce infrastructure
Developer monetization
Merchant payment integration
```

如果以后找 FinTech / Web3 / AI 相关实习，可以把自己包装成：

```
懂一点 Web3 钱包和链上交易
懂一点 AI Agent 场景
理解支付、风控、合规和结算逻辑
愿意往工程能力继续补
```

而不是只说自己会做简单合约。

* * *

## **十九、可以写进作品集 / 简历的方向**

这节课之后，我自己的项目方向可以更贴近：

```
生活支付 + 链上确认 + AI 辅助
```

比如之前想的：

```
Split Bill Promise
```

就可以和今天内容产生联系：

```
普通用户仍然用微信 / 支付宝 / 银行转账完成真实付款
但账单创建、付款承诺、确认状态、交易记录可以用链上方式保存
未来可以加入 AI 自动识别账单和分账
```

这个方向和 FluxA 的核心逻辑有相似之处：

```
不是替代所有传统支付
而是在支付流程中加入更清晰的身份、授权、确认和记录
```

* * *

## **二十、后续 To-do**

```
1. 打开 <https://fluxapay.xyz/skill.md> 看 skill 怎么接入
2. 体验 <https://agentwallet.fluxapay.xyz/>
3. 记录 Agent Wallet 的完整使用流程
4. 了解 Base / USDC / stablecoin payment
5. 了解 KYC / KYB / AML 在 Agent Payment 中的作用
6. 补一点 engineering：前端 + 钱包连接 + API 调用
7. 后续如果投实习，可以发邮件到 jackxiao@fluxapay.xyz
```

邮件方向可以写：

```
自己是 Accounting and FinTech 背景
正在学习 Web3 / Agent payment / smart contract / wallet interaction
对 AI Agent payment、stablecoin payment、risk control、FinTech infrastructure 感兴趣
希望了解是否有 intern / research / product / engineering support 机会
```

# 2.1

# **EPF / Ethereum Protocol 笔记**

## **一、这节到底在讲什么？**

这节不是在讲普通 Web3 应用，也不是在讲怎么用钱包、怎么写简单智能合约，而是在讲：

如果想进入 Ethereum protocol 方向，应该怎么学习、怎么选方向、怎么做贡献、怎么准备 EPF。

也就是说，重点从“使用区块链”往更底层走：

```
普通用户：用钱包、转账、查交易
DApp 开发者：写智能合约、做前端、让用户交互
协议研究 / EPF：研究以太坊本身怎么运行、怎么升级、怎么被维护
```

* * *

## **二、EPF 是什么？**

### **1\. 基本定义**

**EPF = Ethereum Protocol Fellowship，以太坊协议奖学金。**

它是以太坊基金会为了降低进入以太坊协议研究和开发的门槛而设立的项目。

简单说：

EPF 是帮助新人进入 Ethereum protocol 世界的学习和贡献项目。

### **2\. 它不是普通智能合约训练营**

EPF 不是主要教：

```
写 Solidity
做 NFT
做 DeFi
做钱包
做 DApp 页面
```

它更关注：

```
以太坊底层怎么运行
执行层和共识层怎么配合
客户端怎么实现协议
EIP 如何推动协议升级
开发者如何参与开源贡献
```

### **3\. Permissioned / Permissionless**

EPF 有两种参与状态：

**Permissioned**

```
通过申请 / 面试
获得正式支持或津贴
参与更正式的 fellowship 流程
```

**Permissionless**

```
没有通过也可以继续参与
依然可以学习、做项目、提 PR、参与讨论
如果成果好，也可能被认可
```

所以重点不是“有没有被录取”，而是：

最后有没有真正做出东西。

* * *

## **三、它和钱包、链、DApp 有什么关系？**

### **1\. 钱包是什么层级？**

钱包，比如 MetaMask、Rabby，是建在以太坊之上的 **应用层工具**。

钱包主要负责：

```
连接网络
切换链
管理地址
签名交易
发起交易
连接 DApp
```

一句话：

钱包是在帮用户“使用链”。

### **2\. EPF 研究的是什么层级？**

EPF 不是研究钱包这种应用，而是研究 **链本身**。

它关心的是：

```
这条链怎么运行
交易怎么执行
区块怎么确认
协议怎么升级
客户端怎么实现
网络怎么更安全、更快、更便宜
```

一句话：

钱包是在“用路”，EPF 是在研究“这条路怎么修、怎么扩、怎么维护”。

### **3\. 大白话比喻**

```
区块链 = 一条高速公路
钱包 = 车和驾驶证，帮用户上路
智能合约 = 路上的自动机器，触发后按规则执行
客户端 = 维持高速公路运行的基础设施系统
协议开发 = 研究高速公路规则、结构、扩容和安全
```

所以：

```
普通用户关心：怎么转账？
DApp 开发者关心：怎么写合约给用户用？
EPF / 协议开发关心：这条链底层到底怎么运行？
```

* * *

## **四、Ethereum protocol 主要学什么？**

### **1\. 执行层 Execution Layer**

执行层负责：

```
交易执行
账户状态变化
EVM 执行
智能合约运行
gas 消耗
余额变化
```

可以理解为：

执行层负责“交易到底怎么被执行”。

比如用户发起一笔转账或调用合约，执行层要判断：

```
这个账户有没有余额？
这个交易是否合法？
合约代码怎么跑？
执行后状态怎么变化？
gas 怎么扣？
```

### **2\. 共识层 Consensus Layer**

共识层负责：

```
验证者
区块确认
链的安全性
谁来出块
大家如何同意哪条链是正确的
```

可以理解为：

共识层负责“大家怎么达成一致”。

也就是：

```
谁提出新区块？
谁验证新区块？
这个区块是否被接受？
链是否安全？
```

### **3\. 客户端 Client**

客户端是执行层或共识层的具体代码实现。

它不是一个单独抽象概念，而是实际运行以太坊的软件。

比如：

```
go-ethereum / geth
Lighthouse
Prysm
Nethermind
Besu
```

老师提到的 Go 客户端，一般是：

```
go-ethereum / geth
```

也就是用 **Go / Golang** 写的以太坊客户端。

注意：

Go 是 Google 开发的编程语言，不是 Godot 游戏引擎。Godot 里常用的是 GDScript。

### **4\. EIP**

EIP 是 Ethereum Improvement Proposal，以太坊改进提案。

它负责描述：

```
以太坊要改什么
为什么要改
怎么改
会影响哪些部分
执行层 / 共识层 / 客户端是否需要更新
```

老师说 EIP 刚开始看会比较难，因为它写法很像技术论文。

* * *

## **五、EPF 的学习路线**

### **1\. 第一阶段：补基础**

先理解以太坊这台“机器”怎么运行。

需要先搞懂：

```
什么是执行层
什么是共识层
交易从发出到确认发生了什么
钱包和节点是什么关系
客户端在运行什么
EIP 为什么会影响协议升级
```

这一阶段可以看：

```
EPF Wiki
官方 specs
Ethereum 相关 wiki
基础书籍或中文材料
```

### **2\. 第二阶段：看公开讨论**

Ethereum 的很多研究和讨论都是公开的，尤其在 GitHub 上。

可以看：

```
GitHub issue
PR discussion
EIP discussion
specs 更新
客户端仓库里的问题
```

这一阶段重点不是马上贡献，而是先看懂：

```
大家在讨论什么问题？
这个问题属于执行层还是共识层？
它和哪个 EIP / 客户端 / spec 有关？
```

### **3\. 第三阶段：每周做 update / notes**

老师建议每周整理学习记录。

可以写：

```
这周看了什么
理解了什么
哪里还不懂
发现了哪些 issue
对哪个方向更感兴趣
下一步准备看什么
```

然后可以发到社媒，或者 at EPF mentor，让别人给反馈。

它的作用是：

```
逼自己持续输出
留下学习轨迹
让 mentor 看到你的进度
帮助后面申请或项目整理
```

### **4\. 第四阶段：选一个小方向深入**

不能一开始什么都做。

应该先选一个小 scope，比如：

```
执行层规范里的某个结构
某个 EIP 的实现影响
某个客户端里的测试问题
某个文档或 spec 不清楚的地方
某个数据结构替换方向
```

重点是：

范围要小，但问题域要有延展性。

### **5\. 第五阶段：做 issue / PR / proposal**

最后产出可以包括：

```
小 PR
issue 讨论
测试补充
spec 修改
EIP 分析
项目 proposal
最终项目交付
```

其中 PR 是比较硬的产出，但不是一开始就盲目提大 PR。

* * *

## **六、新手怎么贡献？**

### **1\. 不要一开始做大项目**

老师强调：

```
scope 要小
PR 要小
不要一上来改几百行
```

因为大 PR 的问题是：

```
review 成本高
别人不一定愿意看
新手自己也难控制质量
如果方向错了，浪费时间更多
```

### **2\. 可以从小 PR 开始**

新手适合：

```
修文档
修小错误
补测试
跟进一个小 issue
改一个很明确的问题
```

小 PR 的好处是：

```
容易 review
容易 merge
能快速建立正反馈
能慢慢积累贡献记录
```

### **3\. 但小 PR 也要有连续性**

不是今天修 A，明天修 B，后天修一个完全无关的 C。

更好的方式是：

```
先选一个问题域
再找这个问题域下面的 2-3 个小 issue
围绕同一方向连续贡献
```

这样看起来不是“随机贡献”，而是在一个方向上持续深入。

### **4\. 先选问题域，再选 issue**

老师特别强调了这个逻辑：

```
不是先乱找 issue
而是先想清楚自己想研究哪个问题
再找这个问题下能做的小 issue
```

比如：

```
先选：执行层中的某个数据结构
再找：这个结构相关的 spec issue / test issue / PR
```

这样后面更容易形成：

```
学习路线
连续 PR
项目 proposal
面试时可讲的方向
```

* * *

## **七、EPF 面试看什么？**

### **1\. 不是算法八股**

老师说 EPF 面试不是国内那种技术面试。

一般不会让人：

```
现场手撕算法
背八股
讲复杂密码学细节
硬考拜占庭问题
```

### **2\. 更看重理解和动机**

更可能问：

```
如何理解 Ethereum
怎么看 Ethereum 未来发展
对哪个模块感兴趣
对哪个 EIP 感兴趣
为什么想做这个方向
是不是真的想参与协议研究
```

核心是：

是否真的理解以太坊，并且有持续参与的动机。

### **3\. 英文也重要**

老师提到面试可能是英文。

所以除了技术理解，还需要：

```
能听懂问题
能表达自己的方向
能解释自己为什么对某个模块感兴趣
```

* * *

## **八、代码背景重要吗？**

### **1\. 不一定必须是 CS 背景**

老师说自己也不是纯计算机背景，而是电子信息背景。

EPF 并不是只看代码水平。

它更看重：

```
协议理解
研究能力
学习能力
持续探索
对以太坊的兴趣
最终产出
```

### **2\. 但会代码会更轻松**

如果会这些，会更容易进入对应方向：

```
Python：适合看 execution specs 这类规范代码
Go：适合看 go-ethereum / geth
Rust：适合看部分客户端或底层工具
JavaScript：有助于理解 Web3 前端和工具链
```

### **3\. 代码是工具，不是唯一目标**

对于 protocol research 来说：

```
代码实现很重要
但不是一开始就只拼代码
理解协议、看懂讨论、提出合理问题也很重要
```

* * *

## **九、职业发展和现实情况**

### **1\. 协议研究不一定是最稳定的职业路线**

老师讲得很现实：

```
如果单纯从经济回报看，协议研究不一定最稳定
很多人不是全职做协议研究
很多人是白天正常工作，业余参与 Ethereum research
```

### **2\. 更稳定的岗位可能在客户端团队**

比如：

```
维护客户端的公司
协议相关组织
以太坊生态里的基础设施团队
```

客户端团队可能更像正式工作，有更稳定的收入结构。

### **3\. 很多人是因为兴趣参与**

以太坊的特殊之处在于：

```
资料公开
代码开源
讨论公开
任何人都可以学习和参与
```

所以不少人是因为对去中心化、开源协议、以太坊未来感兴趣，才持续贡献。

* * *

## **十、以太坊生态的核心价值**

老师说不谈价格，只谈生态和技术。

他认为以太坊重要的地方在于：

```
全球开发者数量多
优秀开发者持续参与
协议讨论公开
代码开源
EIP 和客户端都能公开查看
生态仍然活跃
```

一句话：

以太坊最重要的不是短期价格，而是开发者、开源生态和长期协议演进。

* * *

## **十一、我这节课第一次知道 / 需要记住的点**

### **1\. EPF 不是智能合约课**

之前容易以为 Web3 学习就是：

```
钱包
合约
DApp
NFT
DeFi
```

但 EPF 关注的是：

```
Ethereum protocol
执行层
共识层
客户端
EIP
协议升级
```

### **2\. 钱包和协议不是一层东西**

```
钱包 = 应用层工具
协议 = 链本身的规则和运行机制
```

钱包是在用以太坊，EPF 是研究以太坊本身。

### **3\. 客户端不是“前端客户端”**

这里的客户端不是普通 app client，而是运行以太坊节点的软件。

比如：

```
go-ethereum / geth
```

### **4\. Go 不是 Godot**

Go 是 Golang，是后端和基础设施常用语言。

Godot 是游戏引擎，常用 GDScript。

### **5\. 小 PR 比大 PR 更适合新手**

不是越大越厉害。

新手应该：

```
scope 小
PR 小
方向连续
review 容易
逐步积累
```

### **6\. EPF 面试更像聊天和理解检查**

不是算法八股，而是看：

```
是否理解 Ethereum
是否真的感兴趣
是否知道自己想研究什么
是否能讲清楚方向
```

* * *

## **十二、后续 To-do**

### **1\. 加入社区**

EPF / Ethereum protocol Discord：

```
<https://discord.gg/8RPnPGEQtJ>
```

老师 X：

```
<https://x.com/JackCC60>
```

EPF Wiki：

```
<https://epf.wiki/#/README>
```

### **2\. 下一步学习路线**

```
先看 EPF Wiki
↓
补 Ethereum 执行层 / 共识层基础
↓
了解客户端是什么
↓
了解 EIP 是什么
↓
每周写一篇学习 update
↓
选一个小问题域
↓
找 1-2 个小 issue 试着理解
```

### **3\. 可以继续问的问题**

```
怎么判断自己更适合执行层还是共识层？
新手第一个小 PR 应该怎么找？
docs / test 类 PR 算不算有效贡献？
怎么让 2-3 个小 PR 看起来有连续性？
如果代码不强，应该先补代码还是先读 specs？
```
<!-- DAILY_CHECKIN_2026-07-08_END -->

<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

* * *

## **1\. Web3 实习怎么找**

Web3 现在行情一般，实习岗位没有以前好找，尤其是技术岗和运营岗都更依赖 **内推 / 社群 / 线下活动**。

比较有效的方式：

**TG 求职群 / Web3 招聘群**

可以先去观察岗位 JD，了解不同岗位到底在做什么。

**线下活动 / 黑客松 / 会议**

很多机会来自线下认识的人。Web3 圈子很看重信任，见过面、一起做过项目、在社群里活跃过，更容易被推荐。

**内推比海投更有效**

但内推的前提是自己要有项目、能力或个人标签，不然别人没有理由推荐。

* * *

## **2\. Web3 运营岗需要什么**

运营不是单一岗位，里面可以分很多方向：

**用户增长**  
负责拉新、转化、提升用户数量。

**社群运营**

维护群聊氛围、组织活动、提高用户活跃度。

**活动运营**

策划线上线下活动，比如 AMA、分享会、任务活动、空投活动。

**内容运营**

做 Twitter / 小红书 / 抖音 / TG 内容，输出项目观点或行业内容。

对新人来说，可以先从 **自己起号** 开始：

-   找一个定位
    
-   持续发内容
    
-   分享 Web3 / AI / 项目学习记录
    
-   有一点流量或内容积累后，可以写进简历
    

简历里可以写：

-   自己运营过的账号
    
-   做过的内容数据
    
-   参加过的活动
    
-   社群贡献
    
-   黑客松经历
    
-   项目经历
    
-   对 Web3 某个方向的理解
    

* * *

## **3\. Web3 技术路线**

技术方向可以先从基础开始：

**Solidity 基础语法**  
先学会智能合约怎么写。

**DApp 开发**

学习前端如何连接钱包、调用合约、完成链上交互。

**项目实操**

Web3 更看重项目经验，不只是理论。学校课程如果只讲哈希算法、比特币结构，但不做项目，对就业帮助有限。

比较重要的是：

**能不能做出东西，而不是只会背概念。**

* * *

## **4\. Web3 找工作的核心逻辑**

Web3 圈子比较小，很多机会不是公开投简历来的，而是通过：

-   社群混脸熟
    
-   线下认识人
    
-   黑客松组队
    
-   朋友推荐
    
-   助教 / 老师 / 同学介绍
    
-   持续输出内容
    

所以求职重点不是单纯投简历，而是建立信任和可见度。

可以理解为：

**能力 + 项目 + 圈子曝光 = 更容易拿到机会**

* * *

## **5\. 个人 IP / 内容输出的重要性**

Twitter、小红书、抖音、TG 都可以成为展示自己的地方。

内容方向可以根据个人定位来做：

**技术型定位**

分享开发过程、合约学习、DApp 项目、踩坑记录。

**运营型定位**

分享活动复盘、社群增长、项目观察、用户增长思考。

**Research 型定位**

分享项目分析、赛道观察、数据分析、Web3 趋势判断。

**学生成长型定位**

分享从 0 学 Web3、参加训练营、做项目、找实习的过程。

重点是先让别人知道：

**这个人正在认真进入 Web3，并且有持续产出。**

* * *

## **6\. Web3 安全意识**

新人很容易被钓鱼网站骗，尤其是空投、领福利、钱包授权类网站。

常见风险：

-   点陌生链接
    
-   连接钱包
    
-   授权恶意合约
    
-   钱包资产被自动转走
    
-   U 卡 / 礼品卡被盗
    
-   交易所、邮箱、身份信息被关联攻击
    

所以新人必须注意：

**陌生链接不要点，钱包不要乱连，授权一定要看清楚。**

Web3 里钱一旦被链上转走，追回非常困难。

* * *

## **7\. 区块链专业和就业现实**

即使是区块链专业，最后真正做区块链相关工作的人也不多。

很多人会转向：

-   普通程序员
    
-   AI
    
-   销售
    
-   主播
    
-   考研
    
-   考公
    
-   其他互联网岗位
    

原因是区块链行业岗位本身不多，而且对项目经验、圈子资源和实战能力要求比较高。

所以学历或专业不是最关键，真正重要的是：

**项目经验、实操能力、社群连接和持续学习能力。**

* * *

## **8\. 黑客松项目案例：喵星智能体**

他们之前做过一个 **AI + Web3 + 宠物硬件** 项目，叫“喵星智能体”。

核心想法：

通过摄像头、声音识别、AI 分析猫咪行为和叫声，判断猫咪需求，比如饿了、想吃东西、状态异常等。

项目流程大概是：

**采集猫咪声音 / 行为数据**  
通过摄像头、麦克风等设备收集信息。

**AI 分析猫咪意图**

把猫叫声转成声纹向量，再结合 RAG 和模型判断猫咪可能想表达什么。

**多轮确认需求**

如果模型不确定，会继续通过互动判断猫咪需求。

**主人审批**

当系统判断猫咪想买东西，比如猫粮或零食，会推送给主人确认。

**链上支付 / 存证**

通过预言机和智能合约，把审批、支付、履约流程连接起来。

* * *

## **9\. 这个项目为什么要用区块链**

有人问：这个项目用互联网也能做，为什么要用区块链？

可以理解为，区块链在这里不是为了炫技，而是用于：

-   支付流程
    
-   链上存证
    
-   订单记录
    
-   权限审批
    
-   透明履约
    
-   多方共享可信数据
    

不过这也提醒一个点：

**Web3 项目不能为了上链而上链，必须说明区块链解决了什么现实问题。**

* * *

## **10\. 对学生最有用的启发**

这场分享真正有用的点是：

**Web3 找机会不能只靠投简历。**

更现实的路径是：

先学习基础 → 做一个项目 → 参加社群 / 黑客松 → 输出内容 → 认识人 → 获得内推或合作机会。

对于学生来说，最应该积累的是：

-   一个能讲清楚的项目
    
-   一段持续学习记录
    
-   一个内容账号或作品集
    
-   一些社群参与经历
    
-   一点行业方向判断
    
-   基础的安全意识
    

* * *

* * *

# **Alu 老师分享会笔记：AI Agent 安全**

## **1\. 今天主题**

这次分享主要讲 **AI Agent 安全**，特别是 Agent 在拥有钱包、API、系统命令、邮箱、数据库等权限后，可能带来的风险。

以前 Web3 安全更多是：

**私钥 / 合约漏洞 / 钓鱼链接 / 恶意交易**

但 AI Agent 时代变成：

**Agent 会自动调用工具、执行命令、操作资产，所以风险从“用户点错”变成“AI 自动做错”。**

* * *

## **2\. 核心观点**

AI Agent 最大的风险是：

**无法审计的自主性**

也就是它可能会：

-   自己理解任务
    
-   自己决定下一步
    
-   过度执行
    
-   产生幻觉
    
-   跳过安全规则
    
-   认真地做错事
    

以前 AI 幻觉只是回答错；

现在 Agent 幻觉可能是 **删数据、转资产、泄露 key、执行危险命令**。

* * *

## **3\. Agent Guard 是什么**

**Agent Guard** 是 GoPlus 做的 AI Agent 安全工具。

它主要保护：

-   Agent 系统
    
-   多 Agent 协作
    
-   工具调用
    
-   MCP / skills
    
-   API key
    
-   钱包权限
    
-   运行环境
    

目标是把 Agent 从“裸奔状态”变成“受控状态”。

* * *

## **4\. 主要风险案例**

### **过度授权**

Agent 如果权限太大，就可能被滥用。

例如：

-   被拉进群后，别人让它搜索 password 文件
    
-   Agent 删除邮件，用户喊 stop 也停不下来
    
-   Agent 拿到高权限 token 后，9 秒删除生产数据库
    

启发：

**Agent 只能给最小权限，敏感操作必须二次确认。**

* * *

### **Prompt Injection**

攻击者把恶意指令藏在网页、教程、代码、评论里。

人看不到，但 Agent 读取内容后可能执行。

例如：

-   假装成 Base 链教程，诱导 Agent 转账
    
-   用摩斯密码隐藏转账指令，让 AI 解析后执行
    

启发：

**外部内容不能直接信任，Agent 读到的不一定都是安全信息。**

* * *

### **恶意 skills / MCP**

Agent 常常需要安装插件、skills、MCP 服务。

但这些扩展也可能是木马。

可能窃取：

-   钱包私钥
    
-   API key
    
-   SSH 凭证
    
-   交易所 key
    
-   本地文件
    

启发：

**排名高、下载量高不代表安全，安装前需要扫描。**

* * *

### **供应链与系统漏洞**

有些风险不是用户主动造成的，而是来自：

-   默认无密码
    
-   网关暴露
    
-   依赖包污染
    
-   没有锁版本
    
-   软件更新中招
    

启发：

**开发者要锁依赖版本，用户要用官方渠道并及时更新。**

* * *

## **5\. Agent Guard 的能力**

Agent Guard 主要可以做：

-   **实时防护**：执行危险命令前拦截
    
-   **恶意插件扫描**：安装前检查 skills / MCP
    
-   **安全体检**：检查配置、权限、依赖风险
    
-   **威胁情报**：更新最新攻击规则
    
-   **自定义策略**：根据自己的使用场景设置安全规则
    

* * *

## **6\. Q&A：能不能用 Agent 管 Agent？**

有人问：能不能用多个 Agent 互相监督，比如一个执行，一个审核？

这个思路是合理的，属于 **多 Agent 协作治理**。

但问题是：

如果安全规则只写在 prompt 里，Agent 可能会忘记或跳过。

所以真正安全的方式不是只靠另一个 Agent 审核，而是要把规则放在：

**权限层 / 系统层 / 运行时防护层**

也就是：

**不能只告诉 Agent 要小心，而是要让它没有机会乱来。**

* * *

## **7\. 我的个人收获**

这次分享让我意识到，AI Agent 安全和 FinTech / Web3 关系很紧。

并且未来，AI 安全问题是一个发展方向领域。

未来如果 Agent 可以自动转账、调用钱包、操作 API，那安全就不只是技术问题，而是金融科技应用能不能真正落地的问题。

我这周做的网站项目，也让我开始思考怎么把区块链和现实连接起来，让不了解 Web3 的普通人也能使用。

但前提是，这些应用必须足够安全、可控、低门槛。

* * *
<!-- DAILY_CHECKIN_2026-07-09_END -->

<!-- DAILY_CHECKIN_2026-07-10_START -->
# 2026-07-10

打卡一下
<!-- DAILY_CHECKIN_2026-07-10_END -->

<!-- DAILY_CHECKIN_2026-07-11_START -->
# 2026-07-11

周末也要打卡吗 打卡一下
<!-- DAILY_CHECKIN_2026-07-11_END -->

<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

周末打卡
<!-- DAILY_CHECKIN_2026-07-12_END -->

<!-- DAILY_CHECKIN_2026-07-13_START -->
# 2026-07-13

打卡
<!-- DAILY_CHECKIN_2026-07-13_END -->

<!-- DAILY_CHECKIN_2026-07-28_START -->
# 2026-07-28

继续推进不要鸽我项目中～
<!-- DAILY_CHECKIN_2026-07-28_END -->

<!-- DAILY_CHECKIN_2026-07-29_START -->
# 2026-07-29

今日也是学习啊
<!-- DAILY_CHECKIN_2026-07-29_END -->
<!-- Content_END -->
