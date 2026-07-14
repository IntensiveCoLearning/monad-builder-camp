---
timezone: UTC+8
---

# euphoria17721-tech

**GitHub ID:** euphoria17721-tech

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-14-1784006262654-__.png)

\# Week 1 Build Log：Monad / Web3 高频交互方向学习记录

\## 1. Week 1 Build Log 初稿

\### 本周目标

Week 1 的主要目标是完成 Monad / Web3 入门实践，并初步判断自己更适合 Week 2 的 Tech、Ops 还是 Research 分轨。我重点关注 Consumer Crypto、高频交互、链上社交小游戏、排行榜任务系统等方向，思考 Monad 适合承载什么类型的用户行为。

\### 链上实践记录

本周完成或准备完成的链上实践包括：

\- 创建或准备 Web3 钱包，用于后续连接 Monad 生态应用。

\- 添加 Monad 相关网络信息，理解 RPC、Chain ID、区块浏览器等基础概念。

\- 体验测试网交互流程，包括连接钱包、确认交易、查看交易状态。

\- 学习如何通过区块浏览器查看地址、交易哈希、区块确认和合约交互记录。

\- 初步了解智能合约交互和普通数据库记录之间的区别。

钱包和交易记录：

\- 钱包地址：0x29152fB7ac93aED4336f118031f5B4BbF3A02287

\- 交易哈希 1：0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7

\- 交易哈希 2：0x3362250756726049c6cdae58b0f725ffc6fe9fd4f0cf036e21f938a54eb3f5bc

\- 交互类型：领取测试币 / 合约交互 / DApp 连接 / 排行榜或任务类交互

\- 区块浏览器链接：\[[https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7\](https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7)](https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7]\(https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7\))

\### 合约或交互记录

本周没有重点部署复杂合约，而是先从“高频交互产品应该如何上链”这个问题切入。我的理解是：不是所有用户行为都应该上链，尤其是游戏点击、临时状态、前端动画、普通 UI 反馈，这些更适合放在前端或服务器处理。

更适合上链的是：

\- 最终战绩

\- 排行榜积分

\- 任务完成记录

\- 奖励领取记录

\- NFT / 徽章 / 赛季身份

\- 用户声望或可组合的社交资产

这让我对链上产品的理解从“把功能搬到链上”变成了“判断哪些状态值得公开、可验证和可组合”。

\### 遇到的问题

1\. **一开始容易把 Web3 产品理解成普通 App 加钱包登录。**\\

后来发现真正重要的是：哪些状态需要用户拥有，哪些状态需要公开验证，哪些状态有跨应用复用价值。

2\. **对链上交互频率的理解不够清晰。**\\

高频交互并不代表每一次点击都要发交易。合理方式应该是前端快速反馈，关键结果再上链。

3\. **对 Ops 和 Tech 的边界有些模糊。**\\

比如排行榜、任务系统、Meme 战绩图既是产品功能，也是传播机制。Week 1 后我意识到 Ops 不只是发推或组织活动，而是要把传播点设计进产品循环里。

4\. **链上记录和数据库记录的取舍需要人工判断。**\\

AI 可以提供框架，但最终是否上链，要看这个记录是否涉及资产、身份、公开竞争、信任和长期复用。

\### AI 帮我解决了什么

AI 主要帮我完成了以下事情：

\- 帮我从多个方向中选择了一个更具体的高频交互场景：链上社交小游戏 + 实时排行榜任务系统。

\- 帮我拆解了为什么这个场景适合 Monad，而不是只停留在“Monad 更快”。

\- 帮我整理了 Research、Tech、Ops 三个方向的提交内容。

\- 帮我扩展了 Ops 方向的产品传播切入点，包括好友反超、战绩 Meme 化、链上赛季身份。

\- 帮我把模糊想法整理成可以提交的结构化文档。

\### 人工修改和判断记录

AI 给出的内容里，我需要人工判断和修改的部分包括：

\- 是否真的选择 Ops 作为 Week 2 主方向。

\- 哪些链上实践是我实际完成的，哪些只是计划或理解。

\- 钱包地址、交易哈希、截图是否可以公开。

\- 产品传播点是否符合 Monad 社区气质。

\- Meme 文案是否过度夸张，是否适合公开发在 X / Discord / Telegram。

\- Week 2 是否要做 Demo，还是先做用户研究和内容传播实验。

\### 我对 Monad / Web3 的理解变化

Week 1 之前，我对 Monad 的理解比较简单，主要是“高性能 EVM 链”。现在我的理解更具体了：Monad 的意义不只是更快，而是让一些过去不适合链上的高频消费级应用有机会成立。

例如小游戏、排行榜、任务系统、社交徽章、AI Agent 行为记录等场景，都需要快速反馈和低成本状态更新。如果链上确认太慢或手续费太高，用户体验会被打断，产品只能退回中心化数据库。

但我也意识到，不是所有东西都要上链。真正适合上链的是有公开验证、资产归属、身份沉淀和跨应用组合价值的结果。普通过程数据仍然可以留在前端或后端。

\### Week 2 希望深入的方向

我希望 Week 2 重点进入 **Ops 方向**，围绕“链上社交小游戏 + 实时排行榜任务系统”继续深入。

具体想做：

\- 设计一个适合 Monad 社区传播的小游戏概念。

\- 做 3–5 个 Meme 化传播模板。

\- 设计排行榜、任务、赛季徽章的运营机制。

\- 写一条 X Thread 或 Discord 活动方案。

\- 如果时间允许，配合 Tech 同学做一个简单 Demo 页面或交互原型。

\---

\## 2. 我选择 Ops / Tech / Research 的初步理由

我初步选择 **Ops 方向**。

原因是我更感兴趣的问题不是单纯“怎么写合约”，而是“什么样的产品机制会让用户愿意反复参与、分享和邀请朋友”。在链上社交小游戏这个方向里，Ops 非常关键，因为产品增长不是靠解释技术参数，而是靠用户能不能感受到竞争、身份、梗和社区参与感。

我认为 Monad 的高频交互场景需要强运营设计，例如：

\- 好友排行榜制造竞争

\- 战绩图制造传播

\- 赛季徽章制造身份

\- 社区任务制造持续参与

\- KOL 挑战制造破圈入口

这些内容更接近我想深入的方向。Tech 可以帮产品跑起来，Research 可以解释为什么成立，而 Ops 负责让用户真的愿意玩、愿意晒、愿意留下来。

\---

\## 3. 本周最重要的 1–3 个学习收获

\### 收获 1：高频交互不等于所有行为都上链

游戏点击、临时状态、动画反馈不一定要上链。真正适合上链的是最终结果、奖励归属、排行榜凭证和身份记录。这样既能保留消费级体验，也能发挥链上的公开验证和可组合优势。

\### 收获 2：Consumer Crypto 的重点是体验，而不是教育用户技术

用户不会因为 TPS 参数而留下来，用户会因为“我赢了朋友”“我上榜了”“我拿到了稀有徽章”“这张战绩图很好笑”而留下来。技术性能应该服务于更顺滑的产品体验。

\### 收获 3：Ops 应该嵌入产品机制

好的 Ops 不是后期宣传，而是从产品设计阶段就开始考虑传播。排行榜、任务、Meme 图、赛季身份、社区挑战，本身就是增长机制的一部分。

\---

\## 4. 希望助教或同伴帮助的问题

1\. 我的 Week 2 方向选择 Ops 是否合理？如果我要补一点 Tech 能力，最小 Demo 应该做到什么程度？

2\. 对于“链上社交小游戏 + 排行榜任务系统”，哪些数据必须上链，哪些数据应该留在后端？

3\. 如果做 Monad 社区传播，什么样的 Meme 风格更适合？偏竞技、偏抽象、偏交易员梗，还是偏 Monad 生态文化？
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->

完成了一次部署合约交互

![559ca49f2d530da4cf31eba2c5195760.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-13-1783925534095-559ca49f2d530da4cf31eba2c5195760.png)
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->


用 AI 生成一个最小 Solidity 合约，学习中。。。。还是不太懂

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-12-1783851047639-image.png)
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->



重新复盘听完本节课内容，系统梳理了AI智能体自主支付的完整底层基础设施。明晰了通过搭建人机共管钱包、配套风控框架，能够有效管控AI自主操作，避免无序支出；同时熟悉了各类支付相关产品、实际落地应用场景，对行业后续的发展趋势也有了整体认知。

![IMG_7852.jpeg](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-11-1783778463090-IMG_7852.jpeg)
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->




我使用 Remix 编写并编译了一个最小化的 `MessageBoard` 留言板智能合约，通过课程专用钱包连接 Monad Testnet 后完成了合约部署，并记录了部署后的合约地址和部署交易 hash。部署完成后，我调用了 `message()` 等 read function 读取链上留言内容，又调用 `setMessage()` write function 修改留言，并再次读取确认链上状态已经更新。通过这次操作，我完整体验了从 Solidity 合约源码、编译、部署、链上交互到区块浏览器验证的全过程。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-10-1783649245388-image.png)
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->





![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-09-1783564839294-image.png)![bfef306100b3094ecd492b7707f8f629.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-09-1783564846939-bfef306100b3094ecd492b7707f8f629.png)

通过这次任务，我使用 AI 生成了一个最小 Solidity 留言板合约，但没有直接把 AI 输出作为最终结果，而是对代码进行了人工阅读和检查。

我主要检查了：

-   合约是否可以编译。
    
-   函数是否符合预期。
    
-   是否存在明显权限问题。
    
-   是否存在不必要的复杂逻辑。
    
-   是否有潜在安全风险。
    
-   命名是否清楚。
    

最终判断：  
该合约适合作为 Solidity 入门练习和 AI 辅助开发练习案例。如果用于正式项目，还需要增加更多安全设计和测试。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->






学会get测试币，会看transaction 的 detail了

![a0c4f59d6e4b1c087ca59258fdf65ba6.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-08-1783517225187-a0c4f59d6e4b1c087ca59258fdf65ba6.png)![a0c4f59d6e4b1c087ca59258fdf65ba6.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-08-1783517266853-a0c4f59d6e4b1c087ca59258fdf65ba6.png)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->







普通互联网产品所有数据都存在公司后台，账号、道具平台想封就封，里面的虚拟东西也只能在这个软件里用。 链上产品数据存在全网节点，改不了，资产是自己私钥说了算，平台拿不走，虚拟资产还能跨软件流转，规则提前写死没法后台暗改。

今天学会了创建钱包和连接区块链，并且进行了第一笔交易

![bea5c285b1515f93ffc6aeb23e81a861.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-07-1783417566733-bea5c285b1515f93ffc6aeb23e81a861.png)![53e40f31bb01e124aefe33ebfb94a63c.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-07-1783417577273-53e40f31bb01e124aefe33ebfb94a63c.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->








学习

![IMG_7709.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-06-1783346226688-IMG_7709.png)

了从安装钱包插件、添加测试网、领取测试水，再到Remix部署交互合约并在区块浏览器查看交易的完整Web3实操流程。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
