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
# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
1.服务器服务器

存放你整套网站代码、数据库、图片的云端电脑，24小时在线。

• 本地电脑：关机别人就打不开

• 服务器：永久在线，全球能力访问

新手免费服务器平台

• Vercel（最适合NFT/DApp前端）

• Netlify（适合静态展示页面）

• 铁路（唯一能办公前端+仓库+数据库）

2\. 域名

一串好记的网址，代替难记的IP数字地址。

• 普通域名：.com .xyz

• Web3专属域名：.eth（ENS域名，本身是NFT）
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->

看了分享会
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->


今天参加了 Web3 术语“谁是卧底”Co-Learning，重点辨析了三组概念：钱包与交易所的核心区别是私钥和资产控制权在用户还是平台手中；私钥用于控制具体账户并签名，助记词可恢复一组账户，二者都不能截图、上传或提供给 AI；ERC-20 用于可互换、通常可拆分的同质化代币，ERC-721 用于具有独立 Token ID 的非同质化资产。课程还让我学会用“做什么、谁会用、用错有什么风险”三问法理解技术概念。下一步继续推进 MessageBoard v0.2，并学习合约测试和代币标准。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->



完成人工智能学习：**从研究之路到公共知识：AI时代Web3研究员的成长**
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->




今天学习了节点开发、交易生命周期和 Web3 产品表达。一笔交易会从 DApp 经 RPC 进入节点交易池，通过 P2P 网络传播，被打包后由 EVM 执行并更新状态，最终经共识获得最终性。节点性能不能只看 TPS，还要关注存储、同步成本、带宽和运行门槛。

Co-Learning 通过角色扮演说明：介绍链上产品时，要先讲用户场景和实际收益，再讲透明、不可篡改和可追溯；面对媒体与评审，还需解释刷票、规则透明度和利益冲突。AI 可以辅助编码、模拟用户质疑和优化表达，但技术事实、安全边界与验收结果仍需人工判断。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->





今天完成了 Week 1 的学习复盘和 Mini Demo 0 整理。我把本周的钱包创建、Monad Testnet 普通交易、MessageBoard 合约编译部署`postMessage` 写入`getMessageCount()` 与 `getMessage(0)` 读取过程整理成完整 Build Log，并复盘了网络确认、Remix 交互异常、区块哈希与交易哈希混淆等问题及修复过程。

同时，我把 MessageBoard v0.1 整理成 Mini Demo 0：补齐作品说明、真实链上操作、AI 辅助部分、人工判断与修改、公开交易链接、README 和截图证据。我进一步明确了 AI 可以帮助生成代码、解释概念和整理材料，但钱包签名、网络与交易核对、代码安全、链上隐私和链上/链下边界必须由人判断。

我确认 Week 2 继续走 Tech / Dev 技术路线。下一步计划把 MessageBoard 升级为 v0.2，先设计“每个地址对同一条留言只能点赞一次”的最小功能，并学习使用 Foundry 或 Hardhat 编写自动化测试，再继续完成前端事件监听和可展示 Demo。

\## 今日完成

\- \[x\] Week 1 Build Log 初稿。

\- \[x\] 回顾本周链上实践、问题、AI 帮助和人工判断。

\- \[x\] 整理 3 条核心学习收获和 3 个求助问题。

\- \[x\] MessageBoard v0.1 Mini Demo 0 提交稿。

\- \[x\] 补齐作品链接、方向选择、作品集一句话和 Week 2 问题。

\- \[ \] Week 1 复盘任务平台提交。

\- \[ \] Mini Demo 0 平台提交。

\## 本周最重要的收获

1\. 链上开发需要同时保存源码、合约地址、交易哈希、浏览器结果和读取证据。

2\. AI 可以提高生成和排错效率，但交易签名、安全检查和产品边界不能交给 AI 决定。

3\. Monad 的高性能只有放进高频、低成本、可验证的产品场景中，才会转化为真实用户体验。
<!-- DAILY_CHECKIN_2026-07-12_END -->

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
