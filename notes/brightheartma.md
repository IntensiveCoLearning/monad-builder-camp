---
timezone: UTC+8
---

# brightheartma

**GitHub ID:** brightheartma

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
\## Week 1 打卡｜部署 NFTBadge 到 Monad Testnet

\### 今天做了什么

\- 用 Remix 编译并部署了一个 Soulbound（不可转让）ERC-721 徽章合约 `NFTBadge`，基于 OpenZeppelin v5

\- 网络：Monad Testnet（Chain ID 10143）

\- 合约地址`0xe9a15a7A91708765b6339dCeED7b89D2b3a3eb9D`

\- 完成了 read`owner()` 等）和 write`setBadgeTypeURI` / `mintBadge`）两类函数调用

\- 给合约配置了真实 IPFS metadata（Pinata 上传图片 + JSON），替换掉了最初的占位字符串

\- 整理了 README v0.1，写清楚了合约功能、部署步骤、交互方式

\### 踩过的坑

1\. **MetaMask 交易排队**：连续点击 write 按钮会攒一堆待确认请求，导致"点 A 方法却弹出 B 方法确认框"——本质是 nonce 顺序问题，不是合约或钱包故障，清空队列即可

2\. \*`mintBadge` revert\*\*：合约设计上"同一地址同一 `badgeType` 不能重复领取"，第二次拿同样参数铸造会被 EVM 回滚。解决方式是新开一个 `badgeType=2` 重新走 `setBadgeTypeURI → mintBadge` 流程

\### 关键技术点

\- **Soulbound 实现**：重写 OpenZeppelin v5 的 `_update` 钩子，只放行铸造`from==0`）和销毁`to==0`），拦截普通转账

\- **CEI 模式**`mintBadge` 里先完成状态写入（Effects）再调用 `_safeMint`（Interactions），降低重入风险

\- **IPFS metadata 流程**：先传图片拿 CID → 写入 JSON 的 `image` 字段 → 再传 JSON 拿最终 CID → 这个 CID 才是 `setBadgeTypeURI` 要填的值

\### 产出

\- 合约地址 + 部署 tx hash + 两笔交互 tx hash`setBadgeTypeURI, mintBadge`）

\- README v0.1（含部署信息、交互说明、安全设计说明）
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

**今日学习内容**

**一、Web3 实习手册·入门导读**

**1\. 区块链基础概念**

-   区块链本质是按时间顺序连接的区块链条，每个区块包含交易记录及前一区块的哈希值，因此历史记录一旦写入便不可篡改
    
-   核心特性：数据不可篡改、公开透明但地址匿名、交易确认速度快
    
-   比特币：节点通过挖矿竞争记账权并获得代币奖励，具备货币属性——总量恒定、可自由转账
    
-   三种链的形态：
    
    -   公链：任何人可自由加入，完全开放
        
    -   联盟链：由多方共同组成"董事会"式治理，半开放
        
    -   私链：由单一主体审批控制，完全封闭
        
-   Web2、Web 3.0、Web3 的区别：
    
    -   Web2：中心化服务器架构，平台掌控用户数据
        
    -   Web 3.0：语义网概念，与区块链技术无关，容易混淆
        
    -   Web3：去中心化互联网，数据主权归还用户，智能合约自动执行规则，资产真正由用户自己掌控
        

**2\. 以太坊概览**

-   以太坊被称为"区块链 2.0"，由 Vitalik Buterin 于 2013—2014 年提出，核心创新是引入智能合约，愿景是打造"世界计算机"
    
-   与比特币的关键差异：
    
    -   图灵完备、可编程，而比特币是简单脚本
        
    -   目前采用 PoS 共识，而比特币是 PoW
        
    -   出块时间约 12 秒，而比特币约 10 分钟
        
-   核心机制：
    
    -   账户体系：EOA（私钥控制的外部账户）与 CA（合约代码控制的合约账户）
        
    -   Gas 模型：费用 = Gas Limit × Gas Price；EIP-1559 之后拆分为销毁的 Base Fee 与支付给验证者的 Tip
        
    -   EVM：全网节点同步执行智能合约的虚拟机环境
        

**3\. 行业赛道全览**

-   **DeFi**：Uniswap（基于恒定乘积公式 x\*y=k 的 AMM 自动定价机制）、Compound（超额抵押借贷，以 cToken 计息）、MakerDAO/Sky（超额抵押生成稳定币 DAI/USDS）
    
-   **NFT**：解决数字资产的唯一性与所有权确权问题，代表项目有 CryptoPunks（先锋项目）、OpenSea（最大交易平台）
    
-   **DAO**：以社区投票取代传统公司科层治理，代表案例有 Nouns DAO、LXDAO（支持 Web3 公共物品建设）；ConstitutionDAO 众筹竞拍美国宪法原稿未遂，反而暴露出 DAO 治理信息过度透明、不利于竞价类场景的短板
    
-   **MEME**：文化驱动型代币（如 DOGE、PEPE），投机性极强，需警惕无实际价值支撑的炒作、盲目跟风与代币集中度过高的风险
    
-   **交叉领域**：DeFi+NFT（NFT 抵押借贷）、DAO+MEME（社区治理与文化资产融合）、AI+DeFi（尚处早期）、RWA（现实资产上链）
    
-   **2026 新趋势**：Intent-Based 交易、账户抽象（ERC-4337）、模块化区块链、AI 与 Web3 深度融合
    

**二、实操**

-   创建了一个课程专用钱包，并添加 Monad Testnet 网络
    
-   领取测试网资产，完成一次测试网转账/应用交互实操
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
