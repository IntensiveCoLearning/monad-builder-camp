---
timezone: UTC+8
---

# elliothunter32-star

**GitHub ID:** elliothunter32-star

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
# Research Notes：质押、金库标准与收益分配机制

## 一、ERC-4626：代币化金库标准

### 背景

2022 年以前，每个 DeFi 协议的金库（vault）接口各自为政——Yearn、Aave、Compound 各有一套存取款函数。聚合器要集成 N 个协议就得写 N 套适配器。ERC-4626（2022 年 3 月定稿）统一了这个接口。

### 核心设计

ERC-4626 继承自 ERC-20，金库本身就是一个 ERC-20 代币，每个代币单位叫"share"（份额），代表对底层资产的按比例所有权。

四个核心操作，本质上是两对镜像：

| 操作 | 用户指定 | 合约计算 | 方向 |
| --- | --- | --- | --- |
| deposit | 存入多少资产 | 应得多少份额 | 资产 → 份额 |
| mint | 想要多少份额 | 需存多少资产 | 份额 → 资产 |
| withdraw | 取回多少资产 | 销毁多少份额 | 资产 → 份额 |
| redeem | 销毁多少份额 | 返还多少资产 | 份额 → 资产 |

### 份额定价

```
shares = deposit_amount × total_shares / total_assets
```

收益体现在"同样数量的份额能赎回更多资产"，份额数量本身不变。这是理解后续所有金库类产品的基础模型。

### 安全考量：通胀攻击

攻击者作为首个存款人存入极小金额，再直接向合约转入大量资产抬高 share 价格，导致后续存款人因取整得到 0 shares。防御手段：规定取整方向有利于金库 + 部署时预存"死份额"。

### 后续扩展

-   ERC-7540：支持异步存取（适配 RWA、跨链等需要等待的场景）
    
-   ERC-7575：支持多资产金库（LP Token 等）
    

* * *

## 二、Lido：流动性质押协议拆解

### 解决的问题

以太坊 PoS 质押的三个摩擦：门槛高（需 32 ETH）、资金被锁定（无法参与 DeFi）、需要技术运维能力。Lido 把散户的 ETH 池化后交给专业节点运营商，发行 stETH 作为可流通的质押凭证。

### 核心机制

**stETH — Rebase 模式**：用户存入 10 ETH 得到 10 stETH，每天预言机上报验证者奖励后，所有持有者的 stETH 余额自动增加。收益体现在代币数量变化。

**wstETH — 份额模式**：因为 rebase 代币与许多 DeFi 协议不兼容（余额变化会搞乱会计逻辑），Lido 提供 wstETH 包装版本——余额不变，但每个 wstETH 随时间能赎回越来越多的 ETH。这与 ERC-4626 的份额思路一致。

**两种模式对比**：

|   | stETH（Rebase） | wstETH（Shares） |
| --- | --- | --- |
| 余额 | 每天自动增加 | 固定不变 |
| 收益体现 | 数量增长 | 单位价值增长 |
| DeFi 兼容性 | 差 | 好 |
| 底层逻辑 | 供应量动态调整 | ERC-4626 式份额模型 |

### 用户角色

-   **质押者**：存入 ETH，持有 stETH/wstETH，赚取质押收益 + 保持 DeFi 流动性
    
-   **节点运营商**：运行验证者节点，赚取 5% 收益分成
    
-   **LDO 持有者**：治理投票，决定协议参数
    
-   **DeFi 用户**：用 stETH/wstETH 在 Aave、Morpho、Pendle 等协议中做抵押借贷、收益拆分
    
-   **机构**：通过 Lido V3 的 stVaults 定制合规质押方案
    

### 收入模型

协议对质押收益（不碰本金）收取 10% 费用：90% 归质押者，5% 归节点运营商，5% 归 DAO 国库。当前 ETH 质押年化约 2.5%-4%。Lido 2024 年产生约 9,900 万美元协议收入。

### 风险

-   **智能合约风险**：合约漏洞可能导致资金损失
    
-   **罚没风险**：节点运营商违规被以太坊协议惩罚（分散到多运营商可降低）
    
-   **脱锚风险**：stETH 二级市场价格可能偏离 ETH（2022 年 Terra 崩盘时跌至 0.93，上海升级后显著改善）
    
-   **中心化风险**：Lido 控制约 23-28% 的以太坊质押量，引发网络去中心化争论
    
-   **治理风险**：LDO 大户可能控制 DAO 决策方向
    

### 竞争格局

Lido 的护城河在于先发优势和 stETH 的 DeFi 集成网络效应。主要竞争者包括 Rocket Pool（更去中心化，任何人可运行节点）、Coinbase cbETH（中心化背景，合规优势）等。

* * *

## 三、质押收益分配：RewardPerShare 累加器

### 问题

如果协议有 10 万个质押者，每次产生收益就给每个人转一笔账，gas 成本会指数增长。

### 解决方案

只维护一个全局变量 `rewardPerShare`，每次新奖励到账时更新一次：

```
rewardPerShare += 新奖励 / 总质押量
用户可领取 = 用户质押量 × (当前 rewardPerShare - 用户上次领取时的 rewardPerShare)
```

不管有多少用户，每次更新只改一个数字，gas 成本固定。用户领取时合约当场计算应得金额。

### 前端的"实时跳动"

用户界面上每几秒数字增长一次，并不是链上每几秒发一笔交易，而是前端用 JavaScript 根据 rewardRate 做本地插值，让数字看起来平滑增长。实际的链上状态更新频率远低于视觉呈现频率。

### 不同协议的奖励源头频率

| 协议类型 | 奖励更新频率 | 原因 |
| --- | --- | --- |
| Lido（stETH） | 每天一次 | 预言机每日上报验证者奖励 |
| DEX 手续费 | 逐笔交易 | 每笔 swap 实时产生手续费 |
| 通胀释放型 | 逐区块（~12秒） | 合约按区块释放固定数量代币 |

分红频率是技术设计问题，不反映项目基本面。

* * *

## 四、质押挖矿（Staking Mining）

### 概念溯源

"挖矿"一词经历了三次含义迁移：

1.  **PoW 挖矿**（比特币）：投入算力 + 电力 → 获得区块奖励。真实的物理资源消耗。
    
2.  **流动性挖矿**（2020 DeFi Summer）：投入资金提供流动性 → 获得协议代币奖励。Compound 首创，"挖矿"从算力变成资金。
    
3.  **质押挖矿**：投入代币锁仓 → 获得新发行代币奖励。把"提供流动性"简化为"锁仓"。
    

**质押挖矿 = 锁仓领补贴。**

### 典型合约结构

```
rewardRate  = 每秒释放多少奖励代币
totalStaked = 池子总质押量
userStaked  = 用户质押量

用户每秒收益 = rewardRate × (userStaked / totalStaked)
```

底层就是 RewardPerShare 累加器的直接应用。

### 项目方的真实目的

| 目的 | 机制 |
| --- | --- |
| 减少抛压 | 用高 APY 激励持有者锁仓不卖，降低流通盘卖压 |
| 冷启动 | 用代币激励吸引早期资金，快速积累 TVL |
| 代币分发 | 通过质押释放代币，比空投有更好的筛选作用 |

### 经济学本质

质押挖矿的奖励几乎都来自代币增发，本质是再分配：

-   早期参与者：质押时参与者少、APY 高、币价可能还在涨 → 获利
    
-   后期参与者：质押时参与者多、APY 被稀释、早期参与者开始抛售奖励代币 → 币价承压
    

内在矛盾：高 APY 吸引更多人 → 稀释每人收益 → APY 下降 → 早期卖出 → 币价下跌 → 后来者以法币计价实际亏损。

### 区分质押的三种模式

| 模式 | 收益来源 | 经济功能 | 可持续性 |
| --- | --- | --- | --- |
| 协议级质押（如以太坊 PoS） | 网络增发 + 交易小费 | 保障网络安全 | 高——购买真实服务 |
| 分润型质押 | 协议真实收入（手续费等） | 分享协议价值 | 中高——取决于协议收入 |
| 通胀奖励型质押（多数质押挖矿） | 代币增发 | 减少抛压 / 代币分发 | 低——纯再分配 |

### 判断框架

遇到任何"质押赚收益"的项目，问五个问题：

1.  **收益来源**：是协议真实收入还是代币增发？
    
2.  **经济功能**：质押在保障什么？网络安全？流动性？还是仅仅"别卖"？
    
3.  **罚没机制**：有没有 slashing？有 → 质押在约束真实行为；没有 → 可能只是营销包装
    
4.  **解锁条件**：锁定期多长？有没有流动性收据？提前退出的成本？
    
5.  **剥离测试**：如果去掉代币激励，这个协议还有人用吗？有真实产品 → 激励是增长手段；没有 → 激励就是产品本身
    

* * *

## 概念关系图

```
ERC-20（同质化代币标准）
  │
  └─ ERC-4626（代币化金库标准）── 份额模型：份额数量不变，单位价值增长
        │
        ├─ Lido wstETH（份额型质押凭证）
        ├─ Yearn V3 Vaults
        ├─ Morpho Vaults
        └─ 更多金库类产品...

质押生态
  │
  ├─ 协议级质押（PoS）── 保障网络安全，有罚没
  │    └─ 流动性质押（Lido, Rocket Pool）── 发行收据代币释放流动性
  │
  ├─ 应用级质押
  │    ├─ 分润型 ── 收益来自协议真实收入
  │    ├─ 通胀奖励型（质押挖矿）── 收益来自代币增发
  │    └─ 治理锁仓型（ve 模型）── 锁仓换治理权重
  │
  └─ 收益分配机制
       └─ RewardPerShare 累加器 ── 一个全局变量解决 N 个用户的分账问题
```
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->


今天学习，怎么用ai辅助，开发自己的钱包
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->



今天主要学习，如果之前没有web3经历，只在其他传统行业工作过，如何修改自己的简历，进入web3行业
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->




今天看了两节课，一个是ai时代如何做reserach，一个是web3的运营流程是怎么样的，ai reserach这边了解到grokpedia这个工具，然后运营的话，核心就是利用ai提升效率，综合来说，还是要利用ai区完善自己的工作流。我在这次web3的学习中，理解熟悉了一些基础概念，感觉核心还是怎么把ai嵌入自己的日常工作当中，最好能利用ai去完善自己的产品
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->





![592fe03df460c9a59e134965af642c7a.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/elliothunter32-star/images/2026-07-15-1784121207325-592fe03df460c9a59e134965af642c7a.png)

agents.md这个，我后续会结合自己的工作流深入研究一下；然后关于区块链开发，对于区块链的定义和理解比之前深了，也了解了区块链开发的几个方向和我们需要的技术栈
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->






## 一、ERC-20 是什么？

**ERC** = Ethereum Request for Comments（以太坊意见征求稿），**20** 是提案编号。

ERC-20 不是一段代码，而是一份**接口规范**——它规定了：如果你要在以太坊上发行一个代币（Token），你的智能合约**必须包含哪些函数和事件**。

### 类比理解

ERC-20 就像 **USB 接口标准**：

-   不管你做的是键盘、鼠标还是U盘，只要你用 USB 接口，任何电脑都能识别
    
-   不管你发的代币叫什么名字、有什么用途，只要你的合约实现了 ERC-20 标准，MetaMask、Uniswap、交易所都能识别和操作你的代币
    

### 为什么需要标准？

如果没有 ERC-20，每个人发的代币合约函数名都不一样：有人叫 `send()`，有人叫 `moveTo()`，有人叫 `give()`。MetaMask 就不可能做一个通用界面来支持所有代币。有了标准，所有代币都用 `transfer()`，钱包只需要对接一套接口就行。

* * *

## 二、ERC-20 代币的本质

### 代币 ≠ 真的"币"

ERC-20 代币并不像 ETH 那样是区块链原生资产。它的本质就是：**一个合约里的记账数字**。

合约内部维护一个 mapping：

solidity

```solidity
mapping(address => uint256) private _balances;
```

-   你"持有"100个代币 = 这个 mapping 里你的地址对应的值是 100
    
-   "转账" = 把你的值减少，对方的值增加
    
-   没有任何实体在移动，只是数字在变
    

### 对比 ETH（原生币）

| 对比项 | ETH | ERC-20 代币 |
| --- | --- | --- |
| 存在方式 | 以太坊协议层内置 | 智能合约里的一个 mapping |
| 转账方式 | 直接用 msg.value 发送 | 调用合约的 transfer() 函数 |
| 需要 gas 吗 | 需要 | 需要（而且 gas 用 ETH 付） |
| 谁都能发？ | 不能，ETH 只有一个 | 任何人都能部署一个 ERC-20 合约发代币 |

> 之前做预测市场时，用户用 ETH 下注（msg.value）。如果改用 ERC-20 代币下注，用户需要先 approve 授权你的合约，然后你的合约通过 transferFrom 把代币转过来。

* * *

## 三、ERC-20 的 6 个核心函数

### 读取类（view，不消耗 gas）

| 函数 | 作用 | 类比 |
| --- | --- | --- |
| totalSupply() | 返回代币发行总量 | 查这个币一共发了多少 |
| balanceOf(address) | 查某个地址持有多少代币 | 查银行余额 |
| allowance(owner, spender) | 查 owner 授权 spender 能花多少 | 查委托书上写的额度 |

### 写入类（要消耗 gas）

| 函数 | 作用 | 类比 |
| --- | --- | --- |
| transfer(to, amount) | 直接转代币给别人 | 我亲自转账给你 |
| approve(spender, amount) | 授权别人最多能从我账户花多少 | 给银行签委托扣款书 |
| transferFrom(from, to, amount) | 被授权者从别人账户转代币 | 银行按委托书从我账户扣款 |

### 2 个必须的事件（Event）

solidity

```solidity
event Transfer(address indexed from, address indexed to, uint256 value);
event Approval(address indexed owner, address indexed spender, uint256 value);
```

事件会写入区块链日志，前端 DApp 可以监听这些事件来更新 UI（就像之前预测市场里监听下注事件一样）。

* * *

## 四、approve + transferFrom 两步机制（重点）

### 为什么不能一步完成？

在以太坊里，智能合约**不能主动从你的地址拿走代币**。你必须先"授权"它，它才能操作。

### 流程拆解

以 Uniswap 换币为例：

```
第 1 步：你调用代币合约的 approve(Uniswap合约地址, 1000)
   → 意思是：我允许 Uniswap 合约最多动用我 1000 个代币

第 2 步：你调用 Uniswap 合约的 swap()
   → Uniswap 合约内部调用 token.transferFrom(你的地址, Uniswap地址, 500)
   → 因为你已经 approve 了，所以这个调用会成功
   → allowance 从 1000 减少到 500
```

### 类比

你去餐厅吃饭：

1.  **approve** = 你把信用卡交给服务员，说"最多刷 500 块"
    
2.  **transferFrom** = 服务员拿你的卡去刷了 200 块
    
3.  你没亲自操作，但你授权了，所以餐厅可以扣款
    

### 安全注意事项

-   approve 的金额设太大有风险：如果合约有漏洞或恶意代码，它可以把你的代币全部转走
    
-   所以有些 DApp 会让你 approve 精确的金额，而不是 "无限授权"
    

* * *

## 五、一个最简 ERC-20 合约长什么样

solidity

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MyToken {
    string public name = "MyToken";
    string public symbol = "MTK";
    uint8 public decimals = 18;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(uint256 _initialSupply) {
        totalSupply = _initialSupply * 10 ** decimals;
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function transfer(address to, uint256 amount) public returns (bool) {
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
        require(balanceOf[from] >= amount, "Insufficient balance");
        require(allowance[from][msg.sender] >= amount, "Allowance exceeded");
        balanceOf[from] -= amount;
        balanceOf[to] += amount;
        allowance[from][msg.sender] -= amount;
        emit Transfer(from, to, amount);
        return true;
    }
}
```

### 逐段解读

-   **name / symbol / decimals**：代币名称、符号（像股票代码）、小数位数。decimals = 18 是惯例，跟 ETH 一样（1 ETH = 10^18 wei）
    
-   **constructor**：部署时铸造初始代币，全部给部署者
    
-   `address(0)` **的 Transfer 事件**：从零地址转出 = "铸造"新代币（行业惯例）
    
-   **双层 mapping**：`allowance[owner][spender]` 记录每对授权关系的额度
    

* * *

## 六、decimals 为什么是 18？

Solidity 没有小数（浮点数），所以用整数模拟小数：

-   decimals = 18 意味着合约内部存的数字要乘以 10^18
    
-   你看到余额显示 "1.5 MTK"，合约里实际存的是 `1500000000000000000`
    
-   这跟 ETH 的单位一样：1 ETH = 10^18 wei
    

如果你要发行总量 1000 个代币，constructor 里传入 1000，代码 `_initialSupply * 10 ** decimals` 会算出实际存储的数字。

* * *

## 七、常见 ERC-20 代币实例

| 代币 | 符号 | 用途 | 说明 |
| --- | --- | --- | --- |
| Tether | USDT | 稳定币 | 1 USDT ≈ 1 美元 |
| USD Coin | USDC | 稳定币 | Circle 发行，合规性更好 |
| Chainlink | LINK | 预言机 | 用来支付 Chainlink 预言机服务费 |
| Uniswap | UNI | 治理 | 持有者可以投票决定 Uniswap 协议的升级方向 |
| Wrapped ETH | WETH | 包装 ETH | 把 ETH 包装成 ERC-20 格式，方便在 DeFi 中使用 |

> WETH 很有意思：ETH 本身不是 ERC-20，但 DeFi 协议都按 ERC-20 标准来对接。所以 WETH 合约让你存入 ETH，给你等量的 WETH（ERC-20 格式），用完再换回来。

* * *

## 八、ERC-20 与其他代币标准的关系

| 标准 | 用途 | 特点 |
| --- | --- | --- |
| ERC-20 | 同质化代币（Fungible Token） | 每个代币完全相同，可互换（像人民币） |
| ERC-721 | 非同质化代币（NFT） | 每个代币独一无二（像一幅画） |
| ERC-1155 | 混合代币 | 一个合约同时管理 FT 和 NFT |
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->







分享一下今天的学习成果，从零搭了一个预测市场 DApp，已经上线了：

[https://prediction-market-dapp.vercel.app/](https://prediction-market-dapp.vercel.app/)

「特朗普能否赢得中期选举」，连钱包下注 Yes 或 No，结果出来后赢家瓜分全部奖池。Sepolia 测试网

![c9eab26e552fbf4c6f965982fc56b4a4.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/elliothunter32-star/images/2026-07-11-1783758327579-c9eab26e552fbf4c6f965982fc56b4a4.png)
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->








## 一、账户与身份体系

传统互联网产品中，每个 App 都需要注册一个独立账号（手机号、邮箱、密码），你的身份由平台管理和验证。忘记密码了可以找回，账号被盗了可以冻结，一切依赖平台这个"中间人"。

链上产品中，你的身份就是一个钱包地址，用一把私钥来控制。私钥是一串 64 位的随机数，谁持有它，谁就拥有这个地址里的一切。没有客服、没有重置密码、没有冻结机制——自由的代价是完全自负其责。

助记词（12 个英文单词）可以推导出无数个私钥和地址，而私钥只对应一个地址。助记词泄露意味着所有账户丢失，私钥泄露只影响单个地址。

## 二、数据归属与透明性

传统产品的数据存储在公司的服务器上，用户实际上并不拥有自己的数据。公司可以随时修改规则、封禁账号、删除内容，你的"资产"本质上只是数据库里的一条记录，由公司替你保管。

链上产品的数据写在区块链上，公开、透明、不可篡改。智能合约的代码对所有人可见，规则怎么运行，所有人都能检验。没有"暗箱操作"的空间。

## 三、交易与手续费机制

传统产品的操作可以撤销——转错了可以联系客服退款，下单错了可以取消。这些操作对用户来说通常是"免费"的，成本由平台承担。

链上的每一笔交易一旦被确认，就不可逆转。而且每笔交易都需要支付 Gas 费。Gas 是衡量计算资源的单位，Gas Price 是每单位的价格，两者相乘就是手续费（Transaction Fee）。

以我在 Monad Testnet 上的一笔实际交易为例：

-   **Transaction Hash**: `0x551e6b...bc56a9`
    
-   **Gas Used**: 1,000,000
    
-   **Gas Price**: 102.91 Gwei
    
-   **Transaction Fee**: 0.10291 MON
    

关键的一点是：**即使交易失败，Gas 费也照扣**。因为验证者已经花了计算资源去执行你的交易，不管结果成功还是失败，这部分工作已经完成了。这就好比你打车去一个地方，开到半路发现路断了，但司机已经开了这段路，你仍然要付这段路程的费用。

## 四、网络与连接方式

传统产品用户只需要联网、登录即可使用。用户不需要关心数据存在哪个服务器、走的什么协议。

链上产品需要用户主动选择连接哪条链。钱包通过 RPC URL 与区块链节点通信，Chain ID 确保交易不会被误发到其他链上。一个钱包可以连接多条链（如 Ethereum、Monad、Polygon），但同一时间只与一条链交互。如果不配置网络参数，钱包就像一部没插 SIM 卡的手机，有能力但连不上网络。

## 五、测试与试错成本

传统产品的测试对用户不可见，发生在公司内部的开发环境中。

链上产品有公开的测试网（Testnet），任何人都可以通过水龙头（Faucet）领取免费测试币进行操作。测试网与主网完全隔离，代币没有真实价值。Faucet 的本质是一个"金库钱包"自动向你转账，和正式转账是同一种链上操作。

![1783655065623_67160194-70f3-4460-b5ec-0a75b0100cd3.png](https://claude.ai/api/8c6db129-397b-4bf1-80cf-516b9ce14b5b/files/283038cc-576f-46bf-861f-db9cc0b21ce5/preview)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/elliothunter32-star/images/2026-07-10-1783655529379-image.png)
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->









![a672f12f-102a-4ea0-bd4a-38f9327668ac.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/elliothunter32-star/images/2026-07-09-1783605084886-a672f12f-102a-4ea0-bd4a-38f9327668ac.png)![12eaf9c6-c760-4770-a0ae-35d2ccda8daf.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/elliothunter32-star/images/2026-07-09-1783605100079-12eaf9c6-c760-4770-a0ae-35d2ccda8daf.png)

1.agent高风险实例 过度授权 建议经常审查权限清单 使用agent guard工具 对于敏感操作使用人工复核  
2.prompt注入

3.恶意skills  
4.系统漏洞、ai幻觉、恶意安装软件

![cf82f41f-edc0-490f-9562-57b52f30de1e.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/elliothunter32-star/images/2026-07-09-1783605903816-cf82f41f-edc0-490f-9562-57b52f30de1e.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->










![cb2e9382af2db6f6146bc1f17762cdb1.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/elliothunter32-star/images/2026-07-08-1783509776518-cb2e9382af2db6f6146bc1f17762cdb1.png)

[ai](http://1.ai) agent可以为一些个人开发者，opc提供了一个变现渠道；

在微交易的场景下，agent可以集中结算，比如电商场景；

传统金融对于风控的要求非常高；

agent可以去完成某个任务，然后拿到激励；

skill可以被付费调用，这是一个很重要的变现场景；
<!-- DAILY_CHECKIN_2026-07-08_END -->
<!-- Content_END -->
