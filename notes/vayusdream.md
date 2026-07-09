---
timezone: UTC+8
---

# vayusdream

**GitHub ID:** vayusdream

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# D4 AI Agent高危示例&安全防线构建

-   自动化到自主化的威胁演变
    
-   AgentGuard：要定期审查，防止过度授权。
    
-   安全加固建议：最小权限原则 / Human-in-the-loop（HITL）-对敏感操作设置人工复核或二次确认 / 部署安全监控工具 / 零信任，严防Prompt注入攻击 / 使用agentguard在安装skill前扫描，对安全skill可加入trust registry进行安全管理 / 拉依赖要锁版本，下载认准官方渠道
    
-   高风险实例：高风险skills / Prompt注入（恶意指令） / 系统漏洞 / 恶意安装包 / AI幻觉
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->

# D3

## AI Agent支付

[https://fluxapay.xyz/](https://fluxapay.xyz/)

-   智能体经济：针对不同场景探索（OPC/研究机构/不同行业B端/自动抢红包抢票）发挥agent支付潜力
    
-   skill：Read and install [https://fluxapay.xyz/skill.md](https://fluxapay.xyz/skill.md)
    
-   运行在Base链上，使用USDC作为支付货币；关键卖点是不需要管理私钥
    
-   fluxa与coinbase是生态伙伴
    
-   支付方式：目前是一次性的卡支付（agent card法币结算）/区块链形式，agent没有identity（B端做完）
    
-   实现形式：只要两方link装的同样skill，就可以通过payment link付款（点对点直接转账）；或者创建mandate让对方安装转账
    
-   稳定币usdc目前是最大的链，现在有ousd，哪条链无所谓，看agent更支持哪个。目前支持跨链。
    
-   未来一定会走上主流，所以需要与visa等传统发卡机构合作。
    
-   从业agent payment：engineering/懂流程
    
-   如何让自己的资料或产品能让ai更容易找到，从而成为塔常用付费的商户？——与百度合作
    
-   未来希望实现的场景：自动交水电费等高频生活场景
    
-   agent payment过程中不涉及黑箱，支付比llm呈现的结果更清晰，协议中已经规定很清楚。
    
-   数字人民币：🈚️的原因主要是需求不够，单纯靠政策推动远远不够
    
-   Jack [Xiao邮箱：jackxiao@fluxapay.xyz](mailto:Xiao%E9%82%AE%E7%AE%B1%EF%BC%9Ajackxiao@fluxapay.xyz)
    
-   体验链接：[https://agentwallet.fluxapay.xyz/](https://agentwallet.fluxapay.xyz/)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->




# D2

## 以太坊协议层

-   以太坊 EPF（Ethereum Protocol Fellowship）：[epf.wiki](http://epf.wiki)
    
-   [https://github.com/orgs/eth-protocol-fellows/repositories](https://github.com/orgs/eth-protocol-fellows/repositories)
    
-   [https://github.com/ethereum](https://github.com/ethereum)
    
-   学习路径：自驱动项目/加入discord/X上交付让epf的mentors看看/提pr（不要太大）—》〉wiki/官方specs—》〉选一个方向：共识层/执行层/客户端
    
-   EIP（[https://ethereum.org/zh/eips/](https://ethereum.org/zh/eips/)）：最好熟读某一个项目 [https://github.com/ethereum/EIPs](https://github.com/ethereum/EIPs)
    
-   选项目时，先选问题域，再选issue
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->





# D1

## Basic Concepts

-   **Value**：是这笔交易转移的原生代币数量，单位是 MON。它不包括 Gas 费——**Gas** 是另外从 From 地址扣的「过路费」，用来补偿网络节点帮你打包、验证这笔交易。（交易失败也会消耗Gas，计算的是执行费，不是交易费）
    
-   **Gas Limit**：你愿意为这笔交易支付的最大 Gas 上限（类似「最多愿意出多少运费」）。
    
-   **Gas Price / Gas Fee**：实际单价和总费用。
    
-   **Transaction Fee** = Gas Used × Gas Price，从 From 地址扣除。
    
-   **链上产品**不是「多了一个区块链字段的 App」，而是**状态在链上、规则靠共识、操作靠签名**的另一套系统。
    
-   Monad（Layer 1公链）
    
    | 核心技术 | 优势 |
    | --- | --- |
    | 并行执行 Parallel Execution传统 EVM 链很多交易是顺序执行的，Monad 会尝试把不冲突的交易（多笔交易）并行处理，从而提升吞吐量。除此之外，它还用了异步执行、MonadDb 自定义状态数据库、MonadBFT 共识等一整套优化，而不是只靠一个“并行执行”概念。 | 接近高性能链体验，同时保留 EVM 兼容性 |
    
-   文档：[https://docs.monad.xyz/](https://docs.monad.xyz/)
    
-   github：[https://github.com/monad-developers](https://github.com/monad-developers)
    
-   skills：[https://skills.devnads.com/](https://skills.devnads.com/)
    

## 测试网交易

1.  我的课程专用钱包地址：0x60c79c1aD7A190882505245A2a7717b49d925160
    
2.  Monad Testnet 网络配置✅区块浏览器（[https://testnet.monadvision.com/](https://testnet.monadvision.com/)）中查询该地址✅读懂基本字段✅拿测试币（测试网页：[faucet.monad.xyz](http://faucet.monad.xyz)）完成monad链上交易&复盘✅
    
3.  transaction hash：0x99e47db86c037efa395d4138c543e4540a81dca62df259dbb56b6699731dba5b
    

## AI辅助开发：**生成一个最小 Solidity 合约**

### Prompt

请帮我生成一个 Solidity 留言板智能合约，适合部署在 EVM 兼容链上。

合约需求如下：

1.  合约名称为 `MessageBoard`
    
2.  使用 Solidity `^0.8.20`
    
3.  用户可以发布留言
    
4.  每条留言需要记录：
    
    -   留言内容
        
    -   留言者地址
        
    -   留言时间
        
5.  所有人都可以查看全部留言
    
6.  可以根据留言 ID 查看单条留言
    
7.  合约需要提供一个函数返回留言总数
    
8.  发布留言时触发事件 `MessagePosted`
    
9.  留言内容不能为空
    
10.  代码需要简洁、清晰、安全
     
11.  请在代码中添加中文注释
     

请输出以下内容：

第一部分：完整 Solidity 合约代码。

第二部分：解释合约结构，包括：

-   `struct` 的作用
    
-   `array` 的作用
    
-   `event` 的作用
    
-   每个函数的作用
    
-   用户发布留言时链上发生了什么
    

第三部分：请重点帮我检查这个合约，检查维度包括：

-   合约是否能正常编译
    
-   函数是否符合留言板合约的预期
    
-   是否存在明显权限问题
    
-   是否使用了不必要的复杂逻辑
    
-   是否存在潜在安全风险
    
-   变量和函数命名是否容易理解
    
-   是否适合 Solidity 初学者学习
    

第四部分：如果发现问题，请指出问题原因，并给出修改后的推荐版本。

请尽量写成适合区块链初学者理解的版本，不要加入过度复杂的功能。

### AI输出（Codex）

【第一部分：完整 Solidity 合约代码】

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MessageBoard {
    // 留言结构体：用于描述一条留言的数据格式
    struct Message {
        string content;      // 留言内容
        address author;      // 留言者地址
        uint256 timestamp;   // 留言时间
    }

    // 使用数组保存所有留言
    Message[] private messages;

    // 发布留言时触发的事件
    event MessagePosted(
        uint256 indexed messageId,
        address indexed author,
        string content,
        uint256 timestamp
    );

    // 发布留言
    function postMessage(string calldata _content) external {
        require(bytes(_content).length > 0, "Message cannot be empty");

        messages.push(
            Message({
                content: _content,
                author: msg.sender,
                timestamp: block.timestamp
            })
        );

        uint256 messageId = messages.length - 1;

        emit MessagePosted(
            messageId,
            msg.sender,
            _content,
            block.timestamp
        );
    }

    // 获取全部留言
    function getAllMessages() external view returns (Message[] memory) {
        return messages;
    }

    // 根据留言 ID 获取单条留言
    function getMessage(uint256 _messageId) external view returns (Message memory) {
        require(_messageId < messages.length, "Message does not exist");

        return messages[_messageId];
    }

    // 获取留言总数
    function getMessageCount() external view returns (uint256) {
        return messages.length;
    }
}
```

【第二部分：合约结构解释】

### `struct` 的作用

`struct Message` 用来定义一条留言的数据结构。

每条留言包含：

-   `content`：留言内容
    
-   `author`：留言者地址
    
-   `timestamp`：留言发布时间
    

可以把 `struct` 理解成 Solidity 里的“自定义数据类型”。

### `array` 的作用

```
Message[] private messages;
```

这里使用数组保存所有留言。

每当用户发布一条新留言，合约都会把这条留言追加到 `messages` 数组中。

留言 ID 就是它在数组里的下标：

-   第一条留言 ID 是 `0`
    
-   第二条留言 ID 是 `1`
    
-   第三条留言 ID 是 `2`
    

### `event` 的作用

```
event MessagePosted(...)
```

事件用于记录链上发生的重要动作。

当用户发布留言时，合约会触发 `MessagePosted` 事件。

前端、区块浏览器或链下服务可以监听这个事件，从而知道有人发布了新留言。

### 每个函数的作用

`postMessage`

```
function postMessage(string calldata _content) external
```

用户调用这个函数发布留言。

它会做三件事：

1.  检查留言内容不能为空
    
2.  把留言保存到数组里
    
3.  触发 `MessagePosted` 事件
    

`getAllMessages`

```
function getAllMessages() external view returns (Message[] memory)
```

返回所有留言。

这个函数是 `view`，只读取链上数据，不修改状态。

`getMessage`

```
function getMessage(uint256 _messageId) external view returns (Message memory)
```

根据留言 ID 查询单条留言。

如果 ID 不存在，会回滚交易并提示：

```
"Message does not exist"
```

`getMessageCount`

```
function getMessageCount() external view returns (uint256)
```

返回留言总数。

比如当前有 10 条留言，这个函数返回 `10`。

### 用户发布留言时链上发生了什么

当用户调用 `postMessage("hello")` 时：

1.  用户的钱包发起一笔交易
    
2.  EVM 执行 `postMessage` 函数
    
3.  合约检查 `"hello"` 是否为空
    
4.  合约把留言内容、用户地址、当前区块时间保存到链上
    
5.  合约触发 `MessagePosted` 事件
    
6.  交易被打包进区块后，这条留言永久保存在链上
    

【第三部分：合约检查】

### 是否能正常编译

可以。代码使用：

```
pragma solidity ^0.8.20;
```

语法符合 Solidity `0.8.20` 及以上兼容版本。

### 函数是否符合留言板预期

符合。

合约支持：

-   发布留言
    
-   查看全部留言
    
-   根据 ID 查看单条留言
    
-   查看留言总数
    
-   发布时触发事件
    
-   禁止空留言
    

### 是否存在明显权限问题

没有明显权限问题。

这是公开留言板，所以任何地址都可以发布留言、查看留言。这里不需要 `owner` 或管理员权限。

### 是否使用了不必要的复杂逻辑

没有。

合约只使用了：

-   `struct`
    
-   数组
    
-   `event`
    
-   基础查询函数
    
-   `require` 校验
    

适合初学者学习。

### 是否存在潜在安全风险

没有明显高危风险。

但需要注意一点：`getAllMessages()` 如果留言数量非常多，返回所有留言会变得很重。对于初学者合约没问题，但真实生产环境通常会使用分页查询。

例如：

```
getMessages(uint256 offset, uint256 limit)
```

不过为了保持简单，这里没有加入分页逻辑。

### 命名是否容易理解

容易理解。

例如：

-   `MessageBoard`
    
-   `postMessage`
    
-   `getMessage`
    
-   `getAllMessages`
    
-   `getMessageCount`
    
-   `MessagePosted`
    

这些命名都比较直观。

### 是否适合 Solidity 初学者学习

适合。

这个合约覆盖了 Solidity 初学者常见知识点：

-   状态变量
    
-   结构体
    
-   数组
    
-   事件
    
-   `msg.sender`
    
-   `block.timestamp`
    
-   `require`
    
-   `view` 查询函数
    

【第四部分：问题与推荐版本】

当前版本没有明显问题，可以作为推荐版本使用。

唯一需要提醒的是：如果留言数量非常大，`getAllMessages()` 在前端读取时可能变慢。因此真实项目中可以考虑分页查询。但对于学习版留言板来说，现在这个版本更简洁，也更适合初学者理解。

### 源码需人工判断的地方

是否限制留言长度（不限）、是否保留全量查询（是）、是否接受完全开放且永久存储的内容模型（是）。

### 最终源码

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MessageBoard {
    // 留言结构体：用于描述一条留言的数据格式
    struct Message {
        string content;      // 留言内容
        address author;      // 留言者地址
        uint256 timestamp;   // 留言时间
    }

    // 使用数组保存所有留言
    Message[] private messages;

    // 发布留言时触发的事件
    event MessagePosted(
        uint256 indexed messageId,
        address indexed author,
        string content,
        uint256 timestamp
    );

    // 发布留言
    function postMessage(string calldata _content) external {
        require(bytes(_content).length > 0, "Message cannot be empty");

        messages.push(
            Message({
                content: _content,
                author: msg.sender,
                timestamp: block.timestamp
            })
        );

        uint256 messageId = messages.length - 1;

        emit MessagePosted(
            messageId,
            msg.sender,
            _content,
            block.timestamp
        );
    }

    // 获取全部留言
    function getAllMessages() external view returns (Message[] memory) {
        return messages;
    }

    // 根据留言 ID 获取单条留言
    function getMessage(uint256 _messageId) external view returns (Message memory) {
        require(_messageId < messages.length, "Message does not exist");

        return messages[_messageId];
    }

    // 获取留言总数
    function getMessageCount() external view returns (uint256) {
        return messages.length;
    }
}
```

## 部署Monad合约
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
