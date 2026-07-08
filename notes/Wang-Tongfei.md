---
timezone: UTC+8
---

# Tongfei

**GitHub ID:** Wang-Tongfei

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
## **Day 3 —— 编写并部署第一个 Solidity 智能合约（OnchainTodo）**

工具：Remix IDE、MetaMask、Monad Testnet、Solidity

### **一、学习目标**

本次学习的目标：

了解 Solidity 智能合约的基本结构

编写一个简单的 Todo List 合约

学习智能合约的读写函数（Read / Write Function）

使用 Remix 编译并部署合约

在测试网上验证合约并进行交互

### **二、实践内容**

本次实现了一个简单的 **OnchainTodo** 合约。

主要功能包括：

添加待办事项（Add Todo）

修改完成状态（Toggle Completed）

查询待办事项（Get Todo）

获取 Todo 数量（Get Todo Count）

代码中使用了以下 Solidity 基础知识：

Contract

Struct

Dynamic Array

State Variable

Event

Require

View Function

### **三、核心知识**

**1\. Contract**

Solidity 使用 contract 定义智能合约。

contract OnchainTodo {

}

可以理解为 Java 中的 Class，只不过部署后运行在区块链上。

**2\. Struct**

使用 Struct 定义 Todo 数据结构。

struct Todo {

string task;

bool completed;

}

每个 Todo 包含：

-   task：任务内容
    
-   completed：完成状态
    

**3\. Dynamic Array**

Todo\[\] private todos;

动态数组用于保存所有 Todo。

调用 addTodo() 时，会向数组末尾新增一个 Todo。

**4\. Read Function**

getTodo()

getTodoCount()

读取链上数据。

特点：

-   不修改区块链
    
-   使用 view
    
-   一般不消耗 Gas（本地调用）
    

**5\. Write Function**

addTodo()

toggleCompleted()

修改链上数据。

特点：

-   会产生交易（Transaction）
    
-   需要用户签名
    
-   消耗 Gas
    

**6\. Event**

event TodoAdded(...);

event TodoUpdated(...);

事件用于记录链上日志。

部署后，前端可以监听 Event 获取状态变化。

### **四、实践过程**

**1\. 编写合约**

创建文件：

OnchainTodo.sol

使用 Solidity 0.8.20 编写 Todo 合约。

2\. 编译合约

在 Remix 中：

-   Solidity Compiler
    
-   Compiler Version：0.8.x
    
-   Compile OnchainTodo.sol
    

编译成功，没有 Error。

**3\. 部署合约**

进入：

Deploy & Run Transactions

连接 MetaMask。

网络：

Monad Testnet

点击：

Deploy

钱包确认交易后，合约成功部署。

**4\. 调用 Write Function**

添加 Todo：

addTodo("Finish Assignment")

交易成功。

随后调用：

toggleCompleted(0)

成功修改完成状态。

**5\. 调用 Read Function**

查询 Todo：

getTodo(0)

返回：

task = Finish Assignment

completed = true

查询数量：

getTodoCount()

返回：

1

### **五、部署结果**

部署网络：

Monad Testnet

合约名称：

OnchainTodo

编译器：

solc 0.8.34

源码验证：

Sourcify Verified

说明部署的源码与链上的 Bytecode 完全一致，可以在区块链浏览器查看和验证。

### **六、学习收获**

通过本次实践，我了解了 Solidity 智能合约开发的基本流程：

-   编写 Solidity 合约。
    
-   使用 Remix 编译。
    
-   部署到测试网络。
    
-   调用写函数修改链上数据。
    
-   调用读函数读取链上数据。
    
-   完成源码验证，确保链上部署的代码可公开验证。
    

同时，也进一步理解了智能合约与传统程序的区别：智能合约部署到区块链后，其状态由链维护，写操作需要发起交易并支付 Gas，而读操作通常可以直接查询，不会改变链上状态。这种模式保证了数据的公开性、透明性和不可篡改性。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

### **前置准备｜进入 Web3 与链上世界**

**Summary**

1.  创建或准备一个**课程专用钱包**，不要使用主力钱包。
    
2.  添加 Monad Testnet 测试网络。
    
3.  获取测试网资产，俗称“取水”。
    
4.  打开 Monad Explorer 或相关区块浏览器，可通过账户地址查询个人交易详情。
    

**Specific operations**

1.  拓展插件Metamask
    

2.  创建钱包
    

Metamask创建钱包，并记住私钥

3.  添加 Monad Testnet 测试网络
    

![bfae7d7454727fd78127d9b2e2b26ea8.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783429910381-bfae7d7454727fd78127d9b2e2b26ea8.png)

4.  获取测试币，俗称“取水”
    

![878fab6dac3d34dcc22dd41ff2003d54.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783429969137-878fab6dac3d34dcc22dd41ff2003d54.png)

**ps. 24h内每天上限50Mon**

5.  用 Monad Explorer 查询你的课程钱包地址
    

![d9c3b1d5a43fd8684d7fe5f8595c0568.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783430245323-d9c3b1d5a43fd8684d7fe5f8595c0568.png)![b8c56d1f57c0850d75af789fddb35f50.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Wang-Tongfei/images/2026-07-07-1783430196336-b8c56d1f57c0850d75af789fddb35f50.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->


**一、Day1整体学习主线**

1. Web3入门实操分三步：搭建Mod测试网钱包、链上取水获Mon测试币、AI生成部署智能合约。

2. 智能合约是链上不可篡改自动执行程序，留言板写入类操作消耗Gas，查询类操作免费。

**二、公私钥核心区别**

1. 私钥：掌控资产唯一凭证，必须保密，泄露资产会被盗。

2. 公钥：钱包公开地址，可对外分享，用于收款、查链上数据。

**三、Gas费相关要点**

1. Gas为链上操作燃料费，计算方式：Gas消耗量×油价。

2. 操作越复杂、网络越拥堵，Gas费用越高。

3. 合约留言写入消耗Gas，读取留言无Gas成本。

4. 每次留言是一笔上链交易，必耗Gas；留言文字越多，单笔Gas越高。

5. 合约Gas消耗可在Remix查看，受代码写法、链网络情况影响。

**四、Mod测试网钱包操作流程**

1. 火狐浏览器安装MetaMask，创建钱包妥善保管私钥。

2. 钱包添加Mod Test网络，复制公钥去水龙头领测试币，每日限一次。

3. 区块浏览器查询余额，即为钱包任务完成校验标准。

**五、链上交易要点**

1. 互转测试币后留存交易哈希，所有链上记录永久留存、不可篡改。

2. 区块浏览器可查看交易收发地址、金额、Gas消耗全部信息。

**六、留言板智能合约部署流程**

1. GPT生成Solidity合约代码，Remix粘贴自动编译。

2. Remix连接MetaMask测试网钱包，支付Gas完成合约部署。

3. 写入留言生成交易哈希可溯源；查询留言仅读取链上数据不扣费。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
