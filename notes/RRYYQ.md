---
timezone: UTC+8
---

# yurik rong

**GitHub ID:** RRYYQ

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# **Day 4｜AI + Solidity + 合约部署 学习笔记**

## **学习目标**

完成本章学习后，应能够：

使用 AI 生成一个最小 Solidity 智能合约

不盲信 AI 输出，能够阅读并理解合约代

使用 Remix IDE 编译、部署并调用智能合约

理解 Solidity 源码、ABI、Bytecode、Contract Address、Read Function、Write Function、Transaction Hash 等核心概念

# **一、Solidity**

Solidity 是 Ethereum/EVM 生态最常用的智能合约开发语言，用于编写部署到区块链上的业务逻辑。

主要用途：

编写智能合约

管理数字资产

实现 DApp 后端逻辑

与钱包及前端交互

一个 Solidity 文件通常以：

pragma solidity ^0.8.20;

开头，用于指定编译器版本。

# **二、AI 在 Solidity 开发中的作用**

AI 可以帮助开发者：

生成基础智能合约

补全函数

解释报错信息

重构代码

编写测试

但 AI 并非完全可靠，开发者仍需检查：

Solidity 版本是否正确

是否存在安全漏洞

是否符合业务需求

是否引用正确的库

AI 是开发助手，而不是代码审核者。

三、Remix IDE

Remix 是官方推荐的在线 Solidity 开发环境。

主要功能：

编写 Solidity

编译智能合约

部署到测试网或主网

调用合约函数

调试合约

基本流程：

创建 Solidity 文件

        ↓

编写代码

        ↓

Compile

        ↓

Deploy

        ↓

调用 Read / Write Function

# **四、编译（Compile）**

在 Remix 中点击 **Solidity Compiler** 进行编译。

编译完成后会生成：

ABI

Bytecode

如果没有 Error，则表示源码能够成功编译。

# **五、ABI（Application Binary Interface）**

ABI 可以理解为：

智能合约的接口说明书。

作用：

告诉钱包有哪些函数

告诉前端如何调用函数

定义参数和返回值

例如：

increment()

count()

reset()

钱包就是根据 ABI 与智能合约交互。

# **六、Bytecode**

Bytecode 是 Solidity 编译后的机器代码。

流程如下：

Solidity Source Code

        ↓

Compiler

        ↓

Bytecode

        ↓

EVM 执行

EVM 无法直接执行 Solidity，只能执行 Bytecode。

# **七、部署（Deploy）**

在 Remix 中：

Deploy & Run Transactions

选择环境：

Remix VM

Injected Provider（MetaMask）

Monad Testnet

点击 Deploy 后：

钱包确认交易

部署完成后即可获得：

**Contract Address（合约地址）**

# **八、Contract Address（合约地址）**

部署成功后，每个智能合约都会拥有唯一地址，例如：

0xABCD...

作用：

唯一标识链上的智能合约

所有人都可以通过该地址调用合约

# **九、Read Function**

Read Function 用于读取链上数据。

特点：

不修改状态

不消耗 Gas

不需要钱包签名

不产生 Transaction Hash

例如：

count()

# **十、Write Function**

Write Function 用于修改链上状态。

特点：

修改数据

消耗 Gas

需要钱包签名

会产生交易

例如：

increment()

执行流程：

点击函数

      ↓

钱包签名

      ↓

区块链执行

      ↓

更新状态

# **十一、Transaction Hash（交易哈希）**

每一笔成功提交到区块链的交易都会生成唯一的：

**Transaction Hash（Tx Hash）**

作用：

查询交易状态

查询 Gas 消耗

查询区块高度

查询交易时间

查询输入数据

可以通过区块浏览器查看交易详情。

# **十二、整体流程**

Source Code

      ↓

Compile

      ↓

ABI + Bytecode

      ↓

Deploy

      ↓

Contract Address

      ↓

Read / Write Function

      ↓

Transaction

      ↓

Transaction Has

# **十三、OpenZeppelin**

OpenZeppelin 是目前最常用的智能合约安全库。

提供大量经过审计的标准合约，例如：

ERC20

ERC721

Ownable

AccessControl

Pausable

实际开发中通常直接继承 OpenZeppelin，而不是从零实现标准协议。

# **十四、Monad 部署流程**

推荐流程：

连接 MetaMask

      ↓

切换 Monad Testnet

      ↓

Compile

      ↓

Deploy

      ↓

钱包确认

      ↓

获取 Contract Address

      ↓

开始交互

# **推荐学习资源**

Web3 实习手册（智能合约开发）

Solidity 官方文档

Remix IDE

OpenZeppelin Contracts

Monad Remix Deployment Guide

BuildAnything Freshman

# **今日总结**

Solidity 是 EVM 智能合约开发的核心语言。

AI 可以辅助编写代码，但不能替代人工审核。

编译后会生成 **ABI** 和 **Bytecode**。

部署成功后获得唯一的 **Contract Address**。

**Read Function** 不消耗 Gas，不产生交易。

**Write Function** 会修改链上状态，需要签名并支付 Gas。

每笔写操作都会生成唯一的 **Transaction Hash（Tx Hash）**。

OpenZeppelin 是实际开发中最重要的智能合约标准库之一。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

# Web3 实习课程 Day 1 自学笔记 —— 钱包、Monad Testnet 与交易浏览器

## 一、学习目标

本次学习的目标主要包括：

-   创建课程专用钱包，不使用日常主力钱包进行实验。
    
-   添加 Monad Testnet 测试网络。
    
-   获取测试网测试币（Test Token）。
    
-   完成第一笔测试网交易或 DApp 交互。
    
-   学会使用 Block Explorer 查看并分析一笔交易。
    

* * *

## 二、知识点总结

### 1\. 为什么要创建课程专用钱包

学习过程中会连接各种测试网站和 DApp，如果使用主力钱包，存在资产安全风险。因此最好新建一个课程专用钱包，仅用于实验和学习。

另外，钱包最重要的是**助记词（Seed Phrase）**，它代表钱包的所有权。

需要注意：

-   助记词不能截图。
    
-   不要上传到云盘或聊天软件。
    
-   不要告诉任何人。
    
-   最好离线保存。
    

* * *

### 2\. Monad Testnet

Monad 是兼容 EVM 的高性能区块链，因此可以直接使用 MetaMask 等 EVM 钱包连接。

添加测试网后，可以通过 Faucet 领取测试币，用于测试交易，不需要真实资产。

测试网资产没有实际价值，仅用于开发和学习。

* * *

### 3\. Ethereum 交易基础

一次交易通常包含以下内容：

-   Sender（发送地址）
    
-   Receiver（接收地址）
    
-   Value（转账金额）
    
-   Gas Fee（Gas 消耗）
    
-   Transaction Hash（交易哈希）
    
-   Block Number（所在区块）
    
-   Status（Success 或 Failed）
    

其中：

-   Transaction Hash 是交易的唯一编号。
    
-   每笔交易都会被打包进某一个区块。
    
-   Gas 是执行交易需要支付的计算费用。
    

* * *

### 4\. Block Explorer 的作用

区块浏览器相当于区块链的搜索引擎。

输入 Transaction Hash 后，可以查看：

-   是否交易成功
    
-   转账金额
    
-   Gas 消耗
    
-   区块高度
    
-   时间
    
-   From 和 To 地址
    
-   交易详情
    

因此，Explorer 是排查交易问题的重要工具。

* * *

## 三、实操过程

本次按照课程步骤完成了以下内容：

1.  安装 MetaMask 钱包。
    
2.  创建课程专用钱包。
    
3.  妥善备份助记词（未截图、未上传网络）。
    
4.  添加 Monad Testnet 网络。
    
5.  领取测试网测试币。
    
6.  完成一次测试网交易（或完成一次测试网 DApp 交互）。
    
7.  在 Block Explorer 中输入 Transaction Hash，查看交易详情，包括交易状态、区块高度、Gas Fee、发送地址和接收地址等信息。
    

* * *

## 四、学习收获

通过本次学习，我对 Web3 钱包的作用和安全规范有了更深入的理解，也认识到助记词的重要性。实践过程中掌握了测试网络的配置方法，并完成了第一笔测试网交易。

此外，我学会了利用 Block Explorer 查询交易记录，能够根据 Transaction Hash 查看交易是否成功以及相关信息。这为后续学习智能合约、DApp 开发和链上数据分析打下了基础。

* * *

## 五、总结

今天完成了钱包创建、Monad Testnet 配置、测试币领取、交易实践以及 Explorer 查询等内容，对 EVM 钱包和区块链交易流程有了较为完整的认识。

后续希望继续学习智能合约部署、DApp 交互以及 Solidity 开发，进一步理解区块链应用的运行机制。
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
