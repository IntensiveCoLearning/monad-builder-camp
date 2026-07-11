---
timezone: UTC+8
---

# xxxkuze-tools

**GitHub ID:** xxxkuze-tools

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
## 今天总结下这周的内容

### 主要完成了两个链上的实践

-   日常打卡合约**：** 实现了基于链上时间戳的用户状态记录，用于验证用户的持续活跃度
    
-   任务排行榜与积分体系合约：每日签到（结合时间周期验证，为连续活跃用户发放基础积分），指定任务（包括交易一笔monad，验证用户对特定交互或业务逻辑的完成状态，并同步记录积分）
    

### 你遇到了哪些问题

-   私钥格式不匹配**：** 从 MetaMask 导出私钥时，默认生成的是不带 `0x` 前缀的 64 位十六进制字符串。
    
-   工具链报错**：** 我直接将该字符串填入 `.env` 文件，而底层的 Web3 部署脚本（如基于 Ethers.js 或 Viem 的工具）在初始化 Wallet/Signer 时，严格要求私钥字符串必须以 `0x` 开头，导致脚本解析失败、签名初始化报错，无法顺利完成合约部署。
    

### AI 帮你解决了什么

-   基础合约代码生成**：** 快速编写了 `contracts/` 目录下的核心 `.sol` 业务代码，包括打卡逻辑、积分数据结构（Mapping 与 Array 的结合应用）、排行榜排序逻辑以及邀请人的关联存储。
    
-   部署脚本的自动兼容性修改**：** 针对 MetaMask 私钥报错问题，AI 对部署脚本进行了逻辑优化，加入私钥格式自动检测与兼容算法——在读取 `process.env.PRIVATE_KEY` 时，自动判断字符串首部是否存在 `0x`，若无则自动拼接，从代码层面彻底解决了不同钱包导出私钥格式不统一的部署痛点。
    

### 哪些地方必须由你人工判断

-   业务逻辑与需求匹配度校验**：** AI 写出的 Solidity 代码往往能通过编译，但这并不代表它符合业务逻辑。我必须人工检查 `.sol` 文件，判断其是否存在漏洞或偏离需求。例如：每日签到的时间锁是否严格限制在 24 小时？积分排行榜的更新逻辑是否会消耗过多的 Gas？
    
-   安全与环境配置约束**：** 涉及到真实的资产和账户权限（私钥管理、`.env` 的 `.gitignore` 配置）时，必须由人工主导。在部署脚本出现异常时，需要由我准确定位问题边界（是合约逻辑问题还是脚本传参问题），再向 AI 下达明确的修改指令。
    

### 你对 Monad 或 Web3 的理解发生了什么变化

-   开发范式的转移**：** 过去我认为 Web3 链上实践的最大门槛是熟练掌握 Solidity 语法；通过这次 AI 辅助实操，我认识到具体的编码工作正在被 AI 大幅平权，真正的核心竞争力在于“机制设计”与“业务验收能力**”**。
    
-   高并发公链与应用场景的结合**：** 像打卡、任务积分排行榜、轻资产的“增长型逻辑”，在传统的低速、高 Gas 公链上极难有好的用户体验。而像 Monad 这类强调极致性能、高吞吐量（EVM 兼容且具备并行执行能力）和超低手续费的底层基础设施，正是让此类面向大众的 Web3 社交增长与积分系统能够真正落地、达成 Web2 级别流畅体验的土壤。
    

### 你更适合进入 Tech、Ops 还是 Research 方向

从我目前做的两个业务来说，比较倾向于做 Ops（运营），因为我所实践的题目（签到、积分、排行榜）天然带有极强的用户增长与运营属性。我又擅长借助 AI 完成底层编码与排障，将个人的核心精力释放到需求定义、功能验证、业务闭环和落地调优上。这种“懂技术实现、能看懂合约逻辑、且精通如何用技术工具达成运营目标”的能力，正是目前 Web3 生态和协议层（如 Monad 生态应用）在获取用户、做真实 DApp 活跃度时最急需的 Tech-Ops 桥梁能力。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

今天理解适合 Monad 的高频交互场景，并说明为什么它可能更适合在 Monad 上构建，我选择的是排行榜 / 任务系统。

举个例子：

-   每天签到
    
-   完成任务获得积分
    
-   实时更新排行榜
    
-   前100名获得奖励
    

1.  **为什么需要频繁交互？**
    

-   用户每天签到、完成任务、领取奖励、查看排名，都会不断产生交互。
    

2.  **链慢或 Gas 高有什么问题？**
    

-   排名更新慢，用户不知道自己是不是前100。
    

-   每次签到都要花很多手续费，大家就不愿意参与。
    

3.  **Monad 能带来什么？**
    

-   低延迟让积分和排行榜几乎实时更新。
    

-   高性能支持很多人同时参加活动。
    
-   EVM 兼容，开发者容易开发。
    

4.  **为什么需要上链？**
    

-   排名公开透明，不能后台修改。
    

-   奖励发放公平，所有人都能验证。
    

### 一句话概括

**用户每天签到、完成任务、领取奖励都会产生大量链上交互。如果区块链速度慢或手续费高，排行榜更新不及时、参与成本过高，用户体验会变差。Monad 的高性能和低延迟能够支持大量实时交互，让积分和排行榜快速更新，同时链上记录保证排名和奖励公开透明，避免后台篡改。**
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->


听了Coooder老师的co learning，今天学习使用AI Coding来做一个智能合约项目，首先我选择codex来做，首先创建一个文件目录，用来存放项目，然后导入进去codex，在这个项目里面与它对话，内容：\*\*帮我做一个发行nft的智能合约，\*\*等待codex完成（过程中有些指令需要确认）

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/xxxkuze-tools/images/2026-07-09-1783579549303-image.png)

执行过程：检查项目目录，写入文件，编码，安装依赖，部署吗，测试，最终生成结果

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/xxxkuze-tools/images/2026-07-09-1783579856430-image.png)

接着继续对话：**需要部署到monad的测试网**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/xxxkuze-tools/images/2026-07-09-1783579900446-image.png)

这里需要把密钥填写到`.env.example`，这里需要复制一份.`env`

```
copy .env.example .env
```

打开`.env`文件，这里的`PRIVATE_KEY`需要填写，密钥需要在钱包里面，找到账户，账户详情，点击私钥

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/xxxkuze-tools/images/2026-07-09-1783581002696-image.png)

跟codex对话，\*\*帮我运行命令，\*\*这里会发现部署失败，是因为我的密钥不是`0x`开头的，需要叫codex改

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/xxxkuze-tools/images/2026-07-09-1783581617481-image.png)

接着对话codex，**我的私钥不是0x开头的，我是metamask钱包**

![image.png](https://cdn.nlark.com/yuque/0/2026/png/50461568/1783525657705-c4c8f5ca-ff00-4032-b638-0ab267afca9e.png?x-oss-process=image%2Fformat%2Cwebp)

再次叫codex执行命令，经过了几轮的对话，终于是部署成功了

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/xxxkuze-tools/images/2026-07-09-1783581715115-image.png)

今天使用了AI Coding完成合约的部署
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->




今天学习用 AI 生成一个最小 Solidity 合约，我选择了每日打卡合约，写了个提示词，让chatgpt给我生成 Solidity ^0.8.20 的代码，CheckIn包含totalCount（累计打卡次数），lastCheckIn（最后打卡时间），streak（连续打卡天数），然后是三个函数，checkIn() -> 用户每天只能打卡一次，canCheckIn() -> 判断今天是否还能打卡，getMyRecord() -> 查看自己的打卡信息，AI生成的合约代码，人工检查了Remix 能编译通过，三个函数与 Prompt 一致，数据类型正确，没有不必要的库，变量命名和注释读得懂。然后部署到Remix。大概的流程：Remix 新建 `ChecklnContract.sol` → 编译 `0.8.20` → **Deploy & Run Transactions** → Browser Extension 连 Rabby → 选 Monad Testnet → Deploy，交互Remix **Deployed Contracts** 里：蓝点函数 **call**（如 getMyRecord），橙点函数 **transact**（如 checkIn）。Write 后在区块链浏览器查看确认交易 Status 为 Success。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->





听了7.6晚上参加「Co-learning」线上活动，自己尝试在浏览器上安装MetaMask插件，创建钱包，添加 Monad Testnet 网络，添加成功之后，复制钱包地址，打开区块浏览器，我看到的页面信息，**Address**：我的钱包地址，一串 `0x` 开头的字符，**Balance**：当前账户余额，刚创建时应该是 `0 MON`**，Transactions：**历史交易列表，空钱包时这里是空的。从而**我理解链上产品和普通互联网产品最大的区别是**：普通互联网产品的数据主要存在平台自己的服务器里，而链上产品的关键操作会记录在区块链上，可以通过区块浏览器查询。接着继续了解**行业赛道全览和Web3 工作方式。**
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->







今天观看了开营仪式回访内容，学习了web3实习手册，了解了区块链基础概念，以太坊概览，下一步会继续学习了解行业赛道全览，web3的工作方式。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
