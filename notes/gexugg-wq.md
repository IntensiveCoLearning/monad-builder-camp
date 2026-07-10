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
