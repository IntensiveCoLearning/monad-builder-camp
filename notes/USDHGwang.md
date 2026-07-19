---
timezone: UTC+8
---

# John

**GitHub ID:** USDHGwang

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
shMONAD adapter 昨天寫完跑過 mainnet，今天回頭看整個過程，注意到一件事：在 Moss repo 裡貢獻程式碼的體驗，和在自己的 EIV/AIP repo 裡寫東西完全不同。

Moss 有 11 份 ADR 記錄架構決定，有 [CONTRIBUTING.md](http://CONTRIBUTING.md) 列 Definition of Done 清單，有 [protocol-onboarding.md](http://protocol-onboarding.md) 從零走一遍流程，有 \_template 可以直接複製。每一步該做什麼、做到什麼程度算完成，都有人替你想過了。我照著走，三小時出一個通過 live test 的 adapter。

回頭看自己的 EIV repo：沒有 onboarding 文件，沒有 contributor 指引，ADR 為零。如果今天有人想貢獻一個新的 chain adapter 或新的 verification rule，得先花時間讀完整個 codebase 才能開始。我自己能寫是因為上下文都在腦子裡，但這東西不 scale。

一個具體的對比：Moss 的 ADR 0007 規定 ABI 不能手寫，必須從 explorer 或 upstream package 拿，來源寫在檔案頭。我的 EIV 裡的 ABI 就是手抄的，沒有標來源。如果哪天合約升級，沒有人知道那份 ABI 是從哪裡來的。

看到一個成熟度更高的 repo 怎麼處理同類問題，就知道自己下一步可以補什麼。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->

今天開始寫 Moss protocol adapter。第一步就踩坑：原本打算寫 WMON adapter，結果翻 repo 才發現 `@themoss/system` 已經有了。計劃直接作廢，重新在 GitHub issues 裡找目標。

最後挑了 FastLane shMONAD staking（issue #12），maintainer 標了 `good first issue`，說形狀接近 WMON。實際寫的時候發現「形狀接近」有具體含義：單一合約、native token 進出、receipt token 回來。但 shMONAD 用的是 ERC4626 vault standard，deposit 函數多了一個 receiver 參數，unstake 有 atomic 和 queued 兩條路，receipt parser 要處理 Deposit/Withdraw/Transfer 三種 event 交錯出現。「形狀接近」讓你能套模板開始，但細節全是自己的。

寫 receipt parser 的時候有一個發現：Moss 要求每個 Capability 配一個 Receipt，收到 simulation 產生的所有 event，逐一 decode 並驗證數量和順序。這件事和我在 EIV 做的很像。EIV 是事後拿 transaction trace 驗 agent 有沒有越權，Moss receipt 是事前拿 simulation trace 確認交易行為符合預期。同一套 trace 資料，一個朝前看，一個朝後看。

從讀完 [protocol-onboarding.md](http://protocol-onboarding.md) 到 8 個 test 全過（包含 live Monad mainnet simulation），花了大概三小時。中間最花時間的是搞 ABI 來源：ADR 0007 不接受手寫 ABI，要從 explorer 的 verified contract 拿，還要記來源和日期。這種規矩在讀文件的時候覺得囉唆，自己寫的時候才理解它在防什麼。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->


今天把技術、研究、運營三個賽道的任務都交了。十份文件寫的都是 Moss 和 agent authorization，但寫給不同讀者的時候，自己對材料的理解會被迫重組。

研究賽道要我拆解 ERC-7579。這個標準我在 AIP 專案裡用了快一年，以為很熟。但要按 Reading Card 格式寫「背景問題 → 核心方案 → 爭議風險」的時候，發現自己一直在用 hook 的部分，對 validator 和 fallback handler 的設計取捨其實沒認真想過。寫的過程補了這塊。

運營賽道要設計一場 Space。技術內容我都知道，但要寫成「讓沒有技術背景的人也能聽懂」的活動文案時卡了一下。最後發現有效的方式是從場景進去（「你敢讓 AI 幫你做鏈上交易嗎？」），技術細節變成回答這個問題的工具，不是主角。

一個收穫：對一個主題的理解深度，可以用「你能不能對完全不同背景的人講清楚」來測試。講不清楚的地方通常是自己也沒真正想透的地方。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->



# **Day 11 — 一份深入，四個賽道的產出**

今天做了一件事：把 Week 2 所有賽道的任務全部打開看過一遍，然後發現一個有趣的現象——我在 Moss 和 agent authorization 上的積累，可以直接轉化成四個不同賽道的產出。

技術賽道要一份 AI-assisted Dev Plan，我寫的是 Moss protocol adapter 開發計劃。研究賽道要一份 EIP Reading Card，我拆解了 ERC-7579（這是我 AIP 專案的核心標準，讀過很多遍了）。運營賽道要一個 Space 策劃案，我設計了一場「AI Agent 操作鏈上協議的風險」討論。通用賽道的 Moss 介紹和教程，之前已經寫好了。

同一塊知識，技術面寫成開發計劃，研究面寫成結構化分析，運營面寫成活動設計。每個面向寫的時候會逼你從不同角度重新組織同一套資訊——給開發者看的要寫清楚 what to mock，給研究者看的要寫清楚 gap 在哪，給一般聽眾看的要用場景帶入而不是術語堆疊。

這比我預期的有效率。不是分散注意力做四件不相關的事，而是把一個深入的主題壓出四種形狀。

今天的實際產出：10 份任務草稿（技術 2 + 研究 4 + 運營 4），等 review 後提交。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->




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









**2026-07-09 — Day 4｜Agent 安全框架 vs 自己的 AIP/EIV**

-   **Agent Guard 四能力**：Discover（資產發現）/ Red-Team（模擬攻擊）/ Defend（運行時攔截 allow-warn-approve-block）/ Govern（治理儀表盤）。它是**廣而淺**的通用平台，橫跨 prompt injection、RAG 洩露、MCP 提權這些層——正好是我 AIP/EIV **刻意 scope out 的感知/推理層**。
    
-   **關鍵差異在「攔截時機」**：Agent Guard 的 Defend 是 off-chain 本地護欄，屬於 advisory/攔截層——如果 agent 本身被攻陷，這層可能被繞過。AIP 的差異化在**鏈上原子 enforce**：就算 off-chain agent 完全被欺騙，on-chain hook 仍把執行綁死在授權參數內。這正是我研究方向的核心（被攻陷也越不了授權邊界）。
    
-   **概念收斂**：Agent Guard 兩大核心機制（最小權限 + HITL）跟我 AIP V2→V6 對抗式強化的結論一致——EIP-712 簽章 = HITL 批准綁定，執行目標 allowlist = 最小權限。不同團隊各自收斂到同一組 invariant。
    
-   **一個定位提醒**：Agent Guard 直接打「AI Agent 安全」大旗。我的論文正是因為 overclaim「AI security」被拒兩次。EIV 是 **authorization compliance**（授權符合性），不是 AI security——這個界線得守住，別被這類框架的框架帶回去。
    
-   **收穫**：這份框架是個外部驗證——問題空間商業上是真的（有人做成平台）；而它最淺的地方（Web3 合約風險只靠 GoPlus 黑名單）正是 AIP/EIV 做深的地方。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->










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











**2026-07-07 — Day 2**

-   今天在整理 agent 發展框架的路徑時想到：ML/DL 演算法本身有價值但不是技術核心，agent 的價值來自「堆疊」出來的工程概念——prompt engineering → context engineering → harness engineering → loop engineering，是一層層疊上去的，不是取代關係（prompts → context/memory → skill）
    
-   但也有點不安：不確定這個框架理解得對不對，而且發現自己好像每次跟人聊天都在講 harness，有點擔心自己知識面太窄、被綁在單一主題上
    
-   Task 進度：Week1 六項任務裡，前置准备、鏈上實踐、AI輔助開發、智能合約實踐 四項的提交材料都準備好了（新建測試錢包 + 測試網交易 + HelloMonad.sol 生成部署），Monad理解跟階段總結還沒寫
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->












**2026-07-06 — Day 1｜工具準備與 Builder 身份**

-   工具/帳號：GitHub / X / Telegram / Discord 已具備，AI coding 用 Claude Code
    
-   DevRel 分享：講者 xiaohai.eth（@XiaoHai67890）是本計畫上一期學長，現任 SvpChain DevRel，分享內容是他自己從 Builder 走到生態連接者的路徑；Co-Learning 也有參加
    
-   鏈上實踐 + Monad 合約部署：沿用 AIP Protocol 既有部署（非今天新做）— `AIPRegistry` @ `0x6835B0A7bf1Bb28C40030CbeE662C965686eFd0F`，creation tx `0x22e23ecb...ddf7c35`（2026-03-24，MonadVision 驗證過）
    
-   AI 輔助開發：AIP `IntentBoundHook.sol` V1→V6 對抗式強化，每版經獨立 subagent 審核揪錯後補強，49 tests 全綠
    
-   Monad 理解：部署需加 `--legacy` flag，跟一般 EVM 鏈執行環境有差異
    
-   Week 2 方向：Tech（保留其他賽道彈性）
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
