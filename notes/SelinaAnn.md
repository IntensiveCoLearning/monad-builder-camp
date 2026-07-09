---
timezone: UTC+8
---

# SelinaAnn

**GitHub ID:** SelinaAnn

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/SelinaAnn/images/2026-07-09-1783601532564-image.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->

### **硬核解读：用 Block Explorer 拆解交易**

Block Explorer（区块浏览器）是链上的“搜索引擎”，它将底层区块链的数据翻译成人类可读的格式。将你的 Tx Hash 粘贴到 Monad 的 Block Explorer 搜索框中，你将看到如下核心字段（这是今天的**硬核知识点**）：

| 字段名 | 含义 | 今天这笔交易中的表现 |
| --- | --- | --- |
| Transaction Hash | 交易的唯一标识符。 | 你查询的那串字符。 |
| Status | 交易状态。 | 应为 Success（成功）。如果失败，会显示 Reverted 并给出原因。 |
| Block | 交易被打包进哪个区块。 | 一个数字（例如 #10245）。数字越大代表越新。 |
| From | 交易的发起方（签名者）。 | 你的课程专用钱包地址。 |
| To | 交易的接收方。(注意：如果是与智能合约交互，这里显示的是合约地址) | 你转账的对方地址 / DApp 的合约地址。 |
| Value | 转移的原生代币数量。 | 你转账的 MON 数量（如果是 0，说明是单纯的合约调用）。 |
| Transaction Fee | 你实际支付的手续费。 | Gas Used × Gas Price。虽然你用的是测试币，但这里的计算逻辑与主网完全一致。 |
| Gas Limit | 你愿意为这笔交易支付的最大 Gas 数量（由发起者设置）。 | 转账通常是 21,000。如果调用了复杂合约，这个数值会很高。 |
| Gas Used | 这笔交易实际消耗的 Gas 数量。 | 必须 ≤ Gas Limit。未用完的 Gas 会退还。 |

> 💡 **理解 Gas 的绝佳时机**： 在 Explorer 中对比 **Gas Limit** 和 **Gas Used**。你会发现普通转账的 Gas Used 刚好是 21,000。这就解释了为什么以太坊将基础转账手续费固定在这个数值——这是执行一次最简单转账所需的计算步骤数。

* * *
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->


![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/SelinaAnn/images/2026-07-07-1783431460944-image.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->



![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/SelinaAnn/images/2026-07-06-1783336548272-image.png)
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
