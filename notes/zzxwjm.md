---
timezone: UTC+8
---

# Zixie Zheng

**GitHub ID:** zzxwjm

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
**1\. 分轨初步理由**

我目前更倾向于 **Research 为主，Tech 为实践支撑，Ops 作为表达辅助**。

我不是 CS 或金融背景，纯 Tech 还需要补很多基础；但这周完成钱包、交易、合约部署和交互后，我也意识到 Tech 是理解 Web3 的必要工具。相比之下，Research 更接近我的背景。我会自然关心：链上行为到底代表什么？badge、积分、排行榜是否真的代表贡献？如何区分真实参与和刷量？这些问题和我的心理测量、研究方法背景更有连接。

**2\. 本周最重要的学习收获**

1.  我重新理解了交易：链上 transaction 不只是买卖，而是一次签名后的状态改变请求。转账、部署合约、调用 `createTodo` 都是状态改变。
    
2.  我理解了钱包、链和区块浏览器的关系：钱包负责签名，链记录状态，区块浏览器让状态改变可以被公开验证。
    
3.  我意识到 AI 可以辅助学习，但不能替代人工判断。合约权限、安全边界、是否使用课程钱包、哪些内容不该上链，都必须由我自己负责。
    

**3\. 希望助教或同伴帮助的问题**

我最想了解的是：Web3 Research 到底在做什么？它是偏行业研究、协议研究、用户研究、机制设计、链上数据分析，还是产品策略？

其次，我想知道我的背景是否有真实价值。我不是工程或交易出身，但我会关注“链上记录到底代表什么”“指标是否有效”“激励是否会扭曲行为”这类问题。我想知道这些问题在真实 Web3 项目中是否有需求。

如果 Week 2 选择 Research 为主，我也希望得到建议：我应该产出什么样的小成果？比如生态研究、链上行为指标框架，还是结合一个小 demo 来验证问题。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->

Week 1 - AI 辅助开发 - Onchain Todo

\## 任务理解

这个任务不是单纯让 AI 写一段代码，而是练习一套开发流程：

1\. 我先写出清晰的 Prompt，告诉 AI 要生成什么合约。

2\. AI 给出一个 Solidity 合约初稿。

3\. 我阅读代码，理解每个部分的作用。

4\. 我人工检查代码是否符合预期、是否过度复杂、是否有明显风险。

5\. 我记录自己保留、修改或拒绝了 AI 输出中的哪些设计。

这次选择的方向是 `Onchain Todo`。它的含义是：把待办事项记录到链上。用户可以创建一条 todo，也可以把自己的 todo 标记为完成或未完成。

\## 我最初给 AI 的 Prompt

\`\`\`text

现在我们开始这个项目吧。你之前已经向我解释过这个任务的需求了。现在请你先帮助我清晰地理解这个任务。然后我们一起做一个onchain Todo的demo

\`\`\`

这个 Prompt 是自然语言式的，适合开启协作，但如果直接用来生成 Solidity 合约，还不够精确。它说明了我要做 `Onchain Todo`，也说明了我需要先理解任务，但没有明确合约的功能边界、权限要求、编译版本和安全限制。

\## AI 协助后改进的 Prompt

\`\`\`text

请帮我生成一个最小的 Solidity Onchain Todo 合约，适合 Web3 初学者阅读和检查。

要求：

1\. 使用 Solidity ^0.8.20。

2\. 用户可以创建一条 todo，内容是字符串。

3\. 每条 todo 需要记录创建者地址、内容、是否完成、创建时间、更新时间。

4\. 用户只能修改自己创建的 todo 的完成状态。

5\. 提供读取某条 todo 和读取 todo 总数的函数。

6\. 不要加入 owner 管理员、删除功能、付费逻辑、复杂权限系统或外部依赖。

7\. 代码命名要清晰，并加入必要的 event。

8\. 请用简短文字解释合约结构，方便我人工检查。

\`\`\`

\## Prompt 可以改进的地方

1\. 明确合约类型：说明要做 `Onchain Todo`，而不是泛泛地说“做一个 demo”。

2\. 明确 Solidity 版本：例如 `^0.8.20`，这样编译环境更清楚。

3\. 明确最小功能：创建 todo、修改完成状态、读取 todo、读取总数。

4\. 明确权限规则：用户只能修改自己创建的 todo。

5\. 明确不要什么：不要管理员、删除功能、付费逻辑、复杂权限、外部依赖。

6\. 明确安全边界：不要处理真实资产，不要记录隐私信息。

7\. 要求 AI 解释代码结构，方便我人工检查，而不是只输出代码。

\## AI 生成的主要输出

AI 生成了一个 `OnchainTodo` 合约，包含：

\- `Todo` 结构体：定义一条待办事项包含哪些信息。

\- `todos` 数组：保存所有链上 todo。

\- `createTodo` 函数：创建新的 todo。

\- `setCompleted` 函数：修改 todo 的完成状态。

\- `getTodo` 函数：读取指定 todo。

\- `getTodoCount` 函数：读取 todo 总数。

\- `TodoCreated` 和 `TodoCompleted` 事件：方便外部工具追踪链上行为。

合约源码见：

\- `contracts/OnchainTodo.sol`

\## 合约结构解释

\### `pragma solidity ^0.8.20`

指定 Solidity 编译器版本。可以理解为告诉编译器：“请用 0.8.20 或兼容版本来理解这份代码。”

\### `contract OnchainTodo`

定义一个智能合约。可以把它理解成部署到链上的一个小程序。部署后，它的规则会被链执行。

\### `struct Todo`

`struct` 是自定义数据结构。这里一条 todo 包含：

\- `owner`：创建这条 todo 的钱包地址。

\- `content`：待办事项内容。

\- `completed`：是否已经完成。

\- `createdAt`：创建时间。

\- `updatedAt`：最后更新时间。

\### `Todo[] private todos`

这是一个 todo 列表`private` 不是说链上数据真的保密，而是说不能自动生成公开读取函数。我们通过 `getTodo` 控制读取格式。链上数据本质上仍然是公开的。

\### `createTodo`

用户调用这个函数创建 todo。函数会检查内容不能为空，然后把 todo 加入列表。

\### `setCompleted`

用户调用这个函数更新完成状态。这里有一个权限检查：只有创建这条 todo 的地址可以修改它。

\### `getTodo` 和 `getTodoCount`

这两个函数是读取函数，不改变链上状态。读取函数通常不需要支付 gas，除非它被另一个写入交易内部调用。

\### `event`

事件是链上的日志。它不会改变合约核心状态，但方便区块浏览器或前端知道发生了什么。

\## 人工修改或判断说明

1\. 保留了 `owner` 字段，因为如果没有 owner，任何人都可以修改任何 todo，权限太松。

2\. 保留了 `require(bytes(content).length > 0)`，避免创建空内容 todo。

3\. 没有加入管理员 `owner`，因为这个 demo 不需要中心化管理员。

4\. 没有加入删除功能，因为删除会增加复杂度，也会让初学者更难检查状态变化。

5\. 没有加入付款逻辑，因为这个任务重点是理解合约结构，不是处理资产。

6\. 使用 `private todos + getTodo`，让读取接口更清晰。

7\. 加入 `event`，因为链上应用通常需要事件让前端或区块浏览器追踪行为。

\## 至少 3 条人工检查点

\### 1. 合约是否能编译

需要在 Remix 或本地 Solidity 编译器中选择 `0.8.20` 或兼容版本进行编译。当前代码没有使用外部依赖，理论上更容易编译通过。

\### 2. 函数是否符合预期

预期功能是：

\- 用户可以创建 todo。

\- 用户可以读取 todo。

\- 用户可以查看 todo 总数。

\- 用户只能修改自己创建的 todo。

当前合约符合这些预期。

\### 3. 是否存在明显权限问题

关键权限在 `setCompleted`：

\`\`\`solidity

require(todos\[todoId\].owner == msg.sender, "Only owner can update this todo");

\`\`\`

这表示只有创建者地址可以修改自己的 todo。其他地址不能修改。

\### 4. 是否使用了不必要的复杂逻辑

当前合约没有管理员、没有 token、没有 NFT、没有外部依赖、没有复杂角色系统。对于最小 demo 来说，复杂度合适。

\### 5. 是否有潜在安全或隐私风险

这份合约不处理真实资产，所以金融风险较低。但 todo 内容会上链，链上数据公开可查，不应该写入隐私信息、真实身份信息、私钥、助记词、API Key 或其他敏感内容。

\### 6. 变量和函数命名是否容易理解

`createTodosetCompletedgetTodogetTodoCount` 都能直接表达用途`ownercontentcompletedcreatedAtupdatedAt` 也比较容易理解。

\## 我的初步理解

这个 demo 让我看到：智能合约不是神秘的金融工具，它也可以只是一个记录状态的小程序`createTodo` 是一次链上状态改变`setCompleted` 也是一次状态改变。链上记录的不是“我的内心计划”，而是某个地址公开提交过的 todo 状态。

这个例子也提醒我：链上数据是公开的，所以 Onchain Todo 不适合记录私人生活事项，更适合作为学习 demo 或公开任务证明。

\## 可提交内容草稿

1\. Prompt：见“我最初给 AI 的 Prompt”和“AI 协助后改进的 Prompt”部分。

2\. AI 生成的主要输出`OnchainTodo` 合约，包括创建 todo、更新完成状态、读取 todo 和读取总数。

3\. 合约源码`contracts/OnchainTodo.sol`。

4\. 人工修改或判断说明：保留 owner 权限检查、空内容检查、事件；拒绝管理员、删除、付费等复杂逻辑。

5\. 人工检查点：编译、功能、权限、复杂度、安全/隐私、命名。

\## Remix 编译和测试结果

我已经在 Remix 中完成了编译、部署和基础功能测试。

\### 编译 / 部署

\- 工具：Remix 2.5.2

\- Environment：Remix VM

\- Contract`OnchainTodo`

\- Deploy：成功

\- Deployment value`0 wei`

\- Remix VM 部署交易 hash`0xd77...9411f`

说明：这个 hash 来自 Remix VM，只代表本地模拟链中的部署交易，不是 Monad Testnet 上的真实交易 hash。

\### 功能测试

已确认：

1\. `createTodo("Learn Solidity basics")` 可以创建 todo。

2\. `getTodoCount()` 返回了预期数量。

3\. `getTodo(0)` 可以读取第一条 todo。

4\. `setCompleted(0, true)` 可以把第一条 todo 标记为完成。

5\. 再次调用 `getTodo(0)` 时`completed` 已变成 `true`。

\### 测试后的判断

这说明合约的最小功能路径可以跑通：

\`\`\`text

部署合约 -> 创建 todo -> 读取 todo 数量 -> 读取 todo 内容 -> 修改完成状态 -> 再次读取确认

\`\`\`

这次测试只是在 Remix VM 中完成，适合验证语法和基础逻辑。它不是正式审计，也不代表合约可以直接用于生产环境。

\## Remix 使用步骤

Remix 是一个网页版 Solidity 开发环境。它适合初学者，因为不需要本地安装编译器、Node.js 或 Hardhat。

\### 1. 打开 Remix

在 Chrome 里打开：

\`\`\`text

[https://remix.ethereum.org/](https://remix.ethereum.org/)

\`\`\`

第一次打开可能会出现一些欢迎页面或插件提示，可以先关闭弹窗。

\### 2. 新建 Solidity 文件

在左侧文件区找到 `contracts` 文件夹。

操作步骤：

1\. 点击 `contracts` 文件夹旁边的新建文件按钮。

2\. 文件名输入：

\`\`\`text

OnchainTodo.sol

\`\`\`

3\. 把 `contracts/OnchainTodo.sol` 的源码放进去。

\### 3. 打开 Solidity Compiler

左侧栏有一个类似 Solidity 图标的按钮，通常叫 `Solidity compiler`。

进入后检查：

\- Compiler 版本选择 `0.8.20` 或兼容的 `0.8.x` 版本。

\- 如果看到 `Auto compile`，可以打开，也可以手动点击 compile。

然后点击：

\`\`\`text

Compile OnchainTodo.sol

\`\`\`

如果编译成功，说明合约语法上没有问题。这个结果可以记录到人工检查点里。

\### 4. 打开 Deploy & Run Transactions

左侧栏选择 `Deploy & Run Transactions`。

这里是在 Remix 里测试合约如何部署和调用。

初学阶段建议先用：

\`\`\`text

Environment: Remix VM

\`\`\`

`Remix VM` 是浏览器里的模拟链，不会连接真实钱包，也不会消耗真实 gas 或测试网 MON。它适合先确认合约功能。

\### 5. 部署合约

在 `Contract` 下拉框中选择：

\`\`\`text

OnchainTodo

\`\`\`

然后点击：

\`\`\`text

Deploy

\`\`\`

部署成功后，页面下方会出现一个 `Deployed Contracts` 区域，里面有刚部署的 `OnchainTodo`。

\### 6. 测试 `createTodo`

展开部署好的合约，找到 `createTodo`。

输入一个公开、无隐私的测试内容，例如：

\`\`\`text

Learn Solidity basics

\`\`\`

点击 `transact`。

这一步会改变合约状态，所以在真实链上需要 gas。在 Remix VM 里只是模拟。

\### 7. 测试 `getTodoCount`

点击 `getTodoCount`。

如果刚刚成功创建了一条 todo，应该返回：

\`\`\`text

0: uint256: 1

\`\`\`

意思是当前合约里一共有 1 条 todo。

\### 8. 测试 `getTodo`

找到 `getTodo`，输入：

\`\`\`text

0

\`\`\`

因为第一条 todo 的编号是 `0`，不是 `1`。

点击后应该能看到：

\- owner：创建 todo 的地址。

\- content：todo 内容。

\- completed：是否完成，初始应该是 `false`。

\- createdAt：创建时间戳。

\- updatedAt：更新时间戳。

\### 9. 测试 `setCompleted`

找到 `setCompleted`。

输入：

\`\`\`text

todoId: 0

completed: true

\`\`\`

点击 `transact`。

然后再次调用 `getTodo(0)`，如果 `completed` 变成 `true`，说明更新成功。

\### 10. 人工检查时重点看什么

1\. 编译是否成功。

2\. `createTodo` 是否能创建 todo。

3\. `getTodoCount` 是否能返回正确数量。

4\. `getTodo(0)` 是否能读取第一条 todo。

5\. `setCompleted(0, true)` 是否能修改完成状态。

6\. todo 内容是否公开，因此不能写隐私内容。

7\. 只有创建者可以修改 todo；这个权限逻辑在 Remix VM 里可以切换不同 Account 测试。

\### 11. 如果要连接 Monad Testnet

初学阶段建议先用 `Remix VM`。确认合约能正常编译和运行后，再考虑连接 MetaMask 和 Monad Testnet。

如果连接测试网，需要：

1\. MetaMask 当前网络切换到 `Monad Testnet`。

2\. Remix 的 Environment 选择 `Injected Provider - MetaMask`。

3\. MetaMask 弹窗确认连接。

4\. 部署合约时会消耗测试网 MON。

注意：不要用主力钱包，不要提交私钥、助记词、API Key 或 `.env` 文件。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->



**分享笔记：一个新手如何理解 Web3**

**1\. 我的起点**

我的背景是社科phd，做测量和方法。

在开始学习之前，我对 Web3 的理解很窄：

> Web3 = 炒币 / 投机 / 金融交易 / 很技术 / 离普通生活很远

但这几天做完基础任务后，我的理解发生了一些变化。

我现在会先把 Web3 理解成：

> 一套公开、可验证、由用户自己控制入口的数字状态系统。

也就是说，Web3 不只是关于“钱”，也关于“哪些数字状态可以被公开记录、验证和执行”。

**2\. 我做了什么**

这几天我主要完成了几件事：

-   学习了 Web2 / Web3、区块链、钱包、交易、gas、区块浏览器、智能合约等基础概念。
    
-   创建了课程专用 MetaMask 钱包，并添加 Monad Testnet。
    
-   完成了第一笔 Monad Testnet 测试网转账。
    
-   在区块浏览器里查看了 transaction hash、from、to、value、gas、status。
    
-   用 AI 辅助生成了一个最小 Solidity demo：Onchain Todo。
    

这些任务让我把很多抽象概念连起来：

> 链记录状态，钱包控制身份，交易改变状态，gas 支付改变状态的成本。

**3\. 我最大的概念转变**

我原来以为“交易”就是买卖。

但在链上，transaction 更像是：

> 一个地址签名后，向某条链提交一次状态改变请求。

所以：

-   转账是交易，因为余额状态改变了。
    
-   部署合约是交易，因为链上新增了一个合约。
    
-   调用合约函数是交易，因为合约状态可能改变了。
    
-   创建 onchain todo 也是交易，因为 todo 列表改变了。
    

这让我意识到：

> Web3 的核心不只是交易资产，而是通过交易改变公共状态。

**4\. Web3 和 Web2 / 现实生活的不同**

在 Web2 和现实生活里，我们习惯相信中心化机构或平台：

-   银行记录余额。
    
-   学校认证身份。
    
-   平台保存账号和数据。
    
-   App 告诉我们某个操作是否成功。
    

Web3 试图改变一部分信任结构：

> Web2 更像是相信平台；Web3 更像是相信规则、签名、代码、共识和公开记录。

但这不是简单地“更方便”。  
相反，它常常意味着：

-   用户要自己保管助记词。
    
-   用户要知道自己在哪条链上操作。
    
-   用户要理解签名和交易风险。
    
-   用户要承担更多责任。
    

所以我现在觉得：

> Web3 是用更高的个人责任，换取更强的控制权和可验证性。

**5\. 我问过的几个问题**

这几天我问过几个对我很关键的问题：

-   ETH 和 Ethereum 是什么关系？
    
-   为什么会有很多不同的链？
    
-   为什么 Web3 里的交易这么重要？
    
-   哪些状态需要上链，哪些应该留在链下？
    
-   链上记录是真的，但它到底代表什么？
    

其中最重要的是：

> 可验证不等于自动有意义。链上记录是真实的，但记录的意义仍然需要解释。

**6\. 我的测量背景带来的疑问**

因为我的背景和心理测量、研究方法有关，所以我会自然关心：

> 链上记录到底测量或代表了什么？

比如：

-   一个地址交易很多次，代表真实参与，还是只是刷交互？
    
-   一个地址持有 badge，代表真的学习过，还是只是完成任务？
    
-   一个地址参与投票，代表治理贡献，还是只是为了奖励？
    
-   一个项目链上活跃度很高，代表生态健康，还是代表激励被套利？
    

但我也不想把传统心理测量直接套到 Web3 上。因为 Web3 的主体不一定是现实生活中的“人”，而可能是地址、钱包、合约，或者多个地址之间的关系。

**7\. 阶段性总结**

我现在感觉，Web3 当下的很多实践主要受金融和计算机科学两套heuristic method影响。金融启发式让它关心资产、激励、成本、收益和风险；计算机科学/密码学启发式让它关心规则、协议、签名、验证和自动执行。

这两套思路让 Web3 很擅长处理“开放系统中如何协调不互相信任的人”这个问题。但它们也有边界：它们能很好地记录和激励行为，却不一定自动解释这些行为的意义。

所以我的疑问是：当 Web3 用金融激励和代码规则把行为记录下来之后，我们是否还需要另一层问题意识：这些被记录和激励的行为，到底代表什么？
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->






## What I Did Today

Today I continued Monad Builder Camp Week 1 and moved from concept learning into real onchain practice.

I learned and discussed:

-   why there are many different blockchains
    
-   how to understand chains as different public worlds or games
    
-   why Web3 does not simply put everything into one unified world
    
-   how Web3 relates to Web2
    
-   what blockchain state means at a beginner level
    
-   why a transaction changes the state of a specific chain
    

I also completed hands-on work:

-   created a course-only MetaMask wallet
    
-   added Monad Testnet
    
-   confirmed MetaMask was on Monad Testnet
    
-   got testnet MON
    
-   completed my first Monad Testnet transfer
    
-   opened the transaction in MonadVision
    
-   recorded the transaction hash and key fields
    

## Proof

Course wallet:

```
0x2596976c6D2D5a301eFc3833e2749bDF368223e5
```

First Monad Testnet transaction:

```
0xfcc719809d3984548a88305e8753d47539eb251fd800136a8c6bf24eeea441bd
```

Explorer link:

```
https://testnet.monadvision.com/tx/0xfcc719809d3984548a88305e8753d47539eb251fd800136a8c6bf24eeea441bd
```

Transaction summary:

-   Status: Success
    
-   From: `0x2596976c6D2D5a301eFc3833e2749bDF368223e5`
    
-   To: `0xb11256348b1cba14E77Ca541A9b4a7E2Da2ecC49`
    
-   Value: `0.01 MON`
    
-   Transaction fee: `0.002162856067344 MON`
    
-   Gas used: `21,000`
    

## Problems I Had

### 1\. I did not understand why there are many chains

At first, it felt strange that Web3 has many separate chains. If everything were stored together, it seemed like it would be more convenient.

Resolution:

I started using the "different public game worlds" analogy. Different chains have different rules, costs, communities, speeds, and opportunities. People use a chain when the thing they want to do lives in that world.

### 2\. I did not understand blockchain state

The word "state" felt abstract because I do not have a finance or computer science background.

Resolution:

I understood state as "the current situation of a world." In a game, this means level, inventory, gold, and completed quests. Onchain, this means balances, ownership, votes, contract data, and transaction results.

### 3\. The website said "No wallet detected"

When trying to connect from the Codex in-app browser, the page said:

```
No wallet detected. Please install a compatible wallet to continue
```

Resolution:

The Codex in-app browser does not have the MetaMask extension. Wallet actions need to happen in Chrome or another browser where MetaMask is installed.

### 4\. I was confused by network vs token

I saw MON in the wallet and wondered why there was no "Monad Testnet" in the receiver field.

Resolution:

Monad Testnet is the network. MON is the token used on that network. The receiver field is only an address.

Mental model:

```
current network = the world I am acting in
MON = the currency in that world
receiver address = the account I send to
```

## What I Learned

The most important sentence from today:

```
链记录状态，钱包控制身份，交易改变状态，gas 支付改变状态的成本。
```

I also learned:

-   public addresses can be shared, but private keys and seed phrases must never be shared
    
-   a wallet does not store assets; the chain records balances, and the wallet controls keys
    
-   a transaction can be inspected through a block explorer
    
-   failed transactions can still consume gas because the network may spend computation before failure
    
-   Web3 is not just "trading"; it is about verifiable state and user-controlled actions
    

## How I Felt

At the beginning, the concepts felt scattered and abstract. I was especially confused by why Web3 needs many chains and why transactions are so central.

The "different games / public worlds" analogy made things easier. After completing the first transaction and seeing it in MonadVision, the idea became more concrete: the transaction was not just a button click in a website. It was a signed action that changed state on Monad Testnet and left a public record.

I still feel that Web3 has many layers and the user experience is not very intuitive yet. But I now have a clearer first mental map, and I completed a real onchain action safely with a course-only wallet.

## Next

-   Practice reading more transaction details in the explorer.
    
-   Learn what `from`, `to`, `value`, `gas price`, and `gas fee` mean in more depth.
    
-   Continue Week 1 tasks toward AI + Solidity + contract deployment.
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->








学习ai agent支付，先打卡
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->









和ai讨论心理测量+web3，得到结论为理念很好，但是实践层面上可能没有现存的启发式测量有削。  
今日目标：开始实践
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->











### **Web3实习计划开营式（录播）：**

比特币本质上是一种区块链，强共识但是低性能。Monad在不降低性能的情况下，提高了区块链的上限。Monad同时进行“共识”和“执行交易”。

### **实习手册：**

进行入门导读的时候，我有几个问题：  
1\. 为什么需要有区块链？  
2\. 为什么需要更快的交易速度？  
3\. 如何在保证区块链上的信息是公开透明的同时，又是匿名的？

沿着这几个问题，我初步理解区块链的核心动机，是在没有中心化信任机构的情况下，通过分布式共识与密码学机制维护一个不可篡改的账本，从而降低对银行、平台等中介的依赖，并支持数字价值的唯一性与可验证转移。为了成为真正的基础设施，它还必须提升交易效率，否则难以进入现实支付与金融场景。

在机制层面，区块链通过地址与公私钥体系实现伪匿名，使交易公开透明但不直接绑定现实身份，从而在可验证性与隐私之间做出折中。同时，它的“可信性”更多来自**_分布式结构_**与**_经济激励假设_**，而非绝对安全，本质是降低单点操控风险，而不是消除操控可能。

但我也仍然存在疑问：这种“信任代码规则”的结构，是否真的比信任人更可靠？它在实践中依然可能被算力、资本或开发者影响，从而形成新的权力集中。此外，所谓规则不可随意更改，也引出了系统如何演化的问题：当进化依赖社区共识时，它是否只是把政治过程技术化，而并未真正消除权力结构？从这个角度看，区块链更像是一种权力的重新分布，而不是权力的消失，这种“技术中心主义”的可信性仍值得进一步质疑。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
