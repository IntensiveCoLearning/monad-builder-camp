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
