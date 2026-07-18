---
timezone: UTC+8
---

# jensen-Zeng

**GitHub ID:** jensen-Zeng

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
### 一、 智能合约做什么

智能合约上是一段代码，用于自动处理链上产生交易时和之后需要执行的规则，例如发行、转移和管理代币、去中心化金融（DeFi）、去中心化组织（DAO）等等。

### 二、 智能合约的部署

部署智能合约就是将编译好的代码写入区块链的过程，通常包含步骤如下：

1.  使用 Solidity 或 Vyper 等编程语言编写合约代码。
    
2.  编译成字节码和 ABI。Bytecode是实际部署到链上的机器码，ABI是前端和外部程序调用合约函数的“说明书”。
    
3.  使用节点服务连接到以太坊主网或测试网。
    
4.  使用钱包支付 Gas 费，将合约发布到链上，成功后会生成唯一的合约地址，代表部署成功。
    

### 三、 与智能合约的交互

与智能合约交互就是调用合约中的函数（如读取代币余额或转账），主要通过dApp进行。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->

Web3技术的就业方向如下  
**1\. 智能合约开发**：负责编写、测试和部署运行在区块链上的智能合约（相当于后端程序）。需要掌握 Solidity、Rust 等编程语言；熟悉以太坊虚拟机（EVM）、Solana 等链的生态系统。

**2\. 区块链协议与基础建设开发**：负责构建和维护区块链底层架构、共识机制、跨链桥及节点基础设施。需要熟悉底层密码学、Go、Rust 或 C++；了解以太坊客户端开发或 Layer2（如 ZK-Rollups）技术。

**3\. 全栈开发：**负责构建与用户直接交互的去中心化应用程序（dApp）界面，连接用户钱包并与智能合约进行交互。需要掌握传统前端技术（React, Vue）；熟悉 Web3.js、Ethers.js 或 viem 等库，以及 MetaMask 等钱包的集成。

**4\. 区块链安全与审计（今日coleaning，类网络安全方向）：**专门对智能合约和底层协议进行安全检查和代码审计，查找漏洞以防止黑客攻击，保障资金安全。需精通多种区块链语言，具备极强的代码逆向工程能力、逻辑思维和攻防经验。

**5\. DevOps 与节点运维：**负责区块链节点的搭建、维护，以及自动化部署工具的开发，确保 Web3 服务的稳定性和高可用性。需要熟悉 Docker、K8s 等云服务。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->


Monad合约的优势：  
1、高吞吐量的同时具备较短的的出块时间和极短的交易确认时间。  
2、交易费用极其低廉，由于其底层技术优化，计算和存储效率大幅提高。  
3、完全兼容以太坊虚拟机，以太坊上的合约、钱包、开发工具和代码可以直接迁移，开发者重写代码。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->



记录今天学习遇到的问题：  
在remix中编辑合约时，连接钱包应该选Browser Extension还是WalletConnect？  
metamask中添加monad测试网络后不显示，需要在设置中打开“显示测试网络”，但仍未找到。  
连接钱包后，需要切换到monad测试网络，才能在测试网络中部署，但仍未找到怎么切换

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/jensen-Zeng/images/2026-07-13-1783952407420-image.png)
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->




### prompt

生成一个最小的，有留言功能的板Solidity 合约

### 合约源码：

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

contract MessageBoard {

string private message;

address public lastAuthor;

uint256 public updatedAt;

event MessageUpdated(

address indexed author,

string newMessage,

uint256 timestamp

);

constructor(string memory initialMessage) {

require(bytes(initialMessage).length > 0, "Message cannot be empty");

message = initialMessage;

lastAuthor = msg.sender;

updatedAt = block.timestamp;

emit MessageUpdated(msg.sender, initialMessage, block.timestamp);

}

function setMessage(string calldata newMessage) external {

require(bytes(newMessage).length > 0, "Message cannot be empty");

require(bytes(newMessage).length <= 280, "Message too long");

message = newMessage;

lastAuthor = msg.sender;

updatedAt = block.timestamp;

emit MessageUpdated(msg.sender, newMessage, block.timestamp);

}

function getMessage() external view returns (string memory) {

return message;

}

}

### 人工判断该合约

1、留言不能为空，需要加入了 require 检查，避免用户提交空内容。

2、链上存储字符串会消耗 Gas，必须限制留言长度。

3、保留了 MessageUpdated 事件，用于在区块浏览器中查看每次留言更新的记录。

### AI 生成代码需要人工检查的地方

1、基础的变量、事件、构造函数。

2、是否符合用户需求。

3、边界条件与资金风险
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->





Solidity合约就是用于自动处理web3中各种需求的自动化程序，相当于后端。  
但是合约的优势在于自动运行，对链上所有人透明可见。

基于此，Web3中就有了和Web2一样构建各种应用的基础，比如游戏、NFT、社区共建等等
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->






区块链一笔交易经历的过程：

1、用户在钱包中发起交易，填写接收地址、转账金额、Gas 费用等信息

2、钱包使用私钥对交易签名，证明这笔交易由该地址的控制者真实发起

3、签名后的交易会被发送到区块链网络中的节点，所有节点会检查交易是否合法

4、验证通过的交易会矿工选中，并打包进新的区块

5、利用共识机制确认新区块的有效性

6、区块被确认后，交易状态会变为成功或失败，并永久记录在链上，交易完成
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->







### Web3的价值

Web2产品的数据由平台集中管理，用户仅拥有使用权；而链上产品的数据在区块上，用户完全掌握数字资产的所有权。

传统应用依靠企业信誉或权威机构背书；链上产品则依靠所有区块的共识机制产生信任。

链上产品的交易记录对全网节点公开透明，具有高度可追溯性；Web2数据则由平台控制披露与否。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->








# 新进入Web3世界

安装metamask，创建自己的钱包地址，领取测试币并尝试测试交易。

参加线上活动「DevRel 的成长之路 —— 从 Builder 到生态连接者」线上活动和co-learning，了解到了真实Web3的行业现状和职业发展

学习区块链基础概念：去中心化、BTC、公钥私钥的区别、公链与私有链的区别。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
