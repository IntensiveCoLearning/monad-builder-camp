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
