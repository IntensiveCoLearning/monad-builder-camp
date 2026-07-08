---
timezone: UTC+8
---

# xxxkuze-tools

**GitHub ID:** xxxkuze-tools

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
今天学习用 AI 生成一个最小 Solidity 合约，我选择了每日打卡合约，写了个提示词，让chatgpt给我生成 Solidity ^0.8.20 的代码，CheckIn包含totalCount（累计打卡次数），lastCheckIn（最后打卡时间），streak（连续打卡天数），然后是三个函数，checkIn() -> 用户每天只能打卡一次，canCheckIn() -> 判断今天是否还能打卡，getMyRecord() -> 查看自己的打卡信息，AI生成的合约代码，人工检查了Remix 能编译通过，三个函数与 Prompt 一致，数据类型正确，没有不必要的库，变量命名和注释读得懂。然后部署到Remix。大概的流程：Remix 新建 `ChecklnContract.sol` → 编译 `0.8.20` → **Deploy & Run Transactions** → Browser Extension 连 Rabby → 选 Monad Testnet → Deploy，交互Remix **Deployed Contracts** 里：蓝点函数 **call**（如 getMyRecord），橙点函数 **transact**（如 checkIn）。Write 后在区块链浏览器查看确认交易 Status 为 Success。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

听了7.6晚上参加「Co-learning」线上活动，自己尝试在浏览器上安装MetaMask插件，创建钱包，添加 Monad Testnet 网络，添加成功之后，复制钱包地址，打开区块浏览器，我看到的页面信息，**Address**：我的钱包地址，一串 `0x` 开头的字符，**Balance**：当前账户余额，刚创建时应该是 `0 MON`**，Transactions：**历史交易列表，空钱包时这里是空的。从而**我理解链上产品和普通互联网产品最大的区别是**：普通互联网产品的数据主要存在平台自己的服务器里，而链上产品的关键操作会记录在区块链上，可以通过区块浏览器查询。接着继续了解**行业赛道全览和Web3 工作方式。**
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->



今天观看了开营仪式回访内容，学习了web3实习手册，了解了区块链基础概念，以太坊概览，下一步会继续学习了解行业赛道全览，web3的工作方式。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
