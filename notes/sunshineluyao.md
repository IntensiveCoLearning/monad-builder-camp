---
timezone: UTC+8
---

# Sunshine

**GitHub ID:** sunshineluyao

**Telegram:** zlypioneer

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# DAY 4｜Test & Improve：把 Demo 交给真实的人

今天的学习重点是将 Mini Demo 交给团队之外的真实用户测试，并根据反馈完成最后一轮优化。我认识到，用户的口头认可并不等于产品真正有效，更重要的是观察用户能否在没有团队帮助的情况下理解项目，并独立完成核心操作。

测试过程中，需要重点记录用户在哪一步感到困惑、是否信任钱包连接、签名和交易提示，以及哪些功能真正有价值。对于 Web3 项目，还需要判断用户是否理解项目为什么适合部署在 Monad 上，而不是只强调速度快、费用低等笼统优势。

团队中的 Research、Ops 和 Dev 应共同参与反馈处理。Research 负责判断测试结果是否支持原有假设；Ops 记录用户的真实表达，优化项目叙事、操作引导和展示流程；Dev 则优先修复影响核心体验的问题，并将暂时无法解决的问题记录为 Known Issues。

在整理反馈时，可以按照 Must Fix、Should Fix、Later 和 Won’t Fix 进行分类，避免在有限时间内不断增加功能。完成最高优先级修改后，还需要保存修改前后的截图、版本记录或 GitHub 提交，作为 Iteration Evidence。

我今天最大的收获是：Demo 测试的目标不是证明团队做得很好，而是尽早发现用户无法理解、无法完成或不愿信任的地方。下一步，我将邀请至少三名测试者完成核心任务，根据测试结果修改一次产品流程或说明，并通过 Demo Freeze Checklist 确认最终展示版本、钱包、测试资产、交易链接、代码仓库和备份方案均可正常使用。
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->

**DAY 3 学习笔记｜Co-build：让研究、运营和开发真正一起工作**

今天的学习让我认识到，跨角色协作并不是 Research、Ops 和 Dev 各自完成一份任务，而是建立一条可以持续传递和迭代的交付链。Research 需要将用户问题、竞品、协议能力和风险整理成 Product Brief；Ops 再将研究结果转化为项目叙事、测试邀请和反馈问题；Dev 则依据明确的 Scope 完成最小可运行功能，并说明技术边界与已知问题。

其中，我认为最重要的是“中间成果必须能被下一个角色直接使用”。如果研究结论不清楚、运营材料与产品脱节，或开发没有及时反馈限制，团队就容易在最后合并时出现冲突。因此，每日 Stand-up 和 Decision Log 不只是管理工具，也能帮助团队尽早发现阻塞、记录证据与假设，并明确 AI 和人工判断各自发挥的作用。

今天的关键收获是：真正的协作不在于任务是否平均分配，而在于信息能否顺畅流动、决策是否可追溯、成果是否可以继续被加工。下一步，我会重点完善 Product Brief 和 Team Decision & AI Log，确保团队成员能够基于同一套信息继续推进项目。
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->

# 2026-07-06｜DAY 1：开营与 Web3 入门｜我为什么要进入链上世界？

今天是 Web3 暑期实习计划的第一天。今天的重点是了解 Web3、区块链、以太坊、钱包、地址、交易、Gas 和智能合约等基础概念，并开始建立自己的 Week 1 Build Log。

## 📚 今日学习内容

* [Web3 实习手册](https://web3intern.xyz/zh/)
* [前言 / 入门导读](https://web3intern.xyz/zh/preface/)
* [区块链基础概念](https://web3intern.xyz/zh/blockchain-basic/)
* [以太坊概览](https://web3intern.xyz/zh/overview-of-ethereum/)
* [BuildAnything Freshman：Vibecoding 入门](https://buildanything.so/zh/tracks/freshman/lessons/introduction-to-vibecoding)
* [BuildAnything Freshman Track](https://buildanything.so/zh/tracks/freshman)

## 🧠 我的理解

今天让我对 Web3 有了一个更清晰的初步认识。

我理解的 Web3，不只是“加密货币”或者“炒币”，而是一种尝试让用户拥有更多数据、资产和数字身份控制权的互联网协作方式。区块链提供了一个公开、可验证、难以随意篡改的记录系统，而以太坊则让开发者可以在链上部署智能合约和去中心化应用。

几个基础概念的理解如下：

* **区块链：** 一个由多个节点共同维护的分布式账本。
* **以太坊：** 一个支持智能合约和去中心化应用的平台。
* **钱包：** 用于管理链上身份和资产的工具；更准确地说，钱包帮助用户管理私钥。
* **地址：** 链上的公开身份标识，类似于可以接收资产的账号。
* **交易：** 用户向区块链提交的一次操作，例如转账、调用合约或铸造 NFT。
* **Gas：** 在以太坊等链上执行交易或计算时需要支付的网络费用。
* **智能合约：** 部署在区块链上的程序，可以按照预先设定的规则自动执行。

我觉得智能合约是今天最有意思的概念之一。它让我理解到，区块链不仅可以记录资产，也可以承载规则、协议和应用逻辑。这也是为什么人们会在链上构建金融、社群、游戏、身份和协作工具。

## 💭 为什么我想进入链上世界？

我想进入 Web3，不只是因为它是一个新兴技术领域，也因为它提供了一种更开放的协作方式。

在 Web3 生态中，学习者、开发者、研究者、社区成员和项目方之间的边界相对开放。很多机会不是等别人分配，而是通过主动学习、公开记录、参与社区和完成作品慢慢获得。

我希望通过这次实习计划：

* 理解 Web3 的基本技术逻辑；
* 学习如何参与远程和开源协作；
* 尝试使用 AI 和 Vibecoding 工具进行构建；
* 形成持续记录和输出的习惯；
* 留下可以展示的 `proof of work`。

## 🛠️ Week 1 Build Log

今天开始建立自己的 Week 1 Build Log，用于记录：

* 每日学习内容；
* 阅读链接和参考资料；
* 使用过的 Prompt；
* 实践截图；
* 遇到的错误；
* 修复过程；
* 每天完成的小成果。

我觉得这个记录过程很重要。即使现在还没有做出复杂的项目，也可以通过持续的学习日志展示自己的参与、思考和成长。

## ✅ 今日完成

* [x] 了解 Web3、区块链、以太坊等基础概念
* [x] 阅读 Web3 实习手册入门材料
* [x] 了解 Vibecoding 的基本方向
* [x] 建立 Week 1 Build Log
* [x] 完成 Day 1 学习记录

## 🔗 今日参考

* [Web3 实习手册｜入门导读](https://web3intern.xyz/zh/preface/)
* [Web3 实习手册｜区块链基础概念](https://web3intern.xyz/zh/blockchain-basic/)
* [Web3 实习手册｜以太坊概览](https://web3intern.xyz/zh/overview-of-ethereum/)
* [BuildAnything｜Vibecoding 入门](https://buildanything.so/zh/tracks/freshman/lessons/introduction-to-vibecoding)

---

# 2026-07-07｜DAY 2：工具准备与 Builder 身份｜我如何开始远程协作？

今天的重点是完成 Web3 常用工具准备，并进一步理解 DevRel、Builder 和生态连接者的角色。

今天让我意识到，进入 Web3 不只是注册账号、下载软件或学习几个平台，而是在逐步建立自己的远程协作能力、公开身份和长期 `proof of work`。

## 🛠️ 工具准备

| 工具                                        | 用途                 | 我的理解                                               |
| ----------------------------------------- | ------------------ | -------------------------------------------------- |
| [X / Twitter](https://x.com/)             | 获取行业信息、建立公开身份、参与讨论 | 可以作为记录学习过程和关注 Web3 生态动态的平台。                        |
| [Telegram](https://desktop.telegram.org/) | 项目社群、即时沟通、活动通知     | Web3 社区常用工具，需要特别注意安全和钓鱼信息。                         |
| [Discord](https://discord.com/)           | 开发者社区、项目协作、技术支持    | 适合加入项目社区后阅读规则、观察讨论和参与学习。                           |
| [GitHub](https://github.com/)             | 开源协作、代码和文档管理       | 不只是开发者的作品集，也可以记录学习笔记、文档贡献和项目过程。                    |
| Notion / Google Docs                      | 知识整理、异步协作、会议记录     | 可以帮助我把每天的学习内容沉淀下来。                                 |
| AI Coding 工具                              | 辅助学习、代码理解和项目构建     | 可以使用 Cursor、Claude Code、Codex 或 ChatGPT，但需要自己检查结果。 |

## 🔐 Web3 安全提醒

在 Telegram、Discord 和 X 等平台中，安全意识很重要。

我需要特别注意：

* 不向任何人提供助记词、私钥或验证码；
* 不点击来源不明的链接；
* 不随意连接钱包；
* 不相信主动私信的“官方客服”；
* 加入社群时尽量通过项目官网或官方账号提供的链接；
* 涉及资产或交易操作时，先确认信息是否来自官方渠道。

## 👩‍💻 我理解的 Builder

今天让我重新理解了 Builder 的含义。

Builder 不一定只是写智能合约或开发底层协议的人。广义上，只要是在通过行动、作品和协作推动生态发展的人，都可以是 Builder。

例如：

* 开发者
* 技术写作者
* 社区组织者
* 教育者
* 研究者
* 内容创作者
* 开源贡献者
* 活动协调者
* DevRel 从业者

我现在可能还不是一个成熟的开发者，但可以从小型贡献开始，例如记录学习过程、整理资源、尝试教程、阅读文档、提出问题，或者帮助其他新人理解基础概念。

## 🤝 我理解的 DevRel

DevRel，即 Developer Relations，是连接开发者、产品团队和社区的重要角色。

我理解 DevRel 不只是推广产品，而是帮助开发者真正理解和使用产品。例如：

* 编写开发文档和教程；
* 制作代码示例；
* 组织 Workshop 和 Hackathon；
* 回答开发者问题；
* 收集开发者反馈；
* 帮助产品团队理解真实的使用障碍；
* 建立长期的开发者社区。

我觉得 DevRel 很适合既喜欢技术、又喜欢沟通、教育和社区建设的人。

## 🌐 生态连接者

今天还学习到“生态连接者”这个身份。

生态连接者不一定是技术最强的人，但能够帮助不同的人、资源和机会相互找到。例如：

* 帮助新人找到学习材料；
* 帮助开发者发现项目和协作机会；
* 帮助项目方了解社区真实需求；
* 整理零散信息，降低理解门槛；
* 在不同语言、地区和背景之间发挥桥梁作用。

我觉得自己未来也可以尝试成为这样的人，因为我喜欢整理信息、写学习笔记，也愿意连接不同的人和资源。

## 📚 Co-learning 与 Proof of Work

今天我很喜欢的概念是 `Co-learning`，即共同学习。

在 Web3 中，学习不一定是完全私人的过程。通过公开或半公开地记录自己的学习、尝试、错误和改进过程，可以逐渐形成自己的 `proof of work`。

`Proof of work` 不一定是一个大型项目，也可以包括：

* 每日学习笔记；
* GitHub 提交记录；
* 项目研究笔记；
* 小型教程；
* 工具使用体验；
* 社群答疑；
* Hackathon 项目；
* Demo 或小型应用；
* 文档修订和翻译。

我理解的重点是：让别人看到自己不仅“对 Web3 感兴趣”，而且在持续学习、持续参与和持续产出。

## ✅ 今日完成

* [x] 了解 Web3 远程协作常用工具
* [x] 理解 X、Telegram、Discord、GitHub 的基本用途
* [x] 学习 Web3 社群中的基础安全意识
* [x] 理解 Builder、DevRel 和生态连接者的角色
* [x] 开始思考如何形成自己的 `proof of work`
* [x] 完成 Day 2 学习记录

## 🎯 下一步计划

* [ ] 完善 X、Telegram、Discord 和 GitHub 账号
* [ ] 创建一个 Web3 学习记录仓库，例如 `web3-learning-notes`
* [ ] 建立 Notion 或 Google Docs 学习空间
* [ ] 加入至少 1-2 个 Web3 开发者或学习社群
* [ ] 尝试使用一个 AI Coding 工具
* [ ] 每周完成至少一项可以展示的学习成果

## 🔗 今日参考

* [Web3 实习手册｜Web3 工作方式 / 工具安装指南](https://web3intern.xyz/zh/remote-work-guide/)
* [Web3 实习手册｜岗位全景图](https://web3intern.xyz/zh/position-introduction/)
* [X / Twitter](https://x.com/)
* [Telegram Desktop](https://desktop.telegram.org/)
* [Discord](https://discord.com/)
* [GitHub](https://github.com/)

今天的学习重点，是从“我们想做什么”转向“用户遇到了什么问题”。团队需要结合 Research、Ops、Dev 三个视角，确认问题是否真实、用户是否明确、功能是否能在 3–4 天内完成。

Mini Demo 应只保留一个核心用户动作，并划分 Must Have、Nice to Have 和 Not This Week。同时说明为什么需要链上能力、为什么适合 Monad，并明确尚待验证的假设。

今日收获：好的 Demo 不在于功能多，而在于能否围绕真实问题完成一个可测试、可反馈的最小闭环。
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->



# Week 3 学习笔记｜从个人 Proof of Work 走向团队 Mini Demo

Week 3 的核心，不再是证明“我一个人能够完成什么”，而是学习如何把不同 Builder 的能力组合起来，共同完成一个可以展示、验证和迭代的 Mini Demo。Week 1 帮助我们进入链上世界，Week 2 帮助我们明确 Research、Ops 或 Dev 的个人方向，而 Week 3 则进一步提出了一个更现实的问题：个人能力如何转化为团队生产力？

我认为，本周最重要的启发是：组队并不是寻找与自己最相似的人，而是寻找能够形成互补、共同交付成果的人。Research Builder 负责理解用户、场景、协议与风险；Ops Builder 负责团队节奏、项目叙事、反馈收集与成果传播；Dev Builder 则负责将想法转化为可以运行和测试的技术原型。三种角色并不是彼此独立的，而是围绕同一个用户问题形成连续的协作链条。

Mini Demo 也不等于完整产品。它可以只是一个可运行的小功能、一个合约 Demo、一个交互原型，或者一套研究驱动的产品方案。关键在于团队是否共同定义了具体问题，明确了目标用户，并留下可以检查的协作证据，例如 GitHub Issues、任务看板、代码提交、会议记录、用户反馈和演示材料。相比在群聊中反复讨论宏大的项目愿景，先在一周内共同完成一个最小成果，更能够检验成员之间是否真正适合长期合作。

结合我在 Week 2 完成的 Moss x402 Payment Adapter，我可以将已有的开源贡献作为 Dev Proof of Work，同时发挥自己在研究、项目组织和科学传播方面的经验。我可以帮助团队完成协议研究、产品逻辑设计、任务拆解、文档撰写和 Demo 叙事；与此同时，我需要寻找能够加强前端交互、链上部署、UI 设计或用户测试的互补队友。

Day 1 的 Builder Profile Card、Idea Pitch、Team Formation Card 和 Working Agreement 看似只是组队文档，实际上是在建立团队协作的最小基础设施。一个高质量的团队需要明确成员投入时间、每日同步节奏、任务负责人、决策机制、缺席处理方式以及安全边界。只有先把这些规则说清楚，团队才能减少沟通成本，把有限的一周投入到真正的构建之中。

本周我希望完成的目标是：用一句话明确团队要为谁解决什么问题；找到能够互补的搭子；把问题缩小为一个可验证的 Mini Demo；通过 GitHub 或 Notion 公开记录任务、决策和进度；最终获得真实反馈，并据此判断项目是否值得继续进入 Week 4 Hackathon。

Week 3 所训练的并不只是“组队”，而是一种更加重要的 Builder 能力：在不确定、时间有限和资源不完整的条件下，与他人建立信任、形成分工，并共同交付一个真实成果。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->




今天我围绕 Moss 完成了从“认识开源项目”到“真实参与开源协作”的完整学习闭环：先阅读 README、Docs、Issues 与 Pull Requests，理解 Moss 如何通过 discover、load、action、simulate 支持 AI Agent 安全构建和模拟链上交易；随后以 Dev Builder 为主要身份，结合 Research 与 Ops 能力，提出了关于模拟后用户意图核查的 Issue #97，并通过 Fork、Branch、Documentation、Commit 和 Pull Request #99 完成了一次真实、公开、可验证的开源贡献。

在协作过程中，我进一步理解了 Issue 用于解释“为什么需要改变”，Commit 用于记录“具体改变了什么”，Pull Request 用于请求社区审查，而 Review 则帮助贡献者发现 Markdown 格式、技术边界和表述准确性方面的问题。我也根据反馈继续修改中文文档结构，认识到即使是 Documentation Contribution，也必须理解 Receipt、Outcome、Capability 和交易保护之间的区别。

此外，我还学习了如何把开源贡献转化为可复用的新手教程，并比较了 HackMD、GitHub README、Paragraph、Medium、Substack、DEV 和独立 Blog 的不同定位。最终形成的发布框架是：HackMD 用于起草协作，GitHub README 作为可版本追踪的权威原稿，Paragraph 用于 Web3 社区传播，Medium 或 DEV 用于二次分发。今天最大的收获是：真正的 Proof of Work 不只是最终成果，而是一条能够被公开验证、持续审查和不断迭代的完整过程。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->





在BuildAnything完成初中课程，进一步深化Vibe-coding及Monash Chain开发的基础知识和实践
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->






今天完成了 Week 2 学习成果的系统整理，将研究、运营或开发过程中的任务证据、AI 协作记录与个人判断沉淀为可展示的 Portfolio Pack，并进一步明确了 Week 3 组队中能够承担的角色、可提供的 Proof of Work 以及所需队友。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->







今天完成了从学习到产出的关键跨越：围绕真实产品形成市场判断，将 Space 策划细化为可执行、可复盘的运营方案，并通过 repo、README、截图、交易记录或开源贡献沉淀了可验证的 Proof of Work。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->








今日深入学习 Research、Ops 与 Dev 三类 Builder 的真实工作流，从提案与协议拆解、Space 活动设计到 AI 辅助开发，进一步理解如何将知识转化为可验证、可执行的实际产出。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->









DAY 2 打卡：今天围绕 Research、Ops 与 Dev 三条路径，将职业方向进一步拆解为可执行的 Scope，明确了研究问题与资料边界、活动目标与参与闭环，以及 AI-assisted Web3 MVP 的技术方案与实现边界。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->










Week 2 Day 1 打卡：通过了解 Research、Ops 与 Dev 三条 Web3 Builder 路径，我完成了职业方向选择，并开始用 Role Log 和 AI Collaboration Log 记录学习过程、产出目标与人机协作边界，为本周 Proof of Work 和 Week 3 组队做好准备。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->











Week 1 复盘：全面复习 Week 1 五项任务的完整链路，完成时间打卡，并通关 [https://buildanything.so/zh/tracks](https://buildanything.so/zh/tracks) 新手课程全模块——钱包配置、链上交易、AI 辅助合约开发、高频场景分析与首个 Monad 合约部署，全部完成闭环验证。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->












**This week I bridged theory and practice: mastering wallet mechanics and transaction fundamentals, then executing my first testnet interaction and verifying it independently on-chain—establishing the verification habit that will anchor the rest of this course.**
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->













DAY 5 的学习重点是理解 Monad 如何通过高性能与低成本支持更高频的链上交互体验，并将第一周的钱包、交易、合约部署、AI 协作与安全实践整理为 Build Log，完成 Mini Demo 0，同时明确 Week 2 的 Tech / Ops / Research 主攻方向。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->














DAY 4：借助 AI 生成并理解最小 Solidity 智能合约，在 Remix 完成编译、部署与交互，掌握合约源码、ABI、合约地址、读写函数和交易哈希之间的关系，迈出从链上交易到智能合约开发实践的第一步。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->















DAY 3｜已完成课程专用钱包创建、Monad Testnet 添加、测试币领取与第一笔测试网交易，并通过区块浏览器查看 transaction hash，理解了钱包安全、交易、gas 与链上记录的基本流程。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
















完成 Web3 常用工具准备，了解了 X、Telegram、Discord、GitHub、Notion 和 AI Coding 工具的用途，并开始理解 Builder、DevRel 和 Proof of Work 的意义。
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
