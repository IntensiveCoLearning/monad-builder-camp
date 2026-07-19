---
timezone: UTC+8
---

# gexugg-wq

**GitHub ID:** gexugg-wq

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
本周我主要通过 Vibe Coding 的方式开发一个“假学习识别 Agent”。这个 Agent 的目标不是简单记录用户学习了多长时间，而是结合学习目标、过程记录、实际产出和用户反馈，判断用户是否存在形式化打卡、低效学习或缺乏真实进展的情况。我目前将自己的方向定位为 **AI Agent 与 Python 后端开发**，主要使用 Python 和 LangGraph 搭建工作流。

### 本周完成的工作

本周首先对产品逻辑进行了拆分。目前 Agent 的基本判断流程包括：

1.  获取用户当天的学习目标；
    
2.  记录用户实际学习内容和时间；
    
3.  要求用户提交学习成果或过程证明；
    
4.  对目标、时间、行为和结果之间的一致性进行分析；
    
5.  输出有效学习、低效学习或疑似形式化打卡的判断；
    
6.  根据判断结果给出下一步学习建议。
    

在开发过程中，我尝试使用 LangGraph 管理不同节点之间的状态流转，例如用户信息输入、学习证据分析、结果判断和建议生成。目前已经完成部分节点和 Prompt 的设计，但完整的 Agent 流程还没有全部打通。

### 目前开发过程中主要遇到了以下问题：

-   用户可以通过编造学习过程影响 Agent 的判断；
    
-   仅依靠学习时长无法判断学习是否真正有效；
    
-   大模型的判断结果有时不够稳定，相似输入可能得到不同结论；
    
-   判断标准过于宽泛时，Agent 容易输出空泛建议；
    
-   LangGraph 不同节点之间需要保存的状态还没有完全确定；
    
-   当前还缺少更可信的学习证据，例如代码提交、文档记录、答题结果或链上行为。
    

这些问题让我意识到，产品不能只做“对话式监督”，而应该尽可能引入可以验证的过程信息。  
当前项目还没有全部完成，但已经完成或正在推进的内容包括：

-   明确产品问题和目标用户；
    
-   梳理 Agent 的基础判断流程；
    
-   初步设计 LangGraph 工作流；
    
-   编写和修改判断 Prompt；
    
-   测试部分输入与输出；
    
-   发现仅依赖用户自述存在的问题；
    
-   调整产品方向和判断标准。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->

今日终于可以开始学习
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->


![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/gexugg-wq/images/2026-07-16-1784205101157-image.png)

今日小游戏打卡类似狼人杀，也挺有意思
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->



可以从以下三个部分进行工作流变更

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/gexugg-wq/images/2026-07-15-1784114194000-image.png)
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->




Research：判断方向对不对 Ops：让用户愿不愿意参与 Dev：把产品真正做出来
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->





pdd的编程理念能够帮助理解测试和边界的理解
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->






确认好目标是做一个假学习监督助手，将基于openhands基础上实现
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->







Mini Demo 0：Monad FocusProof Agent

我的 Week 1 Mini Demo 方向是 Monad FocusProof Agent，一个面向 Web3 学习者的有效学习监督助手。它不是普通的打卡工具，也不是单纯记录学习时间，而是尝试判断用户这段时间是否真的在学习，是否产生了可以复盘的学习证据。这个 Demo 的核心问题是：很多学习打卡只证明用户点了一下按钮，不能证明用户真的理解了内容。因此我希望用 AI Agent 来判断“有效学习”和“假学习”的区别。用户开始学习前需要设定目标，例如学习 Solidity、理解 transaction hash、完成一次合约交互或整理 Build Log。学习结束后，Agent 会要求用户提交学习证据，例如笔记、截图、交易 hash、合约代码、错误记录或区块浏览器链接。Agent 不只是问“你今天学了什么”，而是会根据目标、学习时间、产出证据和理解回答进行判断。如果用户说自己学了合约部署，但没有合约地址、部署 hash、截图或错误记录，Agent 会判断这次学习可信度较低。如果用户能提供交易 hash、Remix 截图，并能解释 read function 和 write function 的区别，Agent 会判断这次学习更有效。这个 Demo 可以设置一个有效学习评分，例如时间连续性、目标匹配度、学习产出、理解验证和下一步计划。只有达到一定分数的学习记录，才计入 streak。这样它不是 Proof of Time，而更接近 Proof of Focus：不是证明我坐了多久，而是证明我真的产生了学习结果。链上部分可以只记录轻量结果，例如学习日期、主题、有效学习分钟数、评分、连续学习天数和总结 hash，而不是把完整学习笔记全部上链。这样既能保留 Monad Testnet 上的成长记录，也能避免暴露隐私。我选择 Week 2 继续走 Tech 方向，希望把这个想法进一步做成一个小型 Demo。第一步可以设计 FocusProof 合约，支持记录学习主题、学习时长、评分和 streak；第二步设计 Agent 的判断流程，让它识别用户是否只是摆烂式学习；第三步再考虑前端或交互原型，让用户可以开始学习、结束学习、上传证据，并生成 Build Log。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->








我认识到裸奔状态下的智能体存在提示词注入、越权工具调用、自主执行不可控等高危风险，了解了对Agent做权限管控、结构化输出约束、流程校验的安全构建思路。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->









![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/gexugg-wq/images/2026-07-08-1783509612025-image.png)

```
三种agent运行模式-Print 模式（外带）**：你在柜台说一句「要一份套餐」，店员做完递给你，你拿走就结束。对应 **单次 `--prompt`**：进程启动 → 问一句 → 打印结果 → 退出，适合脚本和 CI。
- **Interactive 模式（堂食）**：你坐在店里，可以一轮轮加点菜、问店员菜单，还有「换桌、看分店树、开新单」等**斜杠命令**。对应 **REPL**：`you> ` 循环输入，内置 `/help`、`/session` 等，扩展还能注册自己的命令。
- **RPC 模式（外卖平台 API）**：不写人类终端 UI，而是 **stdin 一行一条 JSON**，像 App 向后厨下单：「来一条 prompt」「查状态」「fork 分支」「关机」。IDE 或插件用管道对接进程，用结构化 **request/response + event** 驱动。
```
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->












![faf35fdce29f9006cf9b3549f2863e3b.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/gexugg-wq/images/2026-07-07-1783406763416-faf35fdce29f9006cf9b3549f2863e3b.jpg)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->













```
我对链上产品的理解：
链上产品和普通互联网产品最大的不同是数据和资产状态记录在区块链上，而不是只存在某个平台自己的服务器里。普通互联网产品通常用账号密码登录，数据由平台管理；链上产品通常用钱包地址和签名交互，交易和资产变化可以在区块浏览器公开查询。链上产品的规则很多时候由智能合约执行，所以更透明，但操作成本和安全要求也更高。
```
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
