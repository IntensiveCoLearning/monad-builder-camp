---
timezone: UTC+8
---

# Milli

**GitHub ID:** tz-hao

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
今天和团队进一步讨论了项目方向，围绕目标用户、实际需求和使用场景进行了梳理，并对现阶段的想法做了调整。

在讨论过程中，我们尝试缩小项目范围，明确最小可行版本需要完成的核心流程，同时初步划分了开发、资料整理和展示等任务。接下来将继续完善需求与分工，优先验证关键功能是否能够顺利实现。
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->

今天完成了组队方向和协作需求的梳理。我目前还在寻找队友，计划围绕 **AI × Web3 应用**完成项目实践。我主要负责技术开发和 Demo 实现，希望找到擅长运营、内容整理和项目展示的队友。

接下来我会在社群发布组队信息，进一步确认项目要解决的用户问题，并在组队后建立群聊、任务看板和固定同步时间。协作过程中会注意安全边界，不共享私钥、助记词及其他敏感信息。
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->


第三周 Day1 打卡记录

今天进入第三周的学习，我主要完成了自己的 Builder Profile 梳理。在寻找队友之前，我先明确了自己的方向：偏 Dev，重点关注 Web3 / Agent / 智能合约相关项目。

通过这次整理，我更清楚自己能为团队提供什么：我可以负责 Agent 后端逻辑、智能合约开发、前后端联调和 Demo 搭建，也能结合之前做过的 Web3 Agent、智能钱包、链上信誉系统等项目经验，帮助团队把想法落到一个可以演示的 MVP。

我目前比较想参与 AI x Web3、Agent Wallet、链上信誉、预测市场、隐私计算或开发者工具类项目。相比只做展示页面，我更希望项目能跑通真实链路，比如包含合约、后端接口、前端交互和链上记录。

接下来我希望寻找擅长前端设计、产品梳理、运营展示或合约安全的队友。第三周的目标是尽快找到合适的团队，明确项目方向和分工，开始推进一个完整 Demo。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->



今天主要围绕 Moss 的 Clober V2 Adapter PR #56 进行了第二轮 Review 修复和完善。

这次收到 Reviewer 的两个后续反馈：一是 Receipt 不能只用 `transferCount >= 2` 判断交易结算完成；二是 99.9% 输入利用率会让部分可执行的小额 MON → USDC 交易被拒绝。针对第一个问题，我重新梳理了 Clober Controller 的真实结算流程，补充了 BookId 白名单、交易方向、代币、参与地址、非零金额和 native 退款守恒校验，避免缺失输出或混入无关转账时仍被错误解析。

针对 99.9% 利用率问题，我没有简单降低阈值。通过阅读 Clober 的 Viewer 和 Controller 逻辑，发现当前返回数据无法可靠区分“单位取整产生的 dust”和“真实流动性不足”，贸然放宽会削弱安全保护。因此保留 fail-closed 策略，并在 README 中明确说明小额交易可能出现方向相关、非单调的拒绝边界。

今天新增了多组边界测试，包括错误 BookId、缺失输出、错误代币或参与者、零金额、多余转账、native 退款不守恒，以及 99.9% 临界值和 0.001 MON 的实际案例。最终完成全仓 `lint`、`build`、`typecheck`、`test`，并通过 Monad 主网双向报价与 Viewer/Controller 模拟验证。

我也更新了 PR 描述并用英文回复 Reviewer，邀请对方重新运行其 characterization tests。PR 当前已推送到最新提交，GitHub CI 处于等待维护者批准运行的状态。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->




打卡打卡
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->





今天主要围绕 **Moss Adapter 开源贡献** 和 **GitHub PR 提交流程** 完成学习与实践。

今天继续深入参与 Moss 开源项目，不只是停留在阅读文档或整理内容，而是真正尝试为项目新增一个 Adapter。通过阅读 Moss 的项目结构、已有 Adapter 实现方式和协议接入逻辑，我对 Moss 如何通过 Adapter 扩展 AI Agent 能力有了更清晰的理解。每新增一个 Adapter，就相当于让 AI Agent 多支持一种链上协议、工具或服务。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->







打卡打卡
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->








## **今日完成**

-   阅读 Moss 的 README、Getting Started、MCP Tools 与 Agent Skill Guide。
    
-   理解 Moss 通过 `discover → load → action → simulate` 统一链上协议能力。
    
-   完成 Moss 的项目简介、核心问题、核心能力和应用场景整理。
    

## **关键收获**

Moss 的价值不只是帮助 Agent 调用链上协议，更重要的是限制 Agent 在高风险操作中的自由度：

1.  协议 adapter 负责构建正确的未签名交易，Agent 不手写 calldata。
    
2.  Plan 会声明资产流出、流入和授权预期；模拟后必须对账。
    
3.  出现任何警告时必须停止，不能交给钱包签名。
    
4.  即使模拟零警告，Agent 仍需确认交易结果是否与用户原始意图一致。
    

## **我的理解**

对 AI Agent 来说，“能完成交易”不足以代表安全。更可靠的路径是：让框架处理确定性的交易构建与效果验证，让 Agent 负责理解用户意图，让用户保留最终签名权。Moss 正是在 Monad 生态中把这三层责任分开的一个实践。

## **后续计划**

-   继续关注 Moss 支持的协议 adapter 与能力目录。
    
-   尝试在本地或 Monad fork 环境中跑通一次 `discover → load → action → simulate`。
    
-   对比 Agent 直接调用合约与经由能力层调用的安全边界。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->









今天看完了实习手册感觉收获满满
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->










打卡打卡
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->











打卡打卡
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->












打卡打卡
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->













打卡打卡
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->














打卡
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->















加深之前的学习、了解  
**1\. Web3 行业赛道**  
主要赛道包括 DeFi、NFT、DAO、MEME，以及它们的组合创新。

-   `DeFi`：用智能合约做金融，比如 Uniswap 交易、Compound 借贷、MakerDAO/Sky 稳定币。
    
-   `NFT`：解决数字资产的唯一性和所有权问题，不只是图片，也可以和金融、社区权益结合。
    
-   `DAO`：用链上投票、金库和智能合约组织协作，适合社区治理和公共物品建设。
    
-   `MEME`：本质是社区文化和注意力资产，机会大但投机风险也高。
    
-   交叉方向很重要，比如 `DeFi + NFT`、`DAO + MEME`、`AI + DeFi`。
    

**2\. Web3 岗位方向**  
Web3 岗位大致分为技术岗和非技术岗。

-   `前端`：负责 DApp 页面、钱包连接、交易签名、合约交互。
    
-   `后端`：负责 API、链上数据读取、数据库、索引和服务稳定性。
    
-   `智能合约工程师`：负责 Solidity 合约开发、测试、部署、安全和 Gas 优化。
    
-   `产品 / 运营 / 社区 / 研究`：负责用户需求、社区增长、内容、生态分析和项目判断。
    

对我这个 Monad 学习者来说，当前最匹配的是：

```
主方向：Tech
重点：智能合约 + DApp 交互 + 技术文档
辅助：Research，用来理解 Monad 生态和 Web3 赛道
```
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
















听了jack老师的讲解，对eth协议方面有了更深的了解
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

















今天听了xiaohai老师的分享，对行业认知更加清晰一点
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
