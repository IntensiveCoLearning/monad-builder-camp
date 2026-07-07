---
timezone: UTC+8
---

# wsui3y-spec

**GitHub ID:** wsui3y-spec

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
今天主要完成了 Web3 开发环境和工具的准备，并参加了Jack老师的会议和晚上的 Co-learning。分享会主要介绍了普通开发者如何逐步参与以太坊协议层开发，了解了 EPF、Client、EIP、Consensus 等概念。Co-learning 则进一步梳理了智能合约开发和部署流程。

我今天更多时间花在自己动手实践上，完成了第一个 Solidity 智能合约的部署。

整个流程重新走了一遍：

1.使用 Remix 编写并编译 Solidity 合约

2.使用 Browser Extension 连接 MetaMask，并切换到 Monad Testnet

3.部署合约到 Monad Testnet

4.调用 write function（leaveMessage） 完成留言，再调用 read function（getLastMessage、getLastSender） 验证数据是否成功写入和读取

5.获取 Contract Address、Deployment Transaction Hash、Interaction Transaction Hash

6.创建 GitHub 仓库，编写 README，对项目进行整理和记录。

过程中也顺便弄清楚了几个之前一直比较模糊的概念：

1.Contract Address：合约部署到链上后生成的唯一地址，用于后续与合约交互。

2.Deployment Transaction Hash：部署合约对应的链上交易记录，可以用于查询部署是否成功。

3.Interaction Transaction Hash：每次调用 write function 都会生成一笔新的交易，也会对应一个唯一的 Transaction Hash。  
[4.read](http://4.read) function 不会产生链上交易，只负责读取数据；write function 会修改链上状态，因此需要钱包签名并支付 Gas（测试网使用测试币）。

今天最大的收获还是第一次完整走通了编写合约 → 编译 → 部署 → 调用合约 → GitHub 整理项目这一整套流程。中间遇到了不少问题，比如 Remix 和 MetaMask 的连接、README 的编写、GitHub 上传等，但都通过查资料和不断尝试解决了。相比单纯看教程，自己完整实践一遍之后，对智能合约开发流程有了更清晰的理解，也没那么陌生了
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

今天参加了 Web3 实习计划开营仪式，并学习了 Web3 基础手册中的《前言》《区块链基础概念》和《以太坊概览》，同时了解了BuildAnything Freshman Track、Vibecoding 以及为什么要构建去中心化应用，对整个 Web3 的学习路线有了初步认识。

学习过程中，我了解到 Web3 与传统互联网最大的区别在于，用户可以真正拥有自己的数字资产和身份，而不是依赖中心化平台。区块链本质上是一种公开透明、不可篡改的分布式账本，其中每个区块都会通过 Hash与前一个区块相连，任何数据发生变化都会导致哈希值改变，从而保证数据的完整性。以太坊不仅是一条公链，更是支持智能合约和 DApp 运行的平台。今天还学习了 TPS、EVM、RPC以及 Indexer等基础概念，对它们在区块链生态中的作用有了初步理解。实践部分，我完成了 Monad Testnet 环境配置，创建课程钱包、添加测试网络、领取测试币，并完成了第一笔链上交易，学会了使用区块浏览器查看 Transaction Hash、Gas Fee、From、To 等交易信息，对链上交易流程有了更加直观的认识。

傍晚的分享会中，我结合自己的设计专业向老师请教了未来在 Web3 领域的发展方向，了解到设计专业除了 UI 和视觉设计外，还可以向Web3 产品设计、钱包、DeFi、NFT、DAO、产品体验、交易、签名、授权流程设计等方向发展。随后参加了 co-learning，与助教一起了解了后续任务完成流程，并学习了一些链上操作和基础知识，对后续课程安排和学习目标有了更加清晰的认识。

今天最大的收获是第一次真正完成了一笔链上交易。之前觉得钱包、测试网、Gas、Transaction Hash 这些概念很抽象，但亲自操作之后发现，很多知识只有实践一遍才能真正理解。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
