---
timezone: UTC+8
---

# james_1022

**GitHub ID:** ARZER-TW

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
**Local Mempool**

-   為什麼不用全域 mempool：gossip 協定在幾千 TPS 下光是各節點重傳就會吃光頻寬預算；多跳傳播拉長 time-to-inclusion；而且 leader schedule 本來就預先可知，廣播給全網是浪費。所以改成定向轉發
    
-   交易流程：交易送進某個 RPC 節點，這個節點成為它的 owner node，負責向用戶回報狀態 → RPC 做靜態檢查 → consensus process 再對 MonadDb 的本地狀態做動態檢查（餘額、nonce）→ 合法就轉發給接下來 N=3 個 leader → 每個 leader 同樣檢查後放進自己的 local mempool，輪到自己出塊時從裡面選交易
    
-   重送機制：owner node 盯著後續區塊，N 個區塊內沒看到這筆交易就再轉發給下一批 N 個 leader，最多重試 K=3 輪（主網 N、K 都是 3）
    
-   淘汰規則：區塊 finalize 時清掉已上鏈的複本；定期重驗交易有效性（nonce 太低、餘額不足就踢）；mempool 逼近軟上限時先淘汰舊交易
    
-   附帶意涵：pending 交易只存在接下來幾個 leader 的 local mempool 裡，沒有公開 mempool 可監聽。做套利的話，以太坊那套盯 mempool 的資訊來源在這裡不存在，競爭面被壓縮到 leader 端
    

**EIP-7702 × Reserve Balance**

-   這頁回答了我昨天的疑問：為什麼 7702 委託會跟 reserve balance 綁在一起。未委託的 EOA，consensus 靜態看交易就能算出最大花費；委託之後，EOA 的錢可能被「別人送的交易」經由 delegate code 花掉，共識層靜態分析不到
    
-   解法是一份 consensus 與 execution 的契約：執行端保證每個 EOA 結餘不會低於 user\_reserve\_balance（10 MON），共識端就能在只看 inflight gas 預算的情況下安全收單
    
-   "dips below" 語意很精確：只有「餘額遞減且結果低於 10 MON」的交易會 revert。餘額不變或增加的交易沒事，結餘剛好停在 10 MON 以上也沒事
    
-   未委託帳戶有 emptying transaction 例外，可以把錢花到見底（例如餘額 5 MON 轉出 4.99 加 gas），前提之一是最近 k 個區塊內沒有委託或解除委託的請求。委託中的 EOA 不適用這個例外
    
-   實務坑：很多 sponsored gas 流程讓用戶的 7702 帳戶完全不持有 MON，這類設計搬到 Monad 要重新想 10 MON 下限怎麼處理
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

**2026.07.07**

接續昨天的 differences 總覽，今天細讀 Gas Pricing 和 Opcode Pricing 兩頁，順便補 Reserve Balance 的機制。

**Gas Pricing**

-   收費公式：gas\_paid = gas\_limit × price\_per\_gas，按 limit 收不退差額。理由昨天寫過：非同步執行下 leader 出塊時還沒執行，若按 gas\_used 收費，攻擊者可以用大 limit 低實耗的交易便宜佔滿區塊，是 DoS 向量
    
-   支援 EIP-1559：price\_per\_gas = min(base + priority, max\_price)，跟以太坊介面一致
    
-   base fee controller 跟以太坊不同：用指數型控制器，且步長 η 會依近期區塊用量的波動自適應調整。設計成「漲得慢、跌得快」，避免 base fee 高估造成區塊空間閒置。目標使用率 80%（160M / 200M）
    
-   幾個常數：block gas limit 200M、單筆交易上限 30M、最低 base fee 100 MON-gwei
    
-   預設交易排序是 Priority Gas Auction，按總 gas price 降冪
    
-   開發建議：gas 成本固定的操作（如 21000 的原生轉帳）直接寫死 gas limit，別叫 eth\_estimateGas。文件特別警告 MetaMask 的行為——estimateGas 遇到 revert 會直接塞一個超大 limit，在以太坊上無感，在 Monad 上是照 limit 全額扣款，等於幫用戶多付一大筆
    

**Opcode Pricing**

-   定價哲學：與其把幾乎所有 opcode 降價，不如把少數幾個調漲，相對效果等同全面打折。改動最小化
    
-   cold access 調漲約 4 倍（反映他們的執行客戶端裡，磁碟讀 state 相對運算變貴了）：
    

|   | Ethereum | Monad |
| cold account | 2600 | 10100 |
| cold storage | 2100 | 8100 |

warm access 維持 100 不變。受影響的是 BALANCE、EXTCODE 系列、CALL 系列、SLOAD、SSTORE

-   precompile 重新定價：ecRecover 2×、ecAdd 2×、ecMul 5×、ecPairing 5×、blake2f 2×、point evaluation 4×。這條對我影響最直接——Groth16 鏈上驗證主要就是吃 ecPairing 和 ecMul，等於在 Monad 上驗證 ZK proof 的相對成本比以太坊高不少，之前對 verifier gas 的直覺要重新校準
    

**Reserve Balance 補充**

-   每個 EOA 固定保留一筆餘額（10 MON）只能付 gas，用來覆蓋接下來 k=3 個區塊內的 inflight gas fee
    
-   共識端：leader 只收 inflight gas fee 未超額的交易；執行端：若交易會把餘額打到 10 MON 以下（例如把 MON 全換掉），交易 revert 但 gas 照扣
    
-   這就是為什麼執行可以落後共識三個區塊還能保證付得起錢
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->


今天簡單的掃過了 Monad 文件裡整理的與 Ethereum 差異清單，記幾個有感的點。

最值得想的是收費模型：Monad 按 gas limit 收費而不是實際用量，總扣款是 value + gas\_bid × gas\_limit。第一眼覺得反直覺，但回推架構就通了——非同步執行下，共識階段還不知道實際 gas 用量，只能拿 limit 當上限防 DoS。等於架構選擇的成本反映在收費上，以後在 Monad 上預估 gas limit 要抓緊，不能像以太坊那樣隨手開大。

Reserve Balance 是同個脈絡的產物：為了保證進共識的交易付得起錢,所以鏈上會看到「已上鏈但因餘額不足而 revert」的交易，gas 照扣。文件強調這不算協議層差異（以太坊本來就有 revert 交易上鏈），但跟一般預期確實不同。

其他幾點簡記：

-   合約大小上限 128 KB（以太坊 24 KB），init code 上限 256 KB
    
-   部分 opcode 和預編譯重新定價，反映 Monad 優化後的資源稀缺性變化
    
-   原生支援 secp256r1 (P256) 預編譯（位址 0x0100，EIP-7951），可直接在鏈上驗 WebAuthn / passkey 簽章，做帳戶抽象不用再用合約硬刻 P256 燒 gas
    
-   不支援 EIP-4844 blob 交易
    
-   沒有全域 mempool，交易只轉發給接下來幾個 leader。交易傳播模型跟以太坊完全不同，對做套利的人影響很大
    
-   EIP-7702 委託過的 EOA 有兩個限制：餘額不能低於 10 MON（綁 Reserve Balance 規則）、被當合約呼叫時禁用 CREATE/CREATE2
    
-   吞吐量太高導致 full node 不提供任意歷史狀態查詢
    

整體感想：這些差異幾乎都能回推到同一個根源——非同步、平行執行的架構。gas limit 收費、Reserve Balance、local mempool、砍歷史狀態，全是執行與共識解耦後為了讓系統撐住的配套。EVM 相容只是介面層，底下的資源模型已經是另一套東西。要搬合約過去，gas 預估和任何依賴 mempool 行為的邏輯都得重新驗，本地開發記得用官方客製的 Monad Foundry 對齊鏈上行為。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
