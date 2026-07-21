---
timezone: UTC+8
---

# Leon

**GitHub ID:** Foreon

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
Day16  
从我的角色检查项目：Dev Builder

我的主方向：Dev

本周最需要完成的核心功能是：

完成一个最小真实链上交互闭环，让「星核档案馆」不再只是前端模拟抽卡，而是能在 Monad 测试网上完成一次真实交易。

核心流程是：

玩家连接钱包  
→ 前端读取当前卡池信息  
→ 玩家点击抽卡或远征  
→ 钱包发起交易  
→ 合约记录请求与结果  
→ 合约发出事件  
→ 前端监听事件并展示结果  
→ 把结果加入本地记录和排行榜展示

这个功能不要求一次做完整游戏，但必须证明：用户的关键行为可以被链上记录，前端可以读取链上结果，项目具备继续扩展成链游的基础。

可以复用的现有工具、合约、SDK 或代码：

1.  现有前端代码  
    目前已经完成像素风页面、多卡池切换、抽卡按钮、概率展示、保底进度、模拟交易记录和角色卡展示。Week 2 可以直接在这个基础上接入钱包和合约，不需要重新做 UI。
    
2.  Solidity 合约  
    可以先写一个简化版 Gacha 或 Expedition 合约，包含卡池概率版本、玩家抽卡次数、保底计数、事件日志等字段。第一版不追求复杂经济模型，只做最小可验证流程。
    
3.  EVM 兼容工具  
    Monad 兼容 EVM，因此可以复用常见以太坊开发工具，例如 Solidity、Hardhat 或 Foundry、viem / wagmi、MetaMask 钱包连接方式。这样可以降低开发成本，也方便后续迁移和调试。
    
4.  前端事件监听  
    可以使用 viem 或 ethers 监听合约事件，把 DrawRequested、DrawSettled 或 ExpeditionCompleted 这类事件展示到页面里，形成“链上发生了什么，前端立刻反馈什么”的体验。
    

暂时无法完成、需要简化或使用 Mock 的部分：

1.  真随机数暂时 Mock  
    完整的可验证随机数需要 VRF 或其他随机数方案。Mini Demo 阶段可以先用合约内的简化随机逻辑或前端 Mock，但必须在界面和 README 中明确说明这不是最终公平随机方案。
    
2.  完整排行榜暂时 Mock  
    真实排行榜最好来自链上事件索引器，例如后续使用 Envio、The Graph 类工具，或自建轻量索引服务。本周可以先用前端本地数据或事件数组模拟排行榜，只验证展示逻辑。
    
3.  NFT 资产暂时不做  
    角色暂时不需要立刻铸造成 NFT。第一阶段只记录玩家抽到的角色 ID 和稀有度，等链上流程稳定后再考虑 ERC-721 或 ERC-1155。
    
4.  复杂经济系统暂时不做  
    代币、市场交易、赛季奖励、付费抽卡等功能先不接入。本周目标是验证“链上交互闭环”，不是做完整商业系统。
    

简单技术方案：

前端页面  
连接钱包  
调用 Monad 测试网合约  
发送 requestDraw 或 startExpedition 交易  
合约写入玩家状态  
合约发出事件  
前端读取事件结果  
更新抽卡记录、角色展示、排行榜 Mock 数据

真实开发部分：

1.  钱包连接  
    实现用户连接钱包，并显示钱包地址和当前网络。
    
2.  合约部署  
    在 Monad 测试网上部署一个最小 Gacha / Expedition 合约。
    
3.  链上交易  
    让用户至少能完成一次真实的抽卡或远征交易。
    
4.  链上事件读取  
    前端读取合约事件，并把交易哈希、区块号、角色结果或积分变化展示出来。
    
5.  状态边界说明  
    在 README 中明确哪些数据来自链上，哪些只是前端展示或 Mock。
    

Mock 部分：

1.  随机数结果  
    第一版可以简化随机数，不把它包装成正式公平随机。
    
2.  排行榜  
    先用本地事件结果生成排行榜，不接真实索引服务。
    
3.  角色资产  
    暂时只用前端像素图片展示，不做 NFT 铸造。
    
4.  奖励结算  
    暂时只展示积分变化，不接真实奖励或代币。
    

待验证的问题：

这个项目的核心技术问题不是“页面能不能做出来”，而是：用户是否愿意为了抽卡后的远征、积分和排行榜，频繁发起链上交易。如果 Monad 的低延迟和低成本体验足够顺滑，那么这个方向有机会从普通抽卡 Demo 变成真正适合链上的高频小游戏。
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->

# Day15

我的方向：Dev

我会什么：  
React 前端与交互原型开发；像素风游戏 UI、抽卡和任务系统页面；基础 DApp 流程设计，包括钱包连接、合约事件展示、链上/链下数据边界设计。目前正在推进 Solidity、Monad 测试网交互和排行榜索引。

我想做什么：  
想做 Monad 上的高频链游或任务型 DApp。当前方向是“像素角色抽卡 + 链上远征 + 短周期排行榜”：角色不只是收藏，而是能参与探索、抢点、任务和赛季奖励结算。

我能为团队做什么：  
负责前端 Demo、交互流程、游戏化 UI、钱包交互接入、合约事件结果展示，以及把产品想法快速做成可演示原型。也可以协助梳理链上可信结算、链下实时展示的架构。

我希望寻找的队友：  
希望找会 Solidity、智能合约安全、Monad 测试网部署的开发者；也欢迎擅长游戏策划、社区增长、Meme 传播或视觉设计的同学一起组队。

本周可以投入的时间：  
每天约 3～5 小时，可集中完成一个可演示的链上交互 MVP。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->


\# GitHub 开源项目阅读记录：Hardhat

\> 阅读日期：2026-07-19

\> 主项目：\[NomicFoundation/hardhat\]([https://github.com/NomicFoundation/hardhat](https://github.com/NomicFoundation/hardhat))

\> 对照项目：\[nishuzumi/moss\]([https://github.com/nishuzumi/moss)、\[argotorg/solidity\](https://github.com/argotorg/solidity)](https://github.com/nishuzumi/moss\)、[argotorg/solidity]\(https://github.com/argotorg/solidity\))

\## 1. 为什么选择 Hardhat

Hardhat 是一个以太坊智能合约开发环境，覆盖编译、测试、部署和调试等流程。相比另外两个参考项目，它同时具备以下特点：

\- 项目规模较大，采用 pnpm workspace 管理多个核心包和插件。

\- README、用户文档、贡献指南和工程规范的分工清楚。

\- Issues、Pull Requests 和 Discussions 都比较活跃，适合观察维护者如何管理需求、实现、测试和发布。

\- 同时维护核心框架、插件、模板和真实项目端到端测试，能看到成熟项目如何控制改动影响范围。

\## 2. README 阅读

\[README\]([https://github.com/NomicFoundation/hardhat#readme](https://github.com/NomicFoundation/hardhat#readme)) 很短，主要承担“项目入口”职责，而不是把全部细节堆在首页：

\- 一句话说明产品定位：以太坊软件开发环境。

\- 明确当前 README 对应 Hardhat 3，旧版 Hardhat 2 使用单独分支。

\- 给出最短启动命令`npx hardhat --init`。

\- 把深入学习引导到\[官方文档\]([https://hardhat.org/docs/)。](https://hardhat.org/docs/\)。)

\- 把参与开发引导到 \[[CONTRIBUTING.md](http://CONTRIBUTING.md)\]([https://github.com/NomicFoundation/hardhat/blob/main/CONTRIBUTING.md)。](https://github.com/NomicFoundation/hardhat/blob/main/CONTRIBUTING.md\)。)

我的理解是，成熟项目的 README 更像导航页。用户操作、内部实现和贡献流程分别交给 Docs、工程文档和 CONTRIBUTING 管理，避免同一信息在多个地方重复维护。

\## 3. 项目目录结构

Hardhat 是一个 TypeScript monorepo，核心目录可以简化为：

\`\`\`text

hardhat/

├── .changeset/ # 每个 PR 的版本变更和发布说明

├── .github/

│ ├── ISSUE\_TEMPLATE/ # Bug、功能请求等结构化模板

│ └── workflows/ # CI、发布、文档、回归测试等自动化

├── docs/ # 面向贡献者的工程规范

├── e2e/ # 仓库级端到端测试及 fixture

├── end-to-end/ # Aave、ENS、OpenZeppelin、Uniswap 等真实项目回归

├── packages/

│ ├── hardhat/ # 核心框架

│ ├── hardhat-errors/ # 统一错误定义

│ ├── hardhat-mocha/ # Mocha 测试插件

│ ├── hardhat-ethers/ # Ethers 集成

│ ├── hardhat-viem/ # Viem 集成

│ ├── hardhat-verify/ # 合约验证插件

│ └── ... # 其他插件、工具和模板

├── scripts/ # benchmark、发布、e2e 等维护脚本

├── package.json # 仓库级命令

├── pnpm-workspace.yaml # workspace 和依赖安装策略

└── [CONTRIBUTING.md](http://CONTRIBUTING.md) # 贡献流程、分支和质量要求

\`\`\`

目录设计体现了几个边界：

1\. 核心能力放在 `packages/hardhat`，可选能力尽量做成插件。

2\. 单元/集成测试靠近对应 package，跨包流程放到 `e2e`。

3\. `end-to-end` 使用真实生态项目做回归，比只跑人工构造的小样例更能发现兼容性问题。

4\. 发布信息放入 `.changeset`，使代码改动和版本说明一起审查。

\## 4. Docs 与工程规范

用户文档位于 \[hardhat.org/docs\](https://hardhat.org/docs/)，从安装和项目初始化开始，逐步介绍配置、编译、测试、部署、网络和插件。它回答的是“使用者怎么用”。

仓库内的 \[[engineering-guidelines.md](http://engineering-guidelines.md)\]([https://github.com/NomicFoundation/hardhat/blob/main/docs/engineering-guidelines.md](https://github.com/NomicFoundation/hardhat/blob/main/docs/engineering-guidelines.md)) 回答“维护者希望代码怎么写”，其中比较重要的规则有：

\- `packages/hardhat/src/core` 只保留最小基础设施，不放应用逻辑。

\- 插件入口文件只描述插件和类型扩展，业务实现延迟加载，避免拖慢启动。

\- `src/internal` 之外的内容视为公共 API，修改时必须考虑兼容性。

\- 测试应优先验证错误类型或错误码，而不是脆弱的完整错误文本。

\- 尽量减少依赖，以降低安装体积和供应链风险。

\- 优先写集成测试，只有确有必要时才使用复杂 fixture 或 monkey patch。

\[[CONTRIBUTING.md](http://CONTRIBUTING.md)\]([https://github.com/NomicFoundation/hardhat/blob/main/CONTRIBUTING.md](https://github.com/NomicFoundation/hardhat/blob/main/CONTRIBUTING.md)) 则规定了实际协作流程：较大的修改先在 Issue 中讨论，开发前完成环境安装，提交前运行 build、lint、format 和 test；技术功能需要同步文档`main` 对应可发布版本，高风险开发可进入 `development`，发布前还要经过完整 QA。

\## 5. Issues 如何管理需求

\[Issues 页面\]([https://github.com/NomicFoundation/hardhat/issues](https://github.com/NomicFoundation/hardhat/issues)) 中能看到 Bug、功能请求、依赖工作和大型 Epic。维护方式包括：

\- Issue 模板要求版本、现象、最小复现和检索关键词，减少维护者来回追问信息。

\- 使用 `prioritystatusdependency` 等标签表达优先级、状态和外部阻塞。

\- 大功能先定义跨模块行为和产品语义，再分配负责人实现。

\- 提问和一般排障被引导到文档或 Discussions，Issue 更偏向可执行工作。

本次阅读时，仓库没有简单地给大量问题打 `good first issue` 标签。贡献指南更强调从维护者维护的项目看板中寻找合适任务。这说明“容易做”并不是唯一标准，任务是否已经澄清、是否符合路线图也很重要。

\## 6. Pull Requests 如何控制质量

\[Pull Requests 页面\]([https://github.com/NomicFoundation/hardhat/pulls](https://github.com/NomicFoundation/hardhat/pulls)) 展示了几类典型修改：功能实现、Bug 修复、性能 benchmark、依赖升级，以及由 Changesets 机器人生成的版本发布 PR。

从 \[PR 模板\]([https://github.com/NomicFoundation/hardhat/blob/main/.github/pull\_request\_template.md](https://github.com/NomicFoundation/hardhat/blob/main/.github/pull_request_template.md)) 和 CI 配置可以看出：

\- Bug 修复必须附带测试；新功能应先经过讨论。

\- PR 需要 changeset，机器人会计算受影响 package 和版本级别。

\- 自动检查不只有编译和测试，还包括 lint、包体积、依赖数量、文档链接、拼写、端到端回归等。

\- monorepo 测试覆盖多个 Node.js 版本、操作系统和 CPU 架构，避免只在作者电脑上成立。

\- 自动发布 PR 把多个 changeset 聚合成版本升级，降低手工发布遗漏。

一个有意思的细节是，CI 会在 PR 中报告安装包大小和传递依赖数量。维护者因此能在代码审查时同时看到功能收益与分发成本。

\## 7. Discussions 的作用

\[Discussions\]([https://github.com/NomicFoundation/hardhat/discussions](https://github.com/NomicFoundation/hardhat/discussions)) 使用 `IdeasTroubleshootingGeneral` 等分类。它适合：

\- 用户提问和排障。

\- 尚未形成明确方案的产品想法。

\- 比较多种实现方式，先讨论兼容性和用户体验。

\- 标记已采纳答案，沉淀可被搜索的社区知识。

Issue 和 Discussion 的差别是：Discussion 可以从开放问题开始，而 Issue 最好已经接近“可以被实现和验收的工作项”。例如 `getSigners` behavior can be error-prone\]([https://github.com/NomicFoundation/hardhat/discussions/2029](https://github.com/NomicFoundation/hardhat/discussions/2029)) 先讨论 API 是否容易误用，以及不同修复方案的破坏性；这类问题不适合一开始就直接提交代码。

\## 8. 感兴趣的功能：排除指定名称的测试

我选择跟踪 \[Issue #8184：Support negation of test name filters\]([https://github.com/NomicFoundation/hardhat/issues/8184)。Hardhat](https://github.com/NomicFoundation/hardhat/issues/8184\)。Hardhat) 原有 `--grep` 可以只运行匹配名称的测试，但不能表达“运行除这些名称之外的所有测试”。简单使用正则负向前瞻也不可行，因为底层 Rust 正则实现不支持 look-around。

\### Issue 阶段

Issue 没有急着确定某个代码实现，而是先规定统一行为：

\- 新增 `--grep-exclude`。

\- 可以和 `--grep` 同时使用，语义是“包含集合减去排除集合”。

\- Mocha`node:test` 和 Solidity Test 三种运行器应提供一致体验。

社区成员提出了增加独立参数、在 Node 侧反向过滤、替换 Rust 正则库等方案。维护者随后明确：暂时不扩展到路径和合约名排除，并让 EDR 依赖侧实现 Solidity Test 的排除能力。Issue 最终被标记为 `dependency:edr` 并分配负责人。

\### PR 阶段

\[PR #8442\]([https://github.com/NomicFoundation/hardhat/pull/8442](https://github.com/NomicFoundation/hardhat/pull/8442)) 实现该需求，并链接原 Issue、设计文档和单独的网站文档 PR。以阅读时状态为准，它仍处于开放状态。

该 PR 的改动横跨：

\- 核心 `test` 任务的参数注册和转发。

\- Solidity Test 到 EDR 的参数映射。

\- Mocha 中包含与排除表达式的合并逻辑。

\- `node:test` 的 skip pattern 支持。

\- 错误码、fixture、单元测试和集成测试。

\- `.changeset` 发布记录和网站文档。

PR 页面显示 32 个文件发生变化。总增删行数很大，但其中大部分来自 `pnpm-lock.yaml`；因此审查时不能只看 GitHub 的总行数，还要先把生成文件与手写逻辑分开。最新提交触发了大量跨平台、跨 Node 版本的 package 级 CI 检查，核心 `cilint` 和 `bundle` 检查均通过。

\### 我会怎样继续参与

这个 PR 已经有负责人，不适合重复实现。我更有价值的参与方式是：

1\. 拉取 PR，在 Linux、Windows 或不同 Node 版本复现组合行为。

2\. 补充边界测试，例如空字符串、同时包含与排除、特殊字符和并行 Mocha。

3\. 核对三种测试运行器是否真正保持相同 CLI 语义和错误信息。

4\. 阅读配套文档 PR，检查示例是否覆盖常见迁移场景。

\## 9. 三个参考项目的组织方式对比

| 项目 | 技术与规模 | 文档/协作特点 | 适合学习的重点 |

| --- | --- | --- | --- |

| \[moss\]([https://github.com/nishuzumi/moss](https://github.com/nishuzumi/moss)) | Node.js/TypeScript、pnpm monorepo，较新的 Monad 工具项目 | README、快速开始、ADR、Changesets 较完整；当前未启用 GitHub Discussions | 新项目如何尽早建立 package 边界、架构决策记录和安全声明 |

| \[solidity\]([https://github.com/argotorg/solidity](https://github.com/argotorg/solidity)) | C++ 编译器，历史长、构建和测试体系复杂 | 有 CodingStyle、ReviewChecklist、ReleaseChecklist、Security；主要通过论坛和聊天社区讨论 | 编译器项目的严格评审、发布、安全响应和长期兼容性 |

| \[hardhat\]([https://github.com/NomicFoundation/hardhat](https://github.com/NomicFoundation/hardhat)) | TypeScript 大型 monorepo，核心加插件生态 | 用户文档、贡献规范、工程指南、Issue/PR/Discussion 分工清楚，自动化程度高 | 多 package 协作、插件边界、CI 矩阵、changeset 发布和真实项目回归 |

\## 10. 我的发现

1\. 阅读真实项目不能只读源码。README 告诉我入口，Docs 告诉我产品模型，CONTRIBUTING 和工程规范告诉我维护者的判断标准，Issue/PR 才展示这些规则如何真正执行。

2\. 目录结构是在表达所有权。核心、插件、测试、真实项目回归和发布脚本被分开后，改动影响范围更容易判断。

3\. 高质量 Issue 的关键是可验收语义，而不只是描述“缺一个功能”。#8184 先规定三种测试运行器的一致行为，PR 才有明确边界。

4\. 成熟 PR 是一个完整交付单元：代码、测试、文档、依赖更新、版本说明缺一不可。

5\. 自动化是 Maintainer 的管理工具。Issue 模板、标签、Changesets、CI 矩阵和发布机器人把隐性的经验变成了可重复执行的规则。

6\. 找贡献点时应先确认任务是否有人负责、是否需要设计讨论、是否依赖其他仓库。直接写代码可能制造重复劳动。

\## 11. 推荐的真实项目阅读顺序

以后阅读其他开源项目时，我会按以下顺序进行：

1\. 读 README，回答“它解决什么问题、如何启动、文档在哪里”。

2\. 看根目录和依赖清单，识别语言、构建工具、单仓库或 monorepo。

3\. 读 CONTRIBUTING、SECURITY、CODEOWNERS 和工程规范，理解协作规则。

4\. 从一个核心 package 沿着入口、实现、测试追踪一条调用链。

5\. 在 Issues 中选一个有明确上下文的问题，记录标签、负责人、依赖和验收条件。

6\. 找到关联 PR，检查代码、测试、review、CI、文档和 release note。

7\. 浏览 Discussions，理解哪些问题仍在探索，哪些已经成为可执行 Issue。

8\. 最后才决定贡献方式：复现、补测试、改文档、提交设计意见或实现代码。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->



# Day13

\# Moss 开源贡献计划：Ops Builder

\## 我的 Builder 方向

**Ops Builder：完善新人入门文档与 FAQ。**

\## 本周准备完成的任务

提交一个聚焦的文档 PR：补充或改进“第一次运行 Moss”相关内容，覆盖环境要求`wrap` 示例、常见 RPC/构建问题，以及 Agent 必须遵守的安全规则。

计划中的产物：

\- 一份可按步骤执行的新手教程或对现有教程的增量改进；

\- 3～5 条经过实际复现或官方文档核对的 FAQ；

\- 一个最小验证记录：示例命令、预期结果与本地检查结果。

若仓库已有同类文档，将优先补足缺失内容或修正过期信息，不重复新增一篇内容相同的教程。

\## 为什么选择这个方向

我已经完成 Moss 的中文入门笔记与教程草稿，擅长将架构、安全边界和命令步骤整理为新人能够复用的内容。相比直接开发 Protocol Adapter，文档贡献的依赖面更小，也更符合当前能力边界：我可以先通过实际运行验证体验，再提交准确、可审阅的改进。

这也与 Moss 的贡献指南一致：项目将“可运行教程、示例与文档”列为明确欢迎的贡献类型。对一个需要 Agent 遵守仿真和意图对齐规则的项目来说，清晰文档本身就是降低误用风险的产品能力。

\## 本周计划

| 时间 | 任务 | 产出 / 验收标准 |

| --- | --- | --- |

| Day 1 | 阅读 `AGENTS.mdCONTEXT.md`、相关 ADR 与现有入门文档；建立内容差异清单。 | 确认不重复现有文档，并列出 3 个新人最可能卡住的问题。 |

| Day 2 | 在本地完成 `pnpm installpnpm buildwrap` 示例或离线测试。 | 记录实际命令、关键输出、环境版本和异常。 |

| Day 3 | 编写教程增量与 FAQ 初稿。 | 每条命令、RPC 限制、warning 规则均有官方资料或实测依据。 |

| Day 4 | 自审：核对安全表述、链接与术语；按贡献规范执行 `pnpm lintpnpm buildpnpm typecheckpnpm test:offline`。 | 文档、示例和验证记录一致；无误导性“保证安全/收益”表述。 |

| Day 5 | 从 `main` 创建分支，提交 PR。 | PR 说明动机、改动范围、未改动的边界与验证证据；根据 review 修订。 |

\## PR 范围与边界

**会做：** 新手环境说明、首个可模拟示例、错误排查、Agent 的“必须 simulate / warning 即停止 / 意图对齐”提示。

**不会做：** 声称项目已审计或可安全用于生产资金；提供私钥/签名自动化；未经验证地补充协议地址、代币信息或 Adapter 行为；将文档 PR 扩展为核心框架重构。

\## 完成定义

本周完成不只是写好 Markdown，而是满足以下条件：

1\. 新手可从文档中找到一条零私钥、零资金的最小体验路径；

2\. 所有命令和安全结论已由官方文档或实际运行核对；

3\. 本地质量检查通过，或在 PR 中透明记录外部 RPC 导致的限制；

4\. 已发起一个范围清晰、含验证证据的 Pull Request。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->




# Day12

为什么 AI Agent 需要 Moss？

当用户对 AI Agent 说“把 1 MON 换成 USDC”，背后并不只是一次合约调用：路由地址、代币精度、滑点、授权、原生币包装和多笔交易顺序，任何一项出错都可能造成资金损失。

[Moss](https://github.com/nishuzumi/moss) 是一个面向 Monad 的 AI Agent × Web3 开源框架。它把复杂的 DApp 交互封装成统一的能力，让 Agent 不必手写 ABI、calldata 或合约地址，而是按固定流程工作：

```Plain
discover → load → action → simulate
```

-   `discover`：按 `swap`、`supply` 等用户意图寻找能力；
    
-   `load`：读取参数语义、风险标签与调用规则；
    
-   `action`：生成未签名的 Plan；
    
-   `simulate`：在实时链状态下模拟，并核对实际效果。
    

Plan 会明确声明：最多能转出什么资产、至少应收到什么资产、允许给谁多少授权。模拟结果若发现未声明的资产流出、超额授权、交易回滚或 Plan 被篡改，就会产生 warning；**有 warning 就必须停止，不能交给用户签名。**

我认为 Moss 最重要的设计，是将责任分开：Moss 负责验证“实际发生的效果是否符合 Plan”；Agent 负责验证“这个 Plan 是否符合用户的原始意图”；钱包与用户保留最终签名权。Moss 不持有私钥，也不签名或广播交易。

这让它适合用于自然语言钱包助手、跨协议 DeFi 工作流、DAO 的受限执行流程，以及无真实资金的开发者教学沙盒。

Moss 目前仍处于 Alpha 阶段，模拟不能保证最终成交结果，用户仍应在钱包中审阅交易。想开始体验，只需准备 Node.js 22+ 与 pnpm，运行项目提供的 `wrap` 示例即可完整走一遍“发现、构建、模拟”的流程。

对开发者而言，最有价值的贡献包括：接入新协议、补充能力与查询、完善文档示例，或改进核心验证逻辑。协议适配器是 Moss 生态的核心：一个协议一个包，并为能力提供量化预期和零 warning 的模拟测试。

Moss 的意义不在于让 Agent 更大胆地替用户操作资产，而在于让它更克制地帮助用户完成一笔可解释、可验证、最终由用户决定的链上交易。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->





# Day11

AI 协作记录

AI 帮助完成的内容：

AI 协助我完成了像素风抽卡 Demo 的前端结构，包括卡池切换、抽卡交互、保底进度、概率展示、模拟账本记录和响应式页面样式。

AI 协助生成了三组像素角色立绘素材，并将其整理为卡牌可调用的图片资源。

AI 协助梳理了 Monad 的高频交互产品方向，提出“抽卡角色进入链上远征、任务与短周期排行榜”的后续路线，并协助整理 Week 1、Week 2 的提交文档。

人类做出的判断、删改与核查：

我决定不把当前 Demo 称为真实链上产品。页面中的随机数、区块号、交易哈希和 Chainlink VRF 仅用于展示交互流程，目前都不是实际 Monad 链上数据。

我手动确定了产品方向为“公开概率抽卡 + 链上远征与冲榜”，并选择了三套卡组的主题、角色名称、稀有度、保底规则、页面文案和后续技术路线。

我核查并修改了 AI 生成内容，避免出现“已经部署合约”“已经完成 VRF 验证”“数据已上链”等不真实表述。

我保留了“链上可信结算、链下实时展示”的架构判断：影响公平性和奖励的状态应上链，动画、素材和缓存排行榜不必全部上链。

不能交给 AI 的内容：

不能把私钥、助记词、钱包签名、API Key、.env 文件或任何敏感账户信息交给 AI。

不能让 AI 替代真实的交易确认、合约部署确认、区块浏览器核验和测试网结果验证。

不能直接相信 AI 生成的合约代码。涉及资金、奖励、随机数、权限控制和资产转移的合约，需要人工测试、审查，必要时进行第三方安全审计。

不能把 AI 的产品建议直接当作用户需求或市场结论。是否真的需要链上、用户是否愿意频繁交互、奖励机制是否可持续，仍需要通过真实 Demo、用户反馈和链上数据验证。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->






# Day10

# Moss 新手教程：从零跑通一次“可验证”的链上操作

> 目标：约 15 分钟跑通 Moss 的 `discover → load → action → simulate` 流程，理解它如何在签名前验证交易效果。本文使用模拟，不需要私钥和真实资金。

## 你会学到什么

-   Moss 如何把链上协议封装为 Agent 可调用能力；
    
-   如何运行第一个 `wrap` 示例；
    
-   Plan、仿真结果和 warning 分别代表什么；
    
-   如何将 Moss 接入 MCP 客户端，以及下一步如何参与开源贡献。
    

## 0\. 先理解 Moss 的边界

Moss 面向 Monad，将协议交互统一为 `discover → load → action → simulate` 四个步骤。它会构建未签名交易并模拟验证，但**不会持有私钥、签名或发送交易**；最终决定权仍在用户和钱包手中。

项目目前为 Alpha，模拟反映的是模拟时的链状态，并不保证签名后的最终成交结果。因此，请把它视为交易前的安全检查，而不是执行结果承诺。

## 1\. 准备环境

需要：

-   Node.js 22 或更高版本；
    
-   pnpm。
    

克隆、安装并构建项目：

```Bash
git clone https://github.com/nishuzumi/moss
cd moss
pnpm install
pnpm build
```

如果只想先确认本地工具链，可运行离线测试：

```Bash
MOSS_SKIP_E2E=1 pnpm test
```

## 2\. 跑通第一个示例

执行：

```Bash
pnpm --filter @themoss/example-simple-flow wrap
```

该示例的意图是将 1.5 MON 包装成 WMON。它会依次完成：

1.  `discover`：找到提供 `wrap` 能力的协议；
    
2.  `load`：读取参数含义、意图模板和风险标签；
    
3.  `action`：构建 Plan，即完整编码但未签名的交易与预期效果；
    
4.  `simulate`：在实时 Monad 链状态上回放交易并进行效果对账。
    

成功时，输出应显示 **No warnings**。这表示模拟得到的资产变化、授权等效果没有超出 Plan 的声明，未签名交易才可以交给钱包审阅。

## 3\. 看懂 Plan 与仿真

Plan 不是单纯的 calldata。它包含：

-   `txs`：未签名的完整交易；
    
-   `expects`：最多允许转出什么、至少应收到什么、能授予谁多少额度；
    
-   `intent` 与 `declaredRisk`：交易意图和风险标签；
    
-   `planHash`：用于发现 Plan 是否在生成后被修改。
    

`simulate` 会提取实际的资产流、授权、接收者和 NFT 变化，再与 `expects` 对比。只要出现未声明的流出、超额授权、回滚、最低收入不满足或 Plan 被篡改等 warning，就应该停止，不能签名。

> 零 warning 不等于“用户一定想做这件事”。Agent 仍要检查模拟效果是否符合用户原话：付出的资产、收到的资产、操作类型和授权对象都要一致。这一步叫作意图对齐（intent alignment）。

## 4\. 接入自己的 AI Agent（MCP）

构建完成后，在 MCP 客户端配置中加入 Moss（将路径替换为你本地的克隆目录）：

```JSON
{
  "mcpServers": {
    "moss": {
      "command": "node",
      "args": ["<path-to-moss>/packages/mcp-server/dist/cli.js"],
      "env": { "MOSS_RPC_URL": "https://rpc.monad.xyz" }
    }
  }
}
```

Agent 将获得四个工具：`discover`、`load`、`action`、`simulate`。使用时务必遵循这三条规则：

1.  不直接拼合约调用，先 discover 和 load；
    
2.  每一个 Plan 都必须先 simulate；
    
3.  任意 warning 都停止；零 warning 后再做意图对齐，并让用户在钱包中最终确认。
    

如果模拟失败，先检查 RPC 是否支持 `debug_traceCall`；Moss 的默认 Monad RPC 支持这一要求，部分第三方免费 RPC 会屏蔽该调试接口。

## 5\. 下一步：贡献你的第一个改进

不必一开始就接入整个协议。更适合新手的贡献顺序是：

1.  跑通示例，为文档或 FAQ 补充一个踩坑说明；
    
2.  修正示例、测试或中文说明；
    
3.  阅读 `packages/protocols/_template`，尝试为协议增加一个查询或能力。
    

新增协议适配器时，Moss 要求一个协议一个包，并为能力写清语义化参数、风险标签、量化的 `expects` 和模拟测试。提交 PR 前可执行：

```Bash
pnpm build
pnpm typecheck
pnpm lint
pnpm test
```

## 小结

Moss 的重点不是让 Agent 更自由地调用合约，而是让链上操作变成可发现、可解释、可模拟、可验证的能力。先跑通 `wrap`，再观察 Plan 和仿真结果，是理解这个项目最快也最安全的起点。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->







# Day9  
为什么 AI Agent 需要 Moss？

当用户对 AI Agent 说“把 1 MON 换成 USDC”，背后并不只是一次合约调用：路由地址、代币精度、滑点、授权、原生币包装和多笔交易顺序，任何一项出错都可能造成资金损失。

\[Moss\]([https://github.com/nishuzumi/moss](https://github.com/nishuzumi/moss)) 是一个面向 Monad 的 AI Agent × Web3 开源框架。它把复杂的 DApp 交互封装成统一的能力，让 Agent 不必手写 ABI、calldata 或合约地址，而是按固定流程工作：

discover → load → action → simulate

\- `discover`：按 `swapsupply` 等用户意图寻找能力；

\- `load`：读取参数语义、风险标签与调用规则；

\- `action`：生成未签名的 Plan；

\- `simulate`：在实时链状态下模拟，并核对实际效果。

Plan 会明确声明：最多能转出什么资产、至少应收到什么资产、允许给谁多少授权。模拟结果若发现未声明的资产流出、超额授权、交易回滚或 Plan 被篡改，就会产生 warning；\*\*有 warning 就必须停止，不能交给用户签名。\*\*

我认为 Moss 最重要的设计，是将责任分开：Moss 负责验证“实际发生的效果是否符合 Plan”；Agent 负责验证“这个 Plan 是否符合用户的原始意图”；钱包与用户保留最终签名权。Moss 不持有私钥，也不签名或广播交易。

这让它适合用于自然语言钱包助手、跨协议 DeFi 工作流、DAO 的受限执行流程，以及无真实资金的开发者教学沙盒。

Moss 目前仍处于 Alpha 阶段，模拟不能保证最终成交结果，用户仍应在钱包中审阅交易。想开始体验，只需准备 Node.js 22+ 与 pnpm，运行项目提供的 `wrap` 示例即可完整走一遍“发现、构建、模拟”的流程。

对开发者而言，最有价值的贡献包括：接入新协议、补充能力与查询、完善文档示例，或改进核心验证逻辑。协议适配器是 Moss 生态的核心：一个协议一个包，并为能力提供量化预期和零 warning 的模拟测试。

Moss 的意义不在于让 Agent 更大胆地替用户操作资产，而在于让它更克制地帮助用户完成一笔可解释、可验证、最终由用户决定的链上交易。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->








## 认识一个开源项目：Moss

Moss 是一个面向 AI Agent 的链上交互框架。它把不同 DApp / 协议原本各不相同的合约调用，统一封装为可调用的 **capability（能力）**。Agent 不再直接拼接 ABI、合约地址、calldata、代币精度、multicall 和清理步骤，而是遵循固定流程：

`discover → load → action → simulate`

-   `discover`：按用户意图（如 `swap`、`supply`、`transfer`）寻找可用能力；
    
-   `load`：读取该能力的参数语义、意图模板和风险标签；
    
-   `action`：生成一个不含私钥、未签名的 `Plan`；
    
-   `simulate`：在实时链上状态下回放交易，核对实际资产效果是否符合 Plan 的声明。
    

Moss 只构建并验证交易，**永不持有私钥、永不签名、永不广播交易**；最终签署权始终在用户钱包中。

## 核心问题

对人而言“用 1 MON 换 USDC”很简单；对 Agent 来说，背后却可能涉及：选择正确路由合约、区分 exact-in / exact-out、处理原生币包装与解包、进行精度与滑点计算、增加授权、拼装 multicall、处理退款或 sweep。只要其中一环“差一点”，就可能产生错误交易甚至资金损失。

Moss 要解决的不是让大模型临时学会更多合约细节，而是把这些高风险、协议特定的细节移到经过维护的适配器中，并在签名之前增加可机械执行的验证关卡。

## 核心能力

### 1\. 用“用户意图”而不是合约函数组织能力

Moss 使用 `swap`、`wrap`、`supply`、`claim` 等用户视角的动词；例如 WMON 合约的 `deposit()` 对 Agent 暴露为 `wrap`。这样 Agent 可先理解用户要做什么，再发现匹配协议，而不必从函数名和 ABI 中猜测语义。

### 2\. Plan：可审阅、可传递的未签名交易合同

`action` 的产物是 Plan，其中包含：

-   完整编码但未签名的交易列表；
    
-   `expects`：允许转出资产的上限、必须收到资产的下限、允许授予的 token / spender / 授权上限；
    
-   `intent`、风险标签和协议收据要求；
    
-   `planHash`，用于发现 Plan 在生成后是否被篡改。
    

这使“交易会做什么”成为显式、量化的数据，而不是隐藏在 calldata 中的推测。

### 3\. 仿真与效果对账

Moss 通过 `debug_traceCall` 在当前链状态模拟 Plan，提取真实发生的资产流、授权、NFT 操作、接收者、原生 MON 转移，以及 WMON 的铸造/销毁等不一定带有 `Transfer` 事件的效果。随后把结果与 `expects` 对账：

-   有未声明的资产流出、授权或 NFT 操作；
    
-   流出或授权超过预设上限；
    
-   收入未达到最低要求；
    
-   Plan 被篡改、交易回滚或应有的协议收据缺失；
    

都会产生 warning。**任何 warning 都必须停止，不能把交易交给签名者。**

### 4\. 多步骤组合

`simulate` 可按顺序模拟多个 Plan，并把前一步的模拟状态传给后一步。因此可验证“领取奖励 → 兑换 → 存入”的流程，后一步能使用仅在前一步模拟中得到的资产；每一步仍独立按自己的 `expects` 对账。

### 5\. Agent 与系统各自承担不同安全责任

Moss 能机械验证“实际执行效果是否符合 Plan 的声明”，但无法知道用户最初说了什么。因此 Agent 仍必须做 **intent alignment（意图对齐）**：把仿真的资产流出/流入、操作类型和授权对象，与用户的真实请求逐项比较。零 warning 表示“Plan 被正确执行”，不自动表示“Plan 就是用户想要的”。

## AI Agent 为什么需要这个框架

AI Agent 擅长理解自然语言、选择工具和编排步骤，但不适合成为直接拼装高价值链上交易的可信计算基：它可能误读 ABI、混淆单位、引用错误地址，或在复杂协议细节中遗漏一步。Moss 的设计将职责拆开：

| 角色 | 应负责的事 | | 协议适配器 | 将特定协议的细节实现为标准能力，并量化风险与预期效果 | | Moss | 生成未签名 Plan、回放交易、提取实际效果、发现不一致 | | AI Agent | 理解用户意图、选择能力、强制仿真、解释结果并做意图对齐 | | 用户钱包 | 最终审阅、签名与广播 |

这种分工减少了 Agent 的“自由发挥”空间，同时保留它在自然语言交互、跨协议编排和结果解释方面的优势。

## 未来可能的应用场景

1.  **安全型 DeFi 助手**：用户说“把我的奖励换成 USDC，再存入收益池”，Agent 发现能力、生成多步 Plan、统一仿真后，用人类可读的资产变化摘要交给用户确认。
    
2.  **钱包内的自然语言交易层**：钱包不只展示难读的 calldata，还可展示“支付什么、至少收到什么、给谁授权多少”的验证结果；Moss 适合成为交易前的解释和校验层。
    
3.  **协议聚合与执行编排**：随着更多协议以“一协议一适配包”接入，Agent 可按用户意图从 DEX、借贷、质押、奖励协议中选择能力，统一处理而不需要为每个协议重写一套 Agent 逻辑。
    
4.  **机构或 DAO 的受限执行工作流**：把 Plan、仿真结果和风险标签接入审批流程；策略 Agent 可以提出执行建议，但只有通过效果对账、策略规则和多人签名后才可签署。
    
5.  **Web3 客服与教学沙盒**：因为构建与模拟不需要私钥和真实资金，可让用户或开发者先观察“某个意图会产生什么交易与资产变化”，再决定是否实盘执行。
    

## 我的理解

我认为 Moss 最有价值的地方，是把 AI Agent 在链上的权限从“直接写交易”收缩为“理解意图、调用受约束的能力、解释已验证结果”。它不是自动交易机器人，也不是替用户做决定的签名器；更像是 Agent 与钱包之间的一道交易防火墙和语义适配层。

它也没有宣称仿真等于绝对安全：链上状态、价格和流动性会在模拟到签名之间变化，协议也可能升级。因此，Moss 依靠的是“链上最小保护 + 模拟发现差异 + 用户最终签名”的组合，而非单次仿真的保证。当前不支持跨链桥、Permit / typed-data 签名流程和 flash-loan 原子组合，也说明它优先选择可验证的范围，而非急于覆盖一切。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->










# Day7

## 部署你的第一个 Monad 合约

1.  合约地址：`0x398E6E1E63315d2095749171e1269c7DEA80e7b7`
    
2.  部署交易 hash：0x398E6E1E63315d2095749171e1269c7DEA80e7b7
    
3.  至少一次合约交互 transaction hash：
    
4.  合约部署或交互截图
    
5.  README v0.1 链接或截图
    

这个实际上和之前做的solidity部分有些重合了，直接部署之前的留言板合约。

在remix中编译

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ODU0MGExYjJhYmZiZDhkZTA1MTE2MmY0NTE5ZTY2YTJfMDRuQzR3cjUyRU5yZWc3a3J4RE9GQU5ubTB2bmNDelhfVG9rZW46TVJDM2JUcGJSb2lNN3V4TUxkN2NvSUVpbmxoXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)

编译通过

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NDFhNzdjZjAxNjMwMmJjNjBjZDU1MjQ3YjE2NTA3YmRfakRVYTFvNGpjYllod1E2dG1Ebk5Yb2htTkg2R1FFY25fVG9rZW46WFZoWWJ1TDVRb1lkaER4QXN2RmN0dlZqbnhoXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)

接着部署到Monad Testnet，第一张图选错了，应该是browser wallet

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTBmZjQ0MTIyZTc3Y2Y4NjQ2YTE1MzQ4ZWUzMTllNTRfcjZhMXNLQVc0YWtkWUNSYzFBcnREdFR1Y3VkV0ZyMzlfVG9rZW46TGpDQ2JvT1lxb0Y1VlJ4SDJCSWNESHNpbkhnXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDZhMzZjYjZjNjlhN2RkNzQzYzgzMzRkNGJiMmE2MDRfbTVvc21TSlhtUEx4WUJZUVdOejRWQ0ttT25hU3FYS1NfVG9rZW46UHdhNWJzVDBXb3U2SDB4elhwQ2NLWlFqblBiXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTE1Zjg5ZmY5MzZmZTc3Y2FmZjZhMjU2NzdhMWEyNmVfR0xlcmpmV0FtS09qSmxqZ2ZIazBtNjJlWURTVU5uaURfVG9rZW46RUJHSmI5U3dmb2IybG94a0k1OGMxNUdsbmdmXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OWQ3ZGFlYTQ1MmYxOTM0NzdmZjYzZWE5YzUwZGM1YmJfVHJuNnpqeHo3TDAxcWRGcHVnWU10RWV1YmxTZnY4Q2JfVG9rZW46TjhldWJFZEIzbzVIYUR4T1NNeWNCR1NKbkVyXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)

| 字段 | 含义 |
| status: 1 Transaction mined and execution completed | 表示交易成功。status: 1 代表成功，mined 表示交易已经被打包进区块，execution completed 表示合约部署执行完成。 |
| transaction hash: 0x0ca647...b1f5d4 | 这是本次部署交易的唯一编号。以后可以用这个哈希在区块浏览器中查询这笔交易。 |
| block hash: 0x99f718...0309e | 这是打包该交易的区块哈希，可以理解为这个区块的唯一标识。 |
| block number: 42965461 | 表示这笔部署交易被打包进了第 42965461 个区块。这个区块号比较大，说明这次不是 Remix 本地 VM 的第 1 个区块，而是部署到了真实的测试链或公链环境。 |
| contract address: 0x398E6E1E63315d2095749171e1269c7DEA80e7b7 | 这是部署成功后生成的合约地址。以后调用 postMessage、getMessageCount、getMessage，就是在和这个地址上的 MessageBoard 合约交互。 |
| from: 0x20295E0ED60dcE1147f4b474D689ce95dcABc4bf | 这是发起部署交易的钱包地址，也就是部署这个合约的人。 |
| to: MessageBoard.(constructor) | 表示这不是普通转账，而是在执行 MessageBoard 合约的构造过程，也就是部署合约。部署时合约地址还不存在，所以这里显示的是调用构造函数 constructor。 |
| transaction cost: 1140996 gas | 表示这次部署合约总共消耗了 1,140,996 gas。部署合约需要把合约代码写入链上，所以通常比普通转账消耗更多 gas。 |
| decoded input: {} | 表示部署时没有传入构造函数参数。你的 MessageBoard 合约没有需要初始化传入的参数，所以这里是空对象。 |
| decoded output: - | 表示这笔部署交易没有普通函数返回值。部署交易的主要结果是生成一个新的合约地址，而不是返回某个变量。 |
| logs: [] | 表示部署过程中没有触发事件日志。虽然合约里定义了 NewMessage 事件，但只有调用 postMessage 发布留言时才会触发，部署时不会触发。 |
| raw logs: [] | 表示原始事件日志为空，和 logs: [] 对应，说明本次部署没有产生事件。 |
| [Verification] Contract deployed. Checking explorers for registration... | Remix 检测到合约已经部署成功，并开始尝试把合约源码提交到区块浏览器或验证服务进行合约验证。 |
| Etherscan verification skipped: API key not provided. | Etherscan 验证被跳过，因为 Remix 没有配置 Etherscan API Key。这不影响合约部署，只是不能通过 Etherscan 完成源码验证。 |
| Please input the API key... | 提示如果想用 Etherscan 验证合约，需要在 Remix 设置或合约验证插件中填写 API Key。 |
| [Sourcify] Verification submitted. Awaiting confirmation... | Remix 已经把合约源码提交给 Sourcify 进行验证，正在等待确认。 |
| [Sourcify] Verification Successful! View Code | Sourcify 验证成功。说明你的合约源码和链上部署的字节码能够对应起来，别人可以通过 Sourcify 查看合约源码。 |

使用post message

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2IyYjI1ZmM1N2IzOTFkYWYyZDRhZjUxY2ExYmY2YTZfWmQzRjR5ejlwZ2NCc0xMTWNxUkVkTGV6WUd5U3NMQ3ZfVG9rZW46SWtUZWJpZTdPb29kWGh4NVVQTGNLUllZbmdnXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YTQwZjIwZjNiZGM2NzA1ZjhmYWU5ZmQzZDE3YjI2MWZfTnExUlNTZHl2T0tUejhMblRJZGg5QWdIUm1WRUlmakhfVG9rZW46WHVycGIyUVpNbzJtdnl4MHdEamNSZDVhbjlmXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)

| 字段 | 含义 |
| status: 1 Transaction mined and execution completed | 这次留言交易成功执行。 |
| transaction hash: 0x0a5836...1189a | 这是调用 postMessage 的交易哈希，也就是本次合约交互的交易编号。 |
| block number: 44228423 | 这次留言交易被打包进第 44228423 个区块。 |
| from: 0x20295E...Bc4bf | 调用函数的钱包地址，也就是留言者地址。 |
| to: MessageBoard.postMessage(string) 0xc1064...22423 | 表示这笔交易发给了你的留言板合约，并调用了 postMessage(string) 函数。 |
| transaction cost: 209631 gas | 这次发布留言消耗了 209631 gas。因为它把 "hello world" 写入了链上状态，所以需要 gas。 |
| decoded input | Remix 解码后的函数输入，说明你传入的留言内容是 "hello world"。 |
| decoded output: - | 这个函数没有返回值，所以输出为空。 |
| logs: 1 | 这次交易触发了 1 条事件日志，也就是 NewMessage 事件。 |

输入post message的hash查询，可以看到post的信息，hello world

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MGEzM2I5ZDFmYWFhYzcxODRiYmE0ZmFmZGNhYmNhZjBfQ2ZzMnlkTkNYaHVIZzRxS3Rwcml1Z1Z0dVRCd1duSnRfVG9rZW46WHROcGJka3JMb2NEcFp4WWExTGM4clNubk9kXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)

暂时无法在飞书文档外展示此内容

## week1的demo

## 1\. 我做了什么

我完成了「星核档案馆」Mini Demo 0：一个像素风的可验证概率抽卡游戏原型。用户可以在三个卡池「晨星回响」「遗迹守望」「深渊星图」之间切换，查看角色预览、概率配置、SSR 保底进度，并完成一次模拟抽卡。每套卡池有六张角色卡，共 18 张像素卡面。

产品判断是：抽卡本身的交互频率有限，但抽到的角色可以进入后续的“链上远征 + 短周期冲榜”玩法，使角色资产、任务积分、排行榜和奖励结算形成更高频的 Monad 使用场景。

## 2\. 哪部分是真实链上操作

**当前没有真实链上写入。** Mini Demo 0 是界面与交互流程原型，随机数、区块号、交易哈希和记录都由前端模拟生成。这个限制已在 README 和产品界面中明确标注，避免把演示数据误称为链上数据。

已完成的是链上产品接口的设计边界：

-   链上应记录卡池概率版本、抽卡请求、VRF 结算结果、保底计数和奖励资格。
    
-   链下应负责像素动画、角色素材、索引查询和排行榜缓存。
    
-   Week 2 的第一个真实交互将是部署 Monad 测试网合约，并通过钱包发送 `requestDraw` 交易，再读取事件结果。
    

## 3\. AI 辅助完成的部分

-   使用 AI 生成了三组统一风格的像素角色立绘表，并通过去背处理转为网页素材。
    
-   使用 AI 协助搭建 React 页面结构、卡池切换、模拟抽卡和响应式样式。
    
-   使用 AI 梳理 Monad 高交互方向的产品假设和 Demo 功能清单。
    

## 4\. 我做出的人工判断与修改

-   我选择“公开概率的抽卡 + 可进入远征玩法的角色资产”，而非只做 NFT 展示页。
    
-   我将三组角色分别设计为月光、遗迹、深渊主题，手动确定卡池名称、角色名称、稀有度、概率、保底规则与交互文案。
    
-   我明确区分了当前模拟状态与未来真实链上状态；不使用伪造的“已验证”链上结论。
    
-   我判断实时动画与排行榜展示不必全部上链，只有影响公平性与奖励的状态应写入合约。
    

## 5\. Week 2 主方向

**方向选择：Tech**

我将继续推进“Monad 上的高频链上远征与冲榜小游戏”。目标是让抽到的角色不只是收藏品，而是可以频繁参与探索、任务、抢点与赛季排行榜的链上资产。

### Week 2 要解决的问题

如何在 Monad 测试网上实现一个足够轻量的真实循环：玩家连接钱包 → 发起一次探索/抽卡交易 → 合约写入状态并发出事件 → 前端即时读取结果 → 排行榜根据链上事件更新，同时把高频展示数据保留在链下索引层。

## 作品集 / 简历一句话

独立设计并实现了一个面向 Monad 高频交互场景的像素风链游 Demo，完成多卡池抽卡交互、可验证概率产品流程、18 张像素资产接入，以及从链上公平结算到链下实时展示的技术边界设计。

## 参考的下一步架构

```Plain
钱包 / 前端
  → Monad 测试网 Solidity 合约
  → 抽卡或远征事件
  → 前端事件监听与索引层
  → 实时地图、排行榜与任务 UI
```

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTY3NTdhZmQyMjUyNzg4NGExZjY4MTJhNGM4ZjE2MzdfOHloRmhRVzRkRk9YbjZqcHI5MTFDU3kzVHVBNDNNYTZfVG9rZW46VmJDNmJBemZyb1poaXF4dURIaWNZVHpmbnJkXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YzY0NTc5ZWI2YzUxNWVlMWY3OWQzZDcwZmFjNzdkMjhfRjhVbHozclVaU2x2OUtLQzcxMWFKNEo3cm50RHYxU25fVG9rZW46RVZyUWJFdXQyb28wMHR4b0VVeGN0bklKbk5iXzE3ODM4NjA5NTY6MTc4Mzg2NDU1Nl9WNA&add_watermark=true&scene_type=CCM)
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->











# Day6

## 实现 Cadence：单时隙版本

在浏览器里实现一个可视化的 BFT 共识模拟器，理解提案、投票、最终确定和并发 slot 调度。

## 核心概念

-   `f = floor((N - 1) / 3)`：最多可容忍的 Byzantine 节点数。
    
-   `Quorum = N - f`：达到该数量的相同投票，提案可以被决定。
    
-   `Rebuild = f + 1`：收集到这么多不同的 key piece 后，可以模拟“解密”提案。
    
-   Slot：一轮共识的时间窗口；proposer 会随 slot 轮转。
    
-   Speculative finality：首轮投票已完整送达 observer，结果可用于 commit。
    
-   Finality：收到 quorum 个相同 commit vote 后，区块真正最终确定。
    

## 今天完成

-   搭建 Next.js 仪表盘，支持调节验证者数量与 `tau`。
    
-   模拟了带 30–90ms 延迟的网络、消息队列和逻辑时钟。
    
-   实现 Propose → First Vote → Commit Vote → Finalize 流程。
    
-   实现提案 chunk、yes/no 投票、key piece 解密、quorum 判断。
    
-   从单 slot 重构为多个 slot 并发运行，并加入 Auto-Run Conductor。
    
-   实现有序链：乱序完成的区块先进入 pending，补齐缺口后再按 slot 顺序写入链。
    
-   加入 10-slot 刹车、Faulty Proposer 的 SKIPPED block 和 Outage 恢复。
    

## 一个重要理解

“消息到达”“提案被决定”“区块最终确定”是三个不同阶段。并发运行时，后面的 slot 可能先完成，但链仍必须按 slot 顺序提交，所以需要 pending 队列。

## 验证结果

-   手动模式能完成一轮出块。
    
-   Auto-Run 可持续产生并发 slot。
    
-   Outage 时 in-flight 会增长到 10 后被刹车；恢复网络后消息批量投递。
    
-   `npm run build` 通过。
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YjRjM2I1NjE1ZjRjM2MxM2UwYjVhYTZmNGE4Y2RhYTJfMndHUUo3Y1o4aFVTbXUxUmtCbWZnWUxlV1lvcVMwaUVfVG9rZW46TlNZVmJnUFdrb0hnaUJ4Rzl3OGNsTnFubmNjXzE3ODM3ODQ2ODQ6MTc4Mzc4ODI4NF9WNA&add_watermark=true&scene_type=CCM)
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->












# Day5

今天跟着buildanything网站上的剩余课程复习了钱包，智能合约，构建去中心化的内容，周末打算把进度补上，跟进学习页面上的其他挑战。  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Foreon/images/2026-07-10-1783697951764-image.png)
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->













# Day4

今天学习了10000tps对于构建应用服务的重要性，并且学习了数据库和sql语言，以及数据上传的安全问题，比如上传文件攻击等等。  

![QQ20260708-233039.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Foreon/images/2026-07-09-1783611711974-QQ20260708-233039.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->














学习buildanything

今天在buildanything网站上学习，了解到了vibe coding这个概念，学习了怎么用vibe coding去开发，维护一个应用。

维护一个应用，需要：数据库、身份认证（Auth）、文件存储、支付、邮件、错误追踪。

而这些需求都是一个你无法控制的外部服务。它们都有可能宕机、被攻击、冻结你的账号，或者一夜之间消失。所以我们需要开发一个去中心化掉应用。 然后学习了区块链的相关概念：

> 交易被打包进 区块。节点用 共识 机制——在 Ethereum 和 Monad 这里就是 Proof of Stake——来就哪些区块有效达成一致。验证者因为质押的抵押品，以及 slashing（罚没） 的风险，被迫保持诚实。用户支付 gas 用来酬谢验证者并防止垃圾信息。当足够多的区块被确认后，交易就达到了 最终性，从此永久。
> 
> 实用工具集是你真正与上面这一切打交道的方式：chain ID 告诉钱包用哪条网络，RPC 承载你的请求，faucet 给你 testnet 代币玩，explorer 让你看到链上正在发生什么。

monad像是以太坊一样的区块链系统生态，可以让很多web3应用运行在上面，但monad拥有更高的性能。mon是这条链的代币。  

![QQ20260708-233039.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Foreon/images/2026-07-08-1783525034675-QQ20260708-233039.png)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->















## 完成第一笔交易

1.  Transaction hash：0xe3862356c253773e48beb9e2a2ddf80b9a53ce53335c441dc4d5ff492a7bfa9c
    
2.  区块浏览器链接：[testnet.monadvision.com](http://testnet.monadvision.com)
    

from / to 分别是谁： from 是发起交易的钱包地址，也就是付款方：`0x20295E0ED60dcE1147f4b474D689ce95dcABc4bf`；to 是接收交易的钱包地址，也就是收款方：`0xbc58724dBcA23325a4b0C0eaEB050B573192Bd32`。

value 表示什么： value 表示这笔交易实际转出的主币数量。这里的 `Value: 1` 表示 from 地址向 to 地址转了 **1 个 MON**。

gas 和手续费代表什么： gas 是执行这笔链上交易所消耗的计算资源，可以理解成“链上操作成本”。手续费是发送方为这次交易支付给网络的费用，计算方式大致是 `Gas Used × Gas Price`。这笔交易用了 `31,500 gas`，gas price 是 `122.4 Gwei`，所以手续费是 `0.0038556 MON`。其中 `0.00315 MON` 被销毁，剩余部分可能给验证者/矿工作为激励。

交易状态是成功还是失败： 这笔交易状态是 **Success**，说明交易已经成功执行，并且已经被打包进区块 `#42943344`。

失败交易为什么也可能消耗 gas：

因为失败交易虽然最终没有完成目标操作，但区块链节点仍然要执行它、验证它、计算它为什么失败。只要交易已经被链上执行，就会消耗计算资源，所以即使最后因为余额不足、合约条件不满足、滑点过高、gas 不够等原因失败，已经消耗掉的 gas 通常也不会退回。

在https://faucet.monad.xyz/领测试币

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjFjN2UwNzMxMzgwYTg5ZTcyZWY3MmFiNDc5YWM5YzNfVnpTbzBPT0xEdlhrZkhWUjE1cVJFenJCRFZ0eVdiNlNfVG9rZW46WHg4bGI3M2gwb1Mza1d4YldFcGNTUmJhbkpjXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

Transactions

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MWI5ZTY1MmM0ZjM3NmJkMGQ5YmUyMzQzZDYzODU3NDhfNzY4dlcxdVRnalNieTlId1ZEUnd2U2ZidG9TQXZWQkNfVG9rZW46QWdCdmJSbmhMb1h0bFN4TnduT2NDRjdZblhjXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

给自己又注册了一个钱包地址，并发起转账

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NDc0MTAwOGE2OTViMzJlZTAxNjNiN2MzMTBlZmQ5MWZfOUd1akpZbzlEMXplUU5hWEVQZzBjcks0TUNHUEx6MmFfVG9rZW46TlFOemIzZlV3b3Viamt4TnMyMmMxemd5bjZjXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ODllNmU4NDQ0N2QzYjI2ZWQ4YzlhMjA5OTZlYzJmYWRfejZSSTlxcjlGRlhRYW9FV21KTDNkcjJ5QTdRYklKRXRfVG9rZW46TWs4VWJIdWtFb3dYWTF4dm43d2NVZEFYbmRkXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

Hash transaction

0xe3862356c253773e48beb9e2a2ddf80b9a53ce53335c441dc4d5ff492a7bfa9c

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YmQ0N2FhYjAwZWEzYTg2NmU0ZTdjZGZmY2UzYTM1OThfVnc4cWR0ZXZzRVZhTzJGdUtVRklqalljdFIyV09uTXRfVG9rZW46SDM0cWJpbW5Wb3NuOFl4NjFTcmNnRzBKbjlkXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

## 用 AI 生成一个最小 Solidity 合约

1.  Prompt：帮我写一个留言板solidity合约
    
2.  AI 生成的主要输出：
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MessageBoard {
    // 定义一条留言的数据结构
    struct Message {
        address sender;     // 留言者地址
        string content;     // 留言内容
        uint256 timestamp;  // 留言时间
    }

    // 保存所有留言
    Message[] public messages;

    // 留言事件，方便前端或区块浏览器监听
    event NewMessage(address indexed sender, string content, uint256 timestamp);

    // 发布留言
    function postMessage(string memory _content) public {
        require(bytes(_content).length > 0, "Message cannot be empty");

        messages.push(Message({
            sender: msg.sender,
            content: _content,
            timestamp: block.timestamp
        }));

        emit NewMessage(msg.sender, _content, block.timestamp);
    }

    // 获取留言总数
    function getMessageCount() public view returns (uint256) {
        return messages.length;
    }

    // 根据编号获取某一条留言
    function getMessage(uint256 _index)
        public
        view
        returns (address sender, string memory content, uint256 timestamp)
    {
        require(_index < messages.length, "Message does not exist");

        Message memory message = messages[_index];
        return (message.sender, message.content, message.timestamp);
    }
}
```

3.  合约源码如上所示。
    
4.  你做过的人工修改或判断说明。
    

review发现，需要加入最长消息长度：

```Solidity
require(
            bytes(_content).length <= MAX_MESSAGE_LENGTH,
            "Message is too long"
        );
```

5.  至少 3 条“AI 生成代码需要人工检查的地方”： 1️⃣**检查合约是否能正常编译：** 需要把代码复制到 Remix 或其他 Solidity 编译器中，确认 `pragma solidity ^0.8.20`、结构体、数组、函数、`require` 等语法没有报错。 2️⃣**检查函数是否符合需求：** `postMessage` 负责发布留言，`getMessageCount` 负责查看留言数量，`getMessage` 负责按编号读取留言，功能和留言板需求基本一致。 3️⃣**检查留言长度限制是否符合预期：** 合约中设置了 `MAX_MESSAGE_LENGTH = 200`，但这里限制的是**字节长度**，不是字符数。英文 200 个字母大约是 200 字节，但中文一个字通常占多个字节，所以实际可输入的中文字符会少于 200 个。
    

完整代码：

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MessageBoard {
    // 最长留言长度，单位是字节
    uint256 public constant MAX_MESSAGE_LENGTH = 200;

    // 定义一条留言的数据结构
    struct Message {
        address sender;     // 留言者地址
        string content;     // 留言内容
        uint256 timestamp;  // 留言时间
    }

    // 保存所有留言
    Message[] public messages;

    // 留言事件，方便前端或区块浏览器监听
    event NewMessage(address indexed sender, string content, uint256 timestamp);

    // 发布留言
    function postMessage(string memory _content) public {
        require(bytes(_content).length > 0, "Message cannot be empty");
        require(
            bytes(_content).length <= MAX_MESSAGE_LENGTH,
            "Message is too long"
        );

        messages.push(Message({
            sender: msg.sender,
            content: _content,
            timestamp: block.timestamp
        }));

        emit NewMessage(msg.sender, _content, block.timestamp);
    }

    // 获取留言总数
    function getMessageCount() public view returns (uint256) {
        return messages.length;
    }

    // 根据编号获取某一条留言
    function getMessage(uint256 _index)
        public
        view
        returns (address sender, string memory content, uint256 timestamp)
    {
        require(_index < messages.length, "Message does not exist");

        Message memory message = messages[_index];
        return (message.sender, message.content, message.timestamp);
    }
}
```

在remix中编译

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YzNhZDkzMzQyNmI2MThmNGJjNTA1YjE0NGNkZGEyMTlfclJldENrWTA0QkNSYlJnWHc4ZW0zRk1yenRRZzlVbVRfVG9rZW46R2NaWmJpdDNSb2dLRTZ4TEppc2NoTFhJbkliXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

编译通过

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDA4ODVlZDFmYTZmMDdmM2EzYWEwNzZjMmZlNDAzNWFfWHhxVGs3NmFaRWRiUnFlcDhBMDhueTB5VDdLbTVacEdfVG9rZW46TlNlNGI1NDNlb1FUcGR4OVI1OWN3bGdQblRiXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

接着部署到Monad Testnet，第一张图选错了，应该是browser wallet

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ODQ3MDNlOTA5YjBlZmE3YWQzMGEzMTRlNmQ2MzcyMjdfUGhEYTBRVlhFRGo1RmRIN2d5ejhmZFdmVVNjb3REUldfVG9rZW46TUZ1bmJtaExOb2lBU1Z4MG4wUGNGWWVEbk5nXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MGVhOThjMzM0MjhjMDgyYmJhNjdhOGE3YjRlZWVhZTNfV2wxQ1g0TVhFbXo5eHAwV1hWb2NHRXhaV1U2Q2JGbnJfVG9rZW46QVVzdmJhM2hVb0tiRWx4SURBWmNRNnJ1bmdiXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NWNlYmYyOTk0YTMyMGMzMTUyMWUyOWFkYjNiOGI5NmRfcFpOckFvcUR0YklZaDhXcXo3Y2lua1ZQR0VhWVFFTG1fVG9rZW46VmJmRmJBeDlEb2piN0Z4WnVTemNXZmN1bjhjXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NDllZGFlZDk5ZGMyYjI4YTk4ZjU0ZWQ1MjliZWE4ZDVfMTFTZE5kcU1vcXp2ZThoY2x2b3UySU9vbWJlZFVPbTZfVG9rZW46UGhzNGJOVUN6b3hiaVF4M01TRWNHck84bmhiXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

| 字段 | 含义 |
| status: 1 Transaction mined and execution completed | 表示交易成功。status: 1 代表成功，mined 表示交易已经被打包进区块，execution completed 表示合约部署执行完成。 |
| transaction hash: 0x0ca647...b1f5d4 | 这是本次部署交易的唯一编号。以后可以用这个哈希在区块浏览器中查询这笔交易。 |
| block hash: 0x99f718...0309e | 这是打包该交易的区块哈希，可以理解为这个区块的唯一标识。 |
| block number: 42965461 | 表示这笔部署交易被打包进了第 42965461 个区块。这个区块号比较大，说明这次不是 Remix 本地 VM 的第 1 个区块，而是部署到了真实的测试链或公链环境。 |
| contract address: 0x398E6E1E63315d2095749171e1269c7DEA80e7b7 | 这是部署成功后生成的合约地址。以后调用 postMessage、getMessageCount、getMessage，就是在和这个地址上的 MessageBoard 合约交互。 |
| from: 0x20295E0ED60dcE1147f4b474D689ce95dcABc4bf | 这是发起部署交易的钱包地址，也就是部署这个合约的人。 |
| to: MessageBoard.(constructor) | 表示这不是普通转账，而是在执行 MessageBoard 合约的构造过程，也就是部署合约。部署时合约地址还不存在，所以这里显示的是调用构造函数 constructor。 |
| transaction cost: 1140996 gas | 表示这次部署合约总共消耗了 1,140,996 gas。部署合约需要把合约代码写入链上，所以通常比普通转账消耗更多 gas。 |
| decoded input: {} | 表示部署时没有传入构造函数参数。你的 MessageBoard 合约没有需要初始化传入的参数，所以这里是空对象。 |
| decoded output: - | 表示这笔部署交易没有普通函数返回值。部署交易的主要结果是生成一个新的合约地址，而不是返回某个变量。 |
| logs: [] | 表示部署过程中没有触发事件日志。虽然合约里定义了 NewMessage 事件，但只有调用 postMessage 发布留言时才会触发，部署时不会触发。 |
| raw logs: [] | 表示原始事件日志为空，和 logs: [] 对应，说明本次部署没有产生事件。 |
| [Verification] Contract deployed. Checking explorers for registration... | Remix 检测到合约已经部署成功，并开始尝试把合约源码提交到区块浏览器或验证服务进行合约验证。 |
| Etherscan verification skipped: API key not provided. | Etherscan 验证被跳过，因为 Remix 没有配置 Etherscan API Key。这不影响合约部署，只是不能通过 Etherscan 完成源码验证。 |
| Please input the API key... | 提示如果想用 Etherscan 验证合约，需要在 Remix 设置或合约验证插件中填写 API Key。 |
| [Sourcify] Verification submitted. Awaiting confirmation... | Remix 已经把合约源码提交给 Sourcify 进行验证，正在等待确认。 |
| [Sourcify] Verification Successful! View Code | Sourcify 验证成功。说明你的合约源码和链上部署的字节码能够对应起来，别人可以通过 Sourcify 查看合约源码。 |

总结：这次 `MessageBoard` 合约已经成功部署到了链上，合约地址是 `0x398E6E1E63315d2095749171e1269c7DEA80e7b7`，部署者是 `0x2029...c4bf`，本次部署消耗了 `1,140,996 gas`，并且源码已经通过 Sourcify 验证。

**gas是哪来的？** 你这里的 **gas 不是你自己“拥有的一种东西”**，而是 Remix 帮你在本地测试环境里模拟出来的“计算费用单位”。

你现在看到的是 Remix VM 里的部署结果，大概率不是在真实主网/测试网上操作。Remix VM 会自动给几个测试账户分配很多假的 ETH/MON，用来支付 gas，所以你不需要自己充值，也不会真的扣钱。

简单理解：

`gas`：表示执行合约需要消耗多少计算资源。 `gas fee`：表示这些计算资源折算成要付多少链上币。 `Remix VM`：本地模拟区块链，账户里自带测试币，所以能直接部署。 `真实链上`：你必须钱包里有该链的原生币，比如 ETH、MON、BNB，才能支付 gas。

比如你部署合约时显示：

这不是说你真的有 `753791` 个 gas，而是说：**这次部署消耗了 753791 单位的链上计算资源**。

如果在真实 Monad 测试网或以太坊测试网上部署，钱包里就必须有对应的测试币，否则会报类似：

也就是：余额不够支付 gas。 **合约部署上链是什么意思，链是从哪来的，我部署合约本质上是在干什么？**

**合约部署上链**的意思是：你把写好的 Solidity 合约代码，编译成区块链能执行的字节码，然后通过一笔交易发送到区块链网络里。交易成功后，链上会生成一个新的合约地址，这个地址里保存着你的合约代码和状态数据。

你可以这样理解：

**链 = 一个由很多节点共同维护的公共数据库 / 公共账本。**

比如以太坊、BSC、Polygon、Monad 测试网，都是不同的链。每条链都有很多区块，区块里面记录交易。用户转账是一种交易，部署合约也是一种交易。

你在 Remix 里部署时，可能有两种情况：

**第一种：Remix VM 本地链。** 这是 Remix 在浏览器里模拟出来的一条假链，只用于测试。账户、余额、区块、gas 都是模拟的，不是真的上公网区块链。

**第二种：真实测试网或主网。** 比如连接 MetaMask 后部署到某个测试网，那就是真的把合约部署到那条链上。别人也可以通过合约地址查到和调用它。

你部署合约本质上是在做这件事：

比如你部署留言板合约后，链上生成了：

这个地址就像你留言板程序的“线上地址”。以后调用 `postMessage()`，就是给这个合约地址发送交易，让链上的程序执行“保存留言”这段逻辑。

所以一句话总结：

**部署合约上链，就是把你的程序发布到区块链这个公共运行环境里，让它获得一个链上地址，并且以后可以被钱包或其他程序调用。**

部署合约的地址：0x398E6E1E63315d2095749171e1269c7DEA80e7b7
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
















# Day1

今天学会了web3入门的前置准备，设置好了钱包，添加了monad testnet，并且测试了地址存在于链上，明天继续尝试交易。  
前置准备

安装metamask钱包

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MjZmZDIxNDRiMjBmYTAyOGQ0ZjEwOGJlNzBmZTUzNjRfUmN6TGRtbUJuOHJOMmpSU3hFamFLM2x5OHVWbGxHcGdfVG9rZW46U1VKMWJ1ekg5b2Q4TVF4VUtramM0RkRkblNkXzE3ODMzNTI2MDc6MTc4MzM1NjIwN19WNA&add_watermark=true&scene_type=CCM)

0x20295E0ED60dcE1147f4b474D689ce95dcABc4bf

添加monad testnet，其实我注册的时候已经被添加了

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZThlNjA3MWIyZTdlZTdjZTg3ODg4YjM2ODAxOTc2ODJfZkJjZllmNmpPQWE5OEFRdWNTNlJibUpGcFljMWxyRW5fVG9rZW46T3M5WGJNMXJkb2QxYmF4N0Y3S2NndER2bkFnXzE3ODMzNTI2MDc6MTc4MzM1NjIwN19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MzcwYjZhN2VlYjY5Zjc3MWQ3ZWU1YjAwMmU1ZTQ4ZjZfTzZZTGVmOTlwZ2xKaGgzR1VvbHlRbHc2SjY3enZKc1ZfVG9rZW46Tk5DM2JLa25ib1g1U0F4OWdGSmNRcmR0bkdoXzE3ODMzNTI2MDc6MTc4MzM1NjIwN19WNA&add_watermark=true&scene_type=CCM)

打开 Monad Explorer 或相关区块浏览器，确认可以查询地址。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NTM5YWIwYjRhNjNiZjhjNTgwMDVjNTg1NjcwMGU2MjRfWXVDNWkzMVVIa1FQZEh0NWN0YmdiNGlWWUhaVEVlcTNfVG9rZW46WmczbGJTN1VYb2VjWFJ4TEcwb2Nuc3M5bnNmXzE3ODMzNTI2MDc6MTc4MzM1NjIwN19WNA&add_watermark=true&scene_type=CCM)

使用钱包工具metamask

地址0x20295E0ED60dcE1147f4b474D689ce95dcABc4bf

链上产品和普通互联网产品最大的区别是：**普通互联网产品主要由公司服务器控制，链上产品主要由区块链网络和智能合约控制。**

普通互联网产品，比如微信、淘宝、抖音，用户的数据、账号、交易记录一般都存在公司的服务器里。公司可以修改规则、封号、下架内容、调整数据，用户更多是在使用平台提供的服务。

链上产品的核心数据和交易通常记录在区块链上。很多规则写在智能合约里，一旦部署，任何人都可以查看合约逻辑和链上记录，数据更透明，也更难被单方面篡改。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
