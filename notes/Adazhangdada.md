---
timezone: UTC+8
---

# Xinyue Zhang

**GitHub ID:** Adazhangdada

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# DAY2 转账

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2ZiYTYxZmNiMjRmYzI3MzNlNWY4YzFjZmRmYjk5OTdfaDdBYkxTM0pFRUxtSzN5bmF2bzBKd3VURkljbm93WUxfVG9rZW46S3pkS2JUU3JTb1Q4QWN4a011TGNjbFVOblRoXzE3ODM0MzM1MjI6MTc4MzQzNzEyMl9WNA&add_watermark=true&scene_type=CCM)

## Transaction hash

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTg3MzZhMzMzMzliNjM3NjEzNGM5NzBkMDQwYmY5NjFfY3paMnFSWVBtc01icGh4cHhsSW1PZlpUR29PT3hnamlfVG9rZW46TXBZSGJCZmVvb25YY3Z4cjBsUmNTVmphbnVnXzE3ODM0MzM1MjI6MTc4MzQzNzEyMl9WNA&add_watermark=true&scene_type=CCM)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

DAY1 希望坚持！

Web3入门：[https://web3intern.xyz/zh/preface/](https://web3intern.xyz/zh/preface/)

# **区块链基础概念**

## **一、区块链**

区块链是一种去中心化的分布式账本技术，用于在网络节点之间安全、透明且不可篡改地记录事务数据。每条链由一系列按照时间顺序相连的“区块”组成，每个区块内部包含了多笔交易数据及元数据，确保了数据记录的完整性与可追溯性。

一个区块链网络中有非常多的节点（矿机）来记账，每个节点都会记录完整的、相同的区块链信息。

因为有分布式网络，区块链有了**去中心化**和**真正的不可篡改**的新特性。

-   **去中心化**
    
    区块链网络通常分布在全球，每个节点都将会存储一份相同的区块链数据。没有人能够控制全部的节点，因此这份区块链数据将会一直存在。
    
-   **真正的不可篡改**
    
    区块链网络通常分布在全球，一个人控制大部分节点几乎不可能，因此即便有人修改了部分节点的区块链数据，只要被修改记录的节点不超过 51%，这个改动将不会被认可。
    

## 二、比特币

去中心化和分布式网络很好，可是为什么会有人愿意提供这项服务呢？因为他们可以因此得到奖励：比特币！

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Adazhangdada/images/2026-07-06-1783349132553-image.png)

## 三、区块链的核心组成部分

### 由两个核心组成：

1.  **去中心化的网络和区块链**
    

区块链将会有一条链来记录全部的信息，这条链将存在对应的去中心化网络中。 去中心化的网络，将由无数节点提供服务来维持网络运行。节点通过计算验证交易获得代币奖励。

2.  **维持网络运行的代币激励**
    

去中心化的网络由无数节点提供服务来维持网络运行。以 PoW 链为例，这些操作常被称为“挖矿”，参与者一般被称为矿工。矿工（或 PoS 链中的验证者）维持网络运行需要奖励，就像你工作需要工资。更准确地说，节点收入通常由协议奖励与交易手续费两部分组成。你经常听说的燃料费（Gas Fee）属于交易手续费的一部分，并不等于全部节点奖励。 你使用这个网络进行交易、转账、铸造 NFT 等等，均需要支付代币。如果你没有代币又想使用这个区块链网络服务，则需要进行代币之间交换或者法币交换。

### 区块链生态系统的运行包含以下几个关键步骤：

1.  用户发起交易：用户通过钱包应用发起转账、智能合约调用等操作
    
2.  交易广播：交易信息被广播到整个网络中的各个节点
    
3.  节点验证：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）
    
4.  打包成块：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块
    
5.  链接上链：新区块被添加到区块链上，更新全网的账本状态
    
6.  奖励发放：成功出块的节点可获得协议奖励（如区块奖励/质押奖励）和交易手续费（或其中一部分）
    

## 四、公链 vs 联盟链 vs 私链 Web2，3.0，3

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Adazhangdada/images/2026-07-06-1783349323238-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Adazhangdada/images/2026-07-06-1783349355415-image.png)

# **以太坊概览**

## **核心机制**

以太坊不仅仅是一条区块链，更是一个全球共享的“分布式计算机”。它的核心价值在于通过代码实现无需信任的自动化规则（如智能合约），而这一切的背后依赖于三个关键机制：**账户系统、Gas 模型 和 以太坊虚拟机（EVM）**。

我们平时使用 **账户系统** 里面的 **外部账户（EOA）** 与区块链中的其他用户的 **外部账户（EOA）** 或者与智能合约所在的 **合约账户（CA）** 进行交互，而其中的 **Gas 模型** 是支撑整个网络的经济基础， **Gas 模型** 和其他经济来源支撑着全球的 **以太坊虚拟机（EVM）** 来实现以太坊网络。

### [**1\. 账户系统：你的数字身份**](https://web3intern.xyz/zh/overview-of-ethereum/#_1-%E8%B4%A6%E6%88%B7%E7%B3%BB%E7%BB%9F-%E4%BD%A0%E7%9A%84%E6%95%B0%E5%AD%97%E8%BA%AB%E4%BB%BD)

**账户系统** 包含由私钥控制的 **外部账户（EOA）** 和由智能合约代码控制的 **合约账户（CA）** 。

想象你第一次接触以太坊——你需要一个“数字钱包”来参与网络。这个钱包的核心是 **外部账户（EOA）** ，它由一对密钥（私钥和公钥）生成，就像银行账户的密码和账号。私钥是你控制账户的“钥匙”，必须严格保密；公钥通过加密算法生成一个唯一的地址（如 `0xAbc...123`），你可以把它分享给朋友接收转账。

除了用户控制的 EOA，还有 **合约账户（CA）** 。它们不像 EOA 那样受私钥控制，而是由代码驱动。比如，你部署一个智能合约（如一个 NFT 市场），区块链会自动生成一个 CA 地址（如 `0xDef...456`）。这个账户不能主动发起交易，只能通过 EOA 触发——比如你点击“购买 NFT”按钮时，EOA 向 CA 发送交易，CA 的代码自动执行出货逻辑。

每个账户都包含四个关键字段：

-   **Nonce**：防止重复交易的计数器（EOA 记录发送次数，CA 记录创建合约次数）。
    
-   **余额**：账户持有的 ETH 数量（单位为 Wei）。
    
-   **CodeHash**：EOA 为空哈希，CA 存储合约字节码的哈希值。
    
-   **StorageRoot**：记录账户数据的 Merkle 树根哈希（如 NFT 归属关系）。
    

### [**2\. Gas 模型：交易的燃料费**](https://web3intern.xyz/zh/overview-of-ethereum/#_2-gas-%E6%A8%A1%E5%9E%8B-%E4%BA%A4%E6%98%93%E7%9A%84%E7%87%83%E6%96%99%E8%B4%B9)

当你使用自己的 EOA 钱包发起一笔交易（比如转账或操作合约），这件事当然 **不会是免费的**，**你需要支付“燃料费”——也就是 Gas。**

Gas 是怎么计算的？

Gas 费用 = **用多少 × 每单位多少钱**，就像你打车一样：

-   **Gas Limit（限额）**：你最多愿意“烧”多少燃料。 比如你觉得最多可能需要 15 万单位，就设置 150,000。
    
-   **Gas Price（单价）**：每单位燃料多少钱，用 Gwei 表示（1 Gwei = 0.000000001 ETH）。 网络越拥堵，价格越贵，就像打车高峰期加价。
    

所以，**总费用 = Gas Limit × Gas Price**。

假设你设置 Gas Limit 为 150 000，Gas Price 为 100 Gwei，总费用就是 0.015 ETH。如果实际消耗 120 000 Gas，剩余 30 000 Gas × 100 Gwei = 0.003 ETH 会退还给你。

Gas 的存在有两个目的：

-   **激励矿工/验证者**：你给得越多（Gas Price 越高），他们越愿意优先处理你的交易。
    
-   **防止资源滥用**：如果有人想让合约死循环，Gas 会用光，交易失败，系统不会被拖垮。
    

**在 EIP-1559 升级后，Gas Price 被拆分了：**

以前，Gas Price 全部给矿工。 现在，分成两部分：

-   **基础费用（Base Fee）**：每个区块都会有，自动计算，直接销毁（直接消失）来帮助 ETH 通缩，毕竟物以稀为贵。
    
-   **小费（Tip）**：你额外加的钱，用来鼓励矿工优先处理你。
    

### [**3\. 以太坊虚拟机（EVM）：代码的执行引擎**](https://web3intern.xyz/zh/overview-of-ethereum/#_3-%E4%BB%A5%E5%A4%AA%E5%9D%8A%E8%99%9A%E6%8B%9F%E6%9C%BA-evm-%E4%BB%A3%E7%A0%81%E7%9A%84%E6%89%A7%E8%A1%8C%E5%BC%95%E6%93%8E)

EVM（Ethereum Virtual Machine）是 **以太坊的“大脑”**，是专门用来**运行智能合约的虚拟计算机**。它运行在每个节点上，确保整个网络在处理代码时，**结果都一致、可信任**。

**EVM 的核心特点：**

-   **图灵完备**：就像真正的电脑一样，EVM 可以执行各种逻辑，比如 if 判断、循环等。
    
-   **全球同步**：每个矿工/节点都会自己执行一遍合约代码，保证结果一样。
    
-   **隔离安全**：EVM 把合约“关”在一个小房间里运行，不允许它乱访问用户的数据和网络，保护隐私和安全。
    

## 类比：

### [**1\. 一般流程**](https://web3intern.xyz/zh/overview-of-ethereum/#_1-%E4%B8%80%E8%88%AC%E6%B5%81%E7%A8%8B)

1.  **用户通过 EOA 发起交易**（如调用合约与 **CA** 交互，或者转账与其他用户的 **EOA** 交互）。
    
2.  **交易附带 Gas 参数**，矿工 / 验证者选择打包。
    
3.  **EVM 执行合约代码**，修改存储（如更新 NFT 归属）。
    
4.  **Gas 费用按 Gas Limit × Gas Price 扣除**，保障资源合理使用。
    

### [**2\. 相似类比**](https://web3intern.xyz/zh/overview-of-ethereum/#_2-%E7%9B%B8%E4%BC%BC%E7%B1%BB%E6%AF%94)

-   **EOA = 个人钱包**：你用私钥“签名”来证明身份，可以主动发起任何交易
    
-   **CA = 智能保险箱**：按照预设程序自动运行，只有满足条件才执行操作（如：收到正确密码后自动转账）
    
-   **Gas = 计算资源费用**：
    
    -   Gas Limit：你愿意支付的最大计算量
        
    -   Gas Price：每单位计算的价格
        
    -   如果设置 1000 单位 ×10Gwei，实际用了 800 单位，退还 200 单位的费用
        
-   **EVM = 全球共享计算机**：所有节点同时运行相同的程序，确保结果一致且可信
    

# Vibecoding

[https://buildanything.so/zh/tracks/freshman/lessons/introduction-to-vibecoding](https://buildanything.so/zh/tracks/freshman/lessons/introduction-to-vibecoding)

**Replit** — 一个基于浏览器的编程环境，内置 AI 智能体。无需安装，所有功能都在云端运行。免费版足以让你上手。

**建议在沙箱中运行智能体，而不是在本地电脑上。** 沙箱是一个云端的隔离环境，与你的个人文件和敏感数据相互分离。即便智能体在沙箱中被攻陷，损害也会被限制在沙箱内。

我们推荐两种选择：

-   [**Replit**](https://replit.com/) — 基于浏览器，无需任何配置，适合新手。
    
-   [**GitHub Codespaces**](https://github.com/codespaces) — 在云端运行 VS Code，需要 GitHub 账号。
    

**绝不要把真实的私钥、密码或 API key 交给智能体。** 一旦你把这些交给智能体，就应当把它们视为已经泄露。请始终使用没有真实资金或敏感访问权限的开发专用账号。

**把智能体的输出当作来自陌生人的代码来对待。** 在部署前审查它构建的内容，尤其是涉及金钱或用户数据的部分。

推荐 [**Component Gallery**](https://component.gallery/)，里面收录了几乎所有常见 UI 组件的可视化参考，并附有标准名称。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
