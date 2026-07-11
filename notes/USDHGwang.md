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
