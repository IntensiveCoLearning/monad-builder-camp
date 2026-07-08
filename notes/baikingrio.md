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
