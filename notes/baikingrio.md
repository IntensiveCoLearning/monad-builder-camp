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
