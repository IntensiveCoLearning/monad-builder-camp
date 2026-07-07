---
timezone: UTC+8
---

# honora888

**GitHub ID:** honora888

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 延续2026/7/6——

# 三、NFT 入门实操笔记

## 1\. NFT 基础概念

NFT = Non-Fungible Token 非同质化代币

-   同质化代币（ETH/BTC）：可等价互换、可分割
    
-   NFT：唯一标识、不可拆分、一对一确权（头像、艺术品、游戏道具、会员凭证）
    

## 2\. NFT 四大价值

1.  所有权链上确权，平台无法删除没收资产
    
2.  交易全程透明可追溯，版权验证成本极低
    
3.  跨平台流通（游戏资产不再绑定单一游戏）
    
4.  衍生价值：社区门票、版权分润、实体资产代币化
    

## 3\. 完整 Mint 铸造六步流程

1.  安装 MetaMask 浏览器钱包，保存 12 位助记词（资产唯一凭证，丢失无法找回）
    
2.  学习 Web3 安全：钓鱼、私钥泄露、恶意签名风险
    
3.  钱包存入 ETH，用于支付 Gas 手续费
    
4.  DApp 点击 Connect Wallet 连接钱包（等同于 Web3 登录）
    
5.  选择素材组装 NFT，确认 Gas 并签名交易，上链铸造
    
6.  在钱包 / 市场查看、交易 NFT
    

## 4\. Web3 钱包安全核心规则

1.  助记词、私钥绝不分享、不上传云端
    
2.  冷钱包（硬件）存大额资产，热钱包仅小额日常使用
    
3.  陌生网站签名、空投授权谨慎，极易被盗资产
    
4.  区分官方域名，警惕仿冒钓鱼网站
    

## 5\. 行业术语

-   DYOR：自行研究项目，勿盲从热点
    
-   FOMO：踏空焦虑，追高风险
    
-   FUD：恐慌情绪，低位抛售
    

* * *

# 四、Vibecoding AI 开发新模式

## 1\. 定义（2025 诞生）

无需手动写代码，以**自然语言描述需求**，AI 智能体自动完成全流程开发；人是产品指挥官，AI 是工程团队。

### 聊天机器人 vs AI 智能体

-   聊天机器人：仅返回文字回答，无操作能力
    
-   AI 智能体：创建文件、编写代码、运行调试、自动修复 Bug
    

## 2\. 标准开发循环（核心流程）

描述需求 → AI 构建代码 → 预览效果 → 反馈修改 → 迭代重复

## 3\. 主流开发工具

1.  Replit（浏览器云端沙箱，新手首选，隔离本地文件）
    
2.  OpenAI Codex / Claude Code（终端本地 AI 编码）
    

## 4\. 安全红线：提示注入攻击

攻击者在素材、文件中隐藏劫持指令，控制 AI 读取本地文件、泄露密钥；

### 安全最佳实践

1.  全部在云端沙箱运行 AI，禁止本地电脑直接运行智能体
    
2.  绝不向 AI 输入真实私钥、API 密钥、账号密码
    
3.  AI 生成代码上线前人工完整审查
    

## 5\. 能力边界

### 可实现

网站、DApp 前端、数据库配置、交互组件、Bug 修复、Web3 页面开发

### 局限

无法自动保证生产级安全；复杂业务仍需持续迭代；不能替代需求设计能力

## 6\. 核心技能：高质量提示词 Prompt

差示例：帮我做 NFT 网站

优质示例：深色主题 NFT 铸造 DApp，集成 MetaMask 连接，上传图片生成 ERC721，页面适配手机，使用 Ethers.js 交互以太坊。

* * *

# 五、完整 Web3 技术栈汇总

## 1\. Web2 传统开发栈

`React + Node.js + MySQL`

## 2\. Web3 去中心化 DApp 开发栈（核心）

`React（前端页面） + Ethers.js（链交互工具） + Solidity（智能合约后端） + IPFS（去中心化文件存储）`

分工拆解：

1.  React：用户可视化界面、按钮交互
    
2.  Ethers.js：连接 MetaMask、读写智能合约、发送链上交易
    
3.  Solidity：部署以太坊的业务逻辑（转账、NFT、交易）
    
4.  IPFS：存储图片、元数据、前端静态资源，替代阿里云等中心化服务器
    

## 3\. Web 3.0 语义网栈

`Python + RDFLib + SPARQL`（W3C、DBpedia 知识图谱体系）

* * *

# 六、高频核心术语速查表

1.  分布式账本：全网多节点同步完整账本
    
2.  哈希：数据唯一指纹，篡改即变更
    
3.  共识机制：PoW 挖矿 / PoS 质押验证
    
4.  验证者：PoS 网络自动运行程序记账节点
    
5.  智能合约：链上自动执行代码，DApp 后端
    
6.  Gas：以太坊交易计算手续费
    
7.  EVM：以太坊运行合约的虚拟机
    
8.  Layer2：以太坊扩容二层网络，低手续费
    
9.  MetaMask：ConsenSys 出品主流 Web3 钱包
    
10.  Uniswap：以太坊头部去中心化交易所 DEX
     
11.  IPFS：分布式文件存储协议
     
12.  NFT：链上唯一确权数字资产
     
13.  Vibecoding：自然语言驱动 AI 自动编码
     
14.  DApp：去中心化区块链应用
     
15.  Slashing：PoS 验证者作恶罚没质押 ETH
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

# **Blockchain Basics**

## 1\. Blockchain Definition

A decentralized distributed ledger made up of sequentially linked blocks. Transactions are stored and synchronized across all network nodes, making data transparent, traceable, and hard to tamper with.

Block Structure

Previous Block Hash: links to the previous block, forming a chain

Random Number: field for consensus computation

Batch of Transactions (up to around 4,000 per block)

Current Block Hash: fingerprint of the block’s data

Immutability Principle

Changing any historical transaction → current block hash changes → all subsequent block hashes break;

Changing only a few nodes’ ledgers is ineffective; you must control over 51% of nodes in the network to successfully tamper, which is very costly.

## 2\. Two Core Mechanisms: Chain Storage & Distributed Ledger

Distributed Network

Countless independent nodes (miners/validators) each store an identical complete ledger; no single center, so no single point of failure affects the network.

Node Incentives (Protocol Rewards) Nodes earn rewards for maintaining the network = block issuance rewards + user transaction fees (Gas fees).

## 3\. Three Main Types of Blockchain (from high to low decentralization)

| Public Chain | Anyone can join freely | Visible to everyone on the network | BTC, ETH, DeFi, NFT |

| Consortium Chain | Institutions approved by invitation | Only accessible to consortium members | Banking alliances, supply chain |

| Private Chain | Fully controlled by enterprise | Internal and closed data | Internal audits |

## 4\. Differences Between Web2 / Web 3.0 (Semantic Web) / Web3

Web2 (Current Internet)

Data resides on centralized servers like Alibaba, Tencent; platforms own user data, users only have usage rights; tech stack: React, Node, MySQL.

Web 3.0 (Semantic Web, W3C Standard)

Uses RDF and knowledge graphs (DBpedia) for machines to understand data, does not rely on blockchain; essentially an upgrade of traditional internet.

Web3 (Decentralized Internet)

Based on blockchain, smart contracts, and IPFS; users control digital assets and data via private keys; tech stack: React, Ethers.js, Solidity, IPFS; representative projects: Uniswap, MetaMask.

## 5\. Blockchain Pros and Cons

Advantages

No intermediary trust needed, no third-party guarantees Resistant to censorship, no platform bans Users have full ownership of digital assets Open source and anyone can develop DApps

Current Challenges

Public chains have low TPS and high fees during congestion Private keys are hard for average users to manage Anonymity brings compliance and money-laundering risks Smart contract code vulnerabilities can lead to asset theft

## 6\. Core Logic of Bitcoin (Blockchain 1.0)

Positioning: Decentralized digital gold, fixed total supply of 21 million coins, inflation-resistant

Consensus: Proof of Work (PoW), one block generated every 10 minutes

Anonymity Logic: Transactions only display random wallet addresses, not linked to individuals; once the address is exposed, privacy is compromised

Limitations: Only supports simple transfers, cannot execute complex programs, transaction speed is slow

## 7\. Complete Blockchain Operation Process

-   User wallet initiates a transaction (transfer / contract call)
    
-   Transaction is broadcast to all nodes in the network
    
-   Nodes verify signature, balance, and other legality
    
-   Consensus mechanism competes to pack a new block
    
-   Block is linked to the main chain, and the ledger is updated across the network
    
-   The node that successfully mines the block receives protocol rewards and transaction fees
    

# Ethereum Core System (Blockchain 2.0, World Computer)

## 1\. Ethereum Positioning

Ethereum, founded by Vitalik, is a programmable blockchain with smart contracts as its core innovation, supporting DeFi, NFTs, and DAOs. Its native token is ETH. In 2022, it completed The Merge, switching from PoW mining to PoS proof-of-stake.

## 2\. Complete Mechanism of PoS Validators

### Entry Threshold

Stake 32 ETH and run a client online 24/7.

### Two automatic functions of validators (the programs run fully automatically; human intervention is only for server maintenance)

-   Block Proposer (randomly selected): bundles transactions to create new blocks and receives high rewards
    
-   Attester (majority): verifies blocks and votes across the network to earn stable base rewards
    

### Reward and Penalty System

-   Rewards: block issuance of ETH, Gas fees, MEV earnings
    
-   Penalties (Slashing): small deductions for offline missed votes; severe forfeiture of staked ETH for double signing or malicious behavior
    

## 3\. Ethereum’s Three-Layer Architecture

**Execution Layer (Mainnet)**: Handles transactions, runs the EVM, and executes smart contracts

**Consensus Layer (Beacon Chain)**: Manages network validators, block ordering, and PoS voting

**Scaling Layer (Layer 2 Rollup)**: Arbitrum, zkSync, ConsenSys Linea—batch transactions to reduce gas fees

## 4\. Three Core Underlying Components

### (1) Account System

EOA External Account (wallet account, MetaMask): controlled by private keys, can actively initiate transactions

CA Contract Account: generated by Solidity deployment, no private key, can only be called by external accounts

### (2) Gas Fees

Users must pay to perform any on-chain operation, which goes to validators;

Fee = Gas used × Gas price; EIP-1559 breakdown: base fee (burned) + tip (given to validators).

### (3) EVM Ethereum Virtual Machine

Runs on every chain node, executes smart contract code uniformly, ensuring consistent computation across the network and security isolation.

## 5\. Ethereum Scaling Roadmap

**EIP-4844 (Cancun Upgrade):** Blob data storage, L2 transaction fees reduced by 70%-90%

**ZK-Rollup:** Batch verification using zero-knowledge proofs, secure and efficient

**Data Sharding:** Mainnet focuses on settlement, L2 handles massive transactions

## 6\. Representative Products of the Ethereum Ecosystem

**Wallet**: MetaMask (developed by ConsenSys)

**DEX**: Uniswap Decentralized Exchange

**Infrastructure**: Infura nodes, Besu client, Linea Layer-2 network
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
