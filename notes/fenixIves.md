- GitHub ID: 252904734
- Name: fenixIves
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

# 智能合约开发流程

智能合约开发是一个**从需求定义到上线维护的闭环流程**，核心遵循「**设计→开发→测试→部署→交互**」的步骤，且每个环节都需要严格把控安全性（因为合约部署后无法修改）。以下是详细的、可落地的具体流程：

### **一、需求与架构设计阶段（核心：明确目标，规避风险）**

这是开发的起点，直接决定后续合约的功能和安全性，不能跳过。

1.  **明确业务需求**
    

-   确定合约的核心功能：是NFT铸造、DeFi借贷、DAO投票，还是简单的转账/数据存储？
    
-   定义核心规则：比如NFT的铸造数量、价格、权限；DeFi的利率计算、抵押率、清算规则。
    
-   示例：需求是「开发一个限量1000枚的NFT合约，仅合约拥有者可开启铸造，用户支付0.01 ETH铸造1枚」。
    

2.  **选择目标区块链与技术栈**
    

-   **区块链选择**（优先兼容EVM的链，新手友好）：
    

-   以太坊（Ethereum）：生态最成熟，适合高安全性需求的合约（如DeFi）。
    
-   BSC/Polygon：手续费低、速度快，适合面向大众用户的应用型合约（如NFT、小游戏）。
    

-   **技术栈确定**：
    

-   开发语言：**Solidity**（主流，优先学习）。
    
-   开发框架：**Hardhat/Truffle**（二选一，推荐Hardhat，文档清晰、生态活跃）。
    
-   辅助工具：Remix IDE（在线编写，快速验证小合约）、OpenZeppelin（开源安全合约库，避免重复造轮子）。
    

3.  **架构设计与风险评估**
    

-   拆分合约模块：复杂需求建议分多个合约（如主合约+权限管理合约+数据存储合约），降低单个合约的复杂度。
    
-   预判风险点：比如权限控制（谁能调用核心函数）、溢出问题（Solidity 0.8.x已内置检查，低版本需手动处理）、重入攻击（转账时使用`ReentrancyGuard`防护）。
    
-   选择安全方案：优先使用OpenZeppelin的合约（如`Ownable`权限管理、`ERC721`NFT标准、`ReentrancyGuard`防重入）。
    

### **二、合约开发阶段（核心：编写代码，遵循规范）**

1.  **搭建本地开发环境**
    

-   以Hardhat为例，命令行步骤：
    

```
# 1. 新建项目文件夹并进入
mkdir nft-contract && cd nft-contract
# 2. 初始化npm项目
npm init -y
# 3. 安装Hardhat
npm install --save-dev hardhat
# 4. 初始化Hardhat项目（选择Create a JavaScript project）
npx hardhat
# 5. 安装OpenZeppelin合约库（必装，提升安全性）
npm install @openzeppelin/contracts
```

2.  **编写合约代码**
    

-   在项目的`contracts/`目录下创建`.sol`文件（如`MyNFT.sol`）。
    
-   遵循Solidity规范：
    

-   开头指定编译器版本（如`pragma solidity ^0.8.20;`，避免使用过时版本）。
    
-   继承开源库合约（如`ERC721`+`Ownable`），减少自研代码量。
    
-   注释清晰：对核心函数、参数、权限进行说明，方便后续测试和维护。
    

-   示例（简单NFT合约）：
    

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyNFT is ERC721, Ownable {
    uint256 public totalSupply;
    uint256 public maxSupply = 1000;
    uint256 public mintPrice = 0.01 ether;

    // 构造函数：初始化NFT名称、符号，设置合约拥有者
    constructor() ERC721("MyNFT", "MNFT") Ownable(msg.sender) {}

    // 铸造函数：仅当未达到最大供应量时，用户支付ETH可铸造
    function mint() external payable {
        require(totalSupply < maxSupply, "Sold out");
        require(msg.value == mintPrice, "Invalid payment");
        totalSupply++;
        // _safeMint：安全铸造，确保接收方是ERC721兼容地址
        _safeMint(msg.sender, totalSupply);
    }

    // 提取合约余额：仅拥有者可操作
    function withdraw() external onlyOwner {
        (bool success, ) = payable(owner()).call{value: address(this).balance}("");
        require(success, "Withdraw failed");
    }

    // 接收ETH的函数
    receive() external payable {}
}
```

3.  **编译合约**
    

-   使用Hardhat编译合约，生成**字节码**（部署到区块链的代码）和**ABI**（前端交互的接口）：
    

```
npx hardhat compile
```

-   编译成功后，会在`artifacts/`目录下生成合约的编译产物，ABI是后续前端交互的核心文件。
    

### **三、测试阶段（核心：全面验证，消灭BUG）**

**智能合约上线后无法修改，测试是重中之重**，必须覆盖功能测试、边界测试、安全测试。

1.  **编写自动化测试用例**
    

-   在项目的`test/`目录下创建测试文件（如`MyNFT.test.js`），使用JavaScript/TypeScript编写，基于`Chai`断言库。
    
-   测试覆盖场景：
    

-   功能测试：铸造NFT、提取余额是否正常。
    
-   边界测试：达到最大供应量后是否无法铸造、支付金额错误是否会报错。
    
-   权限测试：非拥有者是否无法调用`withdraw`函数。
    

-   示例测试代码片段：
    

```
const { expect } = require("chai");
const { ethers } = require("hardhat");

describe("MyNFT", function () {
  let myNFT;
  let owner;
  let user;

  beforeEach(async function () {
    // 部署合约
    [owner, user] = await ethers.getSigners();
    const MyNFTFactory = await ethers.getContractFactory("MyNFT");
    myNFT = await MyNFTFactory.deploy();
    await myNFT.waitForDeployment();
  });

  // 测试铸造功能
  it("Should mint NFT successfully", async function () {
    const mintPrice = await myNFT.mintPrice();
    await expect(myNFT.connect(user).mint({ value: mintPrice }))
      .to.emit(myNFT, "Transfer") // 验证Transfer事件是否触发
      .withArgs(ethers.ZeroAddress, user.address, 1);
    expect(await myNFT.totalSupply()).to.equal(1);
  });

  // 测试权限控制
  it("Should reject withdraw by non-owner", async function () {
    await expect(myNFT.connect(user).withdraw()).to.be.revertedWithCustomError(myNFT, "OwnableUnauthorizedAccount");
  });
});
```

2.  **运行自动化测试**
    

-   执行测试命令，查看所有用例是否通过：
    

```
npx hardhat test
```

-   若测试失败，根据报错信息修改合约代码，直到所有用例通过。
    

3.  **本地节点手动调试（可选）**
    

-   启动Hardhat本地节点，模拟区块链环境：
    

```
npx hardhat node
```

-   另开终端，部署合约到本地节点，手动调用函数验证：
    

```
npx hardhat run scripts/deploy.js --network localhost
```

4.  **安全审计（可选但推荐）**
    

-   个人/小型项目：使用工具自查（如`Slither`静态分析工具）。
    
-   商业/高价值项目：委托专业审计机构（如慢雾、CertiK）进行审计，排查安全漏洞。
    

### **四、部署阶段（核心：先测试网，后主网）**

遵循「**测试网验证→主网上线**」的流程，避免主网资产损失。

1.  **编写部署脚本**
    

-   在项目的`scripts/`目录下创建`deploy.js`：
    

```
const { ethers } = require("hardhat");

async function main() {
  const MyNFTFactory = await ethers.getContractFactory("MyNFT");
  console.log("Deploying MyNFT...");
  const myNFT = await MyNFTFactory.deploy();
  await myNFT.waitForDeployment();
  console.log("MyNFT deployed to:", myNFT.target); // 打印合约地址，务必保存
}

main().catch((error) => {
  console.error(error);
  process.exit(1);
});
```

2.  **配置测试网/主网**
    

-   修改`hardhat.config.js`，添加测试网（如Sepolia）或主网配置，需要：
    

-   钱包私钥（MetaMask导出，**绝对保密**）。
    
-   RPC节点URL（从Alchemy/Infura免费申请）。
    

-   配置示例：
    

```
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config(); // 用dotenv管理私钥，避免硬编码

module.exports = {
  solidity: "0.8.20",
  networks: {
    sepolia: {
      url: process.env.ALCHEMY_SEPOLIA_URL,
      accounts: [process.env.PRIVATE_KEY],
    },
    // 以太坊主网（部署前务必谨慎）
    mainnet: {
      url: process.env.ALCHEMY_MAINNET_URL,
      accounts: [process.env.PRIVATE_KEY],
    },
  },
};
```

-   创建`.env`文件存储私钥和RPC URL：
    

```
PRIVATE_KEY=你的钱包私钥
ALCHEMY_SEPOLIA_URL=你的Sepolia RPC地址
```

3.  **部署到测试网**
    

-   获取测试网代币（如Sepolia ETH，通过水龙头免费领取），用于支付部署手续费。
    
-   执行部署命令：
    

```
npx hardhat run scripts/deploy.js --network sepolia
```

-   部署成功后，记录**合约地址**，并在区块链浏览器（如Etherscan）查看合约状态。
    

4.  **测试网功能验证**
    

-   通过Remix或前端Demo，在测试网手动调用合约函数，验证所有功能是否正常（如铸造NFT、提取余额）。
    

5.  **部署到主网（最终步骤）**
    

-   测试网验证无误后，执行主网部署命令（需支付真实Gas费，务必谨慎）：
    

```
npx hardhat run scripts/deploy.js --network mainnet
```

-   主网部署后，**建议在Etherscan上验证合约源代码**，提升透明度。
    

### **五、上线与维护阶段（核心：交互开发，监控运行）**

1.  **前端交互开发**
    

-   使用Ethers.js/Web3.js，结合合约ABI和地址，开发前端DApp：
    

-   实现钱包连接（如MetaMask）。
    
-   调用合约函数（如铸造NFT、查询余额）。
    
-   监听合约事件（如`Transfer`事件，实时更新NFT持有状态）。
    

2.  **合约监控与维护**
    

-   监控合约的交易记录和资产变动（通过区块链浏览器）。
    
-   若合约设计了可升级方案（如使用代理模式），可通过升级合约修复非核心问题；普通合约无法升级，需提前做好需求调研。
    

### **总结：智能合约开发核心流程**

`需求设计 → 环境搭建 → 代码编写 → 编译 → 自动化测试 → 测试网部署验证 → 主网部署 → 前端交互 → 监控维护`

其中，**需求设计和测试**是决定合约成败的关键，新手切勿跳过这两个环节。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
# 2026-07-10

# 部署你的第一个 Monad 合约

## 一、最小合约（Solidity）

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

/// @title Counter
/// @notice 一个最小可行合约：链上计数器，用于验证 Monad Testnet 部署与读写交互
contract Counter {
    uint256 public count;

    event CountIncreased(uint256 newCount, address indexed by);

    /// @notice 读函数：获取当前计数（public 变量自动生成 getter，也可显式写一个）
    function getCount() external view returns (uint256) {
        return count;
    }

    /// @notice 写函数：计数 +1
    function increment() external {
        count += 1;
        emit CountIncreased(count, msg.sender);
    }
}
```

选这个合约是因为它足够简单（无外部依赖、无复杂逻辑），能干净地演示"一个 read function + 一个 write function"，适合作为第一次上链验证流程的最小单元。

## 二、部署步骤（Remix + 课程专用钱包）

1.  **打开 Remix**：[https://remix.ethereum.org](https://remix.ethereum.org)
    
2.  **新建文件** `Counter.sol`，粘贴上面的代码
    
3.  **编译**：左侧 Solidity Compiler 面板，编译器版本选 `0.8.24` 左右，点击 Compile Counter.sol
    
4.  **配置钱包连接 Monad Testnet**（以 MetaMask 为例）：
    

-   打开 MetaMask → 添加网络 → 手动填入 Monad Testnet 参数（Chain ID、RPC URL、区块浏览器地址，以 Monad 官方文档 [docs.monad.xyz](http://docs.monad.xyz) 当前公布的为准，测试网参数偶尔会变，部署前务必去官方文档核对一次）
    
-   用课程专用钱包地址，去官方 Faucet 领取测试网 MON 作为 gas
    

5.  **部署**：Remix 左侧 Deploy & Run Transactions 面板，Environment 选择 "Injected Provider - MetaMask"，确认钱包已切到 Monad Testnet 且账户是课程专用钱包，点击 Deploy，在 MetaMask 弹窗里确认交易
    
6.  **记录**：部署成功后，在 Remix 下方 Deployed Contracts 里能看到合约地址；在 MetaMask 的活动记录里点开这笔交易，复制交易哈希（Tx Hash）
    

## 三、合约交互

-   **Read**：在 Remix 部署面板里点击 `getCount`（蓝色按钮，不消耗 gas，不用签名）
    
-   **Write**：点击 `increment`（橙色按钮，会弹出 MetaMask 签名确认，消耗少量测试网 gas）——这一笔也会产生一个交易哈希，建议一并记录
    

## 四、README 模板（填空即可）

```
# Counter Contract — Monad Testnet 部署记录

## 合约说明
Counter 是一个最小可行的链上计数器合约，包含：
- 一个公开状态变量 count（uint256）
- 一个只读函数 getCount()，用于查询当前计数
- 一个写函数 increment()，每次调用使 count 加一，并触发 CountIncreased 事件

用途：作为熟悉 Monad Testnet 部署与交互流程的最小示例。

## 部署信息
- 网络：Monad Testnet
- 合约地址：<部署完成后填入>
- 部署交易哈希：<部署完成后填入>
- 部署账户：<课程专用钱包地址>
- 部署时间：<填入日期>

## 部署方式
1. Remix IDE 编写并编译 Counter.sol（Solidity 0.8.24）
2. MetaMask 连接 Monad Testnet，使用课程专用钱包
3. Remix Deploy & Run 面板，Injected Provider 部署
4. 部署成功后记录合约地址与交易哈希

## 交互记录
### Read
- 调用函数：getCount()
- 返回结果：<填入调用结果，如 0>

### Write
- 调用函数：increment()
- 交易哈希：<填入>
- 调用后再次读取 getCount()，确认结果：<填入，如 1>

## 备注
- Gas 来源：Monad Testnet Faucet
- 区块浏览器验证链接：<MonadVision 或 Monadscan 上该合约/交易的链接>
```
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

# 使用 AI 工具生成一个最小 Solidity 合约

我选择的方向是 **打卡合约（CheckIn）**。下面按你要求的五步走一遍完整流程。

## 1\. 我写的 Prompt

帮我用 Solidity 写一个最小可用的"打卡合约"：每个地址每天（UTC）最多打卡一次；需要记录每个地址的累计打卡次数；打卡成功要触发事件；再加一个查询"今天是否已打卡"的只读函数。要求代码尽量精简，不要用第三方库，加上必要的注释。

## 2\. AI 输出的代码

（已生成并附在上方文件 `CheckIn.sol` 中，核心逻辑如下）

```
mapping(address => Record) private records; // Record { totalCheckIns, lastCheckInDay }

function checkIn() external {
    uint256 today = block.timestamp / 1 days;
    Record storage r = records[msg.sender];
    require(r.lastCheckInDay < today, "Already checked in today");
    r.lastCheckInDay = today;
    r.totalCheckIns += 1;
    emit CheckedIn(msg.sender, today, r.totalCheckIns);
}
```

## 3\. 让 AI 解释合约结构

-   `Record` **结构体**：每个地址对应一条记录，存 `totalCheckIns`（累计次数）和 `lastCheckInDay`（最近打卡的"天数编号"）。
    
-   `records` **映射**：`address => Record`，是整个合约唯一的状态存储。
    
-   `checkIn()`：核心写函数，用 `block.timestamp / 1 days` 把时间戳换算成"第几天"，通过比较天数编号防止同一天重复打卡。
    
-   **两个 view 函数**：`getTotalCheckIns` 和 `hasCheckedInToday`，都是只读查询，不消耗 gas（链下调用）。
    
-   `CheckedIn` **事件**：每次打卡成功都会 emit，方便前端监听或做索引。
    

## 4\. 人工检查（实际操作，不是空谈）

**✅ 检查点一：合约是否能编译** 没有只看代码"猜"，而是真的装了 `solc@0.8.24` 跑了一遍编译：

```
✅ 编译成功
ABI 函数数量: 4
字节码长度: 1278 bytes
```

确认语法、pragma 版本、类型都没问题。

**✅ 检查点二：是否存在权限问题** 逐个函数看谁能调用：

-   `checkIn()`：任何地址都能调用，且只能操作 `msg.sender` 自己的记录，不存在越权写别人数据的问题。
    
-   两个 view 函数：只读，任何人都能查任何地址，这是预期行为（打卡记录本来就该公开可查）。
    
-   合约**没有 owner、没有管理员函数**，这点要特别确认是不是我想要的——对于"打卡"这种场景，我认为不需要管理员权限，这个设计是合理的。
    

**✅ 检查点三：潜在安全风险**

-   用 `block.timestamp` 来判断"天"存在矿工可小范围操纵时间戳的理论风险，但打卡场景对精确到秒的时间没有强依赖，误差几十秒不影响业务逻辑，可接受。
    
-   没有外部调用（没有 `call`/`transfer`/`delegatecall`），不存在重入风险。
    
-   没有资金操作，不存在资金被卷走的风险。
    
-   `Record storage r = records[msg.sender]` 这里用 `storage` 引用而不是复制到 memory，逻辑正确，不会出现"改了本地变量但没写回链上"的常见 bug。
    

**✅ 检查点四（额外做的）：命名与逻辑复杂度**

-   命名清晰：`checkIn`、`totalCheckIns`、`hasCheckedInToday` 语义直白，不需要额外注释也能看懂。
    
-   没有多余的复杂逻辑（没有过度设计的角色系统、没有不必要的升级代理模式），符合"最小合约"的要求。
    

## 5\. 我对 AI 输出做的修改 / 判断

| 项目 | AI 原始建议 | 我的判断/修改 |
| 时间计算 | 直接用了 block.timestamp / 1 days | 保留，但我特意确认了这是"UTC 天"而非"用户当地时间天"，并在注释里明确标注，避免用户误解为按时区打卡 |
| 数据可见性 | records声明为 private | 认可这个选择，因为已经提供了两个 public 的 getter，没必要把整个 mapping 设为 public 暴露内部结构 |
| 是否加管理员/暂停功能 | AI 没主动加 | 我确认不需要，打卡这种低风险场景加 owner 权限反而是不必要的复杂度和攻击面 |
| 编译验证 | AI 只给了代码，没验证 | 我额外用 solc 实际编译了一遍，而不是单纯相信 AI 说"这段代码没问题" |

**小结**：这次练习里 AI 负责了"从零到能跑的初稿"，但真正决定"这个设计对不对、要不要加权限、时间戳精度够不够"的，是人工检查这一步。AI 生成的代码语法层面基本没问题，但业务判断（比如要不要 owner 权限）必须由人来定夺。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

# **Monad 理解｜为什么 Monad 体验不同**

\- 高频交互  
\- 低延迟体验  
\- Consumer Crypto  
\- 游戏 / 小游戏  
\- Onchain Social  
\- AI Agent 交互

在以上这些场景里，我会选 **全链游戏 / 链上小游戏（Onchain Games / Mini-games）** 作为最适合 Monad 的高频交互场景。理由不是单一维度的匹配，而是它几乎踩中了 Monad 架构里每一个针对性设计的优化点。

## 为什么是游戏，而不是交易或社交

**1\. 状态天然"低冲突"，正好吃到并行执行的红利**

Monad 的核心技术乐观并行执行，本质是"识别互不依赖的交易，同时跑"。游戏里绝大多数动作——移动、攻击、开箱、升级、铸造装备——都是**作用在单个玩家自己的状态上**，玩家 A 打怪和玩家 B 交易装备之间几乎没有数据依赖。这种"高频但低耦合"的写入模式，是并行执行效果最好的场景。

对比一下高频交易（perp DEX、订单簿撮合）：大量交易会集中读写**同一个流动性池 / 同一本订单簿**，这是并行执行里最难处理的"热点状态"（hot state）——冲突率高，乐观执行频繁回滚，实际上很难摸到 10,000 TPS 的理论上限。所以虽然 Monad 官方叙事里常拿"高频 DeFi"举例，但从架构适配角度看，游戏这种"状态天然分片"的场景反而更容易把并行执行的优势发挥出来。

**2\. 亚秒级终局性直接决定"能不能玩"**

游戏对延迟极度敏感——这不是"体验更好"的加分项，而是"能不能形成实时反馈闭环"的门槛项。以太坊主网 12 秒以上的确认时间，意味着你打一下怪要等十几秒才能看到链上结果，游戏逻辑基本没法直接放在链上，只能靠链下模拟 + 事后同步这种妥协方案。Monad 的 0.8 秒单槽终局 + 0.4 秒出块，第一次让"动作—链上确认—UI 反馈"这个循环进入了勉强可接受的实时区间，游戏才有可能把更多逻辑真正放到链上而不是仅仅做资产层。

**3\. 高频小额交易要求成本几乎为零**

一局游戏可能产生几十上百笔链上写入（每一次移动、每一次交互），如果 gas 费按以太坊主网水平计算，游戏经济模型直接不成立。Monad 的低费用是这类场景的准入条件，而不是锦上添花。

**4\. EVM 兼容性降低了"链上游戏"这个高风险品类的开发成本**

全链游戏本身工程复杂度就很高（状态机设计、防作弊、可组合性），如果还要额外学一套新的 VM 和工具链（比如 Solana 的 Sealevel 编程模型），团队的试错成本会进一步推高。Monad 让团队可以直接复用 Solidity、Foundry、现有的钱包和衍生品/资产协议，把精力集中在游戏本身而不是重新造轮子。

## 需要留意的边界

并不是游戏里所有交互都天然低冲突——**全局排行榜、抢购/开盒类的全服共享资源、PvP 直接对战的同一场次状态**，这些仍然是热点状态，一样会触发并行回滚。所以更准确的说法是：Monad 更适合承载"以玩家个体状态为主、全局共享状态为辅"的游戏设计——比如养成类、探索类、慢节奏 MMO 的背包/装备系统，而不是強撮合、强实时对抗的品类（这类可能仍需要链下撮合+链上结算的混合架构）。

排行榜/任务系统其实是游戏场景的一个子集，可以作为同一套基础设施上的附加层，但如果单独拎出来看，它的"读多写少、偶发批量更新"特征，对 Monad 高吞吐架构的利用率反而不如游戏本身高。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

# Solidity学习笔记

# 一、值类型（Value Types）

值类型变量在赋值或传递时，会创建一个完整的副本，修改副本不会影响原变量。Solidity中常见的值类型包括布尔型、整数型、地址型、字节型、枚举型等。

## 1\. 布尔型（bool）

仅包含两个值：true（真）和false（假），支持逻辑运算（与&&、或||、非!）。

```
// 布尔型示例
pragma solidity ^0.8.20;

contract BoolExample {
    bool public isActive = true; // 状态变量，默认公开可访问
    
    function toggleActive() public {
        isActive = !isActive; // 逻辑非运算，切换状态
    }
    
    function checkAnd(bool a, bool b) public pure returns (bool) {
        return a && b; // 逻辑与运算
    }
}
```

## 2\. 整数型（int/uint）

int为有符号整数，uint为无符号整数，后缀可指定位数（8~256，步长8），默认256位。支持加减乘除（+、-、\*、/）、取余（%）、自增自减（++、--）等运算。

```
// 整数型示例
pragma solidity ^0.8.20;

contract IntExample {
    uint256 public count = 0; // 无符号256位整数，初始值0
    int16 public balance = -100; // 有符号16位整数
    
    function increment() public {
        count++; // 自增运算
    }
    
    function add(uint256 a, uint256 b) public pure returns (uint256) {
        return a + b; // 加法运算，无符号整数不会出现负数
    }
    
    function remainder(int16 a, int16 b) public pure returns (int16) {
        return a % b; // 取余运算，结果符号与被除数一致
    }
}
```

## 3\. 地址型（address）

用于存储以太坊账户地址，长度为20字节（160位），分为普通地址（address）和可接收ETH的地址（address payable）。address payable拥有transfer()、send()方法用于转账。

```
// 地址型示例
pragma solidity ^0.8.20;

contract AddressExample {
    address public owner; // 普通地址
    address payable public recipient; // 可接收ETH的地址
    
    constructor() {
        owner = msg.sender; // 部署合约时，将部署者地址赋值给owner
        recipient = payable(0x5B38Da6a701c568545dCfcB03FcB875f56beddC4); // 转换为可支付地址
    }
    
    // 向recipient转账ETH
    function sendETH() public payable {
        require(msg.value > 0, "Amount must be greater than 0");
        recipient.transfer(msg.value); // 转账，失败会回滚交易
    }
    
    // 获取地址余额
    function getBalance(address addr) public view returns (uint256) {
        return addr.balance; // 返回地址的ETH余额，单位为wei
    }
}
```

## 4\. 字节型（bytes）

分为固定长度字节数组（bytes1~bytes32）和动态长度字节数组（bytes）。固定长度字节数组更省gas，动态长度字节数组类似字符串。

```
// 字节型示例
pragma solidity ^0.8.20;

contract BytesExample {
    bytes32 public fixedBytes = "Solidity"; // 固定长度32字节，不足补0
    bytes public dynamicBytes = "Hello Solidity"; // 动态长度字节数组
    
    function getByteLength() public view returns (uint256 fixedLen, uint256 dynamicLen) {
        fixedLen = fixedBytes.length; // 固定长度始终为32
        dynamicLen = dynamicBytes.length; // 动态长度为字符串实际字节数
    }
    
    function getByteAt(uint256 index) public view returns (bytes1) {
        require(index < dynamicBytes.length, "Index out of range");
        return dynamicBytes[index]; // 获取指定索引位置的字节
    }
}
```

## 5\. 枚举型（enum）

自定义值类型，用于限制变量取值范围，默认从0开始按顺序赋值，可显式指定值。

```
// 枚举型示例
pragma solidity ^0.8.20;

contract EnumExample {
    // 定义枚举，代表订单状态
    enum OrderStatus { Pending, Paid, Shipped, Delivered, Cancelled }
    
    OrderStatus public currentStatus = OrderStatus.Pending; // 初始状态为Pending
    
    // 更新订单状态
    function updateStatus(OrderStatus newStatus) public {
        currentStatus = newStatus;
    }
    
    // 获取状态对应的数值
    function getStatusValue() public view returns (uint256) {
        return uint256(currentStatus); // Pending=0，Paid=1，依次类推
    }
}
```

# 二、函数（Functions）

函数是Solidity合约的核心执行单元，用于封装业务逻辑。函数定义需包含可见性、修饰符、返回值、参数等要素，还可指定状态可变性。

## 1\. 函数结构

语法：function 函数名(参数类型 参数名, ...) 可见性 状态可变性 修饰符 returns (返回值类型) { 函数体 }

```
function <function name>([parameter types[, ...]]) {internal|external|public|private} [pure|view|payable] [virtual|override] [<modifiers>]
[returns (<return types>)]{ <function body> }
```

## 2\. 可见性修饰符

-   public：合约内外均可访问，状态变量默认public。
    
-   private：仅当前合约可访问，子类也无法访问。
    
-   internal：当前合约及子类可访问，默认函数可见性。
    
-   external：仅合约外部可访问，合约内部需通过this调用。
    

## 3\. 状态可变性修饰符

-   view：仅读取状态变量，不修改合约状态，不消耗gas（外部调用时）。
    
-   pure：不读取也不修改状态变量，仅处理输入参数，不消耗gas（外部调用时）。
    
-   payable：允许函数接收ETH，调用时需附带value参数。
    

```
// 函数示例
pragma solidity ^0.8.20;

contract FunctionExample {
    uint256 public num = 10;
    
    // public函数，可内外访问，修改状态
    function setNum(uint256 _num) public {
        num = _num;
    }
    
    // private函数，仅当前合约访问，读取状态
    function getPrivateNum() private view returns (uint256) {
        return num * 2;
    }
    
    // internal函数，当前合约及子类访问
    function getInternalNum() internal view returns (uint256) {
        return getPrivateNum();
    }
    
    // external函数，仅外部访问
    function getExternalNum() external view returns (uint256) {
        return num;
    }
    
    // pure函数，不读写状态
    function add(uint256 a, uint256 b) public pure returns (uint256) {
        return a + b;
    }
    
    // payable函数，接收ETH
    function receiveETH() public payable {
        // 无需额外逻辑，ETH自动存入合约地址
    }
}
```

# 三、函数输出（Function Outputs）

Solidity函数支持返回单个或多个值，可通过returns指定返回类型，也可使用return语句显式返回，还支持命名返回值（自动初始化，无需显式赋值）。

## 1\. 单个返回值

```
// 单个返回值示例
pragma solidity ^0.8.20;

contract SingleReturnExample {
    function getNum() public pure returns (uint256) {
        return 100; // 显式返回单个值
    }
    
    function getString() public pure returns (string memory) {
        return "Hello Solidity"; // 返回字符串，需指定memory
    }
}
```

## 2\. 多个返回值

```
// 多个返回值示例
pragma solidity ^0.8.20;

contract MultiReturnExample {
    // 普通多个返回值
    function getInfo() public pure returns (uint256, string memory, bool) {
        return (25, "Alice", true); // 按顺序返回多个值
    }
    
    // 命名返回值，自动初始化
    function getNamedInfo() public pure returns (uint256 age, string memory name, bool isActive) {
        age = 30;
        name = "Bob";
        isActive = false;
        // 无需显式return，自动返回命名变量的值
    }
    
    // 接收多个返回值
    function receiveMultiReturn() public pure returns (uint256, string memory) {
        (uint256 age, string memory name, ) = getInfo(); // 忽略第三个返回值
        return (age, name);
    }
}
```

# 四、变量数据存储和作用域

## 1\. 数据存储位置

Solidity中变量存储位置分为三类，不同位置影响gas消耗和访问规则：

-   storage：存储在区块链上的持久化存储，用于状态变量，gas消耗最高。
    
-   memory：临时存储，仅在函数执行期间存在，函数结束后释放，用于函数参数、局部变量（引用类型需显式指定），gas消耗较低。
    
-   calldata：类似memory，但仅用于外部函数的输入参数，不可修改，gas消耗最低。
    

```
// 存储位置示例
pragma solidity ^0.8.20;

contract StorageExample {
    uint256 public storageVar = 10; // 状态变量，默认storage
    
    function testStorage(uint256 calldata _calldataVar) public view returns (uint256, uint256) {
        uint256 memoryVar = _calldataVar; // 局部变量，存储在memory
        return (storageVar, memoryVar);
    }
    
    // 引用类型需显式指定存储位置
    function copyString(string calldata _str) public pure returns (string memory) {
        string memory newStr = _str; // calldata复制到memory
        return newStr;
    }
}
```

## 2\. 变量作用域

-   全局作用域：全局变量（也叫状态变量），定义在合约内部、函数外部，整个合约及子类可访问，持久化存储在storage。
    
-   局部作用域：局部变量，定义在函数内部，仅函数执行期间有效，存储在memory或stack（值类型），函数结束后销毁。
    
-   合约作用域：合约内的private变量，仅当前合约可访问，跨合约不可见。
    

```
// 作用域示例
pragma solidity ^0.8.20;

contract ScopeExample {
    uint256 public globalVar = 100; // 全局变量（状态变量）
    uint256 private privateGlobalVar = 200; // 合约作用域全局变量
    
    function testScope() public view returns (uint256, uint256) {
        uint256 localVar = 300; // 局部变量，仅函数内访问
        return (globalVar, localVar);
    }
    
    function getPrivateGlobal() private view returns (uint256) {
        return privateGlobalVar; // 仅当前合约可访问
    }
}
```

# 五、引用类型（Reference Types）

引用类型变量赋值或传递时，仅传递引用（内存地址），修改副本会影响原变量。需显式指定存储位置（storage、memory、calldata），常见引用类型包括字符串（string）、数组（array）、结构体（struct）。

## 1\. 字符串（string）

本质是动态长度字节数组，支持length属性获取长度，可通过bytes转换实现字节级操作。

```
// 字符串示例
pragma solidity ^0.8.20;

contract StringExample {
    string public name = "Solidity"; // 状态变量，存储在storage
    
    function getStringLength() public view returns (uint256) {
        return bytes(name).length; // 转换为bytes获取长度
    }
    
    function concatString(string memory _str1, string memory _str2) public pure returns (string memory) {
        return string(abi.encodePacked(_str1, " ", _str2)); // 字符串拼接
    }
    
    function changeChar(string memory _str, uint256 _index, bytes1 _char) public pure returns (string memory) {
        bytes memory strBytes = bytes(_str);
        require(_index < strBytes.length, "Index out of range");
        strBytes[_index] = _char; // 字节级修改
        return string(strBytes);
    }
}
```

## 2\. 数组（array）

分为固定长度数组（长度初始化后不可变）和动态长度数组（长度可动态增减），支持push()、pop()等方法，可通过索引访问元素。

```
// 数组示例
pragma solidity ^0.8.20;

contract ArrayExample {
    // 固定长度数组
    uint256[5] public fixedArray = [1, 2, 3, 4, 5];
    // 动态长度数组
    uint256[] public dynamicArray;
    // 二维动态数组
    uint256[][] public twoDArray;
    
    function addElement(uint256 _num) public {
        dynamicArray.push(_num); // 向动态数组添加元素
    }
    
    function removeLastElement() public {
        require(dynamicArray.length > 0, "Array is empty");
        dynamicArray.pop(); // 删除最后一个元素
    }
    
    function getArrayLength() public view returns (uint256 fixedLen, uint256 dynamicLen) {
        fixedLen = fixedArray.length; // 固定长度不可变
        dynamicLen = dynamicArray.length; // 动态长度随元素增减变化
    }
    
    function addTwoDElement(uint256[] memory _arr) public {
        twoDArray.push(_arr); // 向二维数组添加一维数组
    }
}
```

## 3\. 结构体（struct）

自定义复合类型，可包含多个不同类型的变量，用于封装一组相关数据。

```
// 结构体示例
pragma solidity ^0.8.20;

contract StructExample {
    // 定义结构体，代表用户信息
    struct User {
        string name;
        uint256 age;
        address addr;
        bool isActive;
    }
    
    // 结构体数组，存储多个用户
    User[] public users;
    // 结构体映射，通过地址关联用户
    mapping(address => User) public userMap;
    
    // 添加用户
    function addUser(string memory _name, uint256 _age) public {
        User memory newUser = User({
            name: _name,
            age: _age,
            addr: msg.sender,
            isActive: true
        });
        users.push(newUser);
        userMap[msg.sender] = newUser;
    }
    
    // 更新用户状态
    function updateUserStatus(address _addr, bool _isActive) public {
        userMap[_addr].isActive = _isActive; // 引用修改，影响原数据
    }
    
    // 获取用户信息
    function getUser(address _addr) public view returns (string memory, uint256, bool) {
        User memory user = userMap[_addr];
        return (user.name, user.age, user.isActive);
    }
}
```

# 六、映射类型（Mapping）

映射是Solidity中键值对存储结构，类似哈希表，支持高效的键查找。语法：mapping(键类型 => 值类型) 变量名。键类型支持值类型（bool、int、uint、address、bytes等），值类型可是任意类型（包括映射、结构体、数组）。

映射的特点：无长度属性，无法遍历（需手动维护索引），仅支持通过键获取值，默认值为对应类型的零值。

```
// 映射类型示例
pragma solidity ^0.8.20;

contract MappingExample {
    // 基础映射：地址=>余额
    mapping(address => uint256) public balanceMap;
    // 嵌套映射：地址=>（字符串=>bool），存储用户权限
    mapping(address => mapping(string => bool)) public permissionMap;
    // 结构体映射：地址=>用户结构体
    struct User {
        string name;
        uint256 score;
    }
    mapping(address => User) public userMap;
    
    // 存入余额
    function deposit() public payable {
        balanceMap[msg.sender] += msg.value;
    }
    
    // 提取余额
    function withdraw(uint256 _amount) public {
        require(balanceMap[msg.sender] >= _amount, "Insufficient balance");
        balanceMap[msg.sender] -= _amount;
        payable(msg.sender).transfer(_amount);
    }
    
    // 授予权限
    function grantPermission(address _user, string memory _permission) public {
        permissionMap[_user][_permission] = true;
    }
    
    // 设置用户信息
    function setUser(string memory _name, uint256 _score) public {
        userMap[msg.sender] = User(_name, _score);
    }
    
    // 检查权限
    function hasPermission(address _user, string memory _permission) public view returns (bool) {
        return permissionMap[_user][_permission];
    }
}
```

# 七、变量初始值（Default Values）

Solidity中未显式初始化的变量，会自动赋予对应类型的零值（默认值），无需手动赋值。

## 常见类型默认值

-   布尔型：false
    
-   整数型：0
    
-   地址型：address(0)（空地址，0x0000000000000000000000000000000000000000）
    
-   字节型：bytes1(0x00)、bytes（空数组）
    
-   字符串：空字符串（""）
    
-   数组：空数组（长度为0）
    
-   映射：无默认值，访问不存在的键返回对应值类型的零值
    
-   结构体：各成员变量均为对应类型的零值
    

```
// 变量初始值示例
pragma solidity ^0.8.20;

contract DefaultValueExample {
    bool public boolVar; // 默认false
    uint256 public uintVar; // 默认0
    int256 public intVar; // 默认0
    address public addrVar; // 默认address(0)
    string public strVar; // 默认空字符串
    uint256[] public arrVar; // 默认空数组
    mapping(address => uint256) public mapVar; // 访问不存在的键返回0
    
    struct User {
        string name;
        uint256 age;
    }
    User public userVar; // 成员name为空字符串，age为0
    
    function checkDefaultValues() public view returns (bool, uint256, address, string memory) {
        return (boolVar, uintVar, addrVar, strVar);
    }
}
```

# 八、常数（Constants）

Solidity中常数分为两种：constant和immutable，均为不可修改的变量，编译时确定值（或部署时确定值），可节省gas。

## 1\. constant（常量）

必须在定义时显式赋值，值需为编译时常量（不能依赖运行时数据，如msg.sender），存储在合约字节码中，而非storage。

## 2\. immutable（不可变变量）

可在定义时赋值，也可在构造函数中赋值（仅一次），值为部署时常量，存储在storage中，但访问时无需读取storage，gas消耗低于普通状态变量。

```
// 常数示例
pragma solidity ^0.8.20;

contract ConstantExample {
    // constant：编译时常量，定义时赋值
    uint256 public constant MAX_NUM = 1000;
    string public constant CONTRACT_NAME = "SolidityConstantDemo";
    address public constant DEFAULT_ADDR = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;
    
    // immutable：部署时常量，可在构造函数赋值
    uint256 public immutable INIT_NUM;
    address public immutable OWNER;
    
    constructor(uint256 _initNum) {
        INIT_NUM = _initNum; // 仅构造函数中可赋值一次
        OWNER = msg.sender; // 依赖部署时的msg.sender，无法用constant
    }
    
    function getMaxNum() public pure returns (uint256) {
        return MAX_NUM; // constant变量可在pure函数中访问
    }
}
```

# 九、控制流（Control Flow）

Solidity控制流与其他编程语言类似，包括条件语句、循环语句、跳转语句，用于控制函数执行流程。

## 1\. 条件语句（if-else、if-else if-else）

```
// 条件语句示例
pragma solidity ^0.8.20;

contract ConditionExample {
    function checkNum(uint256 _num) public pure returns (string memory) {
        if (_num > 100) {
            return "Greater than 100";
        } else if (_num == 100) {
            return "Equal to 100";
        } else {
            return "Less than 100";
        }
    }
    
    function checkAddress(address _addr) public pure returns (bool) {
        if (_addr == address(0)) {
            return false; // 禁止空地址
        }
        return true;
    }
}
```

## 2\. 循环语句（for、while、do-while）

```
// 循环语句示例
pragma solidity ^0.8.20;

contract LoopExample {
    // for循环：计算1~n的和
    function sum(uint256 _n) public pure returns (uint256) {
        uint256 total = 0;
        for (uint256 i = 1; i <= _n; i++) {
            total += i;
        }
        return total;
    }
    
    // while循环：查找数组中第一个大于10的元素
    function findFirstGreaterThanTen(uint256[] memory _arr) public pure returns (uint256) {
        uint256 index = 0;
        while (index < _arr.length && _arr[index] <= 10) {
            index++;
        }
        require(index < _arr.length, "No element greater than 10");
        return _arr[index];
    }
    
    // do-while循环：至少执行一次
    function doWhileDemo(uint256 _num) public pure returns (uint256) {
        uint256 count = 0;
        do {
            count++;
            _num--;
        } while (_num > 0);
        return count;
    }
}
```

## 3\. 跳转语句（break、continue）

```
// 跳转语句示例
pragma solidity ^0.8.20;

contract JumpExample {
    // break：跳出循环
    function breakDemo(uint256[] memory _arr) public pure returns (uint256) {
        uint256 count = 0;
        for (uint256 i = 0; i < _arr.length; i++) {
            if (_arr[i] == 0) {
                break; // 遇到0则跳出循环
            }
            count++;
        }
        return count;
    }
    
    // continue：跳过当前循环，进入下一次
    function continueDemo(uint256[] memory _arr) public pure returns (uint256) {
        uint256 sum = 0;
        for (uint256 i = 0; i < _arr.length; i++) {
            if (_arr[i] % 2 == 0) {
                continue; // 跳过偶数，只累加奇数
            }
            sum += _arr[i];
        }
        return sum;
    }
}
```

# 十、构造函数和修饰器

## 1\. 构造函数（Constructor）

构造函数是合约部署时自动执行的特殊函数，仅执行一次，用于初始化合约状态（如设置所有者、初始化变量）。一个合约只能有一个构造函数，Solidity 0.8.0+支持重载构造函数（但实际部署时仅一个生效）。

```
// 构造函数示例
pragma solidity ^0.8.20;

contract ConstructorExample {
    address public owner;
    uint256 public initValue;
    
    // 基础构造函数
    constructor(uint256 _initValue) {
        owner = msg.sender; // 部署者为所有者
        initValue = _initValue; // 初始化变量
    }
    
    // 仅所有者可调用的函数（配合修饰器使用，下文会讲）
    function changeInitValue(uint256 _newValue) public onlyOwner {
        initValue = _newValue;
    }
    
    // 修饰器：检查调用者是否为所有者
    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner");
        _; // 执行函数体
    }
}
```

## 2\. 修饰器（Modifier）

修饰器用于修改函数的行为，可在函数执行前/后添加逻辑（如权限检查、参数验证、状态判断），减少代码冗余。修饰器中的“\_;”表示函数体的执行位置。

```
// 修饰器示例
pragma solidity ^0.8.20;

contract ModifierExample {
    address public owner;
    bool public isContractActive = true;
    
    constructor() {
        owner = msg.sender;
    }
    
    // 权限检查修饰器
    modifier onlyOwner() {
        require(msg.sender == owner, "Caller is not owner");
        _;
    }
    
    // 状态检查修饰器
    modifier contractActive() {
        require(isContractActive, "Contract is not active");
        _;
    }
    
    // 带参数的修饰器：检查金额是否大于最小值
    modifier minAmount(uint256 _minAmount) {
        require(msg.value >= _minAmount, "Amount is too small");
        _;
    }
    
    // 组合修饰器：需同时满足多个条件
    function stopContract() public onlyOwner contractActive {
        isContractActive = false;
    }
    
    // 带参数修饰器的函数
    function deposit() public payable minAmount(1 ether) contractActive {
        // 接收ETH，满足至少1 ETH的条件
    }
    
    // 函数执行后执行修饰器逻辑
    modifier logAfterExecution() {
        _; // 先执行函数体
        emit ExecutionLogged(msg.sender, block.timestamp); // 函数执行后触发事件
    }
    
    event ExecutionLogged(address indexed caller, uint256 timestamp);
    
    function testLog() public logAfterExecution {
        // 函数逻辑
    }
}
```

# 十一、时间（Time）

Solidity提供全局时间变量，用于获取区块链上的时间信息，单位为秒（uint256类型），基于区块的时间戳。

## 常见时间变量

-   block.timestamp：当前区块的时间戳（自1970年1月1日以来的秒数）。
    
-   now：block.timestamp的别名，0.8.20+版本仍支持，但推荐使用block.timestamp。
    

注意：时间戳可被矿工轻微篡改（误差通常在几秒到几分钟），不可用于高精度时间场景。

```
// 时间示例
pragma solidity ^0.8.20;

contract TimeExample {
    uint256 public startTime;
    uint256 public duration = 1 days; // 1天 = 86400秒，内置常量：minutes、hours、days、weeks、years
    
    constructor() {
        startTime = block.timestamp; // 记录合约部署时间
    }
    
    // 检查是否超过有效期
    function isExpired() public view returns (bool) {
        return block.timestamp >= startTime + duration; // 部署后1天过期
    }
    
    // 计算剩余时间
    function getRemainingTime() public view returns (uint256) {
        if (isExpired()) {
            return 0;
        }
        return startTime + duration - block.timestamp;
    }
    
    // 时间转换：秒转天、时、分、秒
    function formatTime(uint256 _seconds) public pure returns (uint256 days, uint256 hours, uint256 minutes, uint256 secs) {
        days = _seconds / 86400;
        _seconds %= 86400;
        hours = _seconds / 3600;
        _seconds %= 3600;
        minutes = _seconds / 60;
        secs = _seconds % 60;
    }
}
```

# 十二、继承（Inheritance）

Solidity支持单继承和多继承（通过线性化解决歧义），子类可继承父类的状态变量、函数（除private外），并可重写父类函数（需用virtual和override关键字）。继承可实现代码复用和逻辑分层。

## 核心关键字

-   is：子类继承父类时使用，格式：contract 子类 is 父类1, 父类2...。
    
-   virtual：父类函数标记为可被重写。
    
-   override：子类重写父类virtual函数时使用。
    
-   super：调用父类的函数或构造函数。
    

```
// 继承示例
pragma solidity ^0.8.20;

// 父类（基础合约）
contract ParentContract {
    address public owner;
    uint256 public parentNum = 10;
    
    constructor() {
        owner = msg.sender;
    }
    
    // 可被重写的函数
    function getMessage() public virtual view returns (string memory) {
        return "This is parent contract";
    }
    
    // 仅父类及子类可访问的函数
    function parentOnlyFunction() internal view returns (uint256) {
        return parentNum * 2;
    }
}

// 子类，继承父类
contract ChildContract is ParentContract {
    uint256 public childNum = 20;
    
    // 重写父类构造函数（可选）
    constructor() {
        // 无需重复初始化owner，父类构造函数已执行
        childNum = 30;
    }
    
    // 重写父类virtual函数
    function getMessage() public override view returns (string memory) {
        return "This is child contract";
    }
    
    // 调用父类函数
    function callParentFunction() public view returns (string memory, uint256) {
        string memory parentMsg = super.getMessage(); // 调用父类getMessage
        uint256 parentVal = parentOnlyFunction(); // 调用父类internal函数
        return (parentMsg, parentVal);
    }
    
    // 组合父类和子类变量
    function getTotalNum() public view returns (uint256) {
        return parentNum + childNum;
    }
}

// 多继承示例（需注意线性化顺序）
contract GrandParent {
    function getGrandParentMsg() public virtual view returns (string memory) {
        return "GrandParent";
    }
}

contract Parent is GrandParent {
    function getGrandParentMsg() public override view returns (string memory) {
        return "Parent override GrandParent";
    }
}

contract Child is Parent, GrandParent {
    // 多继承时，override需指定所有父类
    function getGrandParentMsg() public override(Parent, GrandParent) view returns (string memory) {
        return "Child override";
    }
}
```

# 十三、抽象合约和接口

## 1\. 抽象合约（Abstract Contract）

包含至少一个未实现函数（无函数体，仅声明）的合约，无法直接部署，需被子类继承并实现所有未实现函数。抽象合约可包含已实现的函数和状态变量，用于定义基础逻辑和规范。

```
// 抽象合约示例
pragma solidity ^0.8.20;

// 抽象合约
abstract contract AbstractContract {
    uint256 public baseNum = 100;
    
    // 已实现函数
    function getBaseNum() public view returns (uint256) {
        return baseNum;
    }
    
    // 未实现函数，需子类实现
    function calculate(uint256 a, uint256 b) public virtual pure returns (uint256);
    
    // 带修饰器的未实现函数
    function updateValue(uint256 _value) public virtual onlyValidValue;
    
    modifier onlyValidValue() {
        require(_value > 0, "Value must be positive");
        _;
    }
}

// 子类实现抽象合约
contract ConcreteContract is AbstractContract {
    // 实现抽象合约的calculate函数
    function calculate(uint256 a, uint256 b) public override pure returns (uint256) {
        return a * b;
    }
    
    // 实现抽象合约的updateValue函数
    function updateValue(uint256 _value) public override onlyValidValue {
        baseNum = _value;
    }
}
```

## 2\. 接口（Interface）

接口是特殊的抽象合约，仅包含函数声明（无函数体、无状态变量、无构造函数、无修饰器），所有函数默认为external和virtual。接口用于定义合约交互规范，子类必须实现接口的所有函数，也可用于跨合约调用。

```
// 接口示例
pragma solidity ^0.8.20;

// 定义接口
interface IERC20 {
    // 函数声明，无函数体
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    
    // 事件声明
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// 实现接口
contract MyERC20 is IERC20 {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    
    constructor(uint256 initialSupply) {
        _totalSupply = initialSupply;
        _balances[msg.sender] = initialSupply;
    }
    
    // 实现接口函数
    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }
    
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        require(recipient != address(0), "Invalid recipient");
        require(_balances[msg.sender] >= amount, "Insufficient balance");
        
        _balances[msg.sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }
    
    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }
    
    function approve(address spender, uint256 amount) public override returns (bool) {
        require(spender != address(0), "Invalid spender");
        
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }
    
    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        require(sender != address(0), "Invalid sender");
        require(recipient != address(0), "Invalid recipient");
        require(_balances[sender] >= amount, "Insufficient balance");
        require(_allowances[sender][msg.sender] >= amount, "Insufficient allowance");
        
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        _allowances[sender][msg.sender] -= amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }
}
```

# 十四、异常（Exceptions）

异常用于处理合约执行中的错误场景（如参数非法、权限不足、余额不足），触发异常后，交易回滚（所有状态修改撤销），并消耗已产生的gas。Solidity提供多种异常处理方式。

## 常见异常处理方法

-   require()：最常用，用于输入验证、权限检查，异常时返还剩余gas（0.8.0+版本），语法：require(条件, 错误信息)。
    
-   revert()：手动触发异常，可自定义错误信息，语法：revert(错误信息)，或配合自定义错误类型。
    
-   assert()：用于内部逻辑检查（如 invariants），异常时不返还剩余gas，语法：assert(条件)，通常用于确保代码逻辑正确性。
    
-   自定义错误（Custom Errors）：0.8.4+支持，更省gas，可传递参数。
    

```
// 异常示例
pragma solidity ^0.8.20;

contract ExceptionExample {
    mapping(address => uint256) public balances;
    
    // 自定义错误，可传递参数
    error InsufficientBalance(address caller, uint256 balance, uint256 required);
    error InvalidAddress(address invalidAddr);
    
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }
    
    // 使用require()
    function withdrawWithRequire(uint256 _amount) public {
        require(_amount > 0, "Amount must be positive");
        require(balances[msg.sender] >= _amount, "Insufficient balance");
        
        balances[msg.sender] -= _amount;
        payable(msg.sender).transfer(_amount);
    }
    
    // 使用revert()和自定义错误
    function withdrawWithRevert(uint256 _amount) public {
        if (_amount <= 0) {
            revert("Amount must be positive");
        }
        if (balances[msg.sender] < _amount) {
            revert InsufficientBalance(msg.sender, balances[msg.sender], _amount); // 自定义错误传参
        }
        
        balances[msg.sender] -= _amount;
        payable(msg.sender).transfer(_amount);
    }
    
    // 使用assert()
    function transfer(address _recipient, uint256 _amount) public {
        require(_recipient != address(0), "Invalid recipient");
        require(balances[msg.sender] >= _amount, "Insufficient balance");
        
        uint256 oldSenderBalance = balances[msg.sender];
        balances[msg.sender] -= _amount;
        balances[_recipient] += _amount;
        
        // 内部逻辑检查：确保转账后总额不变
        assert(balances[msg.sender] + balances[_recipient] == oldSenderBalance + balances[_recipient] - _amount + _amount);
    }
    
    // 自定义错误的捕获（外部调用时）
    function testCustomError(address _recipient) public view {
        if (_recipient == address(0)) {
            revert InvalidAddress(_recipient);
        }
    }
}
```
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
# 2026-07-06

# Monad 研究笔记

> 整理时间：2026年7月 | 信息来源：[monad.xyz](http://monad.xyz) 官网、Monad 官方文档、Ledger、Gate、Bitget 等公开资料

* * *

## 一、项目定位：这是做什么的

Monad 是一条**高性能、EVM 完全兼容的 Layer 1 公链**。它的核心命题不是"发明一套全新的公链范式"，而是**在保持以太坊虚拟机（EVM）字节码兼容的前提下，重写节点底层的共识与执行引擎**，从而把吞吐量提升到能与 Solana 等非 EVM 高性能公链竞争的水平。

简单说：Solidity 合约、EVM 地址体系、钱包、开发工具（Hardhat、Foundry 等）在 Monad 上可以"零改动或极低改动"直接部署，但链的运行速度和成本体验接近 Solana。

**关键节点**：

-   2025年2月19日：测试网上线
    
-   2025年11月24日：公开主网正式上线，同步完成原生代币 $MON 的 TGE（无传统 ICO，通过主网上线时的代币生成事件发放）
    
-   主网上线前估值约 30 亿美元
    

* * *

## 二、核心技术架构（这是 Monad 真正的护城河)

Monad 的性能提升并非依赖单一黑科技，而是**共识层、执行层、网络层、存储层的协同重构**，官方文档中提到五大创新方向，核心是以下几项：

### 1\. MonadBFT(共识机制)

一种"流水线化"的拜占庭容错（BFT）共识协议，用于解决传统 BFT 共识中的"尾部分叉"（tail-forking）问题，目标是实现单槽终局性（single-slot finality）——交易一旦被打包进区块即视为最终确认，无需像以太坊那样等待多个区块确认。

### 2\. 异步执行(Asynchronous Execution)

Monad 把"排序（共识）"和"执行"两个阶段解耦：网络先对交易顺序达成共识，执行在此之后异步进行，不阻塞下一批交易的共识流程。这是经典计算机体系结构中的流水线（pipelining）思想被引入到区块链场景。

### 3\. 乐观并行执行(Optimistic Parallel Execution)

这是 Monad 最核心的性能来源。在网络对交易顺序达成一致后，Monad 会调用所有可用的 CPU 核心并行处理多笔交易，然后按照约定好的顺序应用结果，从而在不改变最终状态的前提下避免完全串行执行。

与以太坊的本质区别在于：以太坊在共识完成后仍然逐笔串行执行交易，哪怕节点硬件有多个 CPU 核心可用，也只能同时运行一笔交易，这也是网络在高峰期变慢的原因。Monad 通过识别交易之间是否存在数据依赖关系，将无冲突的交易并行执行，冲突的交易再回退（rollback）重新按序处理——这也是"乐观"（optimistic）二字的由来。

需要注意：Monad 的交易和区块顺序依然是严格线性排列的，Monad 只是在这个线性顺序内部识别出可以并行执行而不影响结果的交易，这保证了对开发者透明——用以太坊写的应用部署到 Monad 上行为不变。

### 4\. MonadDB(自研状态数据库)

一套专为区块链场景优化的存储引擎，支持高速随机读写和"瞬态状态"（transient state），允许大量交易在最终原子提交前并行推进。文档中强调，没有这一层，"并行执行在实践中根本无法成立"。

### 5\. RaptorCast(网络广播层)

用于提升区块和交易在验证者网络间的传播效率，是支撑高 TPS 的网络层基础设施。

* * *

## 三、性能指标与去中心化设计

| 指标 | Monad 官方目标/宣传值 | 说明 |
| --- | --- | --- |
| TPS | ~10,000（约 300M gas/秒） | 官网及多方资料给出的目标值 |
| 出块时间 | ~0.4秒 | — |
| 终局确认 | ~0.8秒（单槽终局） | 无需多区块确认 |
| EVM 兼容性 | 字节码级完全兼容 | Solidity/Vyper 合约可直接部署 |

**去中心化设计的差异化卖点**：Monad 强调"性能提升来自软件架构而非硬件堆料"，即验证者可以用消费级硬件参与网络，而不是只有拥有数据中心级服务器的机构才能运行节点。这是 Monad 试图打破"性能-去中心化-安全性不可能三角"的核心叙事——多数高性能链（如 Solana）为了速度牺牲了硬件门槛（导致验证者集中化),而 Monad 希望用架构创新而非提高硬件门槛来实现高吞吐。

⚠️ 需要保持的怀疑态度：目前官网首页统计的"验证者数量""EVM兼容度"等数字展示为动态占位符（页面显示为 0，实际数字需要在 [gmonads.com](http://gmonads.com) 等页面查看真实值),说明网络仍处于早期成长阶段,规模尚未完全展现。

* * *

## 四、生态与落地情况

**主网早期数据**（截至资料整理时）：

-   主网上线首日处理了370万笔交易,模因币（memecoin）发行曾使 TPS 冲高至 350+,DeFi 总锁仓量（TVL）在两周内突破 2.45 亿美元
    

**代表性生态合作/进展**：

-   **Aave V3** 已在 Monad 部署,首期支持包括 USDC、GHO、wstETH 在内的 12 种资产,Monad 基金会承诺提供 1500 万美元的首年流动性激励
    
-   **MetaMask** 于 2026年7月宣布基于 Monad 推出"Money Account"生息账户功能,并集成 Mastercard 用于消费
    
-   机构化探索：Franklin Templeton 宣布计划在 Monad 上扩展其现实世界资产（RWA）业务;有报道提及在 Monad 上试点韩国国债代币化,用于实现无需韩国券商账户的次秒级结算(此类机构合作多为早期试点,信息以官方公告为准)
    
-   生态类别覆盖 DeFi、NFT、游戏、支付、跨链桥、预言机(Chainlink、Pyth 等)
    

* * *

## 五、代币经济（$MON）

-   总供应量:1000 亿枚 $MON,无传统 ICO,通过 2025年11月主网上线时的 TGE 发放
    
-   已上线 Coinbase、Bybit 等主流交易所
    
-   代币解锁是市场关注的风险点之一:2026年上半年有多轮解锁事件,部分分析认为解锁后短期内价格易承压
    
-   币价在 2026 年上半年整体呈现较大区间波动(不同资料给出约 0.027–0.043 美元的震荡区间),这部分**纯属市场行情信息,不构成投资建议**
    

* * *

## 六、优势总结

1.  **兼容性零摩擦**:字节码级 EVM 兼容,现有以太坊合约、审计记录、开发工具几乎可以直接迁移,大幅降低开发者迁移成本——这是相较于 Solana、Sui 等非 EVM 高性能链的最大差异化优势
    
2.  **架构性能提升**:通过并行执行 + 异步执行 + 专用数据库 + 优化共识的组合创新,理论吞吐和终局速度相较以太坊有数量级提升
    
3.  **去中心化叙事**:强调消费级硬件即可运行验证节点,试图在"性能"与"去中心化"之间找到新的平衡点,而非像多数高性能链那样依赖少数高配节点
    
4.  **开发者与机构双线布局**:既通过黑客马拉松、加速器(如 Mach Accelerator)吸引原生开发者,也在通过 RWA、机构级资产结算尝试打开传统金融的合作空间
    

## 七、需要关注的风险与争议点

1.  **技术成熟度尚待验证**:部分技术社区人士仍持谨慎态度,认为乐观并行执行在极端 MEV 或高竞争状态场景下的回滚率仍需要在主网环境下验证,当前宣传的万级 TPS 数字可能更多反映的是理想化测试环境而非真实生产条件下的表现
    
2.  **去中心化尚未经过时间检验**:验证者规模、地理分布、长期经济安全性仍是新兴 L1 的通病,需要观察网络成长后是否维持"低硬件门槛"的初衷
    
3.  **代币解锁压力**:较大比例的代币仍处于锁仓/分批释放阶段,可能持续对二级市场价格构成压力
    
4.  **竞争格局激烈**:EVM 高性能赛道(以及以太坊 L2、Solana)同时在演进,Monad 需要持续证明"EVM 兼容 + 高性能"组合相较其他方案的护城河足够深
    

* * *

## 八、一句话总结

Monad 试图回答的问题是:**能不能在不放弃 EVM 生态(开发者、工具、审计、流动性)的前提下,做出一条媲美 Solana 速度的公链?** 它的答案是通过共识与执行解耦、乐观并行执行、专用存储引擎的系统性架构创新,而不是简单复制 Solana 的技术栈或牺牲 EVM 兼容性。目前主网仅上线约8个月,生态和机构合作处于早期验证阶段,技术承诺(万级 TPS、去中心化验证)最终仍需生产环境的长期数据来检验。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
