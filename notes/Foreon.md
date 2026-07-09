---
timezone: UTC+8
---

# Leon

**GitHub ID:** Foreon

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# Day4

今天学习了10000tps对于构建应用服务的重要性，并且学习了数据库和sql语言，以及数据上传的安全问题，比如上传文件攻击等等。  

![QQ20260708-233039.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Foreon/images/2026-07-09-1783611711974-QQ20260708-233039.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->

学习buildanything

今天在buildanything网站上学习，了解到了vibe coding这个概念，学习了怎么用vibe coding去开发，维护一个应用。

维护一个应用，需要：数据库、身份认证（Auth）、文件存储、支付、邮件、错误追踪。

而这些需求都是一个你无法控制的外部服务。它们都有可能宕机、被攻击、冻结你的账号，或者一夜之间消失。所以我们需要开发一个去中心化掉应用。 然后学习了区块链的相关概念：

> 交易被打包进 区块。节点用 共识 机制——在 Ethereum 和 Monad 这里就是 Proof of Stake——来就哪些区块有效达成一致。验证者因为质押的抵押品，以及 slashing（罚没） 的风险，被迫保持诚实。用户支付 gas 用来酬谢验证者并防止垃圾信息。当足够多的区块被确认后，交易就达到了 最终性，从此永久。
> 
> 实用工具集是你真正与上面这一切打交道的方式：chain ID 告诉钱包用哪条网络，RPC 承载你的请求，faucet 给你 testnet 代币玩，explorer 让你看到链上正在发生什么。

monad像是以太坊一样的区块链系统生态，可以让很多web3应用运行在上面，但monad拥有更高的性能。mon是这条链的代币。  

![QQ20260708-233039.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Foreon/images/2026-07-08-1783525034675-QQ20260708-233039.png)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->


## 完成第一笔交易

1.  Transaction hash：0xe3862356c253773e48beb9e2a2ddf80b9a53ce53335c441dc4d5ff492a7bfa9c
    
2.  区块浏览器链接：[testnet.monadvision.com](http://testnet.monadvision.com)
    

from / to 分别是谁： from 是发起交易的钱包地址，也就是付款方：`0x20295E0ED60dcE1147f4b474D689ce95dcABc4bf`；to 是接收交易的钱包地址，也就是收款方：`0xbc58724dBcA23325a4b0C0eaEB050B573192Bd32`。

value 表示什么： value 表示这笔交易实际转出的主币数量。这里的 `Value: 1` 表示 from 地址向 to 地址转了 **1 个 MON**。

gas 和手续费代表什么： gas 是执行这笔链上交易所消耗的计算资源，可以理解成“链上操作成本”。手续费是发送方为这次交易支付给网络的费用，计算方式大致是 `Gas Used × Gas Price`。这笔交易用了 `31,500 gas`，gas price 是 `122.4 Gwei`，所以手续费是 `0.0038556 MON`。其中 `0.00315 MON` 被销毁，剩余部分可能给验证者/矿工作为激励。

交易状态是成功还是失败： 这笔交易状态是 **Success**，说明交易已经成功执行，并且已经被打包进区块 `#42943344`。

失败交易为什么也可能消耗 gas：

因为失败交易虽然最终没有完成目标操作，但区块链节点仍然要执行它、验证它、计算它为什么失败。只要交易已经被链上执行，就会消耗计算资源，所以即使最后因为余额不足、合约条件不满足、滑点过高、gas 不够等原因失败，已经消耗掉的 gas 通常也不会退回。

在https://faucet.monad.xyz/领测试币

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjFjN2UwNzMxMzgwYTg5ZTcyZWY3MmFiNDc5YWM5YzNfVnpTbzBPT0xEdlhrZkhWUjE1cVJFenJCRFZ0eVdiNlNfVG9rZW46WHg4bGI3M2gwb1Mza1d4YldFcGNTUmJhbkpjXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

Transactions

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MWI5ZTY1MmM0ZjM3NmJkMGQ5YmUyMzQzZDYzODU3NDhfNzY4dlcxdVRnalNieTlId1ZEUnd2U2ZidG9TQXZWQkNfVG9rZW46QWdCdmJSbmhMb1h0bFN4TnduT2NDRjdZblhjXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

给自己又注册了一个钱包地址，并发起转账

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NDc0MTAwOGE2OTViMzJlZTAxNjNiN2MzMTBlZmQ5MWZfOUd1akpZbzlEMXplUU5hWEVQZzBjcks0TUNHUEx6MmFfVG9rZW46TlFOemIzZlV3b3Viamt4TnMyMmMxemd5bjZjXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ODllNmU4NDQ0N2QzYjI2ZWQ4YzlhMjA5OTZlYzJmYWRfejZSSTlxcjlGRlhRYW9FV21KTDNkcjJ5QTdRYklKRXRfVG9rZW46TWs4VWJIdWtFb3dYWTF4dm43d2NVZEFYbmRkXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

Hash transaction

0xe3862356c253773e48beb9e2a2ddf80b9a53ce53335c441dc4d5ff492a7bfa9c

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YmQ0N2FhYjAwZWEzYTg2NmU0ZTdjZGZmY2UzYTM1OThfVnc4cWR0ZXZzRVZhTzJGdUtVRklqalljdFIyV09uTXRfVG9rZW46SDM0cWJpbW5Wb3NuOFl4NjFTcmNnRzBKbjlkXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

## 用 AI 生成一个最小 Solidity 合约

1.  Prompt：帮我写一个留言板solidity合约
    
2.  AI 生成的主要输出：
    

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MessageBoard {
    // 定义一条留言的数据结构
    struct Message {
        address sender;     // 留言者地址
        string content;     // 留言内容
        uint256 timestamp;  // 留言时间
    }

    // 保存所有留言
    Message[] public messages;

    // 留言事件，方便前端或区块浏览器监听
    event NewMessage(address indexed sender, string content, uint256 timestamp);

    // 发布留言
    function postMessage(string memory _content) public {
        require(bytes(_content).length > 0, "Message cannot be empty");

        messages.push(Message({
            sender: msg.sender,
            content: _content,
            timestamp: block.timestamp
        }));

        emit NewMessage(msg.sender, _content, block.timestamp);
    }

    // 获取留言总数
    function getMessageCount() public view returns (uint256) {
        return messages.length;
    }

    // 根据编号获取某一条留言
    function getMessage(uint256 _index)
        public
        view
        returns (address sender, string memory content, uint256 timestamp)
    {
        require(_index < messages.length, "Message does not exist");

        Message memory message = messages[_index];
        return (message.sender, message.content, message.timestamp);
    }
}
```

3.  合约源码如上所示。
    
4.  你做过的人工修改或判断说明。
    

review发现，需要加入最长消息长度：

```Solidity
require(
            bytes(_content).length <= MAX_MESSAGE_LENGTH,
            "Message is too long"
        );
```

5.  至少 3 条“AI 生成代码需要人工检查的地方”： 1️⃣**检查合约是否能正常编译：** 需要把代码复制到 Remix 或其他 Solidity 编译器中，确认 `pragma solidity ^0.8.20`、结构体、数组、函数、`require` 等语法没有报错。 2️⃣**检查函数是否符合需求：** `postMessage` 负责发布留言，`getMessageCount` 负责查看留言数量，`getMessage` 负责按编号读取留言，功能和留言板需求基本一致。 3️⃣**检查留言长度限制是否符合预期：** 合约中设置了 `MAX_MESSAGE_LENGTH = 200`，但这里限制的是**字节长度**，不是字符数。英文 200 个字母大约是 200 字节，但中文一个字通常占多个字节，所以实际可输入的中文字符会少于 200 个。
    

完整代码：

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MessageBoard {
    // 最长留言长度，单位是字节
    uint256 public constant MAX_MESSAGE_LENGTH = 200;

    // 定义一条留言的数据结构
    struct Message {
        address sender;     // 留言者地址
        string content;     // 留言内容
        uint256 timestamp;  // 留言时间
    }

    // 保存所有留言
    Message[] public messages;

    // 留言事件，方便前端或区块浏览器监听
    event NewMessage(address indexed sender, string content, uint256 timestamp);

    // 发布留言
    function postMessage(string memory _content) public {
        require(bytes(_content).length > 0, "Message cannot be empty");
        require(
            bytes(_content).length <= MAX_MESSAGE_LENGTH,
            "Message is too long"
        );

        messages.push(Message({
            sender: msg.sender,
            content: _content,
            timestamp: block.timestamp
        }));

        emit NewMessage(msg.sender, _content, block.timestamp);
    }

    // 获取留言总数
    function getMessageCount() public view returns (uint256) {
        return messages.length;
    }

    // 根据编号获取某一条留言
    function getMessage(uint256 _index)
        public
        view
        returns (address sender, string memory content, uint256 timestamp)
    {
        require(_index < messages.length, "Message does not exist");

        Message memory message = messages[_index];
        return (message.sender, message.content, message.timestamp);
    }
}
```

在remix中编译

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YzNhZDkzMzQyNmI2MThmNGJjNTA1YjE0NGNkZGEyMTlfclJldENrWTA0QkNSYlJnWHc4ZW0zRk1yenRRZzlVbVRfVG9rZW46R2NaWmJpdDNSb2dLRTZ4TEppc2NoTFhJbkliXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

编译通过

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDA4ODVlZDFmYTZmMDdmM2EzYWEwNzZjMmZlNDAzNWFfWHhxVGs3NmFaRWRiUnFlcDhBMDhueTB5VDdLbTVacEdfVG9rZW46TlNlNGI1NDNlb1FUcGR4OVI1OWN3bGdQblRiXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

接着部署到Monad Testnet，第一张图选错了，应该是browser wallet

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ODQ3MDNlOTA5YjBlZmE3YWQzMGEzMTRlNmQ2MzcyMjdfUGhEYTBRVlhFRGo1RmRIN2d5ejhmZFdmVVNjb3REUldfVG9rZW46TUZ1bmJtaExOb2lBU1Z4MG4wUGNGWWVEbk5nXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MGVhOThjMzM0MjhjMDgyYmJhNjdhOGE3YjRlZWVhZTNfV2wxQ1g0TVhFbXo5eHAwV1hWb2NHRXhaV1U2Q2JGbnJfVG9rZW46QVVzdmJhM2hVb0tiRWx4SURBWmNRNnJ1bmdiXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NWNlYmYyOTk0YTMyMGMzMTUyMWUyOWFkYjNiOGI5NmRfcFpOckFvcUR0YklZaDhXcXo3Y2lua1ZQR0VhWVFFTG1fVG9rZW46VmJmRmJBeDlEb2piN0Z4WnVTemNXZmN1bjhjXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NDllZGFlZDk5ZGMyYjI4YTk4ZjU0ZWQ1MjliZWE4ZDVfMTFTZE5kcU1vcXp2ZThoY2x2b3UySU9vbWJlZFVPbTZfVG9rZW46UGhzNGJOVUN6b3hiaVF4M01TRWNHck84bmhiXzE3ODM0MzgyMTM6MTc4MzQ0MTgxM19WNA&add_watermark=true&scene_type=CCM)

| 字段 | 含义 |
| status: 1 Transaction mined and execution completed | 表示交易成功。status: 1 代表成功，mined 表示交易已经被打包进区块，execution completed 表示合约部署执行完成。 |
| transaction hash: 0x0ca647...b1f5d4 | 这是本次部署交易的唯一编号。以后可以用这个哈希在区块浏览器中查询这笔交易。 |
| block hash: 0x99f718...0309e | 这是打包该交易的区块哈希，可以理解为这个区块的唯一标识。 |
| block number: 42965461 | 表示这笔部署交易被打包进了第 42965461 个区块。这个区块号比较大，说明这次不是 Remix 本地 VM 的第 1 个区块，而是部署到了真实的测试链或公链环境。 |
| contract address: 0x398E6E1E63315d2095749171e1269c7DEA80e7b7 | 这是部署成功后生成的合约地址。以后调用 postMessage、getMessageCount、getMessage，就是在和这个地址上的 MessageBoard 合约交互。 |
| from: 0x20295E0ED60dcE1147f4b474D689ce95dcABc4bf | 这是发起部署交易的钱包地址，也就是部署这个合约的人。 |
| to: MessageBoard.(constructor) | 表示这不是普通转账，而是在执行 MessageBoard 合约的构造过程，也就是部署合约。部署时合约地址还不存在，所以这里显示的是调用构造函数 constructor。 |
| transaction cost: 1140996 gas | 表示这次部署合约总共消耗了 1,140,996 gas。部署合约需要把合约代码写入链上，所以通常比普通转账消耗更多 gas。 |
| decoded input: {} | 表示部署时没有传入构造函数参数。你的 MessageBoard 合约没有需要初始化传入的参数，所以这里是空对象。 |
| decoded output: - | 表示这笔部署交易没有普通函数返回值。部署交易的主要结果是生成一个新的合约地址，而不是返回某个变量。 |
| logs: [] | 表示部署过程中没有触发事件日志。虽然合约里定义了 NewMessage 事件，但只有调用 postMessage 发布留言时才会触发，部署时不会触发。 |
| raw logs: [] | 表示原始事件日志为空，和 logs: [] 对应，说明本次部署没有产生事件。 |
| [Verification] Contract deployed. Checking explorers for registration... | Remix 检测到合约已经部署成功，并开始尝试把合约源码提交到区块浏览器或验证服务进行合约验证。 |
| Etherscan verification skipped: API key not provided. | Etherscan 验证被跳过，因为 Remix 没有配置 Etherscan API Key。这不影响合约部署，只是不能通过 Etherscan 完成源码验证。 |
| Please input the API key... | 提示如果想用 Etherscan 验证合约，需要在 Remix 设置或合约验证插件中填写 API Key。 |
| [Sourcify] Verification submitted. Awaiting confirmation... | Remix 已经把合约源码提交给 Sourcify 进行验证，正在等待确认。 |
| [Sourcify] Verification Successful! View Code | Sourcify 验证成功。说明你的合约源码和链上部署的字节码能够对应起来，别人可以通过 Sourcify 查看合约源码。 |

总结：这次 `MessageBoard` 合约已经成功部署到了链上，合约地址是 `0x398E6E1E63315d2095749171e1269c7DEA80e7b7`，部署者是 `0x2029...c4bf`，本次部署消耗了 `1,140,996 gas`，并且源码已经通过 Sourcify 验证。

**gas是哪来的？** 你这里的 **gas 不是你自己“拥有的一种东西”**，而是 Remix 帮你在本地测试环境里模拟出来的“计算费用单位”。

你现在看到的是 Remix VM 里的部署结果，大概率不是在真实主网/测试网上操作。Remix VM 会自动给几个测试账户分配很多假的 ETH/MON，用来支付 gas，所以你不需要自己充值，也不会真的扣钱。

简单理解：

`gas`：表示执行合约需要消耗多少计算资源。 `gas fee`：表示这些计算资源折算成要付多少链上币。 `Remix VM`：本地模拟区块链，账户里自带测试币，所以能直接部署。 `真实链上`：你必须钱包里有该链的原生币，比如 ETH、MON、BNB，才能支付 gas。

比如你部署合约时显示：

这不是说你真的有 `753791` 个 gas，而是说：**这次部署消耗了 753791 单位的链上计算资源**。

如果在真实 Monad 测试网或以太坊测试网上部署，钱包里就必须有对应的测试币，否则会报类似：

也就是：余额不够支付 gas。 **合约部署上链是什么意思，链是从哪来的，我部署合约本质上是在干什么？**

**合约部署上链**的意思是：你把写好的 Solidity 合约代码，编译成区块链能执行的字节码，然后通过一笔交易发送到区块链网络里。交易成功后，链上会生成一个新的合约地址，这个地址里保存着你的合约代码和状态数据。

你可以这样理解：

**链 = 一个由很多节点共同维护的公共数据库 / 公共账本。**

比如以太坊、BSC、Polygon、Monad 测试网，都是不同的链。每条链都有很多区块，区块里面记录交易。用户转账是一种交易，部署合约也是一种交易。

你在 Remix 里部署时，可能有两种情况：

**第一种：Remix VM 本地链。** 这是 Remix 在浏览器里模拟出来的一条假链，只用于测试。账户、余额、区块、gas 都是模拟的，不是真的上公网区块链。

**第二种：真实测试网或主网。** 比如连接 MetaMask 后部署到某个测试网，那就是真的把合约部署到那条链上。别人也可以通过合约地址查到和调用它。

你部署合约本质上是在做这件事：

比如你部署留言板合约后，链上生成了：

这个地址就像你留言板程序的“线上地址”。以后调用 `postMessage()`，就是给这个合约地址发送交易，让链上的程序执行“保存留言”这段逻辑。

所以一句话总结：

**部署合约上链，就是把你的程序发布到区块链这个公共运行环境里，让它获得一个链上地址，并且以后可以被钱包或其他程序调用。**

部署合约的地址：0x398E6E1E63315d2095749171e1269c7DEA80e7b7
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->



# Day1

今天学会了web3入门的前置准备，设置好了钱包，添加了monad testnet，并且测试了地址存在于链上，明天继续尝试交易。  
前置准备

安装metamask钱包

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MjZmZDIxNDRiMjBmYTAyOGQ0ZjEwOGJlNzBmZTUzNjRfUmN6TGRtbUJuOHJOMmpSU3hFamFLM2x5OHVWbGxHcGdfVG9rZW46U1VKMWJ1ekg5b2Q4TVF4VUtramM0RkRkblNkXzE3ODMzNTI2MDc6MTc4MzM1NjIwN19WNA&add_watermark=true&scene_type=CCM)

0x20295E0ED60dcE1147f4b474D689ce95dcABc4bf

添加monad testnet，其实我注册的时候已经被添加了

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZThlNjA3MWIyZTdlZTdjZTg3ODg4YjM2ODAxOTc2ODJfZkJjZllmNmpPQWE5OEFRdWNTNlJibUpGcFljMWxyRW5fVG9rZW46T3M5WGJNMXJkb2QxYmF4N0Y3S2NndER2bkFnXzE3ODMzNTI2MDc6MTc4MzM1NjIwN19WNA&add_watermark=true&scene_type=CCM)![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MzcwYjZhN2VlYjY5Zjc3MWQ3ZWU1YjAwMmU1ZTQ4ZjZfTzZZTGVmOTlwZ2xKaGgzR1VvbHlRbHc2SjY3enZKc1ZfVG9rZW46Tk5DM2JLa25ib1g1U0F4OWdGSmNRcmR0bkdoXzE3ODMzNTI2MDc6MTc4MzM1NjIwN19WNA&add_watermark=true&scene_type=CCM)

打开 Monad Explorer 或相关区块浏览器，确认可以查询地址。

![](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NTM5YWIwYjRhNjNiZjhjNTgwMDVjNTg1NjcwMGU2MjRfWXVDNWkzMVVIa1FQZEh0NWN0YmdiNGlWWUhaVEVlcTNfVG9rZW46WmczbGJTN1VYb2VjWFJ4TEcwb2Nuc3M5bnNmXzE3ODMzNTI2MDc6MTc4MzM1NjIwN19WNA&add_watermark=true&scene_type=CCM)

使用钱包工具metamask

地址0x20295E0ED60dcE1147f4b474D689ce95dcABc4bf

链上产品和普通互联网产品最大的区别是：**普通互联网产品主要由公司服务器控制，链上产品主要由区块链网络和智能合约控制。**

普通互联网产品，比如微信、淘宝、抖音，用户的数据、账号、交易记录一般都存在公司的服务器里。公司可以修改规则、封号、下架内容、调整数据，用户更多是在使用平台提供的服务。

链上产品的核心数据和交易通常记录在区块链上。很多规则写在智能合约里，一旦部署，任何人都可以查看合约逻辑和链上记录，数据更透明，也更难被单方面篡改。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
