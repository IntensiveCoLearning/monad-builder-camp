---
timezone: UTC+8
---

# ComAsUare

**GitHub ID:** ComAsUare

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
1、进入epf回放观看。

2、实习手册看完

3、参加周例会

项目记录仓库：[https://github.com/ComAsUare/LearnMonadBuild](https://github.com/ComAsUare/LearnMonadBuild)

相关笔记：

```markdown
1. ethereum protocal fellowship: 协议层学习，研究，提案，交付。
    区别于合约培训班：需要交付可验证性贡献。
    每年3-4月自学，填写申请表
2. epf资源：
    官方文档；https://epf.wiki/
    epf discord群组
    
3. 学习过程：
    1) 学习；补协议基础，理解自己感兴趣问题
    2）写作；每周进展，下一步计划。
    3）讨论： standup, office hour, issue, pr。mentor会跟进并交流
    4）交付： 路线图，交付
4. 申请通过后，6-11月：
    1. 准备 协议，read.
         协议的核心讨论都在github上：[eth-protocol-fellow](https://github.com/eth-protocol-fellows)
    2. 定方向
        了解项目领域，写topic of interest
    3. 成项目
        深挖问题，写project proposal
    4. 做交付
        执行roadmap, 每周development update
    5. 做展示
        项目wrap-up.持续贡献
5. 学习顺序要服务于一个目标
    1. 先了解地图。
```
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->

观看agent支付、进入epf两个分享。

```markdown
ai高效，安全的支付是核心问题；
1.和传统支付场景不同：小额高频支付，agent无身份认证
    速度，凭证(credentials),粒度(granularity): 传统0.5$, agent:0.001micropayment
    agent自动执行，也不会像传统那样每笔交易确认，点击。

2. fluxa产品 端到端ai payment 组件，包括钱包

3. 场景分类：
    *1 a2a
    *2. agent 2 merchant： 2b， 订酒店
    *3. agent 2 tool: 无需订阅，每次api调用，skills, mcp付费
    *4. 转账社交的逻辑。
4. 产品矩阵：
    *1. agent wallet
        一键撤销revoke, 更灵活spending control , offline approval, audit trail.
        钱包场景： 身份证明，拿到bugget，为服务付费，接受支付，把钱花出。
        x402协议与agent
    *2. agent card
        *1. agent card ai 身份认证是为了风控ai幻觉造成乱花钱。
        *2.  finantial harness engineering
    agent风控,固定bugget，一次性。
    *3. agent charge agent收付款。
    *4. 开发者skills. mcp收款
        agent embedded payment protocol. 类似于layer1, layer2。把收款人验证，接受服务放在链下，来提高效率
    *5. user case: 社交，agent红包
    *6  user case: 自动化任务平台
    *7. user case： 订票

```
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->


开营仪式，xiaohai build分享，直播回放。

相关笔记：

```markdown
1. 五周规时间划，关键节点：week3协作制作demo，week4黑客松
2. marcus分享monad：
    1. 链上活动增长需求->高吞吐量区块链:
        * 1. 稳定币供应量增加
        * 2. DEX现货份额，现货区分于衍生品/合约交易
        * 3. 预测市场交易量
        * 4. a2a
    2. 提高吞吐量、速度的技术：
        * 1. 本质是对区块链不可能三角进行权衡和优化
        * 2. block time，出块时间，生成一个区块的时长； TPS ，吞吐量，每秒处理交易数。
        * 3. 技术分类：
            ** 1. L1 基础层优化
                *** 1. Parallel Execution
                    Solana（SVM 架构）、Move 语言生态（Aptos、Sui），以及 Parallel EVM（如 Monad、Sei）
                *** 2. 共识机制与出块机制优化
                    Solana PoH（历史证明）
                *** 3. sharding分片技术
                    NEAR Protocol 的夜影协议（Nightshade）、TON（自适应无限分片架构）
            ** 2. L2 链下分层外包
                *** 1. rollup卷叠技术
                    Optimistic Rollups（乐观卷叠）， ZK Rollups（零知识证明卷叠）
                *** 2. State Channels
                    比特币闪电网络
    
    3. 实时验证者可视化网址： gmonads.com
    4. 节点门槛不提高，通过架构提高速率，而不是提高节点计算性能
    5. 除了Parallel Execution， monad在基础层使用的原语：asynchronous excution流水线化共识和执行，以及monadDB状态查询
        * 1. asynchronous excution
            传统共识-执行交错，共识耗时>>执行。
    6. monad在共识和runtime中使用的技术：
        * 1. JIT(just in time) compliation
        * 2. monadBFT 共识
        * 3. raptocast 区块传播
```
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
