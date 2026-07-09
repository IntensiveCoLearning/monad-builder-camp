---
timezone: UTC+8
---

# Ttttt7777

**GitHub ID:** Ttttt7777

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 手册学习

## 区块链岗位全景图

Web3 催生出一系列新岗位形态，既包括技术端的智能合约工程师，也涵盖了产品、运营、社区管理等多种非技术职能。求职的第一性原理：从 **JD（Job Description）** 开始了解要求。

* * *

## 一、技术岗

### 1\. 前端工程师

**核心职责**：

-   Dapp 前端界面开发，钱包连接、交易签名
    
-   集成智能合约，链上与用户界面的无缝连接
    

**技术栈**：

```
HTML5 / CSS3 / JavaScript (ES6+)
React / Vue / TypeScript / Next.js
Viem / Ethers.js / Web3.js
```

**职位要求**：本科及以上，了解 L1/L2 网络，有 Dapp 经验优先。

### 2\. 后端工程师

**核心职责**：

-   后端服务开发，链上数据交互，API 构建
    
-   与合约团队协作，优化链上数据读写
    

**技术栈**：

```
Node.js / Go / Python
Viem / Web3.js / Ethers.js
RESTful API / GraphQL
MySQL / PostgreSQL / Docker / Kubernetes
```

### 3\. 智能合约工程师

**核心职责**：

-   设计、开发、部署智能合约
    
-   编写测试，进行安全审计与 Gas 优化
    

**技术栈**：

```
Solidity
Remix / Foundry / Hardhat
Phalcon / Tenderly / Yul
```

**职位要求**：3 年以上 Solidity 经验，熟悉 ERC-20/721/1155，了解 Reentrancy/Overflow 等攻击模式。

### 4\. 合约审计工程师

**核心职责**：

-   审查合约逻辑、权限控制、资产流向
    
-   输出结构化审计报告
    

**技术栈**：

```
Solidity / Foundry / Hardhat
Slither / Echidna / Mythril
Tenderly / Phalcon
```

* * *

## 二、非技术岗

### 1\. 产品与运营

**职责**：产品生命周期协调、增长战略、数据分析  
**技能**：Go-to-market 全流程、SQL/Excel 数据分析

### 2\. 社区管理

**职责**：社群建设、AMA 活动、社区健康度追踪  
**平台**：Telegram, Twitter (X), Discord  
**技能**：文案撰写、活动策划、数据分析

### 3\. 研究分析

**职责**：行业数据分析、深度研究报告、Tokenomics 分析  
**工具**：Excel, SPSS, Python, Glassnode, Token Terminal  
**技能**：定性定量研究、链上数据分析、报告撰写

* * *

## 三、求职平台推荐

| 平台 | 特点 | 语言 |
| --- | --- | --- |
| DeJob (dejob.ai) | 大量高频工作机会 | 中文 |
| SmartDeer | 综合性招聘平台 | 中文 |
| Web3Career | 专业 Web3 招聘 | 英文 |
| CryptoJobs | 区块链行业招聘 | 英文 |
| ETHPanda 社区 | 内推与 Bounty 机会 | 中文 |
| LXDAO 社区 | Bounty 岗位 | 中文 |
| LinkedIn | 成熟公司职位 | 英文 |

* * *

## 四、项目可靠性 12 项评分表

评估项目方是否可靠（满分 **120 分**，低于 **72 分**需谨慎）：

| # | 评估项 | 分值 |
| --- | --- | --- |
| 1 | 项目愿景（做什么、如何实现） | 0-10 |
| 2 | 项目声誉（负面舆情情况） | 0-10 |
| 3 | 创始人背景（可查 rootdata.com） | 0-10 |
| 4 | 是否上市及交易所层次 | 0-10 |
| 5 | 办公地点 | 0-10 |
| 6 | LinkedIn 可检索 | 0-10 |
| 7 | 总市值排名 | 0-10 |
| 8 | 创始年份 | 0-10 |
| 9 | 合规性 | 0-10 |
| 10 | 价值观匹配 | 0-10 |
| 11 | 投资机构支持 | 0-10 |
| 12 | 社区运营情况 | 0-10 |

> 拿不定主意时，可在 **LXDAO 或 ETHPanda 社区**中问询。

* * *

## 五、识别招聘骗局

### 常见骗局特征

1.  🚩 要求**先付费**（培训费/工具费/押金）
    
2.  🚩 要求安装**不明软件或插件**
    
3.  🚩 公司无法核验：官网、LinkedIn、GitHub 缺失
    
4.  🚩 岗位描述极宽泛，承诺高回报
    
5.  🚩 使用**非官方邮箱**沟通（TG/WhatsApp/私人邮箱）
    

### 自我保护清单

-   ✅ 核验主体：官网、LinkedIn、GitHub、工商信息
    
-   ✅ 拒绝付费招聘
    
-   ✅ 只使用官方渠道沟通
    
-   ✅ 不运行未知程序
    
-   ✅ 发现骗局立即停止沟通并保留证据
    

## 智能合约开发

## 一、Dapp 架构与开发流程

### Dapp 架构

Dapp 由三大核心部分组成：

```
┌──────────────────────────────────────────────┐
│  前端 (React/Vue) ←→ 钱包 (MetaMask)        │
│      ↓                                       │
│  智能合约 (Solidity → EVM)                   │
│      ↓                                       │
│  数据检索器 (Indexer) → 数据库 (PostgreSQL)  │
│      ↓                                       │
│  区块链 + IPFS/Arweave                       │
└──────────────────────────────────────────────┘
```

### 开发流程

1.  **需求分析与规划** → 2. **智能合约开发** → 3. **检索器开发** → 4. **前端开发** → 5. **部署上线**
    

* * *

## 二、开发环境搭建

### 基础环境

```bash
# 安装 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
nvm install --lts

# 安装 Foundry
curl -L https://foundry.paradigm.xyz | bash
foundryup
```

### 推荐框架

| 框架 | 特点 | 核心工具 |
| --- | --- | --- |
| Foundry | Rust 实现，极快 | forge(构建测试) + anvil(本地节点) + cast(链上交互) |
| Hardhat | 现代框架，插件丰富 | TypeScript 支持，npx hardhat |

### 常用工具链

-   **Remix IDE**：[https://remix.ethereum.org（在线快速测试）](https://remix.ethereum.org（在线快速测试）)
    
-   **OpenZeppelin 合约库**：`npm install @openzeppelin/contracts`
    

* * *

## 三、RPC 节点服务

### RPC 是什么？

RPC（Remote Procedure Call）是连接前端应用与区块链网络的桥梁。好比 **银行 ATM 机**——查询余额（读取）、转账（发送交易）。

### 常用 JSON-RPC 方法

| 方法 | 功能 |
| --- | --- |
| eth_getBalance | 查询账户余额 |
| eth_blockNumber | 获取最新区块号 |
| eth_sendTransaction | 发送交易 |
| eth_call | 调用合约（只读） |
| eth_getLogs | 查询事件日志 |

### 主流 RPC 服务商

| 服务商 | 特点 | 免费额度 |
| --- | --- | --- |
| Alchemy | 企业级，文档完善 | 每月 3 亿次 |
| Infura | 老牌，ConsenSys 旗下 | 每月 10 万次 |
| QuickNode | 高性能，低延迟 | 有限免费 |
| Public Node | 完全免费免注册 | 有速率限制 |
| Ankr | 多链支持 | 有限免费 |

### 代码中使用 RPC

```javascript
// Viem (推荐)
import { createPublicClient, http } from 'viem'
import { mainnet } from 'viem/chains'

const client = createPublicClient({
  chain: mainnet,
  transport: http('https://eth-mainnet.g.alchemy.com/v2/YOUR_API_KEY')
})
const balance = await client.getBalance({ address: '0xd8dA6BF2...' })
```

* * *

## 四、Solidity 基础语法

### 数据类型

| 类型 | 描述 | 示例 |
| --- | --- | --- |
| bool | 布尔值 | true/false |
| uint256 | 256 位无符号整数 | 0~(2^256-1) |
| address | 以太坊地址 | 0x... |
| bytes32 | 固定长度字节 | 0x00 |
| string | UTF-8 字符串 | "Hello" |

### 复合类型

```solidity
mapping(address => uint256) public balances;
struct Person { string name; uint age; }
enum Status { Pending, Active, Inactive }
```

### 函数修饰符

**可见性**：`public`（内外均可） | `external`（仅外部） | `internal`（本合约+子合约） | `private`（仅本合约）

**状态修改**：`pure`（不读不写） | `view`（只读） | `payable`（可收 ETH） | 无修饰符（可读写）

### 完整合约示例

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MessageBoard {
    mapping(address => string[]) public messages;
    event NewMessage(address indexed sender, string message);

    constructor() {
        messages[msg.sender].push("Hello ETH Pandas");
        emit NewMessage(msg.sender, "Hello ETH Pandas");
    }

    function leaveMessage(string memory _msg) public {
        messages[msg.sender].push(_msg);
        emit NewMessage(msg.sender, _msg);
    }

    function getMessage(address user, uint256 index) public view returns (string memory) {
        return messages[user][index];
    }
}
```

* * *

## 五、安全实践 ⭐

| 风险 | 攻击机理 | 防护措施 |
| --- | --- | --- |
| 重入攻击 | 回调中重复提款 | CEI 模式（先改状态后转账）、ReentrancyGuard |
| 访问控制 | 未保护的管理函数 | Ownable、AccessControl、多签 |
| 整数溢出 | 加减法越界 | Solidity 0.8+ 内置检查 |
| 预言机操纵 | 操纵喂价 | Chainlink、TWAP、多源验证 |

### CEI 模式示例

```solidity
// ❌ 危险：先转账后更新状态
function withdraw() external {
    uint256 amount = balances[msg.sender];
    (bool success,) = msg.sender.call{value: amount}("");  // 此处可被重入
    balances[msg.sender] = 0;
}

// ✅ 安全：先更新状态后转账 (CEI)
function withdraw() external {
    uint256 amount = balances[msg.sender];
    balances[msg.sender] = 0;                              // 先更新
    (bool success,) = msg.sender.call{value: amount}("");  // 后转账
}
```

* * *

## 六、合约审计

### 审计流程

1.  **静态分析**（Slither、Mythril）
    
2.  **动态测试**（Fuzzing/Property Testing）
    
3.  **人工审查**（资深审计员）
    
4.  **审计报告生成**
    

### 知名审计机构

| 机构 | 特点 |
| --- | --- |
| 慢雾科技 | 国内领先，攻击复现 |
| OpenZeppelin | 社区信赖，基础库作者 |
| ConsenSys Diligence | 精通以太坊底层 |

* * *

## 七、开发协作规范

**Git 分支模型**：`main`(可部署) → `develop`(开发) → `feature/xxx`(功能) → `fix/xxx`(修复)  
**提交信息**：`feat:` / `fix:` / `docs:` / `refactor:` / `test:` / `chore:`  
**PR 流程**：每个功能新建分支 → 通过测试 → 至少一位 reviewer → 合并

* * *

## 八、Layer 2 开发

| 平台 | 类型 | 语言 | 特点 |
| --- | --- | --- | --- |
| Starknet | ZK-Rollup | Cairo | 高扩展 |
| zkSync | ZK-Rollup | Solidity | 开发体验佳 |
| Arbitrum | Optimistic | Solidity | 兼容 Hardhat/Foundry |
| Base | Optimistic(OP Stack) | Solidity | Coinbase 支持 |

## 7.09会议 从「裸奔」到「受控」：AI Agent 高危案例解析与安全构建

AI Agent 正从“信息应答者”蜕变为“行动执行者”——它能够调用数字钱包、发起链上交易、管理资金、对接 API，并串联多种工具与服务。这种自主能力的跃升大幅提升了生产效率，但也将安全风险从“数据泄露”升级为“资产损失与系统操控”，影响层面截然不同。

当前，AI Agent 面临的安全威胁主要集中在以下三类：

外部指令劫持（Prompt Injection）：攻击者将恶意指令隐藏于网页、文档或外部内容中，诱导 Agent 绕过系统预设规则，擅自执行转账、泄露敏感信息或调用危险工具。

内部权限失控：一旦 Agent 持有钱包私钥、API 密钥或高权限系统账号，被恶意利用后其破坏力将远超传统聊天误差，可能直接导致资金盗取或业务瘫痪。

基础组件脆弱性：私钥明文存储、Tool Call 参数校验不足、MCP 等插件接口的安全缺陷，均为攻击者提供可乘之机，亟需开发者重点排查。

因此，AI Agent 的安全底座不应是“盲目信任”，而应是“边界治理”。可行策略包括：恪守最小权限，仅开放任务必需的权限；高风险操作人工介入，对转账、删除、授权等动作强制加入审批或二次确认；密钥物理隔离，禁止 Agent 以明文形式访问私钥，改用硬件安全模块或密钥管理服务；全链路可观测，对每次 Tool Call 和插件调用实施日志审计、异常检测和频率限制。最终，AI Agent 越像一个真实员工，就越需要像管理员工一样，为其划定明确的职责边界、操作流程和合规检查，这才是安全落地的根本逻辑。

## 7.09 CO-Learning

现在的招聘开始要求开发者不仅要懂智能合约，还要懂如何将 AI Agent 接入链上操作（如 MCP 协议、Tool Call 安全沙盒），这种 “区块链 + AI”的复合型岗位要求极高，但市场上的对口人才极少，导致“企业招不到，求职者够不着”的结构性错配。

远程招聘为主流，求职者很难实地考察公司。大量项目方并没有真实的商业模式，只是靠叙事融资。求职者入职后可能发现：

1.工资由“稳定币 + 项目代币”组成，代币部分随市场暴跌归零。

2.项目方用高薪吸引人才完成开发后，突然停止运营（软 Rug），甚至拖欠工资。

3.背调极其困难，很多 Web3 项目的创始人匿名，过往履历无法核实，求职者踩雷概率远高于传统行业。

目前找工作最效率的方式是内推.
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->

# 今日学习

### 主要利用了 AI 辅助理解代码，逐步提升自己阅读与编写合约的能力。

* * *

## 1\. SPDX许可证

```solidity
// SPDX-License-Identifier: MIT
```

**作用：**

告诉编译器，这份代码采用 **MIT 开源许可证**。

这是 Solidity 官方推荐写法，没有这一行 Remix 会出现警告，但不会影响部署。

* * *

## 2\. 指定编译器版本

```solidity
pragma solidity ^0.8.26;
```

**作用：**

告诉编译器：

> 使用 **0.8.26及以上（但低于0.9.0）** 的 Solidity 编译。

其中：

-   `pragma`：告诉编译器配置
    
-   `^`：表示兼容当前大版本
    
-   `0.8.26`：最低版本
    

* * *

## 3\. 定义合约

```solidity
contract SimpleCheckIn {
```

相当于 Java 的

```java
class SimpleCheckIn{
}
```

或者 C++ 的

```cpp
class SimpleCheckIn{
};
```

这里定义了一个智能合约，名字叫

> **SimpleCheckIn**

部署以后，这就是链上的一个程序。

* * *

# 4\. 定义用户数据结构

```solidity
struct UserInfo {
```

`struct` 的意思就是

> **结构体**

用于把多个变量组合在一起。

里面包含：

```solidity
uint256 lastTime;
```

表示

> 最近一次签到时间（时间戳）。

例如：

```
1720000000
```

代表某一天的 Unix 时间。

* * *

第二个变量：

```solidity
uint256 count;
```

表示

> 用户累计签到多少次。

例如：

```
第一次签到
count = 1

第二次
count = 2

第三次
count = 3
```

整个结构体就是

```solidity
UserInfo

├── lastTime
└── count
```

每一个用户都有一份这样的数据。

* * *

# 5\. mapping

```solidity
mapping(address => UserInfo) public users;
```

这是 Solidity 最重要的数据结构之一。

意思是：

```
钱包地址
      │
      ▼
UserInfo
```

例如

```
0x111...

↓

{
lastTime: 1720000000
count: 5
}
```

再例如

```
0x222...

↓

{
lastTime: 1721000000
count: 2
}
```

可以理解成 Java 的

```java
HashMap<Address, UserInfo>
```

或者 Python 的

```python
dict[address] = UserInfo
```

* * *

# 6\. 定义24小时

```solidity
uint256 public constant COOLDOWN = 24 hours;
```

Solidity 内置时间单位。

这里

```
24 hours
```

自动转换成

```
86400 秒
```

因此

```
COOLDOWN

=

86400
```

以后代码不用写

```
86400
```

直接写

```
COOLDOWN
```

更容易理解。

* * *

# 7\. 定义事件

```solidity
event CheckedIn(
    address indexed user,
    uint256 time,
    uint256 count
);
```

事件(Event)不会保存到 storage。

作用只有一个：

> 告诉链外程序：

"刚刚有人签到成功了！"

比如前端网页可以监听：

```
CheckedIn
```

然后自动刷新页面。

里面包含：

```
用户地址

签到时间

签到次数
```

* * *

# 8\. checkIn()

```solidity
function checkIn() external {
```

定义一个函数。

名字：

```
checkIn
```

任何人都可以调用。

* * *

## 第一行

```solidity
UserInfo storage user = users[msg.sender];
```

这一句非常重要。

### msg.sender

代表：

> 当前调用者的钱包地址。

例如

```
钱包A

调用
checkIn()

那么

msg.sender

=

钱包A
```

如果

钱包B调用

那么

```
msg.sender

=

钱包B
```

* * *

### users\[msg.sender\]

从 mapping 中找到自己的数据。

例如：

```
users

↓

0x123

↓

UserInfo
```

* * *

### storage

表示：

不是复制数据。

而是直接操作区块链里的原始数据。

如果写

```solidity
storage
```

修改

```
user.count++
```

链上的数据真的会改变。

* * *

# 9\. require

```solidity
require(
    block.timestamp >= user.lastTime + COOLDOWN,
    "Already checked in"
);
```

这是整个合约最关键的一句。

先看

```
block.timestamp
```

表示

> 当前区块时间。

例如：

```
今天

1720000000
```

* * *

再看

```
user.lastTime
```

例如

```
昨天

1719913600
```

加上

```
COOLDOWN

86400
```

得到

```
今天
```

于是判断

```
当前时间

>=

上次签到+24小时
```

如果成立

允许签到。

否则

直接报错：

```
Already checked in
```

交易回滚。

* * *

# 10\. 更新时间

```solidity
user.lastTime = block.timestamp;
```

保存

当前签到时间。

以后判断24小时就是依据它。

* * *

# 11\. 次数+1

```solidity
user.count += 1;
```

例如

原来

```
count

=

5
```

执行以后

```
count

=

6
```

由于 Solidity 0.8+ 默认启用了整数溢出检查，因此无需额外使用 `SafeMath`。

* * *

# 12\. emit

```solidity
emit CheckedIn(
    msg.sender,
    block.timestamp,
    user.count
);
```

触发事件。

告诉链外：

```
某用户

签到成功

签到次数

6
```

前端可以监听这个事件更新界面。

* * *

# 13\. 查询函数

```solidity
function getRecord(address user)
```

这是查询接口。

传入一个钱包地址。

例如

```
0x123...
```

* * *

函数声明：

```solidity
external
view
```

表示：

-   `external`：只能从合约外部调用。
    
-   `view`：只读取数据，不会修改区块链状态，因此调用通常不消耗 Gas（通过 RPC 查询）。
    

* * *

返回值：

```solidity
returns(
    uint256 lastTime,
    uint256 count
)
```

表示返回两个数据：

```
最近签到时间

签到次数
```

* * *

函数内部：

```solidity
UserInfo memory info = users[user];
```

这里使用 `memory`。

意思是：

> 把链上的数据复制到内存里读取，不会修改原始数据。

因为这里只查询，不需要 `storage`。

* * *

最后：

```solidity
return(
    info.lastTime,
    info.count
);
```

把数据返回给调用者。

例如：

```
lastTime

1720000000

count

5
```

* * *

## 整个执行流程

```
用户点击签到
        │
        ▼
checkIn()
        │
        ▼
读取自己的签到记录
        │
        ▼
距离上次是否超过24小时？
      │         │
      │否       │是
      ▼         ▼
交易失败     更新时间
Already...      │
                ▼
          次数 +1
                │
                ▼
          触发签到事件
                │
                ▼
            交易成功
```

这份代码虽然只有几十行，却涵盖了 Solidity 中最常见的几个核心概念：`struct`（结构体）、`mapping`（映射）、`storage` 与 `memory` 的区别、`msg.sender`、`block.timestamp`、`require` 条件校验、事件（`event`/`emit`）以及状态变量更新。掌握了这些内容，就已经具备了阅读和编写许多基础智能合约的能力。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->


# Solidity合约实践

## 打卡合约

### 首先使用了豆包查询了相关信息与生成了一个 Prompt

**打卡合约（Attendance/Check-in 智能合约）**

**一、基础定义**

打卡合约是专门记录链上签到行为的智能合约，属于最基础的链上行为存证合约，和你之前问的 Onchain Todo 同属新手入门级项目，部署在 EVM 公链（以太坊、Monad、Polygon 等）。

核心作用：用代码固化签到规则，自动记录钱包地址、打卡时间，防重复签到、自动发奖励/勋章，全程无后台人工干预。

简单理解：链上版签到系统，代码=考勤规则。

**二、核心底层逻辑**（代码层面）

**1. 数据存储**

用  mapping(address => 打卡记录数组)  绑定每个钱包，永久保存每一次打卡时间戳、连续打卡天数、领取奖励记录，数据写在区块账本不可篡改。

**2. 核心校验规则（合约自带防作弊）**

\- 时间锁：限制24小时仅能打卡1次，杜绝批量刷签到；

\- 重复校验：同一地址当日重复调用直接拦截；

\- 连续打卡统计：自动计算签到断签、连续天数（签到断一天清零）；

\- 权限控制：仅合约部署者可修改奖励参数，普通用户仅能打卡。

**3. 核心函数**

\-  checkIn() ：用户连接钱包发起打卡主函数；

\-  claimReward() ：完成签到后自动领取代币/NFT奖励；

\-  getUserRecord(address) ：任何人可查询某钱包全部打卡历史；

\-  resetReward() ：管理员调整每日奖励数量。

**三、四大主流应用场景（Web3最常用）**

**1\. DAO/项目社区日常签到（最普遍）**

项目用来激活社区活跃度，用户每日打卡累积积分，兑换治理权重、空投、白名单。

\- 优势：所有用户活跃度公开可查，无法后台篡改数据，杜绝刷量造假。

**2\. 活动出席存证（POAP底层逻辑）**

线下峰会、线上AMA、黑客松签到，打卡合约铸造SBT/POAP纪念NFT，作为链上参与凭证，后续可凭勋章解锁专属权益。

**3\. 任务激励/链上Quest平台**

Monad、各公链测试网新手任务标配：每日交互、每日签到领测试币、积分。你做Monad Builder Camp开发入门，第一个练手项目就是打卡合约。

**4\. 自律/行为约束合约（小众）**

健身、学习打卡质押合约：用户质押代币，连续打卡全额取回；断签自动扣除质押金分给持续打卡用户，自动强制执行，无人工仲裁。

**四、两种主流打卡合约类型**

**1\. 纯存证型（无奖励）**

只记录打卡时间，不发放代币/NFT，多用于企业链上考勤、DAO出勤审计，Gas消耗极低。

**2\. 激励型（带经济模型）**

打卡自动发放代币、积分、灵魂绑定NFT（SBT），内置国库池，每日奖励额度由合约总量控制，适合社区运营。

**五、优缺点**

**优点**

1. 完全去中心化：没有平台后台删除、篡改签到记录；

2. 公开可审计：区块浏览器输入钱包地址，可完整导出全部打卡流水；

3. 自动化执行：签到、发奖、惩罚全部由代码自动执行，无需人工审核；

4. 开发门槛低：Solidity最简入门项目，Monad测试网新手开发首选。

**缺点**

1. 每次打卡需支付Gas手续费，高频签到成本高；

2. 所有记录公开：任何人查到你的打卡时间与频率，无隐私；

3. 规则固化：修改签到冷却、奖励额度需要重新部署合约；

4. 无法验证线下真实行为：仅校验钱包交互，GPS、真人打卡需要搭配预言机。

## Prompt：

用 Solidity 编写一份最简打卡智能合约，适配 Monad 测试网，要求： 1. 每个钱包地址24小时只能打卡一次； 2. 记录每个用户总打卡次数、连续打卡天数，断签重置连续天数； 3. 只包含基础打卡、查询记录两个核心函数。 4. 代码附带中文注释，规避溢出漏洞，代码尽量简短易懂； 5. 最后附上简单部署、测试操作说明。

## 合约代码：

```remix-solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.26;


/**
 * @title SimpleCheckIn
 * @dev 简单打卡合约
 */
contract SimpleCheckIn {

    // 用户签到记录
    struct UserInfo {
        uint256 lastTime;   // 最近一次签到时间
        uint256 count;      // 总签到次数
    }


    // 用户地址 => 签到数据
    mapping(address => UserInfo) public users;


    // 24小时冷却时间
    uint256 public constant COOLDOWN = 24 hours;


    // 签到事件
    event CheckedIn(
        address indexed user,
        uint256 time,
        uint256 count
    );


    /**
     * @dev 用户签到
     */
    function checkIn() external {

        UserInfo storage user = users[msg.sender];


        // 24小时内不能重复签到
        require(
            block.timestamp >= user.lastTime + COOLDOWN,
            "Already checked in"
        );


        // 更新签到时间
        user.lastTime = block.timestamp;


        // 增加签到次数
        user.count += 1;


        // 触发事件
        emit CheckedIn(
            msg.sender,
            block.timestamp,
            user.count
        );
    }



    /**
     * @dev 查询签到记录
     */
    function getRecord(address user)
        external
        view
        returns(
            uint256 lastTime,
            uint256 count
        )
    {

        UserInfo memory info = users[user];

        return(
            info.lastTime,
            info.count
        );
    }

}
```

通过以上流程我学会了如何使用Remix将一个简单合约部署到钱包中，并理解了合约是怎么工作，gas 费怎么收取。并且知道了怎么将部署的合约重新找出来
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->



# 手册学习

## 区块链基础概览

* * *

## 一、区块链是什么？

区块链是一种**去中心化的分布式账本技术**，用于在网络节点之间安全、透明且不可篡改地记录事务数据。

### 区块结构

-   每个区块包含**交易记录** + **前一个区块的哈希摘要**
    
-   每个区块有**有限的存储容量**
    
-   在一定时间内被打包（如比特币约 10 分钟生成一个区块）
    

### 链的结构

-   任意一条边只可以连接两个区块
    
-   只有一条链
    
-   区块按顺序依次串联
    

### 核心特性

| 特性 | 说明 |
| --- | --- |
| 不可篡改 | 修改历史区块需修改后面全部区块，几乎不可能 |
| 公开透明 | 链上所有记录和钱包余额公开可查 |
| 匿名性 | 无人知道钱包归属 |
| 快速交易 | 无论金额大小和地理位置，交易打包即完成 |

### 如何确保历史记录可信？

区块链网络中有非常多的节点（矿机）来记账，每个节点都记录完整、相同的区块链信息。

### 分布式网络的新特性

-   **去中心化**：网络分布在全球，无人能控制全部节点
    
-   **真正的不可篡改**：只要被修改节点不超过 **51%**，改动不被认可
    

* * *

## 二、什么是比特币（BTC）？

**节点奖励**：网络节点服务提供商可获得代币奖励（如比特币）。  
**货币属性**：供应量有限，可自由转账，具备货币特性。

### FAQ

| 问题 | 解答 |
| --- | --- |
| 比特币有什么价值？ | 跨境转账结算方便，有使用价值 |
| 为什么价格高且波动大？ | 供需决定，无监管易受新闻影响 |
| 如何平衡透明与隐私？ | 通过随机生成的钱包地址实现匿名 |
| 有什么缺点？ | 常被黑产利用，区块生成约10分钟，存储有限 |

* * *

## 三、区块链的核心组成部分

**两个核心**：去中心化的网络 + 区块链数据

**节点激励机制**：

-   PoW 链中称为"挖矿"，参与者是"矿工"
    
-   节点收入 = **协议奖励** + **交易手续费**
    
-   Gas Fee（燃料费）属于交易手续费的一部分
    

### 一条区块链的运行流程

```
用户发起交易 → 交易广播到网络 → 节点验证
    → 打包成块 → 链接上链 → 奖励发放
```

* * *

## 四、公链 vs 私链 vs 联盟链

| 类型 | 节点加入 | 数据可见性 | 管理模式 | 适合场景 |
| --- | --- | --- | --- | --- |
| 公链 | 任何人自由加入 | 所有人可见 | 去中心化（投票） | 加密货币、公共存证 |
| 联盟链 | 邀请/审批 | 仅联盟成员可见 | 多中心化（董事会） | 供应链、金融协作 |
| 私链 | 老板严格审批 | 仅内部可见 | 中心化 | 企业内部管理、审计 |

* * *

## 五、Web3 vs Web 3.0 vs Web2

### Web2（当前互联网）

-   中心化控制，数据归平台
    
-   用户是生产者但不拥有数据
    
-   广告驱动，平台抽取佣金
    
-   比喻：**租房子**，房东随时能收回钥匙
    

### Web 3.0（语义网）

-   RDF/OWL 标准描述数据关系
    
-   知识图谱构建语义关系网络
    
-   ⚠️ **不是区块链技术**，而是数据组织升级
    
-   比喻：**图书馆贴标签**
    

### Web3（去中心化互联网）

-   数据主权归用户，区块链身份和资产
    
-   无需信任中介，**智能合约**自动执行
    
-   典型应用：MetaMask、Uniswap、ENS
    
-   比喻：**自己买地盖房**
    

### 对比矩阵

| 维度 | Web2 | Web 3.0 | Web3 |
| --- | --- | --- | --- |
| 控制权 | 平台垄断 | 部分开放 | 用户自治 |
| 数据存储 | 中心服务器 | 混合存储 | 区块链/IPFS |
| 支付系统 | 信用卡/支付宝 | 集成支付 | 加密货币 |
| 典型技术 | JavaScript | RDF/OWL | 智能合约 |
| 代表企业 | 腾讯/阿里 | W3C/DBpedia | Uniswap/ConsenSys |

### 常见误解

1.  **Web3 ≠ Web 3.0**：区块链 vs 语义网
    
2.  **Web3 不是万能的**：不适合高频社交（如微博）
    
3.  **渐进式过渡**：存在 Web2.5 案例（如 Reddit 社区积分）
    

* * *

## 六、去中心化的优势与挑战

### 优势

-   **信任最小化**：无需依赖中心化第三方
    
-   **抗审查与高弹性**：单点故障难阻断网络
    
-   **用户自主管理**：私钥掌控资产
    
-   **开放创新生态**：任何人可参与创新
    

### 挑战

-   **可扩展性瓶颈**：吞吐量低，Layer 2 方案正在优化
    
-   **安全与治理**：代码漏洞、投票权集中
    
-   **用户体验**：私钥管理复杂、Gas 费浮动
    
-   **法律合规**：匿名特性与法规冲突
    

* * *

## 以太坊概览

* * *

以太坊被誉为\*\*“区块链 2.0"**和**"世界计算机”\*\*——不仅是一种加密货币（ETH），更是支持智能合约的全球共享计算机。2022 年完成从 PoW 到 PoS 的升级（The Merge），并借助分片和 Layer 2 技术提升扩展性。

* * *

## 一、以太坊介绍

-   开源去中心化区块链平台，原生代币为 **ETH**
    
-   提供 **EVM（以太坊虚拟机）** 处理点对点合约
    
-   由 **Vitalik Buterin** 于 2013-2014 年提出
    
-   核心创新：**智能合约**——存储在链上的可执行代码
    
-   截至 2025 年，ETH 全球市值排名第二
    
-   The Merge 后 ETH 呈轻微通缩趋势（年通胀约 0.2%-0.8%）
    

* * *

## 二、Ethereum vs Bitcoin

| 维度 | 比特币 | 以太坊 |
| --- | --- | --- |
| 目标 | 去中心化数字货币（数字黄金） | 去中心化平台（世界计算机） |
| 编程能力 | 脚本语言有限 | 图灵完备（Solidity） |
| 共识机制 | PoW | PoS（The Merge 后） |
| 交易速度 | 每 10 分钟一个区块 | 约 12 秒一个区块 |
| 经济模型 | 总量固定 2100 万 | 供应灵活，EIP-1559 后通缩 |

* * *

## 三、以太坊的定位与演进

### 1.0 阶段（PoW）

-   矿工通过算力挖矿，消耗大量电力
    
-   问题：能耗高、扩展性差（约 30 TPS）
    

### The Merge（2022.9.15）

**完整历程**：

| 时间 | 事件 |
| --- | --- |
| 2020.12 | 信标链启动，独立运行 PoS 共识 |
| 2022.9.15 | The Merge 发生，主网关闭 PoW，切换到 PoS |
| 合并后 | 执行层（交易处理）+ 共识层（验证者管理） |

**PoS 机制**：

-   验证者质押 **32 ETH** 成为节点
    
-   系统随机选择验证者提议区块
    
-   作恶者被 **Slashing**（质押销毁）
    
-   能耗降低 **99.95%**
    

### 未来升级路线

| 升级 | 状态 | 效果 |
| --- | --- | --- |
| The Merge | ✅ 2022 完成 | PoW → PoS |
| EIP-4844（Cancun） | ✅ 2024.3 上线 | Blob 交易，L2 费用降 70-90% |
| Pectra | 🚧 2025+ | 账户体验、验证器效率 |
| 数据分片 | 🚧 2025-2026 | Proto-Danksharding |

* * *

## 四、以太坊生态分层架构

```
应用层 (Dapps)
  ├── DeFi: Uniswap, Aave, Compound
  ├── NFT: OpenSea, Foundation
  ├── 钱包: MetaMask, Rainbow
  └── DAO 工具: Snapshot, Aragon
        ↓
协议层 (核心基础设施)
  ├── 共识层: Prysm, Lighthouse, Nimbus
  └── 执行层: Geth, Nethermind, Erigon
        ↓
扩展层 (Scaling)
  ├── L2 Rollups: Arbitrum, Optimism, zkSync
  ├── 侧链: Polygon PoS, Gnosis Chain
  └── 状态通道
```

* * *

## 五、以太坊文化与价值观

| 价值观 | 说明 |
| --- | --- |
| 去中心化治理 | 无单一控制者，EIP 机制共同决策 |
| 无需许可与开放性 | 任何人都可开发、部署智能合约 |
| 抗审查性 | 交易和合约不受政府/机构干预 |
| 密码朋克精神 | “代码即法律”，密码学保护隐私 |
| 公共物品导向 | 支持开源项目和基础设施建设 |
| 可持续发展 | The Merge 体现环境责任承诺 |

* * *

## 六、以太坊核心机制

### 账户系统

| 类型 | 控制方式 | 能否发起交易 |
| --- | --- | --- |
| EOA（外部账户） | 私钥控制 | ✅ 可主动发起 |
| CA（合约账户） | 合约代码控制 | ❌ 需 EOA 触发 |

每个账户含：Nonce、余额、CodeHash、StorageRoot

### Gas 模型

```
Gas 费用 = Gas Limit × Gas Price
```

-   **Gas Limit**：最多愿意消耗的燃料单位
    
-   **Gas Price**：用 Gwei 表示（1 Gwei = 0.000000001 ETH）
    
-   **EIP-1559** 后拆分为：
    
    -   **Base Fee**（基础费用）：自动计算，直接**销毁**
        
    -   **Tip**（小费）：给验证者的额外奖励
        

### EVM（以太坊虚拟机）

-   **图灵完备**：可执行各种逻辑（if、循环等）
    
-   **全球同步**：每个节点独立执行合约，结果一致
    
-   **隔离安全**：合约在隔离沙箱中运行
    

### 类比理解

-   **EOA = 个人钱包**，私钥签名证明身份
    
-   **CA = 智能保险箱**，按预设程序自动运行
    
-   **Gas = 计算资源费用**
    
-   **EVM = 全球共享计算机**
    

* * *

# 分享会

## **DevRel 的成长之路**

\*\*1. 技术岗门槛高：\*\*智能合约开发需要项目实操经验，不适合零基础新人；开发类岗位（合约、前端）需求量小，单个项目仅需2-3人，衍生品开发更不适合小白，新人可先通过运营实习、入职项目方、GitHub实操积累相关经验。

\*\*2. 业务岗缺口大：\*\*市场、运营、BD类岗位招聘需求更高，是新人更易切入的方向。

\*\*3. 语言硬性要求：\*\*行业高频使用英语，掌握英语对求职、日常工作十分关键。

\*\*4. 就业渠道：\*\*交易所相关岗位数量整体多于区块链项目方。

## Co-learning

跟着苏生老师将前三个任务过了一遍，通过简单的留言板合约了解到了怎么在 Remix 中部署并且与合约交互。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
