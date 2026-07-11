---
timezone: UTC-7
---

# Zixie Zheng

**GitHub ID:** zzxwjm

**Telegram:** 

## Self-introduction

从Measurement到web3.

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
打卡
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

**分享笔记：一个新手如何理解 Web3**

**1\. 我的起点**

我的背景是社科phd，做测量和方法。

在开始学习之前，我对 Web3 的理解很窄：

> Web3 = 炒币 / 投机 / 金融交易 / 很技术 / 离普通生活很远

但这几天做完基础任务后，我的理解发生了一些变化。

我现在会先把 Web3 理解成：

> 一套公开、可验证、由用户自己控制入口的数字状态系统。

也就是说，Web3 不只是关于“钱”，也关于“哪些数字状态可以被公开记录、验证和执行”。

**2\. 我做了什么**

这几天我主要完成了几件事：

-   学习了 Web2 / Web3、区块链、钱包、交易、gas、区块浏览器、智能合约等基础概念。
    
-   创建了课程专用 MetaMask 钱包，并添加 Monad Testnet。
    
-   完成了第一笔 Monad Testnet 测试网转账。
    
-   在区块浏览器里查看了 transaction hash、from、to、value、gas、status。
    
-   用 AI 辅助生成了一个最小 Solidity demo：Onchain Todo。
    

这些任务让我把很多抽象概念连起来：

> 链记录状态，钱包控制身份，交易改变状态，gas 支付改变状态的成本。

**3\. 我最大的概念转变**

我原来以为“交易”就是买卖。

但在链上，transaction 更像是：

> 一个地址签名后，向某条链提交一次状态改变请求。

所以：

-   转账是交易，因为余额状态改变了。
    
-   部署合约是交易，因为链上新增了一个合约。
    
-   调用合约函数是交易，因为合约状态可能改变了。
    
-   创建 onchain todo 也是交易，因为 todo 列表改变了。
    

这让我意识到：

> Web3 的核心不只是交易资产，而是通过交易改变公共状态。

**4\. Web3 和 Web2 / 现实生活的不同**

在 Web2 和现实生活里，我们习惯相信中心化机构或平台：

-   银行记录余额。
    
-   学校认证身份。
    
-   平台保存账号和数据。
    
-   App 告诉我们某个操作是否成功。
    

Web3 试图改变一部分信任结构：

> Web2 更像是相信平台；Web3 更像是相信规则、签名、代码、共识和公开记录。

但这不是简单地“更方便”。  
相反，它常常意味着：

-   用户要自己保管助记词。
    
-   用户要知道自己在哪条链上操作。
    
-   用户要理解签名和交易风险。
    
-   用户要承担更多责任。
    

所以我现在觉得：

> Web3 是用更高的个人责任，换取更强的控制权和可验证性。

**5\. 我问过的几个问题**

这几天我问过几个对我很关键的问题：

-   ETH 和 Ethereum 是什么关系？
    
-   为什么会有很多不同的链？
    
-   为什么 Web3 里的交易这么重要？
    
-   哪些状态需要上链，哪些应该留在链下？
    
-   链上记录是真的，但它到底代表什么？
    

其中最重要的是：

> 可验证不等于自动有意义。链上记录是真实的，但记录的意义仍然需要解释。

**6\. 我的测量背景带来的疑问**

因为我的背景和心理测量、研究方法有关，所以我会自然关心：

> 链上记录到底测量或代表了什么？

比如：

-   一个地址交易很多次，代表真实参与，还是只是刷交互？
    
-   一个地址持有 badge，代表真的学习过，还是只是完成任务？
    
-   一个地址参与投票，代表治理贡献，还是只是为了奖励？
    
-   一个项目链上活跃度很高，代表生态健康，还是代表激励被套利？
    

但我也不想把传统心理测量直接套到 Web3 上。因为 Web3 的主体不一定是现实生活中的“人”，而可能是地址、钱包、合约，或者多个地址之间的关系。

**7\. 阶段性总结**

我现在感觉，Web3 当下的很多实践主要受金融和计算机科学两套heuristic method影响。金融启发式让它关心资产、激励、成本、收益和风险；计算机科学/密码学启发式让它关心规则、协议、签名、验证和自动执行。

这两套思路让 Web3 很擅长处理“开放系统中如何协调不互相信任的人”这个问题。但它们也有边界：它们能很好地记录和激励行为，却不一定自动解释这些行为的意义。

所以我的疑问是：当 Web3 用金融激励和代码规则把行为记录下来之后，我们是否还需要另一层问题意识：这些被记录和激励的行为，到底代表什么？
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->




## What I Did Today

Today I continued Monad Builder Camp Week 1 and moved from concept learning into real onchain practice.

I learned and discussed:

-   why there are many different blockchains
    
-   how to understand chains as different public worlds or games
    
-   why Web3 does not simply put everything into one unified world
    
-   how Web3 relates to Web2
    
-   what blockchain state means at a beginner level
    
-   why a transaction changes the state of a specific chain
    

I also completed hands-on work:

-   created a course-only MetaMask wallet
    
-   added Monad Testnet
    
-   confirmed MetaMask was on Monad Testnet
    
-   got testnet MON
    
-   completed my first Monad Testnet transfer
    
-   opened the transaction in MonadVision
    
-   recorded the transaction hash and key fields
    

## Proof

Course wallet:

```
0x2596976c6D2D5a301eFc3833e2749bDF368223e5
```

First Monad Testnet transaction:

```
0xfcc719809d3984548a88305e8753d47539eb251fd800136a8c6bf24eeea441bd
```

Explorer link:

```
https://testnet.monadvision.com/tx/0xfcc719809d3984548a88305e8753d47539eb251fd800136a8c6bf24eeea441bd
```

Transaction summary:

-   Status: Success
    
-   From: `0x2596976c6D2D5a301eFc3833e2749bDF368223e5`
    
-   To: `0xb11256348b1cba14E77Ca541A9b4a7E2Da2ecC49`
    
-   Value: `0.01 MON`
    
-   Transaction fee: `0.002162856067344 MON`
    
-   Gas used: `21,000`
    

## Problems I Had

### 1\. I did not understand why there are many chains

At first, it felt strange that Web3 has many separate chains. If everything were stored together, it seemed like it would be more convenient.

Resolution:

I started using the "different public game worlds" analogy. Different chains have different rules, costs, communities, speeds, and opportunities. People use a chain when the thing they want to do lives in that world.

### 2\. I did not understand blockchain state

The word "state" felt abstract because I do not have a finance or computer science background.

Resolution:

I understood state as "the current situation of a world." In a game, this means level, inventory, gold, and completed quests. Onchain, this means balances, ownership, votes, contract data, and transaction results.

### 3\. The website said "No wallet detected"

When trying to connect from the Codex in-app browser, the page said:

```
No wallet detected. Please install a compatible wallet to continue
```

Resolution:

The Codex in-app browser does not have the MetaMask extension. Wallet actions need to happen in Chrome or another browser where MetaMask is installed.

### 4\. I was confused by network vs token

I saw MON in the wallet and wondered why there was no "Monad Testnet" in the receiver field.

Resolution:

Monad Testnet is the network. MON is the token used on that network. The receiver field is only an address.

Mental model:

```
current network = the world I am acting in
MON = the currency in that world
receiver address = the account I send to
```

## What I Learned

The most important sentence from today:

```
链记录状态，钱包控制身份，交易改变状态，gas 支付改变状态的成本。
```

I also learned:

-   public addresses can be shared, but private keys and seed phrases must never be shared
    
-   a wallet does not store assets; the chain records balances, and the wallet controls keys
    
-   a transaction can be inspected through a block explorer
    
-   failed transactions can still consume gas because the network may spend computation before failure
    
-   Web3 is not just "trading"; it is about verifiable state and user-controlled actions
    

## How I Felt

At the beginning, the concepts felt scattered and abstract. I was especially confused by why Web3 needs many chains and why transactions are so central.

The "different games / public worlds" analogy made things easier. After completing the first transaction and seeing it in MonadVision, the idea became more concrete: the transaction was not just a button click in a website. It was a signed action that changed state on Monad Testnet and left a public record.

I still feel that Web3 has many layers and the user experience is not very intuitive yet. But I now have a clearer first mental map, and I completed a real onchain action safely with a course-only wallet.

## Next

-   Practice reading more transaction details in the explorer.
    
-   Learn what `from`, `to`, `value`, `gas price`, and `gas fee` mean in more depth.
    
-   Continue Week 1 tasks toward AI + Solidity + contract deployment.
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->






学习ai agent支付，先打卡
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->







和ai讨论心理测量+web3，得到结论为理念很好，但是实践层面上可能没有现存的启发式测量有削。  
今日目标：开始实践
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->









### **Web3实习计划开营式（录播）：**

比特币本质上是一种区块链，强共识但是低性能。Monad在不降低性能的情况下，提高了区块链的上限。Monad同时进行“共识”和“执行交易”。

### **实习手册：**

进行入门导读的时候，我有几个问题：  
1\. 为什么需要有区块链？  
2\. 为什么需要更快的交易速度？  
3\. 如何在保证区块链上的信息是公开透明的同时，又是匿名的？

沿着这几个问题，我初步理解区块链的核心动机，是在没有中心化信任机构的情况下，通过分布式共识与密码学机制维护一个不可篡改的账本，从而降低对银行、平台等中介的依赖，并支持数字价值的唯一性与可验证转移。为了成为真正的基础设施，它还必须提升交易效率，否则难以进入现实支付与金融场景。

在机制层面，区块链通过地址与公私钥体系实现伪匿名，使交易公开透明但不直接绑定现实身份，从而在可验证性与隐私之间做出折中。同时，它的“可信性”更多来自**_分布式结构_**与**_经济激励假设_**，而非绝对安全，本质是降低单点操控风险，而不是消除操控可能。

但我也仍然存在疑问：这种“信任代码规则”的结构，是否真的比信任人更可靠？它在实践中依然可能被算力、资本或开发者影响，从而形成新的权力集中。此外，所谓规则不可随意更改，也引出了系统如何演化的问题：当进化依赖社区共识时，它是否只是把政治过程技术化，而并未真正消除权力结构？从这个角度看，区块链更像是一种权力的重新分布，而不是权力的消失，这种“技术中心主义”的可信性仍值得进一步质疑。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
