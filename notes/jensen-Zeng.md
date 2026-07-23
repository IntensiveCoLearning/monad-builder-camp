---
timezone: UTC+8
---

# jensen-Zeng

**GitHub ID:** jensen-Zeng

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
**solidity就是用代码定义区块链上的资产、数据和交易规则**。合约一旦部署，修改成本高，而且每次写入链上数据通常都需要支付 Gas。

## 1\. Solidity 合约的基本结构

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Counter {
    uint256 public count;

    constructor() {
        count = 0;
    }

    function add() public {
        count += 1;
    }

    function getCount() public view returns (uint256) {
        return count;
    }
}
```

这段代码有几个基础部分：

-   `pragma solidity ^0.8.20`：指定编译器版本。
    
-   `contract Counter`：定义一个名为 `Counter` 的智能合约。
    
-   `count`：保存在区块链上的状态变量。
    
-   `constructor`：合约部署时执行一次。
    
-   `add()`：修改链上状态。
    
-   `getCount()`：读取链上状态。
    

其中，`public` 状态变量会自动生成查询函数，因此实际上可以直接调用 `count()`，不一定需要额外写 `getCount()`。

## 2\. 状态变量和局部变量

状态变量会永久保存在区块链上：

```
uint256 public count;
```

局部变量只在函数执行期间存在：

```
function calculate() public pure returns (uint256) {
    uint256 result = 10 + 20;
    return result;
}
```

保存状态变量需要链上存储空间，因此通常比使用局部变量昂贵。

## 4\. view、pure 和 payable

### view读取状态，但不修改状态：

```
function getCount() public view returns (uint256) {
    return count;
}
```

### pure既不读取状态，也不修改状态：

```
function addNumbers(
    uint256 a,
    uint256 b
) public pure returns (uint256) {
    return a + b;
}
```

### payable允许函数接收原生代币，例如 ETH 或 Monad 测试币：

```
function deposit() public payable {
}
```

用户调用时可以向合约发送资金。

## 6\. 数组

### 固定长度数组

```
uint256[3] public numbers;
```

长度固定为 3。

### 动态数组

```
uint256[] public numbers;

function addNumber(uint256 number) public {
    numbers.push(number);
}
```

常见操作：

```
numbers.push(10);
numbers.pop();
numbers.length;
numbers[0];
```

## 7\. mapping 映射

`mapping` 类似 Python 字典或 Java Map。

```
mapping(address => uint256) public balances;
```

表示每个地址对应一个余额。

```
function setBalance(uint256 amount) public {
    balances[msg.sender] = amount;
}
```

读取：

```
uint256 myBalance = balances[msg.sender];
```

mapping 不能直接遍历，也无法直接获取全部 key。如果需要遍历，通常还要额外维护地址数组。

## 8\. struct 结构体

结构体用于组合多个字段。

```
struct Student {
    string name;
    uint256 age;
    bool active;
}

Student public student;
```

创建结构体：

```
function setStudent(string memory name, uint256 age) public {
    student = Student({
        name: name,
        age: age,
        active: true
    });
}
```

* * *

## 9\. enum 枚举

枚举用于表示有限状态。

```
enum Status {
    Pending,
    Approved,
    Rejected
}

Status public status;
```

修改状态：

```
function approve() public {
    status = Status.Approved;
}
```

枚举底层实际上会被转换成整数：

-   `Pending` 为 0
    
-   `Approved` 为 1
    
-   `Rejected` 为 2
    

## 10\. memory、storage 和 calldata

这是 Solidity 初学者非常容易混淆的部分。

### storage

数据永久保存在链上。

```
string public message;
```

状态变量默认使用 `storage`。

### memory

数据只在函数执行期间临时存在。

```
function setMessage(string memory newMessage) public {
    message = newMessage;
}
```

### calldata

只读参数，主要用于 `external` 函数，通常比 `memory` 更节省 Gas。

```
function setMessage(string calldata newMessage) external {
    message = newMessage;
}
```

可以简单理解为：

-   `storage`：链上硬盘。
    
-   `memory`：运行内存。
    
-   `calldata`：调用函数时传入的只读参数。
    

## 11\. 条件判断和循环

### if 判断

```
function checkAge(uint256 age) public pure returns (bool) {
    if (age >= 18) {
        return true;
    } else {
        return false;
    }
}
```

### for 循环

```
function sum(uint256 n) public pure returns (uint256) {
    uint256 result = 0;

    for (uint256 i = 1; i <= n; i++) {
        result += i;
    }

    return result;
}
```

链上循环需要特别谨慎。如果循环次数过多，可能消耗大量 Gas，甚至导致交易无法执行。

## 12\. require、revert 和 assert

### require用于检查用户输入、权限和执行条件：

```
require(msg.sender == owner, "Not owner");
```

条件不满足时，交易回滚。

### revert主动终止执行：

```
if (msg.sender != owner) {
    revert("Not owner");
}
```

### assert主要用于检查理论上不应该失败的内部逻辑：

```
assert(count >= 0);
```

由于 `uint` 本身不会小于 0，这个示例只是说明语法。一般业务条件检查优先使用 `require` 或自定义错误。

## 13\. 权限控制与 modifier

可以通过修饰器复用权限检查逻辑。

```
contract AdminContract {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    function reset() public onlyOwner {
        // 只有 owner 可以执行
    }
}
```

## 14\. 事件 Event

事件用于记录链上操作，方便前端和区块浏览器监听。

```
event MessageChanged(
    address indexed user,
    string newMessage
);

function setMessage(string memory newMessage) public {
    message = newMessage;
    emit MessageChanged(msg.sender, newMessage);
}
```

事件适合记录：

-   谁执行了操作；
    
-   修改了什么内容；
    
-   转账金额；
    
-   订单状态变化；
    
-   NFT 铸造情况。
    

`indexed` 字段可以被前端或区块浏览器快速检索。
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->

## 黑客松是什么？

**黑客松（Hackathon）**作为一种限时开发活动。参赛者通常在几小时到几周内组队，围绕指定技术或主题，完成一个可以演示的产品原型。

基本流程通常是：**报名与组队 → 选择赛道 → 确定问题 → 开发原型 → 提交代码和演示视频 → 路演答辩 → 评审颁奖。**

评委关注创意、技术实现、完成度、实际价值和现场展示效果。Devpost 是目前较常见的线上黑客松发布与作品提交平台。

### 当前的AI黑客松

使用大模型、Agent、计算机视觉、语音或机器学习技术开发应用。

常见项目包括：多智能体协作系统、AI 旅游规划、教育或医疗助手、文档分析和自动化办公工具、AI 编程、数据分析工具

Devpost 长期设有 AI 黑客松专区。
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->


### 什么是BTC？

Bitcoin作为一种去中心化数字资产，在08年金融危机之后，见识到主权货币的不可靠之后由匿名人士提出。它不由某家公司、银行或政府发行，而是通过全球计算机组成的点对点网络共同维护。

### 它是怎么运行的？

比特币的每一笔交易会被记录在公开的区块链上。矿工通过计算完成“工作量证明”，竞争打包新区块，并获得区块奖励和交易手续费。区块之间通过哈希连接，修改历史交易需要改动50%以上的区块，其成本高到不可承受。

### BTC的可靠来源于哪里？

-   **稀缺性**：发行上限明确，不能由某个机构任意增发。
    
-   **去中心化**：没有单一服务器或公司负责记账。
    
-   **跨境转移**：持有私钥的人可以直接控制和转移资产。
    
-   **市场共识**：起初只是极客之间的实验，但近年来交易所、机构投资者和部分上市金融产品已经参与BTC的投资。
    
-   **“数字黄金”**：部分人将BTC视为对抗通货膨胀的价值储存工具。
    

### BTC的风险点

BTC 的价格及其不稳定，可能在较短时间内出现大幅上涨或下跌，虽然供应有限，但价格高度依赖“共识”。

丢失风险高，自托管钱包依靠私钥或助记词控制资产；一旦丢失，无法找回。而交易所托管与普通银行无异，甚至交易所倒闭风险大得多。

BTC最重要的特性和缺点都是交易不可逆。如果产生错误交易，无法撤销，只能依赖收款方主动退回。
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->



### 一、 智能合约做什么

智能合约上是一段代码，用于自动处理链上产生交易时和之后需要执行的规则，例如发行、转移和管理代币、去中心化金融（DeFi）、去中心化组织（DAO）等等。

### 二、 智能合约的部署

部署智能合约就是将编译好的代码写入区块链的过程，通常包含步骤如下：

1.  使用 Solidity 或 Vyper 等编程语言编写合约代码。
    
2.  编译成字节码和 ABI。Bytecode是实际部署到链上的机器码，ABI是前端和外部程序调用合约函数的“说明书”。
    
3.  使用节点服务连接到以太坊主网或测试网。
    
4.  使用钱包支付 Gas 费，将合约发布到链上，成功后会生成唯一的合约地址，代表部署成功。
    

### 三、 与智能合约的交互

与智能合约交互就是调用合约中的函数（如读取代币余额或转账），主要通过dApp进行。
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->




Web3技术的就业方向如下  
**1\. 智能合约开发**：负责编写、测试和部署运行在区块链上的智能合约（相当于后端程序）。需要掌握 Solidity、Rust 等编程语言；熟悉以太坊虚拟机（EVM）、Solana 等链的生态系统。

**2\. 区块链协议与基础建设开发**：负责构建和维护区块链底层架构、共识机制、跨链桥及节点基础设施。需要熟悉底层密码学、Go、Rust 或 C++；了解以太坊客户端开发或 Layer2（如 ZK-Rollups）技术。

**3\. 全栈开发：**负责构建与用户直接交互的去中心化应用程序（dApp）界面，连接用户钱包并与智能合约进行交互。需要掌握传统前端技术（React, Vue）；熟悉 Web3.js、Ethers.js 或 viem 等库，以及 MetaMask 等钱包的集成。

**4\. 区块链安全与审计（今日coleaning，类网络安全方向）：**专门对智能合约和底层协议进行安全检查和代码审计，查找漏洞以防止黑客攻击，保障资金安全。需精通多种区块链语言，具备极强的代码逆向工程能力、逻辑思维和攻防经验。

**5\. DevOps 与节点运维：**负责区块链节点的搭建、维护，以及自动化部署工具的开发，确保 Web3 服务的稳定性和高可用性。需要熟悉 Docker、K8s 等云服务。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->





Monad合约的优势：  
1、高吞吐量的同时具备较短的的出块时间和极短的交易确认时间。  
2、交易费用极其低廉，由于其底层技术优化，计算和存储效率大幅提高。  
3、完全兼容以太坊虚拟机，以太坊上的合约、钱包、开发工具和代码可以直接迁移，开发者重写代码。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->






记录今天学习遇到的问题：  
在remix中编辑合约时，连接钱包应该选Browser Extension还是WalletConnect？  
metamask中添加monad测试网络后不显示，需要在设置中打开“显示测试网络”，但仍未找到。  
连接钱包后，需要切换到monad测试网络，才能在测试网络中部署，但仍未找到怎么切换

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/jensen-Zeng/images/2026-07-13-1783952407420-image.png)
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->







### prompt

生成一个最小的，有留言功能的板Solidity 合约

### 合约源码：

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

contract MessageBoard {

string private message;

address public lastAuthor;

uint256 public updatedAt;

event MessageUpdated(

address indexed author,

string newMessage,

uint256 timestamp

);

constructor(string memory initialMessage) {

require(bytes(initialMessage).length > 0, "Message cannot be empty");

message = initialMessage;

lastAuthor = msg.sender;

updatedAt = block.timestamp;

emit MessageUpdated(msg.sender, initialMessage, block.timestamp);

}

function setMessage(string calldata newMessage) external {

require(bytes(newMessage).length > 0, "Message cannot be empty");

require(bytes(newMessage).length <= 280, "Message too long");

message = newMessage;

lastAuthor = msg.sender;

updatedAt = block.timestamp;

emit MessageUpdated(msg.sender, newMessage, block.timestamp);

}

function getMessage() external view returns (string memory) {

return message;

}

}

### 人工判断该合约

1、留言不能为空，需要加入了 require 检查，避免用户提交空内容。

2、链上存储字符串会消耗 Gas，必须限制留言长度。

3、保留了 MessageUpdated 事件，用于在区块浏览器中查看每次留言更新的记录。

### AI 生成代码需要人工检查的地方

1、基础的变量、事件、构造函数。

2、是否符合用户需求。

3、边界条件与资金风险
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->








Solidity合约就是用于自动处理web3中各种需求的自动化程序，相当于后端。  
但是合约的优势在于自动运行，对链上所有人透明可见。

基于此，Web3中就有了和Web2一样构建各种应用的基础，比如游戏、NFT、社区共建等等
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->









区块链一笔交易经历的过程：

1、用户在钱包中发起交易，填写接收地址、转账金额、Gas 费用等信息

2、钱包使用私钥对交易签名，证明这笔交易由该地址的控制者真实发起

3、签名后的交易会被发送到区块链网络中的节点，所有节点会检查交易是否合法

4、验证通过的交易会矿工选中，并打包进新的区块

5、利用共识机制确认新区块的有效性

6、区块被确认后，交易状态会变为成功或失败，并永久记录在链上，交易完成
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->










### Web3的价值

Web2产品的数据由平台集中管理，用户仅拥有使用权；而链上产品的数据在区块上，用户完全掌握数字资产的所有权。

传统应用依靠企业信誉或权威机构背书；链上产品则依靠所有区块的共识机制产生信任。

链上产品的交易记录对全网节点公开透明，具有高度可追溯性；Web2数据则由平台控制披露与否。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->











# 新进入Web3世界

安装metamask，创建自己的钱包地址，领取测试币并尝试测试交易。

参加线上活动「DevRel 的成长之路 —— 从 Builder 到生态连接者」线上活动和co-learning，了解到了真实Web3的行业现状和职业发展

学习区块链基础概念：去中心化、BTC、公钥私钥的区别、公链与私有链的区别。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
