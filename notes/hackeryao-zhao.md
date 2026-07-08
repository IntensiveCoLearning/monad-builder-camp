---
timezone: UTC+8
---

# hackeryao-zhao

**GitHub ID:** hackeryao-zhao

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
学习了以太坊底层协议
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

一、以太坊认识

1、基于账户模型，也可以称为基于交易驱动的账户状态变更机器(transaction-driven status machine)

2、共识机制变更：

以太坊1.0：基于POW工作量证明

以太坊2.0：基于POS权益证明的方式，通过质押32ETH称为验证者，通过一定的随机算法从验证者们中抽取出提议者，获取记账权。

二：账户分类

1、外部账户EOA：和比特币‘账户’创建的方式相同，本地通过公私钥完成创建。

2、智能合约账户：

(1)概念：可以理解为一个机器人，它包含智能合约代码+私有状态数据，只不过部署在链上，本身不可以发起交易；

(2)关键特征：

-   没有私钥进行签名，不可发起交易(只能被调用)；
    
-   拥有独立以太坊地址，0x开头，后面跟40位十六进制；
    
-   内部永久存贮状态Storage，数据结构是MPT树；
    
-   内置可执行代码(字节码)；
    
-   智能合约账户本身不支持支付gas费用，gas费用由调用合约函数的发起者(上文中的外部账户EOA支付)。
    

二、以太坊数据结构

状态数＋交易树+收据树都是采用MPT树结构

1、状态树：存贮的是账户和状态(banlance，nonce，codeHah,stroage)映射关系；

2、交易树：每一个区块有自己独属的交易树，每个区块间互不影响；
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->


### flag:

1.  今天开始学习，一个月后成为大佬
    
2.  奋斗百天，重回巅峰
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
