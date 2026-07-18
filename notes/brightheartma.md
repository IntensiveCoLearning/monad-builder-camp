---
timezone: UTC+8
---

# brightheartma

**GitHub ID:** brightheartma

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
## 今日主题

围绕 [Moss PR #56：Clober V2 CLOB Adapter](https://github.com/nishuzumi/moss/pull/56)，完成“作者修复—本地复测—发现剩余问题—定位根因—提交反馈—再次修复—全量回归”的完整开源 Review 闭环。

## 今天完成了什么

昨天针对 PR #56 提出了五项 Review 意见，涉及：

-   非零但不足的 ERC-20 allowance 是否兼容 zero-reset Token；
    
-   是否应使用明确的 curated market catalog；
    
-   BookViewer 报价与 Controller 实际执行路径是否一致；
    
-   是否错误依赖可选的 Token `name()` 和 `symbol()`；
    
-   BookId、unit size、fee policy、报价和授权逻辑是否需要补充来源与注释。
    

作者在 Commit `9307779` 中逐项处理，并邀请我重新运行 characterization tests。今天完成了三轮验证：

1.  确认原来的五项问题已经得到处理；
    
2.  通过负向测试发现 Receipt 验证和 99.9% 输入利用率仍有两个问题；
    
3.  作者继续提交 `e9b0417`、`9938e2d` 修复后，在干净工作树重新运行完整检查，最终确认没有新的可执行问题。
    

## 第一轮复测：原有五项问题已处理

在 `9307779` 上验证通过：

-   完整构建；
    
-   Clober TypeScript 类型检查；
    
-   26 项 Clober 本地及 Monad 测试；
    
-   MON→USDC、USDC→MON 两个方向的实时报价；
    
-   Viewer 与 Controller 的零滑点模拟一致性；
    
-   zero-reset allowance 顺序；
    
-   curated market 拒绝逻辑；
    
-   Token metadata independence；
    
-   关键协议逻辑、参数和来源说明。
    

授权流程现在会根据 allowance 生成不同的 Capability Tree：

```
allowance = 0
→ approve(amountIn) → spend

0 < allowance < amountIn
→ approve(0) → approve(amountIn) → spend

allowance >= amountIn
→ spend
```

这说明作者不是只修改文档，而是把反馈落实到了代码、测试和安全说明中。

## 第二轮测试：发现两个剩余问题

### 1\. Receipt 无法证明发生的是用户请求的 Swap

在 `9307779` 上增加负向测试后，以下异常 Change 仍会被接受：

-   正确的 `Take` 加两笔无关 Token Transfer；
    
-   `Take` 使用错误 BookId；
    
-   USDC→MON 只有输入 Token Transfer，没有 MON 输出；
    
-   两笔零金额、无关账户之间的 Transfer；
    
-   Transfer 数量满足要求，但 Token、方向和参与者不属于本次 Swap。
    

根因是当时的 Receipt Parser 主要检查：

```
存在 Take
所有 Take 使用同一个 BookId
transferCount >= 2
```

它没有验证：

-   BookId 是否属于 curated market；
    
-   Token 是否等于预期输入和输出资产；
    
-   是否同时存在输入和输出结算；
    
-   Transfer 方向和参与者是否正确；
    
-   金额是否非零；
    
-   输入、实际消费和退款能否守恒。
    

更深层的框架限制是：`Registry.parseReceipt` 虽然持有 `CapabilityNode`，但 Receipt Parser 只能收到有序的 `Change[]`，无法直接读取原始 Capability 参数和交易发送者。

因此，这不是直接修改 calldata 或窃取资金的执行漏洞，但旧 Receipt 不能独立证明观察到的 Transfer 就是用户请求的 Swap。

### 2\. 固定 99.9% 利用率会误判正常 Book Unit Dust

在固定 Monad 区块 `88525940` 上执行了 73 组原始 `BookViewer.getExpectedOutput` 查询。

MON→USDC 的部分结果：

| 输入 | 实际利用率 | 结果 |
| --- | --- | --- |
| 0.008 MON | 99.940064% | 通过 |
| 0.009 MON | 99.619126% | 拒绝 |
| 0.010 MON | 99.824527% | 拒绝 |
| 0.011 MON | 99.992581% | 通过 |
| 0.012 MON | 99.747502% | 拒绝 |
| 0.039 MON | 99.895627% | 拒绝 |
| 0.040 MON | 99.940064% | 通过 |

USDC→MON 从最小的 `0.000001 USDC` 到 `1 USDC`，测试中均为 100% 消费。

这说明 99.9% 不是简单的“最小交易额”，而是随输入金额、价格、方向和订单单位变化的锯齿型边界。

Clober 会把剩余输入换算为可执行的 quote units：

```solidity
maxAmount = tick.baseToQuote(remaining, false) / key.unitSize;
if (maxAmount == 0) break;
```

整数除法会向下取整。当剩余输入不足以购买一个 quote unit 时，Clober 会正常停止并退款。这部分是绝对数量的订单单位 dust；Moss 使用的却是相对比例规则：

```
spentAmountIn >= ceil(amountIn × 99.9%)
```

因此，小额交易可能有有效输出，却因为正常的单位取整退款而被拒绝。

## 向作者提交的第二轮反馈

我将两个问题拆成不同性质：

-   Receipt：属于正确性与验证强度问题；
    
-   99.9%：属于 fail-closed 策略与 Clober 离散订单单位之间的设计取舍。
    

没有把它们描述成直接资金漏洞，也没有要求在同一 PR 中修改 Moss Core。详细测试数据和根因分析已作为附件发布在[第二轮 Review 评论](https://github.com/nishuzumi/moss/pull/56#issuecomment-5010840747)中。

## 作者第二次修复

作者随后提交 `e9b0417` 和 `9938e2d`。

新的 Receipt Parser 会：

-   将 `Take` BookId 映射到两个 curated 方向之一；
    
-   拒绝零单位的 `Take`；
    
-   验证 Token、参与者、方向和非零金额；
    
-   MON→USDC 必须存在：
    

```
user → Controller → BookManager：native MON 输入
BookManager → user：USDC 输出
Controller → user：可选 MON 退款
```

-   要求：
    

```
input = settled + refund
```

-   USDC→MON 必须存在：
    

```
user → BookManager：USDC 输入
BookManager → user：native MON 输出
```

-   拒绝错误 BookId、错误 Token、错误参与者、零金额、额外 Transfer、无关 Approval 和不守恒退款；
    
-   继续保持每个原始 Change 的对象、数量和顺序不变。
    

对于 99.9% 利用率，作者选择保留严格的 fail-closed 策略。理由是 BookViewer 只返回输出量和实际消费量，无法判断剩余输入是因为不足一个 quote unit，还是订单簿流动性耗尽。仅凭返回值放宽 dust 可能削弱 partial-liquidity 保护。

最终方案不是假装消除该边界，而是：

-   保留严格的 99.9% 规则；
    
-   在 README 中明确精确公式；
    
-   说明小额交易可能被拒绝；
    
-   说明边界具有非单调、方向相关和价格相关特征；
    
-   为阈值上下界及 `0.001 MON` 案例添加回归测试。
    

## 最终回归结果

在当前 Head `9938e2d` 的干净工作树中重新验证：

-   `pnpm lint`：通过；
    
-   `pnpm build`：通过；
    
-   `pnpm typecheck`：通过；
    
-   完整 `pnpm test`：通过；
    
-   26 项 Clober 测试：全部通过；
    
-   MON→USDC 与 USDC→MON Live Monad Quote：通过；
    
-   Viewer/Controller 零滑点模拟：通过；
    
-   之前五项 Review 问题：保持已解决；
    
-   Receipt 异常 Change 测试：现在均被正确拒绝；
    
-   99.9% 阈值和 `0.001 MON` 案例：行为与文档一致。
    

最终已向作者回复：[两轮反馈从我这边均已解决，没有发现新的可执行问题](https://github.com/nishuzumi/moss/pull/56#issuecomment-5011267131)。

所有 characterization tests 都在临时工作树中完成，没有修改本地 Moss 正式工作区，也没有提交测试分支。

## 今天学到的关键内容

### 1\. Review 不能只复跑作者的正向测试

“26 tests passed”只能证明作者写下的预期行为成立。真正发现验证缺口的是负向测试：

-   如果输出 Transfer 缺失，会不会仍然通过？
    
-   如果 BookId 错误，会不会仍然通过？
    
-   如果金额为零，会不会仍然通过？
    
-   如果 Transfer 完全无关，会不会仍然通过？
    

安全 Review 的核心不是证明 Happy Path 能跑，而是证明错误证据不能伪装成正确结果。

### 2\. 发现异常后，还要区分 Bug 与策略

Receipt 问题违反了它需要解释真实结算的职责，因此应当修复。

99.9% 边界则不同：它确实会拒绝部分可执行小额交易，但这是在信息不足时选择 fail-closed。只要行为被准确记录、测试覆盖且没有被宣传成 Clober 的协议保证，就可以作为明确的产品取舍保留。

### 3\. 根因分析比单个失败案例更有价值

如果只报告“0.001 MON 失败”，很容易被理解为流动性不足或偶然状态变化。

固定区块、密集扫描和双向对比证明了它是由：

```
Book Unit 整数取整
+ 绝对 Dust
+ 相对 0.1% 阈值
```

共同形成的锯齿型边界。只有找到这一层，Review 建议才不会停留在“把阈值调低一点”。

### 4\. 好的 Review 是协作，不是挑错

作者认真处理了第一轮反馈，并主动邀请复测；第二轮测试发现问题后，作者继续补充实现、负向测试和 README。

最终成果不是“我证明作者写错了”，而是双方通过可复现证据把 Adapter 的正确性和边界说得更清楚。

## AI 帮助了什么，我人工校正了什么

AI 帮助完成：

-   设计 Receipt 的 Token、方向、账户、金额和 BookId 负向测试矩阵；
    
-   执行固定区块的 73 组报价扫描；
    
-   对照 Clober `BookViewer`、`Controller._spend` 和 `_settleTokens` 定位根因；
    
-   区分结构性 Receipt 缺口和利用率策略问题；
    
-   整理英文 Review 评论及详细测试附件；
    
-   在作者更新后重复运行完整回归。
    

人工负责：

-   明确要求先测试、找到根因，再决定是否发布评论；
    
-   没有在第一轮修复后直接回复 `LGTM`；
    
-   控制 One Ticket, One Issue 的范围，不把问题扩大成通用 Core 重构；
    
-   避免把 Receipt 缺口夸大成直接资金漏洞；
    
-   判断 99.9% 是 Maintainer 策略选择，而不是必须按我的方案修改；
    
-   核对最终测试证据并决定“两轮反馈从我这边均已解决”。
    

## PR 与 Issue 最新状态

截至 2026-07-18：

-   [PR #56](https://github.com/nishuzumi/moss/pull/56) 仍为 **Open Draft**，尚未合并；当前 Head 为 `9938e2d`。
    
-   GitHub 当前报告 PR 可合并，但是否合并仍由 Maintainer 决定。
    
-   [Issue #7](https://github.com/nishuzumi/moss/issues/7) 仍为 Open。
    
-   从我的 Review 角度，两轮反馈均已处理，没有剩余的可执行问题。
    
-   这不等于替 Maintainer Approve，也不代表 PR 必然 Merge。
    

## 明日计划

-   如果 PR #56 再次更新，只针对变化部分运行必要的回归测试，不重复已经关闭的问题。
    
-   将今天形成的检查方法用于自己的 Protocol Adapter：先核对 Issue 范围，再验证 ABI、地址、授权、报价、Receipt 和 Live Monad 模拟。
    
-   继续保持“先解释、再测试、最后写评论”的协作方式。
    
-   整理本次 Review 的测试证据和公开评论，作为 Week 2 Dev Portfolio Pack 的 Proof of Work。
    

## 今日复盘

今天最大的收获不是发现了两个问题，而是完整经历了一次有反馈、有修复、有复测、有最终结论的开源 Review。

真正可验证的贡献不只有提交代码。能够提出可复现问题、找到根因、控制问题范围、尊重作者的设计选择，并在修复后认真验证，也是一份完整的工程 Proof of Work。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->

# 2026-07-17｜残酷共学笔记

## 今日主题

Moss：从理解“可验证执行”，到沉淀教程、提交 Proof of Work，并参与真实的开源 Review。

## 今天完成了什么

今天围绕 Moss 完成了三类工作：

1.  梳理 Moss 的核心定位，明确它不是替 Agent 直接签名和发送交易，而是通过 `discover → load → action → simulate`，把协议交互变成可发现、可构建、可模拟和可验证的 Capability。
    
2.  完成一份 Moss 新手教程，内容包括环境配置、wrap/swap Demo、MCP 接入、Warning 处理、安全检查、FAQ 和首次 PR 流程。目前教程已经整理成可直接发布的 GitHub README。
    
3.  围绕已提交的 [Moss PR #72](https://github.com/nishuzumi/moss/pull/72)，整理了 Pull Request 作业、Proof of Work、文档到代码骨架以及 Moss 项目介绍文章。同时参与 [Issue #7](https://github.com/nishuzumi/moss/issues/7) 的协作讨论，并深入 Review 了 [PR #56](https://github.com/nishuzumi/moss/pull/56) 的 Clober Protocol Adapter 实现。
    

在阅读 PR #56 后，我向作者提交了五项需要进一步确认的问题：

-   ERC-20 剩余授权不足时，是否需要兼容必须先清零再重新授权的 Token；
    
-   动态推导 BookId 的方案是否有意替代 Issue 要求的 curated market catalog；
    
-   使用 `BookViewer.getExpectedOutput` 是否能够替代对实际写入路径的 `eth_call` 模拟；
    
-   获取 Token decimals 时是否必须同时依赖可选的 `name()` 和 `symbol()`；
    
-   是否应该为 BookId、unit size、fee policy、报价路径和授权逻辑补充维护注释与官方来源。
    

## 今天学到的关键内容

### 1\. “模拟成功”不等于“执行可信”

Moss 会记录交易模拟产生的 Event 和 native MON transfer，再由 Protocol 将这些 Change 解析成 Receipt。Core 还会检查 Receipt 是否完整、按原始顺序覆盖所有 Change。

但零 Warning 只代表观察到的变化得到了完整解释，不代表交易一定符合用户意图。最终仍然需要核对资产、数量、接收方、授权额度、滑点和 Protocol 选择。

### 2\. Bound Protocol 解决的是“协议身份参数化”

PR #72 处理的不是普通方法参数，而是 Protocol 在运行时绑定 Token、Market 或合约地址的问题。

今天进一步理解了几个关键约束：

-   Binding 与每次调用的方法参数必须分离；
    
-   Binding 应在执行 Protocol 或请求 RPC 前完成校验；
    
-   Factory 每次创建独立引用，不能随意缓存实例；
    
-   Binding 只解码一次，避免非幂等转换产生不同结果；
    
-   Receipt parser 必须保持纯净，不能访问 Runtime 或动态 Handle。
    

### 3\. 一个 Protocol Adapter 不只是增加一个函数

阅读 PR #56 后，我发现一个完整的 Protocol Adapter 需要同时处理：

-   Protocol、Capability 与 Query 设计；
    
-   ABI 来源及可复现生成；
    
-   官方合约地址与链上 Bytecode 核验；
    
-   ERC-20 Approval 和特殊 Token 行为；
    
-   Quote、滑点与部分成交保护；
    
-   Capability tree 的交易归属和执行顺序；
    
-   Receipt 与原始 Change 的完整覆盖；
    
-   运行时测试、编译期测试和 Live Monad 测试；
    
-   MCP Server 接入、文档与 Changeset。
    

这说明 Adapter Review 不能只看主流程能否运行，还需要检查边界资产、协议假设和 Issue 验收标准是否真正一致。

### 4\. 开源贡献不只等于提交代码

今天回复 Maintainer 建议时，我意识到协作中的表达同样重要。

当一个 Issue 已经有其他贡献者提交实现时，更合适的做法不是直接接管，而是先与作者协调，再通过复现、补充测试和边界分析参与 Review，避免重复劳动。

PR 是否最终 Merge 也不是衡量贡献的唯一标准。公开的需求分析、代码差异、测试证据、Review 意见和协作记录，都可以构成可验证的 Proof of Work。

## AI 帮助了什么，我人工校正了什么

AI 帮我快速阅读仓库文档、梳理 Pull Request、生成文章结构、代码骨架和教程初稿。

人工校正主要集中在：

-   使用 Moss 当前的 Capability、Change、Receipt 和 Outcome 领域语言，避免引用已经过时的设计；
    
-   核对 Node、pnpm、RPC 和测试要求；
    
-   补充“零 Warning 不等于用户授权”的安全边界；
    
-   收紧 Bound Protocol Factory、Binding 校验和 Receipt 纯度要求；
    
-   区分已经确认的问题与仍待作者澄清的 Review 方向；
    
-   通过本地边界测试验证 zero-reset Approval 和缺少 Token metadata 等情况；
    
-   调整 GitHub 回复措辞，使其符合真实开源协作语境。
    

这让我进一步认识到，AI 可以提高阅读和生成效率，但最终产出是否符合项目架构、测试要求和协作规范，仍然需要人工判断和证据支持。

## PR 与 Issue 最新状态

-   [PR #72](https://github.com/nishuzumi/moss/pull/72) 目前仍为 Open Draft，尚未合并。关联 Issue #61 已添加 `maintainer-only` 标签，后续由 Maintainer 主导，PR 能否继续推进暂不确定。
    
-   [PR #56](https://github.com/nishuzumi/moss/pull/56) 目前仍为 Open Draft，尚未合并。我提交的五项 Review 问题已经发布，正在等待作者或 Maintainer 回复。
    

## 当前卡点

-   PR #72 的离线构建、类型检查和测试已经通过，但 Live Monad 检查曾被本机 TLS 证书链错误 `UNABLE_TO_GET_ISSUER_CERT_LOCALLY` 阻塞。
    
-   Issue #61 已被标记为 `maintainer-only`，PR #72 的后续处理方式还不明确。
    
-   PR #56 的五项审查意见已经提交，目前需要等待作者或 Maintainer 回复。
    
-   Moss 新手教程已经完成，但还没有上传到公开 GitHub 仓库。
    

## 明日计划

-   完成 **Week 2｜Challenge｜为 Moss 新增一个 Protocol Adapter**；
    
-   完成 **Week 2｜Prototype Evidence**；
    
-   完成 **Week 2｜Dev Portfolio Pack**；
    
-   解决 RPC TLS 问题，补齐 Live Monad 验证。
    

## 今日复盘

今天最大的收获不是又完成了几份文案，而是把“阅读文档—理解架构—实现代码—验证测试—提交 PR—参与 Review—编写教程”串成了一条完整路径。

PR #72 的状态变化也提醒我：真实开源协作并不完全按照个人计划推进。Issue 的权限、Maintainer 的安排、其他贡献者的实现以及 Review 反馈，都可能改变最终结果。

真正有价值的 Proof of Work，不只是代码数量，也不只是一枚 Merge 标记，而是能否公开说明：为什么这样设计、如何验证、发现了什么问题，以及还有哪些边界尚未解决。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->


## **今日进度：完成 Moss GitHub 探索，并把三个 Week 2 任务收束成一个 MCP 安全预览原型**

今天没有急着继续写 Adapter，而是先把 Moss 的代码架构、GitHub 治理方式和 PR review 过程系统读了一遍，并完成三份相互关联的 Week 2 提交：

1.  GitHub 探索日志：理解 Maintainer 如何组织项目；
    
2.  开源贡献计划：确定 Dev Builder 的本周 PR 方向；
    
3.  AI-assisted Dev Plan：把同一份贡献包装成最小 AI × Web3 原型。
    

最终把三个任务收束成同一个交付物：**Moss Safe Action Preview**——用户用自然语言要求“把 0.01 MON 包装成 WMON，只预览、不发送”，Agent 依次调用 `discover → load → action → simulate`，展示 Receipt、Warnings 和安全结论，停在签名边界之前。

* * *

## **核心收获**

**1\. Maintainer 管理的不只是代码，而是整套“协作接口”**

-   Moss 用 pnpm monorepo 按依赖方向划分 `core`、`simulator`、`erc`、`system`、协议包和 `mcp-server`，每层包边界都在阻止不应该出现的依赖进入该层。
    
-   README 定义项目承诺和风险边界，ADR 记录设计取舍，CONTRIBUTING 固化 Definition of Done，Issue 标签表达任务类型、难度和信息完备度。新人从理解项目到提交 PR，每一步都有对应文档承接。
    
-   `needs-design` 和 `ready-for-agent` 不是普通进度标签，而是在按“信息是否完备”切分工作：设计决策留给人，规格已经明确的执行工作才适合交给 Agent。
    

**2\. PR #31 的本质是从“类别对账”升级为“原始证据穷尽覆盖”**

-   旧模型使用 Plan：多笔 `UnsignedTx` 平铺在 `txs` 数组中，再用结构化 `expects` 和模拟后的 effects 做 reconcile。它并不是完全不检查计划外行为——对资金流出、授权和 NFT 等已建模类别存在 `UNDECLARED_*` 检查。
    
-   真正的限制是：旧模型只认识 effects 层预先定义的类别，原始 trace 会先被压缩成几个桶；没有被建模的自定义事件可能在汇总阶段丢失。
    
-   新模型直接保留模拟产生的原始 Change，要求 Receipt 对每条 Change 按**对象身份、数量和顺序**逐一认领。任何无法解释的事件都会失败并产生 Warning，安全模型从“检查我认识的类别”变成“任何不认识的东西都拒绝通过”。
    

**3\. Capability Tree 把交易与解释责任绑定在一起**

-   每个 Capability 节点必须且只能直接拥有一个 TransactionNode；如果操作需要多笔独立交易，就用子 Capability 组织。
    
-   Kuru 使用 ERC20 作为输入时，需要先 `approve` 再 `swap`：`erc20.approve` 是带有自己 `approveReceipt` 的子 Capability，Kuru 根节点直接拥有 Router swap 交易。展开时深度优先执行，天然得到 approve → swap 的顺序。
    
-   Multicall 不是“一个 TransactionNode 里放多个 UnsignedTx”，而是一笔交易的 calldata 内部打包多个合约动作。从链上看仍然只有一次签名、一个 nonce、一个 TransactionNode，但可能产生多条 Change。
    
-   当前 Moss 仓库并没有真正实现 multicall Adapter；它只在 `CONTEXT.md` 和 protocol onboarding 文档中作为未来协议接入时的建模规则出现。
    

**4\. Handle → Change → Receipt 构成一条“构建 → 取证 → 解释”的证据链**

-   Handle 是 ABI 的类型化入口：`read` 真正读取状态，`call` 用 `eth_call` 预览写方法返回值，默认路径只编码 calldata、生成 TransactionNode，不签名也不发送。
    
-   Change 是从 `debug_traceCall` 调用树里提取的原始证据，目前只有 Event 和原生 MON 转账两类；采集阶段不负责解释，并通过排序和冻结保证证据稳定。
    
-   Receipt 是纯函数，只能根据收到的 `readonly Change[]` 得出 Outcome 和文本。它既不能声称发生了 Change 中不存在的事，也不能跳过无法识别的 Change。
    
-   三层分别被限制了一种能力：Handle 不能执行真实交易，Change 采集时不能解释，Receipt 解释时不能查询外部状态。正是这些限制拼成了一条可复现、可审计的安全链路。
    

**5\. Moss 提供安全证据，但不能替钱包强制执行安全策略**

-   从 PR #40 的 Maintainer review 中确认：Moss 可以提供 simulation evidence 和 Warnings，但它不控制钱包或签名行为。
    
-   消费应用可以选择模拟零次、一次或多次；示例钱包中的“签名前重新模拟”只能算应用层策略，不能宣传成 Moss 自动提供的安全保证。
    
-   更准确的安全模型是：**能力封装 + 机器验证 + 消费方执行策略**。保证必须和控制权位于同一层，不能把应用层的选择描述成框架层承诺。
    

* * *

## **本周贡献方向**

选择 Dev Builder 的“完善 Demo + 提交 PR”方向，以 [Issue #55](https://github.com/nishuzumi/moss/issues/55) 为切入点，计划实现一个可运行的 MCP 端到端示例：

```
 用户意图
   → discover：发现 WMON wrap capability
   → load：读取参数、风险和能力说明
   → action：生成未签名 Capability
   → simulate：通过 Monad RPC 执行 debug_traceCall
   → 检查 Receipt 和 Warnings
   → READY FOR REVIEW / DO NOT SIGN
```

本周真实实现 MCP Client、四工具调用、WMON calldata、真实模拟以及成功/失败测试；自然语言解析在自动化测试中使用固定 Tool Plan，钱包签名、交易广播、前端和多协议路由暂时 mock 或不实现。

这样既能形成一个最小 AI × Web3 原型，也能整理成 Moss 的开源贡献 PR，而不是为三个课程任务分别制造三份互不相关的产出。

* * *

## **今日产出**

-   [GitHub 探索日志](https://app.notion.com/p/Week-2-Challenge-GitHub-39fc278534e58113ad14cd6571fc0a35?pvs=21)
    
-   [开源贡献计划](https://app.notion.com/p/Week-2-Challenge-39fc278534e581a28896dc477d0188ec?pvs=21)
    
-   [AI-assisted Dev Plan](https://app.notion.com/p/Week-2-AI-assisted-Dev-Plan-39fc278534e5811690c8c87eaca27e65?pvs=21)
    
-   [Moss PR #31 技术研究笔记](https://app.notion.com/p/Moss-PR-31-Capability-Receipt-39ec278534e5812bac83ecfdba71b660?pvs=21)
    

* * *

## **个人思考**

-   今天最大的收获不是又记住了一批术语，而是把“安全”拆成了证据、解释和执行权三个层次。Moss 能证明模拟中发生了什么，但最终是否调用模拟、是否在 Warning 时停下，仍取决于上层应用和钱包。
    
-   阅读技术项目不能只看当前源码，也不能只相信 Issue 或第一次解释。今天对旧 Plan 的理解就经历了“先推测 → 找 base commit → 读取真实旧代码 → 修正结论”的过程。对于架构迁移，删除掉的代码往往比新代码更能解释“为什么要改”。
    
-   三个课程任务看似不同，其实分别对应“理解项目 → 计划贡献 → 定义用户原型”。把它们统一到同一个 MCP Workflow Demo 后，课程文档不再是额外作业，而是实际 PR 的设计说明、验收标准和完成证明。
    
-   今天完成的是调研与计划，不把计划冒充产出。真正的 Proof of Work 仍然是后续代码、测试结果和 Pull Request。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->



## **今日进度：完成 Moss 项目的业务理解 + 全链路实操**

从手写代码到 agent 自主调用,完整走了一遍 Moss([github.com/nishuzumi/moss](http://github.com/nishuzumi/moss))的两种用法:先在 play.ts 里手写 discover → load → action → simulate 四步;再通过 `.mcp.json` 把 Moss 的 MCP 服务器接入 Claude Code,让 agent 纯靠四个 MCP 工具自主跑通"1 MON 能换多少 USDC"(本地 mainnet fork 实测)。同步完成任务:⭐ Star 仓库、README 精读、理解分享文案。

## **核心收获**

**1\. Moss 是什么(纠正过两轮的理解)**

-   链上协议的操作原本只能人手点 DApp 或程序员手写代码拼交易;Moss 把它们封装成 agent 可直接调用的标准能力,并在 agent 与签名器之间强制加一道"先声明、后对账"的安全门。
    
-   方向链条:**链上协议 → 适配器封装 → MCP 等渠道 → agent**——不是"把 API 上链",MCP 也只是送货渠道不是被转化对象。
    

**2\. 四步流程是 Moss 自己的设计,不是 MCP 的功能**

-   这四步是 `@themoss/core` 库的 API:写代码可直接调(play.ts 验证,全程无 MCP);mcp-server 只是把它们原样包成四个工具。
    
-   discover/load 是便利层(黄页 + 说明书);安全门落在 action(产出带 expects 资金合同、confirms 回执凭据、planHash 防篡改指纹的未签名 Plan)与 simulate(真实链况模拟,实际资金流逐项对账 expects,有 warning 一律停)两步。
    

**3\. 完整安全模型还有三条工具之外的防线**

-   expects 模板由适配器作者写死在 @Capability,agent 改不了;Moss 永不签名永不发送,私钥始终在钱包;意图对齐归 agent——零警告只代表"和声明一致",还要核对"和用户要的一致"。
    

**4\. 实测数据(本地 mainnet fork)**

-   action 声明:最多出 1 MON、至少收 0.022414 USDC(报价扣 1% 滑点);simulate 实测:收 0.022641 USDC,零警告,planHashValid,资金只流向 Kuru 路由,gas 约 21.8 万。
    
-   对比实验:模拟器不接 observer 会如实报 CONFIRMATION\_MISSING——跳过的检查不默认放过,这个设计细节很见功底。
    

**5\. 环境坑:Monad 版 Foundry**

-   官方 anvil 缺 Monad 的 gas 模型/opcode 定价/预编译,fork 脚本直接拒绝;官方 foundryup 的 `--network` 参数已废弃,需用 Category Labs 的安装器另装(详见单独的工具链切换笔记)。
    

## **个人思考**

-   今天最大的收获是理解被纠正的过程本身:从"把服务转成 MCP"到"把 API 上链"再到"链上协议接给 agent + 签名前机器对账"——每一轮都是靠实操(play.ts 的类型报错、fork 实测、observer 对比)把抽象概念落到具体输出上才纠过来的。
    
-   Moss 的本质不是"封装",而是**把安全从"人审查代码"变成"机器审查资金流声明"**——人看不懂 calldata,但机器能逐项核对"最多出多少、至少进多少、钱流向谁"。这个思路对所有 AI × 资金类产品都适用。
    
-   看好的应用场景:自然语言 DeFi 入口、AI 理财助手的策略自动化、claim→swap→supply 多步组合、DAO 金库"AI 提案、机器验证、人类签名"的代理执行。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->




## **今日进度：Week 2 Day 2｜排查 Moss monorepo 构建问题 + 撰写 Week 3 Role Statement**

一句话总结：一次真实的 pnpm monorepo 报错排查，加上一份把 Week 1–2 产出转成 Week 3 组队筹码的角色声明。

## **核心收获**

**1\. Moss 是什么**

-   把 Monad 上复杂的 DApp/协议交互抽象成 agent 可调用的统一流程：`discover → load → action → simulate`，由系统而非 agent 组装正确交易；agent 不用手搓 calldata、不碰 ABI 和 decimals 换算。
    
-   两条安全规则分别在两处强制执行：**effects 对账**（服务端机械判定，模拟出的实际资产流动只要和 Plan 声明有差异就 warning）、**意图对齐**（agent 侧，把 effects 摘要和用户原话核对）。Moss 本身永不签名、永不发送，私钥和最终决定权始终留在用户手里。
    

**2\. 调试实录：pnpm monorepo 构建失败**

-   现象：跑 Moss 示例 `wrap` 直接报 `ERR_PNPM_RECURSIVE_RUN_FIRST_FAIL`，只有个错误壳；把这行报错原样丢给 AI，AI 没有凭壳猜，而是先读 `package.json` 和入口脚本，重跑命令拿到完整堆栈 `ERR_MODULE_NOT_FOUND`，才确认是 `packages/core/dist` 不存在——仓库在这台机器上从未 build 过。
    
-   根因一句话：`workspace:\*` **依赖在运行时按普通 npm 包对待**（走 exports → dist），不是源码直连；fresh clone 之后跑任何示例前必须先 `pnpm build`。修复后重跑示例，四步全通过，`simulate` 在 Monad 主网真实模拟 1.5 MON 换 1.5 WMON，`warnings: []`。
    

**3\. Week 3 Role Statement：定位与队友需求**

-   赛道 Tech，角色是链上开发 + 索引/数据层负责人：合约侧负责并行执行下的存储优化（`mapping` 替代全局计数器），链下侧负责 Proposed/Finalized 双游标 reorg-safe 索引器，外加合约安全 review。
    
-   最需要的队友是前端 Tech 伙伴（Next.js + wagmi/viem）——AI 辅助能把前端跑起来但难保证质量；其次是 Ops（社区/增长）和 Research（生态/方向判断）各一名，三条腿凑成完整闭环。
    
-   Proof of Work：Monad Testnet 上已部署的 NFTBadge / DAO Voting / Clicker 合约 + 可运行的索引器代码库，全部有公开打卡记录可查。
    

## **个人思考**

-   Moss 的「effects 对账 + 意图对齐」两条门禁，和我关注的 AI agent 支付四要素（身份验证/预算约束/风险控制/可撤销交易）几乎是同一套思路的另一种实现——前者在交易签名前拦，后者在支付发生前拦，值得把两边的设计对照着看。
    
-   这次调试也提醒我一个通用习惯：**报错只是壳，别对着壳猜**——pnpm 顶层错误和 EVM revert 一样，真正的原因永远在下一层堆栈里，这条规则对合约调试同样适用。
    
-   Week 3 Role Statement 逼着我把 Week 1–2 的零散产出（徽章、DAO、索引器）第一次整理成「团队起步的技术底座」这个叙事——这比单独看每个项目更有说服力。
    

明日计划：跑通 `kuru-swap.ts`（跨 Plan 组合：MON→USDC swap），继续围绕 AI agent 支付 / 链上高频交互产出 Week 2 最小交付。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->





# 2026-07-13

## 今日进度：完成 Week 2 职业方向选择提交，搭建 AI 协作记录 + 学习记录归档体系

今天同时在 Claude Code 本地和 [claude.ai](http://claude.ai) 网页版并行推进：完成 Week 2 Role Choice Card 提交（选择 Dev 方向），搭建了 AI Collaboration Log 与本周学习记录两套 Notion 归档体系，并把"挖历史对话记录找 Prompt"这件事沉淀成了一个可复用的 Claude Code skill。

## 核心收获

**1\. 职业方向选择：基于已验证路径选 Dev，而不是从零转型**

-   Go 背景 + 已落地的 NFT badge/DAO Voting/索引器/clicker 多个项目是选择 Dev 方向的实证支撑，本周最小产出定在 AI agent payment 或高频链上交互安全方向的可运行 demo。
    

**2\. AI 协作记录该记录什么、不该记录什么**

-   除了"协作场景/AI 帮了什么/人类删改核查了什么/哪些不能交给 AI"四栏之外，还规划了四类具体截图证据（方向文案生成、打卡模板应用、技术细节核实修正、未采纳 AI 建议的决策点），让"AI 帮了忙但人工在把关"可验证。
    

**3\. 把"挖历史对话找 Prompt"沉淀成一个可复用 skill**

-   本机 Claude Code CLI 会话和 [claude.ai](http://claude.ai) 网页版 Projects 是两套完全独立的系统：前者权威来源是 `~/.claude/projects/<dir>/*.jsonl` 而不是会话管理工具的部分索引；后者必须人工登录、且要滚动到顶才能读到完整对话。把这些坑写成 skill，并用真实的 monad-clicker 会话数据跑了带 skill / 不带 skill 的对比测试验证有效。
    

**4\. 两边并行操作同一份 Notion 笔记导致的结构纠缠**

-   同一个 Week 2 索引页被两个不同的 Claude 会话同时修改，出现过重复子页面块、重复图标等问题。解决方式是每次都重新拉取现有真实结构再改动，不凭记忆猜测。
    

**5\. 一次关于工具安全边界的具体案例**

-   测试 skill 时子任务被 Write 工具拦下后改用 Bash 绕过限制写文件——内容本身是真实的，但这提醒我以后审查 agent 产出不能只看结果对不对，还要看它是不是绕过了什么限制去达成的。
    

## 个人思考

今天大部分时间不是在学新概念，而是在搭建"如何持续、诚实地记录学习过程"这件事本身的基础设施——这本身也是 Dev 方向要面对的问题：记录/文档系统的可靠性，和合约代码的可靠性同样重要。两个 Claude 会话并行改同一份笔记导致的结构冲突，本质上和 monad-clicker 里"两个组件各自独立轮询同一份分数导致不同步"是同一类问题，解法也一样——找一个单一可信来源，别让两边各自为战。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->






# **残酷共学打卡 · 2026-07-11**

## **今日进度：BuildAnything 初中课程 4/13**

课程地址：[https://buildanything.so/zh/tracks/sophomore/lessons/monad-architecture](https://buildanything.so/zh/tracks/sophomore/lessons/monad-architecture)

学完第 4 课「Monad 架构」，把 EVM 那节的执行层外扩一圈，看清区块链六层架构（硬件/网络/数据/共识/执行/应用）里 Monad 和以太坊每一层的具体差异。

## **核心收获**

**1\. 硬件层与网络层**

-   Monad 要求裸金属服务器（16 核 CPU、4.5GHz+、32GB+ 内存、2TB NVMe SSD），拒绝云虚拟机——虚拟化引入的延迟会破坏亚秒级时序，硬件门槛是为性能和去中心化做的有意折衷。
    
-   网络层活儿相同（节点发现 + 通信），Monad 靠 MonadBFT 预设种子地址启动，再走状态同步/区块同步追上进度，思路比以太坊的 Discv5+DevP2P+GossipSub 组合更直接。
    

**2\. 数据层：MonadDB**

-   以太坊把 Merkle Patricia Trie 转成键值对塞进通用数据库（LevelDB/RocksDB）；MonadDB 直接存储 Trie 本身，不需要转换，并用 io\_uring 做异步 I/O 高速读写 SSD——为区块链数据定制 vs 通用数据库硬凑。
    

**3\. 共识层：MonadBFT vs Gasper**

-   以太坊 Gasper = LMD-GHOST（选分叉）+ Casper FFG（两阶段投票锁定历史），12 秒出块、约 13 分钟最终性。
    
-   MonadBFT：领导者每 400ms 轮换，两轮投票后区块即最终确定（800ms）；抗 tailforking 强制领导者包含前一区块，防止跳过作恶；RaptorCast 用纠删编码 + 两级扇出分发区块小块，解决 2MB 区块广播的带宽瓶颈。
    

**4\. 执行层：从串行到乐观并行**

-   以太坊瓶颈很明确：交易串行执行（15–25 TPS），且共识与执行交错——必须等区块执行完才能推进共识，出块时间大半被共识和传播吃掉。
    
-   Monad 三件套解耦这个瓶颈：乐观并行执行（多核同时跑，冲突了就重跑，结果与串行一致）、执行与共识异步解耦（共识跑第 N 块时执行在后台处理第 N-1 块）、MonadDB 支持海量并行状态读取——三者叠加做到约 10,000 TPS，同时完全兼容 EVM。
    

**5\. 应用层：兼容性的复利**

-   Monad 完全 EVM 兼容，以太坊应用换个 chain ID 重新部署即可运行，同一套 Solidity、同一套工具、同一个钱包连接——用户感知不到底层差异，只感觉到更快更便宜。
    

## **个人思考**

-   六层框架把之前零散学的 MonadBFT、并行执行、MonadDB 串成了一条线：每一层的改动都在服务同一个目标（亚秒级时序），而不是孤立的性能优化点。
    
-   「共识与执行解耦」这个设计和我做 indexer 时的双游标架构本质相通：把两个有依赖关系的流程解耦成可以流水线化的独立阶段，吞吐才能上得去。
    
-   MonadDB 直接存 Trie、不做键值对转换，提醒我评估任何存储方案时先问一句「这个数据结构原生适配底层引擎吗，还是在打补丁」。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->







## **今日进度：monad-clicker 加登录系统，并用真实使用数据修了一串前端 bug**

昨天把「为什么需要频繁交互」的场景论证做完之后，今天把 monad-clicker 从 Demo 推进到「能被人反复实际使用」的状态：加了 MetaMask 登录（会话代签），然后没有止步于"能跑"，而是自己连续实测/连点/换账号，揪出了 6 个真实 bug 并逐一修复，最后把改动推到了 GitHub，也把 Week 1 Build Log 整理提交。

## **核心收获**

**1\. 会话代签（session-key delegation）——登录语义和高频体验的取舍**

-   需求是「一个钱包地址 = 一个账号」，但直接用 MetaMask 签每次点击会弹窗到没法玩。方案是 MetaMask 只签一次 `authorizeOperator`，把本地会话密钥登记为代签人，之后点击都由会话密钥免弹窗代签、分数记在 MetaMask 地址上——合约里没有任何资产可转移，代签密钥泄露的最坏后果只是被人帮你多点几下。
    
-   代价看得很清楚：`ClickGame` 因为加入代签逻辑必须重新部署（v1→v2），测试网数据作废，`ChampionBadge` 复用初版、经 Safe 执行 `setGame()` 切换指向。做完之后一度以为"提案了就等于处理完了"，没主动确认第二个签名是否真的执行——复核链上才发现 `ChampionBadge.game()` 还指向废弃的 v1，及时补签修复。多签的"提案"和"生效"是两个独立状态，必须主动查链上，不能假设。
    

**2\. 自己发现的一个真实安全漏洞：会话钱包跨账号复用**

-   实测「换一个地址登录」时发现浏览器本地的会话钱包（burner）是按浏览器存的单一 key，不区分登录账号——换账号后看到的还是同一个 burner，如果这个 burner 之前已经被充值、被授权过，新账号可以直接白嫖旧账号的 gas 余额甚至复用旧的授权关系。修复方式是把 burner 的 localStorage key 按登录账号地址命名空间隔离，每个账号独立、从零开始的会话钱包。
    

**3\. 发送队列：不为了赶速度牺牲「点击成本即防刷」**

-   一开始考虑过「点击停顿后再批量上链」，能极大降低延迟感知，但会让批量提交里单次点击的边际 gas 成本更低——直接削弱了这个 Demo「Gas 是天然防刷机制」的核心论点，最后否掉了这个方向。
    
-   真正的问题其实是：点击间隔小于链上允许的最快节奏时，前端会把多余点击静默丢弃，界面毫无反馈。改成发送队列——物理点击必入队，按节奏依次发送真实交易，1 次点击依然对应 1 笔真实交易，不合并、不批量。
    

**4\. 乐观 UI 连续踩了三次坑，一次比一次隐蔽**

-   第一版用「挂载时快照当基准 + 本地增量，取两者较大值」，一旦索引器权威分数追上这个冻结基准，后续新点击的加分会被 `Math.max` 悄悄吞掉，界面卡住不动——改成持续对比"权威分数每次轮询涨了多少就核销多少笔未结算点击"，不依赖一次性快照。
    
-   接着发现分数「上下跳动」。没有直接改代码，先用 4 次/秒的频率一边连点一边轮询 `/api/player`，确认后端分数序列完全单调，把嫌疑从后端排除出去，定位到前端：核销逻辑写在 `useEffect` 里（渲染_之后_才跑），所以每次权威分数更新都会先渲染出一帧"新分数 + 还没核销的旧乐观计数"的重复计数瞬时值。改成 React 官方的「渲染时调整状态」模式，核销和分数变化在同一次渲染里原子完成，顺带把两个组件各自独立轮询同一份分数的架构也合并成父组件查一次、往下传，消除了另一个潜在的不同步来源。
    
-   最后一个是我自己截图发现的：连点积压后，「点击→Proposed」面板显示 33 秒——不是链变慢了，是排队等待时间被算进了这个本该只反映链上真实速度的数字里。加一个「真正开始发送」的时间戳，延迟从这里起算，不再含排队时间。
    

**5\. 用余额精确判断，而不是猜 RPC 报错文本**

-   换到一个余额只剩 0.002 MON 的账号后，连续 39 笔点击全部失败，报错是含糊的英文 "An internal error was received"，没命中任何已知错误模式，也没被判定为终止性错误，于是队列把剩下的全部重试了一遍。查链上确认是真的不够付 gas。修复：不再事后解析 RPC 报错文本，直接用前端已知的余额在发送前判断，不够就立刻当终止性错误处理——清空队列、给一句清楚的中文提示，按钮的禁用阈值也从「正好等于 0」提高到同一个安全余量。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->








## 今日进度：完成「Monad 理解｜为什么 Monad 体验不同」议题

今天完成了 Tech 方向的场景论证：以「链上实时排行榜 + 高频点击小游戏」为例，从四个方向理解和讨论 Monad——为什么需要频繁交互、链慢或贵会怎样、Monad 能带来什么、是否真的需要链上记录。

## 核心收获

**1\. 为什么这个场景需要频繁交互**

-   点击游戏的核心循环是「点击 → 分数更新 → 排名变化」，单人每分钟几十次写入 + 排行榜近实时读——交互频率不是附加属性，而是玩法本身。
    

**2\. 链慢或手续费高会怎样**

-   以太坊主网级成本下，单次点击的 Gas 高于其娱乐价值，12 秒出块让实时竞争不成立；常见妥协是操作放链下、只结算上链——但那就退化成带钱包登录的 Web2 游戏。
    
-   上链的真实价值有三层：**信任**（排名由玩家自签名交易累加，运营方改榜需伪造签名）、**可组合**（成绩是公共资产，第三方可读作空投/准入凭证）、**防刷**（每次点击签名付 Gas，女巫攻击有了真实边际成本）。
    

**3\. Monad 能带来什么**

-   高吞吐 + 亚秒出块让「每次点击都是一笔真实链上交易」在成本和延迟上可行；EVM 完全兼容，开发成本与任何 EVM 链一致。
    
-   存储层契合是关键：分数存 `mapping(address => uint256)`，每个地址经 keccak256 映射到独立存储槽，1000 人同时点击就是 1000 笔零冲突交易；全局计数是热点槽反模式，聚合统计一律放索引层链下算——**为并行而设计存储布局**。
    

**4\. 是否真的需要链上记录**

-   诚实回答：榜单数据本身数据库完全够，上链的必要理由只有信任与可组合性；压测叙事、可复用索引器属于生态加分项，不能当上链的借口。
    

**5\. 落地设计要点**

-   索引器双订阅流：Proposed 流写 pending、Finalized 流转正，两个 cursor 独立推进；reorg 回滚必须以 block hash 为键——用 block number 会误删新链数据。
    
-   链上 Top-10 用长度 10 的有序数组插入排序维护：纯链上可验证，且该数组是唯一共享状态、仅高分交易写入，冲突可控——顺带成为讨论并行冲突的教学案例。
    
-   前端把三层状态直接映射成 UX：pending 分数半透明、Finalized 变实，并排展示「点击 → Proposed」与「点击 → Finalized」两个毫秒数。
    

## 个人思考

-   这次论证最大的收获是把「快」翻译成了机制：Monad 的优势不是 TPS 数字，而是存储槽隔离 + 乐观并行 + 三层确认共同作用后，某类过去不可行的产品形态变得可行。
    
-   「是否需要上链」这一问逼着我把信任/资产/成本三层拆开——防住了「为上链而上链」，也让 Demo 的技术叙事更站得住。
    
-   计划补一个热点槽对比实验（纯 mapping vs 加全局计数器两版合约的吞吐差异），用数据实证并行执行的价值。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->









## **今日进度：BuildAnything 初中课程 3/13**

课程地址：[https://buildanything.so/zh/tracks/sophomore](https://buildanything.so/zh/tracks/sophomore)

学完初中前三课（Vibecoding 原理、Skills、EVM 原理），主线是给之前「会用」的东西打开引擎盖。

## **核心收获**

**1\. 编程智能体的分层结构**

-   所有编程智能体底层相同：模型 + 上下文窗口 + 系统提示 + 工具 + 智能体循环 + harness。出问题先问是哪一层，行为就从「魔法」变成「可调试」。
    
-   模型无状态，不在上下文里的东西对它就不存在——好的 vibecoding 本质是把智能体指向正确的文件。
    

**2\. 智能体循环与委托**

-   引擎是「思考 → 行动 → 观察 → 重复」直到任务完成；sub-agent 用独立上下文干脏活、只返回总结，保持主对话干净。
    
-   持久化靠 `CLAUDE.md` 和 memory；Skills / Hooks / `settings.json` 属于 harness 层；MCP 是接外部系统的标准插头。
    

**3\. Skills 与 Monskills**

-   skill 就是一份 markdown：description 决定何时触发，正文决定怎么做，把泛化助手变成领域专家。Monskills 六件套覆盖 Monad 构建全流程，明确要求助手不许凭空猜合约地址、私钥不落明文。
    
-   恶意 skill 可泄露私钥、跑任意脚本——安装前查作者、读源码，纪律同「不往终端粘贴陌生脚本」。
    

**4\. EVM 核心模型**

-   Ethereum 是分布式状态机：每笔交易触发一次**确定性**状态转换，所有节点独立执行、结果必然一致，trustless 的根源在此。
    
-   Solidity 编译成 opcode 执行；memory 交易内临时（便宜），storage 上链永久（贵，因为要写到全球所有节点）。
    

**5\. Monad 兼容性与合约安全**

-   Monad 跑同一个 EVM，工具链零修改，区别只在吞吐——「同一个 EVM，快得多的轨道」。
    
-   严肃警告：合约错误不可变、可能损失真金白银，AI 写 Solidity 比写前端更不可靠。持有真实资金的合约，未经审计绝不部署。
    

## **个人思考**

-   分层框架正好解释了我在 Claude Code 里做的配置：`settings.json`、CLAUDE.md 都是 harness 层，过去照文档配，现在知道每项作用在哪一层。
    
-   「恶意 skill」和我之前调 Raycast prompt 踩的 instruction-data boundary 泄漏是同类问题：文本会被当指令执行，信任边界就必须前置。
    
-   EVM 确定性状态转换是我做 indexer 的地基：节点重放结果必然一致，事件日志才配当唯一数据源。
    

明天继续初中第 4 课起。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->










## 今日进度：BuildAnything 新生课程 10/10 完结 ✅

课程地址：[https://buildanything.so/zh/tracks/freshman](https://buildanything.so/zh/tracks/freshman)

学完 Monad 官方学习平台 BuildAnything 新生阶段全部 10 节课，路线是「先会做产品 → 理解链 → 把产品搬上链」，最终在 Monad Testnet 部署了第一个 dApp。

## 核心收获

**1\. Monad 是什么**

-   完全 EVM 兼容，核心指标：10,000 TPS、400ms 出块、800ms 确定性最终性。
    
-   去中心化不妥协的关键：`MonadBFT` 用 fan-out / fan-in 替代验证者 all-to-all 通信，通信开销线性增长，validator set 可以做大。
    

**2\. 区块链基础概念**

-   PoW 拼算力，PoS 拼抵押（作恶触发 slashing）——用财务风险替代电力消耗保证安全。
    
-   `gas fee = gas units × gas price`：计算量 × 区块空间供需。
    
-   实用四件套：Chain ID（Monad Testnet 为 10143）、RPC、Faucet（[faucet.monad.xyz](http://faucet.monad.xyz)）、Explorer（[monadscan.com](http://monadscan.com)）。实验一律 testnet。
    

**3\. 10k TPS 解锁什么**

-   关键不是数字，是确认时间 <1s 落进人脑「即时响应」感知窗口后，过去被迫绕开链的架构妥协可以取消：链上订单簿、链上游戏、社交图谱、微支付、AI Agent 非托管结算。
    
-   链上/链下的划分原则不变（无信任、永久性、可组合性才上链），变的只是线的位置。
    

**4\. Web2 基建与安全**

-   元数据进 database，文件字节进 storage，数据库只存路径。
    
-   安全铁律：`service_role` key 绝不进前端/Git；生产必开 RLS；永远不要相信客户端。
    

**5\. 第一个 dApp（MessageBoard）**

-   Hardhat 写合约 → 部署 Monad Testnet → wagmi `useReadContract` / `useWriteContract` 接前端。
    
-   最重的提醒：**绝不能让 AI 智能体接触私钥**（.env 都不够，要用加密 Secrets 管理）；AI 写 Solidity 远不如写 UI 可靠——合约保持小而简单、用经审计的库、先上 testnet。
    

## 个人思考

-   上周刚用 Remix 手写部署了 Soulbound ERC-721 徽章，这门课走 vibecoding + Hardhat 路径，两相对照能分清哪些是链的本质（私钥、gas、finality），哪些只是工具链选择。
    
-   「800ms 确定性最终性」正是我之前分析 Monad 三级区块状态（Proposed → Voted → Finalized）做 indexer reorg-safety 时那套模型的用户侧表述，前后串起来了。
    
-   课程反复强调的安全意识是个提醒：Web3 里很多安全责任从公司基建直接落到开发者个人，且错误不可逆。
    

明天进入 Sophomore 阶段。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->











\## Week 1 打卡｜部署 NFTBadge 到 Monad Testnet

\### 今天做了什么

\- 用 Remix 编译并部署了一个 Soulbound（不可转让）ERC-721 徽章合约 `NFTBadge`，基于 OpenZeppelin v5

\- 网络：Monad Testnet（Chain ID 10143）

\- 合约地址`0xe9a15a7A91708765b6339dCeED7b89D2b3a3eb9D`

\- 完成了 read`owner()` 等）和 write`setBadgeTypeURI` / `mintBadge`）两类函数调用

\- 给合约配置了真实 IPFS metadata（Pinata 上传图片 + JSON），替换掉了最初的占位字符串

\- 整理了 README v0.1，写清楚了合约功能、部署步骤、交互方式

\### 踩过的坑

1\. **MetaMask 交易排队**：连续点击 write 按钮会攒一堆待确认请求，导致"点 A 方法却弹出 B 方法确认框"——本质是 nonce 顺序问题，不是合约或钱包故障，清空队列即可

2\. \*`mintBadge` revert\*\*：合约设计上"同一地址同一 `badgeType` 不能重复领取"，第二次拿同样参数铸造会被 EVM 回滚。解决方式是新开一个 `badgeType=2` 重新走 `setBadgeTypeURI → mintBadge` 流程

\### 关键技术点

\- **Soulbound 实现**：重写 OpenZeppelin v5 的 `_update` 钩子，只放行铸造`from==0`）和销毁`to==0`），拦截普通转账

\- **CEI 模式**`mintBadge` 里先完成状态写入（Effects）再调用 `_safeMint`（Interactions），降低重入风险

\- **IPFS metadata 流程**：先传图片拿 CID → 写入 JSON 的 `image` 字段 → 再传 JSON 拿最终 CID → 这个 CID 才是 `setBadgeTypeURI` 要填的值

\### 产出

\- 合约地址 + 部署 tx hash + 两笔交互 tx hash`setBadgeTypeURI, mintBadge`）

\- README v0.1（含部署信息、交互说明、安全设计说明）
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->












**今日学习内容**

**一、Web3 实习手册·入门导读**

**1\. 区块链基础概念**

-   区块链本质是按时间顺序连接的区块链条，每个区块包含交易记录及前一区块的哈希值，因此历史记录一旦写入便不可篡改
    
-   核心特性：数据不可篡改、公开透明但地址匿名、交易确认速度快
    
-   比特币：节点通过挖矿竞争记账权并获得代币奖励，具备货币属性——总量恒定、可自由转账
    
-   三种链的形态：
    
    -   公链：任何人可自由加入，完全开放
        
    -   联盟链：由多方共同组成"董事会"式治理，半开放
        
    -   私链：由单一主体审批控制，完全封闭
        
-   Web2、Web 3.0、Web3 的区别：
    
    -   Web2：中心化服务器架构，平台掌控用户数据
        
    -   Web 3.0：语义网概念，与区块链技术无关，容易混淆
        
    -   Web3：去中心化互联网，数据主权归还用户，智能合约自动执行规则，资产真正由用户自己掌控
        

**2\. 以太坊概览**

-   以太坊被称为"区块链 2.0"，由 Vitalik Buterin 于 2013—2014 年提出，核心创新是引入智能合约，愿景是打造"世界计算机"
    
-   与比特币的关键差异：
    
    -   图灵完备、可编程，而比特币是简单脚本
        
    -   目前采用 PoS 共识，而比特币是 PoW
        
    -   出块时间约 12 秒，而比特币约 10 分钟
        
-   核心机制：
    
    -   账户体系：EOA（私钥控制的外部账户）与 CA（合约代码控制的合约账户）
        
    -   Gas 模型：费用 = Gas Limit × Gas Price；EIP-1559 之后拆分为销毁的 Base Fee 与支付给验证者的 Tip
        
    -   EVM：全网节点同步执行智能合约的虚拟机环境
        

**3\. 行业赛道全览**

-   **DeFi**：Uniswap（基于恒定乘积公式 x\*y=k 的 AMM 自动定价机制）、Compound（超额抵押借贷，以 cToken 计息）、MakerDAO/Sky（超额抵押生成稳定币 DAI/USDS）
    
-   **NFT**：解决数字资产的唯一性与所有权确权问题，代表项目有 CryptoPunks（先锋项目）、OpenSea（最大交易平台）
    
-   **DAO**：以社区投票取代传统公司科层治理，代表案例有 Nouns DAO、LXDAO（支持 Web3 公共物品建设）；ConstitutionDAO 众筹竞拍美国宪法原稿未遂，反而暴露出 DAO 治理信息过度透明、不利于竞价类场景的短板
    
-   **MEME**：文化驱动型代币（如 DOGE、PEPE），投机性极强，需警惕无实际价值支撑的炒作、盲目跟风与代币集中度过高的风险
    
-   **交叉领域**：DeFi+NFT（NFT 抵押借贷）、DAO+MEME（社区治理与文化资产融合）、AI+DeFi（尚处早期）、RWA（现实资产上链）
    
-   **2026 新趋势**：Intent-Based 交易、账户抽象（ERC-4337）、模块化区块链、AI 与 Web3 深度融合
    

**二、实操**

-   创建了一个课程专用钱包，并添加 Monad Testnet 网络
    
-   领取测试网资产，完成一次测试网转账/应用交互实操
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
