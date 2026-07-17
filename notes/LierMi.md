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
