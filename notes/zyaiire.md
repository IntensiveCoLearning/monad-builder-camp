---
timezone: UTC+8
---

# Shize Li

**GitHub ID:** zyaiire

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 从“裸奔”到受控：AI Agent高危，案例解析与安全构建

AgentGuard

ai agent安全防线看起来 coding agent 也会做

安全加固建议：

1.  最小权限原则：仅授予完成任务所需的最低权限
    
2.  Human-in-the-Loop：设置安全基线，对资金转账、邮件删除等敏感操作人工复核
    
3.  布置安全监控工具：如 AgentGuard 等专业工具
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->

# AI Agent 如何拥有支付能力？

## 嘉宾自我介绍

来自 FluxA，负责基础生态。让 Agent 能够非常安全的花钱是非常重要的。而且对于 Agent 来说它需要完成一系列的操作。比如 Agent 会进行一些小微的操作（比如 0.01USD），这对于传统支付来说手续费特别贵，同时 Agent 也有一个问题是它没有身份证（鉴权问题），这个体系是没法办银行卡的。主要分享的内容是为了达到这个目的，需要做哪些**基础设施。（让 Agent 走进商户，个人的实际支付场景）**

## 现状

目前 Agent 进行支付场景存在如下问题：

1.  Identity
    
2.  Authorization
    
3.  Credentials
    
4.  Speed
    
5.  Granularity
    

比如在 chatbox 上输入自己的身份信息是比较危险的

FluxA 不仅包含了钱包，而是一整套协议（最重要的是风控），主要涉及如下的场景

1.  Agent to Agent
    
2.  Agent to Merchant
    
3.  Agent to Tool
    
4.  Transfers and Tipping
    

核心团队是蚂蚁出来的

我觉得比较有新意的是 mandates，有点像给 agent 一部分预算，然后预算内的消费都由 agent 自行处理

其中比较有意思的是 x402 payment。

这是一次性的 agent card

layer 2 和 layer 1 之间的关系是什么（嘉宾说 layer 2 处理的交易会最后集中到 layer 1 进行确认）
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->


epf都是自学的，epf 和智能合约相比更偏向于密码学和数学的知识，会更难。  
可以做执行层和共识层去找上面的 issue 然后去进行修。

最开始可以选择简单 issue，通过提 pr 的过程然后让自己的理解更完备。

ETH这个机器可以简单分为

1.  账户、交易、gas
    
2.  执行层
    
3.  共识层
    
4.  节点和网络
    

metamask 等钱包就是构建在以太坊之上的应用
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->



很早之前用过 okx wallet，第一次用小狐狸。  
  
之前还是主要通过交易的用户用钱包，完全没有想过区块链底层技术有这么多的方向。  
  
后续从技术角度感觉前端+ui 反而比智能合约工程师之类的更好找工作
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->




# DevRel的成长之路 — 从 Builder 到生态链接者

## 成长经历

24 年 9 月份有了第一份工作。**智能合约工作不是很好找。**

什么是 DevRel

即开发者关系，感觉像是带有技术背景的运营人员

SVP 是一个 AI Native 链，有 agent的 mcp 能够有强力的安全边界。

现在运营的话工作机会会更多一些。

对于新人来说智能合约一般没有工作机会，得要 3～5 年的

### 黑客松冠军经验

项目背景: 是万物皆可预测的项目
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->





# Web3 暑期实习计划 - Monad Buidler Camp

**21 天做出你的第一个链上产品，第 4 周进入 Monad Hackathon，第 5 周整理成作品集。适合想进入 Web3 / AI × Crypto 方向的开发、运营、研究和内容创作者。**

# 开营仪式

日期: 7.6 - 8.2

后续有实习的对接，三周的共学+一周的黑客松+一周的工作的对接

会提供岗位以及生态大使的机会

研究向是新增的

优秀的黑客松产品的话有孵化

不怎么会推荐交易所的工作，希望以以太坊基金会为目标，还会有 Monad 的大使计划

## MONAD介绍

更高吞吐，更低成本

交易不止包含转账，还有 swap、landing 等高复杂的交易

**活跃验证者:** 已经质押足够资产，并正在参与区块生产、区块验证、共识投票等工作的验证者节点。验证者可以理解为是区块链的记账员，验证过程是基于一套持续运行的验证程序的。

通过软件优化希望达到家用消费级的电脑就能跑MONAD 节点

后续可以研究一下现有的共识算法和 MONAD 的共识算法的区别

### Asynchronous Execution

### Optimized Parallel Execution

优先完成的交易会把输出暂存，然后纠正输入。相当于是并行执行+顺序纠错的方案替代掉串行执行

### MonadDB

什么是 ssd？

后续可能demo 应该也是基于 monad，可以先装了试试看

## 开发框架与Monad 兼容体系

Monad 兼容所有主流的开发框架

应该是和链上支付相关的

## 工具与基础设施

### Builder Camp机会

Delta V 类似孵化器的感觉，需要定期更新产品的动态。如果更新比较有质量，Delta V 会进行推荐
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
