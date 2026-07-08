---
timezone: UTC+8
---

# 0xLoong

**GitHub ID:** Silence-dream

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
AI Agent 获得支付能力，本质是解决「机器如何安全、合规地代表人类完成资金转移」的问题，核心是**授权机制 + 身份凭证 + 支付编排 + 风控护栏**四层技术体系。以下从实现路径、主流协议、架构设计、安全合规四个维度系统说明。

## 一、三种核心实现路径

按自动化程度和技术深度，AI Agent 的支付能力可分为三个层级：

### 1\. 用户确认式支付（弱自动化）

这是当前最主流的落地形态，Agent 完成选品、比价、填单、生成订单等前置流程，**最终扣款仍需用户手动确认**。

-   技术实现：Agent 通过浏览器自动化（如 Playwright）或商家 API 生成订单，将支付链接 / 二维码返回给用户，由人类完成授权扣款
    
-   典型场景：美团小美导购、ChatGPT 结账推荐、各类比价 Agent
    
-   特点：不触碰支付核心链路，合规风险最低，但不是真正的 "自主支付"
    

### 2\. 委托令牌式支付（强自动化）

用户预先将支付权限以**受限令牌**的形式委托给 Agent，Agent 在授权范围内自主完成支付。这是当前行业主推的标准方案。

-   技术实现：用户向支付机构申请一张绑定了「商户范围 + 金额上限 + 有效期 + 场景限制」的虚拟卡 / 支付令牌，Agent 持有该令牌在约束条件下直接扣款
    
-   核心机制：Scope-limited credentials（范围受限凭证），类似 OAuth 的授权令牌，但作用于支付场景
    
-   典型代表：Visa Intelligent Commerce、Stripe 虚拟卡、支付宝 AI 付
    

### 3\. 原生链上支付（去中心化）

基于区块链和加密货币，Agent 自身持有钱包私钥，实现完全自主的机器对机器（M2M）支付。

-   技术实现：Agent 生成独立的链上身份（公私钥对），通过 x402 等协议在调用服务时自动完成链上结算
    
-   特点：无需人工开户、全球可用、毫秒级结算，但合规性和法币兑换是主要瓶颈
    
-   典型代表：DigiRails、Coinbase Agentic Wallets、Gate for AI Agent
<!-- DAILY_CHECKIN_2026-07-08_END -->
<!-- Content_END -->
