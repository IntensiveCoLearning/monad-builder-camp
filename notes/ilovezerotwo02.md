---
timezone: UTC+8
---

# 希羽

**GitHub ID:** ilovezerotwo02

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
Dapp 的架构主要由三个核心部分组成：

**1\. 前端（User Interface）：**

-   前端是 DApp 与用户交互的界面，通常由 HTML、CSS 和 JavaScript（如 React、Vue 等框架）构建。与传统 Web 应用不同，DApp 前端不会直接连接区块链网络，而是通过 钱包注入的 Provider 或第三方 RPC 节点 与区块链交互：
    
    -   通过 RPC 节点对 智能合约发起只读调用（如 eth\_call），获取合约状态、事件日志等链上数据
        
    -   对需要修改状态的操作，由前端构造对 智能合约的交易调用，交由钱包完成签名后，再通过 RPC 节点广播到区块链网络并最终上链执行
        
-   前端还需要集成区块链钱包（如 MetaMask）来进行身份验证和签署交易，确保用户的隐私和安全。
    

**2\. 智能合约（Smart Contracts）：**

-   智能合约是 Dapp 的核心，它定义了应用的业务逻辑，并部署在区块链上。智能合约通过执行自动化的规则来确保交易和操作的透明性与不可篡改性。
    
-   在以太坊平台上，智能合约通常使用 **Solidity** 编程语言编写，并通过 **Ethereum Virtual Machine (EVM)** 执行。
    

**3\. 数据检索器（Indexer）：**

-   智能合约通常以 `Event` 形式释放日志事件，比如释放代表 NFT 转移的 `Transfer` 事件，数据检索器会检索这些数据并将其写入到 PostgreSQL 等传统数据库中
    
-   Dapp 在前端进行数据展示时需要检索器内的数据。一个简单的示例是某 NFT 项目需要展示用户持有的所有 NFT，但是 NFT 合约并不会提供通过输入地址参数返回该地址下的所有 NFT 的函数，此时我们可以运行数据检索器将 `Transfer` 事件读取后写入传统数据库内，前端可以在传统数据库内检索用户持有的 NFT 数据
    

**4\. 区块链和去中心化存储（Blockchain & Decentralized Storage）：**

-   区块链用于存储智能合约的状态数据及交易记录。去中心化存储如 **IPFS**（InterPlanetary File System）或 **Arweave**，用于存储大规模的非结构化数据（如图片、文档等），确保数据不易丢失和篡改。
    
-   通过使用去中心化存储，Dapp 确保所有数据在多个节点上备份，保证数据的持久性和去中心化特性。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->

## **Web3 安全意识**

新人很容易被钓鱼网站骗，尤其是空投、领福利、钱包授权类网站。

常见风险：

-   点陌生链接
    
-   连接钱包
    
-   授权恶意合约
    
-   钱包资产被自动转走
    
-   U 卡 / 礼品卡被盗
    
-   交易所、邮箱、身份信息被关联攻击
    

所以新人必须注意：

**陌生链接不要点，钱包不要乱连，授权一定要看清楚。**

Web3 里钱一旦被链上转走，追回非常困难。

## **这个项目为什么要用区块链**

有人问：这个项目用互联网也能做，为什么要用区块链？

可以理解为，区块链在这里不是为了炫技，而是用于：

-   支付流程
    
-   链上存证
    
-   订单记录
    
-   权限审批
    
-   透明履约
    
-   多方共享可信数据
    

不过这也提醒一个点：

**Web3 项目不能为了上链而上链，必须说明区块链解决了什么现实问题。**
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->


AI Agent 支付基础设施的架构、落地进展、生态布局及行业趋势展开分享答疑，核心内容如下：

-   **Agent 支付核心架构**
    
    -   **全链路能力分层**
        
        -   接入层：支持 Cloud、GPT、LangChain 等各类 Agent 载体对接，用户仅需输入简单 Prompt 即可触发 Agent 完成全流程操作。
            
        -   核心能力层：拆解为身份标识、预算管控、风控校验、交易可审计、可撤销五大模块，覆盖支付全链路需求。
            
    -   **核心技术模块**
        
        -   人机共管钱包：采用人机共管模式，无需逐笔交易人工审批，支持直接操作撤销，避免传统方案需修改代码的繁琐问题。
            
        -   X402 支付协议：基于 HTTP 402 原生支付协议优化，仅需初始授权即可实现 Agent 全链路自主支付，无需人工干预。
            
        -   批量结算协议：推出嵌入式多结算节点授权协议，预授权后预算内交易无需逐笔审批，类 Layer2 批量结算提升微支付效率。
            
        -   风控约束框架：配套 Policy Engineering 风控引擎，严格管控 Agent 幻觉带来的超支风险，满足传统金融机构的合规要求。
            
    
    ![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OWQ1YjM5OWViZGM5YTFhMTk2ZWYzZGZiZjNhYjhjYzNfRUhyNHV0dVc4UzVJOTB6amtNTkJoNU9XaWN4c0JuUldfVG9rZW46TlYwcWJ0SU5tb09iaG94NXlUaGMwTTFpbk5nXzE3ODM1MTYwNDk6MTc4MzUxOTY0OV9WNA&add_watermark=true&scene_type=CCM_DOUBAO)
-   **核心产品落地进展**
    
    -   **Agent 卡系列产品**
        
        -   一次性 Agent 卡：预设固定预算额度，单次使用后自动作废，可适配订票等场景的风控需求，当前已完成试点跑通。
            
        -   长期 Agent 卡：与 VISA 联合开发的长期可用 Agent 卡已完成底层适配，即将正式面向用户推出，可支持 Agent 持续自主支付。
            
    -   **三类支付解决方案**
        
        -   开发者货币化方案：提供无代码上架工具，开发者无需技术基础即可将 API、MCP、Skill 转化为可被 Agent 自动调用的商品。
            
        -   通用电商支付方案：针对金额不固定的零售场景，自动生成支付链接，付款方仅需一键确认即可完成交易。
            
        -   Agent 点对点转账：基于 Agent 唯一 ID 直接完成转账操作，适配 Agent 之间自主发起的交易需求。
            
        
        ![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTVmZWI1N2E2NjdmNTBhMjcxNmJhMWExOWQzMjJiNDBfV0dBSlZ5Z0t6UUZwcEc3cTN1Tm5weGg0bllwdVNSVGNfVG9rZW46T0E4TmJKZ2Myb2owYVV4aEZ6Y2NuYU1ublJiXzE3ODM1MTYwNDk6MTc4MzUxOTY0OV9WNA&add_watermark=true&scene_type=CCM_DOUBAO)
    -   **资产与充值支持**
        
        -   链上资产：当前主落地场景为 Base 链 USDC 结算，后续将根据生态需求，支持其他公链及跨链操作。
            
        -   法币通道：支持通过 Stripe 完成法币充值，兑换为平台内部凭证用于交易，暂不支持法币直接由 Agent 调用。
            
-   **已落地场景案例**
    
    -   **社交场景试点**：春节期间上线 Agent 社交产品「Cloud 派」，支持 Agent 自动识别并完成抢红包操作，适配国内用户春节场景，上线后参与度较高。
        
    -   **百度生态合作**：承接百度生态内各类 AI 资产、OPC 项目的上架与货币化，联动双方资源做全球分发，收益归属对应开发者。
        
    
    ![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NmNiMzk0YTgzOTA0NDMxMjkyZTUyODQ2NDkxOWU1Y2VfOUhNUUdETE5FcWNNdjZxV3pmNlY1c0VNamMzZDZiaGhfVG9rZW46UjAwY2JmTldOb1Y5YXR4MXRiU2NhNkRRbktoXzE3ODM1MTYwNDk6MTc4MzUxOTY0OV9WNA&add_watermark=true&scene_type=CCM_DOUBAO)
    -   **多场景验证**：已落地 Agent 代订票务、行业调研 Agent Skill 等试点，用户可通过付费调用对应能力获取票务、调研成果等服务。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->



1\. 创建或准备一个**课程专用钱包**，不要使用主力钱包。  
2\. 添加 Monad Testnet 网络。  
3\. 打开 Monad Explorer 或相关区块浏览器，确认可以查询地址。

理解ethereum、协议资料、客户端、go客户端、数据和证明 EIP
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->




了解基础概念

-   `区块链`：一种分布式账本，很多节点共同保存同一份交易记录。为什么需要：不依赖单一公司或服务器，数据更难被篡改，任何人都能验证。
    
-   `区块`：把某一小段时间内的交易打包成一个“数据包”，再按顺序链接到前一个区块后面。为什么需要：方便统一排序、验证和追溯历史。
    
-   `共识`：让全网节点对“哪一批交易有效、顺序是什么”达成一致的规则。为什么需要：没有中心管理员时，必须靠共识避免双花、分叉和账本不一致。
    
-   `合约 / 智能合约`：部署在区块链上的程序，满足条件就自动执行。为什么需要：把“规则”写进代码里，减少中介，适合做转账、交易、借贷、NFT、游戏等。
    
-   `Web3`：基于区块链的钱包、资产、合约和去中心化应用生态。为什么需要：让用户自己持有账号和资产，不完全依赖平台。
    
-   `吞吐量`：系统单位时间能处理多少工作量。区块链里常看交易数、gas/s、数据量。为什么需要：吞吐量越高，越能支撑更多用户和应用。
    
-   `TPS`：`Transactions Per Second`，每秒交易数，是吞吐量最常见的表示。为什么需要：便于直观看链快不快、能不能承载大规模使用。
    
-   `出块时间`：平均多久产生一个新区块。为什么需要：出块越快，用户越快看到交易被打包；但它不等于最终确认。
    
-   `最终确认 / Finality`：交易基本不可回滚的时间。为什么需要：支付、交易所、DeFi 更关心“多久真的稳了”，不只是“多久出块”。
    

**Monad 是什么**

-   `Monad`：一个 `EVM 兼容` 的高性能 `Layer 1` 区块链。官方文档当前写的是大约 `10,000 TPS`、`400ms 出块`、`800ms finality`。为什么需要：它想解决传统 EVM 链“兼容性强但性能受限”的问题，让以太坊开发者不用换技术栈，也能获得更高性能。
    
-   这里的 `Monad` 指区块链项目，不是函数式编程里的 `monad` 概念。
    

**Monad 术语**

-   `MonadBFT`：Monad 的共识机制，属于 BFT 类共识。简单说，就是验证者投票，超过 2/3 就能形成有效确认。为什么需要：保证分布式环境下大家对同一条链达成一致，而且能抗一部分恶意节点。
    
-   `RaptorCast`：Monad 用来高效传播区块的网络广播方案。为什么需要：区块越大、节点越多，传播就越容易变慢；RaptorCast 是为“快且大”的区块传播服务的。
    
-   `Asynchronous Execution / 异步执行`：先对交易顺序达成共识，再执行，不把执行卡在共识主路径里。为什么需要：把共识和执行“流水线化”，提高利用率，让执行有更充足的时间预算。
    
-   `Optimized Asynchronous Execution`：你图里 `区块 N、N+1、N+2` 的意思就是，后面的区块在共识推进时，前面的区块还可以并行执行。为什么需要：减少“等前一步做完才能做下一步”的空转时间。
    
-   `Optimistic Parallel Execution / 乐观并行执行`：先假设多笔交易彼此不冲突，放到多核并行跑；如果发现读写冲突，再重跑冲突交易。为什么需要：现代 CPU 是多核的，不并行就浪费硬件性能。
    
-   `JIT Compilation`：`Just-In-Time` 即时编译，把热点合约编译成本地机器码并缓存。为什么需要：比一直用解释器逐条执行 EVM 指令更快，特别适合高频调用的合约。
    
-   `MonadDB`：Monad 自研的状态数据库，专门为区块链状态读写优化。为什么需要：链上执行常常卡在“读状态、写状态”，数据库是瓶颈，优化它能明显提速。
    
-   `128KB 合约`：Monad 把合约大小上限提得更高。为什么需要：复杂应用可以少拆分合约、少走代理，开发和调用都更直接。
    
-   `eth_sendRawTransactionSync`：Monad 的 RPC 接口之一，发送交易后同步返回更完整的结果。为什么需要：前端和应用不用一直轮询，交互体验更接近“即时响应”。
    
-   `执行事件 / Execution Events`：这是 Monad 的高性能实时数据流，不等同于 Solidity 的 `event`。为什么需要：高频应用、索引器、做市程序需要更低延迟、更高吞吐的数据接入方式。
    
-   `MIP-8`：我在 `2026-07-06` 查到的官方资料里，它是一个 `draft proposal`，核心是让连续存储槽访问更便宜。为什么需要：很多合约会连续读取状态，如果状态访问更符合底层存储特性，gas 和性能都能优化。
    
-   `Gas`：链上计算和存储的“资源计价单位”。为什么需要：防止有人无限占用网络资源，也让不同操作有明确成本。
    
-   `冷槽访问 / cold storage access`：第一次读取某个还没被访问过的存储槽，成本通常更高。为什么需要这个设计：访问真实状态有代价，计费需要反映资源消耗。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
