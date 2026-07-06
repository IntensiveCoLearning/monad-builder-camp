---
timezone: UTC+8
---

# N109-cat

**GitHub ID:** N109-cat

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
# 分享会内容

学习路径分享：智能合约——前端——后端

黑客松流程：组队——方案讨论——分工——做demo

听力如何练习：多给自己创造一个有英文的环境，不带字母的英文电影/美剧

行动是第一步，先动起来再说  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/N109-cat/images/2026-07-06-1783338652528-image.png)

# co-learning

week1任务

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/N109-cat/images/2026-07-06-1783340976070-image.png)

黑话：领水

合约：把用户的留言永久写入区块链，任何人都能读取，无法删除或篡改。

# 手册学习

## 区块链基础知识

区块链是一种**去中心化的分布式账本技术**，用于在网络节点之间安全、透明且不可篡改地记录事务数据。每条链由一系列按照时间顺序相连的“区块”组成，每个区块内部包含了多笔交易数据及元数据，确保了数据记录的完整性与可追溯性。

区块：由交易信息和前一个区块的信息摘要组成，有存储限制，打包的速度很快

链：链接两个区块链的链条

### 区块链特点

1.  不可篡改性
    
2.  透明性
    
3.  匿名性
    
4.  快速交易
    
5.  去中心化
    

分布式网络记账模式：保证交易信息未被篡改和有效真实

比特币：**Bitcoin**，是一种基于区块链技术的**去中心化数字货币**。一种不依赖银行或政府发行、通过区块链网络记录和转移的数字货币

### 区块链组成

区块链由两个核心组成：

1.  去中心化的网络和区块链
    
2.  维持网络运行的代币激励
    

### 区块链运行流程

1.  用户发起交易
    
2.  交易广播
    
3.  节点验证
    
4.  打包成块
    
5.  链接上链
    
6.  奖励发放
    

### 区块链类型

区块链根据访问权限与治理模式大致分为公链、联盟链、私链

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/N109-cat/images/2026-07-06-1783340738837-image.png)

按照去中心化程度从高到低排列

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/N109-cat/images/2026-07-06-1783340705619-image.png)

web2 vs web3.0 vs web3

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/N109-cat/images/2026-07-06-1783340885663-image.png)

## 以太坊

### 基本名词概念

定义：一个开源的去中心化区块链平台，通过其原生加密货币以太币（Ether，简称 ETH）提供去中心化的以太虚拟机（EVM）来处理点对点合约。

智能合约：是存储在区块链上的可执行代码，能够在满足预设条件时自动执行操作，无需人工干预。这一特性使得以太坊不仅是数字货币的载体，更是构建去中心化应用（Dapps）、去中心化金融（DeFi）、非同质化代币（NFT）等生态系统的基础设施。

### 以太坊和比特币的对比

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/N109-cat/images/2026-07-06-1783341238891-image.png)

### 以太坊发展历程

![以太坊升级路线图](https://web3intern.xyz/assets/ethereum-roadmap_01-Bi2f1MRr.jpg)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/N109-cat/images/2026-07-06-1783341311889-image.png)

### 以太坊生态

太坊的生态系统由多层架构组成，包括 **L1（主网）、L2（二层扩展解决方案）、侧链（Sidechains）**

**以太坊分层架构：**应用层、协议层、拓展层

### 以太坊文化和价值观

以太坊的文化深受 **密码朋克运动（Cypherpunk）** 影响，体现了对技术赋权个人、重塑社会协作的愿景

核心价值观：

1.  **去中心化治理（Decentralization）**
    
2.  **无需许可与开放性（Permissionless & Open）**
    
3.  **无需许可与开放性（Permissionless & Open）**
    
4.  **密码朋克精神（Cypherpunk Ethos）**
    
    -   代码即法律：用算法和数学构建信任
        
    -   密码学保护隐私和自主权
        
    -   技术驱动的社会变革，而非政治手段
        
5.  **公共物品导向（Public Goods Orientation）**
    
6.  **可持续发展理念**
    

### 以太坊核心机制

1.  **账户系统**：包含由私钥控制的 **外部账户（EOA）** 和由智能合约代码控制的 **合约账户（CA）**
    
2.  **Gas：**当使用自己的 EOA 钱包发起一笔交易（比如转账或操作合约），这件事当然 **不会是免费的**，**你需要支付“燃料费”——也就是 Gas。**
    
    -   **总费用 = Gas Limit （限额）× Gas Price（单价）**
        
    -   Gas 的存在有两个目的：
        
        -   **激励矿工/验证者**：你给得越多（Gas Price 越高），他们越愿意优先处理你的交易。
            
        -   **防止资源滥用**：如果有人想让合约死循环，Gas 会用光，交易失败，系统不会被拖垮。
            
    -   **Gas Price组成**
        
        -   基础费用（Base Fee）
            
        -   小费（Tip）
            
3.  EVM（Ethereum Virtual Machine）是 **以太坊的“大脑”**，是专门用来**运行智能合约的虚拟计算机**。它运行在每个节点上，确保整个网络在处理代码时，**结果都一致、可信任**。
    
    -   **核心特点：**图灵完备**、**全球同步、隔离安全
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
