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
# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
\### 2026.07.23

主題：實作篇（一）——Monad Foundry 環境與 gas 行為實測

**環境**

\- Monad Foundry（foundryup --network monad）＋ foundry-monad 模板，Monad Testnet（chain\_id 10143）

\- 帳戶以 keystore 管理，faucet 領測試 MON；部署 Boom.sol 作 revert 測試用（0xE377...c667）

**實驗一：gas limit 收費驗證**

\- 方法：同額自轉帳兩筆，gas limit 分別設 21000 與 300000，比對前後餘額

\- tx1（limit=21000）：price 104.195 gwei，扣 0.0021881 MON——limit 等於實際用量，作為基準組

\- tx2（limit=300000）：price 103.000 gwei，餘額差 30,900,000,000,300,000 wei，

恰等於 300000 × price，一 wei 不差；若按以太坊 used×price 模型應只扣 0.0021630 MON

\- 結論：扣款依 gas limit 計，與 gas pricing 文件一致，實付為必要值的 14.3 倍

\- 額外觀察（文件未明講）：receipt 的 gasUsed 欄回報的是 limit（300000）而非實際用量，

任何依 receipt 估算成本或合約效率的工具在 Monad 上讀到的其實是 limit

!\[實驗一：balance 與 receipt 對照\](./images/exp1\_receipts.png)

!\[gas 收費模型對比\](./images/gas\_comparison.png)

**實驗二：revert 交易的成本**

\- 對 Boom.boom()（必定 revert）以 gas limit 200000 硬送

\- 結果：交易上鏈、status 0 (failed)、revertReason "boom"（Error(string) selector 0x08c379a0，

Monad Foundry 的 trace 解碼直接還原字串）

\- 扣款 = 200000 × 105 gwei = 0.021 MON，全額按 limit 計

\- 對應文件警告的錢包情境：estimateGas 遇 revert 時部分錢包會自動塞大 limit，

在以太坊上多付有限，在 Monad 上是照 limit 全額實付

!\[實驗二：revert 交易 receipt\](./images/exp2\_revert.png)

**實驗三：RPC 固定值**

\- eth\_maxPriorityFeePerGas 回傳 0x77359400 = 2 gwei，與 RPC differences 文件所述固定值一致

\- 三筆交易 effectiveGasPrice 為 103.0～105.0 gwei，即 base fee 約 101～103 gwei 且逐塊浮動，

與 gas pricing 頁的動態 base fee 控制器描述相符

小結：三項實測全部與文件一致。開發面結論——在 Monad 上 gas limit 不是安全餘裕而是直接成本參數，

必須顯式管理；靠 estimateGas 自動填 limit 的流程（尤其 revert 時塞大值的錢包行為）是真實的資損來源；

receipt gasUsed 語意與以太坊不同，讀數工具需要重新校準。
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->

先打個卡
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->


主題：交易生命週期總覽（串接前面各篇）

**1\. 建構與簽署**

-   前端組好交易後交給錢包簽署；多數錢包以 `eth_estimateGas` 自動填 gas limit，用戶可手動覆寫；gas price 由用戶指定，單位是每 gas 的原生代幣數
    
-   銜接 gas pricing 篇：Monad 按 limit 全額收費，錢包在 estimateGas 遇 revert 時塞大 limit 的行為，在這個階段就決定了實際成本風險
    

**2\. 提交與轉發**

-   交易經 `eth_sendTransaction` / `eth_sendRawTransaction` 送入 RPC 節點，該節點成為 owner node
    
-   RPC 做靜態有效性檢查（簽章、nonce、gas limit 不超過單筆上限等）後，轉發給接下來 N 個 leader；各 leader 重複同樣檢查後放入自己的 local mempool
    
-   若這批 leader 出的塊都沒收錄，owner node 換下一批 N 個 leader 重送，最多 K 輪（主網 N、K 皆為 3）
    
-   銜接 local mempool 篇：全程沒有全域 mempool
    

**3\. 進塊前的動態檢查**

-   leader 選入交易前確認帳戶餘額足以支付 gas，依據是共識時的餘額驗證規則（含 reserve balance）
    
-   銜接 reserve balance 篇：這筆保留額就是 async execution 下共識端敢在未執行狀態收單的擔保
    

**4\. 傳播與終局**

-   leader 出塊，經 RaptorCast 對外廣播
    
-   區塊依序 Proposed →（1 塊後）Voted →（2 塊後）Finalized；Finalized 即代表交易正式寫入歷史
    
-   文件的關鍵表述：順序一旦確定，交易的真值（成功或失敗、結果為何）即已確定，執行只是把它算出來
    
-   銜接 MonadBFT 篇：Voted 即具投機終局性，回滾僅限可證明的 leader equivocation
    

**5\. 執行與結果**

-   節點收到區塊即可開始投機執行，不需等 finality
    
-   樂觀平行執行、依原順序序列提交，結果等同逐筆序列執行
    
-   全網執行一致性由 D=3 的 delayed merkle root 驗證，對應 Verified 狀態；receipt 與 log 隨執行產出，經 RPC 對外提供
    
-   銜接執行層篇：這段的效率來自 parallel execution 與 MonadDb 的組合
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->



假日打卡
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->




假日請假
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->





2026.07.16

主題：節點同步與網路層（statesync、blocksync、peer discovery）

**Statesync**

-   定位：Monad 吞吐量下，從創世重放歷史不可行；節點初始化改為向其他節點複製近期狀態，只重放最後一段
    
-   角色：同步中的節點為 client，向已同步的 validator（server）請求資料；server 依 MonadDb 節點級版本 metadata 快速鎖定需要傳送的 subtrie，讀取請求高度可平行
    
-   範圍：MonadDb 儲存的資料中，僅「參與 active set 所需」的子集會納入 statesync
    
-   分工：資料切成 chunk，每個 chunk 綁定一個 server 直到同步完成；server 從可用 peer 隨機選取，client 同時維持設定上限內的 session 數；server 無回應則 timeout 改派其他節點
    
-   順序：client 從最久未更新的狀態開始請求、往最新收斂；server 以 client 當前最新區塊為基準只傳 diff
    
-   收斂流程：tip 是移動目標，statesync 期間新進區塊先暫存；一輪結束後若 target 距 tip 小於 statesync\_threshold（預設 600 塊），statesync 結束並驗證 state root，剩餘區塊改走 blocksync 補齊；距離仍大則再跑一輪
    
-   信任模型：現階段 client 信任 server 回傳的資料（含 state root、parent hash）；server 依 stake weight 從驗證者集隨機抽樣，也可白名單指定已知供應者；逐 server 驗證機制實作中，屆時遇到壞資料只需丟棄並重試受影響的 prefix
    

**Blocksync**

-   定位：補齊缺失區塊；判定方式為觀察到 QC 引用了本地不存在的區塊
    
-   兩種觸發場景：RaptorCast chunk 收不足導致漏塊；statesync 完成後追 last mile
    
-   節點恢復、full node 升格 validator 等維運流程的停機時間，主要就取決於 statesync／blocksync 的耗時
    

**Peer Discovery**

-   用途：新 validator 或 full node 加入網路時連上既有節點，開始接收共識訊息並追上鏈頂
    
-   身分結構：節點產生 MonadNameRecord，內含 NameRecord 與以 secp 金鑰對其做的簽章；NameRecord 記錄 IPv4 位址、port 資訊與序號，目前僅支援 IPv4
    
-   兩種 wire format：舊版所有協定共用單一 port；新版支援依協定（TCP、UDP 等）分別標記的多 port
    

**補充：區塊傳播機制對比（FAQ）**

-   Ethereum 走 libp2p gossip，重複訊息多、路徑迂迴，區塊傳播預算以秒計
    
-   Solana Turbine 與 RaptorCast 同屬 erasure coding、切 MTU chunk、broadcast tree 的做法；差異在 Monad 用 Raptor code（Turbine 用 Reed-Solomon）、Monad 是兩層樹且所有其他 validator 皆為第一層節點（Turbine 樹較深、結構較鬆）、Monad 對區塊送達有 BFT 保證
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->






2026.07.14

主題：執行層架構（asynchronous execution、optimistic parallel execution、JIT、MonadDb）

**Asynchronous Execution**

-   核心設計：共識只對交易順序達成協議，不等執行結果；區塊 finalize 後才正式執行，共識同時繼續推進後續區塊
    
-   對比 interleaved execution（以太坊模式）：leader 提案前要先執行、驗證者投票前也要執行，執行預算只占 block time 的一小部分，其餘全花在跨洲通訊；async execution 把接近完整的 block time 讓給執行
    
-   一致性驗證：區塊 N 的提案攜帶區塊 N−D（D=3）的 delayed merkle root，各節點以此偵測執行分歧，對應 Verified 狀態
    
-   節點收到提案即可 speculative execution，不必等 finality；回滾安全性部分來自 MonadDb 以 merkle trie diff 形式儲存，捨棄投機結果成本低
    
-   昨天前面看過的 gas limit 收費、Reserve Balance 都是此設計的衍生配套
    

**Optimistic Parallel Execution**

-   保證：區塊內交易線性排序，最終狀態等同逐筆序列執行；平行化純屬實作細節，合約端不需配合改寫
    
-   機制：計算與提交分離。交易先樂觀地平行執行，產出 pending result（記錄讀入的 input slots 與寫出的 output slots 及其值）；再依交易順序序列提交，提交時檢查 inputs 是否仍有效，被先前提交改動過就重新執行，完成前後續交易不得提交
    
-   每筆交易至多執行兩次：一次樂觀執行、至多一次提交時重執行；重執行通常便宜，因為所需 slot 多半已在快取，只有走到不同 codepath 才需要新的 SSD 讀取
    
-   可理解為 two-pass：第一遍平行執行把 storage slot 依賴全部拉進快取，第二遍序列提交
    
-   狀態競爭以 slot 為單位判定，不是以合約為單位：同一個 ERC-20 合約內兩筆不相干的轉帳互不衝突
    
-   官方不強制 EIP-2930 access list：長期瓶頸在頻寬，slot 依賴的發現交給系統底層處理
    

**JIT Compilation**

-   執行端同時具備高度優化的 interpreter 與客製 native compiler
    
-   追蹤最常被呼叫的合約，非同步編譯成原生碼，後續呼叫直接跑編譯結果
    
-   完整保留 EVM 語意，含 gas 計算與錯誤行為
    
-   不走 LLVM IR / Cranelift 等中間層，直接輸出 x86-64；理由是客戶端鎖定特定硬體規格，換取最大控制與效能
    

**MonadDb**

-   客製 key-value 資料庫，原生實作 Patricia trie（磁碟與記憶體皆是），儲存狀態、receipts、區塊頭與 payload 等 authenticated data
    
-   對比既有客戶端：MPT 塞進 B-Tree（LMDB）或 LSM-Tree（LevelDB/RocksDB）等通用資料庫，等於資料結構裡再包一層資料結構；原生實作消除這層間接，單次查詢的 IOPS 與 page read 大幅減少
    
-   async I/O 以 io\_uring 實作，讀取不阻塞執行緒、不需生成大量 kernel thread；平行執行同時發出大量狀態讀取，必須配這種資料庫才撐得住
    
-   繞過檔案系統，避開 block allocation、碎片化、讀寫放大、metadata 管理等隱性成本
    
-   persistent（immutable）trie 設計：更新時沿分支產生新版本節點、舊版本保留。帶來三個效果——節點級版本化、單一 writer（execution）對多 reader（consensus、RPC）的同步簡化、寫入變成 SSD 友善的循序寫
    
-   磁碟有限，僅保留近期版本並依可用空間動態調整歷史長度，配合 inline compaction 回收空間（對應前幾天 historical data 的 40,000 區塊回看限制）
    
-   節點版本化同時支撐 statesync：同步節點提供目前版本與目標版本，對方只需傳送差異的 trie 元件；缺塊另走 blocksync 補齊
    

小結：執行層四個元件互相咬合——async execution 爭取時間預算、parallel execution 吃滿 CPU、MonadDb 吃滿 SSD 並提供版本化基礎、JIT 壓低單筆計算成本；投機執行與快速回滾的安全性建立在 trie diff 的資料模型上。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->







### 2026.07.13

主題：共識層架構（MonadBFT、區塊狀態、RaptorCast）

**MonadBFT 基本屬性**

-   衍生自 pipelined HotStuff 家族的 BFT 協定，n = 3f+1，容忍 f 個拜占庭節點
    
-   單輪達成投機終局性（speculative finality），兩輪達成完全終局性
    
-   happy path 為線性訊息與簽章複雜度，支撐上百節點的共識集
    
-   optimistic responsiveness：依實際收到的訊息推進回合，不依賴固定 timeout；僅在故障恢復時才使用較重的通訊
    
-   出塊間隔 0.4 秒；區塊 N 於 N+2 提案時完成 finalize，約 0.8–1 秒
    

**Pipeline 運作方式**

-   每輪一位 leader；提案內容 = 新 payload + 前一提案的 QC（由前輪投票聚合而成）
    
-   驗證者驗證提案後，直接將票送給下一輪的指定 leader
    
-   驗證者收到第 K+2 輪提案（內含對 K+1 的 QC，即對 K 的 QC²）時，將第 K 輪區塊標記為 Finalized、K+1 為 Voted、K+2 為 Proposed
    
-   每一輪同時推進三個高度的區塊狀態
    

**Tail-forking 問題與對策**

-   定義：pipelined 協定中，前一區塊的第二階段訊息搭載在下一位 leader 的提案上；若下一位 leader 離線或作惡，即使前一區塊已獲超級多數投票仍會被丟棄
    
-   影響：誠實 leader 損失出塊獎勵；產生跨區塊重排交易的 MEV 空間；讀取 Voted 狀態的應用會遇到回滾
    
-   MonadBFT 規則：接任 leader 必須 repropose 已獲足夠支持的前一區塊；欲提出全新區塊，須附上超級多數簽署的 No-Endorsement Certificate（NEC），證明前一區塊確實未獲足夠支持
    
-   若接任 leader 完全未提案，該輪 timeout，由再下一位 leader 接手
    

**區塊狀態進程**

-   Proposed → Voted → Finalized → Verified
    
-   Voted：提案後約 0.5 秒；此狀態僅在 leader equivocation（可證明並可懲罰）時回滾，實務上可視為近似安全
    
-   Finalized：兩輪後，交易順序不可變；Monad 的終局性定義在共識層，不等執行
    
-   Verified：Finalized 後再過 D=3 個區塊，delayed merkle root 確認全網執行結果一致（state root finality）
    
-   文件建議：涉及大量鏈下金融邏輯的系統（交易所、跨鏈橋、穩定幣/RWA 發行方）應以 Verified 為準
    

**RaptorCast**

-   問題：MonadBFT 要求 leader 直接送區塊給所有驗證者；以 10,000 tx/s × 200 bytes 估算約 2 MB/s，直發 200 個驗證者需約 400 MB/s（3.2 Gbps）上傳頻寬，不可行
    
-   解法：區塊經 erasure coding 切成多個 chunk，chunk 總量大於原始資料，但以幾乎任意組合湊滿原始大小即可還原
    
-   傳播採兩層 broadcast tree，每個其他驗證者皆為第一層節點，負責轉發部分 chunk
    
-   傳輸層用 UDP，以 Raptor code 補資料完整性、對 merkle root 簽章做訊息認證，整體效率優於 TCP
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->








假日水個打卡
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->









**Historical Data**

**•** 先把兩種資料分開：ledger data（區塊、交易、receipt、event、trace）和 state data（交易依序套用後的世界狀態，存在 merkle trie）。Monad full node 提供完整歷史 ledger data，但不提供任意舊區塊的 state

**•** 這頁直接把以太坊的節點分類作廢了：以太坊 full node 留最近 128 個區塊的狀態、archive node 存到創世；Monad 的每個 full node 都是「盡力而為的 archive node」——歷史 state trie 能留多少看磁碟多大。2 TB SSD 目前大約回看 40,000 個區塊，實際取決於每塊 state diff 的量

**•** 為什麼沒有服務商提供完整歷史 state：單塊最多 5,000 筆交易（以太坊約 200 筆的 25 倍）、0.4 秒出塊（12 秒的 30 倍），changeset 是完全不同的量級。硬體上不是不能做，是目前沒人做

**•** 實務結論：需要日後查的狀態，合約設計階段就要用 event 記錄，或交給 indexer 離線重建。臨時想用 eth\_call 帶舊 block number 回查，超過 trie 保留範圍就直接報錯

**RPC Differences**

**•** eth\_maxPriorityFeePerGas 目前寫死回 2 gwei，eth\_feeHistory 也只回預設值，文件標注是暫時的。意思是現階段的 fee 估算邏輯不能依賴這兩個方法，要自己從區塊資料算

**•** newHeads / logs 訂閱永遠不會出現 reorg，因為只推送 finalized 區塊的即時資料。另外有 Monad 專屬的 monadNewHeads / monadLogs 訂閱，走更低延遲的路徑

**•** debug\_trace\* 系列必須顯式帶 trace options 參數，省略會回 -32602，跟一般 EVM client 把它當可選參數的行為不同
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->










先打卡晚點補充筆記
<!-- DAILY_CHECKIN_2026-07-10_END -->

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
