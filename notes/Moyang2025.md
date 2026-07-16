---
timezone: UTC+8
---

# Moyang2025

**GitHub ID:** Moyang2025

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
今日主要了解了改进提案和治理提案

| 名称 | 全称 | 所属范围 | 主要解决什么问题 |
| --- | --- | --- | --- |
| EIP | Ethereum Improvement Proposal | 以太坊 | 修改以太坊协议、客户端接口、流程或应用标准 |
| ERC | Ethereum Request for Comments | 以太坊应用层 | 规定代币、NFT、账户、钱包等智能合约的通用接口 |
| MIP | Monad Improvement Proposal（在 Monad 语境下） | Monad | 修改 Monad 协议、生态标准或治理流程 |
| 治理提案 | Governance Proposal | 某个 DAO 或协议 | 决定资金使用、参数调整、人员任命、合约升级等具体事务 |

## **EIP：以太坊的“制度与技术修订草案”**

EIP 是 **Ethereum Improvement Proposal，以太坊改进提案**。

任何人都可以提出一个 EIP，用来说明：

-   当前存在什么问题；
    
-   建议进行什么改变；
    
-   具体技术规范是什么；
    
-   为什么要这样设计；
    
-   有哪些安全风险和兼容性问题。
    

以太坊官方将 EIP 定义为一种设计文档，用来向社区提供信息，或者描述以太坊及其流程、环境中的新功能。提出者需要推动社区共识，并记录反对意见。

## ERC：EIP 中面向应用层的“通用产品标准”

ERC 是 **Ethereum Request for Comments**。

它主要规定应用和智能合约之间应该怎样交互。例如：

-   一个代币应该有哪些函数；
    
-   钱包如何查询余额；
    
-   NFT 如何转移；
    
-   DApp 如何识别某类资产；
    
-   不同协议如何共享同一种接口。
    

最著名的是 **ERC-20**。它规定了同质化代币需要提供的一组基础接口，例如：

```
balanceOf()
transfer()
approve()
transferFrom()
allowance()
```

正因为 USDC、UNI、LINK 等代币都遵循类似的 ERC-20 接口，钱包和交易所才不需要为每个代币重新设计一套连接方式。ERC-20 的目的就是让代币可以被钱包、去中心化交易所和其他应用重复使用和组合。

| ERC | 用途 |
| --- | --- |
| ERC-20 | 同质化代币 |
| ERC-721 | NFT |
| ERC-1155 | 同一合约中管理多种同质化和非同质化资产 |
| ERC-4626 | 代币化金库 |
| ERC-4337 | 账户抽象相关标准 |
| ERC-8004 | 无需预先信任的 AI Agent 发现与交互标准 |

### EIP 和 ERC 的关系

可以将它们理解为：

> **EIP 是总类别，ERC 是其中专门面向应用层标准的一类。**

可以用“国家标准体系”类比：

-   EIP：所有标准和制度修订提案；
    
-   ERC：其中的产品接口和行业标准。
    

因此，ERC-20 既属于以太坊改进提案体系，又是一项应用层合约标准。

但并不是所有 EIP 都是 ERC：

-   修改共识机制的提案是 EIP，但不是 ERC；
    
-   修改 Gas 机制的是 EIP，但不是 ERC；
    
-   规定代币接口的通常属于 ERC。
    

## 治理提案：DAO 对具体事务作出的“集体决策”

“治理提案”不是某一条链专属的技术标准，而是一个更宽泛的概念。

它通常指 DAO 或协议成员提出的一项具体决策，例如：

-   是否拨款 100 万 USDC 支持某个项目；
    
-   是否提高某种资产的抵押率；
    
-   是否增加新的交易市场；
    
-   是否升级某个智能合约；
    
-   是否更换预言机；
    
-   是否任命或罢免某个委员会成员；
    
-   是否修改代币激励比例；
    
-   是否将国库资金投入某个流动性池。
    

一个典型的链上治理提案可能经历：

```
社区讨论
   ↓
形成正式提案
   ↓
达到提案门槛
   ↓
链上投票
   ↓
达到法定人数并获得多数支持
   ↓
进入时间锁
   ↓
智能合约自动执行
```

## 改进提案与治理提案的核心区别

### 改进提案更强调“规则和规范”

EIP、ERC、MIP 通常会详细说明：

-   技术规范；
    
-   接口定义；
    
-   设计理由；
    
-   安全考虑；
    
-   向后兼容性；
    
-   实现方式。
    

它首先是一份**标准化设计文档**。

### 治理提案更强调“集体作出决定”

治理提案通常关注：

-   是否批准某项行动；
    
-   是否使用国库资金；
    
-   是否调整某项参数；
    
-   是否部署或升级合约。
    

它首先是一项**组织决策**。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->

| An NFT internet  一个非同质化代币网络 | The internet today  如今的互联网 |
| --- | --- |
| You own your assets! Only you can sell or swap them.你的资产属于你！只有你有权出售或交换这些资产。 | You rent an asset from some organization and it can be taken away from you.你从某个机构租借了一处资产，而这些资产最终可能会被夺走。 |
| NFTs are digitally unique, no two NFTs are the same.NFTs 具有独特的数字特性，没有两个 NFT 是完全相同的。 | A copy often cannot be distinguished from the original.一份副本往往与原件难以区分。 |
| Ownership of an NFT is stored on the blockchain for anyone to verify publicly.非同质化代币的所有权信息被存储在区块链上，任何人都可以公开验证。 | The access to ownership records of digital items is controlled by institutions – you must take their word for it.数字物品的所有权记录访问权限由相关机构控制——你必须相信这些机构的说法。 |
| NFTs are   NFTs 就是这样的技术。smart contracts  智能合约 on Ethereum. This means they can easily be used in other smart contracts and apps on Ethereum!它们在以太坊上运行。这意味着它们可以轻松地被用于以太坊上的其他智能合约和应用程序中！ | Companies with digital items usually require their own "walled garden" infrastructure.拥有数字产品的公司通常需要自己的“围墙花园”基础设施。 |
| Content creators can sell their work anywhere and can access a global market.内容创作者可以将他们的作品销售到任何地方，并进入全球市场。 | Creators rely on the infrastructure and distribution of the platforms they use. These are often subject to terms of use and geographical restrictions.创作者们依赖他们所使用的平台的基础设施与分发机制。而这些平台的使用条款和地理限制往往也是无法规避的。 |
| NFT creators can retain ownership rights over their own work, and program royalties directly into the NFT contract.NFT 创作者可以保留对自己作品的所有权，并将版税直接纳入 NFT 合同中。 | Platforms, such as music streaming services, retain the majority of profits from sales.像音乐流媒体服务这样的平台，仍然能够从销售收益中获得大部分利润。 |

主要进一步了解了NFT相关内容，扩展了自己的思考
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->


初步地了解了Moss这个项目；回顾了有关Tokenomics的一些学习资料  
人们进入你的生态系统的一个最重要的原因是什么？这是网络的最有价值的交互（Most Valuable Interaction，MVI），代币设计的重点是激励这种 MVI 的可持续反馈循环。  

![image.png](attachment:30dd7ac8-954f-40b9-a7ae-208885ca5dce:image.png)![image.png](attachment:b5a63ee7-6373-4bb0-bd0e-65954f622672:image.png)

链接：[https://mp.weixin.qq.com/s/HQ4cU9mv6VfcaD8WZKX3kw](https://mp.weixin.qq.com/s/HQ4cU9mv6VfcaD8WZKX3kw)
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->




继续自行了解相关领域的研究，今天主要是预言机和多代币系统
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->





完成了week1 的Demo，给下一周的思考定了基本的方向，细化了一些细节
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->






对自己初步构思的web3产品进行了深入思考，争取明天做出初版的demo
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->







根据提供的资料，更深入地了解了Monad的特点。根据任务的指引，思考并尝试构建了一个简单的适合Monad的交互场景。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->








根据分享和Co-Learning中的内容，了解了一些新东西；继续了解Monad
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->









对于AI Agent在web3支付领域的应用有了初步了解
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->










用ai agent写了第一份智能合约，并完成了部署
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->











观看了昨天开营式的回放，了解了本次共学的计划和目标，我本人对于本次共学也有了相应的期望，并且预想了接下来的学习计划。听了XiaoHai的分享，对于他的经历和研究领域有了一些了解。参加了7.06Co-learning，跟着分享人的步骤操作，尝试创建了自己的钱包，接下来会继续跟着任务推进，去完成一笔交易
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
