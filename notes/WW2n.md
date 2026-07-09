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
