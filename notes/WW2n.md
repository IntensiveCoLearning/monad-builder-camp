---
timezone: UTC+8
---

# WW2n

**GitHub ID:** WW2n

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
### 步骤 1：AI 生成最小 Solidity 合约初稿

向 AI 输入提示词：生成兼容 Solidity 0.8.x 的极简存储合约，包含 1 个数字状态变量、set 写函数、get 读函数，附带基础注释。

输出示例极简合约：

solidity

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleStorage {
    uint public storeNum;

    // Write函数：修改状态，消耗Gas
    function setNum(uint _num) external {
        storeNum = _num;
    }

    // Read函数：只读，免费查询
    function getNum() external view returns(uint) {
        return storeNum;
    }
}
```

### 步骤 2：人工校验 AI 代码（防坑）

1.  检查开源协议 SPDX、编译器版本；
    
2.  确认函数可见性、view 修饰符正确；
    
3.  无转账逻辑，无重入风险，极简合约无高危漏洞；
    
4.  如需权限控制，引入 OpenZeppelin Ownable 加固。
    

### 步骤 3：Remix 编译与安全检查

1.  [打开remix.ethereum.org](http://打开remix.ethereum.org)，新建`SimpleStorage.sol`粘贴代码；
    
2.  Compiler 面板选择匹配 0.8.20 编译器，开启静态分析；
    
3.  点击 Compile，无红报错、无高危警告即编译通过；
    
4.  复制编译输出的 ABI、Bytecode 备用。
    

### 步骤 4：切换 Monad Testnet 部署环境

1.  MetaMask 切换至 DAY3 创建的课程专用钱包、切换 Monad Testnet；
    
2.  Remix Deploy 面板环境选择`Injected Provider - MetaMask`，自动连接钱包；
    
3.  确认钱包有足够 MON 测试币支付部署 Gas。
    

### 步骤 5：部署合约并获取合约地址

1.  构造函数无参数，直接点击 Deploy；
    
2.  MetaMask 签名部署交易，等待区块确认；
    
3.  部署成功后复制生成的**合约地址**与部署 TxHash。
    

### 步骤 6：合约读写交互，区分 Read/Write

1.  Read 操作（蓝色 getNum 按钮）：直接点击，无需签名，页面返回存储数字，不产生交易；
    
2.  Write 操作（橙色 setNum 按钮）：输入任意数字，点击执行，钱包签名交易；
    
3.  交互完成后复制本次操作 TxHash，进入 Monad 区块浏览器查询交易详情。
    

### 步骤 7：打通源码 - ABI - 地址 - TxHash 逻辑

1.  源码编译生成 ABI，工具依靠 ABI 解析合约函数；
    
2.  合约地址是链上合约唯一标识；
    
3.  所有 write 操作会生成独立 TxHash，永久记录部署、修改、交互行为；
    
4.  区块浏览器输入 TxHash，可查看调用函数消耗 Gas、修改的存储状态。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

## **核心知识点整理**

### **1\. Monad 官方文档（核心理解 Monad 底层逻辑）**

**（1）Monad vs Ethereum 核心差异**

链接：[https://docs.monad.xyz/developer-essentials/differences](https://docs.monad.xyz/developer-essentials/differences)

**1. 执行模型根本区别**

1. 以太坊：串行执行交易，区块内交易按顺序逐一处理，交易间存在锁等待，TPS 上限低；

2. Monad：并行 EVM 执行架构，无冲突交易可同时并行处理，理论万级 TPS，出块速度约 1 秒。

**2. 存储与状态架构**以太坊单一全局状态树，读写阻塞；Monad 拆分状态分片，大幅降低高频交互下的链上阻塞。

**3. Gas 与手续费机制**兼容 EIP1559 基础模型，但并行执行大幅降低拥堵，基础 Gas 费长期维持低位，适合小额高频交互。

**4. 开发兼容**完全兼容 Solidity、MetaMask、Remix 等以太坊生态工具，现有 EVM 合约几乎无需修改即可部署，迁移成本极低。

**5. 产品层面影响**以太坊受低 TPS、高 Gas 限制，链游、高频 DEX、微交易、链上社交等产品体验极差；Monad 并行架构解决性能瓶颈，让高频实时 Web3 产品落地可行。

**（2）高性能应用开发最佳实践**

链接：[https://docs.monad.xyz/developer-essentials/best-practices](https://docs.monad.xyz/developer-essentials/best-practices)

1. 适配并行执行的合约编码规范

1. 避免全局共享状态锁，拆分独立存储变量，减少交易读写冲突；

2. 高频交互合约拆分逻辑，降低单笔交易状态修改范围，最大化并行效率。

2. 高频产品适配场景链上游戏、小额高频 Swap、NFT 碎片化交易、链上签到社交、微支付系统等传统以太坊难以承载的产品。

3. 性能优化编码建议减少存储读写操作，多用 memory 临时变量；批量操作逻辑拆分，适配 Monad 并行调度；合理规划合约状态结构，规避状态冲突。

4. 测试网开发规范利用 Monad Testnet 快速迭代，依托高确认速度缩短调试周期，区块浏览器实时查看并行执行交易日志。

**（3）Gas 定价机制**

链接：[https://docs.monad.xyz/developer-essentials/gas-pricing](https://docs.monad.xyz/developer-essentials/gas-pricing)

1. 底层沿用 EIP1559 双层手续费模型：Base Fee（基础费）+ Priority Fee（小费）。

2. 并行执行带来的 Gas 优势：网络拥堵阈值大幅提高，同等交易量下 Base Fee 涨幅远低于以太坊，小额交互手续费成本极低。

3. Gas 消耗基准：基础转账 21000 Gas，合约交互 Gas 消耗逻辑与以太坊一致，但确认速度提升数十倍。

4. 开发实操建议：高频 DApp 可设置极低 Priority Fee，依然能快速打包；批量交易无需担心 Gas 暴涨。

### **2\. BuildAnything Freshman 入门内容**

**1. 什么是 Monad？**Monad 是兼容 EVM 的高性能一层公链，核心创新为并行执行 EVM，在完全保留以太坊开发生态的前提下，解决以太坊串行执行带来的性能瓶颈，定位承载高频、实时化 Web3 应用。

**2. 10000 TPS 带来的产品可能性**

1. 链游：实时对战、连续多次链上操作无等待，不用等待区块确认；

2. 微支付：几分钱级别的链上转账，手续费低廉，支持打赏、订阅等场景；

3. 高频 DEX：高频做市、小额兑换，无 Gas 拥堵成本；

4. 链上社交：发帖、点赞、评论全部上链，实时交互；

5. 传统以太坊受限于 TPS 与 Gas，以上产品仅能做链下半中心化方案，Monad 可实现全链上原生产品。

**3. 数据库与文件存储**链上存储成本高，Monad 高 TPS 仅优化执行速度，不降低存储成本；生产级 DApp 采用「链上存核心资产逻辑 + 链下数据库 / IPFS 存海量数据」混合架构。

**4. 应用生产级品质标准**

1. 合约层面：复用 OpenZeppelin 审计库、AI 代码人工审计、多轮测试网调试；

2. 链交互层面：适配 Monad 并行特性优化合约逻辑；

3. 用户体验：低手续费、秒级交易确认、清晰的链上状态反馈；

4. 安全层面：钱包隔离、授权风险管控、链上操作日志留存。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->


### （一）Web3 实习手册｜安全与合规 [https://web3intern.xyz/zh/security/](https://web3intern.xyz/zh/security/)

1\. 钱包基础安全准则（本课新建专用钱包的理论依据）

1.  **公私钥 / 助记词安全红线**
    
    -   助记词（12/24 词）= 钱包全部资产所有权，一旦泄露，任何人可转走全部代币，无找回渠道。
        
    -   禁止截图、拍照、上传云端、发送聊天框、存手机备忘录，仅纸笔离线备份 2 份分开存放。
        
    -   私钥、Keystore 文件绝不对外分享；公开地址仅可对外展示。
        
2.  **测试网与主网钱包隔离强制要求**
    
    -   测试网存在大量未审计恶意 DApp、钓鱼网站、漏洞合约，若使用主力钱包交互，授权漏洞会直接盗取主网真实资产。
        
    -   实操规范：实验专用钱包仅存放测试币，不导入任何主网助记词，实验结束可废弃。
        
3.  **代币授权重大风险**
    
    -   授权≠转账：授权是授予合约无限划转你代币的权限，无限授权是盗币高发渠道。
        
    -   交互三看原则：看网站域名是否官方、看合约是否经过审计、看授权额度是否为无限。
        
    -   定期在区块浏览器查询授权记录，及时撤销无用授权。
        
4.  **国内 Web3 合规法律风险**
    
    -   我国明令禁止 ICO、IDO、代币发行融资、虚拟货币场外交易炒作；仅可做纯技术层面测试网学习，不可涉及真实法币交易、代币募资。
        
    -   开发、交互仅用于技术实训，严禁参与代币炒作、矿机运营等高风险行为。
        
5.  **钓鱼攻击识别**
    
    -   仿冒钱包插件、假水龙头、假区块浏览器、空投诱导链接均为高频骗局；仅使用官方文档标注链接。
        

### （二）BuildAnything Freshman：加密钱包 & 区块链基础概念

1\. 加密钱包核心原理

1.  钱包本质：不存储代币，仅管理账户私钥；代币余额记录在区块链全局状态中，钱包仅做查询、签名交互工具。
    
2.  钱包分类
    
    -   热钱包（MetaMask、浏览器插件钱包）：联网便捷，适合小额、测试网交互，风险更高；
        
    -   冷钱包（硬件钱包）：私钥离线存储，适合主网大额资产长期持有。
        
3.  账户四要素关系
    
    助记词 → 推导私钥 → 私钥生成公钥 → 公钥哈希得到钱包地址
    
    -   可公开：钱包地址；绝对保密：助记词、私钥。
        

2\. 基础区块链概念

-   外部账户（EOA）：用户钱包地址，可发起交易、持有资产；
    
-   合约账户：部署在链上的智能合约地址，无私钥，仅能被外部账户调用；
    
-   区块：批量打包多笔交易的数据单元，包含区块高度、哈希、Gas 消耗、交易列表；
    
-   交易：链上状态变更最小单元，转账、合约调用、NFT 铸造均属于交易，上链后永久不可篡改；
    
-   Gas：执行交易的手续费，每条 EVM 链独立设定 Gas 价格，Gas 不足会导致交易失败。
    

### （三）Monad 官方文档 [https://docs.monad.xyz/](https://docs.monad.xyz/)

1\. Testnets 测试网文档 [https://docs.monad.xyz/developer-essentials/testnets](https://docs.monad.xyz/developer-essentials/testnets)

1.  Monad Testnet 定位：面向开发者的 EVM 兼容一层公链测试环境，用于提前测试合约、DApp，代币无真实价值。
    
2.  钱包添加网络官方标准参数（MetaMask 配置必填）
    
    表格
    
    | 配置项 | 参数值 |
    | --- | --- |
    | 网络名称 | Monad Testnet |
    | RPC URL | https://testnet-rpc.monad.xyz |
    | Chain ID | 10143 |
    | 原生代币符号 | MON |
    | 区块浏览器 | https://testnet.monadexplorer.com |
    
3.  测试币水龙头：官方水龙头用于领取 MON 测试币，支付交易 Gas；仅官方渠道有效，第三方假水龙头会窃取钱包权限。
    
4.  链特性：并行执行 EVM，出块速度约 1 秒，TPS 性能远高于以太坊，测试网完全复刻主网交易逻辑。
    

2\. Block Explorers 区块浏览器文档 [https://docs.monad.xyz/tooling-and-infra/block-explorers](https://docs.monad.xyz/tooling-and-infra/block-explorers)

1.  浏览器作用：区块链公开可视化查询工具，所有链上数据永久公开可查，无删除权限。
    
2.  四大核心查询功能：
    
    -   地址查询：查看钱包余额、全部历史交易、代币授权记录；
        
    -   交易哈希 (TxHash) 查询：单条交易完整明细；
        
    -   区块查询：批量区块内所有交易、Gas 消耗统计；
        
    -   合约查询：合约代码、ABI、读写交互记录。
        
3.  交易解读核心字段：交易状态 Success/Failed、发送方 From、接收方 To、转账金额、Gas 消耗、区块高度、Input 调用数据。
    

### （四）以太坊钱包与交易底层文档

1\. Wallets 钱包 [https://ethereum.org/en/wallets/](https://ethereum.org/en/wallets/)

EVM 钱包通用标准：所有兼容以太坊的链（含 Monad）共用同一套账户、签名逻辑，MetaMask 可一键切换多链；钱包核心能力：账户创建、交易签名、链网络切换、代币资产查询。

2\. Transactions 交易底层 [https://ethereum.org/en/developers/docs/transactions/](https://ethereum.org/en/developers/docs/transactions/)

1.  三类 EVM 交易：
    
    -   普通转账：To 为钱包地址，Data 字段为空；
        
    -   合约交互：To 为智能合约地址，Data 存储调用函数与参数；
        
    -   合约部署：无 To 地址，Data 为合约源代码。
        
2.  交易固定关键字段：
    
    -   Nonce：账户交易序号，防止重复交易；
        
    -   Gas Limit：允许消耗的最大 Gas；
        
    -   Value：转账代币数量；
        
    -   签名：私钥签名证明账户所有权，未签名交易无法上链。
        
3.  基础转账固定消耗 21000 Gas，合约交互根据逻辑复杂度消耗更多 Gas。
    

3\. Gas 手续费机制 [https://ethereum.org/en/developers/docs/gas/](https://ethereum.org/en/developers/docs/gas/)

1.  EIP1559 手续费模型（Monad 同步沿用）
    
    总手续费 = Base Fee（协议基础费）+ Priority Fee（矿工优先打包小费）
    
2.  Gas Limit 作用：设置上限避免恶意合约无限消耗资产；若实际消耗 Gas 超过 Limit，交易失败，已消耗 Gas 不予退还。
    
3.  手续费逻辑：网络拥堵时 Base Fee 自动上涨，可提高 Priority Fee 加快交易确认。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->



### 以太坊钱包与交易底层文档

1\. Wallets 钱包 [https://ethereum.org/en/wallets/](https://ethereum.org/en/wallets/)

EVM 钱包通用标准：所有兼容以太坊的链（含 Monad）共用同一套账户、签名逻辑，MetaMask 可一键切换多链；钱包核心能力：账户创建、交易签名、链网络切换、代币资产查询。

2\. Transactions 交易底层 [https://ethereum.org/en/developers/docs/transactions/](https://ethereum.org/en/developers/docs/transactions/)

1.  三类 EVM 交易：
    
    -   普通转账：To 为钱包地址，Data 字段为空；
        
    -   合约交互：To 为智能合约地址，Data 存储调用函数与参数；
        
    -   合约部署：无 To 地址，Data 为合约源代码。
        
2.  交易固定关键字段：
    

Nonce：账户交易序号，防止重复交易；

Gas Limit：允许消耗的最大 Gas；

转账代币数量；

-   签名：私钥签名证明账户所有权，未签名交易无法上链。
    

1.  基础转账固定消耗 21000 Gas，合约交互根据逻辑复杂度消耗更多 Gas。
    

3\. Gas 手续费机制 [https://ethereum.org/en/developers/docs/gas/](https://ethereum.org/en/developers/docs/gas/)

1.  EIP1559 手续费模型（Monad 同步沿用）
    
    总手续费 = Base Fee（协议基础费）+ Priority Fee（矿工优先打包小费）
    
2.  Gas Limit 作用：设置上限避免恶意合约无限消耗资产；若实际消耗 Gas 超过 Limit，交易失败，已消耗 Gas 不予退还。
    
3.  手续费逻辑：网络拥堵时 Base Fee 自动上涨，可提高 Priority Fee 加快交易确认。
    

### 区块浏览器解析交易

1.  复制 TxHash，打开 Monad 区块浏览器搜索；
    
2.  逐项核对：交易状态、发送 / 接收地址、转账金额、Gas 消耗、区块高度、Input 调用数据；
    
3.  记录实操结论：链上所有操作永久公开留存，无法删除，是唯一链上凭证。
<!-- DAILY_CHECKIN_2026-07-08_END -->
<!-- Content_END -->
