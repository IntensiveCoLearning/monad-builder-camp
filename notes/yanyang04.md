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
# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
学习了适合 Monad的场景

1\. Research：

场景高频交互需求：多人游戏内玩家移动、拾取道具、对战操作、开宝箱、排行榜更新均需要实时上链，单局数十人会持续产生大量独立小额交易，天然属于高频交互场景。

传统公链体验痛点：普通 EVM 链串行执行，多人同时操作会造成交易排队拥堵，单次交互确认等待数十秒，破坏游戏实时反馈；频繁操作累积高额 Gas，普通玩家不愿持续参与。

Monad 适配优势：并行执行架构可同步处理无冲突玩家操作，亚秒级出块实现手游级实时反馈；极低手续费支撑玩家无压力高频操作；全 EVM 兼容可直接复用 Solidity 合约与 Remix 开发工具，降低开发成本。

链上存储必要性：游戏道具、对战胜负、积分资产属于用户自有数字资产，若使用中心化数据库存在道具篡改、平台跑路风险；链上不可篡改记录保障玩家资产确权与对战结果公平透明，是普通数据库无法替代的。

2\. Tech：

基础链上交互功能

玩家入场 / 退出：链上记录玩家账户、初始道具、初始积分

实时动作上链：移动、拾取道具、攻击对战，每步操作生成独立链上交易

道具系统：开宝箱铸造随机道具、道具转移、道具销毁，读写合约状态

积分排行榜：实时更新玩家对战积分，链上实时查询排名

结算模块：对局结束自动发放奖励道具 / 积分，链上校验对局胜负

配套开发功能

合约读写接口：read 函数查询道具、积分；write 函数执行游戏动作、结算

测试网适配：一键部署至 Monad Testnet，兼容 MetaMask 钱包交互

交易日志读取：抓取链上动作记录，前端渲染游戏对局回放

3.Ops：

0Gas 休闲链上小游戏

主打卖点：Monad 低手续费实现零门槛游玩，打出 “不用心疼 Gas，随便点随便玩” 传播点，对比以太坊高 Gas 形成反差 Meme，吸引普通 Web2 玩家入场。

全链上公平对战小游戏

传播点：所有对战动作、胜负记录、道具全部链上可查，无后台篡改空间，主打 “不存在暗箱操作的链上竞技”，面向竞技、区块链公平性爱好者社群。

多人在线链上派对小游戏

切入点：轻量化多人联机派对玩法，支持数十人同时在线互动，突出 Monad 高并发承载优势，产出 “Web2 级流畅联机” 体验对比素材，适合社区直播、活动引流。
<!-- DAILY_CHECKIN_2026-07-10_END -->

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
