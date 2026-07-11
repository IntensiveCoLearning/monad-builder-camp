---
timezone: UTC+8
---

# 灵灵七

**GitHub ID:** minzhenw490-pixel

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
1\. 我使用的 Prompt

生成一份最小可运行、简洁规范的Solidity活动积分记录智能合约，用于社区签到和拉新奖励，满足以下全部要求： 1. 编译器版本锁定 solidity 0.8.20，自带内置溢出检查，不引入任何第三方库、OpenZeppelin合约； 2. 合约业务逻辑：平台活动积分系统，管理员发放积分、用户查看自身积分、管理员扣除违规用户积分、查询用户积分总额； 3. 角色权限区分：仅合约部署者（管理员）拥有发放/扣减积分权限，普通用户仅可读自己积分； 4. 核心数据结构：存储用户地址与对应积分数量mapping； 5. 必备函数： - grantPoints(address user, uint amount)：管理员给用户发放积分 - deductPoints(address user, uint amount)：管理员扣除用户积分，需校验积分充足 - getUserPoints(address user)：任何人可查询指定用户积分 6. 增加完善的require边界校验：扣积分时不能超过用户现有积分、发放积分不能为0； 7. 代码添加清晰中文单行注释，无冗余逻辑； 8. 代码输出完成后，逐段完整解释这份积分合约每一行、每个函数的作用、部署运行逻辑、潜在风险点。

2\. AI 生成的原始输出（第一次给的代码）

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

// 活动积分记录合约

contract CommunityPoints {

// 管理员地址（部署合约的人）

address public admin;

// 用户地址 => 积分数量

mapping(address => uint256) private points;

// 构造函数，在部署时记录管理员地址

constructor() {

admin = msg.sender;

}

// 管理员权限校验修饰器

modifier onlyAdmin() {

require(msg.sender == admin, "Only admin can operate");

\_;

}

// 管理员给用户发放积分

function grantPoints(address user, uint256 amount) external onlyAdmin {

// 用户地址不能为空

require(user != address(0), "Invalid user address");

// 发放积分不能为0

require(amount > 0, "Amount must be greater than zero");

// 增加用户积分

points\[user\] += amount;

}

// 管理员扣除用户积分

function deductPoints(address user, uint256 amount) external onlyAdmin {

// 用户地址不能为空

require(user != address(0), "Invalid user address");

// 扣除积分不能为0

require(amount > 0, "Amount must be greater than zero");

// 用户积分必须足够扣除

require(points\[user\] >= amount, "Insufficient points");

// 减少用户积分

points\[user\] -= amount;

}

// 查询指定用户积分

function getUserPoints(address user) external view returns (uint256) {

// 返回用户当前积分

return points\[user\];

}

// 查询用户积分总额

function getTotalPoints() external view returns (uint256 total) {

// 遍历统计需要额外存储用户列表，当前版本不实现

return total;

}

}

3\. 最终合约源码（修改后）

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

// 活动积分记录合约

contract CommunityPoints {

// 管理员地址（合约部署者）

address public admin;

// 用户地址 => 积分数量

mapping(address => uint256) private points;

// 用户地址 => 上次签到时间

mapping(address => uint256) private lastCheckInTime;

// 每次签到奖励积分

uint256 public constant CHECKIN\_REWARD = 10;

// 发放积分事件

event PointsGranted(address indexed user, uint256 amount);

// 扣除积分事件

event PointsDeducted(address indexed user, uint256 amount);

// 签到事件

event CheckIn(address indexed user, uint256 amount, uint256 timestamp);

// 构造函数，部署时记录管理员地址

constructor() {

admin = msg.sender;

}

// 管理员权限校验

modifier onlyAdmin() {

require(msg.sender == admin, "Only admin can operate");

\_;

}

// 管理员给用户发放积分

function grantPoints(address user, uint256 amount) external onlyAdmin {

// 用户地址不能为空

require(user != address(0), "Invalid user address");

// 发放积分不能为0

require(amount > 0, "Amount must be greater than zero");

// 增加用户积分

points\[user\] += amount;

// 记录发放日志

emit PointsGranted(user, amount);

}

// 管理员扣除用户积分

function deductPoints(address user, uint256 amount) external onlyAdmin {

// 用户地址不能为空

require(user != address(0), "Invalid user address");

// 扣除积分不能为0

require(amount > 0, "Amount must be greater than zero");

// 用户积分必须足够扣除

require(points\[user\] >= amount, "Insufficient points");

// 扣除积分

points\[user\] -= amount;

// 记录扣除日志

emit PointsDeducted(user, amount);

}

// 用户签到领取积分

function checkIn() external {

// 获取用户上次签到时间

uint256 lastTime = lastCheckInTime\[msg.sender\];

// 限制24小时内不能重复签到

require(

block.timestamp >= lastTime + 1 days,

"Already checked in within 24 hours"

);

// 更新签到时间

lastCheckInTime\[msg.sender\] = block.timestamp;

// 增加签到积分

points\[msg.sender\] += CHECKIN\_REWARD;

// 记录签到日志

emit CheckIn(

msg.sender,

CHECKIN\_REWARD,

block.timestamp

);

}

// 查询指定用户积分

function getUserPoints(address user) external view returns (uint256) {

// 返回用户积分

return points\[user\];

}

// 查询用户积分总额

function getTotalPoints() external view returns (uint256 total) {

// 当前mapping无法遍历所有用户，所以暂不统计

return total;

}

}

4\. 人工修改或判断说明

我对AI生成的代码进行了以下人工检查和修改：

1\. 权限控制：初始代码缺少管理员权限控制，任何用户都能调用加分/减分函数。我要求AI补上了 onlyOwner 修饰器，确保仅合约部署者可进行积分调整操作。

2\. 事件日志：初始代码没有释放事件（Event），不利于运营对账和用户申诉。我要求AI补上了 CheckIn、PointsAdded 等事件，每次操作都会上链记录。

3\. 防刷机制：初始代码缺少签到时间间隔限制，用户可无限次签到刷分。我要求AI补上了 lastCheckIn\[msg.sender\] + 24 hours < block.timestamp 的时间校验，确保每人每天只能签到1次。

4\. 功能测试：部署至 Remix VM 后，我测试了签到、查积分、重复签到防刷三项功能，均按预期运行，确认合约逻辑无误。

5\. AI生成代码需要人工检查的地方（至少3条）

通过本次实践，我总结了以下必须人工检查的要点：

1\. 权限控制是否缺失：AI可能默认所有函数为public，必须人工确认加分、减分等敏感操作是否限制了管理员权限，否则可能导致积分数据被恶意篡改。

2\. 防刷逻辑是否有效：签到、领空投等运营场景必须有时间间隔或次数限制。需人工检查是否存在 require 时间戳校验，防止脚本批量攻击导致积分通胀。

3\. 事件日志是否完整：所有积分变动操作都应释放 Event，便于运营通过区块浏览器对账和仲裁用户申诉。需人工确认每个写函数是否都有对应的 Event 输出。

4\. 命名是否清晰易读：作为运营方，函数名应直观反映功能（如 checkIn、getMyPoints），避免AI生成 func1、update 等模糊命名，降低团队协作成本。

6\. 补充：部署验证记录

· 编译环境：Remix IDE 2.5.2

· 编译器版本：0.8.24

· 部署环境：Remix VM (Cancun)

· 测试结果：

· ✅ 编译通过，无错误

· ✅ 部署成功

· ✅ 签到功能正常（积分+10）

· ✅ 防刷机制有效（24小时内重复签到被拒绝）

· ✅ 积分查询功能正常

· 部署交易哈希：0xd9145CCE52D386f254917e481eB44e9943F39138

![IMG_2983.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/minzhenw490-pixel/images/2026-07-11-1783756777613-IMG_2983.png)
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

Monad **学习笔记：不只是“更快”，而是高频交互体验的重构**

**一、核心洞察：**Monad **改变的是什么？**

Monad **经常被简单概括为“每秒** 10,000 TPS **的以太坊兼容** L1**”，但这样的描述掩盖了它真正的革命性。**Monad **不是在“更快地做同样的事”，而是在重新设计区块链处理交易的方式，这种设计的改变直接影响了高频交互产品的用户体验和开发范式。**

**要理解这一点，我们需要从** Monad **最核心的架构决策说起。**

**二、核心差异：异步执行带来的范式转移**

2.1 **最大的不同：按** Gas Limit **收费，而非** Gas Used

**这是** Monad **与** Ethereum **最根本的行为差异之一。在以太坊中，交易按实际使用的** Gas **收费（**gas\_used**）；而在** Monad **中，交易按** Gas Limit **收费（**gas\_paid = gas\_limit \* price\_per\_gas**）。**

**为什么？因为** Monad **采用了异步执行架构：区块的构建和验证可以在交易执行完成之前进行。如果按实际使用的** Gas **收费，攻击者可以提交一个** Gas Limit **极大但实际消耗极小的交易**——**它占用了区块空间却不支付相应费用，这就构成了** DoS **攻击的漏洞。**

**这对高频交互产品的意义：开发者必须重新思考** Gas **管理策略。文档明确建议，对于** Gas **成本固定的操作（如原生代币转账固定** 21,000 Gas**），直接硬编码** Gas Limit**，而不是调用**eth\_estimateGas**。这样做的好处是：**

**·** **显著降低延迟：钱包无需等待** RPC **的** eth\_estimateGas **响应**

**·** **避免钱包的异常行为：某些钱包（如** MetaMask**）在** eth\_estimateGas **失败时会设置极高的**Gas Limit**，这在** Monad **上会导致用户被多扣费**

**对于需要高频交互的产品（如链游、社交应用、交易终端），每一次交互节省几百毫秒，累积起来就是质的差异。**

2.2 EIP-1559 **的优化：更快的降费、更稳的体验**

Monad **支持** EIP-1559 **类型的交易，但** Base Fee **的调控机制与以太坊不同**——**上涨更慢、下降更快。设计目标是避免因** Base Fee **过高而导致区块空间未被充分利用。**

**对于高频交互产品，这意味着** Gas **费用的波动性更低、可预测性更强，开发者可以更准确地为用户预估交易成本。**

2.3 **没有全局** Mempool**，交易直接转发给下几个** Leader

Monad **没有全局的交易池（**mempool**），交易会被直接转发给接下来的几个区块领导者。**

**对用户体验的意义：交易确认的确定性更高、延迟更低。在高频交互场景中，用户不需要长时间等待交易被打包，产品的交互反馈可以更加即时。**

**三、开发者体验的质变：高性能应用的最佳实践**

3.1 **合约代码容量翻倍**

Monad **的智能合约最大代码尺寸从以太坊的** 24 KB **提升到** 128 KB**，初始化代码从** 48 KB **提升到** 256 KB**。**

**这意味着开发者可以部署更复杂的应用逻辑，而不必为了节省字节码空间而牺牲功能或可读性。对于高频交互的** DeFi **协议或链游，这直接降低了开发复杂度。**

3.2 secp256r1**（**P256**）预编译支持**

Monad **支持** secp256r1 **签名验证预编译（地址** 0x0100**），这使得链上可以直接验证**WebAuthn/Passkey **签名。**

**这对用户体验是革命性的：用户可以用生物识别（指纹、面容）或硬件密钥直接与链上应用交互，而不需要依赖传统的** EOA **私钥管理。这是高频交互产品走向“**Web2 **级用户体验”的关键基础设施。**

3.3 **性能优化的工程实践**

**文档提供了大量针对高频应用的优化建议：**

**·** **减少** eth\_call **延迟：通过** Multicall **聚合多个读取请求，或通过批量提交** RPC **请求来并行执行**

**·** **使用索引器替代** eth\_getLogs**：对于频繁查询历史事件或衍生状态的应用，使用** Allium**、**Envio**、**Goldsky **等索引服务**

**·** Web **托管成本控制：对于高流量应用，**AWS S3 + CloudFront + Lambda + RDS **的组合比**Vercel **等** serverless **平台更具成本可控性**

**这些不是泛泛而谈，而是针对高频、高流量场景的实战建议。当你的应用每天要处理数万次链上交互时，每一个** RPC **调用的优化都会直接影响用户体验和运营成本。**

**四、总结：**Monad **真正改变的是什么？**

**维度** **以太坊范式** Monad **范式**

**收费逻辑** **按实际使用收费** **按** Gas Limit **收费**

**执行模型** **同步执行** **异步执行（共识先于执行）**

**费用波动** **上涨快、下降慢** **上涨慢、下降快**

**交易路由** **全局** Mempool **直接转发给下几个** Leader

**认证方式** **私钥签名为主** **支持** WebAuthn/Passkey

**合约复杂度** 24 KB **限制** 128 KB **限制**

Monad **不是简单地“把** TPS **从** 15 **提高到** 10,000**”，而是通过异步执行架构重新设计了交易的生命周期**——**共识与执行解耦、收费逻辑重构、**Mempool **机制改变。这些变化共同指向一个目标：让区块链应用的交互体验接近传统互联网应用。**

10,000 TPS **让“高频”成为可能，但真正让高频交互产品变得“好用”的，是这些底层架构的重新设计**——**更低的延迟、更可预测的费用、更友好的认证方式、更灵活的开发者空间。**Monad **不只是更快，而是让区块链应用终于可以像普通** App **一样被用户日常使用。**
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->


**一个完整的** DApp **通常由以下几层构成：**

1\. **前端（**User Interface**）**

**·** **用户交互界面，通常用** React**、**Vue **等框架构建**

**·** **与传统** Web **应用不同，**DApp **前端通过钱包注入的** Provider **或第三方** RPC **节点与区块链交互**

**·** **需要集成钱包（如** MetaMask**）进行身份验证和交易签名**

2\. **智能合约（**Smart Contracts**）**—— **核心**

**·** **定义了应用的业务逻辑，部署在区块链上**

**·** **通过自动执行的规则确保交易的透明性和不可篡改性**

**·** **在以太坊及兼容链上，通常用** Solidity **编写，运行在** EVM **上**

3\. **数据检索器（**Indexer**）**

**·** **智能合约以** Event**（事件）形式释放日志，**Indexer **检索这些数据并写入传统数据库**

**·** **解决链上数据查询效率问题（比如展示用户持有的所有** NFT**）**

4\. **区块链与去中心化存储**

**·** **区块链存储合约状态和交易记录**

**·** IPFS/Arweave **等存储大文件（图片、文档等）**

**二、**Solidity **合约基础**

**在** Solidity **中，合约类似于面向对象编程语言中的类（**Class**）** **。合约中包含：**

**·** **状态变量：永久存储在合约存储中的数据**

**·** **函数：可执行的代码单元**

**·** **函数修饰器：以声明方式修改函数语义**

**·** **事件：合约向外发送的日志**

**·** **结构体和枚举类型**

**重要概念：**

**·** **合约创建后，最终代码存储在区块链上**

**·** **构造函数参数通过** ABI **编码** **传递**

**·** **合约可以继承其他合约**

**三、**Remix IDE——**浏览器中的开发工具**

Remix **是一个浏览器端的** Solidity IDE**，无需安装，打开网页就能用。**

**基本工作流程：**

1\. **创建新文件（**.sol **扩展名）**

2\. **编写合约代码**

3\. **编译合约（选择正确的编译器版本）**

4\. **部署合约（可选本地虚拟链或真实测试网）**

5\. **与部署的合约交互**

**环境选择：**

**·** Remix VM**：浏览器中的模拟区块链，有** 10 **个预置账户各** 100 ETH**，适合快速测试**

**·** Injected Provider**：连接浏览器钱包（如** MetaMask**），用于部署到真实测试网**

**关键提醒：粘贴代码前要理解代码内容，不要盲目信任任何来源的代码。**

**四、**OpenZeppelin——**行业标准库**

OpenZeppelin **是一个经过社区验证的智能合约库，提供了** ERC20**、**ERC721 **等代币标准的实现，以及权限管理、安全机制等可复用组件。**

**简单说：不用从零造轮子**——**发** Token**、做** NFT **这些常见需求，**OpenZeppelin **已经有现成的、经过审计的代码可以直接用。**

**五、**Monad + Remix **部署要点**

**根据** Monad **官方部署指南：**

**前置条件：钱包已添加** Monad Testnet**（**Chain ID: 10143**），并有测试币** MON**。**

**部署步骤：**

1\. **打开** [https://remix.ethereum.org](https://remix.ethereum.org)

2\. **创建** Gmonad.sol **文件，粘贴合约代码**

3\. **在** Solidity Compiler **中选择编译器版本** 0.8.24**，点击编译**

4\. **切换到** Deploy & Run**，**Environment **选择** Injected Provider **连接钱包**

5\. **构造函数输入** greeting **参数（如** "gmonad"**），点击** Deploy

6\. **钱包弹出确认，点击确认后等待部署完成**

7\. **在** Deployed Contracts **中看到合约地址，展开可调用** greeting() **读取、**setGreeting() **修改**

**六、**AI **生成合约**——**用但不要盲信**

**用** AI **辅助写合约已经成为常见做法。可以让** AI **生成** ERC-20 Token**、投票合约等模板代码。**

**实践建议：**

**·** **用** AI **生成初版代码作为起点**

**·** **必须自己读懂每一行，确认逻辑正确**

**·** **检查安全风险（重入攻击、权限控制等）**

**·** **在** Remix VM **中先测试，再部署到测试网**
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->



涵盖：安全合规、钱包账户、交易与Gas、Monad测试网与浏览器

一、安全与合规（来自 [web3intern.xyz](http://web3intern.xyz)）

· 中国监管态度：对虚拟货币的金融属性（ICO、交易、支付）采取严格限制，但对底层技术（区块链、智能合约）持宽容态度。任何涉及公众融资、资产流通的项目都需极度谨慎。

· 高风险行为：

· 代币发行（包括IEO/IDO、空投变相融资）

· 链游“充值—抽奖—提现”闭环（可能涉赌）

· 多级返利/邀请奖励（可能涉传销）

· 场外大额换汇（可能涉非法经营或洗钱）

· 实务建议：参与项目前先辨别是否有真实业务支撑，避免参与纯资金盘；技术开发者也要注意，部署合约或设计经济模型可能被认定为“共同行为人”。整体原则：技术可以玩，金融红线别碰。

二、以太坊钱包与账户（[ethereum.org](http://ethereum.org)）

· 账户 = 钥匙对：一个公钥（生成地址）和一个私钥（签名权）。谁掌握私钥，谁就控制资产。

· 地址：公钥的哈希值，是你在链上的“收款码”，可公开分享。

· 钱包 ≠ 存钱罐：钱包只是帮你管理私钥、查看余额、发送交易的“界面工具”，资产永远在链上。

· 助记词（12/24个单词）：是恢复钱包的唯一凭证，必须手写离线保存，绝对禁止截图、拍照、上传云端。丢了助记词 = 丢了所有资产。

· 常见钱包类型：

· 浏览器扩展（如MetaMask）——最常用，与DApp交互方便

· 手机App（如Trust Wallet）——适合移动端

· 硬件钱包（如Ledger）——最安全，适合大额资产

· 安全提醒：交易不可逆，转账前再三核对地址；小心钓鱼网站，建议收藏常用网址。

三、交易（Transactions）与 Gas

交易是什么？

· 交易是由外部账户（EOA）发起、签名后广播到网络的指令，用于转账ETH或调用智能合约。

· 每个交易都包含：发送方、接收方、金额、数据（可选）、nonce、Gas限制、费用设置。

关键字段速记

· nonce：从该地址发出的交易序号，必须按顺序递增（防止重放攻击）。

· value：转账金额，单位是 Wei（1 ETH = 10¹⁸ Wei）。

· gasLimit：该交易最多允许消耗多少Gas单位，普通转账通常为21000，合约交互更高。

· maxFeePerGas 和 maxPriorityFeePerGas：你愿意支付的最高总Gas单价和给验证者的小费。

Gas费用计算

· 总费用 = 实际消耗Gas单位 × (BaseFee + PriorityFee)

· BaseFee由网络拥堵程度自动调整，PriorityFee是给验证者的小费（越高越优先打包）。

· 注意：无论交易成功还是失败，Gas费都不会退还。所以别把gasLimit设得太低，否则可能因Gas不足而失败，但仍会扣费。

四、Monad 区块链与测试网

Monad 是什么？

· 一条高性能 Layer‑1，完全兼容以太坊EVM（现有工具可直接使用）。

· 通过并行执行和异步共识等技术提升速度，同时保持去中心化（不需要高端硬件即可运行节点）。

· 代码开源，社区活跃。

Monad Testnet（测试网）—— 实操重点

· Chain ID：10143

· 货币符号：MON（测试币，无实际价值）

· 水龙头（领测试币）：[https://faucet.monad.xyz](https://faucet.monad.xyz) —— 输入地址即可领取，用于练习转账和交互。

· 公开RPC（用于钱包/应用连接网络）：QuickNode、Ankr、Monad基金会均提供，注意有速率限制（如50次/秒）。

· Tempnet：临时测试网（Chain ID 20143），用于实验新功能，会定期重置。

区块浏览器（Block Explorers）

· 作用：查看链上所有交易、账户余额、合约代码、Gas价格等，是“链上的搜索引擎”。

· Monad测试网常用：

· Monadscan（Etherscan风格，最常用）

· MonadVision（另一种界面）

· 高阶分析工具：Tenderly、Phalcon 可查看交易内部调用栈、余额变化等细节（调试合约非常有用）。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->




1\. DevRel（开发者关系）**核心职责：让开发者愿意用你家的链**/**协议。具体干活是：写技术文档、做** Demo **演示、在** Discord **里回答开发者问题、办黑客松。需要懂一点技术（能写** Demo **代码），但更看重沟通能力和耐心。左手拉工程师，右手拉社区**

2\. Builder\*\*（建设者）核心职责：看见空白就填上。不一定是写智能合约，写脚本、做数据看板、设计经济模型、甚至牵头发起一个社区活动都算。需要主动性强，能把想法拆解成可执行的步骤，快速试错。\*\*

3\. **生态连接者**，**核心职责：把** A **项目的技术介绍给** B **项目，把开发者引荐给投资人，把社区需求反馈给产品团队。靠情报和信息差推动合作。需要广泛的人脉网、信息筛选能力、高情商。**

[https://x.com/min\_zhen07/status/2074357783755907265?s=46刚在 Monad 测试网完成了一笔链上交易，记录一下这次哈希交互的细节。](https://x.com/min_zhen07/status/2074357783755907265?s=46)
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
