---
timezone: UTC+8
---

# Qy

**GitHub ID:** pillowtalk-Qy

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
今天主要围绕链上产品、交易理解、Solidity 合约和 Monad Testnet 开发流程完成了一轮基础练习。

首先，我梳理了链上产品和普通互联网产品的区别。链上产品的关键数据、资产和规则更多依赖区块链和智能合约，而不是只存在平台数据库里。用户通常通过钱包登录，资产归属更接近用户自己，规则也更公开、可验证，但同时交易确认、gas 成本和不可轻易撤销等问题也会影响体验。

然后，我学习并整理了交易中的基础字段，包括 from、to、value、gas、手续费和交易状态。我的理解是：from 是发起交易并支付 gas 的地址，to 是接收方或合约地址，value 表示随交易发送的原生币数量。交易即使失败，也可能消耗 gas，因为链上节点已经执行了计算，只是最终状态被回滚。

在合约开发部分，我使用 AI 辅助生成了一个最小 Solidity 合约 OnchainTodo。这个合约支持用户添加 todo、切换完成状态、读取 todo 和查询 todo 数量。我没有完全照搬 AI 输出，而是进行了人工检查和判断：确认合约可以编译，函数符合预期，用户只能修改自己的 todo，没有引入不必要的管理员权限，并且增加了输入长度限制，避免空内容和过长字符串上链。

今天还整理了 Monad Testnet 的部署与交互流程。由于没有在环境中直接使用课程钱包私钥，所以没有实际代替钱包完成部署，但已经准备好了 Remix 和 Foundry 两种方式的 README v0.1，包括编译、连接 Monad Testnet、部署合约、调用 read/write function、保存合约地址和交易 hash 的步骤。

最后，我选择了“链上小游戏排行榜 / 任务系统”作为适合 Monad 的高频交互场景。这个方向需要频繁提交分数、更新排名、领取 Badge 和挑战好友。如果链慢或手续费高，用户体验会很差。Monad 的高性能、低延迟和 EVM 兼容性，可以帮助这类 Consumer Crypto 产品获得更接近互联网小游戏的体验，同时保留链上排行榜、奖励和身份记录的公开可验证特性。

**今日产出**

-   完成链上产品基础理解整理
    
-   完成交易字段与 gas 机制理解
    
-   生成并人工检查最小 Solidity 合约
    
-   本地完成合约编译验证
    
-   整理 Monad Testnet Remix / Foundry 部署流程
    
-   完成一个适合 Monad 的高频交互应用方向分析
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
