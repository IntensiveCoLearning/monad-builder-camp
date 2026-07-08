---
timezone: UTC+8
---

# lij818556-hash

**GitHub ID:** lij818556-hash

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 1.Fluxa公司（由前蚂蚁团队创办）推出AI智能体专属支付工具Agent Wallet，堪称“龙虾（OpenClaw）版支付宝”。

该工具彻底解决了AI Agent付费场景的“最后一公里”痛点，支持OpenClaw、[Claude Code](https://www.chooseai.net/tools/107/)、CodeX等主流AI Agent一键接入，让智能体具备自主支付、参与经济活动的能力，甚至能通过抢红包、参赛赚钱“反哺”人类，标志着AI Agent从“工具”向“具备经济行为能力的数字个体”跨越。

想要解决的核心痛点：让 AI Agent 在“被授权、被限制、可审计、可结算”的前提下，安全地替人花钱和收钱。

Agentic payment：未来是如何发展的

未来高频低额的线上支付问题如何解决，需要依靠这个进行解决方案的实施。

![](https://zcn6rqjg1z5s.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWE2OGQ1NDI4MjEyY2E5NjkyNWYzZmE1MzljNTE5YTlfWWRVSEJWQ1FiRTdZc0xFTEg0ZlVuVGJDYWlMVFg2WUxfVG9rZW46UHFTamJPc2g0b2tuZzd4VEVJVGNTRm5YbmZlXzE3ODM1MTI1NzY6MTc4MzUxNjE3Nl9WNA&add_watermark=true&scene_type=CCM)

# 会议纪要：AI Agent Payment 支付基础设施分享

## 一、会议主题

本次分享主要围绕 **AI Agent 支付基础设施** 展开，重点讨论当 AI Agent 从“对话工具”走向“可执行任务的智能体”后，为什么支付会成为绕不开的基础设施，以及 FluxA 团队如何围绕 Agent 身份、授权、预算、风控、收付款、结算和可撤销交易等环节构建产品体系。

核心判断是：未来 AI Agent 不只是回答问题，而是会替用户完成一系列任务，例如写研究报告、调用大模型 API、购买付费数据、订酒店、订机票、调用第三方工具等。只要 Agent 进入真实执行场景，就一定会涉及支付。

* * *

## 二、背景与问题

传统支付体系是围绕“人”设计的。用户开银行账户、绑定银行卡、输入支付信息、手动确认交易，都依赖人的身份和操作。但 AI Agent 本身没有身份证、没有银行卡，也不能像人一样完成 KYC、填写银行信息和逐笔确认交易。

因此，Agent Payment 面临几个核心问题：

1.  **身份问题**：Agent 如何拥有可识别的支付身份？
    
2.  **授权问题**：人如何授权 Agent 在一定范围内花钱？
    
3.  **风控问题**：如何防止 Agent 幻觉导致乱花钱？
    
4.  **微支付问题**：Agent 调用 API、数据、MCP、Skill 时，可能是极小金额支付，传统支付手续费不适合。
    
5.  **结算问题**：Agent 执行一连串任务时，不适合每一步都让人手动确认。
    
6.  **安全问题**：不能让用户在 chatbot 里直接暴露银行卡等敏感信息。
    

分享中强调，Agent 支付不能简单照搬传统“我给钱，你给货”的一次性交易逻辑，而是要适配 Agent 完成一组任务、调用一系列工具的流程化场景。

* * *

## 三、产品定位

FluxA 的定位不是单纯做一个钱包，而是做一套面向 AI Agent 的支付协议和基础设施。

可以概括为：

> 为 AI Agent 提供安全、自动化、可控的收付款与结算能力，让 Agent 能够在用户授权和风控约束下完成支付任务。

它主要服务于以下几类场景：

1.  **Agent to Agent** 一个 Agent 调用另一个 Agent 的能力，并为其服务付费。
    
2.  **Agent to Merchant** Agent 帮用户订酒店、订机票、购买商品、支付订阅服务等。
    
3.  **Agent to Tool / API / MCP / Skill** Agent 调用第三方 API、数据服务、MCP 或 Skill，按照调用次数或任务结果付费。
    
4.  **Agent 转账 / 社交支付** Agent 之间可以基于 nickname、Agent ID 或 payment link 进行转账、收款和红包等社交支付。
    

* * *

## 四、核心产品模块

### Agent Identity

Agent 需要一个可识别的支付身份。当前方案中，用户可以通过安装 skill、授权登录等方式获得钱包。该钱包既可以支持区块链钱包形态，也可以支持法币充值后形成平台内 credential，类似平台积分或额度。

身份模块的目的不是让 Agent 拥有传统人的身份证，而是让它具备可被识别、可被授权、可被审计的支付主体能力。

* * *

### Agent Wallet

Agent Wallet 是与人共管的钱包，不是完全交给 AI 自主控制，也不是每一笔都必须人手动确认。

核心逻辑是：

> 人设定预算、权限和约束条件，Agent 在这个范围内自主执行任务和支付。

例如，用户可以给 Agent 一个 50 美元预算，在该预算内允许它调用 API、购买数据、完成任务。这样既避免 Agent 失控花钱，也避免每一步都让人确认导致流程过长。

* * *

### Agent Card

Agent Card 是面向 Agent 的支付卡能力，目前包括一次性卡和后续与 Visa 深度合作的持续性卡能力。

一次性 Agent Card 的逻辑是：

1.  用户给 Agent 一个固定预算，例如 450 美元；
    
2.  Agent 自动生成一张一次性卡；
    
3.  Agent 用这张卡完成一次任务，例如订票；
    
4.  任务完成后，这张卡失效。
    

这样可以降低 Agent 乱花钱的风险，也适合一次性交易场景。分享中提到，团队正在与 Visa 深度合作，后续会推出更持续可用的 Agent Card 能力。

* * *

### Agent Charge / Payment Link

Agent Charge 面向商家或个体收款方，帮助他们接收 Agent 发起的付款。

支付方式包括：

1.  **Payment Link** 适合电商、一次性交易等场景。收款方生成支付链接，付款方点击并授权即可完成支付。
    
2.  **Agent ID 转账** 两个 Agent 安装对应 skill 后，可以基于 Agent ID 直接转账。
    
3.  **Mandate 授权支付** 用户提前签署一个授权，例如“最多可花 100 美元”，Agent 在预算内可以执行多笔支付，最后统一结算。
    

* * *

### Monetize Gateway

Monetize Gateway 主要服务开发者、独立创作者和一人公司。

很多开发者会把自己的能力做成：

```Plain
API
MCP
Skill
数据服务
AI 服务
行业知识库
```

FluxA 希望帮助这些能力被 Agent 发现、调用并付费，从而让开发者获得持续收入。

分享中特别提到与百度智能体相关生态的合作：百度侧有很多 AI 服务、API、MCP、Skill 等资产，FluxA 可以帮助这些资产上架、分发和变现，让 Agent 调用时按次付费，收入归开发者。

* * *

### Embedded Mandate / 延后结算

这是整套支付协议中很关键的一点。

传统支付往往是每一笔都即时确认，但 Agent 执行任务时可能会产生很多微支付。如果每一步都要求人点击确认，流程会非常低效。

Embedded Mandate 的思路是：

> 用户先授权一个预算和规则，Agent 在约束条件内执行多笔交易，最后集中结算。

这可以提高微支付场景下的效率，也能适应 Agent 连续执行任务的需求。

* * *

## 五、协议与技术方向

### x402

分享中特别提到了 **x402**。它可以理解为一种面向 Agent 付费调用的协议方向，来源于 HTTP 402 Payment Required 的思路，并由 Coinbase 等机构推动。

它的核心是：

> Agent 在调用某个资源、API 或服务时，可以直接根据协议完成付费，而不必每次经过人的手动确认。

FluxA 基于 x402 做了自己的优化和扩展，用来适配 Agent Payment 的实际场景。

* * *

### Base + USDC

当前主要支付链路运行在 **Base 上的 USDC**。

原因包括：

1.  Base 当前在相关交易场景中较活跃；
    
2.  USDC 在合规性上更容易被接受；
    
3.  微支付和 Agent 支付场景更适合低成本、高确认效率的链上稳定币支付。
    

分享中也提到，目前暂不优先支持跨链，因为跨链会增加钱包和支付复杂度，现阶段 Base 上的 USDC 已经能满足多数需求。

* * *

## 六、风控与合规

分享中反复强调，支付最重要的是**安全、风控和可审计**。

Agent 和普通软件不同，它可能产生幻觉。如果幻觉出现在金融场景中，后果会非常严重。因此 Agent Payment 必须设置清晰的约束条件，例如：

1.  单次支付上限；
    
2.  总预算上限；
    
3.  可支付对象范围；
    
4.  可调用服务范围；
    
5.  每笔交易可审计；
    
6.  异常交易可撤销；
    
7.  未来接入 KYC / KYB / 反洗钱体系。
    

在合规方面，团队认为未来 Agent Payment 一旦走向主流，就必然需要与传统支付公司、发卡方、银行、Visa 等机构合作，而不是完全绕开传统金融体系。

* * *

## 七、案例与实践

分享中提到了几个实践案例：

### Agent 红包 / 社交支付

春节期间团队做过 Agent 社交产品，用户可以通过 prompt 让 Agent 去识别红包、抢红包，甚至设置答题红包，例如“床前明月光下一句是什么”。这个案例展示了 Agent 支付在社交场景中的可能性。

### 百度合作

FluxA 与百度智能体生态有合作，帮助平台上的 AI 服务、MCP、Skill、API 等能力被上架、发现、调用和变现。

### 订票场景

团队曾与大麦等场景做过探索，让 Agent 帮用户完成订票、订行程等操作，展示 Agent 在生活服务支付中的应用潜力。

### 调研报告付费 Skill

团队还提到与硅谷 VC 相关的调研类 Skill 合作。用户可以付费调用某个研究能力，让 Agent 获取特定行业或公司的研究报告。

* * *

## 八、问答要点

### Agent 是否需要 KYC？

当前 Agent 创建门槛较低，但未来如果走向主流金融场景，需要加入 KYC / KYB / 反洗钱体系。部分场景可以由 B 端主体提前完成 KYB，再让 Agent 在授权范围内使用支付能力。

### Agent Card 和传统银行卡有什么区别？

Agent Card 底层仍然离不开传统发卡和支付体系，但产品设计上会针对 Agent 做额外控制，例如一次性额度、预算锁定、单次使用、约束条件等。

### 支付过程会不会是黑箱？

不会。分享者强调，支付不能像大模型能力那样不可解释。支付链路必须清晰、可审计、可追踪，否则无法真正落地。

### 为什么选择 Base 而不是以太坊主网？

主要因为以太坊主网手续费较高，且流程推进较慢；Base 依托 Coinbase，在金融机构合作、稳定币支付、开发执行力方面更灵活，因此更适合当前 Agent Payment 场景。

### 数字人民币是否会成为方向？

分享者认为数字人民币是未来可能方向之一，但在国内，微信和支付宝已经非常便捷，普通用户缺乏强烈迁移动力；数字人民币国际化还涉及更复杂的政治、经济和国力因素。

### FinTech 背景学生是否适合进入 Agent Payment？

适合。分享者认为 FinTech 与 Agent Payment 会融合发展。未来支付公司、金融科技公司都会拥抱 Agent Payment，相关学生如果理解 Agent、支付流程、风控和技术结构，会有进入该方向的机会。

* * *

## 九、后续安排与行动项

1.  **PPT 暂不公开分享** 主讲人表示当前 deck 不是最终版本，且涉及内部内容，因此暂不适合直接发群里。
    
2.  **关注官方动态** 建议大家关注 FluxA 官方账号，持续跟踪 AI Agent Payment 方向进展。
    
3.  **体验产品链接** 主讲人提到会提供体验链接，大家可以通过 prompt 安装 skill，体验 Agent 抢红包、支付等功能。
    
4.  **实习机会** 团队近期有实习生需求。后续如果学习积分、打卡表现靠前，可能会优先获得内推机会。投递时需要写清楚个人身份、背景和简历信息。
    
5.  **后续 Visa 合作进展** 与 Visa 的合作预计很快会有进一步公开，后续可以关注团队在 Agent Card 方面的产品发布。
    

* * *

## 十、会议核心结论

本次会议的核心结论可以概括为：

**AI Agent 要真正进入日常生活和商业场景，必须具备被授权、可控、安全、可审计的支付能力。FluxA 正在围绕 Agent Identity、Agent Wallet、Agent Card、Agent Charge、Monetize Gateway、x402 和 Base USDC 支付体系，构建面向智能体经济的支付基础设施。**

更通俗地说：

> 未来 Agent 不只是“帮你想”，还要“帮你做”。 一旦它帮你做事，就会涉及花钱。 一旦涉及花钱，就必须有身份、预算、授权、风控、结算和合规。 FluxA 做的就是这套 Agent 时代的支付底层设施。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

[https://zcn6rqjg1z5s.feishu.cn/wiki/HiupwekhEiAaJ1kT28ucM2CEnad?from=from\_copylink](https://zcn6rqjg1z5s.feishu.cn/wiki/HiupwekhEiAaJ1kT28ucM2CEnad?from=from_copylink)

飞书链接  
  
DAY3 ：以太坊协议层开发，离普通开发者到底有多远？

# 概念1：EPF7 活动：并非单纯的上课，而是一个自驱项目。（后面的序号代表第几期）

EPF = Ethereum Protocol Fellowship，以太坊协议奖学金/协议研究员计划。更多依靠自己的跟进。（参与需要使用英文进行面试）

EPS = Ethereum Protocol Studies，中文可以理解为：以太坊协议学习计划 / 协议研究学习小组。更多用于新手、学生的入门学习。用于为EPF输送人才。

## 核心任务：

-   **改进以太坊本身** 比如提升性能、安全性、去中心化程度、验证者机制、网络通信效率等。
    
-   **参与协议升级** 研究和实现以太坊未来升级方向，比如账户抽象、PBS、Danksharding、Verkle Tree、EVM 优化等。
    
-   **开发和维护客户端** 以太坊不是一个单一软件，而是由多个客户端共同运行，例如 Geth、Nethermind、Lighthouse、Prysm 等。EPF 参与者可能会为这些客户端贡献代码、测试或研究。
    
-   **做协议研究和工程实现** 不是只写论文，而是把研究变成可以被以太坊网络采用的技术方案。
    

# 概念2：EIP = Ethereum Improvement Proposals，以太坊改进提案。

### 定义：任何人如果想对以太坊协议、标准或生态规则提出正式修改，都可以写成一份 EIP。

### EIP的具体包含什么：

标题 Title 这个提案要解决什么问题

作者 Authors 谁提出这个 EIP

状态 Status Draft、Review、Final、Stagnant、Withdrawn 等

类型 Type Core、Networking、Interface、ERC、Meta、Informational 等

摘要 Abstract 用简短语言概括这个提案

动机 Motivation 为什么以太坊需要这个改进

规范 Specification 最核心部分：具体技术规则怎么改

理由 Rationale 为什么选择这种方案，而不是其他方案

兼容性 Backwards Compatibility 会不会影响旧版本、旧合约、旧客户端

安全考虑 Security Considerations 可能带来的攻击风险、安全隐患

测试 Tests 如何验证这个改动有效

参考实现 Reference Implementation 代码层面如何实现

版权 Copyright 通常采用 CC0 公共版权声明

### 一个具体的EIP网址：[https://eips.ethereum.org/EIPS/eip-4844?utm\_source=chatgpt.com](https://eips.ethereum.org/EIPS/eip-4844?utm_source=chatgpt.com)

该EIP目标：Layer2 想扩容，但把数据提交到以太坊主网太贵。（layer1和laye2区别：Layer2 是为了帮以太坊“分担交易压力”的扩容方案，具体案例：每一笔都要付主网 Gas 费，成本很高；而Layer2 先把这 10,000 笔交易打包处理，然后把压缩后的数据或结果提交到以太坊主网。）

该EIP内容：Layer2 可以处理大量交易，但为了继承以太坊的安全性，它仍然要把关键数据发回以太坊。而过去这些数据主要通过 calldata 提交，费用较高，所以 EIP-4844 引入 Blob 来降低这部分成本。

### 选择EIP项目的时候，先选择问题域：再深度解读和解决问题

并不需要了解完整的协议，可以一边学习一遍构建具体的EIP。

### 当前EIP提案总量：1175个左右。总量相对来说其实很少。

### EIP提交前提：

任何人都可以：

1.  **提出一个以太坊改进想法**
    
2.  **在社区先讨论**
    
3.  **按照 EIP-1 的格式写成正式文档**
    
4.  **通过 GitHub 向 Ethereum/EIPs 仓库提交 Pull Request**
    
5.  **等待 EIP Editors 审查格式、范围和规范性**
    
6.  **一般是在几个月到1年左右的周期。**
    

# EIP 和 EPF 的区别：

| 对比 | EIP | EPF |
| 全称 | Ethereum Improvement Proposal | Ethereum Protocol Fellowship |
| 中文理解 | 以太坊改进提案 | 以太坊协议奖学金/协议研究员计划 |
| 本质 | 一份正式技术提案文档 | 一个培养协议开发者的项目 |
| 核心对象 | 技术方案、标准、协议改动 | 人才、开发者、研究者 |
| 作用 | 提出“以太坊应该怎么改” | 培养“谁来参与以太坊底层开发” |
| 输出成果 | EIP 文档、规范、标准 | 研究成果、代码贡献、协议项目、开发者成长 |
| 例子 | EIP-1559、EIP-4844、ERC-20 | EPF5、EPF6、EPF7 |

# 概念3：ETH 开发本身与基于其构建的Metamask钱包的区别。

更多是内容（技术）与商业化（产业）的关系。

# 概念4：区块链不可能三角：去中心化——无限拓展——降低成本。

# 概念5：安装部一个NFT并且上线。

NFT测试上线专用钱包私钥  
  
已经铸造一个NFT地址，等待后续进一步关联图片

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/lij818556-hash/images/2026-07-07-1783434128514-image.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
