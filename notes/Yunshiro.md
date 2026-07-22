---
timezone: UTC+8
---

# Jeong Un-Seo

**GitHub ID:** Yunshiro

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
onekey产品经理分享

学习了一下钱包相关 如何设计一款钱包
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-21-1784635356499-image.png)

参与coleaning 然后组队
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->


![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-20-1784548428415-image.png)
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->



### Monad 高性能应用开发最佳实践总结

**1\. 控制托管成本**  
Vercel/Railway 用起来方便但量大后可能溢价严重；AWS 等云厂商更适合高流量场景（S3+CloudFront、Lambda、ECS/EKS、RDS）,注意很多套餐是"低价引流档",超量后单价会跳涨很多。

**2\. Gas 估算优化**  
如果某个操作 gas 消耗是固定的(比如原生代币转账固定 21,000 gas),就直接硬编码,不要每次都调 `eth_estimateGas`——能显著加快钱包交互速度,还能避免某些钱包在 `eth_estimateGas` revert 时的异常行为。

**3\. 减少** `eth_call` **延迟**  
串行调用多个 `eth_call` 会因多次往返 RPC 而产生延迟,可以:

-   用 **Multicall3**(Monad 主网/测试网都已部署在 `0xcA11bde05977b3631167028862bE2a173976CA11`)把多个读请求合并成一次调用(注意 Multicall 内部是串行执行的,别塞太多重调用)
    
-   用 viem 等库的批量请求功能(`Promise.all` 自动打包成一个 batch)
    
-   高频读历史事件/派生状态的场景,直接上 indexer
    

**4\. 用 Indexer 代替反复调用** `eth_getLogs`  
文档列了一堆索引方案接入方式(Allium、Envio HyperIndex、GhostGraph、Goldsky、QuickNode Streams、The Graph、thirdweb Insight API),每个都给了 Monad 主网/测试网的 network ID 或参数(比如 chain ID 主网 143 / 测试网 10143,或者 `monad-mainnet` / `monad-testnet` 这种字符串标识,具体用哪个格式看服务商)。

**5\. 本地管理 Nonce**  
如果是手动设置 nonce(没有交给钱包托管),短时间内连续发多笔交易时不要每次都调 `eth_getTransactionCount`(会有网络请求延迟),自己在本地维护 nonce 计数。

**6\. 并发提交交易**  
多笔交易不要用 `for` 循环串行 `await` 发送,改成用 `Array.map` + `Promise.all` 并发提交,提升吞吐效率。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->




今天加入lxdao，参加了onboarding流程
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->





![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-17-1784290477168-image.png)

优化前端视觉，链上2048
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->






今天给Moss提了一条pr，给kuru适配器添加了限价单功能

[feat(kuru): add limit order support by Yunshiro · Pull Request #51 · nishuzumi/moss](https://github.com/nishuzumi/moss/pull/51)
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->







链上2048开发

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-15-1784120813826-image.png)
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->








晚上参加分享会，了解了三大赛道的职业方向和职责

今天给Moss项目提了一条issue

[https://github.com/nishuzumi/moss/issues/18](https://github.com/nishuzumi/moss/issues/18)
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->









[https://github.com/Yunshiro/Monad-Buidler-Camp-Jeong/blob/main/notes/week2/chain2048-%E9%9C%80%E6%B1%82%E6%96%87%E6%A1%A3.md](https://github.com/Yunshiro/Monad-Buidler-Camp-Jeong/blob/main/notes/week2/chain2048-%E9%9C%80%E6%B1%82%E6%96%87%E6%A1%A3.md)

链上2048游戏方案、思路
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->










今天构思了一下后续黑客松做什么内容：

1、链上五子棋继续优化，添加AI Agent对战功能

2、链上德州扑克

3、链上质押打卡
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->











做了一个链上五子棋，游玩地址：[https://gomoku-onchain.vercel.app/](https://gomoku-onchain.vercel.app/)

github repo：[https://github.com/Yunshiro/gomoku-contract](https://github.com/Yunshiro/gomoku-contract)
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->












今天晚上参加例会分享，小白从0到1部署你的第一个Dapp
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->













今天做了一个需求清单，链上五子棋：[https://github.com/Yunshiro/Monad-Buidler-Camp-Jeong/blob/main/notes/链上五子棋需求清单.md](https://github.com/Yunshiro/Monad-Buidler-Camp-Jeong/blob/main/notes/链上五子棋需求清单.md)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->














今天整理了一个周五分享笔记，repo地址：[https://github.com/Yunshiro/Monad-Buidler-Camp-Jeong/blob/main/notes/小白从0-1上线一个留言板dapp.md](https://github.com/Yunshiro/Monad-Buidler-Camp-Jeong/blob/main/notes/小白从0-1上线一个留言板dapp.md)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->















今天产出一个简单的DAPP，Monad测试网链上留言板，部署在：[https://message-board-ui.vercel.app/](https://message-board-ui.vercel.app/)

repo地址：[Yunshiro/message-board-ui](https://github.com/Yunshiro/message-board-ui)

合约地址：0x5407DA3398519b8E4fB6Ff96c46F36d2E1E85e40

体验了一下dapp的前后端交互，感觉和web2类似，只不过dapp应用的后端部署在链上，逻辑都在链上运行，而Web2在服务器当中运行

![1783416330279_a09d13b9bb5b479494730672088bd6c1.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-07-1783418252390-1783416330279_a09d13b9bb5b479494730672088bd6c1.png)![1783416360643_39b50e84557646958287422ca1056d55.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-07-1783418263107-1783416360643_39b50e84557646958287422ca1056d55.png)![1783416289749_606f7bbd91744a9d9e1c6d38fc51bc2a.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-07-1783418237725-1783416289749_606f7bbd91744a9d9e1c6d38fc51bc2a.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
















## 学习内容

1、了解钱包

2、了解Monad测试网、如何在测试网查询钱包地址、交易

3、如何通过水龙头网站获取测试币，[https://faucet.monad.xyz/](https://faucet.monad.xyz/)

4、编写留言板智能合约，利用claude code辅助

5、部署留言板合约在monad testnet上：[https://testnet.monadvision.com/address/0x5407DA3398519b8E4fB6Ff96c46F36d2E1E85e40](https://testnet.monadvision.com/address/0x5407DA3398519b8E4fB6Ff96c46F36d2E1E85e40)

## 产出成果

1、注册metamask钱包

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-06-1783325732799-image.png)

2、简单给自己另外一个钱包转账，查询交易

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-06-1783325756990-image.png)

3、根据水龙头网站获取测试代币：

![](https://cdn.nlark.com/yuque/0/2026/png/34855639/1783298654616-1909ad2a-0ebb-44d4-a5b4-73646f57e4da.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-06-1783325775814-image.png)

4、

提示词：

-   我想要写一个留言板功能的智能合约，包含读写函数的，可以写入留言、读取留言，编译器版本使用solidity-0.8.34
    
-   解释一下这个留言板合约，帮助我理解一个合约的结构，并且输出markdown文档
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Yunshiro/images/2026-07-06-1783325812108-image.png)

5、产出成果repo：[https://github.com/Yunshiro/Monad-Buidler-Camp-Jeong/tree/main/source/messageboard](https://github.com/Yunshiro/Monad-Buidler-Camp-Jeong/tree/main/source/messageboard)

包含留言板合约源码、合约解析文档、合约用法README

## 今日小结

今天简单体验了一下从创建数字钱包、领取代币、创建一个合约并且部署到Monad测试网，跑通了一个小的流程：需求->编码->部署->调用。

在链上也是有了一个永久存在的一个留言板合约。

但是对于solidity相关知识了解过少，对于其语法和相关规范还没有很熟悉，需要进一步学习。

疑问点（后续自行搜索或者询问助教老师）：

1、合约如果要修改代码后部署了还是同一个合约地址吗？

2、为什么读取查询不消耗gas费用？
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
