---
timezone: UTC+8
---

# Tongfei

**GitHub ID:** Wang-Tongfei

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
## **Week 2 | Moss 学习笔记**

### **一、项目简介**

Moss 是一个将 **Monad 区块链**上复杂的 DApp/协议交互抽象为 AI Agent 可调用统一能力的框架。它的核心工作流是 `discover → load → action → simulate`——由系统而非 Agent 负责组装正确的交易。

Moss 目前处于 **Alpha 阶段**，运行在 Monad 主网（chain id 143）上。它**永不签名、永不发送**交易，只负责构建和验证，私钥始终留在钱包里，最终决定权在用户手中。

### **二、它解决了什么问题？**

**2.1 核心痛点：AI Agent 手搓交易数据太危险**

Moss 要解决的根本问题是：**让 AI Agent 安全地与区块链协议交互**。

考虑一个 DEX 上的“简单”swap：涉及 router 地址、exact-in 与 exact-out 变体、原生代币的包装与解包装、退款和扫尾调用、滑点计算……。一个 AI Agent 通过读取 ABI 来组装这些调用，“最终会把其中某一个搞错”——而一个 **“几乎正确”** 的 AI 构建的交易，恰恰是资金丢失的方式。

**2.2 两条安全规则**

Moss 通过两条在不同位置强制执行的安全规则来防范风险：

| 规则 | 执行位置 | 作用 |
| --- | --- | --- |
| Effects 对账 | 服务器侧（机械判定） | 模拟提取实际发生的资产流出/流入、授权、收款方（包括不发 Transfer 事件的原生 MON 流和 wrapped 代币铸毁），对任何 Plan 未声明的差异产生警告——有 warning 即停 |
| 意图对齐 | Agent 侧 | 将 effects 摘要与用户的原始意图对比——只有 Agent 掌握用户的原话 |

**2.3 为什么现有方案不够？**

如果让 Agent 直接读取合约 ABI 并手搓 calldata，风险在于：

-   ABI 解析复杂，多步调用（如 multicall）容易出错
    
-   不同协议参数格式不统一
    
-   小数位换算、滑点计算等细节极易失误
    
-   一旦 Agent 犯错，后果是直接的资金损失
    

Moss 的解法是：**把复杂性收拢到按协议维护的统一能力层背后**，在每一次签名前加一道机械安全闸门。

### **三、核心能力**

**3.1 四步调用流**

text

```
discover(verb?, category?) → 跨协议发现能力
load(coordinates) → intent、参数语义、风险标签
action(protocol, method, params) → 返回未签名交易 Plan + 量化期望
simulate(plans[]) → 实际 effects + warnings（声明 vs 实际对账）
```

**3.2 Agent 不再手搓 calldata**

Agent 不需要接触 ABI、合约地址、multicall 扫尾、decimals 换算——能力层接受人类可读的参数，返回组装完毕的未签名交易。

**3.3 多步流程支持**

`simulate` 接受 Plan 数组并在计划之间延续状态——Plan B 可以花掉只在 Plan A 模拟结果里才存在的代币。这是多步流程（claim → swap → supply）的基础。

**3.4 已支持协议**

目前支持 WMON（wrap/unwrap/balanceOf）、ERC20 通用协议（任意代币转账/余额/授权查询，含原生 MON）、ERC721 通用协议（任意 NFT 转账/归属查询）、Kuru（市价单 swap、报价、市场列表）。

### **四、可能应用场景**

**场景：AI 驱动的 DeFi 收益策略自动执行**

**描述**：用户用自然语言描述一个收益策略，例如“当 wMON 价格低于 X 时，将 30% 的 USDC 换成 wMON，然后存入 Kuru 的流动性池”。Moss 框架负责：

1.  **discover**：发现可用的 swap 和流动性池能力
    
2.  **load**：加载对应协议的 intent 和参数语义
    
3.  **action**：构建未签名交易的 Plan
    
4.  **simulate**：在真实链上状态模拟执行，验证 effects 是否匹配预期
    

**为什么 Moss 适合这个场景**：

-   Agent 不需要知道 Kuru 的合约地址或 ABI 细节
    
-   模拟机制可以在签名前验证每一步的资产流动是否符合预期
    
-   多步 Plan 链式模拟确保整个流程（swap → 添加流动性）可串联执行
    
-   任何未声明的差异都会产生 warning，阻止危险交易
    

**安全边界**：Moss 不签名、不发送，最终交易由用户在钱包中审核后签名——这确保了即便 Agent 的策略逻辑有误，用户仍有最终否决权。

### **五、我的个人理解**

**5.1 Moss 的本质：为 AI Agent 建立区块链操作的“安全操作系统”**

Moss 做的不是让 Agent 更“聪明”，而是让 Agent 更“安全”。在 AI Agent 自主执行链上操作这个方向上，最大的障碍不是 Agent 的理解能力，而是**可靠性**——一个会犯错但看起来很自信的 Agent，在金融场景中是灾难性的。

Moss 的贡献在于：它把“Agent 如何与链交互”这个开放性问题，变成了“系统提供标准化能力、Agent 只做意图决策、模拟验证作为安全闸门”的封闭框架。这有点像操作系统把硬件抽象成系统调用——应用（Agent）不需要直接操作硬件（合约），只需要调用标准接口。

**5.2 “永不签名”是设计亮点**

Moss 明确声明“永不签名、永不发送”，这不仅是技术选择，更是**信任边界的设计**。在 AI 安全尚未成熟的今天，让 AI 拥有签名权是危险的。Moss 把“构建和验证”交给系统，“签名和执行”留给用户——这个边界划分是务实的。

**5.3 Alpha 阶段的现实提醒**

Moss 目前是 Alpha 软件，未经审计，API 和 Plan 格式可能变动。项目文档也坦诚指出：“模拟是一道安全网，不是保证——模拟结果反映的是模拟那一刻的链上状态，从模拟到签名之间，价格、流动性、合约状态都可能变化。”

这种对自身局限的坦诚，反而增加了项目的可信度。

### **六、参考链接**

-   [Moss GitHub 仓库](https://github.com/nishuzumi/moss)
    
-   [Moss 中文 README](https://github.com/nishuzumi/moss/blob/main/README.zh-CN.md)
    
-   [Moss](https://github.com/nishuzumi/moss/blob/main/SECURITY.md) [Security.md](http://Security.md)
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->

## **Wagmi**

> Wagmi 是 React 的 Web3 工具库，基于 Viem，提供类型安全的 Hooks 与合约交互。

* * *

### **1\. 安装**

bash

```
npm install wagmi viem @tanstack/react-query
```

* * *

### **2\. 配置**

**2.1 定义链（以 Monad 为例）**

ts

```
// wagmi.ts
import { createConfig, http } from 'wagmi';
import { defineChain } from 'viem';

export const monad = defineChain({
  id: 10143,
  name: 'Monad Testnet',
  nativeCurrency: { name: 'Monad', symbol: 'MON', decimals: 18 },
  rpcUrls: { default: { http: ['https://testnet-rpc.monad.xyz'] } },
});

export const config = createConfig({
  chains: [monad],
  transports: { [monad.id]: http() },
});
```

**2.2 包裹 Provider**

tsx

```
// main.tsx
import { WagmiProvider } from 'wagmi';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { config } from './wagmi';

const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById('root')!).render(
  <WagmiProvider config={config}>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </WagmiProvider>
);
```

* * *

### **3\. 核心 Hooks**

### `useAccount`

获取当前连接地址及连接状态。

tsx

```
const { address, isConnected } = useAccount();
```

### `useConnect`

连接钱包（MetaMask 等）。

tsx

```
const { connect, connectors } = useConnect();
// 使用：connect({ connector: connectors[0] })
```

### `useDisconnect`

断开连接。

tsx

```
const { disconnect } = useDisconnect();
```

### `useReadContract`

读取合约数据（只读）。

tsx

```
const { data, refetch } = useReadContract({
  address: contractAddress,
  abi,
  functionName: 'balanceOf',
  args: [address],
  query: { enabled: !!address },   // 条件启用
});
```

### `useWriteContract`

写入合约（发起交易）。

tsx

```
const { writeContract, data: hash, isPending } = useWriteContract();

// 调用
writeContract({
  address: contractAddress,
  abi,
  functionName: 'vote',
  args: [memeId],
});
```

### `useWaitForTransactionReceipt`

等待交易确认。

tsx

```
const { isLoading: isConfirming } = useWaitForTransactionReceipt({
  hash: writeData,   // 来自 useWriteContract 的 data
});
```

* * *

## **4\. 组合示例（投票 + 刷新）**

tsx

```
const { writeContract, data: writeData, isPending } = useWriteContract();
const { isLoading: isConfirming } = useWaitForTransactionReceipt({ hash: writeData });

const { refetch } = useReadContract({ ... });

useEffect(() => {
  if (writeData && !isConfirming) refetch();
}, [writeData, isConfirming]);
```

* * *

### **5\. 重要提醒**

-   **ABI 必须** `as const` 以获得类型推断。
    
-   `args` **条件传递**：当参数可能为 `undefined` 时，使用 `args: address ? [address] : undefined` 配合 `enabled`。
    
-   **QueryClient** 必须放在 `WagmiProvider` 内部。
    
-   所有读写操作均基于 Viem，类型安全。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->


## **Meme Battle 应用开发笔记**

### **📌 项目概述**

基于 **Monad 测试网** 的链上 Meme 对战与排行榜 DApp。核心功能包括：

-   用户提交 Meme（存储 IPFS URI）
    
-   投票给 Meme（每次 +1 分，作者积分增加）
    
-   每日任务（每天完成 +10 分）
    
-   积分达到阈值自动获得徽章（Bronze/Silver/Gold/Diamond）
    
-   积分可兑换奖励（合约中预留可领取奖励机制）
    

技术栈：**Solidity（Remix 可编译）+ React + TypeScript + Wagmi + Viem + Vite**

* * *

### **1\. 环境准备**

-   **Node.js** ≥ 18.0
    
-   **MetaMask** 浏览器扩展
    
-   添加 Monad 测试网到 MetaMask：
    
    -   网络名称：Monad Testnet
        
    -   RPC URL：`https://testnet-rpc.monad.xyz`
        
    -   链 ID：`10143`
        
    -   货币符号：MON
        
    -   区块浏览器：`https://testnet.monadscan.com`
        
-   领取测试币：前往 [Monad Faucet](https://testnet.monad.xyz/faucet) 获取 MON
    

* * *

### **2\. 智能合约（Solidity）**

**2.1 合约代码**

在 Remix 中新建 `MemeBattle.sol`

### **2.2 部署步骤（Remix）**

1.  打开 Remix IDE，创建 `MemeBattle.sol`。
    
2.  编译：选择 Solidity 版本 0.8.20，启用优化。
    
3.  部署：环境选 `Injected Provider`，连接 MetaMask（确保网络为 Monad Testnet）。
    
4.  点击 `Deploy`，确认交易。
    

### **3\. 前端项目构建**

**3.1 项目文件结构**

text

```
meme-battle/
├── src/
│   ├── wagmi.ts
│   ├── contract.ts
│   ├── App.tsx
│   ├── main.tsx
│   └── ...
├── package.json
└── ...
```

**3.2 各文件代码**

（待整理）

### **4\. 运行项目**

1.  确保 `contract.ts` 中的 `contractAddress` 已替换为实际部署地址，且 ABI 正确。
    
2.  终端执行：
    
    bash
    
    ```
    npm run dev
    ```
    
3.  打开浏览器访问 `http://localhost:5173`。
    
4.  点击 **Connect Wallet**，授权 MetaMask 连接。
    
5.  提交 Meme（输入 IPFS URI），投票，完成每日任务，观察积分和排行变化。
    

* * *

### **5\. 注意事项**

-   **ABI 更新**：每次修改合约并重新部署，都需要复制新的 ABI 到 `contract.ts`。
    
-   **Gas 费用**：确保钱包有足够的 MON 测试币。
    
-   **每日任务**：每天只能完成一次，基于 UTC 时间。
    
-   **排行榜优化**：生产环境建议使用后端索引（如 The Graph）替代 `getAllMemes` 链上查询，以降低 Gas 并提升速度。
    
-   **徽章**：积分达到阈值自动触发，可在事件中监听 `BadgeEarned`。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->



## Week 1 总结

### 学习内容

-   了解 Monad 的核心特点：高性能、低延迟、EVM 兼容。
    
-   学习 Solidity 基础语法，包括状态变量、函数、事件和映射（Mapping）。
    
-   完成 OnchainTodo 智能合约练习，熟悉智能合约开发与部署流程。
    
-   思考哪些应用场景更适合部署在 Monad 上。
    

### 产品调研

选择 **Meme Arena（链上 Meme PK 排行榜）** 作为高频交互 Demo。

原因：

-   用户会频繁进行投票、排行榜刷新和任务领取。
    
-   需要低延迟、低成本的链上交互体验。
    
-   链上记录投票、积分和奖励，保证数据公开透明。
    

### 技术方案

计划使用：

-   **Solidity**：开发智能合约
    
-   **Monad Testnet**：部署和测试合约
    
-   **React + Ethers.js/Viem**：实现钱包连接和前端交互
    

### 本周收获

1.  理解了 Monad 更适合高频交互型 Web3 应用，而不仅仅是“更快”。
    
2.  熟悉了 Solidity 智能合约开发的基本流程。
    
3.  学会从产品需求出发，思考哪些数据需要上链、哪些数据适合由数据库管理。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->




## **Day4 | Monad 高频交互应用设计：Meme Arena（链上 Meme 对战排行榜）**

### **1\. Research（约 300 字）**

随着 Consumer Crypto 的发展，越来越多用户参与 Meme 创作、社区投票和社交互动。这类产品最大的特点不是价值转账，而是大量、高频、低价值的交互：点赞、投票、助力、PK、排行榜刷新、任务打卡等都可能每分钟发生数百甚至数千次。

如果底层区块链确认速度慢或 Gas 成本较高，用户需要等待交易确认才能看到自己的投票、积分或排名变化，容易打断互动节奏。当一次点赞或投票都需要支付较高手续费时，用户也会倾向减少操作，社区活跃度和传播效果都会下降。因此，这类产品需要一条能够支持低延迟、高吞吐、低成本交易的链。

Monad 更适合这一场景，不只是因为 TPS 更高，而是因为它能够让每一次互动都接近实时反馈。用户完成投票、领取任务奖励或刷新排行榜后，可以快速看到链上状态更新，形成连续互动体验。同时，Monad 与 EVM 兼容，开发者能够直接使用 Solidity、Hardhat、Foundry 等成熟工具迁移以太坊生态项目，降低开发成本。

相比普通数据库，排行榜本身可以放在数据库缓存，但积分、投票记录、奖励领取、NFT Badge 和赛季结果更适合记录在链上。这样既保证数据公开透明、防止篡改，也方便不同应用共享同一套链上身份和荣誉体系，让用户的社区贡献成为真正可验证、可组合的链上资产。

### **2\. Tech：高频交互 Demo 功能清单**

**MVP 功能**

**① Meme PK Arena**

-   两张 Meme 卡片随机对战
    
-   用户点击支持其中一个 Meme
    
-   每次投票立即写入链上
    

**② 实时排行榜**

-   根据票数实时更新 Top 100
    
-   支持按小时、每天、每周赛季排行
    

**③ Daily Quest**

每日完成：

-   投票 10 次
    
-   分享 1 次
    
-   评论 3 次
    

完成后链上领取奖励

**④ On-chain Badge**

例如：

-   Early Voter
    
-   Top Contributor
    
-   Meme Creator
    
-   Season Champion
    

Badge 全部 NFT 化。

**⑤ XP & Level System**

每次交互获得 XP：

-   Vote +1 XP
    
-   Share +5 XP
    
-   Create Meme +20 XP
    

升级后解锁头像框、称号等奖励。

**⑥ Creator Reward Pool**

赛季结束：

-   Top Meme Creator
    
-   Top Voter
    
-   Top Community
    

自动按照智能合约分配奖励。

### **3\. Ops：3 个适合 Meme / 社区传播的产品切入点**

**① Vote to Pump**

每天举行 Meme Battle。

用户不断投票，把自己支持的 Meme 冲上榜首。

社区自然形成拉票传播。

传播口号：

“Vote your Meme to the Moon.”

**② Raid Mission**

社区发布任务：

-   投票
    
-   转发 Twitter/X
    
-   邀请好友
    
-   加入 Discord
    

完成任务即可获得链上积分和 Badge。

形成持续裂变增长。

**③ Season Championship**

每周开启一个新赛季：

-   新排行榜
    
-   新 Badge
    
-   新奖励池
    

赛季结束后永久记录：

-   Season #1 Champion
    
-   Season #2 Champion
    

所有荣誉保存在链上，可长期展示，增强用户参与感和社区竞争氛围。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->






## **Day 3 —— 编写并部署第一个 Solidity 智能合约（OnchainTodo）**

工具：Remix IDE、MetaMask、Monad Testnet、Solidity

### **一、学习目标**

本次学习的目标：

了解 Solidity 智能合约的基本结构

编写一个简单的 Todo List 合约

学习智能合约的读写函数（Read / Write Function）

使用 Remix 编译并部署合约

在测试网上验证合约并进行交互

### **二、实践内容**

本次实现了一个简单的 **OnchainTodo** 合约。

主要功能包括：

添加待办事项（Add Todo）

修改完成状态（Toggle Completed）

查询待办事项（Get Todo）

获取 Todo 数量（Get Todo Count）

代码中使用了以下 Solidity 基础知识：

Contract

Struct

Dynamic Array

State Variable

Event

Require

View Function

### **三、核心知识**

**1\. Contract**

Solidity 使用 contract 定义智能合约。

contract OnchainTodo {

}

可以理解为 Java 中的 Class，只不过部署后运行在区块链上。

**2\. Struct**

使用 Struct 定义 Todo 数据结构。

struct Todo {

string task;

bool completed;

}

每个 Todo 包含：

-   task：任务内容
    
-   completed：完成状态
    

**3\. Dynamic Array**

Todo\[\] private todos;

动态数组用于保存所有 Todo。

调用 addTodo() 时，会向数组末尾新增一个 Todo。

**4\. Read Function**

getTodo()

getTodoCount()

读取链上数据。

特点：

-   不修改区块链
    
-   使用 view
    
-   一般不消耗 Gas（本地调用）
    

**5\. Write Function**

addTodo()

toggleCompleted()

修改链上数据。

特点：

-   会产生交易（Transaction）
    
-   需要用户签名
    
-   消耗 Gas
    

**6\. Event**

event TodoAdded(...);

event TodoUpdated(...);

事件用于记录链上日志。

部署后，前端可以监听 Event 获取状态变化。

### **四、实践过程**

**1\. 编写合约**

创建文件：

OnchainTodo.sol

使用 Solidity 0.8.20 编写 Todo 合约。

2\. 编译合约

在 Remix 中：

-   Solidity Compiler
    
-   Compiler Version：0.8.x
    
-   Compile OnchainTodo.sol
    

编译成功，没有 Error。

**3\. 部署合约**

进入：

Deploy & Run Transactions

连接 MetaMask。

网络：

Monad Testnet

点击：

Deploy

钱包确认交易后，合约成功部署。

**4\. 调用 Write Function**

添加 Todo：

addTodo("Finish Assignment")

交易成功。

随后调用：

toggleCompleted(0)

成功修改完成状态。

**5\. 调用 Read Function**

查询 Todo：

getTodo(0)

返回：

task = Finish Assignment

completed = true

查询数量：

getTodoCount()

返回：

1

### **五、部署结果**

部署网络：

Monad Testnet

合约名称：

OnchainTodo

编译器：

solc 0.8.34

源码验证：

Sourcify Verified

说明部署的源码与链上的 Bytecode 完全一致，可以在区块链浏览器查看和验证。

### **六、学习收获**

通过本次实践，我了解了 Solidity 智能合约开发的基本流程：

-   编写 Solidity 合约。
    
-   使用 Remix 编译。
    
-   部署到测试网络。
    
-   调用写函数修改链上数据。
    
-   调用读函数读取链上数据。
    
-   完成源码验证，确保链上部署的代码可公开验证。
    

同时，也进一步理解了智能合约与传统程序的区别：智能合约部署到区块链后，其状态由链维护，写操作需要发起交易并支付 Gas，而读操作通常可以直接查询，不会改变链上状态。这种模式保证了数据的公开性、透明性和不可篡改性。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->







### **前置准备｜进入 Web3 与链上世界**

**Summary**

1.  创建或准备一个**课程专用钱包**，不要使用主力钱包。
    
2.  添加 Monad Testnet 测试网络。
    
3.  获取测试网资产，俗称“取水”。
    
4.  打开 Monad Explorer 或相关区块浏览器，可通过账户地址查询个人交易详情。
    

**Specific operations**

1.  拓展插件Metamask
    

2.  创建钱包
    

Metamask创建钱包，并记住私钥

3.  添加 Monad Testnet 测试网络
    

![bfae7d7454727fd78127d9b2e2b26ea8.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783429910381-bfae7d7454727fd78127d9b2e2b26ea8.png)

4.  获取测试币，俗称“取水”
    

![878fab6dac3d34dcc22dd41ff2003d54.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783429969137-878fab6dac3d34dcc22dd41ff2003d54.png)

**ps. 24h内每天上限50Mon**

5.  用 Monad Explorer 查询你的课程钱包地址
    

![d9c3b1d5a43fd8684d7fe5f8595c0568.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783430245323-d9c3b1d5a43fd8684d7fe5f8595c0568.png)![b8c56d1f57c0850d75af789fddb35f50.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783430196336-b8c56d1f57c0850d75af789fddb35f50.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->








**一、Day1整体学习主线**

1. Web3入门实操分三步：搭建Mod测试网钱包、链上取水获Mon测试币、AI生成部署智能合约。

2. 智能合约是链上不可篡改自动执行程序，留言板写入类操作消耗Gas，查询类操作免费。

**二、公私钥核心区别**

1. 私钥：掌控资产唯一凭证，必须保密，泄露资产会被盗。

2. 公钥：钱包公开地址，可对外分享，用于收款、查链上数据。

**三、Gas费相关要点**

1. Gas为链上操作燃料费，计算方式：Gas消耗量×油价。

2. 操作越复杂、网络越拥堵，Gas费用越高。

3. 合约留言写入消耗Gas，读取留言无Gas成本。

4. 每次留言是一笔上链交易，必耗Gas；留言文字越多，单笔Gas越高。

5. 合约Gas消耗可在Remix查看，受代码写法、链网络情况影响。

**四、Mod测试网钱包操作流程**

1. 火狐浏览器安装MetaMask，创建钱包妥善保管私钥。

2. 钱包添加Mod Test网络，复制公钥去水龙头领测试币，每日限一次。

3. 区块浏览器查询余额，即为钱包任务完成校验标准。

**五、链上交易要点**

1. 互转测试币后留存交易哈希，所有链上记录永久留存、不可篡改。

2. 区块浏览器可查看交易收发地址、金额、Gas消耗全部信息。

**六、留言板智能合约部署流程**

1. GPT生成Solidity合约代码，Remix粘贴自动编译。

2. Remix连接MetaMask测试网钱包，支付Gas完成合约部署。

3. 写入留言生成交易哈希可溯源；查询留言仅读取链上数据不扣费。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
