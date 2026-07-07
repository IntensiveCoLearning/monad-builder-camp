---
timezone: UTC+8
---

# lij818556-hash

**GitHub ID:** lij818556-hash

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
[https://zcn6rqjg1z5s.feishu.cn/wiki/HiupwekhEiAaJ1kT28ucM2CEnad?from=from\_copylink](https://zcn6rqjg1z5s.feishu.cn/wiki/HiupwekhEiAaJ1kT28ucM2CEnad?from=from_copylink)

飞书链接  
  
DAY3 ：以太坊协议层开发，离普通开发者到底有多远？

# 概念1：EPF7 活动：并非单纯的上课，而是一个自驱项目。（后面的序号代表第几期）

EPF = Ethereum Protocol Fellowship，以太坊协议奖学金/协议研究员计划。更多依靠自己的跟进。（参与需要使用英文进行面试）

EPS = Ethereum Protocol Studies，中文可以理解为：以太坊协议学习计划 / 协议研究学习小组。更多用于新手、学生的入门学习。用于为EPF输送人才。

## 核心任务：

-   **改进以太坊本身** 比如提升性能、安全性、去中心化程度、验证者机制、网络通信效率等。
    
-   **参与协议升级** 研究和实现以太坊未来升级方向，比如账户抽象、PBS、Danksharding、Verkle Tree、EVM 优化等。
    
-   **开发和维护客户端** 以太坊不是一个单一软件，而是由多个客户端共同运行，例如 Geth、Nethermind、Lighthouse、Prysm 等。EPF 参与者可能会为这些客户端贡献代码、测试或研究。
    
-   **做协议研究和工程实现** 不是只写论文，而是把研究变成可以被以太坊网络采用的技术方案。
    

# 概念2：EIP = Ethereum Improvement Proposals，以太坊改进提案。

### 定义：任何人如果想对以太坊协议、标准或生态规则提出正式修改，都可以写成一份 EIP。

### EIP的具体包含什么：

标题 Title 这个提案要解决什么问题

作者 Authors 谁提出这个 EIP

状态 Status Draft、Review、Final、Stagnant、Withdrawn 等

类型 Type Core、Networking、Interface、ERC、Meta、Informational 等

摘要 Abstract 用简短语言概括这个提案

动机 Motivation 为什么以太坊需要这个改进

规范 Specification 最核心部分：具体技术规则怎么改

理由 Rationale 为什么选择这种方案，而不是其他方案

兼容性 Backwards Compatibility 会不会影响旧版本、旧合约、旧客户端

安全考虑 Security Considerations 可能带来的攻击风险、安全隐患

测试 Tests 如何验证这个改动有效

参考实现 Reference Implementation 代码层面如何实现

版权 Copyright 通常采用 CC0 公共版权声明

### 一个具体的EIP网址：[https://eips.ethereum.org/EIPS/eip-4844?utm\_source=chatgpt.com](https://eips.ethereum.org/EIPS/eip-4844?utm_source=chatgpt.com)

该EIP目标：Layer2 想扩容，但把数据提交到以太坊主网太贵。（layer1和laye2区别：Layer2 是为了帮以太坊“分担交易压力”的扩容方案，具体案例：每一笔都要付主网 Gas 费，成本很高；而Layer2 先把这 10,000 笔交易打包处理，然后把压缩后的数据或结果提交到以太坊主网。）

该EIP内容：Layer2 可以处理大量交易，但为了继承以太坊的安全性，它仍然要把关键数据发回以太坊。而过去这些数据主要通过 calldata 提交，费用较高，所以 EIP-4844 引入 Blob 来降低这部分成本。

### 选择EIP项目的时候，先选择问题域：再深度解读和解决问题

并不需要了解完整的协议，可以一边学习一遍构建具体的EIP。

### 当前EIP提案总量：1175个左右。总量相对来说其实很少。

### EIP提交前提：

任何人都可以：

1.  **提出一个以太坊改进想法**
    
2.  **在社区先讨论**
    
3.  **按照 EIP-1 的格式写成正式文档**
    
4.  **通过 GitHub 向 Ethereum/EIPs 仓库提交 Pull Request**
    
5.  **等待 EIP Editors 审查格式、范围和规范性**
    
6.  **一般是在几个月到1年左右的周期。**
    

# EIP 和 EPF 的区别：

| 对比 | EIP | EPF |
| 全称 | Ethereum Improvement Proposal | Ethereum Protocol Fellowship |
| 中文理解 | 以太坊改进提案 | 以太坊协议奖学金/协议研究员计划 |
| 本质 | 一份正式技术提案文档 | 一个培养协议开发者的项目 |
| 核心对象 | 技术方案、标准、协议改动 | 人才、开发者、研究者 |
| 作用 | 提出“以太坊应该怎么改” | 培养“谁来参与以太坊底层开发” |
| 输出成果 | EIP 文档、规范、标准 | 研究成果、代码贡献、协议项目、开发者成长 |
| 例子 | EIP-1559、EIP-4844、ERC-20 | EPF5、EPF6、EPF7 |

# 概念3：ETH 开发本身与基于其构建的Metamask钱包的区别。

更多是内容（技术）与商业化（产业）的关系。

# 概念4：区块链不可能三角：去中心化——无限拓展——降低成本。

# 概念5：安装部一个NFT并且上线。

NFT测试上线专用钱包私钥  
  
已经铸造一个NFT地址，等待后续进一步关联图片

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/lij818556-hash/images/2026-07-07-1783434128514-image.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
