---
timezone: UTC+8
---

# UU

**GitHub ID:** yyou89830-coder

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
## 一、核心问题：AI Agent 为什么需要支付能力？

### 1.1 背景

AI Agent 正从"对话助手"走向"自主执行者"。自主执行需要调用外部资源：API、链上交易、计算服务等等。关键矛盾在于：Agent 有"意愿"但没"钱包"。

### 1.2 典型场景

第一个场景是 API 调用付费，比如 Agent 自主调用 LLM 或数据服务等付费接口。第二个是链上交易，Agent 自主发起转账、Swap、NFT 铸造等操作。第三个是 Agent-to-Agent 结算，多个 Agent 协作完成任务后的自动分账。第四是微服务消费，Agent 按需租用计算或存储资源。第五是订阅或续费，Agent 代用户管理订阅服务。

## 二、技术方案与架构

### 2.1 整体架构

用户的授权和策略通过 AI Agent 传递给 Agent Wallet，再经由支付通道到达目标服务。Agent Wallet 内部包含几个能力模块：预充值、按需签名（Session Key）、Paymaster 代付、链上自动化支付。

### 2.2 关键组件

第一个是 Agent Wallet，也就是 Agent 专属的链上账户，可编程控制，代表方案有 Safe{Wallet} 和 ERC-4337 Account。

第二个是 Session Key，临时授权密钥，拥有有限权限和有效期，基于 ERC-4337 Session Key 实现。

第三个是 Paymaster，负责代付 Gas，让 Agent 无需持有原生代币即可发起交易。

第四个是 Gasless Transaction，也就是元交易，由用户或赞助方代付手续费，对应 EIP-2771 和 ERC-4337。

第五个是 Stream Payment，持续支付或订阅模型，代表项目有 Superfluid 和 Sablier。

第六个是 Chain Abstraction，跨链统一支付接口。

### 2.3 技术路线对比

Agent 持私钥直签的方式优点是简单直接，缺点是安全隐患大、密钥管理复杂，适合实验性或低价值场景。

智能合约钱包的方式优点是可编程控制、可恢复，缺点是 Gas 成本高、部署复杂，适合生产环境和高价值操作。

Session Key 加预算的方式优点是灵活授权、风险可控，缺点是需要链上合约支持，适合高频小额操作。

Paymaster 代付的方式优点是用户体验好、Agent 无 Gas 负担，缺点是需要 Paymaster 服务商，适合面向用户的消费场景。

链下加链上结算的方式优点是速度快、成本低，缺点是需要可信执行环境，适合低延迟场景。

## 三、项目与生态

### 3.1 相关项目一览

GoPlus —— AI Agent 安全和支付基础设施  
Chainlink —— 去中心化预言机加自动化  
Safe —— 智能合约钱包标准  
ERC-4337 —— 账户抽象标准  
Superfluid —— 流式支付

## 五、待探索问题

Agent 支付的安全边界在哪里？如何防止恶意消耗？  
链上支付和链下支付，什么时候选什么？  
Agent 之间的信任模型如何建立？  
现有的钱包基础设施是否足够支持 Agent 场景？  
中国市场或合规维度下的特殊性？
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

**收获**：AI 极大降低了智能合约开发的门槛，从写代码到部署全流程都可以在 Codex 里完成，不再需要依赖 Remix 那种老路子。

**启发**：技术基础（钱包、测试网、ABI、L1/L2）还是要先打牢，不能因为 AI 能写代码就跳过理解。安全和风险管理意识比写合约本身更重要。

**行动**：按实习计划走完三周，重点理解 Week 3 的产品化和商业化部分；同时把 Rabby 钱包换成"开发专用钱包"，规范自己的开发流程。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->


**1、创建 Mod 测试网专用钱包**

**2、完成首笔测试网链上交易**

**3、AI 生成并部署留言板智能合约**
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
