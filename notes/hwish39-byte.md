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
