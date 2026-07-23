---
timezone: UTC+8
---

# euphoria17721-tech

**GitHub ID:** euphoria17721-tech

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# Open Source Contribution Plan：Moss

项目链接：[https://github.com/nishuzumi/moss](https://github.com/nishuzumi/moss)

## 1\. Builder 身份

我选择的 Builder 身份是：**Ops Builder / Product Ops Builder**。

我目前更擅长的不是直接开发新的 Adapter 或修复杂 Bug，而是阅读项目、理解用户路径、整理文档、优化新手体验，并把复杂概念转化成更容易理解的教程、FAQ 或学习笔记。因此我希望从文档和开发者体验入手，为 Moss 做轻量但有价值的开源贡献。

## 2\. 贡献方向

我的贡献方向是：**完善 Moss 的新手入门材料和 Agent Workflow 说明**。

Moss 的 README 已经说明它通过 `discover → load → action → simulate` 把 Monad 协议交互变成 Agent-callable Capabilities，并且只构建和模拟未签名交易，不负责签名和发送。但对刚接触 AI Agent + Web3 的新手来说，这套流程还是比较抽象。

我希望贡献一份更偏新手友好的文档，比如：

-   Beginner-friendly Quick Start 中文笔记
    
-   Agent Workflow 图解说明
    
-   Moss 常见问题 FAQ
    
-   “为什么 Moss 不签名、不发送交易”的解释
    
-   `discover / load / action / simulate` 四步流程示例说明
    

## 3\. 为什么选择这个方向

我选择这个方向有三个原因。

第一，我自己这几周也在学习 Monad、AI Agent 和 Web3 开源项目，能比较清楚地感受到新手卡在哪里。比如很多人可能知道 AI Agent 可以调用工具，但不理解为什么链上操作需要模拟、Receipt、Warnings 和用户签名分离。

第二，Moss 这类项目的价值不只在代码，也在于让更多开发者理解它的安全边界。如果文档能讲清楚“Agent 可以构建和模拟交易，但不能替用户签名”，会更容易降低误用风险。

第三，我的 Week 2 主方向是 Ops / Product Ops，适合承担文档、教程、传播和开发者体验优化工作。这类贡献虽然不是核心代码，但能帮助项目吸引更多 Builder。

## 4\. 本周目标

本周我计划完成以下目标：

1.  阅读 Moss README、Getting Started、MCP tool contracts、Agent safety rules。
    
2.  整理 Moss 的项目结构和核心概念。
    
3.  输出一份中文学习笔记，解释 Moss 解决什么问题。
    
4.  写一份新手 FAQ 初稿。
    
5.  选择一个最小贡献点，准备提交 Issue 或 Pull Request。
    

## 5\. 预计产出

本周预计产出包括：

-   一份 `Moss Beginner Notes.zh-CN.md`
    
-   一份 `Moss FAQ Draft.md`
    
-   一张 Agent Workflow 流程说明：  
    `discover → load → action → simulate → user reviews → wallet signs`
    
-   一个 GitHub Issue：  
    建议补充 Beginner-friendly 中文 Quick Start 或 FAQ
    
-   如果时间允许，提交一个文档类 Pull Request
    

## 6\. 完成计划

### Day 1：阅读和拆解

阅读 README、docs 和 examples，记录项目结构、核心术语和不理解的地方。

### Day 2：整理 Agent Workflow

重点理解 `discover / load / action / simulate` 四步分别解决什么问题，并用新手能理解的语言改写。

### Day 3：整理 FAQ

围绕新手常见问题写 FAQ，例如：

-   Moss 是 SDK 还是 Agent 框架？
    
-   Moss 为什么不签名和发送交易？
    
-   Receipt 和 Warnings 有什么作用？
    
-   Moss 和钱包之间是什么关系？
    
-   开发者如何新增协议能力？
    

### Day 4：对照原文检查准确性

确认自己的文档没有误解 Moss 的安全边界，不把模拟说成真实发送交易，不把 Agent 能力描述得过度自动化。

### Day 5：准备开源贡献

整理成 Markdown 文档，并在 GitHub 上提交 Issue 或 PR。如果暂时不确定是否适合直接 PR，可以先开 Issue 说明想补充 Beginner FAQ，并附上文档草稿。

## 7\. 我能提供的 Proof of Work

我已经完成过 Moss 的 GitHub Exploration Log，阅读了 README、Docs、Issues、Pull Requests，并整理了项目结构、核心问题和我感兴趣的 Issue / PR。

我也完成过 Monad Rank Rush Campaign Kit，说明我可以把 Web3 项目从技术概念整理成更容易理解的产品文档、活动方案和用户路径。

已有作品链接：

-   Monad Rank Rush：[https://github.com/euphoria17721-tech/monad-rank-rush](https://github.com/euphoria17721-tech/monad-rank-rush)
    
-   Moss 项目：[https://github.com/nishuzumi/moss](https://github.com/nishuzumi/moss)
    

## 8\. 一句话总结

我希望作为 Ops Builder，从文档、教程和开发者体验切入，为 Moss 补充更适合新手理解的 Agent Workflow 和 FAQ，让更多 Builder 能安全、准确地理解 Moss 如何帮助 AI Agent 调用 Monad 协议能力。
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->

今天学习了一下钱包的设计，收获颇深

![7f79603506567bdf1cc83128c5a2fab7.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-22-1784724979177-7f79603506567bdf1cc83128c5a2fab7.png)
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->


# GitHub Exploration Log：Moss

项目链接：[https://github.com/nishuzumi/moss](https://github.com/nishuzumi/moss)

## 1\. 项目简介

Moss 是一个面向 Monad 生态和 AI Agent 的开源框架。它把链上协议交互包装成 Agent 可以调用的 Capabilities，核心流程是：

`discover → load → action → simulate`

它的重点不是直接帮用户签名或发送交易，而是构建并验证 **未签名交易**。AI Agent 可以先发现协议能力、读取参数要求、构建 action，再通过模拟交易得到 Receipt 和 Warnings，让用户或钱包在签名前知道这笔交易可能产生什么结果。

我理解 Moss 解决的问题是：AI Agent 不能只凭自然语言就盲目发起链上操作，它需要一个标准化、安全、可验证的框架来理解协议、构建交易、模拟结果，并和用户意图对齐。

## 2\. 项目目录结构

从 GitHub 仓库首页可以看到 Moss 的主要结构包括：

-   `.github`：项目的 GitHub 配置，通常用于 Issue / PR 模板、CI 工作流等。
    
-   `docs`：项目文档，包括 Getting Started、MCP tool contracts、Protocol onboarding、Agent safety rules、Architecture decisions 等。
    
-   `examples`：示例代码，用来演示如何运行 Moss 的流程。
    
-   `packages`：核心代码所在位置，按功能拆成多个 package。
    
-   `README.md` / `README.zh-CN.md`：英文和中文项目介绍。
    
-   `CONTRIBUTING.md`：贡献指南，告诉外部开发者如何参与项目。
    
-   `SECURITY.md`：安全说明和漏洞报告方式。
    
-   `CODE_OF_CONDUCT.md`：社区行为规范。
    
-   `package.json` / `pnpm-workspace.yaml`：说明这是一个 TypeScript / pnpm monorepo 项目。
    

README 中也解释了 package 分工：

-   `@themoss/core`：Registry、参数规则、Capability tree、Receipt 验证。
    
-   `@themoss/simulator`：交易模拟、状态链、Change 提取。
    
-   `@themoss/erc`：ERC-20 / ERC-721 等通用协议能力。
    
-   `@themoss/system`：Monad Runtime、系统协议和官方常量。
    
-   `@themoss/protocol-*`：具体协议适配器，比如 Kuru、PancakeSwap 等。
    
-   `@themoss/mcp-server`：MCP server，让 Agent 可以通过 MCP 调用 Moss。
    

## 3\. README 观察

README 的结构很清楚，先说明 Moss 是什么，再解释 Why Moss、Supported Protocols、Quickstart、MCP server、Library usage、Verification、Repository layout、Documentation、Contributing 和 Security。

我觉得 Maintainer 在 README 里做得比较好的一点是：它没有只写“这是一个 SDK”，而是直接说明 Moss 的安全边界：**只构建和模拟未签名交易，不签名也不发送交易**。这对 AI Agent 项目很重要，因为 Agent 可以帮用户分析和准备交易，但最终签名权仍然应该留给用户或钱包。

## 4\. Docs 观察

Docs 的作用是把 README 里的概念拆成更具体的操作指南：

-   Getting Started：帮助新用户跑通项目。
    
-   MCP tool contracts：说明 `discover / load / action / simulate` 四个 MCP 工具的输入输出。
    
-   Protocol onboarding：指导开发者如何新增协议包。
    
-   Agent safety rules：说明 Agent 使用 Moss 时必须遵守的安全规则。
    
-   Architecture decisions：记录架构选择和 trade-off。
    
-   Domain language：统一项目里的核心术语。
    

这说明 Moss 不是只给核心开发者看的项目，也在尝试让外部贡献者能理解如何接入协议、如何遵守安全约束。

## 5\. Issues 观察

Issues 主要用于记录功能需求、协议适配、文档改进和安全相关任务。比如我看到的 open issues 包括：

-   `#114 [feature] Clarify RiskLabel semantics for debt-increasing lending capabilities`
    
-   `#94 [protocol] Kintsu sMON liquid staking`
    
-   `#90 Core/MCP: bound Capability tree complexity and reject cycles`
    
-   `#67 Docs: align the public architecture and verify the repository`
    
-   `#16 Documentation Improvement: Add a Beginner-friendly Quick Start Guide`
    

我感兴趣的是 **#90 Core/MCP: bound Capability tree complexity and reject cycles**。这个问题和 AI Agent 安全很相关。如果 Capability tree 太复杂，或者出现循环结构，Agent 可能很难正确解释交易意图，也可能带来验证和安全风险。这个 Issue 说明 Moss 的 Maintainer 不只关心“支持更多协议”，也关心 Agent 调用链上能力时的复杂度边界。

## 6\. Pull Requests 观察

Pull Requests 里可以看到很多协议接入和核心能力改进，例如：

-   `#111 fix(core): freeze delegated receipt evidence`
    
-   `#109 feat(pendle): add PT swap adapter`
    
-   `#104 feat(protocols): add aPriori aprMON liquid staking adapter`
    
-   `#102 feat(core): observe Query metadata for OnChain labels`
    
-   `#96 feat(protocols): add Kintsu staking adapter`
    
-   `#91 fix(core): bound Capability tree complexity`
    
-   `#84 feat(protocol/uniswap-v4): ship Uniswap v4 adapter on Monad`
    
-   `#38 feat(protocols): add Aave v3 lending adapter with MCP registration`
    

这些 PR 说明 Moss 的开发方向主要有两条线：一条是接入更多 Monad 生态协议，另一条是强化 core / simulator / MCP 的安全和表达能力。

## 7\. Discussions 观察

我在仓库导航里主要看到 Code、Issues、Pull requests、Actions、Projects、Security、Insights 等模块，没有明显看到 Discussions 模块。因此我理解 Moss 目前更依赖 Issues 和 PR 来组织协作，而不是通过 Discussions 做开放式社区讨论。

## 8\. GitHub 各模块作用

-   Code：查看源码、目录结构、README 和发布版本。
    
-   Docs：理解项目怎么使用、怎么扩展、怎么保证安全。
    
-   Issues：记录待解决问题、功能需求、协议接入和文档改进。
    
-   Pull Requests：查看实际代码贡献、维护者如何 review 和管理变更。
    
-   Actions：通常用于自动化测试、构建、lint 和 CI。
    
-   Projects：可能用于项目任务管理和路线图。
    
-   Security：查看安全政策和漏洞报告方式。
    
-   Insights：观察项目活跃度、贡献者和代码变化趋势。
    

## 9\. 我的学习收获

这次阅读 Moss 让我第一次比较系统地理解了一个真实开源项目是怎么组织的。README 负责快速解释项目价值，Docs 负责把使用和贡献流程讲清楚，Issues 负责记录问题和需求，PR 负责承载真实代码变更，Security 和 Contributing 则负责给协作设边界。

我最大的收获是：一个 AI Agent + Web3 项目不能只追求“Agent 能帮我操作链上协议”，还必须重视安全边界。Moss 的设计让我看到一种更谨慎的方式：Agent 可以发现能力、构建交易、模拟结果、解释 Receipt，但不应该直接替用户签名和发送交易。未来这类框架可能会成为 AI 钱包、DeFi 助手、自动化链上任务、协议聚合器的重要基础设施。

参考来源：  
[Moss README](https://github.com/nishuzumi/moss)、[Moss Issues](https://github.com/nishuzumi/moss/issues)、[Moss Pull Requests](https://github.com/nishuzumi/moss/pulls)
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->



进入 Week 3 团队后，我希望主要承担 **Ops / Product Ops** 角色。我目前的方向是基于 Monad 的高频交互场景，继续推进 **Monad Rank Rush**：一个链上社交小游戏 + 实时排行榜 + 任务徽章 + Meme 传播的 Campaign 原型。我能负责的部分不是复杂合约开发，而是把产品机制设计得更适合用户参与和社区传播。

具体来说，我可以承担三类工作：第一，设计用户参与路径，比如用户如何进入活动、完成挑战、刷新排行榜、领取徽章、分享战绩图；第二，设计运营机制，包括 7 天挑战活动、每日任务、排行榜规则、奖励节奏、Meme 文案和 X / Discord 发布内容；第三，协助 Dev 同学把 Ops 方案拆成清楚的功能需求，比如哪些数据需要上链、哪些数据留在链下、战绩卡需要哪些字段、徽章触发条件是什么。

我希望队友里有 **Dev** 同学，能负责钱包连接、简单小游戏、成绩提交、排行榜和基础合约；也希望有 **Research** 同学，帮助研究 Monad 生态、Consumer Crypto 案例、Onchain Social 或任务系统的竞品，为产品方向提供更多参考。

我能提供的 Proof of Work 包括：Week 1 完成的 GitHub 作品 **Monad Rank Rush Campaign Kit**，里面包含 README、Campaign Kit、钱包截图、Meme 卡片模板和链上交易记录；Week 2 我整理了学习记录，包括 Prompt、资料链接、错误修正、链上 / 链下数据判断表、7 天活动方案初稿和 X Thread 初稿。  
作品链接：[https://github.com/euphoria17721-tech/monad-rank-rush](https://github.com/euphoria17721-tech/monad-rank-rush)
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->




\# Week 2 学习记录：Monad / Web3 / Ops

\## 0. 本周主方向

本周主方向：\*\*Ops / Product Ops\*\*

我延续 Week 1 的作品 **Monad Rank Rush Campaign Kit**，继续围绕 Monad 高频交互、Consumer Crypto、链上社交小游戏、排行榜任务系统和 Meme 传播机制做学习记录。

本周记录重点：

\- 资料链接

\- Prompt 记录

\- AI 输出与人工修改

\- 截图 / 链上记录

\- 遇到的错误

\- 判断变化

\- 下一步计划

\---

\## 1. 本周目标

本周我希望从 Week 1 的概念型 Mini Demo，继续推进到更具体的 Ops 方案。

我想重点解决的问题是：

1\. Monad Rank Rush 如果真的做成社区活动，用户为什么愿意参与？

2\. 排行榜、任务、徽章、Meme 战绩图之间如何形成一个增长循环？

3\. 哪些数据应该上链，哪些数据只需要前端或后端记录？

4\. Ops 如何给 Dev / Research 同学提供清楚的协作需求？

5\. Week 3 我希望承担什么角色？

\---

\## 2. 资料链接记录

| 日期 | 链接 | 类型 | 我为什么看它 | 学到什么 |

| --- | --- | --- | --- | --- |

| 2026-07-19 | [https://docs.monad.xyz/](https://docs.monad.xyz/) | 官方文档 | 理解 Monad 基础能力和开发者定位 | Monad 的价值不只是“更快”，而是能支持低延迟、高频反馈的应用场景 |

| 2026-07-19 | [https://testnet.monadvision.com/](https://testnet.monadvision.com/) | 区块浏览器 | 查看钱包地址、交易哈希和链上记录 | 钱包地址和交易状态可以公开验证，适合作为链上实践记录 |

| 2026-07-19 | [https://github.com/euphoria17721-tech/monad-rank-rush](https://github.com/euphoria17721-tech/monad-rank-rush) | Week 1 作品 | 整理自己的 Mini Demo 0 作品链接 | 把学习内容整理成 GitHub 仓库后，更像一个可展示作品 |

| 2026-07-19 | Farcaster / X / Discord 社区内容 | 社区传播观察 | 观察 Web3 社区如何用梗、挑战和身份标签传播 | 社区传播不只是信息发布，更需要用户愿意转发的“社交瞬间” |

| 2026-07-19 | Consumer Crypto / Onchain Social 案例 | 竞品方向 | 参考排行榜、任务、徽章、邀请机制 | 高频交互产品需要让用户不断产生“再玩一次”和“发出去”的理由 |

\---

\## 3. Prompt 记录

\### Prompt 1：方向判断

**我问 AI：**

\> 选择 Research / Ops / Dev 一个主方向，说明选择理由、服务的问题、本周最小产出、参考资料和 Week 3 角色。

**AI 帮我输出了：**

\- 建议选择 Ops。

\- 理由是 Week 1 的 Monad Rank Rush 更偏社区活动、传播机制和用户参与循环。

\- 本周最小产出可以是 7 天活动机制、Meme 战绩图模板、X Thread 初稿和链上 / 链下数据判断表。

**我自己的判断：**

这个建议和我目前兴趣比较一致。我不是完全不想做 Tech，而是这阶段更想先把用户为什么参与、为什么分享、为什么回流想清楚。Monad 的高频能力最终要转化成用户能感知的活动机制，而不是只停留在技术说明里。

\### Prompt 2：学习感想

**我问 AI：**

\> 根据我上面的内容帮我做个总结和感想让我记录在笔记里，比如学到了什么。

**AI 帮我输出了：**

\- Web3 产品不是普通 App 加钱包登录。

\- 高频交互不等于所有行为都上链。

\- Monad 的优势要转化成具体用户体验。

\- Ops 不是后期宣传，而是产品机制的一部分。

**我自己的判断：**

这部分比较贴近我这周真正的理解变化。尤其是“Ops 是产品机制的一部分”这一点，对我很重要。我开始意识到排行榜、任务、Meme 图和徽章不是活动包装，而是用户增长循环的一部分。

\### Prompt 3：继续写学习笔记

**我问 AI：**

\> 继续写一个，我今天又要交逼急了。

**AI 帮我输出了：**

\- 一篇 Week 2 学习记录与阶段总结。

\- 内容包括为什么继续选择 Ops、为什么用户体验比技术参数更重要、Ops 如何参与产品机制设计、Week 3 想继续推进什么。

**我自己的判断：**

这次输出更适合作为学习笔记，因为它不是单纯回答作业要求，而是像连续记录一样整理了我的理解变化。后续我可以继续沿用这种写法，每天记录自己今天学到了什么、判断有没有变化、下一步要补什么。

\### Prompt 4：建立学习记录

**我问 AI：**

\> 建立本周学习记录，持续记录资料链接、Prompt、截图、错误、判断变化和下一步计划。

**AI 帮我输出了：**

\- 一个 Week 2 Learning Log 模板。

\- 包含资料链接、Prompt、截图、错误、判断变化、今日记录和下一步计划。

**我自己的判断：**

这个模板很适合后面持续记录。今天我把它补全成可以提交的版本，同时也保留了后续可以继续追加的结构。

\---

\## 4. 截图 / 链上记录

\### 已有截图

\- 钱包截图`outputs/image.png`

\- 说明：钱包已切换到 Monad Testnet，并显示测试网 MON 余额。

\### 钱包与交易记录

| 类型 | 内容 |

| --- | --- |

| 钱包地址 | `0x29152fB7ac93aED4336f118031f5B4BbF3A02287` |

| 地址浏览器链接 | [https://testnet.monadvision.com/address/0x29152fB7ac93aED4336f118031f5B4BbF3A02287](https://testnet.monadvision.com/address/0x29152fB7ac93aED4336f118031f5B4BbF3A02287) |

| 交易记录 1 | [https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7](https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7) |

| 交易记录 2 | [https://testnet.monadvision.com/tx/0x3362250756726049c6cdae58b0f725ffc6fe9fd4f0cf036e21f938a54eb3f5bc](https://testnet.monadvision.com/tx/0x3362250756726049c6cdae58b0f725ffc6fe9fd4f0cf036e21f938a54eb3f5bc) |

\### 敏感信息检查

\- 不记录私钥。

\- 不记录助记词。

\- 不记录 API Key。

\- 不上传 `.env` 文件。

\- 不放未公开会议链接。

\---

\## 5. 错误 / 卡点记录

\### 错误 1：一开始把“高频交互”理解得太链上化

**问题：**

我一开始容易觉得 Web3 应用应该尽量把行为都放到链上。

**修正：**

后来我意识到，高频交互不是每次点击都上链，而是把真正有价值的结果上链。比如最终成绩、任务完成、奖励领取和赛季徽章更适合记录；点击过程、动画、临时分数可以放在链下。

\### 错误 2：早期提交材料更像作业模板

**问题：**

最开始的提交内容太像文字说明，不够像一个能展示的作品。

**修正：**

后来把它改成了 **Monad Rank Rush Campaign Kit**，增加了 README、Campaign Kit、Meme 卡片模板、钱包截图和 GitHub 仓库。这样更像一个轻量作品，而不是只有学习总结。

\### 错误 3：作品链接不够正式

**问题：**

一开始只使用本地文件路径，不能直接作为公开作品链接提交。

**修正：**

后来上传到 GitHub，并使用仓库链接作为正式作品链接：

[https://github.com/euphoria17721-tech/monad-rank-rush](https://github.com/euphoria17721-tech/monad-rank-rush)

\### 错误 4：容易只写“Monad 更快”

**问题：**

一开始解释 Monad 的时候，容易停留在“更快、性能更高”这种比较泛的说法。

**修正：**

现在我会把它放回具体产品场景里解释：如果是小游戏、排行榜、任务系统，用户需要快速反馈、低成本更新和频繁状态变化。Monad 的性能优势要落到这些具体体验上，才有说服力。

\---

\## 6. 判断变化记录

\### 判断变化 1：从“Monad 更快”到“Monad 能支持哪些用户体验”

我不再只是把 Monad 理解成高性能 EVM 链，而是开始思考它适合什么产品场景。对我来说，Monad 更适合承载高频、低延迟、反馈密集的 Consumer Crypto 应用，比如小游戏、排行榜、任务系统、徽章身份和社交挑战。

\### 判断变化 2：从“做一个 Demo”到“设计一个用户循环”

我一开始关注的是能不能交一个 Demo。现在我更关注这个 Demo 有没有用户循环：用户为什么玩、为什么回来、为什么分享、为什么邀请朋友。

\### 判断变化 3：从“Ops 是宣传”到“Ops 是产品机制”

我现在觉得 Ops 不只是发推、写活动公告或整理社群，而是要参与产品机制设计。排行榜、任务、奖励、徽章、Meme 图和分享路径，本身就是 Ops 需要设计的东西。

\### 判断变化 4：从“链上记录越多越好”到“链上记录要有价值”

我现在更认同一种判断：不是所有数据都应该上链，只有那些涉及公开验证、资产归属、排名竞争、身份沉淀和跨应用复用的数据，才更值得上链。

\---

\## 7. 本周最小产出计划

本周最小产出目标：

1\. 完成一份更具体的 **Monad Rank Rush 7 天活动方案**。

2\. 设计 3-5 个 Meme 战绩图文案模板。

3\. 整理一张链上 / 链下数据判断表。

4\. 写一条 X Thread 初稿。

5\. 准备一份给 Dev 同学看的最小功能需求。

\---

\## 8. 本周最小产出初稿

\### 8.1 Monad Rank Rush 7 天活动方案

活动名：\*\*Monad Rank Rush: 7-Day Leaderboard Sprint\*\*

活动目标：

\- 让用户体验 Monad Testnet 高频交互。

\- 用排行榜制造竞争。

\- 用 Meme 战绩图制造传播。

\- 用赛季徽章沉淀身份。

活动机制：

| 天数 | 主题 | 用户任务 | 传播点 |

| --- | --- | --- | --- |

| Day 1 | Join the Rush | 连接钱包，完成第一次挑战 | 晒出首局分数 |

| Day 2 | Beat Your Friend | 邀请好友挑战同一关卡 | “我超过你了”分享图 |

| Day 3 | Top 100 Chase | 冲击排行榜 Top 100 | 排名变化截图 |

| Day 4 | Meme Day | 分享最离谱战绩图 | 社区投票选最佳 Meme |

| Day 5 | Comeback Day | 排名下降后重新挑战 | “Run it back”主题 |

| Day 6 | Badge Hunt | 完成任务领取徽章 | 晒出赛季身份 |

| Day 7 | Final Sprint | 最后冲榜与赛季结算 | 公布榜单和称号 |

\### 8.2 Meme 战绩图文案模板

1\. **Monad Tap King**

用于高分玩家，强调排名和胜利感。

2\. **One Point Away From Glory**

用于差一点上榜的玩家，适合制造不甘心和二次挑战。

3\. **Publicly Replaced**

用于被好友反超的玩家，适合社交传播。

4\. **Transaction Confirmed, Skill Not Found**

用于低分或失败玩家，让失败也能变成梗。

5\. **Top 100 Survivor**

用于赛季结算仍留在 Top 100 的玩家，适合作为徽章身份。

\### 8.3 链上 / 链下数据判断表

| 数据类型 | 是否上链 | 理由 |

| --- | --- | --- |

| 游戏点击过程 | 不上链 | 频率太高，适合前端处理 |

| 动画和实时反馈 | 不上链 | 主要服务体验，不需要公开验证 |

| 临时分数 | 不上链 | 可以先由前端或后端记录 |

| 最终成绩 | 上链 | 影响排行榜和奖励，需要可验证 |

| 任务完成记录 | 上链 | 影响奖励领取和用户进度 |

| 奖励领取记录 | 上链 | 涉及资产归属 |

| 赛季徽章 | 上链 | 代表身份和长期记录 |

| 排行榜快照 | 可部分上链 | 重要节点可上链，实时排序可链下计算 |

\### 8.4 X Thread 初稿

1\. What if a Monad mini-game could turn every score into a social moment?

2\. Monad Rank Rush is a 30-second leaderboard challenge built around fast feedback, friend competition, and meme result cards.

3\. Not every click needs to be onchain. The game can stay fast offchain, while final scores, badges, and reward claims become verifiable records.

4\. The core loop is simple: play, submit score, climb the board, unlock a badge, share the result, challenge a friend.

5\. The Ops idea: make winning, losing, and getting replaced all worth sharing.

6\. Week 2 focus: turn this into a 7-day community campaign with leaderboard rules, daily tasks, meme templates, and onchain identity hooks.

\### 8.5 给 Dev 同学的最小功能需求

如果后续和 Dev 同学合作，我希望第一版 Demo 至少包括：

\- 钱包连接。

\- 30 秒挑战按钮或简单小游戏。

\- 分数展示。

\- 提交最终成绩。

\- 简单排行榜。

\- 任务完成状态。

\- 徽章 / 称号展示。

\- 一张可分享的战绩图。

第一版不需要复杂经济模型，也不需要把所有动作都上链。重点是让用户能完成一轮完整体验。

\---

\## 9. 今日记录

\### 2026-07-19

今天补全了本周学习记录文件，用来持续记录资料链接、Prompt、截图、错误、判断变化和下一步计划。

今天的主要想法：

我现在更明确自己 Week 2 / Week 3 想继续走 Ops / Product Ops 方向。相比单纯解释 Monad 的技术优势，我更想把它转化成用户看得懂、愿意参与、愿意传播的产品机制。Monad Rank Rush 对我来说不是一个完整产品，而是一个帮助我理解 Web3 产品运营的练习：如何把链上记录、排行榜、任务和社区传播结合起来。

今天的学习收获：

\- Web3 产品不能只是“普通 App + 钱包连接”。

\- 高频交互不代表所有行为都要上链。

\- Ops 应该参与产品机制设计，而不是只做后期宣传。

\- 一个好的社区活动应该让用户自然产生分享理由。

今天的下一步：

\- 把 7 天活动方案继续细化成可提交版本。

\- 给 Meme 战绩图模板补更多文案。

\- 继续整理 X Thread。

\- 如果有机会，找 1-2 个类似 Consumer Crypto 案例做参考。

\---

\## 10. 下一步计划

短期：

\- 完成 7 天 Campaign 方案。

\- 完成 X Thread 初稿。

\- 给 Meme 卡片模板补更多文案。

中期：

\- 和 Dev 同学沟通最小可开发功能。

\- 判断第一版 Demo 需要哪些链上记录。

\- 找 2-3 个 Consumer Crypto 或 Onchain Social 案例做参考。

长期：

\- 把 Monad Rank Rush 从一个 Campaign Kit 推进成可演示的社区活动原型。

\- 形成可以写进作品集 / 简历的完整项目经历。

\---

\## 11. 本周阶段性总结

这周我最大的变化，是开始把 Monad / Web3 学习从“技术理解”转向“产品机制理解”。

一开始我会比较关注链本身，比如钱包、交易、区块浏览器、交易哈希、合约交互。但现在我更关心这些链上能力如何变成真实的用户体验。Monad 的高性能如果只写在文档里，用户其实感受不到；但如果它能支持更快的小游戏结算、更频繁的排行榜刷新、更低成本的任务奖励和更及时的徽章领取，用户就能真正感受到它的价值。

我也更确认自己适合继续做 Ops 方向。因为我感兴趣的问题是：一个 Web3 产品怎么让用户愿意参与？怎么让用户愿意分享？怎么让链上记录变成身份和社交资产？这些问题不是单纯靠合约就能解决，而是需要产品和运营一起设计。

一句话总结：

**我这周学到的是，好的 Web3 Ops 不是把产品宣传出去，而是让产品本身不断产生值得被分享、被验证、被记录的瞬间。**
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->





# Web3 学习笔记续篇

今天继续整理 Week 2 的学习内容，其实这几天我对自己方向的判断比一开始清楚了很多。刚开始做 Monad 相关任务的时候，我更多是在想“我要交什么”“我要做出什么东西”，但现在慢慢发现，这个过程其实也在帮我判断自己更适合在 Web3 里做哪一类事情。

Week 1 的时候，我做了 Monad Rank Rush 这个方向，一个基于 Monad 的链上社交小游戏 + 排行榜 + 任务徽章 + Meme 传播方案。当时更多是在搭一个概念框架：这个产品为什么需要高频交互，为什么 Monad 可能适合，哪些数据应该上链，哪些数据不用上链。到了 Week 2，我开始更关注这个东西如果真的给用户玩，它凭什么让用户愿意留下来。

我发现自己对 Ops 的兴趣其实越来越明显。相比写一个复杂合约，我更在意的是用户参与的理由。比如一个小游戏，用户为什么要玩第二次？为什么要截图发出去？为什么要喊朋友来挑战？为什么会因为排名下降而想回来再玩？这些问题听起来不像技术问题，但它们决定了一个 Consumer Crypto 产品有没有生命力。

这周我最大的感受是，Monad 的高性能不能只被写成技术优势，而要被翻译成用户体验。用户不会因为“这条链 TPS 高”就留下来，但用户会因为“我刚刚被朋友超过了”“我差 3 分进榜”“我拿到了一个赛季徽章”“这张失败战绩图太好笑了”而产生参与感。也就是说，技术优势最后要变成一个个具体的产品瞬间。

我也更理解了为什么不是所有东西都要上链。之前我会觉得 Web3 应用是不是应该尽量把交互都放到链上，但现在我觉得这反而不合理。游戏点击、临时状态、动画反馈这些东西放链上没有太大意义，还会让体验变慢。真正值得上链的是最终成绩、任务完成、奖励领取、排行榜记录和徽章身份，因为这些东西和公开验证、资产归属、社区竞争有关。

所以我现在对 Web3 产品的理解变成了：不是为了上链而上链，而是判断什么东西值得成为公共记录。这个判断其实很重要，也不是 AI 能完全替我决定的。AI 可以帮我整理思路、写文档、拆结构，但最后我要自己判断这个方向是不是成立，传播文案是不是自然，社区用户会不会真的愿意参与。

这也是为什么我 Week 2 选择继续走 Ops。对我来说，Ops 不只是发推或者写活动公告，而是设计一种用户循环。排行榜是循环的一部分，任务是循环的一部分，Meme 图是循环的一部分，徽章身份也是循环的一部分。一个好的 Ops 方案应该让用户在产品里不断产生“我想再试一次”“我想发出去”“我想叫朋友来”的动机。

接下来 Week 3，我希望继续把 Monad Rank Rush 往更具体的方向推进。现在它还只是一个 Campaign Kit，但我希望下一步能把它拆成一个更完整的 7 天社区挑战活动，包括每日任务、排行榜规则、徽章设置、Meme 战绩图模板和 X / Discord 文案。如果有 Dev 同学合作，我也希望能把这些运营机制转化成更清楚的功能需求。

这几天学下来，我觉得自己最大的变化是：我不再只把 Web3 看成一个技术学习任务，而是开始把它看成一个产品和社区问题。链上记录、钱包、交易哈希、区块浏览器这些是基础，但真正难的是怎么让用户觉得这些链上行为和自己有关。Monad 给了高频交互的基础设施，但产品要做的事情，是把这种能力变成用户能感受到、愿意参与、愿意传播的体验。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->






这两周我围绕 Monad 和 Web3 高频交互场景做了一次比较完整的入门实践。从一开始只是知道 Monad 是一条高性能 EVM 链，到后来逐渐理解它更适合承载什么类型的产品，我对 Web3 应用的理解变得具体了很多。

我最开始会比较自然地把 Web3 产品理解成“普通 App 加钱包登录”，但在整理 Monad Rank Rush 这个方向时，我发现钱包连接只是入口，真正重要的是判断哪些状态值得上链。不是所有用户行为都应该记录到链上，尤其是点击过程、动画、临时分数、普通 UI 反馈，这些放在前端或后端会更合理。真正适合上链的是最终成绩、排行榜积分、任务完成记录、奖励领取、赛季徽章和用户身份，因为这些内容涉及公开验证、资产归属、社区竞争和跨应用复用。

这也是我这次最大的收获：**高频交互不等于所有动作都上链，而是要把有价值的结果上链。** 如果一个小游戏每一次点击都要发交易，用户体验会非常差；但如果只把最终战绩、徽章和奖励记录上链，就能在保留流畅体验的同时，发挥区块链的公开性和可验证性。

我选择 Ops 方向，是因为我发现自己更感兴趣的不只是“能不能做出来”，而是“用户为什么愿意玩、愿意分享、愿意邀请朋友”。Monad 的高性能本身不是产品卖点，真正能让用户感知到的是更快的挑战反馈、更及时的排行榜变化、更低成本的任务奖励，以及更容易传播的 Meme 战绩图。技术需要转化成用户体验，才会变成真正的 Consumer Crypto 产品。

在设计 Monad Rank Rush 的过程中，我也意识到 Ops 不只是宣传。排行榜、任务系统、徽章、Meme 图、好友反超提醒，本身就是产品机制的一部分。好的运营不是产品做好之后再发推，而是在产品设计阶段就考虑用户会在什么瞬间截图、转发、喊朋友、回流。

AI 在这个过程中帮我做了很多整理工作，比如帮我把零散想法变成 README、Campaign Kit 和提交文档，也帮我拆解 Tech / Ops / Research 不同方向的区别。但有些判断必须由我自己完成，比如我到底适合哪个方向、哪些链上记录是真实完成的、哪些内容适合公开、Meme 文案是否符合社区语气，以及 Week 2 应该继续推进什么问题。

接下来我想继续沿着 Ops / Product Ops 方向深入。我的重点不是马上做一个复杂 DApp，而是先把 Monad Rank Rush 变成一个更完整的社区 Campaign：包括 7 天挑战活动、排行榜规则、任务徽章机制、3–5 张战绩图模板、X Thread 和 Discord 发布文案。如果后续能和 Dev 同学合作，再把这些运营机制拆成最小可开发功能。

总的来说，这次学习让我对 Web3 的理解从“链上技术”变成了“链上产品”。我开始意识到，一个好的 Web3 产品不是把所有东西都搬到链上，而是判断哪些东西值得公开、值得验证、值得被用户拥有，并且让这些链上记录服务于真实的用户体验和社区传播。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->







认真看完这份 Web3 运营全景流程图，很多之前模糊的思路一下子理顺了，也纠正了我长久以来对 Web3 运营比较片面的理解。

在此之前，我一直简单认为 Web3 运营的工作重心就是维护社群、组织线上活动、日常和社区成员沟通。但这份流程图完整展示了从目标设定一直到生态持续增长的九大闭环流程：先明确项目目标，开展 BD 拓展合作伙伴，配合市场宣传推广，搭建开发者激励计划，招募 Builder，深耕社区运营，接收项目申报，组织评审反馈，最后落地孵化实现生态增长，并且全程依靠数据分析持续复盘迭代。

一条链路走下来才意识到，社群运营仅仅是其中一环。前端需要主动对外联动资源、对接合作；后端还要跟进项目孵化、持续扶持开发者。所有零散的工作都指向同一个终点 —— 打造一个可以自我循环、可持续发展的开发者生态，而不只是短暂热闹的社区。

其中 AI 如何重塑运营工作流这部分让我感触很深。不管是前期调研整理资料、撰写方案，中期策划整套活动物料，还是后期多平台、多语种内容的拆分分发，AI 都能承接大量重复、机械的基础工作，极大降低从零启动一件事的时间成本。但图里一句话点透关键：AI 不会取代运营人。信息汇总、文案初稿都可以交给工具，可方向判断、策

略制定、资源谈判、复杂问题的权衡，依旧需要人来主导。对我而言，更重要的是学会善用工具解放精力，把时间投入到更高价值的思考与沟通中。

运营日程时间轴也高度还原了真实的工作状态。一整天被社群沟通、合作对接、方案撰写、活动落地、数据复盘、项目跟进填满，事务琐碎、切换频繁。想要做好这份工作，沟通协调、项目落地、数据分析、内容策划、长期关系维护几项能力缺一不可。

最打动我的是最后这句总结：运营的本质不是办活动，而是解决问题、连接资源、创造价值，让生态持续增长。

活动、推文、社群互动、AMA 都只是手段。往后我也提醒自己，不要陷入单纯埋头执行的误区，做每一件事之前都往上多想一层：这件事能连接什么资源，可以解决生态里的什么问题，能不能为长期生态增长带来正向价值。学会站在完整生态的视角审视日常工作，跳出基础执行思维，建立更全局的思考方式。

![c50c3cb4cb81d2dbf94077f9bde96241.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-16-1784192714731-c50c3cb4cb81d2dbf94077f9bde96241.png)
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->








1\. Week 1 作品链接

作品名：Monad Rank Rush Campaign Kit

当前本地作品文件：

\- `README.md`

\- `outputs/Campaign-Kit.md`

\- `outputs/meme-card-template.svg`

\- `outputs/image.png`

GitHub 公开链接：

[https://github.com/euphoria17721-tech/monad-rank-rush/tree/main](https://github.com/euphoria17721-tech/monad-rank-rush/tree/main)

2\. Demo 截图、录屏、README 或公开说明文档

我提交的是一个轻量级 README + Campaign Kit + 截图 + Meme 卡片模板。

Demo 说明：

Monad Rank Rush是一个 Ops-first 的 Mini Demo 0，围绕 Monad 高频交互场景设计。它不是完整 DApp，而是一个产品与运营方向验证：用户完成 30 秒小游戏挑战后，可以提交最终成绩、刷新排行榜、领取任务奖励，并生成可分享的 Meme 战绩图。

已包含材料：

\- README`README.md`

\- Campaign Kit`outputs/Campaign-Kit.md`

\- 钱包截图`outputs/image.png`

\- Meme 战绩卡模板`outputs/meme-card-template.svg`

真实链上实践记录：

\- 钱包地址`0x29152fB7ac93aED4336f118031f5B4BbF3A02287`

\- 地址浏览器链接`https://testnet.monadvision.com/address/0x29152fB7ac93aED4336f118031f5B4BbF3A02287`

\- 交易记录 1`https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7`

\- 交易记录 2`https://testnet.monadvision.com/tx/0x3362250756726049c6cdae58b0f725ffc6fe9fd4f0cf036e21f938a54eb3f5bc`

 3. 我的方向选择

Ops：

我选择 Ops，因为我更想继续研究如何把 Monad 的高频交互能力变成用户愿意参与、分享、邀请朋友的社区机制。相比只做一个技术 Demo，我更想探索排行榜竞争、任务系统、Meme 传播、赛季徽章和社区 Campaign 如何结合。

4\. 一句话说明这个作品可以如何写进作品集或简历

设计 Monad 高频交互 Consumer Crypto Mini Demo，围绕链上小游戏、实时排行榜、任务徽章和 Meme 传播机制，完成产品方向分析、Campaign Kit、链上实践记录和 Week 2 Ops 路线规划。

5\. Week 2 想继续推进的问题

1\. 如何把 Monad Rank Rush 做成一个更完整的社区活动方案？

2\. 排行榜、任务、徽章中，哪些数据最值得优先上链？

3\. Monad 社区更适合哪种 Meme 风格：竞技、抽象、交易员梗，还是生态文化？

4\. 如何设计 3-5 张可以直接分享的战绩图模板？

5\. 如果和 Tech 同学合作，Ops 侧最小可交付物应该是什么？
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->









![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-14-1784006262654-__.png)

\# Week 1 Build Log：Monad / Web3 高频交互方向学习记录

\## 1. Week 1 Build Log 初稿

\### 本周目标

Week 1 的主要目标是完成 Monad / Web3 入门实践，并初步判断自己更适合 Week 2 的 Tech、Ops 还是 Research 分轨。我重点关注 Consumer Crypto、高频交互、链上社交小游戏、排行榜任务系统等方向，思考 Monad 适合承载什么类型的用户行为。

\### 链上实践记录

本周完成或准备完成的链上实践包括：

\- 创建或准备 Web3 钱包，用于后续连接 Monad 生态应用。

\- 添加 Monad 相关网络信息，理解 RPC、Chain ID、区块浏览器等基础概念。

\- 体验测试网交互流程，包括连接钱包、确认交易、查看交易状态。

\- 学习如何通过区块浏览器查看地址、交易哈希、区块确认和合约交互记录。

\- 初步了解智能合约交互和普通数据库记录之间的区别。

钱包和交易记录：

\- 钱包地址：0x29152fB7ac93aED4336f118031f5B4BbF3A02287

\- 交易哈希 1：0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7

\- 交易哈希 2：0x3362250756726049c6cdae58b0f725ffc6fe9fd4f0cf036e21f938a54eb3f5bc

\- 交互类型：领取测试币 / 合约交互 / DApp 连接 / 排行榜或任务类交互

\- 区块浏览器链接：\[[https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7\](https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7)](https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7]\(https://testnet.monadvision.com/tx/0x07bfeb001d6cb1da695f915d8d7bfee4a0a3798de745d1bb906c9620dc6c35c7\))

\### 合约或交互记录

本周没有重点部署复杂合约，而是先从“高频交互产品应该如何上链”这个问题切入。我的理解是：不是所有用户行为都应该上链，尤其是游戏点击、临时状态、前端动画、普通 UI 反馈，这些更适合放在前端或服务器处理。

更适合上链的是：

\- 最终战绩

\- 排行榜积分

\- 任务完成记录

\- 奖励领取记录

\- NFT / 徽章 / 赛季身份

\- 用户声望或可组合的社交资产

这让我对链上产品的理解从“把功能搬到链上”变成了“判断哪些状态值得公开、可验证和可组合”。

\### 遇到的问题

1\. **一开始容易把 Web3 产品理解成普通 App 加钱包登录。**\\

后来发现真正重要的是：哪些状态需要用户拥有，哪些状态需要公开验证，哪些状态有跨应用复用价值。

2\. **对链上交互频率的理解不够清晰。**\\

高频交互并不代表每一次点击都要发交易。合理方式应该是前端快速反馈，关键结果再上链。

3\. **对 Ops 和 Tech 的边界有些模糊。**\\

比如排行榜、任务系统、Meme 战绩图既是产品功能，也是传播机制。Week 1 后我意识到 Ops 不只是发推或组织活动，而是要把传播点设计进产品循环里。

4\. **链上记录和数据库记录的取舍需要人工判断。**\\

AI 可以提供框架，但最终是否上链，要看这个记录是否涉及资产、身份、公开竞争、信任和长期复用。

\### AI 帮我解决了什么

AI 主要帮我完成了以下事情：

\- 帮我从多个方向中选择了一个更具体的高频交互场景：链上社交小游戏 + 实时排行榜任务系统。

\- 帮我拆解了为什么这个场景适合 Monad，而不是只停留在“Monad 更快”。

\- 帮我整理了 Research、Tech、Ops 三个方向的提交内容。

\- 帮我扩展了 Ops 方向的产品传播切入点，包括好友反超、战绩 Meme 化、链上赛季身份。

\- 帮我把模糊想法整理成可以提交的结构化文档。

\### 人工修改和判断记录

AI 给出的内容里，我需要人工判断和修改的部分包括：

\- 是否真的选择 Ops 作为 Week 2 主方向。

\- 哪些链上实践是我实际完成的，哪些只是计划或理解。

\- 钱包地址、交易哈希、截图是否可以公开。

\- 产品传播点是否符合 Monad 社区气质。

\- Meme 文案是否过度夸张，是否适合公开发在 X / Discord / Telegram。

\- Week 2 是否要做 Demo，还是先做用户研究和内容传播实验。

\### 我对 Monad / Web3 的理解变化

Week 1 之前，我对 Monad 的理解比较简单，主要是“高性能 EVM 链”。现在我的理解更具体了：Monad 的意义不只是更快，而是让一些过去不适合链上的高频消费级应用有机会成立。

例如小游戏、排行榜、任务系统、社交徽章、AI Agent 行为记录等场景，都需要快速反馈和低成本状态更新。如果链上确认太慢或手续费太高，用户体验会被打断，产品只能退回中心化数据库。

但我也意识到，不是所有东西都要上链。真正适合上链的是有公开验证、资产归属、身份沉淀和跨应用组合价值的结果。普通过程数据仍然可以留在前端或后端。

\### Week 2 希望深入的方向

我希望 Week 2 重点进入 **Ops 方向**，围绕“链上社交小游戏 + 实时排行榜任务系统”继续深入。

具体想做：

\- 设计一个适合 Monad 社区传播的小游戏概念。

\- 做 3–5 个 Meme 化传播模板。

\- 设计排行榜、任务、赛季徽章的运营机制。

\- 写一条 X Thread 或 Discord 活动方案。

\- 如果时间允许，配合 Tech 同学做一个简单 Demo 页面或交互原型。

\---

\## 2. 我选择 Ops / Tech / Research 的初步理由

我初步选择 **Ops 方向**。

原因是我更感兴趣的问题不是单纯“怎么写合约”，而是“什么样的产品机制会让用户愿意反复参与、分享和邀请朋友”。在链上社交小游戏这个方向里，Ops 非常关键，因为产品增长不是靠解释技术参数，而是靠用户能不能感受到竞争、身份、梗和社区参与感。

我认为 Monad 的高频交互场景需要强运营设计，例如：

\- 好友排行榜制造竞争

\- 战绩图制造传播

\- 赛季徽章制造身份

\- 社区任务制造持续参与

\- KOL 挑战制造破圈入口

这些内容更接近我想深入的方向。Tech 可以帮产品跑起来，Research 可以解释为什么成立，而 Ops 负责让用户真的愿意玩、愿意晒、愿意留下来。

\---

\## 3. 本周最重要的 1–3 个学习收获

\### 收获 1：高频交互不等于所有行为都上链

游戏点击、临时状态、动画反馈不一定要上链。真正适合上链的是最终结果、奖励归属、排行榜凭证和身份记录。这样既能保留消费级体验，也能发挥链上的公开验证和可组合优势。

\### 收获 2：Consumer Crypto 的重点是体验，而不是教育用户技术

用户不会因为 TPS 参数而留下来，用户会因为“我赢了朋友”“我上榜了”“我拿到了稀有徽章”“这张战绩图很好笑”而留下来。技术性能应该服务于更顺滑的产品体验。

\### 收获 3：Ops 应该嵌入产品机制

好的 Ops 不是后期宣传，而是从产品设计阶段就开始考虑传播。排行榜、任务、Meme 图、赛季身份、社区挑战，本身就是增长机制的一部分。

\---

\## 4. 希望助教或同伴帮助的问题

1\. 我的 Week 2 方向选择 Ops 是否合理？如果我要补一点 Tech 能力，最小 Demo 应该做到什么程度？

2\. 对于“链上社交小游戏 + 排行榜任务系统”，哪些数据必须上链，哪些数据应该留在后端？

3\. 如果做 Monad 社区传播，什么样的 Meme 风格更适合？偏竞技、偏抽象、偏交易员梗，还是偏 Monad 生态文化？
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->










完成了一次部署合约交互

![559ca49f2d530da4cf31eba2c5195760.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-13-1783925534095-559ca49f2d530da4cf31eba2c5195760.png)
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->











用 AI 生成一个最小 Solidity 合约，学习中。。。。还是不太懂

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-12-1783851047639-image.png)
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->












重新复盘听完本节课内容，系统梳理了AI智能体自主支付的完整底层基础设施。明晰了通过搭建人机共管钱包、配套风控框架，能够有效管控AI自主操作，避免无序支出；同时熟悉了各类支付相关产品、实际落地应用场景，对行业后续的发展趋势也有了整体认知。

![IMG_7852.jpeg](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-11-1783778463090-IMG_7852.jpeg)
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->













我使用 Remix 编写并编译了一个最小化的 `MessageBoard` 留言板智能合约，通过课程专用钱包连接 Monad Testnet 后完成了合约部署，并记录了部署后的合约地址和部署交易 hash。部署完成后，我调用了 `message()` 等 read function 读取链上留言内容，又调用 `setMessage()` write function 修改留言，并再次读取确认链上状态已经更新。通过这次操作，我完整体验了从 Solidity 合约源码、编译、部署、链上交互到区块浏览器验证的全过程。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-10-1783649245388-image.png)
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->














![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-09-1783564839294-image.png)![bfef306100b3094ecd492b7707f8f629.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-09-1783564846939-bfef306100b3094ecd492b7707f8f629.png)

通过这次任务，我使用 AI 生成了一个最小 Solidity 留言板合约，但没有直接把 AI 输出作为最终结果，而是对代码进行了人工阅读和检查。

我主要检查了：

-   合约是否可以编译。
    
-   函数是否符合预期。
    
-   是否存在明显权限问题。
    
-   是否存在不必要的复杂逻辑。
    
-   是否有潜在安全风险。
    
-   命名是否清楚。
    

最终判断：  
该合约适合作为 Solidity 入门练习和 AI 辅助开发练习案例。如果用于正式项目，还需要增加更多安全设计和测试。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->















学会get测试币，会看transaction 的 detail了

![a0c4f59d6e4b1c087ca59258fdf65ba6.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-08-1783517225187-a0c4f59d6e4b1c087ca59258fdf65ba6.png)![a0c4f59d6e4b1c087ca59258fdf65ba6.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-08-1783517266853-a0c4f59d6e4b1c087ca59258fdf65ba6.png)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
















普通互联网产品所有数据都存在公司后台，账号、道具平台想封就封，里面的虚拟东西也只能在这个软件里用。 链上产品数据存在全网节点，改不了，资产是自己私钥说了算，平台拿不走，虚拟资产还能跨软件流转，规则提前写死没法后台暗改。

今天学会了创建钱包和连接区块链，并且进行了第一笔交易

![bea5c285b1515f93ffc6aeb23e81a861.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-07-1783417566733-bea5c285b1515f93ffc6aeb23e81a861.png)![53e40f31bb01e124aefe33ebfb94a63c.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-07-1783417577273-53e40f31bb01e124aefe33ebfb94a63c.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

















学习

![IMG_7709.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/euphoria17721-tech/images/2026-07-06-1783346226688-IMG_7709.png)

了从安装钱包插件、添加测试网、领取测试水，再到Remix部署交互合约并在区块浏览器查看交易的完整Web3实操流程。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
