---
timezone: UTC+8
---

# yaoyaodata

**GitHub ID:** yaoyaodata

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
Part 1 创建钱包+领取测试币

![Screenshot of Testnet 2026-07-07 at 02.06.33.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yaoyaodata/images/2026-07-08-1783520129398-Screenshot_of_Testnet_2026-07-07_at_02.06.33.png)

Part 2 ai agent支付

**AI Agent 支付 = 区块链钱包（作为账户）+ 稳定币（作为货币）+ 账户抽象协议（作为授权逻辑）+ Web2 金融公司（作为合规与落地管道）**。

-   **x402 协议 (HTTP 402 Payment Required)**：这是目前最成熟的 Agent 支付标准。由 Coinbase 等机构推动，利用 HTTP 协议中的 402 状态码，当 Agent 请求某个数据或服务时，服务端返回 402 要求付费，Agent 自动通过钱包支付稳定币（如 USDC），随后即可获取资源。
    
-   **ERC-4337 (账户抽象)**：这是 Agent 实现“自主支付”的核心技术。
    
    -   **会话密钥 (Session Keys)**：用户可以在智能合约层面为 Agent 设置限额（例如：每天允许支出 100 USDC，仅限 API 服务购买）。Agent 持有临时签名权，无需用户对每笔交易手动确认。
        
-   **A2A (Agent-to-Agent) 协议**：正在由 Google、Salesforce 等推行，旨在定义一套标准化的沟通模式，让不同的 Agent 能够发现服务、协商价格并完成结算。
    

-   **sa/Mastercard 模式**：Visa 推出的 `Intelligent Commerce` 等解决方案，本质上是**将 Web2 的支付网关与 Web3 的技术进行“桥接”**。他们并不要求 Agent 去拿银行牌照，而是通过与支付基础设施商（如 Stripe, MoonPay）合作，让 Web2 的商家能接受 Agent 的支付，同时提供合规保障。
    
-   **这种方式的逻辑**：银行和支付巨头提供“受监管的合规通道”，确保 Agent 的支付行为不会触碰洗钱或欺诈红线。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

今天学习到了一些新知识(^-^)

Part 1 Monad

**monad: 兼容evm的高性能链 和其他链的区别如下👇**

![Screenshot 2026-07-07 at 03.37.02.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yaoyaodata/images/2026-07-06-1783359441436-Screenshot_2026-07-07_at_03.37.02.png)

Part 2 智能合约开发

-   Dapp架构：
    
    1.  前端：不会直接连接区块链网络，而是通过“钱包注入的 Provider 或第三方 RPC 节点”与区块链交互：
        
        -   通过 RPC 节点对 智能合约发起只读调用（如 eth\_call），获取合约状态、事件日志等链上数据
            
        -   对需要修改状态的操作，由前端构造对 智能合约的交易调用，交由钱包完成签名后，再通过 RPC 节点广播到区块链网络并最终上链执行
            
        -   前端还需要集成区块链钱包来进行身份验证和签署交易
            
    2.  智能合约部署
        
    3.  数据检索器
        
    4.  区块链和去中心化存储（IPFS or Arweave）
        

![Screenshot 2026-07-07 at 03.48.28.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yaoyaodata/images/2026-07-06-1783360120290-Screenshot_2026-07-07_at_03.48.28.png)

3.  合约结构 [https://web3intern.xyz/zh/smart-contract-development/](https://web3intern.xyz/zh/smart-contract-development/) （好有用啊啊啊）！
    

Part 3 cv模版

![Screenshot 2026-07-07 at 03.54.27.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yaoyaodata/images/2026-07-06-1783360479794-Screenshot_2026-07-07_at_03.54.27.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
