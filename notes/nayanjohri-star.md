---
timezone: UTC+8
---

# nayanjohri-star

**GitHub ID:** nayanjohri-star

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
今天主要完成了三部分：

1.  **MessageBoard v0.1 读写闭环**
    
    -   `getMessageCount()`：`0 → 1`
        
    -   `postMessage` 写入成功
        
    -   `getMessage(0)` 成功读取留言
        
    -   README、交易哈希和证据截图全部整理完成
        
2.  **Monad 理解任务**
    
    -   完成并提交 Tech 方向的高频交互 Demo 功能清单
        
    -   设计 `Monad Pulse Board`
        
    -   理解 Monad 的低延迟、高吞吐、EVM 兼容性及链上/链下边界
        
3.  **确定技术路线**
    
    -   后续走 Tech / Dev
        
    -   下一步开发 MessageBoard v0.2 链上点赞功能
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

今天复盘了 AI Agent 安全课程，并参加 Week 1 Co-Learning。理解了 Agent 安全不能只依赖 Prompt，而要通过最小权限、人工审批、工具层拦截、沙箱、审计和恢复建立模型外防线。通过同学的 DApp 和 Monad Buddy 案例，我进一步区分了前端演示与真实链上交互：真实上链需要钱包签名、正确网络、可查询交易和合约状态变化。下一步完成 MessageBoard 读写闭环，并选择 Week 2 的 Research / Ops / Dev 方向。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->


\## 2026-07-09 Web3 学习打卡

今天主要完成了 Solidity 合约从编写、编译到 Monad Testnet 部署的完整流程。

学习内容包括：使用 AI 生成一个最小留言板合约 `MessageBoard`，理解合约中的 `struct`、数组存储、事件 `event`、写入函数 `postMessage`、读取函数 `getMessageCount` 和 `getMessage`。随后在 Remix 中完成编译，并连接 MetaMask 的 Monad Testnet 环境进行部署。

实践中我确认了部署网络不是 Remix VM，而是 Monad Testnet；部署时 Value 为 0，合约成功创建。部署后在 Remix 中看到 `Deployed Contracts 1`，并通过 MonadVision 查询到交易状态为 `Success`。

今日部署结果：

\- 合约名`MessageBoard`

\- 网络：Monad Testnet

\- 合约地址`0x2E1b2776877b5d10238F5Cde30F9f70919E99adC`

\- 交易哈希`0xa8193f11431c7bd3c0f2e269a77b7f72cbe73f252cc4b3ced2eb7bed0e047069`

今天也进一步理解了链上产品和普通互联网产品的区别：链上产品的数据和操作结果会公开记录在区块链上，交易需要钱包签名并消耗 Gas，用户需要自行保管资产和私钥，产品设计也必须更重视公开性、不可篡改性和安全边界。

人工检查方面，我重点确认了合约是否满足任务要求、是否部署到正确网络、是否避免加入复杂权限控制，并检查了 AI 生成代码中可能存在的权限、越界读取、Gas 成本、隐私泄露和时间戳使用等问题。

明天计划继续学习合约交互、读取链上数据，并尝试调用自己部署的合约发布一条留言。

![cf8784af7772e29d6fc696f33d76ba51.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/nayanjohri-star/images/2026-07-09-1783606940985-cf8784af7772e29d6fc696f33d76ba51.png)![65dcfc34a3e864d9ea20f06e0f1adb5b.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/nayanjohri-star/images/2026-07-09-1783606956033-65dcfc34a3e864d9ea20f06e0f1adb5b.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->



今天学习了两场直播：第一场是 FluxA 关于 AI Agent 支付体系的分享，理解了 Agent 支付为什么需要身份、预算、风控、审计、撤销和结算能力，也了解了 X402、人机共管钱包、AgentCard、Monetize Gateway、Payment Link、Pay to Agent 等概念。

第二场 Co-Learning 重点是 Web3 入行经验，强化了 Git/GitHub、`.env` 安全、AI 辅助开发、项目经验、远程工作和接单风险意识。明天会继续进入 Remix / Solidity / Monad 合约部署，把今天的概念落到一个可验证的小项目里。、Pay to Agent 等概念。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->




今天完成了 Monad Builder Camp Week 1 的链上交互最小闭环：创建并使用课程测试钱包，添加 Monad Testnet，领取 Testnet MON，并完成了一笔 Monad Testnet 转账测试。

链上验证记录：

\- 交易哈希`0xdd874fb3e668806aefc9e10c114c5b8bce56f771de052ca9cef6efd5e196a7da`

\- MonadVision 链接：[https://testnet.monadvision.com/tx/0xdd874fb3e668806aefc9e10c114c5b8bce56f771de052ca9cef6efd5e196a7da](https://testnet.monadvision.com/tx/0xdd874fb3e668806aefc9e10c114c5b8bce56f771de052ca9cef6efd5e196a7da)

今天理解到：钱包地址是公开链上身份入口，交易哈希可以作为链上操作证明；Gas 是执行链上交易需要消耗的费用；区块浏览器可以用来验证交易状态、区块、Gas 和账户活动。私钥、助记词、真实 `.env`、API Key 都不能写入公开仓库或打卡内容。

晚上还学习了 Co-Learning 直播和 JackCC 的以太坊协议层 EPF 分享。直播让我明白，后续 Remix 部署合约本身也是一次链上交易，需要保存合约地址、部署交易哈希，并用区块浏览器验证。EPF 分享给我的启发是：协议层学习不需要一开始就追大 EIP，而是先理解 Ethereum 这台机器，从执行层/共识层地图、specs、客户端仓库、小 issue 和小 PR 慢慢进入。

明天计划：继续补 Week 1 的安全、Remix、Solidity 入门和 Monad 合约部署内容，尝试用 Remix 在 Monad Testnet 部署一个最小合约，并记录合约地址、交易哈希和学习问题。

![000247fcdaa3a05f5eb5fcf1d2cd3bee.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/nayanjohri-star/images/2026-07-07-1783432670188-000247fcdaa3a05f5eb5fcf1d2cd3bee.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
