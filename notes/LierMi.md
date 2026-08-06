- GitHub ID: 193933043
- Name: LierMi
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
<!-- DAILY_CHECKIN_2026-07-27_START -->
# 2026-07-27

由于残酷共学平台问题导致本人没有打上卡，此条记录为补卡。
<!-- DAILY_CHECKIN_2026-07-27_END -->

# 2026-07-25
<!-- DAILY_CHECKIN_2026-07-25_START -->
# 2026-07-25

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
# 2026-07-24

今日黑客松项目持续中。。

和团队沟通。。

争取这周天确认下周项目主题
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# 2026-07-23

今天主要学习了 Web3 中钱包授权、智能合约和 DeFi 协议的安全机制。我认识到，Web3 的真正安全边界是链上合约，而不是项目网页：断开钱包不代表取消授权，隐藏按钮也不能阻止别人直接调用合约。

在合约开发方面，我学习了重入攻击、外部调用返回值、状态机以及代理升级等问题。合约不仅要实现正常业务流程，还要考虑恶意调用顺序、失败情况和升级后的数据兼容性。

在 DeFi 方面，我了解到预言机价格需要检查时效性和抗操纵能力，Swap 也必须设置最低到账金额和过期时间。对于签名，则需要通过 EIP-712、nonce、有效期和域分离防止签名被误用或重复使用。

今天最重要的认识是：**Web3 安全不能只验证“正常情况下能不能运行”，还要思考权限是否最小、数据是否可信、操作能否被重复、价格能否被操纵，以及发生事故后能否及时停止损失。**
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
# 2026-07-22

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
# 2026-07-21

完成组队，拟定了团队分工计划，我的角色：PM + Tech  
继续学习Monad链 + MOSS相关知识、源码  
和团队成员一起持续讨论中……
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->
# 2026-07-20

听了分享会，关于未来agent经济和web3国内外社媒推广营销方向

  
**智能合约安全与防御工程 (Security & Engineering)**

-   **漏洞攻防：** 深度拆解了 Web3 杀伤力最大的重入攻击（Reentrancy Attack）原理，并牢记了企业级防御黄金法则——**CEI 模式（检查-生效-交互）**。
    
-   **前沿视野：** 了解了 ZK（零知识证明）技术如何解决链上金融隐私与以太坊 Layer 2 扩容痛点。
    
-   **工程部署：** 掌握了 Hardhat 等工具的标准化合约上链 SOP（密钥隔离、环境配置、节点广播）。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
# 2026-07-18

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
# 2026-07-17

参加了这周例会和co-learning  
完成了上一个项目，今天浅浅休息一下，周末继续
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
# 2026-07-16

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
# 2026-07-15

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
# 2026-07-14

学习solidity的各种函数  
听了两场分享会，对Desci方向很感兴趣，阅读了一些相关资料，再研究研究，看能不能从传统科研往这个方向发展
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
# 2026-07-13

做了一个小项目来更好的学习和实践如何接入钱包，如何用agent管理钱包，以及各环节的安全审查
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

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
# 2026-07-10

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
# 2026-07-09

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
# 2026-07-08

跟着web3实习手册学习，查漏补缺，逐个击破，夯实基础

学习了TEE和ZK，在解决信任场景的时候可以搭配使用

探索Agent经济体，Agent支付相关内容，未来是一个很大的发展方向
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

1，写简单的合约，部署合约，Remix（直接用AI也行，反正以后合约都是AI写）  
2，听了老师分享会，学习了关于EPF (Ethereum Protocol Fellowship) 和 EIP（Ethereum Improvement Proposal），学习路线、研究方向等等
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
# 2026-07-06

第一天！  
听了Xiaohai老师的分享会和co-learning，得到了很多实际的建议

这期争取把能做的任务都做了，查漏补缺，把之前不太懂的内容弄明白
<!-- DAILY_CHECKIN_2026-07-06_END -->



<!-- DAILY_CHECKIN_2026-07-28_START -->
# 2026-07-28

打卡测试
<!-- DAILY_CHECKIN_2026-07-28_END -->

<!-- DAILY_CHECKIN_2026-07-29_START -->
# 2026-07-29

黑客松开发中
<!-- DAILY_CHECKIN_2026-07-29_END -->

<!-- DAILY_CHECKIN_2026-07-30_START -->
# 2026-07-30

黑客松项目开发的一些心得，做中学，学中做。

<br />

\# 一、写合约最该记住的九件事

\## 1. 钱的守恒，要用减法写，不能用加法

\`\`\`solidity
// ❌ 直觉写法 —— 加法可能溢出
if (toAgent + toClient + frozen != escrowed) revert;

// ✅ 实际写法 —— 逐级收敛，每一步都不可能溢出
if (toAgent > escrowed
&#x20;   || toClient > escrowed - toAgent
&#x20;   || frozen != escrowed - toAgent - toClient) revert InvalidAllocation(...);
\`\`\`

\*\*为什么：\*\* 三个数相加可能超过 uint256 上限，绕回一个小数字，恰好等于 \`escrowed\`——检查就被绕过了。改成"每一步都先减、保证不会为负"，就没有溢出空间。

\> \*\*通用原则：涉及钱的比较，永远想一下"这个算术能不能被撑爆"。\*\*

\## 2. 不要用自增 ID

\`\`\`solidity
// ❌ uint256 public globalTaskId;  taskId = globalTaskId++;
// ✅
taskId = keccak256(abi.encode(block.chainid, address(this), msg.sender,
&#x20;                             requirementsHash, deadline, msg.value));
\`\`\`

\*\*为什么：\*\* 自增 ID 意味着\*\*每一笔交易都要读写同一个存储位置\*\*。在 Monad 这种并行执行的链上，所有交易都会争抢这一格，被迫串行甚至重新执行。用哈希算 ID，每个任务落在自己的格子里，互不干扰。

\*\*代价：\*\* ID 完全由参数决定 → 同样的参数创建第二次会撞。见第 8 条。

\## 3. Checks-Effects-Interactions（检查 → 改状态 → 再转账）

顺序不能反。\*\*先把状态改成"已支付"，最后才真的转钱。\*\*

\*\*为什么：\*\* 转账会把控制权交给对方。如果对方是合约，它可以在收到钱的瞬间反过来再调用你一次——这时如果状态还没改，它就能重复领钱。这叫\*\*重入攻击\*\*。

\## 4. 重入锁要按业务对象隔离，不要用全局锁

\`\`\`solidity
modifier nonReentrant(bytes32 taskId)   // ✅ 按 taskId 隔离
// 而不是一个全局的 locked 布尔值
\`\`\`

\*\*为什么：\*\* 全局锁会让 A 任务的回调把 B 任务也锁住——功能上正确，但制造了不必要的状态争用（回到第 2 条的问题）。

\## 5. 推（push）改成拉（pull）：转账失败不能卡死流程

直接给对方转钱如果失败（对方是个会 revert 的合约、或 gas 不够），\*\*不要让整笔交易回滚\*\*，而是记一笔"欠他多少"，让他自己来提。

\*\*为什么：\*\* 一个收款方出问题，不能拖垮另一个收款方，更不能让案子永远结不了。

\> 这是 Solidity 里非常经典的模式：\*\*Pull over Push\*\*。

\## 6. 权限要职责分离，转移要两步

\`\`\`
settlementAuthority   能结算、能释放冻结的钱
authorityAdmin        immutable，只能"提名"新的 authority
两者强制必须是不同地址
\`\`\`

换人流程：\*\*admin 提名 → 新地址主动接受 → 旧的立即失权。\*\*

\*\*为什么要两步：\*\* 如果一步到位，手滑填错一个地址，权限就永久掉进一个没人能打开的黑洞。要求新地址主动接受，等于证明"这个地址是活的、有人控制"。

\## 7. 功能没写完时，用 constructor 挡住部署

\`\`\`solidity
constructor() {
&#x20;   if (block.chainid != 31337) revert Gate3IncompleteLifecycle(block.chainid);
}
\`\`\`

\*\*为什么：\*\* 这个版本只能锁钱、没有任何取钱路径。一旦部署到真实网络，锁进去的钱就永远出不来。用构造函数把它钉死在本地测试链，等生命周期写完再移除。

\> \*\*合约不可修改\*\*，所以"防止自己犯错"要写进代码，不能靠记性。

\## 8. 确定性带来的两个陷阱

因为 \`taskId\` 完全由参数算出，没有随机项：

\| 陷阱 | 后果 |
\|---|---|
\| 同样的参数创建第二次 | \`TaskAlreadyExists\` —— \*\*彩排跑两遍就撞\*\* |
\| 参数里的时间戳写死 | 过了那个时间就 revert —— \*\*今天正常，下周失败\*\* |

\*\*解法：deadline 在运行时计算（\`现在 + 1 小时\`），不要用固定日期。\*\*
这样时间永远在未来，而且每次值都不同，ID 天然唯一。

\> 今天这个 bug 是\*\*延时引爆\*\*的：写代码那天还有 46 小时才过期，一切正常，几天后突然开始失败，看起来像莫名其妙的回归。\*\*跟时间有关的常量，要主动去想"它什么时候会过期"。\*\*

\## 9. 链上只存哈希，统计放链下

正文、文件、大段 JSON 都不上链，只上一个哈希。全网统计（案件总数、成功率）通过\*\*读事件\*\*在链下算，不在合约里维护一个全局计数器。

\*\*为什么：\*\* 上链的每个字节都要花钱；而且全局统计变量又会撞上第 2 条的状态争用问题。

\---

\# 二、测试：单元测试之外还有一层

\## 不变量测试（Invariant Testing）

\| | 单元测试 | 不变量测试 |
\|---|---|---|
\| 我写什么 | "调用 A 之后，结果应该是 B" | "\*\*不管怎么调用，这句话永远成立\*\*" |
\| 谁决定调用顺序 | 我 | 机器随机排列组合 |

今天见到的这条：

\`\`\`
invariant\_PerTaskLiabilitiesNeverExceedOriginalEscrow()
&#x20; runs: 1000, calls: 500000, reverts: 0
\`\`\`

翻译：\*\*跑了 1000 轮、50 万次随机操作\*\*（创建/交付/验收/争议/退款/释放/提取 七个动作乱序组合），每次都验证"每个任务欠出去的钱，永远不超过最初存进来的钱"。

\*\*\`reverts: 0\` 很关键\*\*——说明这 50 万次全都真的执行了，不是靠报错蒙混过去的。

\> 单元测试验证你\*\*想到的\*\*情况；不变量测试帮你找\*\*没想到的\*\*组合。

\## fuzz 测试

\`\`\`
testFuzz\_CreateTaskRetainsExactEscrowAmount(uint96, bytes32, uint64) (runs: 1000)
\`\`\`

机器随机生成 1000 组参数去调用。用来找边界值（0、最大值、临界点）。

\---

\# 三、工程习惯（非合约）

\## 1. \`.gitignore\` 要写宽，不要写精确

\| | |
\|---|---|
\| ❌ \`.env\` | 挡不住 \`.env.local\`、\`.env.production\` |
\| ✅ \`.env\*\` | 全挡住 |
\| ❌ \`node\_modules/\` | \*\*带斜杠只匹配目录，挡不住同名符号链接\*\* |
\| ✅ \`node\_modules\` | 两者都挡 |

今天真的因为第二条把一个符号链接提交进去了。\*\*写宽一点比写准一点安全。\*\*

\## 2. 什么时候该让人审，什么时候直接合

\| 改动 | 做法 |
\|---|---|
\| 只有自己用 | 直接合，群里说一句 |
\| 别人要用 | 直接合，但发一条"怎么用" |
\| \*\*改了共同约定\*\*（接口、范围、预算、对外口径） | \*\*合并前问一声\*\* |

\*\*审核不是礼节，是有成本的往返。\*\*只在改动跨过别人接口时才值得。

\## 3. 自己合并可以，但不能不说

PR 正文本身就是记录。即使自己合，改了什么、验证了什么都留在那儿。
\*\*风险不是代码质量，是队友不知道有这个东西存在，然后重复造一遍。\*\*

\## 4. 别人说"测试通过"，和你亲眼看到通过，是两件事

今天之前三次审 PR，我都只能说"测试结果只有作者的本地记录"。装上 forge 跑完 33 个测试之后，这句话才变成"已独立验证"。

\*\*尤其是涉及钱的代码。\*\*

\## 5. 时间成本要算往返，不只算工作量

三人小队最贵的不是干活，是等待。所以：

\- 接口先行——前端用假数据先做，不等后端
\- 环境搭建和数据结构分开——第一小时三个人可以完全并行

\---

```markdown
```
<!-- DAILY_CHECKIN_2026-07-30_END -->

<!-- DAILY_CHECKIN_2026-08-01_START -->
# 2026-08-01

黑客松项目开发中遇到的问题总结：

<br />

&#x20; 1\. 托管合约 — 资金守恒要用减法写，加法会溢出

  2. Monad 并行 EVM — 不用自增 ID，用 keccak256 算 ID

  3. 重入攻击 — CEI 顺序 + 按 taskId 隔离的锁

  4. Pull over Push — 转账失败转可提取额度，不拖垮别人

  5. 权限设计 — 职责分离 + 两步转移

  6. 哈希承诺 — 权重事前钉死，争议后改不动

  7. 部署验证 — 我怎么直接查链核对的（含 immutable 那个坑）

  8. chainId 与部署守卫 — 用 constructor 挡住未完成的合约上真网

  9. Foundry — 单元/fuzz/不变量三种测试的区别

  10. wagmi/viem — 构造层和 hooks 拆开

  11. ERC-8004 / ERC-7710 / x402 — Agent 上链的新标准

  12. Moss — 签名前解释，以及 verb 语义损失怎么诚实记录

  13. 我犯过的三个错 — 时间常量延时引爆、^0.70.0 升不上去、临时环境验证等于没验证
<!-- DAILY_CHECKIN_2026-08-01_END -->

<!-- DAILY_CHECKIN_2026-08-02_START -->
# 2026-08-02

## 测试网、chainId 与部署守卫

### chainId 是网络身份

```
Monad Mainnet   143
Monad Testnet   10143
本地 Foundry    31337
```

### 用 constructor 挡住部署

Gate 3 阶段的合约只能锁钱、**没有任何取钱路径**——一旦部到真实网络，钱就永远出不来。

```solidity
constructor() {
    if (block.chainid != 31337) revert Gate3IncompleteLifecycle(block.chainid);
}
```

\*\*合约不可修改，所以"防止自己犯错"必须写进代码，不能靠记性。\*\*资金生命周期写完并测试通过之后，这道守卫才被移除。

### 同类问题：协议要声明自己属于哪条链

Moss 的 Registry 有一道守卫：

```ts
if (config.chainId !== undefined && config.chainId !== runtimeChainId) throw;
```

注意 `!== undefined`——**没声明的协议在任何链上都放行**。一个写死了测试网地址的协议如果漏声明，接到主网上会构造出指向陌生地址的交易。

***

## Foundry 工具链

| 测试类型        | 我写什么                 | 谁决定输入      |
| :---------- | :------------------- | :--------- |
| 单元测试        | "调用 A 之后结果应该是 B"     | 我          |
| **fuzz 测试** | "对任意输入，这个性质都成立"      | 机器随机生成参数   |
| **不变量测试**   | "**不管怎么调用**，这句话永远成立" | 机器随机排列组合调用 |

项目实测（本机复跑，33/33 通过）：

```
invariant_PerTaskLiabilitiesNeverExceedOriginalEscrow()
  runs: 1000, calls: 500000, reverts: 0
```

**1000 轮、50 万次随机操作**，七个动作（创建/交付/验收/争议结算/退款/释放/提取）乱序组合，每次都验证"每个任务欠出去的钱不超过最初存进来的钱"。

`reverts: 0` 很关键——说明这 50 万次**全都真的执行了**，不是靠报错蒙混过去。

> 单元测试验证你**想到的**情况；不变量测试帮你找**没想到的**组合。

***
<!-- DAILY_CHECKIN_2026-08-02_END -->

<!-- DAILY_CHECKIN_2026-08-03_START -->
# 2026-08-03

## 重入攻击与防御

### 攻击原理

合约给对方转账时，**控制权交到了对方手里**。如果对方是合约，它能在收钱的一瞬间反过来再调用你——这时如果状态还没改，就能重复领钱。

### 两道防御

**① Checks-Effects-Interactions（检查 → 改状态 → 再转账）**

顺序不能反。先把状态标成"已支付"，最后才真的转钱。

**② 重入锁，且按业务对象隔离**

```solidity
modifier nonReentrant(bytes32 taskId)   // ✅ 按 taskId 隔离
// 而不是一个全局的 locked 布尔值
```

全局锁会让 A 任务的回调把 B 任务也锁住——功能上正确，但**制造了不必要的状态争用**，正好违反第二节的原则。

项目里有专门的测试验证：`test_PerTaskLockAllowsIndependentTaskProgressDuringCallback`。

<br />

## 托管合约（Escrow）

### 是什么

把资金锁进合约，**按事先约定的条件释放**，任何一方都不能单方面拿走。

### 项目里怎么用

```
委托人付 0.2 MON → 锁进 TaskEscrow
  ├─ 验收通过        → 全额给 Agent
  ├─ 过期未交付      → 全额退委托人
  └─ 有争议 → 结算   → 按规则分账，判不了的部分冻结
```

### 关键：资金守恒的写法

```solidity
// ❌ 直觉写法 —— 加法可能溢出绕回小数字，检查被绕过
if (toAgent + toClient + frozen != escrowed) revert;

// ✅ 逐级减，每一步都不可能为负
if (toAgent > escrowed
    || toClient > escrowed - toAgent
    || frozen != escrowed - toAgent - toClient) revert InvalidAllocation(...);
```

**涉及钱的算术，永远先想"能不能被撑爆"。**

***
<!-- DAILY_CHECKIN_2026-08-03_END -->

<!-- DAILY_CHECKIN_2026-08-04_START -->
# 2026-08-04

打卡，项目开发遇到一点问题，主要是前端这一块，希望最后能顺利提交
<!-- DAILY_CHECKIN_2026-08-04_END -->

<!-- DAILY_CHECKIN_2026-08-05_START -->
# 2026-08-05

# 一、法律概念进了代码：可分给付

## 问题

原来的分账是加权求和：

```
C1 按时 25% ✅ + C2 PNG 25% ✅ + C3 透明 25% ✅ = 75%
C4 是猫 25% ⬜ 判不了
→ 付 0.15，冻 0.05
```

**看起来很合理，其实错了。**

## 为什么错

加权求和有个**隐含前提**：条款之间可分割、价值可独立累加。

* 买 10 箱水，送到 7 箱 → 那 7 箱**有独立价值**，付 70% 合理
* 定制一张桌子，少一条腿 → **没有独立价值**，桌子不能用

一张按时交付的、PNG 的、背景透明的**土豆**图，对委托人的价值是零。
格式条款之所以有意义，是因为它服务于「一只猫」这个主体。

> 大陆法系管这个叫**可分给付 / 不可分给付**：
> 判断标准是「部分履行对债权人是否有独立价值」。

## 更要命的一层

我们整个项目的主张是「**判不了的，不替人做决定**」。

但按权重付 75%，本身就是一个价值判断 —— **认定「格式合规值 75%」**，
而这个判断没有依据。

**一边说「我判不了」，一边把 75% 的钱付出去了。**

## 怎么改的

给条款加一个性质字段：

```ts
essential: boolean   // 是否核心条款（不可分给付）
```

| 核心条款状态    | 结果           |
| :-------- | :----------- |
| ✅ 满足      | 按权重正常结算      |
| ❌ 违反      | 全额退委托人       |
| ⬜ **判不了** | **全额冻结，交给人** |

土豆案从 `0.15/0/0.05` 变成 **`0/0/0.2`**。

`violated` 优先于 `undecidable` —— 已经确定不合格，不必再等另一条判出来。

## 留下的一个设计巧思

引擎同时保留了「**不适用这条规则时会怎么分**」：

```ts
essentialOverride.wouldHaveBeen   // { toAgent: "0.15", ... }
```

于是路演可以演成：

> 三个章依次盖下 → 算出「本可支付 0.15」→ **但核心条款判不了，一分钱都不动**

\*\*能算，但克制。\*\*比直接付 75% 有说服力得多。

***

# 二、规范化序列化（canonical serialization）

## 要解决什么

`keccak256` 吃的是**字节**，不是对象。而同一份数据能写成很多种字节：

```
{"id":"C1","weightBps":2500}      键序不同
{"weightBps":2500,"id":"C1"}
{"id": "C1", "weightBps": 2500}   空格不同
[C1,C2,C3,C4] / [C4,C3,C2,C1]     数组顺序不同
```

**意思一样，字节不同，哈希不同 → 承诺失效。**

所以必须钉死唯一写法：键升序、无空格、条款按 id 排序、字段清单固定、带版本号。

## 坑 1：`JSON.stringify` 的第二个参数不是排序器

这是原代码里的真实 bug：

```js
JSON.stringify(e3, Object.keys(e3).sort())
```

看着像「按键排序」，**实际上第二个参数传数组时是一个作用于所有层级的字段白名单**：

```
输入  { b: 1, a: { nested: 2, other: 3 } }
输出  {"a":{},"b":1}          ← 嵌套对象被清空
```

顶层键名恰好不在嵌套层出现，嵌套内容就整个消失。

**后果**：两份完全不同的数据算出同一个哈希。E3 里的 `unsignedTx`、
`simulation`、`semantics` 全被清成 `{}`。

## 坑 2：Date / Map / Set 会静默变成 `{}`

我自己写的递归版本又踩了另一个：

```js
canonicalJson(new Date())  →  "{}"
canonicalJson(new Map())   →  "{}"
canonicalJson(new Set())   →  "{}"
canonicalJson({})          →  "{}"     ← 四者哈希相同
```

**根因**：只判了 `typeof value === "object"`，而这些类型的
`Object.keys()` 都是空数组。

**最难看的地方**：我在文件开头白纸黑字写了「遇到 Date 一律报错」，
**代码根本没做**。

> 📌 **注释描述的是意图，不是行为。这正是需要测试的地方。**

修法是加一道「朴素对象」检查：

```ts
function isPlainObject(v: object): boolean {
  const proto = Object.getPrototypeOf(v);
  return proto === Object.prototype || proto === null;
}
```

## 设计原则：白名单 + 拒绝未知字段

只用白名单会留下静默陷阱：将来有人加了个有语义的字段，
它不在清单里 → **没被承诺**，而谁都不会发现。

所以反过来再卡一道：**出现清单外的字段就直接报错**，
逼加字段的人显式决定「它要不要进哈希」。

```
条款 C1 含未登记字段：sneaky。请在 COMMITTED_FIELDS 中显式决定
它是否进 requirementsHash，并同步升级 CANONICAL_VERSION。
```

## 设计原则：宁可报错，不要猜

canonical 现在拒绝：`undefined`、函数、symbol、BigInt、Date、Map、Set、
RegExp、类实例、浮点数、NaN、Infinity。

> **承诺环节里，静默的转换就是静默的伪造。**

浮点数为什么也拒绝：十进制表示在不同语言/平台间不保证一致。
金额一律用整数 wei 或基点，本来也不该出现浮点。

## 格式要带版本号

```ts
canonicalJson({ version: "req-canon-v1", requirements: [...] })
```

将来 serializer 升级，历史证据还能被认出该用哪套规则复算，
而不是算出一个对不上的值却查不出原因。

***

# 三、哈希必须覆盖「真正存的那份东西」

这是今天最反直觉的一条。

## 犯的错

E3 的哈希是对**生成时的中间对象**算的，而案件档案里存的是**另一个形状**
（domain 的类型多 `rpcFingerprint`，`semantics` 嵌套层级也不同）。

**后果**：第三方拿到案件档案，跑一遍复算，得到的是**另一个值**。
这个字段就只是装饰。

## 正确做法

```
domain 定义存档形状 → moss-bridge 直接产出那个形状
                   → 哈希盖那个形状 → 案件存那个形状
```

**存进去的、算过哈希的、第三方能读到的，必须是同一个东西。**

## 顺带学到的 TypeScript 知识

原来的签名是：

```ts
function verifyE3PayloadHash(e3: { canonicalPayloadHash: string; [k: string]: unknown })
```

**TypeScript 的** **`interface`** **不满足索引签名**，所以 `MossPreSignEvidence`
传不进去，逼得调用处写 `as unknown as Record<...>`。

改成 `object & { canonicalPayloadHash: string }` 就都通了，
那个丑陋的 cast 也能删掉。

***

# 四、证据真实性：归档的必须是**实际发生的**

Neo 这轮审计最有价值的一句话：

> **当前 hash 只能证明内容后来没被改过，不能证明内容是真的。**

这两件事完全不同。哈希防的是**事后篡改**，防不了**一开始就写假的**。

## 犯的三个错

### ① 归档参数 ≠ 实际发出去的参数

实际传给 Moss 的 `requirementsHash` 是**十进制**
（Moss 的参数校验要求非负整数字符串），我归档的是**十六进制**。

而且 `buildE3` 接受调用方任意提供的 `capabilityParams` —— 想写什么写什么。

**修法**：让 `PreparedTask` 携带 `registry.action()` 的原样入参，
`buildE3` 从 task 读，把 `E3Provenance.capabilityParams` **删掉**。
编译期就没法传假的了。

### ② 溯源字段是抄的，而且抄错了

```
代码里写死: mossCommit = 5d70524e…   protocolVersion = 0.1.0
实际用的:   b00ed2db…（moss.lock.json）            0.0.1
```

**一份声称可供第三方复算的证据，如果溯源字段是抄进来的，它只是好看。**

**修法**：改成必填参数，由调用方从 `moss.lock.json` 和 `package.json`
真实读取，**默认值删掉 —— 不给撒谎留位置**。

### ③ 时间线对不上：证据来自另一笔任务

案件时间线写的是 8/1，模拟用的 deadline 是 8/5。**真实模拟成立，
但它不是这个案子的签前证据。**

E3 的全部意义就是「用户在**这笔任务**签名前看到的那句话」。
来自另一笔任务，整条叙事就不成立。

**修法**：整份 fixture 的时间线由那次模拟锚定。

这里有个**环**要注意：

```
C1.expect（截止时间）属于 requirements
      → 决定 requirementsHash
      → 又是模拟的入参
```

所以生成脚本必须按 **deadline → requirements → hash → simulate** 的
顺序一次性对齐，不能分开跑。

## 一条安全规则：不要归档私密 RPC Key

很多付费节点把密钥放在 URL 里：

```
https://user:pass@rpc.example.com/v1/KEY?apikey=SECRET
```

归档完整 URL 等于把密钥写进档案。去敏规则：

* 丢掉 userinfo（`user:pass@`）
* 丢掉**整个** query
* 路径里长度 ≥ 16 的段替换成 `***`（那种长度基本只可能是密钥）
* **保留 host 和路径结构** —— 「用的哪个服务商」本身有验证价值

***

# 五、校验器不该自己崩掉

## 问题

`validateCase()` 直接调用哈希校验，没有捕获异常。

反序列化回来的案件只要深层含有 `Date`、`undefined`，
或者 `canonicalPayloadHash` 不是字符串 —— **校验器自己抛异常**，
调用方**一条 issue 都拿不到**。

## 原则

> **校验器的职责是报告问题，不是自己崩掉。**

包 try/catch，失败记一条结构化的 P0：

```
E3_HASH_UNCOMPUTABLE  该 E3 无法规范化，哈希无从校验：<原因>
```

***

# 六、pnpm 工程：三个真实的坑

## 坑 1：`file:` 协议会**复制**，不是链接

```json
"@themoss/simulator": "file:../../vendor/moss/packages/simulator"
```

`file:` 会把包**复制**进 `.pnpm` 快照，而**快照是安装时拍的** ——
那时 Moss 还没构建，`dist` 是空的。

表现很怪：

```
@themoss/core                        ✅ 能找到
@themoss/simulator                   ❌ 找不到类型声明
@themoss/system                      ❌
@themoss/protocol-silicon-arbitration ❌
```

为什么 `core` 特殊？**它没有自己的依赖，pnpm 直接符号链接了。**

**修法**：这几个包本来就是 `pnpm-workspace.yaml` 的成员，
改用 `workspace:*` → 四个全部变成指向 `vendor/moss` 的符号链接，
构建产物立刻可见。

## 坑 2：`postinstall` 永远来不及

我之前用 `postinstall` 自动 init 子模块。**那个从来就没生效过。**

```
pnpm-workspace.yaml 引用 vendor/moss/packages/*
      → pnpm 必须先解析 workspace
      → 子模块没拉下来
      → ENOENT: scandir …/protocols/silicon-arbitration
      → install 直接失败，postinstall 根本轮不到
```

`preinstall` 我也实测了，**一样来不及** —— pnpm 在**任何**生命周期脚本
之前就要解析 workspace。

**这个先后顺序没有脚本钩子能绕开。** 只能：

```bash
git clone --recurse-submodules <repo>
```

它一直「能用」，只是因为我本地早就 init 过了。

## 坑 3：不要为了构建用不到的包而失败

`vendor/moss` 有 16 个包，其中 `@themoss/abi-tools` 缺 `@types/node`，
全量构建会挂在它的 dts 步骤上。

**而我们只用 4 个。**

```bash
# 按 <pkg>... 语法只构建目标包及其依赖
pnpm -r --filter "@themoss/simulator..." build
```

既绕开了失败，也快得多。

***

# 七、测试

## 零依赖的测试基础设施

项目此前**一个测试都没有**。用 Node 自带的 `node:test`，
不引入任何测试框架：

```ts
import { strict as assert } from "node:assert";
import { describe, it } from "node:test";
```

```json
"test": "node --import tsx --test src/**/*.test.ts"
```

## 什么样的用例值得写

今天写的 52 条里，**几乎每一条都对应一个真实发生过的错误**，
不是假想的边界：

| 用例              | 对应的真实错误                        |
| :-------------- | :----------------------------- |
| 拒绝 Date/Map/Set | 四者算出同一个哈希                      |
| 拒绝 undefined 属性 | `buildE3` 两参数调用直接崩             |
| 嵌套完整保留          | `JSON.stringify(o, keys)` 清空嵌套 |
| 归档参数是十进制        | 归档了十六进制，与实际不符                  |
| 校验器返回 P0 而非抛错   | 嵌套 Date 让校验器崩掉                 |

> **哈希错了不会崩，只会静默地让承诺失效。只能靠测试兜住。**

## 编译期断言也是测试

```ts
const slot: NonNullable<Case["evidence"][number]["mossPreSign"]> = e3;
```

这一行运行时什么都不做，**它的意义在编译期** ——
证明 `buildE3()` 的返回值能直接存进 `Case`。类型对不上就通不过 `tsc`。

***

# 八、协作流程

## Stacked PR（叠加的 PR）

```
#18 (E3)  ← 基于 #17 (canonical)  ← 基于 main
```

**教训**：合并 #17 并删除它的分支时，**GitHub 会自动关闭 #18，而且不允许重开**：

```
Cannot change the base branch of a closed pull request
```

只能新建 PR。所以叠加 PR 要么先 retarget 再合，要么接受要重开。

## 评审边界

Neo 定的顺序很值得学：

> 类型漂移属于 #18 的范围，塞进 #17 会扩大范围；
> 但如果 #18 带着两个不一致的类型合并，它交付的实时 E3 仍然写不进 domain，
> **因此不能留到合并后**。
>
> 最短方案：不新开 #19，不扩大 #17，直接在 #18 修。

**判断标准不是「谁的代码」，而是「这个问题不解决，那个 PR 的交付物成不成立」。**

***

# 九、贯穿今天的一条教训

## 「验证环境和真实环境不一致，等于没验证」—— 今天出现了三次

| 第几次 | 形式                                                           |
| :-- | :----------------------------------------------------------- |
| 1   | 用临时目录的 `node_modules` 做 typecheck，那边装的是最新版                   |
| 2   | `domain` / `moss-bridge` 根本没装 `@types/node`，typecheck 一直是残缺的 |
| 3   | `postinstall` init 子模块「能用」，只因本地早就 init 过了                    |

**第三次尤其典型**：那是我**为了修同一类问题而写的代码**，
自己又犯了同一类错误。

## 唯一有效的做法

真的克隆一次，用文档里写的命令跑一遍：

```bash
git clone --recurse-submodules <repo>
pnpm install --frozen-lockfile
pnpm verify
```

不是「我觉得应该没问题」，是**看到它绿**。
<!-- DAILY_CHECKIN_2026-08-05_END -->

<!-- DAILY_CHECKIN_2026-08-06_START -->
# 2026-08-06

```
ERC-8004   这个 Agent 是谁？信得过吗？        →  身份与信誉
ERC-7710   我授权它做什么？能做到什么程度？    →  授权
x402       它怎么自己付钱？                   →  支付
```

**三个都是「让 Agent 能干活」的基础设施。**

# 一、ERC-8004 · Agent 的身份证和履历

## 解决什么问题

你要雇一个 AI Agent 干活。**但你从没见过它。**

* 它是谁做的？
* 以前干过多少活？
* 有没有搞砸过？
* 有没有人验证过它的输出？

现在这些信息散落在各个平台，**换个平台就归零**。

## 它怎么做

在链上建三个**登记处**：

| 登记处               | 记什么                 | 类比       |
| :---------------- | :------------------ | :------- |
| **身份** Identity   | 这个 Agent 的唯一编号和基本信息 | 身份证      |
| **信誉** Reputation | 谁给过它什么评价            | 履历 / 评价页 |
| **验证** Validation | 谁核查过它的输出，结论是什么      | 第三方体检报告  |

**关键在「链上」**：换平台就带不走的评价，和写在链上谁都能查的评价，是两回事。

***

# 二、ERC-7710 · 可验证的授权书

## 解决什么问题

你让主 Agent 帮你办事，它又雇了一个子 Agent。

**问题来了：子 Agent 到底被允许做什么？**

* 能花多少钱？
* 能不能再转包？
* 只能在今天之内？
* 只能调用特定的合约？

如果这些只是「口头说的」，出事时**根本查不清谁越权了**。

## 它怎么做

把「授权」变成**链上可验证的对象**：

```
我（委托人）→ 授权 → 主 Agent
                        ↓ 再授权（带限制）
                     子 Agent
```

而且授权可以**带条件**（业内叫 caveat，字面意思就是「但书」）：

```
✅ 可以花，但上限 0.2 MON
✅ 可以调用，但只能调这一个合约
✅ 可以执行，但只在今天 12 点前
✅ 可以转包，但不能再往下转
```

**这些限制由合约强制执行，不是靠 Agent 自觉。**

# 三、x402 · Agent 的自动付费

## 解决什么问题

人上网付费要注册账号、绑卡、填验证码。**Agent 干不了这些。**

但 Agent 经常需要付一点点钱：调一次 API、买一次数据、租一秒算力。
金额可能只有几分钱，**走信用卡通道手续费都比货款贵**。

## 它怎么做

它复用了一个存在了三十年、但一直没人用的 HTTP 状态码：

```
404  Not Found         找不到
403  Forbidden         没权限
402  Payment Required  需要付费   ← 这个
```

> **402 是 1990 年代就留在 HTTP 标准里的一个空位**，
> 当年设想「以后网络支付会用到」，结果一直空着。

流程变成：

```
Agent   ：给我这份数据
服务器  ：402 —— 要 0.001 美元，付到这个地址
Agent   ：（自动付款）
服务器  ：给你数据
```

\*\*没有账号、没有 API key、没有订阅。\*\*一次请求一次付费，几分钱也划算。
<!-- DAILY_CHECKIN_2026-08-06_END -->
<!-- Content_END -->
