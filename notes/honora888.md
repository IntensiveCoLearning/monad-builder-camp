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
