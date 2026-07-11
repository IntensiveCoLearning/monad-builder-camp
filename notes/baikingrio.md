---
timezone: UTC+8
---

# Quinn

**GitHub ID:** baikingrio

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
今天继续完善 Monad 上的 ERC-4337 学习实践，重点是 Sponsor Authorization 的安全边界。

我做了一个只在服务端运行的 Sponsor 授权接口。它的作用是为符合规则的 UserOperation 签发 Paymaster 授权，但不会直接广播交易，也不提供公开的 Relay 或 Bundler。

这次我重点补了几个安全限制：

1\. Sponsor 私钥只保存在服务端，前端和浏览器钱包都不会接触。

2\. Sponsor 授权会绑定用户账户、nonce、调用内容、Gas 参数、有效期、链 ID 和 Paymaster 地址，避免授权被换成其他交易内容。

3\. 服务端会检查实际的调用内容，只允许当前学习场景中的 checkIn 操作。

4\. 增加了 Gas 和费用上限，避免一次授权消耗过多 Paymaster 预算。

5\. Sponsor 接口默认关闭，只用于本地学习，不作为公开的免 Gas 服务开放。

今天让我更清楚地理解到，ERC-4337 的 Sponsor 不只是“帮用户付 Gas”。如果没有成本限制、精确调用校验、短时有效期和私钥隔离，Paymaster 很容易变成被滥用的资金入口。

下一步我准备部署新版 Paymaster 到 Monad Testnet，再在本地完成一次“Sponsor 服务签名 → Paymaster 验证授权”的完整验证。在补齐认证、限流、配额、预算管理和熔断机制之前，不会公开部署 Sponsor 服务。

今日学习笔记：

[https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-11.md](https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-11.md)
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

今天继续完善 Monad Testnet 上的 ERC-4337 实验，并学习和实践了 ERC-1363。

在 ERC-4337 部分，我进一步理解了 Paymaster 的作用不只是代付 Gas，更重要的是限制赞助的范围和风险。通过把账户、调用内容、nonce、有效期等信息绑定到 sponsor 授权中，可以避免授权被复用到其他交易里。

在 ERC-1363 部分，我完成了一个“质押 + 收益分红”的练习。用户可以通过 transferAndCall 在转入 Token 的同时完成质押，减少一次 approve 和 stake 的交互。分红则按照用户的质押份额累计，用户可以领取收益，也可以提取本金。

这次实践让我更清楚地感受到：ERC-4337 主要改善账户和 Gas 体验，ERC-1363 则适合把 Token 支付和后续业务逻辑放到同一笔交易中。后面我想继续完善前端交互，并把 ERC-4337 的 sponsor 和 ERC-1363 的业务场景结合起来，探索更顺畅的链上使用体验。

今日学习笔记：

[https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-10.md](https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-10.md)
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->


今天主要围绕 ERC-4337 Account Abstraction 做了三组连续实验，把智能账户从“能执行 UserOperation”继续扩展到更接近真实应用体验的能力，重点学习和实践了三个方向：

1\. counterfactual deployment / initCode

我通过 CREATE2 factory 实验理解了“账户还没部署，但地址已经可以提前确定”的机制。实际场景可以理解为：一个新用户第一次进入链上活动 App 时，前端可以先计算出他的智能账户地址，这个地址虽然还没有部署合约，但已经可以先接收测试币、空投或活动资产。等用户第一次真正点击领取徽章、签到或转账时，UserOperation 带上 initCode，EntryPoint 会在同一笔 handleOps 中先部署账户，再执行操作。

这让我理解到，initCode 的作用不是普通 calldata，而是“第一次 UserOperation 里携带的账户部署说明书”。

2\. Paymaster / sponsored UserOp

我又做了一个简单的 Paymaster 概念验证，用来理解“项目方替用户支付 gas”的流程。实验中，Paymaster 先往 EntryPoint 的 deposit 里充值，然后 UserOperation 通过 paymasterAndData 指向这个 Paymaster。用户仍然需要自己签名授权操作，但 gas 费用由 Paymaster 的 deposit 承担。

我用“链上活动 App 赞助新用户第一次签到”来理解这个功能：用户不需要先准备 MON，也可以完成第一次链上操作。这个能力很适合新用户 onboarding、免费领取徽章、积分任务、游戏新手任务等场景。

同时也意识到 Paymaster 不是“无限免费 gas”，生产环境一定要设计规则，比如赞助哪些账户、哪些函数、每个用户赞助几次、是否需要后端签名、防重放和额度限制等。

3\. Session Key / 受限权限

最后我做了一个 session key 实验。这个实验中，owner 仍然保留完整权限，但额外授权了一个临时 session key。这个 session key 只能调用指定合约的 checkIn(string) 函数，不能转 MON，不能调用其他合约，也不能替代 owner。

我用“链游 / 活动 App 的每日签到”来理解这个功能：如果用户每次签到、移动、领取积分都要弹钱包签名，体验会很差。session key 可以让前端在低风险范围内帮用户签 UserOperation，同时不交出完整账户控制权。

今天的链上 proof 包括：

\- Counterfactual / initCode：预测地址、预充值、首次 UserOperation 部署账户并执行成功

\- Paymaster / sponsored UserOp：Paymaster deposit 充值成功，UserOperation gas 由 Paymaster 赞助

\- Session Key / 受限权限：session key 只调用允许的 checkIn(string)，checkInCount = 1，nativeSpent = 0

本地测试也从 5 个增加到 12 个，覆盖了 factory、Paymaster、session key 权限限制等关键路径。

今天最大的收获是：ERC-4337 不只是把 EOA 换成智能合约账户，而是把“账户创建、gas 支付、操作权限”都变成可以编程的产品能力。结合 Monad 这种高性能 EVM 链，这些能力很适合链游、任务平台、活动签到、积分系统、社交应用等高频交互场景。

但同时也要注意安全边界：体验优化不能等于无限授权。无论是 Paymaster 还是 Session Key，都必须有明确的白名单、额度、有效期、防重放、可撤销机制和监控。

今日学习笔记：[https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-09.md](https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-09.md)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->



今天继续做链上实践和学习记录，重点做了 ERC-4337 最小实践，从 EntryPoint v0.7 迁移到 Monad Testnet 官方 EntryPoint v0.8，并记录两者差异。

合约实验项目：  

[https://github.com/baikingrio/monad-builder-camp/tree/main/experiments/monad-playground](https://github.com/baikingrio/monad-builder-camp/tree/main/experiments/monad-playground)

今天完成的主要内容：

1\. 新增了 Minimal4337PracticeV08.s.sol 脚本，把原来的 ERC-4337 最小实践迁移到 EntryPoint v0.8。

2\. 使用课程专用账户重新部署了一个最小智能账户，并通过 v0.8 EntryPoint 成功执行了一次 handleOps。

3\. 对比整理了 v0.7 和 v0.8 的实践差异，包括 EntryPoint 地址、UserOperation 核心流程、initCode 安全、EIP-7702、paymaster、unused gas penalty 和错误处理等内容。

4\. 把今天的实践结果和理解补充到了 daily 学习笔记中，形成可复盘的 Build Log。

本次 v0.8 实践记录：

EntryPoint v0.8：  

0x4337084D9E255Ff0702461CF8895CE9E3b5Ff108

Minimal4337AccountV08：  

0xff76634Ea6D1407266a0fc6096f042416F258E24

部署交易：  

0x9d11f70e00619503e0f1aaa13dd7f8cf7a097099ba95a71a21df9fd7817dc539

handleOps 交易：  

0xa7d556f9d4e30119b654a7c248281916bac433413e99b4ebeecdebdef374c34d

UserOpHash：  

0xf29daf942acc744a63dcb17a772e77524ff78ad0b10190cf168f086d7ec72864

Explorer：  

[https://testnet.monadvision.com/tx/0xa7d556f9d4e30119b654a7c248281916bac433413e99b4ebeecdebdef374c34d](https://testnet.monadvision.com/tx/0xa7d556f9d4e30119b654a7c248281916bac433413e99b4ebeecdebdef374c34d)

今天的收获是：v0.7 和 v0.8 在最基础的 ERC-4337 路径上是连续的，仍然是 UserOperation -> EntryPoint -> validateUserOp -> execute。但 v0.8 不只是换了一个 EntryPoint 地址，它更面向真实产品场景，比如支持 EIP-7702、改进 initCode 安全、修复 paymaster 计费问题，并让错误处理更清晰。

这让我更理解 Monad 和 Account Abstraction 结合的价值：Monad 提供高吞吐、低延迟和 EVM 兼容的执行环境，而 ERC-4337 提供更灵活的账户交互方式。两者结合后，更适合继续探索任务系统、排行榜、小游戏、链上社交、AI Agent 操作等高频交互场景。

下一步计划继续做 initCode / counterfactual deployment，让账户可以在第一次 UserOperation 里通过 factory 创建出来，更接近真实 AA 钱包的 onboarding 体验。

今日学习笔记：[https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-08.md](https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-08.md)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->




今天学习了 ERC-4337 Account Abstraction 和 thirdweb 的 Session Keys 文档，重点理解了智能账户、UserOperation、Bundler、EntryPoint、Paymaster 之间的关系。

我的理解是，ERC-4337 让钱包不再只是一个由私钥控制的 EOA，而是可以变成有自定义验证逻辑和权限管理能力的智能账户。用户发起的不是普通交易，而是 UserOperation，由 Bundler 打包，再通过 EntryPoint 统一验证和执行。Paymaster 可以在特定条件下帮用户代付 gas，从而改善新用户进入链上应用时必须先准备 gas 的体验。

今天另一个重点是 Session Key。Session Key 可以理解成主账户临时授权出来的受限密钥，它可以在一定时间和权限范围内代表账户执行操作，但不会暴露主钱包私钥。它适合用于链游、频繁交互、移动端应用等场景，可以减少用户每一步都手动签名的操作成本。

结合 Monad Builder Camp 的学习，我觉得这些内容对理解未来 Monad 生态里的钱包体验和应用交互方式很有帮助。高性能链不仅需要更快的执行环境，也需要更顺滑的钱包和账户体验。ERC-4337、Paymaster、Session Key 这类账户抽象能力，可以让用户更容易进入链上应用，也能让开发者设计出更接近普通互联网产品体验的 Web3 应用。

今日产出：整理了 ERC-4337 与 Session Keys 的完整学习笔记，重点理解了账户抽象的执行流程、核心组件、Session Key 权限设计，以及它们对 Monad 生态应用交互体验的启发。

今日学习笔记：[https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-07.md](https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-07.md)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->





今天是 Monad Builder Camp 的 Day 1，我先搭建了独立学习仓库，并在 `experiments/monad-playground` 里创建了一个 Foundry 实验项目。

今天完成了一个简单 ERC20 合约 `CampToken` 的编写、部署和源码验证。合约已经部署到 Monad Testnet，地址是 `0xef694e5411A70d05270722A38Ea9Ef242E3afcf2`，部署交易是 `0x59d72a3596bc405f024b01a31a257a8ab2eed6dcb652473af924f312c53a5ff2`，并且在 MonadVision / Sourcify 上完成了 `exact_match` 源码验证。

今天最深的感受有两个：第一，Monad 测试币领水很方便，对新手非常友好，不会一开始就卡在 faucet 门槛上；第二，源码验证不需要额外 API Key，可以直接用 `forge verify-contract` 走 Sourcify / MonadVision 完成，这让合约开源和验证变得很顺滑。

对我来说，今天不只是部署了一个 ERC20，而是完整走通了 Monad Testnet 的基础开发路径：领水、部署、浏览器查看、源码验证和 README 记录。下一步我会继续做第一笔交互交易和合约读写操作，把它整理成 Week 1 的任务 proof 和 Mini Demo 0 材料。

今日学习笔记：[https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-06.md](https://github.com/baikingrio/monad-builder-camp/blob/main/daily/2026-07-06.md)
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
