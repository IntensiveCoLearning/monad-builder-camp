- GitHub ID: 181855088
- Name: jzhao0
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-26
<!-- DAILY_CHECKIN_2026-07-26_START -->
# 2026-07-26

【2026-07-26｜Monad Builder Camp 今日打卡】

今天完成：

1\. 完成并提交 Week 3 项目研究简报，围绕 Monad DeFi 用户的交易前理解问题，整理了 3 条问题证据、2 类现有解决方案、项目风险和团队建议，并对关键资料进行了人工核验。

2\. 参加 Parallax 团队第一次正式项目会议。会议使用 Zoom，20:00 开始，20:50 左右结束，五名成员全部参加。

3\. 团队确认项目继续围绕 DeFi 交易风险评估推进，并确定 Kuru 与 PancakeSwap 两个协议方向。项目工作名暂定为 Parallax，后续使用 GitHub 协作，并计划每天晚上同步进度。

4\. 共同查看 YO Risk Graph，主要参考其风险信息和依赖关系的界面表达。Antony 展示了当前前端原型，已有钱包地址、交易意图、Risk Policy、分析结论、Pass/Warn/Fail 和 Risk Receipt 等静态界面，但尚未接入真实 Moss Simulation 和后端数据。

5\. 阅读 Clare 分享的 Dependency-Aware Risk Receipt 产品方案和 Parallax PRD v0.1。当前文档仍属于 Review Draft，明天结合 Moss 具体规则继续评审后再决定是否上传 GitHub 和冻结 Scope。

6\. 收到 Moss 开源贡献反馈：

\- PR #133 获得维护者 Approved，尚未合并；

\- PR #134 收到 Changes Requested，维护者肯定主网读取和 ABI provenance 工作，同时提出 7 项类型、CI、ABI 验证、文档和集成测试修改要求。

今天的问题：

\- Zoom 转写语言误设为英文，无法作为会议记录使用；

\- Parallax 仓库尚未创建完成；

\- 前端目前主要是静态或 Mock 原型；

\- 团队尚未锁定 Moss 输入输出、Risk Receipt Schema、任务分工细节和 GitHub 协作规则；

\- [Nad.fun](http://Nad.fun) Adapter PR #134 需要后续根据维护者 Review 系统修复。

明日计划：

\- 学习 Moss 方向的具体规则；

\- 继续评审 Parallax PRD；

\- 确认双协议真实开发标准；

\- 冻结 P0 / P1 / P2；

\- 确认团队角色和第一个可检查交付物；

\- 确认 GitHub 仓库、分支和 PR 规则；

\- 之后再开始正式项目开发。
<!-- DAILY_CHECKIN_2026-07-26_END -->

# 2026-07-25
<!-- DAILY_CHECKIN_2026-07-25_START -->
# 2026-07-25

今天继续推进 Moss Week 2 的 **Protocol Adapter Challenge**，围绕 Monad 主网上的 [Nad.fun](http://Nad.fun) Lens 开发了一个 Query-only Adapter。

已完成 `quoteBuy`、`quoteSell` 和 `tokenStatus` 三个查询能力，并将官方完整 ABI 固定到指定 Git Commit 和 SHA-256，补充了离线确定性生成、上游 ABI 更新脚本、来源记录和主网在线验证。当前包级测试 `7/7` 通过，主网查询测试通过，MCP Server 测试 `11/11` 通过，全仓 Build、Typecheck、Lint 和离线测试也全部通过。

目前还剩 Monadscan Explorer ABI 交叉核验，因为终端尚未配置 `MONADSCAN_API_KEY`，所以没有将这一项伪装成已通过。下一步是完成交叉核验、审计最终 Diff、Commit、Push，并向 Moss 官方仓库提交 PR。
<!-- DAILY_CHECKIN_2026-07-25_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
# 2026-07-24

今天周五，明天开始完成没完成的week任务
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# 2026-07-23

gpt会员到账了，我要开始完成没有完成的任务了
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
# 2026-07-22

onekey 感觉很有意思，我都想买一个冷钱包了
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
# 2026-07-21

人生胶囊比skill有意思，然后colearning是非诚勿扰，全程观战
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->
# 2026-07-20

今天gpt过期了，还在研究续费，但是有个点错了，现在要等明天退款才能继续看能不能订阅了，要是不行就只能等72小时用苹果了，今天晚上听了一些，还在完成week2任务，但是gpt一下子没有了，就很难办
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
# 2026-07-19

```
今日完成：
1. 提交了“认识 Moss”“GitHub 探索日志”“开源贡献计划”3项任务；
2. 完成本地开发环境检查，Node.js、pnpm、Git 均已配置；
3. 将 pnpm 调整为项目指定的 11.10.0；
4. 克隆个人 Moss Fork，并配置官方 upstream；
5. 验证个人 Fork 与官方 main 分支完全同步；
6. 正在执行 pnpm install，安装项目依赖。

今日收获：
进一步理解了 Fork、origin、upstream 和分支同步的基本流程，也完成了 Moss 本地开发环境的初步搭建。

下一步：
等待依赖安装完成，执行 build、typecheck、lint 和离线测试，并尝试跑通 Moss 官方示例。
```
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->
# 2026-07-17

现在在听yoyo分享，先打个卡，怕忘了
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
# 2026-07-16

今天没有开发项目，colearning是玩游戏，谁是卧底
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->
# 2026-07-15

【今日打卡】

今天参加了「Web3 如何从 0 到 1 开始运营」线上课程，系统了解了一个 Web3 项目或活动从策划到复盘的完整运营流程。

我过去对运营的理解主要是内容发布、社群管理和活动宣传。今天课程让我认识到，运营实际承担的是产品、用户、合作方和内部团队之间的连接工作。一个完整流程需要先明确目标和目标用户，再完成 Proposal、合作方对接、活动设计、渠道宣发、用户招募、社群承接、活动执行、项目提交、评审反馈、数据复盘和后续人才或项目跟进。

课程还重点讲解了用户路径。用户可能从第一次看到活动开始，依次完成报名、进入社群、参加 Workshop、开始 Build、组队、提交项目和获得反馈，最终留在生态中继续贡献。运营需要通过多个渠道和多次触达推动用户走完这条路径，而不是只关注最初的报名人数。

我还学习到，运营同样需要可验证的 Proof of Work。宣传浏览量、报名人数、实际到场人数、项目提交数量、用户留存和后续转化，都可以成为衡量运营效果的数据。公开的活动方案、社群 SOP、宣传内容和复盘报告，也可以成为作品集的一部分。

AI 可以辅助完成调研、Proposal、宣传文案、主持稿、评审标准、图片和活动复盘，但最终仍需由人根据目标用户、实际环境和历史经验进行修改和判断。

虽然我选择的是 Dev 主方向、Research 辅助方向，但运营能力对我仍然重要。后续开发 DailyCheckIn 和预测市场研究工具时，我不能只关注代码是否能够运行，还需要思考谁会使用、用户如何完成核心动作、如何获得第一批真实反馈，以及用什么指标证明产品具有实际价值。

今天最重要的收获是：一个 Web3 Demo 的完成标准不仅是“代码写完”，而应该是“产品能够运行、用户能够理解、操作可以完成、结果可以验证、反馈能够被记录”。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
# 2026-07-14

【今日打卡】

今天参加了两场与 Web3 Research、职业方向和开源协作有关的线上课程。

第一场课程的主题是「从研究到公共知识：AI 时代 Web3 Researcher 的成长之路」。我认识到，研究工作的基础不是快速生成内容，而是保证证据真实、来源可追溯、结论能够接受公开核查。AI 可以用于阅读、整理、分析和建立研究框架，但不能代替人类承担真实性责任。在使用 AI 时，需要主动核查原始来源、数据口径、引用和不确定性，避免把 AI 生成内容直接当成事实。

课程还介绍了 EAS、文件 Hash 和链上证明等工具。研究原始数据和完整报告可以保存在链下，而时间、报告 Hash、模型版本、签发者和关键声明可以记录到链上，从而证明某项研究何时存在以及后续是否被修改。这与我后续计划开发的预测市场研究和链上量化工具有直接联系。

第二场 Co-Learning 课程进一步梳理了 Research、Ops 和 Dev 三条职业方向，并说明 Web3 更看重公开、可验证的 Proof of Work。GitHub 仓库、智能合约、Demo、Issue、Pull Request、研究报告和黑客松作品，通常比只有文字描述的简历更能证明能力。

结合自己的金融工程背景、DailyCheckIn 合约、EA 开发和预测市场研究项目，我进一步确认 Week 2 选择 Dev 作为主方向，Research 作为辅助方向。我的定位是使用 AI 辅助完成智能合约、DApp、数据脚本和研究工具，同时由自己负责需求判断、测试验证、风险核查和项目文档。

课程还介绍了 MOSS 开源项目及其贡献方式。我的下一步不是立即让 AI 大量修改开源代码，而是先阅读仓库规则、运行本地示例、理解协议发现和交易模拟流程，再从一个明确的小问题开始尝试 Issue、文档改进或代码贡献。

今天最重要的收获是：Web3 学习不能只停留在听课和与 AI 对话，而要持续把学习结果变成公开、可验证的作品。接下来我会继续完善 DailyCheckIn Web DApp，并逐步探索将预测市场数据分析、AI 研究报告和链上可验证记录结合起来。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
# 2026-07-13

【今日打卡】

今天参加了「Web3 技术如何从 0 到 1 用 AI 开发」线上分享。

课程首先明确了“0 到 1”的含义：并不是一次性做出完整产品，而是先完成一个能够证明核心价值、让目标用户实际体验和判断的 Demo。AI 可以明显降低开发门槛，提高代码生成、缺陷修复和重构效率，但 AI 生成代码速度越快，越需要开发者主动进行需求定义、任务拆分、测试验证和人工审查。

我还学习了 AI Agent 与普通问答模型的区别。Agent 不只是回答问题，而是可以根据目标分析环境、拆解任务、调用工具并执行操作。课程重点介绍了 [AGENTS.md](http://AGENTS.md) 等项目规则文件，它可以记录项目目标、架构边界、编码规范、允许和禁止的操作、测试要求及协作方式，使不同 AI 工具进入项目后能够遵循统一规则。

在开发方法方面，课程介绍了测试驱动开发和规范驱动开发。相比直接让 AI 写代码，更可靠的流程是先明确“为什么做、要做成什么样、如何验收”，再拆分任务、完成最小实现、执行测试并逐步重构。最终结果必须由开发者自己判断和验证，不能因为代码能够运行就默认其逻辑和安全性正确。

课程还梳理了 Web3 技术的五个主要方向：DApp 开发、智能合约开发、钱包开发、节点开发和安全审计。结合我 Week 1 完成的 DailyCheckIn 项目，我已经经历了 Solidity 源码、AI 辅助生成、人工修改、Remix 编译、Monad Testnet 部署、read/write function 调用和区块浏览器验证的完整 0 到 1 流程。

今天最重要的收获是：AI 开发的核心不是让 AI 完全代替自己，而是建立清晰的规则、任务边界和可执行的验收标准，让 AI 在受约束、可观察、可验证的流程中协助完成开发。

下一步，我计划为 DailyCheckIn GitHub 项目补充 [AGENTS.md](http://AGENTS.md)，并在开发前端 DApp 前先定义钱包连接、Monad 网络检测、checkIn() 调用和 checkInCount 查询的验收标准，再使用 AI 辅助实现。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

今天完成了 Week 1 核心产出的最终整理，并提交了第一个轻量级 Mini Demo 0。

我的作品是部署在 Monad Testnet 上的 DailyCheckIn 每日打卡智能合约。我已经将 Solidity 源码、README v0.1、链上部署信息、MonadVision 交互截图和 Remix read function 查询截图整理到公开 GitHub 仓库中。

本周我通过 AI 生成合约初稿，但没有直接照搬代码。我人工检查了业务逻辑、权限、时间判断、变量命名和安全风险，并将 AI 最初的“间隔 24 小时打卡一次”修改为“每个 UTC 自然日打卡一次”，同时增加了 CheckedIn 事件。

在真实链上实践中，我完成了课程钱包连接、Monad Testnet 合约部署、checkIn() 写入调用和 checkInCount(address) 只读查询，并通过 MonadVision 验证了合约地址、部署交易 Hash 和成功交互交易 Hash。

Week 2 我确认选择 Tech 作为主方向，Research 作为辅助能力。下一步计划使用 ethers.js 为 DailyCheckIn 开发一个简单前端，并继续增加连续签到、积分、任务和排行榜功能，将当前合约逐步升级为 Monad Quest DApp。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
# 2026-07-11

今天完成了 Monad Testnet 钱包实操任务。由于之前的课程钱包浏览器扩展密码遗失，并且现有助记词恢复出的地址与旧钱包不一致，我在确认旧钱包仅含测试网资产后重新创建了课程专用钱包，并将新助记词离线备份。

随后，我为新钱包添加了 Monad Testnet，领取了测试币，并向另一个由自己控制的测试钱包地址转账了一小笔 MON。转账完成后，我复制了 Transaction Hash，并在 Monad 测试网区块浏览器中查询了这笔交易。

我在区块浏览器中查看了交易状态、区块高度、发送地址、接收地址、转账金额和 Gas 费用，确认交易状态为 Success。通过这次实操，我理解了钱包负责保管密钥和签名交易，而 Transaction Hash 是链上每笔交易的唯一标识，区块浏览器可以用来公开验证交易执行结果。

Transaction Hash：**0xb7694e86f4d6b4756c874c3e8aaf5b189c8ac423bcf3b280367e41fa2e59d5c0**
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
# 2026-07-10

今日参加了 Web3 实习计划第一周 Co-Learning 例会，主要学习了其他同学在第一周完成钱包、测试网、链上转账、智能合约部署、DApp Demo 和作业复盘过程中的经验。

今天我最大的收获是：Web3 学习不能只停留在概念层面，而要通过真实操作把钱包、交易 Hash、区块浏览器、合约地址、部署交易和前端交互串起来。通过同学的分享，我进一步理解了测试网转账、Remix 编译部署合约、调用读写函数、查看链上记录等基础流程。

本次例会也让我看到，AI 工具可以大幅降低新人进入 Web3 开发的门槛。即使不会完整手写代码，也可以通过 AI 生成合约、前端和部署说明，再结合人工检查、调试和复盘，完成一个可展示的小型 DApp。后续我也会尝试用这种方式推进自己的 Demo。

另外，我还学到了 Web3 求职准备的重要性，包括阅读 JD、关注 HC、准备 CV、了解 Mentor、SOP、Onboarding 等常见职场术语，以及建立固定的信息渠道。Web3 行业信息变化很快，后续我会继续记录学习过程，整理 Build Log，并逐步形成自己的项目作品集。

接下来我会继续补齐智能合约、DApp 交互和 AI 辅助开发基础，尝试把自己的 AI × Web3 项目方向拆成更小的 MVP，并坚持完成每日打卡和复盘。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

今日完成了「AI Agent 高危案例解析与安全构建」线上活动的学习。本次分享主要围绕 AI Agent 的安全风险展开，重点讲解了当 Agent 拥有钱包权限、API 权限、文件访问权限、系统命令权限或插件调用能力后，可能出现的高危场景。

今天我理解到，AI Agent 和普通聊天机器人最大的区别在于，它不仅会生成内容，还可能执行真实操作。因此它的安全风险也不只是“回答错了”，而是可能因为过度授权、提示词注入、恶意插件、恶意 Skill、MCP 工具风险或运行时缺乏防护，造成数据删除、凭证泄露、恶意转账、敏感信息外发等问题。

本场分享让我印象最深的是“最小权限原则”。如果 Agent 只需要整理文档，就不应该给它系统命令权限；如果只是查询信息，就不应该让它接触钱包私钥或 API Key。对于资金转账、邮件删除、链上授权、合约交互等敏感操作，也应该设置二次确认和审计机制。

结合我的 Web3 学习路径，今天的内容让我意识到 AI × Web3 项目不能只关注功能实现，还必须重视安全边界。钱包、私钥、授权、交易签名、API Key 都是高风险对象，后续做项目时应尽量使用课程专用钱包和测试网，避免让 Agent 直接接触主力钱包或真实资产。

接下来我会继续学习 Agent 安全、Web3 钱包安全和链上交互安全，并在自己的项目方向中加入安全设计思路，例如限制 Agent 权限、记录操作日志、让用户确认敏感操作，以及对外部插件和工具进行检查。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

今日完成了「AI Agent 如何拥有支付能力？」线上活动的学习。本次分享主要围绕 Agent Payment 展开，讲解了 AI Agent 在未来执行任务时，为什么需要具备安全、可控、可结算的支付能力。

今天我理解到，AI Agent 不只是帮助用户聊天或生成内容，它还可能在未来替用户完成更复杂的任务，例如调用 API、购买数据、订票、访问付费工具、调用其他 Agent 的服务等。这些操作背后都会涉及支付问题。传统支付体系在 Agent 场景下存在身份、手续费、结算速度、敏感信息安全和自动化程度不足等问题，因此 Web3、稳定币和链上结算可能成为一种新的解决方向。

本场分享让我印象最深的是：涉及钱的 Agent 必须被约束，不能让 AI 无限制地花钱。因此，一个完整的 Agent 支付系统需要包括身份识别、预算限制、风控策略、交易审计和可撤销机制。这个逻辑和金融工程中的风险控制也有一定联系。

结合我的学习方向，我后续会继续关注 AI × Web3、Agent 支付、链上数据分析和金融场景应用。我也会思考如何把今天学到的内容和后续项目结合起来，例如在原本的 AI 辅助链上交易解释器基础上，加入对支付行为、预算控制和交易原因的解释，让项目更贴近真实 Web3 应用场景。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

今天完成了 Web3 实习计划第一阶段的基础任务。首先，我创建并准备了一个课程专用钱包，没有使用自己的主力钱包，以降低后续测试和交互过程中的资产安全风险。随后，我按照任务要求添加了 Monad Testnet 网络，并打开 Monad Explorer / 区块浏览器，确认可以正常查询自己的钱包地址。

通过这次操作，我初步理解了链上产品和普通互联网产品的区别。普通互联网产品主要依赖账号体系、平台数据库和中心化服务器；而链上产品通常通过钱包连接，资产、交易和合约交互会记录在区块链上，可以被公开查询和验证。链上产品更强调数据透明、资产归属和智能合约执行规则，但同时也对钱包安全、Gas、网络配置和用户理解门槛提出了更高要求。

今天的任务让我完成了从“只听概念”到“实际连接链上网络”的第一步，后续会继续学习钱包、交易 Hash、Gas、区块浏览器和智能合约交互等基础内容。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
# 2026-07-06

今天参加了 Web3 实习计划分享会，主要学习了 Web3 新人的成长路径、智能合约入门、AI Coding、DevRel、黑客松和实习机会相关内容。

今天的核心收获是：Web3 不只是炒币，也不只是单纯写代码，而是包含智能合约、DApp、前端、DevRel、运营、研究、治理和生态协作等多个方向。对于技术新人来说，可以先从智能合约入手，因为它是理解链上应用和 DApp 的基础。

我也认识到，AI Coding 可以作为重要的开发辅助工具。后续做项目时，不只是让 AI 写代码，更重要的是自己能提出需求、拆解任务、看懂代码、review 逻辑，并能清楚说明项目做了什么、自己负责了什么。

结合自己的金融工程背景、Python 基础和 AI 工具使用经验，我目前更倾向于从 AI × Web3、链上数据分析、智能合约基础和 Web3 Research 方向切入。接下来会继续学习钱包、交易 Hash、Gas、智能合约和 DApp 交互等基础内容，并持续记录 Build Log，为后续 Hackathon、作品集和实习机会做准备。
<!-- DAILY_CHECKIN_2026-07-06_END -->

<!-- DAILY_CHECKIN_2026-07-28_START -->
# 2026-07-28

日期：
2026 年 7 月 28 日

今日完成事项：
跟进 Moss 开源项目 PR #133 的最终状态。

PR 信息：

* PR 标题：docs: add toolchain checks to getting started
* PR 编号：#133
* 上游仓库：nishuzumi/moss
* 当前状态：已合并至主分支

完成过程：
此前我向 Moss 项目的 Getting Started 文档补充了工具链检查相关说明，并同步维护了英文与中文文档。该 PR 经维护者审查后获得 Approved。

今天维护者在合并前补充了一个文档排序提交 `468fbc7`，用于调整工具链检查部分中 Yarn 内容的位置。随后，PR #133 被正式合并至上游主分支。

今日结果：
本次文档贡献已完成完整的开源协作流程：

提交修改
→ 创建 Pull Request
→ 接受 Maintainer Review
→ 获得 Approval
→ 维护者补充调整
→ 正式 Merge

个人收获：

1. 完成了第一次被上游正式合并的 Moss 开源贡献；
2. 进一步熟悉了 Fork、分支、Commit、Pull Request、Review 和 Merge 的完整流程；
3. 理解了开源贡献不仅要求内容正确，还需要保持多语言文档同步、遵守仓库格式，并接受维护者对内容顺序和表达方式的调整；
4. 维护者补充的提交属于正常协作过程，最终成果由贡献者和维护者共同完善。

证据：

* Moss PR #133 链接
* GitHub 页面显示 Merged 的截图
* 维护者 Approval 邮件
* “将 #133 并入主线”的 GitHub 通知邮件
* 维护者补充提交 `468fbc7`

下一步：
继续等待 PR #134 的最终 Review，同时保存 PR #133 的合并证据，用于后续课程提交、作品集和简历中的开源贡献记录。
<!-- DAILY_CHECKIN_2026-07-28_END -->

<!-- DAILY_CHECKIN_2026-07-29_START -->
# 2026-07-29

日期：
2026 年 7 月 29 日

今日主题：
Parallax 第二次团队会议与 P0 Scope Freeze

今日完成：

1. 参加 Parallax 第二次团队会议

今晚团队通过 Zoom 召开了第二次会议，Kai、Jie、Clare、Antony 和 Rei 五名成员全部参加。

会议围绕 Parallax PRD v0.2 进行逐项确认，主要目标是停止继续扩大产品范围，冻结目标用户、核心问题、P0 流程、技术边界和成员职责，为后续正式开发做准备。

1. 确认产品定位

团队确认 Parallax 是一个由 Moss 驱动的 Monad Swap 交易决策层。

核心 Demo 表达为：

“Moss tells us what will happen. Parallax tells the user what to do next.”

产品核心从 Risk Report 和风险总分收缩为 Transaction Decision，帮助用户决定：

* PROCEED；
* ADJUST；
* STOP；
* UNKNOWN。

INTEGRATION\_ERROR、UNAVAILABLE 和 TIMEOUT 等技术问题将与协议风险判断分开。

1. 确认目标用户与核心问题

Primary User 确认为：

在 Monad 上进行基础 Swap、理解钱包基本操作，但无法判断失败原因和正确调整方式的轻度 DeFi 用户。

明确排除完全零基础用户、专业 Trader、机构用户和 Agent Builder 作为当前消费者产品主用户。

Primary Problem 确认为：

用户看到 Swap Failed、Simulation Failed、No Route 或 Revert 后，无法判断应该调整 Slippage、Priority Fee、Amount、Token Pair 还是 Route，容易进行无效或危险的重复试错。

1. 冻结 P0 产品范围

团队确认首个协议为 Kuru，首个 Token 范围为 MON 与 USDC。

P0 核心闭环为：

Swap Input
→ Check before signing
→ Verdict
→ Reason and Action
→ Adjust
→ Re-run
→ Previous vs New Diff
→ Evidence Drawer

当前不同时开发 PancakeSwap 完整 E2E，PancakeSwap 暂时作为 Stretch、技术闸门或 UNAVAILABLE。

P0 不自研智能合约，不签名、不广播、不托管资产，也不开发浏览器插件、聊天式 Agent、复杂 Policy、Session History 和完整协议评级。

1. 确认规则边界

第一版只保留三组确定性规则：

* Execution；
* Economic Boundary；
* Evidence Completeness。

Rule Engine 必须使用确定性纯函数，LLM 不参与最终风险裁决。

团队同时确认：

* Simulation Success 不自动等于 PROCEED；
* Zero Warning 不等于经济结果可接受；
* UNKNOWN 不等于 PASS；
* Integration Error 不得聚合为协议 FAIL；
* 原生 MON 场景没有 Approval 时返回 NOT\_APPLICABLE。

1. 明确个人职责

我在团队中正式负责：

* 锁定 Moss 版本；
* 验证 Kuru MON / USDC 双向兼容性；
* Kuru 真实 E2E；
* Moss Bridge；
* Raw Evidence 保存；
* Evidence Normalization；
* Error Normalization；
* BigInt 转 string；
* Rule Engine；
* Verdict 聚合；
* Fixtures；
* Smoke Test；
* 与 Rei 对接 Evidence → Rule → Action；
* 与 Clare 对接 Moss Output → API Contract。

当前第一项开发任务为：

Parallax Day 4 — Kuru Moss Compatibility and E2E Evidence Spike。

1. 建立 GitHub 协作基础

团队已经建立 GitHub Organization：

parallax-monad

组织内已有5名团队成员，并创建公共仓库：

parallax

仓库基础框架预计于 7月30日完成。在框架完成前，我只进行本地技术验证，不把个人代码提前上传或描述为团队正式集成。

今日结果：

* 第二次团队会议完成；
* 五人团队与角色确认；
* PRD v0.2 确认；
* Primary User 和 Primary Problem 确认；
* P0 Scope Freeze 完成；
* Kuru 主协议确认；
* PancakeSwap 降为 Stretch；
* 暂不自研智能合约；
* GitHub Organization 和公共仓库建立；
* Jie 的第一开发任务明确。

今日证据：

* Zoom 五人参会截图；
* Primary User 和 Trigger Moment 勾选截图；
* 团队群聊确认 PRD 截图；
* GitHub Organization 五人成员截图；
* GitHub parallax 公共仓库截图；
* PRD v0.2 文件。

下一步：

1. 等待共享仓库基础框架完成；
2. 本地选择并锁定 Moss 版本或 Commit；
3. 验证 Kuru MON → USDC 与 USDC → MON 双向支持；
4. 完成 Quote、Action、Simulation 和 Receipt Smoke Test；
5. 保存真实 Raw Moss Evidence；
6. 开始 Evidence Normalization 和 Rule Engine Skeleton；
7. 保持本地验证与团队正式集成状态分离。
<!-- DAILY_CHECKIN_2026-07-29_END -->

<!-- DAILY_CHECKIN_2026-07-30_START -->
# 2026-07-30

日期：
2026 年 7 月 30 日

今日主题：
完成 Parallax 第一阶段 Kuru Evidence Baseline，并提交团队 Draft PR

今日完成事项：

一、完成 Parallax 第一阶段技术交付

今天完成了 Parallax 的第一阶段任务：

Kuru Baseline Evidence Pipeline and ERC-20 Simulation Boundary。

该阶段的目标是先建立一套不依赖完整实时 Moss Runtime 的可移植 Evidence、规则和 Replay 基础，并明确当前 ERC-20 Simulation 的真实技术边界。

二、完成团队仓库第一份功能分支和 Draft PR

功能分支：

feat/kuru-baseline-evidence

Draft PR：

<https://github.com/parallax-monad/parallax/pull/1>

PR 标题：

feat(moss): add Kuru baseline evidence pipeline

当前状态：

* Open；
* Draft；
* Mergeable；
* 已请求 Clare 和 Rei Review；
* 尚未收到正式 Review；
* 尚未合并；
* 当前没有 GitHub Actions Workflow 检查结果。

三、完成四个 Commit

1. a42e329
   feat(moss): add Kuru evidence normalization

2. 0f74da2
   feat(risk): add deterministic execution decisions

3. 773096c
   test(moss): add Kuru baseline fixtures and replay checks

4. 0df65fc
   docs(moss): record ERC-20 simulation boundary

四、完成 Evidence Pipeline 和 Risk Engine 基础

本阶段已经实现：

* Portable Evidence Normalization；
* Raw Evidence 与 Normalized Evidence 边界；
* Evidence Source Provenance；
* Replay 标记；
* Recursive BigInt-safe Serialization；
* Moss Error Classification；
* Integration Status 与用户风险 Verdict 隔离；
* 确定性纯函数 Risk Engine；
* Real Fixtures；
* Mock Fixtures；
* Replay Tests；
* Kuru Smoke Gate；
* ERC-20 Simulation Boundary ADR。

当前代码不会把 Integration Error 包装成协议风险，也不会把 Replay 或 Mock 冒充为实时链上证据。

五、完成本地测试

实际验证结果：

* pnpm lint：通过；
* pnpm typecheck：通过；
* pnpm test：通过；
* 共 3 个测试文件；
* 共 11 个测试通过；
* pnpm build：未执行，因为当前仓库没有 build script；
* pnpm smoke:kuru：按设计返回 UNAVAILABLE，没有伪造实时成功。

Git 操作过程中：

* 未 Force Push；
* 未直接 Push main；
* 未提交私钥、助记词或 Secret；
* 未提交真实 .env；
* 未添加 AI 工具身份或 Co-author 标记。

六、记录 Kuru 当前真实技术状态

当前已经验证：

MON → USDC：

* Capability：已验证；
* Quote：已验证；
* Action：已验证；
* Simulation：部分验证；
* Receipt：仍受 FlipOrderUpdated Parser 缺口影响。

USDC → MON：

* Capability：已验证；
* Quote：已验证；
* Action：已验证；
* Approval Action：已验证；
* Simulation：部分验证并发生 Revert；
* Revert 根因尚未完全证明。

当前不能把 USDC → MON 的 Revert 直接解释为：

* Kuru 不支持双向；
* Moss 不支持双向；
* 协议风险；
* 用户余额不足；
* Allowance 不足。

目前只能确认余额和授权条件尚未被完整证明。

七、明确 ERC-20 Simulation 边界

当前稳定 Moss Simulator 只通过 Native Prefund 为 Sender 提供 MON。

稳定版本目前没有公开的 ERC-20 Synthetic Balance 和 Allowance 注入接口。

因此当前 P0 决定为：

* MON → USDC 作为 Baseline；
* 保留 USDC → MON 的双向构造证据；
* USDC → MON 完整 Simulation 不阻塞第一阶段；
* 不依赖随机第三方地址作为稳定 Demo；
* 不依赖尚未合并的 State Overrides；
* 不把未验证的技术路径写成已完成。

八、参加团队会议并审核阶段计划

今晚团队继续召开会议，完成：

1. 共同审核 PRD；
2. 各成员汇报进度；
3. 汇报阶段目标和时间节点；
4. 讨论 Moss、Risk、API 和团队协作边界；
5. 审核 Stage 1—Stage 7 的阶段规划。

团队确认当前不应继续无边界扩张开发。

我当前进入等待状态，等待：

* PR #1 Review；
* Clare 对 Moss Output → API Contract 的意见；
* Rei 对 Evidence → Rule → Action 的意见；
* Portable Moss Dependency 的团队决策；
* Shared Contract v0.1 的协作冻结。

今日结果：

* Parallax 团队仓库基础框架已建立；
* 第一阶段技术工作完成；
* Draft PR #1 已提交；
* 4 个 Commit 已进入功能分支；
* 11 个测试通过；
* Evidence Normalization、Risk Engine、Fixtures 和 ADR 已形成；
* Kuru MON → USDC Baseline 技术状态已记录；
* ERC-20 Simulation 边界已明确；
* 团队阶段计划已审核；
* 当前进入 Review 和接口边界确认阶段。

当前准确状态：

第一阶段：
已完成并提交 Draft PR。

第二阶段：
尚未正式开始。

当前不是继续自由开发，而是等待 Review、Shared Contract 和协作边界进一步明确。

下一步进入条件：

1. PR #1 收到正式 Review；
2. Node 22 环境复验完成；
3. Clare、Rei、Jie 的接口边界明确；
4. Shared Contract v0.1 可供团队 Review；
5. Portable Moss Dependency 形成明确决议。

在以上条件未满足前，不继续扩大 Schema、API、规则、PancakeSwap 或智能合约范围。
<!-- DAILY_CHECKIN_2026-07-30_END -->

<!-- DAILY_CHECKIN_2026-07-31_START -->
# 2026-07-31

日期：
2026 年 7 月 31 日

今日主题：
推进 Parallax PR #1 收敛，并明确 PR #3、PR #4 的 Review 与接口依赖

今日完成事项：

一、继续完善 Kuru Evidence Pipeline

今天针对 Parallax 团队仓库 PR #1 的首轮技术 Review 意见进行了修复。

PR：
<https://github.com/parallax-monad/parallax/pull/1>

分支：
feat/kuru-baseline-evidence

最新 Head：
9d413b95d76655292c11efd40b1e318f7f96ed6c

当前 PR 状态：

* Open；
* 非 Draft；
* Mergeable；
* Changes Requested；
* Merge State 为 Blocked；
* Node 22 CI 已通过；
* 尚未合并。

二、处理 PR #1 的五项技术 Blocker

本轮主要完成：

1. 保留结构化 Moss Error：
   * stage；
   * code；
   * message；
   * integrationStatus；
   * source。

2. 对不可信关键 Evidence 执行 Fail Closed：
   * source=unknown；
   * 不可复现；
   * mock 支撑核心 Verdict。

3. 未解释的关键 Asset Changes 不再静默通过，而是返回 UNKNOWN。

4. Simulation Result 必须与 Action Transaction 完整匹配：
   * from；
   * to；
   * data；
   * value；
   * 一对一匹配；
   * 缺少字段不得错误匹配。

5. Minimum Received 必须记录来源：
   * original\_swap；
   * user\_declared；
   * demo\_preset；
   * unavailable。

三、完成三个修复 Commit

* 965611af
  fix(moss): preserve structured errors and require full transaction identity matching

* fc91277c
  fix(risk): fail closed on untrusted critical evidence and unexplained asset changes

* 9d413b95
  fix(risk): record economic boundary provenance and regenerate fixtures

四、完成最新验证

实际验证结果：

* pnpm install：通过；
* pnpm lint：通过；
* pnpm typecheck：通过；
* pnpm test：通过；
* 3 个测试文件；
* 41 个测试全部通过；
* Node 22 GitHub CI：通过；
* pnpm smoke:kuru：按设计返回 UNAVAILABLE。

当前没有把 Live Runtime 的不可用状态伪装成成功，也没有把 Replay 或 Mock 写成实时链上 Evidence。

五、明确团队 Review 边界

团队对交叉 Review 进行了重新分工：

* Kai 不再重复 Review Jie 的底层 Moss、Normalizer 和 Risk Engine 实现；
* Kai 后续使用 Jie 的 Evidence Chain 与 Rei 的 Rule Spec形成初步用户语义；
* Clare继续负责 PR #1 技术复核；
* Jie负责 PR #3 的 Moss / Evidence 可实现性 Review；
* Rei继续负责 RuleResult、Central Verdict Policy 和 Reason-to-Action；
* Antony负责消费最终 API 和 Shared Contract。

这样可以减少重复 Review 和相互冲突的修改意见。

六、同步 PR #3 状态

PR #3：
docs(risk): propose P0 rule and reason-to-action spec

当前状态：

* Open；
* Draft；
* Mergeable；
* CI 通过；
* Product Semantics 已获得 Approval；
* Jie 的 Moss / Evidence 可实现性 Review 尚待完成。

PR #3 目前是 docs-only、non-normative 的方法论和实现目标，不代表完整 Runtime 已实现。

我的 Review 范围只包括：

* Moss Evidence 可实现性；
* Stage-aware Evidence；
* NO\_ROUTE Classification Gate；
* Mock Fixture 限制；
* Classification Gate 与 Action Gate 分离；
* Economic Boundary Runtime 依赖；
* Evidence Provenance；
* Integration Error Mapping；
* Asset Changes 和 Warnings；
* Live / Replay / Mock；
* PR #4 Shared Contract 依赖。

七、同步 PR #4 状态

PR #4：
feat(contracts): define P0 swap contracts and trusted API boundary

当前状态：

* Open；
* 非 Draft；
* Mergeable；
* Changes Requested；
* CI 通过；
* Antony 已从前端范围 Approve；
* Rei 提出的 Contract Blocker 仍待 Clare修改。

PR #4 当前等待 PR #3 的规则语义冻结，不应提前基于未冻结 Contract 开发 Adapter。

八、确认当前技术边界

当前 Portable Live Moss Runtime 仍为 UNAVAILABLE。

MON → USDC：

* 使用真实录制 Moss Evidence；
* Receipt 中存在 FlipOrderUpdated；
* 当前基线 Parser 尚未支持；
* Execution Status 保持 UNKNOWN。

USDC → MON：

* Approval 与 Swap Action 已生成；
* Simulation 在泛化 Revert 后停止；
* 不推断余额不足；
* 不推断 Allowance 不足；
* Execution Status 保持 UNKNOWN。

当前 NO\_ROUTE Fixture 仍属于 Mock Rule Test Input，只能验证 Contract、Normalizer 和 Risk Rule 路径，不能证明真实 Kuru Runtime 会稳定返回 NO\_ROUTE。

今日结果：

* PR #1 五项 Review Blocker 已完成针对性修复；
* 测试数量增加到 41 个并全部通过；
* Node 22 CI 通过；
* PR #1 已从 Draft 转为非 Draft；
* 团队 Review 职责进一步收敛；
* PR #3 的技术 Review 范围已经明确；
* PR #4 确认继续等待规则和 Contract 冻结；
* 没有扩大到 PancakeSwap、智能合约、签名、广播或真实资金。

当前准确状态：

PR #1：
等待读取 Clare 最新 GitHub Comment，并判断是否存在新 Blocker。

PR #3：
等待 Jie 完成 Moss / Evidence 可实现性 Review。

PR #4：
等待 PR #3 收敛后，由 Clare处理 Contract Blocker。

下一步：

1. 读取 Clare 在 PR #1 的最新 Comment；
2. 判断该 Comment 是新 Blocker、非阻塞建议、澄清还是通过确认；
3. 如果存在明确 Blocker，只做针对性修复；
4. 完成 PR #3 的 Moss / Evidence 可实现性 Review；
5. 不重复 Kai 的产品 Review；
6. 不在 PR #1、PR #3 和 PR #4 收敛前启动新的 Adapter 开发；
7. 不 Merge，直到 CI、Code Owner Review、Conversation Resolution 和 Branch Protection 全部满足。

Git 与安全记录：

* 未 Force Push；
* 未直接 Push main；
* 未绕过 Branch Protection；
* 未使用管理员权限强制 Merge；
* 未提交私钥、助记词、Token、Cookie、RPC Secret 或 .env；
* 未在 Branch、Commit、PR、代码或文档中添加 AI 工具身份标识。
<!-- DAILY_CHECKIN_2026-07-31_END -->
<!-- Content_END -->
