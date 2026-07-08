---
timezone: UTC+8
---

# BareerahBenjamin

**GitHub ID:** BareerahBenjamin

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# RaptorCast

### 1\. 什么是 RaptorCast？

-   **定位**：它是 MonadBFT 共识机制中专用的**多播消息传递协议**，主要用于将区块提案（Block Proposals）从领导者（Leader）高效、可靠地传播给所有验证者（Validators）。
    
-   **核心技术**：采用 [RFC 5053](https://datatracker.ietf.org/doc/html/rfc5053) 标准的 **Raptor 纠删码（Erasure Coding）**。区块提案会被切分为多个数据块（Chunks），接收方只需收集到足够数量的任意数据块组合，即可完整解码出原区块。
    

### 2\. 核心架构与工作原理

-   **两级广播树（Two-Level Broadcast Tree）**：
    
    -   **根节点（Level 0）**：消息发起者（当前的 Leader）。
        
    -   **第一级（Level 1）**：由一个非领导者节点充当“中转站”。
        
    -   **第二级（Level 2）**：其他所有的验证节点。
        
-   **权益权重分配**：Leader 会根据验证者的**质押权益权重（Stake Weight）**，按比例将不同的连续数据块范围分发给不同的第一级节点，由它们负责向全网做第二跳（Second-hop）的广播。
    

### 3\. 设计优势与特性

-   **高带宽利用率**：通过两级树状分发，充分利用了**整个网络所有节点的上传带宽**，避免了 Leader 节点自身遇到带宽瓶颈。
    
-   **极低延迟**：最坏情况下的传播时间仅为网络中任意两点间最大往返时间（RTT）的两倍（双跳延迟）。
    
-   **高容错性（拜占庭容错）**：
    
    -   只要全网超过 **2/3** 的权益权重运行正常，就能保证消息的可靠送达。
        
    -   **无重传机制**：为了追求极致延迟，它不使用类似 TCP 的丢包重传，而是通过直接增加冗余数据块（Redundancy）来抵消网络丢包和恶意节点不转发带来的损失。
        
-   **完整性校验**：Leader 会对数据块生成默克尔树并进行签名，中间节点（第一级）在转发前会先验证签名，防止恶意篡改和垃圾信息轰炸。
    

### 4\. 其他应用场景

除了共识阶段的区块传播，RaptorCast 还被应用于：

-   **交易转发**：例如从全节点向后续的验证节点转发交易（仅需一跳，不重复广播）。
    
-   **二级全节点区块传播**：验证者在重构区块后，会以自身为根构建二级 RaptorCast 网络，向其连接的全节点（Full Nodes）平分数据块进行传播。
    

# **Asynchronous Execution  异步执行**

### 什么是异步执行？

-   **解耦共识与执行：** 传统区块链（如以太坊）采用“交错执行（Interleaved Execution）”，节点必须先执行完区块内的交易、算出状态根（State Root），才能对区块进行共识投票。而 Monad 将两者解耦，节点**只对交易的正式顺序达成共识，而不需要在投票前执行它们**。
    
-   **释放 100% 的区块时间：** 在以太坊中，12 秒的区块时间里只有约 100 毫秒（1%）留给执行。Monad 解耦后，执行可以占用**整个区块时间**，从而大幅提升了网络的交易吞吐量（TPS）。
    

* * *

### 关键技术机制

-   **确定性保证：** 尽管执行落后于共识，但只要交易的**顺序**确定了，最终的执行结果和状态就是完全确定的（即使部分交易因余额不足等原因失败，其失败结果也是确定的）。
    
-   **延迟默克尔根（Delayed Merkle Root）：**
    
    -   因为打包区块时还没执行，所以 Monad 的区块头不包含当前区块的状态根，而是包含 **$D$ 个区块之前（目前设为 3 个区块）** 的默克尔根。
        
    -   如果某个节点执行出错导致状态不一致，它会在 $D$ 个区块后因默克尔根对不上而脱离共识，并触发回滚和重新执行。
        
-   **储备余额（Reserve Balance）：** 为了防止节点打包无法支付 Gas 费的无效交易，Monad 引入了储备余额规则，在共识阶段对交易准入进行轻量化限制。
    
-   **投机性执行（Speculative Execution）：** 节点在收到新区块但尚未最终确认（Finalized）的间隙，会本地预先执行该区块。这使得用户可以针对最新的“投机状态”进行快速的智能合约模拟（如 `eth_call`）。
    

* * *

### 对用户的实际影响

-   **新资金账户的延迟限制：** 如果一个余额为 0 的新账户刚刚收到一笔转账，由于执行滞后于共识，该账户**不能立刻**发送交易，需要等待该转账区块进入 Proposed 状态并再等待约 **1.2 秒**（即 $D$ 个区块的延迟）。
    
    -   _替代方案：_ 可以通过智能合约将“注资”和“消费”合并在同一个交易中完成，从而避免这种延迟。
        

# 区块状态 **Block States**

### 1\. 四种区块状态

在 MonadBFT 中，由于采用**异步执行（Asynchronous Execution）**，区块的“最终确认（Finalization）”和“状态根验证（State Root Verification）”是分离的。从观察者的角度看，一个区块会经历以下四种状态：

-   **Proposed（已提案）：** 区块由 Leader 提出，但尚未被投票。如果执行没有滞后，节点可能会对该区块进行**投机性执行（Speculative Execution）**。
    
-   **Voted（已投票）：** 节点已获得该区块的法定人数证书（QC），意味着它已获得绝对多数验证者的赞成票。在此状态下，区块可以被**投机性最终确认**。
    
-   **Finalized（已最终确认）：** 节点获得了 QC-squared（即针对含有该区块 QC 的子区块的 QC）。这证明绝对多数验证者已批准该区块的有效性，在共识规则下，该区块**不可逆转**。
    
-   **Verified（已验证）：** 包含“延迟默克尔根（Delayed Merkle Root）”的区块已被最终确认，意味着绝对多数验证者已对该区块的执行输出达成一致。
    

* * *

### 2\. 与以太坊 JSON-RPC 承诺级别的映射

虽然 Monad 的共识算法不同于以太坊，但它在 API 上兼容 Geth 客户端的 JSON-RPC 接口。两者的状态映射关系如下：

| Geth RPC 状态标签 | 对应 Monad 区块状态 | 对应原因 |
| --- | --- | --- |
| "latest" | Proposed | 都指最新观察到、但尚未通过共识最终确认的区块。 |
| "safe" | Voted | 意味着极难被回滚，但理论上仍存在微小可能。 |
| "finalized" | Finalized | 含义完全相同，除非发生硬分叉，否则不可逆转。 |

> 📌 **注意：** Geth 中的 `"earliest"`（创世区块别名）和 `"pending"` 在 Monad 中并不适用，因为 Monad 的交易传播机制不同。

* * *

### 3\. 实时数据与投机性执行

Monad 提供的部分快速实时数据源会直接报告 **Proposed** 状态下的最新区块。因为这些区块属于“投机性执行”，在极少数罕见情况下，这些区块最终可能不会成为 Monad 区块链的一部分。若想精确追踪每个区块在共识中的状态演变，可以参考其扩展的 WebSocket 接口 `monadNewHeads`。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

# 异步 I / O（**Asynchronous I/O）**

CPU 在通信进行的同时继续并发执行其它操作。

# 管道（流水线技术，Pipelining）

通过将任务分解成一系列可以并行处理的较小任务来实现并行性的技术。

# MonadBFT

MonadBFT 不仅可以结合流水线式领导者BFT协议（pipelined leader-based BFT protocols）的所有特性，还可以抵抗其无法解决的尾部分叉（tail-forking）问题。具体实现了：

-   **推测性最终性通过一轮共识达成** ， **完全最终性通过两轮共识达成** 。
    
-   在正常情况下（即未发生故障时）， **消息和认证器的复杂度均为线性** 。这使得共识验证器集能够扩展到大量节点。
    
-   **乐观响应** ：在正常情况下以及从失败的回合中恢复时，无需等待最坏的网络延迟即可推进回合进程。
    
-   **领导者故障隔离：** 单个领导者故障只会导致一次超时延迟。所有其它轮次都能以网络允许的最快速度继续进行。这与现有的流水线式 BFT 协议不同，后者对于一个故障领导者会造成两次超时。
    
-   **抗尾分叉** ：内置的抗[**尾分叉保护机制，尾分叉**](https://docs.monad.xyz/monad-arch/consensus/monad-bft#no-tail-forking)是最大可提取价值（MEV）攻击的一种类型，恶意领导者可能会分叉掉其前一个区块。这解决了先前基于流水线领导者的 BFT 共识机制中的一个关键问题。
    

MonadBFT 中一个区块由一个轮次编号、有效载荷（有序的交易列表）和一个 [**质量控制（QC）**](https://docs.monad.xyz/monad-arch/consensus/monad-bft#quorum-certificate-qc) （用于验证父区块）组成。验证者使用 **BLS** 签名，它有签名聚合的特性，便于将多个签名聚合提高验证效率。

## 标准恢复机制中的特有概念

**提案：**

**提案 propose**

**新提案 fresh proposal**

包含新区块的[**提案**](https://docs.monad.xyz/monad-arch/consensus/monad-bft#proposal) ，即不受先前失败提案影响的提案

**重新提案 reproposal**

包含先前新提案中某个区块的[**提案**](https://docs.monad.xyz/monad-arch/consensus/monad-bft#proposal) ，当前领导人试图恢复或最终确定该新提案。重新提案的整数编号将大于其[**质量控制提案**](https://docs.monad.xyz/monad-arch/consensus/monad-bft#quorum-certificate-qc)的整数编号加 1

**小费：**

**小费 tip**

提案减去区块有效载荷后的结果

**高额小费 high tip**

给定一组小费，高额小费就是整数值最高的小费

**超时信息 Timeout Message**

验证器在预期时间内未从预定领导者处收到有效区块时生成的签名证明。超时消息证明缺少有效区块

**超时证书 Timeout Certificate (TC)**

当发生超时时，验证器开始发送和接收[**超时消息**](https://docs.monad.xyz/monad-arch/consensus/monad-bft#timeout-message) 。每个验证器都会累积收到的超时消息；如果累积到绝大多数此类消息，则会生成超时证书（TC））

**无背书消息和无背书证书（No-Endorsement Message and No-Endorsement Certificate）**

在特定情况下，领导者会向其他验证者索取与提示对应的完整提案（区块）。如果验证者没有该提案，他们会回复一条已签名的“不认可消息”；如果领导者在尝试恢复某个提示的提议时收到绝大多数的“不认可”消息，他们可以生成“不认可”证书——证明网络中的绝大多数人没有该提议。

## 块状态

1.  `Proposed` 建议的
    
2.  `Voted` 投票
    
3.  `Finalized` 最终版
    

第四个状态 `Verified` 是在 MonadBFT 之外，作为[**异步执行的**](https://docs.monad.xyz/monad-arch/consensus/asynchronous-execution)一部分实现的。

### 几种情况

Happy Path

快乐路径描述的是一个区块从被提议到最终完成的正常情况，没有任何超时或失败的轮次。

Unhappy Path（Fault Handling）

不愉快的路径描述的是领导者未能发出有效提案或 QC 构建者（下一任领导者）未能构建 QC 的异常情况。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->


今天完成了 week1 的大部分通用任务，熟悉了一些 Monad 生态
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
