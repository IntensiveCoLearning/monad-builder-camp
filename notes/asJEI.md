---
timezone: UTC+8
---

# Hokkai

**GitHub ID:** asJEI

**Telegram:** 

## Self-introduction

一个正在学习的大三学生而已。
具体学习记录与总结可查看：https://www.hokkai2005.online
具体学习记录可查看：https://www.hokkai2005.online
具体学习记录可查看：https://www.hokkai2005.online
具体学习记录可查看：https://www.hokkai2005.online/blog/web3-notes-02/

## Notes

<!-- Content_START -->
# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
上班第一天
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->

震撼美味
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->


今天有事，非常之忙。总结笔记和制作项目规划书

笔记地址：[https://www.hokkai2005.online/blog/talk-with-ai-founder/](https://www.hokkai2005.online/blog/talk-with-ai-founder/)
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->



水一天
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->




0\. 背景介绍

在参加 Monad Builder Camp 之前，我对于 Web3 的理解主要停留在区块链、钱包、智能合约等概念层面。因此 Week 1 的目标不是单纯完成任务，而是尝试建立 Web2 开发经验与 Web3 系统之间的联系。

通过这一周的钱包实践、链上交易、智能合约部署，我逐渐从“应用开发者”的视角转向理解 Web3 中的“状态、身份和规则”。

1\. Week 1 完成的链上实践

1钱包创建与 Monad Testnet 配置：

首先创建了 Monad Builder Camp 专用钱包，并完成 Monad Testnet 网络配置。我使用了如下内容：

钱包：Rabby Wallet

网络：Monad Testnet

Chain ID：10143

RPC：[testnet-rpc.monad.xyz](http://testnet-rpc.monad.xyz)

通过配置测试网络，我完整理解了钱包地址是什么、私钥和签名的作用、钱包如何作为 Web3 身份入口。相比 Web2 的账号密码体系，钱包并不是账号，而是一种基于密码学证明身份的方式。

2 第一笔 Monad Testnet 链上交易：

测试币领取之后，我进行了第一次 Monad Testnet 转账。

交易信息：

From：课程测试钱包

To：个人测试地址

Amount：4 MON

Transaction Hash：

0xc67a286420da659ab9d7afb4676fb5fd741ef2ba111b62d4072feeb53b8353c9

然后通过 Monad Explorer 查看了交易详情，让我第一次真正理解到了链上交易不是简单调用 API，而是：

用户签名 → 网络验证 → 状态改变 → 永久记录

的多个连续步骤

3 Solidity 合约编写与部署

完成钱包和交易实践后，我进一步尝试使用 AI 辅助生成简单 Solidity 合约，并在 Remix 环境完成部署。

关于分轨问题

我的首选是 Research。因为我的技术背景主要集中在 AI 应用开发和全栈开发方向，过去参与过 AI Agent、LLM 应用以及开发者工具相关项目。在接触 Web3 之前，我更多关注的是如何利用技术解决实际问题。而且我对于分析文章或者说撰写教程指导这一类也比较有兴趣

备选/辅学Tech。原因也简单，我认为研究不能脱离实际构建。通过实际 Build 验证 Research 中提出的想法是一种很有成就感的事情
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->





已完成本日大学习
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->






晚上要赶火车先占位打卡一下
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->







———————————————————— 一、今日任务完成情况 ————————————————————

【任务一】用 AI 生成最小 Solidity 合约并完成人工检查

\[完成\] 编写 Prompt让 AI 生成合约初稿

\[完成\] 阅读 AI 输出的代码

\[完成\] 让 AI 解释合约结构

\[完成\] 完成至少 3 项人工检查

\[完成\] 记录对 AI 输出的修改或判断

人工检查结论：

-   能否编译：通过，Remix 编译成功（Solidity 0.8.20）
    
-   函数是否符合预期：通过，addTodo / toggleTodo / getTodos 与 Prompt 一致
    
-   权限问题：通过，按 msg.sender 隔离，每人只能操作自己的列表
    
-   是否过度复杂：通过，无外部库、无继承；AI 多加了 Event，保留
    
-   命名与可读性：通过，结构清晰，注释完整
    

对 AI 输出的修改：未改代码，与 AI 初稿一致。

【任务二】部署到 Monad Testnet 并完成合约交互

\[完成\] Remix 编译合约

\[完成\] 课程专用钱包（Rabby）连接部署环境

\[完成\] 部署到 Monad Testnet

\[完成\] 保存合约地址和部署 tx hash

\[完成\] 调用 read 函数（getTodos）

\[完成\] 调用 write 函数（addTodo）

\[完成\] 整理 README

———————————————————— 二、关键产出 ————————————————————

合约地址： 0x57A55353E44DE57E2b1F5DD11dF12A2cdCa1CdC6

部署 tx hash： 0x7f6ebc580981badc5105beedc11cf49b6ecd31a18e7a86491555f61a4770222b

部署者地址： 0x240e0b16297C9F738260866B0934fb99CBC33e7E

部署区块：42931286 Gas 消耗：1144787 gas

交互记录：

1.  Read（第一次） 函数：getTodos 参数：无 结果：空数组 \[\]
    
2.  Write 函数：addTodo 参数："完成今日 web3 的学习" 结果：Explorer 出现 AddTodo 记录，Status 成功
    
3.  Read（第二次） 函数：getTodos 参数：无 结果：返回 1 条 todo 内容："完成今日 web3 的学习", false, 1723408389 （text = 待办内容，false = 未完成，1723408389 = 创建时间戳）
    

Explorer 链接：

合约页： [**https://testnet.monadexplorer.com/address/0x57A55353E44DE57E2b1F5DD11dF12A2cdCa1CdC6**](https://testnet.monadexplorer.com/address/0x57A55353E44DE57E2b1F5DD11dF12A2cdCa1CdC6)

部署交易： [**https://testnet.monadexplorer.com/tx/0x7f6ebc580981badc5105beedc11cf49b6ecd31a18e7a86491555f61a4770222b**](https://testnet.monadexplorer.com/tx/0x7f6ebc580981badc5105beedc11cf49b6ecd31a18e7a86491555f61a4770222b)

———————————————————— 三、今日完整链路 ————————————————————

Prompt → AI 初稿 → 人工审查 → Remix 编译 → Rabby 部署 → getTodos（空）→ addTodo（write）→ Explorer 验证 → getTodos（有数据）

———————————————————— 四、核心认知 ————————————————————

1.  AI 是草稿机，不是审计师。Prompt 要明确规定版本、字段、函数
    
2.  部署不等于转账。To 从钱包地址变成合约地址；改状态靠调函数，不是靠 value 字段。
    
3.  read 和 write 是两套逻辑。call 本地读、不花 Gas；transact 签名写、上链留 tx、付 Gas。
    
4.  权限模型：本合约用 mapping(address => Todo\[\]) + msg.sender 实现按地址隔离，无需 onlyOwner。
    
5.  忘了合约地址：从部署者钱包在 Explorer 找 OUT 方向、Method 为 0x60806040 的部署交易，To 行显示 \[Contract ... created\] 即为合约地址。
    

———————————————————— 五、合约 README（精简版） ————————————————————

合约名称：OnchainTodo 说明：链上待办清单练习合约，每个地址独立维护自己的列表。

主要函数：

-   addTodo(string text) — write，新增待办
    
-   toggleTodo(uint256 index) — write，切换完成状态
    
-   getTodos() — read，读取当前地址全部 todo
    

部署方式： Remix 新建 OnchainTodo.sol → 编译 0.8.20 → Deploy & Run Transactions → Browser Extension 连 Rabby → 选 Monad Testnet → Deploy

交互方式： Remix Deployed Contracts 里，蓝点函数点 call（如 getTodos），橙点函数点 transact（如 addTodo），最后在 Explorer 验证。

Gas 不足可去 [faucet.monad.xyz](http://faucet.monad.xyz) 领测试 MON。

———————————————————— 六、详细教程 ————————————————————

博客链接：[Web3 Day2 学习记录：从第一次编写合约到成功部署 | 北海](https://www.hokkai2005.online/blog/web3-notes-03/)

————————————————————
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->








今日完成 Monad Builder Camp 第一日学习，重点从理论走向实操，建立钱包安全习惯与基本链上认知。

**已完成任务：**

1.  创建课程专用钱包（Rabby），备份助记词，与主力钱包隔离；
    
2.  添加 Monad Testnet（Chain ID 10143），网络切换正常；
    
3.  使用 Monad Explorer 查询地址与交易，理解 Address、Balance、Transactions 等字段；
    
4.  通过官方 Faucet 领取测试 MON，完成第一笔链上转账（4 MON），并保存 tx hash 在浏览器复盘。
    

**核心收获：**

-   Web2 是平台服务器改状态，用户信任平台；Web3 是用户签名 → 节点验证 → 状态写入链上，所有人可验证。
    
-   身份靠钱包私钥签名，操作是需付 Gas 的交易，而非普通 API 调用。
    
-   读懂交易字段：From 为发起方，To 为接收方，Value 为转账金额，Gas 为执行计算的手续费；失败交易也可能消耗 Gas。
    

Day 1 未写合约，但已走通「领水 → 转账 → 查 hash → 复盘」全流程，为后续学习打下基础。

详细学习记录可以查看：[Web3 Day1 学习记录：从自己的钱包到第一笔链上交易 | 北海](https://www.hokkai2005.online/blog/web3-notes-02/)
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
