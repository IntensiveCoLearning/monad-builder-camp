---
timezone: UTC+8
---

# AGF-DOT

**GitHub ID:** AGF-DOT

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
**\# Moss 项目学习笔记**

\> 学习时间：2026-07-14

\> 项目地址：[https://github.com/nishuzumi/moss](https://github.com/nishuzumi/moss)

\> 学习目标：理解 Moss 的设计理念、核心能力及应用场景

\---

**\## 一、项目简介**

**\### 1.1 Moss 是什么**

Moss 是一个面向 **Monad 区块链** 的 AI Agent 交互框架，它把复杂的 DApp/协议交互抽象为 Agent 可调用的统一能力层。核心流程可以概括为四个步骤：

\`\`\`

discover → load → action → simulate

发现    → 加载 → 行动  → 模拟验证

\`\`\`

**一句话总结：** Moss 让 AI Agent 用人类可读的语言操作区块链，不用关心底层技术细节，并且每笔交易在签名前都经过模拟验证。

**\### 1.2 核心理念**

Moss 的设计哲学是\*\*"系统负责组装正确的交易，Agent 负责理解用户意图"\*\*。它强调两条安全红线：

| 规则 | 负责方 | 说明 |

|------|--------|------|

| Moss 永不签名、永不发送 | 框架 | 私钥留在钱包，最终决定权在用户 |

| 每笔写操作签名前必须验证 | 机械执行 | 模拟交易，对比声明效果与实际效果 |

\---

**\## 二、核心问题**

**\### 2.1 AI Agent 操作区块链的痛点**

如果没有 Moss，一个 AI Agent 要完成"在 DEX 上用 1 MON 换 USDC"这个简单操作，需要手动处理以下所有细节：

\`\`\`

❌ 知道 DEX 路由合约地址

❌ 读取并解析 Router 合约的 ABI

❌ 区分 exactInput / exactOutput 两种调用方式

❌ 处理原生 MON → WMON 的包装（如果路由只收 WMON）

❌ 计算滑点（还要考虑 decimals 换算，USDC 是 6 位，MON 是 18 位）

❌ 构建 multicall 数组（交换 + 退款 + 扫尾清理）

❌ 批准代币授权（approve）

\`\`\`

**现实问题：** Agent 只要弄错其中一项，就可能导致：

\- 交易回滚（损失 Gas）

\- 滑点计算错误（损失资金）

\- 授权给错误的地址（资金被盗）

\- 遗漏退款逻辑（小额资金永久锁在合约里）

**\### 2.2 Moss 的解决方案**

Moss 把这些复杂性全部封装到**\*\*能力层\*\***后面，Agent 只需要说人话：

\`\`\`

用户：用 1 MON 换 USDC

   ↓

Agent：discover(swap) → load(kuru.swap) → action(kuru, swap, { tokenIn: MON, tokenOut: USDC, amount: 1 })

   ↓

Moss 自动：构建未签名交易 + 声明预期效果（expects）

   ↓

Moss 自动：在真实链上模拟，对比实际效果 vs 声明效果

   ↓

零警告 → 交给用户钱包签名

\`\`\`

\---

**\## 三、核心能力**

**\### 3.1 四步调用流程**

**\#### 1️⃣ discover（发现）**

Agent 告诉 Moss "我想做什么动作"，Moss 返回匹配的能力列表。

\`\`\`ts

discover(verb?: 'swap' | 'wrap' | 'transfer', category?: 'token' | 'dex')

// → 返回所有匹配的协议方法

\`\`\`

**设计意义：** Agent 不需要提前知道有哪些协议，能力是动态发现的。

**\#### 2️⃣ load（加载）**

获取某个具体能力的详细定义：意图描述、风险标签、参数说明。

\`\`\`ts

load({ protocol: 'kuru', method: 'swap' })

// → { intent, risk: \['fundOut', 'slippage'\], params: { tokenIn, tokenOut, amount } }

\`\`\`

**设计意义：** Agent 用这些信息向用户确认操作，确保意图对齐。

**\#### 3️⃣ action（行动）**

根据用户输入参数，构建**\*\*未签名交易计划（Plan）\*\***。

Plan 包含两个关键部分：

\- **txs\[\]**：未签名的交易数组（可能是多笔，如 approve + swap）

\- **expects**：声明的预期效果（流出/流入哪些资产、金额范围）

\`\`\`

expects 示例：

  流出：最多 1 MON

  流入：最少 1234.56 USDC（扣除滑点后的下限）

  授权：\[\] 或 \[{ spender, token, amount }\]

\`\`\`

**\#### 4️⃣ simulate（模拟验证）**

在真实链上状态执行 Plan，提取实际效果，与 expects 对账。

\`\`\`

模拟结果结构：

  reverted: false           // 交易是否回滚

  effects: { assetsOut, assetsIn, approvals, recipients }

  warnings: \[\]              // 任何差异都会产生 warning

\`\`\`

**关键安全机制：**

| 检查项 | 说明 |

|--------|------|

| 资产对账 | expects 声明的资产流入/流出范围是否匹配 |

| 授权检查 | 是否有未声明的 approve 操作 |

| 收款人验证 | 资金是否流向预期地址 |

| Native 追踪 | 不发 Transfer 事件的原生 MON 流动也被追踪 |

| 跨 Plan 状态 | Plan B 可以花掉 Plan A 模拟结果里刚获得的代币 |

**\### 3.2 两条安全规则**

\`\`\`

规则一（机械，服务器侧）：Effects 对账

  → 模拟提取实际发生的一切，任何未声明差异都产生 warning

  → 有 warning 即停，不允许签名

规则二（语义，Agent 侧）：意图对齐

  → 把 effects 摘要和用户的原话对比

  → 只有 Agent 拿着用户的原始意图

\`\`\`

**\### 3.3 当前支持的协议**

| 协议 | 能力 | 查询 |

|------|------|------|

| WMON（封装 MON） | wrap, unwrap | balanceOf |

| ERC20（任意代币） | transfer | balanceOf, allowance |

| ERC721（任意 NFT） | transfer | ownerOf, balanceOf |

| Kuru（链上 CLOB DEX） | swap（市价单） | quote, markets |

\---

**\## 四、可能应用场景**

**\### 4.1 场景一：AI 交易助手（DeFi 自动化）**

**用户需求：**

\> "我想把钱包里的 50% MON 换成 USDC，然后把 USDC 的一半存到借贷协议里赚利息，剩下的一半定投某个代币，每天自动执行。"

**Moss 做什么：**

\`\`\`

1\. discover 找到：swap + 借贷协议的 supply

2\. 多步 action：swap(MON→USDC) → supply(USDC) → 记录定投计划

3\. simulate 串联验证：swap 得到的 USDC 可以被 supply 使用

4\. 每一步都有 expects 对账，确保没有未声明的资金流动

\`\`\`

**价值：** 用户不用关心任何协议细节，AI 全程操作，且每一步都经过验证。

**\### 4.2 场景二：NFT 自动化管理**

**用户需求：**

\> "帮我监控某个 NFT 系列的地板价，如果跌到 0.5 MON 以下就自动买 1 个，然后挂到市场上以 1 MON 出售；如果 7 天没卖出就降价 10%。"

**Moss 做什么：**

\`\`\`

1\. erc721 查询：ownerOf(系列地址) + balanceOf(我的账户)

2\. erc20 转账：准备购买资金（可能需要 approve）

3\. NFT 市场协议 action：listNFT / buyNFT

4\. 每笔 action 都 simulate 验证：

   - 确认 NFT 真正转入我的钱包

   - 确认挂单授权金额正确

   - 确认版税和手续费符合预期

\`\`\`

**\### 4.3 场景三：链上资产管理仪表盘（企业级）**

**企业需求：**

\> "我们公司有一个多签钱包，分散在 5 个协议里有资产，需要每天生成资产报告，发现异常授权自动撤销，定期将收益转回国库地址。"

**Moss 做什么：**

\`\`\`

1\. discover 加载所有相关协议：staking、lp、lending、vesting

2\. action 查询每个协议的用户资产余额

3\. 汇总生成资产报表（effects 结构天然适合做这个）

4\. 异常授权检测：

   - allowance 查询所有大额授权

   - 发现未使用的无限授权 → action(approve, 0) → simulate → 提交多签

5\. 定期转账：

   - 计算各协议应收收益

   - 构建 multi-step Plan：claim → swap → transfer(国库)

   - simulate 全链路验证

\`\`\`

**\### 4.4 场景四：去中心化社交 + 打赏机器人**

**场景描述：**

在 Farcaster / Lens 等去中心化社交协议中，一个 AI Bot 监听用户提到"打赏"关键词，自动给优质内容创作者转账，同时自动兑换成创作者想要的币种。

\`\`\`

用户发帖："这篇分析太赞了，打赏 5 MON！"

   ↓

Bot 解析：

  1. erc20 查询：我的余额够不够 5 MON

  2. 如果不够，discover → swap(其他代币→MON)

  3. action：transfer(MON, 创作者地址, 5)

  4. simulate 验证：

     - 确实转出 5 MON（不多不少）

     - 收款人确实是创作者地址

     - 没有额外的 approve 操作

  5. 零警告 → Bot 触发签名

\`\`\`

**\### 4.5 场景五：跨协议套利（需谨慎）**

**思路：** 监控同一个代币在不同 DEX 上的价差，发现套利空间时自动执行。

**Moss 的角色：**

\`\`\`

1\. 同时查询多个 DEX 的 quote（kuru、未来接入的 Uniswap 等）

2\. 发现价差 → 构建 Plan A：买入（DEX A） → Plan B：卖出（DEX B）

3\. simulate 串联执行：

   - Plan A 得到的代币立即用于 Plan B

   - 验证扣除 Gas 和手续费后确实盈利

   - 验证没有意外的滑点损失

4\. 有 warning → 放弃执行（市场可能已变化）

\`\`\`

\---

**\## 五、个人理解**

**\### 5.1 Moss 在 AI + Web3 栈中的位置**

\`\`\`

用户自然语言层

    ↓

   AI Agent（LLM）← Moss 的 MCP 工具接口

    ↓              discover / load / action / simulate

   Moss 能力层

    ↓              协议适配器（每个协议一个包）

   区块链层（Monad）

\`\`\`

**我的判断：** Moss 解决的是 AI Agent "手搓交易" 的不可靠问题。之前 LLM 直接调 viem/ethers，就像让实习生直接操作公司银行账户 —— 他可能很聪明，但不熟悉流程，弄错一个数字就是灾难性损失。

Moss 相当于在中间加了一层**\*\*财务审核系统\*\***：

| 传统企业 | Moss |

|----------|------|

| 填报销单 | load + action（声明用途和金额） |

| 财务审核 | simulate（验证实际流向 vs 声明） |

| 老板签字 | 用户钱包签名 |

| 打款 | 用户自己发送（Moss 不干） |

**\### 5.2 模拟验证机制的深度理解**

Moss 的 `debug_traceCall` 模拟机制是整个系统的灵魂。我理解它的价值有三层：

**第一层：技术正确性**

\- 合约有没有回滚？

\- 实际转出/转入金额对不对？

\- 有没有意外的 approve？

**第二层：协议理解正确性**

\- 很多协议有隐藏逻辑（比如 Kuru 的市价单可能遇到盘口深度不足）

\- 模拟能发现"代码正确但业务结果不对"的情况

\- expects 就是 Moss 版的"单元测试断言"

**第三层：信任边界**

\- 用户不需要信任 Agent，不需要信任 Moss

\- 只需要信任：**\*\*模拟结果显示我会转出 1 MON，收回 1200 USDC，收款人正确，没有异常授权\*\***

\- Moss 不持有私钥，做不了恶

**\### 5.3 为什么选择 Monad 首发**

Monad 是一个高吞吐量的 EVM 兼容链，这对 Moss 的设计非常契合：

1. **高频交互**：Monad 能处理大量并发交易，Agent 自动化场景的吞吐量瓶颈在链上被消除

2. **Gas 费低**：simulate 免费（dry-run），实际执行的 Gas 成本低 → 更适合 Agent 频繁操作

3. **debug\_traceCall 支持**：Monad 默认 RPC 支持 Moss 需要的模拟接口，第三方免费 RPC 大约有一半不支持

**\### 5.4 对未来的思考**

Moss 现在的架构是"每个协议手动写适配器"，这保证了质量，但扩展速度慢。我推测未来可能的方向：

1. **协议适配器市场**：类似 Chainlink 的节点运营，第三方可以贡献适配器并获得激励

2. **AI 自动生成适配器**：用 LLM 读取合约 ABI + 文档，自动生成 protocol 包，然后自动跑测试验证

3. **跨链扩展**：现在 Moss 明确不支持跨链（因为目标链 effects 无法验证），但如果未来有跨链证明系统，可以支持

4. **策略编排层**：在 Moss 之上再加一层，用声明式 DSL 描述投资策略，自动编译为 multi-step Plan

**\### 5.5 风险提示与边界**

学习过程中我也注意到 Moss 明确声明的限制：

| 不支持的功能 | 原因 |

|--------------|------|

| Permit / typed-data 签名 | 签名流的 effects 无法通过 call trace 验证 |

| 跨链桥 | 目标链的 effects 无法在源链模拟 |

| 闪电贷原子组合 | 组合后的 effects 超出单个协议的 expects 范围 |

| 模拟结果 ≠ 执行保证 | 链上状态随时变化，模拟只是快照 |

这些边界恰恰说明 Moss 的设计是**\*\*负责任的\*\*** —— 它不承诺做不到的事，只在能验证的范围内提供价值。

\---

**\## 六、总结**

| 维度 | Moss 的价值 |

|------|------------|

| 对 Agent | 不用懂 ABI、不用算 decimals、不用管 multicall，用人话就能操作区块链 |

| 对用户 | 每笔签名前都有"效果预览 + 差异警告"，大幅降低误操作风险 |

| 对协议方 | 写一个适配器包就能让所有接入 Moss 的 Agent 自动支持你的协议 |

| 对整个 AI + Web3 生态 | 建立了"声明 → 验证 → 执行"的安全范式，而不是让 LLM 瞎蒙 |

Moss 不是"又一个 Web3 SDK"，它是 AI Agent 安全地与价值网络交互的基础设施。正如它的名字 —— Moss（苔藓），不起眼，但覆盖整个地表，为更高层的生态提供坚实的基础。

\---

_学习笔记完_
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->

````markdown


 今日学习目标

1. 完成代币迁移合约开发与测试
2. 完成 Week 2 职业选择任务
3. 创建 AI 协作记录
4. 完成 Week 3 Role Statement



一、代币迁移合约开发

 1.1 项目结构

contracts/TokenMigration.sol
├── OldToken      旧版代币合约
├── NewToken      新版代币合约（带手续费）
└── TokenMigrator 迁移合约
```

1.2 核心功能

OldToken 改进：
- 新增 `holders` 数组，记录所有代币持有者
- 新增 `migrationController` 机制，允许迁移合约暂停旧合约
- 新增 `pauseForMigration()` 函数，由迁移控制器调用

NewToken 特性：
- 支持交易手续费（默认 0.1%）
- `initializeAfterMigration()` 函数，批量初始化余额
- `setTransferFee()` 和 `setFeeCollector()` 管理函数

TokenMigrator 流程：
1. `deployNewToken()` - 部署新合约
2. `startMigration()` - 暂停旧合约，标记开始
3. `executeMigration()` - 收集余额，初始化新合约，标记完成

1.3 关键技术点



持有者追踪 | 通过 `holders` 数组 + `_isHolder` mapping 实现 
 权限控制 | 使用 `onlyOwner` 和 `onlyMigrationController` 修饰器 
迁移安全 | 暂停旧合约防止余额变动，`migrationCompleted` 防止重复迁移 

 



 二、单元测试

 2.1 测试覆盖

| 测试模块 | 测试用例 | 状态 |
|----------|----------|------|
| OldToken | 部署、转账、持有者追踪、暂停 | ✅ |
| NewToken | 迁移初始化、手续费扣除 | ✅ |
| TokenMigrator | 部署、迁移流程、数据获取、重复初始化保护 | ✅ |

2.2 测试结果


12 passing (649ms)


2.3 测试文件

[TokenMigration.js](file:///d:/Web3暑期实习/solidity-project/test/TokenMigration.js)



 三、部署脚本

 3.1 部署流程

```bash
npx hardhat run scripts/deploy-migration.js --network monadTestnet
```

3.2 脚本步骤

1. 部署 OldToken（初始供应量 10000 OTK）
2. 部署 TokenMigrator
3. 设置迁移控制器
4. 通过 Migrator 部署 NewToken
5. 开始迁移（暂停旧合约）
6. 执行迁移（余额迁移）

 3.3 部署文件

[deploy-migration.js](file:///d:/Web3暑期实习/solidity-project/scripts/deploy-migration.js)

-

四、Week 2 职业选择任务

 4.1 角色选择

主方向：Dev（智能合约开发者）

选择理由：
- 技术背景匹配：已掌握 Solidity、Hardhat、ethers.js 等核心技术
- 项目经验：完成了 MessageBoard dApp 和代币迁移系统
- 市场需求：智能合约开发是 Web3 领域最紧缺的岗位之一

 4.2 任务完成

| 任务 | 文件 |
|------|------|
| Role Choice Card | Week2-Role-Log.md |
| Role Log | Week2-Role-Log.md |
| AI 协作记录 | AI-Collaboration-Record.md |
| Week 3 Role Statement | Week3-Role-Statement.md |



五、遇到的问题与解决方案

 问题 1: 迁移控制器权限问题

| 项目 | 详情 |
|------|------|
| 现象 | TokenMigrator 调用 OldToken.pause() 失败，报错 "Not owner" |
| 原因| OldToken 的 onlyOwner 检查的是调用者（Migrator 合约），而非部署者 |
| 解决方案| 添加 migrationController 机制，允许 Migrator 暂停旧合约 |

 问题 2: BOM 字符编译错误

| 项目 | 详情 |
|------|------|
| 现象| 合约编译失败，提示 "Expected pragma" |
| 原因 | 文件开头有 UTF-8 BOM 字符 |
| 解决方案 | 使用 UTF-8 无 BOM 编码重新创建文件 |

问题 3: 地址收集困难

| 项目 | 详情 |
|------|------|
| 现象| 无法遍历 mapping 获取所有代币持有者 |
| 原因 | Solidity mapping 无法直接遍历 keys |
| 解决方案 | 在 OldToken 中维护 holders 数组 |

---

六、今日学习收获

 收获 1: 代币迁移设计模式

重要性: ⭐⭐⭐⭐⭐

学会了代币迁移的完整设计模式：
1. 暂停机制：迁移前暂停旧合约
2. 余额快照：收集所有用户余额
3. 批量初始化：一次性设置新合约余额
4. 完成标记：防止重复迁移

 收获 2: 权限控制设计

重要性: ⭐⭐⭐⭐

理解了智能合约中权限控制的重要性：
- 使用修饰器封装权限检查
- 支持多角色权限（owner + migrationController）
- 最小权限原则：只授予必要的权限

 收获 3: 测试驱动开发

重要性: ⭐⭐⭐⭐⭐

学会了使用测试驱动开发方法：
1. 先写测试用例定义预期行为
2. 实现代码使测试通过
3. 通过测试验证功能正确性

 收获 4: AI 协作流程

重要性: ⭐⭐⭐⭐

总结了与 AI 协作的最佳实践：
- AI 适合快速生成代码框架和测试用例
- 人类负责审核代码、做关键决策、管理安全敏感操作
- 建立清晰的协作流程和分工

---

 七、Week 3 学习计划

 7.1 技术学习目标

| 目标 | 优先级 | 说明 |
|------|--------|------|
| DeFi 协议深度解析 | 高 | AMM、借贷、稳定币 |
| 安全审计入门 | 高 | 常见漏洞分析、审计工具 |
| 合约升级模式 | 中 | 代理模式、钻石标准 |
| Layer 2 开发 | 中 | Rollup、ZK 证明 |

 7.2 实践计划

1. 复现简单 AMM 合约：学习 Uniswap V2/V3 原理，实现简易版本
2. 安全审查练习：使用 Slither 分析开源合约，输出审计报告
3. 参与团队项目：与队友协作完成 Week 3 团队任务

---

八、需要帮助的问题

1. DeFi 协议原理：AMM 的数学原理和实现细节
2. 安全审计工具：Slither、Mythril 的使用方法
3. 合约升级：代理模式的安全注意事项
4. Layer 2 开发：ZK Rollup 的开发框架和工具

---

 总结

今天的学习非常充实！主要完成了：

1. ✅ 代币迁移合约开发（OldToken、NewToken、TokenMigrator）
2. ✅ 单元测试编写（12/12 通过）
3. ✅ 部署脚本编写
4. ✅ Week 2 职业选择任务全部完成
5. ✅ AI 协作记录创建
6. ✅ Week 3 Role Statement 创建

通过今天的实践，我对智能合约开发有了更深入的理解，特别是代币迁移系统的设计模式和权限控制机制。同时也完成了 Week 2 的所有职业选择任务，为 Week 3 的团队协作做好了准备。期待 Week 3 的团队项目！
````
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->


今日学习笔记：ERC20代币完整知识点梳理

**学习日期**：2026年07月12日

**学习主题**：ERC20代币标准原理、特性与应用

## 一、学习前言

本次学习聚焦以太坊生态最核心、最通用的代币标准ERC20，弄懂其设计逻辑、核心规则、基础函数以及实际应用场景。ERC20是区块链代币开发、链上转账、DeFi交互的基础核心知识点，绝大多数以太坊及兼容公链的代币均遵循该标准，掌握ERC20是入门区块链代币体系的关键。

## 二、ERC20基础定义

ERC20全称 **Ethereum Request for Comments 20**，是以太坊社区提出的**同质化代币技术标准**，也是一套统一的智能合约接口规范。

1\. **同质化代币核心特点**：代币之间完全等价、可互换、可分割，无唯一标识。例如1枚A项目ERC20代币和另一枚同项目代币价值、权益完全一致，可自由兑换、转账。

2\. **标准意义**：统一的接口让所有ERC20代币可以兼容以太坊钱包、交易所、DeFi协议，无需单独适配，实现一键转账、充值、交易、质押等操作，极大降低了区块链生态的交互成本。

## 三、ERC20核心关键特性

这是ERC20代币区别于其他代币的核心属性，也是日常链上交互的基础：

### 1\. 同质化、可互换

同一种ERC20代币无编号、无差异，任意两枚代币价值对等，可自由替换，这也是主流平台币、稳定币、项目治理币均采用ERC20标准的核心原因。

### 2\. 可分割

ERC20代币支持小数位分割，默认精度为18位，最小单位可精确到小数点后18位，能够满足小额转账、交易、兑换等场景需求，不会出现代币无法拆分使用的问题。

### 3\. 总量固定/可控

代币发行总量由智能合约代码提前设定，分为固定总量、可增发、可销毁三种模式，所有代币流转规则公开透明，链上可查，无法被人为篡改。

### 4\. 链上公开透明

所有ERC20代币的转账记录、余额、合约代码、持币地址分布均记录在以太坊区块链上，任何人可通过区块浏览器查询，公开可溯源、不可篡改。

## 四、ERC20六大核心标准函数

ERC20标准规定了智能合约必须实现的基础接口，所有合规ERC20代币都包含以下核心方法，是链上交互的核心逻辑：

### 1\. totalSupply（总发行量）

查询代币的最大发行总量，该数值在合约部署时确定，部分代币合约支持后续增发或销毁修改总量。

### 2\. balanceOf（查询余额）

输入任意钱包地址，即可查询该地址持有当前ERC20代币的余额，是钱包、交易所展示用户资产的核心接口。

### 3\. transfer（直接转账）

核心转账函数，用户可直接将自己钱包内的代币转账至其他地址，转账成功后实时扣减余额、记录链上交易。**注意**：仅能转账本人钱包内的代币。

### 4\. approve（授权）

核心授权函数，用户授权第三方地址（交易所、DeFi合约、机器人等），允许对方调用自己的代币资产，是质押、交易、理财等DeFi操作的前提。

### 5\. transferFrom（代付转账）

配合approve授权使用，第三方地址在获得用户授权额度后，可代为划转用户的代币资产，交易所提币、DeFi流动性挖矿均依赖该函数实现。

### 6\. allowance（查询授权额度）

查询某个地址对第三方的剩余授权额度，可用于核对授权是否过期、是否需要重新授权，规避资产授权风险。

## 五、ERC20代币优缺点总结

### 优点

-   **通用性极强**：全生态通用，适配所有以太坊钱包、交易所、DeFi协议，兼容性拉满；
    
-   **交互便捷**：接口统一，转账、授权、交易逻辑标准化，开发和用户使用成本低；
    
-   **公开透明**：所有数据链上可查，规则透明，无暗箱操作；
    
-   **灵活性高**：支持分割、增发、销毁、授权，适配各类金融场景。
    

### 缺点

-   **无原生安全校验**：早期ERC20标准无内置安全机制，存在重入攻击、授权漏洞等风险；
    
-   **转账不可逆**：链上转账一旦出错（地址输错、转账失误），无法撤回、无法追回；
    
-   **依赖矿工费**：所有转账、授权操作均需要支付ETH作为Gas手续费，以太坊拥堵时手续费高、转账延迟。
    

## 六、易混淆知识点区分

1\. **ETH vs ERC20代币**：ETH是以太坊原生主币，用于支付Gas手续费；ERC20是基于以太坊合约发行的衍生代币，本身无Gas支付能力，转账必须消耗ETH手续费。

2\. **ERC20 vs NFT(ERC721)**：ERC20是同质化代币，可互换、可分割；ERC721是非同质化代币，每一枚独一无二、不可互换、不可分割。

## 七、今日学习总结与后续计划

**学习总结**：今日系统掌握了ERC20代币的定义、核心特性、六大标准函数、应用场景及优缺点，清晰区分了原生币与合约代币、同质化与非同质化代币的核心差异，理解了转账、授权的底层逻辑，夯实了区块链代币生态的基础认知。

**后续学习计划**：下一步将深入学习ERC20安全漏洞、授权风险、代币销毁/增发逻辑，以及对比ERC223、ERC777等升级代币标准，进一步完善代币体系知识。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->



Solidity 进阶语法 · 今日学习笔记（0.8.x 稳定版）

**学习定位**：夯实进阶核心语法，适配合约工程开发、安全优化、Gas 优化，重点掌握 0.8.x 版本独有特性与易踩坑知识点

**核心原则**：懂底层区别、会优化 Gas、规避编译/运行时风险

* * *

## 一、数据类型进阶：自定义值类型

### 1\. 核心作用

0.8.8+ 新增，基于基础类型封装独立自定义类型，**杜绝隐式类型转换**，解决业务参数传参错误、类型混淆问题，提升合约强安全性。

-   包装：`自定义类型.wrap(原始值)`
    
-   解包：`自定义类型.unwrap(实例)`
    

### 3\. 极简示例

```solidity
type TokenId is uint256;
type Amount is uint256;

contract CustomTypeDemo {
    // 两种类型无法互相赋值，编译报错
    function mint(TokenId id, Amount amt) public pure returns(uint, uint) {
        return (TokenId.unwrap(id), Amount.unwrap(amt));
    }
}
```

### 4\. 笔记总结

仅做**编译期类型校验**，底层存储仍为原基础类型，无 Gas 损耗，纯业务安全优化。

* * *

## 二、数据位置深度辨析（进阶重中之重）

三大数据位置：**calldata / memory / storage**，核心差异在拷贝机制、Gas 消耗、使用场景

| 数据位置 | 特性 | Gas | 适用场景 |
| --- | --- | --- | --- |
| calldata | 只读、不拷贝、外部入参专属 | 最低（最优） | external 函数参数，字符串/结构体/数组入参 |
| memory | 临时拷贝、函数内有效、可修改 | 中等 | 函数内部临时变量、返回值组装 |
| storage | 链上持久存储、指针引用、可修改 | 极高 | 合约全局状态变量 |

### 关键踩坑点

`storage` 局部变量是**指针引用**，修改会直接改动链上数据；`memory` 是值拷贝，修改不影响原数据。

* * *

## 三、常量与不可变变量（Gas 优化核心）

### 1\. constant 编译常量

-   编译期硬编码，部署后永久固定
    
-   仅支持数值、字符串，**不能通过构造函数赋值**
    
-   无存储占用，读取 Gas 极低
    

### 2\. immutable 不可变变量

-   部署时（构造函数中）赋值，部署后不可修改
    
-   支持地址、数值等复杂类型
    
-   存储在代码段，而非状态存储槽，读 Gas 接近 0
    

### 示例代码

```solidity
contract ConstDemo {
    uint public constant MAX_SUPPLY = 10000; // 编译常量
    address public immutable OWNER; // 部署固化

    constructor() {
        OWNER = msg.sender;
    }
}
```

### 学习总结

固定业务阈值用 `constant`，部署时确定的唯一值（管理员地址、合约地址）用 `immutable`。

* * *

## 四、错误处理进阶：自定义错误 Custom Error

0.8.4+ 核心特性，**全面替代 require 字符串报错**

### 1\. 核心优势

-   大幅节省 Gas：无需存储字符串信息
    
-   支持传参：可返回具体错误数值、地址，方便链下排查
    
-   代码更简洁，条件判断更灵活
    

### 2\. 标准用法

```solidity
contract ErrorDemo {
    // 定义自定义错误（可携带参数）
    error InsufficientBalance(uint256 have, uint256 need);
    error NotOwner(address caller);

    address public immutable owner;
    uint256 public balance = 100;

    constructor() {
        owner = msg.sender;
    }

    function withdraw(uint256 amt) external {
        if (msg.sender != owner) revert NotOwner(msg.sender);
        if (amt > balance) revert InsufficientBalance(balance, amt);
        balance -= amt;
    }
}
```

### 笔记重点

优先使用 `if + revert 自定义错误`，放弃 `require(条件, "文字报错")`，工程开发标配。

* * *

## 五、函数进阶与特殊函数

### 1\. 可见性与可变性最优实践

-   **external**：外部专属，Gas 比 public 更低，外部调用函数优先用 external
    
-   **view/pure**：只读不写状态，无交易 Gas，仅查询
    
-   **payable**：唯一可接收 ETH 的函数修饰符
    

### 2\. 修饰符 Modifier 进阶

支持**传参修饰符**，可复用权限、参数校验逻辑，代码解耦

```solidity
modifier onlyMax(uint256 max) {
    require(msg.value <= max, "Over limit");
    _; // 执行原函数逻辑
}

function pay() external payable onlyMax(100) {}
```

### 3\. 接收 ETH 双函数机制

-   `receive()`：专属接收**纯 ETH 转账（无 calldata）**
    
-   `fallback()`：函数不存在 / 有 calldata 转账时触发
    

* * *

## 六、继承与重写（工程架构核心）

### 1\. 核心关键字

-   **virtual**：父函数允许被子合约重写
    
-   **override**：子合约重写父函数
    
-   **abstract**：抽象合约，包含未实现函数，无法部署
    
-   **interface**：接口，仅定义函数签名，无逻辑实现，用于合约交互
    

### 2\. 多重继承冲突解决

多父类存在同名函数时，必须显式声明 `override(父类1, 父类2)`，并指定具体调用父类逻辑

```solidity
contract A { function f() public virtual pure returns(uint) { return 1; } }
contract B { function f() public virtual pure returns(uint) { return 2; } }

contract C is A, B {
    function f() public override(A,B) pure returns(uint) {
        return B.f(); // 显式指定父类
    }
}
```

* * *

## 七、库 Library 进阶

### 1\. 两类库函数

-   内存库：操作 memory/calldata 数据，无状态，纯计算逻辑
    
-   存储库：接收 storage 指针，可直接修改合约状态数据
    

### 2\. 全局绑定特性

`using 库 for 类型 global;`可实现**全局合约生效**，无需每个合约重复引入。

* * *

## 八、ABI 编码与底层调用

### 1\. 三大编码函数

-   `abi.encode()`：32字节对齐编码，用于合约 call 调用
    
-   `abi.encodePacked()`：紧凑编码，用于哈希、签名计算（更省空间）
    
-   `abi.decode()`：解码合约返回的字节数据
    

### 2\. 底层调用核心区别

-   **call**：调用外部合约，使用**目标合约存储**，可传 ETH
    
-   **delegatecall**：复用外部合约逻辑，使用**当前合约存储**，代理模式核心
    

* * *

## 九、今日学习复盘（核心口诀）

1.  入参优先 calldata，临时变量用 memory，状态变量固定 storage
    
2.  固定值 constant，部署值 immutable，Gas 优化必用
    
3.  报错不用字符串，自定义 error 省 Gas 易排查
    
4.  父函数 virtual，子重写 override，多继承必须显式指定
    
5.  外部调用优先 external，转账区分 receive/fallback
    

## 十、明日待精进方向

1\. 插槽存储布局与变量打包优化 2. Yul 内联汇编极简优化 3. 事件 indexed 索引与链下数据筛选
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->




Ethers.js 今日学习笔记（v6 最新版）

**学习版本**：Ethers.js v6（当前官方主推稳定版本）

**学习目标**：掌握 Ethers.js 核心概念、五大核心模块、基础实操代码，区分版本差异，具备简单链上交互开发能力

## 一、Ethers.js 基础认知

### 1\. 什么是 Ethers.js

Ethers.js 是一款**轻量、安全、高性能**的 JavaScript/TypeScript 区块链开发库，主要用于以太坊及所有 EVM 兼容公链（Polygon、BSC、Arbitrum 等）的链上交互，是目前 Web3 开发的主流工具库。

核心作用：让浏览器/Node.js 程序实现**读取链上数据、连接钱包、签名交易、调用智能合约、监听链上事件**等全套 Web3 操作。

### 2\. 与 Web3.js 核心区别

Web3.js 是以太坊早期官方库，目前逐渐被淘汰；Ethers.js 是社区主流替代方案，优势显著：

-   **体积更小**：压缩后仅80KB左右，前端加载速度更快
    
-   **架构清晰**：严格区分「读（Provider）」和「写（Signer）」操作，逻辑分层明确
    
-   **安全性更高**：原生支持助记词、私钥加密，内置安全钱包机制
    
-   **开发体验好**：原生支持 TypeScript，API 简洁易懂，错误提示精准
    
-   **规避精度问题**：自动处理链上大整数（BigInt），杜绝数值丢失问题
    

### 3\. 版本核心差异

-   **v5**：老牌稳定版本，存量项目居多，API 相对繁琐
    
-   **v6**：官方主推新版，重构大量 API，模块化更强、性能更优，本次学习核心版本
    

## 二、Ethers.js 五大核心模块（重点）

Ethers.js 所有功能均围绕五大模块展开，是开发的核心基础。

### 1\. Provider 区块链读取器（只读）

Provider 是程序与区块链节点的**只读连接入口**，仅负责查询链上数据，无私钥、无签名、无法发起交易。

**核心能力**：查询账户余额、区块信息、交易详情、合约公开数据、解析链上日志

**常用类型**：

-   JsonRpcProvider：自定义 RPC 节点（Infura、Alchemy、本地节点，前后端通用）
    
-   BrowserProvider：v6 新版专属，替代 v5 Web3Provider，专门用于连接浏览器钱包（MetaMask）
    
-   StaticJsonRpcProvider：稳定 RPC 连接，支持自动重试，适合后端批量查询
    

### 2\. Signer 签名器（写入操作）

Provider 只能读链，**所有链上写入操作（转账、合约调用）必须通过 Signer 签名**，是区分读写操作的核心模块。

**两类核心签名器**：

-   浏览器端 JsonRpcSigner：依托用户钱包签名，私钥由钱包保管，安全无风险，DApp 主流方案
    
-   后端 Wallet：代码导入私钥/助记词签名，适用于脚本、链上机器人，需严格做好私钥保密
    

### 3\. Contract 智能合约交互器

专门用于与链上智能合约交互的核心对象，可理解为「链上合约翻译官」，将 JS 代码与链上二进制数据互相转换。

**合约交互三要素（缺一不可）**：

1.  合约地址：智能合约在区块链上的唯一标识
    
2.  ABI：合约接口描述文件，定义合约所有方法、参数、返回值
    
3.  Provider/Signer：读操作传 Provider，写操作必须传 Signer
    

**合约方法分类**：

-   读方法（view/pure）：免费、无需签名、不消耗 Gas，仅查询数据
    
-   写方法：需要签名、消耗 Gas、修改链上数据，交易上链生效
    

### 4\. Utils 工具函数库

Ethers 内置高频工具方法，无需手动封装，解决链上通用数据处理问题：

-   单位转换：parseEther（ETH 转 wei）、formatEther（wei 转 ETH）
    
-   数据加密：keccak256 哈希计算、签名验证
    
-   格式校验：地址合法性校验、ABI 编解码
    
-   钱包工具：BIP39 助记词生成、BIP44 分层钱包推导
    

### 5\. Event 链上事件监听

实时监听智能合约触发的日志事件，可监控转账、NFT 铸造、合约交互等链上动态，常用于 DApp 实时数据更新、链上数据监控。

## 三、v6 核心实操代码（可直接复用）

### 1\. 安装依赖

```bash
npm install ethers
```

### 2\. 前端连接钱包 + 查询余额

```javascript
import { ethers } from "ethers";

// 1. 初始化浏览器钱包提供者（v6 新语法）
const provider = new ethers.BrowserProvider(window.ethereum);

// 2. 请求用户连接钱包
await provider.send("eth_requestAccounts", []);

// 3. 获取签名器（用于后续写入操作）
const signer = await provider.getSigner();

// 4. 获取钱包地址、查询余额
const address = await signer.getAddress();
const balanceWei = await provider.getBalance(address);
const balanceEth = ethers.formatEther(balanceWei);

console.log("钱包地址：", address);
console.log("账户余额：", balanceEth, "ETH");
```

### 3\. 基础 ETH 转账交易

```javascript
// 构建交易参数
const txParams = {
  to: "接收地址",
  value: ethers.parseEther("0.01") // 0.01 ETH 转为 wei
};

// 发起交易（钱包签名）
const tx = await signer.sendTransaction(tx);

// 等待交易上链确认
await tx.wait();
console.log("交易成功，哈希：", tx.hash);
```

### 4\. 读取智能合约数据

```javascript
// 合约基础信息
const contractAddr = "合约地址";
const contractABI = [/* 粘贴合约 ABI */];

// 初始化合约实例（只读，使用 Provider）
const contract = new ethers.Contract(contractAddr, contractABI, provider);

// 调用合约读方法
const result = await contract.合约方法名();
console.log("合约查询结果：", result);
```

## 四、核心易错点 & 避坑总结

-   **读写分离**：Provider 仅可读，调用合约写方法、转账必须传入 Signer，否则报错
    
-   **单位转换必做**：链上所有数值均为 wei（BigInt），展示需 formatEther，传参需 parseEther，直接运算会精度丢失
    
-   **v6 语法变更**：废弃 Web3Provider，统一使用 BrowserProvider；部分工具函数命名简化
    
-   **私钥安全**：前端禁止手动导入私钥，必须依赖钱包签名；后端私钥需配置环境变量，禁止硬编码
    
-   **交易确认**：sendTransaction 后必须执行 wait()，否则无法确保交易上链成功
    

## 五、今日学习总结

1\. 明确 Ethers.js 核心定位：Web3 链上交互主流库，凭借轻量、安全、架构清晰的优势，全面替代 Web3.js，是 DApp 开发、合约调试的核心工具。

2\. 掌握核心架构逻辑：**Provider 读、Signer 写、Contract 交互、Utils 工具、Event 监听**五大模块分工明确，读写分离是 Ethers 最核心的设计思想。

3\. 熟练掌握基础实操：完成钱包连接、余额查询、ETH 转账、合约只读查询基础操作，理解链上数据单位转换、交易签名、上链确认的完整流程。

4\. 后续学习方向：深入学习合约写入调用、链上事件监听、批量数据查询、交易异常处理、Gas 费用自定义配置。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->





## 今日学习笔记

DApp 架构、开发流程、以太坊基础开发环境、RPC 节点基础认知

## 一、DApp 架构核心组成

DApp（去中心化应用）区别于传统中心化 Web 应用，运行在区块链分布式网络，逻辑与数据无单一主体管控，由全网参与者共同维护，整体分为三大核心模块：

### 1\. 前端（User Interface）

1.  技术栈：HTML/CSS/JS，主流框架 React、Vue，和传统 Web 前端技术栈一致。
    
2.  链交互逻辑（和传统 Web 最大区别）：前端**不直接直连区块链**，依靠两种渠道通信：
    
    -   钱包注入的 Provider（MetaMask 等浏览器钱包）
        
    -   第三方 RPC 节点服务商
        
3.  两类交互操作：
    
    -   只读调用（eth\_call）：读取合约数据、链上状态、事件日志，无需签名、不消耗 Gas；
        
    -   写交易操作：修改合约数据，前端构造交易数据，交由用户钱包签名后，通过 RPC 广播上链执行，消耗 Gas。
        
4.  必备集成：Web3 钱包（MetaMask），负责用户链上身份校验、交易签名，保障资产与隐私安全。
    

### 2\. 智能合约（Smart Contracts）

1.  定位：DApp 的业务核心，全部业务规则写在合约内，部署在区块链上；
    
2.  特性：执行规则自动、交易透明、数据不可篡改；
    
3.  以太坊开发标准：使用 Solidity 语言编写，运行在 EVM（以太坊虚拟机）中；
    
4.  作用：存储持久化数据、定义转账 / 业务逻辑、对外提供读写接口供前端调用。
    

### 3\. 区块链网络（底层链）

承载智能合约运行、交易打包共识、数据永久存储的底层分布式网络（以太坊、Monad、Polygon 等 EVM 兼容链）。

## 二、DApp 完整开发流程（梳理总结）

1.  **需求设计**：确定 DApp 业务逻辑，拆分合约存储结构、交互功能；
    
2.  **合约开发**：使用 Solidity 编写智能合约，完成业务逻辑；
    
3.  **本地测试**：搭建本地开发链，单元测试合约，规避漏洞；
    
4.  **前端开发**：搭建页面，集成 web3/ethers 库，对接钱包与 RPC；
    
5.  **联调交互**：前端实现合约读、写两类接口调用；
    
6.  **测试网部署**：将合约部署至公链测试网（如 Monad Testnet），全流程实机测试；
    
7.  **主网上线**：审计合约后部署主网，正式对外提供服务。
    

## 三、以太坊开发环境搭建要点

### 1\. 基础环境准备

-   运行环境：Node.js + npm/yarn（前端、合约编译测试工具依赖）
    
-   合约 IDE：Remix（在线快速调试）、VS Code+Solidity 插件（本地开发）
    
-   钱包工具：MetaMask，用于测试网转账、交易签名、前端页面连接
    

### 2\. 本地开发链

本地私有链（Hardhat、Foundry、Ganache），优势：无 Gas 消耗、交易秒确认，适合合约迭代与单元测试，无需测试网代币。

### 3\. 钱包与前端交互核心逻辑

前端通过注入 Provider 检测用户钱包，监听链切换、账户切换事件；

-   读数据：无需用户授权，直接 RPC 调用合约 view 函数；
    
-   写交易：需要用户连接钱包、授权签名，等待区块确认后更新页面状态。
    

## 四、RPC 节点服务核心认知

### 1\. RPC 定义

JSON-RPC 是区块链与外部程序通信的标准协议，前端 / 工具通过 RPC 接口向节点发送指令，实现查询链数据、广播交易。

### 2\. RPC 在 Web3 开发中的作用

1.  作为前端与区块链中间桥梁，转发读写请求；
    
2.  同步全网区块、交易、合约状态数据；
    
3.  打包用户签名后的交易，发送至矿工节点打包上链。
    

### 3\. RPC 服务分类

1.  公共免费 RPC：上手简单，但延迟高、并发限制大，适合学习；
    
2.  付费专业 RPC（Alchemy、Infura 等）：稳定低延迟、高并发，适合线上 DApp；
    
3.  自建 RPC 节点：完全自主可控，维护成本高，多用于大型项目。
    

### 4\. RPC 使用最佳实践

-   开发阶段：公共测试网 RPC 快速调试；
    
-   线上项目：多服务商 RPC 做故障备份，避免单点服务宕机；
    
-   区分读写请求：高频读操作做缓存，减少 RPC 请求频次。
    

## 五、今日学习小结

1.  理清 DApp 三层分离架构：前端（交互层）+ 智能合约（业务层）+ 区块链（底层存储执行层），分清读写链上操作的区别；
    
2.  掌握 DApp 从合约到前端、本地到测试网的完整开发链路；
    
3.  理解 RPC 节点是前端和区块链通信的核心载体，区分免费 / 商用 RPC 的适用场景；
    
4.  明确开发必备工具栈：Remix/Hardhat 合约开发、MetaMask 钱包、RPC 节点服务、React/Vue 前端框架。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->






**#学习笔记 - 2026年7月8日**

**今日学习目标**

1. 修复钱包连接问题

2. 完成 dApp 前端开发

3. 将 dApp 部署到 Vercel

4. 整理 Mini Demo 0 和 Week 1 Build Log

5. 确认 Week 2 方向

\---

**一、钱包连接问题排查与修复**

**1.1 问题现象**

点击 "Connect MetaMask" 按钮后，页面显示 "Not Connected"，MetaMask 没有弹出授权窗口。

**1.2 问题原因**

使用 wagmi 框架时，状态同步出现问题。虽然 `window.ethereum` 存在且 MetaMask 可以正常工作，但 wagmi 的 `useAccount()` hook 无法正确检测到连接状态。

**1.3 解决方案**

**放弃 wagmi，改用 ethers.js 直接实现钱包连接**

\`\`\`javascript

// 使用 ethers.js 直接连接钱包

const provider = new ethers.BrowserProvider(window.ethereum)

await provider.send('eth\_requestAccounts', \[\])

const signer = await provider.getSigner()

const addr = await signer.getAddress()

\`\`\`

**1.4 关键代码**

**钱包连接组件**: \[WalletConnect.jsx\](file:///d:/Web3暑期实习/solidity-project/my-app/src/components/WalletConnect.jsx)

\`\`\`javascript

useEffect(() => {

  const checkConnection = async () => {

    if (typeof window !== 'undefined' && window.ethereum) {

      try {

        const provider = new ethers.BrowserProvider(window.ethereum)

        const signer = await provider.getSigner()

        const addr = await signer.getAddress()

        setAccount(addr)

      } catch (err) {

        setAccount(null)

      }

    }

  }

  checkConnection()

}, \[\])

\`\`\`

**监听账户变化**:

\`\`\`javascript

const handleAccountsChanged = (accounts) => {

  if (accounts.length > 0) {

    setAccount(accounts\[0\])

  } else {

    setAccount(null)

  }

}

window.ethereum?.on('accountsChanged', handleAccountsChanged)

\`\`\`

**1.5 学习收获**

| 知识点 | 理解 |

| **wagmi vs ethers.js** | wagmi 是封装层，ethers.js 是底层库；当封装层出现问题时，直接使用底层库更可靠 |

| **钱包连接原理** | 通过 `eth_requestAccounts` 请求授权，然后获取 signer 和地址 |

| **事件监听** | 需要监听 `accountsChanged` 事件，处理用户切换账户的情况 |

\---

**二、dApp 前端开发**

**2.1 技术栈**

| 技术 | 版本 | 用途 |

|------|------|------|

| Next.js | 16.2.10 | 前端框架 |

| Tailwind CSS | 4.4.14 | 样式框架 |

| ethers.js | 6.x | 区块链交互 |

**2.2 组件结构**

\`\`\`

src/

├── app/

│   └── page.js          # 主页（客户端组件）

└── components/

    ├── WalletConnect.jsx    # 钱包连接组件

    ├── MessageList.jsx      # 消息列表组件（Read Function）

    └── PostMessage.jsx      # 发布消息组件（Write Function）

\`\`\`

**2.3 合约交互**

**Read Function（读取消息）**

\`\`\`javascript

const provider = new ethers.BrowserProvider(window.ethereum)

const contract = new ethers.Contract(contractAddress, contractAbi, provider)

const messages = await contract.getMessages()

\`\`\`

**特点**: 不消耗 gas，直接从节点读取数据

**Write Function（发布消息）**

\`\`\`javascript

const provider = new ethers.BrowserProvider(window.ethereum)

const signer = await provider.getSigner()

const contract = new ethers.Contract(contractAddress, contractAbi, signer)

const tx = await contract.addMessage(content)

await tx.wait()

\`\`\`

**特点**: 消耗 gas，需要等待交易确认

**2.4 关键代码**

**消息列表组件**: \[MessageList.jsx\](file:///d:/Web3暑期实习/solidity-project/my-app/src/components/MessageList.jsx)

**发布消息组件**: \[PostMessage.jsx\](file:///d:/Web3暑期实习/solidity-project/my-app/src/components/PostMessage.jsx)

\---

**三、部署到 Vercel**

**3.1 部署步骤**

1. **安装 Vercel CLI**:

   \`\`\`bash

   npm install -g vercel

   \`\`\`

2. **登录 Vercel**:

   \`\`\`bash

   vercel login

   \`\`\`

3. **部署项目**:

   \`\`\`bash

   cd my-app

   vercel deploy --prod

   \`\`\`

**3.2 部署结果**

| 链接类型 | URL |

|----------|-----|

| **主链接** | [https://my-gvhshqdyd-zxb1.vercel.app](https://my-gvhshqdyd-zxb1.vercel.app) |

| **备用链接** | [https://my-app-hazel-eight-93.vercel.app](https://my-app-hazel-eight-93.vercel.app) |

**\### 3.3 学习收获**

| 知识点 | 理解 |

|--------|------|

| **Vercel 部署** | Next.js 项目可以轻松部署到 Vercel，提供免费的 HTTPS 访问 |

| **环境要求** | 部署前需要先构建成功`npm run build`） |

| **域名分配** | Vercel 会自动分配一个随机域名 |

\---

**四、Mini Demo 0 整理**

**4.1 Demo 内容**

| 项目 | 详情 |

|------|------|

| **合约地址** | `0x92e00ffd40925aB9364884901290C18Ce2e060B9` |

| **网络** | Monad Testnet |

| **Demo URL** | [https://my-gvhshqdyd-zxb1.vercel.app](https://my-gvhshqdyd-zxb1.vercel.app) |

**4.2 真实链上操作**

\- ✅ 合约部署（已验证）

\- ✅ 3 次消息发布（已验证）

\- ✅ 区块浏览器可查

**4.3 AI 辅助与人工判断**

| 类型 | AI 贡献 | 人工决策 |

|------|----------|----------|

| 代码编写 | 部署脚本、交互脚本、前端组件 | 私钥管理、技术栈选择 |

| 问题排查 | BigInt 类型错误、RPC 连接问题 | RPC 验证、安全配置 |

| 文档编写 | README、Build Log | 功能需求定义 |

\---

**五、Week 1 Build Log**

**5.1 本周核心产出**

1. **智能合约**: MessageBoard.sol

2. **部署脚本**: scripts/deploy.js, scripts/interact.js

3. **dApp 前端**: Next.js + ethers.js

4. **部署结果**: 合约已上线 Monad Testnet，dApp 已部署到 Vercel

**5.2 方向选择：Tech**

**选择理由**:

\- 兴趣驱动：对智能合约开发和 dApp 构建有浓厚兴趣

\- 能力匹配：已有 Java、Vue 开发经验

\- 实践成果：成功完成合约开发、部署和 dApp 构建

\---

**六、遇到的问题与解决方案**

**问题 1: wagmi 钱包连接失败**

| 项目 | 详情 |

|------|------|

| **现象** | 点击连接按钮后，MetaMask 不弹出授权窗口 |

| **原因** | wagmi 的状态同步问题 |

| **解决方案** | 改用 ethers.js 直接实现 |

**问题 2: Hardhat Ignition 需要交互式确认**

| 项目 | 详情 |

|------|------|

| **现象** | `npx hardhat ignition deploy` 需要用户确认 |

| **原因** | Hardhat Ignition 的安全机制 |

| **解决方案** | 使用自定义部署脚本 |

**问题 3: BigInt 类型错误**

| 项目 | 详情 |

|------|------|

| **现象** | `TypeError: Cannot mix BigInt and other types` |

| **原因** | Solidity 的 `uint` 返回 JavaScript 的 `BigInt` |

| **解决方案** | 使用 BigInt 字面量 |

**\### 问题 4: RPC 连接超时**

| 项目 | 详情 |

|------|------|

| **现象** | 使用官方 RPC 连接超时 |

| **原因** | 公共 RPC 节点不稳定 |

| **解决方案** | 切换到 Ankr 的公共 RPC |

\---

**七、今日学习收获**

  **收获 1: ethers.js 直接连接钱包**

**重要性**: ⭐⭐⭐⭐⭐

学会了使用 ethers.js 直接与 MetaMask 交互，理解了钱包连接的底层原理：

1. 通过 `BrowserProvider` 连接到 MetaMask

2. 通过 `eth_requestAccounts` 请求授权

3. 通过 `getSigner()` 获取签名者

4. 通过 `getAddress()` 获取钱包地址

**收获 2: 合约交互的两种方式**

**重要性**: ⭐⭐⭐⭐

理解了区块链上两种操作的区别：

| 操作类型 | 特点 | Gas 消耗 |

|----------|------|----------|

| **Read Function** | 只读，直接从节点获取 | 免费 |

| **Write Function** | 需要签名，写入链上 | 消耗 gas |

**收获 3: dApp 部署流程**

**重要性**: ⭐⭐⭐⭐⭐

学会了将 dApp 部署到 Vercel 的完整流程，理解了：

1. 构建项目`npm run build`）

2. 登录 Vercel 账号

3. 执行部署命令

4. 获取公开访问链接

**收获 4: 问题排查能力**

**重要性**: ⭐⭐⭐⭐

学会了通过调试信息和日志来定位问题：

1. 添加调试面板查看关键信息

2. 使用测试按钮验证底层功能

3. 通过浏览器控制台查看错误日志

4. 根据错误信息调整技术方案

\---

**八、Week 2 学习计划**

**8.1 技术学习目标**

| 目标 | 优先级 | 说明 |

|------|--------|------|

| Solidity 高级特性 | 高 | 继承、接口、修饰器、库 |

| 前端与智能合约交互 | 高 | Wagmi/Viem 框架深入学习 |

| ERC 标准合约 | 中 | ERC20、ERC721 |

| 智能合约测试 | 中 | Hardhat 测试框架 |

**8.2 实践计划**

1. **完善 dApp**: 添加更多功能（用户消息过滤、消息点赞等）

2. **学习 Wagmi**: 解决之前的状态同步问题，掌握 Wagmi 的正确用法

3. **学习 ERC20**: 编写一个代币合约并部署

4. **编写测试**: 使用 Hardhat 编写合约测试用例

\---

**九、需要帮助的问题**

1. **智能合约安全审计**: 常见漏洞和安全编码最佳实践

2. **Wagmi 状态同步**: 如何正确配置和使用 Wagmi

3. **Gas 优化**: 如何降低合约的 gas 消耗

4. **前端性能**: dApp 的性能优化技巧

\---

 **总结**

今天的学习非常充实！主要完成了：

1. ✅ 修复了钱包连接问题（从 wagmi 切换到 ethers.js）

2. ✅ 完成了 dApp 前端开发（消息列表、发布消息）

3. ✅ 将 dApp 部署到 Vercel，获得了公开访问链接

4. ✅ 整理了 Mini Demo 0 和 Week 1 Build Log

5. ✅ 确认了 Week 2 的 Tech 方向

通过今天的实践，我对 Web3 开发有了更深入的理解，特别是钱包连接和合约交互的底层原理。在遇到问题时，学会了通过调试和测试来定位问题，并选择合适的技术方案。期待 Week 2 的深入学习！
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->







````markdown
学习笔记 - 2026年7月7日



 一、今日学习目标

1.  配置 Hardhat 连接 Monad Testnet
2.  编译 Solidity 智能合约
3.  将合约部署到 Monad Testnet
4.  调用合约的 Read Function
5.  调用合约的 Write Function
6.  学习智能合约安全最佳实践

---

 二、项目概述

### 项目名称
MessageBoard（消息板智能合约）

### 功能描述
一个简单的去中心化消息板应用，允许用户发布和查看消息。

### 技术栈
- **框架**: Hardhat
- **语言**: Solidity ^0.8.28
- **网络**: Monad Testnet (Chain ID: 10143)
- **钱包**: MetaMask
- **交互**: ethers.js

---

## 三、关键知识点

### 3.1 Hardhat 网络配置

在 [hardhat.config.js](file:///d:/Web3暑期实习/solidity-project/hardhat.config.js) 中添加自定义网络：

```javascript
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.28",
  networks: {
    monadTestnet: {
      url: process.env.MONAD_TESTNET_RPC_URL,
      accounts: [process.env.WALLET_PRIVATE_KEY],
      chainId: 10143,
    },
  },
};
```

知识点:
- 使用 `dotenv` 加载环境变量，避免硬编码敏感信息
- `accounts` 字段接受私钥数组，第一个私钥作为部署者账户
- `chainId` 用于验证网络身份

### 3.2 环境变量配置

在 `.env` 文件中存储敏感信息：

```env
MONAD_TESTNET_RPC_URL=https://rpc.ankr.com/monad_testnet
WALLET_PRIVATE_KEY=你的钱包私钥
```

**知识点**:
- `.env` 文件必须添加到 `.gitignore` 中
- 使用公共 RPC（如 Ankr）可以快速连接测试网
- 私钥是钱包的核心，泄露会导致资产损失

### 3.3 合约编译

```shell
npx hardhat compile
```

**知识点**:
- 编译产物存放在 `artifacts/` 目录
- Hardhat 会自动检测 Solidity 版本
- 编译错误会在终端显示详细信息

### 3.4 合约部署

创建部署脚本 [scripts/deploy.js](file:///d:/Web3暑期实习/solidity-project/scripts/deploy.js)：

```javascript
const { ethers } = require("hardhat");

async function main() {
  const [deployer] = await ethers.getSigners();
  console.log("Deploying with account:", deployer.address);

  const MessageBoard = await ethers.getContractFactory("MessageBoard");
  const messageBoard = await MessageBoard.deploy();
  await messageBoard.waitForDeployment();

  const contractAddress = await messageBoard.getAddress();
  console.log("Deployed to:", contractAddress);
}

main();
```

执行部署：
```shell
npx hardhat run scripts/deploy.js --network monadTestnet
```

**知识点**:
- `ethers.getSigners()` 获取配置的账户列表
- `ContractFactory.deploy()` 部署合约到链上
- `waitForDeployment()` 等待交易确认
- `getAddress()` 获取部署后的合约地址

### 3.5 部署结果

| 项目 | 值 |
|------|-----|
| 合约地址 | `0x92e00ffd40925aB9364884901290C18Ce2e060B9` |
| 交易 Hash | `0x372d13974a796aa4c37c74b9b35f0c8366c91875972313e867498d84f52b2866` |
| 部署者地址 | `0x000ca106EC26a06c3180a51aC1D56b0b2521bB46` |

### 3.6 合约交互

#### Read Function（只读调用，无需 gas）

```javascript
const count = await contract.getMessageCount();      // 获取消息数量
const messages = await contract.getMessages();       // 获取所有消息
const msg = await contract.getMessageByIndex(0);     // 获取指定索引的消息
```

**知识点**:
- Read Function 不修改链上状态
- 调用 Read Function 不需要支付 gas
- 返回值直接从节点获取

#### Write Function（写操作，需要 gas）

```javascript
const tx = await contract.addMessage("Hello World!");
await tx.wait();
```

**知识点**:
- Write Function 会修改链上状态
- 需要支付 gas 费用
- 返回 Transaction 对象，需等待确认

### 3.7 智能合约安全

#### 敏感信息保护

在 [.gitignore](file:///d:/Web3暑期实习/solidity-project/.gitignore) 中添加：

```
.env                    # 私钥
deployment-info.json    # 部署信息
```

**安全原则**:
1. 永远不要将私钥提交到 Git
2. 使用环境变量管理密钥
3. 定期轮换密钥
4. 使用 Secret Scanning 工具

---

## 四、遇到的问题与解决方案

### 问题 1: RPC 连接超时

**现象**: 使用 `https://testnet-rpc.monad.xyz` 连接超时

**原因**: 部分公共 RPC 可能不稳定或被墙

**解决方案**: 切换到 Ankr 的公共 RPC `https://rpc.ankr.com/monad_testnet`

### 问题 2: Hardhat Ignition 需要交互式确认

**现象**: `npx hardhat ignition deploy` 会询问是否确认部署

**解决方案**: 使用自定义部署脚本 `scripts/deploy.js`，避免交互式确认

### 问题 3: BigInt 类型错误

**现象**: `TypeError: Cannot mix BigInt and other types`

**原因**: Solidity 的 `uint` 类型在 ethers.js 中返回 `BigInt`，不能直接与普通数字运算

**解决方案**: 使用 `BigInt` 字面量（如 `1n`）或转换方法

```javascript
// 错误写法
const lastMsg = await contract.getMessageByIndex(newCount - 1);

// 正确写法
const lastMsg = await contract.getMessageByIndex(newCount - 1n);
```

---

## 五、学习收获

### 技术层面
1. ✅ 掌握了 Hardhat 配置自定义网络的方法
2. ✅ 学会了使用 ethers.js 部署和交互智能合约
3. ✅ 理解了 Read Function 和 Write Function 的区别
4. ✅ 掌握了环境变量和 .gitignore 的安全配置

### 实践层面
1. ✅ 成功将合约部署到真实测试网
2. ✅ 完成了完整的合约交互流程
3. ✅ 学会了排查 RPC 连接问题
4. ✅ 掌握了 BigInt 类型的处理方法

### 安全意识
1. ✅ 理解了私钥保护的重要性
2. ✅ 学会了配置 .gitignore 防止敏感信息泄露
3. ✅ 了解了测试网和主网的区别

---

## 六、下一步学习计划

1. 📚 学习 Solidity 高级特性（继承、接口、库）
2. 📚 学习测试网代币获取方法
3. 📚 学习前端与智能合约交互（Wagmi/Viem）
4. 📚 学习智能合约安全审计基础
5. 📚 尝试部署到其他测试网（Sepolia、Goerli）

````
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->








今天已经把安装插件、创建钱包、添加网络、领水、看区块链浏览器这些步骤全部完成了
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
