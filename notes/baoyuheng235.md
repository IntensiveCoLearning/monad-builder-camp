- GitHub ID: 292343146
- Name: baoyuheng235
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
<!-- DAILY_CHECKIN_2026-07-27_START -->
# 2026-07-27

由于残酷共学平台问题导致本人没有打上卡，此条记录为补卡。
<!-- DAILY_CHECKIN_2026-07-27_END -->

# 2026-07-25
<!-- DAILY_CHECKIN_2026-07-25_START -->
# 2026-07-25

今天做了moss的mini demo，这个demo的code部分差不多已经完成了，但是一开始考虑用 MON/WMON 做 lending，但是检查 Monad 上的 Aave V3 Pool 后发现当前部署并没有支持 WMON reserve，所以不能直接做 MON lending，现在确认可用的是 USDC。

问题是当前连接的是 Monad Mainnet，USDC 是真实资产，不是测试币。如果直接做 live demo，需要真实 USDC 和 MON gas，风险比较高也不是很安全。

agent给的方案是做 deterministic fixture：模拟一份真实链上执行后的 Change\[\] evidence，然后验证 Moss 的完整流程：User Intent → Capability Discovery → Transaction Tree → Receipt Verification。 我们后面可以再研究一下这个方案是不是ok的
<!-- DAILY_CHECKIN_2026-07-25_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
# 2026-07-24

找到组员完善moss demo中
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# 2026-07-23

休息今日
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
# 2026-07-22

**开源学习日志**：今天我深入研读了 Web3 AI Agent 交互框架 Moss (`nishuzumi/moss`)，不仅厘清了其“只组装与验证、永不持私钥（Never sign, never send）”的确定性安全哲学，还结合 Hardhat 与 Argot 工具链，系统掌握了开源项目 Maintainer 的 Monorepo 目录划分、双层安全对账（`expects` vs `effects`）以及基于 GitHub 的 CI/CD 与 Issue/PR 协作规范；在克服了对硬核代码的畏难情绪后，我学会了按照官方标准指南去梳理 Issue 和 Issue Template，成功迈出了从开源“旁观者”向规范“参与者”转变的关键一步。
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
# 2026-07-21

今天最大的顿悟在于：**真正的高效不是独立完成所有繁琐细节，而是学会做系统架构者与流程控场人。** 从用通俗语言解构复杂协议的底层逻辑，到用产品思维精细化编排 Space 的每一分钟，再到以“人类指挥、AI 铺路”的 Workflow 快速构建代码骨架——当 Research、Ops 与 Dev 形成闭环，原本庞杂的工程便不再是枯燥的消耗，而变成了一场清晰可控、不断提效的创造体验。
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->
# 2026-07-20

今天深耕了 Dev 赛道的工程思维，重点掌握了 Solidity、Viem/Wagmi 与 Monad 测试网等 Web3 技术栈的协同关系；明确了 AI 辅助开发的核心是“AI 负责读文档、拆任务与生成骨架，人类负责运行、验证与 Debug”的人机分工；更深刻理解了 Scope（开发边界）的精髓在于用 MVP 思路控制范围，把本周的 Prototype 砍成只保留一个核心链上动作的“滑板车”，不必要的复杂 UI 和后端全部用 Mock 处理，确保高效实现端到端闭环。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
# 2026-07-19

本次学习我掌握了智能合约项目 README v0.1 的完整制作流程，理解了 README 作为项目官方说明书的核心作用，熟练熟记了留言板 dApp 项目标准化中英文对照 README 模板的核心内容，同时掌握了 GitHub 仓库上传生成项目链接、本地文档截图两种作业提交方式，理清了智能合约项目文档规范化的实操步骤，为后续区块链项目标准化开发与归档打下了基础。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
# 2026-07-18

今天深入学习了现代 AI 驱动开发工作流（AI-assisted Dev Workflow）与开源项目阅读的底层逻辑及工程规范。真正的“人机协同”并非盲目让 AI 堆砌业务代码，而是由 AI 充当高效画图员，利用 Cursor 的 Context Awareness（上下文感知）精准吞吐文档，并在 [计划模式](https://cursor.com/docs/agent/plan-mode#plan) 下完成 Task Decomposition（任务拆解）与 Code Skeleton Generation（代码骨架生成），在编译受阻时快速解释错误；而我作为总设计师，牢记“尚未理解就开始修改”的失败模式，严格把控核心的工程直觉，以测试驱动开发（TDD）的理念进行代码的运行、修改与逻辑验证，并完成 `AI Collaboration Log` 的规范记录。同时，将这种规范性逆向运用到开源项目的阅读中，由宏观到微观层层剥离 `README`、`Issues`、`Pull Requests` 和 `Code Structure` 的逻辑链路，构建起清晰的代码库心智模型，为后续的高效工程交付与作品集组队打下了扎实的 Proof of Work 基础。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->
# 2026-07-17

今天我使用 Solidity 语言编写并成功在 Remix 环境下部署了名为 `MessageBoard`（留言板）的智能合约。在部署过程中，我深入分析了交易哈希、合约地址生成、运行时字节码以及 Gas 消耗等区块链底层回执信息，并通过控制台日志成功验证了合约初始化时抛出的默认留言事件 `"Hello ETH Pandas"`。

随后，我对合约暴露的 `messages(address, uint256)` 公共访问器进行了功能与边界测试。在传入索引 `1` 遭遇经典的 `revert` 报错后，我精准诊断出其本质是数组越界引起的底层异常，并举一反三地厘清了 `Value`（交易面值）与 `Gas Fee`（燃油费）的本质区别，明确了由于未声明 `payable` 关键字，在后续写入留言时必须保持 `Value` 为 0 的开发规范。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->
# 2026-07-15

今天重点翻看了 Github 上的 MOSS 开源项目，照着文档跑代码。代码实际效果和文档对不上，靠 AI 排障后顺利解决。不过后面的步骤说明看得一头雾水，只能继续借助 AI 辅助学习，先完整吃透项目，再准备提交 PR。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
# 2026-07-14

今天参与了两场会议。

明确了自己的方向，准备试试开源项目moss。

学习了Web3 公共物品资助、AI 时代科研落地、传统科研与 Web3 结合。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
# 2026-07-13

今天主要学习如何用vibecoding的数据库存入数据，用replit和database配合。

参与了两场会议。

完成了freshman这一章节，理解了基本入门的知识，正在疯狂的赶进度之中。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

# 主要学习了重要的区块链概念，掌握了基本vibe coding的能力
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
# 2026-07-10

今天参与了周五同伴分享会，知道了很多同伴学习的办法，接下去要继续vibe coding。

同时自己获得了第一个nft代币。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

今天主要是参加了两场会议，其中ai agent的安全问题非常值得注意，我们在架构设计中缺少了对 AI 的敬畏与对安全的防御，它随时都会变成现实。智能体给予了 AI 改变现实世界的“手”和“脚”，而我们作为架构师，必须为它装上名为“安全”的铁轨与刹车阀来保证安全，不能让人工智能操控人类的第一步真正发生。agent guard的理念让我非常受到启发，或许我们可以从更多角度思考如何为市场创造需求。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

完成任务：编写由 AI 生成的最简留言板 Solidity 合约，完成人工安全审计并修复代码漏洞，随后在 Remix 中将该合约部署至 Monad 测试网，依次调用合约读函数与写函数，完成双向链上交互操作。

今天参与两场会议，未来AI Agent支付将全面融入各类生活与商业场景，成为智能体经济的重要基础设施。这让我认识到计算机开发不止是代码编写，更要贴合场景、合规与生态落地。后续我将聚焦协议、区块链与风控方向，积累Agent工程实践能力，顺应行业发展趋势。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

选一个长期扎根的方向（比如“改善某个客户端的测试可靠性”），在这个领域里修文档、补测试、提小 PR，把学习变成一个持续输出、社区可见的过程。先利用 EPF Wiki 建立宏观地图，然后挑一个最简单的本地工具或客户端模块，让它在你的电脑上成功跑起来。

昨天学会了在链上“走路”（钱包转账），今天完成了第一次在链上运行自己审查的程序。下一步将探索合约与前端的连接交互！
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
# 2026-07-06

今天重点学习了 MetaMask 钱包准备，主要任务是创建 Mod 专用钱包，用AI 生成的部署留言板智能合约，了解了私钥的重要性，理解了一些很重要的概念，比如哈希值，Gas 费，对于刚入门来说打下了一定的基础。
<!-- DAILY_CHECKIN_2026-07-06_END -->

<!-- DAILY_CHECKIN_2026-07-28_START -->
# 2026-07-28

本周主要完成了 Moss V2 下 Aave V3 Lending Demo 的搭建与协作流程实践。从最初分析已有 PR #38 和 Issue #8 的旧版 Action 架构，到理解新版 Moss 使用 Capability、Protocol、Receipt 的设计方式，我逐渐明确了如何将传统 DeFi 协议接入 Agent 框架。通过创建 `aave-v3` protocol package，实现了基于 USDC 的 supply Capability，包括参数校验、6 位精度转换、精确 ERC20 approve、Aave Pool supply transaction tree 构建，以及 deterministic simulation fixture。同时，我也学习了为什么不能直接使用 WMON 作为 demo 资产，因为当前 Monad Aave Pool 并未配置 WMON reserve，而 USDC 才是已验证支持的资产。整个过程让我理解了 Research、Dev 和 Ops 在团队协作中的衔接方式：Research 负责确认协议事实和风险，Dev 负责实现可验证功能，最终通过 Demo 展示完整用户流程。
<!-- DAILY_CHECKIN_2026-07-28_END -->

<!-- DAILY_CHECKIN_2026-07-29_START -->
# 2026-07-29

今天完成了 Aave V3 Lending 模块的整体开发与验证工作。在完成并验证 supply 的基础上，继续实现了 withdraw、repay、borrow 以及账户和储备查询（getAccountData、getReserveData）等能力，同时补充了对应的 Receipt 校验、确定性模拟（deterministic simulation）和 MCP 能力发现测试。最终通过了 Aave V3 协议包（51 passed，1 skipped）和 MCP Server（10 passed，1 skipped）的全部测试，并完成了代码提交、分支整理和 GitHub 推送，对 Moss Capability 架构、Aave V3 协议交互、Receipt 验证机制以及团队协作开发流程有了更加系统的理解。
<!-- DAILY_CHECKIN_2026-07-29_END -->

<!-- DAILY_CHECKIN_2026-07-30_START -->
# 2026-07-30

今天完成了 Aave V3 Lending 模块的开发收尾工作，对 supply、withdraw、borrow、repay 以及账户查询功能进行了整体验证，并通过了协议包和 MCP Server 的相关测试，确认各项能力能够正常运行。同时整理了 Git 分支并将完整实现推送至 GitHub，为后续团队代码审查做好准备。通过本次实践，我进一步熟悉了 Moss Capability 架构、Aave V3 协议交互流程、Receipt 验证机制以及 Git 团队协作开发流程，对完整功能开发、测试与代码管理有了更深入的理解。
<!-- DAILY_CHECKIN_2026-07-30_END -->

<!-- DAILY_CHECKIN_2026-07-31_START -->
# 2026-07-31

今天主要完成了 Aave V3 Lending 模块的功能完善与验证，对各项能力进行了测试和检查，确保功能实现符合预期。同时，对项目整体架构和代码流程进行了进一步梳理，加深了对智能合约交互、Receipt 验证机制以及团队协作开发流程的理解，也进一步提升了独立分析问题、实现功能和解决开发过程中实际问题的能力。
<!-- DAILY_CHECKIN_2026-07-31_END -->

<!-- DAILY_CHECKIN_2026-08-02_START -->
# 2026-08-02

今天主要对项目开发成果进行了整理与总结，进一步熟悉了 Aave V3 Lending 模块的整体流程，包括交易能力实现、查询功能、测试验证以及 Git 分支管理等内容。通过对开发过程的回顾和代码梳理，加深了对区块链协议交互、模块化开发和团队协作流程的理解，也提升了分析问题、解决问题和规范管理代码的能力。
<!-- DAILY_CHECKIN_2026-08-02_END -->

<!-- DAILY_CHECKIN_2026-08-03_START -->
# 2026-08-03

今天主要完成了 Aave V3 Lending 模块的功能完善与验证，对各项能力进行了测试和检查，确保功能实现符合预期。同时，对项目整体架构和代码流程进行了进一步梳理，加深了对智能合约交互、Receipt 验证机制以及团队协作开发流程的理解，也进一步提升了独立分析问题、实现功能和解决开发过程中实际问题的能力。
<!-- DAILY_CHECKIN_2026-08-03_END -->

<!-- DAILY_CHECKIN_2026-08-05_START -->
# 2026-08-05

今天主要围绕项目开发与团队协作开展学习，进一步梳理了 Aave V3 Lending 模块的整体实现流程，对功能开发、测试验证、Git 分支管理以及代码提交流程有了更深入的理解。通过实践加深了对智能合约交互、模块化开发和团队协作规范的认识，同时提升了独立定位问题、解决问题以及规范管理代码的能力。
<!-- DAILY_CHECKIN_2026-08-05_END -->
<!-- Content_END -->
