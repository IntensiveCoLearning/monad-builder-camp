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
