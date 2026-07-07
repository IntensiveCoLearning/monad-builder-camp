---
timezone: UTC+8
---

# zz

**GitHub ID:** zz-wa

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
-   **进入 Web3 与链上世界**
    

准备好课程所需要的钱包理解基本知识，添加 Monad Testnet 网络，

普通互联网产品 VS 链上产品

普通的互联网产品围绕 平台账号+平台服务器 运转

链上产品围绕 钱包地址+区块链合约运转

-   **完成第一笔测试网交易**
    

MonTransfer 流程

交易涉及的名词

```
transaction Hash  这笔交易的为一编号，就像快递单号
Method  MonTransfer 转账
From 发送方  To 接收方 
Gas/Fee 为交易支付的测试手续费 
Block 区块 区块链会把很多的交易打包成为一快一块的记录 ，块就是区块
Block Confirmations 区块确认数 表示这比交易之后又接链多少新的区块，确认数越多，越稳定
```

-   AI辅助开发合约并进行部署
    

智能合约--放在链上的程序 使用钱包去调用这个链上的程序

使用 Remix IDE 编写了一个简单的 MessageBoard 留言板合约，并将它部署到了 Monad Testnet。

合约的作用是：用户可以调用 postMessage 函数写入留言，合约会记录留言发送者地址、留言内容和时间戳。之后可以通过 getTotalMessages 或 getMessage 读取链上的留言数据。

合约地址 VS 部署hash VS 交互交易 hash

合约地址表示这个合约部署在链上的位置；部署交易 hash 是创建合约那笔交易的记录；交互交易 hash 是调用 write function 时产生的交易记录。read function 只读取链上数据，不会产生新的交易 hash
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->


-   区块链概述
    
-   以太网坊概述
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
