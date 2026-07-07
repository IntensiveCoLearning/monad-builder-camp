---
timezone: UTC+8
---

# Yue05X

**GitHub ID:** yue05x

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# EPF（Ethereum Protocol Fellowship）协议层学习与贡献经验分享笔记

> 主题：以太坊协议层学习路线、EPF 申请流程、开源贡献经验  
> 分享人：A  
> 核心方向：Ethereum Protocol / Client Development / Research

* * *

# 一、以太坊协议层是什么？

以太坊是一个复杂的大型系统，不只是智能合约。

它包含：

-   共识机制（Consensus）
    
-   网络协议（Networking）
    
-   密码学（Cryptography）
    
-   执行层（Execution Layer）
    
-   共识层（Consensus Layer）
    
-   节点网络（P2P Network）
    

如果想深入学习以太坊底层，需要理解：

-   区块如何产生
    
-   节点如何通信
    
-   交易如何执行
    
-   共识如何达成
    
-   数据结构如何设计
    

* * *

# 二、EPF（Ethereum Protocol Fellowship）介绍

## EPF是什么？

EPF：

Ethereum Protocol Fellowship

即：

以太坊协议开发者培养计划。

主要目标：

培养能够参与 Ethereum 协议层开发的人才。

方向：

-   Ethereum Client Development
    
-   Protocol Research
    
-   EIP 提案
    
-   Core Development
    

* * *

# 三、EPF申请流程

## 时间流程

一般：

1.  自学准备
    
2.  提交申请
    
3.  面试
    
4.  Fellowship 项目
    

整个周期较长。

如果成功进入：

大约持续：

5个月左右。

* * *

# 四、EPF学习特点

相比智能合约：

EPF难度更高。

原因：

涉及：

-   数学
    
-   密码学
    
-   分布式系统
    
-   网络协议
    
-   数据结构
    

尤其：

Execution Layer

Consensus Layer

都有大量底层知识。

* * *

# 学习建议

不要直接硬啃官方文档。

推荐：

## AI辅助学习

利用 AI：

-   解释英文文档
    
-   分析代码
    
-   梳理协议流程
    
-   帮助理解复杂概念
    

因为官方资料：

-   大量英文
    
-   偏技术规范
    
-   阅读门槛较高
    

* * *

# 五、学习资源

## 1\. Ethereum 官方 Spec

重点：

### Consensus Layer Spec

适合：

研究共识机制。

* * *

### Execution Layer Spec

适合：

研究执行流程。

* * *

## 2\. GitHub

非常重要。

Ethereum 核心讨论：

主要集中在 GitHub。

包括：

-   Issue
    
-   PR
    
-   Protocol Discussion
    
-   Research
    

很多研究成果公开。

* * *

## 3\. Discord

Ethereum 有专门协议学习 Discord。

可以：

-   提问 Mentor
    
-   和开发者交流
    
-   获取学习建议
    

* * *

# 六、EPF学习路线

## 第一阶段：基础学习

学习：

-   Ethereum 基础架构
    
-   Execution Layer
    
-   Consensus Layer
    
-   Protocol
    

目标：

理解：

以太坊整体运行流程。

* * *

## 第二阶段：持续输出

学习过程中：

每周整理一次学习笔记。

例如：

-   学到了什么
    
-   遇到什么问题
    
-   对协议的理解
    

可以：

发布到社区。

并：

@ Ethereum Mentor

获取反馈。

* * *

## 第三阶段：代码探索

开始阅读客户端代码。

Ethereum 有多个客户端：

例如：

-   Geth
    
-   Nethermind
    
-   Besu
    
-   Erigon
    

推荐：

## Geth（Go Client）

原因：

-   使用最广
    
-   最成熟
    
-   资料最多
    
-   更容易理解
    

* * *

# 七、从零开始贡献开源

不要一开始挑战大项目。

推荐：

从小 PR 开始。

* * *

## 初期贡献方式

例如：

### 修改错误文件

-   修复 bug
    
-   修改文档
    
-   优化代码
    

* * *

### 小范围代码优化

例如：

替换旧接口。

或者：

优化数据结构。

* * *

逐渐：

从：

小修改

↓

测试

↓

代码贡献

↓

协议理解

* * *

# 八、EPF面试准备

面试可能涉及：

## 1\. 你如何理解 Ethereum？

需要理解：

-   Ethereum架构
    
-   Execution Layer
    
-   Consensus Layer
    
-   节点网络
    

* * *

## 2\. 你怎么看 Ethereum 未来发展？

需要准备：

例如：

-   Layer2发展
    
-   ZK技术
    
-   模块化区块链
    
-   去中心化方向
    

* * *

## 3\. 英语能力

面试：

英文进行。

要求：

-   英文听力
    
-   技术交流能力
    

建议：

提前适应英文技术讨论。

* * *

# 九、客户端学习路线

推荐：

## Geth Client

原因：

-   成熟
    
-   社区大
    
-   使用广泛
    

学习：

1.  阅读代码结构
    
2.  理解模块关系
    

例如：

```

交易  
↓  
执行层  
↓  
状态更新  
↓  
共识确认  
↓  
网络传播

```

* * *

# 十、EIP学习

EIP：

Ethereum Improvement Proposal

即：

以太坊改进提案。

类似：

协议升级方案。

* * *

学习方式：

不要一开始随便看。

建议：

选择感兴趣方向：

例如：

-   执行层
    
-   数据结构
    
-   ZK
    
-   Gas优化
    

深入研究。

* * *

注意：

EIP 文档像论文。

刚开始：

理解困难。

可以：

-   AI辅助阅读
    
-   按顺序学习
    
-   查看社区讨论
    

* * *

# 十一、进入EPF后的经验分享

## 1\. 项目范围一定要小

常见误区：

刚开始：

想做很多东西。

例如：

-   这个想研究
    
-   那个也想改
    

结果：

没有深入。

建议：

选择一个小方向。

* * *

## 2\. 面试前熟悉一个项目

面试会问：

“你对哪个项目感兴趣？”

因此：

提前：

深入研究一个项目。

包括：

-   背景
    
-   代码结构
    
-   为什么需要这个修改
    

* * *

## 3\. 连续贡献比零散贡献重要

建议：

至少：

2~3 个相关 PR。

体现：

持续投入。

不要：

今天改客户端。

明天研究钱包。

后天研究 DeFi。

这样容易显得方向不明确。

* * *

# 十二、PR提交经验

## PR不要太大

错误：

一次修改：

几百行代码。

问题：

-   Review困难
    
-   社区维护者压力大
    
-   合并概率降低
    

* * *

推荐：

小PR。

例如：

-   一个文件
    
-   一个问题
    
-   一个功能优化
    

优势：

-   容易Review
    
-   容易交流
    
-   更容易被接受
    

* * *

# 十三、AI在协议学习中的应用

AI可以帮助：

## 阅读代码

例如：

让 AI：

“帮我解释 Geth 里面这个模块作用。”

* * *

## 寻找贡献机会

例如：

告诉 AI：

“帮我找 Ethereum Client 中适合新手贡献的问题。”

* * *

## 理解复杂协议

例如：

解释：

-   Consensus流程
    
-   数据结构
    
-   EIP设计
    

* * *

# 十四、总结

## EPF适合什么人？

适合：

-   想深入区块链底层
    
-   喜欢协议研究
    
-   想参与 Ethereum Core Development
    
-   想成为 Blockchain Engineer
    

* * *

## 学习路线总结

```

Ethereum基础  
↓  
Consensus / Execution Layer  
↓  
阅读官方Spec  
↓  
学习Geth源码  
↓  
关注GitHub Issue  
↓  
小PR贡献  
↓  
持续输出Research  
↓  
申请EPF

```

* * *

## 核心建议

1.  不要急着贡献大代码
    
2.  从小PR开始
    
3.  使用AI降低学习门槛
    
4.  每周整理学习笔记
    
5.  找一个方向持续深入
    
6.  英文技术能力非常重要
    

> Ethereum协议学习是一条长期路线，但公开资源丰富，只要持续积累，可以逐渐参与真正的区块链基础设施建设。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

学习到合约是运行在区块链上的执行规则，因为开发者不能随意修改自己已经部署的内容，所以比传统后端（可能会被公司删改数据、封号）要更可靠。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
