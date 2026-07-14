---
timezone: UTC+8
---

# kkiii2025

**GitHub ID:** kkiii2025

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
看看能不能打上
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->


### AI Agent 能力飞跃的 4 大底层原因

1.  **大模型本体变强**：基础推理、逻辑理解能力大幅提升，复杂问题处理更成熟
    
2.  **超长上下文窗口**：可读取海量代码、项目文档、历史交互记录，推理更精准
    
3.  **工具生态完善**：支持 CLI 命令行、代码执行、Bash、外部 API 调用，工具箱丰富
    
4.  **完整记忆闭环**：持久化存储交互记忆，自动总结用户习惯形成专属 Skill，实现个性化迭代
    

### 两大适配 AI 的开发范式

（1）TDD 测试驱动开发（提升代码健壮性首选）

流程：**先写测试用例定义预期结果 → 测试失败 → 最小代码实现 → 测试通过 → 按需重构** 优势：给 AI 搭建自动纠错回路，防止改代码破坏原有逻辑，AI 可自动执行`cargo test`等自检。

（2）SDD 规范驱动开发（大型项目标准化）

流程：明确需求目的 → 制定开发规范 → 设计技术方案 → 拆解细分任务 → 落地实现 → 文档留痕溯源 配套工具：

### 开发者和 AI 协作必备 4 项能力

1.  **会描述**：写高质量 Prompt，清晰说明用户、场景、约束、验收标准
    
2.  **会拆解**：拆分复杂任务为可执行、可验证小步骤，理清依赖与风险
    
3.  **会验证**：自动化测试、手动校验、日志埋点，人工把控最终交付质量
    
4.  **会设计**：自主掌握架构边界、数据流、长期质量体系，避免完全依赖 AI
    

### Web3开发赛道

├─ 强耦合组合：DApp ↔ 智能合约（项目标配） ├─ 独立赛道1：钱包开发（项目极少自研） ├─ 独立赛道2：底层节点开发（公链团队专属） └─ 独立赛道3：安全审计（第三方上线必备） 通用底层基础：密码学 + 区块链底层原理
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->



理解 Monad ，首先要明白 Monad 的高频交互场景并非针对所有需要高频交互的产品。

  
适合 Monad 的产品通常需要满足：**高频交互 × 资产状态变化 × 需要开放可组合/可信结算**。  
不适合的场景则是：**高频but低价值、无资产、无跨平台组合需求、只需要平台自己记账**。例如普通社交 feed、浏览记录、经验值、非交易型游戏行为，大多数应该放数据库，链上只记录最终结算或资产变更。

如果只是点赞、评论、普通积分、内部排行榜，普通数据库更好；如果每次交互都会改变可转让资产或公共状态，Monad 的价值才明显 —— Monad 的机会不是把数据库替换掉，而是让过去因为 Ethereum 太慢太贵而无法上链的应用变得可用，尤其是预测市场、onchain CLOB/perps、agent 微支付、高组合性的 DeFi 消费应用、可验证资金池游戏。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->




打卡
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->





**注意 Vibecoding 智能合约风险：**AI 智能体编写 UI 代码的能力，要远比编写智能合约代码的能力强得多。Solidity 的训练数据更少，逻辑更专业，错误也更难被发现。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->






## 7.09 学习笔记

Replit 网站无需梯子。

**Monad 的由来：**

2009 年，Bitcoin 证明“价值可以在没有中间人的情况下转移”，但不能在它上面构建完整应用。

2015 年，Ethereum 证明“去中心化计算确实可行”，它的目标不是让去中心化应用用起来像普通软件一样快。

2025年，Monad 继承了 Ethereum 建立起来的一切，然后继续问下一个问题：**如果去中心化应用快到像普通软件一样，会发生什么？**

**Monad 设计目标**：10,000 TPS、400ms 出块、800ms 确定性最终性（deterministic finality）

**主网和测试网：**Mainnet 使用具有真实价值的代币；testnet 上的代币没有价值，是用来安全实验的。

**Chain ID** 是一个用来标识某条区块链的唯一编号。

**RPC（Remote Procedure Call，远程过程调用）**是钱包或应用用来与区块链对话的一个 URL，相当于“给区块链电话”。RPC 也分 Public RPC 和 Paid RPC，面向真实用户上线时使用Paid RPC，一般测试就用Public RPC。

**Faucet（水龙头）**是一个免费发放 testnet 代币的网站。官方 Monad faucet： [faucet.monad.xyz](http://faucet.monad.xyz)

**Block explorer** 用于查询链上的任何东西：交易、钱包、合约，以及它们的历史。

大多数钱包也支持通过 [chainlist.org](http://chainlist.org) 一键添加。

**Supabase：**一项免费服务，开箱即用地提供**数据库**和**文件存储**。

**数据库**存放结构化数据（文本、数字、日期、各种关系），**文件存储**存放复杂的文件（路径 URL 放在数据库，文件字节放Sotrage）。

**给应用加上 Storage：**

在侧栏的 Storage **创建一个 Bucket**，暂时保持 Public bucket 关闭

**设置一条 Storage 策略**，New policy → 选 template → 选 Allow access to JPG images in a public folder to anonymous users → Allowed operation模块中勾选 SELECT（读取 / 下载）和 INSERT（上传。（后面在 authentication 那节课再把它收紧）

**需要在表里有一个地方存放每个上传文件的路径**。告诉智能体我要加一列来引用文件，它会给我用于输入 SQL Editor 中去运行的内容

接下来只需告诉智能体：**在应用里上传文件、显示这些文件、限制用户可以上传什么**相应的需求
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->







打！
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->








打卡
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->









Kiiyasun完成2026.07.06打卡！
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
