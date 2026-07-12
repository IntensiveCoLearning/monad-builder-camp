---
timezone: UTC+8
---

# AuralRain

**GitHub ID:** AuralRain

**Telegram:** 

## Self-introduction

Java后端开发工程师

## Notes

<!-- Content_START -->
# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
### 1\. 基础设施与成本控制

### 选择合适的Web托管服务，避免因流量超出套餐额度而产生意外高额费用。

-   警惕“诱饵”定价：许多服务商（如 Vercel、Railway）的低价套餐包含一定流量额度，但超额部分单价可能极高。务必提前计算成本（例如，$20/月套餐含1TB流量，超额$0.20/GB，则第二个TB实际成本为$200）。
    
-   生产环境推荐 (AWS)：
    
    -   静态文件与CDN：Amazon S3 + CloudFront
        
    -   无服务器函数：AWS Lambda
        
    -   容器化应用：Amazon ECS 或 EKS
        
    -   数据库：Amazon RDS
        
    
    > ### 此组合能提供更精细的成本控制和更好的高流量扩展性。
    

### 2\. 优化链上交互与RPC调用

### 2.1 使用硬编码Gas值

-   原则：若操作的Gas消耗固定（如原生代币转账恒为21,000 Gas），应直接硬编码Gas值。
    
-   优势：
    
    -   省去 `eth_estimateGas` 的RPC请求，大幅提升钱包操作速度。
        
    -   避免某些钱包在 `eth_estimateGas` 调用失败时的异常行为。
        

### 2.2 减少 `eth_call` 延迟

-   核心方法：并发处理多个请求，避免串行导致的多次网络往返延迟。
    

| 方案 | 实现方式 | 适用场景 | 注意事项 |
| --- | --- | --- | --- |
| Multicall合约 | 使用标准 Multicall3 合约（已部署于Monad主网/测试网，地址：0xcA11bde05977b3631167028862bE2a173976CA11），将多个读请求聚合为单次 eth_call。 | 同时获取多个独立数据点（如余额、授权额、多项参数）。 | 调用串行执行，避免放入过多或过昂贵的调用。 |
| RPC请求批处理 | 利用库（如 viem）的批量请求功能，将多个RPC请求合并成一条消息。 | 需要并发执行多个独立RPC调用。 | RPC服务端可并行处理，效率更高。 |
| 自定义合约 | 部署定制合约，将复杂的数据读取逻辑聚合在一个函数中。 | 标准Multicall难以处理的复杂读取模式。 | 合约内读取逻辑仍为串行。 |

### 2.3 使用索引器替代 `eth_getLogs`

### 对于频繁查询历史事件或派生状态的应用，强烈建议使用专业索引服务，而不是重复调用 `eth_getLogs`。文档列举了以下主流方案：

-   Allium：提供SQL访问（Explorer）、实时数据流（Datastreams）和钱包活动API。
    
-   Envio HyperIndex：通过配置文件（指定网络ID `10143` 测试网 / `143` 主网）和事件处理器快速创建索引器。
    
-   GhostGraph：详见官方文档。
    
-   Goldsky：支持子图（Subgraph）和直接数据流（Mirror），配置中网络标识为 `monad-mainnet` 或 `monad-testnet`。
    
-   QuickNode Streams：在仪表盘创建数据流，或通过REST API管理，支持Webhook、S3等目标。
    
-   The Graph：使用子图（Subgraph），网络标识为 `monad-mainnet` 或 `monad-testnet`。
    
-   thirdweb Insight API：REST API，使用链ID `143`（主网）或 `10143`（测试网）。
    

### 3\. 交易发送与管理

### 3.1 本地管理Nonce

-   场景：若手动设置nonce，且短时间内从同一钱包发送多笔交易。
    
-   实践：实现本地nonce跟踪，避免每次都调用 `eth_getTransactionCount` 产生网络请求。
    

### 3.2 并发提交交易

-   原则：发送多笔交易时，采用并发提交（如 `Promise.all`）替代串行（`for`循环 `await`）。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->

### 1\. 虚拟机 (VM) 差异

-   合约代码大小：
    
    -   最大合约代码大小：128 KB (以太坊为 24 KB)。
        
    -   最大初始化代码大小：256 KB (以太坊为 48 KB)。
        
-   操作码与预编译重定价：部分操作码和预编译合约的Gas成本已重新调整，以反映Monad优化下的资源稀缺性。
    
-   新预编译合约：支持 `secp256r1` (P256) 验证预编译（地址 `0x0100`），支持在链上验证 WebAuthn/passkey 签名（参见 [EIP-7951](https://eips.ethereum.org/EIPS/eip-7951)）。
    

### 2\. 交易 (Transaction) 差异

-   Gas计费方式：按Gas上限扣费，而非实际使用量。即扣除总额为 `交易价值 + gas价格 * gas上限`。这是为适应异步执行而采取的DoS防护措施。
    
-   储备余额 (Reserve Balance) 机制：
    
    -   共识与执行层使用该机制，确保所有被包含的交易都能被支付。
        
    -   它限制了交易在共识阶段的包含，并定义了交易在执行阶段会回退 (revert) 的条件（如账户余额不足以支付最大可能费用）。
        
    -   注意：您可能会看到因尝试花费超过账户余额的MON而最终失败（回退） 的交易，但这些交易仍会支付Gas费。这与以太坊上包含许多回退交易的情况类似，但回退原因可能不同。
        
-   不支持EIP-4844：交易类型3（即携带Blob数据的交易）在Monad上不受支持。
    
-   无全局Mempool：为提高效率，交易会直接转发给接下来的几个区块提议者（领导者），而非广播至全局内存池（详见“本地Mempool”机制）。
    

### 3\. EIP-7702 委托 (Delegation) 差异

### 当外部账户（EOA）设置了EIP-7702委托代码时：

-   最低余额限制：该账户的余额不能低于 10 MON（受储备余额规则约束）。但若移除委托，则允许余额低于此阈值。
    
-   创建操作码被禁：当该账户被作为合约调用时，`CREATE` 和 `CREATE2` 操作码被禁用。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->


Monad与以太坊虚拟机在字节码层面完全兼容。以太坊上的智能合约可以直接部署到Monad上，现有的Solidity、Vyper等编程语言也全部支持，可以继续使用已有钱包（如MetaMask）、开发框架和RPC API接口，获得性能提升。

Monad的性能：10,000 TPS（每秒交易数）、400毫秒的出块时间和800毫秒的交易最终性。这比以太坊主网快了数个量级。

确保节点能快速读取和写入数据。

思考：想要利用Monad的优势：关键在于寻找那些对高频、低延迟、大规模并发交互有刚性需求的场景。

目前 Monad 生态中最活跃、也最契合其性能优势的方向主要有三个：高频 DeFi、全链上游戏，以及 AI 智能体经济  
传统链游大多只有资产上链，而 Monad 的高性能让更复杂的游戏逻辑直接在链上执行成为可能。这会带来几个关键变化：

大规模并发交互：游戏玩家和 NFT 市场往往需要高频交易与交互，高 TPS 网络可避免用户体验因链上滞后而受阻。游戏中的每一次攻击、移动、交易都可以是链上交易，承载数千玩家同时互动。

低费率促进玩法创新：低于以太坊的手续费可以降低用户尝试新游戏和交易 NFT 的门槛，有利于吸引更广泛的用户群。

真正链上资产与经济系统：游戏内资产和 NFT 形成链上经济，利用高速确认促进道具流通。这意味着游戏规则完全透明、资产真正属于玩家，并且可以开放给任何人基于游戏逻辑做二次开发。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->



合约源码 (Source Code)

Solidity/Vyper 代码（`.sol` 文件）。

用来编译生成 ABI 和 Bytecode（字节码）。

ABI (Application Binary Interface)

JSON 格式的接口描述文件（包含函数名、参数类型、返回值类型）。

ABI 由源码编译生成。ABI 不是合约地址的一部分，它是链下（Off-chain）的工具。前端（DApp）需要 ABI 才能知道如何格式化数据，去调用链上的合约。

合约地址 (Contract Address)

一串 `0x...` 哈希值，部署时根据你的钱包地址和 Nonce 计算得出。

合约地址是链上（On-chain）的唯一标识。地址 + ABI = 可用的 API

Read / Write Function

Read (Call)：读取数据（如查询余额），不消耗 Gas，不需要签名。Write (Send)：写入数据/修改状态（如转账），消耗 Gas，需要私钥签名。

Transaction Hash (交易哈希)

当你发送一笔 Write 交易后，生成的唯一 `0x...` 哈希值。

完成应用的前端开发，并进行完整的交易流程

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/AuralRain/images/2026-07-09-1783610122005-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/AuralRain/images/2026-07-09-1783610208065-image.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->




Transaction Hash: 交易唯一标识

Status: 交易是否成功

Block: 所在区块号

Timestamp: 交易时间

From: 发送方地址

To: 接收方地址（合约地址或普通地址）

Value:转账金额

Transaction Fee: 交易手续费

Gas Price: Gas 单价

Gas Limit & Usage: Gas 限制和使用量

完成留言板的信息交互测试

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/AuralRain/images/2026-07-08-1783522498835-image.png)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->





**Developer Relations（开发者关系）** 是连接**技术生态**与**开发者社区**的桥梁角色，核心使命是**让更多开发者为某一技术栈或生态持续构建应用**。

### **核心职能**

| 职能维度 | 具体工作内容 | 产出形式 |
| --- | --- | --- |
| 技术赋能 | 撰写文档、教程、入门指南 | 技术博客、视频课程、代码示例库 |
| 社区运营 | 维护 Discord/Telegram 社群、解答技术问题 | 活跃的开发者社区、FAQ 文档 |
| 活动组织 | 举办黑客松、线上 Workshop、线下 Meetup | 赛事、工作坊、技术沙龙 |
| 开发者营销 | 在技术大会演讲、撰写案例研究 | 演讲 PPT、成功案例白皮书 |
| 产品反馈 | 收集开发者痛点，向内部产品团队传递需求 | 需求文档、用户调研报告 |
| 生态增长 | 设计激励计划（Grant、赏金任务） | 资助项目、生态基金分配 |

### **Builder 的五大类型**

| 类型 | 核心能力 | 典型产出 | 示例角色 |
| --- | --- | --- | --- |
| 技术型 Builder | 编写智能合约、前端/后端开发 | DApp、协议、基础设施工具 | Solidity 开发者、全栈工程师 |
| 产品型 Builder | 产品设计、路线图规划、用户体验 | 产品原型、功能迭代方案 | 产品经理、创业者 |
| 社区型 Builder | 社群运营、内容创作、活动策划 | 活跃社区、教育内容、线下活动 | 社区经理、KOL、内容创作者 |
| 研究型 Builder | 经济模型设计、数据分析、尽职调查 | Tokenomics 白皮书、研究报告 | 研究员、数据分析师 |
| 设计型 Builder | UI/UX 设计、品牌视觉、NFT 艺术 | 界面设计、品牌手册、数字艺术 | 设计师、艺术家 |

完成vibecoding工具部署

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/AuralRain/images/2026-07-07-1783437030627-image.png)

生成智能合约留言板代码

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/AuralRain/images/2026-07-07-1783438678990-image.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->






| 概念 | 技术定义与核心特征 |
| --- | --- |
| 1. 区块链 | 由区块通过哈希值首尾相连形成的分布式数据库。核心特征：去中心化、不可篡改、公开透明。 |
| 2. 智能合约 | 部署在区块链上的不可更改的程序。当预设条件满足时，自动执行逻辑（如：A给B转钱，条件达成则放行）。它是DApp的后端逻辑。 |
| 3. 去中心化应用（DApp） | 前端（网页/手机端） + 后端（智能合约）。用户通过钱包连接，所有交互数据都记录在链上。 |
| 4. 钱包 & 私钥 | 钱包是管理私钥的工具。私钥是一串随机生成的数字字母，代表链上资产的唯一控制权。丢失即永久丢失资产，无找回机制。 |
| 5. Gas费（矿工费） | 执行智能合约或转账时支付给区块链网络的计算费用。读取数据免费，写入数据付费。费用随网络拥堵情况浮动。 |

* * *

### 去中心化让数据所有权回归到个人  

完成钱包的创建并获取免费MON

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/AuralRain/images/2026-07-06-1783352518893-image.png)
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
