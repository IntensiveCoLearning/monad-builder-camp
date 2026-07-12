---
timezone: UTC+8
---

# BIGrice66

**GitHub ID:** BIGrice66

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
今天一天都不在家，到家已经挺晚了，在cc的帮助下整理了这一周的所做所学，思考再三，最后还是选择对技术要求不算太高的运营方向，准备交作业了
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->

今天利用cc做的一个小东西  
要解决什么问题

做一个"链上判断力声誉系统"——让人对二选一事件公开下注、写理由，结果出来后验证谁真有判断力。虎扑"预言家"的链上版。

V1 → V1.1 → V1.2 的演化

V1（单社区 + Badge）：最朴素的想法——预测对错计胜负、给理由打分、发勋章。勋章系统是 9 级枚举（从 GAMBLER 到

LEGEND），按 goodReasonWins/losses 计算。问题：勋章是离散标签，缺乏梯度；社区投票靠 VOTE\_FEE 防刷，门槛太高没人投票；质疑窗口（DISPUTE\_BOND = 1 MON）太重，轻量预测场景不实用。

V1.1（多社区 Badge）：加了多社区隔离，每个社区的声誉独立。但 Badge 是重资产——枚举穷举所有状态，升级路径僵化，难扩展。

V1.2（XP 升级 + 加权投票替代 Badge）：核心思路变了——不给你贴标签，改用连续的 XP 数字 +

等级（新秀→传奇），升级路径平滑可预期。同时去掉 VOTE\_FEE，投票免费但按等级加权（Lv1 1 票、Lv6 13

票），靠权益激励参与而非靠收费挡水军。保留 30 天不活跃衰减（每天 -10%），防止僵尸账号。灵魂绑定 NFT（getNFTData）给

Lv5+ 自定义勋章 URI，留了未来扩展口。getPendingResolutions 给外部 AI 脚本预留接口，机器自动查赛果结算。

部署流程

Remix + MetaMask Injected Provider → Monad Testnet。两次踩坑：① VALUE 残留导致非 payable 函数 revert（predict 的 0.1

MON 没清掉），② Remix 重启后合约丢失，靠 "At Address" 回绑已有合约。

V1.2 验证结果

完整链路跑通：registerCommunity → predict（0.1 MON + 20字理由）→ voteReason（加权投票 → predictor +5 XP）→

resolve（correct + goodReason → +100 XP）→ getLevel（105 XP, Lv1 新秀）。合约地址 0x7880...10ff，Monad Testnet。

还没做的

\- 降级惩罚（错误+烂理由扣分）

\- view 函数需要通过网页前端调用，Remix 体验太差

\- ERC-721 灵魂绑定勋章铸造（代码留了 customURI 和 getNFTData 但没连 NFT）

\- AI 结算机器人（getPendingResolutions 接口写好，外部调用还没写）
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->


1.今天听了同学们的分享会，感受良多，web3.0是一个特殊的领域，尽管发展多年，但仍有许多领域待人探索。2.用 AI 生成了一个Solidity 合约，快到12点了，打卡先，等等上remix看看怎么个事儿
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->



跟着buildanything第一次用ai做了一个网页，还不是很熟练，不知道如何让ai能更精确地替换网页中的图片，也可能是我要的内容太多了，借着ai的力量把d1-d3又简单过了一遍，等等要去踢球，晚上回来过d4
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->




读完了d1d2的课程文章，正在读d3的法律章节，完成了第一次转账，现在正在查询，等等用remix试着做一次留言，今天就先到这吧..
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->





今天才开始看day1的内容，明天尽量把进度赶上吧...大致了解了区块链的基本逻辑，终于把“挖矿”、“比特币”这些此前模糊的概念弄明白了，现在准备看以太坊概览  
以下是对区块链的理解：区块链顾名思义是由block区块组成的链，区块头尾相连，连成物理世界的线段，区块链和区块都有前后两端。区块由交易信息和哈希组成，哈希又由上一区块的交易信息与上上区块的识别信息组成。哈希由函数处理后得出哈希值，这东西稍微改动一点就不是原来那个了。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->






下载了 meta mask，从 Monad Faucet 领取了测试币，下一步是前往 Remix，不过马上要到12点了，先提交吧。  
复制了别人的学习笔记，链上产品和普通互联网产品最大的不同是数据和资产状态记录在区块链上，而不是只存在某个平台自己的服务器里。普通互联网产品通常用账号密码登录，数据由平台管理；链上产品通常用钱包地址和签名交互，交易和资产变化可以在区块浏览器公开查询。链上产品的规则很多时候由智能合约执行，所以更透明，但操作成本和安全要求也更高。在学校最喜欢期末周抄人家的笔记了。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
