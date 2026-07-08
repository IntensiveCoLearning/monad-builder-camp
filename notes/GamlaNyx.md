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
