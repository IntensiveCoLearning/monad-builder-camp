---
timezone: UTC+8
---

# AGF-DOT

**GitHub ID:** AGF-DOT

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
````markdown
学习笔记 - 2026年7月7日



 一、今日学习目标

1.  配置 Hardhat 连接 Monad Testnet
2.  编译 Solidity 智能合约
3.  将合约部署到 Monad Testnet
4.  调用合约的 Read Function
5.  调用合约的 Write Function
6.  学习智能合约安全最佳实践

---

 二、项目概述

### 项目名称
MessageBoard（消息板智能合约）

### 功能描述
一个简单的去中心化消息板应用，允许用户发布和查看消息。

### 技术栈
- **框架**: Hardhat
- **语言**: Solidity ^0.8.28
- **网络**: Monad Testnet (Chain ID: 10143)
- **钱包**: MetaMask
- **交互**: ethers.js

---

## 三、关键知识点

### 3.1 Hardhat 网络配置

在 [hardhat.config.js](file:///d:/Web3暑期实习/solidity-project/hardhat.config.js) 中添加自定义网络：

```javascript
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.28",
  networks: {
    monadTestnet: {
      url: process.env.MONAD_TESTNET_RPC_URL,
      accounts: [process.env.WALLET_PRIVATE_KEY],
      chainId: 10143,
    },
  },
};
```

知识点:
- 使用 `dotenv` 加载环境变量，避免硬编码敏感信息
- `accounts` 字段接受私钥数组，第一个私钥作为部署者账户
- `chainId` 用于验证网络身份

### 3.2 环境变量配置

在 `.env` 文件中存储敏感信息：

```env
MONAD_TESTNET_RPC_URL=https://rpc.ankr.com/monad_testnet
WALLET_PRIVATE_KEY=你的钱包私钥
```

**知识点**:
- `.env` 文件必须添加到 `.gitignore` 中
- 使用公共 RPC（如 Ankr）可以快速连接测试网
- 私钥是钱包的核心，泄露会导致资产损失

### 3.3 合约编译

```shell
npx hardhat compile
```

**知识点**:
- 编译产物存放在 `artifacts/` 目录
- Hardhat 会自动检测 Solidity 版本
- 编译错误会在终端显示详细信息

### 3.4 合约部署

创建部署脚本 [scripts/deploy.js](file:///d:/Web3暑期实习/solidity-project/scripts/deploy.js)：

```javascript
const { ethers } = require("hardhat");

async function main() {
  const [deployer] = await ethers.getSigners();
  console.log("Deploying with account:", deployer.address);

  const MessageBoard = await ethers.getContractFactory("MessageBoard");
  const messageBoard = await MessageBoard.deploy();
  await messageBoard.waitForDeployment();

  const contractAddress = await messageBoard.getAddress();
  console.log("Deployed to:", contractAddress);
}

main();
```

执行部署：
```shell
npx hardhat run scripts/deploy.js --network monadTestnet
```

**知识点**:
- `ethers.getSigners()` 获取配置的账户列表
- `ContractFactory.deploy()` 部署合约到链上
- `waitForDeployment()` 等待交易确认
- `getAddress()` 获取部署后的合约地址

### 3.5 部署结果

| 项目 | 值 |
|------|-----|
| 合约地址 | `0x92e00ffd40925aB9364884901290C18Ce2e060B9` |
| 交易 Hash | `0x372d13974a796aa4c37c74b9b35f0c8366c91875972313e867498d84f52b2866` |
| 部署者地址 | `0x000ca106EC26a06c3180a51aC1D56b0b2521bB46` |

### 3.6 合约交互

#### Read Function（只读调用，无需 gas）

```javascript
const count = await contract.getMessageCount();      // 获取消息数量
const messages = await contract.getMessages();       // 获取所有消息
const msg = await contract.getMessageByIndex(0);     // 获取指定索引的消息
```

**知识点**:
- Read Function 不修改链上状态
- 调用 Read Function 不需要支付 gas
- 返回值直接从节点获取

#### Write Function（写操作，需要 gas）

```javascript
const tx = await contract.addMessage("Hello World!");
await tx.wait();
```

**知识点**:
- Write Function 会修改链上状态
- 需要支付 gas 费用
- 返回 Transaction 对象，需等待确认

### 3.7 智能合约安全

#### 敏感信息保护

在 [.gitignore](file:///d:/Web3暑期实习/solidity-project/.gitignore) 中添加：

```
.env                    # 私钥
deployment-info.json    # 部署信息
```

**安全原则**:
1. 永远不要将私钥提交到 Git
2. 使用环境变量管理密钥
3. 定期轮换密钥
4. 使用 Secret Scanning 工具

---

## 四、遇到的问题与解决方案

### 问题 1: RPC 连接超时

**现象**: 使用 `https://testnet-rpc.monad.xyz` 连接超时

**原因**: 部分公共 RPC 可能不稳定或被墙

**解决方案**: 切换到 Ankr 的公共 RPC `https://rpc.ankr.com/monad_testnet`

### 问题 2: Hardhat Ignition 需要交互式确认

**现象**: `npx hardhat ignition deploy` 会询问是否确认部署

**解决方案**: 使用自定义部署脚本 `scripts/deploy.js`，避免交互式确认

### 问题 3: BigInt 类型错误

**现象**: `TypeError: Cannot mix BigInt and other types`

**原因**: Solidity 的 `uint` 类型在 ethers.js 中返回 `BigInt`，不能直接与普通数字运算

**解决方案**: 使用 `BigInt` 字面量（如 `1n`）或转换方法

```javascript
// 错误写法
const lastMsg = await contract.getMessageByIndex(newCount - 1);

// 正确写法
const lastMsg = await contract.getMessageByIndex(newCount - 1n);
```

---

## 五、学习收获

### 技术层面
1. ✅ 掌握了 Hardhat 配置自定义网络的方法
2. ✅ 学会了使用 ethers.js 部署和交互智能合约
3. ✅ 理解了 Read Function 和 Write Function 的区别
4. ✅ 掌握了环境变量和 .gitignore 的安全配置

### 实践层面
1. ✅ 成功将合约部署到真实测试网
2. ✅ 完成了完整的合约交互流程
3. ✅ 学会了排查 RPC 连接问题
4. ✅ 掌握了 BigInt 类型的处理方法

### 安全意识
1. ✅ 理解了私钥保护的重要性
2. ✅ 学会了配置 .gitignore 防止敏感信息泄露
3. ✅ 了解了测试网和主网的区别

---

## 六、下一步学习计划

1. 📚 学习 Solidity 高级特性（继承、接口、库）
2. 📚 学习测试网代币获取方法
3. 📚 学习前端与智能合约交互（Wagmi/Viem）
4. 📚 学习智能合约安全审计基础
5. 📚 尝试部署到其他测试网（Sepolia、Goerli）

````
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->

今天已经把安装插件、创建钱包、添加网络、领水、看区块链浏览器这些步骤全部完成了
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
