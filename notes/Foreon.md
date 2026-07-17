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
