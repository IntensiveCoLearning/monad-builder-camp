---
timezone: UTC+8
---

# yanyang04

**GitHub ID:** yanyang04

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
今天学习了在Monad测试网上部署智能合约并完成了测试

1.合约地址：0x73bBdDe262634640fC330c1B5563E23C2Dd11826

2\. 部署交易 hash：0xb393cebdccb9c4125138ebcdf3cceccc043802efb05c71433c88241d5a28e6ac

![屏幕截图 2026-07-09 183752.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yanyang04/images/2026-07-09-1783594045052-_____2026-07-09_183752.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

/// @notice 最小链上留言板合约

contract MessageBoard {

/// @notice 单条留言数据结构体

struct Message {

address sender; // 留言发布者钱包地址

string content; // 留言文本内容

uint256 timestamp; // 发布区块时间戳

}

/// @notice 存储全网所有留言

Message\[\] public allMessages;

/// @notice 用户地址 => 该用户发布的全部留言

mapping(address => Message\[\]) public userMessages;

/// @notice 留言发布触发事件，支持链下监听

/// @param sender 发布者地址

/// @param content 留言内容

/// @param time 发布时间戳

event MessagePosted(address indexed sender, string content, uint256 time);

/\*\*

\* @dev 用户发布新留言

\* @param \_content 留言文本，禁止空内容

\*/

function postMessage(string calldata \_content) external {

// 校验留言内容非空

require(bytes(\_content).length > 0, "Message cannot be empty");

Message memory newMsg = Message({

sender: msg.sender,

content: \_content,

timestamp: block.timestamp

});

allMessages.push(newMsg);

userMessages\[msg.sender\].push(newMsg);

// 修复事件参数缺失bug

emit MessagePosted(msg.sender, \_content, block.timestamp);

}

/// @notice 查询链上全部留言

function getAllMessages() external view returns (Message\[\] memory) {

return allMessages;

}

/// @notice 查询调用者本人发布的留言

function getMyMessages() external view returns (Message\[\] memory) {

return userMessages\[msg.sender\];

}

/// @notice 查询任意指定地址用户的留言

/// @param \_user 目标用户钱包地址

function getUserMessages(address \_user) external view returns (Message\[\] memory) {

return userMessages\[\_user\];

}

}

这是一段**以太坊 Solidity 链上留言板最小合约**，实现去中心化公开留言功能，任何人连接钱包都可以在链上发布文字留言，同时支持多种方式查询历史留言，代码极简无多余复杂逻辑
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->


今天知道了什么是EPF

EPF = Ethereum Protocol Fellowship

它不教写 Solidity、不教发币、不做 DApp/NFT，专门面向以太坊底层协议层开发：

共识层 / 执行层客户端（Geth、Lighthouse、Teku、Prysm 等）

底层密码学、分片、Verkle 树、MEV、PoS 优化、协议规范 EIP 落地

网络测试、安全审计、协议研究论文

项目运行规则

周期：每一期 5 个月（EPF7 最新一期 2026 年 6-11 月）

准入无许可：任何人都能提交申请，无需背景门槛

学习模式（学徒制）

学员自主选底层技术课题、提交项目方案，全程参与真实以太坊开源开发，交付可合并进官方仓库的代码 / 研究成果；大量 EPF 毕业生直接入职以太坊核心开发团队、Flashbots、Optimism 等头部机构。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->



[https://x.com/tj1yVQuH8p7668/status/2073868297481920770](https://x.com/tj1yVQuH8p7668/status/2073868297481920770)
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
