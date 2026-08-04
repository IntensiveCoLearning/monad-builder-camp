- GitHub ID: 123364309
- Name: linkmedcs
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-28
<!-- DAILY_CHECKIN_2026-07-28_START -->
**Monad Farm**

Fully On-chain Farming Game

**MVP核心流程**

连接钱包

      │

      ▼

Mint 土地NFT

      │

      ▼

购买种子

      │

      ▼

播种

      │

      ▼

等待成熟（链上时间）

      │

      ▼

浇水（可选，提高品质）

      │

      ▼

收获作物NFT

      │

      ▼

出售到Marketplace

      │

      ▼

获得游戏Token

      │

      ▼

继续升级土地

整个流程只有这一个循环。

**MVP包含哪些模块**

**① Wallet**

支持

-   MetaMask
    
-   Rabby
    
-   OKX Wallet
    

登录即钱包地址。

**② Farm NFT（土地）**

每个人只能拥有一块初始土地。

土地NFT记录：

tokenId

owner

level

unlockSlots

exp

例如：

Farm #18

Level 1

5 plots

EXP 32

升级：

Lv1

5格

↓

Lv2

8格

↓

Lv3

12格

**③ Seed Shop**

出售：

🌽 Corn

🥕 Carrot

🍅 Tomato

每种：

价格

成长时间

售价

经验值

例如：

| 作物 | 成长 | 成本 | 售价 |
| 胡萝卜 | 5 min | 5 | 8 |
| 玉米 | 10 min | 10 | 16 |
| 番茄 | 20 min | 20 | 35 |

**④ Plant**

点击土地：

Plant Seed

链上记录：

CropType

PlantTime

WaterCount

Harvested

**⑤ Water**

成熟前：

每天（或者每小时）

可以浇一次。

作用：

品质+

经验+

产量+

不是必须。

**⑥ Harvest**

成熟以后：

Harvest

获得：

Corn NFT

NFT Metadata：

Crop

Quality

HarvestTime

FarmID

例如：

Tomato

Rare

Lv2 Farm

**⑦ Marketplace**

出售：

成熟作物NFT。

别人可以买。

以后：

可以合成。

**⑧ Game Token**

游戏Token：

FARM

用途：

购买种子

升级土地

未来买工具

黑客松：

可以简单ERC20。

**NFT设计**

整个游戏：

Farm NFT

ERC721

↓

Crop NFT

ERC721

↓

未来：

Tool NFT

Animal NFT

Decoration NFT

**合约架构**

FarmNFT.sol

负责土地。

CropNFT.sol

负责Mint作物。

FarmGame.sol

负责：

Plant

Water

Harvest

Upgrade

Marketplace.sol

负责交易。

FarmToken.sol

ERC20。

**前端页面**

**首页**

Connect Wallet

↓

Enter Farm

**我的农场**

显示：

□□□□□

🌱

🌽

🥕

🍅

点击：

出现：

Plant

Water

Harvest

**商店**

Corn

Buy

Carrot

Buy

Tomato

Buy

**Marketplace**

展示：

Rare Tomato

10 FARM

Epic Corn

25 FARM

**我的NFT**

Farm NFT

Crop NFT

全部展示。

**预留AI接口（先不实现）**

MVP中保留几个字段：

farmMood

cropHealth

weatherId

以后：

AI读取这些。

例如：

AI Farmer

↓

今天建议：

种玉米。

或者：

AI NPC

↓

天气不好。

今天成长速度-20%

这些全部不用修改核心合约。

**黑客松演示（Demo）**

1.  连接钱包。
    
2.  Mint 一块土地 NFT。
    
3.  购买番茄种子并播种。
    
4.  演示浇水，提高品质。
    
5.  时间快进（测试环境可缩短成熟时间）。
    
6.  收获一株番茄 NFT。
    
7.  在 Marketplace 挂售并完成购买。
    
8.  获得 FARM Token，用于升级土地，解锁更多种植格。
    

**MVP 的设计原则**

-   **简单但完整**：完成“种植 → 成长 → 收获 → 交易 → 再投资”的核心循环。
    
-   **链上资产真实可拥有**：土地和作物均为 NFT，可自由交易。
    
-   **易于扩展**：后续可加入工具、宠物、天气、任务、社交以及 AI 助手，而无需重构整体架构。
    
-   **突出 Monad 优势**：频繁的链上交互（播种、浇水、收获、交易）能够很好地展示 Monad 的高性能和低延迟。
<!-- DAILY_CHECKIN_2026-07-28_END -->

# 2026-07-26
<!-- DAILY_CHECKIN_2026-07-26_START -->
# 2026-07-26

\# 关于 Aptos 和 Sui 公链的一些看法

Aptos 和 Sui 都源于 Meta（原 Facebook）Diem 项目的技术积累，并采用 Move 智能合约语言，在安全性、资源管理和并行执行方面具有明显优势。两条公链的发展路径却有所不同：Aptos 更注重生态的稳定扩展和开发者体验，逐步完善 DeFi、钱包、基础设施等应用；Sui 则充分利用对象（Object）模型和并行执行能力，在链游、NFT、社交和高频交互等场景展现出更高的性能潜力。对于开发者而言，Move 的学习成本虽然高于 Solidity，但其更严格的资源管理机制能够降低许多常见的智能合约安全风险。从长期来看，决定一条公链竞争力的不仅是 TPS 或融资规模，更重要的是是否能够持续吸引开发者、沉淀优质应用并形成健康的生态循环。对于普通参与者，与其关注短期市场情绪，不如持续学习 Move 语言、理解底层架构，并关注真正具备产品价值和用户增长能力的项目，因为只有技术创新最终转化为实际应用，公链生态才能拥有更长久的生命力。
<!-- DAILY_CHECKIN_2026-07-26_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
# 2026-07-24

\# 关于符文与铭文的思考

铭文（Ordinals）和符文（Runes）都诞生于比特币生态，但两者的设计目标并不相同。铭文更强调将数据写入单个聪（Satoshi），赋予其独特属性，因此更适合 NFT、数字藏品等非同质化资产；而符文则更加专注于同质化代币的发行与流通，通过更加简洁的 UTXO 模型管理资产，减少链上冗余数据，更符合比特币原生架构的设计理念。从生态发展的角度来看，两者并不是简单的替代关系，而是面向不同场景的技术方案。铭文推动了比特币生态的创新，让更多开发者和用户关注链上资产，而符文则进一步优化了同质化资产的发行效率，为未来 DeFi、交易和支付等应用提供了更好的基础。对于开发者来说，比起讨论谁会取代谁，更值得关注的是底层协议设计、开发工具和基础设施的成熟程度，因为真正决定生态长期发展的，始终是技术能力、用户需求和应用场景的结合。
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# 2026-07-23

\# 关于 BTC 铭文的一些思考

BTC 铭文的出现，让比特币生态从单纯的价值存储逐渐扩展到资产发行和链上应用，也为开发者和投资者带来了新的机会。经历了市场的快速发展后，铭文赛道已经从单纯追逐热点，逐步回归到价值和应用本身。未来真正具有生命力的项目，不仅需要社区共识和流动性支持，更需要实际的使用场景、持续的开发能力以及完善的生态建设。对于参与者而言，与其盲目追逐短期热点，不如关注底层协议的发展、基础设施的完善以及生态创新方向，理解资产发行机制、链上数据结构和市场运行逻辑。长期来看，只有那些能够创造真实价值、持续吸引开发者和用户参与的铭文生态，才更有可能在下一轮行业发展中脱颖而出。
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
# 2026-07-22

\# 合约安全学习计划

合约安全的学习目标不仅是掌握漏洞类型，更重要的是培养安全思维。计划从基础开始，系统学习 Solidity、EVM 执行机制、存储布局、调用流程以及 Gas 原理，理解智能合约运行的底层逻辑。随后深入研究常见安全问题，包括重入攻击、权限控制、签名验证、代理合约、预言机操纵、闪电贷攻击、随机数缺陷、DoS、价格操纵等，并通过复现真实攻击案例理解漏洞产生的根本原因。在实践阶段，坚持阅读优秀开源协议源码和专业审计报告，结合 Foundry、Slither、Mythril、Echidna 等工具完成静态分析、模糊测试和单元测试，逐步形成规范的审计流程。同时积极参与 CTF、安全靶场和 Bug Bounty 项目，在真实场景中锻炼漏洞挖掘与风险分析能力。希望通过持续学习与实践，建立完整的合约安全知识体系，最终具备独立完成智能合约审计、安全评估和漏洞修复的能力。
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
# 2026-07-21

\# 精通智能合约的学习计划

想要真正精通智能合约开发，不能只停留在会写 Solidity，而是要建立完整的知识体系。第一阶段重点夯实基础，深入学习 Solidity、EVM 底层原理、Gas 优化、存储布局以及常见设计模式，做到理解每一行代码在链上的执行过程。第二阶段开始阅读优秀开源协议源码，如 OpenZeppelin、Uniswap、Aave 等，学习成熟项目的架构设计与工程实践。第三阶段聚焦安全领域，系统掌握重入攻击、整数溢出、权限控制、闪电贷攻击、预言机操纵等常见漏洞，并结合 Ethernaut、Damn Vulnerable DeFi、Capture the Ether 等靶场进行实战训练。最后，将开发与审计结合，坚持阅读真实审计报告、复现历史攻击事件，并尝试独立完成合约审计和测试。长期坚持下来，不仅能够提升开发能力，也能够培养安全思维，最终具备设计、开发、测试和审计复杂智能合约系统的综合能力。
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->
# 2026-07-20

\# AI 时代 Web3 合约审计方向的思考

随着 AI 编码能力不断提升，智能合约的开发门槛正在快速降低，未来链上部署的合约数量只会越来越多，这也意味着对合约安全的需求将持续增长。对于 Web3 开发者而言，向合约审计方向转型或许是一个值得长期投入的选择。AI 可以帮助发现常见漏洞、生成测试用例、分析代码逻辑，甚至完成初步审计，但复杂的业务逻辑、经济模型设计、权限管理以及跨协议交互等问题，仍然需要审计人员结合经验进行深入分析。未来优秀的审计工程师不仅要掌握 Solidity、EVM 原理和常见攻击手法，还需要学会利用 AI 提高审计效率，将静态分析、模糊测试、符号执行与大模型结合，构建更加智能的审计流程。AI 不会削弱合约审计的价值，反而会推动审计从“找漏洞”逐步升级为“保障协议安全”的综合能力，这也将成为 Web3 长期具有竞争力的发展方向之一。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
# 2026-07-19

币圈的牛熊交替本质上是一场流动性与人性的能量转化：\*\*熊市是财富的“孕育期”，而牛市是泡沫的“变现期”。\*\* 辩证来看，熊市表面冰冷、资产不断缩水，实则是泡沫出清、价值回归的黄金深耕期，此时风险被价格的连跌不断释放，反而是以极低成本积攒优质筹码、用认知换空间的最佳左侧布局时机，其潜在收益最高，唯一的陷阱是“死在黎明前的黑暗”；相反，牛市表面狂热、资产日新月异，实则是情绪溢价对未来价值的严重透支，此时高收益的背后正积聚着崩盘的极大风险，其核心机会在于顺应趋势、拥抱泡沫以实现资产跨越，而最大的考验则是克服贪婪、在狂欢中分批止盈。因此，真正的高手无一不在熊市中保持“牛市思维”去审视估值洼地，在牛市中保持“熊市思维”去防范流动性枯竭——在荒凉中寻找珍珠，在繁华中卖掉泡沫，唯有这种逆周期的动态平衡，才能将市场的周期波动转化为自身资产的复利增长。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
# 2026-07-18

TON 生态的发展仍处于快速迭代阶段，赚钱机会更多来自于信息差、执行力和持续参与，而不是单纯依靠运气。除了关注优质项目的空投和早期激励外，更重要的是建立自己的研究框架，包括观察资金流向、链上活跃度、用户增长、Telegram 社区热度以及项目的真实产品价值。对于链上交互、质押、流动性提供等玩法，需要综合评估收益与风险，避免为了追求高 APR 而承担过大的资产损失。同时，要养成记录每次参与成本、收益和复盘的习惯，不断优化自己的策略。长期来看，真正能够持续盈利的人，往往不是追逐每一个热点，而是在风险可控的前提下，持续积累对生态的理解，并抓住少数高确定性的机会。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->
# 2026-07-17

\# Solana 套利学习笔记

最近在学习 Solana 链上的套利逻辑。由于 Solana 具有高吞吐、低 Gas 和快速确认的特点，DEX（如 Raydium、Orca、Meteora 等）之间经常会因为流动性分散而出现短暂的价格差。套利的核心就是利用这些价差，在同一区块或极短时间内完成买入和卖出，从中获取利润。不过，真正稳定盈利并不容易，需要实时监控多个交易池价格、计算滑点和交易费用，并结合优先费（Priority Fee）、Jito Bundle 等方式提升交易成功率。同时还要考虑 MEV、抢跑、网络拥堵以及失败交易带来的成本，因此套利本质上是速度、算法和基础设施的综合竞争，而不仅仅是发现价差。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
# 2026-07-16

币圈交易，本质上是一场\*\*披着科技外衣的概率博弈与人性修行\*\*。在这个噪音喧嚣的市场里，认知决定了资产的上限，唯有将逻辑锚定在链上数据与底层价值中，才能在不确定性中看清趋势；交易不求完美，求的是“高盈亏比”的非对称机会，而最终决定生死的，往往是克制FOMO、知行合一的铁律。在加密世界里，暴富只是概率的幸存者偏差，\*\*管好风险、守住本金、活得足够久，才是唯一的复利引擎\*\*。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->
# 2026-07-15

以太坊与 Solana 的未来之争，本质上是\*\*“模块化安全哲学”\*\*与\*\*“单体化极致性能”\*\*的路线决战。以太坊凭借无与伦比的去中心化和安全性，正通过 Layer 2 演变为全球高价值资产和终极结算的\*\*“数字瑞士银行”\*\*，尽管目前仍受困于生态割裂与体验复杂；而 Solana 则以极致的低成本和亚秒级响应，将所有交互统一在单一链条上，成为承载高频交易、DePIN 和消费级应用的\*\*“数字纳斯达克”\*\*。未来的 Web3 并非非黑即白的零和博弈，而大概率是两者的分工共存——\*\*以太坊负责沉淀价值，Solana 负责释放效率\*\*。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
# 2026-07-14

**AI Agent 与以太坊套利的结合，正在将链上博弈从“硬编码的速度内卷”推向“认知与策略的降维打击”**。不同于传统套利机器人（Bots）只能死板地执行预设算法，具备自主决策能力的 AI Agent 能够实时跨多模态（社交媒体、巨鲸动态、跨链协议）进行关联分析，在价差形成前进行预判性布局，并自主规划人类难以洞察的非线性、跨协议隐性套利路径。在“意图导向（Intent-centric）”架构下，Agent 更是将套利简化为极致的“收益意图表达”。这种进化虽然让链上市场趋近于绝对理性的高效，但也意味着套利已不再是单纯的硬件和 Gas 费较量，而是演变为谁的 Agent 能更具创造性地感知情绪、理解规则并掌控全链流动性的硅基智力对决。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
# 2026-07-13

在以太坊（Ethereum）的 DeFi 生态中，套利（Arbitrage）是利用不同协议、交易所（DEX/CEX）或 Layer 2 网络之间的短暂资产价差来获取利润的交易策略。套利者通常借助闪电贷（Flash Loans）在单笔区块交易内实现“无本金借贷、自动对冲并归还”，或通过 **MEV（最大可提取价值）** 捕捉大额交易和借贷清算带来的错价机会。虽然该领域高度依赖自动化机器人（Bots）的算法与速度博弈，竞争激烈，但它在让套利者获利的同时，也客观上抹平了市场价格不均，是维持以太坊生态流动性与价格平衡的核心机制。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

以太坊与传统金融的关系正在从“颠覆对抗”走向“深度共生”：通过现货 ETF 的普及以及 RWA（真实世界资产）代币化基金的爆发，以太坊正凭借 7×24 小时实时清算和自动化合规优势，被华尔街重塑为全球最大的“公共数字结算层”。未来，以太坊不会消灭传统银行，而是将作为不可逆的底层铁轨“吞噬”并升级其陈旧的基础设施，在双向赋能中构建一个资本流动如邮件般便捷的全球金融新秩序；而在这场数万亿资金入场的进程中，如何在拥抱合规的同时守住去中心化与抗审查的 Web3 核心灵魂，将是决定其最终高度的关键博弈。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
# 2026-07-11

以太坊协议层的演进是一场在飞行中更换客机引擎的壮举。从 PoW 到 PoS 的合并，再到以 EIP-4844 为核心的信标链升级，它不仅是代码的迭代，更是分布式共识范式的重塑。最令人惊叹的是其在“去中心化”、“安全性”与“可扩展性”之间进行的高难度平衡。协议层作为整个 Web3 的底层坚石，正逐渐走向模块化与极简主义，将执行压力释放给 Layer 2，自己则专注于提供绝对的经济安全与数据可用性。这种甘作“信任基座”的底层设计哲学，正是以太坊无可替代的魅力所在。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

共识层（Consensus Layer）是区块链中负责“让全网对账本状态达成一致”的核心部分，主要解决“谁来出块、区块是否有效、链上最终采用哪条链”这几个问题。以太坊在 The Merge 之后，已经从原来的 PoW（工作量证明）切换为 PoS（权益证明），其共识层主要由 **Beacon Chain（信标链）**承担。

在以太坊中，共识层与执行层是分开的：执行层（Execution Layer）负责处理用户交易、执行智能合约、维护账户状态；共识层（Consensus Layer）负责组织验证者（Validator）对区块进行提议、投票和确认，从而决定区块顺序和链的最终状态。两者通过 Engine API 协同工作。

以太坊 PoS 的基本机制是：用户质押 32 ETH 可成为验证者。系统会在每个 slot（12 秒）中随机选出一个验证者提议新区块，同时分配一组验证者对该区块进行 attestation（证明/投票）。32 个 slot 构成一个 epoch。当足够多的验证者（通常不少于总质押权重的 2/3）对区块链上的 checkpoint 达成一致时，该区块就会获得最终性（finality），意味着它几乎不可能被回滚。

共识层的核心作用包括：验证者管理、区块提议、投票确认、奖励与惩罚、分叉选择和最终性确认。如果验证者诚实参与，会获得质押奖励；若作恶，例如双重签名、提交冲突投票，则会被 slash（罚没）。因此，以太坊共识层本质上是一个通过“质押 + 投票 + 惩罚”来维护全网安全与一致性的系统。相比 PoW，PoS 大幅降低了能耗，也为后续扩容和模块化发展打下了基础。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

打算一个完全链上结算的轻量 PvP 链游：玩家带着自己的英雄 NFT / 装备进入竞技场，进行实时匹配 + 回合制战斗 + 随机事件 Roguelike 强化，战斗结果、掉落、排行榜、赛季奖励全部在 Monad 上完成
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

**Monad** 是一条面向高性能场景设计的 **Layer 1 公链**，目标是在保持 **EVM 兼容** 的基础上，大幅提升区块链的吞吐量与执行效率。它希望解决以太坊生态中“性能不足、交易成本高、链上应用扩展受限”等问题，为 DeFi、链游、社交和高频交易等应用提供更强的基础设施支持。Monad 的核心特点在于对执行层进行了深度优化，通过并行执行、优化共识与底层系统架构，提高每秒可处理的交易数量，并尽可能降低延迟。同时，Monad 强调对以太坊开发生态的兼容性，使 Solidity 开发者和现有 EVM 应用能够较低成本迁移或部署到其网络上。总体来看，Monad 的定位是“高性能 + EVM 兼容”的新一代公链，试图在不放弃以太坊生态优势的前提下，提供接近 Web2 级别的性能体验，从而吸引更多开发者与用户进入链上应用生态。
<!-- DAILY_CHECKIN_2026-07-07_END -->

<!-- DAILY_CHECKIN_2026-07-29_START -->
# 2026-07-29

BSC 和 Ethereum 并非简单的竞争关系，而是服务于不同的发展方向。Ethereum 仍然是整个 Web3 的价值层和金融层，拥有最成熟的开发者生态、最强的安全性和最深的 DeFi 流动性，是机构资金、RWA、DeFi 基础设施和协议创新的首选；而 BSC 则凭借低 Gas、高性能以及 Binance 带来的庞大用户流量，更适合面向散户市场，尤其是在 Meme、NFT、链游和消费级应用等领域，更容易实现产品冷启动和快速形成财富效应。对于创业者而言，如果目标是快速验证产品、获取用户并降低试错成本，BSC 是更合适的起点；而当产品逐渐成熟后，再向 Ethereum 及其 Layer2 扩展，以获得更强的安全性、更大的流动性和更广泛的生态支持，是更加合理的发展路径。
<!-- DAILY_CHECKIN_2026-07-29_END -->

<!-- DAILY_CHECKIN_2026-07-30_START -->
# 2026-07-30

我认为，BSC未来的机会不一定在于和Ethereum争夺底层基础设施，而更可能来自其低成本、高性能以及Binance庞大用户和流量入口所带来的应用层优势。随着链上用户逐渐从单纯的交易投机转向游戏、社交、NFT、支付和其他消费级应用，BSC有机会成为Web3应用大规模落地的重要平台。尤其是链游、NFT、Meme、SocialFi以及AI与链上应用结合的方向，都具备较大的想象空间。对于开发者而言，BSC最大的机会在于降低Web3产品的使用门槛，让普通用户能够以更低成本参与链上应用，因此未来真正有价值的机会可能不是简单复制DeFi项目，而是围绕用户需求打造具有持续使用场景的消费级产品，通过Binance生态的流量优势实现快速冷启动，再逐步形成自己的用户和资产生态。
<!-- DAILY_CHECKIN_2026-07-30_END -->

<!-- DAILY_CHECKIN_2026-07-31_START -->
# 2026-07-31

目前ETH链游已经从早期的“Play-to-Earn”和“NFT+游戏”逐渐进入更加注重游戏本身和链上经济的阶段，过去单纯依靠Token激励和NFT炒作的模式已经被证明难以长期维持，未来更重要的方向是“游戏原生体验+链上资产+可持续经济系统”。同时，随着Layer2的发展，链游不再需要直接部署在Ethereum主网，而是可以利用L2实现低Gas、高性能和更好的用户体验，因此Ethereum生态的优势更多体现在安全性、流动性和基础设施上。未来我认为真正值得关注的是Fully Onchain Game、链上模拟经营、策略、卡牌、RPG和社交类游戏，通过土地、角色、装备等资产形成可组合的链上经济，让玩家真正拥有并交易游戏资产。相比过去“先发NFT和Token再做游戏”的模式，未来更可能是先打造好玩的游戏，再逐步将核心资产和经济系统上链。
<!-- DAILY_CHECKIN_2026-07-31_END -->

<!-- DAILY_CHECKIN_2026-08-01_START -->
# 2026-08-01

举一个最经典、最值得研究的链游模式， **Axie Infinity**。

 



它最经典的模式可以概括成：



**玩家 → NFT角色 → 游戏战斗 → 获得资源 → 资源交易 → NFT资产流通 → 玩家形成经济循环**



具体来说，玩家首先购买或获得 Axie NFT，然后组建队伍进行战斗、繁殖和培养，通过游戏产生游戏资源；这些角色和资源可以在链上交易，优秀的 Axie 甚至可以产生一定的经济价值。早期 Axie 最大的创新就是把\*\*“游戏里的角色和资产”变成真正属于玩家的链上资产\*\*，再通过 Token 和 NFT 建立玩家、交易者、投资者之间的经济循环。



但 Axie 后来暴露出一个非常重要的问题：**如果玩家主要是为了赚钱，而不是为了游戏本身，那么当 Token/NFT 价格下跌时，整个经济系统就容易失去动力。**



所以如果把 Axie 的模式升级到今天，我认为应该变成：



**好玩的游戏 → 玩家留存 → 游戏内经济 → NFT资产所有权 → 玩家之间交易 → 长期生态价值**



而不是：



**发NFT → 发Token → 玩家为了赚钱进入 → Token上涨 → 吸引更多玩家。**



这也是你之前提到的\*\*“QQ农场 + NFT”**比较值得尝试的地方：可以把**土地、作物、宠物、装饰、稀有道具\*\*设计成可拥有的资产，但核心还是先让“种地、经营、社交、交易”本身好玩。
<!-- DAILY_CHECKIN_2026-08-01_END -->

<!-- DAILY_CHECKIN_2026-08-02_START -->
# 2026-08-02

链游大致可以分成 **5类模式**：

 

1. **P2E / 经济激励型**：玩游戏 → 获得 Token/NFT → 交易变现。代表：Axie。
2. **NFT资产型**：游戏本身为主 → 角色/装备/土地等资产上链 → 玩家真正拥有和交易。
3. **模拟经营型**：采集 → 生产 → 建设 → 交易 → 形成游戏内经济循环。代表：农场、城市、商业模拟。
4. **策略竞技型**：角色/卡牌/NFT → PVP/PVE → 排名、竞技、奖励，强调玩家之间的竞争。
5. **Fully Onchain / 开放世界型**：游戏规则、状态、资产全部或大部分上链 → 玩家和开发者可以基于链上世界继续创造内容。

如果从**未来潜力**排序，我个人更看好：**模拟经营型 ≈ NFT资产型 > Fully Onchain > 策略竞技 > 纯P2E**。

之前考虑的\*\*“QQ农场 + NFT”\*\*，本质上属于 **模拟经营 + NFT资产 + 玩家交易**，其实是比较适合个人开发者做 MVP 的方向。
<!-- DAILY_CHECKIN_2026-08-02_END -->

<!-- DAILY_CHECKIN_2026-08-03_START -->
# 2026-08-03

如果你是从\*\*“链游开发机会”\*\*的角度看，我觉得现在最值得关注的不是某个 NFT 标准，而是 **账户抽象（AA）这一套 EIP**。它可以直接改变链游的用户体验。

 

**最值得关注的几个**

1. **EIP-7702：EOA → 临时/可编程智能账户**
   * 可以批量执行操作、Gas 代付、权限控制。 
   * **链游价值：非常大**
   * 例如玩家点击“收获”，后台可以一次完成：\
     **收获 → 领取奖励 → 升级土地 → 购买种子**\
     用户只需要签一次。
2. **EIP-5792：Batch Call**
   * 钱包可以一次处理多个链上操作。 
   * 特别适合链游，把传统游戏的“一次点击”映射成多个链上动作。
3. **ERC-4337：Account Abstraction**
   * 支持 Smart Account、Paymaster、Bundler 等。 
   * 可以做到：\
     **玩家不用持有ETH，也能玩游戏。**
   * 游戏方可以替玩家支付Gas。
4. **ERC-7677：Paymaster标准**
   * 是围绕 EIP-5792 + ERC-4337 的 Paymaster Web Service 标准。 
   * 对链游非常实用：\
     **新玩家注册 → 免费获得角色 → 免费进行前几次游戏 → 游戏方承担Gas。**
5. **ERC-7902：Wallet Capabilities**
   * 进一步让钱包和DApp协商 AA 能力，例如 Paymaster、权限、Gas 参数等。 
   * 可以理解成让**钱包真正理解“这个游戏需要什么权限”**。



**如果落到你之前想做的 QQ 农场链游**

我觉得可以设计成：

**普通钱包**\
→ EIP-7702 Smart Account\
→ EIP-5792 批量操作\
→ ERC-4337 AA\
→ Paymaster 代付Gas\
→ 玩家只需要点击“收获”\
→ 链上自动完成：

**收获作物 → 更新土地 → 获得道具 → 经验增加 → NFT资产变化**

这样就能把传统链游最大的痛点——**钱包、签名、Gas、复杂交易**——全部隐藏起来。

所以如果想找一个\*\*“新EIP + 链游”真正可以做成黑客松项目的方向\*\*，最推荐：

**EIP-7702 + EIP-5792 + ERC-4337，做一个“无感链游钱包/游戏账户系统”。**
<!-- DAILY_CHECKIN_2026-08-03_END -->

<!-- DAILY_CHECKIN_2026-08-04_START -->
# 2026-08-04

**链上养成类小农场游戏（类似 Farm + NFT + 资源经济 + 成长系统）的核心合约架构。重点放在游戏核心逻辑上链**，采用 Solidity 风格伪代码，不考虑具体链（Monad/EVM 均可适配）。

 

游戏设定：

* 玩家拥有自己的农场 NFT
* 种植作物
* 作物经过时间成长
* 收获获得资源
* 资源用于升级土地、购买种子、培养宠物
* 高级农场产生更高收益
* 所有核心资产链上验证

***

# **合约架构**

```
FarmGame
│
├── FarmNFT        // 农场土地 NFT
├── CropNFT        // 作物 NFT
├── ItemNFT        // 道具 NFT
├── ResourceToken  // 游戏资源 Token
│
└── GameLogic      // 核心玩法合约
```

核心只需要一个：

```
FarmGame.sol
```

***

# **1. 玩家农场结构**

```
contract FarmGame {


struct Farm {

    uint256 owner;

    // 土地等级
    uint256 level;

    // 最大种植数量
    uint256 capacity;


    // 经验值
    uint256 exp;


    // 当前金币
    uint256 gold;


    // 创建时间
    uint256 createdAt;


    // 已解锁功能
    uint256 features;

}



mapping(uint256 => Farm) public farms;


// 用户拥有的农场
mapping(address => uint256[]) public userFarms;


}
```

***

# **2. 创建农场**

玩家第一次进入游戏：

```
function createFarm()
external
returns(uint256 farmId)
{

    farmId = farmCounter++;


    farms[farmId] = Farm({

        owner: msg.sender,

        level:1,

        capacity:3,

        exp:0,

        gold:100,

        createdAt:block.timestamp,

        features:0

    });


    userFarms[msg.sender].push(farmId);


    // 铸造土地NFT
    farmNFT.mint(
        msg.sender,
        farmId
    );

}
```

***

# **3. 种子系统**

种子作为 NFT：

```
struct Seed {


    uint256 id;


    // 作物类型
    CropType crop;


    // 生长时间
    uint256 growTime;


    // 基础产量
    uint256 reward;


    // 稀有度
    uint256 rarity;


}



mapping(uint256=>Seed)
public seeds;
```

例如：

```
普通小麦
成长 10分钟
收益 10金币


黄金南瓜
成长 24小时
收益 500金币
```

***

# **4. 种植逻辑**

玩家消耗种子：

```
struct Plant {


    uint256 seedId;


    uint256 farmId;


    uint256 plantedTime;


    bool harvested;

}



mapping(uint256=>Plant)
public plants;



function plant(
uint256 farmId,
uint256 seedId
)
external
{


Farm storage farm = farms[farmId];


require(
farm.owner==msg.sender,
"not owner"
);



require(
activePlants[farmId]
<
farm.capacity,
"farm full"
);



seedNFT.burn(
msg.sender,
seedId
);



plants[plantId++] = Plant({

seedId:seedId,

farmId:farmId,

plantedTime:block.timestamp,

harvested:false

});


}
```

***

# **5. 作物成长算法**

核心：

```
收益 = 基础收益 × 土地等级 × 稀有度 × 随机因子
```

伪代码：

```
function calculateReward(
uint256 plantId
)
internal
view
returns(uint256)
{


Plant p = plants[plantId];


Seed memory seed =
seeds[p.seedId];


Farm memory farm =
farms[p.farmId];



uint256 timePassed =
block.timestamp
-
p.plantedTime;



require(
timePassed >= seed.growTime,
"not mature"
);



reward = 
seed.reward
*
farm.level
*
seed.rarity;



return reward;


}
```

***

# **6. 收获**

```
function harvest(
uint256 plantId
)
external
{


Plant storage p =
plants[plantId];


Farm storage farm =
farms[p.farmId];



require(
farm.owner == msg.sender
);



require(
!p.harvested
);



uint256 reward =
calculateReward(
plantId
);



p.harvested=true;



// 发放资源
resourceToken.mint(
msg.sender,
reward
);



// 增加经验

farm.exp += reward/10;



checkLevelUp(
p.farmId
);


}
```

***

# **7. 农场升级系统**

升级公式：

```
升级费用 = 当前等级² ×100
```

例如：



Lv1 → Lv2



100金币



Lv5 → Lv6



2500金币

```
function upgradeFarm(
uint256 farmId
)
external
{


Farm storage farm =
farms[farmId];


require(
farm.owner==msg.sender
);



uint256 cost =
farm.level *
farm.level *
100;



resourceToken.burn(
msg.sender,
cost
);



farm.level++;



farm.capacity +=2;


}
```

***

# **8. 随机事件系统**

增加游戏性：

例如：

* 下雨
* 虫灾
* 黄金收成

```
function randomEvent(
uint256 farmId
)
internal
{


uint256 random =
keccak256(
abi.encodePacked(
block.timestamp,
msg.sender
)
)%100;



if(random <5)
{

// 黄金事件

goldBuff[farmId]=2;

}


else if(random <15)
{

// 虫灾

growthPenalty[farmId]=50;

}


}
```

***

# **9. 宠物系统（长期养成）**

宠物 NFT：

```
struct Pet {


uint256 level;


uint256 power;


uint256 bonus;


}



mapping(uint256=>Pet)
pets;
```

宠物影响：

```
收益提升
|
├── 小鸡 +5%
├── 小狗 +10%
└── 神龙 +50%
```

收获：

```
reward *=
(100 + pet.bonus)
/
100;
```

***

# **10. 链上经济模型**

推荐：

```
                 玩家
                  |
                  |
              Resource Token
                  |
       -----------------------
       |                     |
    升级土地             购买种子
       |                     |
       |                     |
     产量增加 <--------  高级作物
```

***

# **11. 防止机器人设计**

链游非常重要：

## **每日体力**

```
struct Player {


uint256 energy;


uint256 lastUpdate;


}



function consumeEnergy()
{

require(
energy>0
);


energy--;

}
```

***

## **随机种子**

不要使用：

```
block.timestamp
```

正式版：

```
Chainlink VRF
Monad native randomness
```

***

# **12. PVP玩法扩展**

可以加入：

偷菜：

```
function steal(
uint256 targetFarm
)
{


require(
targetFarm.level < attacker.level
);



uint256 amount =
farm.gold * 5 /100;



farm.gold -= amount;


attacker.gold += amount;


}
```

农场战争：

```
struct Battle {


uint256 attacker;


uint256 defender;


uint256 power;


uint256 reward;


}
```

***

# **13. NFT资产设计**

| **资产** | **类型**  | **作用** |
| :----- | :------ | :----- |
| 土地     | ERC721  | 农场入口   |
| 作物     | ERC721  | 成长对象   |
| 种子     | ERC1155 | 消耗品    |
| 宠物     | ERC721  | 永久加成   |
| 金币     | ERC20   | 经济循环   |

***

# **更适合 Monad 黑客松的版本**

如果做 Monad 链游，我建议简化成：

```
Farm NFT
    |
    |
Plant Contract
    |
    |
Growth Engine
    |
    |
$SEED Token
    |
    |
Marketplace
```

核心卖点：

1. **低 Gas 高频操作**
   * 每分钟成长计算
   * 自动化收获
2. **链上 AI 农夫**
   * AI NPC 帮玩家管理农场
3. **PVP 生态**
   * 偷菜
   * 农场战争
   * 排行榜
4. **可组合 NFT**
   * 土地 + 作物 + 宠物组合生成稀有农场

这个结构基本可以作为一个 **Monad Hackathon 的完整链游 MVP 合约设计**。
<!-- DAILY_CHECKIN_2026-08-04_END -->
<!-- Content_END -->
