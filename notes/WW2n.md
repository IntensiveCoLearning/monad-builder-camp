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
