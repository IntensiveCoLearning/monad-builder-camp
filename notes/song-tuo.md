---
timezone: UTC+8
---

# song-tuo

**GitHub ID:** song-tuo

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
![Weixin Image_20260710190640_269_2.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/song-tuo/images/2026-07-10-1783681933203-Weixin_Image_20260710190640_269_2.png)
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->

![cab88576632f3d57cfa1f1a9fac07351.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/song-tuo/images/2026-07-09-1783597076612-cab88576632f3d57cfa1f1a9fac07351.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->


# 课程笔记：Agent 如何高效、安全地花钱（FluxA · AI-Native Payment Infrastructure）

## 主题

把支付基础设施**从"为人设计"改成"为 agent 设计"**——让 AI agent 能自主、安全、即时地收付钱，同时不把人类用户暴露在金融风险里。定位是"**the checkout layer for AI agents**"。

## 1、核心痛点：传统支付逻辑撑不住 agent

Agent 已经从"被动聊天机器人"变成"能规划、推理、用工具、执行多步任务的自主经济体"。但**一到付钱那一步就撞墙**。原因是传统支付的五个前提对 agent 全部失效：

| 需求 | 传统世界 | Agent 的现实 |
| --- | --- | --- |
| 身份 Identity | 人的身份证 / KYC | agent 没有法律身份（你记的"没有身份证"） |
| 授权 Authorization | 人手动点击、签名 | 需要自主执行，不能每笔都等人点 |
| 凭证 Credentials | 表单里填卡号 | 卡号出现在 prompt 里极不安全（敏感信息安全性） |
| 速度 Speed | T+1 到 T+3 | 需要毫秒级响应 |
| 颗粒度 Granularity | 最低 $0.50 起 | 需要 $0.001 级微支付（支付颗粒度） |

→ 这就是"支付链路的痛点"：**没有身份、不能自主授权、凭证不安全、太慢、颗粒度太粗**。

## 2、产品形态：一个给 agent 的"共管钱包"（Co-wallet）

关键设计哲学：**不是自托管，也不是纯托管，而是"共管"（Co-managed）——人定规则，agent 在规则内自主操作。**

对比传统：

| 能力 | 传统 | FluxA |
| --- | --- | --- |
| 花钱控制 | 全有或全无 | 每笔限额（per-tx limits） |
| 撤销 | 改代码 | 一键撤销 |
| 离线审批 | 做不到 | 预授权策略 |
| 审计 | 手动记录 | 自动追踪（可读的账本） |

一句话：**Set one budget, approve one mandate**（设一次预算、批一次授权），agent 就能到处花钱，还留一份你真能看懂的账本。

## 3、最关键的安全思想：签"目的"，不签"每一笔"（Intent-Pay）

这是解决"敏感信息安全 + 授权"的核心机制，也是最值得记的一页：

-   **传统**：每笔支付都要人点一下 → agent 停下、等待、丢失上下文 → 本质是"带卡号表单的聊天机器人"。
    
-   **Intent-Pay**：只打断你**一次**，之后 agent 自己跑：
    
    1.  **Draft** — agent 读懂任务，提出一个支付意图（预算 + 用途）
        
    2.  **Sign Once** — 用户批准这个意图，意图内的每笔由钱包自动签，无需逐笔审批
        
    3.  **Harness** — 风险引擎兜底：**符合任务的放行，偏离任务的在钱包层直接拦截**
        

→ "签目的不签每笔"，让主动型 AI 保持主动，同时守住风险边界。

## 4、凭证安全怎么做：一次性卡 AgentCard

针对"卡号在 prompt 里不安全"这个痛点，桥接传统信用卡结账：

1.  agent 判断需要一笔 $450 机票支出
    
2.  向钱包申请一张**锁定 $450**的 AgentCard
    
3.  立即签发一次性 Visa/Master 卡号
    
4.  agent 在商家结账页填卡
    
5.  支付成功 → **卡自动销毁**，未用余额退回钱包
    

三条安全属性：**金额锁定**（不能被多扣）、**单次使用**（泄露也没用）、**零凭证泄露**（真实金融信息永不进入 agent 上下文）。

## 5、协议层：AEP2（Agent Embedded Payment Protocol）

把支付**直接嵌进 agent 请求里**——给 x402 / A2A / MCP / tool call 附上一次性签名支付授权（signed mandate）。核心 5 步：**Authorize（签一次性授权）→ Verify（链下验证）→ Serve（不等链上确认就服务）→ Settle（延迟结算）→ Batch（批量结算、支持微支付）**。

## 6、Agent 能做的 8 件事（能力全景）

Identity（Agent ID）· Spending Budget（预算授权）· x402 Payment · Payment Link（收款）· Payout（转账）· Agent Card（一次性卡）· Paid API/MCP（付费调用）· Earn（接 A2A 任务赚钱）。

## 7、商业化 & 市场

-   **收钱侧 Agent Charge** 三种模式：Monetize Gateway（零代码代理网关，每次调用自动扣费）/ x402 Payment Link（动态金额收款链接）/ Pay to Agent（按 Agent ID 直接转 USDC）。
    
-   **Monetize**：把任意 script / API / MCP 几分钟变成"付费 skill"——零代码网关、按次计费（如 $0.01/次）、无需 key 无需 auth。
    
-   **Traction**：注册 agent 钱包 110K+、日峰值交易 30K+、ODP 支付延迟 0.28s（相比传统 x402 的 4.4–4.9s，约 15× 提升）。
    
-   **生态**：Coinbase、Cloudflare、Ant Group、Qwen、MoonPay；工具集成 Claude、Cursor、Codex。团队背景是前阿里 / 蚂蚁高管。
    

## 8、总

Agent 高效安全花钱的关键 = **给它一个身份（Agent ID）+ 一次授权一个目的（Intent-Pay）+ 用一次性凭证隔离敏感信息（AgentCard）+ 风险引擎在钱包层兜底 + 微支付级颗粒度**。让 AI agent 成为"一等的经济公民"，但人始终握着规则和一键撤销。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->



# 普通开发者如何进入以太坊协议层（EPF7）

## 1、核心心法

思路：**先入门，能复现，再贡献。**

不要等"完全准备好"才开始，先让自己**连续几周产出可见的证据**（命令记录、公开 update、issue comment、小 PR）。

## 2、选项目：先选问题域，再选 issue

| 不稳的写法 | 更稳的写法 |
| --- | --- |
| 我想做某个 issue | 我想改善一个问题域 |
| 如果它被别人合并，项目就失去中心 | issue 是证据，PR 是阶段性交付 |

一个 EPF 项目要能撑住几个月，不能只押注在"某一个还没被人做的 issue"上。把自己锚定在一个**问题域**（比如：测试可靠性、客户端某个模块、SSZ / 数据证明作为深入方向），issue 和 PR 只是这个问题域下的阶段性产出。

## 3、4 周启动路线

| 周次 | 目标 | 具体动作 |
| --- | --- | --- |
| Week 1 | 画地图 | 用 EPF Wiki / Study 建立 EL、CL、节点、交易的基本认知图 |
| Week 2 | 跑起来 | 跑一个本地工具、测试或轻量 devnet，记录命令和遇到的问题 |
| Week 3 | 读进去 | 选一个仓库，读一个 issue、一个 PR、一小段 spec |
| Week 4 | 公开输出 | 写一篇学习笔记、发 issue comment，或尝试一个小 PR |

## 4、给自己定位（三个问题）

1.  **我现在在哪一层？** 还在补基础 / 已经能读 spec / 已经能跑测试 / 已经想找 issue
    
2.  **我下一周做什么？** 看一组课程 / 跑一个仓库 / 写一篇笔记 / 问一个具体问题
    
3.  **我怎么证明进展？** 命令记录 / 公开 update / issue comment / 小 PR
    

## 5、几条重要经验

**PR 要小**。大 PR review 很困难，小的阶段性 PR 更容易被 merge，也更容易积累信任。

**不要太功利**。web3 整体还在很初级的阶段，看价格没有意义，真正能撑住长期投入的是**对以太坊本身的热情**。

## 6、参考资源

客户端 / 项目点子：[cohort-seven](https://github.com/eth-protocol-fellows/cohort-seven/blob/main/projects/project-ideas.md) [project-ideas.md](http://project-ideas.md)

以太坊改进提案（EIPs）：[https://ethereum.org/zh/eips/](https://ethereum.org/zh/eips/)

以太坊生态入门 Wiki：[https://epf.wiki/#/README](https://epf.wiki/#/README)

执行层规范仓库（可作为读 spec / 找小 issue 的入口）：[https://github.com/ethereum/execution-specs](https://github.com/ethereum/execution-specs)
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
