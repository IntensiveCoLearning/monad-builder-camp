---
timezone: UTC+8
---

# Riso

**GitHub ID:** LierMi

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-25
<!-- DAILY_CHECKIN_2026-07-25_START -->
学习Monad链的知识，以更好进行黑客松  
  
绝大多数区块链之所以慢，是因为它们在串行处理交易，且受到了 CPU 读写硬盘数据（I/O）的严重拖累。Monad 通过重构以太坊最底层的四个核心组件，完成了整机的“极速改造”：

1\. 并行 EVM (Parallel EVM) 与乐观并发控制 (OCC)

-   **传统 EVM 痛点**：以太坊是“单线程”的，几万笔交易必须老老实实排队，一笔执行完了才能跑下一笔。哪怕 A 转账给 B，和 C 在 Uniswap 交易压根毫不相干，也必须排队。
    
-   **Monad 解决方案**：引入**乐观并发控制（Optimistic Concurrency Control, OCC）**。系统会“乐观地”假设同时发来的多笔交易彼此之间没有冲突，利用多核 CPU 瞬间**并行执行**它们。
    
-   **防冲突机制**：如果执行完发现其中两笔交易碰了同一个账户余额（产生状态冲突），系统会在后台以极快的速度对冲突的交易进行重新排序并重新执行。最终呈现给全局账本的结果，完全等同于串行执行的正确顺序。
    

2\. MonadDb —— 为区块链而生的自定义状态数据库

-   **传统 EVM 痛点**：区块链性能真正的最大瓶颈往往不是 CPU 算力，而是**状态访问（State Access）**——即电脑去硬盘里读取账户余额和智能合约数据这一步。传统 EVM 使用的外部数据库（如 LevelDB）并不是为区块链树状结构（默克尔帕特里亚树 MPT）设计的，每次读写都要在硬盘里翻找很久。
    
-   **Monad 解决方案**：Monad 团队从零用 C++ 和 Rust 编写了 **MonadDb**。这是一个原生支持区块链状态树结构的数据库，并且结合 Linux 底层的异步 I/O 机制（`io_uring`），允许 SSD 硬盘同时并发处理数千个数据读写请求，把硬盘的性能压榨到了极限。
    

3\. 延迟执行 (Deferred Execution)

-   **传统 EVM 痛点**：在传统以太坊网络中，验证节点做共识（大家同意区块里有哪些交易、顺序如何）和执行交易（跑 Solidity 代码算余额）是**绑在一起同时进行的**。这导致节点必须花大量时间等待代码跑完，才能盖章确认区块。
    
-   **Monad 解决方案**：将“共识”与“执行”彻底解耦。
    
    -   **共识阶段**：节点们先极速达成一致：“我们确定这一块包含这 5000 笔交易，顺序锁死！”（此时即可确认交易打包，达到共识终局）。
        
    -   **执行阶段**：在共识达成后，节点再利用充裕的时间在后台调用 Parallel EVM 去并行计算这些交易的最终余额状态。这极大地压缩了出块的时间窗口。
        

4\. MonadBFT —— 高性能流水线共识机制

-   基于著名的 HotStuff 算法升级而来，是一种高性能的拜占庭容错共识机制。它通过两阶段投票和领袖轮换流水线设计，确保在数百个去中心化验证节点之间，依然能够以毫秒级的速度达成通信与共识，彻底摆脱了传统 PoW 或早期 PoS 复杂的通信开销。
    

### **三、 核心维度对比：Monad vs 以太坊 vs Solana**

在实际面试或撰写技术方案时，评委非常喜欢考核你对不同 L1 路线的权衡认知（Trade-offs）：

| 核心维度 | Ethereum 主网 (传统 EVM) | Solana 主网 (Alt-L1 代表) | Monad 主网 (高性能 EVM) |
| --- | --- | --- | --- |
| 理论吞吐量 (TPS) | 约 15 - 30 TPS | 约 3,000 - 5,000+ TPS | 10,000+ TPS |
| 开发语言与架构 | Solidity / EVM | Rust / SVM (Solana 虚拟机) | Solidity / EVM (100% 字节码兼容) |
| 开发与迁移成本 | 极低（生态标准） | 极重（需重新学习 Rust/Account 模型） | 零迁移成本（代码直接拷贝部署） |
| 状态访问机制 | 串行执行 + LevelDB 瓶颈 | 需在代码中提前显式声明读写账户 | 乐观并发控制 (OCC) + MonadDb |
| 共识与执行关系 | 共识与执行捆绑同步 | 依靠 POH 历史证明流水线 | 延迟执行 (Deferred Execution) 解耦 |

### **💡 给 Web3 实习工程师的实战启示**

理解了 Monad 的底层，对你现在的写代码实战（如 Hardhat / Foundry 部署、Ethers.js 调用）有两点极其重要的实战价值：

1.  **“开箱即用”的开发红利**：因为 Monad 做到的是底层的**字节码级兼容（Bytecode Compatibility）**，这意味着你这周学的 **Hardhat、Foundry 部署脚本，以及 MetaMask、Viem、Ethers.js 交互库，不需要做任何专用 SDK 的修改** 。你只需要在脚本的配置文件（如 `hardhat.config.ts`）中把 RPC URL 换成 Monad 测试网的节点地址 ，你写的那些防重入锁、打卡合约 就能瞬间获得 10,000 TPS 的超跑级执行速度。
    
2.  **DeFi 可组合性（Lego）的复兴**：传统为了解决以太坊慢而出现的 Layer 2（如 Arbitrum、Optimism），导致了“流动性割裂”（资金分散在不同链上，彼此跨链极其麻烦）。而 Monad 作为一个统一的 Layer 1，允许极其复杂的 DeFi 协议（如将 Uniswap、Aave、MakerDAO 串联在一起的闪电贷或多重衍生品 ）在同一瞬间、同一底层状态池中毫无摩擦地高频并发执行，这为未来的**高频链上交易工具**和 **AI 代理高频微支付（如 x402 协议）** 提供了完美的终局基建。
<!-- DAILY_CHECKIN_2026-07-25_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->

今日黑客松项目持续中。。

和团队沟通。。

争取这周天确认下周项目主题
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->


今天主要学习了 Web3 中钱包授权、智能合约和 DeFi 协议的安全机制。我认识到，Web3 的真正安全边界是链上合约，而不是项目网页：断开钱包不代表取消授权，隐藏按钮也不能阻止别人直接调用合约。

在合约开发方面，我学习了重入攻击、外部调用返回值、状态机以及代理升级等问题。合约不仅要实现正常业务流程，还要考虑恶意调用顺序、失败情况和升级后的数据兼容性。

在 DeFi 方面，我了解到预言机价格需要检查时效性和抗操纵能力，Swap 也必须设置最低到账金额和过期时间。对于签名，则需要通过 EIP-712、nonce、有效期和域分离防止签名被误用或重复使用。

今天最重要的认识是：**Web3 安全不能只验证“正常情况下能不能运行”，还要思考权限是否最小、数据是否可信、操作能否被重复、价格能否被操纵，以及发生事故后能否及时停止损失。**
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->



今天系统了解了 Moss 项目，并完成了项目介绍、GitHub 探索日志和新手教程。

Moss 是一个面向 Monad 的 AI Agent 交易框架，可以把协议操作变成 Agent 可调用的能力。它的核心流程是 `discover → load → action → simulate`：先查找能力、读取规则、生成未签名交易，再提前模拟并检查 Receipt。Moss 不保管私钥、不签名、不发送交易，最终决定权仍在用户和钱包手中。

同时学习了 GitHub 开源协作的基本概念：README 是项目说明书，Docs 保存教程和设计规则，Issue 表示待解决的问题，PR 表示“我已经完成修改，请检查并合并”。Maintainer 会通过标签、Review、CI、测试和 ADR 管理项目质量。

\## 实践成果

\- 成功安装和构建 Moss；

\- 离线测试 177 项通过，8 项联网测试跳过；

\- 成功运行 WMON Wrap 和 Kuru Swap 模拟；

\- 两次模拟均为 `reverted: falsewarnings: []`；

\- 掌握了 Fork、Branch、Commit、Push、PR 和 Review 的基本流程；

\- 学会了先检查 Issue、Assignee、评论和关联 PR，避免重复贡献。

\## 今日收获

我最大的收获是：可靠的 AI Agent 不是“什么都敢自动做”，而是知道什么时候必须读取规则、模拟验证、停止操作并把最终权限交还给用户。开源贡献也不只是写代码，还包括文档、测试、问题说明和协作沟通。
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->




完成组队，拟定了团队分工计划，我的角色：PM + Tech  
继续学习Monad链 + MOSS相关知识、源码  
和团队成员一起持续讨论中……
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->





听了分享会，关于未来agent经济和web3国内外社媒推广营销方向

  
**智能合约安全与防御工程 (Security & Engineering)**

-   **漏洞攻防：** 深度拆解了 Web3 杀伤力最大的重入攻击（Reentrancy Attack）原理，并牢记了企业级防御黄金法则——**CEI 模式（检查-生效-交互）**。
    
-   **前沿视野：** 了解了 ZK（零知识证明）技术如何解决链上金融隐私与以太坊 Layer 2 扩容痛点。
    
-   **工程部署：** 掌握了 Hardhat 等工具的标准化合约上链 SOP（密钥隔离、环境配置、节点广播）。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->






### 自动做市商 (AMM) 与流动性池

传统的中心化交易所使用订单薄（Order Book）来匹配买家和卖家。但在去中心化世界中，为了提高效率和去中心化程度，AMM 应运而生。AMM 依靠流动性池（Liquidity Pools）运作。流动性池是智能合约中锁定的资金，通常由一对代币组成（例如 ETH 和 USDC）。

用户（称为流动性提供者，LP）将等值的两种代币存入池中。交易者在这个池子中进行交易，而不是与特定的买家或卖家交易。作为提供流动性的回报，LP 可以获得交易者支付的交易费，这就是流动性挖矿（Yield Farming）的基础收益来源之一。

### 恒定乘积公式 (Constant Product Formula)

AMM 如何决定代币的价格呢？最经典的算法是 Uniswap V2 推广的恒定乘积公式：**x \* y = k**。

-   **x** = 池中代币 A 的数量
    
-   **y** = 池中代币 B 的数量
    
-   **k** = 恒定乘积（在没有新的流动性添加或移除时，这个值保持不变）
    

当交易者从池中买走代币 A（x 减少），为了保持 k 不变，必须向池中放入代币 B（y 增加）。这种数量变化决定了代币的相对价格。

### 无常损失 (Impermanent Loss)

无常损失是 LP 面临的最大风险之一。当你向 AMM 池提供流动性时，如果池中代币的价格（相对于你存入时的价格）发生变化，你此时撤回流动性所得到的代币总价值，会**低于**你当初如果只是将这些代币一直持有在钱包里的总价值。这种差额就是无常损失。

它之所以被称为“无常”，是因为如果代币价格最终回到了你最初存入时的水平，这个损失就会消失。但这在加密市场剧烈波动的情况下很难发生。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->







参加了这周例会和co-learning  
完成了上一个项目，今天浅浅休息一下，周末继续
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->








继续学习DeSci相关内容以及和医药大佬聊了它未来的应用方向

### DeSci 药物专利绑定 IP-NFT 的核心难点

让现实世界中的实物专利成功绑定链上 IP-NFT 并向千名持有人分红，最棘手的难点不仅在智能合约代码，而在**链下法律映射（Legal-to-Code Bridging）与证券化合规（Securities Compliance）**：

-   **难点一：实体法律主权的锚定（法律与代码的桥接）：** 智能合约无法直接强制现实世界各国的专利局。必须在链下设立专门的法律实体（如 SPV 特殊目的载体或瑞士/开曼 DAO 实体）来真正持有纸质专利文件，通过签署严格的现实法律协议（如 Molecule 提出的 IP-NFT 法律框架），在法律上明确该实体产生的全部商业利润独家授权并归宿于链上对应 IP-NFT 的持有者。
    
-   **难点二：代币化分红的金融合规（防范证券法打击）：** 将药物销售版税自动化分红给成千上万个链上散户地址，在大多数国家（特别是美国 SEC）会被直接界定为**未经注册的非法证券发行**。智能合约层必须放弃无权限的自由转账，转而集成合规证券型代币标准（如 ERC-3643），确保只有通过了现实 KYC/AML（身份与反洗钱认证）的白名单地址，才能持有该分红份额并领取收益。
    

### 智能合约部署基础版（Hardhat vs Foundry）

**方案 A：Hardhat 部署脚本 (**`scripts/deploy.ts`**)**

TypeScript

```
import { ethers } from "hardhat";

async function main() {
  const [deployer] = await ethers.getSigners();
  console.log("Deploying contract with the account:", deployer.address);

  // 获取合约工厂并部署
  const myContract = await ethers.deployContract("MyContract");
  await myContract.waitForDeployment();

  console.log(`MyContract deployed successfully to: ${myContract.target}`);
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

**方案 B：Foundry 部署脚本 (**`script/Deploy.s.sol`**)**

Solidity

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "forge-std/Script.sol";
import "../src/MyContract.sol";

contract DeployScript is Script {
    function run() external {
        // 读取环境变量中的私钥并开启链上广播
        uint256 deployerPrivateKey = vm.envUint("PRIVATE_KEY");
        vm.startBroadcast(deployerPrivateKey);

        // 实例化合约
        MyContract myContract = new MyContract();

        vm.stopBroadcast();
    }
}
```

继续学习写合约
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->









学习了DeSci相关的内容/学习手册，Dapp开发流程

### Traditional Web vs. Web3 (DApp) Architecture

The fundamental difference lies in where the backend logic and data are stored.

-   **Traditional (Web2):** Your frontend talks to a centralized backend server via APIs. The server processes logic and reads/writes to a centralized database (like PostgreSQL or MongoDB). The company controls the server and the data.
    
-   **Decentralized (Web3/DApp):** The frontend talks to a decentralized blockchain via a Web3 provider. The "backend" logic is defined by smart contracts, and the "database" is the blockchain itself. No single entity controls the network.
    

### The Standard DApp Architecture

A typical DApp consists of three main components:

1.  **The Frontend:** The user interface (built with React, Vue, etc.). It looks and acts like a normal website, but it requires a specific library (like ethers.js or web3.js) to understand blockchain data.
    
2.  **The Smart Contract (The Backend):** The immutable code deployed on the blockchain. It contains the core business logic (e.g., how tokens are transferred, how votes are counted). It replaces the centralized server.
    
3.  **The Web3 Provider (The Bridge):** Because a normal web browser cannot directly talk to a blockchain node, it needs a bridge. Wallets like MetaMask inject a provider object into the browser, allowing the frontend to send transactions to the blockchain on the user's behalf.
    

1

User Interaction

The action begins

A user clicks a button on the frontend (e.g., "Mint NFT").

2

Provider Connection

The bridge is crossed

The frontend uses a library (like ethers.js) and the Web3 Provider (MetaMask) to create a transaction request.

3

Wallet Signature

User authorization

MetaMask pops up, asking the user to sign the transaction with their private key and pay the gas fee.

4

Blockchain Execution

The smart contract runs

The transaction is broadcast to the network. Miners/validators execute the smart contract code and record the state change on the blockchain.
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->










学习solidity的各种函数  
听了两场分享会，对Desci方向很感兴趣，阅读了一些相关资料，再研究研究，看能不能从传统科研往这个方向发展
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->











做了一个小项目来更好的学习和实践如何接入钱包，如何用agent管理钱包，以及各环节的安全审查
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->












### 智能合约标准化部署 SOP

为了确保线上资产的安全与合约执行的可靠性，规范的工程部署须严格遵从以下四步工作流：

-   **第一步：代码编译与底层优化（Compile & Optimize）** 确保 `pragma` 版本锁定一致。在配置文件中开启编译器优化（如 `optimizer: { enabled: true, runs: 200 }`），以降低实际部署时的 Gas 消耗和用户未来的调用成本。
    
-   **第二步：环境变量与密钥隔离（Environment & Security）** 严禁将私钥直接硬编码在业务脚本中。必须结合 `.env` 配置文件与环境变量管理工具（如 `dotenv`），通过读取 `process.env.PRIVATE_KEY` 或 Foundry 的加密 KeyStore 导入敏感凭证。
    
-   **第三步：节点连接与网络估算（RPC & Gas Estimation）** 接入可靠的去中心化节点服务商（如 Alchemy 或 Infura）获取稳健的 JSON-RPC 接口。在发送部署交易前，利用开发工具的 Gas 估算接口评估当前网络基准费用，确保部署账户内保有充足的原生代币（如 ETH）用于支付矿工费。
    
-   **第四步：上链广播与开源验证（Broadcast & Verification）** 交易确认上链后，立刻使用工具的插件（如 `@nomicfoundation/hardhat-verify` 或 `forge verify-contract`），传入 Etherscan API Key 和精确的构造函数参数。这能将链上的二进制字节码反编译映射为开源的 Solidity 源代码，为社区和用户提供代码透明度与可信度。
    

### 部署工程安全红线与自检清单

-   \[ \] **构造函数检查**：确认初始化参数（如 `initialOwner` 或初始代币铸造数量、接收地址）在脚本中传参准确无误。
    
-   \[ \] **权限转移验证**：若使用了 OpenZeppelin 的 `Ownable` 或 `AccessControl`，需确认部署后管理员权限是否需要从部署账户（Deployer）转移至多签钱包（Multisig/Gnosis Safe）或 DAO 治理合约。
    
-   \[ \] **测试网先行原则**：任何主网部署前，必须在 Sepolia 等测试网上进行完整的部署-调用-升级（若为代理合约）全链路闭环验证。
    

智能合约一旦部署完成便具备不可篡改性，“落子无悔”的特性决定了部署不仅是写完代码后的最后一步操作，更是连接本地开发与去中心化主网状态最严肃的安全关口。严格执行自动化、模块化且具备完备验证链条的部署流程，是衡量一名成熟 Web3 合约工程师的核心标准。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->













### 今日继续学习 智能合约开发 (Smart Contract Development)

智能合约是部署在区块链网络上的自执行代码，其本质是运行在去中心化状态机（如 EVM）上的程序，通过网络中的事务（Transaction）触发状态转移，践行了 Web3 的“Code is Law”原则 。

1\. 核心技术特性

-   **开发语言 (Solidity)**：当前 EVM 生态最主流的高级编程语言 。它是面向对象、静态类型的，专为数字资产和去中心化状态流转设计 。
    
-   **部署与不可篡改性**：合约的字节码一旦部署上链，即获得不可篡改属性 。除了在架构初期预置复杂的代理模式（Proxy Pattern）逻辑外，原生代码无法进行热更新，这对代码的鲁棒性提出了极高要求 。
    
-   **计算与存储成本 (Gas)**：网络节点执行指令与存储状态需要消耗 Gas 。由于区块链的链上存储（Storage）极其昂贵，开发者必须在代码层进行严格优化，以降低执行复杂度与存储开销 。
    

2\. Solidity 合约的标准架构

根据 Solidity 开发规范，一份标准合约通常包含以下核心结构 ：

-   **编译指令 (Pragma)**：声明编译器版本限制（如 `pragma solidity ^0.8.0;`），这在底层直接影响编译器的优化规则与内置安全特性 。
    
-   **状态变量 (State Variables)**：永久存储于区块链上的数据结构，直接决定了合约的全局持久化状态 。
    
-   **函数 (Functions)**：合约的执行逻辑实体。
    
    -   **可见性控制**：通过 `public`/`private`/`external`/`internal` 严格界定函数的调用作用域。
        
    -   **状态可变性**：通过 `view` 或 `pure` 声明不消耗 Gas 的链下只读调用；通过 `payable` 允许该函数接收原生代币入账 。
        
-   **修饰器 (Modifiers)**：提取并复用前置条件校验逻辑，通常包含 `require` 断言，最常用于权限收敛与状态前置检查（如 `onlyOwner`） 。
    
-   **事件 (Events)**：利用 EVM 的日志（Logs）基础设施，将状态变更信息低成本地抛出至链下，供前端 DApp 或索引节点（如 The Graph）进行监听和同步 。
    

3\. 安全工程与防御实践

由于智能合约管理着核心资产且字节码全网开源，上线前必须经过专业的代码审计（Audit） 。以下是三大核心安全考量：

-   **重入攻击 (Reentrancy)**：当合约通过 `call` 调用外部不受信任的地址时，攻击者可利用回退函数（Fallback）劫持执行流，在当前状态尚未更新前反复重入当前函数提款。**防御规范**：必须遵循 检查-生效-交互（Checks-Effects-Interactions, CEI）模式，或在函数级引入防重入锁（如 OpenZeppelin 的 `ReentrancyGuard`） 。
    
-   **访问控制 (Access Control)**：未受保护的敏感管理函数被恶意调用。**防御规范**：针对高危接口实施严格的鉴权，通过 `Ownable` 修饰符或基于角色的访问控制树（Role-Based Access Control）进行隔离 。
    
-   **算术溢出 (Integer Overflow/Underflow)**：早期版本（< 0.8.0）的算术运算可能导致数值环绕漏洞。**防御规范**：自 Solidity 0.8.0 起，编译器已默认开启了运算的溢出与下溢检查，发生异常会自动触发 `revert`，开发者应尽量使用 0.8.0 及以上版本的编译器 。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->














学习了怎么写智能合约，搭建了本地开发环境，基本上能看懂简单的合约，各种不同的语言都了解了一下。  

1. 什么是智能合约？ —— “不会耍赖的自动售货机” 

-   **通俗解释**：在 Web3 中，智能合约（Smart Contract）就是一段写在区块链上的代码 。只要预设的条件被满足，它就会自动执行 。\* **实际作用**：就像自动售货机，你投币并满足预设条件，它就会出饮料 。中间没有任何中介干预，也没有人能私吞你的资产，这贯彻了 Web3 中“Code is Law（代码即法律）”的原则 。
    

2. 用什么开发？(Solidity) —— “以太坊上的‘普通话’” 

-   **通俗解释**：你要和这台机器沟通并告诉它怎么运行，就需要用到专门的编程语言，目前以太坊生态中最主流的语言叫做 Solidity 。
    
-   **实际作用**：它专门为了处理“钱”和“资产”而生 。学会了 Solidity，你就能在区块链上发行代币、创造 NFT，或者搭建 DeFi 应用 。
    

3. 上链部署 (Deployment) —— “一诺千金，落子无悔” 

-   **通俗解释**：传统 App 有 Bug 可以随时更新代码，但智能合约一旦“部署”到区块链上，就永远无法修改（除非有极其复杂的提前设计） 。
    
-   **实际作用**：这种“不可篡改”的特性带来了绝对的信任，因为开发者自己也不能偷偷修改规则卷款跑路 。这也意味着写代码必须极其严谨，一旦出错就无法挽回 。
    

4. Gas 费 (矿工费) —— “驱动售货机的电费” 

-   **通俗解释**：想让全网的电脑帮你计算和执行合约，就必须给网络节点支付一笔辛苦费 。
    
-   **实际作用**：这就是 Web3 中的 Gas 费 。代码写得越复杂、计算步骤越多，Gas 费就越贵，所以开发者不仅要把代码写对，还要写得“省钱” 。
    

5. 安全与审计 (Security & Audit) —— “防盗门与质检员” 

-   **通俗解释**：由于智能合约里往往锁着大量真金白银且代码是全网公开的，一旦出现漏洞，里面的资产会被黑客瞬间掏空 。
    
-   **实际作用**：在合约正式上线前，必须经过专业的安全公司进行“代码审计（Audit）”，就像找最牛的锁匠来检查防盗门是否牢固 。安全是智能合约开发的第一生命线 。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->















跟着web3实习手册学习，查漏补缺，逐个击破，夯实基础

学习了TEE和ZK，在解决信任场景的时候可以搭配使用

探索Agent经济体，Agent支付相关内容，未来是一个很大的发展方向
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
















1，写简单的合约，部署合约，Remix（直接用AI也行，反正以后合约都是AI写）  
2，听了老师分享会，学习了关于EPF (Ethereum Protocol Fellowship) 和 EIP（Ethereum Improvement Proposal），学习路线、研究方向等等
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

















第一天！  
听了Xiaohai老师的分享会和co-learning，得到了很多实际的建议

这期争取把能做的任务都做了，查漏补缺，把之前不太懂的内容弄明白
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
