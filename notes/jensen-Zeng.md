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
