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

下一步我建議學 **「解碼」**。

因為你剛剛理解的是：

```
人看得懂的資料 -> SchemaEncoder -> 0x bytes
```

下一步就是反過來：

```
0x bytes -> SchemaEncoder.decodeData -> 人看得懂的資料
```

這樣你會真正理解 EAS 的 `data` 欄位不是神秘亂碼，而是「照 schema 打包後的資料」。

範例：

```
import { SchemaEncoder } from "@ethereum-attestation-service/eas-sdk";

const schema = "uint256 eventId, uint8 voteIndex";

const schemaEncoder = new SchemaEncoder(schema);

const encodedData = schemaEncoder.encodeData([
  { name: "eventId", value: 1, type: "uint256" },
  { name: "voteIndex", value: 1, type: "uint8" }
]);

console.log("encoded:", encodedData);

const decodedData = schemaEncoder.decodeData(encodedData);

console.log("decoded:", decodedData);
```

你應該會看到類似：

```
encoded: 0x...
decoded: [
  {
    name: "eventId",
    type: "uint256",
    value: 1
  },
  {
    name: "voteIndex",
    type: "uint8",
    value: 1
  }
]
```

這一步的學習重點是：

```
schema 必須和資料一致
```

例如 schema 說：

```
"uint256 eventId, uint8 voteIndex"
```

那資料就必須有：

```
eventId: uint256
voteIndex: uint8
```

如果你改成：

```
"uint256 eventId, bool approved"
```

那資料就要變成：

```
eventId: 1
approved: true
```

所以第二步不是上鏈，而是建立直覺：

**EAS 的 attestation data = schema + encoded bytes。**

等你熟悉 encode/decode 後，下一步才適合學：

```
查詢一筆真實鏈上的 attestation，但仍然不寫鏈、不花 gas
```

EAS 的 data 是一串 0x…，decode 後就生成 schema . 鏈上儲的就是一串 0x…，但可以解碼為真正有意義的資料？

這樣可以大覆減省鏈上存資料的 size 及 gas fee?

對，大方向你理解對了，但有一個小地方要修正：

**不是 decode 後生成 schema。**  
正確是：

```
schema + 0x data -> decode -> 有意義的資料
```

也就是說，`0x...` 本身只是 bytes。你必須知道它對應哪個 schema，才知道怎麼解讀它。

例如鏈上儲的是：

```
0x0000000000000000000000000000000000000000000000000000000000000001
0000000000000000000000000000000000000000000000000000000000000001
```

如果 schema 是：

```
"uint256 eventId, uint8 voteIndex"
```

那就可以解碼成：

```
eventId = 1
voteIndex = 1
```

但如果你用錯 schema 去 decode，解出來的意義就會錯，甚至 decode 失敗。

所以 EAS 的關係是：

```
Schema 定義資料格式
Attestation.data 儲存 encoded bytes
解碼時需要同一個 schema
```

至於你問的 gas fee：**可以減少一部分，但它不是壓縮工具。**

它省 gas 的主要原因是：

```
schema 只需要註冊一次
每筆 attestation 不需要重複儲存欄位名稱和完整 JSON
只儲存 schema UID + encoded data
```

例如不要每次都存：

```
{
  "eventId": 1,
  "voteIndex": 1,
  "description": "..."
}
```

而是存：

```
schemaUID: 0x...
data: 0x...
```

這樣比把 JSON 字串、欄位名稱、描述文字全部塞上鏈便宜很多。

但要注意：EVM 的 ABI encoding 不是極限壓縮。很多基本型別會被補齊到 32 bytes，例如：

```
uint8 voteIndex = 1
```

雖然 `uint8` 理論上只要 1 byte，但 ABI encode 後通常仍然會佔 32 bytes 的 slot。

所以更精準地說：

**EAS 不是靠把資料壓到最小來省 gas，而是靠標準化 schema、避免重複存欄位結構、讓資料可驗證且可解碼。**

如果真的要大幅省 gas，常見做法是：

```
只把 hash / UID / Merkle root 放鏈上
完整資料放鏈下，例如 IPFS、資料庫、off-chain attestation
需要驗證時再拿原始資料來比對 hash
```

一句話總結：

**鏈上儲的是** `0x...` **bytes；靠 schema 才能解碼成有意義資料；它通常比存 JSON 省 gas，但不是萬能壓縮，真正大幅省 gas 要用 off-chain data 或 hash/Merkle root。**

下一步學 **Schema UID**，這是 EAS 從「資料編碼」走向「真正 attestation」的關鍵。

你現在已經懂：

```
schema + data -> encoded 0x bytes
```

下一步要加上：

```
schema 本身也會有一個 UID
```

也就是：

```
schema:
"uint256 eventId, uint8 voteIndex"

schemaUID:
0xabc123...
```

在 EAS 裡，一筆 attestation 通常不是只存 `data`，而是像這樣：

```
schema: 0xabc123...
recipient: 0xUser...
attester: 0xSigner...
data: 0x0000...
expirationTime: 0
revocable: true
```

其中 `schema` 欄位放的不是完整 schema 字串，而是 **schema UID**。

這樣做的原因是：

```
schema 註冊一次
之後所有 attestation 只引用 schema UID
```

可以想像成：

```
Schema Registry:
0xabc123... => "uint256 eventId, uint8 voteIndex"

Attestation:
schema = 0xabc123...
data = 0x0000...
```

所以解碼時流程是：

```
1. 看到 attestation.schema = 0xabc123...
2. 去 Schema Registry 找到 schema string
3. 用 schema string 建立 SchemaEncoder
4. decode attestation.data
5. 得到真正資料
```

這一步你要建立的直覺是：

**EAS 的 data 不能單獨理解，必須搭配 schema UID。**

完整關係：

```
Schema string
    ↓ 註冊後
Schema UID
    ↓ 被 attestation 引用
Attestation.data
    ↓ 用 schema string decode
Meaningful data
```

下一個最輕量練習可以不用寫鏈，只做「模擬版 Schema Registry」：

```
import { SchemaEncoder } from "@ethereum-attestation-service/eas-sdk";

const schemaUID =
  "0xabc0000000000000000000000000000000000000000000000000000000000000";

const schemaRegistry = {
  [schemaUID]: "uint256 eventId, uint8 voteIndex"
};

const attestation = {
  schema: schemaUID,
  data: "0x..." // 前一步 encode 出來的 bytes
};

const schema = schemaRegistry[attestation.schema];

const encoder = new SchemaEncoder(schema);

const decoded = encoder.decodeData(attestation.data);

console.log(decoded);
```

這是在模擬 EAS 真實世界裡的邏輯：

```
schemaUID -> 找 schema -> decode data
```

學完這步，下一步就可以進入：

```
讀取一筆真實鏈上的 attestation
```

那一步仍然不需要私鑰、不寫鏈、不花 gas，只需要讀公開資料。
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
