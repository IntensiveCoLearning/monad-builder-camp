---
timezone: UTC+7
---

# Qy

**GitHub ID:** pillowtalk-Qy

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
## 今日打卡笔记：Moss 开源贡献记录

日期：2026-07-19  
项目：Moss  
仓库：[https://github.com/nishuzumi/moss](https://github.com/nishuzumi/moss)  
GitHub：[https://github.com/pillowtalk-Qy](https://github.com/pillowtalk-Qy)

今天继续深度参与 Moss 开源建设，重点围绕 **Adapter 基础能力、Core 安全边界、Simulator 验证、Query metadata 机制** 展开。今天的工作不只是提交代码，也包括对维护者 PR 的独立 review、rebase 风险分析和安全问题反馈。

### 今日主要贡献

1\. 更新 #41：Kuru Market Discovery 资源边界

PR：[https://github.com/nishuzumi/moss/pull/41](https://github.com/nishuzumi/moss/pull/41)

今天将 #41 rebase 到最新 main，并重新完成完整验证。这个 PR 主要为 Kuru market discovery 增加资源边界，避免 off-chain API 返回过慢、过大或候选市场过多时影响 Agent 的 quote / capability 构造流程。

今天新增了一个关键修复：原本 256 candidate cap 仍可能在 via-MON 路由组合时扩展成更大的 route fanout，因此我补充了 **256 route limit**，并增加 256 pass / 257 fail 的边界测试。

验证结果：

-   `pnpm lint` 通过
    
-   `pnpm build` 通过
    
-   `pnpm typecheck` 通过
    
-   `NODE_USE_ENV_PROXY=1 pnpm test` 通过
    
-   62/62 tests passed，包含 Monad mainnet WMON 和 Kuru live checks
    

2\. 更新 #83：ERC-4626 ABI Interface Layer

PR：[https://github.com/nishuzumi/moss/pull/83](https://github.com/nishuzumi/moss/pull/83)

今天将 #83 rebase 到最新 main，并重新完成完整验证。这个 PR 是围绕 #13 ERC-4626 interface layer 的第一阶段贡献，目标是先为 Moss 增加标准 ERC-4626 ABI 基础，而不是提前固定 vault identity、Protocol、Query、Capability 或 Receipt 设计。

这是一项 Adapter 前置能力建设。后续 Morpho、Euler、vault / lending / yield 类 Adapter 都可以复用这一层。

验证结果：

-   `pnpm --filter @themoss/erc gen:abis` 生成结果无 diff
    
-   `pnpm lint` 通过
    
-   `pnpm build` 通过
    
-   `pnpm typecheck` 通过
    
-   `NODE_USE_ENV_PROXY=1 pnpm test` 通过
    
-   57/57 tests passed，包含 Monad mainnet WMON 和 Kuru checks
    

3\. 提交 #101：Simulator Chained Transaction State 测试

PR：[https://github.com/nishuzumi/moss/pull/101](https://github.com/nishuzumi/moss/pull/101)

今天新提交 #101，补充 Moss Simulator 对多笔交易状态传递的测试。这个 PR 验证：前一笔 simulated transaction 的 post-state 是否会正确传递给后一笔 transaction。

这对 Moss 很重要，因为 Agent 经常会构造多步操作，例如 approve + action、wrap + transfer。如果 Simulator 不能正确模拟链式状态变化，用户看到的 simulation 结果就不可信。

#101 覆盖内容包括：

-   balance propagation
    
-   nonce propagation
    
-   bytecode propagation
    
-   storage propagation
    
-   cleared storage slot
    
-   state override preservation
    
-   state diff failure 时中止后续交易
    
-   live Monad mainnet WMON 两笔交易状态链路
    

4\. 提交 #102：Query Metadata Observation / OnChain Labels

PR：[https://github.com/nishuzumi/moss/pull/102](https://github.com/nishuzumi/moss/pull/102)  
对应 issue：[https://github.com/nishuzumi/moss/issues/63](https://github.com/nishuzumi/moss/issues/63)

今天在得到 Box 老师同意后，开始实现 #63 的 focused slice，并提交 #102。

这个 PR 的目标是让 Registry 可以从成功的 Query 中观察 token metadata，并用于 Receipt text 的 OnChain label 展示。设计上保持安全边界清晰：

-   metadata 必须通过显式 `tokenMetadata` helper 提供；
    
-   不从普通 Query 字段隐式推断 metadata；
    
-   只在 Query 成功后处理 observation；
    
-   cache 只存在于当前 Registry 生命周期；
    
-   Receipt parser 不访问 metadata cache 或 RPC；
    
-   OnChain label 只影响展示，不改变 outcome / data / Change evidence；
    
-   label 优先级保持为 `Trusted -> Package -> OnChain -> raw address`。
    

这个贡献和后续 ERC20 bound asset、ERC721 collection metadata、MCP label safety 都有关，是 Moss 可解释性和 Agent-facing 输出质量的重要基础。

### 今日 Review / Audit 贡献

1\. Review #98：Delegated Receipt Mutation Gap

PR：[https://github.com/nishuzumi/moss/pull/98](https://github.com/nishuzumi/moss/pull/98)

今天 review 已合并的 #98 时，发现 delegated Receipt protection 可能存在一个边界问题：当前机制能防止替换 delegated Receipt 或伪造 protocol provenance，但如果保留原始 object identity，仍可能 mutate delegated Receipt 的 text、outcome、changes 或 nested descendants。

我没有直接开 PR，而是先向维护者确认这个 invariant 是否需要 Core-enforced runtime protection。这是更稳妥的处理方式，因为它涉及 ADR 0011 对 Receipt evidence 的核心定义。

2\. Review #74：Bound Protocol Factories Rebase 分析

PR：[https://github.com/nishuzumi/moss/pull/74](https://github.com/nishuzumi/moss/pull/74)

今天 preview 了 #74 在最新 main 上的 rebase，确认主要冲突不只是文本或构造函数冲突，而是 #98 之后 Receipt type contract 发生变化。

我在评论里明确指出：#74 合并时不能简单恢复旧 Receipt alias，否则会破坏 #98 引入的 Core-owned Receipt provenance contract。这个 review 主要帮助维护者识别后续合并时需要保留的核心不变量。

3\. Review #91：Capability Tree Complexity 边界

PR：[https://github.com/nishuzumi/moss/pull/91](https://github.com/nishuzumi/moss/pull/91)

今天继续 review #91，并补充 rebase 和测试建议。我重点强调：

-   complexity validation 必须发生在 MCP recursive decoding 和 Simulator 前；
    
-   transaction value 需要 uint256 / 32-byte 边界；
    
-   calldata 需要 byte-aligned 校验；
    
-   后续 #74 引入 `CapabilityNode.binding` 后，binding 也要和 params 一起纳入 JSON complexity budget。
    

### 今日状态汇总

今天实际推进的内容：

-   #41 更新并完成 live verification；
    
-   #83 更新并完成 live verification；
    
-   #101 新提交；
    
-   #102 新提交；
    
-   #63 在得到 Box 老师同意后进入实现；
    
-   #74 / #91 / #98 完成独立 review 和风险反馈。
    

当前主要 PR 状态：

-   #41 open，等待 review；
    
-   #83 open，等待 review；
    
-   #101 open，等待 review；
    
-   #102 open，等待 review；
    
-   #43 / #44 / #45 / #48 已 merge，可作为此前 Proof of Work。
    

### 今日收获

今天对 Moss 的理解更进一步：Adapter 能力并不只是“接入一个协议”，还依赖 Core 层的安全模型、Simulator 的状态可信性、Receipt evidence 的完整性，以及 Agent-facing label 的表达质量。

今天的贡献集中在这些基础层：

-   #83 为 ERC-4626 vault Adapter 提供标准接口基础；
    
-   #101 验证多交易 simulation 的状态链路；
    
-   #102 建立 Query metadata 到 OnChain label 的安全通道；
    
-   #41 限制 Adapter discovery 和 quote 前的资源风险；
    
-   #74 / #91 / #98 review 帮助维护 Core 架构不变量。
    

下一步会继续跟进 #41 / #83 / #101 / #102 的 review，根据维护者反馈做小范围修正。如果 #83 被认可，后续可以继续推进 ERC-4626 query-only interface；如果 #102 被认可，可以继续配合 ERC20 / ERC721 metadata producer 和 MCP label exposure 的后续工作。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->

## 今日打卡笔记：Moss 开源贡献与 Adapter 架构建设

今天继续围绕 Moss 做真实开源贡献，重点从单纯提交 PR，扩展到 **Adapter 基础能力建设、核心架构 review、安全边界审查** 三个层面。

今天最重要的新增 PR 是：

**PR #83：feat(erc): add compiled IERC4626 ABI**  
链接：[https://github.com/nishuzumi/moss/pull/83](https://github.com/nishuzumi/moss/pull/83)

这个 PR 是围绕 #13 `Interface layer: ERC-4626 tokenized vaults` 推进的第一阶段贡献。由于 #13 仍然处于 `needs-design` 状态，而且 Moss 当前还在推进 bound Protocol、Query observation、Receipt labels、ERC20 bound asset 等架构工作，我没有直接实现完整 vault Adapter，而是选择了一个更稳的最小切片：先把 ERC-4626 标准 ABI 作为 `@themoss/erc` 的通用接口层补进去。

#83 的主要内容包括：

-   新增 `IERC4626.sol` 作为 ERC-4626 标准接口来源；
    
-   通过现有 Foundry / Wagmi ABI pipeline 生成完整 ABI；
    
-   导出 `ERC4626Abi`；
    
-   增加 ERC-4626 函数与事件 surface 测试；
    
-   增加 `Deposit` / `Withdraw` 事件解码测试；
    
-   增加 `Handle<typeof ERC4626Abi>` 类型推导测试；
    
-   增加 changeset，说明这是 `@themoss/erc` 的 patch 级更新。
    

这个贡献的价值在于：ERC-4626 是 vault、lending、yield 类协议的底层标准，后续 Morpho、Euler 或其他 vault Adapter 都可以复用这一层。它不是完整 Adapter，但它是 Adapter 能力的基础建设，避免每个协议重复维护 ABI，也避免在架构尚未确认前提前固定 vault 地址模型、Query、Capability 或 Receipt 设计。

今天还有一个重要结果是：

**PR #43 已合并：test: harden receipt coverage safety**  
链接：[https://github.com/nishuzumi/moss/pull/43](https://github.com/nishuzumi/moss/pull/43)

#43 是之前提交的 Receipt safety 测试增强 PR，今天已经完成 merge。这个 PR 加强了 Moss 对 Receipt evidence 的安全约束，包括：

-   空 `Receipt.text` / `ReceiptChange.text` 不应通过验证；
    
-   nested Receipt 必须保留原始 Change 的身份和顺序；
    
-   cyclic Receipt tree 应该被拒绝；
    
-   cyclic / non-JSON-safe payload 应该被拒绝；
    
-   reverted trace 不应调用 Receipt parser；
    
-   forged Receipt coverage 应在后续 gas estimation、state chaining、later transactions 前中止。
    

这对 Moss 很关键，因为 Agent 最终会把 simulation 结果解释给用户看，而 Receipt 是用户理解链上行为的证据层。如果 Receipt 可以为空、伪造或覆盖不完整，Agent 给出的解释就不可信。#43 的合并说明这部分安全边界已经被项目接受。

今天还做了几项 review / audit 型贡献。

第一项是对 **#63 / #64 架构方向** 做了提前理解和评论：  
链接：[https://github.com/nishuzumi/moss/issues/63#issuecomment-5009902247](https://github.com/nishuzumi/moss/issues/63#issuecomment-5009902247)

我在评论里梳理了自己对 #63 和 #64 的理解：

-   #63 应该在 Query 成功返回后处理显式、不可枚举的 Query observations；
    
-   Receipt parsing 仍然应该保持纯净，不能访问 cache 或 RPC；
    
-   #64 应该把 ERC20 identity 迁移到 validated binding；
    
-   native MON 应该拆成独立 Protocol；
    
-   Kuru 的 token-in/token-out 和 dynamic market 语义需要保留；
    
-   approval 应通过 bound ERC20 factory 组合。
    

同时我也明确说明：#63 被 #62 阻塞，#64 被 #61/#63 阻塞，而且它们是 maintainer-only，所以当前不直接开分支，而是等待前置架构落地后再接具体实现或 review。

第二项是对 **#74 bound Protocol factories** 做了独立 review：  
链接：[https://github.com/nishuzumi/moss/pull/74#issuecomment-5011793922](https://github.com/nishuzumi/moss/pull/74#issuecomment-5011793922)

这次 review 重点检查了：

-   malformed bindings 是否会在 Protocol 执行或 RPC 前失败；
    
-   重复和不同 bound instances 是否保持独立；
    
-   Capability、Query、Receipt dependency surface 是否分离；
    
-   CapabilityNode 是否把 binding 和 method params 分开保存；
    
-   Receipt parsing 是否保持 pure、binding-free；
    
-   类型推导是否有正向 fixture 和 `@ts-expect-error` 反向 fixture 覆盖；
    
-   docs、ADR、onboarding、template 是否描述同一套架构。
    

我还在本地独立跑了：

-   `pnpm lint`
    
-   `pnpm build`
    
-   `pnpm typecheck`
    
-   full live `pnpm test`
    

包括 Monad mainnet 的 WMON 和 Kuru 检查都通过。结论是 #74 本身没有发现 blocking issue，但我指出它和 #91 合并时需要注意：如果 `CapabilityNode.binding` 加入后，#91 的 complexity bound 也必须覆盖 binding，否则复杂度可能从 params 转移到 binding。

第三项是对 **#91 bound Capability tree complexity** 做了安全审查：  
链接：[https://github.com/nishuzumi/moss/pull/91#issuecomment-5011795587](https://github.com/nishuzumi/moss/pull/91#issuecomment-5011795587)

我独立 review 了 #91，并本地运行了 lint、build、typecheck 和 full live test。整体上，#91 的 iterative traversal、累计 params/calldata budget、cycle/shared-node rejection、depth-first order、MCP prevalidation 都是合理的。

但我发现了一个需要修复的边界问题：`assertTransactionNode` 限制了 calldata，但 `transaction.value` 在 `BigInt(value)` 之前没有 byte / uint256 bound。我复现了 33-byte value 被 `flattenCapabilityTree` 接受的情况，这意味着超长 hex value 可能绕过 complexity limit，并触发不受限制的 BigInt parse。

我还发现 odd-length calldata，例如 `data: "0x0"`，目前 Core 会接受，但发到 Monad mainnet `debug_traceCall` 会返回 `-32602 Invalid params`。因此我建议 #91 在 merge 前增加：

-   32-byte / uint256 transaction value 限制；
    
-   canonical hex quantity 校验；
    
-   calldata 必须是完整 byte；
    
-   32-byte pass / 33-byte fail 测试；
    
-   odd-length calldata rejection 测试；
    
-   MCP 在 Simulator 前拒绝非法输入的测试。
    

今天同步跟进的已有贡献状态：

-   #41：Kuru market discovery resource bound，仍 open  
    [https://github.com/nishuzumi/moss/pull/41](https://github.com/nishuzumi/moss/pull/41)
    
-   #43：Receipt coverage safety，已 merge  
    [https://github.com/nishuzumi/moss/pull/43](https://github.com/nishuzumi/moss/pull/43)
    
-   #44：cross-platform offline test command，已 merge  
    [https://github.com/nishuzumi/moss/pull/44](https://github.com/nishuzumi/moss/pull/44)
    
-   #45：halted MCP simulation projection test，已 merge  
    [https://github.com/nishuzumi/moss/pull/45](https://github.com/nishuzumi/moss/pull/45)
    
-   #48：Registry-owned Capability Receipts，已 merge  
    [https://github.com/nishuzumi/moss/pull/48](https://github.com/nishuzumi/moss/pull/48)
    
-   #83：compiled IERC4626 ABI，已提交，等待 review  
    [https://github.com/nishuzumi/moss/pull/83](https://github.com/nishuzumi/moss/pull/83)
    

今天的核心收获是：Moss 的 Adapter 建设不只是写某个协议包，还包括把 Adapter 依赖的底层标准、Receipt 安全、Capability tree 边界、binding 模型、MCP projection 都建设清楚。尤其是 ERC-4626 这种 interface layer，本身会影响后续 Morpho、Euler、yield vault 等多个 Adapter 的实现方式，因此更适合先做小而稳的基础 PR，而不是直接提交一个大而全的协议实现。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->


## **今日学习笔记：Moss Adapter 方向判断与开源协作推进**

今天主要围绕 Moss 的 Adapter Challenge 做了方向判断和开源协作规划。任务要求是为 Moss 开发一个新的 Adapter，并提交 Pull Request，包括 PR 链接、GitHub 主页、Adapter 名称和功能简介。

我没有急着直接写代码，而是先回到 Moss 仓库当前状态，重新核查 open issues 和 open PR，判断哪些方向已经有人在做，哪些方向还存在真实贡献空间。经过对比发现，Uniswap、PancakeSwap、Aave、Clober、FastLane、ERC-1155 等方向已经有对应 PR，因此不适合重复建设。相对来说，ERC-4626 Interface Layer、Euler v2 Lending Adapter、Pendle Yield Trading Adapter 仍然是较核心且尚未完全落地的方向。

今天最重要的判断是：**开源贡献不是抢着提交一个看起来很大的 PR，而是要先确认项目真正需要什么。** 尤其 Moss 近期刚完成 #48 相关架构调整，Capability Receipt 已经改为 Registry-owned，后续 Adapter 需要遵循新的 Receipt、安全验证和 simulation 规则。如果贸然提交一个未经确认的大型 Adapter，很可能会和维护者正在推进的架构冲突，造成返工。

在几个候选方向中，我重点关注了 #13 ERC-4626 tokenized vaults 和 #11 Pendle yield trading。ERC-4626 更像 Moss 的基础接口层，可以服务 Morpho、Euler、收益 vault 等多个协议；Pendle 则代表更复杂的收益交易场景，但涉及 category、verb、yield 语义等设计问题。因此这两个方向都不是简单复制模板就能完成的 starter adapter，需要先和维护者确认实现边界。

我已经在 #13 和 #11 下向维护者询问下一步具体动作是否合适，希望确认是否可以先从最小 scope 开始，例如 ERC-4626 先做 ABI + query-only interface，Pendle 先确认 category / verb 设计，再推进 Adapter 实现。目前维护者还没有回复，所以今天没有直接开 Adapter PR。我认为这是合理的，因为真实开源协作中，等待 maintainer 对设计方向的确认，本身也是负责任的协作方式。

同时，我也复盘了自己已经完成和正在推进的 Moss 贡献。虽然这些 PR 不都是新的 Adapter，但它们和 Adapter 体系密切相关，属于 Moss Adapter 能力建设的基础部分。

已合并的贡献包括：

-   [**#44**](https://github.com/nishuzumi/moss/pull/44)：新增跨平台 offline test command，让贡献者可以更稳定地在不同系统环境下运行离线测试。
    
-   [**#45**](https://github.com/nishuzumi/moss/pull/45)：补充 halted MCP simulation projection 测试，确保失败或中止的 simulation 不会被错误地展示成可执行结果。
    
-   [**#48**](https://github.com/nishuzumi/moss/pull/48)：让 Capability Receipts 由 Registry 管理，移除 serialized CapabilityNode 中可能被伪造或过期的 receipt 字段。这是 Moss 当前 Adapter 安全模型中的重要架构修正。
    

此外，还有两个正在推进、但尚未合并的 PR 也很重要：

-   [**#41**](https://github.com/nishuzumi/moss/pull/41)：fix: bound Kuru market discovery  
    这个 PR 主要是为 Kuru market discovery 增加资源边界和安全约束。Kuru 的市场发现会先从 off-chain API 获取候选 market，再通过 Router 的 on-chain verifiedMarket 进行验证。这个 PR 补上了 timeout、response size limit、candidate count limit 等保护，避免慢响应、超大响应或过多候选市场导致 discovery 阶段消耗过多资源。它对 Adapter 很重要，因为 Agent 在调用协议能力前，需要先安全地发现和验证市场。
    
-   [**#43**](https://github.com/nishuzumi/moss/pull/43)：test: harden receipt coverage safety  
    这个 PR 主要加强 Receipt coverage 的安全测试。它覆盖了空 Receipt 文本、嵌套 Receipt 顺序和身份保持、循环 Receipt tree、非 JSON-safe payload、reverted trace 不应调用 Receipt parser、伪造 Receipt coverage 应该提前中止等情况。它对 Moss 很关键，因为 Receipt 是 Agent 向用户解释链上操作结果的证据层，如果 Receipt 可以为空、伪造或覆盖不完整，就会影响用户对 Agent 操作的判断。
    

今天对任务提交策略也做了判断。当前阶段不急着提交最终版本会更合适。更好的节奏是：先等待维护者对 #13 / #11 的回复；如果得到确认，就基于确认后的最小范围开 PR；如果 deadline 临近仍未回复，再提交一版诚实说明，说明当前已完成 Adapter 方向选择、issue 沟通、方案拆解，但具体 PR 仍在等待维护者确认。

今天的收获是，我对“开发者关系型开源贡献”的理解更具体了。它不只是写代码，也包括判断项目真正缺什么、避免重复劳动、理解维护者的架构意图、提出可 review 的最小改动，并把自己的执行节奏放到整个项目协作流程里。对我来说，这种方式更符合 DevRel 角色：既要能理解技术，也要能在社区和维护者之间建立清晰、可信的沟通。

下一步计划是继续关注 #13 和 #11 的维护者回复。如果 #13 得到确认，优先推进 ERC-4626 Interface Layer 的第一阶段 PR；如果 #11 更快得到明确方向，则先推进 Pendle Adapter 的 scope 设计或 query-only 实现。同时继续跟进 #41 和 #43 的 review 状态，因为这两个 PR 虽然不是 Adapter 本身，但它们会让 Moss 的 Adapter discovery、simulation 和 Receipt 验证流程更加可靠。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->



今天的主线非常明确：围绕 **Moss 开源贡献与建设** 展开。从单纯阅读项目，进一步进入到真实 PR、项目维护方式、贡献定位和内容沉淀。

## **1\. Moss 贡献状态更新**

今天最重要的进展是：我在 Moss 的一条 PR 已经被合并。

已合并 PR：

`#44 feat: add cross-platform offline test command`

链接：

`https://github.com/nishuzumi/moss/pull/44`

这个 PR 增加了：

`pnpm test:offline`

它的作用是让贡献者在没有 live Monad RPC 的情况下，也能更方便地运行离线测试。这个改动不只是一个脚本，而是一个偏 **Developer Experience** 的贡献：降低新贡献者本地跑测试的门槛，也让跨平台测试更稳定。

这条 PR 被 merge，对我来说是一个很重要的 Proof of Work。它说明我不只是阅读 Moss 或写学习笔记，而是已经真实参与了项目建设，并且有贡献进入了主仓库。

## **2\. 重新整理自己的 Moss 贡献画像**

今天重新查看了我在 Moss 中的 PR，包括：

`#40 fix(example): re-simulate before wallet send #41 fix: bound Kuru market discovery #42 fix: reject unsafe protocol injection keys #43 test: harden receipt coverage safety #44 feat: add cross-platform offline test command #45 test: cover halted mcp simulation projection #46 docs: add protocol contribution roadmap #48 fix: make capability receipts registry-owned`

这些 PR 覆盖的方向包括：

-   Safety hardening
    
-   Receipt coverage testing
    
-   MCP simulation boundary
    
-   Capability validation
    
-   Protocol discovery guardrails
    
-   Offline testing / DevEx
    
-   Protocol contribution docs
    

今天我对自己的定位更清楚了：

`Safety-oriented DevRel Builder`

这比单纯说 Research Builder 更准确。因为我不仅在读文档和写解释，也实际在 Moss 的核心安全边界上补测试、修问题、改开发体验。

## **3\. Moss 的核心安全边界理解**

今天围绕 Moss 的贡献，我再次梳理了项目的核心理念：

`Moss builds and verifies unsigned transactions; it never signs or sends them.`

也就是说，Moss 的价值不是让 AI Agent 更快地发交易，而是让 Agent 的链上动作在签名前变得：

`可解释 可模拟 可验证 可停止`

我做的几个 PR 其实都围绕这个边界展开。

例如：

-   #40 关注 wallet send 前是否应该重新 simulate。
    
-   #43 关注 Receipt 是否真正覆盖 simulation changes。
    
-   #45 关注 halted simulation 对 Agent 是否应该是 stop signal。
    
-   #48 关注 forged / stale capability 是否不应该进入 simulation。
    
-   #41 关注 Kuru market discovery 这种动态路径是否需要 timeout 和 size guardrails。
    

这些都说明，在 AI Agent × Web3 的场景里，安全问题不只发生在合约层，也发生在：

`Agent 输出 Capability 构造 Simulation 边界 Receipt 解释 MCP 投影 开发者测试流程`

## **4\. 今天对开源项目维护方式的观察**

今天也继续查看 Moss 仓库的维护方式，包括 README、docs、CONTRIBUTING、SECURITY、ADR、Issues 和 Pull Requests。

我观察到 Moss maintainer 对项目管理有几个明显特点：

第一，安全边界写得非常明确。  
Moss 不签名、不发送、不存私钥，不替代钱包 review。

第二，文档和设计记录很重要。  
例如 ADR 0007 规定 ABI 必须有来源，不能手抄。因为 ABI 是安全关键材料：

`wrong ABI -> wrong calldata -> wrong fund flow`

第三，Issue 写得很结构化。  
例如 #28 Tooling: Fetch verified ABIs through the Monadscan API，里面清楚写了 Problem、Solution、User Stories、Implementation Decisions、Testing Decisions 和 Out of Scope。

第四，PR 需要证据。  
一个好的 PR 不只是“我改了代码”，而是要说明：

`Problem Changes Evidence Out of scope`

这也影响了我之后写 PR 的方式。

## **5\. 今天完成的内容沉淀**

今天除了查看 PR 和项目状态，也整理了几份围绕 Moss 的内容。

### **Moss 项目介绍文章**

我写了一篇面向开发者的介绍文章，主题是：

`我的第一次 Moss 开源实践：AI Agent 为什么需要一个安全的链上执行框架？`

文章重点解释：

-   Moss 是什么。
    
-   为什么 AI Agent 需要 Moss。
    
-   Moss 的 discover -> load -> action -> simulate 流程。
    
-   Moss 为什么不签名、不发送交易。
    
-   我在 Moss 中做过哪些贡献。
    
-   新人如何开始参与 Moss。
    

### **Moss 新人教程**

我还整理了一份新人教程：

`Moss 新人入门教程：从理解项目到提交第一次贡献`

内容包括：

-   Moss 是什么。
    
-   核心概念：Protocol、Capability、Query、Receipt、Warning。
    
-   本地环境准备。
    
-   推荐阅读顺序。
    
-   如何运行 simple-flow 示例。
    
-   如何选择第一个贡献方向。
    
-   一个好的 Moss PR 应该怎么写。
    
-   FAQ。
    

这个教程的目标是帮助新人更快理解 Moss，而不是一上来就陷入复杂源码。

## **6\. 今天的关键判断变化**

今天最大的判断变化是：

`我在 Moss 中的角色不是“准备贡献的人”，而是已经有实际贡献记录的建设者。`

尤其是 #44 被合并后，我对自己的信心更强了一些。之前我可能还会把自己定位成学习者、Research Builder 或文档贡献者，但现在更准确的定位是：

`能理解项目安全模型，并能通过测试、文档和小型修复参与建设的 Safety-oriented DevRel Builder。`

这也和我想培养 DevRel 能力的方向一致。DevRel 不只是公开演讲或活动运营，也包括：

-   理解项目架构。
    
-   找到开发者卡点。
    
-   改善贡献路径。
    
-   写清楚文档。
    
-   补测试和示例。
    
-   帮助项目把安全边界讲清楚。
    

## **7\. 当前仍在进行的 PR**

目前仍有几条 PR 处于 open 状态，例如：

`#41 fix: bound Kuru market discovery #43 test: harden receipt coverage safety #45 test: cover halted mcp simulation projection #48 fix: make capability receipts registry-owned`

这些都比普通文档 PR 更靠近 Moss 的核心安全边界。后续需要继续跟进 maintainer feedback，必要时缩小 scope、补测试或调整实现方式。

## **8\. 今天的收获**

今天最重要的收获有三个。

第一，真实开源贡献比学习笔记更能检验理解。  
只有当 PR 被 review、被要求修改、甚至被 merge 时，才真正进入项目协作。

第二，Moss 的核心不是自动化，而是安全边界。  
AI Agent 可以辅助链上操作，但必须经过 capability、simulation、receipt 和用户签名这些边界。

第三，DevRel 可以从真实贡献中生长出来。  
我不是先写宣传文章再理解项目，而是在做 PR、读 docs、理解 maintainer 反馈之后，再把学习过程整理成文章和教程。这样的内容更扎实。

## **9\. 下一步计划**

接下来我会继续：

-   跟进 #41、#43、#45、#48 的 maintainer feedback。
    
-   把 Moss 新人教程继续打磨成更适合公开发布的版本。
    
-   总结 #44 被 merge 的经验，作为 GitHub Portfolio 里的 Proof of Work。
    
-   继续观察 maintainer 新开的 issues，例如 bound protocol、receipt labels、architecture docs 等方向。
    
-   思考是否可以把自己的安全测试经验整理成：
    

`Moss Safety Contribution Checklist`
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->




今天的学习重点主要围绕 **Week 3 角色定位、DevRel 方向表达、活动运营设计，以及 DeFi 代表产品研究** 展开。相比前几天偏单点文章阅读或原型整理，今天更像是在把自己的方向、能力边界和研究方法进一步具体化。

## **1\. 明确 Week 3 团队角色定位**

今天首先整理了进入 Week 3 团队后，我能承担什么角色、需要什么队友、可以提供什么 Proof of Work。

我对自己的定位是：

`技术背景驱动的 DevRel / Builder`

这里的 DevRel 不是单纯做社区运营，也不是只负责公开表达，而是建立在技术理解基础上的开发者关系能力。

我希望自己未来能理解 Web3 各类职能背后的技术逻辑，包括：

-   协议
    
-   合约
    
-   产品
    
-   生态
    
-   开发者工具
    
-   AI x Crypto
    
-   安全与研究工作流
    

今天我也明确了自己的优势和短板。我的优势是可以做技术理解、资料整理、Research Card、README、Known Issues、AI 协作记录和开发者路径设计；短板是公共场合的 demo 展示、现场讲解和 pitch 不是目前最擅长的部分。

所以我需要的队友主要有两类：

-   能快速做 Demo 或前端展示的 builder。
    
-   擅长公开表达、主持、演示和 pitch 的队友。
    

我可以提供的 Proof of Work 包括：

-   Week 1 的 OnchainTodo 最小合约和 Monad 学习记录。
    
-   Week 2 的 EIP-7702 Reading Card。
    
-   Polymarket Product Reading Card。
    
-   AI Agent Finding Triage Assistant v0.1 原型方案。
    
-   README、mock 数据、Known Issues 和 AI Collaboration Log。
    

今天这个任务让我更清楚地看到：我适合的不是单纯执行某个孤立任务，而是作为技术理解和内容结构之间的连接者。

## **2\. 设计 Web3 / AI 新人活动主题**

今天继续围绕前几天的 AI Finding Triage 原型，设计了一场适合新人参与的小型线上活动。

活动主题是：

`AI Agent 安全报告怎么判断真假？Web3 Finding Triage 入门`

这个活动的核心不是教大家“用 AI 自动审计”，而是帮助新人建立一个判断框架：

`AI finding -> triage card -> 人工判断 -> 下一步验证`

活动面向的目标用户包括：

-   Web3 安全初学者
    
-   Solidity / 合约开发学习者
    
-   想使用 AI 辅助代码审查的人
    
-   对 AI Agent + Security 感兴趣的 builder
    
-   想进入 Research / DevRel / Security 方向的新人
    

我认为这个主题值得办，是因为 AI 会越来越多进入代码审查、安全分析和协议开发流程。但如果新人只学会“让 AI 找问题”，却不会判断 AI 输出是否可靠，就很容易制造噪音，甚至误把 hallucination 当成漏洞。

所以活动的核心定位是：

`AI finding 不是结论，而是 triage 的起点。`

## **3\. 完成活动当天内容流程设计**

基于活动主题，今天进一步设计了活动当天的内容流程。

活动设计为 60 分钟，包含：

-   0–5 min：开场，说明活动目标。
    
-   5–15 min：背景介绍，解释为什么 AI finding 需要 triage。
    
-   15–25 min：介绍 Triage Card 字段。
    
-   25–40 min：拆解 3 条 mock finding。
    
-   40–50 min：互动环节，让参与者判断一条 finding。
    
-   50–57 min：总结 AI 和人类如何分工。
    
-   57–60 min：收尾 CTA。
    

Triage Card 的字段包括：

`target invariant mechanism impact reproducibility status next step`

今天也设计了一个互动环节：

`你来当 Triage Reviewer`

参与者会看到一条 mock finding：

`AI 报告称：某合约的 claimReward() 没有限制调用次数，用户可能重复领取奖励。`

然后他们需要选择：

`Accepted / Rejected / Needs Reproduction`

这个互动的重点不是让大家立刻判断出正确答案，而是训练他们意识到：很多 finding 在没有看代码、没有测试、没有复现之前，最合理的状态其实是 Needs Reproduction。

## **4\. 完成活动执行预案**

今天还继续把活动方案推进到执行层面，写了一份活动执行预案。

预案里包括：

-   时间安排
    
-   宣传计划
    
-   人员分工
    
-   活动当天执行流程
    
-   风险预案
    
-   预期目标
    

时间安排按 T-5 到 T+1 设计：

-   T-5：确定主题、嘉宾、平台。
    
-   T-4：准备内容材料。
    
-   T-3：第一次宣传。
    
-   T-2：内容彩排。
    
-   T-1：第二次宣传和提醒。
    
-   活动当天：执行活动。
    
-   T+1：复盘和发布总结。
    

人员分工包括：

-   主持人
    
-   分享人
    
-   互动协助人
    
-   内容记录人
    
-   设计 / 传播协助人
    

风险预案也做了几个场景，比如：

-   参与者觉得内容太技术。
    
-   互动冷场。
    
-   有人误以为这是“AI 自动审计教学”。
    
-   安全问题被过度简化。
    
-   嘉宾或主持临时无法参与。
    
-   平台或麦克风出现问题。
    

今天这个部分让我意识到，运营不是简单发文案，而是要提前设计用户理解路径、参与门槛和风险控制。

## **5\. 完成活动基础运营物料**

在执行预案之后，今天又继续把活动转化为可以上线的基础运营物料。

完成的物料包括：

-   活动标题
    
-   活动宣传文案
    
-   报名页 / 活动介绍
    
-   开场前提醒文案
    
-   收尾 CTA
    

宣传文案的核心是：

`AI 说得很像真的，不代表它真的找到了漏洞。`

报名页强调这不是高门槛安全审计课，而是一个入门级练习。参与者不需要丰富审计经验，只需要知道 Solidity 是什么、看过简单合约即可。

收尾 CTA 设计为一个 10 分钟小练习：

`找一条 AI 生成的 Web3 security finding，或者使用活动里的 mock finding，填写一张 triage card。`

提交格式是：

`Finding: Target: Invariant: Mechanism: Impact: Reproducibility: Status: Next Step:`

这个设计的目的，是让活动结束后用户不是“听完就走”，而是可以带着一个具体模板继续练习。

## **6\. 整理运营 Case Study**

今天还把前面完成的活动方案、流程、执行预案和运营物料整理成一份运营案例。

这个 Case Study 的重点不是“做了多少内容”，而是说明背后的运营判断。

我认为最重要的运营判断是：

`不把活动包装成“AI 自动审计课”，而是定位成“新人判断 AI finding 的入门练习”。`

因为如果直接宣传 AI 审计，很容易让新人误解为 AI 可以替代审计员，也容易提高活动门槛。而用 “Finding Triage 入门” 作为主题，可以降低参与压力，同时强调人工判断的重要性。

这也符合我对 DevRel 的理解：好的活动不是把内容包装得更厉害，而是帮助目标用户真正理解、参与，并带走一个可复用的方法。

## **7\. 开始 DeFi 代表产品研究：Uniswap 和 Aave**

今天后半部分开始转向 DeFi 产品研究。任务要求从 DefiLlama Top TVL 协议或代表性产品中选一个研究对象，我选择了两个：

`Uniswap Aave`

原因是它们分别代表 DeFi 的两个核心基础设施：

`Uniswap：交易流动性 Aave：借贷流动性`

我为它们分别建立了研究范围。

### **Uniswap 研究主题**

`Uniswap V3 的集中流动性解决了什么问题？为什么它提高了资金效率，但也让 LP 变得更复杂？`

研究对象：

`Uniswap`

所属分类：

`DEX / AMM`

我想回答的问题是：Uniswap 为什么从 V2 的全区间流动性转向 V3 的集中流动性？这个设计到底改善了交易者体验，还是主要服务专业 LP？

资料边界是：本次重点看 Uniswap V3，不深入 V4 hooks、UniswapX、Unichain，也不做合约逐行审计。

参考来源包括：

-   DefiLlama Uniswap
    
-   Uniswap Concentrated Liquidity Docs
    
-   Uniswap V3 Whitepaper
    
-   Uniswap V3 Core GitHub
    
-   Uniswap Governance Forum
    

我查到的关键数据是：截至 2026-07-15，DefiLlama 显示 Uniswap TVL 约 **$3.116B**，30 日 fees 约 **$61.44M**，24h fees 约 **$6.03M**。

### **Aave 研究主题**

`Aave 如何把链上借贷做成通用流动性基础设施？它如何管理抵押、利率和清算风险？`

研究对象：

`Aave`

所属分类：

`Lending`

我想回答的问题是：Aave 为什么能长期占据 DeFi 借贷头部位置？它的核心机制如何让用户在无需信用审查的情况下完成存款、借款和清算？

资料边界是：本次重点看 Aave V3 的基础借贷模型，包括 supply、borrow、collateral、health factor、liquidation。不深入 GHO、Aave V4、Umbrella、Aave Horizon，也不做参数治理逐项分析。

参考来源包括：

-   DefiLlama Aave
    
-   Aave V3 Overview
    
-   Aave Market Operations
    
-   Aave Governance Forum
    
-   Aave V3 GitHub
    

我查到的关键数据是：截至 2026-07-15，DefiLlama 显示 Aave TVL 约 **$14.156B**，30 日 fees 约 **$29.19M**，24h fees 约 **$911K**。

## **8\. 完成 Uniswap 和 Aave 的 Product-to-Market Brief**

在确定研究对象后，今天进一步写了两份简短的市场判断。

### **Uniswap 的判断**

我认为 Uniswap 满足的是链上用户最基础的交易需求：不用注册账户、不依赖中心化交易所，也可以直接用钱包换资产。

它的优势包括：

-   无许可
    
-   可组合
    
-   资产上线开放
    
-   对长尾资产友好
    
-   V3 提高资金效率
    

但反方观点是，V3 对普通 LP 来说太复杂。集中流动性可能更适合专业做市商，普通 LP 可能需要承担无常损失和区间管理成本。

我的最终判断是：Uniswap 已经不只是换币工具，而是 DeFi 的流动性基础设施。未来增长不一定来自普通 LP，而可能来自专业化流动性管理、钱包集成、Intent 和 Agent 交易场景。

### **Aave 的判断**

我认为 Aave 满足的是链上用户的借贷和资金管理需求。它让用户在不做信用审查的情况下，通过超额抵押完成存款和借款。

Aave 成立的关键条件包括：

-   超额抵押
    
-   动态利率
    
-   health factor
    
-   清算机制
    
-   预言机
    
-   足够深的存款流动性
    
-   用户对协议安全的信任
    

反方观点是，Aave 的安全性高度依赖预言机、清算机制和治理参数。如果极端行情下价格剧烈波动、流动性不足或预言机异常，协议可能产生坏账。

我的最终判断是：Aave 的市场成立非常扎实，因为借贷是金融最基本的需求之一，而 Aave 已经把它做成了 DeFi 的核心资金市场。未来机会在机构化、稳定币、RWA 和自动化资金管理，但它必须持续证明自己的风险控制能力。

## **9\. 今天的整体收获**

今天最明显的收获是，我同时在训练两种能力：

第一是 **DevRel / Ops 能力**。  
通过活动策划、执行预案、宣传物料和 Case Study，训练如何把复杂技术主题变成新人能理解、愿意参与、能带走方法的内容。

第二是 **Research / Product 判断能力**。  
通过研究 Uniswap 和 Aave，训练如何从用户需求、产品优势、成立条件、增长机会和风险角度理解一个真实协议为什么能成立。

这两条线其实是互相连接的。DevRel 不是只做活动，也不是只写文案；它需要对产品、协议和用户需求有真实理解。Research 也不是只读资料，而是要能判断一个产品为什么成立，以及它对开发者和生态有什么启发。

## **10\. 下一步计划**

接下来可以继续做：

-   把 Uniswap 和 Aave 的研究整理成更清晰的 Reading Card。
    
-   对比 Uniswap 和 Aave：一个是交易流动性，一个是借贷流动性。
    
-   继续研究它们对 AI Agent 或自动化策略的启发。
    
-   把 AI Finding Triage 活动方案压缩成一页可公开发布的活动介绍。
    
-   继续完善 Week 3 自己的角色定位：Research-driven DevRel / Builder / Ops。
    
-   选择一个更具体的方向，形成 Week 3 可以参与团队协作的实际切入点。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->





今天主要完成了两类任务：一是阅读并拆解一个真实 EIP，二是选择一个已经上线的 Web3 产品做 Product / Protocol Reading Card。今天的重点是从“看懂概念”进一步转向“结构化分析真实协议和产品”。

## **1\. EIP 阅读：EIP-7702**

今天选择阅读的是：

`EIP-7702: Set Code for EOAs`

原文链接：

`https://eips.ethereum.org/EIPS/eip-7702`

这篇提案关注的是 EOA 钱包能力不足的问题。传统 EOA 只能发交易，不能像智能合约钱包一样拥有复杂逻辑，所以用户经常会遇到多步操作、gas 门槛、权限无法细分、账户迁移困难等体验问题。

我对 EIP-7702 的核心理解是：

`EOA 还是原来的地址，但它可以授权自己在执行时按照某个合约代码的逻辑运行。`

也就是说，它不是简单把 EOA 变成智能合约钱包，而是通过 delegation indicator，让 EOA 可以委托到某个代码地址，从而获得类似智能钱包的一部分能力。

这个提案带来的产品启发很多：

-   钱包会成为更重要的安全入口。
    
-   用户授权体验需要被重新设计。
    
-   AI Agent 可以使用更受限的 session key。
    
-   DApp onboarding 可以更像普通互联网产品。
    
-   生态可能需要可信 delegate contract registry。
    

但它也有明显风险，例如用户签错授权、delegate code 有漏洞、存储冲突、tx.origin 假设被破坏、relayer 被消耗 gas 等。

今天最大的收获是：EIP-7702 不只是一个账户抽象技术提案，它也会深刻影响钱包产品、安全提示、DApp 交互设计和 AI Agent 授权场景。

## **2\. Product / Protocol Reading：Polymarket**

今天选择分析的 Web3 产品是：

`Polymarket`

官网：

`https://polymarket.com/`

文档：

`https://docs.polymarket.com/`

Polymarket 解决的问题是：人们对未来事件有很多判断和分歧，但普通社媒上的观点没有成本，也很难形成清晰概率。Polymarket 把观点变成可交易的价格，让用户用真实资金表达自己对事件结果的判断。

我的理解是：

`Polymarket 的核心不是下注，而是把分散的信息和分歧变成可观察的市场价格。`

它的核心机制是预测市场。用户针对一个事件买卖 Yes / No 份额，份额价格可以大致理解为市场当前认为事件发生的概率。例如 Yes = 0.63，可以理解为市场认为事件发生概率约 63%。

主要用户包括：

-   事件交易者
    
-   关注政治、体育、Crypto、AI、宏观事件的人
    
-   想观察市场预期的研究者和媒体
    
-   使用 API 构建数据产品的开发者
    
-   做市商和高频交易者
    

今天查到的关键数据包括：

-   DeFi Rate 页面显示，Polymarket 分类交易量中 Sports 类别约 $313.7M，Politics/Gov 类别约 $163.4M，列出总量约 $658.9M。
    
-   Polymarket 官网显示部分热门市场有百万美元级交易量，例如某 Bitcoin 相关市场约 $8M Vol.。
    

我的主要疑问是：

`预测市场价格到底多大程度代表“群体智慧”，多大程度只是少数大户、内幕信息或流动性结构的结果？`

这和我最近关注的 AI 治理、公共协调机制也有关。预测市场可能成为公共判断工具，但它不是天然客观的，还需要理解资金结构、参与者结构和市场操纵风险。

## **3\. 今天的方法变化**

今天的学习方式更偏结构化阅读，而不是单纯总结文章。

我开始用固定框架拆解提案和产品：

`背景问题 核心方案 / 核心机制 影响对象 / 主要用户 关键术语 风险与争议 产品启发 仍未解决的问题 资料来源`

这个框架对 Research-driven DevRel 很有帮助。因为它不只是“我读懂了什么”，而是能把复杂内容转化成别人也能理解和继续讨论的结构。

## **4\. 和 Week 2 主线的关系**

Week 2 的主线是：

`AI x Crypto 的开发者关系与可信协作基础设施`

今天的两个内容都和这个主线有连接。

EIP-7702 连接的是账户、授权、钱包安全和 AI Agent 权限控制。  
Polymarket 连接的是预测市场、公共判断、信息聚合和 AI 治理中的协调机制。

这让我更清楚地看到，Crypto 的价值不只是链上交易，而是在多方不完全互信的情况下，提供可验证、可协调、可组合的机制。

## **5\. 今日收获**

今天最重要的三个收获：

1.  EIP-7702 的关键不是“EOA 变成合约”，而是让 EOA 可以通过委托代码获得更接近智能钱包的能力。
    
2.  Polymarket 的价值不是单纯下注，而是把分散判断转化成市场价格。
    
3.  结构化阅读能帮助我把技术提案和产品案例转化成 DevRel 可用的解释材料。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->






今天主要完成了 Week 2 的方向调整、学习记录建立，以及 AI 协作方式的反思。今天的重点不是继续做基础合约 Demo，而是重新确认自己真正想探索的主题和工作方式。

## **1\. Week 2 方向调整**

今天一开始讨论了 Week 2 应该选择 Research / Ops / Dev 哪个主方向。

最初的版本偏向 Dev，围绕 OnchainTodo、Monad Testnet 部署、read/write 交互和 README 展开。但我意识到，这些内容更像 Week 1 的基础复习，不是我真正感兴趣和想继续深入的方向。

经过调整后，Week 2 的主方向确定为：

`Research`

主题是：

`AI x Crypto 的开发者关系与可信协作基础设施`

个人定位是：

`Research-driven DevRel`

这意味着 Week 2 不再围绕基础合约 Demo 展开，而是更关注 AI Agent 安全、AI 治理、Agent 工具经济、高级 Crypto 内容表达、以太坊协议贡献等方向。

## **2\. 为什么选择 Research**

今天重新梳理后，我更明确了：Research 不是远离技术，也不是只写文章。

对我来说，Research 的意义是建立判断力：

-   判断哪些技术方向重要。
    
-   判断哪些问题值得服务。
    
-   判断哪些内容需要被解释清楚。
    
-   判断开发者可以从哪里参与。
    
-   判断一个主题是否值得在 Week 3 继续深挖。
    

Dev 仍然重要，但在 Week 2 里，Dev 更像是验证工具，而不是主线。Ops 也重要，但它更偏向把内容变成社区能理解、愿意参与、愿意传播的表达方式。

所以当前理解是：

`Research 建立判断 Dev 验证判断 Ops 传播判断`

## **3\. 本周学习记录建立**

今天建立了 Week 2 的学习记录框架，重点记录三类内容：

-   资料链接
    
-   判断变化
    
-   下一步计划
    

本周资料主线包括：

-   AI Agent 安全 / Triage Workflow
    
-   Crypto x AI 治理 / 公共协调机制
    
-   高级 Crypto 内容的 DevRel 表达
    
-   Agent 工具经济 / MCP 技能变现
    
-   以太坊协议学习 / 贡献路径
    

本周希望形成的最小产出是：

`AI x Crypto 开发者入口地图 v0.1`

这份地图不追求一次性深入所有主题，而是先回答每个方向：

-   解决什么问题？
    
-   为什么属于 AI x Crypto？
    
-   开发者如何参与？
    
-   我是否想在 Week 3 继续深挖？
    

## **4\. 判断变化**

今天最重要的判断变化有三个。

第一，从基础 Demo 转向 Research 主线。  
原本以为 Week 2 可以继续做 OnchainTodo 部署，但现在判断基础合约 Demo 更像复习，不是长期兴趣所在。

第二，Research 不是理论化，而是技术判断力。  
Research 不只是读文章，而是把复杂问题整理成开发者可以理解、判断和参与的路径。

第三，DevRel 需要三种能力组合。  
我现在更清楚，未来如果走开发者关系方向，需要同时具备：

`Dev：理解和验证技术 Research：判断方向和问题价值 Ops：把内容转化成社区可参与的表达`

Week 2 选择 Research，不是放弃 Dev 或 Ops，而是先用 Research 建立主题地图，再用 Dev 和 Ops 去验证与传播。

## **5\. AI 协作记录**

今天还整理了一份 AI 协作记录。

我对 AI 的定位更清楚了：

`主方向我掌握，中间衔接执行由 AI 辅助。`

AI 今天主要帮助我：

-   整理 Week 2 方向选择。
    
-   把零散主题收束成统一主线。
    
-   建立学习记录结构。
    
-   生成可提交的文本初稿。
    
-   优化表达和排版。
    

但我人工完成了关键判断：

-   不继续选择基础 Dev Demo。
    
-   把主方向调整为 Research。
    
-   把主题收束为 AI x Crypto + DevRel。
    
-   判断哪些内容是真正感兴趣的。
    
-   判断哪些地方需要删改，避免 AI 输出过于顺滑但不贴合真实想法。
    

今天也确认了哪些不能交给 AI：

-   方向选择
    
-   价值判断
    
-   真实性核查
    
-   安全边界
    
-   个人表达和最终负责
    

## **6\. 下一步计划**

接下来我需要继续推进：

-   补充 MCP / Agent 工具经济资料。
    
-   整理 AI Agent finding triage checklist。
    
-   为 AI x Crypto 开发者入口地图 v0.1 写目录。
    
-   找 2-3 个以太坊协议贡献相关仓库或 issue。
    
-   把已有文章笔记压缩成更适合对外分享的短版。
    
-   持续记录资料链接、判断变化和下一步计划。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->







这一周的学习主线，可以概括为：

`从链上基础实践，走向对 Web3 / AI / Crypto 交叉方向的系统理解。`

本周不是单纯学某一个工具，而是在逐步建立判断框架：什么适合上链，AI 能辅助什么，哪些地方必须人工判断，以及未来自己更适合怎样参与生态。

## **1\. 本周完成的核心内容**

本周先从链上产品基础开始，重新梳理了链上产品和普通互联网产品的区别。我的理解从“钱包、Token、链”变得更具体：链上产品真正重要的是关键状态、资产和规则是否公开、可验证、可组合。

随后学习了交易字段，包括 from、to、value、gas、手续费和交易状态，也理解了为什么失败交易仍然可能消耗 gas。

实践上，我完成了一个最小 Solidity 合约 OnchainTodo，并用 AI 辅助生成初稿，再由自己人工检查权限、输入校验、复杂度和安全边界。合约已完成本地编译，并整理了 Remix / Foundry 部署到 Monad Testnet 的流程。

同时，我形成了一个轻量级 Mini Demo 0：  
OnchainTodo + Monad 学习记录。

它包含：

-   最小合约 Demo
    
-   README v0.1
    
-   AI 辅助与人工判断记录
    
-   Monad Testnet 部署流程
    
-   Week 2 方向选择
    

## **2\. 本周重要学习主题**

本周除了链上实践，也学习了几篇更偏研究和行业判断的内容。

**以太坊学习路径**  
理解到协议层学习需要先打基础，再选定细分方向长期深挖。真正有价值的参与方式不是泛泛学习，而是持续写笔记、跟 issue、提小 PR、形成公开贡献记录。

**AI Agent 支付**  
通过 Affluxa / Fluxa 的案例，理解到 Agent 支付不是简单“AI + 钱包”，而是一套包含身份、预算、风控、可撤销和结算的支付基础设施。Agent Commerce 的关键不是让 AI 随便花钱，而是让 AI 在可控、可追溯、可撤销的边界里完成支付。

**Vitalik 的 obfuscation 文章**  
理解到 obfuscation 的核心不是隐藏数据，而是隐藏程序本身。它和普通代码混淆不同，更接近可证明安全的密码学目标。虽然 iO 目前距离工程可用还很远，但它代表了密码学试图移除可信第三方的一个终极方向。

**Ethereum Blog：Triage is the Product**  
这篇让我意识到，AI Agent 在安全审计中的价值不是替代研究员，而是扩大搜索空间。真正核心的产品不是“生成 bug 报告”，而是 triage：复现、去重、验证、判断严重性。AI 没有消灭人工判断，而是把判断推到了更核心的位置。

**Vitalik 关于 AI 2040 的讨论**  
今天学习到，AI 治理的重点不是简单预测未来，而是在高度不确定和分歧中建立公共协调机制。Crypto / Web3 在其中的价值，可能是提供公开、可验证、可协调的规则层。

## **3\. 本周理解变化**

这一周最大的变化是：我开始把 Web3 理解成一种“协调基础设施”，而不只是链、币、钱包或合约。

链上产品解决的是状态、规则、资产和协作的可信问题。  
AI Agent 支付解决的是自动化任务中的身份、授权和结算问题。  
Obfuscation、ZK、FHE 等密码学方向解决的是如何减少对可信第三方的依赖。  
AI Agent 审计和 AI 治理则提醒我：工具越强，越需要可靠的判断、验证和协调机制。

这一周反复出现的关键词其实是：

`trust verification coordination human judgment`

## **4\. AI 帮助了什么**

AI 在本周主要帮助我：

-   生成 Solidity 合约初稿。
    
-   解释交易字段和链上概念。
    
-   整理 README、Build Log 和 Mini Demo 0。
    
-   辅助理解英文文章。
    
-   帮我把零散学习内容整理成结构化笔记。
    
-   协助提炼 Tech / Ops / Research 三个方向的关系。
    

但我也更清楚地意识到：AI 适合辅助生成、解释和整理，最终判断仍然要由我自己完成。

尤其是这些地方不能交给 AI 自动决定：

-   合约权限是否合理。
    
-   功能是否过度复杂。
    
-   是否应该上链。
    
-   部署时如何保护私钥。
    
-   一篇文章的真实含义和长期价值。
    
-   自己适合走什么方向。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->








今天学习的是 Vitalik 关于 **AI 2040、AI 治理与公共协调机制** 的讨论。核心不是单纯争论“AI 会不会很快到来”，而是：当社会对 AI 未来存在巨大分歧时，应该如何建立一种开放、可信、可协调的公共决策机制。

## **1\. 核心问题**

Vitalik 提到，围绕 AI 2040 的讨论里，双方像是处在两个完全不同的世界观中。

一边认为，超级智能很可能在未来十几年内出现，如果不主动减速或建立治理机制，可能带来失控、权力集中、社会冲击等风险。

另一边则认为，AI 只是普通技术进步的一部分，不应该被过度神化，也不应该因为极端风险叙事就压制创新。

我觉得这篇内容真正讨论的不是“谁预测得更准”，而是：

`当人们对未来风险的判断差异巨大时，社会如何提前设计协调机制？`

## **2\. AI 2040 的背景**

AI 2040: Plan A 的核心设想是：人类通过某种国际协调，把超级智能的出现推迟到 2040 年左右，从而争取更多时间做安全、治理、验证和社会准备。

它强调这不是预测，而是一种政策建议和情景推演。重点是避免 AI 公司或大国之间进入失控竞赛，导致超级智能被少数公司、少数政府或少数个人控制。

这个思路背后的担忧是：如果 AI 发展太快，社会没有足够时间建立防护栏，那么风险不只是技术失控，也包括权力高度集中。

## **3\. Vitalik 的重点：不是监管，而是协调**

Vitalik 的表达里，我理解最重要的一点是：他并不是简单呼吁“政府加强监管”，而是更关注 **coordination**。

监管往往意味着由政府或少数中心化机构制定规则。  
而协调更强调多方参与、提前约定、公开讨论和可验证执行。

他提到可以预先设定一些触发条件，如果出现严重风险，就触发 AI 减速或暂停机制。例如：

-   超级疫情风险出现。
    
-   失业率超过某个极高阈值，例如 25%。
    
-   大规模自主致命无人机部署。
    
-   其他可以公开衡量的重大社会风险指标。
    

这里的重点不是具体数字本身，而是“提前约定触发条件”。这样未来面对高风险事件时，不至于完全依赖临时政治博弈。

## **4\. X 作为 AI 治理协调平台**

这次讨论里比较有意思的是，Vitalik 提到 X 可能被重新设计成一种公共协调平台。

他的想法不是让 X 只是一个发帖平台，而是让它帮助社会发现更大的 win-win deal，让更多普通人参与 AI 未来的治理讨论，而不是只让政府、大公司、大实验室和少数非营利机构决定。

我理解这里有三层含义：

`信息层：让公众看到更高质量的信息 判断层：用预测市场、Community Notes 等机制辅助判断 协调层：让不同立场的人找到可接受的共同方案`

这和他之前长期关注的公共物品、机制设计、去中心化治理是一脉相承的。

## **5\. 和 d/acc 的关系**

这篇内容也能看出 Vitalik 的 d/acc 思路。

d/acc 不是盲目加速所有技术，而是加速那些增强防御、韧性和人类自主性的技术，例如：

-   密码学
    
-   形式化验证
    
-   安全硬件
    
-   公共信息系统
    
-   疫情防御
    
-   隐私保护
    
-   去中心化治理工具
    

所以他对 AI 的态度不是简单的“加速”或“停止”，而是希望社会加速那些能让人类更安全、更可验证、更有韧性的基础设施。

## **6\. 和 Crypto / Web3 的关系**

这篇内容让我看到，Crypto 在 AI 治理里可能不是主角，但可以成为重要工具。

可能的结合点包括：

-   用预测市场判断某些 AI 风险触发条件是否发生。
    
-   用链上治理记录公共决策过程。
    
-   用 ZK 等密码学工具在保护隐私的同时验证信息。
    
-   用去中心化身份和声誉机制辅助公共讨论。
    
-   用公开、可审计的基础设施减少对单一平台或机构的信任。
    

这和我之前学 Monad、Agent 支付、链上产品时的理解有连接：区块链最有价值的地方，不只是“发币”或“交易”，而是在多方不完全互信的情况下，提供公开、可验证、可协调的规则层。

## **7\. 我的理解变化**

今天之前，我可能会把 AI 治理理解成两个极端：

`要么让市场自由发展 要么让政府强监管`

但 Vitalik 的这篇讨论提供了第三种视角：用机制设计和公共协调工具，让更多人参与到 AI 未来的规则制定中。

这不是简单的技术问题，而是社会如何处理不确定性的问题。

如果大家对 AI 时间线、风险大小、权力集中程度的判断完全不同，那更需要建立可以讨论、预测、验证和触发行动的机制。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->









今天学习了 Ethereum Foundation Blog 的文章：[**The triage is the product: running AI agents against Ethereum's protocol code**](https://blog.ethereum.org/2026/07/09/triage-is-the-product)。

这篇文章主要讲的是：**AI Agent 可以帮助安全研究人员更快地发现协议代码中的潜在问题，但真正重要的产品不是“生成 bug 报告”，而是“如何筛选、验证和判断这些报告”。**

## **1\. 核心观点**

文章最重要的一句话可以概括为：

`AI Agent 不是安全审计里的神谕，而是一个搜索工具。`

它可以像 fuzzer 一样，帮助研究人员在复杂代码库里寻找潜在问题。但不同的是，Agent 不只给出 crash 或 stack trace，它还会给出解释、调用链、影响分析、严重性判断和 PoC。

问题在于：这些输出看起来很完整、很自信，但不一定是真的。

所以真正的瓶颈不再是“有没有候选 bug”，而是：

`如何判断哪些是真的，哪些只是看起来像真的。`

## **2\. AI Agent 在协议安全中的作用**

Ethereum Foundation Protocol Security 团队已经在真实协议代码上运行协调式 AI Agents，包括系统软件、密码学代码和关键合约代码。

文章提到，Agent 确实发现过真实问题，例如 libp2p gossipsub 中一个可远程触发的 panic，后来被修复并披露为 CVE-2026-34219。

但作者强调，发现 bug 本身不是最意外的地方。真正意外的是：**找到候选问题并不难，难的是从大量候选中筛出真正成立的问题。**

## **3\. 工作流：不是一个 Agent，而是一组 Agent**

文章介绍了一种多 Agent 并行协作的方式。

多个 Agent 会针对同一个目标代码库工作，通过 repo 和版本控制共享状态，而不是依赖一个中心化调度器。

大致角色包括：

-   **Recon**：把攻击面转化成具体、可测试的假设。
    
-   **Hunting**：沿着某个假设追踪代码路径，尝试构造 reproducer。
    
-   **Gap-filling**：根据已接受和已拒绝的结果，补充新的假设，避免重复探索。
    
-   **Validation**：独立复核候选问题，去重，并决定是否成立。
    

我觉得这个流程很像把传统安全研究流程拆成多个小任务，让 Agent 去扩展覆盖面，但最终判断仍然需要严格验证。

## **4\. 一个候选问题必须包含什么**

文章提到，一个候选 finding 不能只是“这里看起来有风险”，而是必须有清晰结构：

`target：攻击者能实际触达的组件和入口 invariant：必须保持成立的性质 mechanism：可能破坏这个性质的具体方式 success：可观察的成功标准，例如 panic、stall、接受非法输入 reproducer：能在真实代码上运行的自包含复现材料 dedup：去重标识，避免多个 Agent 重复追同一个问题`

这个结构很重要，因为它强迫 Agent 把模糊判断变成可测试的 claim。

## **5\. Reproducible or it didn't happen**

今天最重要的安全工程原则是：

`不能复现，就不能算 finding。`

候选问题必须有一个自包含 reproducer，而且这个 reproducer 要能在真实代码、真实构建方式下运行。

文章举了几类常见误判：

-   只在 debug build 里 panic，真实发布配置下并不会崩。
    
-   reproducer 手动构造了真实攻击者无法输入的内部状态。
    
-   formal verification 里的证明成立，但证明的是一个太弱或无意义的性质。
    

这些问题说明，Agent 很容易生成“看起来正确”的报告，但如果没有严格复现，它只是噪音。

## **6\. Signal-to-noise 才是主要工作**

文章反复强调，大多数候选问题都是错的、重复的或不在范围内。

这不是方法失败，而是这个方法本来就会产生大量候选。关键是快速拒绝错误结果，并为真正的问题提供强证据。

每个候选问题至少要过两个判断：

`真实攻击者是否能在正常配置下触达？ 攻击成本和影响是否匹配？`

如果一个问题需要特殊权限、巨大资源或非现实输入，那它和“任意 peer 都能触发”的问题严重性完全不同。

## **7\. Agent 擅长什么，不擅长什么**

文章中我觉得很有价值的一点是，它没有神化 Agent。

Agent 擅长：

-   同时阅读 spec 和代码。
    
-   提出 invariant。
    
-   从一个想法草拟 reproducer。
    
-   给出初步 root cause 假设。
    

Agent 容易误导的地方：

-   认为不可达的调用链是可达的。
    
-   为了让测试通过而“钻检查标准的空子”。
    
-   根据报告语气夸大严重性。
    
-   对多步骤状态型 bug 判断较弱。
    

尤其是最后一点很重要：有些严重 bug 不是单步触发的，而是多个合法步骤按特定顺序组合后出问题。Agent 单独做一次性推理时可能不擅长发现这种 bug，更适合用来建议哪些序列值得放进 stateful test harness。

## **8\. 如何保持可信**

文章提出了几个让 Agent 审计结果可信的习惯：

-   每个 artifact 都要有来源记录：哪个模型、什么上下文、对应哪个代码版本。
    
-   构建和运行环境要确定，避免“只在某台机器上复现”。
    
-   给 Agent 设定原则和判断标准，而不是过度脚本化流程。
    
-   最终判断必须由人完成。
    

我很认同最后一点：

`Agent 可以建议，但不能决定什么是真的、什么是重复问题、什么应该披露。`

## **9\. 我的理解变化**

今天之前，我可能会把 AI 安全审计理解成“让 Agent 自动找 bug”。

看完这篇文章后，我的理解变成：

`AI Agent 的价值不是替代安全研究员，而是扩大搜索空间。 真正的核心能力是 triage、验证和判断。`

AI 把问题从“找不到候选 bug”变成了“候选太多，如何筛选”。这其实更接近真实工程问题：不是缺信息，而是缺可靠判断。

这也和我前几天对 DevRel / Tech / Research 的理解连接起来了。未来开发者关系不只是教别人怎么用工具，也要能解释工具的边界：AI 能加速什么，不能替代什么，哪些地方仍然需要人工判断。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->










今天主要阅读和理解了 Vitalik 关于 **obfuscation（程序混淆 / 程序不可读化）** 的文章。今天最大的收获是：这篇文章讨论的重点不是“如何隐藏数据”，而是 **如何隐藏程序本身**。

## **1\. 核心问题**

普通加密主要隐藏数据。  
ZK 主要证明某个计算是正确的，同时隐藏 witness。  
FHE 主要让计算可以直接在密文上进行。

而 obfuscation 想解决的是另一层问题：

`程序可以被执行，输入输出保持正确，但程序内部逻辑尽量不可见。`

也就是说，它不是只保护数据，而是希望保护“计算规则”本身。

## **2\. 和普通代码混淆的区别**

今天一个重要区分是：Vitalik 讨论的不是传统工程里的代码混淆。

普通代码混淆更多是提高逆向成本，比如让代码更难读、更难分析。但这种方式通常不是严格安全的，最后仍然可能被破解。

Vitalik 讨论的是可证明安全的 obfuscation，尤其是：

`indistinguishability obfuscation，简称 iO`

iO 的核心要求是：如果两个程序功能等价，那么它们被混淆后应该不可区分。

这意味着攻击者即使拿到混淆后的程序，也很难判断它原本是哪一个等价程序。

## **3\. 为什么 iO 重要**

iO 重要的原因在于，它接近一种通用的：

`trustless trusted third party`

很多密码学协议最开始都会假设有一个可信第三方：它接收所有人的输入，诚实执行规则，然后返回结果。

但现实中，我们不想真的信任某个中心化第三方。所以密码学一直在尝试去掉这个可信第三方。

ZK、MPC、FHE 都是在不同方向逼近这个目标，而 obfuscation 更像是试图把“可信执行逻辑”本身封装成一个可运行但不可读的对象。

我的理解是：  
如果 ZK 是“证明我算对了”，FHE 是“我可以在看不见数据的情况下计算”，那么 obfuscation 更接近“你可以运行这个规则，但看不懂规则内部”。

## **4\. 为什么它和区块链有关**

Obfuscated program 本身有一个明显问题：它不能防止复制。

如果一个程序可以被复制，那它单独很难处理：

-   state
    
-   money
    
-   balance
    
-   double-spending
    
-   唯一性
    

而区块链正好提供了全局状态、唯一性、抗双花和公开可验证的执行环境。

所以 obfuscation 和 blockchain 结合后，理论上可以打开一些很有想象力的场景：

-   private voting
    
-   sealed-bid auction
    
-   dark pool
    
-   collusion-resistant mechanism design
    
-   更少信任假设的链上机制设计
    

这让我意识到，区块链不只是“执行公开合约”，它也可能成为某些高级密码学原语的状态层和协调层。

## **5\. 现实判断**

文章并没有把 obfuscation 描述成马上可用的技术。相反，现实判断非常清楚：目前距离工程可用还很远。

理想黑盒 obfuscation 已经被证明不可能，所以研究转向 iO。

而当前比较严谨的构造依赖非常复杂的技术堆栈，包括：

-   functional encryption
    
-   XiO
    
-   garbled circuits
    
-   FHE
    
-   ABE
    
-   lattices
    

这些方案在理论上是 polynomial，但实际 runtime 仍然非常夸张，可以理解为远远超出工程可用范围。

所以今天的结论不是“这个东西马上能落地”，而是：它代表了密码学中一个非常强、非常理想化、但仍然遥远的方向。

## **6\. 今日理解变化**

今天之前，我对 obfuscation 的理解更接近“代码混淆”或“隐藏实现细节”。

看完后我意识到，Vitalik 讨论的 obfuscation 其实是一个更底层的密码学目标：它试图让程序变成一个可运行的黑盒，既能执行正确逻辑，又不暴露内部规则。

这也让我重新理解了区块链和密码学的关系。区块链不是孤立地解决所有问题，而是可以和 ZK、FHE、MPC、obfuscation 等工具结合，去逼近“无须信任第三方”的系统设计。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->











今天课程主要围绕 **AI Agent 支付** 展开，重点讨论了 Agent 自主完成任务时为什么需要新的支付体系，以及 Fluxa 团队推出的 **Affluxa 支付协议体系** 如何解决 Agent 支付中的身份、预算、风控和结算问题。

## **1\. 核心主题**

今天最大的关键词是：

`Agent Commerce / 智能体商务`

课程中提到，未来 AI Agent 不只是回答问题，而是会逐渐替用户完成完整任务流程，例如订酒店、买票、调用工具、购买 API、向其他 Agent 付费等。

一旦 Agent 开始自主执行任务，就会自然产生大量支付需求，尤其是小额、高频、自动化支付。但传统支付体系并不是为 Agent 设计的，所以会出现很多不适配的问题。

## **2\. Agent 支付的核心痛点**

今天总结到的主要痛点有三个：

第一，Agent 很难在传统支付体系中开户。  
传统支付通常要求明确的个人或企业主体，但 Agent 本身不是传统意义上的自然人或公司，身份认证和账户体系都不匹配。

第二，传统结算链路慢且敏感。  
如果 Agent 需要频繁调用工具或购买服务，传统支付的结算周期、信息暴露、合规流程都会让体验变重。

第三，小额支付不适配。  
Agent 场景里可能经常出现 0.01 美元级别的小微支付，例如调用一次 API、购买一次数据、完成一次 Agent 间能力调用。传统手续费结构很难支持这种高频低额支付。

所以我理解，Agent 支付不是简单“给 AI 接一个钱包”，而是需要一套新的身份、预算、授权、风控、结算机制。

## **3\. Affluxa 的产品定位**

Affluxa 不是单一钱包，而是一个围绕 Agent 自动收付和结算设计的协议体系。

它的核心目标是让 Agent 可以在用户授权范围内完成支付，同时避免因为 Agent 幻觉、误操作或恶意调用导致资金风险。

课程中提到它包含四个基础模块：

`身份 预算 风控 可撤销`

这四个模块对应了 Agent 支付中最关键的问题：

-   谁在支付？
    
-   能花多少钱？
    
-   什么行为可以被允许？
    
-   出错后能不能撤销或止损？
    

## **4\. 风控和钱包机制**

今天比较有意思的是 Affluxa 的风控设计。

它不是让 Agent 拿到无限权限，而是通过预算、策略和可撤销机制约束 Agent 行为。

例如：

-   预设预算内自主交易。
    
-   用户可以一键撤销授权。
    
-   每笔交易全程可追溯。
    
-   使用一次性智能体卡，提前锁定固定额度。
    
-   卡使用后自动作废，减少 AI 乱消费风险。
    

我觉得“一次性智能体卡”这个设计很好理解。它类似给 Agent 一张临时、限额、可过期的支付工具，而不是把完整钱包权限交给 Agent。

这也说明 Agent 支付的核心不是“让 AI 更自由地花钱”，而是“让 AI 在可控边界内自动完成支付”。

## **5\. 技术体系理解**

课程提到 Affluxa 基于 Coinbase 的 X402 协议做了优化和落地，并构建了 harness engineering 风控引擎，用来约束 Agent 的交易行为。

我的理解是，Agent 支付协议需要同时处理两层问题：

`支付执行层：如何完成付款、收款、结算 风控控制层：如何限制 Agent 的行为边界`

如果只有支付执行，没有风控，Agent 可能因为错误理解任务而乱花钱。  
如果只有风控，没有顺畅结算，Agent 又很难真正完成自动化任务。

所以 Agent 支付的难点不是单点技术，而是身份、授权、预算、风控、结算一起配合。

## **6\. 主要落地场景**

今天提到的场景可以分成几类：

### **To B 和工具调用场景**

Agent 调用其他 Agent 的能力并付费。  
Agent 对接商户，自动订酒店、机票、行程。  
Agent 调用工具 API，按需付费。

这类场景很适合小额高频支付，因为 Agent 可能在完成一个任务时连续调用多个工具。

### **社交和 C 端场景**

例如 Agent 之间转账，或者用户通过 Agent 自动完成抢红包、互动、任务参与等操作。

课程中提到春节期间的 Agent 社交产品「Cloud派」，可以让用户输入指令后自动完成抢红包。这类案例说明 Agent 支付不只是 B 端工具，也可以变成 C 端互动和娱乐产品。

### **开发者变现场景**

与百度合作，把开发者技能封装成 MCP，上架后按调用付费，收益归开发者所有。

这个方向我觉得和开发者关系很相关，因为它把“开发一个能力”变成了“可被 Agent 调用并自动结算的服务”。如果这个模式跑通，开发者可以通过技能、工具、Agent 能力获得持续收入。

## **7\. 合作与生态进展**

课程中提到 Fluxa 团队核心成员来自蚂蚁集团，并且已经与蚂蚁、千问、百度智能云、VISA、Coinbase 等有合作。

目前平台注册 Agent 数量超过 13 万。

后续产品方向包括：

-   面向 Agent 的专属 VISA 信用卡。
    
-   开发者技能封装为 MCP 后按调用付费。
    
-   法币和稳定币两套支付链路。
    
-   默认使用 Base 链上合规 USDC 完成交易。
    
-   后续把 KYC / KYB 纳入体系。
    

这里我比较关注的是：Agent 支付要真正进入主流场景，最终一定绕不开身份、合规和用户信任。

## **8\. 今日理解变化**

今天之前，我对 Agent 支付的理解比较简单，可能会以为它只是“AI Agent + 钱包”。

听完后我觉得它更像是一套新的商业基础设施：

`Agent 身份 Agent 授权 Agent 预算 Agent 风控 Agent 结算 Agent 收入分配`

它解决的不是单次付款，而是未来 Agent 自主协作、调用工具、购买服务、完成任务时的支付和结算问题。

我也意识到，Agent 支付的关键不是让 Agent 拥有无限权限，而是要建立用户信任。它的发展路径可能类似早期支付宝：一开始用户不会放心把大额支付交给系统，只会从小额、低风险、高频场景开始，慢慢建立信任。

## **9\. 和我后续方向的关系**

这节课和我想探索的 DevRel / Tech / Ops / Research 方向都有关。

Tech 角度：  
需要理解 Agent 支付协议、钱包授权、链上结算、稳定币支付、MCP 调用付费等技术机制。

Ops 角度：  
Agent 支付需要教育用户和开发者，让他们理解为什么 Agent 可以安全支付、如何控制预算、如何避免乱消费。

Research 角度：  
这个方向值得继续研究，因为它连接了 AI Agent、稳定币、链上支付、开发者经济和新型商业模式。

我后续可以继续关注：

-   Agent 支付和普通钱包支付的区别。
    
-   X402 协议的具体机制。
    
-   MCP 技能如何按调用付费。
    
-   哪些 Agent 场景适合小额高频支付。
    
-   Agent 支付中如何设计用户信任和风控边界。
    

## **10\. 今日总结**

今天最大的收获是：**Agent 支付不是简单的钱包功能，而是 AI Agent 进入真实商业世界所需要的基础设施。**

未来如果 Agent 要替用户完成订票、订酒店、调用工具、购买服务、支付 API、和其他 Agent 协作，就必须有一套适合 Agent 的支付协议。

Affluxa 的价值在于，它尝试用身份、预算、风控、可撤销、一次性智能体卡、稳定币结算等机制，让 Agent 可以在安全边界内完成自动支付。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->












今天主要完成了 Week 1 的复盘整理，并为 Week 2 的方向选择做了确认。整体上，今天不是从零学习新概念，而是把前几天完成的链上实践、AI 辅助开发、Monad 产品方向分析和个人分轨判断整理成可以提交的材料。

## **1\. Week 1 Build Log 整理**

今天先整理了 Week 1 的学习和实践记录，内容包括：

-   完成了哪些链上实践。
    
-   遇到了哪些问题。
    
-   AI 帮助解决了什么。
    
-   哪些地方必须由我人工判断。
    
-   我对 Monad / Web3 的理解发生了什么变化。
    
-   Week 2 更适合进入哪个方向。
    

整理后发现，Week 1 的重点不是做出一个完整产品，而是走通链上开发的基础流程：

`理解链上产品 -> 理解交易字段 -> 生成 Solidity 合约 -> 人工检查 -> 本地编译 -> 准备 Monad Testnet 部署流程`

## **2\. Mini Demo 0 整理**

今天还把 Week 1 的核心产出汇总成了一个轻量级 Mini Demo 0。

Mini Demo 0 的主题是：

`OnchainTodo + Monad 学习记录`

这个 Demo 不是完整产品，而是一个帮助别人看懂我 Week 1 做了什么的最小展示。

它包含：

-   一个最小 Solidity 合约 OnchainTodo
    
-   合约功能说明
    
-   本地编译结果
    
-   Monad Testnet 部署流程准备
    
-   哪些部分由 AI 辅助完成
    
-   哪些部分由我人工判断和修改
    
-   Week 2 的继续推进方向
    

## **3\. 链上实践回顾**

Week 1 完成的链上相关实践主要有：

-   理解 from、to、value、gas、交易状态等字段。
    
-   理解失败交易为什么也可能消耗 gas。
    
-   使用 AI 辅助生成一个最小 Solidity 合约。
    
-   人工检查合约权限、复杂度、输入校验和安全边界。
    
-   使用 Foundry 完成本地编译验证。
    
-   准备 Remix 和 Foundry 两种部署方式。
    
-   验证 Monad Testnet RPC 和 chainId = 10143。
    

当前还没有完成真实合约部署，因为部署需要课程专用钱包在 Remix 或钱包插件里签名。后续需要补充：

`合约地址 部署交易 hash write function 交互 hash read function 调用截图 Monad Explorer 链接`

## **4\. AI 辅助与人工判断**

今天复盘时特别区分了 AI 辅助和人工判断。

AI 帮助我完成了：

-   生成 OnchainTodo 合约初稿。
    
-   解释合约结构和函数作用。
    
-   梳理交易字段含义。
    
-   整理 Remix / Foundry 部署流程。
    
-   辅助形成 Week 1 Build Log 和 Mini Demo 0 文档。
    
-   帮助分析 Monad 适合的高频交互方向。
    

但关键判断仍然必须由我完成：

-   合约是否应该保持最小。
    
-   是否需要管理员权限。
    
-   用户数据是否应该按 msg.sender 隔离。
    
-   是否需要限制输入长度。
    
-   是否加入 NFT、积分、删除、批量查询等复杂功能。
    
-   是否应该把私钥交给 AI 或写进文件。
    

最终判断是：Week 1 的 Demo 应该保持最小，只做 OnchainTodo，不要过度扩展。

## **5\. 对 Monad / Web3 的理解变化**

今天整理后，我对 Web3 和 Monad 的理解更清楚了。

链上产品不是“普通 App 加钱包”，真正的区别是：

`关键状态、资产和规则是否公开、可验证、可组合`

普通互联网产品更多依赖平台数据库，而链上产品把关键状态和规则放到智能合约里。这带来了更强的公开验证和资产归属，但也带来了 gas、等待确认、失败交易成本、签名和不可逆操作等体验问题。

对 Monad 的理解也从“更快的链”变成了更具体的判断：

`Monad 的高性能、低延迟和 EVM 兼容性，可能更适合高频交互型 Consumer Crypto 产品。`

例如小游戏排行榜、任务系统、Badge、社交互动、AI Agent 操作等场景，都可能受益于更快的反馈和更低的交互摩擦。

## **6\. 高频交互方向思考**

今天确认了一个适合 Monad 的产品方向：

`链上小游戏排行榜 / 任务系统`

这个方向适合 Monad 的原因是：

-   小游戏天然需要频繁交互。
    
-   用户会提交分数、刷新排行榜、挑战好友、领取 Badge。
    
-   如果链慢或手续费高，体验会被等待和成本打断。
    
-   Monad 的低延迟和 EVM 兼容性可以改善体验。
    
-   排名、最高分、徽章、任务完成记录等状态有链上记录价值。
    
-   游戏过程本身不需要全部上链，关键结果上链即可。
    

这个判断也让我意识到，高频产品不是所有数据都上链，而是要区分：

`哪些状态需要公开验证 哪些状态只需要前端或后端记录`

## **7\. Week 2 方向确认**

今天最重要的调整是：我不再把 Week 2 简单定义成只选 Tech。

更准确的方向是：

`Tech / Ops / Research 都参与探索 个人定位：技术背景 + 开发者关系导向`

原因是我本身有技术背景，Week 1 的内容更多是复习和校准。对我来说，Tech 是底座，但如果未来关注开发者关系 / DevRel，只会写代码是不够的。

Week 2 我希望三条线都参与：

-   **Tech**：继续完成合约部署、链上交互、Demo 和技术文档。
    
-   **Ops**：学习如何把技术内容转化成社区可理解、可参与、可传播的活动和表达。
    
-   **Research**：继续判断哪些场景适合上链，哪些产品方向适合 Monad，哪些只是普通数据库就够了。
    

一句话总结：

`以技术能力为底座，同时探索 Ops 和 Research，向 DevRel 能力靠拢。`

## **8\. 下一步计划**

Week 2 的下一步计划是：

-   用 Remix 将 OnchainTodo 部署到 Monad Testnet。
    
-   保存合约地址和部署交易 hash。
    
-   调用 addTodo 完成一次 write function。
    
-   调用 getTodoCount 完成一次 read function。
    
-   保存截图和 Monad Explorer 链接。
    
-   完善 README v0.1。
    
-   继续思考 Monad Mini Leaderboard 作为下一个 Demo 方向。
    
-   记录 Demo 如何面向社区表达，包括任务设计、传播文案和参与门槛。
    
-   输出短 Research 笔记，判断哪些状态适合上链。
    

## **9\. 今日总结**

今天的核心产出是完成了 Week 1 的材料收束：

-   整理了 Week 1 Build Log。
    
-   汇总了 Mini Demo 0。
    
-   明确了 AI 辅助和人工判断的边界。
    
-   确认了 Week 2 不做单一分轨，而是 Tech / Ops / Research 都参与探索。
    
-   重新定义了自己的定位：技术背景 + 开发者关系导向。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->













今天主要围绕链上产品、交易理解、Solidity 合约和 Monad Testnet 开发流程完成了一轮基础练习。

首先，我梳理了链上产品和普通互联网产品的区别。链上产品的关键数据、资产和规则更多依赖区块链和智能合约，而不是只存在平台数据库里。用户通常通过钱包登录，资产归属更接近用户自己，规则也更公开、可验证，但同时交易确认、gas 成本和不可轻易撤销等问题也会影响体验。

然后，我学习并整理了交易中的基础字段，包括 from、to、value、gas、手续费和交易状态。我的理解是：from 是发起交易并支付 gas 的地址，to 是接收方或合约地址，value 表示随交易发送的原生币数量。交易即使失败，也可能消耗 gas，因为链上节点已经执行了计算，只是最终状态被回滚。

在合约开发部分，我使用 AI 辅助生成了一个最小 Solidity 合约 OnchainTodo。这个合约支持用户添加 todo、切换完成状态、读取 todo 和查询 todo 数量。我没有完全照搬 AI 输出，而是进行了人工检查和判断：确认合约可以编译，函数符合预期，用户只能修改自己的 todo，没有引入不必要的管理员权限，并且增加了输入长度限制，避免空内容和过长字符串上链。

今天还整理了 Monad Testnet 的部署与交互流程。由于没有在环境中直接使用课程钱包私钥，所以没有实际代替钱包完成部署，但已经准备好了 Remix 和 Foundry 两种方式的 README v0.1，包括编译、连接 Monad Testnet、部署合约、调用 read/write function、保存合约地址和交易 hash 的步骤。

最后，我选择了“链上小游戏排行榜 / 任务系统”作为适合 Monad 的高频交互场景。这个方向需要频繁提交分数、更新排名、领取 Badge 和挑战好友。如果链慢或手续费高，用户体验会很差。Monad 的高性能、低延迟和 EVM 兼容性，可以帮助这类 Consumer Crypto 产品获得更接近互联网小游戏的体验，同时保留链上排行榜、奖励和身份记录的公开可验证特性。

**今日产出**

-   完成链上产品基础理解整理
    
-   完成交易字段与 gas 机制理解
    
-   生成并人工检查最小 Solidity 合约
    
-   本地完成合约编译验证
    
-   整理 Monad Testnet Remix / Foundry 部署流程
    
-   完成一个适合 Monad 的高频交互应用方向分析
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
