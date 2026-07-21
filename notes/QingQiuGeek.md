---
timezone: UTC+8
---

# QingQiuGeek

**GitHub ID:** QingQiuGeek

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
Day 16 | Week 3 | 产品方向收敛 + 智能合约架构设计

今天干了啥

昨天组完队，今天确定产品方向。讨论了一上午，最后决定做一个 Monad 上的链上安全检测工具 —— 跟我 Web3 安全方向对口。

产品思路：用户输入一个合约地址，工具自动分析合约代码，返回安全评分和风险点。类似简化版的 Slither，但是链上版，直接在 Monad 上运行。

产品设计

核心功能：

合约代码获取 — 通过 MonadScan API 拉取合约源码

静态分析 — 检测常见漏洞模式（重入、整数溢出、权限问题）

安全评分 — 0-100 分，按风险等级分类

报告生成 — 结构化的安全报告，可导出

技术栈：

智能合约：Solidity（分析引擎的链上部分）

前端：React + viem + TailwindCSS

后端：Node.js（调用分析 API）

索引：Envio HyperIndex（链上数据查询）

智能合约架构

设计了一个简单的合约分析记录合约：

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.24;

contract SecurityScanner {

struct ScanResult {

address contractAddress;

address scanner;

uint256 score; // 0-100

uint256 timestamp;

string ipfsHash; // 完整报告存 IPFS

RiskLevel riskLevel;

}

enum RiskLevel { Safe, Low, Medium, High, Critical }

mapping(address => ScanResult\[\]) public scanHistory;

ScanResult\[\] public allScans;

event ScanCompleted(

address indexed contractAddress,

address indexed scanner,

uint256 score,

RiskLevel riskLevel,

string ipfsHash

);

function submitScan(

address contractAddress,

uint256 score,

RiskLevel riskLevel,

string calldata ipfsHash

) external {

require(score <= 100, "Invalid score");

require(bytes(ipfsHash).length > 0, "Empty hash");

ScanResult memory result = ScanResult({

contractAddress: contractAddress,

scanner: msg.sender,

score: score,

timestamp: block.timestamp,

ipfsHash: ipfsHash,

riskLevel: riskLevel

});

scanHistory\[contractAddress\].push(result);

allScans.push(result);

emit ScanCompleted(contractAddress, msg.sender, score, riskLevel, ipfsHash);

}

function getScanCount(address contractAddress) external view returns (uint256) {

return scanHistory\[contractAddress\].length;

}

function getLatestScan(address contractAddress) external view returns (ScanResult memory) {

require(scanHistory\[contractAddress\].length > 0, "No scans");

return scanHistory\[contractAddress\]\[scanHistory\[contractAddress\].length - 1\];

}

}

\`\`\`

设计要点：

`ScanResult` 结构体存储完整扫描结果

`ipfsHash` 存详细报告的 IPFS 哈希，链上只存摘要

`scanHistory` 按合约地址索引，可以查历史扫描

`ScanCompleted` 事件方便链下索引

Monad vs 以太坊：项目架构考量

维度 以太坊方案 Monad 方案

数据存储 IPFS + 链上哈希 相同（链上存更多元数据也行，Gas 便宜）

索引 The Graph Envio HyperIndex（Monad 原生支持）

前端交互 viem + ethers viem（原生支持 Monad）

部署成本 高（合约大小受限） 低（128 KB 限制，更灵活）

Monad 的 128 KB 合约大小限制对我们很有利 —— 分析引擎的链上部分可以写得更复杂，不用拆分合约。

安全思考

扫描结果的可信度：链上存储的扫描结果由 `msg.sender` 提交，不能保证准确性。解决方案：

引入多个扫描器，取共识

staking 机制，扫描器质押代币，结果被证伪则罚没

社区审核机制

IPFS 报告的完整性`ipfsHash` 存在链上，但 IPFS 内容可能被篡改（如果 pin 的节点被攻击）。建议：

报告内容加签名

用 IPNS 指向最新版本

关键数据直接存链上

合约分析的局限性：静态分析只能检测已知模式，无法发现逻辑漏洞。需要结合：

形式化验证（如 Certora）

模糊测试（如 Echidna）

人工审计

明天计划

实现合约分析引擎的核心逻辑

集成 MonadScan API 获取合约源码

写前端页面：合约地址输入 + 结果展示

部署到 Monad Testnet 测试
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->

Day 15 | Week 3 | 组队与角色分工 + Monad 生态探索

今天干了啥

Week 3 开始，从单人学习转入团队协作。今天的任务是了解 Monad 生态项目、确定团队分工方向。

前两周一直在写合约、部署、测试，都是单人作战。从今天开始要和其他学员组队，围绕一个产品方向协作。我选的是 Tech Track，负责智能合约和链上交互部分。

Monad 生态现状

花了半天时间梳理 Monad 生态：

基础设施层：

MonadBFT 共识 — 1 秒出块，10,000 TPS

EVM 完全兼容 — 现有 Solidity 合约直接部署

异步 I/O + 乐观并行执行 — 性能核心

DeFi 生态（已有/即将上线）：

Uniswap V3 — 已部署 Monad Testnet

PancakeSwap — 跨链部署中

Curvance — Monad 原生借贷协议

Kuru — Monad 原生订单簿 DEX

开发者工具：

Monad Foundry — 自定义 Foundry，匹配链上行为

Multicall3 — 批量调用（地址与以太坊相同）

Safe v1.4.1 — 多签钱包

EntryPoint v0.6/0.7/0.8 — ERC-4337 账户抽象

区块浏览器：

MonadVision ([monadvision.com](http://monadvision.com))

Monadscan ([monadscan.com](http://monadscan.com))

团队分工

初步分工：

角色 负责人 职责

智能合约 我 Solidity 开发、安全审计、Gas 优化

前端 DApp 待定 React + viem 连接合约

产品设计 待定 用户场景、交互设计

运营/研究 待定 社区运营、文档撰写

Monad vs 以太坊：生态层面的区别

维度 以太坊 Monad

出块时间 12 秒 1 秒

TPS 15-30 10,000

DeFi 生态 成熟，数千个协议 早期，快速增长中

开发工具 最完善 兼容以太坊工具 + 自定义版本

用户基础 最大 早期，需要引导

Gas 成本 高（L1） 低（但按 limit 收费）

Monad 的生态还在早期，这意味着：

竞争少，机会多

但用户也少，需要自己做增长

合约安全更重要（早期项目更容易被攻击）

安全思考

生态早期的安全风险：Monad 生态刚起步，很多协议的合约还没经过充分审计。作为安全方向的参与者，可以考虑：

给生态项目做安全审计

开发安全工具（如链上监控、异常检测）

写安全教育内容

并行执行的安全影响：乐观并行执行意味着交易的执行顺序可能和提交顺序不一致。这会影响：

MEV 的形态（不同）

三明治攻击的可行性（需要重新评估）

重入攻击的检测（并发冲突）

团队协作的安全要求：

私钥不能共享，用多签或角色分离

代码审查必须到位，不能一个人 push 直接上线

测试覆盖率要高，特别是边界条件

明天计划

确定团队产品方向

开始写产品需求文档

设计智能合约架构

与团队成员沟通技术方案
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->


一、今日学习内容

今天主要深入研究了 Monad 的 Gas 定价机制和以太坊的差异，以及如何在 Monad 上构建高性能 DApp 的最佳实践。作为一个从以太坊生态过来的开发者，Monad 在 Gas 模型上的改动让我非常意外——它不是简单的"便宜快速"，而是从根本上重新设计了资源计费的逻辑。

二、Monad vs 以太坊：Gas 机制深度对比

2.1 核心参数对比表

参数 以太坊 Monad 影响

Gas 计费基准 gas\_used（实际消耗） gas\_limit（设定上限） Monad 防止 DoS 攻击

区块 Gas 上限 ~30M gas 200M gas Monad 吞吐量提升 6.6 倍

单笔交易 Gas 上限 ~30M gas 30M gas 相同

最低 Base Fee 无硬性下限 100 MON-gwei Monad 有地板价保护

Base Fee 控制器 EIP-1559 线性调整 指数平滑控制器 Monad 上升慢、下降快

合约代码大小上限 24 KB 128 KB Monad 支持更复杂合约

Init Code 大小上限 48 KB 256 KB Monad 支持更大的构造函数

Blob 交易 (EIP-4844) 支持 不支持 Monad 用其他方案

交易排序 Gas Price 优先 Priority Gas Auction 类似但实现不同

Mempool 全局 mempool 无全局 mempool 本地转发给后续 leader

2.2 Gas 计费模型差异详解

以太坊的 Gas 计费非常直观：用户支付 `gas_used × price_per_gas`。未使用的 Gas 会退还。但 Monad 的设计完全不同：

\`\`\`

gas\_paid = gas\_limit × price\_per\_gas

\`\`\`

为什么 Monad 要按 gas\_limit 收费？

这是为了支持异步执行（asynchronous execution）。在 Monad 中，leader 构建区块、validator 投票验证区块有效性，这些都发生在实际执行之前。如果像以太坊一样按 `gas_used` 收费，恶意用户可以提交一个 `gas_limit` 很大但实际消耗极少 Gas 的交易。这种交易会占用大量区块空间却只支付很少的费用，形成 DoS 攻击向量。

实际影响：开发者必须精确设置 `gas_limit`，不能像在以太坊上那样随意设置一个很高的值。MetaMask 等钱包在 `eth_estimateGas` revert 时会自动设置很高的 gas limit，这在 Monad 上会导致用户支付远超预期的费用。

2.3 Base Fee 控制器的数学模型

Monad 的 base fee 控制器比以太坊更复杂，使用了指数平滑算法：

\`\`\`

base\_price\_per\_gas\_{k+1} = max{

min\_base\_price\_per\_gas,

base\_price\_per\_gas\_k × exp(η\_k × (block\_gas\_k - target) / block\_gas\_limit)

}

\`\`\`

关键参数：

`max_step_size = 1/28`

`target = 160M`（80% 满块）

`β = 0.96`（平滑因子）

`min_base_price_per_gas = 100 MON-gwei`

与以太坊的 EIP-1559 相比，Monad 的控制器上升更慢、下降更快。设计意图是避免 base fee 过高导致区块空间利用率不足。以太坊在高拥堵时 base fee 会快速飙升，而 Monad 的设计更温和，确保网络始终保持高利用率。

三、Monad 并行执行机制

Monad 最核心的技术创新是乐观并行执行（Optimistic Parallel Execution）。

以太坊是严格顺序执行的：交易 T1 完全执行完毕后才执行 T2。Monad 则会在 T1 还未完成时就开始执行 T2，假设它们不会冲突。如果确实发生了冲突（T2 读取了 T1 修改的状态），Monad 会重新执行 T2。

关键点：

执行结果与以太坊完全一致 — 最终状态相同，只是执行效率更高

状态合并是顺序的 — 虽然执行并行，但状态更新按顺序合并

重复执行优化 — 不依赖状态的工作（如签名验证）不需要重复执行

这意味着开发者不需要为并行执行做任何特殊适配，现有的 Solidity 合约直接兼容。但如果你想充分利用 Monad 的性能，需要注意减少合约间的状态依赖。

四、高并发合约交互最佳实践

4.1 硬编码 Gas Limit

\`\`\`solidity

// 不推荐：每次调用 eth\_estimateGas

const tx = await contract.transfer(recipient, amount);

// 推荐：直接设置已知的 gas limit

const tx = await contract.transfer(recipient, amount, {

gasLimit: 21000 // 原生转账固定消耗

});

\`\`\`

对于 ERC-20 transfer，可以预估一个固定值（通常 65,000 gas），避免钱包反复调用 `eth_estimateGas` 造成的延迟。

4.2 使用 Multicall3 合并读取

Monad 上部署了标准的 Multicall3 合约（地址`0xcA11bde05977b3631167028862bE2a173976CA11`），可以将多个 `eth_call` 合并为一次请求：

\`\`\`typescript

import { createPublicClient, http } from 'viem';

import { monad } from 'viem/chains';

const client = createPublicClient({

chain: monad,

transport: http('[https://rpc.monad.xyz](https://rpc.monad.xyz)')

});

// 批量读取多个合约状态

const results = await client.multicall({

contracts: \[

{ address: tokenA, abi: erc20Abi, functionName: 'balanceOf', args: \[wallet\] },

{ address: tokenB, abi: erc20Abi, functionName: 'balanceOf', args: \[wallet\] },

{ address: dex, abi: dexAbi, functionName: 'getReserves' },

\]

});

\`\`\`

注意：Multicall3 内部是顺序执行的，所以不要在一个 Multicall 中塞入太多昂贵的调用。对于大量数据查询，应该使用索引器（Allium、Envio、The Graph）。

4.3 并发提交交易

\`\`\`typescript

// 并发发送多笔交易（注意 nonce 管理）

const nonce = await client.getTransactionCount({ address: wallet });

const txPromises = Array(10).fill(null).map((\_, i) =>

walletClient.sendTransaction({

to: recipient,

value: parseEther('0.1'),

gasLimit: 21000n,

nonce: nonce + i,

})

);

const hashes = await Promise.all(txPromises);

\`\`\`

4.4 使用索引器替代轮询

不要用 `eth_getLogs` 轮询事件，改用：

Allium — SQL 查询历史数据

Envio HyperIndex — 实时事件索引

The Graph — 去中心化索引协议

Envio 配置示例（Monad Testnet Chain ID: 10143）：

\`\`\`yaml

name: my-monad-indexer

networks:

\- id: 10143 # Monad Testnet

start\_block: 0

contracts:

\- name: MyContract

address: "0x..."

handler: src/EventHandlers.ts

events:

\- event: Transfer(address from, address to, uint256 amount)

\`\`\`

五、安全分析：Monad 特有的风险点

5.1 Gas Limit 陷阱

风险：在以太坊上，开发者习惯设置较高的 gas limit 作为安全余量。在 Monad 上，这会导致实际支付远超预期。如果一笔交易 gas limit 设为 30M 但实际只用了 100K，用户仍然要为 30M gas 付费。

缓解措施：

前端代码必须精确估算 gas，不能"宁多勿少"

对于已知 gas 消耗的操作，硬编码 gas limit

在 UI 上明确显示最大可能费用

5.2 Reserve Balance 机制

Monad 的 Reserve Balance 机制要求 EIP-7702 委托的账户余额不能低于 10 MON。这意味着：

如果用户账户余额接近 10 MON，某些交易可能在共识阶段被包含但执行时 revert

这些 revert 的交易仍然会消耗 gas

开发者需要在 UI 中提醒用户保持最低余额

5.3 无全局 Mempool 的影响

Monad 没有全局 mempool，交易只转发给后续几个 leader。这意味着：

传统的 MEV 搜索策略可能不适用

交易传播的可见性降低

前端不能依赖 mempool 监控来预测交易状态

5.4 合约代码大小放宽的安全隐患

Monad 将合约代码上限从 24KB 提升到 128KB。虽然这允许更复杂的合约，但也意味着：

更大的攻击面

审计成本增加

需要更严格的模块化设计

六、Monad 网络配置信息

测试网

RPC URL: `https://testnet-rpc.monad.xyz`

Chain ID: 10143

货币符号: MON

区块浏览器: `https://testnet.monadexplorer.com`

水龙头: `https://faucet.monad.xyz`

主网

Chain ID: 143

Multicall3: `0xcA11bde05977b3631167028862bE2a173976CA11`

七、个人思考

今天的学习让我意识到，Monad 不只是"更快的以太坊"。它的 Gas 模型设计是一个根本性的架构变化——从"按使用量付费"到"按承诺量付费"。这种设计牺牲了一定的用户体验（需要精确估算 gas），但换来了异步执行的安全保障。

最让我印象深刻的是 Base Fee 控制器的设计。以太坊的 EIP-1559 在高拥堵时 base fee 飙升很快，导致用户在牛市时 gas fee 高到离谱。Monad 的控制器用指数平滑让上升更温和、下降更快速，这是一个更优雅的工程权衡。

对于前端开发者来说，最大的改变是必须放弃"设置高 gas limit 保平安"的思维惯式。在 Monad 上，精确估算 gas 不是优化，而是必须。这也意味着我们需要在 DApp 中加入更好的 gas 估算逻辑和用户提示。

另外，Monad 不支持 EIP-4844 blob 交易，这对于需要大量数据可用性的 L2 方案可能有影响。但考虑到 Monad 本身就是 L1，这个设计选择是合理的。

八、明日计划

研究 Monad 的 EIP-7702 账户委托机制

实操：在 Monad 测试网部署一个带 Multicall 的合约

探索 Monad 上的 DeFi 协议集成

\---

参考资料:

Monad 官方文档 - Gas Pricing

Monad 官方文档 - Differences

Monad 官方文档 - Best Practices

Monad 官方文档 - Parallel Execution

以太坊官方文档 - Gas
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->



\# Day 12 | Week 2 | 优化与部署：Gas 优化 + Pausable 合约 + Monad Multicall3

\## 今天干了啥

昨天搭好了 Hardhat 环境，今天做三件事：给合约加 Pausable（可暂停）、优化 Gas、学 Monad 上的 Multicall3 批量调用。

\## Pausable 合约

在昨天的 MonadToken 基础上加紧急暂停功能：

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.24;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

import "@openzeppelin/contracts/access/Ownable.sol";

import "@openzeppelin/contracts/utils/Pausable.sol";

contract MonadToken is ERC20, Ownable, Pausable {

uint256 public constant MAX\_SUPPLY = 1\_000\_000 _10_\*18;

uint256 public constant MAX\_MINT\_PER\_TX = 10\_000 _10_\*18;

mapping(address => uint256) public mintedAmount;

constructor() ERC20("MonadToken", "MONAD") Ownable(msg.sender) {

_mint(msg.sender, 100_000 _10_\*18);

}

function mint(uint256 amount) external whenNotPaused {

require(amount > 0 && amount <= MAX\_MINT\_PER\_TX, "Invalid amount");

require(totalSupply() + amount <= MAX\_SUPPLY, "Exceeds max supply");

mintedAmount\[msg.sender\] += amount;

\_mint(msg.sender, amount);

}

function transfer(address to, uint256 amount) public override whenNotPaused returns (bool) {

return super.transfer(to, amount);

}

function transferFrom(address from, address to, uint256 amount) public override whenNotPaused returns (bool) {

return super.transferFrom(from, to, amount);

}

function pause() external onlyOwner {

\_pause();

}

function unpause() external onlyOwner {

\_unpause();

}

}

\`\`\`

关键点：

\- `whenNotPaused` 修饰符加在 `minttransfertransferFrom` 上

\- 只有 owner 能调 `pause()` / `unpause()`

\- 紧急情况（发现漏洞、攻击）可以暂停所有操作

\- 生产环境建议用 `AccessControl` 给 PAUSER\_ROLE，而不是只用 Ownable

\## Gas 优化技巧

在 Monad 上，Gas 按 limit 收费，优化比以太坊更重要。

**1\. 用 custom error 替代 require string**

\`\`\`solidity

// 浪费 Gas（存储错误字符串）

require(amount > 0, "Amount must be positive");

// 省 Gas（自定义错误）

error InvalidAmount();

if (amount == 0) revert InvalidAmount();

\`\`\`

实测差异：一个 require string 平均多花 200-500 gas。如果合约有 10 个 require，累积下来不少。

**2\. 用 immutable 存储不变量**

\`\`\`solidity

// 每次读取都要从 storage 读（贵）

address public owner;

// 部署时写入，之后从 bytecode 读（便宜）

address public immutable owner;

\`\`\`

**3\. 批量操作代替多次调用**

\`\`\`solidity

// 差：3 次交易 = 3 × 21000 基础 gas

token.transfer(alice, 100);

token.transfer(bob, 200);

token.transfer(charlie, 300);

// 好：1 次交易

function batchTransfer(address\[\] calldata recipients, uint256\[\] calldata amounts) external {

require(recipients.length == amounts.length, "Length mismatch");

for (uint256 i = 0; i < recipients.length; i++) {

super.transfer(recipients\[i\], amounts\[i\]);

}

}

\`\`\`

**4\. Monad 特有的 Gas 注意事项**

| 操作 | 以太坊建议 | Monad 建议 |

|------|-----------|-----------|

| 固定操作（转账） | 让钱包自动估 gas | **硬编码 21000** |

| 合约调用 | eth\_estimateGas | eth\_estimateGas + **手动减小** |

| 部署合约 | 设高 gas\_limit 无妨 | **精确设置**，多了不退 |

\## Multicall3 在 Monad 上的用法

Multicall3 是一个标准合约，可以把多个 `eth_call` 合并成一次调用。在以太坊和 Monad 上地址相同`0xcA11bde05977b3631167028862bE2a173976CA11`

\`\`\`javascript

import { createPublicClient, http, encodeFunctionData } from 'viem';

import { monadTestnet } from 'viem/chains';

const client = createPublicClient({

chain: monadTestnet,

transport: http("[https://testnet-rpc.monad.xyz](https://testnet-rpc.monad.xyz)")

});

// 批量读取多个代币余额

const calls = [tokenAddresses.map](http://tokenAddresses.map)(addr => ({

target: addr,

allowFailure: true,

callData: encodeFunctionData({

abi: erc20Abi,

functionName: 'balanceOf',

args: \[userAddress\]

})

}));

const results = await client.readContract({

address: '0xcA11bde05977b3631167028862bE2a173976CA11',

abi: multicall3Abi,

functionName: 'aggregate3',

args: \[calls\]

});

\`\`\`

Monad 上 Multicall3 的特殊点：

\- 地址与以太坊完全相同`0xcA11bde...`）

\- Monad Testnet 和 Mainnet 都已部署

\- Multicall3 内部是\*\*串行执行\*\*的，避免把太多昂贵调用塞进一个 multicall

\- 如果需要并行，用 RPC batch 请求（viem 的 `Promise.all()` + 自动 batch）

\## 测试补充

今天补了昨天遗漏的边界测试：

\`\`\`javascript

it("刚好到总量上限应成功", async function () {

// 初始 10 万，再铸 90 万刚好到 100 万上限

await token.connect(addr1).mint(ethers.parseEther("10000"));

// 重复 90 次

for (let i = 0; i < 89; i++) {

await token.connect(addr1).mint(ethers.parseEther("10000"));

}

expect(await token.totalSupply()).to.equal(ethers.parseEther("1000000"));

});

it("超出总量 1 wei 应 revert", async function () {

// 铸到只剩 9999，再铸 10001 就超了

for (let i = 0; i < 99; i++) {

await token.connect(addr1).mint(ethers.parseEther("10000"));

}

await expect(token.connect(addr1).mint(ethers.parseEther("1")))

.[to.be](http://to.be).revertedWith("Exceeds max supply");

});

\`\`\`

边界测试很无聊但很重要。差 1 wei 的 revert 行为必须覆盖到。

\## 踩坑记录

1\. **Pausable 的 transfer 覆写**：OpenZeppelin 的 ERC20.transfer 不是 virtual 的，必须显式覆写并加 `whenNotPaused`。一开始忘了，pause 了但 transfer 还能用

2\. **Multicall3 的串行执行**：以为 Multicall3 是并行的，实际上代码里是 for 循环串行。大量调用还是会慢，改用 RPC batch

3\. **immutable 不能在 constructor 里修改**：设成 immutable 后就不能改了，适合 owner 这种部署后不变的量

\## 安全思考

1\. **Pausable 的中心化风险**：owner 可以随时暂停所有交易。如果 owner 私钥泄露，攻击者可以冻结整个合约。建议：

\- 用 Timelock + 多签控制 pause 权限

\- 设置 pause 的最长持续时间

\- 事件监控：监听 Paused/Unpaused 事件

2\. **batchTransfer 的重入风险**：如果 recipients 是合约地址`super.transfer` 会调用 `transfer` 函数，可能触发重入。当前实现用 `super.transfer` 是安全的（ERC-20 没有回调），但如果以后改成带 ETH 转账的版本就必须加 ReentrancyGuard

3\. **Multicall3 的信任假设**：Multicall3 是不可变合约，安全。但如果项目自己部署 Multicall 合约，要确保不可升级，防止被替换

\## 明天计划

1\. 写前端用 viem 连接 Monad Testnet 上的合约

2\. 实现钱包连接 + 余额显示 + 铸造功能

3\. 学习 Monad 的历史数据限制和索引器方案

4\. 整理 Week 2 Build Log
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->




Day 11 | Week 2 | 测试与调试：Hardhat + Monad Testnet 实战

Hardhat 环境搭建

\`\`\`bash

mkdir monad-lab && cd monad-lab

npm init -y

npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox @openzeppelin/contracts

npx hardhat init

\`\`\`

选 "Create a JavaScript project"。目录结构：

\`\`\`

monad-lab/

├── contracts/ # Solidity 合约

├── scripts/ # 部署脚本

├── test/ # 测试

├── hardhat.config.js # 配置

├── .env # 私钥（不要提交到 Git）

└── .gitignore

\`\`\`

hardhat.config.js：

\`\`\`javascript

require("@nomicfoundation/hardhat-toolbox");

require("dotenv").config();

module.exports = {

solidity: "0.8.24",

networks: {

monad\_testnet: {

url: "[https://testnet-rpc.monad.xyz](https://testnet-rpc.monad.xyz)",

chainId: 10143,

accounts: \[process.env.PRIVATE\_KEY\]

}

}

};

\`\`\`

.env（不要提交）：

\`\`\`

PRIVATE\_KEY=你的私钥

\`\`\`

第一次提交差点把 .env 推上去。加到 .gitignore 是第一件事。

ERC-20 合约：MonadToken

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.24;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

import "@openzeppelin/contracts/access/Ownable.sol";

contract MonadToken is ERC20, Ownable {

uint256 public constant MAX\_SUPPLY = 1\_000\_000 _10_\*18;

uint256 public constant MAX\_MINT\_PER\_TX = 10\_000 _10_\*18;

mapping(address => uint256) public mintedAmount;

constructor() ERC20("MonadToken", "MONAD") Ownable(msg.sender) {

_mint(msg.sender, 100_000 _10_\*18);

}

function mint(uint256 amount) external {

require(amount > 0 && amount <= MAX\_MINT\_PER\_TX, "Invalid amount");

require(totalSupply() + amount <= MAX\_SUPPLY, "Exceeds max supply");

mintedAmount\[msg.sender\] += amount;

\_mint(msg.sender, amount);

}

}

\`\`\`

设计要点：

`MAX_SUPPLY` 硬顶 100 万，防无限增发

`MAX_MINT_PER_TX` 单次最多铸 1 万

`mintedAmount` 记录每个地址铸造量，方便后续做限额

`Ownable` 限制 owner 权限（生产环境建议换 `AccessControl` 多角色）

测试

\`\`\`javascript

const { expect } = require("chai");

const { ethers } = require("hardhat");

describe("MonadToken", function () {

let token, owner, addr1;

beforeEach(async function () {

\[owner, addr1\] = await ethers.getSigners();

const Token = await ethers.getContractFactory("MonadToken");

token = await Token.deploy();

});

it("初始供应量 10 万", async function () {

expect(await token.totalSupply()).to.equal(ethers.parseEther("100000"));

});

it("正常铸造", async function () {

await token.connect(addr1).mint(ethers.parseEther("1000"));

expect(await token.balanceOf(addr1.address)).to.equal(ethers.parseEther("1000"));

});

it("超过单次限额应 revert", async function () {

await expect(token.connect(addr1).mint(ethers.parseEther("10001")))

.[to.be](http://to.be).revertedWith("Invalid amount");

});

it("超过总量上限应 revert", async function () {

await expect(token.connect(addr1).mint(ethers.parseEther("900001")))

.[to.be](http://to.be).revertedWith("Exceeds max supply");

});

});

\`\`\`

\`\`\`

npx hardhat test

MonadToken

✓ 初始供应量 10 万

✓ 正常铸造

✓ 超过单次限额应 revert

✓ 超过总量上限应 revert

4 passing

\`\`\`

Monad vs 以太坊：测试层面

维度 以太坊 Hardhat Monad

本地网络 `npx hardhat node` Monad Foundry（自定义版本）

区块时间 按需出块 模拟 1 秒出块

Gas 行为 按 gas\_used 按 gas\_limit

测试代码 完全兼容 完全兼容

关键区别： Monad 提供了自定义 Foundry 工具链确保本地行为与链上一致。普通 Hardhat 能用，但 Gas 行为可能有差异。

部署到 Monad Testnet

\`\`\`bash

npx hardhat run scripts/deploy.js --network monad\_testnet

\`\`\`

输出：

\`\`\`

MonadToken deployed to: 0x...

\`\`\`

去 MonadVision 搜索合约地址验证。

踩坑记录

`waitForDeployment()` vs `deployed()`：Hardhat 新版用前者，旧教程用后者已废弃

Gas 费用：部署 ERC-20 花了约 0.05 MON，比以太坊便宜但比预估高（OpenZeppelin 合约代码量大）

测试覆盖率：写测试比写合约花的时间多 3 倍，但安全相关的测试不能省

`.env` 泄露：差点推到 Git，第一件事加 .gitignore

安全思考

`Ownable` 单点故障：生产环境用 `AccessControl` 或多签

`mint` 权限：当前任何人都能铸造，生产环境应限制

重入攻击：ERC-20 标准函数没有外部调用，安全。但扩展 `receive()` 或与其他合约交互时必须加 ReentrancyGuard

测试盲区：没测 `MAX_SUPPLY` 边界情况，明天补上

明天计划

合约升级：加 Pausable（可暂停）

学习 Monad Multicall3 批量调用

用 viem 写前端连接合约

整理 Week 2 Build Log
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->





Monad Builder Camp Day 9 — 智能合约进阶与 Monad 开发最佳实践

📅 学习日期：2026-07-14（课程第 9 天）

🎯 今日主题：智能合约进阶 — Monad 与 Ethereum 的核心差异

\---

一、Monad 与以太坊的关键技术差异

今天深入研读了 Monad 官方文档中关于 Monad 与以太坊差异的部分，收获非常大。以下是我总结的核心差异点：

1\. 虚拟机层面的差异

Monad 在 EVM 层面做了几个关键调整：

合约代码大小限制：Monad 将最大合约代码大小从以太坊的 24KB 提升到了 128KB，对应的 init code 大小限制也从 48KB 提升到 256KB。这对开发者来说是巨大的利好——在以太坊上，我们经常需要使用 proxy pattern、diamond pattern 或者将合约拆分成多个 library 来规避代码大小限制，而在 Monad 上，大型合约可以直接部署，不需要复杂的拆分策略。

Opcode 重新定价：由于 Monad 的优化架构（并行执行、异步 I/O），某些 opcode 和 precompile 的 gas cost 被重新调整，以反映 Monad 中资源的相对稀缺性。这意味着在以太坊上优化 gas 的策略在 Monad 上可能不完全适用，需要根据 Monad 的 opcode pricing 重新评估。

新增 secp256r1 (P256) precompile：地址 `0x0100` 支持 EIP-7951，允许在链上验证 WebAuthn/passkey 签名。这为账户抽象和生物识别认证打开了新的可能性。

2\. 交易层面的重大差异

这是最值得关注的部分：

Gas 按 gas\_limit 而非 gas\_used 收费：在以太坊上，用户只支付实际消耗的 gas，多余的会退还。但在 Monad 上，由于异步执行的需要，gas 费用是按 `gas_bid * gas_limit` 从发送者余额中扣除的。这是一个 DoS 防护机制——在异步执行环境中，无法在交易被纳入区块前确定实际 gas 使用量。

Reserve Balance 机制：Monad 引入了储备金机制来确保所有被纳入共识的交易都能被支付。这意味着你可能会看到一些最终执行失败的交易仍然出现在区块链上（它们仍然支付了 gas 费用）。

不支持 EIP-4844 (Blob 交易)：Type 3 交易在 Monad 中不可用。

无全局 Mempool：与以太坊不同，Monad 没有全局内存池。交易被直接转发给接下来的几个 leader，这提高了效率但改变了 MEV 的游戏规则。

3\. Gas 定价机制对比

特性 Ethereum Monad

Gas 收费基础 gas\_used（实际消耗） gas\_limit（设定上限）

价格模型 EIP-1559: base\_fee + priority\_fee EIP-1559 兼容，但 base fee 控制器更慢增长

最低 base fee 无固定下限 100 MON-gwei

Block gas limit ~30M 200M

单交易 gas limit ~30M 30M

交易排序 Priority Gas Auction Priority Gas Auction（默认）

Gas 退款 有（如存储清除） 需确认具体政策

Monad 的 block gas limit 达到了 200M，是以太坊的约 6.7 倍，这意味着每个区块能处理的交易量大幅增加。配合 1 秒的出块时间，Monad 的理论 TPS 远超以太坊。

\---

二、Monad 高性能应用开发最佳实践

从 Monad 官方 best practices 文档中，我学到了几条关键的性能优化原则：

1\. 硬编码 Gas Limit

\`\`\`solidity

// ❌ 不推荐：每次交易都调用 eth\_estimateGas

const gasEstimate = await provider.estimateGas(tx);

// ✅ 推荐：对于固定操作直接硬编码

const gasLimit = 21000; // 原生代币转账固定消耗

// 或者对于合约调用，使用已知的固定值

const gasLimit = 100000; // 已知的合约交互消耗

\`\`\`

这个优化看似简单，但在高频场景下效果显著：每次省去一次 RPC 调用，减少了网络延迟，也避免了某些钱包在 `eth_estimateGas` revert 时的异常行为。

2\. 并发提交 eth\_call

在 Monad 上，并发调用比串行调用效率高得多：

\`\`\`javascript

// ❌ 串行调用 — 每次等待上一次完成

const balance1 = await [provider.call](http://provider.call)(tx1);

const balance2 = await [provider.call](http://provider.call)(tx2);

const balance3 = await [provider.call](http://provider.call)(tx3);

// ✅ 并发调用 — 同时发起所有请求

const \[balance1, balance2, balance3\] = await Promise.all(\[

[provider.call](http://provider.call)(tx1),

[provider.call](http://provider.call)(tx2),

[provider.call](http://provider.call)(tx3),

\]);

\`\`\`

3\. 使用 Multicall3 合约

Monad 主网上部署了标准的 `Multicall3` 合约，可以将多个读取请求合并为单次调用：

\`\`\`solidity

// Multicall3 接口

interface IMulticall3 {

struct Call3 {

address target;

bool allowFailure;

bytes calldata;

}

function aggregate3(Call3\[\] calldata calls) external returns (Result\[\] memory);

}

\`\`\`

4\. 管理 Nonce

在高并发场景下，本地管理 nonce 而不是依赖 `eth_getTransactionCount`：

\`\`\`javascript

let nonce = await provider.getTransactionCount(address);

async function sendTx(txData) {

const tx = {

...txData,

nonce: nonce++,

gasLimit: 100000, // 硬编码 gas limit

};

return await wallet.sendTransaction(tx);

}

\`\`\`

5\. 并发提交多笔交易

\`\`\`javascript

// 同时提交多笔交易，每笔使用递增的 nonce

const txs = [transactions.map](http://transactions.map)((tx, i) => ({

...tx,

nonce: baseNonce + i,

gasLimit: 100000,

}));

const receipts = await Promise.all(

[txs.map](http://txs.map)(tx => wallet.sendTransaction(tx))

);

\`\`\`

\---

三、Web3 安全与合规深度思考

今天还研读了 Web3 实习手册中的安全与合规部分，这对在 Monad 上开发有直接的指导意义。

1\. 智能合约安全的核心原则

在 Monad 上开发智能合约，安全意识不能因为性能提升而降低。以下是关键的安全检查清单：

重入攻击防护：

\`\`\`solidity

// 使用 ReentrancyGuard

import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

contract MyContract is ReentrancyGuard {

function withdraw() external nonReentrant {

uint256 balance = balances\[msg.sender\];

balances\[msg.sender\] = 0;

(bool success, ) = [msg.sender.call](http://msg.sender.call){value: balance}("");

require(success, "Transfer failed");

}

}

\`\`\`

整数溢出：Solidity 0.8+ 已内置溢出检查，但在 unchecked 块中仍需注意。

访问控制：

\`\`\`solidity

import "@openzeppelin/contracts/access/AccessControl.sol";

contract MyContract is AccessControl {

bytes32 public constant ADMIN\_ROLE = keccak256("ADMIN\_ROLE");

constructor() {

_grantRole(ADMIN_ROLE, msg.sender);

}

function sensitiveAction() external onlyRole(ADMIN\_ROLE) {

// 只有管理员可以执行

}

}

\`\`\`

2\. 合规性考量

作为开发者，在 Monad 上部署合约时需要考虑：

代币设计：避免设计具有证券属性的代币模型，避免涉及 ICO/IDO 等融资行为

KYC/AML：如果是面向用户的 DApp，需要考虑是否集成 KYC 检查

赌博机制：任何涉及随机收益和提现的设计都需要极其谨慎

多级返佣：避免设计层级返利机制，这可能触犯传销相关法规

3\. Monad 特有的安全考量

由于 Monad 的异步执行模型，有几个特殊的安全点：

Reserve Balance 预期：交易可能被纳入区块但最终执行失败，合约逻辑不应假设交易一定成功执行

并发执行：Monad 的乐观并行执行可能在某些时序敏感的合约中引入新的攻击面

Gas 按 limit 收费：这意味着即使交易 revert，用户也会被收取 gas 费用，合约设计应尽量减少不必要的 revert

\---

四、实操：部署一个优化过的 Monad 合约

基于今天学到的知识，我设计了一个针对 Monad 优化的合约框架：

\`\`\`solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

import "@openzeppelin/contracts/access/AccessControl.sol";

import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

import "@openzeppelin/contracts/utils/Pausable.sol";

/\*\*

\* @title MonadOptimizedVault

\* @notice 针对 Monad 优化的金库合约

\* @dev 利用 Monad 的大代码限制，不需要 proxy pattern

\*/

contract MonadOptimizedVault is AccessControl, ReentrancyGuard, Pausable {

bytes32 public constant OPERATOR\_ROLE = keccak256("OPERATOR\_ROLE");

mapping(address => uint256) public deposits;

mapping(address => uint256) public lastDepositBlock;

uint256 public constant MIN\_DEPOSIT = 0.01 ether;

uint256 public constant WITHDRAWAL\_DELAY = 1; // Monad 出块快，可以缩短

event Deposited(address indexed user, uint256 amount);

event Withdrawn(address indexed user, uint256 amount);

constructor() {

_grantRole(DEFAULT_ADMIN\_ROLE, msg.sender);

_grantRole(OPERATOR_ROLE, msg.sender);

}

function deposit() external payable nonReentrant whenNotPaused {

require(msg.value >= MIN\_DEPOSIT, "Below minimum");

deposits\[msg.sender\] += msg.value;

lastDepositBlock\[msg.sender\] = block.number;

emit Deposited(msg.sender, msg.value);

}

function withdraw(uint256 amount) external nonReentrant whenNotPaused {

require(deposits\[msg.sender\] >= amount, "Insufficient balance");

// Monad 出块快，可以使用更短的延迟

require(

block.number >= lastDepositBlock\[msg.sender\] + WITHDRAWAL\_DELAY,

"Too early"

);

deposits\[msg.sender\] -= amount;

(bool success, ) = [msg.sender.call](http://msg.sender.call){value: amount}("");

require(success, "Transfer failed");

emit Withdrawn(msg.sender, amount);

}

// 利用 Monad 的大代码限制，可以直接内联复杂逻辑

// 而不是通过 proxy 调用外部 library

function getAccountInfo(address user) external view returns (

uint256 balance,

uint256 lastBlock,

bool canWithdraw,

uint256 maxWithdraw

) {

balance = deposits\[user\];

lastBlock = lastDepositBlock\[user\];

canWithdraw = block.number >= lastBlock + WITHDRAWAL\_DELAY;

maxWithdraw = balance;

}

}

\`\`\`

\---

五、今日总结与思考

今天的学习让我对 Monad 的技术架构有了更深入的理解。几个关键收获：

Monad 的 EVM 兼容性是真正的"高兼容"——它不是另起炉灶，而是在保持 EVM 兼容的前提下做了底层优化（并行执行、异步 I/O、共识优化），开发者迁移成本很低。

Gas 按 limit 收费是一个重要的心理转变——在以太坊上我们习惯了"用多少付多少"，但在 Monad 上需要更精确地估算 gas limit，否则会多付费用。

无全局 mempool 和本地 mempool 设计改变了 MEV 的博弈格局——交易不会在公开内存池中等待，减少了 sandwich attack 的窗口。

安全不能因为性能提升而松懈——反而，Monad 的高吞吐量意味着攻击者可以在更短时间内发起更多攻击尝试，安全审计的重要性不降反升。

Web3 合规是一个永恒的话题——无论在哪个链上开发，都需要遵守当地法律法规，避免触碰红线。

明天计划继续深入 Monad 的前端交互部分，探索如何利用 Monad 的高性能特性来构建流畅的 DApp 用户体验。

\---

参考资料：

Monad 官方文档 - Differences

Monad 官方文档 - Best Practices

Monad 官方文档 - Gas Pricing

Ethereum Gas 文档

Web3 实习手册 - 安全与合规
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->






Monad Builder Camp Day 8 — 智能合约进阶：深入理解 Monad 的 Gas 机制与高性能开发实践

一、今日学习内容

今天是 Monad Builder Camp 的第八天，正式进入 Week 2。今天的核心主题是智能合约进阶，我重点深入研究了 Monad 与以太坊在 Gas 机制、交易处理、合约部署等关键维度的差异，并结合 Monad 官方文档和 Web3 安全实践手册，梳理了在 Monad 上进行高性能 DApp 开发的核心要点。

\---

二、Monad vs 以太坊：Gas 机制深度对比

2.1 Gas 计费方式的根本性差异

这是 Monad 与以太坊最核心的区别之一，也是开发者最容易踩坑的地方。

维度 以太坊 Monad

Gas 收费基准 按 `gas_used`（实际消耗）收费 按 `gas_limit`（设定上限）收费

未使用 Gas 退还 ✅ 退还 `gas_limit - gas_used` 的部分 ❌ 不退还，全额按 `gas_limit` 扣除

Block Gas Limit ~30M gas 200M gas（6.7x）

Transaction Gas Limit 无硬性上限（受 block limit 约束） 30M gas

最低 Base Fee 动态，可低至 1 wei 100 MON-gwei `100 × 10⁻⁹ MON`)

Base Fee 调整机制 最大 ±12.5%/block 更慢增长、更快下降的指数控制器

区块出块时间 ~12s 400ms

最终确认时间 ~12min（64 slots） 800ms

吞吐量 ~15-30 tps 10,000 tps

合约代码大小上限 24 KB 128 KB

Init Code 大小上限 48 KB 256 KB

Blob 交易 (EIP-4844) ✅ 支持 ❌ 不支持

全局 Mempool ✅ 有 ❌ 无（Local Mempool，仅转发给后续几个 leader）

为什么 Monad 按 gas\_limit 收费？

这是 Monad 实现异步执行（Asynchronous Execution）的关键设计。在异步执行模型下，leader 构建区块和 validator 投票发生在交易执行之前。如果按 `gas_used` 收费，恶意用户可以提交一个 `gas_limit` 极高但实际消耗极少 gas 的交易，占据大量区块空间却不支付相应费用，形成 DoS 攻击向量。按 `gas_limit` 收费从根本上消除了这个问题。

2.2 Base Fee 控制器设计差异

以太坊的 Base Fee 控制器：

\`\`\`

base\_fee\_change = ±12.5% per block

target\_block\_size = gas\_limit / 2

\`\`\`

Monad 的 Base Fee 控制器采用了一种更精细的指数平滑算法：

\`\`\`

base\_price\_per\_gas(k+1) = max{min\_base\_price\_per\_gas, base\_price\_per\_gas(k) · exp(η(k) · (block\_gas(k) - target) / block\_gas\_limit)}

\`\`\`

关键参数：

`max_step_size = 1/28`

`target = 160M`（80% of block\_gas\_limit）

`β = 0.96`

`min_base_price_per_gas = 100 MON-gwei`

这意味着 Monad 的 Base Fee 增长更慢、下降更快，避免了因 base fee 过高导致区块空间利用率不足的问题。对于高频交易场景（如 Monad 上 400ms 出块），这种设计能更好地平衡供需。

2.3 EIP-1559 兼容性

Monad 完全兼容 EIP-1559 的 Type 2 交易：

\`\`\`

price\_per\_gas = min(base\_price\_per\_gas + priority\_price\_per\_gas, max\_price\_per\_gas)

\`\`\`

开发者在构造交易时仍使用熟悉的 `maxFeePerGas` 和 `maxPriorityFeePerGas` 参数，但需要特别注意`base_price_per_gas` 的最低值是 100 MON-gwei，这比以太坊的 base fee 高得多，但由于 MON 代币价格与 ETH 不同，实际成本需要综合考虑。

\---

三、Reserve Balance 机制：Monad 独有的交易保障

这是 Monad 文档中一个非常重要的概念，也是与以太坊的关键差异。

3.1 机制原理

Monad 使用 Reserve Balance 机制确保共识阶段纳入的所有交易都能被支付。该机制对交易纳入设置了轻量级约束，并定义了交易在执行阶段会 revert 的特定条件。

3.2 对开发者的影响

你可能会看到一些交易被纳入链上但最终执行失败（revert），这些交易仍然会支付 gas 费用

EIP-7702 委托的 EOA 账户，其余额不能低于 10 MON（除非移除委托）

使用 EIP-7702 委托的合约代码中`CREATE` 和 `CREATE2` 操作码被禁止

踩坑记录：最初我在理解这个机制时，误以为"交易 revert 不收费"是以太坊的通用行为。实际上在以太坊上，revert 的交易也消耗 gas，只是没有 Monad 这么明确地将 `gas_limit` 作为收费基准。在 Monad 上，你需要更谨慎地设置 `gas_limit`，避免因为设置过高而支付不必要的费用。

\---

四、Monad 上的高性能开发最佳实践

4.1 硬编码 Gas Limit

对于 gas 消耗固定的操作（如 ETH/MON 转账固定为 21,000 gas），直接硬编码 `gasLimit` 而非调用 `eth_estimateGas`：

\`\`\`javascript

// ✅ 推荐：直接硬编码

const tx = await walletClient.sendTransaction({

to: recipientAddress,

value: parseEther('1.0'),

gasLimit: BigInt(21000), // 固定值

baseFeePerGas: BigInt(100\_000\_000\_000), // 100 gwei

chain: monadChain,

});

// ❌ 不推荐：每次都调用 eth\_estimateGas

const gas = await publicClient.estimateGas({...}); // 多一次 RPC 往返

\`\`\`

关键警告：某些钱包（包括 MetaMask）在 `eth_estimateGas` 调用 revert 时，会将 gas limit 设置为一个极高值。在以太坊上这通常不是大问题（只会用实际消耗的 gas），但在 Monad 上这会导致用户支付巨额费用！

4.2 并发交易提交

Monad 400ms 出块意味着可以高效处理并发交易：

\`\`\`javascript

// ✅ 并发提交多个交易

const transactionsPromises = Array(BATCH\_SIZE)

.fill(null)

.map(async (\_, i) => {

return await walletClient.sendTransaction({

to: recipientAddress,

value: parseEther('0.1'),

gasLimit: BigInt(21000),

baseFeePerGas: BigInt(100\_000\_000\_000),

chain: monadChain,

nonce: nonce + Number(i),

});

});

const hashes = await Promise.all(transactionsPromises);

\`\`\`

注意：并发提交时需要手动管理 nonce。通过 `eth_getTransactionCount` 查询 nonce 会有网络延迟，建议本地维护 nonce 计数器。

4.3 Multicall 优化 eth\_call

Monad Testnet 和 Mainnet 都部署了标准的 `Multicall3` 合约：

\`\`\`

Multicall3 地址: 0xcA11bde05977b3631167028862bE2a173976CA11

\`\`\`

使用 viem 的 multicall 功能：

\`\`\`typescript

import { multicall } from 'viem/contract';

const results = await publicClient.multicall({

contracts: \[

{ address: tokenA, abi: erc20Abi, functionName: 'balanceOf', args: \[userAddress\] },

{ address: tokenB, abi: erc20Abi, functionName: 'balanceOf', args: \[userAddress\] },

{ address: tokenA, abi: erc20Abi, functionName: 'allowance', args: \[userAddress, spenderAddress\] },

\],

});

\`\`\`

重要限制：Multicall3 内部是串行执行的。如果单个调用很昂贵，不要在一个 Multicall 中塞太多调用。对于批量查询，考虑使用 RPC 的 batch 请求或索引器。

4.4 使用索引器替代 eth\_getLogs

Monad 高吞吐量意味着链上数据增长极快，反复调用 `eth_getLogs` 效率低下。官方推荐的索引方案：

索引器 特点 Monad 网络 ID

Envio HyperIndex 事件驱动，快速索引 Testnet: 10143, Mainnet: 143

The Graph 去中心化索引，子图模式 支持 Monad

Allium SQL 查询，实时数据流 支持 Monad

thirdweb Insight REST API，token 数据丰富 Chain ID 143 / 10143

\---

五、安全分析：Web3 开发中的合规与防护

5.1 智能合约安全关键点

结合 Web3 安全实践手册，以下是在 Monad 上开发时需要特别注意的安全事项：

1\. 私钥管理

永远不要在代码中硬编码私钥

使用环境变量或硬件钱包

测试网和主网使用不同的钱包

2\. 合约权限控制

\`\`\`solidity

// 使用 OpenZeppelin 的 AccessControl

import "@openzeppelin/contracts/access/AccessControl.sol";

contract MyContract is AccessControl {

bytes32 public constant ADMIN\_ROLE = keccak256("ADMIN\_ROLE");

constructor() {

_grantRole(DEFAULT_ADMIN\_ROLE, msg.sender);

_grantRole(ADMIN_ROLE, msg.sender);

}

function sensitiveAction() external onlyRole(ADMIN\_ROLE) {

// 只有管理员能调用

}

}

\`\`\`

3\. 重入攻击防护

\`\`\`solidity

import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

contract MyContract is ReentrancyGuard {

function withdraw() external nonReentrant {

// 防止重入攻击

}

}

\`\`\`

5.2 链上安全威胁

根据 Web3 实习手册的安全指南，以下是常见的 Web3 安全威胁：

攻击类型 描述 防护措施

钓鱼攻击 伪造网站/邮件获取私钥 验证 URL、不点击可疑链接

剪贴板木马 替换复制的钱包地址 转账前核对完整地址

社交工程 假冒好友/客服 不在社交平台透露敏感信息

供应链攻击 恶意依赖包/插件 审计依赖、使用官方渠道

Punycode 钓鱼 相似域名欺骗 仔细检查域名字符

踩坑记录：在本地开发环境中，我曾因为图方便直接将私钥写在 `.env` 文件中并提交到 Git。虽然测试网资金不多，但这是一个严重的安全习惯问题。现在我使用 `git-secrets` 钩子在提交前自动扫描敏感信息，并将 `.env` 加入 `.gitignore`。

\---

六、Monad 网络信息速查

6.1 Testnet 配置

\`\`\`json

{

"chainId": 10143,

"networkName": "Monad Testnet",

"currencySymbol": "MON",

"rpcUrl": "[https://rpc.testnet.monad.xyz](https://rpc.testnet.monad.xyz)",

"blockExplorer": "[https://testnet.monadvision.com](https://testnet.monadvision.com)"

}

\`\`\`

6.2 已部署的规范合约

合约名称 地址

Multicall3 `0xcA11bde05977b3631167028862bE2a173976CA11`

Wrapped MON 见官方文档

Permit2 见官方文档

EntryPoint v0.6/0.7/0.8 见官方文档

Safe v1.4.1 见官方文档

\---

七、个人思考与总结

7.1 对异步执行的理解

Monad 最让我印象深刻的设计是异步执行——共识和执行的流水线化。传统区块链（包括以太坊）是同步的：先执行交易，再共识出块。Monad 反过来：先共识确认交易的有效性（通过 Reserve Balance 保证能支付），然后异步执行。这将执行的时间预算从共识时间中解耦，大幅提升了吞吐量。

7.2 对开发者体验的影响

作为一个从以太坊生态转过来的开发者，Monad 的 EVM 兼容性意味着大部分 Solidity 代码和工具链可以直接使用。但 Gas 机制的差异（按 gas\_limit 收费）是一个需要特别注意的设计决策，它要求开发者在前端和合约层面都要更精确地管理 gas。

7.3 本周计划

Week 2 的重点是智能合约进阶和前端交互。接下来几天我会：

在 Monad Testnet 上部署一个完整的 DApp

实践 Multicall 和并发交易的最佳实践

研究 Monad 的 Parallel Execution 如何影响合约设计

完成一个 Mini Demo 的原型

\---

学习时长: 约 3 小时

参考资料: Monad 官方文档 ([docs.monad.xyz](http://docs.monad.xyz))、[ethereum.org](http://ethereum.org) Gas 文档、Web3 实习手册安全章节
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->







一、Week 1 学习脉络回顾

Week 1（7/6-7/10）覆盖了从 Web3 入门到 Monad 核心架构理解的完整路径。这五天的节奏非常紧凑，从钱包配置、测试网交互到 AI+Solidity 合约编写，最后落到 Monad 的底层架构深度理解。今天是复盘日，正好把前面零散吸收的知识做一次系统性梳理。

回顾下来，最大的感受是：Monad 不是在以太坊上做简单修补，而是从共识层到执行层到存储层都做了系统性重构，同时保持了 EVM 字节码级兼容。这种"大改底层、不动上层"的设计哲学，是我这周最大的认知收获。

\---

二、以太坊 vs Monad 核心参数对比

这是本周学习中我最关注的部分。整理成表格后，差异一目了然：

参数 Ethereum Monad

TPS ~15-30 10,000

出块时间 12s 400ms

最终性 ~12min (2 epoch) 800ms

区块 Gas 上限 30M 200M

单笔交易 Gas 上限 ~30M 30M

最低 Base Fee 动态 (通常 ~1 gwei) 100 MON-gwei

合约代码大小限制 24 KB 128 KB

Init Code 大小限制 48 KB 256 KB

Gas 计费方式 按 gas\_used 按 gas\_limit

共识机制 Gasper (Casper FFG + LMD-GHOST) MonadBFT

执行模式 顺序执行 并行执行 + JIT 编译

存储层 LevelDB/PebbleDB MonadDb (定制化)

区块传播 libp2p gossip RaptorCast (喷泉码)

EIP-4844 (Blob) ✅ 支持 ❌ 不支持

全局 Mempool ✅ 有 ❌ 无（Local Mempool）

MMR (区块承诺) ❌ ✅ (Append-Only MMR)

secp256r1 预编译 ❌ ✅ (EIP-7951)

几个关键差异值得展开说：

1\. Gas 计费：按 limit 还是按 used？

这是 Monad 和以太坊最容易踩坑的区别。以太坊上交易只收取实际消耗的 gas（gas\_used），未消耗的部分会退还。但 Monad 收取的是 gas\_limit 的全额，无论实际用了多少。

这是为异步执行（Asynchronous Execution）服务的设计决策。因为在 Monad 中，Leader 构建区块时还未执行交易，如果按 gas\_used 收费，攻击者可以提交一个 gas\_limit 很高但实际消耗极少的交易来占用区块空间，形成 DoS 攻击。所以 Monad 直接按 gas\_limit 收费，消除了这个攻击向量。

对开发者的影响： 不要随意把 gas\_limit 设得很高！在以太坊上这无非是多冻结一点额度，用不完会退。但在 Monad 上，设多了就真扣了。最佳实践是对于已知固定 gas 消耗的操作（如 ETH/MON 转账恒定 21,000），直接硬编码 gas\_limit，不要调用 eth\_estimateGas。

2\. 区块 Gas 上限：30M vs 200M

Monad 的区块 Gas 上限是以太坊的约 6.7 倍。这意味着每个区块能容纳的交易量大幅提升，配合 400ms 的出块时间，才有了 10,000 TPS 的吞吐量。

3\. 合约大小限制：24KB vs 128KB

Monad 把合约代码上限从 24KB 提升到了 128KB。这对开发者来说是个好消息——复杂的 DeFi 协议不需要再费尽心思做合约拆分和 delegatecall 拼接。但也要注意，更大的合约意味着更大的部署成本和更长的验证时间，不能滥用。

\---

三、Monad 核心架构深度复盘

本周花了大量时间研究 Monad 的五大核心创新，这里逐一整理：

3.1 MonadBFT — 共识层

MonadBFT 是一种前沿的 BFT 共识机制，解决了 tail-forking 问题。传统 BFT 共识在面对恶意 leader 时，需要等待完整的 2/3 投票才能确认区块。MonadBFT 通过优化的消息传递和投票聚合机制，实现了更快的响应速度和更强的抗分叉能力。

关键特性：

基于 HotStuff 的改进变体

解决 tail-forking 问题

验证者网络分布广泛（可参考 [gmonads.com](http://gmonads.com) 的验证者地图）

3.2 RaptorCast — 区块传播

RaptorCast 使用喷泉码（Fountain Code）来高效传播区块数据。传统的 gossip 协议在大区块场景下带宽消耗大、延迟高。RaptorCast 通过编码技术，让验证者只需收到足够多的编码片段就能恢复完整区块，大幅降低了传播延迟。

3.3 异步执行 — 共识与执行解耦

这是 Monad 最核心的创新之一。在以太坊中，共识和执行是同步的——Leader 先执行交易、计算状态根，然后才出块。Monad 把这两个阶段解耦：

Leader 在共识阶段不需要执行交易，直接打包

执行在共识确认后异步进行

下一个区块的共识可以和上一个区块的执行并行

这就是为什么 Monad 需要按 gas\_limit 收费的原因——共识阶段无法知道实际 gas 消耗。

3.4 并行执行 + JIT 编译

Monad 的执行引擎支持并行交易执行。传统 EVM 交易是严格串行的，Monad 通过乐观并行执行（Optimistic Parallel Execution）来突破这个限制：

多个 CPU 核心同时执行不同交易

如果出现冲突（读写同一状态），通过调度重试解决

JIT（Just-In-Time）编译将热点字节码编译为本地机器码，提升执行效率

3.5 MonadDb — 定制化存储

MonadDb 是专门为区块链状态存储设计的数据库。以太坊使用 LevelDB/PebbleDB 作为通用存储引擎，I/O 效率成为瓶颈。MonadDb 针对区块链的读写模式做了深度优化：

针对 MPT（Merkle Patricia Trie）的访问模式优化

更高效的 SSD I/O 利用率

支持 Monad 的并行执行需求

\---

四、Monad 测试网与开发环境配置

本周实际操作的部分，整理关键信息：

4.1 网络信息

项目 值

Chain ID (Testnet) 10143

Chain ID (Mainnet) 143

货币符号 MON

测试网水龙头 [https://faucet.monad.xyz](https://faucet.monad.xyz)

测试网应用中心 [https://testnet.monad.xyz/](https://testnet.monad.xyz/)

区块浏览器 MonadVision / Monadscan

主网上线日期 2025-11-24

4.2 RPC 端点

提供商 速率限制 批量请求 归档支持

QuickNode 50 rps 100 ✅

Ankr 300 reqs/10s 100 ❌

Monad Foundation 20 rps 不支持 ✅

4.3 核心合约地址（测试网）

Multicall3: `0xcA11bde05977b3631167028862bE2a173976CA11`

Permit2: 已部署

EntryPoint v0.6/v0.7/v0.8: 已部署

Safe v1.4.1 全套合约: 已部署

Wrapped MON: 已部署

4.4 Foundry 配置

Monad 提供了自定义的 Monad Foundry，确保本地开发环境与链上行为一致。这点很重要——普通的 Foundry 可能无法准确模拟 Monad 的 gas 计费差异。

\---

五、踩坑记录

坑 1：MetaMask 的 gas limit 陷阱

在测试网交互时，有一次调用合约方法 revert 了，MetaMask 自动把 gas limit 设到了一个极大的值（看起来是放弃了估算）。在以太坊上这不是问题，revert 后未使用的 gas 会退还。但在 Monad 上，这意味着你为一个失败的交易付了天价 gas。最佳实践：对于已知 gas 消耗的操作，一定要手动设置 gas limit。

坑 2：eth\_estimateGas 的延迟

多次串行调用 eth\_estimateGas 会显著拖慢钱包交互体验。在高 TPS 的 Monad 上，这种延迟更加明显。解决方案：对于固定 gas 操作直接硬编码，对于需要估算的场景使用 Multicall 合并请求。

坑 3：历史数据不可用

由于 Monad 的高吞吐量，全节点不提供任意历史状态查询。这意味着不能像在以太坊上那样调用 `eth_getStorageAt(blockTag)` 来查询任意区块的状态。需要使用索引器（Allium、Envio、The Graph 等）来处理历史数据查询。

坑 4：并发交易的 nonce 管理

在 Monad 上提交多笔交易时，如果依赖 `eth_getTransactionCount` 来获取 nonce，可能因为网络延迟导致 nonce 冲突。必须在本地维护 nonce 计数器，特别是在批量发送交易时。

\---

六、安全思考

本周在学习 Web3 基础的同时，也深入研究了 Web3 安全与合规。结合 [https://web3intern.xyz/zh/security/](https://web3intern.xyz/zh/security/) 的内容，整理几个关键安全要点：

6.1 智能合约安全基础

重入攻击（Reentrancy）：依然是最常见的漏洞类型。在 Monad 的并行执行环境下，重入攻击的模式可能有新的变化，需要持续关注。

整数溢出/下溢：Solidity 0.8+ 已内置检查，但 unchecked 块中仍需小心。

访问控制：确保关键函数有正确的权限修饰符。

6.2 Monad 特有的安全考量

Gas limit 而非 gas\_used 计费：这意味着合约中的 gas 优化变得更加重要，因为你无法通过"实际消耗少"来省钱。

并行执行冲突：在设计合约时，如果多个交易频繁修改同一状态变量，可能导致大量执行冲突和重试，影响性能。

EIP-7702 代理限制：如果 EOA 被 EIP-7702 委托，余额不能低于 10 MON，且被调用时不能使用 CREATE/CREATE2。

6.3 合规意识

作为开发者，需要了解：

中国法律禁止代币发行融资（ICO/IEO/IDO）

虚拟货币不具有法偿性

涉及赌博、传销、洗钱机制的项目有刑事风险

技术人员参与代币模型设计也可能被追责

这不是吓唬人，而是每个 Web3 开发者必须有的底线意识。在 Monad 上开发 DApp 时，要确保项目设计不触碰这些红线。

\---

七、个人总结与下周展望

Week 1 收获

技术认知升级：从"知道 Monad 是个高性能 L1"升级到理解其五大核心创新的原理和相互关系。

开发环境搭建：钱包、测试网、Foundry 全部配置完毕，可以开始实际开发。

安全意识建立：不只是会写合约，还要知道怎么写安全的合约，以及法律合规的边界在哪里。

踩坑经验积累：gas 计费差异、历史数据不可用、并发 nonce 管理——这些都是在实际开发中会遇到的真实问题。

Week 2 展望

下周进入智能合约进阶、前端交互和 AI 辅助开发。期待能把 Week 1 的理论知识转化为实际的 DApp 开发能力。特别是在 Monad 的高 TPS 环境下，如何设计高效的前端交互模式、如何利用 AI 工具加速开发，是接下来的重点。

\---

八、参考资源

Monad 官方文档：[https://docs.monad.xyz/](https://docs.monad.xyz/)

Monad 与以太坊差异：[https://docs.monad.xyz/developer-essentials/differences](https://docs.monad.xyz/developer-essentials/differences)

Gas 定价机制：[https://docs.monad.xyz/developer-essentials/gas-pricing](https://docs.monad.xyz/developer-essentials/gas-pricing)

最佳实践：[https://docs.monad.xyz/developer-essentials/best-practices](https://docs.monad.xyz/developer-essentials/best-practices)

网络信息：[https://docs.monad.xyz/developer-essentials/testnets](https://docs.monad.xyz/developer-essentials/testnets)

以太坊 Gas 文档：[https://ethereum.org/en/developers/docs/gas/](https://ethereum.org/en/developers/docs/gas/)

Web3 安全合规指南：[https://web3intern.xyz/zh/security/](https://web3intern.xyz/zh/security/)

Monad GitHub（共识）：[https://github.com/category-labs/monad-bft](https://github.com/category-labs/monad-bft)

Monad GitHub（执行）：[https://github.com/category-labs/monad](https://github.com/category-labs/monad)

\---

Monad Builder Camp Day 7 打卡完成。Week 1 从入门到架构理解，打好了基础。下周开始写真正的合约和 DApp。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->








一、Week 1 学习脉络回顾

过去五天（7/6-7/10），我从零开始搭建了 Monad 开发环境，完成了钱包配置、测试网交互和基础智能合约部署。今天作为 Week 1 的复盘日，我重新梳理了 Monad 文档中的核心架构设计，重点关注 Monad 与以太坊的关键差异，以及在实际开发中必须注意的坑。

\---

二、Monad 核心架构：五大技术创新

Monad 并非简单的"更快的 EVM"，而是从底层重新设计了区块链的执行管线。文档明确列出了五个核心创新：

1\. MonadBFT — 解决尾部分叉问题

Monad 采用自研的 BFT 共识机制 MonadBFT，专门解决了传统 BFT 中的 tail-forking 问题。尾部分叉是指在共识过程中，最后一个区块的提议者恶意行为导致链分叉，MonadBFT 通过优化投票和超时机制来防止这种情况。

2\. RaptorCast — 高效区块传播

使用 Raptor 码（一种喷泉码变体）进行区块数据传播，确保即使部分数据包丢失，验证者也能高效重建完整区块。

3\. 异步执行（Asynchronous Execution）

这是 Monad 最关键的设计之一。在以太坊中，区块提议者必须先执行交易才能验证区块的有效性。而 Monad 将共识和执行解耦：

Leader 构建区块后，不等待执行完成就开始共识投票

验证者在投票时只验证区块的合法性（签名、格式等），不执行交易

执行在后台异步进行

这意味着共识的时间预算不再受执行时间限制，大幅提升吞吐量。

4\. 并行执行 + JIT 编译

交易并行执行，配合即时编译（JIT Compilation），大幅提升 EVM 字节码的执行效率。

5\. MonadDb — 高效状态存储

专为以太坊状态存储优化的数据库，使用 C++ 和 Rust 编写，解决 I/O 瓶颈。

\---

三、Ethereum vs Monad 核心参数对比

特性 Ethereum Monad

TPS ~15-30 10,000

出块时间 12 秒 400ms

最终性 ~12 分钟（2 epochs） 800ms

Gas 收费方式 按 `gas_used` 收费 按 `gas_limit` 收费

区块 Gas 上限 30M 200M

交易 Gas 上限 无硬限制 30M

最低 Base Fee 动态（可接近 0） 100 MON-gwei

Base Fee 控制器 EIP-1559 标准 自定义（慢升快降）

EIP-1559 兼容 原生 完全兼容

共识机制 Gasper (Casper FFG + LMD GHOST) MonadBFT

执行模型 顺序执行 并行执行 + JIT

节点语言 Go/Rust（多客户端） C++/Rust（双客户端）

\---

四、Gas 机制深度分析：最容易踩坑的地方

4.1 收费逻辑的根本差异

以太坊：

\`\`\`

gas\_paid = gas\_used \* price\_per\_gas

\`\`\`

Monad：

\`\`\`

gas\_paid = gas\_limit \* price\_per\_gas

\`\`\`

这是开发者最容易踩的坑。 在 Monad 上，即使你的交易实际只消耗了 21,000 gas，如果你设置了 100,000 的 gas limit，你就要为 100,000 gas 付费。

为什么这样设计？因为异步执行。在 Monad 中，Leader 在构建区块时不知道交易的实际 gas 消耗，它只能根据 gas limit 来计算区块是否超限。如果按 gas\_used 收费，恶意用户可以提交一个 gas\_limit 极高但实际消耗极少的交易来 DoS 网络。

4.2 EIP-1559 兼容但有差异

Monad 完全支持 EIP-1559 Type 2 交易：

\`\`\`

price\_per\_gas = min(base\_price\_per\_gas + priority\_price\_per\_gas, max\_price\_per\_gas)

\`\`\`

但 base fee 控制器不同。Monad 的控制器设计目标是慢升快降：

慢升：避免因短暂拥堵导致 base fee 飙升

快降：避免区块空间因 base fee 过高而浪费

参数对比：

`max_step_size` = 1/28

`target` = 160M gas（80% 的 200M 区块上限）

`β` = 0.96（趋势衰减因子）

`min_base_price_per_gas` = 100 MON-gwei

4.3 开发建议

1\. 对于固定 gas 操作，直接硬编码 gas limit

\`\`\`typescript

// ✅ 推荐：固定 gas 的操作直接设置

const tx = await wallet.sendTransaction({

to: recipient,

value: parseEther("0.1"),

gasLimit: 21000, // ETH 转账固定 21000

});

\`\`\`

这样做的好处：

减少一次 `eth_estimateGas` 调用的延迟

避免钱包在合约 revert 时设置异常高的 gas limit

2\. 批量操作使用 Multicall3

Monad 上 `Multicall3` 已部署在标准地址 `0xcA11bde05977b3631167028862bE2a173976CA11`，可以将多个 `eth_call` 合并为一次调用：

\`\`\`typescript

// 使用 viem 的 multicall

const results = await client.multicall({

contracts: \[

{ address: tokenA, abi: erc20Abi, functionName: 'balanceOf', args: \[wallet\] },

{ address: tokenB, abi: erc20Abi, functionName: 'balanceOf', args: \[wallet\] },

{ address: router, abi: routerAbi, functionName: 'getAmountsOut', args: \[amount, path\] },

\],

});

\`\`\`

3\. 并发提交交易时管理 nonce

由于 Monad 400ms 出块，并发提交交易时必须本地管理 nonce，不能依赖 `eth_getTransactionCount`：

\`\`\`typescript

const baseNonce = await client.getTransactionCount({ address: wallet.address });

const promises = [txs.map](http://txs.map)((tx, i) =>

wallet.sendTransaction({ ...tx, nonce: baseNonce + i })

);

const hashes = await Promise.all(promises);

\`\`\`

\---

五、测试网信息速查

项目 值

网络名 Monad Testnet

Chain ID 10143

货币符号 MON

水龙头 [https://faucet.monad.xyz](https://faucet.monad.xyz)

区块浏览器 MonadVision / Monadscan

App Hub [https://testnet.monad.xyz/](https://testnet.monad.xyz/)

当前版本 v0.14.5 / MONAD\_NINE

RPC 提供商限制：

QuickNode: 50 rps, batch 100, 支持 archive

Ankr: 300 reqs/10s, batch 100, 不支持 archive, 不允许 debug\_\*

Monad Foundation: 20 rps, 不允许 batch, 支持 archive

已部署的标准合约包括：Wrapped MON、CreateX、Multicall3、Permit2、Safe v1.4.1 全套、EntryPoint v0.6/0.7/0.8。

\---

六、踩坑记录

踩坑 1：MetaMask 的 gas limit 陷阱

MetaMask 在 `eth_estimateGas` 调用的合约 revert 时，会将 gas limit 设置为一个非常高的值。在以太坊上这通常问题不大（因为只收实际消耗），但在 Monad 上这意味着你会被收一大笔钱。

解决方案：对于已知 gas 消耗的交易，始终手动设置 gasLimit。

踩坑 2：Batch Request 在 Monad Foundation RPC 上不被允许

如果你习惯用 JSON-RPC batch request（将多个请求打包成一个数组），在使用 Monad Foundation 的公共 RPC 时会被拒绝。需要改用 QuickNode 或 Ankr 的端点。

踩坑 3：eth\_call 并发不等于批量

多个并发的 `eth_call` 是独立的 HTTP 请求，在 Monad 上虽然延迟很低，但如果要查询多个合约状态，用 Multicall3 一次 `eth_call` 搞定效率更高。

\---

七、安全合规思考

本周还回顾了 Web3 安全与合规的知识，作为在中国大陆的开发者，以下几点必须牢记：

代币发行的法律风险：中国明确禁止 ICO/IEO/IDO，参与代币模型设计、空投逻辑配置、合约部署等环节，即使只是技术人员，也可能被视为"共同行为人"。

智能合约安全：在 Monad 上部署合约前，必须进行充分的测试。虽然 Monad 兼容 EVM，但并行执行可能引入新的并发安全问题——如果两个交易并发修改同一 storage slot，时序可能与顺序执行不同。

出金合规：薪酬中如果包含虚拟货币，通过 C2C 出金时务必选择可信渠道，避免接收来路不明的资金导致银行卡冻结。

FATF Travel Rule：超过 1000 美元/欧元的虚拟资产转账，VASP 需要收集和传输发送方/接收方信息，这对 DeFi 应用的合规设计提出了要求。

\---

八、Week 2 展望

下周（Day 8-14）进入智能合约进阶和前端交互阶段。计划：

用 Foundry 在 Monad testnet 上部署一个完整的 DeFi 合约

实现前端与合约的交互（viem + React）

探索 Monad 的并行执行对合约设计的影响

研究 Monad 上的 indexer 方案（Envio、The Graph、Goldsky）

\---

九、今日收获总结

深入理解了 Monad 异步执行的设计动机和对 gas 机制的影响

掌握了 Monad 与以太坊的 8 个关键参数差异

建立了 Monad 开发的最佳实践：硬编码 gas、Multicall3、本地 nonce 管理

复习了 Web3 安全合规要点，特别是国内开发者的法律边界

\> 💡 核心认知：Monad 的高性能不是靠堆硬件实现的，而是通过软件架构的五个创新点（MonadBFT、RaptorCast、异步执行、并行执行、MonadDb）在普通硬件上实现的。这与 Solana 的"硬件换性能"路线完全不同，是对去中心化的坚持。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->









\### 今日学习内容 今天是 Monad Builder Camp 第一周的最后一天，主题聚焦于深入理解 Monad 的核心架构与运行机制。经过前四天对 Web3 基础、钱包操作、测试网交互和 AI+Solidity 的学习，今天终于进入 Monad 的技术内核。 \*\*Monad 的核心定位\*\* Monad 是一条 Layer-1 区块链，其核心愿景是打破"去中心化与高性能不可兼得"的传统认知。与许多通过加重硬件要求来提升性能的链不同，Monad 的性能提升来自纯软件架构创新，同时保持对普通硬件的友好——任何人都可以运行节点。Monad 的代码库完全开源（共识层 \`monad-bft\` 和执行层 \`monad\` 均在 GitHub 上，采用 GPL-3.0 许可证），主要使用 C++ 和 Rust 编写，追求极致性能。 \*\*五大架构创新\*\* Monad 在五个关键领域引入了创新： 1. \*\*MonadBFT\*\*：前沿的 BFT 共识机制，解决了 tail-forking 问题。传统 BFT 共识在面对网络分区时容易产生尾部分叉，MonadBFT 通过新颖的设计解决了这一痛点，同时保持了快速响应和抗分叉特性。 2. \*\*RaptorCast\*\*：高效的区块传播机制。在高吞吐量场景下，如何快速将区块广播到全球节点是一个关键瓶颈。RaptorCast 利用喷泉码（Fountain Code）技术实现高效的数据分发，确保即使部分数据包丢失，接收方也能完整重建区块。 3. \*\*异步执行（Asynchronous Execution）\*\*：这是 Monad 最关键的创新之一。在传统区块链中，共识和执行是串行的——先执行交易，再达成共识。Monad 将共识和执行解耦为流水线（pipeline），共识先完成投票，执行随后异步进行，从而大幅提升了执行的时间预算。 4. \*\*并行执行（Parallel Execution）与 JIT 编译\*\*：Monad 支持交易的并行执行，同时引入即时编译（JIT Compilation / Native Compilation），将 EVM 字节码编译为本地机器码，进一步提升执行效率。 5. \*\*MonadDb\*\*：专为以太坊状态存储优化的数据库。以太坊的状态数据量巨大，传统的 LevelDB/RocksDB 方案存在瓶颈，MonadDb 从底层重新设计了状态存储架构。 \*\*实际性能参数\*\* - 吞吐量：10,000 TPS（每秒交易数） - 出块频率：400ms（以太坊约 12 秒） - 最终确认时间：800ms（以太坊约 12-15 分钟达到合理最终性） - 区块 Gas 上限：200M gas（以太坊约 30M） - 单笔交易 Gas 上限：30M gas（以太坊约 30M） - 最小 base fee：100 MON-gwei ### Monad 深度解析 \*\*Gas 定价机制的关键差异\*\* Monad 的 Gas 机制与以太坊存在几个根本性差异，这是开发者必须深刻理解的： 1. \*\*按 Gas Limit 收费而非 Gas Used\*\*：这是最重要的区别。在以太坊中，用户实际只支付消耗的 gas（\`gas\_used \* price\_per\_gas\`），未使用的 gas 会退还。但在 Monad 中，收费基于 \`gas\_limit\`（\`gas\_limit \* price\_per\_gas\`）。 这个设计决策源于异步执行的需求：由于区块在执行之前就需要达成共识，如果按 gas\_used 收费，恶意用户可以提交一个 gas\_limit 极高但实际消耗极少的交易来占用区块空间而不付费，形成 DoS 攻击向量。因此，开发者必须精确设置 gas limit，避免过度设置导致不必要的费用浪费。 2. \*\*Base Fee 控制器差异\*\*：Monad 采用了一个比以太坊更复杂的 base fee 控制器。该控制器包含趋势（trend）和动量（momentum）两个维度： - 目标区块填充率：80%（160M gas，区块上限为 200M） - 最大步长：1/28 - 平滑系数 β：0.96 - 设计原则：增加更慢，减少更快——避免 base fee 过高导致区块空间浪费 3. \*\*EIP-1559 兼容\*\*：Monad 支持 EIP-1559 类型交易，\`price\_per\_gas = min(base\_price\_per\_gas + priority\_price\_per\_gas, max\_price\_per\_gas)\`，与以太坊的定价公式一致。 4. \*\*无全局 Mempool\*\*：与以太坊的全局交易池不同，Monad 没有全局 mempool，交易仅转发给接下来的几个 leader，这是为了效率优化。 \*\*EVM 兼容性细节\*\* Monad 实现了完整的 EVM 字节码兼容，但在某些参数上有调整： - 合约代码大小上限：128 KB（以太坊为 24 KB） - Init code 大小上限：256 KB（以太坊为 48 KB） - 部分 opcode 和 precompile 的 gas 价格经过重新调整 - 新增 \`secp256r1\`（P256）验证 precompile（地址 \`0x0100\`），支持链上 WebAuthn/Passkey 签名验证 - 不支持 EIP-4844（blob 交易） \*\*Reserve Balance 机制\*\* Monad 引入了储备余额（Reserve Balance）机制，确保所有被共识层包含的交易都能被支付。这意味着： - 由于 EIP-7702 委托的 EOA 余额不能低于 10 MON - 交易可能在链上被包含但最终执行失败（但仍支付 gas），这与以太坊中 reverting 交易的行为一致，但在 Monad 的异步执行模型下更为常见 ### 实操记录 \*\*1. 测试网配置（Monad Testnet）\*\* \`\`\`javascript // MetaMask / WalletConnect 网络配置 const monadTestnet = { chainId: 10143, chainName: "Monad Testnet", currencySymbol: "MON", rpcUrls: \["https://testnet-rpc.monad.xyz"\], // Monad Foundation RPC blockExplorerUrls: \["https://monadvision.com"\] }; // 添加到 MetaMask await window.ethereum.request({ method: 'wallet\_addEthereumChain', params: \[monadTestnet\] }); \`\`\` \*\*2. 使用 viem 连接 Monad Testnet\*\* \`\`\`typescript import { createPublicClient, http, parseEther } from 'viem'; const monadTestnet = { id: 10143, name: 'Monad Testnet', network: 'monad-testnet', nativeCurrency: { name: 'MON', symbol: 'MON', decimals: 18 }, rpcUrls: { default: { http: \['https://testnet-rpc.monad.xyz'\] }, }, blockExplorers: { default: { name: 'MonadVision', url: 'https://monadvision.com' }, }, }; const client = createPublicClient({ chain: monadTestnet, transport: http(), }); // 查询最新区块 const blockNumber = await client.getBlockNumber(); console.log('Latest block:', blockNumber); \`\`\` \*\*3. Gas 估算最佳实践（Monad 特别注意）\*\* \`\`\`typescript // ❌ 不推荐：每次调用 eth\_estimateGas（Monad 上要注意钱包行为） const gasEstimate = await client.estimateGas({...}); // ✅ 推荐：已知固定 gas 操作直接硬编码 const txHash = await walletClient.sendTransaction({ to: recipient, value: parseEther('0.1'), gasLimit: BigInt(21000), // 转账固定 21000 gas // ⚠️ Monad 按 gasLimit 收费，不要设置过高！ }); \`\`\` \*\*4. 批量 RPC 调用优化\*\* \`\`\`typescript // 使用 Multicall3 合约（Monad 主网和测试网均已部署） import { multicall3Abi } from './abis'; const MULTICALL3\_ADDRESS = '0xcA11bde05977b3631167028862bE2a173976CA11'; // 或使用 viem 的内置 multicall import { publicClient } from './client'; const results = await publicClient.multicall({ contracts: \[ { address: tokenA, abi: erc20Abi, functionName: 'balanceOf', args: \[wallet\] }, { address: tokenB, abi: erc20Abi, functionName: 'balanceOf', args: \[wallet\] }, { address: tokenA, abi: erc20Abi, functionName: 'allowance', args: \[wallet, spender\] }, \], }); \`\`\` \*\*5. 并发交易提交\*\* \`\`\`typescript // ✅ Monad 最佳实践：并发提交多笔交易 const BATCH\_SIZE = 5; const nonce = await client.getTransactionCount({ address: account.address }); const txPromises = Array(BATCH\_SIZE).fill(null).map(async (\_, i) => { return await walletClient.sendTransaction({ to: recipient, value: parseEther('0.01'), gasLimit: BigInt(21000), baseFeePerGas: BigInt(100\_000\_000\_000), // 100 gwei (Monad min base fee) nonce: nonce + i, }); }); const hashes = await Promise.all(txPromises); console.log('Transaction hashes:', hashes); \`\`\` ### 关键知识点对比 | 概念 | 以太坊 | Monad | 优势分析 | |------|--------|-------|---------| | 共识机制 | Gasper (Casper FFG + LMD-GHOST) | MonadBFT | 解决 tail-forking，更快响应 | | 出块时间 | ~12 秒 | 400ms | 30 倍提升，用户体验显著改善 | | 最终确认 | ~12-15 分钟 | 800ms | 从分钟级到亚秒级，DeFi 场景更友好 | | 吞吐量 | ~15-30 TPS | 10,000 TPS | 超过 300 倍提升 | | Gas 计费 | 按 gas\_used | 按 gas\_limit | 防 DoS，但需开发者注意精准设置 | | 区块 Gas 上限 | ~30M | 200M | 6.7 倍，支持更多交易打包 | | 最小 Base Fee | 动态（~1 gwei 量级） | 100 MON-gwei | Monad 明确最低值，定价更可预测 | | 状态存储 | LevelDB/RocksDB | MonadDb | 专为高吞吐优化的存储引擎 | | 交易执行 | 串行执行 | 并行执行 + JIT 编译 | 充分利用多核 CPU，大幅加速 | | Mempool | 全局 mempool | 本地 mempool（仅转发给后续 leader） | 减少网络开销，提升效率 | | 合约大小上限 | 24 KB | 128 KB | 5 倍，允许部署更复杂的合约 | | Blob 交易 (EIP-4844) | 支持 | 不支持 | Monad 用原生扩容替代 L2 blob 路径 | | 历史状态访问 | 全节点支持任意历史状态 | 不支持任意历史状态 | 需使用 indexer 查询历史数据 | | WebAuthn 支持 | EIP-7951 提案中 | 原生支持 P256 precompile | 更好的账户抽象和安全签名 | ### 代码片段 \*\*Reserve Balance 检查（Monad 特有逻辑）\*\* \`\`\`solidity // SPDX-License-Identifier: MIT pragma solidity ^0.8.20; /// @title MonadReserveChecker /// @notice 在 Monad 上部署合约前检查余额是否满足 Reserve Balance 要求 contract MonadReserveChecker { // Monad 要求 EOA 账户在 EIP-7702 委托后保持至少 10 MON uint256 constant MIN\_RESERVE\_BALANCE = 10 ether; // 10 MON function checkReserve(address account) external view returns (bool sufficient) { uint256 balance = account.balance; return balance >= MIN\_RESERVE\_BALANCE; } // 在 Monad 上，gasLimit 就是实际收费量，不要盲目设置高值 function estimateActualCost( uint256 gasLimit, uint256 baseFeePerGas, uint256 priorityFeePerGas ) external pure returns (uint256 cost) { uint256 pricePerGas = baseFeePerGas + priorityFeePerGas; // ⚠️ Monad: cost = gasLimit \* pricePerGas（不是 gasUsed） return gasLimit \* pricePerGas; } } \`\`\` \*\*Hardhat 配置（Monad Testnet）\*\* \`\`\`javascript // hardhat.config.js require("@nomicfoundation/hardhat-toolbox"); module.exports = { solidity: "0.8.20", networks: { monadTestnet: { url: "https://testnet-rpc.monad.xyz", chainId: 10143, accounts: \[process.env.PRIVATE\_KEY\], gasPrice: 100000000000, // 100 gwei (Monad minimum) }, }, }; \`\`\` ### 今日思考 \*\*1. 异步执行的深远影响\*\* Monad 的异步执行模型是其最核心的创新，但也带来了开发范式的根本变化。按 gas\_limit 收费意味着开发者必须像管理资金一样精确管理 gas 设置。在以太坊上，设置过高的 gas limit 只是"锁定"资金，多余的会退还；但在 Monad 上，这直接意味着真金白银的损失。这要求我们在开发 DApp 时建立更严格的 gas 估算流程，甚至需要为每种操作类型建立 gas 基准数据库。 \*\*2. 安全视角的思考\*\* 从 Web3 安全合规的角度来看，Monad 的高性能带来了新的安全考量。10,000 TPS 意味着攻击面也相应扩大——闪电贷攻击可能在 400ms 的出块时间内完成更复杂的操作序列。同时，无全局 mempool 的设计改变了 MEV 的形态，传统以太坊上的 MEV 策略（如三明治攻击）在 Monad 上需要完全重新设计。 从法律合规角度，中国对 Web3 的监管基调为"全面封堵金融属性，有限容忍技术创新"。在 Monad 上进行开发时，技术研究和学习是被允许的，但需要注意：不要参与代币发行融资，不要设计带有赌博、传销特征的机制，出金操作务必通过合规渠道。Web3 开发者应该像任何其他软件开发者一样，关注代码质量、安全审计和用户保护。 \*\*3. 生态建设的思考\*\* Monad 已于 2025 年 11 月 24 日启动公共主网，测试网也已重置并稳定运行（当前版本 v0.14.5）。生态中已部署了大量标准化合约（Multicall3、Permit2、Safe v1.4.1 等），这意味着开发者可以直接复用以太坊生态的成熟工具和库，真正实现"无缝迁移"。对于 Builder Camp 的参与者来说，这是一个巨大的优势——我们可以专注于创新应用，而非基础设施适配。 \*\*4. 数据索引的重要性\*\* 由于 Monad 的高吞吐量，全节点不提供任意历史状态访问，这与以太坊有本质区别。这意味着在构建 DApp 时，必须将 indexer（如 Allium、Envio HyperIndex、The Graph、thirdweb Insight）作为基础设施的一部分，而非可选组件。这是一个需要在项目架构设计初期就考虑的问题。 ### 明日计划 明天进入 Day 6（Week 1 复盘），计划： 1. 整理本周五天的学习笔记，建立知识图谱 2. 复习 Wallet 创建、测试网交互的完整流程 3. 总结 Monad 与以太坊的核心差异要点清单 4. 尝试在 Monad Testnet 上部署一个简单的 ERC-20 合约 5. 梳理 Monad 的 gas 机制要点，编写一份开发注意事项 Checklist
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->










````
## Day 4 | Week 1 | AI + Solidity + 合约部署：在 Monad 上部署第一个智能合约

### 今日学习内容

今天的核心任务是用 AI 辅助编写 Solidity 智能合约，并在 Monad Testnet 上完成部署。这是从"理解区块链"到"动手构建"的关键一步。

**Solidity 基础语法回顾：**

Solidity 是以太坊虚拟机（EVM）兼容链的主力智能合约语言，Monad 完全兼容 EVM，因此 Solidity 合约可以直接部署到 Monad 上。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Gmonad {
    string public greeting;
    
    constructor(string memory _greeting) {
        greeting = _greeting;
    }
    
    function setGreeting(string calldata _greeting) external {
        greeting = _greeting;
    }
}
```

这个简单的合约展示了 Solidity 的核心概念：
- **状态变量**：`greeting` 是一个 string 类型的公共状态变量，存储在链上
- **构造函数**：`constructor` 在合约部署时执行一次，用于初始化状态
- **外部函数**：`setGreeting` 是一个 external 函数，允许外部调用修改状态
- **public 自动生成 getter**：声明为 `public` 的变量会自动生成一个同名的只读函数

**AI 辅助合约开发实践：**

使用 AI 工具辅助编写了一个更完整的合约，包含事件发射、访问控制和错误处理：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract MonadGreeter {
    string public greeting;
    address public owner;
    uint256 public updateCount;
    
    event GreetingUpdated(address indexed updater, string newGreeting, uint256 timestamp);
    
    error OnlyOwner();
    error EmptyGreeting();
    
    constructor(string memory _greeting) {
        if (bytes(_greeting).length == 0) revert EmptyGreeting();
        greeting = _greeting;
        owner = msg.sender;
        updateCount = 0;
    }
    
    function setGreeting(string calldata _greeting) external {
        if (bytes(_greeting).length == 0) revert EmptyGreeting();
        greeting = _greeting;
        updateCount++;
        emit GreetingUpdated(msg.sender, _greeting, block.timestamp);
    }
    
    function getGreeting() external view returns (string memory) {
        return greeting;
    }
}
```

改进点：
- **事件（Event）**：`GreetingUpdated` 记录每次更新，方便链下索引
- **自定义错误**：`OnlyOwner()` 和 `EmptyGreeting()` 比 require 更省 Gas
- **计数器**：`updateCount` 追踪更新次数
- **view 函数**：`getGreeting()` 不消耗 Gas（只读操作）

### Monad 深度解析：合约部署在 Monad 上的特殊之处

**1. 合约大小限制**

| 参数 | 以太坊 | Monad | 意义 |
|------|--------|-------|------|
| 最大合约代码大小 | 24 KB | **128 KB** | 可部署更复杂的合约 |
| 最大 init code 大小 | 48 KB | **256 KB** | 构造函数可以更复杂 |

Monad 将合约大小限制提升到 128 KB（以太坊的 5 倍），这意味着可以在单个合约中实现更复杂的逻辑，而不需要拆分成多个合约或使用代理模式。

**2. Gas 计费差异**

这是 Monad 与以太坊最重要的区别之一：

| 维度 | 以太坊 | Monad |
|------|--------|-------|
| 收费基准 | gas_used（实际消耗） | **gas_limit（设置的上限）** |
| 退款机制 | 未消耗的 Gas 退还 | **不退还** |
| 建议 | 可以设置较高 gas_limit | **必须精确设置** |

在 Monad 上，如果设置 gas_limit = 100,000 但实际只用了 50,000，仍然会收取 100,000 的费用。因此：
- 对于固定操作（如转账），直接硬编码 gas_limit = 21,000
- 对于合约调用，使用 `eth_estimateGas` 获取精确值
- MetaMask 在合约调用 revert 时会设置极高的 gas_limit，在 Monad 上这会导致高额费用

**3. Monad Testnet 网络配置**

| 参数 | 值 |
|------|-----|
| 网络名称 | Monad Testnet |
| Chain ID | 10143 |
| 货币符号 | MON |
| RPC URL | https://testnet-rpc.monad.xyz |
| 区块浏览器 | https://testnet.monadvision.com |
| 水龙头 | https://faucet.monad.xyz |
| 当前版本 | v0.14.5 / MONAD_NINE |

**4. Monad 已部署的 canonical 合约**

Monad Testnet 上已经部署了一些标准合约：

| 合约 | 用途 |
|------|------|
| Wrapped MON | MON 的 ERC-20 包装版本 |
| Multicall3 | 批量调用工具（地址与以太坊相同） |
| EntryPoint v0.6/0.7/0.8 | ERC-4337 账户抽象入口 |
| Permit2 | Uniswap 的通用授权合约 |
| Safe v1.4.1 | 多签钱包合约 |

**5. 公共 RPC 端点对比**

| 提供商 | 速率限制 | Batch 请求 | Archive 支持 |
|--------|----------|-----------|-------------|
| QuickNode | 50 rps | ✅ 100 | ✅ |
| Ankr | 300 reqs/10s | ✅ 100 | ❌ |
| Monad Foundation | 20 rps | ❌ | ✅ |

### 实操记录

**Step 1：获取测试币**

访问 https://faucet.monad.xyz，连接钱包后领取 MON 测试币。每个地址每天可领取一定数量的 MON。

**Step 2：在 Remix 中部署合约**

1. 打开 https://remix.ethereum.org/
2. 创建新文件 `MonadGreeter.sol`
3. 粘贴合约代码
4. 编译器选择 0.8.24
5. 点击 "Compile MonadGreeter.sol"
6. 在 Deploy 面板选择 "Injected Provider"（连接 MetaMask）
7. 确认 MetaMask 连接到 Monad Testnet
8. 输入构造函数参数（如 "gmonad"）
9. 点击 Deploy → MetaMask 确认

**Step 3：与合约交互**

部署成功后，在 "Deployed Contracts" 区域：
- 点击 `greeting` 按钮读取当前值 → 输出 "gmonad"
- 在 `setGreeting` 输入框输入新值 "gmonad molandak"
- 点击 transact → MetaMask 确认
- 再次点击 `greeting` → 输出 "gmonad molandak"

**Step 4：在区块浏览器查看**

在 MonadVision (https://testnet.monadvision.com) 搜索合约地址，可以查看：
- 合约创建交易
- 所有调用记录
- 状态变化

### 关键知识点对比

| 概念 | 以太坊 | Monad | 开发者影响 |
|------|--------|-------|-----------|
| 合约大小限制 | 24 KB | 128 KB | 可以写更复杂的合约 |
| Gas 收费方式 | 按实际消耗 | 按设置上限 | 必须精确设置 gas_limit |
| 出块时间 | 12 秒 | 1 秒 | 交易确认更快 |
| 编译器 | solc | 完全兼容 solc | 无需修改代码 |
| EVM 版本 | 最新 | 完全兼容 | 现有合约可直接部署 |
| 水龙头 | 各网络不同 | faucet.monad.xyz | 统一入口 |

### 今日思考

Solidity 的学习曲线比预期要平缓。核心概念就是状态变量、函数、事件、修饰符这几个。AI 工具在写合约时很有用，能快速生成模板代码，但需要人工审查安全性。

Monad 的合约大小限制放宽到 128 KB 是个大利好。在以太坊上，复杂的 DeFi 合约经常需要拆分成多个合约或使用代理模式来绕过 24 KB 限制。在 Monad 上，可以更自由地组织代码结构。

Gas 按 limit 收费的设计需要特别注意。这意味着在 Monad 上，"宁可少设不要多设"。如果合约调用可能 revert，设置过高的 gas_limit 会直接损失 MON。这对钱包开发和 DApp 前端都是新的挑战。

### 明日计划

1. 学习 Monad 的历史数据访问限制和索引器方案
2. 了解 Multicall3 在 Monad 上的使用
3. 尝试用 Hardhat 本地部署合约到 Monad Testnet
4. 学习 ERC-20 代币合约的基本实现
5. 整理 Week 1 的 Build Log
````
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->











\## Day 3 | Week 1 | 测试网初探 —— 连接 Monad Testnet，部署第一个智能合约 ### 今日学习内容 今天是 Monad Builder Camp 的第三天，主要学习了区块链测试网的概念与实操。测试网（Testnet）是开发者在正式部署到主网之前用来测试智能合约、调试 DApp、模拟交易的沙盒环境。它与主网的运行机制几乎完全一致，但使用的是没有实际价值的测试代币，因此开发者可以自由地进行各种实验而无需承担任何经济风险。 在今天的实操中，我主要完成了以下内容：在 MetaMask 中配置 Monad Testnet、通过水龙头获取测试代币、使用 Remix IDE 部署智能合约，以及通过区块浏览器查看链上数据。整个过程中，我深刻体会到了 Monad 相较于以太坊测试网（如 Sepolia）的显著优势——更快的出块速度、更低的交易延迟，以及更流畅的开发体验。 #### 什么是测试网？ 测试网是区块链网络的"影子版本"，它复制了主网的所有核心机制——包括共识协议、虚拟机执行、Gas 定价模型等——但运行在独立的链上，测试代币没有真实价值。常见的以太坊测试网包括 Sepolia 和 Holesky，而 Monad 则提供了自己的 Testnet 和 Tempnet 两种测试环境： - \*\*Testnet\*\*：主要测试环境，已部署数百个应用，于 2025-12-16 从创世区块重置，当前运行版本 v0.14.5 / MONAD\_NINE 修订版 - \*\*Tempnet\*\*：瞬态网络，会定期重置，用作新功能的沙盒环境，当前运行版本 v0.12.3 测试网的存在对于开发者至关重要。在我的 Web3 安全学习经验中，测试网是验证合约安全性的第一道防线——任何在主网上可能导致资金损失的漏洞，都应该在测试网阶段被发现和修复。 ### Monad 深度解析 #### Monad 核心架构 Monad 并非以太坊的简单分叉，而是一个从零开始用 C++ 和 Rust 构建的高性能 EVM 兼容 Layer 1 区块链。其核心创新体现在三个层面： \*\*1. MonadBFT 共识协议\*\* MonadBFT 是一种抗尾部分叉（tail-fork-resistant）的流水线化 BFT 共识协议，源自 HotStuff 的改进版本。它实现了： - \*\*单轮推测性最终性（Speculative Finality）\*\*：在一个共识轮次内即可获得推测性确认，两个轮次内实现完全最终性 - \*\*线性消息复杂度\*\*：正常运行路径下，消息和认证器复杂度为 O(n)，允许共识验证者集扩展到大量节点 - \*\*乐观响应性\*\*：轮次推进无需等待最坏情况的网络延迟，在常见情况和故障恢复中都是如此 - \*\*领导者故障隔离\*\*：单个故障领导者仅导致一次超时延迟，其他轮次可以尽可能快地推进 MonadBFT 的关键创新在于引入了 No-Endorsement 消息和 No-Endorsement Certificate（NEC）机制，这是一种轻量级的方式让验证者拒绝未充分传播的提案，而无需等待完整的超时。 \*\*2. 异步执行（Asynchronous Execution）\*\* 这是 Monad 最重要的架构创新之一。在以太坊中，执行与共识是交织在一起的（interleaved）——领导者必须先执行区块中的所有交易才能提出提案，验证节点也必须先执行交易才能投票。这意味着以太坊的执行时间预算极其有限：尽管出块时间为 12 秒，但 Gas 上限（3000 万 Gas）对应的执行时间预算仅约 100 毫秒，只占出块时间的 1%！ Monad 将共识与执行解耦，将执行移到共识的热路径之外，放入一个略有延迟的独立泳道。在 Monad 中，节点就交易的官方排序达成共识，而无需执行这些交易。这意味着执行可以获得完整的出块时间预算，大幅提升了执行吞吐量。 关键设计细节：由于执行滞后于共识，Monad 区块提案不包含状态树的 Merkle 根。作为预防措施，提案包含 \`D\` 个区块之前的延迟 Merkle 根（D 当前设为 3），允许节点检测是否发生了状态分歧。 \*\*3. 乐观并行执行（Optimistic Parallel Execution）\*\* Monad 使用乐观并行执行来处理交易。这意味着 Monad 会在早期交易完成之前就开始执行后续交易。如果并行执行的交易之间存在状态冲突（例如两个交易都读写同一个账户余额），Monad 会通过跟踪执行输入并与前序交易的输出进行比较来检测冲突，一旦发现不一致，就使用正确数据重新执行该交易。 虽然 Monad 以并行方式执行交易，但区块结果与以太坊完全一致——都是线性排序的交易集合，最终状态完全确定。这种设计借鉴了数据库领域的乐观并发控制（OCC）和软件事务内存（STM）技术。 \*\*4. MonadDb\*\* Monad 实现了自定义数据库 MonadDb，原生地在 SSD 上存储以太坊 Merkle Patricia Trie，优化了 I/O 性能。配合异步 I/O 技术，Monad 能够在通信进行时保持 CPU 的高效利用。 #### Monad Testnet 关键参数 | 参数 | 值 | |------|------| | Chain ID | 10143 | | 货币符号 | MON | | 出块时间 | ~1 秒 | | 吞吐量 | 10,000+ TPS | | 区块 Gas 上限 | 200M Gas | | 交易 Gas 上限 | 30M Gas | | 最低基础费 | 100 MON-gwei | | 最大合约代码大小 | 128 KB（以太坊为 24 KB） | | 最大初始化代码大小 | 256 KB（以太坊为 48 KB） | ### 实操记录 #### 第一步：MetaMask 配置 Monad Testnet 在 MetaMask 中添加 Monad Testnet，需要配置以下参数： | 配置项 | 值 | |--------|-----| | Network Name | Monad Testnet | | RPC URL | \`https://rpc.testnet.monad.xyz\`（Monad Foundation 官方端点，限速 20 rps） | | Chain ID | 10143 | | Currency Symbol | MON | | Block Explorer URL | \`https://monadscan.com\` 或 \`https://monadvision.com\` | 其他可用的公共 RPC 端点： - \*\*QuickNode\*\*：50 rps，支持批量请求（最多 100），支持 Archive 数据，\`eth\_call\` 和 \`eth\_estimateGas\` 限速 25 rps - \*\*Ankr\*\*：300 reqs/10s、12000 reqs/10min，批量请求最多 100，不支持 Archive 数据，\`debug\_\*\` 方法不可用 #### 第二步：获取测试代币 访问 Monad 官方水龙头：\`https://faucet.monad.xyz\`，连接钱包后即可领取测试用的 MON 代币。此外，Monad 生态还有丰富的测试代币，可在 \`https://github.com/monad-crypto/token-list/blob/main/tokenlist-testnet.json\` 查看完整列表。 #### 第三步：Remix 部署智能合约 通过 Remix IDE 在 Monad Testnet 上部署一个简单的 Greeting 合约： \`\`\`solidity // SPDX-License-Identifier: MIT pragma solidity ^0.8.24; contract Gmonad { string public greeting; constructor(string memory \_greeting) { greeting = \_greeting; } function setGreeting(string calldata \_greeting) external { greeting = \_greeting; } } \`\`\` \*\*部署步骤\*\*： 1. 打开 \`https://remix.ethereum.org/\`，创建新文件 \`Gmonad.sol\` 2. 粘贴上述代码，选择编译器版本 0.8.24 3. 在 "Deploy & run transactions" 标签页中，Environment 选择 "Injected Provider"（连接 MetaMask） 4. 确保 MetaMask 已切换到 Monad Testnet 网络 5. 在构造函数参数中输入初始问候语（如 "gmonad"），点击 Deploy 6. MetaMask 弹出确认窗口，点击确认 7. 部署成功后，可在 "Deployed Contracts" 区域调用 \`greeting()\` 读取和 \`setGreeting()\` 修改 #### 第四步：区块浏览器验证 部署完成后，可在两个区块浏览器上查看交易和合约： - \*\*MonadVision\*\*（由 BlockVision 提供）：\`https://monadvision.com\` - \*\*Monadscan\*\*（由 Etherscan 提供）：\`https://monadscan.com\` Monadscan 提供 Sourcify 和 Etherscan 两种验证器类型，方便开发者验证合约源码。此外还有详细的交易分析工具： - \*\*Tenderly Explorer\*\*：提供调用栈、余额变化、Gas 使用详情 - \*\*Blocksec Phalcon Explorer\*\*：提供调用栈、余额变化、资金流向分析 - \*\*JiffyScan\*\*：EIP-4337 UserOp 浏览器 ### 关键知识点对比 | 概念 | 以太坊（Sepolia） | Monad Testnet | 优势分析 | |------|-------------------|---------------|----------| | 共识机制 | Gasper（Casper FFG + LMD-GHOST） | MonadBFT | MonadBFT 实现单轮推测性最终性，延迟更低 | | 出块时间 | 12 秒 | ~1 秒 | Monad 快 12 倍，用户体验更流畅 | | 吞吐量 | ~15-30 TPS | 10,000+ TPS | Monad 高 300-600 倍，支持大规模应用 | | 执行模型 | 交织执行（共识与执行耦合） | 异步执行（共识与执行解耦） | Monad 执行可使用完整出块时间预算 | | 交易并行 | 串行执行 | 乐观并行执行 | Monad 充分利用多核 CPU | | 最大合约大小 | 24 KB | 128 KB | Monad 支持更复杂的合约逻辑 | | Gas 计费 | 按实际消耗（gas\_used） | 按 Gas 上限（gas\_limit） | Monad 防止异步执行下的 DOS 攻击 | | Mempool | 全局公开 mempool | 本地 mempool（仅转发给后续领导者） | Monad 降低带宽消耗，提升隐私性 | | Blob 交易 | 支持 EIP-4844 | 不支持 | Monad 专注于 L1 性能优化 | | 预编译 | 标准以太坊预编译 | 支持 secp256r1（P256）验证 | Monad 支持 WebAuthn/Passkey 签名验证 | | 客户端语言 | Go/Rust（Geth、Erigon 等） | C++ + Rust（从零构建） | Monad 针对性能深度优化 | | 数据库 | LevelDB/PebbleDB | MonadDb（自定义） | Monad 原生支持 SSD 上的 Merkle Patricia Trie | ### 代码片段 #### 并发交易提交（Monad 最佳实践） Monad 官方推荐的并发交易提交方式，充分利用其高吞吐量优势： \`\`\`typescript import { createWalletClient, http, parseEther } from 'viem'; import { monadTestnet } from 'viem/chains'; const walletClient = createWalletClient({ account: ACCOUNT, chain: monadTestnet, transport: http('https://rpc.testnet.monad.xyz'), }); // ❌ 以太坊式串行提交（低效） for (let i = 0; i < 10; i++) { const txHash = await walletClient.sendTransaction({ to: recipientAddress, value: parseEther('0.1'), gas: BigInt(21000), maxFeePerGas: BigInt(50000000000), nonce: nonce + i, }); } // ✅ Monad 最佳实践：并发提交 const BATCH\_SIZE = 10; const txPromises = Array(BATCH\_SIZE).fill(null).map(async (\_, i) => { return await walletClient.sendTransaction({ to: recipientAddress, value: parseEther('0.1'), gas: BigInt(21000), maxFeePerGas: BigInt(50000000000), nonce: nonce + i, }); }); const hashes = await Promise.all(txPromises); \`\`\` #### Multicall3 批量读取 Multicall3 已部署在 Monad Testnet 地址 \`0xcA11bde05977b3631167028862bE2a173976CA11\`，可将多个 \`eth\_call\` 合并为一次调用： \`\`\`typescript import { createPublicClient, http } from 'viem'; import { monadTestnet } from 'viem/chains'; const publicClient = createPublicClient({ chain: monadTestnet, transport: http('https://rpc.testnet.monad.xyz'), }); // 使用 viem 的内置 multicall 支持 const results = await publicClient.multicall({ contracts: \[ { address: tokenA, abi: erc20Abi, functionName: 'balanceOf', args: \[walletAddress\] }, { address: tokenB, abi: erc20Abi, functionName: 'balanceOf', args: \[walletAddress\] }, { address: tokenA, abi: erc20Abi, functionName: 'allowance', args: \[walletAddress, spender\] }, \], }); // 一次 RPC 调用获取三个数据点，减少延迟 \`\`\` #### Envio HyperIndex 配置示例 Monad 官方推荐使用索引器替代反复调用 \`eth\_getLogs\`，以下是 Envio 的配置： \`\`\`yaml name: monad-token-tracker networks: - id: 10143 # Monad Testnet start\_block: 0 contracts: - name: Gmonad address: - 0xYourContractAddress handler: src/EventHandlers.ts events: - event: GreetingChanged(string oldGreeting, string newGreeting) \`\`\` ### 今日思考 #### 1. 异步执行的安全启示 从 Web3 安全的角度来看，Monad 的异步执行模型带来了新的安全考量。由于 Monad 按 gas\_limit 而非 gas\_used 计费，开发者必须精确设置 Gas 上限。如果使用 MetaMask 等钱包的 \`eth\_estimateGas\` 估算，在合约调用 revert 时，某些钱包会将 Gas 上限设为极高值——这在以太坊上影响不大（因为只收取实际消耗），但在 Monad 上会导致用户被收取大量费用。这是一个典型的"链间行为差异导致安全问题"的案例，提醒我们在开发跨链 DApp 时必须深入了解每条链的特有行为。 #### 2. Reserve Balance 机制与 MEV Monad 的 Reserve Balance 机制确保共识中包含的交易都能被支付。这意味着在 Monad 中可能会看到一些在以太坊上不会出现的"包含但执行失败"的交易——这些交易仍然消耗 Gas 并被视为有效交易。这对 MEV 策略和套利机器人的设计有重要影响：在 Monad 上，即使交易最终 revert，它仍然占据区块空间并支付 Gas 费用。 #### 3. 无全局 Mempool 的影响 Monad 没有全局 mempool，交易仅被转发给后续几个领导者。这一设计降低了带宽消耗并提升了交易隐私性，但对 MEV 搜索者（Searcher）提出了新的挑战——传统的 mempool 监控策略在 Monad 上不再适用。开发者需要采用不同的方式来发现和捕获 MEV 机会。 #### 4. 与已有 Web3 安全知识的关联 在 Web3 安全学习中，我了解到测试网是合约安全审计的第一站。Monad Testnet 的高吞吐量特性意味着可以更高效地进行模糊测试（Fuzzing）和形式化验证——相同时间内可以执行更多交易，更快地发现边界情况下的漏洞。结合 Echidna、Slither、Mythril 等安全工具，Monad 测试网将成为智能合约安全研究的理想平台。 #### 5. 踩坑记录 - 在配置 RPC 时，如果使用了不支持 Archive 数据的端点（如 Ankr），某些历史查询可能会失败 - Remix 部署时需要注意选择正确的编译器版本（0.8.24），否则会出现红色波浪线警告 - 由于 Monad 按 gas\_limit 计费，在测试合约时应避免设置过高的 Gas 上限，以免浪费测试代币 ### 明日计划 1. \*\*学习 Solidity 基础语法\*\*：变量类型、函数修饰符、事件、继承等 2. \*\*在 Monad Testnet 上部署更复杂的合约\*\*：尝试包含状态变量、映射、事件的合约 3. \*\*学习 Foundry 测试框架\*\*：了解 Monad Foundry 的定制版本，掌握本地测试流程 4. \*\*深入研究 Monad 的 Gas 定价机制\*\*：理解 EIP-1559 兼容的 base\_price\_per\_gas 动态控制器 5. \*\*探索 Monad 生态工具\*\*：Allium 数据分析、thirdweb Insight API 等 ### 参考资料 - \[Monad Testnet 网络信息\](https://docs.monad.xyz/developer-essentials/testnets) - \[Monad vs Ethereum 区别\](https://docs.monad.xyz/developer-essentials/differences) - \[Monad 高性能最佳实践\](https://docs.monad.xyz/developer-essentials/best-practices) - \[Monad Gas 定价\](https://docs.monad.xyz/developer-essentials/gas-pricing) - \[MonadBFT 共识协议\](https://docs.monad.xyz/monad-arch/consensus/monad-bft) - \[异步执行详解\](https://docs.monad.xyz/monad-arch/consensus/asynchronous-execution) - \[乐观并行执行\](https://docs.monad.xyz/monad-arch/execution/parallel-execution) - \[Remix 部署指南\](https://docs.monad.xyz/guides/deploy-smart-contract/remix) - \[Web3 实习手册 - 安全与合规\](https://web3intern.xyz/zh/security/) <!-- DAILY\_CHECKIN\_2026-07-08\_END `-`
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->












| 维度 | EOA（普通钱包） | 智能账户（Smart Account / AA） | 多签账户（Multisig） |
| --- | --- | --- | --- |
| 谁持有控制权 | 单个私钥持有者 | 用户 + 智能合约规则 | 多个签名人共同控制 |
| 谁可以发起交易 | 私钥持有者直接发起 | 用户提交请求，由合约校验执行 | 任意签名人可发起提案 |
| 是否支持多人确认 | ❌ 不支持 | ✅ 可配置 | ✅ 天然支持 |
| 是否支持恢复机制 | ❌ 基本不支持 | ✅ 支持社交恢复、设备恢复 | ⚠️ 取决于签名人是否仍可访问 |
| 是否支持限额/自动化策略 | ❌ 不支持 | ✅ 支持日限额、自动扣费、Gas 代付等 | ⚠️ 可部分实现，但不灵活 |
| 是否支持程序化权限控制 | ❌ 不支持 | ✅ 核心能力 | ⚠️ 仅支持签名阈值类控制 |
| 使用门槛 | 低 | 中 | 中高 |
| 用户体验 | Web3 原生 | 更接近 Web2 | 偏企业/组织化 |
| 典型使用场景 | 普通用户钱包 | 链游、支付、AA 钱包、Web2 onboarding | DAO 国库、项目资金管理、团队钱包 |
| 主要风险点 | 私钥泄露即资产丢失 | 合约漏洞或权限配置错误 | 多签人串通 / 私钥同时丢失 |
| 常见代表 | MetaMask | ERC-4337 | Safe |
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
