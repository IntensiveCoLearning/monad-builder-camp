---
timezone: UTC+8
---

# Liime-liime

**GitHub ID:** Liime-liime

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
# Web3 智能合约开发笔记

# 一、什么是智能合约（Smart Contract）

## 定义

智能合约本质上是：

> 部署在区块链上的程序。

特点：

-   自动执行
    
-   不可篡改
    
-   公开透明
    
-   去中心化运行
    

简单理解：

```text
传统应用：
用户 → 服务器 → 数据库

DApp：
用户 → 智能合约 → 区块链
```

智能合约相当于：

> Web3 世界的后端服务器。

* * *

# 二、DApp 架构

一个完整的 DApp 通常由四部分组成：

```text
前端
 ↓
钱包（MetaMask）
 ↓
RPC 节点
 ↓
智能合约
 ↓
区块链
```

* * *

## 1\. 前端（Frontend）

负责：

-   页面展示
    
-   用户交互
    
-   调用钱包
    
-   调用智能合约
    

常见技术：

-   HTML
    
-   CSS
    
-   JavaScript
    
-   React
    
-   Next.js
    

* * *

## 2\. 钱包（Wallet）

作用：

-   用户身份认证
    
-   交易签名
    
-   管理私钥
    

常见钱包：

-   MetaMask
    
-   Phantom
    

钱包 ≠ 存储资产

实际上钱包保存的是：

```text
Private Key（私钥）
```

* * *

## 3\. RPC 节点

作用：

> 前端访问区块链的入口。

类似于：

```text
Web2：
浏览器 → API服务器

Web3：
前端 → RPC节点 → 区块链
```

常见 RPC 服务：

-   Infura
    
-   Alchemy
    
-   QuickNode
    

* * *

## 4\. 智能合约

作用：

-   保存状态
    
-   处理业务逻辑
    
-   管理资产
    

开发语言：

```text
Solidity
```

运行环境：

```text
EVM（Ethereum Virtual Machine）
```

* * *

## 5\. 数据检索器（Indexer）

作用：

> 将链上 Event 数据同步到数据库。

为什么需要它？

因为链上查询非常慢且昂贵。

例如：

NFT 合约无法直接查询：

```text
某地址拥有哪些 NFT
```

因此需要：

```text
Transfer Event
↓
Indexer
↓
PostgreSQL
↓
前端查询
```

常见方案：

-   The Graph
    
-   Subsquid
    

* * *

## 6\. 去中心化存储

链上存储非常贵，因此图片、视频等文件通常放在链下。

常见方案：

-   IPFS
    
-   Arweave
    

存储内容：

-   NFT 图片
    
-   用户头像
    
-   文档
    
-   视频
    

* * *

# 三、智能合约开发流程

```text
需求分析
↓
编写 Solidity 合约
↓
本地测试
↓
部署测试网
↓
前端接入
↓
主网上线
```

* * *

# 四、Solidity 合约结构

一个最简单的合约：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyContract {

    uint256 public myNumber;

    constructor() {
        myNumber = 100;
    }

    function setNumber(uint256 _number) public {
        myNumber = _number;
    }
}
```

* * *

## 1\. SPDX License

```solidity
// SPDX-License-Identifier: MIT
```

作用：

声明开源协议。

常见：

-   MIT
    
-   GPL-3.0
    

* * *

## 2\. pragma

```solidity
pragma solidity ^0.8.0;
```

作用：

指定 Solidity 编译器版本。

例如：

```text
^0.8.0
```

表示：

```text
>=0.8.0
<0.9.0
```

* * *

## 3\. contract

```solidity
contract MyContract {

}
```

作用：

定义一个智能合约。

类似于：

```text
Java 的 class
```

* * *

## 4\. 状态变量（State Variables）

例如：

```solidity
uint256 public myNumber;
```

特点：

-   永久存储在链上
    
-   会消耗 Storage Gas
    
-   所有人都可以读取
    

常见类型：

```text
uint
int
bool
address
string
mapping
array
struct
```

* * *

## 5\. 构造函数（Constructor）

```solidity
constructor() {
    myNumber = 100;
}
```

特点：

-   合约部署时执行一次
    
-   之后不会再次调用
    

作用：

-   初始化参数
    
-   设置管理员地址
    
-   初始化状态
    

* * *

## 6\. 普通函数（Function）

例如：

```solidity
function setNumber(uint256 _number) public {
    myNumber = _number;
}
```

作用：

修改或者读取状态。

* * *

# 五、函数修饰符

## 1\. pure

```solidity
function add(uint a,uint b)
public
pure
returns(uint)
```

特点：

-   不读取状态
    
-   不修改状态
    

```text
只做计算
```

* * *

## 2\. view

```solidity
function getBalance()
public
view
returns(uint)
```

特点：

-   可以读取状态
    
-   不允许修改状态
    

* * *

## 3\. payable

```solidity
function deposit()
public
payable
```

特点：

-   可以接收 ETH
    
-   可以修改状态
    

* * *

## 4\. 普通函数

特点：

-   可以读取状态
    
-   可以修改状态
    

* * *

## 面试高频总结

| 类型 | 读取状态 | 修改状态 | 支付 ETH |
| --- | --- | --- | --- |
| pure | ❌ | ❌ | ❌ |
| view | ✅ | ❌ | ❌ |
| payable | ✅ | ✅ | ✅ |
| 普通函数 | ✅ | ✅ | ❌ |

* * *

# 六、前端与智能合约交互

## 读取数据

```text
call()
```

特点：

-   免费
    
-   不消耗 Gas
    
-   不会修改状态
    

例如：

```javascript
contract.methods.balanceOf(address).call()
```

* * *

## 写入数据

```text
send()
```

特点：

-   消耗 Gas
    
-   需要钱包签名
    
-   修改链上状态
    

例如：

```javascript
contract.methods.transfer(to,amount).send()
```

* * *

## 完整流程

```text
用户点击按钮
↓
前端调用 send()
↓
钱包弹窗签名
↓
广播交易
↓
矿工打包
↓
交易确认
↓
前端更新页面
```

* * *

# 七、Gas 优化

## 1\. Storage 最贵

Gas 消耗：

```text
Storage > Memory > Calldata
```

原则：

> 少写链上，多读链下。

* * *

## 2\. 位压缩（Bit Packing）

例如：

```solidity
struct User {
    uint128 a;
    uint128 b;
}
```

多个变量共享一个 Storage Slot。

* * *

## 3\. 循环优化

错误写法：

```solidity
for(uint i=0;i<arr.length;i++)
```

优化：

```solidity
uint len = arr.length;

for(uint i=0;i<len;i++)
```

* * *

## 4\. external 优于 public

如果函数只允许外部调用：

```solidity
external
```

Gas 更低。

* * *

# 八、智能合约安全

## 安全原则

-   最小权限原则
    
-   模块化设计
    
-   完整事件记录
    
-   显式错误处理
    

* * *

## 重入攻击（Reentrancy）

最经典漏洞：

```text
调用外部合约
↓
外部合约再次调用当前函数
↓
重复提款
```

历史案例：

```text
The DAO Hack（2016）
```

损失：

约 6000 万美元 ETH。

最终导致：

```text
Ethereum
Ethereum Classic（ETC）
```

分叉。

* * *

# 九、一句话理解智能合约

> 智能合约 = 部署在区块链上的后端程序。

如果说：

```text
React = Web2 前端
Node.js = Web2 后端
```

那么：

```text
React = Web3 前端
Solidity = Web3 后端
```

这也是目前绝大多数 DApp 的技术架构。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

# Web3 行业知识笔记

## 一、Web3 行业全景图

```text
Web3
├── DeFi（去中心化金融）
├── NFT（数字资产）
├── DAO（去中心化自治组织）
├── Meme（社区文化与注意力经济）
├── Layer1 / Layer2（区块链基础设施）
├── GameFi（游戏金融）
├── SocialFi（社交金融）
├── DePIN（去中心化物理基础设施）
└── AI + Crypto（AI Agent）
```

Web3 的核心理念：

-   Ownership（所有权归用户）
    
-   Decentralization（去中心化）
    
-   Permissionless（无需许可）
    

一句话理解：

> Web2：平台拥有数据  
> Web3：用户拥有资产、身份和数据

* * *

# 二、DeFi（Decentralized Finance）

## 1\. 定义

使用智能合约替代传统金融机构提供金融服务。

传统金融：

```text
用户 → 银行 → 用户
```

DeFi：

```text
用户 → 智能合约 → 用户
```

特点：

-   无需许可
    
-   全球开放
    
-   7×24 小时运行
    
-   用户自托管资产
    

* * *

## 2\. DEX（去中心化交易所）

### 定义

用户直接通过智能合约进行交易，不需要中心化交易所撮合。

代表项目：

-   Uniswap
    
-   PancakeSwap
    
-   Raydium
    

* * *

### AMM（自动做市商）

核心公式：

```text
x × y = k
```

例如：

```text
10 ETH
5000 USDC

10 × 5000 = 50000
```

当 ETH 被买走：

```text
ETH 数量下降
USDC 数量增加
```

ETH 价格自动上涨。

核心思想：

> 通过资产比例变化自动调整价格。

* * *

### LP（流动性提供者）

向资金池存入两种资产：

```text
ETH + USDC
```

即可成为 LP。

收益来源：

-   交易手续费
    
-   Token 激励
    
-   流动性挖矿奖励
    

风险：

-   无常损失（Impermanent Loss）
    

* * *

## 3\. 借贷协议

代表项目：

-   Aave
    
-   Compound
    

运行机制：

```text
抵押资产
↓
借出资产
↓
资产价值低于清算线
↓
自动清算
```

特点：

-   超额抵押
    
-   自动执行
    
-   无需审核
    

* * *

## 4\. 稳定币（Stablecoin）

定义：

与法币（通常为美元）价格挂钩的加密资产。

代表：

-   USDT
    
-   USDC
    
-   DAI
    

作用：

-   交易媒介
    
-   DeFi 基础资产
    
-   避险工具
    

* * *

# 三、NFT（Non-Fungible Token）

## 1\. 定义

不可替代代币，用于证明数字资产所有权。

应用场景：

-   图片
    
-   游戏装备
    
-   音乐版权
    
-   门票
    
-   域名
    

* * *

## 2\. FT 与 NFT 区别

| FT（同质化代币） | NFT（非同质化代币） |
| --- | --- |
| 可以互换 | 不可互换 |
| BTC、ETH | CryptoPunk、BAYC |

例：

```text
1 BTC = 1 BTC
```

但：

```text
一个 NFT ≠ 另一个 NFT
```

* * *

## 3\. NFT 的价值来源

### 社区价值

购买 NFT 本质上是加入社区。

### 身份价值

类似于现实中的：

-   奢侈品
    
-   豪车
    
-   名表
    

属于数字身份象征。

### 使用价值

例如：

-   游戏道具
    
-   活动门票
    
-   会员权益
    

* * *

# 四、DAO（Decentralized Autonomous Organization）

## 1\. 定义

通过智能合约和 Token 投票治理组织。

传统组织：

```text
CEO
↓
管理层
↓
员工
```

DAO：

```text
社区成员
↓
提出提案
↓
Token 投票
↓
自动执行
```

* * *

## 2\. DAO 治理流程

```text
Proposal（提案）
↓
Discussion（讨论）
↓
Voting（投票）
↓
Execution（执行）
```

* * *

## 3\. 优缺点

优点：

-   全球协作
    
-   透明治理
    
-   去中心化决策
    

缺点：

-   决策效率低
    
-   容易被巨鲸控制
    

* * *

# 五、Meme

## 定义

基于社区共识和注意力驱动的资产。

核心逻辑：

```text
文化
↓
社区
↓
传播
↓
市值
```

而不是：

```text
技术
↓
产品
↓
用户
```

代表项目：

-   DOGE
    
-   SHIB
    
-   PEPE
    

核心驱动力：

-   社区
    
-   情绪
    
-   注意力
    

一句话总结：

> Meme 的价值来源于共识，而不是技术。

* * *

# 六、区块链基础设施

## Layer1（公链）

负责资产记录与交易执行。

代表项目：

-   Ethereum
    
-   Solana
    
-   Sui
    

* * *

## Layer2（扩容方案）

目标：

-   提高 TPS
    
-   降低 Gas Fee
    

代表项目：

-   Arbitrum
    
-   Optimism
    
-   Base
    

* * *

## 钱包（Wallet）

钱包并不存储资产。

钱包存储的是：

```text
私钥（Private Key）
```

私钥控制链上资产。

代表钱包：

-   MetaMask
    
-   Phantom
    

* * *

## 区块浏览器

作用：

查询链上所有交易记录。

代表工具：

-   Etherscan
    
-   Solscan
    

可以查看：

-   转账记录
    
-   合约地址
    
-   Token 信息
    
-   Gas 消耗
    

* * *

# 七、Web3 常见术语

## Gas Fee

链上交易手续费。

* * *

## TVL（Total Value Locked）

协议锁仓总价值。

衡量：

> DeFi 项目规模的重要指标。

* * *

## FDV（Fully Diluted Valuation）

完全稀释估值。

计算公式：

```text
FDV = Token 总量 × 当前价格
```

* * *

## Tokenomics

代币经济模型。

主要关注：

-   总供应量
    
-   解锁计划
    
-   分配比例
    
-   通胀机制
    

* * *

## Airdrop

空投。

项目方向用户免费发放 Token。

* * *

## Staking

质押资产获取收益。

类似于：

```text
银行定期存款
```

* * *

## APR 与 APY

APR：

```text
单利收益率
```

APY：

```text
复利收益率
```

通常：

```text
APY > APR
```

* * *

## TGE（Token Generation Event）

代币正式发行上线的时间节点。

类似于：

```text
IPO（首次公开募股）
```

* * *

# 八、Web3 运营岗位学习路线

## 第一阶段（必须掌握）

-   区块链基础原理
    
-   钱包
    
-   Gas Fee
    
-   公链
    
-   DeFi
    
-   NFT
    
-   DAO
    

* * *

## 第二阶段（面试高频）

-   TVL
    
-   FDV
    
-   Tokenomics
    
-   Staking
    
-   APR/APY
    
-   TGE
    
-   Airdrop
    

* * *

## 第三阶段（进阶）

-   Layer2
    
-   Rollup
    
-   MEV
    
-   Restaking
    
-   DePIN
    
-   AI Agent
    

* * *

# 九、面试万能总结

如果面试官问：

> 你如何理解 Web3？

推荐回答：

Web3 的本质是利用区块链技术，将资产所有权、数据所有权和身份所有权重新交还给用户，实现无需许可、去中心化的互联网协作模式。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->


# DApp 架构笔记

### 一、DApp 的四大核心组成

| 组成部分 | 作用 | 常见技术 |
| --- | --- | --- |
| 前端（UI） | 用户交互界面，负责展示数据和发起操作 | HTML、CSS、JavaScript、React、Vue |
| 智能合约（Smart Contract） | 实现业务逻辑，负责链上规则执行 | Solidity、EVM |
| 数据检索器（Indexer） | 将链上事件同步到数据库，提高查询效率 | PostgreSQL、Indexer |
| 区块链与去中心化存储 | 存储交易数据、状态数据及文件资源 | Ethereum、IPFS、Arweave |

* * *

## 二、前端（User Interface）

### 作用

负责用户与 DApp 的交互。

### 与传统 Web 的区别

传统 Web：

> 前端 → 后端服务器 → 数据库

DApp：

> 前端 → 钱包 → RPC 节点 → 区块链

### 两种交互方式

① 读取数据（只读调用）

通过 RPC 节点向智能合约发起：

-   `eth_call`
    
-   查询合约状态
    
-   查询事件日志
    

特点：

-   不消耗 Gas
    
-   不需要用户签名
    

② 修改数据（写操作）

流程：

```
前端构造交易
    ↓
钱包（如 MetaMask）签名
    ↓
RPC 节点广播交易
    ↓
区块链执行并上链
```

特点：

-   消耗 Gas
    
-   必须用户签名授权
    

### 钱包的作用

例如：MetaMask

负责：

-   用户身份认证
    
-   私钥管理
    
-   交易签名
    
-   权限授权
    

* * *

## 三、智能合约（Smart Contracts）

### 作用

DApp 的核心业务逻辑。

例如：

-   NFT 买卖
    
-   Token 转账
    
-   DeFi 借贷
    
-   DAO 投票
    

### 特点

-   自动执行
    
-   公开透明
    
-   不可篡改
    
-   去中心化运行
    

### 技术栈

-   编程语言：`Solidity`
    
-   执行环境：Ethereum Virtual Machine（EVM）
    

流程：

```
用户操作
   ↓
调用智能合约函数
   ↓
EVM执行代码
   ↓
更新链上状态
```

* * *

## 四、数据检索器（Indexer）

### 为什么需要 Indexer？

区块链不擅长复杂查询。

例如：

某 NFT 项目需要展示：

> 用户钱包中持有哪些 NFT？

但 NFT 合约通常不会提供：

```text
getAllNFTs(address)
```

这样的查询接口。

* * *

### 工作流程

智能合约产生事件：

```text
Transfer(from,to,tokenId)
```

↓

Indexer 监听事件

↓

写入数据库（如 PostgreSQL）

↓

前端查询数据库

↓

展示用户拥有的 NFT

* * *

### 优势

-   查询速度快
    
-   支持复杂筛选
    
-   降低链上查询成本
    
-   提升用户体验
    

本质上：

> Indexer = Web3 世界里的「后端数据库同步服务」

* * *

## 五、区块链与去中心化存储

### 1\. 区块链负责存储

存储内容：

-   智能合约代码
    
-   合约状态
    
-   交易记录
    
-   链上资产
    

特点：

-   不可篡改
    
-   可追溯
    
-   去中心化
    

* * *

### 2\. 去中心化存储负责存储大文件

因为区块链存储成本极高，所以：

图片、视频、文档等大文件通常不会直接存到链上。

常见方案：

-   IPFS
    
-   Arweave
    

存储流程：

```
图片上传到 IPFS
      ↓
返回 CID（文件哈希）
      ↓
将 CID 写入智能合约
      ↓
前端通过 CID 获取图片
```

* * *

## 六、整体架构图

```text
                用户
                  │
                  ▼
            DApp 前端
                  │
        ┌─────────┴─────────┐
        │                   │
        ▼                   ▼
      钱包              数据检索器
   (MetaMask)           (Indexer)
        │                   │
        ▼                   ▼
     RPC节点            PostgreSQL
        │
        ▼
     智能合约
        │
   ┌────┴────┐
   ▼         ▼
区块链      IPFS/Arweave
```

* * *

## 七、一句话总结

> **前端负责交互，钱包负责签名，RPC 负责通信，智能合约负责业务逻辑，Indexer 负责高效查询，区块链和去中心化存储负责数据保存。**

如果把 DApp 类比为传统互联网应用：

| Web2 | Web3 |
| --- | --- |
| 前端 | 前端 |
| 后端服务器 | 智能合约 |
| MySQL | 区块链 |
| Redis/ES | Indexer |
| OSS对象存储 | IPFS/Arweave |
| 登录账号密码 | 钱包签名登录 |
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->



# 区块链是什么

区块链是一种去中心化的分布式账本技术，用于在网络节点之间安全、透明且不可篡改地记录事务数据。每条链由一系列按照时间顺序相连的“区块”组成，每个区块内部包含了多笔交易数据及元数据，确保了数据记录的完整性与可追溯性。

# 区块链的特性

不可篡改、公开透明（匿名）、快速交易、去中心化

# 一条区块链是如何运行起来的

区块链生态系统的运行包含以下几个关键步骤：

1.  **用户发起交易**：用户通过钱包应用发起转账、智能合约调用等操作
    
2.  **交易广播**：交易信息被广播到整个网络中的各个节点
    
3.  **节点验证**：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）
    
4.  **打包成块**：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块
    
5.  **链接上链**：新区块被添加到区块链上，更新全网的账本状态
    
6.  **奖励发放**：成功出块的节点可获得协议奖励（如区块奖励/质押奖励）和交易手续费（或其中一部分）
    

# 公链&私链&联盟链

按照去中心化程度从高到低排列。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Liime-liime/images/2026-07-06-1783338901129-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Liime-liime/images/2026-07-06-1783341000416-image.png)

# Web2 VS Web3.0 VS Web3

Web2

**核心特征：**

-   **中心化控制**：数据存储在科技巨头的服务器（如 Google、Facebook）
    
-   **用户角色**：内容生产者，但不拥有数据
    
-   **商业模式**：广告驱动，平台抽取佣金
    
-   **典型应用**：微信、抖音、亚马逊
    

Web3.0

-   **语义标记**：使用 RDF、OWL 等标准描述数据关系
    
-   **结构化数据**：信息按照标准格式组织，便于机器理解
    
-   **知识图谱**：构建实体间的语义关系网络
    
-   **典型技术**：本体工程、语义查询语言（SPARQL）、链接数据
    

Web3

-   **数据主权归用户**：用区块链存储身份和资产
    
-   **无需信任中介**：智能合约自动执行规则
    
-   **真正拥有数字资产**：你的 NFT 头像、游戏道具真正属于你，平台无法删除或收回
    
-   **金融服务无门槛**：无需银行卡，用手机钱包就能借贷、理财、交易
    
-   **应用可自由组合**：一个 DeFi 协议的流动性可以被其他应用直接调用，就像搭积木
    
-   **内容永不消失**：文章、图片存储在分布式网络，不会因为平台关闭而丢失
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Liime-liime/images/2026-07-06-1783341226476-image.png)

# 去中心化的优势与挑战

优势：

1.  **信任最小化**
    
    去中心化网络无需依赖中心化第三方，交易和数据由共识算法和加密证明保障，降低了 “信任成本”。
    
2.  **抗审查与高弹性**
    
    数据分布存储在多个节点，单点故障或审查攻击难以完全阻断网络，提升了系统的安全性和可用性。
    
3.  **用户自主管理**
    
    用户通过私钥掌控资产与数据，平台无法随意更改或冻结账户，赋予个人更高的隐私权和所有权。
    
4.  **开放创新生态**
    
    区块链与智能合约构建了去中心化应用（Dapps）平台，任何开发者都可在此基础上创新并获得代币激励，促进了技术和商业模式的多样化。
    

挑战：

1.  **可扩展性瓶颈**
    
    公链在节点众多时共识效率低下，吞吐量和延迟问题突出，目前各大项目正通过分片、Layer 2 方案等技术进行优化。
    
2.  **安全与治理难题**
    
    区块链的不可篡改特性虽能保证数据安全，但代码漏洞或治理失衡（如 DAO 中投票权集中）也可能导致严重损失。
    
3.  **用户体验与成本**
    
    完全去中心化的系统往往对普通用户不够友好，如私钥管理复杂、交易手续费浮动大，需要在易用性与去中心化程度之间权衡。
    
4.  **法律与合规风险**
    
    去中心化和匿名特性与现行法规存在冲突，跨境监管和合规要求尚未完善，给项目方和用户带来不确定性。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
