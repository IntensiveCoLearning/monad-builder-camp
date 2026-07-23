---
timezone: UTC+8
---

# Tongfei

**GitHub ID:** Wang-Tongfei

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
提交 PR（Pull Request）的简短标准流程如下，按 **准备 → 提交 → 清理** 三步走：

**1\. 本地准备（同步与分支）**

-   **同步主分支**：确保本地主干与远程一致（`git checkout main` → `git pull`）。
    
-   **新建功能分支**：从最新的主干切出专用分支（`git checkout -b feature/your-task-name`）。
    

**2\. 代码提交与推送**

-   **提交更改**：`git add .` → `git commit -m "feat: 简短描述本次改动"`（建议遵循约定式提交规范）。
    
-   **推送远程**：`git push origin feature/your-task-name`（首次推送会提示设置上游分支）。
    

**3\. 发起 PR（Web 端操作）**

-   进入仓库页面，点击 **"Compare & pull request"** 或 **"New pull request"**。
    
-   **填写核心信息**：
    
    -   **标题**：简明扼要（如 `feat: 增加用户登录校验`）。
        
    -   **描述**：按模板填写（解决什么 Issue、改动了什么、如何验证）。
        
    -   **指定**：选择 **Reviewer（审核人）** 和 **Assignee（负责人）**，关联相关的 **Issue** 编号。
        
-   点击 **"Create pull request"** 提交。
    

**4\. 收尾与清理（合并后）**

-   等待 CI（持续集成）通过和 Reviewer 批准。
    
-   合并方式视团队规则（通常点 **"Squash and merge"** 或 **"Rebase and merge"** 以保持历史整洁）。
    
-   **删除远程分支**（点击 PR 页面上的 "Delete branch"），并拉取最新主干到本地（`git checkout main` → `git pull` → `git branch -d feature/your-task-name`）。
    

* * *

**💡 极简核心命令回顾（假设已关联远程）：**

bash

```
git checkout -b feat/new-feature
# ... 写代码 ...
git add . && git commit -m "feat: 完成新功能"
git push origin feat/new-feature
# 然后去网页点 "Create PR" 即可
```

> **小贴士**：若从 Fork 仓库提交，需先在 GitHub 上点 "Contribute" → "Open pull request"，注意选择正确的源分支（你的 Fork 分支）和目标分支（原仓库的主干）。
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->

黑客松（Hackathon），也常被称为**编程马拉松**，是一个充满活力的创新活动。它的名字是“编程”（Hack）和“马拉松”（Marathon）的组合，但它并非指恶意的网络攻击，也不是体力的长跑，而是一场**为把创意变为现实而进行的“思维与技能的冲刺”**。

### **📜 起源与发展**

黑客松的概念最早可以追溯到1999年。

-   **首次使用**：这个词首次被使用是在1999年6月4日，由OpenBSD的开发者们在加拿大卡尔加里的一次密码学开发活动中提出。
    
-   **同期出现**：几乎同时，在1999年6月19日的JavaOne大会上，Sun公司也使用了这个术语。
    
-   **更早的雏形**：这类活动的雏形可以追溯到1986年Sun公司举办的一系列“连接马拉松”。
    

如今，黑客松已成为全球科技圈非常流行的活动形式，像Google、Facebook（Meta）等知名企业都曾通过内部黑客松孵化出重要的产品功能。

### **🤔 到底在做什么？**

简单来说，黑客松就是一群不同背景的人（程序员、设计师、产品经理等）聚集在一起，在**很短时间内（通常是24到48小时）**，**高强度地协作**，将脑海中一个初步的想法，快速打造成一个可以展示和运行的产品原型。

它的核心精神是 **“合作”与“创造”** 。

### **📝 典型活动流程是怎样的？**

一场黑客松通常遵循以下节奏：

1.  **开场与创意提案**：活动组织者介绍主题和规则。随后，参与者可以上台推销自己的创意，招募队友。
    
2.  **自由组队与头脑风暴**：参与者根据兴趣现场组队（通常3-6人），讨论并确定要开发的项目。
    
3.  **极限编程与开发**：这是核心阶段。团队在限定时间内进行高强度的编程和设计工作，目标是做出一个可用的产品原型。
    
4.  **成果展示与评审**：在活动尾声，各团队向评委和观众展示自己的作品，并接受提问和打分。
    

### **🎯 黑客松有哪些类型？**

黑客松的主题非常多样，主要可以分为几类：

-   **按主办方**：有科技公司内部举办的、大学校园的学生黑客松、以及由社区或媒体组织的公开赛。
    
-   **按主题**：涵盖非常广泛，例如**AI**、**Web3/区块链**、**量子计算**、**法律科技**、**教育**等等。
    
-   **按规模**：从几十人的小型活动到数千人参与的大型国际赛事都有。
    

### **💡 为什么要参加黑客松？**

参加黑客松的价值远不止于获奖：

-   **将想法落地**：在最短时间内，把一个想法从零变成现实。
    
-   **快速学习与技能提升**：在实战中学习新技术，锻炼解决问题、团队协作和项目管理等能力。
    
-   **拓展人脉网络**：结识志同道合的伙伴、行业专家和潜在雇主。
    
-   **职业发展机会**：优秀的项目可能获得投资、孵化，甚至成立公司。很多公司也把黑客松表现作为招聘的参考。
    
-   **纯粹的创造乐趣**：享受和一群聪明人一起，在无拘束的环境中创造的快乐。
    

### **💎 总结**

黑客松不仅仅是一场编程比赛，它更代表了一种**将创意快速转化为现实的创新方法论**。无论你是经验丰富的开发者，还是刚入门的新手，只要你有想法和热情，都能在其中找到属于自己的价值和乐趣。
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->


## **本周学习总结：从“Moss 是什么”到“Moss 能做什么”**

### **一、本周主要发现**

本周最重要的一件事是澄清了一个认知偏差：**MegaETH 的 MOSS 钱包**与 **nishuzumi 的 Moss 交易能力框架**，虽然名字相同且都涉及 AI Agent，但实际上是两个不同的项目。

**MegaETH MOSS**（2026 年 6 月 17 日推出）是首款为 AI Agent 设计的**权限控制型钱包**。它通过自然语言设定规则（如“每日交易上限 500 USDM”或“只能在 Aave 上操作”），让 Agent 在授权范围内自主执行交易。用户无需交出私钥，且可随时一键撤销权限。

而此前一直在学习的是 **nishuzumi 的 Moss**（GitHub: nishuzumi/moss），一个面向 Monad 区块链的 **AI Agent 交易能力框架与安全验证层**，核心工作流为 `discover → load → action → simulate`。

这个发现提醒我：在 Web3 和 AI 交叉的快速演进领域，同名项目可能指向完全不同的技术方向，需要保持对项目背景的敏感。

### **二、对 Moss 项目理解的深化**

本周通过学习多份资料，对 Moss 的设计理念有了更深层次的理解。

**2.1 “永不签名”的设计哲学**

Moss 明确声明“永不签名、永不发送”，这不仅是技术选择，更是信任边界的设计。在 AI 安全尚未成熟的今天，让 AI 拥有签名权是危险的。Moss 把“构建和验证”交给系统，“签名和执行”留给用户——这个边界划分是务实的。

MegaETH 官方也表达了类似的观点：“让 AI Agent 靠近你的钱包是危险的。但在 MOSS 上，它不会。”

**2.2 从“手搓 calldata”到“能力抽象”**

Moss 解决的核心问题是：AI Agent 直接读取合约 ABI 并手搓 calldata 的风险太高。“几乎正确”的交易恰恰是最危险的——它可能成功执行，却产生非预期的资产变化。

Moss 的解法是把协议复杂性收拢到统一能力层背后，Agent 只负责意图决策，系统负责交易构建和机械验证。

**2.3 双重安全验证机制**

-   **Effects 对账**（机械判定）：模拟执行后对资产流动进行声明与实际的严格比对，有任何 warning 即停止
    
-   **意图对齐**（Agent 判定）：将模拟结果与用户原始意图对比
    

这套机制的本质是在签名前加了一道“机械安全闸门”，确保 Agent 的“意图”与链上“实际效果”对齐。

### **三、技术实践与探索**

**3.1 环境搭建**

按照 README 指引完成了基础环境配置（Node 22+、pnpm 11）：

bash

```
git clone https://github.com/nishuzumi/moss
cd moss
pnpm install
pnpm build
```

**3.2 示例运行**

成功运行了 WMON wrap 模拟和 Kuru swap 模拟：

bash

```
pnpm --filter @themoss/example-simple-flow wrap
pnpm --filter @themoss/example-simple-flow swap
```

通过实际运行，直观感受到 Moss 的核心价值：只需传入人类可读的参数，系统自动完成 ABI 编码、合约地址选择、小数位换算等复杂工作。

**3.3 MCP Server 集成探索**

Moss 暴露 `discover`、`load`、`action`、`simulate` 四个工具，可无缝接入 MCP 客户端。这意味着 Moss 可以作为 AI Agent 的“链上操作工具箱”被任意支持 MCP 的 Agent 框架调用——这是 AI Agent 与 Web3 结合的关键基础设施。

### **四、思考与展望**

**4.1 AI Agent × Web3 的安全范式**

本周的学习让我认识到，AI Agent 与 Web3 的结合面临一个根本矛盾：**Agent 需要自主权才能有价值，但完全的自主权意味着巨大的安全风险**。

Moss 的解法是**有限授权 + 机械验证**：Agent 只能在预设规则内操作（MegaETH MOSS 的思路），且每次操作前通过模拟验证确保效果符合预期（nishuzumi Moss 的思路）。两者结合，可能构成 AI Agent 安全操作链上资产的完整方案。

**4.2 两条技术路线的汇合**

有趣的是，nishuzumi 的 Moss（Monad 生态）和 MegaETH 的 MOSS（MegaETH 生态）虽然独立发展，但解决的是同一个问题——如何让 AI Agent 安全地操作链上资产。前者从“交易构建安全”角度切入，后者从“钱包权限控制”角度切入。未来两条路线有可能汇合，形成更完整的 AI Agent 链上操作安全体系。

**4.3 个人后续计划**

1.  **深入协议接入**：尝试为 Moss 接入新的 Monad 生态协议，编写 Protocol 包
    
2.  **构建 Demo**：用 Moss + MCP 构建一个完整的 AI Agent 示例
    
3.  **参与开源**：提交 Issue 和 PR，参与 Moss 的开源社区建设
    

### **五、总结**

本周从“Moss 是什么”出发，经历了项目理解、技术实践、生态对比的完整学习过程。最大的收获是：

> **Moss 不是让 AI Agent 更“聪明”，而是让 AI Agent 更“可靠”。**

在 AI Agent 自主执行链上操作这个方向上，最大的障碍不是 Agent 的理解能力，而是可靠性。Moss 的贡献在于把“Agent 如何与链交互”这个开放性问题，变成了“系统提供标准化能力、Agent 只做意图决策、模拟验证作为安全闸门”的封闭框架。

这正是 AI Agent × Web3 走向生产可用的关键一步。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->



## **Moss：为 AI Agent 铺设一条通往 Web3 的安全铁轨**

### **文章主题说明**

本文围绕 Moss 是什么、解决了什么问题、核心设计理念、应用场景以及如何参与展开，旨在帮助开发者快速理解这个 Monad 生态的开源项目，降低认知门槛，鼓励更多人参与到 AI Agent × Web3 的开源贡献中来。

### **文章正文**

**引言：当 AI Agent 遇上 Web3**

大语言模型的爆发，让 AI Agent 正在从单纯的“聊天框”演变为能够自主决策、执行复杂任务的行动体。而在所有执行场景中，Web3 区块链无疑是 AI Agent 最理想的试验场——这里有数字原生资产、无国界的智能合约接口，以及“代码即法律”的透明规则。

然而，当 AI Agent 试图伸手触碰链上资产时，现实并不美好：

-   **Calldata 幻觉**：智能合约调用需要极其精确的 ABI 编码、合约地址、小数点精度换算。LLM 面对复杂的 calldata 组装极易产生“幻觉”，差之毫厘便可能导致资金损失。
    
-   **私钥托管风险**：如果将私钥直接托管给 Agent，一旦遭遇提示词注入攻击或代码 Bug，用户钱包可能被洗劫一空。
    

如何让 AI Agent **安全地**在 Web3 协议间执行操作？Monad 生态的开源项目 **Moss** 给出了一套优雅的解法。

**一、Moss 是什么？**

Moss 是一个面向 Monad 区块链上协议交互的 **AI Agent 交易能力框架与安全验证层**。它将复杂的 DApp 和协议操作封装成 Agent 可调用的统一能力，核心工作流为：

text

```
discover → load → action → simulate
```

-   **discover**：跨协议发现可用的操作能力
    
-   **load**：加载对应协议的意图、参数语义和风险标签
    
-   **action**：传入人类可读的参数，返回结构化的未签名交易 Plan
    
-   **simulate**：在真实链上状态模拟执行，验证实际 effects 是否匹配声明
    

Moss **只构建和验证未签名交易，永不签名、永不发送**。私钥始终留在用户钱包中，最终签名权永远在用户手里。

**二、Moss 解决了什么问题？**

**2.1 把协议复杂性从 Agent 推理中移出**

一个 DEX 上的简单 swap，对实际执行程序来说涉及：确认正确的 Router 地址、选择 exact-in 或 exact-out 方法、处理原生代币与包装代币、查询 decimals、换算最小单位、计算滑点、判断授权、处理退款和扫尾调用……

Moss README 明确指出：**“几乎正确”的交易恰恰是最危险的**——它可能成功执行，却产生非预期的资产变化。

Moss 的解法是：让 LLM 负责 **“语义和决策”** ，让确定性软件负责 **“交易构建和机械验证”** 。

**2.2 安全的三权分立**

Moss 的核心设计哲学是 **“把构建、校验与签名彻底剥离”** ，在 Agent、协议库与钱包之间建立互不信任、互相制约的边界：

| 角色 | 职责 | 不做什么 |
| --- | --- | --- |
| Moss | 构建未签名交易、模拟验证 | 不碰私钥、不签名、不发送 |
| AI Agent | 理解意图、选择能力、对齐结果 | 不碰私钥、不直接组装 calldata |
| 钱包 | 审查 Plan、签名广播 | 不参与交易构建 |

**2.3 双重安全验证**

Moss 通过两道安全闸门防范风险：

1.  **Effects 对账**（机械判定）：模拟执行后，对每一笔资产流出/流入、授权进行声明与实际的严格比对——**有任何 warning 即停止**
    
2.  **意图对齐**（Agent 判定）：将模拟结果摘要与用户原始意图对比——只有 Agent 掌握用户的原话
    

**三、应用场景：AI 驱动的 DeFi 策略自动执行**

**场景描述**：用户用自然语言描述策略——“当 wMON 价格低于 X 时，将 30% 的 USDC 换成 wMON，然后存入流动性池”。Moss 负责：

1.  **discover**：发现可用的 swap 和流动性池能力
    
2.  **load**：加载对应协议的参数语义
    
3.  **action**：构建未签名交易的 Plan
    
4.  **simulate**：在真实链上状态模拟，验证资产流动是否符合预期
    

**为什么是 Moss**：

-   Agent 不需要知道合约地址或 ABI
    
-   模拟机制在签名前验证每一步
    
-   多步 Plan 可串联执行（claim → swap → supply）
    
-   任何未声明差异都会产生 warning，阻止危险交易
    

**四、如何开始参与？**

Moss 目前支持 Monad 主网（chain ID 143），已内置 WMON、ERC-20、ERC-721 和 Kuru 等协议支持。

**快速开始**（需 Node 22+ 和 pnpm 11）：

bash

```
git clone https://github.com/nishuzumi/moss
cd moss
pnpm install
pnpm build

# 模拟一个 WMON wrap
pnpm --filter @themoss/example-simple-flow wrap

# 报价并模拟 Kuru MON→USDC swap
pnpm --filter @themoss/example-simple-flow swap
```

**作为 MCP Server 使用**：Moss 暴露 `discover`、`load`、`action`、`simulate` 四个工具，可无缝接入 MCP 客户端。

**贡献方向**：

-   为新协议编写 Protocol 包（封装地址、ABI、calldata 构建和 Receipt 解析）
    
-   完善文档和教程
    
-   提交 Issue 和 Bug 报告
    

> ⚠️ **提醒**：Moss 目前是未经审计的 Alpha 软件，**请勿用于生产资金**。

**五、我的个人理解**

Moss 解决的本质问题是：**如何让 AI Agent 以可控、可审计、可验证的方式调用智能合约**——这不是让 Agent 更“聪明”，而是让 Agent 更“可靠”。

如果把 Moss 比作 Web3 版的 Claude Code，那么 Claude Code 帮 AI 戴上安全手套去操作文件系统，而 Moss 则是帮 AI 戴上安全手套去操作 DeFi 协议和链上资产。

在 AI 安全尚未成熟的今天，让 AI 拥有签名权是危险的。Moss 把“构建和验证”交给系统，“签名和执行”留给用户——这个边界划分是务实的，也是值得信赖的。

优秀的开源项目不仅需要代码，也需要好的文档和内容来帮助更多人理解、参与。希望这篇文章能让你对 Moss 产生兴趣，一起参与到 AI Agent × Web3 的开源生态中来。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->




### **🧐 Moss 是什么？它解决了什么问题？**

简单来说，Moss 是一个为 AI Agent 安全进入 Web3 世界而设计的开源框架。

当 AI Agent 要操作链上资产时，两大核心风险是：

1.  **“手抖”风险**：AI 在构造复杂的智能合约调用时可能产生“幻觉”，导致资产损失。
    
2.  **“被盗”风险**：将私钥直接交给 AI，一旦被攻击，资产可能被瞬间清空。
    

Moss 的解决之道是 **“三权分立”** ，将权力拆分为三个互不信任的角色：

-   **Moss (构建与验证者)**：作为“能力货架”，负责将用户的自然语言指令翻译成未签名的交易计划（Plan），并在一个**链上沙盒（Fork 链）** 中模拟运行以验证其影响。**核心原则：只构建、能对账、不签名**。
    
-   **钱包 (签名与广播者)**：私钥完全由用户掌握。钱包会严格审查 Moss 生成的交易计划，确认无误后才交由用户手动签名。
    
-   **AI Agent (决策者)**：负责理解用户意图并做出决策，但**不接触私钥，也不直接构建交易**。
    

这种设计确保了 AI 的自由度与资产的安全性可以兼得。

### **🚀 Moss 的应用场景**

Moss 的应用场景远不止于安全框架，它正在多个维度推动 AI Agent 的落地。

-   **场景一：零门槛的 AI 交易员工厂**：Moss 提供了一个开放平台，你只需用大白话描述交易想法（如“我想做趋势反转”），AI 就能自动生成一套完整的量化策略并部署为一个自动运行的交易 Agent。整个过程无需编程，并且创建的 Agent 还有**“地狱模式”**（历史数据回测）和**“实时模式”**供你验证策略。更有趣的是，这些 Agent 具备**每周自我演化**的能力，能根据表现自动调整参数。
    
-   **场景二：信息聚合的 AI 助手**：在交易 Agent 之前，Moss 就已是一款成熟的 Chrome 浏览器插件，能嵌入 X（Twitter）页面，提供**实时行情摘要、KOL 观点聚合和链上 Alpha 信号追踪**。
    
-   **场景三：语义搜索的实时引擎**：Moss 的核心技术是一个**低于10毫秒**的语义搜索引擎。它能在浏览器、边缘设备或云端本地运行，无需依赖外部向量数据库，非常适合需要实时响应的语音机器人、Copilot 等场景。
    

### **🛠️ 如何参与 Moss 开源贡献？**

参与开源是学习技术、积累作品的有效方式。

-   **从何处入手**：关注社区活动，例如由 LXDAO 等组织举办的 Web3 暑期实习计划，会邀请专家讲解如何参与 Moss 贡献。
    
-   **可以做什么**：贡献不限于写代码。你可以从完善**文档（Documentation）**、**测试（Testing）**，或提交一个**功能建议（Feature Request）** 开始。
    
-   **如何操作**：参照标准的 GitHub 协作流程，**Fork** 项目仓库，创建新分支，完成你的工作后提交 **Pull Request (PR)**。
    

### **📝 如何使用 Moss 构建 AI Agent？**

根据官方教程，使用 Moss SDK 构建一个基础 Agent 非常直接。

1.  **环境准备**：确保安装 Node.js (>=22) 和 pnpm。
    
2.  **创建项目**：
    
    bash
    
    ```
    mkdir my-first-moss-agent && cd my-first-moss-agent
    pnpm init
    pnpm add @themoss/core @themoss/simulator @themoss/system @themoss/erc
    pnpm add -D typescript @types/node tsx
    ```
    
3.  **编写 Agent (**`src/agent.ts`**)**：
    
    -   初始化 **Runtime** 连接区块链。
        
    -   创建 **Registry** 并注册官方提供的功能模块。
        
    -   初始化 **Simulator** 用于安全模拟。
        
    -   在核心函数中，让 Agent 解析用户指令，通过 Registry 寻找并构建交易，最后用 Simulator 进行验证。
        
4.  **运行与验证**：运行脚本，Agent 就会在安全的沙盒环境中模拟执行你的指令，整个过程**不涉及真实资产**，却能完整验证逻辑。
    

### **💎 总结**

Moss 项目的核心价值，在于它通过 **“三权分立”的安全架构**，解决了 AI Agent 进入 Web3 的最大障碍——**安全**。同时，它通过 **“零门槛的AI交易员平台”**和 **“超高速的语义搜索引擎”** 等应用，极大降低了普通人拥抱 AI 与 Web3 的门槛。

它的存在，让“人人都能拥有自己的 AI Agent”这件事变得触手可及。如果你对这个领域感兴趣，Moss 无疑是一个值得深入研究和参与的开源项目。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->





## **Web3 Agent**

### **一、什么是 Web3 Agent？**

### **1.1 AI Agent 与对话式 AI 的区别**

先抛开复杂概念，直接对比 AI Agent 和现有大模型（如 ChatGPT）的区别：

| 维度 | 传统 AI 工具（如 ChatGPT） | AI Agent |
| --- | --- | --- |
| 工作模式 | 被动回应 | 主动执行 |
| 任务能力 | 单次任务 | 连续多步骤任务 |
| 工具调用 | 有限 | 可调用外部系统（API、智能合约等） |
| 自主规划 | 较弱 | 较强 |
| 链上互动 | 通常不支持 | 可连接智能合约 |

AI Agent 的核心是“**感知 → 决策 → 执行 → 反馈**”的自主闭环。它不再局限于“回答问题”或“生成内容”，而是能主动感知环境、分析目标并完成复杂任务。

### **1.2 Web3 AI Agent 的独特之处**

Web3 AI Agent = AI + 区块链 + 加密经济。相比 Web2 的 AI Agent，Web3 赋予其三大核心特性：

1.  **去中心化**：操作、数据存储和决策过程透明，不受单一实体控制
    
2.  **经济激励**：通过代币经济模型激励参与和贡献
    
3.  **“永生”能力**：智能合约部署在区块链上，Agent 可永久运行
    

> 学术定义：Web3-AI Agent 是人工智能与去中心化技术的融合，以大语言模型（LLM）为核心推理引擎，作为 Web3 交互的智能前端。

### **二、工作原理与架构**

### **2.1 核心工作流程**

AI Agent 的运作通常包含以下阶段：

1.  **感知**：接收用户指令、链上数据、市场信息等
    
2.  **分析**：AI 模型分析信息，理解意图
    
3.  **规划**：根据目标制定执行计划
    
4.  **执行**：调用 API、智能合约等完成具体任务
    
5.  **反馈**：根据结果优化，提升后续效率
    

### **2.2 四层架构**

2026 年的链上 AI Agent 普遍采用四层架构：

-   **感知层（Perception）**：从区块链状态、价格数据、链上交易、链下 API 等多源获取数据
    
-   **推理层（Reasoning）**：LLM 评估当前状态与目标，考虑 Gas 成本、滑点、风险等
    
-   **规划层（Planning）**：将高层次推理转化为具体链上操作序列
    
-   **执行层（Execution）**：管理钱包、签名交易、Gas 估算、nonce 管理、错误恢复
    

### **三、技术栈与开发框架**

### **3.1 常用技术栈**

构建 Web3 AI Agent 的典型技术组合：

-   **编程语言**：Python（AI 领域最常用）
    
-   **区块链交互**：[Web3.py](http://Web3.py)（Python Ethereum API）
    
-   **DEX 交易**：Uniswap V3/V4
    
-   **LLM 推理**：OpenAI API、Anthropic、本地模型等
    
-   **云服务**：AWS（Bedrock、Lambda、Secret Manager 等）
    

### **3.2 主流开发框架与工具**

| 框架/工具 | 特点 | 适用场景 |
| --- | --- | --- |
| Solana Agent Kit | TypeScript 库，封装 Solana Web3 SDK，为 AI 代理工作流设计 | Solana 生态 Agent 开发 |
| Injective AI Agent SDK | 四合一工具包，单命令构建自主金融 Agent | DeFi 领域 |
| AgentKit (Inngest) | 定义 Agent、工具并运行，专注可扩展性 | Web3 原生 Agent（打赏机器人、代币部署器等） |
| ShellAgent (MyShell) | 无代码/低代码 AI 开发框架 | 快速创建 AI 应用 |
| Iwa | Python 框架，管理加密钱包和智能合约交互 | 自主 Agent 开发 |
| web3agent | MCP 兼容的 Agent 安装工具 | 现有 Agent 扩展 |

### **3.3 典型实现案例**

**案例一：AWS 构建的 Web3 AI Agent**

使用 AWS 服务构建的 Web3 AI Agent，核心功能包括：

-   代币分析与实时价格查询
    
-   Gas 费估算
    
-   链上地址余额查询
    
-   投资建议
    
-   交易执行
    
-   Web3 新闻与数据聚合
    

技术实现：用户请求 → Amazon Bedrock Agent（LLM 处理）→ Action Group（调用 Lambda 函数）→ 区块链操作

**案例二：ETH-USDC 交易 Agent**

从手动交易逐步演进到完全自主的 AI 交易 Agent，涉及：

-   本地优先开发
    
-   直接区块链交互
    
-   自定义微调模型
    
-   GANs、教师-学生蒸馏、强化学习等高级技术
    

### **四、应用场景**

### **4.1 已落地的应用类别**

根据实际生产部署，以下类别已较为成熟：

-   **投资组合再平衡 Agent**：监控用户持仓，按策略参数执行再平衡
    
-   **收益优化 Agent**：根据利率变化在借贷协议间移动资金
    
-   **DAO 治理 Agent**：分析提案并按委托策略投票
    
-   **NFT 交易 Agent**：竞价、挂单、跨市场套利
    
-   **自动交易与 DeFi 组合管理**
    
-   **跨链交易助手与智能钱包界面**
    
-   **智能合约审计与漏洞检测**
    

### **4.2 更广泛的应用方向**

-   DAO 治理（提案分析、自动化执行）
    
-   DeFi（链上数据分析、收益策略管理）
    
-   多链生态（跨链数据同步、协议协调）
    
-   RWA、GameFi、SocialFi 等领域
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->






## **智能合约漏洞核心**

> **核心认知**：合约一旦上链就**无法修改**，一个漏洞可能直接导致千万级损失。2024—2025年，因合约漏洞造成的链上损失超过**20亿美元**，其中**67%** 来自“访问控制”这一个类别。

* * *

### **第一章：OWASP 十大漏洞风险速览**

| 排名 | 漏洞类型 | 一句话定性 |
| --- | --- | --- |
| 1 | 访问控制漏洞 | 权限没锁好，谁都能当管理员 |
| 2 | 预言机操纵 | 价格来源被操控，协议被套利掏空 |
| 3 | 业务逻辑错误 | 代码写出来跟产品经理想的不一样 |
| 4 | 输入验证不足 | 没检查入参，攻击者塞什么合约都吃 |
| 5 | 可重入攻击 | 款没扣完就被反复调用提现 |
| 6 | 未检查外部调用 | 调了别人家的函数，返回失败也当没看见 |
| 7 | 闪电贷攻击 | 一笔交易内借巨款操纵市场状态 |
| 8 | 整数溢出/下溢 | 数字算爆了，变成极大或极小值 |
| 9 | 不安全随机数 | 用链上公开变量抽奖，结果被算出来 |
| 10 | 拒绝服务（DoS） | 合约被卡死，再也无法正常运转 |

* * *

### **第二章：Top 5 高危漏洞详解（务必掌握）**

### **🔴 1. 访问控制漏洞（损失最惨重，占67%）**

-   **怎么回事**：关键函数没加权限修饰符（如 `onlyOwner`），或者角色权限设计得稀烂，导致任意地址都能调用 `withdrawAll()`、`setAdmin()` 这类核弹级函数。
    
-   **怎么防**：直接用 OpenZeppelin 的 `Ownable` 或 `AccessControl` 套件；管理员地址不要写死，支持转移；关键操作加上 **时间锁（Timelock）** 延迟执行，给用户反应时间。
    

### **🔴 2. 可重入攻击（最经典的坑）**

-   **怎么回事**：合约先把钱转给用户，**然后再**去更新用户的余额。攻击者在收款时触发恶意回调，在余额没扣减之前再次调用提款函数，把钱反复提空。
    
-   **怎么防**：严格执行 **CEI 模式（Checks-Effects-Interactions）**，也就是：先校验条件 → 先更新余额（改状态）→ 最后再发起转账。或者直接加 OpenZeppelin 的 `nonReentrant` 修饰器。
    

### **🔴 3. 预言机价格操纵（DeFi 头号杀手）**

-   **怎么回事**：借贷或交易协议依赖某个链上池子的瞬时价格。攻击者用闪电贷借来巨额资金，短时间内大幅拉高或砸盘某个交易对，制造一个虚假的高/低价，然后以这个扭曲的价格借走协议里所有的钱，最后归还闪电贷，白赚一笔。
    
-   **怎么防**：不要用瞬时价格，改用 **TWAP（时间加权平均价格）**；引入多个数据源（如 Chainlink + 多家交易所）交叉验证；对单次价格更新设置合理的涨跌幅上限。
    

### **🔴 4. 业务逻辑错误（最隐蔽的坑）**

-   **怎么回事**：代码语法完全正确，编译器也不报错，但**业务上的计算或判断是错的**。例如：算奖励时用了除法导致大量余数被截断归零、创建借贷池时没检查抵押物是否为官方正版合约、代币销毁和铸造的比例设反了。
    
-   **怎么防**：写代码之前先把复杂的计算公式在纸上推演一遍；编写覆盖所有边界情况的单元测试；最重要的——**务必找多家审计机构交叉审计**，内部开发者的思维定势很难发现自己逻辑上的盲区。
    

### **🔴 5. 整数溢出（旧版 Solidity 重灾区）**

-   **怎么回事**：在 Solidity 0.8.0 版本之前，`uint8` 的范围是 0~255。如果 `255 + 1` 结果会变成 `0`；`0 - 1` 会变成 `255`。黑客常利用这一点绕开余额检查。
    
-   **怎么防**：现在直接**使用 Solidity 0.8.0 及以上版本**编译，编译器默认内置了溢出检查，超限直接报错回滚。如果还在维护古董级老合约，必须引入 `SafeMath` 库。
    

* * *

### **第三章：高频避坑战术清单**

**1\. 编码黄金三原则**

-   **CEI 铁律**：状态更新永远先于外部转账。
    
-   **零信任外部调用**：调用外部合约必须用 `require(success)` 接住返回值，默认它一定会失败。
    
-   **预留逃生舱**：项目初期必须部署 **Circuit Breaker（紧急暂停）** 功能，发现不对立刻熔断。
    

**2\. 随机数红线**

-   **绝对不要**用 `block.timestamp`、`blockhash`、`block.difficulty` 来做高价值抽奖或 NFT 盲盒铸造，矿工有能力轻微操纵这些值。
    
-   **正确的做法**：直接使用 **Chainlink VRF** 获取链下生成的真随机数。
    

**3\. DoS 拒绝服务预防**

-   当合约需要给大量用户分发奖励或退款时，**千万别用 for 循环遍历一个长数组**去挨个转账（Gas 会爆，合约直接卡死）。
    
-   改用 **Pull-over-Push（提款模式）**：只记录每个人该拿多少钱，让用户自己调用 `withdraw()` 来取，把 Gas 成本转嫁给主动提款的人。
    

* * *

### **第四章：安全开发 SOP（照做保平安）**

| 阶段 | 必做事项 |
| --- | --- |
| 编码阶段 | 引用 OpenZeppelin 标准库；锁定 Solidity 具体版本号；复杂逻辑拆成小函数方便测试 |
| 测试阶段 | 编写单元测试覆盖所有正常/边界场景；用 Hardhat/Foundry 模拟恶意攻击尝试 |
| 部署前夕 | 至少做 2 轮独立审计（不同机构）；部署到测试网运行 1~2 周观察 |
| 上线之后 | 部署多签钱包管理合约所有权；开启 24 小时链上监控告警；设置漏洞赏金计划 |

* * *

> **最终结语**：安全不是靠运气，而是靠**严苛的流程与极度的不信任**。每写一个 `call`、每暴露一个 `public` 函数，都要下意识地问自己：“如果这个入口被 1 万个恶意脚本同时攻击，它扛得住吗？” 带着这种心态写合约，才算真正入行。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->







## **Git 协作**

> **核心思想**：Git 不是存储文件，而是**管理文件变化**。每一次提交（Commit）都是项目历史的一个“快照”节点。

### **📝 Git 核心（常用命令速查）**

**1️⃣ 配置与初始化**

| 命令 | 说明 |
| --- | --- |
| git config --global user.name “Name” | 设置全局用户名 |
| git config --global user.email “email” | 设置全局邮箱 |
| git init | 当前目录初始化为 Git 仓库 |
| git clone | 克隆远程仓库到本地 |

**2️⃣ 暂存与提交**

| 命令 | 说明 |
| --- | --- |
| git add | 添加指定文件到暂存区 |
| git add . | 添加当前目录所有更改到暂存区 |
| git commit -m “msg” | 提交暂存区到本地仓库 |
| git commit -am “msg” | 跳过 add，直接提交所有已追踪文件的修改 |
| git commit --amend | 修改上一次提交信息或漏掉的文件（不推荐推过远程后用） |

**3️⃣ 分支操作（现代推荐用法）**

| 命令 | 说明 |
| --- | --- |
| git branch | 查看本地分支列表（当前带 *） |
| git branch -a | 查看所有分支（含远程） |
| git switch -c | 新建分支并切换（Git 2.23+ 推荐，替代 checkout） |
| git switch | 切换分支 |
| git merge | 将目标分支合并到当前分支 |
| git branch -d | 删除本地分支（-D 强制删） |

> 老版本常用 `git checkout -b`，新版本建议用 `switch` 和 `restore` 区分职责。

**4️⃣ 同步与远程**

| 命令 | 说明 |
| --- | --- |
| git remote -v | 查看远程仓库别名及地址 |
| git remote add origin | 关联本地仓库到远程 |
| git pull origin main | 拉取远程更新并合并（相当于 fetch + merge） |
| git pull --rebase | 拉取并变基（保持线性历史，推荐避免多余 Merge 节点） |
| git push origin main | 推送到远程主分支 |
| git push -u origin main | 推送并建立上游追踪（下次直接 git push） |

**5️⃣ 查看与撤销**

| 命令 | 说明 |
| --- | --- |
| git log --oneline --graph | 精简提交树视图（强烈推荐） |
| git log -p | 查看详细修改内容 |
| git diff | 查看工作区与暂存区的差异 |
| git diff HEAD | 查看工作区与最近一次提交的差异 |
| git restore | 丢弃工作区修改（慎用，不可恢复） |
| git restore --staged | 撤回 add（从暂存区移除，保留修改） |
| git reset --soft HEAD~1 | 撤销上一次 commit，保留修改在暂存区 |
| git reset --hard HEAD~1 | 撤销上一次 commit，彻底丢弃修改（极度危险） |

* * *

### **💡 提交信息（Commit Message）**

规范的提交信息能显著提升协作效率，推荐 **约定式提交** 格式：

text

```
<type>(<scope>): <subject>
空一行
<body>
空一行
<footer>
```

**常用 type 速记**：

-   `feat`：新功能
    
-   `fix`：修复 Bug
    
-   `docs`：文档修改
    
-   `style`：代码格式（不影响逻辑，如空格、缩进）
    
-   `refactor`：重构（既非新增功能，也非修复 Bug）
    
-   `perf`：性能优化
    
-   `test`：增加或修改测试
    
-   `chore`：构建工具、依赖更新等杂项
    

> **例子**：`feat(login): 增加手机号验证码登录` 或 `fix(cart): 修复数量为负数时的计算错误`

* * *

### **🆘 常见问题快速急救**

| 问题场景 | 解决方案 |
| --- | --- |
| 提交完了发现漏了文件 | git add 漏掉的文件 → git commit --amend |
| 想撤回刚刚的 add | git restore --staged . |
| 想丢弃某个文件的修改 | git restore <文件名> （切记先备份！） |
| Push 被拒绝（远程有更新） | 先 git pull --rebase，解决冲突后再 git push |
| 合并时发生冲突 | 手动编辑文件删除 <<<<< ===== >>>>> 标记 → git add . → git commit |
| 想切分支但不想提交当前修改 | git stash（暂存） → 切分支 → git stash pop（恢复） |

* * *

> **黄金法则**：**永远不要** `--force` **推送到公共主分支（main/master）**。如果要强制推送，只在个人开发分支使用 `git push --force-with-lease`（比 `-f` 更安全）。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->









## **Week 2 | Moss 学习笔记**

### **一、项目简介**

Moss 是一个将 **Monad 区块链**上复杂的 DApp/协议交互抽象为 AI Agent 可调用统一能力的框架。它的核心工作流是 `discover → load → action → simulate`——由系统而非 Agent 负责组装正确的交易。

Moss 目前处于 **Alpha 阶段**，运行在 Monad 主网（chain id 143）上。它**永不签名、永不发送**交易，只负责构建和验证，私钥始终留在钱包里，最终决定权在用户手中。

### **二、它解决了什么问题？**

**2.1 核心痛点：AI Agent 手搓交易数据太危险**

Moss 要解决的根本问题是：**让 AI Agent 安全地与区块链协议交互**。

考虑一个 DEX 上的“简单”swap：涉及 router 地址、exact-in 与 exact-out 变体、原生代币的包装与解包装、退款和扫尾调用、滑点计算……。一个 AI Agent 通过读取 ABI 来组装这些调用，“最终会把其中某一个搞错”——而一个 **“几乎正确”** 的 AI 构建的交易，恰恰是资金丢失的方式。

**2.2 两条安全规则**

Moss 通过两条在不同位置强制执行的安全规则来防范风险：

| 规则 | 执行位置 | 作用 |
| --- | --- | --- |
| Effects 对账 | 服务器侧（机械判定） | 模拟提取实际发生的资产流出/流入、授权、收款方（包括不发 Transfer 事件的原生 MON 流和 wrapped 代币铸毁），对任何 Plan 未声明的差异产生警告——有 warning 即停 |
| 意图对齐 | Agent 侧 | 将 effects 摘要与用户的原始意图对比——只有 Agent 掌握用户的原话 |

**2.3 为什么现有方案不够？**

如果让 Agent 直接读取合约 ABI 并手搓 calldata，风险在于：

-   ABI 解析复杂，多步调用（如 multicall）容易出错
    
-   不同协议参数格式不统一
    
-   小数位换算、滑点计算等细节极易失误
    
-   一旦 Agent 犯错，后果是直接的资金损失
    

Moss 的解法是：**把复杂性收拢到按协议维护的统一能力层背后**，在每一次签名前加一道机械安全闸门。

### **三、核心能力**

**3.1 四步调用流**

text

```
discover(verb?, category?) → 跨协议发现能力
load(coordinates) → intent、参数语义、风险标签
action(protocol, method, params) → 返回未签名交易 Plan + 量化期望
simulate(plans[]) → 实际 effects + warnings（声明 vs 实际对账）
```

**3.2 Agent 不再手搓 calldata**

Agent 不需要接触 ABI、合约地址、multicall 扫尾、decimals 换算——能力层接受人类可读的参数，返回组装完毕的未签名交易。

**3.3 多步流程支持**

`simulate` 接受 Plan 数组并在计划之间延续状态——Plan B 可以花掉只在 Plan A 模拟结果里才存在的代币。这是多步流程（claim → swap → supply）的基础。

**3.4 已支持协议**

目前支持 WMON（wrap/unwrap/balanceOf）、ERC20 通用协议（任意代币转账/余额/授权查询，含原生 MON）、ERC721 通用协议（任意 NFT 转账/归属查询）、Kuru（市价单 swap、报价、市场列表）。

### **四、可能应用场景**

**场景：AI 驱动的 DeFi 收益策略自动执行**

**描述**：用户用自然语言描述一个收益策略，例如“当 wMON 价格低于 X 时，将 30% 的 USDC 换成 wMON，然后存入 Kuru 的流动性池”。Moss 框架负责：

1.  **discover**：发现可用的 swap 和流动性池能力
    
2.  **load**：加载对应协议的 intent 和参数语义
    
3.  **action**：构建未签名交易的 Plan
    
4.  **simulate**：在真实链上状态模拟执行，验证 effects 是否匹配预期
    

**为什么 Moss 适合这个场景**：

-   Agent 不需要知道 Kuru 的合约地址或 ABI 细节
    
-   模拟机制可以在签名前验证每一步的资产流动是否符合预期
    
-   多步 Plan 链式模拟确保整个流程（swap → 添加流动性）可串联执行
    
-   任何未声明的差异都会产生 warning，阻止危险交易
    

**安全边界**：Moss 不签名、不发送，最终交易由用户在钱包中审核后签名——这确保了即便 Agent 的策略逻辑有误，用户仍有最终否决权。

### **五、我的个人理解**

**5.1 Moss 的本质：为 AI Agent 建立区块链操作的“安全操作系统”**

Moss 做的不是让 Agent 更“聪明”，而是让 Agent 更“安全”。在 AI Agent 自主执行链上操作这个方向上，最大的障碍不是 Agent 的理解能力，而是**可靠性**——一个会犯错但看起来很自信的 Agent，在金融场景中是灾难性的。

Moss 的贡献在于：它把“Agent 如何与链交互”这个开放性问题，变成了“系统提供标准化能力、Agent 只做意图决策、模拟验证作为安全闸门”的封闭框架。这有点像操作系统把硬件抽象成系统调用——应用（Agent）不需要直接操作硬件（合约），只需要调用标准接口。

**5.2 “永不签名”是设计亮点**

Moss 明确声明“永不签名、永不发送”，这不仅是技术选择，更是**信任边界的设计**。在 AI 安全尚未成熟的今天，让 AI 拥有签名权是危险的。Moss 把“构建和验证”交给系统，“签名和执行”留给用户——这个边界划分是务实的。

**5.3 Alpha 阶段的现实提醒**

Moss 目前是 Alpha 软件，未经审计，API 和 Plan 格式可能变动。项目文档也坦诚指出：“模拟是一道安全网，不是保证——模拟结果反映的是模拟那一刻的链上状态，从模拟到签名之间，价格、流动性、合约状态都可能变化。”

这种对自身局限的坦诚，反而增加了项目的可信度。

### **六、参考链接**

-   [Moss GitHub 仓库](https://github.com/nishuzumi/moss)
    
-   [Moss 中文 README](https://github.com/nishuzumi/moss/blob/main/README.zh-CN.md)
    
-   [Moss](https://github.com/nishuzumi/moss/blob/main/SECURITY.md) [Security.md](http://Security.md)
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->










## **Wagmi**

> Wagmi 是 React 的 Web3 工具库，基于 Viem，提供类型安全的 Hooks 与合约交互。

* * *

### **1\. 安装**

bash

```
npm install wagmi viem @tanstack/react-query
```

* * *

### **2\. 配置**

**2.1 定义链（以 Monad 为例）**

ts

```
// wagmi.ts
import { createConfig, http } from 'wagmi';
import { defineChain } from 'viem';

export const monad = defineChain({
  id: 10143,
  name: 'Monad Testnet',
  nativeCurrency: { name: 'Monad', symbol: 'MON', decimals: 18 },
  rpcUrls: { default: { http: ['https://testnet-rpc.monad.xyz'] } },
});

export const config = createConfig({
  chains: [monad],
  transports: { [monad.id]: http() },
});
```

**2.2 包裹 Provider**

tsx

```
// main.tsx
import { WagmiProvider } from 'wagmi';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { config } from './wagmi';

const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById('root')!).render(
  <WagmiProvider config={config}>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </WagmiProvider>
);
```

* * *

### **3\. 核心 Hooks**

### `useAccount`

获取当前连接地址及连接状态。

tsx

```
const { address, isConnected } = useAccount();
```

### `useConnect`

连接钱包（MetaMask 等）。

tsx

```
const { connect, connectors } = useConnect();
// 使用：connect({ connector: connectors[0] })
```

### `useDisconnect`

断开连接。

tsx

```
const { disconnect } = useDisconnect();
```

### `useReadContract`

读取合约数据（只读）。

tsx

```
const { data, refetch } = useReadContract({
  address: contractAddress,
  abi,
  functionName: 'balanceOf',
  args: [address],
  query: { enabled: !!address },   // 条件启用
});
```

### `useWriteContract`

写入合约（发起交易）。

tsx

```
const { writeContract, data: hash, isPending } = useWriteContract();

// 调用
writeContract({
  address: contractAddress,
  abi,
  functionName: 'vote',
  args: [memeId],
});
```

### `useWaitForTransactionReceipt`

等待交易确认。

tsx

```
const { isLoading: isConfirming } = useWaitForTransactionReceipt({
  hash: writeData,   // 来自 useWriteContract 的 data
});
```

* * *

## **4\. 组合示例（投票 + 刷新）**

tsx

```
const { writeContract, data: writeData, isPending } = useWriteContract();
const { isLoading: isConfirming } = useWaitForTransactionReceipt({ hash: writeData });

const { refetch } = useReadContract({ ... });

useEffect(() => {
  if (writeData && !isConfirming) refetch();
}, [writeData, isConfirming]);
```

* * *

### **5\. 重要提醒**

-   **ABI 必须** `as const` 以获得类型推断。
    
-   `args` **条件传递**：当参数可能为 `undefined` 时，使用 `args: address ? [address] : undefined` 配合 `enabled`。
    
-   **QueryClient** 必须放在 `WagmiProvider` 内部。
    
-   所有读写操作均基于 Viem，类型安全。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->











## **Meme Battle 应用开发笔记**

### **📌 项目概述**

基于 **Monad 测试网** 的链上 Meme 对战与排行榜 DApp。核心功能包括：

-   用户提交 Meme（存储 IPFS URI）
    
-   投票给 Meme（每次 +1 分，作者积分增加）
    
-   每日任务（每天完成 +10 分）
    
-   积分达到阈值自动获得徽章（Bronze/Silver/Gold/Diamond）
    
-   积分可兑换奖励（合约中预留可领取奖励机制）
    

技术栈：**Solidity（Remix 可编译）+ React + TypeScript + Wagmi + Viem + Vite**

* * *

### **1\. 环境准备**

-   **Node.js** ≥ 18.0
    
-   **MetaMask** 浏览器扩展
    
-   添加 Monad 测试网到 MetaMask：
    
    -   网络名称：Monad Testnet
        
    -   RPC URL：`https://testnet-rpc.monad.xyz`
        
    -   链 ID：`10143`
        
    -   货币符号：MON
        
    -   区块浏览器：`https://testnet.monadscan.com`
        
-   领取测试币：前往 [Monad Faucet](https://testnet.monad.xyz/faucet) 获取 MON
    

* * *

### **2\. 智能合约（Solidity）**

**2.1 合约代码**

在 Remix 中新建 `MemeBattle.sol`

### **2.2 部署步骤（Remix）**

1.  打开 Remix IDE，创建 `MemeBattle.sol`。
    
2.  编译：选择 Solidity 版本 0.8.20，启用优化。
    
3.  部署：环境选 `Injected Provider`，连接 MetaMask（确保网络为 Monad Testnet）。
    
4.  点击 `Deploy`，确认交易。
    

### **3\. 前端项目构建**

**3.1 项目文件结构**

text

```
meme-battle/
├── src/
│   ├── wagmi.ts
│   ├── contract.ts
│   ├── App.tsx
│   ├── main.tsx
│   └── ...
├── package.json
└── ...
```

**3.2 各文件代码**

（待整理）

### **4\. 运行项目**

1.  确保 `contract.ts` 中的 `contractAddress` 已替换为实际部署地址，且 ABI 正确。
    
2.  终端执行：
    
    bash
    
    ```
    npm run dev
    ```
    
3.  打开浏览器访问 `http://localhost:5173`。
    
4.  点击 **Connect Wallet**，授权 MetaMask 连接。
    
5.  提交 Meme（输入 IPFS URI），投票，完成每日任务，观察积分和排行变化。
    

* * *

### **5\. 注意事项**

-   **ABI 更新**：每次修改合约并重新部署，都需要复制新的 ABI 到 `contract.ts`。
    
-   **Gas 费用**：确保钱包有足够的 MON 测试币。
    
-   **每日任务**：每天只能完成一次，基于 UTC 时间。
    
-   **排行榜优化**：生产环境建议使用后端索引（如 The Graph）替代 `getAllMemes` 链上查询，以降低 Gas 并提升速度。
    
-   **徽章**：积分达到阈值自动触发，可在事件中监听 `BadgeEarned`。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->












## Week 1 总结

### 学习内容

-   了解 Monad 的核心特点：高性能、低延迟、EVM 兼容。
    
-   学习 Solidity 基础语法，包括状态变量、函数、事件和映射（Mapping）。
    
-   完成 OnchainTodo 智能合约练习，熟悉智能合约开发与部署流程。
    
-   思考哪些应用场景更适合部署在 Monad 上。
    

### 产品调研

选择 **Meme Arena（链上 Meme PK 排行榜）** 作为高频交互 Demo。

原因：

-   用户会频繁进行投票、排行榜刷新和任务领取。
    
-   需要低延迟、低成本的链上交互体验。
    
-   链上记录投票、积分和奖励，保证数据公开透明。
    

### 技术方案

计划使用：

-   **Solidity**：开发智能合约
    
-   **Monad Testnet**：部署和测试合约
    
-   **React + Ethers.js/Viem**：实现钱包连接和前端交互
    

### 本周收获

1.  理解了 Monad 更适合高频交互型 Web3 应用，而不仅仅是“更快”。
    
2.  熟悉了 Solidity 智能合约开发的基本流程。
    
3.  学会从产品需求出发，思考哪些数据需要上链、哪些数据适合由数据库管理。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->













## **Day4 | Monad 高频交互应用设计：Meme Arena（链上 Meme 对战排行榜）**

### **1\. Research（约 300 字）**

随着 Consumer Crypto 的发展，越来越多用户参与 Meme 创作、社区投票和社交互动。这类产品最大的特点不是价值转账，而是大量、高频、低价值的交互：点赞、投票、助力、PK、排行榜刷新、任务打卡等都可能每分钟发生数百甚至数千次。

如果底层区块链确认速度慢或 Gas 成本较高，用户需要等待交易确认才能看到自己的投票、积分或排名变化，容易打断互动节奏。当一次点赞或投票都需要支付较高手续费时，用户也会倾向减少操作，社区活跃度和传播效果都会下降。因此，这类产品需要一条能够支持低延迟、高吞吐、低成本交易的链。

Monad 更适合这一场景，不只是因为 TPS 更高，而是因为它能够让每一次互动都接近实时反馈。用户完成投票、领取任务奖励或刷新排行榜后，可以快速看到链上状态更新，形成连续互动体验。同时，Monad 与 EVM 兼容，开发者能够直接使用 Solidity、Hardhat、Foundry 等成熟工具迁移以太坊生态项目，降低开发成本。

相比普通数据库，排行榜本身可以放在数据库缓存，但积分、投票记录、奖励领取、NFT Badge 和赛季结果更适合记录在链上。这样既保证数据公开透明、防止篡改，也方便不同应用共享同一套链上身份和荣誉体系，让用户的社区贡献成为真正可验证、可组合的链上资产。

### **2\. Tech：高频交互 Demo 功能清单**

**MVP 功能**

**① Meme PK Arena**

-   两张 Meme 卡片随机对战
    
-   用户点击支持其中一个 Meme
    
-   每次投票立即写入链上
    

**② 实时排行榜**

-   根据票数实时更新 Top 100
    
-   支持按小时、每天、每周赛季排行
    

**③ Daily Quest**

每日完成：

-   投票 10 次
    
-   分享 1 次
    
-   评论 3 次
    

完成后链上领取奖励

**④ On-chain Badge**

例如：

-   Early Voter
    
-   Top Contributor
    
-   Meme Creator
    
-   Season Champion
    

Badge 全部 NFT 化。

**⑤ XP & Level System**

每次交互获得 XP：

-   Vote +1 XP
    
-   Share +5 XP
    
-   Create Meme +20 XP
    

升级后解锁头像框、称号等奖励。

**⑥ Creator Reward Pool**

赛季结束：

-   Top Meme Creator
    
-   Top Voter
    
-   Top Community
    

自动按照智能合约分配奖励。

### **3\. Ops：3 个适合 Meme / 社区传播的产品切入点**

**① Vote to Pump**

每天举行 Meme Battle。

用户不断投票，把自己支持的 Meme 冲上榜首。

社区自然形成拉票传播。

传播口号：

“Vote your Meme to the Moon.”

**② Raid Mission**

社区发布任务：

-   投票
    
-   转发 Twitter/X
    
-   邀请好友
    
-   加入 Discord
    

完成任务即可获得链上积分和 Badge。

形成持续裂变增长。

**③ Season Championship**

每周开启一个新赛季：

-   新排行榜
    
-   新 Badge
    
-   新奖励池
    

赛季结束后永久记录：

-   Season #1 Champion
    
-   Season #2 Champion
    

所有荣誉保存在链上，可长期展示，增强用户参与感和社区竞争氛围。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->















## **Day 3 —— 编写并部署第一个 Solidity 智能合约（OnchainTodo）**

工具：Remix IDE、MetaMask、Monad Testnet、Solidity

### **一、学习目标**

本次学习的目标：

了解 Solidity 智能合约的基本结构

编写一个简单的 Todo List 合约

学习智能合约的读写函数（Read / Write Function）

使用 Remix 编译并部署合约

在测试网上验证合约并进行交互

### **二、实践内容**

本次实现了一个简单的 **OnchainTodo** 合约。

主要功能包括：

添加待办事项（Add Todo）

修改完成状态（Toggle Completed）

查询待办事项（Get Todo）

获取 Todo 数量（Get Todo Count）

代码中使用了以下 Solidity 基础知识：

Contract

Struct

Dynamic Array

State Variable

Event

Require

View Function

### **三、核心知识**

**1\. Contract**

Solidity 使用 contract 定义智能合约。

contract OnchainTodo {

}

可以理解为 Java 中的 Class，只不过部署后运行在区块链上。

**2\. Struct**

使用 Struct 定义 Todo 数据结构。

struct Todo {

string task;

bool completed;

}

每个 Todo 包含：

-   task：任务内容
    
-   completed：完成状态
    

**3\. Dynamic Array**

Todo\[\] private todos;

动态数组用于保存所有 Todo。

调用 addTodo() 时，会向数组末尾新增一个 Todo。

**4\. Read Function**

getTodo()

getTodoCount()

读取链上数据。

特点：

-   不修改区块链
    
-   使用 view
    
-   一般不消耗 Gas（本地调用）
    

**5\. Write Function**

addTodo()

toggleCompleted()

修改链上数据。

特点：

-   会产生交易（Transaction）
    
-   需要用户签名
    
-   消耗 Gas
    

**6\. Event**

event TodoAdded(...);

event TodoUpdated(...);

事件用于记录链上日志。

部署后，前端可以监听 Event 获取状态变化。

### **四、实践过程**

**1\. 编写合约**

创建文件：

OnchainTodo.sol

使用 Solidity 0.8.20 编写 Todo 合约。

2\. 编译合约

在 Remix 中：

-   Solidity Compiler
    
-   Compiler Version：0.8.x
    
-   Compile OnchainTodo.sol
    

编译成功，没有 Error。

**3\. 部署合约**

进入：

Deploy & Run Transactions

连接 MetaMask。

网络：

Monad Testnet

点击：

Deploy

钱包确认交易后，合约成功部署。

**4\. 调用 Write Function**

添加 Todo：

addTodo("Finish Assignment")

交易成功。

随后调用：

toggleCompleted(0)

成功修改完成状态。

**5\. 调用 Read Function**

查询 Todo：

getTodo(0)

返回：

task = Finish Assignment

completed = true

查询数量：

getTodoCount()

返回：

1

### **五、部署结果**

部署网络：

Monad Testnet

合约名称：

OnchainTodo

编译器：

solc 0.8.34

源码验证：

Sourcify Verified

说明部署的源码与链上的 Bytecode 完全一致，可以在区块链浏览器查看和验证。

### **六、学习收获**

通过本次实践，我了解了 Solidity 智能合约开发的基本流程：

-   编写 Solidity 合约。
    
-   使用 Remix 编译。
    
-   部署到测试网络。
    
-   调用写函数修改链上数据。
    
-   调用读函数读取链上数据。
    
-   完成源码验证，确保链上部署的代码可公开验证。
    

同时，也进一步理解了智能合约与传统程序的区别：智能合约部署到区块链后，其状态由链维护，写操作需要发起交易并支付 Gas，而读操作通常可以直接查询，不会改变链上状态。这种模式保证了数据的公开性、透明性和不可篡改性。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
















### **前置准备｜进入 Web3 与链上世界**

**Summary**

1.  创建或准备一个**课程专用钱包**，不要使用主力钱包。
    
2.  添加 Monad Testnet 测试网络。
    
3.  获取测试网资产，俗称“取水”。
    
4.  打开 Monad Explorer 或相关区块浏览器，可通过账户地址查询个人交易详情。
    

**Specific operations**

1.  拓展插件Metamask
    

2.  创建钱包
    

Metamask创建钱包，并记住私钥

3.  添加 Monad Testnet 测试网络
    

![bfae7d7454727fd78127d9b2e2b26ea8.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783429910381-bfae7d7454727fd78127d9b2e2b26ea8.png)

4.  获取测试币，俗称“取水”
    

![878fab6dac3d34dcc22dd41ff2003d54.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783429969137-878fab6dac3d34dcc22dd41ff2003d54.png)

**ps. 24h内每天上限50Mon**

5.  用 Monad Explorer 查询你的课程钱包地址
    

![d9c3b1d5a43fd8684d7fe5f8595c0568.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783430245323-d9c3b1d5a43fd8684d7fe5f8595c0568.png)![b8c56d1f57c0850d75af789fddb35f50.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783430196336-b8c56d1f57c0850d75af789fddb35f50.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

















**一、Day1整体学习主线**

1. Web3入门实操分三步：搭建Mod测试网钱包、链上取水获Mon测试币、AI生成部署智能合约。

2. 智能合约是链上不可篡改自动执行程序，留言板写入类操作消耗Gas，查询类操作免费。

**二、公私钥核心区别**

1. 私钥：掌控资产唯一凭证，必须保密，泄露资产会被盗。

2. 公钥：钱包公开地址，可对外分享，用于收款、查链上数据。

**三、Gas费相关要点**

1. Gas为链上操作燃料费，计算方式：Gas消耗量×油价。

2. 操作越复杂、网络越拥堵，Gas费用越高。

3. 合约留言写入消耗Gas，读取留言无Gas成本。

4. 每次留言是一笔上链交易，必耗Gas；留言文字越多，单笔Gas越高。

5. 合约Gas消耗可在Remix查看，受代码写法、链网络情况影响。

**四、Mod测试网钱包操作流程**

1. 火狐浏览器安装MetaMask，创建钱包妥善保管私钥。

2. 钱包添加Mod Test网络，复制公钥去水龙头领测试币，每日限一次。

3. 区块浏览器查询余额，即为钱包任务完成校验标准。

**五、链上交易要点**

1. 互转测试币后留存交易哈希，所有链上记录永久留存、不可篡改。

2. 区块浏览器可查看交易收发地址、金额、Gas消耗全部信息。

**六、留言板智能合约部署流程**

1. GPT生成Solidity合约代码，Remix粘贴自动编译。

2. Remix连接MetaMask测试网钱包，支付Gas完成合约部署。

3. 写入留言生成交易哈希可溯源；查询留言仅读取链上数据不扣费。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
