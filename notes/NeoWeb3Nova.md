- GitHub ID: 221855057
- Name: NeoWeb3Nova
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-26
<!-- DAILY_CHECKIN_2026-07-26_START -->
# 2026-07-26

今日完成：  
  
1\. 对本周任务进行了集中收尾和梳理，汇总了团队组建、问题定义、Mini Demo 范围、技术论证、团队计划、重要决定与 AI 使用记录，并检查了各文档中的成员、方向和范围是否一致。  
  
2\. 明确 Week 3 继续采用「自然语言 swap 确认助手」作为最小可展示路径：通过 Moss Kuru Capability 和 simulate 生成交易模拟结果，再用「付出 / 收到 / 风险」帮助用户理解交易，最终签名权仍保留给用户。  
  
3\. 完成创意方向复盘。当前 Mini Demo 方向适合稳定交付；Riso 提出的「不是我干的 / NOT MY FAULT」在 AI × Web3、Moss、Monad 原生性和 Demo 冲击力上更有潜力，保留为 Week 4 Hackathon 的重点候选。  
  
4\. 补齐本周后半段的每日记录，并将已完成的文档成果与尚未形成证据的 Demo、用户测试、交易哈希等事项明确区分，避免把计划误写成完成。  
  
今日收获：  
  
本周最重要的不是堆叠更多功能，而是形成清晰的决策链：先确定真实用户问题，再证明为什么需要 Monad 和 Moss，最后锁定一周内能够交付的范围。今天通过集中梳理，把散落的任务和选题沉淀为可追溯的交付物，也明确了下一阶段需要通过真实 Demo、用户反馈和链上证据补齐的部分。
<!-- DAILY_CHECKIN_2026-07-26_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
# 2026-07-24

## 今日完成

### 选题与创意评审

-   **Riso 四个候选方案的评委视角评审**：完成 `submissions/week-03/riso-ideas-judge-review.md`
    
    -   从 6 个维度（Monad-native、AI × Web3、Moss 利用、技术深度、Demo 冲击力、新颖性）对方案 1–4 打分
        
    -   结论：只有「不是我干的 / NOT MY FAULT」同时满足 Tech track、AI × Web3、Moss 原生与高 Demo 冲击力，建议作为 Week 4 Hackathon 首位备选
        
    -   其余三个方案（全民攻敌 / BOSS IS LEARNING / 忒修斯之船）因缺少 AI/Moss 关联或技术深度不足，建议仅作创意储备
        
-   **团队 Mini Demo 方向保持锁定**：自然语言 swap 确认助手
    
    -   用户输入「swap 100 USDC to MON on Monad」→ 预置意图模板 → Moss Kuru Capability 树 + simulate →「付出 / 收到 / 风险」确认页 → 用户签名
        
    -   今日评审进一步验证：当前方向更稳妥，可在 Week 3 留下 Demo Evidence；方案 4 更适合作为 Week 4 升级或备选
        

### 团队协调与计划同步

-   确认团队本周分工（基于 `submissions/week-03/team-plan.md`）：
    
    -   Neo：前端骨架、意图模板、Moss simulate 接入、Demo Evidence
        
    -   Riso：竞品分析、测试用户招募、Pitch 提纲
        
    -   eleven：3 屏主路径线框 / 高保真、Demo 录屏素材
        
-   明确 Week 3 不做范围：多协议对比、自动签名/执行、私钥托管、多链、借贷/质押、LLM 自然语言解析、链上 Receipt 事后对账
    

### 文档整理

-   维护并更新团队交付物索引与逻辑一致性：
    
    -   `submissions/week-03/brainstorm-meeting.md`：脑暴会议结论与方向选择
        
    -   `submissions/week-03/problem-statement.md`：目标用户、核心问题、方案、验证方式
        
    -   `submissions/week-03/mini-demo-scope.md`：Must Have / Nice to Have / Not This Week
        
    -   `submissions/week-03/team-plan.md`：分工、时间节点、风险应对
        
    -   `submissions/week-03/decisions-and-ai-log.md`：团队决定与 AI 使用记录
        
    -   `submissions/week-03/riso-ideas-judge-review.md`：今日新增评委视角评审
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# 2026-07-23

今日主题: 团队创意投票锁定 & Mini Demo 技术骨架推进  
学习投入时长: 约 2–3 小时  
  
今日完成  
  
\- \[x\] 查看 Week 3 / Day 4 任务与验收要求  
\- \[x\] 更新本地项目记录  
\- \[x\] 今日打卡文件创建：daily/[2026-07-23.md](http://2026-07-23.md)  
  
今日证据  
  
| 产出 | 路径 |  
|----------|---------------------|  
| 今日打卡 | daily/[2026-07-23.md](http://2026-07-23.md) |  
  
\> 注：今日按指令仅生成打卡信息，未新增或修改 submissions、Moss 源码、Demo 代码等其他交付物。  
  
小队快照  
  
| 角色 | 人 | 状态 |  
|------|--------|--------------------------------------|  
| Dev | Neo | ✅ 打卡已更新；待方向锁定后推进 Demo |  
| PM | Riso | ⏳ 待确认创意投票与优先级反馈 |  
| UI | eleven | ⏳ 待确认创意投票与优先级反馈 |  
  
今日复盘  
  
\- 收获: 维持每日记录连续性，为后续方向锁定后快速补全 Evidence 留出结构。  
\- 最卡: 暂无——今日范围明确，仅生成打卡。  
\- 是否更接近作品集: 否——今日未产生新的交付物，但保持了项目日志的完整。  
  
  
  
完整文件路径：/home/neo/workspace/projects/Web3SummerInternshipProgram-MonadBuilderCamp/daily/[2026-07-23.md](http://2026-07-23.md)
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
# 2026-07-22

主题：项目创意收集与 MVP 方向梳理  
  
今日完成：  
  
1\. 从目标用户、真实痛点、核心功能、Monad 必要性和 Moss 使用方式等维度，完成了 4 张项目创意卡片：  
\- Receipt Chat：把链上交易效果解释成人话  
\- Moss Capability 沙盒：帮助协议开发者快速验证 Capability  
\- On-chain 白名单意图信箱：让 DAO 成员理解并执行链上操作  
\- 链上 Receipt 保险柜：结构化保存 DeFi 交互记录  
  
2\. 对 4 个方向进行了优先级和技术可行性评估。目前优先考虑 Receipt Chat，因为它可以直接复用 Kuru Swap、Moss Capability 和交易模拟链路，本周落地风险最低。  
  
3\. 进一步明确了 Mini Demo 的核心价值：通过 Moss 在签名前模拟交易，将复杂的链上变化转化为普通用户能够理解的确认信息。  
  
今日收获：  
  
好的 Web3 创意不能只回答「要做什么」，还要回答「为谁解决什么问题」「为什么需要 Monad」以及「为什么现有方案无法替代」。技术选型也必须服从用户价值和 Demo 范围，而不是单纯堆叠功能。  
  
明日计划：  
  
与队友对齐创意优先级，锁定最终方向，并开始搭建 Mini Demo 前端骨架，验证 Kuru Swap、Moss Capability 和 simulate 的完整技术链路。
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
# 2026-07-21

1\. 今日目标  
  
搭建前端项目骨架（Vite + React + wagmi/viem + TypeScript），并跑通第一条离线模拟链路：  
1\. 预置意图模板 → 解析 tokenIn / tokenOut / amountIn  
2\. 调用 Moss registry.action("kuru", "swap", account, params) 生成 Capability 树  
3\. 用 createTraceSimulator(...).simulate(capability) 获取 Receipts / Changes  
4\. 把 Receipt 翻译成确认页可展示的结构化数据  
5\. 用 mock 钱包状态完成“点击确认 → 展示待签名交易”的流程  
  
  
  
2\. 计划产出  
  
| 产出 | 路径 | 说明 |  
|----------------|--------------------------------------|----------------------------------------------------------|  
| 前端项目骨架 | experiments/moss-demo/ | Vite + React + TS + wagmi/viem，含 ESLint + Prettier |  
| 意图解析模块 | src/intent/parser.ts | 预置模板 swap {amount} {tokenIn} to {tokenOut} |  
| Moss 模拟 Hook | src/moss/useSimulateSwap.ts | 调用 registry.action + simulate + parseReceipt |  
| 确认页组件 | src/components/ConfirmationCard.tsx | 展示 in/out/slippage/approval/risk |  
| 钱包签名按钮 | src/components/SignButton.tsx | 用 wagmi useSendTransaction 触发签名（至少展示 pending） |  
| 今日日志 | submissions/week-03/[day-3-dev-log.md](http://day-3-dev-log.md) | 记录实现过程、踩坑、明日计划 |  
  
  
  
3\. 关键待验证点  
  
\- kuru.swap Capability 在当前 Moss 子模块 commit 下能否正常生成未签名交易树  
\- createTraceSimulator 对 mainnet 还是 testnet 生效；若无 funded account，state override 是否自动生效  
\- 本地 mock 数据能否准确复现 Day 4 UI 需要的字段结构  
  
  
  
4\. 风险与缓解  
  
| 风险 | 缓解 |  
|-------------------------------------|------------------------------------------------------------------|  
| Moss 子模块有未提交修改 | 先检查子模块状态，必要时在子模块内 commit 或在父仓库用临时 patch |  
| mainnet RPC simulate 不稳定 | 先用离线 mock 验证 UI，晚间再切 mainnet |  
| wagmi/viem 与 Moss 的 viem 版本冲突 | 统一使用 pnpm workspace 或锁定 viem 版本 |  
| 时间不够连钱包 | 先做到“展示待签名交易对象”，再补 useSendTransaction |
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->
# 2026-07-17

Week 2 Day 5｜Moss Neverland 协议适配器 PR 提交 + Proof of Work 记录  
  
今日完成：  
  
\- 完成并提交 Moss 新 Protocol Adapter：Neverland（Monad-native Aave V3 借贷市场）。  
\- PR 链接：[https://github.com/NeoWeb3Nova/moss/pull/1](https://github.com/NeoWeb3Nova/moss/pull/1)  
\- 适配器能力：supply、withdraw、accountData 查询。  
\- 核心设计：aToken 地址运行时解析、量化 expects、自动 approval、Supply/Withdraw 事件收据确认。  
\- 验证通过：pnpm -r build / pnpm -r typecheck / pnpm biome check . / MOSS\_SKIP\_E2E=1 pnpm -r test 全部通过。  
\- 补齐交付物：README、changeset、ABI 来源头、离线 shape 测试、主网 e2e 测试。  
\- 在父仓库生成并推送两份提交证明：  
\- submissions/week-02-tech/[moss-adapter-challenge.md](http://moss-adapter-challenge.md)  
\- submissions/week-02-tech/[moss-proof-of-work.md](http://moss-proof-of-work.md)  
\- 父仓库提交：55a8c2b  
  
今日收获：  
  
1\. 协议适配不是简单封装合约调用，而是要把协议原语映射到 Moss 的 discover → load → action → simulate 能力模型里。  
2\. 离线测试必须处理无 RPC 的情况；supply 需要 aToken 地址时，用 stub 保留形状验证即可。  
3\. 开源贡献的证据链很重要：PR、commit、验证日志、README、changeset、父仓库记录，一个都不能少。  
4\. 同一项真实工作可以同时满足多个任务要求（新增 Adapter + Proof of Work），但要分别生成对应的提交文件。  
  
下一步：  
  
\- 等待 Maintainer Review 反馈。  
\- 联网时补跑主网 e2e，确认零 warning。  
\- 收到 Review 或 Merge 后更新提交证明中的状态与链接。  
  
GitHub：[https://github.com/NeoWeb3Nova](https://github.com/NeoWeb3Nova)  
学习仓库：[https://github.com/NeoWeb3Nova/Web3SummerInternshipProgram-MonadBuilderCamp](https://github.com/NeoWeb3Nova/Web3SummerInternshipProgram-MonadBuilderCamp)
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
# 2026-07-16

今日完成：  
  
\- 整理《Web3 如何从 0 到 1 开始运营》分享会，沉淀完整转录与结构化笔记，梳理了从曝光、报名、社群承接、组队 Build、项目提交到生态留存的完整运营漏斗。  
\- 跟进 Moss Core PR #36，收到 Maintainer 正式反馈：上游参数系统已完成重构，需要基于最新 main 重新实现 unsafe number 防护，并补充运行时边界测试和 TypeScript 编译期 fixtures。  
\- 启动 Moss 的 Neverland 协议适配器探索，完成首版 supply、withdraw 和账户健康数据查询能力，并接入 Moss MCP Server 的协议目录。  
\- Neverland Adapter 已通过 build 和 typecheck；离线测试中 2 项通过、1 项暴露测试 fixture 缺少 readContract mock，当前仍是开发中草稿，不提前声明完成。  
  
今日收获：  
  
1\. 开源贡献不能停留在“提交 PR”，上游架构变化后，需要把原始安全意图迁移到新的抽象边界。  
2\. Monad 协议适配不仅是封装合约调用，还要处理授权、动态查询 aToken、量化 expects、模拟验证和事件回执。  
3\. 测试失败也是有效进展：它明确指出离线 Plan 构建测试需要模拟链上读取，而不是掩盖问题或跳过验证。  
4\. 黑客松运营的核心不是单次宣发，而是持续降低“曝光 → 报名 → 参与 → Build → 提交 → 留存”每一环的流失。  
  
下一步：  
  
\- 基于 Moss 最新 main 重构 PR #36，在新的 Zod uint 参数边界实现安全整数校验。  
\- 修复 Neverland 离线测试 fixture，继续验证主网模拟与零 warning 目标。  
\- 补齐 Neverland Adapter 的 README、ABI 来源说明、changeset 和完整质量门。  
  
Moss PR：[https://github.com/nishuzumi/moss/pull/36](https://github.com/nishuzumi/moss/pull/36)  
GitHub：[https://github.com/NeoWeb3Nova](https://github.com/NeoWeb3Nova)  
学习仓库：[https://github.com/NeoWeb3Nova/Web3SummerInternshipProgram-MonadBuilderCamp](https://github.com/NeoWeb3Nova/Web3SummerInternshipProgram-MonadBuilderCamp)
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->
# 2026-07-15

Week 2｜Moss Core 首个开源 PR 打卡  
  
今日完成：  
\- 深入定位 Moss Core 的 unsafe numeric uint 风险：JavaScript 超出安全整数范围后可能先发生舍入，再被 BigInt 转成错误链上整数。  
\- 按 TDD 先复现失败，再增加 Number.isInteger / Number.isSafeInteger 最小 guard。  
\- 保留 safe integer 和任意精度十进制字符串支持，并补充负数、小数及 uint128 最大值测试。  
\- 完成 changeset、lint、build、typecheck、离线全量测试和 Monad mainnet e2e，全部本地通过。  
\- 独立 Review 结论 READY，无 Critical / Important。  
\- 已向 Moss 官方仓库提交 PR：[https://github.com/nishuzumi/moss/pull/36](https://github.com/nishuzumi/moss/pull/36)  
\- GitHub 主页：[https://github.com/NeoWeb3Nova](https://github.com/NeoWeb3Nova)  
  
当前状态：GitHub Actions 正等待 Maintainer 批准运行，尚未收到 Review 或 Merge；后续会继续跟进并补充证据。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
# 2026-07-14

【Week 2｜Dev / Tech Track 打卡】  
  
今日围绕 Moss 开源项目，完成了从项目调研、源码实测到贡献规划和内容输出的一整套学习闭环。  
  
✅ 今日完成  
  
1\. 完成 Tech Role Statement，明确 Week 3 进入团队后承担 Tech / Core Dev，主要负责合约、前端集成和技术验证。  
  
2\. 深度阅读 Moss 的 README、Docs、Issues、PR 和代码结构，理解其核心流程：  
discover → load → action → simulate  
  
3\. 本地实测 Moss：  
\- 完成依赖安装和全量构建  
\- 通过离线测试  
\- 跑通零私钥 WMON Wrap 示例  
\- 验证模拟结果返回 warnings: \[\]  
  
4\. 完成 GitHub Exploration Log，重点分析：  
\- Issue #13：ERC-4626 tokenized vaults  
\- PR #19：Windows CRLF 修复  
\- Issue → Fix → CI → Review 的开源协作流程  
  
5\. 制定 Moss 开源贡献计划，将首个贡献收敛为可独立审查的 ERC-4626 ABI 基础 PR：  
IERC4626.sol → ERC4626Abi → focused tests → docs / changeset → Draft PR  
  
6\. 完成两篇 Moss 公众号内容：  
\- 开发者深度文章：解释 Plan、量化 expects、双 tracer 模拟、effects 对账、意图对齐与签名隔离  
\- 新手教程：从零跑通一笔 Monad 链上交易模拟，整理常见错误、MCP 接入和贡献路径  
  
7\. 完成公众号排版、封面及 4 张 Moss 原理配图，并完成图片内容与文字标签 QA。  
  
💡 今日收获  
  
Moss 的价值不是让 AI Agent 更快发起交易，而是在用户签名前，用量化声明、真实状态模拟和机械对账验证交易效果。即使模拟结果为零 warning，也只能证明实际 effects 没有突破声明边界，不能替代用户意图检查和钱包复核。  
  
开源贡献同样不是“先写一大块代码”，而是先确认真实需求，再用边界清晰、容易测试、容易审查的小 PR 降低 Maintainer 的协作成本。  
  
⚠️ 遇到的问题  
  
文档中的 src/play.ts 示例文件实际不存在。通过检查项目目录找到真实入口，改用 src/wmon-wrap.ts 后成功跑通。  
  
另外，Moss 当前没有启用 GitHub Discussions，因此如实记录其主要协作渠道是 Issues 和 Pull Requests，没有虚构讨论内容。  
  
📌 下一步  
  
阅读 ADR 0007 / 0009，梳理 ERC-4626 的实例化方案；先在 Issue #13 对齐首个 PR 的范围，再推进 ABI 生成链、focused tests、changeset 和 Draft PR。  
  
一句话总结：  
  
今天不只是“读懂了一个项目”，而是完成了从真实运行、问题定位、源码理解到贡献路径设计的完整开源学习闭环。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
# 2026-07-13

今日完成：  
\- 确定 Dev / Tech 主方向。  
\- 提交三份产出：  
\- submissions/week-02-tech/[role-choice-card.md](http://role-choice-card.md)  
\- submissions/week-02-tech/[week-2-role-log.md](http://week-2-role-log.md)  
\- submissions/week-02-tech/[ai-collaboration-log.md](http://ai-collaboration-log.md)  
\- 提交今日打卡：  
\- daily/[2026-07-13.md](http://2026-07-13.md)  
\- 明确本周最小 Demo：基于 Foundry 管理、带前端页面的最小可运行原型。  
\- 明确 AI 协作原则：方向人工拍板，执行交给 AI。  
\- 推送到 remote：NeoWeb3Nova/Web3SummerInternshipProgram-MonadBuilderCamp.git，分支 master。

[https://github.com/NeoWeb3Nova/Web3SummerInternshipProgram-MonadBuilderCamp](https://github.com/NeoWeb3Nova/Web3SummerInternshipProgram-MonadBuilderCamp)  
  
明日计划：  
\- 在 experiments/week-02-contract/ 初始化 Foundry 项目。  
\- 编写第一个 Solidity 合约并通过本地测试。  
\- 记录第一条代码协作相关的 AI Collaboration Log。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
# 2026-07-10

今天是 Web3 Summer Internship Program / Monad Builder Camp 的 Day 5，主题是「AI 工具与 Vibe Coding 入门」。  
  
今日完成：  
1\. 根据 Week 1 Day 5 任务，准备 AI 学习记录提交物 submissions/week-01/[ai-learning.md](http://ai-learning.md)。  
2\. 明确 Vibe Coding 的核心：人用自然语言驱动 AI 写代码，但人必须审查、判断和修正输出。  
3\. 规划今日学习路径：选择一个 AI coding / learning 工具；选一个 Web3 概念让 AI 解释；用 AI 生成 3-5 个 Monad DApp / 合约想法；记录 prompt、输出、人类判断和修正。  
4\. 更新本地项目每日学习日志 daily/[2026-07-10.md](http://2026-07-10.md)。  
  
今日收获：  
\- AI-native Builder 的关键不是会用 AI 写代码，而是能记录「AI 帮了什么、我验证了什么、我做了什么判断」。  
\- 链上学习要留下证据链：钱包地址、交易哈希、网络配置、AI 协作记录都会成为 Proof of Learning / Proof of Work。  
\- 对 AI 输出做修正和说明，才是自己的学习成果。  
  
今日卡点：  
\- 具体 AI 工具和概念尚未选定，需要根据个人环境决定。  
\- 嘉宾分享和 Co-learning 需要登录平台或等待回放。  
  
明日计划：  
\- 完成 AI 概念解释与 DApp 想法列表。  
\- 把 prompt、输出、修正写进 submissions/week-01/[ai-learning.md](http://ai-learning.md)。  
\- 阅读 docs/[monad-technical-notes.md](http://monad-technical-notes.md)，为 Day 6 技术基础做准备。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

今天是 Web3 Summer Internship Program / Monad Builder Camp 的 Day 4，主题是“第一笔 Monad Testnet 链上交易准备”。  
  
今日完成：  
1\. 提交并整理了当前 Monad Builder Camp 项目仓库，形成两次本地提交：  
\- e297688 docs: update monad builder camp learning notes  
\- 1de3756 docs: prepare day 4 monad testnet transaction  
2\. 从 Monad 官方文档确认 Testnet 网络参数：  
\- Network Name: Monad Testnet  
\- Chain ID: 10143 / 0x279f  
\- Currency Symbol: MON  
\- RPC URL: [https://testnet-rpc.monad.xyz](https://testnet-rpc.monad.xyz)  
\- Faucet: [https://faucet.monad.xyz](https://faucet.monad.xyz)  
\- Explorer: [https://testnet.monadvision.com](https://testnet.monadvision.com) / [https://testnet.monadscan.com](https://testnet.monadscan.com)  
3\. 用 JSON-RPC 实测 Monad Testnet RPC，确认 eth\_chainId 返回 0x279f，与官方文档一致。  
4\. 更新了网络配置文档：  
\- submissions/week-01/[network-config.md](http://network-config.md)  
5\. 更新了钱包配置实验记录：  
\- experiments/monad-wallet-setup/[README.md](http://README.md)  
6\. 创建了第一笔链上交易记录模板：  
\- submissions/week-01/[first-tx.md](http://first-tx.md)  
  
今日收获：  
链上任务的关键不是“知道参数”，而是形成一条可验证的 Proof of Work 证据链：  
  
官方文档 → RPC 实测 → 钱包配置 → Faucet 领水 → 交易哈希 → Explorer 链接。  
  
Testnet 虽然没有真实资产价值，但流程非常接近真实链上开发：网络配置、RPC、钱包签名、Gas、Explorer 查询，每一步都对应真实产品开发中的基础能力。  
  
今日卡点：  
1\. Web3Career 学习面板需要登录后才能查看每日任务，未登录状态下只能看到项目概览。  
2\. 钱包添加网络、Faucet 领水和交易签名必须由本人完成，AI 不能接触私钥或助记词，也不能代替签名。  
3\. 下一步需要手动完成第一笔 Monad Testnet 转账或 Remix 合约部署，并回填交易哈希。  
  
下一步计划：  
1\. 在钱包中添加 Monad Testnet。  
2\. 从 [https://faucet.monad.xyz](https://faucet.monad.xyz) 领取测试 MON。  
3\. 完成一笔小额测试网转账，或使用 Remix 部署 Gmonad.sol 合约。  
4\. 记录交易哈希、Explorer 链接和截图，补全 submissions/week-01/[first-tx.md](http://first-tx.md)。  
  
一句话总结：  
今天把 Monad Testnet 的网络参数和交易记录模板准备好了，下一步就是用真实钱包签名，把学习记录推进成可验证的链上 Proof of Work。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

今日完成：  
1\. 根据 Week 1 Day 3 任务，整理 Monad Testnet 配置清单：RPC URL、Chain ID、Explorer、Faucet。  
2\. 在本地项目建立/更新了网络配置提交物 submissions/week-01/[network-config.md](http://network-config.md) 和实验记录 experiments/monad-wallet-setup/[README.md](http://README.md)。  
3\. 计划在钱包中手动添加 Monad Testnet，并从官方 Faucet 领取测试币。  
4\. 确认安全边界：私钥/助记词由本人离线保管，项目文件中只记录公开地址和网络参数。  
  
今日收获：  
\- Testnet 是主网的平行测试环境，代币无真实价值，但配置流程和主网一致。  
\- 链上交互前必须确认四个基础参数：RPC、Chain ID、Explorer、Faucet。  
\- 网络参数会随时间变化，养成从官方文档验证的习惯，而不是照抄旧教程。  
  
今日卡点：  
\- 钱包添加网络和领水需要本人在浏览器操作，AI 不能代为完成。  
\- 昨天（Day 2）的测试钱包地址尚未回填到项目，今天需要一并补齐。  
  
明日计划：  
\- 完成第一笔 Monad Testnet 链上交易。  
\- 记录交易哈希、Explorer 链接和交互反思。  
\- 为 Week 1 后续任务（AI 工具、Monad 技术基础）做准备。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

周次: Week 1 - 认知奠基期 / 进入 Onchain World  
Day: Day 2  
主题: 钱包与安全  
  
今日完成  
  
1\. 对照 Week 1 Day 2 任务，明确今日主线为「钱包与安全」：安装 MetaMask / 兼容 EVM 钱包、创建专用测试钱包、离线纸笔备份助记词、记录公开地址、写 300 字概念解释。  
2\. 整理 7 月 6 日分享会《DevRel 的成长之路 —— 从 Builder 到生态连接者》，生成结构化笔记 docs/[devrel-growth.md](http://devrel-growth.md) 与完整转录 docs/[devrel-growth-transcript.md](http://devrel-growth-transcript.md)。  
3\. 更新项目 [README.md](http://README.md)，加入 DevRel 分享会资料入口。  
4\. 完成 Monad 理解任务：从 Research / Tech / Ops 三个方向梳理高频交互应用场景，输出 submissions/week-01/[monad-high-frequency-app.md](http://monad-high-frequency-app.md)。  
5\. 明确钱包安全原则：测试钱包与主资产钱包隔离；助记词 / 私钥不发给任何人、不上传云端、不复制给 AI；项目内只记录公开地址。  
  
今日收获  
  
\- 钱包不是「账号密码」模型，私钥控制资产，助记词可恢复私钥，地址只是可公开的链上身份。  
\- Web3 学习要留下证据链：钱包地址、交易哈希、截图、学习笔记、项目文档都是 Proof of Learning / Proof of Work。  
\- DevRel 分享让我意识到，Web3 职业入口不只纯技术岗位，也可以从 Builder 作品、社区贡献、运营、研究、生态连接逐步进入。  
  
今日卡点  
  
\- 钱包创建和助记词备份涉及安全敏感信息，必须本人操作，不能让 AI 接触助记词或私钥。  
\- Track 方向（Tech / Ops / Research）尚未最终确定，计划先完成 Week 1 基础任务，再结合实际产出选择主线。  
  
明日计划  
  
\- 完成并核对 submissions/week-01/[wallet.md](http://wallet.md)。  
\- 配置 Monad Testnet。  
\- 获取 Faucet 测试币。  
\- 保存网络配置、Faucet 链接和截图到 assets/week-01/。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
# 2026-07-06

今天核心学习了，monad的gas机制：

## Monad 和 Ethereum 最大区别：Gas Limit 是否会真的收费

这是重点。

在 **Ethereum** 上：

```
实际手续费 = Gas Used × 实际 Gas Price
```

假设你设置：

```
Gas Limit = 100,000
Gas Used = 21,000
```

Ethereum 通常只按 `21,000` 收费，没用完的 gas limit 不扣钱。

但在 **Monad** 上，官方文档写的是：

```
实际手续费 = Gas Limit × Gas Price
```

也就是说，在 Monad 上，如果你把 Gas Limit 设置太高，哪怕实际只用了很少，也可能要按你声明的 Gas Limit 付费。Monad 这样设计，是为了配合它的 **异步执行架构**：区块 leader 在执行交易之前就要构建区块、验证者也会先投票；如果允许用户随便报很高 gas limit 但最后只按 gas used 付费，就容易制造 DoS/资源占用问题。

简单说：

```
Ethereum：Gas Limit 更像“保险额度”，没用完会退。
Monad：Gas Limit 更像“资源预订量”，你报多少就按多少算。
```

## 4\. Monad 为什么要这么设计？

因为 Monad 的目标不是简单复制 Ethereum，而是提高并行执行、高吞吐和异步处理能力。

在 Ethereum 里，执行和区块处理更紧密，实际用了多少 gas 比较容易成为收费依据。

Monad 为了提升性能，采用延迟/异步执行思路：交易先被排进区块，执行结果后面再处理。这样如果仍按实际 Gas Used 收费，用户可以故意设置很大的 Gas Limit，占用区块资源，但最后只执行很少逻辑，成本很低。Monad 按 Gas Limit 收费，就是为了让“占用资源”本身有成本。

这也是为什么在 Monad 上写合约、调钱包、估 gas 时，**Gas Limit 不能像 Ethereum 那样随便给很大 buffer**。官方和生态文章都建议使用 `eth_estimateGas` 作为基准，再加一个适度 buffer，不要过度放大。

## 5\. Base Fee 调整机制也不同

Monad 和 Ethereum 都有动态 Base Fee，但参数不完全一样。

Ethereum 的 Base Fee 会根据上一个区块的 gas 使用量调整，目标区块大小是 gas limit 的一半，Base Fee 每个区块最多上下调整约 12.5%。

Monad 的官方文档说，它的 Base Fee 控制器类似 EIP-1559，但特点是：

```
上涨更慢
下降更快
最低 Base Fee = 100 MON-gwei
Block gas limit = 200M gas
Transaction gas limit = 30M gas
```

Monad 生态技术文章还提到，Monad 的目标利用率是 80%，高于 Ethereum EIP-1559 的 50% 目标，这更适合高吞吐链的资源利用方式。

## 6\. 一句话总结

**Monad 的 Gas 价格模型像 Ethereum：Base Fee + Priority Fee + Max Fee；但收费对象不一样：Ethereum 主要按 Gas Used 收费，Monad 按 Gas Limit 收费。**
<!-- DAILY_CHECKIN_2026-07-06_END -->

<!-- DAILY_CHECKIN_2026-07-28_START -->
# 2026-07-28

&#x20;【Web3 暑期实习计划｜Monad Builder Camp 每日打卡】\
\
&#x20;   日期：2026 年 7 月 28 日\
&#x20;   周次：Week 4 Day 2\
&#x20;   方向：Tech / Dev Builder\
&#x20;   主题：验证 Agent 钱包交易黑匣子 Idea，完成组队与需求预演\
\
&#x20;   今日完成：\
\
&#x20;   1\. 完成 Problem & Idea Card，确定 Week 4 Hackathon 方向为「Agent Wallet Flight Recorder｜Agent 钱包交易黑匣子」。\
\
&#x20;      我想帮助使用 AI Agent 操作链上资产的个人用户，解决两个问题：签名前看不懂自己将授权什么；交易异常后无法还原「用户意图—Agent 提案—风险提示—用户确认—实际执行」之间发生了什么。\
\
&#x20;   2\. 完成团队确认。Week 3 原团队继续合作：\
&#x20;      \- Neo：Dev / Tech Lead，负责 Moss 集成、决策收据和技术验证；\
&#x20;      \- Riso：Product / Research / Pitch，负责需求验证和项目表达；\
&#x20;      \- eleven：UI / Visual Design，负责核心流程和证据时间线设计。\
\
&#x20;      三人每周各投入约 6 小时，继续通过微信群和 GitHub 协作。\
\
&#x20;   3\. 使用 3 个 AI Persona 进行了需求预演：\
&#x20;      \- Agent 钱包新手最在意金额、授权对象和最坏损失；\
&#x20;      \- DeFi 高频用户最在意最终签署的交易是否就是刚才模拟的交易；\
&#x20;      \- 钱包安全开发者最在意交易版本绑定、哈希可复现和证据来源。\
\
&#x20;   4\. 根据预演结果，初步决定「缩小范围」：\
&#x20;      \- 从完整的「责任归属系统」缩小为「交易版本绑定 + 可核验决策收据」；\
&#x20;      \- 第一版只验证一笔 Swap；\
&#x20;      \- 保留 Moss action → simulate、风险解释、用户确认和收据完整性校验；\
&#x20;      \- 暂不做自动判责、多协议、多角色审批、完整聊天记录上链和自动执行。\
\
&#x20;   今日最重要的收获：\
\
&#x20;   留下更多日志，并不等于用户真正理解了风险。产品不应该把用户点击确认包装成免责证明，而应该忠实记录：Agent 提议了什么、模拟发现了什么、用户确认了什么、链上最终执行了什么。\
\
&#x20;   从第一性原理看，这个产品的核心价值不是替任何一方判责，而是保证「被解释和模拟的交易」与「用户最终签署的交易」可以被独立核验。\
\
&#x20;   真实性说明：\
\
&#x20;   今天完成的是 AI Persona 需求预演，不将模拟内容冒充 3 位真实用户反馈。正式进入集中开发前，仍需访谈 3 位真实用户或相关人员，确认问题是否真实存在、他们当前如何解决，以及是否愿意保存和查看决策收据。\
\
&#x20;   下一步：\
\
&#x20;   1\. 找 3 位真实用户完成简短交流；\
&#x20;   2\. 根据真人反馈确认「继续 / 缩小 / 调整 / 合并 / 暂停」；\
&#x20;   3\. 如果需求成立，再进入 Build Sprint，验证一笔 Swap 的最小决策收据。\
\
&#x20;   今日 GitHub 证据：\
\
&#x20;   \- 218eedd：提交 Agent 钱包黑匣子 Idea Card\
&#x20;   \- 7aa018c：提交 Team Matching Card\
&#x20;   \- 86ab681：提交 AI Persona Reality Check，并纠正为 Week 4 Day 2
<!-- DAILY_CHECKIN_2026-07-28_END -->

<!-- DAILY_CHECKIN_2026-07-29_START -->
# 2026-07-29

【Web3 暑期实习计划｜Monad Builder Camp 打卡】

日期：2026 年 7 月 29 日
周次：Week 4｜Monad Playground Hackathon · Day 3
方向：Tech / Dev Builder
主题：Meet Your Team — Team Matching + Day 4 验证工具准备
学分任务：Team Matching Card +20

今日完成：

1. 完成 Team Matching Card。与 Week 3 队友 Riso（Product/Research）、eleven（UI）确认继续组队，推进「Agent 钱包黑匣子」方向：帮助个人用户在 Agent 执行 Swap 时签前看懂授权、事后能还原「提议—风险—确认—执行」事实。对齐四问（同一问题、各约 6h/周、可检查贡献、方向不成立时的缩小/合并策略），并约定微信群 + 每晚 21:00 同步 + 仓库文档的最小协作方式。
2. 补全 Builder Signal，写清我能贡献的 Moss 集成、证据数据结构、Demo Evidence，以及可选招募的钱包安全开发者角色。
3. 为 Day 4 准备两套执行工具：给 Riso 的访谈邀请话术与记录模板；给自己的 Moss Kuru Swap 技术验证 checklist（action→simulate、决策收据字段、真实 vs Mock 边界）。明确现有 Reality Check 仅为 AI Persona 预演，不算正式 3 人需求证据。

今日收获：

组队不等于写好名字，而是把时间、贡献和失败时的调整方式说清楚。Playground 周先匹配人、再验证需求，比立刻堆功能更接近能进入 Build Sprint 的状态。

下一步：

Riso 完成 3 人真人访谈；我跑通一笔 Moss Swap 的 simulate 与收据字段映射；三人根据证据选择继续/缩小/调整，再进入 Day 5 Start Card。

仓库证据：

* daily/2026-07-29.md
* submissions/week-04-hackathon/team-matching-card.md
* submissions/week-04-hackathon/builder-signal.md
* submissions/week-04-hackathon/interview-kit-riso.md
* submissions/week-04-hackathon/moss-swap-tech-checklist.md
<!-- DAILY_CHECKIN_2026-07-29_END -->

<!-- DAILY_CHECKIN_2026-07-30_START -->
# 2026-07-30

# 2026-07-30｜Week 4 Day 4（Test Before Build：3-Person Reality Check）

## 今日信息

* **日期**: 2026-07-30（周四）
* **周次**: Week 4｜Monad Playground Hackathon
* **Track**: Tech / Dev Builder
* **今日主题**: Test Before Build — 先问三个人，再决定做什么
* **学习投入时长**: 约 2–3 小时
* **学分任务**: 3-Person Reality Check +20（进行中，真人访谈待完成）

***

## 固定动作

* 查看学习平台 / `tasks/week-04-hackathon.md` Day 4 要求
* 完成学习平台打卡（提交本文件摘要）
* 参加/回看 19:00 嘉宾分享（如有，待补充）
* 参加/回看 20:00 Co-Learning Idea Reality Check（如有，待补充）
* 更新本地项目记录

***

## 今日完成

### 1. 仓库文档全域对齐｜Week 4 结构修正

本周的核心发现：官方 Week 4 实际是 **Playground 周**（5 个轻量任务、100 学分），而不是之前理解的「直接提交可运行产品」。今日将全仓库 7 个文件统一修正：

| 文件                                                    | 修正内容                                                                                   |
| :---------------------------------------------------- | :------------------------------------------------------------------------------------- |
| `README.md`                                           | Week 4 描述从「提交可运行产品」→「Builder Signal、Idea、组队、3 人验证、Start Card」                          |
| `docs/schedule.md`                                    | Week 4 整节重写：从「产品冲刺 + Demo Day」→「Day 1–5 轻量任务 + Co-Learning」                            |
| `docs/deliverables.md`                                | 拆分为 5a（Playground 本周强制）+ 5b（Build Sprint 终期提交）                                         |
| `docs/web3career-platform.md`                         | Week 4 从「最终交付物列表」→「5 任务学分表 + 每日 Co-Learning」                                           |
| `submissions/README.md`                               | Week 4 文件命名从 `plan.md / demo-link.md` → `builder-signal.md / problem-idea-card.md / …` |
| `submissions/week-04-hackathon/reality-check.md`      | 顶部挂 Day 4 执行工具入口（访谈包 + 技术清单）                                                           |
| `submissions/week-04-hackathon/team-matching-card.md` | Day 3 定稿；新增角色互补表、Co-Learning 话术、调整触发表、学习目标自检                                           |

**影响：** Playground 周不是 Build Sprint——本周产出是「协作信号 + 验证证据 + 第一步」，而不是可运行 Demo。这个认知修正避免了团队在 Day 4 就焦虑「还没写代码」。

### 2. Team Matching Card 深度定稿

在昨天草稿基础上完成最终版：

* 补充「角色互补为什么成立」表（每个角色 → 为什么需要 → 本周最低验收）
* 写好 Co-Learning 60 秒 Idea Holder / 30 秒 Builder 话术（可直接念）
* 补充落单同学可回应的钩子表（匹配 / 冲突 / 已覆盖 / 待合并）
* 新增异步 Stand-up 复制即用模板
* 新增调整触发条件表（4 种场景 → 对应动作）
* 新增 Day 3 学习目标自检表
* 新增相关提交索引

<br />

### 3. Day 4 执行工具就绪

* **Riso 访谈工具包**（`interview-kit-riso.md`）：邀请话术 4 套（中/短/英/开场）、正式 6 问 + 追问、单人记录模板、三人交叉结论模板、Plan B（约不到人）
* **Neo Moss 技术清单**（`moss-swap-tech-checklist.md`）：环境就绪表、Path B/A 双路径、决策收据最小数据模型（14 字段）、真实 vs Mock 表、验证实验记录模板、DoD、时间盒、安全硬约束
* Reality Check 文件顶部挂工具入口，明确 Persona 预演 ≠ 正式证据

<br />

### 4. Reality Check 当前状态

* **AI Persona 预演**已完成（3 类用户：新手 / 高频 / 开发者）
* **真人访谈**尚未完成：Riso 待发出邀请并完成 3 人记录
* **Moss simulate**尚未跑通：Neo 按 checklist 待执行
* **团队标签**（继续/缩小/调整/合并/暂停）待三人 stand-up 根据证据选择

***

<br />

## 今日证据

| 产出                    | 路径                                                          | 说明            |
| :-------------------- | :---------------------------------------------------------- | :------------ |
| 今日打卡                  | `daily/2026-07-30.md`                                       | 本文件           |
| 仓库文档对齐                | `README.md`, `docs/*.md` (×3), `submissions/README.md`      | Week 4 结构修正   |
| Team Matching Card 定稿 | `submissions/week-04-hackathon/team-matching-card.md`       | Day 3 学分 +20  |
| Reality Check 工具入口    | `submissions/week-04-hackathon/reality-check.md`            | 挂 Day 4 执行链接  |
| 访谈工具包（既有）             | `submissions/week-04-hackathon/interview-kit-riso.md`       | Day 4 Riso 执行 |
| Moss 技术清单（既有）         | `submissions/week-04-hackathon/moss-swap-tech-checklist.md` | Day 4 Neo 执行  |

> 今日无链上交易或新代码提交；交付重点是认知修正与文档对齐，确保团队在正确周结构下推进。

***

<br />

## 小队快照

| 角色                         | 人      | 今日状态                                                   |
| :------------------------- | :----- | :----------------------------------------------------- |
| Dev / Tech Lead            | Neo    | ✅ 仓库文档对齐完成；Team Matching Card 定稿；Moss checklist 就绪，待执行 |
| Product / Research / Pitch | Riso   | 📋 访谈工具包已就绪；待发出邀请并完成 3 人访谈                             |
| UI / Visual Design         | eleven | 📋 待根据访谈「最懵的一步」出主路径线框 + 证据时间线                          |

**项目暂定名：** Agent Wallet Flight Recorder｜Agent 钱包黑匣子小队

***

<br />

## AI 协作记录

* **AI 帮了什么**:
  * 识别 Week 4 官方结构（Playground）与旧文档（Builder Hackathon）的不一致
  * 批量修正 7 个文件中的 Week 4 描述、任务列表、学分、文件命名
  * 深度扩展 Team Matching Card（角色互补表、调整触发表、Co-Learning 话术、自检清单）
  * 在 Reality Check 顶部挂 Day 4 执行工具入口
* **我验证了什么**:
  * Week 4 官方定位是 Playground 周（5 任务 100 学分），不是 Build Sprint
  * 所有文档引用路径与实际文件存在一致
  * Persona 预演未被误标为真实用户证据
* **我做了什么判断**:
  * Playground 周优先「匹配 + 验证」，而不是焦虑没写代码
  * 结构对齐比堆功能更重要——错误认知会让团队在错误方向上用力

***

<br />

## 遇到的问题

* **问题 1**: 仓库多处文档将 Week 4 描述为「Builder Hackathon / 提交可运行产品」，与官方 Playground 定位不一致
  * **原因**: 前期理解偏差；官方实际将 Hackathon 拆为 Playground（本周）+ Build Sprint（下周）
  * **处理**: 今日全域对齐 7 个文件，确保团队和潜在读者正确理解本周任务
* **问题 2**: 真人访谈 + Moss 技术验证均未完成
  * **原因**: 今日时间优先用于认知修正和文档对齐
  * **下一步**: Riso 今晚/明早发出邀请；Neo 按 checklist 跑 Path B

***

<br />

## 今日复盘

* **收获**: 花时间修正「Week 4 是什么」的认知，比急着写代码更关键。Playground 周的本质是「发出信号 → 找到问题 → 验证需求 → 确定第一步」，这个框架一旦正确，团队焦虑自然下降。Team Matching Card 从信息对齐推进到可执行工具（话术、模板、调整表、自检）。
* **最卡**: Day 4 核心学分（3 人 Reality Check）仍依赖真人访谈和技术验证——这两个动作都在队友侧，需要今晚 stand-up 确认进度并决定是否需要缩小范围或延期。
* **是否更接近作品集**: 是。文档对齐意味着仓库中 Week 4 的证据结构是正确的，后续不管是提交学分还是向外部展示「我们如何验证需求」，都能直接从 `submissions/week-04-hackathon/` 取用，不需要二次解释。

***

<br />

## 明日计划（Day 5｜Choose Your First Move）

* 补齐 3 人真人访谈记录 → 团队选择标签（继续/缩小/调整/合并/暂停）
* Moss Path B simulate 跑通（或记录阻塞 + Mock 方案）
* 填写 Hackathon Start Card（项目快照、核心动作、真实 vs Mock、头号风险、是否进入 Sprint）
* 准备 90 秒 Project Check-in
* 更新明日打卡 `daily/2026-07-31.md`

***

<br />

## 相关链接

* 昨日打卡: `daily/2026-07-29.md`
* 周任务: `tasks/week-04-hackathon.md`
* Team Matching: `submissions/week-04-hackathon/team-matching-card.md`
* Problem & Idea: `submissions/week-04-hackathon/problem-idea-card.md`
* Reality Check: `submissions/week-04-hackathon/reality-check.md`
* 访谈工具: `submissions/week-04-hackathon/interview-kit-riso.md`
* 技术清单: `submissions/week-04-hackathon/moss-swap-tech-checklist.md`

***

<br />

## 可直接提交的打卡信息

【Web3 暑期实习计划｜Monad Builder Camp 打卡】

日期：2026 年 7 月 30 日 周次：Week 4｜Monad Playground Hackathon · Day 4 方向：Tech / Dev Builder 主题：Test Before Build — 仓库文档对齐 + 3 人 Reality Check 准备 学分任务：3-Person Reality Check +20（进行中，真人访谈待完成）

今日完成：

1. **全仓库 Week 4 结构修正。** 发现并修正了一个关键认知偏差：Week 4 实际是 Playground 周（5 个轻量任务、100 学分），而不是旧文档描述的「Builder Hackathon / 提交可运行产品」。将 README、schedule、deliverables、web3career-platform、submissions/README 等 7 个文件统一对齐到正确结构，确保团队不在错误方向上焦虑「还没写代码」。
2. **Team Matching Card 深度定稿。** 在昨天基础上补充角色互补表（每个角色 → 为什么需要 → 本周最低验收）、Co-Learning 话术（60 秒 Idea Holder + 30 秒 Builder）、落单同学钩子表、异步 stand-up 模板、调整触发条件表（4 场景 → 对应动作）、Day 3 学习目标自检。组队卡从信息对齐推进到可执行工具。
3. **Day 4 执行工具链就绪。** Reality Check 顶部挂上访谈工具包（Riso）和技术验证清单（Neo）入口，明确 Persona 预演 ≠ 正式证据。两套工具均包含话术、模板、Plan B、DoD 和时间盒。

今日收获：

Playground 周最大的坑不是技术难度，而是认知偏差——如果把「认识 Builder、验证需求、确定第一步」误解为「直接提交可运行 Demo」，团队会在 Day 4 就开始焦虑。花 2 小时修正 7 个文件的描述，比写 200 行代码更有价值：它让接下来两天的验证和 Start Card 都发生在正确的周结构里。

下一步：

Riso 完成 3 人真人访谈；Neo 跑通 Moss Path B simulate；三人 stand-up 选择标签（继续/缩小/调整/合并/暂停）；Day 5 根据证据填写 Hackathon Start Card。

仓库证据：

* `daily/2026-07-30.md`
* `submissions/week-04-hackathon/team-matching-card.md`（定稿）
* `submissions/week-04-hackathon/reality-check.md`（AI Persona 预演 + Day 4 工具入口）
* `submissions/week-04-hackathon/interview-kit-riso.md`
* `submissions/week-04-hackathon/moss-swap-tech-checklist.md`
* 全域文档对齐：`README.md`, `docs/schedule.md`, `docs/deliverables.md`, `docs/web3career-platform.md`, `submissions/README.md`
<!-- DAILY_CHECKIN_2026-07-30_END -->

<!-- DAILY_CHECKIN_2026-08-01_START -->
# 2026-08-01

\- 文件: daily/2026-08-01.md\
\- 提交: bc2836c docs(week-04): add Day 5 check-in — Hackathon Start Card submission\
\- 验证: 已推送 origin/master，工作区干净\
\
内容概要（沿用仓库既有 daily 格式）:\
\
今日信息\
&#x20; 2026-08-01（周六）｜Week 4 Day 5 Choose Your First Move\
&#x20; 学分任务: Hackathon Start Card +30\
\
今日完成\
&#x20; 1\. Start Card 定稿提交（commit 7fbeb8f）——项目定为 Silicon Labor Arbitration\
&#x20; 2\. 方向收敛说明——Start Card 对齐真实项目，并提醒 Day 2–4 旧卡片（Agent Wallet Flight Recorder）口径待统一\
&#x20; 3\. 以项目仓库为事实源核验——TaskEscrow 已部署 Testnet、Moss Protocol/MossBridge 已合并、规则引擎 + AI 层已跑通、UI 待建
<!-- DAILY_CHECKIN_2026-08-01_END -->

<!-- DAILY_CHECKIN_2026-08-02_START -->
# 2026-08-02

# 2026-08-02｜Monad Builder Camp 打卡（Silicon Labor Arbitration 开发摘要）

## 今日信息

* **日期**：2026-08-02（周日）
* **项目**：Silicon Labor Arbitration｜硅基劳动仲裁院
* **方向**：Tech / Dev Builder
* **我的角色**：Neo｜合约、Moss、Monad 链上集成

***

## 今日打卡

围绕黑客松项目 **Silicon Labor Arbitration**，我完成并核对了当前开发链路的阶段性成果。项目要解决的是：当用户委托 AI Agent 执行任务但交付发生争议时，责任不应消失在多层委托链中；系统应提供一条可核验的证据时间线，并把无法客观判断的问题交给人类终审。

当前 Demo 收敛为一个核心动作：用户支付 MON 创建「画橙色猫」任务，Agent 交付错误的「土豆」后，用户发起争议。规则引擎自动处理时间、格式、透明背景等可客观验证的条件；“交付是否真的是猫”保留为不可自动裁决项，冻结资金并转人工终审。

### 开发内容摘要

1. **Monad Testnet 合约**：完成并部署 `TaskEscrow`，覆盖 `createTask → assignAgent → submitDelivery → openDispute → settle → releaseFrozen` 生命周期。部署在 Monad Testnet（chain ID `10143`），合约地址为 `0x67040374b8A9756586De0885f01d1291cE8FFCcF`；部署 manifest 已记录交易、区块、ABI 和字节码核验信息。
2. **Moss 链上准备与签前证据**：将 Moss 接入任务创建入口。`createTask` 通过 Moss Capability 构造未签名交易并进行 Testnet 模拟；签前解释、Capability 参数、模拟结果、链 ID、合约地址、Moss 版本与内容 hash 共同构成 E3 证据。后续 `submitDelivery`、`openDispute`、`settle` 走直接交易路径，不错误标注为 Moss verified。
3. **规则与 AI 边界**：确定性规则引擎根据任务预先写入的验收条件计算分账与冻结金额；AI 提供检方、辩方、审计三路、带证据引用的意见，但不能决定资金金额。主观条件 C4 保持 `undecidable`，体现“AI 只解释，人保留终审”的项目原则。
4. **下一步与风险**：合约、Moss、规则与 AI 链路已有基础，当前最大风险是前端尚未形成可点的完整闭环。Build Sprint 的最小下一步是先用 typed mock adapter 跑通下单、交付、争议、结算/冻结、人工终审五屏流程，再接入一次真实 `createTask` 与 `settle` 展示链上证据。

***

<br />

## 证据与进度

* 合约部署证据：项目仓库 `deployments/monad-testnet.json`
* MossBridge、部署证据与架构文档：`4d1baeb`
* 交易构造层与 wagmi hooks：`e71d165`
* TaskEscrow 部署：`6e7c6cf`
* 当前开发分支：`fix/bump-moss-lockfile`

<br />

## 今日收获

这次开发把“Agent 出错后谁负责”从抽象叙事收敛为可演示的链上流程：Moss 负责事前解释与模拟，E3 保留签前证据，合约和规则层处理可验证事实，人类保留对主观问题的最终裁决权。

## 下一步

* 用 typed mock adapter 跑通五屏主路径
* 接入真实 `createTask` 与 `settle`，展示 E3 和链上事件
* 与 Riso、Eleven 对齐 Demo 叙事、线框与可理解性测试
<!-- DAILY_CHECKIN_2026-08-02_END -->

<!-- DAILY_CHECKIN_2026-08-03_START -->
# 2026-08-03

# 2026-08-03｜Week 5 Day 29（Build Sprint：前端方向收敛）

## 今日信息

* **日期**：2026-08-03（周一）
* **周次**：Week 5｜Resume & Portfolio Workshop / Hackathon Build Sprint
* **Track**：Tech / Dev Builder
* **今日主题**：收敛前端实现方向，先搭 2D 主流程，再按时间加入局部 3D 效果
* **学习投入时长**：待补充
* **学分任务**：无新增学分提交；继续推进 Silicon Labor Arbitration MVP

***

## 固定动作

* 查看本周任务与项目当前状态
* 完成学习平台打卡（提交本文件摘要）
* 参加 / 回看嘉宾分享（待补充）
* 参加 / 回看 Co-Learning / 助教答疑（待补充）
* 更新本地项目记录

***

## 今日完成

### 1. 收敛前端技术方向

小组结合剩余时间和前后端集成成本，决定不做全站 3D：

* 先用 AI 跑出可交互的 2D 页面框架，优先打通业务主流程。
* 3D 不作为 MVP 前置条件；有余量时，只在 Hero 或重点场景加入局部 3D 效果。
* 素材与视觉质感在 2D 地基完成后逐段替换，避免因建模、文生图、贴图和 Spline 调试阻塞开发。

<br />

### 2. 明确会前准备方式

* 等 Riso 上线后预约腾讯会议，由成员提前在群内列出待讨论问题，减少会上临时发散。
* 当前待拍板问题包括：页面清单、核心功能、事件交互、素材需求、局部 3D 的使用位置，以及前端与现有后端 / 链上逻辑的对接边界。
* Eleven 计划先制作几张效果图，让小组基于可视方案选择方向。

<br />

### 3. 明确前端最小下一步

* Neo 提出今日开始编写前端代码。
* 最小目标仍是先搭可点击的 2D 骨架，再接入下单、交付、争议、结算 / 冻结、人工终审主流程。
* 截至本次打卡，项目仓库 2026-08-03 尚无新的前端代码提交，因此该项保留为待完成，不记作今日代码成果。

***

<br />

## 今日证据

| 产出     | 路径 / 链接                                                          | 说明                  |
| :----- | :--------------------------------------------------------------- | :------------------ |
| 今日打卡   | `daily/2026-08-03.md`                                            | 本文件，记录小组讨论与范围决策     |
| 设计流程参考 | `https://chatgpt.com/share/6a7029ff-cb44-83ee-a9bb-23a4de2a117b` | 小组分享的设计过程参考         |
| 项目仓库   | `https://github.com/LierMi/Silicon-Labor-Arbitration`            | 8 月 3 日暂未发现新的前端代码提交 |

***

<br />

## 小队快照

| 角色                         | 人      | 今日状态                        |
| :------------------------- | :----- | :-------------------------- |
| Dev / Tech Lead            | Neo    | ✅ 参与前端范围收敛；📋 待提交 2D 前端骨架代码 |
| Product / Research / Pitch | Riso   | 📅 上线后预约会议并参与范围拍板           |
| UI / Visual Design         | Eleven | 🎨 计划制作多张效果图，辅助确定视觉方向与素材方案  |

**项目名称：** Silicon Labor Arbitration｜硅基劳动仲裁院

***

<br />

## AI 协作记录

* **AI 帮了什么**：根据项目整体规划与后端 / 链上代码，可用于整理页面、功能和事件交互；本次协助把群聊讨论整理成可核验的范围决策与任务清单。
* **我验证了什么**：核对活动仓库和项目仓库的分支、状态与 8 月 3 日提交记录；确认尚无今日前端代码证据。
* **我做了什么判断**：比赛阶段先做 2D MVP，局部 3D 仅作为增强项；不把“计划今天写代码”写成“已完成代码”。

***

<br />

## 遇到的问题

* **问题 1：前端缺少明确起点**
  * **原因**：页面、功能、事件交互和素材需求尚未形成共同清单，3D 实现又引入建模与工具选择成本。
  * **处理**：先锁定 2D 主流程；会前整理问题，结合效果图在会议中拍板。
* **问题 2：开发时间有限**
  * **原因**：前端、后端和业务集成都需要迭代与联调，完整 3D 方案的投入不可控。
  * **处理**：把全站 3D 从 MVP 中移除，只在主流程稳定后评估局部 3D。

***

<br />

## 今日复盘

* **收获**：今天最重要的不是选定某个 3D 工具，而是先消除范围风险：2D 主流程负责交付，局部 3D 负责加分，两者不再互相阻塞。
* **最卡**：页面和事件交互尚未形成正式清单，素材场景也需要进一步具体化。
* **是否更接近作品集**：是。团队已从“做 2D 还是 3D”的发散讨论，收敛到“先完成可演示闭环，再增加视觉增强”的可执行路径。

***

<br />

## 明日计划

* 召开腾讯会议，拍板页面清单、事件交互、素材需求和局部 3D 使用位置
* Neo：提交可运行的 2D 前端骨架，并确认与现有业务 / 链上接口的连接点
* Eleven：提供效果图，明确每张图对应的页面、状态与所需素材
* Riso：补充 Demo 叙事、核心用户动作和验收标准
* 建立最小联调顺序：Mock 主流程 → 真实 `createTask` → 真实 `settle` / 冻结事件呈现

***

<br />

## 相关链接

* 本周任务：`tasks/week-05-portfolio.md`
* 昨日打卡：`daily/2026-08-02.md`
* Hackathon Start Card：`submissions/week-04-hackathon/hackathon-start-card.md`
* 项目仓库：`https://github.com/LierMi/Silicon-Labor-Arbitration`
* 设计过程参考：`https://chatgpt.com/share/6a7029ff-cb44-83ee-a9bb-23a4de2a117b`

***

<br />

## 可直接提交的打卡信息

【Web3 暑期实习计划｜Monad Builder Camp 打卡】

日期：2026 年 8 月 3 日 周次：Week 5｜Hackathon Build Sprint 方向：Tech / Dev Builder 主题：前端方向收敛——2D 主流程优先，局部 3D 作为增强

今日完成：

1. 小组结合比赛剩余时间和前后端集成成本，决定不做全站 3D。MVP 先用 AI 搭出可交互的 2D 页面框架，优先跑通业务主流程；3D 仅在时间允许时用于 Hero 或重点场景，避免建模、文生图、贴图和 Spline 调试阻塞交付。
2. 明确会前准备方式：等 Riso 上线后预约腾讯会议，成员提前列出页面、功能、事件交互、素材和集成问题；Eleven 先制作几张效果图，让会议基于可视方案快速拍板。
3. 明确前端最小下一步：Neo 计划今日完成 2D 骨架，随后接入下单、交付、争议、结算 / 冻结、人工终审主流程。截至打卡时，项目仓库今日尚无新的前端代码提交，因此本次只记录范围决策，不把计划写成已完成成果。

今日收获：

黑客松阶段真正稀缺的是可控的迭代时间。先用 2D 打通完整闭环，再用局部 3D 增强重点场景，比一开始追求全站 3D 更能兼顾交付确定性与视觉上限。

下一步：

召开腾讯会议并拍板页面与交互清单；提交可运行的 2D 前端骨架；按 Mock 主流程 → 真实 `createTask` → 真实 `settle` / 冻结事件的顺序联调。

仓库证据：

* `daily/2026-08-03.md`
* `tasks/week-05-portfolio.md`
* 项目仓库：`https://github.com/LierMi/Silicon-Labor-Arbitration`
<!-- DAILY_CHECKIN_2026-08-03_END -->

<!-- DAILY_CHECKIN_2026-08-04_START -->
# 2026-08-04

# 2026-08-04｜Week 5 Day 30（Build Sprint：前端设计方案落地）

## 今日信息

* **日期**：2026-08-04（周二）
* **周次**：Week 5｜Resume & Portfolio Workshop / Hackathon Build Sprint
* **Track**：Tech / Dev Builder
* **今日主题**：把前端视觉分歧收敛为可执行的 UI / 交互落地计划，并完成黑客松组队确认
* **学习投入时长**：待补充
* **学分任务**：无新增学分提交；继续推进 Silicon Labor Arbitration MVP

***

## 固定动作

* 查看本周任务与项目当前状态
* 完成学习平台打卡（提交本文件摘要）
* 参加 / 回看嘉宾分享（待补充）
* 参加小组 Co-Learning / 项目讨论
* 更新本地项目记录

***

## 今日完成

### 1. 将视觉探索收敛为可执行方案

团队围绕“爆炸档案 / 爆炸画廊”入口进行了多轮出图尝试，并识别出当前效果受固定提示词、出图工具与设计拆解方式影响。讨论后不再继续追求一张图直接解决全部页面，而是形成分阶段前端方案：

* 视觉方向采用“机构绿 + 牛皮纸档案 + 红色印章”，避免常见 Web3 蓝紫霓虹和 AI 法官形象。
* 先用现有 Next.js + React + TypeScript + CSS / DOM 完成 80 分可演示版本，不让 3D 和素材阻塞主流程。
* 页面按 `/demo`、`/demo/case`、`/demo/verdict`、`/demo/arguments` 拆分，对应档案入口、责任链、规则结算和 AI 三路意见。
* 核心高光锁定 C4 空章悬停：C1-C3 正常落章，C4 停在半空并转人工复核，不伪装成已经裁决。
* 第一阶段只读取 `@sla/domain` 的土豆案 mock 数据，并明确标注 Mock；不请求 API、不改合约、不改变 C4 `undecidable`。

<br />

### 2. 明确前端落地阶段与验收边界

形成 Phase 0-5 的执行草案：设计文档 → 页面骨架 → 静态高保真 → 核心交互动效 → 素材质感 → 浏览器验证。关键验收包括：

* 30 秒内让评委看懂“AI 不判人，而是把责任链摆出来”。
* C4 必须保持 `undecidable`，结算展示保持 0.15 / 0 / 0.05 MON。
* AI 意见引用证据编号，证据角标可点击联动。
* TypeScript 与 Next build 通过后，才把前端实现记为完成。

截至本次打卡，以上内容属于团队讨论形成的设计与实施草案；项目仓库 8 月 4 日尚未发现新的代码提交，因此不记作已完成的前端实现。

### 3. 改进团队协作与素材探索方式

* 对视觉方向不再只依赖写死提示词，而是先让 AI 生成多个方向，再由团队快速筛选、纠偏。
* 收集 Muuuuu、SiteInspire、Godly、Awwwards 作为版式参考，GSAP、Motion 作为动效参考；仅在 CSS 动效不足时再考虑新增动画依赖。
* 明确成员卡住时可直接拆出具体任务交给 Neo，先完成最小闭环，再逐步完善。
* 拒绝远程控制个人电脑，改用文档、原型图、代码和可复现步骤协作。

<br />

### 4. 完成黑客松报名与组队确认

排查邀请失败后，确认原因是成员尚未完成黑客松报名。成员通过活动页面完成报名并登录确认，随后组队邀请流程完成。

***

<br />

## 今日证据

| 产出     | 路径 / 链接                                                                                                   | 说明                            |
| :----- | :-------------------------------------------------------------------------------------------------------- | :---------------------------- |
| 今日打卡   | `daily/2026-08-04.md`                                                                                     | 本文件，记录前端方案收敛与组队进展             |
| 项目仓库   | `https://github.com/LierMi/Silicon-Labor-Arbitration`                                                     | 8 月 4 日未发现新的代码提交，当前仅记录设计与协作进展 |
| 黑客松报名页 | `https://mojo.devnads.com/events/14`                                                                      | 用于成员报名、登录确认与组队邀请              |
| 视觉参考   | `https://muuuuu.org/`、`https://www.siteinspire.com/`、`https://godly.website/`、`https://www.awwwards.com/` | 页面风格与排版参考                     |
| 动效参考   | `https://gsap.com/`、`https://motion.dev/`                                                                 | 核心交互动效参考，暂不代表已引入依赖            |

***

<br />

## 小队快照

| 角色                         | 人      | 今日状态                                          |
| :------------------------- | :----- | :-------------------------------------------- |
| Dev / Tech Lead            | Neo    | ✅ 协助收敛 UI / 交互实施计划；✅ 提出卡点任务可直接转交；📋 待开始前端代码落地 |
| Product / Research / Pitch | Riso   | ✅ 说明立案需求与后端边界已基本确认；📋 待补充 Demo 开场与讲述逻辑        |
| UI / Visual Design         | Eleven | ✅ 完成多轮效果图探索并提供原型方向；📋 待把原型映射到具体页面与交互          |

**项目名称：** Silicon Labor Arbitration｜硅基劳动仲裁院

***

<br />

## AI 协作记录

* **AI 帮了什么**：辅助生成视觉探索结果，并把小组讨论整理成页面地图、组件、动效、技术边界和验收标准明确的分阶段方案。
* **我验证了什么**：核对活动仓库的日期、分支和状态；查询项目仓库 8 月 4 日提交记录，确认尚无今日代码提交证据。
* **我做了什么判断**：不再把“出一张完整效果图”当作前端设计终点；先用 CSS / DOM 实现核心叙事，C4 空章和业务真实性优先于全站 3D 与素材堆叠。

***

<br />

## 遇到的问题

* **问题 1：视觉结果重复且效果一般**
  * **原因**：提示词约束过死，出图工具与中转服务可能影响结果；同时试图用单张场景图承载页面、交互和素材关系，拆解粒度过大。
  * **处理**：改为多方向生成 → 人工筛选 → 快速纠偏，并把视觉任务拆到页面、状态和交互层。
* **问题 2：场景化设计难以直接映射到前端**
  * **原因**：互动元素、素材和业务状态之间缺少明确对应关系。
  * **处理**：先固定四个 Demo 页面和最小组件，再逐页补素材与动效；CSS / DOM 版本先行。
* **问题 3：组队邀请无响应**
  * **原因**：成员尚未完成活动报名或登录确认。
  * **处理**：通过活动页完成报名并确认登录，组队流程已完成。

***

<br />

## 今日复盘

* **收获**：今天真正推进的不是“找到最强出图模型”，而是把模糊的视觉期待转成前端可以逐阶段验收的工作。第一性原理看，评委需要先理解责任链为何停在 C4，而不是先看到复杂 3D；因此空章叙事、证据引用和资金边界比装饰素材更重要。
* **最卡**：原型图、场景素材和实际页面状态还没有一一对应，设计草案也尚未转化为仓库代码与浏览器证据。
* **是否更接近作品集**：是。团队已从“工具效果一般、需求不好拆”推进到页面、组件、动效和验收标准明确的实施草案，但仍需用可运行前端证明方向成立。

***

<br />

## 明日计划

* 将前端 UI / 交互计划落入项目仓库文档，并由团队确认范围
* 搭建 `/demo`、`/demo/case`、`/demo/verdict`、`/demo/arguments` 最小页面骨架
* 先完成 C4 空章 CSS 动效，再决定是否需要 GSAP
* 将 Eleven 的原型图逐一映射到页面、状态、素材和交互
* 用浏览器截图验证桌面主流程与移动端不崩
* 补充 Demo 开场与 30 秒讲述逻辑

***

<br />

## 相关链接

* 本周任务：`tasks/week-05-portfolio.md`
* 昨日打卡：`daily/2026-08-03.md`
* Hackathon Start Card：`submissions/week-04-hackathon/hackathon-start-card.md`
* 项目仓库：`https://github.com/LierMi/Silicon-Labor-Arbitration`
* 黑客松报名页：`https://mojo.devnads.com/events/14`

***

<br />

## 可直接提交的打卡信息

【Web3 暑期实习计划｜Monad Builder Camp 打卡】

日期：2026 年 8 月 4 日 周次：Week 5｜Hackathon Build Sprint 方向：Tech / Dev Builder 主题：把前端视觉分歧收敛为可执行的 UI / 交互落地计划

今日完成：

1. 围绕“爆炸档案 / 爆炸画廊”入口进行了多轮视觉探索，并识别出固定提示词、出图工具和设计拆解粒度带来的限制。团队不再试图用一张效果图解决全部问题，而是确定“机构绿 + 牛皮纸档案 + 红色印章”的视觉方向，先用现有 Next.js + React + TypeScript + CSS / DOM 完成可演示版本。
2. 明确四段 Demo 流程：档案入口、责任链详情、C1-C4 规则与结算、AI 三路意见。核心高光锁定 C4 空章悬停：系统诚实地停下并进入人工复核，不伪装成已裁决。第一阶段只使用显著标注的 Mock 数据，不请求 API、不改合约、不改变 `undecidable`。
3. 形成 Phase 0-5 落地草案，从设计文档、页面骨架、静态高保真和核心动效，推进到素材质感与浏览器验证。团队采用“多方向生成 → 人工筛选 → 快速纠偏”的素材探索方式；成员卡住时可直接拆出具体任务协作，先完成再完美。
4. 排查并解决黑客松组队邀请问题：成员完成活动报名与登录确认后，组队流程已完成。

今日收获：

第一性原理看，评委首先需要理解“为什么责任链停在 C4”，而不是先看到复杂 3D。把模糊视觉期待拆成页面、组件、状态和验收标准，比继续更换出图模型更接近真实交付。今天完成的是方案与协作收敛；项目仓库尚无 8 月 4 日代码提交，因此不把计划写成已实现成果。

下一步：

将设计方案落入项目仓库，搭建四个 Demo 页面骨架，优先完成 C4 空章 CSS 动效，并用浏览器截图、TypeScript 检查和 Next build 验证真实效果。

仓库证据：

* `daily/2026-08-04.md`
* `tasks/week-05-portfolio.md`
* 项目仓库：`https://github.com/LierMi/Silicon-Labor-Arbitration`
<!-- DAILY_CHECKIN_2026-08-04_END -->

<!-- DAILY_CHECKIN_2026-08-05_START -->
# 2026-08-05

【Web3 暑期实习计划｜Monad Builder Camp 打卡】

日期：2026 年 8 月 5 日
周次：Week 5｜Hackathon Build Sprint
方向：Tech / Dev Builder
主题：canonical requirements hash 与真实 E3 链路技术审计

今日完成：

1. 明确冲刺节奏：今晚先上传代码并同步当前进度与剩余优化项，明天和后天由三人共同收尾。PR 流程坚持最短正确路径，不新增指向 `main` 的并行修复线。
2. 完成 PR #17 技术审计，复现并指出 `walletConsistency: undefined`、Date / Map / Set 被哈希为 `{}`、E1 占位哈希和缺少回归测试四个阻塞项。原作者在原分支修复后，#17 已于今日合并。
3. 收敛 E3 类型统一方案：删除 moss-bridge 重复手写类型，复用 domain 的 `MossPreSignEvidence`，让 `buildE3()` 直接构造最终存档形状后再计算 hash。原 #18 已关闭，接续工作为 #19。
4. 完成 PR #19 审计并 Request Changes：当前仍存在 Moss 实参与归档参数不一致、模拟与案件时间线不一致、RPC fingerprint 未绑定实际 Runtime 且可能泄密、干净环境完整门禁不可复现，以及 malformed E3 会让 validator 直接抛错等阻塞项。#19 当前仍未合并。
5. 团队更新 C4 不可判定规则：交付物整体不可用时，不再支付 0.15、冻结 0.05，而是 0.2 MON 全额冻结。现有前端按数据字段展示、合约 `settle` 已支持全额 frozen，本次决策不要求单独修改展示逻辑或合约。

今日收获：

证据系统不能只保证“同一份 JSON 能算出同一个 hash”，还要保证被哈希的数据来自真实 Moss 输入、真实 Runtime 和同一案件时间线，并且不泄露 RPC 凭据。可复算只是基础，来源绑定和失败边界才决定它能否作为可信证据。

下一步：

等待 #19 修复后重新审计；在干净 clone 重跑完整测试、typecheck 和 ABI gate；同时由三人同步前端代码与剩余优化清单，集中完成 Demo 闭环。

仓库证据：
- `daily/2026-08-05.md`
- PR #17：`https://github.com/LierMi/Silicon-Labor-Arbitration/pull/17`
- PR #19：`https://github.com/LierMi/Silicon-Labor-Arbitration/pull/19`
<!-- DAILY_CHECKIN_2026-08-05_END -->
<!-- Content_END -->
