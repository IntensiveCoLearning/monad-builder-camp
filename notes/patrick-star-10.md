---
timezone: UTC+8
---

# Patrick

**GitHub ID:** patrick-star-10

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
打卡
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->

打卡
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->


打卡
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->



打卡
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->




周末打卡
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->





周末休息打卡
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->






今天我完成了第一次针对 Moss 开源项目的实际 PR 提交。这个 PR 的改动非常小，主要是更 新

.github/PULL\_REQUEST\_[TEMPLATE.md](http://TEMPLATE.md)在 Verification 检查清单中补充了pnpm test:offline 的说明。

这个改动的背景是 Moss 之前已经新增了离线测试命令，但 PR 模板里还只写了pnpm test，没 有提醒贡献者在无法访问 Monad live RPC 时应该如何说明测试情况。 我先阅读了项目近期 PR 和文档，判断这是一个低风险、容易 review 的文档一致性修复。随后 fork 了仓库到自己的 GitHub，clone 到本地桌面，创建了docs/pr-template-offline-test分支，只新增 了一行说明。提交前我本地运行了git diff --check、pnpm lint和pnpm test:offline，确认格式、 lint 和离线测试都通过。 最后我将分支 push 到自己的 fork，并向nishuzumi/moss 提交了 PR：docs: document offline testverification in PR template。这次实践让我理解了开源贡献不一定要从大功能开始，小的文档和流程 改进同样有价值，关键是要找到真实存在的不一致点，并用清晰、可验证的方式提交。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->







今天主要完成了 Web3 新人安全入门活动的一整套运营作业。我选择的活动主题是“从看不懂钱包弹窗开 始：新人如何安全参与 Web3”，目标是帮助刚进入 Web3 的新人理解钱包连接、签名、交易、授权和钓 鱼链接等基础风险。 在内容设计上，我先明确了活动为什么要办、面向谁、解决什么问题；然后进一步设计了活动当天的流 程，包括活动简介、时间安排、主持串场、核心讨论问题、互动环节和收尾 CTA。之后又补充了活动执 行预案，整理了上线前的时间排期、宣传节奏、人员分工、当天执行流程、风险预案和预期目标。 最后，我把活动方案转化成可直接使用的运营物料，包括完整宣传文案、报名页简介、活动提醒文案和 参与 CTA，并整理成 Ops Case Study，复盘了本周的运营思路、AI 协作过程和 Week 3 想承担的运营 角色。整体来看，这次作业让我更清楚地理解了：运营不只是写文案，而是围绕用户真实问题，设计一 套能被理解、能被执行、能沉淀复用的内容路径。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->









今天了解了一下moss这个项目
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->










打卡
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->











打卡
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->












打卡
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->













1\. 我完成了什么

本周主要完成了 Solidity 智能合约开发环境的搭建，并在 Monad Testnet 上部署了多个简单合约，包括 ValueVault、MessageBoard 和 SimpleVote 等合约，熟悉了 Remix、MetaMask、Monad 测试网以及链上交易流程。

除此之外，我还结合 AI 生成了一个简化版 AMM（Automated Market Maker）合约，重点学习了流动性池、代币交换以及恒定乘积做市模型（x \* y = k）的基本思想。虽然目前实现的是教学版本，但让我对 Uniswap V2 的核心机制有了更直观的理解。

我还阅读了部分 AMM 协议源码和《精通以太坊》的相关章节，对 EVM、交易执行流程、执行层与共识层客户端的职责有了初步认识。

2\. 我遇到了什么问题

开发过程中遇到了几个问题：

部署合约时忘记给构造函数（constructor）传入初始化参数，导致部署失败。

一开始混淆了交易哈希（Transaction Hash）和合约地址（Contract Address），提交作业时填写了错误的部署交易哈希。

对执行层客户端、共识层客户端以及 RPC 节点之间的关系理解不够清晰。

阅读 AMM 合约时，虽然能够理解整体流程，但对于流动性计算、价格滑点等数学部分仍需要进一步学习。

3\. AI 帮助我完成了什么

AI 在整个学习过程中主要承担了辅助学习和知识解释的角色，而不是直接替代开发。

具体包括：

帮助定位 Solidity 合约部署失败的原因，并指出构造函数参数必须在部署时传入。

帮助区分交易哈希、合约地址以及函数调用交易。

生成了一个简化版 AMM 合约，用于理解 AMM 的整体架构和资金流转过程。

解释了执行层客户端、共识层客户端、RPC 节点以及 Geth 等概念，使我能够建立更加完整的 Web3 技术体系。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->














这几天都在学习golang的基础语法，尝试调用rpc去查询链上信息
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
















打卡
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->

















今天复习了一下solidity的基本语法以及复习了一遍REC20，ERC721，和UNISWAPV2的源码
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->


















**_今天学习了怎么使用monad浏览器进行交易查询，同时也体验到了monad的交易速度确实很快相比于以太坊网络_**
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->



















### 今天初步了解了 Monad 网络

### **_Monad 是一条新一代 Layer 1（第一层）公链，它最大的特点是兼容以太坊虚拟机（EVM），也就是说，以太坊上的很多智能合约和应用可以较为方便地迁移到 Monad 上运行，不需要重新开发。它的目标是在保持 EVM 生态兼容性的同时，大幅提升网络性能，提供更高的交易吞吐量和更低的交易成本_**

**_据了解，Monad 在底层架构上采用了并行执行、优化状态存储以及执行与共识分离等技术，希望通过这些优化提高交易处理效率，而不是改变开发者熟悉的 EVM 开发环境。相比传统以太坊主网，它希望能够支持更多用户同时进行交易，并降低 Gas 手续费，为 DeFi、NFT、链游等 Web3 应用提供更好的运行环境_**

**_不过，目前市场上已经有不少高性能公链和以太坊 Layer2 网络，例如 Solana、Arbitrum、Base 等，因此 Monad 面临的竞争也非常激烈。虽然官方提出了较高的性能目标，但区块链领域一直存在“去中心化、安全性、性能”三者难以同时兼顾的问题，因此 Monad 是否能够真正实现高性能，同时保持较好的去中心化程度，还需要等待主网长期运行后的实际表现来验证_**

**_总的来说，我认为 Monad 更像是在探索“高性能 EVM 公链”这条路线。它的技术思路值得关注，但未来能否成功，不仅取决于底层技术，还取决于开发者生态、用户数量以及应用的发展情况。对于普通用户来说，现阶段可以保持关注，持续学习和了解它后续的发展_**
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
