---
timezone: UTC+8
---

# wish

**GitHub ID:** hwish39-byte

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
## **创建Builder Profile，寻找队友**
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->

## 参与产品分享会和co-learning
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->


## 今日学习内容完成情况

### 完成 Moss 开源项目学习与一次真实 GitHub 协作实践。

## 今日完成总结

1.  阅读并分析了 Moss 开源项目  
    了解 Moss 在 Monad 生态中的定位：它是一个面向 AI Agent 的链上安全执行框架，通过 `discover -> load -> action -> simulate` 流程帮助 Agent 构造和模拟未签名交易。
    
2.  输出 Moss 项目介绍  
    整理了 Moss 的项目背景、核心问题、安全模型、MCP Server、SDK 使用方式、应用场景和风险边界，保存到github仓库
    
3.  输出 Moss 新手教程  
    编写了一篇面向初学者的 Moss 入门教程，包含环境准备、运行示例、MCP 使用、源码阅读路线、常见问题和练习任务。
    
4.  推送学习仓库到 GitHub  
    将新增文档和文件命名调整提交并推送到自己的 GitHub 仓库 `hwish39-byte/web3-study-monad`。
    
5.  参与 Moss 开源 Issue  
    阅读 Moss Issue #97，理解其目标是补充“post-simulation intent-alignment checklist”，避免开发者误以为模拟成功就等于可以安全签名。
    
6.  学习并实践 GitHub 开源协作流程  
    完成 Fork、Clone、创建分支、修改文档、运行测试、提交、推送、创建 PR 的完整全流程。
    
7.  解决测试环境问题  
    运行 `pnpm test:offline` 时遇到 macOS 临时目录权限问题，通过设置：
    
    ```
    TMPDIR="$PWD/.tmp" pnpm test:offline
    ```
    
    成功通过测试。
    
8.  输出 GitHub 协作过程记录  
    将完成 Moss Issue #97 的完整过程整理成文档，保存到 `tasks/week2-general/task-github-collaboration-moss-issue.md`。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->



## 周末休息
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->




## **今日学习内容完成情况**

1.  整理 Week 2 所有学习、任务和反思
    
2.  将 AI Collaboration Log 纳入作品集
    
3.  为 Week 3 组队做好准备
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->





## **今日学习内容完成情况**

1.  推进原型开发：基于 Monad C API 的监控工具原型
    
2.  完成 Prototype Evidence：repo、README、截图、交易 hash、错误日志
    
3.  完善 AI Collaboration Log：记录 AI 协作全过程
    
4.  理解 Proof of Work 在区块链和实习计划中的双重含义
    

### **关于 "Proof of Work" 的思考**

今天的学习让我对 "Proof of Work" 有了更立体的理解。

在区块链语境下，**Proof of Work (PoW)** 是一种共识机制，矿工通过消耗算力解决密码学难题来竞争记账权，以此保障网络安全 。它解决了"在没有中心化权威的情况下，如何让互不信任的参与者就交易历史达成一致"的问题 。

而在实习计划中，**"Proof of Work" 强调的是"可检查的证据"**——不是口头说"我学会了"，而是通过可追溯的产出（repo、截图、交易哈希、错误日志）来证明学习真实发生。这与 Week 1 强调的"留下链上记录"一脉相承：在 Web3，**"不要相信，要验证"** 不仅是对区块链的要求，也是对 Builder 成长的要求。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->






## **今日学习内容完成情况**

### **Dev Builder 路径**

1.  学习 AI 开发工作流：从"命令"到"协作"的范式转变
    
2.  理解开源项目协作方式（README、Issues、PR、Code Structure）
    
3.  完成"文档到代码骨架"任务：用 AI 辅助阅读 Monad 文档并生成技术方案
    

### vibe coding小技巧

1.  **先规划，再执行：**先定义问题和规划解决方案。与 AI 共同构思一份详细的规格说明（[agent.md](http://spec.md)），再勾勒出分步计划，最后才开始编写实际代码。
    
2.  **将工作拆分为小型的迭代模块：**避免要求 AI 生成庞大、单一的输出。将项目拆分为迭代步骤或任务单（tickets），并逐一解决。每个模块都足够小，确保 AI 能在上下文窗口内处理，而人类也能看懂它生成的代码。
    
3.  **提供详尽的上下文和指导：**为避免ai产生幻觉，应该向ai展示详细的上下文、相关的代码、文档和约束条件。不要让 AI 在信息不全的情况下操作——如果修复一个 bug 需要理解四个不同的模块，那就把这四个模块都给它看。
    
4.  **选择合适的模型：**有可能需要解决的问题正好遇到了模型的盲区**，**不要钻牛角尖。
    
5.  **像合作者一样对待 AI：**提供具体指令、提供可验证的目标（配置好类型检查、linter 和单元测试）、认真审查代码、要求 AI 给出计划并解释其思路。
    

## **文档到代码骨架**

### **使用的文档：Monad Execution Events C API**

### **AI 辅助过程**

**第一步：让 AI 理解文档**

我的prompt：

```
"我正在阅读 Monad 的 Execution Events C API 文档。请帮我理解这个 API 的核心概念，并总结主要的数据结构、函数和典型使用流程。"
```

AI 的解读：

-   **核心对象**：`struct monad_event_ring`（表示事件环）和 `struct monad_event_iterator`（读取事件）
    
-   **关键操作**：通过 `monad_event_ring_init_iterator` 初始化迭代器，用 `monad_event_iterator_try_next` 逐个读取事件
    
-   **典型流程**：mmap 事件环文件 → 初始化迭代器 → 循环读取事件 → 处理事件负载
    

**第二步：生成代码骨架**

AI 生成了如下 C 代码骨架：

```
// SPDX-License-Identifier: MIT
#include <monad/event_ring.h>
#include <monad/event_iterator.h>
#include <monad/exec_event_ctypes.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char** argv) {
    // 1. 打开事件环文件
    const char* ring_path = argc > 1 ? argv[1] : "/var/run/monad/event_ring";
    int fd = open(ring_path, O_RDONLY);
    if (fd < 0) {
        perror("open");
        return 1;
    }
    
    // 2. 映射事件环
    struct monad_event_ring* ring = monad_event_ring_mmap(fd);
    if (!ring) {
        fprintf(stderr, "Failed to mmap event ring\n");
        close(fd);
        return 1;
    }
    
    // 3. 初始化迭代器
    struct monad_event_iterator iter;
    monad_event_ring_init_iterator(ring, &iter);
    
    // 4. 循环读取事件
    while (1) {
        struct monad_event_descriptor desc;
        if (monad_event_iterator_try_next(&iter, &desc) == 0) {
            // 处理事件
            printf("Event type: %d, sequence: %lu\n", 
                   desc.event_type, desc.sequence);
            
            // 根据事件类型处理负载
            if (desc.event_type == MONAD_EXEC_EVENT_BLOCK_START) {
                // 解析 BLOCK_START 事件
                // ...
            }
        }
        usleep(1000);  // 1ms 轮询
    }
    
    // 5. 清理
    monad_event_ring_unmap(ring);
    close(fd);
    return 0;
}
```

**第三步：核查与修改**

**我做了什么修改**：

1.  **添加错误处理**：补充了 `monad_event_ring_get_last_error()` 调用，获取详细的错误信息
    
2.  **添加负载解析**：根据 `enum monad_exec_event_type` 枚举，添加了对 `BLOCK_START`、`BLOCK_QC`、`BLOCK_FINALIZED` 等事件的解码逻辑
    
3.  **添加退出机制**：增加了信号处理（SIGINT）
    
4.  **修复内存泄漏**：添加了 `monad_event_ring_unmap` 调用
    

## **开源项目协作方式学习**

### **如何阅读开源项目**

1.  **README**：了解项目是做什么的、如何构建、如何使用
    
2.  **Issues**：观察社区在讨论什么问题，哪些是 bug，哪些是 feature request
    
3.  **Pull Requests**：学习代码如何被审核、修改、合并
    
4.  **Code Structure**：理解模块划分、依赖关系
    
5.  **Contributor Guide**：了解贡献流程、编码规范、测试要求
    

### **开源协作的基本流程**

1.  Fork 项目仓库
    
2.  在本地开发分支上修改
    
3.  提交 Pull Request
    
4.  代码审查（可能多轮迭代）
    
5.  被合并到主分支
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->







## 听分享会，构思黑客松项目idea
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->








## **今日学习内容完成情况**

1.  理解 Research / Ops / Dev 三类角色的核心职责与解决问题的方式
    
2.  完成 Role Choice Card，确定主职业方向为 **Dev Builder**
    
3.  建立 Week 2 Role Log，规划本周学习路径
    

### **三类角色协作方式**

| 角色 | 核心问题 | 典型工作 | 思维方式 |
| --- | --- | --- | --- |
| research | 发现什么是对的 | 协议研究、生态分析、治理提案、技术写作 | 深度分析、系统性思考、好奇心驱动 |
| ops | 让更多人知道并参与 | 社区运营、活动策划（Space/Hackathon）、内容传播、用户增长 | 共情能力、节奏感、情绪管理 |
| dev | 把它做出来 | 合约开发、Dapp 构建、工具脚本、AI Agent | 逻辑思维、工程能力、快速迭代 |

### 三者并非割裂，而是互相依赖。Dev 需要 Research 提供方向，Ops 需要 Dev 的产品作为社区支点，Research 需要 Ops 的反馈来验证假设。

### **我选择的方向：Dev Builder（开发方向）**
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->









## 周末休息（ ^\_^ ）
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->










## 周末休息(^\_^)
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->











## **今日学习内容完成情况**

### **Monad 文档学习**

1.  理解 Monad 与 Ethereum 的核心差异
    
2.  学习高性能应用的最佳实践
    
3.  了解 Monad 的 Gas 定价机制
    

### **Web3 实习手册**

1.  阅读行业赛道全览（DeFi、NFT、DAO、MEME）
    
2.  浏览社区运营指南
    
3.  回顾岗位全景图，为选择方向做准备
    

## **Monad 核心理解**

### **Monad 不只是"更快" —— 而是重新定义可能**

Monad 是一条 Layer-1 区块链，核心目标是 **消除去中心化与性能之间的权衡**。它通过软件架构创新（而非依赖昂贵硬件）实现高性能，让去中心化网络真正具备支持高频交互应用的能力。

### **Monad 与 Ethereum 的关键差异**

| 维度 | ethereum | monad | 对开发者的影响 |
| --- | --- | --- | --- |
| 合约代码大小限制 | 24k | 128k | 可部署更复杂的合约逻辑 |
| Gas 收费方式 | 按实际用量 | 按gas limit | 需注意设置合理的 Gas Limit，防止多付 |
| 交易池 (Mempool) | 全球统一 | 无全局 Mempool | 直接发给未来领导者 |
| Base Fee 调整 | EIP-1559 标准 | 更慢上升、更快下降 | 减少因 Base Fee 过高导致的区块空间浪费 |
| EIP-4844 (Blob) 交易 | 支持 | 不支持 | Layer 2 数据可用性方案不同 |
| P256预编译 | 不支持 | 支持 | 支持 WebAuthn/Passkey 链上验证 |

### **Gas 定价机制核心要点**

1.  **按 Gas Limit 收费**（而非 Gas Used）
    
2.  **EIP-1559 兼容**
    
3.  **Base Fee 控制器优化**
    
4.  **开发建议**：显式设置 Gas Limit、并发请求、使用索引器等。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->












## **今日学习内容完成情况**

### **Web3 实习手册｜智能合约开发**

1.  理解 Dapp 架构（前端 + 智能合约 + 检索器 + 区块链）
    
2.  学习 Dapp 开发流程（需求分析 → 合约开发 → 检索器 → 前端 → 部署）
    
3.  了解开发环境搭建（Foundry、Hardhat、Remix）
    
4.  理解 RPC 节点服务的作用与配置
    
5.  学习 Solidity 基础语法（数据类型、函数修饰符、合约结构）
    

### **Solidity 官方文档**

1.  了解 Solidity 语言特点（面向对象、静态类型、继承）
    
2.  浏览语言基础部分，为后续深入学习打基础
    

### **OpenZeppelin Contracts**

1.  了解 OpenZeppelin 合约库的作用（标准化、安全、可复用）
    
2.  浏览常用合约模块（ERC20、ERC721、访问控制、治理等）
    

## **核心概念学习笔记**

### **Dapp 架构图**

```
用户
  ↓
前端 (React/Vue) ←→ 钱包 (MetaMask)
  ↓                           ↓
检索器 (Indexer) ←→ 智能合约 (Solidity)
  ↓                           ↓
数据库 (PostgreSQL) ←→ 区块链 (Ethereum/Monad)
```

### **合约源码 → 部署 → 交互的完整链路**

```
1. 编写合约源码 (.sol)
   ↓
2. 编译 → ABI (Application Binary Interface) + Bytecode
   ↓
3. 部署 → 获得 合约地址 (Contract Address)
   ↓
4. 交互
   ├── 读取 (Read) → 调用 view/pure 函数 → 不需要 Gas
   └── 写入 (Write) → 调用修改状态的函数 → 需要 Gas → 产生交易哈希
```

### **Solidity 合约源码**

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Gmonad {
    // 状态变量：存储在链上
    string public greeting;

    // 构造函数：部署时执行一次
    constructor(string memory _greeting) {
        greeting = _greeting;
    }

    // 写入函数：修改状态，消耗 Gas
    function setGreeting(string calldata _greeting) external {
        greeting = _greeting;
    }

    // 读取函数：view 修饰，不消耗 Gas（自动生成，因为 greeting 是 public）
    // function greeting() public view returns (string memory)
}
```
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->













## **今日学习内容完成情况**

1.  **Web3 实习手册｜安全与合规**
    
2.  **Ethereum 基础知识**
    
3.  **Monad 测试网文档**
    
4.  **实操内容（安装钱包、添加monad网络、领取测试币、完成转账、查看交易哈希）**
    

## **安全与合规 —— 关键要点**

### **法律风险红线**

| 风险领域 | 典型案例 | 后果 |
| --- | --- | --- |
| 代币发行/融资 | ICO、IEO、IDO | 非法吸收公众存款罪 |
| 链游/赌博机制 | "充值-抽奖-提现"闭环 | 开设赌场罪 |
| 多级返佣 | 邀请返利、团队计酬 | 组织、领导传销活动罪 |
| 场外交易出金 | 接收涉诈资金 | 洗钱罪、帮信罪 |
| 跨境换汇 | 以虚拟货币为媒介兑换外币 | 非法经营罪 |

### **交易 (Transaction) 的核心字段**

| 字段 | 说明 |
| --- | --- |
| from | 发送方地址（由你签名授权） |
| to | 接收方地址（普通账户或合约地址） |
| value | 发送的 ETH/MON 数量（单位：WEI） |
| nonce | 交易序号，防止重复 |
| gasLimit | 最大计算单位，普通转账 21,000 |
| maxFeePerGas | 你愿意支付的每单位最高费用 |
| maxPriorityFeePerGas | 给验证者的小费 |

### **Gas 费用计算**

```
总费用 = 实际用量 × (Base Fee + Priority Fee)

示例：
- 普通转账：21,000 单位
- Base Fee：10 gwei
- Priority Fee：2 gwei
- 总费用 = 21,000 × 12 = 252,000 gwei = 0.000252 MON

注：Base Fee 会被销毁（通缩机制），Priority Fee 归验证者
```

### **交易生命周期**

1.  **生成哈希**：`0x97d99bc...`（交易唯一标识符）
    
2.  **广播到网络**：进入交易池（Mempool）
    
3.  **验证者打包**：被选中进入区块
    
4.  **区块确认**：区块被添加（"Justified"）
    
5.  **最终确定**："Finalized"，几乎不可逆转
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->














## **今日学习内容完成情况**

1.  **Web3 实习手册｜Web3 工作方式**
    
2.  **Web3 实习手册｜岗位全景图**
    
3.  **工具安装与账号准备**
    

## **关键概念学习笔记**

### **Web3 岗位全景图速览**

技术岗

| 岗位 | 核心职责 | 常用技术栈 |
| --- | --- | --- |
| 前端工程师 | Dapp 界面开发、钱包连接、交易签名 | React、Vue、TypeScript、Viem、Next.js |
| 后端工程师 | 链上数据交互、API 设计、系统架构 | Node.js、Go、Python、GraphQL、Docker |
| 智能合约工程师 | 合约设计、开发、部署、安全审计 | Solidity、Foundry、Hardhat、Remix |
| 合约审计工程师 | 漏洞识别、安全报告、威胁建模 | Slither、Echidna、Mythril、Tenderly |

非技术岗

| 岗位 | 核心职责 | 常用平台/工具 |
| --- | --- | --- |
| 产品运营 | 产品发布、用户增长、数据分析 | SQL、Excel、Notion |
| 社区管理 | 社群运营、活动策划、用户互动 | Telegram、Twitter、Discord |
| 研究分析 | 行业研究、经济模型设计、竞品分析 | Glassnode、Token Terminal、Python |

## **今日学习思考：**

### 通过今天的岗位学习，我对 "DevRel" 这个角色有了更立体的认识：

**DevRel（开发者关系）** 不是一个纯技术岗位，也不是纯运营岗位，而是一个 **"技术 + 社区 + 内容"** 的复合型角色。它的核心是 **赋能开发者**，让更多人基于项目技术栈进行构建。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->















## **今日学习内容完成情况**

1.  **Web3 实习手册｜入门导读**
    
2.  **区块链基础概念**
    
3.  **以太坊概览**
    

## 关键概念学习笔记

### 区块链核心概念

| 特性 | 说明 |
| --- | --- |
| 不可篡改 | 每个区块包含前一个区块的哈希，修改历史数据需要修改后续所有区块 |
| 公开透明 | 所有交易记录公开可查，但身份匿名 |
| 去中心化 | 全球节点共同维护网络，无单一控制者 |
| 快速交易 | 不受地域和金额限制，跨国转账便捷 |

### 区块链类型对比

| 类型 | 加入方式 | 数据可见性 | 管理模式 | 适合场景 |
| --- | --- | --- | --- | --- |
| 公链 | 任何人自由加入 | 所有人可见 | 去中心化（投票） | 加密货币、公共存证 |
| 联盟链 | 需联盟成员邀请 | 仅联盟成员可见 | 多中心化（董事会） | 供应链、金融协作 |
| 私链 | 严格审批 | 仅内部可见 | 中心化（老板决定） | 企业内部管理 |

### 以太坊三大核心机制

1.  **账户系统**：外部账户（EOA，私钥控制）+ 合约账户（CA，代码驱动）
    
2.  **Gas 模型**：Gas 费用 = Gas Limit × Gas Price，EIP-1559 后拆分基础费用和小费
    
3.  **EVM（以太坊虚拟机）**：全球共享的分布式计算机，运行智能合约
    

## **今日学习思考与感悟**

### 通过今天的学习，我意识到 Web3 不仅仅是技术变革，更是一场关于 **信任和所有权** 的革命。以下是我被打动的几个点：

1.  **真正拥有数字资产**：你的 NFT、游戏道具真正属于你，平台无法随意删除或收回。
    
2.  **金融服务无门槛**：无需银行卡，用手机钱包就能借贷、理财、交易。
    
3.  **应用可自由组合**：就像搭积木一样，一个 DeFi 协议的流动性可以被其他应用直接调用。
    
4.  **内容永不消失**：文章、图片存储在分布式网络，不会因为平台关闭而丢失。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
