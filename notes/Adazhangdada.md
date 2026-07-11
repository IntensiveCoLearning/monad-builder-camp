---
timezone: UTC+8
---

# Xinyue Zhang

**GitHub ID:** Adazhangdada

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
周末~  
buildanything freshman课程完结打卡

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Adazhangdada/images/2026-07-11-1783780095126-image.png)
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

今天听了好多优秀的同学们的分享！学习学习学习。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->


# AI Agent安全

”无法审计的自主性“：AI Agent开始拥有资产权限、系统指令权、直接签署区块链交易

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTBjZTliY2I0Zjk0ZTYxYjMxYjc5ODFkNWQxNGI2ODZfN2JjdGdUWFVDWDFqcGVENmRSNE45WHJDYXp5SFFlRDNfVG9rZW46UGtoVmJ1MklUbzRwTWh4dks1RmNiU21wbmRnXzE3ODM2MDA5MjQ6MTc4MzYwNDUyNF9WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MDg5ZWVhYzhmODcxYzVjNWJkMDk4NDc5MjhjYjdlZTlfMUFTMWE5RWY5YlhXbjdRNWpCSGVMdlJ4alBOM3d5a2JfVG9rZW46VVFic2I2NjNab1JKYWF4RGIxdGNIQWhybnFmXzE3ODM2MDA5MjQ6MTc4MzYwNDUyNF9WNA&add_watermark=true&scene_type=CCM)

**授权加固建议：**

-   最小权限
    
-   Human-in-the-loop
    
-   部署安全监控工具
    

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YmFlM2FhYzlmYjUxNTdjZjYwMDNkMzRkYzg4NGRmODZfTUozRGk5NHp6Y3gxVTN3VlpqR1BMZVplTVlIR0Y3eWRfVG9rZW46UWdIcGJzT2ltb0J6TUx4S1VjUmNIcTZzbmVoXzE3ODM2MDA5MjQ6MTc4MzYwNDUyNF9WNA&add_watermark=true&scene_type=CCM)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->




## DAY3 buildanything入门学习

[https://buildanything.so/zh/tracks/freshman/lessons/what-is-monad](https://buildanything.so/zh/tracks/freshman/lessons/what-is-monad)

### monad章节 智能合约

应用到底是怎么在区块链上运行的？答案是：智能合约（smart contracts）。

在传统软件里，应用逻辑运行在你控制的服务器上。你可以随时修改、下线，也可以屏蔽某些用户。正因为如此，一个账号会被封禁，一笔支付会被冻结，一个平台也可能一夜之间被关停。

智能合约把这件事反过来。它是一段运行在区块链上的程序，而不是运行在某台服务器上。部署之后，任何人都不能随意更改或下线它，连作者也不行。它会按照写好的规则持续运行。

你可以把它理解成一台自动售货机。你投入钱，选择商品，机器把商品给你。不需要收银员，也没有店长可以临时决定不卖给你。规则被写进机器本身。智能合约也是这样：规则写在代码里，由区块链自动执行。

智能合约不关心是谁在调用、你在哪个国家，也不关心某个政府是否给数据中心发了函。**只要条件满足，它就会按代码执行，对所有人一视同仁。**

在 Monad 上，智能合约使用 Solidity 编写。Monad 运行在 EVM（Ethereum Virtual Machine，以太坊虚拟机） 上，和 Ethereum 使用同一套标准。这意味着所有为 Ethereum 编写的智能合约、工具和教程，无需任何修改就能在 Monad 上使用。

### 区块

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWIwYmQ1YmVlZjkyZDViMGZmMmE4NzQ4ZDJkNmY3OTRfa3BuaHU4R2g2S2VWcmJINjhPbmUzaGlrbDJZU3VaYlhfVG9rZW46UFhZNGJUeVZub2dHTWh4bjNDUWM1Nng0bnZoXzE3ODM1MTU3Njk6MTc4MzUxOTM2OV9WNA&add_watermark=true&scene_type=CCM)

### Proof of Work 与 Proof of Stake

达成共识的方式有很多种。其中最重要的两种是 Proof of Work（工作量证明）和 Proof of Stake（权益证明）。

Proof of Work 是最早的共识机制，被 Bitcoin 和早期的 Ethereum 采用。在 PoW 里，被称为矿工的节点彼此竞争，去解一道计算上很难的数学谜题。最先解出的那个矿工有权构建下一个区块并获得奖励。这道谜题难解但容易让其他人验证——这就是它的精妙之处。它之所以安全，是因为攻击网络需要巨量的计算资源，成本极高。

它的代价是：消耗巨量电能。Ethereum 一直使用 Proof of Work 直到 2022 年，在一个被称为 The Merge 的事件中切换到了 Proof of Stake——能耗下降了大约 99.95%。

Proof of Stake 是 Ethereum 现在使用的机制，Monad 也是。验证者不再比做题速度，而是用 ETH（在 Monad 上则是 MON）作为抵押（即他们的「stake」）来参与区块生产。他们会被随机抽取去提议新的区块，并对其他人提议的区块进行投票。

抵押是关键。如果一个验证者作弊——比如提议无效交易、或在状态上撒谎——他们就会失去自己质押的 ETH。这被称为 slashing（罚没）。失去质押资产的财务风险，让作恶变得极其昂贵，从而在不烧大量电的前提下保持网络安全。

### Gas

Gas（手续费）是你在区块链上执行交易所支付的费用。

你不会直接用 gas units 付费——你支付的是 ETH，在 Monad 上则是 MON。Gas 费的计算方式是：

```Plain
gas fee = gas units used x gas price
```

Gas price 会根据网络需求波动。当大量用户同时想交易时，gas price 上升；当网络冷清时则下降。这也是为什么你有时会看到 Ethereum 在大型 NFT 发售或市场剧烈波动等高流量事件期间，gas 费用突然飙升。

Gas 有两个作用。第一，它支付给处理你交易的验证者。第二，它防止垃圾信息——既然每笔交易都要花钱，就没法免费往网络里灌垃圾。

### Mainnet 与 Testnet

在所有事情之前，你要先搞清楚自己在和哪一个 Monad 网络交互。每条区块链都有多个网络，从外面看长得一样，行为却差别很大：

Monad Mainnet（主网）。 真实网络。真的 MON 代币、真的交易、真的成本。当你要把产品交付给真实用户时，就在这里。

Monad Testnet（测试网）。 一条单独的链，用于测试。Testnet 上的代币没有真实价值。你可以在这里无风险地试。

经验法则：所有实验都进行在 testnet 上。任何涉及真钱的东西才在 mainnet 上。 你不会希望在合约还没测过的情况下，就把真钱押上 mainnet 部署一遍。

### Faucet

Faucet（水龙头） 是一个免费发放 testnet 代币的网站。之所以需要 faucet，是因为 testnet 代币没有真实价值，所以没人会卖。Monad 团队会把它们送出来，让开发者可以测试。

你连接钱包或粘贴自己的地址，点一个按钮，过几秒钟就会有一小笔 testnet MON 到你的钱包。这样你就可以部署合约、发送交易、反复测试，而不必花真钱。

官方 Monad faucet： [faucet.monad.xyz](http://faucet.monad.xyz)

如果你在别处见到过更老的 HackQuest 上提供的 Monad faucet，跳过它。它已经不可靠了。用上面这个官方版本就好。

### 区块浏览器（Block Explorer）

Block explorer（区块浏览器） 是一个让你查询链上任何东西的网站：交易、钱包、合约、区块。它是你查看链上正在发生什么的窗口。

你会一直用 explorer：

-   「我那笔交易上链了吗？」 把 tx hash 粘进 explorer，看状态、消耗的 gas，以及任何报错。
    
-   「这个合约长什么样？」 把合约地址粘进去，如果它已经验证过，就能看代码，并查到所有针对它的交易。
    
-   「这个钱包里有多少 MON？」 把钱包地址粘进去，就能看到余额和完整的交易历史。
    

Explorer 也是你确认自己在「正确的链上」最快的方式。如果你正在调试 testnet 的部署，结果却看着 mainnet 的 explorer，你会看到一个空钱包然后纳闷哪里出了错。那不是 bug，是 explorer 用错了。

-   Monad Explorer（mainnet）： [monadscan.com](http://monadscan.com)
    
-   Monad Explorer（testnet）： [testnet.monadscan.com](http://testnet.monadscan.com)
    

### 把这些串起来

这些概念就像一条链一样彼此咬合：

交易被打包进 **区块**。节点用 **共识** 机制——在 Ethereum 和 Monad 这里就是 **Proof of Stake**——来就哪些区块有效达成一致。验证者因为质押的抵押品，以及 slashing（罚没） 的风险，被迫保持诚实。用户支付 **gas** 用来酬谢验证者并防止垃圾信息。当足够多的区块被确认后，交易就达到了 **最终性**，从此永久。

**实用工具集**是你真正与上面这一切打交道的方式：**chain ID** 告诉钱包用哪条网络，**RPC** 承载你的请求，**faucet** 给你 **testnet 代币**玩，**explorer** 让你看到链上正在发生什么。

每当你与你的应用互动——调用合约、把数据存上链、转代币——这整套系统都在背后运转。开发时你不必时刻想着它。但知道它就在那里、也知道它为什么这样运作，正是「在区块链上做产品的人」和「真正理解自己在什么基础上做产品的人」之间的差别。

### L9加密钱包

加密钱包

加密钱包（crypto wallet）是用来管理你区块链账号的软件。它存储你的凭证、代表你签署交易，并把你连接到去中心化应用。可以把它理解为你和区块链之间的接口。

其实用另外一个东西类比会很好：浏览器。浏览器本身并不包含整个互联网，它只是为你提供访问互联网的入口。加密钱包也不持有你的代币，它只是为你提供访问区块链账号的入口——你的代币真正存在的地方是区块链。

当你第一次打开钱包时，它会生成一个账号（account）。一个账号由两部分组成：一个地址（address）和一个私钥（private key）。

地址

地址是你在区块链上的身份。它是一串通过密码学生成的文本，可以完全公开地分享。当有人给你转代币时，他们会把代币发送到你的地址。当你与应用交互时，应用看到的也是你的地址。

地址看起来是这样的：`0x71C7656EC7ab88b098defB751B7401B5f6d8976F`

每个账号只对应一个地址，且这个地址是你独有的。

私钥

私钥（private key）是地址的对应物。如果说地址是你的用户名，那么私钥就是你的密码。

私钥看起来是这样的：E9873D79C6D87DC0FB6A5778633389F4453213303DA61F20BD67FC233AA33262

任何拿到你私钥的人，都可以用你的账号发起交易。没有找回流程。没有客服。区块链是去中心化的，这意味着没有任何机构能撤销一笔交易，也没有任何机构能为你重置凭证。如果你弄丢了私钥，你就丢了这个账号。如果有人偷了私钥，你的资产也就没了。

永远不要把你的私钥告诉任何人。

写给开发者：你会在代码库里使用私钥来部署智能合约（smart contract）和发送交易。请使用一个不持有真实资产的开发专用钱包。新手开发者经常因为不小心把私钥推到公开仓库而损失资金。

助记词

你的钱包可以管理多个账号。每个账号都有自己的地址和私钥。但钱包本身还有一个总凭证：助记词（seed phrase）。

助记词是你第一次创建钱包时生成的一组 12 个或 24 个单词。它看起来是这样的：

`dove lumber quote board young robust kit invite plastic regular skull history`

助记词可以生成你钱包里所有的账号。如果你失去了对钱包的访问权，仅凭助记词就可以恢复每一个账号。如果有人拿到了你的助记词，他就拿到了钱包里每一个账号的访问权。

助记词是你最敏感的凭证。请离线保存。不要截图。不要把它粘贴到任何网站。任何正规的应用都永远不会向你索要助记词。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->





# DAY2 转账

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2ZiYTYxZmNiMjRmYzI3MzNlNWY4YzFjZmRmYjk5OTdfaDdBYkxTM0pFRUxtSzN5bmF2bzBKd3VURkljbm93WUxfVG9rZW46S3pkS2JUU3JTb1Q4QWN4a011TGNjbFVOblRoXzE3ODM0MzM1MjI6MTc4MzQzNzEyMl9WNA&add_watermark=true&scene_type=CCM)

## Transaction hash

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OTg3MzZhMzMzMzliNjM3NjEzNGM5NzBkMDQwYmY5NjFfY3paMnFSWVBtc01icGh4cHhsSW1PZlpUR29PT3hnamlfVG9rZW46TXBZSGJCZmVvb25YY3Z4cjBsUmNTVmphbnVnXzE3ODM0MzM1MjI6MTc4MzQzNzEyMl9WNA&add_watermark=true&scene_type=CCM)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->






DAY1 希望坚持！

Web3入门：[https://web3intern.xyz/zh/preface/](https://web3intern.xyz/zh/preface/)

# **区块链基础概念**

## **一、区块链**

区块链是一种去中心化的分布式账本技术，用于在网络节点之间安全、透明且不可篡改地记录事务数据。每条链由一系列按照时间顺序相连的“区块”组成，每个区块内部包含了多笔交易数据及元数据，确保了数据记录的完整性与可追溯性。

一个区块链网络中有非常多的节点（矿机）来记账，每个节点都会记录完整的、相同的区块链信息。

因为有分布式网络，区块链有了**去中心化**和**真正的不可篡改**的新特性。

-   **去中心化**
    
    区块链网络通常分布在全球，每个节点都将会存储一份相同的区块链数据。没有人能够控制全部的节点，因此这份区块链数据将会一直存在。
    
-   **真正的不可篡改**
    
    区块链网络通常分布在全球，一个人控制大部分节点几乎不可能，因此即便有人修改了部分节点的区块链数据，只要被修改记录的节点不超过 51%，这个改动将不会被认可。
    

## 二、比特币

去中心化和分布式网络很好，可是为什么会有人愿意提供这项服务呢？因为他们可以因此得到奖励：比特币！

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Adazhangdada/images/2026-07-06-1783349132553-image.png)

## 三、区块链的核心组成部分

### 由两个核心组成：

1.  **去中心化的网络和区块链**
    

区块链将会有一条链来记录全部的信息，这条链将存在对应的去中心化网络中。 去中心化的网络，将由无数节点提供服务来维持网络运行。节点通过计算验证交易获得代币奖励。

2.  **维持网络运行的代币激励**
    

去中心化的网络由无数节点提供服务来维持网络运行。以 PoW 链为例，这些操作常被称为“挖矿”，参与者一般被称为矿工。矿工（或 PoS 链中的验证者）维持网络运行需要奖励，就像你工作需要工资。更准确地说，节点收入通常由协议奖励与交易手续费两部分组成。你经常听说的燃料费（Gas Fee）属于交易手续费的一部分，并不等于全部节点奖励。 你使用这个网络进行交易、转账、铸造 NFT 等等，均需要支付代币。如果你没有代币又想使用这个区块链网络服务，则需要进行代币之间交换或者法币交换。

### 区块链生态系统的运行包含以下几个关键步骤：

1.  用户发起交易：用户通过钱包应用发起转账、智能合约调用等操作
    
2.  交易广播：交易信息被广播到整个网络中的各个节点
    
3.  节点验证：网络中的矿工节点验证交易的合法性（余额是否足够、签名是否正确等）
    
4.  打包成块：通过共识机制（如工作量证明），矿工将验证过的交易打包成新的区块
    
5.  链接上链：新区块被添加到区块链上，更新全网的账本状态
    
6.  奖励发放：成功出块的节点可获得协议奖励（如区块奖励/质押奖励）和交易手续费（或其中一部分）
    

## 四、公链 vs 联盟链 vs 私链 Web2，3.0，3

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Adazhangdada/images/2026-07-06-1783349323238-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Adazhangdada/images/2026-07-06-1783349355415-image.png)

# **以太坊概览**

## **核心机制**

以太坊不仅仅是一条区块链，更是一个全球共享的“分布式计算机”。它的核心价值在于通过代码实现无需信任的自动化规则（如智能合约），而这一切的背后依赖于三个关键机制：**账户系统、Gas 模型 和 以太坊虚拟机（EVM）**。

我们平时使用 **账户系统** 里面的 **外部账户（EOA）** 与区块链中的其他用户的 **外部账户（EOA）** 或者与智能合约所在的 **合约账户（CA）** 进行交互，而其中的 **Gas 模型** 是支撑整个网络的经济基础， **Gas 模型** 和其他经济来源支撑着全球的 **以太坊虚拟机（EVM）** 来实现以太坊网络。

### [**1\. 账户系统：你的数字身份**](https://web3intern.xyz/zh/overview-of-ethereum/#_1-%E8%B4%A6%E6%88%B7%E7%B3%BB%E7%BB%9F-%E4%BD%A0%E7%9A%84%E6%95%B0%E5%AD%97%E8%BA%AB%E4%BB%BD)

**账户系统** 包含由私钥控制的 **外部账户（EOA）** 和由智能合约代码控制的 **合约账户（CA）** 。

想象你第一次接触以太坊——你需要一个“数字钱包”来参与网络。这个钱包的核心是 **外部账户（EOA）** ，它由一对密钥（私钥和公钥）生成，就像银行账户的密码和账号。私钥是你控制账户的“钥匙”，必须严格保密；公钥通过加密算法生成一个唯一的地址（如 `0xAbc...123`），你可以把它分享给朋友接收转账。

除了用户控制的 EOA，还有 **合约账户（CA）** 。它们不像 EOA 那样受私钥控制，而是由代码驱动。比如，你部署一个智能合约（如一个 NFT 市场），区块链会自动生成一个 CA 地址（如 `0xDef...456`）。这个账户不能主动发起交易，只能通过 EOA 触发——比如你点击“购买 NFT”按钮时，EOA 向 CA 发送交易，CA 的代码自动执行出货逻辑。

每个账户都包含四个关键字段：

-   **Nonce**：防止重复交易的计数器（EOA 记录发送次数，CA 记录创建合约次数）。
    
-   **余额**：账户持有的 ETH 数量（单位为 Wei）。
    
-   **CodeHash**：EOA 为空哈希，CA 存储合约字节码的哈希值。
    
-   **StorageRoot**：记录账户数据的 Merkle 树根哈希（如 NFT 归属关系）。
    

### [**2\. Gas 模型：交易的燃料费**](https://web3intern.xyz/zh/overview-of-ethereum/#_2-gas-%E6%A8%A1%E5%9E%8B-%E4%BA%A4%E6%98%93%E7%9A%84%E7%87%83%E6%96%99%E8%B4%B9)

当你使用自己的 EOA 钱包发起一笔交易（比如转账或操作合约），这件事当然 **不会是免费的**，**你需要支付“燃料费”——也就是 Gas。**

Gas 是怎么计算的？

Gas 费用 = **用多少 × 每单位多少钱**，就像你打车一样：

-   **Gas Limit（限额）**：你最多愿意“烧”多少燃料。 比如你觉得最多可能需要 15 万单位，就设置 150,000。
    
-   **Gas Price（单价）**：每单位燃料多少钱，用 Gwei 表示（1 Gwei = 0.000000001 ETH）。 网络越拥堵，价格越贵，就像打车高峰期加价。
    

所以，**总费用 = Gas Limit × Gas Price**。

假设你设置 Gas Limit 为 150 000，Gas Price 为 100 Gwei，总费用就是 0.015 ETH。如果实际消耗 120 000 Gas，剩余 30 000 Gas × 100 Gwei = 0.003 ETH 会退还给你。

Gas 的存在有两个目的：

-   **激励矿工/验证者**：你给得越多（Gas Price 越高），他们越愿意优先处理你的交易。
    
-   **防止资源滥用**：如果有人想让合约死循环，Gas 会用光，交易失败，系统不会被拖垮。
    

**在 EIP-1559 升级后，Gas Price 被拆分了：**

以前，Gas Price 全部给矿工。 现在，分成两部分：

-   **基础费用（Base Fee）**：每个区块都会有，自动计算，直接销毁（直接消失）来帮助 ETH 通缩，毕竟物以稀为贵。
    
-   **小费（Tip）**：你额外加的钱，用来鼓励矿工优先处理你。
    

### [**3\. 以太坊虚拟机（EVM）：代码的执行引擎**](https://web3intern.xyz/zh/overview-of-ethereum/#_3-%E4%BB%A5%E5%A4%AA%E5%9D%8A%E8%99%9A%E6%8B%9F%E6%9C%BA-evm-%E4%BB%A3%E7%A0%81%E7%9A%84%E6%89%A7%E8%A1%8C%E5%BC%95%E6%93%8E)

EVM（Ethereum Virtual Machine）是 **以太坊的“大脑”**，是专门用来**运行智能合约的虚拟计算机**。它运行在每个节点上，确保整个网络在处理代码时，**结果都一致、可信任**。

**EVM 的核心特点：**

-   **图灵完备**：就像真正的电脑一样，EVM 可以执行各种逻辑，比如 if 判断、循环等。
    
-   **全球同步**：每个矿工/节点都会自己执行一遍合约代码，保证结果一样。
    
-   **隔离安全**：EVM 把合约“关”在一个小房间里运行，不允许它乱访问用户的数据和网络，保护隐私和安全。
    

## 类比：

### [**1\. 一般流程**](https://web3intern.xyz/zh/overview-of-ethereum/#_1-%E4%B8%80%E8%88%AC%E6%B5%81%E7%A8%8B)

1.  **用户通过 EOA 发起交易**（如调用合约与 **CA** 交互，或者转账与其他用户的 **EOA** 交互）。
    
2.  **交易附带 Gas 参数**，矿工 / 验证者选择打包。
    
3.  **EVM 执行合约代码**，修改存储（如更新 NFT 归属）。
    
4.  **Gas 费用按 Gas Limit × Gas Price 扣除**，保障资源合理使用。
    

### [**2\. 相似类比**](https://web3intern.xyz/zh/overview-of-ethereum/#_2-%E7%9B%B8%E4%BC%BC%E7%B1%BB%E6%AF%94)

-   **EOA = 个人钱包**：你用私钥“签名”来证明身份，可以主动发起任何交易
    
-   **CA = 智能保险箱**：按照预设程序自动运行，只有满足条件才执行操作（如：收到正确密码后自动转账）
    
-   **Gas = 计算资源费用**：
    
    -   Gas Limit：你愿意支付的最大计算量
        
    -   Gas Price：每单位计算的价格
        
    -   如果设置 1000 单位 ×10Gwei，实际用了 800 单位，退还 200 单位的费用
        
-   **EVM = 全球共享计算机**：所有节点同时运行相同的程序，确保结果一致且可信
    

# Vibecoding

[https://buildanything.so/zh/tracks/freshman/lessons/introduction-to-vibecoding](https://buildanything.so/zh/tracks/freshman/lessons/introduction-to-vibecoding)

**Replit** — 一个基于浏览器的编程环境，内置 AI 智能体。无需安装，所有功能都在云端运行。免费版足以让你上手。

**建议在沙箱中运行智能体，而不是在本地电脑上。** 沙箱是一个云端的隔离环境，与你的个人文件和敏感数据相互分离。即便智能体在沙箱中被攻陷，损害也会被限制在沙箱内。

我们推荐两种选择：

-   [**Replit**](https://replit.com/) — 基于浏览器，无需任何配置，适合新手。
    
-   [**GitHub Codespaces**](https://github.com/codespaces) — 在云端运行 VS Code，需要 GitHub 账号。
    

**绝不要把真实的私钥、密码或 API key 交给智能体。** 一旦你把这些交给智能体，就应当把它们视为已经泄露。请始终使用没有真实资金或敏感访问权限的开发专用账号。

**把智能体的输出当作来自陌生人的代码来对待。** 在部署前审查它构建的内容，尤其是涉及金钱或用户数据的部分。

推荐 [**Component Gallery**](https://component.gallery/)，里面收录了几乎所有常见 UI 组件的可视化参考，并附有标准名称。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
