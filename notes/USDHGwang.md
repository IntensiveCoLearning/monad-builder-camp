- GitHub ID: 215296031
- Name: USDHGwang
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-26
<!-- DAILY_CHECKIN_2026-07-26_START -->
# 2026-07-26

\# Day 21 — 被打回七項，然後去讀自己送出去的東西

把 Week 2 卡住的四個任務清掉。開任務詳情才發現「完成第一次 GitHub 協作 +50」

和「向 Moss 提交第一個 PR +60」是互斥的，只能選一個，我之前算的 260 學分不成立。

最後交了 +60、+100、+50 三項。

nishuzumi 給了 review，CHANGES\_REQUESTED，七項 blocking。

他不是掃一眼打回。review 裡寫了他自己跑過 lint / build / typecheck / test:offline

和我全部的 live test，重跑 keyed update:abis 確認 ABI bytes 沒變，還用一個有錢的

EOA 手動模擬了一次 redeem。

機械層的東西是對的，calldata 正確、ABI 抓對了。七項全部落在另一層：這東西會不會

出錯、它產出的證據到底證明了什麼。

最具體的是 Receipt 那項。Moss 框架有一個 verifyReceiptCoverage，檢查我有沒有把

模擬產生的每一條 Change 都認領完、順序對不對、握的是不是原始物件。我的 parser

過得了這個檢查，因為每條都被認領了。但我沒有檢查事件是不是 shMONAD 合約發出來的，

native transfer 也只比金額不比 from/to。一個不相干的同簽章事件加一筆金額剛好的轉帳，

就能組出一份看起來權威的結果。框架只檢查有沒有漏認，認得對不對是 adapter 自己的

責任，這件事我當時沒想到。

另一項是 README 宣稱 default CLI 會 expose 這個 adapter，但 package 還是 private，

也沒被 compose 進 defaultProtocolModules。文件寫了一個實際不成立的事。

今天剩下的時間花在把整條 Moss 流程弄清楚：discover / load / action / simulate 各做

什麼、MCP server 是常駐進程不是腳本、LLM 其實不執行任何東西，只是輸出文字由 host

去打 JSON-RPC。
<!-- DAILY_CHECKIN_2026-07-26_END -->

# 2026-07-25
<!-- DAILY_CHECKIN_2026-07-25_START -->
# 2026-07-25

\# 2026-07-25

<!-- DAILY\_CHECKIN\_2026-07-25\_START -->

\## Day 20｜unstake 這個動作有時間維度

讀 shMONAD 完整 ABI 的時候看到一組沒預期的東西：requestUnstake、CompleteUnstake、ManualUnstakeInitiation、ValidatorUnstakeRequested。FastLane 的贖回不只有 ERC4626 的 redeem，底下還有一套從 request 到 complete 的 unbonding 流程。

我的 adapter v1 走 redeem，在合約層這是對的。但把它包成 agent 的一個 action 叫「unstake」，問題就出來了。使用者聽到 unstake，預期是錢現在回來。實際上看協議狀態，可能是請求送出去了、錢晚點回來。同一個動詞，兩種結果。

使用者授權的是「贖回我的 shMON」，agent 執行的是一筆具體交易。中間差的是時間性有沒有被包含在那個授權裡。沒講清楚的話，agent 每一步都照做，結果仍然超出使用者以為自己同意的範圍。

DeFi 協議把 unbonding delay 當常識，文件寫一行就算交代。但 agent 介面會把這些狀態壓成一個動詞。Week 3 如果做 staking 的 demo，這是我想處理的點：執行前給使用者看的不只是金額，還有錢什麼時候回得來。
<!-- DAILY_CHECKIN_2026-07-25_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
# 2026-07-24

\# 2026-07-24

<!-- DAILY\_CHECKIN\_2026-07-24\_START -->

\## Day 19｜驗證器成本這條軸，鏈上鏈下是同一個問題

今天把一份 3 小時的訪談過完（姚順雨，在 Anthropic 訓過 Claude 3.7/4.5、現在在 Google DeepMind 訓 Gemini 3 的研究員），蒸餾成筆記，發現它跟我 Week 1 那份 agent-carrier 的核心是同一個問題。

他講 AI 為什麼能快：因為它能做實驗、有客觀的回饋信號。他自己從高能物理轉出來，理由是物理理論到了實驗追不上的地方就沒有客觀評價標準，對錯只能靠領域裡的人主觀判斷。同一件事在他嘴裡是「這個領域能不能驗」，在我 Week 1 第 3 節那張表裡是「鏈怎麼知道 off-chain 沒作弊」：Bittensor 繞開驗算力改驗產出、opML 用挑戰期、zkML 用密碼學證明、TEE 用硬體信任，都是同一個問題的不同代價。

這讓 Week 1 第 5 節的判斷更硬：agent 上鏈的卡點在驗證與授權這一層，TPS 早就夠用。agent 是個黑盒，從外面看不出它學到什麼、下一步會不會被騙著做越權的事。所以它跑在鏈上，真正需要的是一層能在外面驗它有沒有越過使用者授權邊界的東西，跑得快只是前提。這也是我自己研究方向存在的理由。
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# 2026-07-23

\# 2026-07-23

<!-- DAILY\_CHECKIN\_2026-07-23\_START -->

\## Day 18｜把 adapter 從「能跑」做到「能 merge」

今天把 shMONAD adapter 的 ABI 補成 ADR 0007 合規，開了 PR：[https://github.com/nishuzumi/moss/pull/128](https://github.com/nishuzumi/moss/pull/128)

ABI 從完整 verified 版抓，不是手寫 trim。shMONAD 是 proxy，explorer 打 proxy 只回 proxy 自己的 ABI，要打背後的 implementation 地址才拿到真正的 staking ABI。抓下來 301 個 entry。原本擔心這麼大 tsc 會撐不住，實測 typecheck 10 秒、build 20 秒，沒事，所以保留完整 ABI。

型別 fixture 原本假設 ABI 沒有 requestUnstake，結果完整 ABI 真的有。FastLane 不只 ERC4626 的 redeem，還有一整組 request/complete 的 unbonding 流程。手寫時的假設跟真實 ABI 對不上，改掉。

最該記的是：開 PR 前又犯了一次上週的錯。我從 local main 開 branch，但 upstream 已經往前好幾個 commit，基底是舊的。跟 Day 15 同一個教訓，這次是我自己沒先 sync。把 branch 重放到最新 upstream、在新基底重驗一次才開 PR。

多花的時間都在 provenance 和 sync 上，不是寫新功能。這步做完，Week 2 那四個卡住的任務能交了。

<!-- DAILY\_CHECKIN\_2026-07-23\_END -->
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
# 2026-07-22

\# 2026-07-22

<!-- DAILY\_CHECKIN\_2026-07-22\_START -->

\## Day 17｜這個 adapter 一路連到 hackathon

想把 shMONAD adapter 開成 PR，卡在 ABI 合規。Moss 的 ADR 0007 要完整 ABI，不收手動 trim 的版本。兩條合規路徑：從官方 npm SDK 拿現成 ABI JSON，或從 explorer 拉 verified 合約的 ABI。

FastLane 的官方套件 @fastlane-labs/fastlane-contracts 只 ship Solidity 原始碼，沒有編譯好的 ABI JSON，第一條走不通。shMONAD 又是 proxy 合約，ABI 得從它背後的 implementation 地址拿，不是 proxy 本身。所以只剩 explorer，要 Monadscan API key。

另一件事：去確認 Week 4 hackathon 的結構，才看清這個 adapter 不是拿學分的雜活。Week 2 寫 adapter、Week 3 用它組隊做 Moss demo、Week 4 帶進 hackathon，是同一條線。平台推薦的 demo 方向裡就有一個 Staking Assistant，剛好我這個 adapter 能驅動。

決定不走 trimmed ABI 的捷徑，先把 ABI 弄合規再開 PR。多花的時間會回到 Week 3 demo 和 hackathon 上。

<!-- DAILY\_CHECKIN\_2026-07-22\_END -->
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
# 2026-07-21

今天沒寫 code，花時間把「為什麼模型輸出需要外部驗證」從機制層拆開。起點是一支 AI 科普影片加楊植麟的 K2 訪談，過程跟 Claude 逐條核對理解，錯的當場被抓（驗證器是訓練迴圈內的東西，我原本以為是訓練完的考核；推理時我以為參數還會微調，實際上全程凍結）。

搞清楚的幾件事：

-   訓練是擬合。人設計的是架構、損失函數、更新規則，參數的最終數值是程序跟資料互動找出來的，沒有人寫過、也沒有人讀得懂。黑盒的黑就在這一層。
    
-   推理期參數凍結。模型看起來會適應，是因為輸入變了（in-context learning），權重從頭到尾沒動。
    
-   自我校驗抓不到系統性錯誤。檢查者和生成者是同一組參數，同源就同錯，怎麼檢都過，而且信心十足。
    
-   所以檢查有效的條件是訊號來源獨立於生成者。按獨立性排：同模型重跑 < 跨模型交叉 < 固定規則和測試 < 人工判斷。
    

寫完發現這跟 Day 14、15 是同一件事。Moss 的 ADR 和 DoD 就是放在貢獻者外面的固定規則驗證器，我的測試全綠是自檢，合不合規要看外部標準。harness 的每個檢查點，實際上都在選用哪一級獨立性。
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->
# 2026-07-20

\# Day 15｜shMONAD Protocol Adapter

今天把 Moss 的 shMONAD protocol adapter 從 template 往可用版本推進。

完成內容：

1\. 建立 shMONAD adapter，接入 FastLane shMONAD 合約。

2\. 實作 stake MON → shMON，以及 unstake shMON → MON。

3\. 加入 balanceOf 和 exchangeRate query。

4\. 實作 Deposit / Withdraw receipt parser，並驗證 native MON transfer、ERC20 Transfer event 與資產數量是否一致。

5\. 補上 stake / unstake 的正常流程、錯誤流程和負面測試。

本地測試結果：6 個 offline tests 通過。另有 2 個 live Monad mainnet tests 因目前環境無法連線到 [https://rpc.monad.xyz，未能執行，不能把它們算成通過。](https://rpc.monad.xyz，未能執行，不能把它們算成通過。)

今天比較明確地理解到，protocol adapter 不只是把合約 ABI 包成幾個 function。它還要定義 capability 的參數、風險標籤、token flow，以及交易完成後如何從 receipt 還原成可驗證的結果。這部分和我之前做 EIV / AIP 時關注的 execution verification 有直接關係。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
# 2026-07-19

shMONAD adapter 昨天寫完跑過 mainnet，今天回頭看整個過程，注意到一件事：在 Moss repo 裡貢獻程式碼的體驗，和在自己的 EIV/AIP repo 裡寫東西完全不同。

Moss 有 11 份 ADR 記錄架構決定，有 [CONTRIBUTING.md](http://CONTRIBUTING.md) 列 Definition of Done 清單，有 [protocol-onboarding.md](http://protocol-onboarding.md) 從零走一遍流程，有 \_template 可以直接複製。每一步該做什麼、做到什麼程度算完成，都有人替你想過了。我照著走，三小時出一個通過 live test 的 adapter。

回頭看自己的 EIV repo：沒有 onboarding 文件，沒有 contributor 指引，ADR 為零。如果今天有人想貢獻一個新的 chain adapter 或新的 verification rule，得先花時間讀完整個 codebase 才能開始。我自己能寫是因為上下文都在腦子裡，但這東西不 scale。

一個具體的對比：Moss 的 ADR 0007 規定 ABI 不能手寫，必須從 explorer 或 upstream package 拿，來源寫在檔案頭。我的 EIV 裡的 ABI 就是手抄的，沒有標來源。如果哪天合約升級，沒有人知道那份 ABI 是從哪裡來的。

看到一個成熟度更高的 repo 怎麼處理同類問題，就知道自己下一步可以補什麼。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
# 2026-07-18

今天開始寫 Moss protocol adapter。第一步就踩坑：原本打算寫 WMON adapter，結果翻 repo 才發現 `@themoss/system` 已經有了。計劃直接作廢，重新在 GitHub issues 裡找目標。

最後挑了 FastLane shMONAD staking（issue #12），maintainer 標了 `good first issue`，說形狀接近 WMON。實際寫的時候發現「形狀接近」有具體含義：單一合約、native token 進出、receipt token 回來。但 shMONAD 用的是 ERC4626 vault standard，deposit 函數多了一個 receiver 參數，unstake 有 atomic 和 queued 兩條路，receipt parser 要處理 Deposit/Withdraw/Transfer 三種 event 交錯出現。「形狀接近」讓你能套模板開始，但細節全是自己的。

寫 receipt parser 的時候有一個發現：Moss 要求每個 Capability 配一個 Receipt，收到 simulation 產生的所有 event，逐一 decode 並驗證數量和順序。這件事和我在 EIV 做的很像。EIV 是事後拿 transaction trace 驗 agent 有沒有越權，Moss receipt 是事前拿 simulation trace 確認交易行為符合預期。同一套 trace 資料，一個朝前看，一個朝後看。

從讀完 [protocol-onboarding.md](http://protocol-onboarding.md) 到 8 個 test 全過（包含 live Monad mainnet simulation），花了大概三小時。中間最花時間的是搞 ABI 來源：ADR 0007 不接受手寫 ABI，要從 explorer 的 verified contract 拿，還要記來源和日期。這種規矩在讀文件的時候覺得囉唆，自己寫的時候才理解它在防什麼。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->
# 2026-07-17

今天把技術、研究、運營三個賽道的任務都交了。十份文件寫的都是 Moss 和 agent authorization，但寫給不同讀者的時候，自己對材料的理解會被迫重組。

研究賽道要我拆解 ERC-7579。這個標準我在 AIP 專案裡用了快一年，以為很熟。但要按 Reading Card 格式寫「背景問題 → 核心方案 → 爭議風險」的時候，發現自己一直在用 hook 的部分，對 validator 和 fallback handler 的設計取捨其實沒認真想過。寫的過程補了這塊。

運營賽道要設計一場 Space。技術內容我都知道，但要寫成「讓沒有技術背景的人也能聽懂」的活動文案時卡了一下。最後發現有效的方式是從場景進去（「你敢讓 AI 幫你做鏈上交易嗎？」），技術細節變成回答這個問題的工具，不是主角。

一個收穫：對一個主題的理解深度，可以用「你能不能對完全不同背景的人講清楚」來測試。講不清楚的地方通常是自己也沒真正想透的地方。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
# 2026-07-16

# **Day 11 — 一份深入，四個賽道的產出**

今天做了一件事：把 Week 2 所有賽道的任務全部打開看過一遍，然後發現一個有趣的現象——我在 Moss 和 agent authorization 上的積累，可以直接轉化成四個不同賽道的產出。

技術賽道要一份 AI-assisted Dev Plan，我寫的是 Moss protocol adapter 開發計劃。研究賽道要一份 EIP Reading Card，我拆解了 ERC-7579（這是我 AIP 專案的核心標準，讀過很多遍了）。運營賽道要一個 Space 策劃案，我設計了一場「AI Agent 操作鏈上協議的風險」討論。通用賽道的 Moss 介紹和教程，之前已經寫好了。

同一塊知識，技術面寫成開發計劃，研究面寫成結構化分析，運營面寫成活動設計。每個面向寫的時候會逼你從不同角度重新組織同一套資訊——給開發者看的要寫清楚 what to mock，給研究者看的要寫清楚 gap 在哪，給一般聽眾看的要用場景帶入而不是術語堆疊。

這比我預期的有效率。不是分散注意力做四件不相關的事，而是把一個深入的主題壓出四種形狀。

今天的實際產出：10 份任務草稿（技術 2 + 研究 4 + 運營 4），等 review 後提交。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->
# 2026-07-15

今天開始接觸 Moss 這個 AI Agent × Web3 的開源專案，一開始只是想了解目前 AI Agent 在 Web3 生態中的發展方向，因此花了一些時間閱讀專案的 README 和整體架構。

目前最大的感受是，AI Agent 不再只是回答問題，而是開始朝向「能夠真正執行任務」發展。當 Agent 需要查詢鏈上資料、操作錢包、呼叫智慧合約時，背後就需要一套穩定且容易擴充的基礎設施，而 Moss 正是在這個方向上提供了一種解決方案。

今天主要完成了專案的初步了解，也整理了幾個後續想深入研究的問題，例如 Moss 如何管理不同工具（Tools）的調用、如何與 Web3 協議整合，以及未來是否能應用到更多 AI Agent 的場景中。

雖然目前還沒有開始實際開發，但透過閱讀專案內容，對 AI Agent 與 Web3 的結合有了更清楚的認識，也發現開源專案除了程式碼之外，完善的文件與社群討論同樣重要。

接下來的目標是先把專案成功跑起來，閱讀核心模組的程式碼，再嘗試完成第一個簡單的 Demo，希望能逐步參與社群並貢獻自己的力量。Moss 開源學習打卡 Day 1

今天開始接觸 Moss 這個 AI Agent × Web3 的開源專案，一開始只是想了解目前 AI Agent 在 Web3 生態中的發展方向，因此花了一些時間閱讀專案的 README 和整體架構。

目前最大的感受是，AI Agent 不再只是回答問題，而是開始朝向「能夠真正執行任務」發展。當 Agent 需要查詢鏈上資料、操作錢包、呼叫智慧合約時，背後就需要一套穩定且容易擴充的基礎設施，而 Moss 正是在這個方向上提供了一種解決方案。

今天主要完成了專案的初步了解，也整理了幾個後續想深入研究的問題，例如 Moss 如何管理不同工具（Tools）的調用、如何與 Web3 協議整合，以及未來是否能應用到更多 AI Agent 的場景中。

雖然目前還沒有開始實際開發，但透過閱讀專案內容，對 AI Agent 與 Web3 的結合有了更清楚的認識，也發現開源專案除了程式碼之外，完善的文件與社群討論同樣重要。

接下來的目標是先把專案成功跑起來，閱讀核心模組的程式碼，再嘗試完成第一個簡單的 Demo，希望能逐步參與社群並貢獻自己的力量。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
# 2026-07-14

### **2025.07.14**

Week 2 Challenge 方向是 Moss（github.com/nishuzumi/moss），今天花時間把 repo 翻了一遍。

**Moss 是什麼**：把 DApp/protocol 的互動抽象成 agent 可呼叫的 capability，走 discover → load → action → simulate 四步。系統負責組裝交易，agent 不碰 calldata 也不碰簽名。Alpha 階段，目前只跑 Monad mainnet。

**Repo 結構觀察**：

-   monorepo，pnpm workspace，六個 package：core / simulator / erc / system / protocols / mcp-server
    
-   依賴層級乾淨：pure machinery (core) → verification engine (simulator) → standard interfaces (erc) → instance data (system) → protocol adapters → MCP server
    
-   protocol adapter 目前只有 kuru（Kuru DEX）和一個 \_template，空間很大
    
-   有完整的 [CONTRIBUTING.md](http://CONTRIBUTING.md)，adapter 上線要求明確：decorator 標記、risk label、quantified expects、live e2e simulation 零 warning
    

**技術細節印象深的幾點**：

1.  Plan 是核心資料結構：capability 產出 PlanDraft（未簽名交易序列 + 宣告的 token flow），finalizePlan 把它封裝成帶 keccak256 hash 的 Plan，declared flow vs simulation result 的比對在這層發生
    
2.  semantic type 系統：參數有 describe（給 agent 看）和 decode（runtime 轉換）兩面，token symbol 只查 curated table 不查鏈上 fallback，防 scam token 冒充
    
3.  安全模型是「effects reconciliation + intent alignment」：machine 做 reconciliation（宣告 vs 實際差異偵測），human 做 intent alignment（語義層確認），兩者分工
    

**跟自己方向的交叉**：Moss 的 Plan + simulation verification 跟 AIP 的 preCheck/postCheck 是同一個問題的不同解法——Moss 在 off-chain simulation 層做 declared vs actual 比對，AIP 在 on-chain hook 裡做原子 enforce。Moss 目前不做鏈上 enforce（no-sign, no-send policy），所以兩者互補不衝突。

明天開始看 [protocol-onboarding.md](http://protocol-onboarding.md) 和 \_template，準備實際貢獻一個 adapter。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
# 2026-07-13

Week 2 Dev 軌開跑，這週的 Challenge 是給開源專案 Moss 做貢獻——认识 codebase、

發第一個 PR、完成一次 GitHub 協作。

跟前一週不一樣的地方：Week 1 做的是自己的東西（自己的錢包、自己的合約、自己的 repo），

主導權全在我手上。開源協作是進别人的專案：要先讀懂不是我寫的代碼、照 contributor 的

規矩走、把改動說清楚讓 maintainer 願意 merge。

对我来说这块是短板。我的强项在链上实战和方向判断，但「读别人的大型 codebase +

正规 GitHub 协作流程」平常我大量交给 agent 代劳，自己亲手走完整流程的次数不多。

这週刚好逼我把这块自己做一遍——不是让 agent 帮我发 PR，是我自己看懂再动手，

agent 只当查询和解释工具。

本週目标：把 Moss 认识清楚（它解决什么、代码怎么组织），找一个我真的能改的小地方

（文档、example、小 bug），走完一次真实的 PR 流程。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

**2026-07-11 — Day 6｜Week 1 收官**

```
- Week 1 任務全部提交完畢：最後兩個（階段總結、Mini Demo 0）收掉，
```

`加上五天連續打卡，第一週完整結束。`

`- Mini Demo 0 成品：開了獨立 repo（github.com/USDHGwang/monad-camp-week1），`

`把這週「Monad 作為 agent 載體」的推導整理成公開文檔——含兩次自己的認知修正`

`（Bittensor 沒有用鏈聚合算力、LLM 上鏈差七八個數量級），配 HelloMonad 部署實證。`

`這是這期第一份可以獨立給人看的作品。`

`- 這週的 workflow 教訓：提交任務前先讀提交指引、按欄位逐項寫，不要憑任務標題`

`自由發揮。這條同樣適用於指揮 AI——派任務時要先把驗收標準餵進去，`

`不然產出格式對不上，返工成本比先讀規格高。`

`- Week 2 準備：主軌 Tech，Research / Ops 兼顧。Day 1 要交 Role Choice Card、`

-   `Week 2 Role Log、AI Collaboration Log。`
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
# 2026-07-10

2026-07-10 — Day 5｜Monad 理解 + on-chain agent 探索

\- 完成 Task 5 的思考：Monad 對我的意義不是「更快的 EVM」，是 agent 載體。

agent 的行為模式是持續的 sense→decide→act loop，交易密度天生比人高；

亞秒級出塊把 agent 之間的反應迴路從十幾秒壓到 ~1s，multi-agent 協調從不可行變可行。

\- 延伸探索：Bittensor / DePIN / fully on-chain agent。修正了自己一個誤解——

Bittensor 的鏈沒有聚合算力，訓練在鏈下，鏈聚合的是評分、stake、支付（proof of intelligence）。

DePIN 同一個骨架：鏈下幹活，鏈上驗證分錢。

\- on-chain agent 的階梯：條件觸發 → 策略型（trading bot）→ 多 agent 互動。

AMM 其實是史上最成功的 fully on-chain agent（確定性做市、零鏈下依賴），只是沒人這樣叫它。

主動 agent 有 tick 缺口：EVM 合約不會自己醒來，解法只有 keeper / permissionless bounty / lazy evaluation。

\- 量化了「LLM 上鏈」的 gap：整條 Monad 的等效算力約等於一張 H100 的千萬分之一，

7B 權重寫上鏈要吃掉全鏈 8 小時容量。「on-chain 發包 → off-chain 處理」是物理分工不是妥協。

設計空間被一個問題切分：鏈怎麼知道 off-chain 沒作弊（冗餘評分 / 樂觀挑戰 / zk證明 / TEE）。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

**2026-07-09 — Day 4｜Agent 安全框架 vs 自己的 AIP/EIV**

-   **Agent Guard 四能力**：Discover（資產發現）/ Red-Team（模擬攻擊）/ Defend（運行時攔截 allow-warn-approve-block）/ Govern（治理儀表盤）。它是**廣而淺**的通用平台，橫跨 prompt injection、RAG 洩露、MCP 提權這些層——正好是我 AIP/EIV **刻意 scope out 的感知/推理層**。
    
-   **關鍵差異在「攔截時機」**：Agent Guard 的 Defend 是 off-chain 本地護欄，屬於 advisory/攔截層——如果 agent 本身被攻陷，這層可能被繞過。AIP 的差異化在**鏈上原子 enforce**：就算 off-chain agent 完全被欺騙，on-chain hook 仍把執行綁死在授權參數內。這正是我研究方向的核心（被攻陷也越不了授權邊界）。
    
-   **概念收斂**：Agent Guard 兩大核心機制（最小權限 + HITL）跟我 AIP V2→V6 對抗式強化的結論一致——EIP-712 簽章 = HITL 批准綁定，執行目標 allowlist = 最小權限。不同團隊各自收斂到同一組 invariant。
    
-   **一個定位提醒**：Agent Guard 直接打「AI Agent 安全」大旗。我的論文正是因為 overclaim「AI security」被拒兩次。EIV 是 **authorization compliance**（授權符合性），不是 AI security——這個界線得守住，別被這類框架的框架帶回去。
    
-   **收穫**：這份框架是個外部驗證——問題空間商業上是真的（有人做成平台）；而它最淺的地方（Web3 合約風險只靠 GoPlus 黑名單）正是 AIP/EIV 做深的地方。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

# **Daily Note — 2026-07-08**

## **Today's Events**

-   Co-learning 打卡 — 7/8
    
-   一整天把自己的 agent workflow / harness 系統化，讀 Claude Code 官方 loop engineering 文章
    

## **Key Learnings**

1.  **Loop engineering 這條演化線**：prompt → context → harness → loop。四種 loop 按 trigger / stop condition / 用哪個 primitive / 適合什麼任務分類：turn-based（你判斷完成，短任務）、goal-based（`/goal`，有可驗收出口，evaluator 擋早退）、time-based（`/loop`+`/schedule`，週期性 / 對接外部系統）、proactive（連 prompt 都交出去，well-defined 的重複工作流）。核心紀律：不是每件事都要複雜 loop，先用最簡單的解、selectively 用這些 pattern。
    
2.  **這條線是技術的疊加不是取代**：context 疊在 prompt 上，harness 包含前兩者，loop 是「你不再逐條 prompt，而是設計那個替你 prompt agent 的系統」。loop engineering 之所以 2026 才成立，是因為模型可靠到 loop 幾輪就收斂、context 大到整個 codebase 塞得下、per-token 成本降到划算——下層成熟才長得出上層。
    
3.  **個人感悟**：applied builder 的槓桿在 composition stack（prompt/context/harness/loop）這層，不在 model 層。model 層要資本和 research，是幾家 lab 的事；上層是概念疊加 + 判斷力密集，這剛好是我相對強的位置。
    

## **Actions Taken**

-   全專案盤點 + 安全審計（EIV-core selftest 182/182 綠；找出 2 個 HIGH：signature malleability、max\_slippage\_bps 簽了但不 enforce）
    
-   把 CC 官方 loop 框架落進自己的 workflow-playbook（effort scaling / context 紀律 / deterministic vs advisory 分層 / loop 設計章）
    
-   建 3 個自動觸發 skill：verify-frontend、onchain-deploy-verify、first-source-verify（防已記錄的真實失誤：UI 未驗、私鑰外洩、弱來源誤判）
    
-   settings.json 加 SessionStart repo-status hook（掃 D:\\dev 未 commit / 未 push，防工作遺失）
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

**2026-07-07 — Day 2**

-   今天在整理 agent 發展框架的路徑時想到：ML/DL 演算法本身有價值但不是技術核心，agent 的價值來自「堆疊」出來的工程概念——prompt engineering → context engineering → harness engineering → loop engineering，是一層層疊上去的，不是取代關係（prompts → context/memory → skill）
    
-   但也有點不安：不確定這個框架理解得對不對，而且發現自己好像每次跟人聊天都在講 harness，有點擔心自己知識面太窄、被綁在單一主題上
    
-   Task 進度：Week1 六項任務裡，前置准备、鏈上實踐、AI輔助開發、智能合約實踐 四項的提交材料都準備好了（新建測試錢包 + 測試網交易 + HelloMonad.sol 生成部署），Monad理解跟階段總結還沒寫
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
# 2026-07-06

**2026-07-06 — Day 1｜工具準備與 Builder 身份**

-   工具/帳號：GitHub / X / Telegram / Discord 已具備，AI coding 用 Claude Code
    
-   DevRel 分享：講者 xiaohai.eth（@XiaoHai67890）是本計畫上一期學長，現任 SvpChain DevRel，分享內容是他自己從 Builder 走到生態連接者的路徑；Co-Learning 也有參加
    
-   鏈上實踐 + Monad 合約部署：沿用 AIP Protocol 既有部署（非今天新做）— `AIPRegistry` @ `0x6835B0A7bf1Bb28C40030CbeE662C965686eFd0F`，creation tx `0x22e23ecb...ddf7c35`（2026-03-24，MonadVision 驗證過）
    
-   AI 輔助開發：AIP `IntentBoundHook.sol` V1→V6 對抗式強化，每版經獨立 subagent 審核揪錯後補強，49 tests 全綠
    
-   Monad 理解：部署需加 `--legacy` flag，跟一般 EVM 鏈執行環境有差異
    
-   Week 2 方向：Tech（保留其他賽道彈性）
<!-- DAILY_CHECKIN_2026-07-06_END -->

<!-- DAILY_CHECKIN_2026-07-28_START -->
# 2026-07-28

**Day 23｜定題收斂成檔案**

hackathon 的題目今天收斂完。三份檔案：問題陳述、產品定義、分工包，各自檔頭寫明階段、狀態、下一步，接手的人從檔案讀起，不靠對話記憶。流程參考 superpowers 的組織方式和 Anthropic 的 context engineering 守則，蒸餾成四個 skill 留下來，下個專案直接用。

定了三件事。產品做兩檔行為：面板發起的交易，系統知道使用者要什麼，模擬報告旁邊直接給比對結論；外部貼進來的交易沒有意圖可比，就只攤證據。宣稱收成一句：證據來自執行，不來自構造。示範協議定 Kuru 加 shMONAD。

代價也算清楚了。review 打回的事件歸屬問題，本來是 upstream 的待辦，現在是 demo 關鍵路徑：同簽章事件加金額湊對就能組出假證據的話，這個產品的宣稱直接不成立，所以第一個 task 就是修它。
<!-- DAILY_CHECKIN_2026-07-28_END -->

<!-- DAILY_CHECKIN_2026-07-29_START -->
# 2026-07-29

\## Day 24｜先想清楚要打什麼，再把契約定下來

昨天定完題目，今天先改的是定位。原本要打安全，簽名前擋風險。查了一輪同類產品的下場之後改掉：Wallet Guard 被 Consensys 收購，插件今年三月關站；Harpie 拿過 Coinbase Ventures 的錢，一樣收攤，官方講的理由是做不出可持續的商業模式；Blowfish 被 Phantom 買走。活著的是 Blockaid 這種賣 API 給錢包的。使用者實際被觸達的方式是錢包預設整合，不是自己去裝一個安全插件。

安全工具的難處在於沒被咬過的人不覺得自己需要。所以主打換成 agent 執行端：讓 agent 幫你在鏈上做事，簽名之前你看得到它到底要做什麼。這個需求天天有，不用等出事。

換完才發現比對這件事站得更穩。純面板點選的時候，建交易和描述意圖走的是同一條路，比對出來接近同義反覆。agent 進來就不同了，使用者說的是自然語言，agent 去挑協議、挑方法、填參數，中間是真的翻譯。翻譯會錯，而且錯得聽起來合理。

讀 Moss README 的時候看到一句正好對上這個位置：library 給出完整的 Receipt 樹和結構化 Outcome，MCP 只回給 agent 已驗證的葉節點文字和 warnings。完整證據一直存在，agent 拿不到全部，使用者看到的又是 agent 轉述過的版本。中間那層沒有人拿給人看。

下半天做契約。定下前端要吃的型別，寫了五個情況的假資料（一致、部分不符、不一致、模擬出 warning、協議無 adapter），配兩層測試：vitest 驗不變式，tsc 驗編譯期契約。

兩個檢查各抓到一次我自己的錯。typecheck 抓到我把地址寫成 string，Moss 的 Address 是 \`0x${string}\` 模板字面型別。viem 擋下我編的 fixture 地址，EIP-55 checksum 不合，改成讓函式現算，不手抄。

最有用的一條測試是把 receipt 葉節點換成內容完全相同的複製品，verifyReceiptCoverage 照樣抓得到。這條說明前端不能在渲染時重建 change 物件，也是 Day 21 被打回那項的同一個道理：框架管有沒有漏認，認得對不對是自己的責任。
<!-- DAILY_CHECKIN_2026-07-29_END -->

<!-- DAILY_CHECKIN_2026-07-30_START -->
# 2026-07-30

## Day 25｜三件事都在定邊界

**Paybox**

MoonPay 昨天 GA 了 Paybox，非托管的 credential vault。agent 透過 MCP 接進去，拿不到 raw credential，只拿一次性的衍生憑證，而且授權綁單一動作不可複用。三段式授權：每筆都要 passkey 核准、額度內自動超額才核准、政策內完全不確認。核准畫面列出哪個 agent 在要求、存取什麼、目的地、金額、請求何時失效。

那張欄位表和三段式授權值得抄。但它沒有交易模擬，官方文件裡找不到 simulation 這個詞，audit trail 是事後留痕。它驗的是「這個 credential 該不該交給這個 agent、在什麼範圍內」，我驗的是「這筆交易實際會造成什麼」。範圍對得上，效果仍然可以不對，Bybit 就是這個形狀。

它的 autonomous 模式裡人不看畫面，表面上我的東西在那個模式失效。但我的比對結果是機器可讀的四個值，所以它可以當放行條件用：一致就自動過，不一致才升級給人看。這樣「無人監督」跟「有人把關」不用二選一。這個講法不用寫新 code 就能講。

**介面**

做了模擬畫面。第一版做成獨立面板，做完發現形式不對。我要的是停靠在使用者當前頁面旁邊的側邊欄，不是另開一頁。傳了一張 Monica 的截圖過去才講清楚。

從那張圖抄到最有用的一件：每一條結論後面掛一個 source 連結。這解掉我原本的問題，技術細節收起來之後使用者怎麼知道這些話是查證來的。不用加全域宣告，每條自己帶依據。

也犯了一次錯。我請 Claude 寫給設計那兩位的文件，寫出來變成施工圖，版面順序、欄寬幾 px、按鈕怎麼排都寫死了。那是我在替他們做設計。設計不歸我，我該講的是要達到什麼、什麼不能違反。重寫成 brief，多了一節「我解不掉的問題」直接問他們。

**T1**

把 review 打回的 attribution 修了。三處：事件來源沒驗、ERC20 委派沒驗來源、native transfer 只比金額不比 from/to。

地址比對照 wmon.ts 的既有慣例用 toLowerCase 兩邊。方向性檢查也是照 WMON 抄的，它有我沒有。但查的時候發現 WMON 自己也沒檢查 change.address，錯誤訊息印了那個欄位卻沒拿來比對。同一個病灶在 maintainer 自己的 system 套件裡。

新增五個攻擊型測試：冒充合約發 Deposit、外來代幣的 Transfer、錢沒進 shMONAD、出資人不是 Deposit sender、unstake 的錢不是從 shMONAD 出來。lint / build / typecheck / test 全綠，含 live mainnet 三個。live 那個是對真實主網 trace 跑模擬，它過表示新加的斷言沒有誤擋合法流程，這是我加檢查時最擔心的事。

一件沒解的：unstake 的方向性斷言沒有 live 測試佐證。repo 裡沒有 live unstake 測試，因為測試帳號沒有 shMON，模擬會 revert。如果 shMONAD 實際走 atomic unstaking pool、讓錢從別的合約出來，我這個斷言會誤擋合法交易。
<!-- DAILY_CHECKIN_2026-07-30_END -->

<!-- DAILY_CHECKIN_2026-07-31_START -->
# 2026-07-31

## Day 26｜查完競品把插件砍了，然後當天把新形態的管線打通

昨天定的形態是網頁加瀏覽器插件側邊欄。今天查完同類產品的下場，插件砍掉。

三個樣本：Wallet Guard 被 Consensys 收購後今年三月關站；Harpie 拿過 Coinbase Ventures 的錢，官方講的收攤理由是做不出可持續的商業模式；Blowfish 賣給 Phantom 之後對外 API 停止。兩個被吸收一個倒閉，沒有反例。錢包端該做的事，MetaMask、Rabby、Phantom、Coinbase、Trust、OKX、Backpack 七家都做了。在那裡不會贏。

那新的位置在哪。錢包的地盤是瀏覽器裡按下簽名的那一刻，那格滿了。沒有人佔的是「我叫 agent 做事」到「一筆交易出現」中間那段，那段不在瀏覽器裡，在對話裡。分發也不同，不是 Chrome Web Store 是 MCP，使用者本來就在裝 MCP server。

MCP Apps 在 07-28 併進主規格，兩天前的事。server 可以回傳 ui:// resource 讓 host 在 sandboxed iframe 裡渲染，而且互動是雙向的。

查了一輪現有玩家的授權判斷停在哪。Fireblocks、Turnkey、Privy、Dfns、BitGo、PayBox、Crossmint 都是靜態規則，地址金額方法選擇器。MetaMask 加 Blockaid 那一類是拿模擬結果比對惡意模式庫。真正做「執行結果對不對得上使用者要的」的只有兩個，Cobo Argus 的 postExecCheck 和 MetaMask 四月上線的 Advanced Permissions，兩個都是執行後 revert，不是簽名前決定要不要送。所以那一格是空的，而佔住相鄰兩格的是 MetaMask。

PayBox 昨天查得不夠細，今天補完。它的核准走推播通知加 passkey，發生在對話外面，核准畫面的欄位清單裡沒有任何交易模擬。所以我們跟它不是同一個位置做不同的事，是連渲染層都不同。

下午把 MCP server 寫出來了。沒有改 Moss 的 mcp-server，自己建一個把 Moss 當 library 用，理由是 Moss 那份在我的 PR 分支上，加東西會污染 PR，而且它的依賴裡本來就沒有 shmonad。

過程中排掉三個坑。pnpm 會把訊息寫到 stdout，我把 stderr 丟掉還是看得到錯誤，JSON-RPC 混進一個雜訊字元整條就壞了。vite-node 要從專案根解析設定，換個 cwd 完全沒輸出。最後用 tsup 打包成 dist/cli.js 再用 node 跑，從別的目錄啟動也通。

驗到的：tools/list 的線路輸出裡帶著 \_meta.ui.resourceUri，resource 回得出自包含的 HTML，27 個測試全綠。

還沒驗到的是面板到底會不會渲染出來，那只能在 Claude Desktop 裡看。有一個疑點先記著：server 協商出的 protocol version 是 2025-11-25，MCP Apps 規格寫的是 2026-01-26，會不會因為版本不同而不啟用渲染，不知道。

順手自己驗了一件一直沒親手確認的事。公開的 rpc.monad.xyz 實測 debug\_traceCall 可用、chain ID 143、CORS 對任意 origin 開放。免後端這個架構的地基是穩的。回傳的 trace 裡還看到 shMONAD proxy DELEGATECALL 到 implementation，地址跟 adapter 裡寫的常數一致，等於順便驗了一次。
<!-- DAILY_CHECKIN_2026-07-31_END -->

<!-- DAILY_CHECKIN_2026-08-01_START -->
# 2026-08-01

## Day 27｜同一種錯犯了兩次：拿假設當現實

今天把真的管線接上了。agent 呼叫工具、對 Monad 主網跑 debug\_traceCall、Moss 解讀成 Receipt、組成面板要的資料。129 個測試，其中四個每次都真的打主網。傳輸方式也重新決定過，原本要用本機 stdio 因為好接，查完官方文件與四個 GitHub 實例改成 remote HTTPS，Anthropic 自己把 remote 定位為 recommended、stdio 與 mcpb 為 secondary，而且只有 remote 能讓評審不裝東西就試。

但今天真正學到的是兩次同樣的錯。

第一次在凌晨。管線通了之後我自己開面板看，上面寫的是「Native MON Transfer: 250000000000000000 from 0xcccccccc… to 0x1b68626d…」。金額是 wei，地址是完整四十二個字元。這個產品的前提是非技術使用者三十秒讀懂，這樣直接失敗。

問題不在渲染寫錯。我一直拿自己手寫的 fixture 在跑測試，全綠。而那些 fixture 的資料形狀是我自己編的，跟 Moss 真實輸出根本不同。測試保護的是一個不存在的世界。修法是加一層轉換拿 Moss 的結構化資料重寫成人話，fixture 的形狀改成跟主網實測一模一樣並在檔頭記錄實測日期，再加一組回歸測試掃全部情境，出現完整地址或未格式化長整數就失敗。過程中還抓到一個語意錯，我原本把 Deposit 標成「取得」，但那是協議的記帳事件不是價值移動，前面兩行已經算過，標成取得等於重複計算。

第二次在晚上。我一直把 MCP App 的 HTML 面板當唯一形態，終端機文字版當退路。查了官方的 client matrix 才發現順序反了：支援 MCP Apps 渲染的只有 Claude web 和 Claude Desktop，Claude Code、Codex 這些 CLI 一律不在內。而我自己列的 agent 使用場景第一個就是 Claude Code。

所以文字版不是備案，是那批 host 上的主要形態。今天把它寫出來了，跟 HTML 版共用同一份轉換邏輯，內容一致只是排版媒介不同。順帶把後天的 go/no-go 風險降下來，HTML 渲染就算不通，產品在 CLI agent 上是完整的。

中間還踩了一個小坑值得記。我用 node -e 寫 Claude Desktop 的設定檔，路徑的反斜線經過 bash 雙引號和 JS 字串兩層處理被吃光，變成 D:devmonad-camp…，server 當然啟不動。改用正斜線加獨立腳本檔寫入就好了。

今天的共同教訓是：測試綠不等於東西能用，查到的規格不等於實際支援的清單。兩次都是我拿腦中的假設當現實，而抓出來的方法都一樣，就是拿真東西跑一次然後自己當使用者看一遍。
<!-- DAILY_CHECKIN_2026-08-01_END -->

<!-- DAILY_CHECKIN_2026-08-02_START -->
# 2026-08-02

# 2026-08-02

## **Day 28｜讀懂一個合約，不等於讀懂整組行為**

今天把最後一個核心環節做完：簽名交接。面板碰不到 window\.ethereum，那是 MCP App sandbox 規格寫死的隔離，所以核准過的交易要交給一個獨立的靜態頁，由使用者自己的錢包簽。交易放在網址的 # 後面，fragment 不會進 HTTP 請求，所以就算之後託管在雲端，交易內容也不會送到那台伺服器。兩邊各顯示一次同樣的指紋，讓「被簽的是被模擬的那一筆」從一句宣稱變成可以對照的東西。

交接不繞回 agent，這是刻意的。agent 正是我們假設不可信的那一方，讓它在人核准之後再碰一次交易，前面驗的全部作廢。

順著這條線把私鑰的邊界想清楚了。原本想過用 .env 放金鑰讓流程順一點，問題不只是明文和 push 風險那些操作層面的事。真正的問題是：只要我們的 server 持有金鑰，我們暴露的任何一個 tool 都變成 agent 的簽名能力。agent 根本不需要看到私鑰，它呼叫那個工具就好。現在的設計擋得住不是因為防得好，是因為我們沒有東西可以被拿去用。

然後今天有兩次差點照著自己的推斷去做事。

第一次是 ERC-7715。原本的計畫是讓使用者一次性授權一把有上限的 session key，之後每一筆就不用彈錢包了。查下來 Monad 主網確實支援 EIP-7702，我打 RPC 實測，帶一個 authorization 的 gas 比對照組多兩萬五千多，符合規範的定價，節點不是忽略那個欄位。MetaMask 的委派合約在 Monad 主網也都有 code。看起來全通。我讀了 NativeTokenTransferAmountEnforcer 的原始碼，全部驗證只有一行 require(spent <= allowance)，不管 calldata 也不管目標地址，所以下了「質押這種帶 calldata 的合約呼叫應該可以」的判斷。

錯在只讀了一個 enforcer 就推整組行為。MetaMask 實際上疊了第二個，官方文件寫得很白：套用這個 scope 時 toolkit 預設把 exactCalldata 設成 0x，而 ExactCalldataEnforcer 的邏輯是要求 calldata 雜湊完全相符。0x 就是必須為空。ERC-7715 的請求結構裡也沒有 calldata 欄位，dapp 蓋不掉這個預設。結論是透過 MetaMask 的 7715 只能做純轉帳，呼叫不了合約，這條路對我們的場景是死的。

這次是先花四十分鐘查文件而不是先花十五小時去接，所以只損失四十分鐘。

第二次是簽名頁。原本要做的只是把三個確認動作減成兩個，回到一般 dapp 的步數。改完之後拿真的網址在瀏覽器裡跑，順手試了竄改的版本，發現畫面根本沒更新。原因是只改網址的 # 不會重新載入文件。這代表面板第二次交接如果重用同一個分頁，頁面會顯示並簽出上一筆交易。之前每次都是開新頁測，所以從來沒碰到。加上 hashchange 重讀和一個 generation 計數擋住飛在半空中的舊請求就好了。同時也發現簽名頁的金額還在顯示 wei，面板早就轉成人話了，這一頁漏掉。

最後驗過的東西：改掉收款地址但沿用原指紋的網址，簽名頁會擋下來說「這串網址被改過」，交易不顯示按鈕也不給；不重載只換 # 換成竄改版，一樣擋得住。168 個測試綠。

查證過程裡有一件事對產品判斷有用。MetaMask 的 Advanced Permissions 已經在 Monad 主網上線了。使用者今天就能授權 agent 持續轉走 native token，之後每一筆都沒有錢包彈窗。彈窗消失，人就再也看不到發生了什麼。簽名疲勞是真問題，業界正在用拿掉彈窗來解，而那樣做會拿掉人最後看得見的地方。
<!-- DAILY_CHECKIN_2026-08-02_END -->

<!-- DAILY_CHECKIN_2026-08-03_START -->
# 2026-08-03

# Day 29 · 2026-08-03

## 做了什麼

**簽名交接完成。** 面板 → 簽名頁 → 使用者自己的錢包。三個設計決定：
交易由面板直接交出去不繞回 agent（agent 是假設不可信的那一方，讓它在人核准
之後再碰一次交易，前面的驗證就白做）；交易放在網址的 `#` 後面，fragment 不
會進 HTTP 請求，所以部署到雲端後託管方也收不到交易內容；交接指紋讓「被簽的
是被模擬的那一筆」可查核，解碼時自己重算，網址被改過直接擋。

瀏覽器實測過兩條路：正常的渲染出來、竄改的（改收款地址沿用原指紋）被擋下。

**點擊從三次減成兩次**，跟一般 dapp 一樣。已連過錢包就自動觸發，沒連過才給按鈕。

## 踩到的坑

拿真實地址跑質押 10 MON，面板回報「沒有發現意料外的動作」而且可簽名。
但那個地址只有 9.08 MON，這筆送出去必定 revert。

根因在 Moss 的模擬器：每次跑 trace 之前把發送方餘額蓋成 100 萬 MON
（`DEFAULT_PREFUND_WEI = 10^24`）。對一個通用框架是合理預設——它要能回答
「這個呼叫會做什麼」而不管帳戶有沒有錢。對我們是錯的，因為我們宣稱的是
「簽名前看到實際會發生什麼」。

影響比一筆大：demo 帳戶主網上只有 0.001 MON，連手續費都付不出來。所以先前
所有「對主網實跑」的綠色測試、demo、預覽頁的即時情境，驗的都是一筆送不出去
的交易。

跟上週那個 fixture 坑同一個病：驗收方式錯了，不是程式碼寫錯。而且這次是靠
自己拿真地址當使用者用才發現的，不是靠任何系統性檢查。

對策：模擬跑完另外拿真實餘額對「本金 + gas × gasPrice」，不夠就擋，但效果
照顯示——要先知道這筆想做什麼，才判斷得了值不值得去補錢。

## 另一個洞

結構層比對用 `Approval(address,address,uint256)` 的 topic 抓授權，但
`ApprovalForAll(address,address,bool)` 是不同 topic（`0x17307eab…` vs
`0x8c5be1e5…`），抓不到。所以一筆 `setApprovalForAll` 會被判成一致、顯示
安全、可簽名。

那是 NFT drainer 最標準的第一步，而且比無上限 ERC-20 授權更狠：沒有金額
欄位，交出去的是整個系列的轉移權，含使用者以後才拿到的。我們有規則專門抓
無上限 ERC-20 授權，卻對它的 NFT 版本失明。

補了兩條規則（未預期的整批授權算不一致、明白呼叫的也要人確認）。結構層看的
是原始事件不是 adapter，所以 ERC-721 與 ERC-1155 都會被抓到。

## 查證

**EIP-7702 / ERC-7715 走不走得通**，本來要投 15 小時做 session key。

* Monad 主網支援 EIP-7702：實測帶 authorization 的交易 gas 比對照組多 25,382，
  符合規範每個授權 25,000
* MetaMask 的委派合約在 Monad 主網有 code，Monad 也在官方 ERC-7715 支援清單裡
* **但 ERC-7715 只做得了純轉帳**。官方文件寫明套用 scope 時預設把
  `exactCalldata` 設成 `0x`，配上 `ExactCalldataEnforcer` 的
  `require(keccak256(termsCallData_) == keccak256(callData_))`，calldata 必須
  是空的。請求結構裡沒有 calldata 欄位，dapp 蓋不掉。→ 質押走不了這條

40 分鐘讀文件，省下 15 小時實作。

順帶查了 Moss 的來歷：作者是 Monad Foundation 的 DevRel 工程師，但專案掛在
個人帳號下，README 自標 `unaudited alpha software`。不能講成「Monad 官方框架」。

## 工程

* Moss 收進 `vendor/`，同時升到上游最新（`97df9c1`）。升級踩到一處：上游把
  `monadRuntime()` 移除、Runtime 從 `@themoss/system` 搬到 `@themoss/core`
* 簽名頁改由 server 在執行時提供。先前打包時把建置那台機器的絕對路徑烤進面板，
  只有那一台跑得起來
* fresh clone 驗證：88 個受版控的檔案複製到全新目錄，install → check → build
  全數通過
* 專案進版控（先前完全沒有），定名 **Vigil**（拉丁文：守夜人）
* MIT 授權，README 改寫
* CI 上線：每次 push 跑 typecheck + 離線測試，主網實跑分開排程每天一次

測試從 148 個變成 234 個。

## 明天

真人測試（3 人 × 30 秒，看得懂嗎）、提交材料、部署。
<!-- DAILY_CHECKIN_2026-08-03_END -->

<!-- DAILY_CHECKIN_2026-08-04_START -->
# 2026-08-04

# **Vigil — 0804 進度打卡**

**距離 demo：5 天**

## **今天推進的事**

**產品核心還差一步就閉合。** 面板線做了一輪完整的安全複查（234 測試通過），挖出 6 個 HIGH，其中最刺的一條是：交接指紋沒蓋到 `from`，等於攻擊者可以把「授權給 A」偷換成「授權給 B 的錢包」而不被查覺——這直接打在我們「證據來自執行、不來自構造」的立命根基上。好消息是修法明確、路徑清楚。

**另一條線把「包裝」補上了。** 全新 landing（暗黑 × 守夜→黎明的敘事動畫 × 中英雙語）已打包成純靜態站，可部署。

## **判斷**

今天最大的收穫不是「做了很多」，而是**code review 在 demo 前一天幫我們擋下一個會讓產品宣稱破功的洞**——這正是 Vigil 想解決的問題，發生在我們自己身上。

## **下一步**

* 修 HIGH（指紋涵蓋 `from`、MAC 化）
* Landing 補上真正「拆解級」的 Explode
* UI 排印 token + Monad 重色
<!-- DAILY_CHECKIN_2026-08-04_END -->

<!-- DAILY_CHECKIN_2026-08-05_START -->
# 2026-08-05

# **2026-08-05**

## **今天做的**

把 MCP 面板改成小窗口：預設只顯示做決定需要的東西（使用者的要求、比對結論、鏈上會發生什麼、簽名鍵），參數和免責說明收進可展開的區塊。用原生 `<details>` 而不是自己管狀態——內容留在 DOM 裡，收合不等於消失，測試照樣掃得到。

`pnpm check` 286 passed，含對 Monad 主網的實跑。

## **學到的**

### **1. design token 不能直接跨專案搬**

面板的配色沿用了自己 landing page 的 token。搬完量對比度，三處不合格：深色的 10px 標籤只有 3.30:1（WCAG AA 要 4.5）、淺色的指紋標籤 4.08、淺色警示標題 3.96。

不是顏色挑得差，是**兩邊最小字級不同**。landing 的次要文字比面板大，同一個色值換到更小的字上就過不了。

**token 可以共用，對比度必須在實際使用的字級下重算。**

順帶記一個常被搞錯的門檻：WCAG 的「大字」是 18.66px 粗體或 24px。**13.5px 加粗還算普通文字**，一樣要 4.5:1。

### **2. 同一組值有好幾份時，光靠改是守不住的**

主題 token 在 CSS 裡有四份（預設、系統偏好深色、明確指定深色、明確指定淺色），因為外層決定用哪一份。我改對比度時只改到其中一份，另一份留著舊值——**而畫面上看不出是哪一份在生效**。

寫了測試釘住「四份的名稱一致、兩份深色相同、兩份淺色相同」加上對比度下限。

真正的重點是最後一步：**把舊值塞回去，確認測試會紅**。沒驗過會不會紅的測試，只是讓人安心的裝飾。
<!-- DAILY_CHECKIN_2026-08-05_END -->

<!-- DAILY_CHECKIN_2026-08-06_START -->
# 2026-08-06

**Day 32｜門面亮了，才發現歷史也是門面**

今天 Landing 正式上線。晚上九點半到十點四十是收尾的高密度時段，6 個 commit 把架構段重建成六幕 pinned scrollytelling：GSAP ScrollTrigger、550vh runway、pin 內層舞台、scrub 0.5，幕間 crossfade 加每幕 scale 沉澱；act 06 的選擇岔路——VERIFIED 卡 → YOU DECIDE → SIGN「broadcasts」/ REJECT「nothing moves」，卡片沉鏈、三個區塊依序點亮、天亮；暗底改成帶藍調的 navy（#0a0e18）加 starfield 捲動視差；字體收斂到 Geist、tracking 0.14–0.18em、body 全部 normal。動畫語言整條守 Vercel camp：零 elastic / back / rotate，全 power2.out，opacity 加 8–16px 位移——信任產品只確認，不表演。i18n 一次補到五語系（en/zh-TW/zh-CN/ja/ko），地球 icon dropdown，切語不重建 ScrollTrigger。舊的 Explode 系列和 three.js 整條刪掉。下午把 GitHub Pages 開起來，`pages.yml` 自動跑，根路徑跟 sign 頁都回 200。門面，亮了。

但同一天發現後門是開的。repo 是 public，git 歷史裡躺著 11 個內部檔：根目錄四份 plan/handoff、product-brief、docs/ 整包交接與日誌。就算現在把檔案刪掉，歷史裡每一版都還挖得出來。處理方式是用 `git-filter-repo` 在 fresh clone 上剝歷史，18 commits 變 15，驗證零殘留，最後一步 force push 覆寫 GitHub 等拍板——還沒推。這堂課比 landing 本身貴：public repo 的歷史就是門面的一部分；交接檔跟日誌根本不該進同一個 repo；在 git 裡「之後再清」的代價，比一開始就分 repo 大得多。

下午另一條線把「知識如何到達 agent」寫成文件。naive route 是餵文檔：靜態快照、鏈會變、知識漂移，而且文檔是指令通道——注入的指令跟知識混在同一條通道，模型分不出來，這正是 prompt injection 的本質。Vigil 的路是 server 即知識：不餵文檔、餵 tools，discover 動態回傳 schema，自由文字只以資料進入、被當證據比對、不當指令遵循。寫完順手實測：全新 session、15 個 tool calls、1 分 6 秒，agent 自己 discover → 準備質押 → 模擬 → 主動講出「receiver 要換成 your address」「簽名錢包要跟帳戶一致」。不是預設 agent 懂 Monad，是 agent 讀 Vigil 就會做，然後 Vigil 驗證它做的對不對——這句話可以直接進 pitch。

晚上第一次用使用者的眼睛把產品完整跑一遍：Hermes 當 host 接 Vigil MCP，預覽 0.25 MON 質押成 shMONAD。verdict BLOCKED，而理由全是真的——模擬帳戶只有 0.001 MON，付不起本金加手續費（\~0.266）照送會直接失敗；receiver 還是測試地址，要換成自己的；簽名錢包要跟帳戶一致。三個前提清清楚楚。壞在誠實的地方，比壞在程式碼裡好一百倍。
<!-- DAILY_CHECKIN_2026-08-06_END -->

<!-- DAILY_CHECKIN_2026-08-07_START -->
# 2026-08-07

### 8/07 打卡摘要

#### 一、核心進展

1. **主網 E2E 全面打通**
   * 完成真實交易驗證（0.25 MON stake → shMONAD）與 Kuru Swap 本機實測。
   * 完成 10 個 Capability 測試矩陣（5 可簽名 / 5 預期 Revert），全測項 337 passed，0 typecheck 錯誤。
2. **i18n 多語言補齊**
   * 清零「英文對話混中文」缺口，完成 Panel 框架、Sign 頁面及 5 個資料工具輸出的 i18n 化。
3. **官網與 Repo 整備**
   * 官網（`/add`、`/docs`）部署至 Vercel。
   * 補齊 9 個對抗性測試，完成敏感資訊清理、歷史 Commit Squash 及 Repo 拆分準備。

#### 二、 Demo 關鍵認知（重要）

* **Kuru Swap**：金額需 $\ge$ 1 MON（走 AMM），避免小額訂單簿觸發解析異常。
* **語言切換**：介面語言由 Client 決定，Demo 展示需預先將環境設為英文。
* **安全機制**：錢包與發起帳戶不符會硬阻斷（`wrongAccount`）；多筆交易不支援（`MULTI_TX_UNSUPPORTED`）作為誠實邊界展示。

#### 三、 踩坑與教訓

* **測試規範**：改檔後必須執行 `typecheck + test + build:all` 三連，避免 Vite 快取漏抓型別錯誤。
* **餘額機制**：資產可負擔性（Affordability）依賴真實餘額，預充值（Prefund）僅影響模擬 Trace。
<!-- DAILY_CHECKIN_2026-08-07_END -->
<!-- Content_END -->
