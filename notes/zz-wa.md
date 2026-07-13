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
# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
打卡
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->

-   完成初始的 demo 0 ，从 0 到完成第一条链上留言
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->


> 做一个面向 Web3 零基础用户的 Monad 新手活动：用户在引导下完成一次真实的“链上留言”，获得自己的第一次合约交互凭证。

## 活动主题

**My First Monad Message｜留下你的第一句 Monad 链上寄语**

活动目标：

> 让没有 Web3 经验的新手，不只是“看介绍”，而是在理解每一步的情况下，亲自完成一次合约交互。
> 
> 从一场真实的交易中理解web3
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->



简单整理一周的输入

-   Monad新手链上交互体检记录+Ops方向初步思考
    
-   遇到的问题总结
    
-   对于web3以及monad的进一步思考
    

demo 0 初始构思

ops方向

从一笔真实的交易看懂链上交互--一个面向Monad新手用户的Ops引导方案
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->




## 7.08

AI agent 由聊天-做事

但当涉及“付钱”，就会产生响应问题，

-   AI 能不能自己付款？
    
-   它最多能花多少钱？
    
-   它会不会因为幻觉乱买东西？
    
-   付款记录能不能查？
    
-   …..
    

### so—搭建一套Agent支付系统

### Agent支付核心架构

-   全链路能力分层 —就是从用户下命令，到 Agent 完成付款，中间所有环节都要管住。
    

1.  接入层：支持Claude gpt等各类Agent载体对接，用户仅需要输入简单Prompt即触发Agent完成全流程操作、
    
2.  核心能力层:拆解为身份标识，预算管控，风险校验……
    

### x402协议+批量结算协议

X402协议要做的事情就是当Agent访问一个资源时，如果资源需要付费，则服务器直接返回—需要付款，当Agent付款之后，再继续访问

让Agent可以自然的为内容，API,数据，服务进行小额付款

批量结算协议就是—先记录很多笔小的交易，最后一起结算

**三类支付解决方案**

| 开发者货币化方案 | 通用电商支付方案 | Agent 点对点转账 |
| --- | --- | --- |
| 开发者的一些东西—API，MCP Skill AI工具等等，如果之前想逃卖给用户，可能需要自己制作网站，支付系统，登陆系统，现在的方案时开发着可以把自己的API /Skill包装成一个Agent可以自动购买和调用的产品—-让开发着把自己的工具卖给Agent | 针对金额不固定的零售场景，自动生成支付链接，付款方仅需一键确认即可完成交易 | 基于 Agent 唯一 ID 直接完成转账操作，适配 Agent 之间自主发起的交易需求。 |

## Monad的理解

**Monad 是一条 Layer 1 公链，也就是一条底层区块链。**

它不是一个 App，也不是交易所。

可以把它理解成和 **Ethereum、Solana** 类似的一类东西，都是区块链网络。

更准确地说：

**Monad 想做的是一条高性能、低延迟、EVM 兼容的公链。**
<!-- DAILY_CHECKIN_2026-07-08_END -->

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
