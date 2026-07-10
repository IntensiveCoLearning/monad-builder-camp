---
timezone: UTC+8
---

# AGF-DOT

**GitHub ID:** AGF-DOT

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
Ethers.js 今日学习笔记（v6 最新版）

**学习版本**：Ethers.js v6（当前官方主推稳定版本）

**学习目标**：掌握 Ethers.js 核心概念、五大核心模块、基础实操代码，区分版本差异，具备简单链上交互开发能力

## 一、Ethers.js 基础认知

### 1\. 什么是 Ethers.js

Ethers.js 是一款**轻量、安全、高性能**的 JavaScript/TypeScript 区块链开发库，主要用于以太坊及所有 EVM 兼容公链（Polygon、BSC、Arbitrum 等）的链上交互，是目前 Web3 开发的主流工具库。

核心作用：让浏览器/Node.js 程序实现**读取链上数据、连接钱包、签名交易、调用智能合约、监听链上事件**等全套 Web3 操作。

### 2\. 与 Web3.js 核心区别

Web3.js 是以太坊早期官方库，目前逐渐被淘汰；Ethers.js 是社区主流替代方案，优势显著：

-   **体积更小**：压缩后仅80KB左右，前端加载速度更快
    
-   **架构清晰**：严格区分「读（Provider）」和「写（Signer）」操作，逻辑分层明确
    
-   **安全性更高**：原生支持助记词、私钥加密，内置安全钱包机制
    
-   **开发体验好**：原生支持 TypeScript，API 简洁易懂，错误提示精准
    
-   **规避精度问题**：自动处理链上大整数（BigInt），杜绝数值丢失问题
    

### 3\. 版本核心差异

-   **v5**：老牌稳定版本，存量项目居多，API 相对繁琐
    
-   **v6**：官方主推新版，重构大量 API，模块化更强、性能更优，本次学习核心版本
    

## 二、Ethers.js 五大核心模块（重点）

Ethers.js 所有功能均围绕五大模块展开，是开发的核心基础。

### 1\. Provider 区块链读取器（只读）

Provider 是程序与区块链节点的**只读连接入口**，仅负责查询链上数据，无私钥、无签名、无法发起交易。

**核心能力**：查询账户余额、区块信息、交易详情、合约公开数据、解析链上日志

**常用类型**：

-   JsonRpcProvider：自定义 RPC 节点（Infura、Alchemy、本地节点，前后端通用）
    
-   BrowserProvider：v6 新版专属，替代 v5 Web3Provider，专门用于连接浏览器钱包（MetaMask）
    
-   StaticJsonRpcProvider：稳定 RPC 连接，支持自动重试，适合后端批量查询
    

### 2\. Signer 签名器（写入操作）

Provider 只能读链，**所有链上写入操作（转账、合约调用）必须通过 Signer 签名**，是区分读写操作的核心模块。

**两类核心签名器**：

-   浏览器端 JsonRpcSigner：依托用户钱包签名，私钥由钱包保管，安全无风险，DApp 主流方案
    
-   后端 Wallet：代码导入私钥/助记词签名，适用于脚本、链上机器人，需严格做好私钥保密
    

### 3\. Contract 智能合约交互器

专门用于与链上智能合约交互的核心对象，可理解为「链上合约翻译官」，将 JS 代码与链上二进制数据互相转换。

**合约交互三要素（缺一不可）**：

1.  合约地址：智能合约在区块链上的唯一标识
    
2.  ABI：合约接口描述文件，定义合约所有方法、参数、返回值
    
3.  Provider/Signer：读操作传 Provider，写操作必须传 Signer
    

**合约方法分类**：

-   读方法（view/pure）：免费、无需签名、不消耗 Gas，仅查询数据
    
-   写方法：需要签名、消耗 Gas、修改链上数据，交易上链生效
    

### 4\. Utils 工具函数库

Ethers 内置高频工具方法，无需手动封装，解决链上通用数据处理问题：

-   单位转换：parseEther（ETH 转 wei）、formatEther（wei 转 ETH）
    
-   数据加密：keccak256 哈希计算、签名验证
    
-   格式校验：地址合法性校验、ABI 编解码
    
-   钱包工具：BIP39 助记词生成、BIP44 分层钱包推导
    

### 5\. Event 链上事件监听

实时监听智能合约触发的日志事件，可监控转账、NFT 铸造、合约交互等链上动态，常用于 DApp 实时数据更新、链上数据监控。

## 三、v6 核心实操代码（可直接复用）

### 1\. 安装依赖

```bash
npm install ethers
```

### 2\. 前端连接钱包 + 查询余额

```javascript
import { ethers } from "ethers";

// 1. 初始化浏览器钱包提供者（v6 新语法）
const provider = new ethers.BrowserProvider(window.ethereum);

// 2. 请求用户连接钱包
await provider.send("eth_requestAccounts", []);

// 3. 获取签名器（用于后续写入操作）
const signer = await provider.getSigner();

// 4. 获取钱包地址、查询余额
const address = await signer.getAddress();
const balanceWei = await provider.getBalance(address);
const balanceEth = ethers.formatEther(balanceWei);

console.log("钱包地址：", address);
console.log("账户余额：", balanceEth, "ETH");
```

### 3\. 基础 ETH 转账交易

```javascript
// 构建交易参数
const txParams = {
  to: "接收地址",
  value: ethers.parseEther("0.01") // 0.01 ETH 转为 wei
};

// 发起交易（钱包签名）
const tx = await signer.sendTransaction(tx);

// 等待交易上链确认
await tx.wait();
console.log("交易成功，哈希：", tx.hash);
```

### 4\. 读取智能合约数据

```javascript
// 合约基础信息
const contractAddr = "合约地址";
const contractABI = [/* 粘贴合约 ABI */];

// 初始化合约实例（只读，使用 Provider）
const contract = new ethers.Contract(contractAddr, contractABI, provider);

// 调用合约读方法
const result = await contract.合约方法名();
console.log("合约查询结果：", result);
```

## 四、核心易错点 & 避坑总结

-   **读写分离**：Provider 仅可读，调用合约写方法、转账必须传入 Signer，否则报错
    
-   **单位转换必做**：链上所有数值均为 wei（BigInt），展示需 formatEther，传参需 parseEther，直接运算会精度丢失
    
-   **v6 语法变更**：废弃 Web3Provider，统一使用 BrowserProvider；部分工具函数命名简化
    
-   **私钥安全**：前端禁止手动导入私钥，必须依赖钱包签名；后端私钥需配置环境变量，禁止硬编码
    
-   **交易确认**：sendTransaction 后必须执行 wait()，否则无法确保交易上链成功
    

## 五、今日学习总结

1\. 明确 Ethers.js 核心定位：Web3 链上交互主流库，凭借轻量、安全、架构清晰的优势，全面替代 Web3.js，是 DApp 开发、合约调试的核心工具。

2\. 掌握核心架构逻辑：**Provider 读、Signer 写、Contract 交互、Utils 工具、Event 监听**五大模块分工明确，读写分离是 Ethers 最核心的设计思想。

3\. 熟练掌握基础实操：完成钱包连接、余额查询、ETH 转账、合约只读查询基础操作，理解链上数据单位转换、交易签名、上链确认的完整流程。

4\. 后续学习方向：深入学习合约写入调用、链上事件监听、批量数据查询、交易异常处理、Gas 费用自定义配置。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->

## 今日学习笔记

DApp 架构、开发流程、以太坊基础开发环境、RPC 节点基础认知

## 一、DApp 架构核心组成

DApp（去中心化应用）区别于传统中心化 Web 应用，运行在区块链分布式网络，逻辑与数据无单一主体管控，由全网参与者共同维护，整体分为三大核心模块：

### 1\. 前端（User Interface）

1.  技术栈：HTML/CSS/JS，主流框架 React、Vue，和传统 Web 前端技术栈一致。
    
2.  链交互逻辑（和传统 Web 最大区别）：前端**不直接直连区块链**，依靠两种渠道通信：
    
    -   钱包注入的 Provider（MetaMask 等浏览器钱包）
        
    -   第三方 RPC 节点服务商
        
3.  两类交互操作：
    
    -   只读调用（eth\_call）：读取合约数据、链上状态、事件日志，无需签名、不消耗 Gas；
        
    -   写交易操作：修改合约数据，前端构造交易数据，交由用户钱包签名后，通过 RPC 广播上链执行，消耗 Gas。
        
4.  必备集成：Web3 钱包（MetaMask），负责用户链上身份校验、交易签名，保障资产与隐私安全。
    

### 2\. 智能合约（Smart Contracts）

1.  定位：DApp 的业务核心，全部业务规则写在合约内，部署在区块链上；
    
2.  特性：执行规则自动、交易透明、数据不可篡改；
    
3.  以太坊开发标准：使用 Solidity 语言编写，运行在 EVM（以太坊虚拟机）中；
    
4.  作用：存储持久化数据、定义转账 / 业务逻辑、对外提供读写接口供前端调用。
    

### 3\. 区块链网络（底层链）

承载智能合约运行、交易打包共识、数据永久存储的底层分布式网络（以太坊、Monad、Polygon 等 EVM 兼容链）。

## 二、DApp 完整开发流程（梳理总结）

1.  **需求设计**：确定 DApp 业务逻辑，拆分合约存储结构、交互功能；
    
2.  **合约开发**：使用 Solidity 编写智能合约，完成业务逻辑；
    
3.  **本地测试**：搭建本地开发链，单元测试合约，规避漏洞；
    
4.  **前端开发**：搭建页面，集成 web3/ethers 库，对接钱包与 RPC；
    
5.  **联调交互**：前端实现合约读、写两类接口调用；
    
6.  **测试网部署**：将合约部署至公链测试网（如 Monad Testnet），全流程实机测试；
    
7.  **主网上线**：审计合约后部署主网，正式对外提供服务。
    

## 三、以太坊开发环境搭建要点

### 1\. 基础环境准备

-   运行环境：Node.js + npm/yarn（前端、合约编译测试工具依赖）
    
-   合约 IDE：Remix（在线快速调试）、VS Code+Solidity 插件（本地开发）
    
-   钱包工具：MetaMask，用于测试网转账、交易签名、前端页面连接
    

### 2\. 本地开发链

本地私有链（Hardhat、Foundry、Ganache），优势：无 Gas 消耗、交易秒确认，适合合约迭代与单元测试，无需测试网代币。

### 3\. 钱包与前端交互核心逻辑

前端通过注入 Provider 检测用户钱包，监听链切换、账户切换事件；

-   读数据：无需用户授权，直接 RPC 调用合约 view 函数；
    
-   写交易：需要用户连接钱包、授权签名，等待区块确认后更新页面状态。
    

## 四、RPC 节点服务核心认知

### 1\. RPC 定义

JSON-RPC 是区块链与外部程序通信的标准协议，前端 / 工具通过 RPC 接口向节点发送指令，实现查询链数据、广播交易。

### 2\. RPC 在 Web3 开发中的作用

1.  作为前端与区块链中间桥梁，转发读写请求；
    
2.  同步全网区块、交易、合约状态数据；
    
3.  打包用户签名后的交易，发送至矿工节点打包上链。
    

### 3\. RPC 服务分类

1.  公共免费 RPC：上手简单，但延迟高、并发限制大，适合学习；
    
2.  付费专业 RPC（Alchemy、Infura 等）：稳定低延迟、高并发，适合线上 DApp；
    
3.  自建 RPC 节点：完全自主可控，维护成本高，多用于大型项目。
    

### 4\. RPC 使用最佳实践

-   开发阶段：公共测试网 RPC 快速调试；
    
-   线上项目：多服务商 RPC 做故障备份，避免单点服务宕机；
    
-   区分读写请求：高频读操作做缓存，减少 RPC 请求频次。
    

## 五、今日学习小结

1.  理清 DApp 三层分离架构：前端（交互层）+ 智能合约（业务层）+ 区块链（底层存储执行层），分清读写链上操作的区别；
    
2.  掌握 DApp 从合约到前端、本地到测试网的完整开发链路；
    
3.  理解 RPC 节点是前端和区块链通信的核心载体，区分免费 / 商用 RPC 的适用场景；
    
4.  明确开发必备工具栈：Remix/Hardhat 合约开发、MetaMask 钱包、RPC 节点服务、React/Vue 前端框架。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->


**#学习笔记 - 2026年7月8日**

**今日学习目标**

1. 修复钱包连接问题

2. 完成 dApp 前端开发

3. 将 dApp 部署到 Vercel

4. 整理 Mini Demo 0 和 Week 1 Build Log

5. 确认 Week 2 方向

\---

**一、钱包连接问题排查与修复**

**1.1 问题现象**

点击 "Connect MetaMask" 按钮后，页面显示 "Not Connected"，MetaMask 没有弹出授权窗口。

**1.2 问题原因**

使用 wagmi 框架时，状态同步出现问题。虽然 `window.ethereum` 存在且 MetaMask 可以正常工作，但 wagmi 的 `useAccount()` hook 无法正确检测到连接状态。

**1.3 解决方案**

**放弃 wagmi，改用 ethers.js 直接实现钱包连接**

\`\`\`javascript

// 使用 ethers.js 直接连接钱包

const provider = new ethers.BrowserProvider(window.ethereum)

await provider.send('eth\_requestAccounts', \[\])

const signer = await provider.getSigner()

const addr = await signer.getAddress()

\`\`\`

**1.4 关键代码**

**钱包连接组件**: \[WalletConnect.jsx\](file:///d:/Web3暑期实习/solidity-project/my-app/src/components/WalletConnect.jsx)

\`\`\`javascript

useEffect(() => {

  const checkConnection = async () => {

    if (typeof window !== 'undefined' && window.ethereum) {

      try {

        const provider = new ethers.BrowserProvider(window.ethereum)

        const signer = await provider.getSigner()

        const addr = await signer.getAddress()

        setAccount(addr)

      } catch (err) {

        setAccount(null)

      }

    }

  }

  checkConnection()

}, \[\])

\`\`\`

**监听账户变化**:

\`\`\`javascript

const handleAccountsChanged = (accounts) => {

  if (accounts.length > 0) {

    setAccount(accounts\[0\])

  } else {

    setAccount(null)

  }

}

window.ethereum?.on('accountsChanged', handleAccountsChanged)

\`\`\`

**1.5 学习收获**

| 知识点 | 理解 |

| **wagmi vs ethers.js** | wagmi 是封装层，ethers.js 是底层库；当封装层出现问题时，直接使用底层库更可靠 |

| **钱包连接原理** | 通过 `eth_requestAccounts` 请求授权，然后获取 signer 和地址 |

| **事件监听** | 需要监听 `accountsChanged` 事件，处理用户切换账户的情况 |

\---

**二、dApp 前端开发**

**2.1 技术栈**

| 技术 | 版本 | 用途 |

|------|------|------|

| Next.js | 16.2.10 | 前端框架 |

| Tailwind CSS | 4.4.14 | 样式框架 |

| ethers.js | 6.x | 区块链交互 |

**2.2 组件结构**

\`\`\`

src/

├── app/

│   └── page.js          # 主页（客户端组件）

└── components/

    ├── WalletConnect.jsx    # 钱包连接组件

    ├── MessageList.jsx      # 消息列表组件（Read Function）

    └── PostMessage.jsx      # 发布消息组件（Write Function）

\`\`\`

**2.3 合约交互**

**Read Function（读取消息）**

\`\`\`javascript

const provider = new ethers.BrowserProvider(window.ethereum)

const contract = new ethers.Contract(contractAddress, contractAbi, provider)

const messages = await contract.getMessages()

\`\`\`

**特点**: 不消耗 gas，直接从节点读取数据

**Write Function（发布消息）**

\`\`\`javascript

const provider = new ethers.BrowserProvider(window.ethereum)

const signer = await provider.getSigner()

const contract = new ethers.Contract(contractAddress, contractAbi, signer)

const tx = await contract.addMessage(content)

await tx.wait()

\`\`\`

**特点**: 消耗 gas，需要等待交易确认

**2.4 关键代码**

**消息列表组件**: \[MessageList.jsx\](file:///d:/Web3暑期实习/solidity-project/my-app/src/components/MessageList.jsx)

**发布消息组件**: \[PostMessage.jsx\](file:///d:/Web3暑期实习/solidity-project/my-app/src/components/PostMessage.jsx)

\---

**三、部署到 Vercel**

**3.1 部署步骤**

1. **安装 Vercel CLI**:

   \`\`\`bash

   npm install -g vercel

   \`\`\`

2. **登录 Vercel**:

   \`\`\`bash

   vercel login

   \`\`\`

3. **部署项目**:

   \`\`\`bash

   cd my-app

   vercel deploy --prod

   \`\`\`

**3.2 部署结果**

| 链接类型 | URL |

|----------|-----|

| **主链接** | [https://my-gvhshqdyd-zxb1.vercel.app](https://my-gvhshqdyd-zxb1.vercel.app) |

| **备用链接** | [https://my-app-hazel-eight-93.vercel.app](https://my-app-hazel-eight-93.vercel.app) |

**\### 3.3 学习收获**

| 知识点 | 理解 |

|--------|------|

| **Vercel 部署** | Next.js 项目可以轻松部署到 Vercel，提供免费的 HTTPS 访问 |

| **环境要求** | 部署前需要先构建成功`npm run build`） |

| **域名分配** | Vercel 会自动分配一个随机域名 |

\---

**四、Mini Demo 0 整理**

**4.1 Demo 内容**

| 项目 | 详情 |

|------|------|

| **合约地址** | `0x92e00ffd40925aB9364884901290C18Ce2e060B9` |

| **网络** | Monad Testnet |

| **Demo URL** | [https://my-gvhshqdyd-zxb1.vercel.app](https://my-gvhshqdyd-zxb1.vercel.app) |

**4.2 真实链上操作**

\- ✅ 合约部署（已验证）

\- ✅ 3 次消息发布（已验证）

\- ✅ 区块浏览器可查

**4.3 AI 辅助与人工判断**

| 类型 | AI 贡献 | 人工决策 |

|------|----------|----------|

| 代码编写 | 部署脚本、交互脚本、前端组件 | 私钥管理、技术栈选择 |

| 问题排查 | BigInt 类型错误、RPC 连接问题 | RPC 验证、安全配置 |

| 文档编写 | README、Build Log | 功能需求定义 |

\---

**五、Week 1 Build Log**

**5.1 本周核心产出**

1. **智能合约**: MessageBoard.sol

2. **部署脚本**: scripts/deploy.js, scripts/interact.js

3. **dApp 前端**: Next.js + ethers.js

4. **部署结果**: 合约已上线 Monad Testnet，dApp 已部署到 Vercel

**5.2 方向选择：Tech**

**选择理由**:

\- 兴趣驱动：对智能合约开发和 dApp 构建有浓厚兴趣

\- 能力匹配：已有 Java、Vue 开发经验

\- 实践成果：成功完成合约开发、部署和 dApp 构建

\---

**六、遇到的问题与解决方案**

**问题 1: wagmi 钱包连接失败**

| 项目 | 详情 |

|------|------|

| **现象** | 点击连接按钮后，MetaMask 不弹出授权窗口 |

| **原因** | wagmi 的状态同步问题 |

| **解决方案** | 改用 ethers.js 直接实现 |

**问题 2: Hardhat Ignition 需要交互式确认**

| 项目 | 详情 |

|------|------|

| **现象** | `npx hardhat ignition deploy` 需要用户确认 |

| **原因** | Hardhat Ignition 的安全机制 |

| **解决方案** | 使用自定义部署脚本 |

**问题 3: BigInt 类型错误**

| 项目 | 详情 |

|------|------|

| **现象** | `TypeError: Cannot mix BigInt and other types` |

| **原因** | Solidity 的 `uint` 返回 JavaScript 的 `BigInt` |

| **解决方案** | 使用 BigInt 字面量 |

**\### 问题 4: RPC 连接超时**

| 项目 | 详情 |

|------|------|

| **现象** | 使用官方 RPC 连接超时 |

| **原因** | 公共 RPC 节点不稳定 |

| **解决方案** | 切换到 Ankr 的公共 RPC |

\---

**七、今日学习收获**

  **收获 1: ethers.js 直接连接钱包**

**重要性**: ⭐⭐⭐⭐⭐

学会了使用 ethers.js 直接与 MetaMask 交互，理解了钱包连接的底层原理：

1. 通过 `BrowserProvider` 连接到 MetaMask

2. 通过 `eth_requestAccounts` 请求授权

3. 通过 `getSigner()` 获取签名者

4. 通过 `getAddress()` 获取钱包地址

**收获 2: 合约交互的两种方式**

**重要性**: ⭐⭐⭐⭐

理解了区块链上两种操作的区别：

| 操作类型 | 特点 | Gas 消耗 |

|----------|------|----------|

| **Read Function** | 只读，直接从节点获取 | 免费 |

| **Write Function** | 需要签名，写入链上 | 消耗 gas |

**收获 3: dApp 部署流程**

**重要性**: ⭐⭐⭐⭐⭐

学会了将 dApp 部署到 Vercel 的完整流程，理解了：

1. 构建项目`npm run build`）

2. 登录 Vercel 账号

3. 执行部署命令

4. 获取公开访问链接

**收获 4: 问题排查能力**

**重要性**: ⭐⭐⭐⭐

学会了通过调试信息和日志来定位问题：

1. 添加调试面板查看关键信息

2. 使用测试按钮验证底层功能

3. 通过浏览器控制台查看错误日志

4. 根据错误信息调整技术方案

\---

**八、Week 2 学习计划**

**8.1 技术学习目标**

| 目标 | 优先级 | 说明 |

|------|--------|------|

| Solidity 高级特性 | 高 | 继承、接口、修饰器、库 |

| 前端与智能合约交互 | 高 | Wagmi/Viem 框架深入学习 |

| ERC 标准合约 | 中 | ERC20、ERC721 |

| 智能合约测试 | 中 | Hardhat 测试框架 |

**8.2 实践计划**

1. **完善 dApp**: 添加更多功能（用户消息过滤、消息点赞等）

2. **学习 Wagmi**: 解决之前的状态同步问题，掌握 Wagmi 的正确用法

3. **学习 ERC20**: 编写一个代币合约并部署

4. **编写测试**: 使用 Hardhat 编写合约测试用例

\---

**九、需要帮助的问题**

1. **智能合约安全审计**: 常见漏洞和安全编码最佳实践

2. **Wagmi 状态同步**: 如何正确配置和使用 Wagmi

3. **Gas 优化**: 如何降低合约的 gas 消耗

4. **前端性能**: dApp 的性能优化技巧

\---

 **总结**

今天的学习非常充实！主要完成了：

1. ✅ 修复了钱包连接问题（从 wagmi 切换到 ethers.js）

2. ✅ 完成了 dApp 前端开发（消息列表、发布消息）

3. ✅ 将 dApp 部署到 Vercel，获得了公开访问链接

4. ✅ 整理了 Mini Demo 0 和 Week 1 Build Log

5. ✅ 确认了 Week 2 的 Tech 方向

通过今天的实践，我对 Web3 开发有了更深入的理解，特别是钱包连接和合约交互的底层原理。在遇到问题时，学会了通过调试和测试来定位问题，并选择合适的技术方案。期待 Week 2 的深入学习！
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->



````markdown
学习笔记 - 2026年7月7日



 一、今日学习目标

1.  配置 Hardhat 连接 Monad Testnet
2.  编译 Solidity 智能合约
3.  将合约部署到 Monad Testnet
4.  调用合约的 Read Function
5.  调用合约的 Write Function
6.  学习智能合约安全最佳实践

---

 二、项目概述

### 项目名称
MessageBoard（消息板智能合约）

### 功能描述
一个简单的去中心化消息板应用，允许用户发布和查看消息。

### 技术栈
- **框架**: Hardhat
- **语言**: Solidity ^0.8.28
- **网络**: Monad Testnet (Chain ID: 10143)
- **钱包**: MetaMask
- **交互**: ethers.js

---

## 三、关键知识点

### 3.1 Hardhat 网络配置

在 [hardhat.config.js](file:///d:/Web3暑期实习/solidity-project/hardhat.config.js) 中添加自定义网络：

```javascript
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.28",
  networks: {
    monadTestnet: {
      url: process.env.MONAD_TESTNET_RPC_URL,
      accounts: [process.env.WALLET_PRIVATE_KEY],
      chainId: 10143,
    },
  },
};
```

知识点:
- 使用 `dotenv` 加载环境变量，避免硬编码敏感信息
- `accounts` 字段接受私钥数组，第一个私钥作为部署者账户
- `chainId` 用于验证网络身份

### 3.2 环境变量配置

在 `.env` 文件中存储敏感信息：

```env
MONAD_TESTNET_RPC_URL=https://rpc.ankr.com/monad_testnet
WALLET_PRIVATE_KEY=你的钱包私钥
```

**知识点**:
- `.env` 文件必须添加到 `.gitignore` 中
- 使用公共 RPC（如 Ankr）可以快速连接测试网
- 私钥是钱包的核心，泄露会导致资产损失

### 3.3 合约编译

```shell
npx hardhat compile
```

**知识点**:
- 编译产物存放在 `artifacts/` 目录
- Hardhat 会自动检测 Solidity 版本
- 编译错误会在终端显示详细信息

### 3.4 合约部署

创建部署脚本 [scripts/deploy.js](file:///d:/Web3暑期实习/solidity-project/scripts/deploy.js)：

```javascript
const { ethers } = require("hardhat");

async function main() {
  const [deployer] = await ethers.getSigners();
  console.log("Deploying with account:", deployer.address);

  const MessageBoard = await ethers.getContractFactory("MessageBoard");
  const messageBoard = await MessageBoard.deploy();
  await messageBoard.waitForDeployment();

  const contractAddress = await messageBoard.getAddress();
  console.log("Deployed to:", contractAddress);
}

main();
```

执行部署：
```shell
npx hardhat run scripts/deploy.js --network monadTestnet
```

**知识点**:
- `ethers.getSigners()` 获取配置的账户列表
- `ContractFactory.deploy()` 部署合约到链上
- `waitForDeployment()` 等待交易确认
- `getAddress()` 获取部署后的合约地址

### 3.5 部署结果

| 项目 | 值 |
|------|-----|
| 合约地址 | `0x92e00ffd40925aB9364884901290C18Ce2e060B9` |
| 交易 Hash | `0x372d13974a796aa4c37c74b9b35f0c8366c91875972313e867498d84f52b2866` |
| 部署者地址 | `0x000ca106EC26a06c3180a51aC1D56b0b2521bB46` |

### 3.6 合约交互

#### Read Function（只读调用，无需 gas）

```javascript
const count = await contract.getMessageCount();      // 获取消息数量
const messages = await contract.getMessages();       // 获取所有消息
const msg = await contract.getMessageByIndex(0);     // 获取指定索引的消息
```

**知识点**:
- Read Function 不修改链上状态
- 调用 Read Function 不需要支付 gas
- 返回值直接从节点获取

#### Write Function（写操作，需要 gas）

```javascript
const tx = await contract.addMessage("Hello World!");
await tx.wait();
```

**知识点**:
- Write Function 会修改链上状态
- 需要支付 gas 费用
- 返回 Transaction 对象，需等待确认

### 3.7 智能合约安全

#### 敏感信息保护

在 [.gitignore](file:///d:/Web3暑期实习/solidity-project/.gitignore) 中添加：

```
.env                    # 私钥
deployment-info.json    # 部署信息
```

**安全原则**:
1. 永远不要将私钥提交到 Git
2. 使用环境变量管理密钥
3. 定期轮换密钥
4. 使用 Secret Scanning 工具

---

## 四、遇到的问题与解决方案

### 问题 1: RPC 连接超时

**现象**: 使用 `https://testnet-rpc.monad.xyz` 连接超时

**原因**: 部分公共 RPC 可能不稳定或被墙

**解决方案**: 切换到 Ankr 的公共 RPC `https://rpc.ankr.com/monad_testnet`

### 问题 2: Hardhat Ignition 需要交互式确认

**现象**: `npx hardhat ignition deploy` 会询问是否确认部署

**解决方案**: 使用自定义部署脚本 `scripts/deploy.js`，避免交互式确认

### 问题 3: BigInt 类型错误

**现象**: `TypeError: Cannot mix BigInt and other types`

**原因**: Solidity 的 `uint` 类型在 ethers.js 中返回 `BigInt`，不能直接与普通数字运算

**解决方案**: 使用 `BigInt` 字面量（如 `1n`）或转换方法

```javascript
// 错误写法
const lastMsg = await contract.getMessageByIndex(newCount - 1);

// 正确写法
const lastMsg = await contract.getMessageByIndex(newCount - 1n);
```

---

## 五、学习收获

### 技术层面
1. ✅ 掌握了 Hardhat 配置自定义网络的方法
2. ✅ 学会了使用 ethers.js 部署和交互智能合约
3. ✅ 理解了 Read Function 和 Write Function 的区别
4. ✅ 掌握了环境变量和 .gitignore 的安全配置

### 实践层面
1. ✅ 成功将合约部署到真实测试网
2. ✅ 完成了完整的合约交互流程
3. ✅ 学会了排查 RPC 连接问题
4. ✅ 掌握了 BigInt 类型的处理方法

### 安全意识
1. ✅ 理解了私钥保护的重要性
2. ✅ 学会了配置 .gitignore 防止敏感信息泄露
3. ✅ 了解了测试网和主网的区别

---

## 六、下一步学习计划

1. 📚 学习 Solidity 高级特性（继承、接口、库）
2. 📚 学习测试网代币获取方法
3. 📚 学习前端与智能合约交互（Wagmi/Viem）
4. 📚 学习智能合约安全审计基础
5. 📚 尝试部署到其他测试网（Sepolia、Goerli）

````
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->




今天已经把安装插件、创建钱包、添加网络、领水、看区块链浏览器这些步骤全部完成了
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
