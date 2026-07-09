---
timezone: UTC+8
---

# HerschelLi-31

**GitHub ID:** HerschelLi-31

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
下面是一份可以直接提交/整理到学习记录里的 **Day 4 学习笔记**。你可以把后面 `transaction hash`、截图链接、区块浏览器查询结果再补进去。

* * *

# **Day 4 学习笔记：AI + Solidity + 合约部署**

今天我完成了第一次 Solidity 最小合约实践，并将合约部署到了 Monad Testnet。

本次部署信息如下：

```text
Contract name: MyFirstDapp
Network: Monad Testnet
Contract address: 0x185C9C7a2a8d7F14Bb881A0E738E1Dc5b327d7D1
Deploy transaction hash: 0x81f8179e46a442d8918b637b9a61e80378cadfae67b67f2756a853245060c914
```

* * *

## **1\. 我完成了什么**

在 Remix 中创建了一个 Solidity 合约文件，文件后缀为 `.sol`。合约名称是 `MyFirstDapp`，主要功能是保存和修改一段链上 message。

* * *

## **2\. Remix 部署流程记录**

我在 Remix 中完成了以下步骤：

```text
1. 新建 Solidity 文件，例如 MyFirstDapp.sol
2. 写入 Solidity 合约代码
3. 在 Solidity Compiler 中选择合适版本并编译
4. 在 Deploy & Run Transactions 中连接 MetaMask
5. 将网络切换到 Monad Testnet
6. 输入 constructor 参数
7. 点击 Deploy
8. 在 MetaMask 中确认交易
9. 获得合约地址和部署交易 hash
```

* * *

## **3\. 合约地址**

部署成功后，我得到了一个新的合约地址：

```text
0x185C9C7a2a8d7F14Bb881A0E738E1Dc5b327d7D1
```

这个地址不是我的钱包地址，而是智能合约部署到链上之后生成的地址。

我的钱包地址代表“我这个用户”；合约地址代表“链上的这个程序”。之后如果我要和这个合约交互，就需要通过这个合约地址找到它。

所以可以理解为：

```text
钱包地址 = 用户身份
合约地址 = 链上程序的位置
```

* * *

## **4\. Deploy transaction hash**

我的部署交易 hash 是：

```text
0x81f8179e46a442d8918b637b9a61e80378cadfae67b67f2756a853245060c914
```

这个 hash 记录的是“我把 MyFirstDapp 合约部署到 Monad Testnet 上”这件事。

部署合约本身也是一笔链上交易，因为它改变了区块链状态：原本链上没有这个合约，交易成功后，链上新增了一个合约地址和对应的合约代码。

因此，部署合约需要钱包签名，也需要消耗 gas。

* * *

## **5\. Read function 和 Write function 的区别**

**Read function** 只读取链上已有状态，不改变区块链数据。例如读取当前 message、owner、updateCount。这类操作通常不需要钱包确认，也不会产生新的 transaction hash。

**Write function** 会修改链上状态。例如调用 `setMessage` 修改 message，这会改变合约内部存储的数据。因此它需要钱包签名，需要支付 gas，也会产生 transaction hash。

简单来说：

```text
Read = 看链上数据
Write = 改链上数据
```

只要是“改”，就通常需要发交易。

* * *

## **6\. ABI 的作用**

ABI 可以理解为合约和外部世界沟通的说明书。

Solidity 源码是给人和编译器看的，但前端、钱包、Remix 或其他程序要调用合约时，通常需要 ABI。ABI 会告诉外部程序：

```text
这个合约有哪些函数
函数名字是什么
函数需要什么参数
函数返回什么数据
哪些函数是 read
哪些函数是 write
```

所以一个前端 DApp 如果想调用我的合约，一般需要三个东西：

```text
合约地址 + ABI + 钱包/RPC
```

合约地址告诉前端“调用谁”，ABI 告诉前端“怎么调用”。

* * *

## **7\. 这次交易中 from / to / value / gas**

这次部署交易中：

```text
from = 我的钱包地址
to = 空，或者显示为 Contract Creation
value = 通常是 0 MON
gas = 部署合约消耗的计算费用
```

部署合约和普通转账不一样。普通转账的 `to` 是另一个钱包地址；但部署合约时，交易的目的不是给某个地址转钱，而是在链上创建一个新的合约。因此区块浏览器里可能会显示为 `Contract Creation`。

`value` 表示这笔交易附带转移了多少原生代币。我的部署交易主要是创建合约，不是转账，所以 value 通常是 0 MON。

`gas` 是执行这笔交易所需的计算资源费用。即使 value 是 0，只要这笔交易改变了链上状态，就仍然需要消耗 gas。

* * *

## **8\. 对智能合约的理解**

智能合约是部署在区块链上的程序。它可以保存状态，也可以通过函数改变状态。部署后，它会拥有自己的合约地址。用户通过钱包调用合约函数，read function 用来读取状态，write function 用来修改状态。

这次我的 `MyFirstDapp` 是一个最小合约，但它已经包含了 DApp 的核心结构：

```text
用户钱包 → 调用合约 → 修改链上状态 → 产生交易 hash → 在区块浏览器查询
```

也就是说，Remix 暂时充当了前端界面，而 Monad Testnet 负责执行和记录这次合约部署。

* * *

## **9\. 今日总结**

完成了从“普通链上交易”到“智能合约部署”的第一次实践。在 Remix 中创建 `.sol` 文件、编译 Solidity 合约、连接 MetaMask、选择 Monad Testnet，并成功部署 `MyFirstDapp` 合约。

本次实践最重要的收获是：  
**智能合约部署和合约交互本质上都是链上交易。**  
部署交易会创建一个合约地址；调用 write function 会修改合约状态；而 transaction hash 则是这些操作在链上的唯一记录。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

完成discord，telegram，notion，codex的安装和准备

安全与合规: 常见攻击方式包括：假面试软件、假奖学金空投、假客服、假 HR、群聊钓鱼链接、恶意浏览器插件、剪贴板劫持、地址污染等。最危险的是下载“专用面试软件”、连接钱包签名、输入助记词/私钥，或者安装来路不明的插件。

添加了monad的test net并领取了测试币
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->


区块链可以理解为很多节点共同维护的一本账本，交易被打包进区块，区块之间用哈希连接，所以历史记录很难被篡改。它的可信不是来自某个公司，而是来自分布式网络、共识机制和代币激励。比特币说明了“去中心化货币”如何运行，而公链、联盟链、私链的区别主要在于开放程度和控制权。

Web3 和 Web2 最大区别是：Web2 里账号、数据和规则主要由平台控制；Web3 里用户通过钱包控制身份和资产。以太坊把区块链从“记账系统”扩展成“可编程平台”。用户用钱包发起交易，交易消耗 Gas，EVM 执行智能合约，最终改变链上状态。ETH 不只是币，也是支付网络计算成本的燃料。以太坊从 PoW 转向 PoS 后，验证者通过质押 ETH 维护网络；扩容则主要依靠 Layer 2 和 Rollup
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
