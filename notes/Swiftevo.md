---
timezone: UTC+8
---

# Shift

**GitHub ID:** Swiftevo

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
意思是：我們先只學 **EAS 怎麼把「人看得懂的資料」轉成「鏈上合約看得懂的 bytes」**。

EAS 的 `SchemaEncoder` 做的事情很單純：

你給它一個資料格式，也就是 `schema`：

```
"uint256 eventId, uint8 voteIndex"
```

這句話代表：

```
eventId 是 uint256
voteIndex 是 uint8
```

然後你給它實際資料：

```
eventId = 1
voteIndex = 1
```

`SchemaEncoder` 會把這些資料編碼成一串 hex bytes，例如：

```
0x0000000000000000000000000000000000000000000000000000000000000001...
```

這串 bytes 才是 EAS attestation 裡面 `data` 欄位真正要放的東西。

可以把它想成：

```
原始資料：
{
  eventId: 1,
  voteIndex: 1
}

Schema：
uint256 eventId, uint8 voteIndex

編碼後：
0x......
```

這個階段完全不需要：

-   錢包
    
-   私鑰
    
-   RPC provider
    
-   測試網 ETH
    
-   部署合約
    
-   發交易
    
-   gas
    

因為我們只是**在本機做資料編碼練習**，還沒有把資料送到鏈上。

為什麼先學這個？因為 EAS 的核心流程其實是：

```
1. 定義 schema
2. 根據 schema 編碼資料
3. 用 encoded data 建立 attestation
4. 查詢或驗證 attestation
```

你現在只學第 1、2 步，最輕、最安全，也最適合入門。

一句話：**SchemaEncoder 就是 EAS 的資料打包器，負責把 structured data 變成 EVM 可以理解的 bytes。**
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

**On-chain vs Off-chain**

EAS 支援兩種模式：

-   **On-chain attestation**：資料或其編碼結果寫進鏈上合約，公開、可組合、可被智能合約直接讀取，但需要 gas。
    
-   **Off-chain attestation**：由簽名產生，不一定上鏈，成本低、彈性高；需要時可以驗簽，或把 UID / timestamp 上鏈做存在性證明。
    

**eas-sdk 做什麼？**

這個 SDK 是開發者和 EAS 協議互動的工具包。它提供：

-   `EAS`：連接 EAS 合約、建立/查詢/撤銷 attestations
    
-   `SchemaRegistry`：註冊與查詢 schemas
    
-   `SchemaEncoder`：把 schema 對應的資料編碼成鏈上可用 bytes
    
-   `Offchain`：建立與驗證 off-chain attestations
    
-   Delegated attestation：讓 A 簽名授權，B 幫忙送交易與付 gas
    
-   `PrivateData`：用 Merkle tree 只公開部分資料，其他保持私密
    

安裝方式：

```
npm install @ethereum-attestation-service/eas-sdk
```
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->


今天在看 021 學習以太坊，了解到多個層面叠加的去中心化  
  
综合来看，以太坊的去中心化并不是单一机制的结果，而是多层叠加的产物：  
  
• 共识层：PoS + Gasper 把出块与投票权分散给百万级别的验证者， slashing 和 inactivity leak 提供了强烈的经济约束。  
• 网络层：全球成千上万的 P2P 节点彼此直连，没有中心服务器；全节 点、轻节点和质押节点共同维持网络。  
• 实现层：多客户端、多语言、多团队实现同一协议，主动追求客户端占 比的健康分布，降低实现层单点风险。  
• 扩容路径：通过 Rollup-中心路线，把执行和创新推向大量 L2，L1 聚 焦于数据和共识的高去中心化、安全性。  
• 治理层：没有形式化的链上投票，靠公开讨论和社会共识推动协议演进， 避免简单的「代币持有量 = 治理权」。 任何持有至少 32 ETH 的人，都可以通过质押成为验证者，直接参与到网络
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
