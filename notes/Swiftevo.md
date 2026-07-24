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
# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
`const` 是 JavaScript 裡用來**宣告變數**的關鍵字。

例如：

```
const name = "Alice";
```

意思是：

```
建立一個叫 name 的變數
它的值是 "Alice"
```

`const` 的特點是：**這個變數名稱不能再被重新指定成另一個值**。

例如：

```
const name = "Alice";

name = "Bob"; // 會報錯
```

所以 `const` 常用來放「不打算改變的東西」。

你貼的這句：

```
const fs = require("node:fs");
```

意思是：

```
從 Node.js 載入 fs 模組
然後把它放進一個叫 fs 的變數
```

`fs` 是 file system，負責讀寫檔案。  
例如我們用它讀 `evidence.txt`：

```
fs.readFileSync("evidence.txt");
```

這段：

```
const {
  NO_EXPIRATION,
  Offchain,
  OffchainAttestationVersion,
  SchemaEncoder,
  ZERO_BYTES32,
} = require("@ethereum-attestation-service/eas-sdk");
```

意思是：

```
從 eas-sdk 裡拿出幾個工具：
NO_EXPIRATION
Offchain
OffchainAttestationVersion
SchemaEncoder
ZERO_BYTES32
```

這種寫法叫 **destructuring 解構賦值**。

它等於比較長的寫法：

```
const easSdk = require("@ethereum-attestation-service/eas-sdk");

const NO_EXPIRATION = easSdk.NO_EXPIRATION;
const Offchain = easSdk.Offchain;
const OffchainAttestationVersion = easSdk.OffchainAttestationVersion;
const SchemaEncoder = easSdk.SchemaEncoder;
const ZERO_BYTES32 = easSdk.ZERO_BYTES32;
```

最後這句：

```
const { Wallet, keccak256 } = require("ethers");
```

意思是：

```
從 ethers 裡拿出 Wallet 和 keccak256
```

也等於：

```
const ethers = require("ethers");

const Wallet = ethers.Wallet;
const keccak256 = ethers.keccak256;
```

所以一句話：

`const` **是宣告一個不會被重新指定的變數；這裡用它把外部套件裡的工具拿進目前程式碼裡使用。**

這段是三個小功能，都是在幫主程式整理流程。

**1\.** `requiredEnv(name)`

```
function requiredEnv(name) {
  const value = process.env[name];
  if (!value) {
    throw new Error(`Missing ${name}. Run "node offchain-sign.cjs --help".`);
  }
  return value;
}
```

意思是：**讀取必須存在的環境變數**。

例如：

```
requiredEnv("PRIVATE_KEY")
```

它會去讀：

```
process.env["PRIVATE_KEY"]
```

也就是你在 PowerShell 設的：

```
$env:PRIVATE_KEY="0x..."
```

如果有值，就回傳：

```
return value;
```

如果沒有值，就報錯：

```
Missing PRIVATE_KEY. Run "node offchain-sign.cjs --help".
```

所以它是安全檢查，避免程式沒有 private key 還繼續跑。

* * *

**2\.** `jsonReplacer(_key, value)`

```
function jsonReplacer(_key, value) {
  return typeof value === "bigint" ? value.toString() : value;
}
```

這個是給：

```
JSON.stringify(...)
```

用的。

原因是 JavaScript 的 `JSON.stringify` 不能直接處理 `BigInt`：

```
101n
```

會報錯。

所以我們用 `jsonReplacer` 告訴它：

```
如果 value 是 bigint，就轉成字串
否則保持原樣
```

例如：

```
101n -> "101"
```

這樣才能把 off-chain attestation 寫成 JSON 檔。

* * *

**3\.** `async function main()`

```
async function main() {
  if (process.argv.includes("--help")) {
    printHelp();
    return;
  }
```

這是主程式入口。

`async` 代表這個 function 裡面可以用：

```
await
```

因為簽名這類操作通常是非同步的。

這段：

```
process.argv.includes("--help")
```

是在檢查你執行程式時有沒有帶 `--help`。

例如你跑：

```
node offchain-sign.cjs --help
```

那 `process.argv` 裡就會包含：

```
--help
```

程式就會執行：

```
printHelp();
return;
```

意思是：

```
顯示使用說明
然後停止 main，不繼續簽名
```

所以整段可以用白話理解：

```
requiredEnv:
幫我確認必要設定有沒有給

jsonReplacer:
幫我把 BigInt 轉成 JSON 可以保存的格式

main:
程式真正開始跑的地方；如果使用者要求 --help，就只顯示說明
```
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->

開始了，off-chain attestation 的簽名與驗證腳本已做好並測通。

新增了兩個檔案：

-   \[offchain-sign.cjs\](C:\\\\Users\\\\User\\\\Documents\\\\EAS\\\\offchain-sign.cjs)：用 `PRIVATE_KEY` 對 payload 簽名，產生 `offchain-attestation.json`
    
-   \[offchain-verify.cjs\](C:\\\\Users\\\\User\\\\Documents\\\\EAS\\\\offchain-verify.cjs)：驗證 signature，以及 `evidence.txt` 的 hash 是否吻合
    

你可以這樣簽一份 off-chain attestation：

```
$env:PRIVATE_KEY="0x你的測試錢包私鑰"
$env:EVIDENCE_FILE="evidence.txt"
node offchain-sign.cjs
```

然後驗證：

```
$env:ATTESTATION_FILE="offchain-attestation.json"
$env:EVIDENCE_FILE="evidence.txt"
node offchain-verify.cjs
```

我已用測試私鑰跑過，結果是：

```
Signature valid: true
evidence valid: true
```

這裡就能看到 off-chain 的本質：沒有 RPC、沒有 Sepolia ETH、沒有交易。它只是產生一份 `payload + signature` 的 JSON；任何人拿到這份 JSON 和 evidence，就能驗證「這份 payload 確實由 signer 簽過，而且 evidence 沒被改」。
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->


下一步我建議學 **Off-chain attestation 實作**。

你已經做完 on-chain 主線：

```
schema -> encode data -> hash evidence -> 寫入 Sepolia -> 讀回驗證
```

現在最自然就是學另一半：

```
schema -> encode data -> 建立 payload -> 用私鑰簽名 -> 得到 signed attestation -> 本機驗證簽名
```

這一步的好處是：

```
不用 gas
不用送交易
不用等待區塊
但仍然有「某個地址真的簽過這份資料」的可驗證性
```

學完你會真正懂 EAS 的兩種模式：

```
On-chain:
鏈上保存 attestation

Off-chain:
你自己保存 payload + signature
```

這一步我們可以做一個新腳本：

```
offchain-sign.cjs
```

它會輸出一個檔案：

```
offchain-attestation.json
```

裡面包含：

```
payload
signature
signer / attester
schema
decoded data
evidenceHash
```

然後再做第二個腳本：

```
offchain-verify.cjs
```

它會讀：

```
offchain-attestation.json
evidence.txt
```

然後驗證：

```
1. signature 是否真的是 attester 簽的
2. payload 裡的 evidenceHash 是否等於 evidence.txt 的 hash
```

這會形成 off-chain 的完整閉環：

```
簽發 -> 保存 JSON -> 驗簽 -> 驗 evidence
```

而且這一步可以完全在 PowerShell 本機完成，不需要 Sepolia ETH。只需要測試私鑰來簽名。
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->



以現在查到的資料看，**EAS 還是活躍的，不像是死掉或完全邊緣化的項目**。但它也不是「所有 Web3 app 都在用的主流基礎設施」那種級別。比較準確的定位是：

```
活躍中的 attestation infrastructure
但仍屬於偏開發者 / protocol / identity / reputation / verification 場景的工具
```

幾個活躍訊號：

-   官方網站仍在更新，定位是「attestation infrastructure for verifiable data」，並且主打 devs、agents、structured data signing。官方頁面也顯示 `9.5M+ attestations`、`450k+ attesters`。來源：[attest.org](http://attest.org)
    
-   SDK 還在 npm 發版。`@ethereum-attestation-service/eas-sdk` 目前顯示 `2.9.1`，約一個月前發布，週下載約 `9,380`。來源：[npm eas-sdk](https://www.npmjs.com/package/%40ethereum-attestation-service/eas-sdk)
    
-   EASScan 還有近期 activity。例如 Sepolia 頁面顯示總 attestations 約 `150,426`，unique attestors `2,671`，而且列表裡有幾分鐘前的新 on-chain attestations。來源：[Sepolia EASScan](https://sepolia.easscan.org/attestations)
    
-   EASScan 支援多鏈、GraphQL API、offchain attestation、timestamp、private data 等工具，文件也仍可用。來源：[EASScan docs](https://docs.attest.org/docs/easscan/overview)、[GraphQL API docs](https://docs.attest.org/docs/developer-tools/api)
    

但也要冷靜看：

```
它不是像 Uniswap、Aave、ENS 那種大眾認知度很高的應用層協議。
```

EAS 更像是「底層資料證明標準」。你不一定會在普通用戶端看到「我正在使用 EAS」，但一些應用可以在背後用它做：

```
身份 / reputation
proof of attendance
證書
KYC / accreditation
AI output verification
document hash proof
DAO eligibility
off-chain signed credential
```

所以我的判斷是：

**EAS 沒有被淘汰，仍然活躍；但它是偏基礎設施型、場景型採用，不是全民級 Web3 熱點。**

如果你學它，價值不只在 EAS 本身，也是在學一整套很實用的 Web3 pattern：

```
schema
attestation
hash commitment
off-chain signed data
on-chain verification
revocation
reputation / credential design
```

這些觀念即使將來不用 EAS，也很有用。
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->




好，我們深入一點，但先不掉進數學黑洞。

Ethereum 用的簽名方法大致是：

```
ECDSA + secp256k1
```

意思是：

```
ECDSA      = 橢圓曲線數位簽名演算法
secp256k1  = 使用哪一條橢圓曲線
```

**1\. 私鑰是什麼？**

私鑰本質上是一個很大的隨機數。

例如概念上：

```
privateKey = 一個 256-bit 大數字
```

這個數字必須保密。

**2\. 公鑰怎樣來？**

系統有一個公開的基準點，叫：

```
G
```

然後用私鑰 `k` 做橢圓曲線上的點乘法：

```
publicKey = k * G
```

這裡的 `*` 不是普通乘法，而是橢圓曲線上的「重複點加法」。

神奇之處是：

```
privateKey -> publicKey 很容易
publicKey -> privateKey 幾乎不可能
```

這就是安全性的核心。

**3\. 地址怎樣來？**

Ethereum address 是從 public key 推出來的：

```
publicKey
  ↓ keccak256
取最後 20 bytes
  ↓
address
```

所以：

```
私鑰 -> 公鑰 -> 地址
```

但反過來不行：

```
地址 -> 公鑰 -> 私鑰
```

不可能實際算出來。

**4\. 簽名時發生什麼？**

假設你要簽一份 payload：

```
payload = "student completed course 101"
```

第一步通常是 hash：

```
messageHash = keccak256(payload)
```

然後錢包會產生一個一次性隨機數，叫：

```
nonce / k
```

注意，這不是交易 nonce，是簽名內部用的一次性秘密數。

接著 ECDSA 用：

```
messageHash
privateKey
random nonce
```

算出簽名：

```
signature = (r, s, v)
```

Ethereum 常看到的 signature 其實包含：

```
r
s
v
```

其中：

```
r, s = 簽名的主要數學結果
v    = recovery id，用來幫助還原簽名者公鑰 / 地址
```

**5\. 驗證時發生什麼？**

驗證者拿到：

```
payload
signature
expected attester address
```

他會做：

```
payload -> hash
hash + signature -> recover public key / address
```

如果 recover 出來的 address 等於 expected attester：

```
valid
```

否則：

```
invalid
```

所以驗證時不需要私鑰。

**6\. 為什麼改 payload 會失效？**

因為簽名是針對 `messageHash` 做的。

如果 payload 從：

```
completed = true
```

改成：

```
completed = false
```

那 hash 會完全不同：

```
oldHash != newHash
```

原本 signature 是針對 oldHash 算出來的，拿去驗 newHash 就會失敗。

**7\. 為什麼不能偽造？**

要偽造一個有效簽名，你等於要在不知道 private key 的情況下，產生一個能 recover 到某地址的 `(r, s, v)`。

這在計算上幾乎不可行，因為依賴的是：

```
橢圓曲線離散對數問題
```

簡單說：

```
知道 k 和 G，算 k * G 很容易
知道 G 和 k * G，反推出 k 幾乎不可能
```

**8\. 為什麼簽名 nonce 很重要？**

ECDSA 有一個非常危險的點：簽名用的一次性 nonce 不能重複、不能外洩。

如果同一個私鑰對兩個不同 message 用了同一個 nonce，私鑰可能被推回來。

歷史上真的出過這類事故。

所以現代錢包通常用 deterministic nonce，例如 RFC 6979，避免靠爛亂數出事。

**9\. EAS off-chain attestation 裡怎樣用？**

EAS 不是直接叫你簽任意字串，而是用 EIP-712 typed data。

流程是：

```
payload fields
  ↓ 按 EIP-712 規則結構化
typedDataHash
  ↓ ECDSA 簽名
signature
```

這樣比簽一段模糊文字安全，因為 wallet 可以知道你簽的是哪些欄位：

```
schema
recipient
expirationTime
revocable
refUID
data
nonce
```

一句話總結：

**橢圓曲線簽名就是：用私鑰對 payload 的 hash 產生 signature；任何人可以用 signature recover 出簽名地址，確認這份 payload 確實由該地址簽過，而且內容沒有被改。**
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->





對，**橢圓曲線簽名**是加密學裡的東西，也是 Web3 幾乎每天都在用的核心技術。

Ethereum 錢包用的是一種橢圓曲線簽名演算法：

```
ECDSA over secp256k1
```

拆開看：

```
ECDSA      = Elliptic Curve Digital Signature Algorithm
secp256k1  = Ethereum / Bitcoin 使用的橢圓曲線參數
```

它做的事情是：

```
用私鑰簽名
用公鑰 / 地址驗證
```

你可以想成：

```
私鑰 = 你的印章
簽名 = 蓋章結果
地址 = 大家可以用來確認是不是你蓋的身份
```

但它不是普通數學加減乘除那種「標準算式」。  
它是建立在一套特殊數學結構上：**有限域上的橢圓曲線點運算**。

不用深入數學也可以先抓住這幾個性質：

```
1. 私鑰可以產生公鑰
2. 公鑰可以產生地址
3. 私鑰可以對訊息簽名
4. 任何人可以用簽名反推出/驗證簽名者地址
5. 但不能從地址或簽名反推出私鑰
```

在 Ethereum 裡，當你做：

```
送交易
Sign message
Sign typed data
EAS off-chain attestation
```

背後都在用這套簽名邏輯。

流程簡化成：

```
payload
  ↓ hash
messageHash
  ↓ 用 private key 做 ECDSA 簽名
signature
  ↓ 驗證時 recover signer address
address
```

所以驗證者可以問：

```
這個 signature 是不是 0xABC... 這個地址簽的？
```

如果是，就代表：

```
控制該私鑰的人曾經簽過這份 payload
```

在 Web3 裡非常常見：

```
MetaMask 簽交易
登入網站的 Sign-In with Ethereum
NFT 授權簽名
Permit 簽名
EAS off-chain attestation
DAO 投票簽名
```

一句話：

**橢圓曲線簽名是一種加密學簽名方法；Ethereum 用它來證明「某個地址的私鑰持有人同意/簽署過某段資料」，而不需要暴露私鑰。**
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->






實際上有兩種常見做法，兩種都叫「用錢包簽 off-chain attestation」。

**1\. 前端用 MetaMask 簽**

這是網站常見做法。

流程是：

```
網站準備 payload
    ↓
呼叫 MetaMask
    ↓
MetaMask 跳出視窗，顯示要簽的 typed data
    ↓
使用者按 Sign
    ↓
MetaMask 回傳 signature
    ↓
網站把 payload + signature 存起來
```

這種會跳出 MetaMask，但**不是交易確認**，不會花 gas。  
按鈕通常是 `Sign`，不是 `Confirm transaction`。

**2\. 本機 PowerShell 腳本簽**

這是我們現在可以做的方式。

流程是：

```
腳本準備 payload
    ↓
用 PRIVATE_KEY 建立 wallet
    ↓
wallet 在本機簽 payload
    ↓
得到 signature
    ↓
把 payload + signature 寫成 JSON 檔
```

這種不會跳出 MetaMask，因為私鑰已經在環境變數或腳本裡給了 Node.js。  
也不花 gas，因為沒有送交易。

你問是不是像 `A + B` 這樣的數學算式：**概念上可以這樣想，但實際不是普通加法。**

不是：

```
payload + privateKey = signature
```

這種簡單加法。

更準確是：

```
signature = cryptographicSign(hash(payload), privateKey)
```

也就是：

```
1. payload 先被整理成標準格式
2. 對 payload 做 hash
3. 用私鑰對 hash 做橢圓曲線簽名
4. 得到 signature
```

最後的 signed attestation 是「打包在一起」，不是數學相加：

```
{
  payload: {
    schema: "0x...",
    recipient: "0x...",
    data: "0x...",
    expirationTime: 0,
    revocable: true
  },
  signature: {
    v: 27,
    r: "0x...",
    s: "0x..."
  }
}
```

所以：

```
signed attestation = payload + signature
```

這裡的 `+` 是「一起保存 / 一起傳送」的意思，不是算術加法。

可以把它想成：

```
payload = 文件內容
signature = 文件上的簽名
signed attestation = 文件內容 + 簽名
```

如果用 MetaMask：

```
人手按 Sign
MetaMask 產生 signature
```

如果用 PowerShell：

```
Node.js 用 PRIVATE_KEY 自動簽
產生 signature
```

對，**橢圓曲線簽名**是加密學裡的東西，也是 Web3 幾乎每天都在用的核心技術。

Ethereum 錢包用的是一種橢圓曲線簽名演算法：

```
ECDSA over secp256k1
```

拆開看：

```
ECDSA      = Elliptic Curve Digital Signature Algorithm
secp256k1  = Ethereum / Bitcoin 使用的橢圓曲線參數
```

它做的事情是：

```
用私鑰簽名
用公鑰 / 地址驗證
```

你可以想成：

```
私鑰 = 你的印章
簽名 = 蓋章結果
地址 = 大家可以用來確認是不是你蓋的身份
```

但它不是普通數學加減乘除那種「標準算式」。  
它是建立在一套特殊數學結構上：**有限域上的橢圓曲線點運算**。

不用深入數學也可以先抓住這幾個性質：

```
1. 私鑰可以產生公鑰
2. 公鑰可以產生地址
3. 私鑰可以對訊息簽名
4. 任何人可以用簽名反推出/驗證簽名者地址
5. 但不能從地址或簽名反推出私鑰
```

在 Ethereum 裡，當你做：

```
送交易
Sign message
Sign typed data
EAS off-chain attestation
```

背後都在用這套簽名邏輯。

流程簡化成：

```
payload
  ↓ hash
messageHash
  ↓ 用 private key 做 ECDSA 簽名
signature
  ↓ 驗證時 recover signer address
address
```

所以驗證者可以問：

```
這個 signature 是不是 0xABC... 這個地址簽的？
```

如果是，就代表：

```
控制該私鑰的人曾經簽過這份 payload
```

在 Web3 裡非常常見：

```
MetaMask 簽交易
登入網站的 Sign-In with Ethereum
NFT 授權簽名
Permit 簽名
EAS off-chain attestation
DAO 投票簽名
```

一句話：

**橢圓曲線簽名是一種加密學簽名方法；Ethereum 用它來證明「某個地址的私鑰持有人同意/簽署過某段資料」，而不需要暴露私鑰。**

兩者產生的東西本質一樣：**一份可驗證的 off-chain signed attestation**。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->








不是。**任何人都可以驗證** 的前提是：他拿到了那份 off-chain attestation。

Off-chain attestation 不會自動公開在鏈上，所以不是全世界自然都看得到。

它存在什麼地方，誰就可能看得到：

```
存在你本機          只有你看得到
存在後端資料庫      有資料庫權限的人看得到
寄給某個使用者      收件者看得到
放到 IPFS 公開      任何知道 CID 的人都可能看得到
放到公開網站        任何人都看得到
放到私有儲存        只有被授權的人看得到
```

所以 off-chain 的特點是：

```
可驗證性 ≠ 公開性
```

意思是：

```
只要某人拿到 payload + signature，就可以驗證
但不是每個人都一定拿得到 payload + signature
```

例如：

```
Alice 簽了一份 off-chain attestation
Alice 只發給 Bob
```

那麼：

```
Bob 可以驗證
其他人不知道這份 attestation 存在
```

但如果 Bob 把它轉發出去，其他拿到的人也可以驗證。

還有一點很重要：off-chain attestation 的 payload 通常包含資料本身或 encoded data。  
所以如果 payload 裡有敏感內容，拿到 payload 的人可能可以 decode 出來。

因此私密資料常見做法是：

```
payload 裡只放 hash
原始內容另外加密保存
只把解密 key 給授權者
```

總結一句：

**Off-chain attestation 不會自動公開；誰能看到，取決於你把它存在哪裡、分享給誰。但任何拿到完整 payload + signature 的人，都可以驗證它是真的由 attester 簽過。**
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->









EAS 有兩種主要 attestation：

```
On-chain attestation  鏈上證明
Off-chain attestation 鏈下證明
```

核心分別是：**資料是否真的寫進鏈上合約狀態。**

**On-chain Attestation**

鏈上 attestation 是你剛剛做過的那種。

流程是：

```
schema -> encode data -> 發交易 -> 寫入 EAS 合約 -> 得到 attestation UID
```

特點：

```
需要錢包簽交易
需要 gas
資料存在鏈上
任何人都可以用 EAS 合約 / EASScan 查到
智能合約可以直接讀取或驗證
可以 revoke，如果 schema/attestation 設定允許
```

你之前做的就是：

```
evidence.txt -> hash -> bytes32 evidenceHash -> 寫入 Sepolia EAS
```

鏈上保存：

```
schema UID
recipient
attester
time
revocable
revoked 狀態
encoded data
```

適合：

```
重要憑證
DAO 權限
智能合約需要讀取的資格
公開可查的證明
需要長期公開存在的紀錄
```

**Off-chain Attestation**

鏈下 attestation 不把整筆資料寫進鏈上 EAS 合約。  
它是由 attester 對一份 attestation payload 做簽名。

流程是：

```
schema -> encode data -> 建立 attestation object -> 用錢包簽名 -> 得到 signed attestation
```

特點：

```
不需要發交易
不需要 gas
資料不自動公開在鏈上
本質是一份「可驗簽的聲明」
需要你自己保存、傳送、上傳或放到資料庫/IPFS
驗證時檢查簽名是不是 attester 簽的
```

它像是：

```
「某地址簽了一份 EAS 格式的證明」
```

但這份證明存在：

```
你的後端
使用者手機
IPFS
email
QR code
資料庫
```

而不是 EAS 合約狀態裡。

適合：

```
大量低成本憑證
不想每筆都花 gas
隱私性較高的資料
臨時通行證
活動票券
網站登入/資格檢查
不需要智能合約直接讀取的證明
```

**最大差別**

```
鏈上 attestation:
鏈上合約記錄「這件事存在」

鏈下 attestation:
簽名本身證明「某人說過這件事」
```

可以這樣比喻：

```
On-chain = 到政府登記處正式登記
Off-chain = 簽了一份文件，你自己保存
```

兩者都可以驗證，但驗證方式不同。

**怎樣做？**

鏈上你已經做過，大概是：

```
const tx = await eas.attest({
  schema: schemaUID,
  data: {
    recipient,
    expirationTime: NO_EXPIRATION,
    revocable: true,
    data: encodedData,
  },
});

const uid = await tx.wait();
```

鏈下則是概念上這樣：

```
const offchain = await eas.getOffchain();

const attestation = await offchain.signOffchainAttestation(
  {
    recipient,
    expirationTime: 0n,
    time: BigInt(Math.floor(Date.now() / 1000)),
    revocable: true,
    version: 2,
    nonce: 0n,
    schema: schemaUID,
    refUID: ZERO_BYTES32,
    data: encodedData,
  },
  signer
);
```

得到的不是交易 hash，而是一份 signed attestation，例如：

```
message + signature
```

驗證時做：

```
signed attestation
+ attester address
+ EAS domain/config
-> verify signature
```

**選哪個？**

簡單判斷：

```
需要智能合約讀取？用 on-chain
需要公開、長期、抗審查紀錄？用 on-chain
需要大量、低成本、私密或快速？用 off-chain
只是要證明某人簽過一份資料？用 off-chain
```

你現在已經完成 on-chain 主線。下一步如果學 off-chain，我會建議先做一個完全不花 gas 的練習：

```
用同一個 schema + evidenceHash
建立 off-chain attestation
驗證簽名
確認 signer 就是 attester
```

這裡要分清楚兩件事：

```
用錢包簽名 ≠ 發交易
```

Off-chain attestation **仍然需要 attester 簽名**，但它只是「簽一段資料」，不是「送交易上鏈」。

所以：

```
需要錢包 / 私鑰：是
需要 gas：否
需要把交易送到鏈上：否
```

你可以把它想成：

```
On-chain:
錢包簽交易 -> 交易送到 Sepolia -> 花 gas -> 鏈上保存

Off-chain:
錢包簽 message -> 不送鏈 -> 不花 gas -> 你自己保存 signed attestation
```

簽名怎樣做？

錢包其實有兩種常見簽名能力：

```
1. sign transaction
簽交易，用來改變鏈上狀態，要 gas

2. sign message / sign typed data
簽訊息，只證明「這個地址同意/聲明這段資料」，不改變鏈上狀態，不要 gas
```

EAS off-chain attestation 用的是第二種，通常是 **EIP-712 typed data signing**。

流程是：

```
attestation payload
    ↓
wallet signTypedData
    ↓
signature
    ↓
signed off-chain attestation
```

payload 內容大概包含：

```
schema UID
recipient
attester
time
expirationTime
revocable
refUID
data
nonce
```

錢包會用私鑰對這些內容簽名，產生一個 signature：

```
0xabc123...
```

之後任何人拿到：

```
payload + signature
```

都可以驗證：

```
這個 signature 是不是某個 attester 地址簽的？
payload 有沒有被改過？
```

如果 payload 改一個字，簽名驗證就會失敗。

所以 off-chain attestation 本質是：

```
一份 EAS 格式的 signed message
```

不是鏈上紀錄。

你問「不動用錢包」：嚴格說，**不可能完全不動用錢包或私鑰**，因為沒有私鑰就不能產生 attester 的簽名。

但可以不用「彈出錢包送交易」。例如：

```
後端用測試私鑰簽名
前端用 MetaMask signTypedData
本機腳本用 PRIVATE_KEY 簽名
```

都不需要 gas。

最簡單理解：

```
簽名 = 我證明這句話是我說的
交易 = 我要求鏈上狀態改變
```

Off-chain attestation 只做前者，所以不花 gas。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->











PS C:\\Users\\User\\Documents\\EAS> node sepolia-verify.cjs

Attestation UID: 0x1776cbbfea6a43bde8187e48993b4f1cf523a8333c71ea6b8bbe53ee2cacffc4

Schema UID: 0x322bf30330ec12c96d2723e7f9226ef72dc0e6dbcb3774b0a95951e0934588c9

Schema: address student, uint256 courseId, bool completed, bytes32 evidenceHash

Recipient: 0xDF7e9Dd7D4A48FDf02A82D7F2EA7b88644f055c1

Attester: 0xDF7e9Dd7D4A48FDf02A82D7F2EA7b88644f055c1

Revocable: true

Revoked: false

Decoded data:

student: 0xDF7e9Dd7D4A48FDf02A82D7F2EA7b88644f055c1

courseId: 101

completed: true

chain evidenceHash: 0xb0fa1b6f67425c859b31745416ea24181d98ee915a2b0c3d3787ca11dec778ce

Local evidence file: evidence.txt

local evidenceHash: 0xb0fa1b6f67425c859b31745416ea24181d98ee915a2b0c3d3787ca11dec778ce

verified: true

完美，這就是 EAS + 鏈下 evidence 的完整閉環。

你剛剛驗證成功的是：

```
鏈上 attestation 裡的 evidenceHash
=
你本機 evidence.txt 重新算出的 hash
```

所以：

```
verified: true
```

代表這份 `evidence.txt` 和當初寫入 Sepolia attestation 時使用的內容一致，沒有被改過。

這筆 attestation 目前狀態也有效：

```
Revocable: true
Revoked: false
```

也就是：

```
這筆證明可以被撤銷
但目前尚未撤銷
```

你現在已經學完一條很重要的 EAS 主線：

```
1. 設計 schema
2. encode data
3. 把大內容 / 私密內容放鏈下
4. 鏈上只放 bytes32 hash
5. 寫入 Sepolia attestation
6. 讀回 attestation
7. 用本機 evidence 重新 hash 驗證
```

下一步可以學 **revocation 撤銷 attestation**。  
也就是：如果這張課程完成證明發錯了，attester 可以把它標記為 revoked，之後驗證時就會看到 `Revoked: true`。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->












下一步學 `bytes32 evidenceHash` **實作**。

也就是把你剛剛理解的概念真的跑一次：

```
鏈下內容 -> hash -> bytes32 -> encode 到 EAS data
```

我們先不寫鏈、不花 gas，只在本機做。

概念 schema 可以改成：

```
"address student, uint256 courseId, bool completed, bytes32 evidenceHash"
```

意思是：

```
student       學生地址
courseId      課程 ID
completed     是否完成
evidenceHash  鏈下證明內容的 hash
```

原本你有：

```
string note
```

現在把它換成：

```
bytes32 evidenceHash
```

因為真實內容不直接放進 attestation，只放 hash。

範例流程：

```
const { SchemaEncoder } = require("@ethereum-attestation-service/eas-sdk");
const { keccak256, toUtf8Bytes } = require("ethers");

const evidence = "Finished EAS basics";
const evidenceHash = keccak256(toUtf8Bytes(evidence));

const schema =
  "address student, uint256 courseId, bool completed, bytes32 evidenceHash";

const encoder = new SchemaEncoder(schema);

const encoded = encoder.encodeData([
  {
    name: "student",
    value: "0x0000000000000000000000000000000000000001",
    type: "address",
  },
  {
    name: "courseId",
    value: 101,
    type: "uint256",
  },
  {
    name: "completed",
    value: true,
    type: "bool",
  },
  {
    name: "evidenceHash",
    value: evidenceHash,
    type: "bytes32",
  },
]);

const decoded = encoder.decodeData(encoded);

console.log("evidence:");
console.log(evidence);
console.log();

console.log("evidence hash:");
console.log(evidenceHash);
console.log();

console.log("encoded data:");
console.log(encoded);
console.log();

console.log("decoded data:");
console.log(decoded);
```

這一步學到的是：

```
原文不進 EAS
hash 進 EAS
原文日後重新 hash 來驗證
```

如果原文多一個空格、大小寫不同、內容改一個字，hash 都會完全不同。

下一個練習可以做成兩段：

```
1. hash "Finished EAS basics"
2. hash "Finished EAS basic"
```

你會看到兩個 hash 完全不一樣。這就是 hash 用來驗證內容沒有被改過的原因。

const { keccak256, toUtf8Bytes } = require("ethers"); 這句有甚麼用？

這句是在從 `ethers` 套件裡拿兩個工具出來用：

```
const { keccak256, toUtf8Bytes } = require("ethers");
```

等於說：

```
我要使用 ethers 裡面的 keccak256
我要使用 ethers 裡面的 toUtf8Bytes
```

它們各自的用途是：

```
toUtf8Bytes("Finished EAS basics")
```

把文字轉成 bytes。

因為 hash 函數不是直接吃「人類文字」，而是吃 bytes。  
所以要先把：

```
Finished EAS basics
```

轉成電腦可處理的 bytes。

然後：

```
keccak256(bytes)
```

把 bytes 做 hash，產生一個固定長度的 `bytes32`。

完整流程是：

```
const evidence = "Finished EAS basics";

const evidenceBytes = toUtf8Bytes(evidence);

const evidenceHash = keccak256(evidenceBytes);
```

也可以合成一行：

```
const evidenceHash = keccak256(toUtf8Bytes(evidence));
```

意思是：

```
文字 -> bytes -> keccak256 hash -> bytes32
```

為什麼用 `keccak256`？

因為 Ethereum / EVM 生態最常用的 hash 函數就是 `keccak256`。  
例如地址、交易、簽名、Solidity 裡的 hash，很多地方都用它。

所以這句：

```
const { keccak256, toUtf8Bytes } = require("ethers");
```

就是在準備兩個工具：

```
toUtf8Bytes  負責把文字變 bytes
keccak256    負責把 bytes 變 hash
```

然後你就可以把 hash 放進 EAS 的：

```
bytes32 evidenceHash
```

console.log("schema:");

console.log(schema);

console.log();

這段沒有了？

那段可以保留，沒有問題。它只是負責把 `schema` 印出來，方便你看目前用的是哪個資料格式。

```
console.log("schema:");
console.log(schema);
console.log();
```

意思是：

```
印出文字 "schema:"
印出 schema 變數內容
印出一個空行，讓畫面比較好讀
```

如果我們改成 `bytes32 evidenceHash` 練習，建議還是保留，完整輸出會比較清楚：

```
console.log("schema:");
console.log(schema);
console.log();

console.log("evidence:");
console.log(evidence);
console.log();

console.log("evidence hash:");
console.log(evidenceHash);
console.log();

console.log("encoded data:");
console.log(encoded);
console.log();

console.log("decoded data:");
console.log(decoded);
```

所以不是不能用了，只是我上一段範例重點放在 `keccak256(toUtf8Bytes(...))`，沒有特別解釋 `console.log("schema:")`。你可以放心留著。

這個練習，把 Finished EAS basics 也放入代碼中了。只要查找代碼，那就會曝露了 hash 代表的內容。實際上，又應該怎樣指向原來的內容，但又不需要放入代碼中，避免曝露呢？

keccak256 為甚麼可以由 bytes 變成固定長度的 hash? 它到底做了甚麼？

你問到真正實務設計了。對，練習裡把：

```
const evidence = "Finished EAS basics";
```

寫死在程式碼裡，只是為了教學。真實系統通常**不會把原文寫進公開程式碼**。

實際上會這樣做：

```
原始內容放在鏈下
程式只拿到內容或檔案後計算 hash
鏈上只存 hash
```

例如幾種做法：

```
1. 本機檔案
證書 PDF 放在你電腦或伺服器
程式讀檔 -> hash 檔案 bytes -> 放 hash 上鏈

2. 私有資料庫
完整內容存在後端 DB
DB 裡保存原文
鏈上保存 hash

3. IPFS / Arweave
公開內容放 IPFS
鏈上保存 hash 或 CID
適合不怕公開、但要防竄改的資料

4. 加密後上 IPFS
內容先加密
加密檔放 IPFS
鏈上保存加密檔 hash
只有有 key 的人能看內容

5. 使用 salt
hash(salt + content)
避免別人猜短內容
```

最簡單的真實流程是：

```
certificate.pdf
    ↓
讀取檔案 bytes
    ↓
keccak256(fileBytes)
    ↓
得到 bytes32 evidenceHash
    ↓
EAS attestation.data 存 evidenceHash
```

這樣程式碼裡不需要出現原文。

之後驗證時：

```
拿到 certificate.pdf
重新 hash
比對 EAS 裡的 evidenceHash
```

如果一樣，代表這份 PDF 沒被改過。

* * *

`keccak256` 為什麼能把任意 bytes 變成固定長度 hash？

可以先用直覺理解：

**hash function 是一台資料壓紋機。**

你丟進去：

```
hello
```

它吐出固定長度：

```
0x1c8aff...
```

你丟進去一本書，它還是吐出固定長度：

```
0xabcd...
```

`keccak256` 的輸出永遠是 **256 bits**，也就是：

```
32 bytes
```

所以 EAS schema 可以用：

```
bytes32 evidenceHash
```

它做的不是「壓縮」，因為 hash 不能還原原文。  
它做的是：

```
任意長度輸入 -> 固定長度指紋
```

這個指紋有幾個特性：

```
1. 同樣輸入，永遠得到同樣 hash
2. 輸入改一點點，hash 會完全不同
3. 從 hash 幾乎不可能反推出原文
4. 很難找到兩份不同內容但 hash 一樣
```

所以它適合做「內容指紋」。

它到底做了什麼？簡化講：

```
1. 把輸入 bytes 分成區塊
2. 不斷混合、旋轉、替換、擴散 bits
3. 讓每一點輸入變化都影響整個輸出
4. 最後取出 256 bits 作為結果
```

Ethereum 用的是 Keccak 系列 hash。你可以把它當成 EVM 世界常用的內容指紋算法。

一句話：

**keccak256 不是把內容加密，也不是把內容壓縮；它是把任意 bytes 變成不可逆、固定 32 bytes 的內容指紋。**
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->













`bool` 是 **boolean** 的縮寫，意思是「布林值」。

它只有兩種可能：

```
true
false
```

也就是「是 / 否」、「對 / 錯」、「通過 / 不通過」。

例如這個 schema：

```
"address user, bool approved, string note"
```

裡面的：

```
bool approved
```

意思是：

```
approved 這個欄位是一個布林值
```

資料可以是：

```
{ name: "approved", value: true, type: "bool" }
```

代表：

```
approved = true
```

也就是「已通過」。

為什麼叫 `bool`？  
因為它來自數學家 **George Boole** 的名字。程式語言裡常把 boolean 簡寫成 `bool`。Solidity / EVM 裡也用 `bool` 這個型別。

**大整數是什麼？**

一般 JavaScript 的 `number` 有安全上限：

```
9007199254740991
```

也就是：

```
Number.MAX_SAFE_INTEGER
```

超過這個數字，JavaScript 可能算不準。

但鏈上常用的整數型別是：

```
uint256
```

`uint256` 可以表示非常非常大的非負整數，最大大概是：

```
2^256 - 1
```

這遠遠超過 JavaScript 普通 `number` 能安全表示的範圍。

所以 JavaScript 用 `BigInt` 來表示大整數，例如：

```
1n
1000000000000000000n
```

後面的 `n` 就代表這是 `BigInt`。

例如 ETH 最小單位 wei 常常會用大整數：

```
1 ETH = 1000000000000000000 wei
```

這個數字用普通 `number` 處理不安全，所以會用：

```
1000000000000000000n
```

所以你看到：

```
value: 1n
```

意思只是：

```
這個值是 BigInt 型態的 1
```

不是錯誤，也不是多了一個字母。

對，你這個理解很準。

EAS / 鏈上資料是公開的，所以如果你直接放：

```
note = "Alice completed private medical course..."
```

任何人都可以讀到。  
但如果你只放：

```
evidenceHash = 0xabc...
```

陌生人只能看到一個 hash，看不到原文內容。

而且 `bytes32` 固定只有 32 bytes。真實內容可以很大，例如：

```
一份 PDF
一本書
一張證書圖片
一整份 JSON
一段課程完成紀錄
```

鏈上只存它的 hash。

驗證流程就是你說的：

```
1. 拿到鏈下原始內容
2. 對原始內容做 hash
3. 得到 hashResult
4. 比對 hashResult 是否等於 EAS 裡的 evidenceHash
5. 一樣 = 內容沒有被改過
```

例如：

```
鏈下內容：
Finished EAS basics

hash 後：
0x1234...

EAS 鏈上記錄：
evidenceHash = 0x1234...

比對相同，所以可以證明：
這份鏈下內容就是當初 attestation 指向的內容
```

但有兩個重要補充。

第一，hash 是**驗證完整性**，不是加密。

```
hash 可以隱藏內容
但如果原文很容易猜，就可能被猜出來
```

例如如果原文只有：

```
yes
```

別人可以自己 hash `"yes"`，然後比對你的 hash，就猜到內容。

所以隱私資料不要只靠簡單 hash。可以加 salt：

```
hash(secretSalt + originalContent)
```

或用真正的加密。

第二，hash 只能證明「內容沒變」，不能自動證明「內容是真的」。

它能證明：

```
這份文件 = 當初被 attestation 承諾的那份文件
```

但「這份文件是不是可信」，還要看：

```
誰簽發 attestation？ attester
簽給誰？ recipient
schema 是什麼？
有沒有 revoked？
有沒有過期？
```

所以完整信任邏輯是：

```
hash 驗證內容一致
attester 驗證誰背書
recipient 驗證對象
revoked / expirationTime 驗證是否仍有效
```

一句話：**對，把大內容放鏈下，把** `bytes32 hash` **放 EAS，之後重新 hash 原文來比對，就能驗證內容是否和當初記錄的一樣，同時省 gas 並避免直接公開原文。**
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->















好，下一步學 **讀取一筆真實的 attestation**。

這一步的重點是：**只讀鏈上資料，不寫鏈、不花 gas、不需要私鑰。**

你前面已經知道：

```
schema UID + encoded data -> decode -> 有意義的資料
```

現在我們把它接到真實世界：

```
attestation UID -> 讀取 attestation -> 取得 schema UID 和 data -> decode
```

一筆 EAS attestation 大概長這樣：

```
{
  uid: "0x...",
  schema: "0x...",
  recipient: "0x...",
  attester: "0x...",
  time: 1234567890,
  expirationTime: 0,
  revocable: true,
  revoked: false,
  data: "0x..."
}
```

這裡最重要的是三個欄位：

```
uid      = 這筆 attestation 的唯一 ID
schema   = 這筆資料使用哪個 schema
data     = 依 schema 編碼後的 0x bytes
```

讀取流程是：

```
1. 用 attestation UID 查 attestation
2. 從 attestation.schema 拿到 schema UID
3. 用 schema UID 查 schema string
4. 用 SchemaEncoder(schema string) decode attestation.data
5. 得到真正資料
```

概念上像這樣：

```
const attestation = await eas.getAttestation(attestationUID);

const schemaUID = attestation.schema;
const encodedData = attestation.data;

const schemaRecord = await schemaRegistry.getSchema({ uid: schemaUID });

const encoder = new SchemaEncoder(schemaRecord.schema);

const decodedData = encoder.decodeData(encodedData);
```

這一步你要建立的直覺是：

**EAS 的一筆 attestation 不是只看** `data`**，而是看** `schema + data + attester + recipient`**。**

因為同一串資料，真正有價值的是：

```
誰簽發的？ attester
簽給誰？ recipient
根據什麼格式？ schema
內容是什麼？ data
有沒有過期？ expirationTime
有沒有被撤銷？ revoked
```

所以 EAS 不只是「存資料」，而是「存一份可驗證的聲明」。

下一步如果你要真的動手，就會需要：

```
1. 安裝 SDK
2. 找一個公開的 EAS 合約地址
3. 找一筆真實 attestation UID
4. 寫程式讀它
```

這仍然不需要錢包、不需要私鑰、不花 gas，只需要 RPC provider。
<!-- DAILY_CHECKIN_2026-07-09_END -->

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
