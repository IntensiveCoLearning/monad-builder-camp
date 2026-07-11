---
timezone: UTC+8
---

# kkiii2025

**GitHub ID:** kkiii2025

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
打卡
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

**注意 Vibecoding 智能合约风险：**AI 智能体编写 UI 代码的能力，要远比编写智能合约代码的能力强得多。Solidity 的训练数据更少，逻辑更专业，错误也更难被发现。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->


## 7.09 学习笔记

Replit 网站无需梯子。

**Monad 的由来：**

2009 年，Bitcoin 证明“价值可以在没有中间人的情况下转移”，但不能在它上面构建完整应用。

2015 年，Ethereum 证明“去中心化计算确实可行”，它的目标不是让去中心化应用用起来像普通软件一样快。

2025年，Monad 继承了 Ethereum 建立起来的一切，然后继续问下一个问题：**如果去中心化应用快到像普通软件一样，会发生什么？**

**Monad 设计目标**：10,000 TPS、400ms 出块、800ms 确定性最终性（deterministic finality）

**主网和测试网：**Mainnet 使用具有真实价值的代币；testnet 上的代币没有价值，是用来安全实验的。

**Chain ID** 是一个用来标识某条区块链的唯一编号。

**RPC（Remote Procedure Call，远程过程调用）**是钱包或应用用来与区块链对话的一个 URL，相当于“给区块链电话”。RPC 也分 Public RPC 和 Paid RPC，面向真实用户上线时使用Paid RPC，一般测试就用Public RPC。

**Faucet（水龙头）**是一个免费发放 testnet 代币的网站。官方 Monad faucet： [faucet.monad.xyz](http://faucet.monad.xyz)

**Block explorer** 用于查询链上的任何东西：交易、钱包、合约，以及它们的历史。

大多数钱包也支持通过 [chainlist.org](http://chainlist.org) 一键添加。

**Supabase：**一项免费服务，开箱即用地提供**数据库**和**文件存储**。

**数据库**存放结构化数据（文本、数字、日期、各种关系），**文件存储**存放复杂的文件（路径 URL 放在数据库，文件字节放Sotrage）。

**给应用加上 Storage：**

在侧栏的 Storage **创建一个 Bucket**，暂时保持 Public bucket 关闭

**设置一条 Storage 策略**，New policy → 选 template → 选 Allow access to JPG images in a public folder to anonymous users → Allowed operation模块中勾选 SELECT（读取 / 下载）和 INSERT（上传。（后面在 authentication 那节课再把它收紧）

**需要在表里有一个地方存放每个上传文件的路径**。告诉智能体我要加一列来引用文件，它会给我用于输入 SQL Editor 中去运行的内容

接下来只需告诉智能体：**在应用里上传文件、显示这些文件、限制用户可以上传什么**相应的需求
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->



打！
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->




打卡
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->





Kiiyasun完成2026.07.06打卡！
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
