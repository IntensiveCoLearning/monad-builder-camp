---
timezone: UTC+8
---

# jeryluo555

**GitHub ID:** jeryluo555

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
今天总算是搞懂了remix智能合约的原理了  
**合约源码 → 编译 → 部署 → 合约地址 → read / write 调用 → 区块浏览器验证** 

主要遇见的问题就是在部署合约中的connectwallet里，没找到monadtestnet，之后用了Sepoliatestnet...
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

Solidity合约也太难了，即使布置好了也会遇到网络异常或者账户问题。。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->


今天把初中课程给过完了。  
  
在replit中，Replit 内置了 Secrets 管理器。Secrets 会被加密存储，并在运行时作为环境变量注入。智能体无法从文件系统中读取它们。

1.  在你的 Repl 里，点击左侧栏的 Secrets 标签（锁形图标）
    
2.  点击 New Secret
    
3.  把 key 设为 PRIVATE\_KEY，value 粘贴你的私钥
    
4.  点击 Add Secret
    

部署合约的代码可以通过 process.env.PRIVATE\_KEY 访问它，但智能体在任何文件中都看不到它。

在 Claude Code 中，内建工具箱包括：

-   Read：打开一个文件并查看其内容。
    
-   Write：创建一个新文件。
    
-   Edit：对已有文件做精确修改（diff 风格）。
    
-   Bash：运行一个 shell 命令（测试、构建、git，任何东西）。
    
-   Grep：在文件内容中搜索某个模式。
    
-   Glob：按文件名模式查找文件。
    
-   WebFetch / WebSearch：在线查资料。
    
-   Agent：派生一个子智能体处理一个聚焦的子任务。
    

harness：组装上下文、执行工具、管理循环的应用层代码（比如 Claude Code）。harness 是可配置的。

-   Skills / Slash Commands：可复用的工作流，用 /name 调用。例如：/init 用于生成 [CLAUDE.md](http://CLAUDE.md)，/review 用于审查 PR，/security-review 用于审计最近的改动。一个 skill 把 prompt（通常带特定工具和步骤）打包成一条命令。
    
-   Hooks：harness 会在特定事件自动运行的 shell 命令——工具调用之前、Claude 停下之后、文件被编辑时。非常适合自动格式化代码、拦截危险命令或发送通知。
    
-   Settings（settings.json）：控制权限（哪些工具自动批准）、环境变量、默认模型和 hook 配置。这里就是把 Claude 的行为从「凡事都问」调到「在这个沙箱里可以放手干」的地方。
    
-   Keybindings：自定义常用操作的键盘快捷键。
    

harness 是把一个原始模型变成可用产品的东西。不同的 harness（Claude Code vs. Cursor vs. Replit）以不同方式包裹同一类模型，服务于不同的工作流。

-   Monskills： [skills.devnads.com](http://skills.devnads.com)
    
-   Monskills GitHub： [github.com/therealharpaljadeja/monskills](https://github.com/therealharpaljadeja/monskills)
    
-   Monad 文档： [docs.monad.xyz](http://docs.monad.xyz)
    
-   Monad 区块浏览器（主网）： [monadscan.com](http://monadscan.com)
    
-   Monad 区块浏览器（测试网）： [testnet.monadscan.com](http://testnet.monadscan.com)
    
-   Protocols 仓库： [github.com/monad-crypto/protocols](https://github.com/monad-crypto/protocols)
    
-   Token list 仓库： [github.com/monad-crypto/token-list](https://github.com/monad-crypto/token-list)
    

可以把 EVM 想象成整个网络共享的一台计算器。当一笔交易进来时，每个节点都做同样的运算。如果它们都得到相同的答案，这笔交易就是有效的；只要有一个结果不一样，就说明出了问题。 

  

env 是英文 **environment（环境）** 的缩写。

在编程中，它通常指代**环境变量（Environment Variables）**。 项目中经常会有一个名为 .env 的隐藏文件，它的作用是**专门用来存储当前环境的敏感配置和参数**，比如：

-   数据库的账号和密码
    
-   第三方的 API 密钥（如微信支付、OpenAI 密钥）
    
-   服务器的端口号（比如 PORT=8080）
    

**为什么不直接写在代码里？** 为了安全和灵活。如果把密码直接写在代码里传到 GitHub 上，所有人都看得见，非常危险。而且同一个项目在你的电脑（本地开发环境）和服务器（生产环境）上，用的数据库密码通常是不一样的，使用 .env 文件就可以轻松区分。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->



今天进入了vibecoding的初中阶段，还在慢慢摸索中
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->




1.create a account on [replit.com](http://replit.com) 

2\. Write something to the Replit Agent, tell her what I want to build. 

3\. If there's a blank page, tell the agent and let her fix it.

4.Publish it and I can get my personal url. if there's a error with publishing, copy the error logs and tell the agent, let her fix it.

  

创世状态（Genesis State） stand for the first time of launching

节点（node）当一笔新交易发生时，每个节点都会独立地验证它。并不是单一节点说了算。如果某个节点掉线、或者试图作弊，网络的其他部分不会受到影响。这正是去中心化系统的韧性来源：没有单点故障。  

区块链（block chain）每个区块都通过密码学方式与前一个区块相连。改动其中一个区块，它之后的所有区块都会失效。这就是为什么这份记录是 不可篡改的（immutable）：事后无法更改。 

共识（consensus） 共识机制是一套所有节点都遵循的规则，用来就网络的当前状态达成一致。它决定哪些交易是有效的、哪些区块会被纳入，以及当两个节点意见不一致时该怎么办。 

Proof of Work（工作量证明）和 Proof of Stake（权益证明）。抵押是关键。如果一个验证者作弊——比如提议无效交易、或在状态上撒谎——他们就会失去自己质押的 ETH。这被称为 slashing（罚没）。失去质押资产的财务风险，让作恶变得极其昂贵，从而在不烧大量电的前提下保持网络安全。 

Gas（手续费）是你在区块链上执行交易所支付的费用。gas price波动由参与链上参与数量决定

Mainnet 与 Testnet；所有实验都进行在 testnet 上。任何涉及真钱的东西才在 mainnet 上 

Chain ID 是一个用来标识某条区块链的唯一编号。钱包和工具就是靠它来分辨「这笔交易是发给 Monad 的，不是 Ethereum、不是 Polygon，也不是某条别的链」。 

RPC（Remote Procedure Call 的缩写，远程过程调用）是你的钱包或应用用来与区块链对话的一个 URL。你发送的每一笔交易、读取的每一个余额、调用的每一个合约，都要经过 RPC。 

Faucet（水龙头） 是一个免费发放 testnet 代币的网站。之所以需要 faucet，是因为 testnet 代币没有真实价值，所以没人会卖。 

Block explorer（区块浏览器） 是一个让你查询链上任何东西的网站：交易、钱包、合约、区块。它是你查看链上正在发生什么的窗口。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->





完成了两个小任务，创建metamask钱包并且实现链上交易。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
