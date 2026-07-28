- GitHub ID: 184881686
- Name: daidaidawang
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

# 第11课：实现 Cadence

## 一、Cadence 是什么

Cadence 是由 Category Labs 提出的**区块链共识协议（Consensus Protocol）**。

它主要解决两个问题：

1.  **共识问题**
    
    -   验证者（Validator）需要对区块顺序达成一致。
        
    -   即使部分验证者故障或恶意，也能保持正确运行。
        
    -   Cadence 属于 **BFT（拜占庭容错）协议**。
        

BFT 特点：

> 当恶意验证者数量不超过总验证者的 1/3 时，系统仍然可以正常工作。

2.  **MEV / 抢跑问题**
    

传统区块链：

```
用户交易
 ↓
公开内存池（Mempool）
 ↓
验证者排序
 ↓
打包区块
```

问题：

-   交易内容提前暴露。
    
-   机器人可以观察交易。
    
-   可能发生抢跑、三明治攻击。
    

Cadence 使用：

**Encrypted Mempool（加密内存池）**

流程：

```
交易加密
 ↓
验证者排序
 ↓
共识完成
 ↓
释放密钥
 ↓
交易解密
```

因此：

> 在交易顺序确定之前，没有人可以看到交易内容。

* * *

# 二、核心概念

## 1\. Validator（验证者）

参与区块链共识的节点。

负责：

-   接收提案
    
-   投票
    
-   达成共识
    

* * *

## 2\. Slot（时隙）

一次共识过程。

一个 Slot：

```
提出区块
 ↓
投票
 ↓
提交
 ↓
最终确定
```

成功后产生一个区块。

* * *

## 3\. Proposer（提议者）

负责提出交易列表。

特点：

-   每个 Slot 有多个 proposer。
    
-   Cadence 中不是单 proposer。
    
-   proposer 会轮换。
    

例如：

Slot 1:

```
Validator 0
Validator 1
Validator 2
```

Slot 2:

```
Validator 1
Validator 2
Validator 3
```

* * *

## 4\. Quorum（法定人数）

达到最终确认所需的投票数量。

公式：

```
f = floor((N-1)/3)

Quorum = N-f
```

例如：

N=10

最大容忍：

```
f=3
```

所以：

```
Quorum=10-3=7
```

含义：

至少 7 个验证者同意，区块才能最终确认。

* * *

## 5\. Rebuild（重建数量）

用于恢复加密信息。

公式：

```
Rebuild=f+1
```

例如：

```
f=3

Rebuild=4
```

表示：

至少需要 4 个密钥碎片才能恢复。

* * *

# 三、门限加密（Threshold Encryption）

Cadence 不使用一把完整密钥。

而是：

```
完整密钥
 ↓
拆分
 ↓
N份密钥碎片
 ↓
每个Validator保存一份
```

特点：

-   单个验证者无法解密。
    
-   防止提前查看交易。
    

恢复条件：

```
f+1 个碎片
```

例如：

```
10个Validator

需要4个碎片
```

才能解密。

* * *

# 四、Cadence 一个 Slot 的流程

## Step 1：Proposal（提议）

三个 proposer：

-   从加密内存池取交易。
    
-   生成交易列表。
    
-   对交易进行 Hash。
    

生成：

```
Proposal ID
```

然后：

```
Proposal
 ↓
分片
 ↓
发送给Validator
```

每个验证者获得一个 chunk。

* * *

## Step 2：First Vote（首轮投票）

验证者检查：

-   分片是否收到。
    
-   是否完整。
    

投票：

```
YES
或者
NO
```

同时携带：

```
Key Piece
```

也就是自己的密钥碎片。

重要：

第一次投票同时完成：

1.  判断提案是否可用
    
2.  释放密钥碎片
    

所以：

> 投票决定顺序，投票也触发解密。

* * *

## Step 3：解密

当收到：

```
f+1
```

个密钥碎片后：

恢复密钥。

例如：

```
4个碎片
```

达到：

```
Decrypt
```

交易公开。

注意：

解密阈值：

```
f+1
```

不是最终确认阈值。

* * *

## Step 4：Speculative Finality

推测性最终确认。

条件：

所有 proposer 都已经决定：

-   包含
    
-   排除
    

此时：

进入提交阶段。

* * *

## Step 5：Commit Vote（提交投票）

验证者发送：

```
Commit Vote
```

当收到：

```
2f+1
```

票数：

达到最终确认。

例如：

N=10：

```
2f+1=7
```

最终：

```
Block Finalized
```

* * *

# 五、最终区块生成

区块包含：

-   所有通过 proposer 的交易。
    
-   删除重复交易。
    
-   排除失败 proposal。
    

然后：

```
Slot结束

↓

进入下一Slot
```

* * *

# 六、模拟器实现内容

课程要求使用 Vibecoding 创建 Cadence 模拟器。

## Step 1：基础界面

实现：

-   Validator列表
    
-   Slot显示
    
-   Quorum计算
    
-   Rebuild计算
    
-   Proposer显示
    

数据：

```
Validator

SimState
```

* * *

## Step 2：模拟网络

增加：

-   消息发送
    
-   网络延迟
    
-   时钟
    

消息模型：

```
from
to
type
payload
sentAt
arrivesAt
```

* * *

## Step 3：Proposal

实现：

-   创建交易ID
    
-   Hash
    
-   分片
    
-   广播
    

结果：

Validator收到：

```
chunk
```

* * *

## Step 4：First Vote

实现：

每个Validator：

-   判断proposal
    
-   投票
    
-   发送key piece
    

达到：

```
f+1
```

模拟解密。

* * *

## Step 5：Commit

实现：

-   speculative finality
    
-   commit vote
    
-   quorum确认
    

生成：

```
Block
```

* * *

## Step 6：并发Slot

增加：

-   多Slot同时运行
    
-   Conductor调度
    
-   Auto Run
    

实现：

```
Slot1
Slot2
Slot3
同时执行
```

体现 Cadence 的流水线思想。

* * *

## Step 7：异常处理

实现：

### 有序链

即使：

```
Slot3先完成
Slot2后完成
```

仍然：

```
Slot1
Slot2
Slot3
```

顺序写入。

* * *

### Faulty Proposer

模拟：

-   proposer掉线
    
-   slot失败
    
-   生成空block
    

* * *

### Outage

模拟：

网络中断。

效果：

-   消息暂停。
    
-   时间继续。
    
-   恢复后继续处理。
    

* * *

# 七、生产 Cadence 与模拟器区别

模拟器：

有：

-   proposer
    
-   vote
    
-   quorum
    
-   finality
    
-   f+1解密逻辑
    

但是：

没有真实密码学。

生产环境：

包含：

-   BLS签名
    
-   DKG分布式密钥生成
    
-   真正门限加密
    
-   纠删码
    
-   P2P网络
    
-   恶意节点检测
    

核心区别：

> 协议逻辑是真实的，密码学实现是简化模拟的。

* * *

# 八、重点公式总结

| 概念 | 公式 |
| --- | --- |
| 最大拜占庭节点 | f = floor((N-1)/3) |
| 最终确认 | Quorum=N-f |
| 解密需求 | Rebuild=f+1 |
| 最终投票 | 2f+1 |

例如：

N=10：

```
f=3

Quorum=7

Rebuild=4

Finality=7
```
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
# 2026-07-11

# 第10课：构建你的第一个去中心化应用（dApp）

## 一、课程目标

本课程目标：

> 将一个普通 Web 应用升级为运行在区块链上的去中心化应用（dApp）。

前面课程学习了：

-   数据库 → 保存应用数据
    
-   钱包 → 用户身份
    

本课程进一步学习：

-   智能合约
    
-   Solidity
    
-   Monad链上部署
    
-   前端连接区块链
    

最终构建：

**MessageBoard（链上留言板）**

实现：

-   用户连接钱包
    
-   读取链上消息
    
-   修改链上消息
    
-   数据永久保存到 Monad
    

* * *

# 二、什么是去中心化应用后端？

传统 Web 应用：

```
用户
 ↓
前端
 ↓
服务器
 ↓
数据库
```

服务器属于：

-   公司
    
-   平台
    
-   云服务商
    

可能出现：

-   服务器宕机
    
-   账号封禁
    
-   数据被修改
    

* * *

## dApp 后端

去中心化应用：

```
用户
 ↓
钱包
 ↓
前端
 ↓
智能合约
 ↓
区块链
```

后端逻辑运行在：

> 区块链网络上的智能合约

特点：

### 1\. 无中心控制

部署后：

-   开发者无法随意修改
    
-   公司无法关闭
    

* * *

### 2\. 自动执行

智能合约按照代码运行：

例如：

```
发送交易
 ↓
满足条件
 ↓
执行函数
 ↓
修改链上状态
```

* * *

### 3\. 所有人执行结果一致

因为所有节点保存相同状态。

* * *

# 三、智能合约安全提醒

智能合约和普通程序不同。

普通程序：

```
出现Bug
 ↓
修复
 ↓
重新发布
```

智能合约：

```
部署
 ↓
永久存在
```

错误可能：

-   无法撤销
    
-   无法恢复
    

特别是：

金融类合约：

例如：

-   DeFi
    
-   钱包
    
-   交易协议
    

一个漏洞可能导致：

-   资金永久丢失
    

* * *

## 开发原则

### 1\. 保持合约简单

代码越复杂：

风险越高。

* * *

### 2\. 使用成熟库

例如：

OpenZeppelin

不要重复造轮子。

* * *

### 3\. 先测试网

流程：

```
Testnet测试

↓

安全检查

↓

Mainnet上线
```

禁止：

直接主网上测试。

* * *

# 四、什么是 Solidity？

Solidity：

> 编写 EVM 智能合约的主要语言。

Monad兼容：

```
EVM
```

所以支持：

```
Solidity
```

* * *

## 是否需要精通 Solidity？

不需要。

课程采用：

Vibecoding（AI辅助开发）

但是需要理解：

-   合约结构
    
-   状态变量
    
-   函数
    
-   事件
    

这样才能：

-   阅读AI生成代码
    
-   判断错误
    

* * *

# 五、准备开发环境

本课程提供两种方式：

## 方式1：Replit

适合：

-   新手
    
-   快速开发
    

优势：

-   自动环境
    
-   集成AI Agent
    

* * *

## 方式2：Local本地开发

适合：

-   真实开发
    
-   工程实践
    

需要：

-   Node.js
    
-   Hardhat
    
-   钱包
    

* * *

# 六、Step 1：构建前端

目标：

创建一个留言板页面。

页面包含：

```
Message Board

----------------

当前消息

Loading...

输入框
Update Message按钮
Connect Wallet按钮
```

* * *

## 技术栈

React：

负责界面

wagmi：

负责：

-   连接钱包
    
-   调用合约
    

RainbowKit：

负责：

钱包连接界面

* * *

## 注意

此时：

Update按钮不能真正修改数据。

原因：

目前没有连接区块链。

流程：

先做UI

↓

再接智能合约

* * *

# 七、准备开发钱包

## 为什么需要钱包？

部署合约需要：

-   地址
    
-   私钥
    

钱包负责：

-   身份认证
    
-   签署交易
    

* * *

## 安全要求

不要使用：

主钱包

有真实资产的钱包

创建：

开发钱包。

原因：

开发过程中需要：

-   私钥
    
-   测试交易
    

* * *

# 八、获取私钥

MetaMask：

步骤：

```
账户
 ↓
Account Details
 ↓
Show Private Key
 ↓
输入密码
 ↓
复制
```

私钥拥有：

100%账户控制权。

* * *

# 九、私钥保存方式

错误：

```
privateKey="xxxx"
```

写进代码。

风险：

-   GitHub泄露
    
-   AI读取
    
-   他人获取
    

* * *

正确：

环境变量。

例如：

```
PRIVATE_KEY
```

代码读取：

```
process.env.PRIVATE_KEY
```

* * *

# 十、获取测试网MON

部署合约需要：

Gas费用。

使用：

Monad Faucet

获得：

```
Testnet MON
```

用途：

-   部署合约
    
-   调用函数
    

测试币：

没有真实价值。

* * *

# 十一、Step 2：编写链上逻辑

目标：

创建：

```
MessageBoard
```

智能合约。

* * *

## 合约功能

### 1\. 保存消息

状态变量：

```solidity
string message;
```

用于保存：

当前留言。

* * *

### 2\. 查询消息

函数：

```
getMessage()
```

任何人：

可以读取。

* * *

### 3\. 修改消息

函数：

```
updateMessage()
```

用户发送交易：

修改链上数据。

* * *

### 4\. 事件

事件：

```
MessageUpdated
```

作用：

记录消息变化。

方便：

-   查询
    
-   前端监听
    

* * *

# 十二、Hardhat部署

Hardhat：

智能合约开发工具。

作用：

-   编译 Solidity
    
-   部署合约
    
-   管理网络
    

* * *

部署流程：

```
Solidity代码
↓
Hardhat编译
↓
连接Monad Testnet

↓
发送部署交易
↓
生成Contract Address
```

* * *

部署成功后：

获得：

```
合约地址 Contract Address
```

例如：

```
0x123456789....
```

保存。

后续前端需要它。

* * *

# 十三、前端连接智能合约

现在：

前端：

↓

连接：

MessageBoard合约

* * *

使用：

wagmi

## 读取数据

使用：

```
useReadContract
```

流程：

打开网页

↓

调用合约

↓

读取message

↓

显示页面

* * *

## 修改数据

使用：

```
useWriteContract
```

流程：

用户输入：

```
Hello Monad
```

↓

点击Update

↓

钱包弹出签名

↓

确认交易

↓

发送到Monad

↓

区块确认

↓

重新读取消息

↓

页面更新

* * *

# 十四、完整运行流程

最终架构：

```
用户
↓
React前端
↓
wagmi
↓
钱包MetaMask
↓
Monad Testnet
↓
MessageBoard智能合约
↓
区块链状态
```

* * *

# 十五、测试应用

操作：

1.  打开网页
    
2.  点击：
    

```
Connect Wallet
```

3.  连接Monad测试网
    
4.  查看当前消息
    
5.  输入新消息
    
6.  点击：
    

```
Update Message
```

7.  钱包确认交易
    
8.  等待区块确认
    
9.  页面显示新消息
    

* * *

# 十六、wagmi作用

wagmi：

React区块链交互工具。

提供：

-   钱包连接
    
-   读取合约
    
-   调用合约
    
-   监听交易
    

不是：

智能合约语言。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
# 2026-07-10

# 第09课：加密钱包（Crypto Wallet）

# 一、核心概念

在传统互联网中：

```
账号 = 平台创建
身份 = 邮箱/手机号+密码
控制权 = 平台拥有
```

例如：

-   GitHub账号
    
-   微信账号
    
-   Google账号
    

平台可以：

-   封禁账号
    
-   删除数据
    
-   限制访问
    

* * *

区块链中：

```
钱包 = 用户自己创建
身份 = 钱包地址
控制权 = 用户自己拥有
```

没有：

-   中心服务器
    
-   客服找回
    
-   平台封禁
    

钱包是用户进入区块链世界的入口。

* * *

# 二、什么是加密钱包？

加密钱包（Crypto Wallet）：

> 管理区块链账户的软件，用于保存凭证、签署交易，并连接去中心化应用。

注意：

钱包**不是真正存储代币的地方**。

错误理解：

❌ 钱包里面存着 MON、ETH

正确理解：

✅ 区块链保存资产记录  
✅ 钱包保存访问权限

类似：

浏览器：

```
浏览器 ≠ 互联网

只是访问互联网的工具
```

钱包：

```
钱包 ≠ 区块链资产

只是访问区块链账户的工具
```

* * *

# 三、钱包账户结构

一个钱包账户包含：

```
账户(Account)

   |
   |---- 地址 Address
   |
   |---- 私钥 Private Key

```

另外：

钱包还有：

```
助记词 Seed Phrase
```

作为最高级恢复凭证。

* * *

# 四、地址（Address）

## 作用

地址：

> 区块链上的公开身份。

类似：

```
银行卡号
用户名
收款地址
```

例如：

```
0x71C7656EC7ab88b098defB751B7401B5f6d8976F
```

别人可以：

-   查看你的余额
    
-   给你转账
    
-   查询交易记录
    

* * *

## 地址安全吗？

安全。

因为：

地址只能：

✅ 接收资产  
✅ 查询信息

不能：

❌ 转走资产

所以：

钱包地址可以公开。

例如：

别人问：

```
你的MON地址是多少？
```

可以发送。

* * *

# 五、私钥（Private Key）

私钥：

> 控制钱包资产的真正密码。

关系：

```
地址 = 账号

私钥 = 密码
```

拥有私钥：

↓

拥有账户控制权。

如果别人获得：

```
你的私钥
```

那么他可以：

-   转走代币
    
-   签署交易
    
-   完全控制钱包
    

* * *

## 私钥特点

### 没有找回机制

传统网站：

```
忘记密码
↓
邮箱验证
↓
重置密码
```

区块链：

```
忘记私钥
↓
没有办法恢复
```

* * *

### 私钥绝对不能泄露

禁止：

❌ 发给别人

❌ 截图保存

❌ 上传GitHub

❌ 粘贴到陌生网站

* * *

# 六、助记词（Seed Phrase）

助记词：

> 恢复整个钱包的最高权限凭证。

通常：

12个或者24个英文单词。

例如：

```
apple
river
moon
...
```

作用：

```
助记词

↓

生成钱包

↓

生成多个账户

↓

生成私钥

↓

生成地址

```

关系：

```
助记词
   |
   |
 私钥
   |
   |
 地址

```

* * *

# 七、助记词安全等级

安全等级：

```
助记词
  ↓
私钥
  ↓
地址
```

危险程度：

助记词 > 私钥 > 地址

原因：

拿到地址：

只能查看。

拿到私钥：

控制一个账户。

拿到助记词：

控制整个钱包。

* * *

# 八、正确保存方式

推荐：

✅ 手写记录

✅ 离线保存

✅ 多地点备份

不要：

❌ 手机备忘录

❌ 微信收藏

❌ 云盘

❌ 手机截图

原因：

手机可能：

-   被盗
    
-   被病毒读取
    
-   云同步泄露
    

* * *

# 九、开发钱包规范（重点）

开发时：

不要使用：

```
真实资产钱包
```

应该：

创建：

```
开发钱包
```

例如：

钱包A：

```
存真实MON
```

钱包B：

```
测试部署智能合约
```

分离。

原因：

开发过程中可能：

-   私钥泄露
    
-   测试代码错误
    
-   误发送交易
    

* * *

# 十、Monad 钱包兼容性

Monad：

> EVM Compatible（兼容以太坊虚拟机）

所以支持：

-   MetaMask
    
-   Coinbase Wallet
    
-   Rabby
    
-   EVM钱包
    

原因：

Monad使用：

```
Ethereum生态标准
```

所以：

以太坊工具可以直接使用。

* * *

# 十一、连接 Monad 网络

钱包默认：

```
Ethereum Mainnet
```

需要添加：

```
Monad Network
```

* * *

# 十二、Monad Testnet（测试网）

开发阶段使用：

```
Monad Testnet
```

特点：

-   MON没有真实价值
    
-   可以免费领取
    
-   用于测试
    

配置：

```
Network Name:
Monad Testnet


Chain ID:
10143


Currency:
MON


RPC:
https://testnet-rpc.monad.xyz


Explorer:
https://testnet.monadscan.com
```

* * *

# 十三、Monad Mainnet（主网）

正式运行使用：

```
Monad Mainnet
```

配置：

```
Network Name:
Monad Mainnet


Chain ID:
143


Currency:
MON


RPC:
https://rpc.monad.xyz


Explorer:
https://monadscan.com
```

区别：

|   | Testnet | Mainnet |
| --- | --- | --- |
| 代币 | 无价值 | 真实价值 |
| 用途 | 开发测试 | 正式应用 |
| 风险 | 低 | 高 |

* * *

# 十四、Faucet（水龙头）

测试网没有真实价值：

所以：

不能购买。

通过：

```
Faucet
```

领取。

流程：

```
钱包地址

↓

输入 Faucet

↓

领取测试 MON

↓

用于部署合约
```

MON用途：

-   支付Gas费用
    
-   调用智能合约
    
-   部署DApp
    

* * *

# 十五、钱包与DApp交互流程（重点）

完整流程：

```
用户打开DApp

↓

连接钱包

↓

钱包提供地址

↓

用户操作

↓

生成交易请求

↓

钱包确认

↓

私钥签名

↓

发送到Monad网络

↓

验证者确认

↓

交易完成
```

重点：

私钥永远不会发送给DApp。

DApp得到：

✅ 地址

不知道：

❌ 私钥

* * *
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

# 第08课：数据库与文件存储（Database & File Storage）

# 一、核心目标

一个真正的生产级应用不能只展示页面，还需要具备：

-   **记忆能力 → 数据库（Database）**
    
-   **文件保存能力 → 文件存储（File Storage）**
    

简单来说：

> 数据库存储“结构化信息”，文件存储保存“非结构化文件”。

例如一个社交软件：

| 数据类型 | 存储位置 |
| --- | --- |
| 用户名、密码、点赞数量 | 数据库 |
| 用户头像、视频、图片 | 文件存储 |

数据库 + 文件存储，才是完整应用的基础。

# 二、数据库（Database）

## 1\. 什么是数据库？

数据库就是：

> 应用保存、读取和管理数据的地方。

没有数据库：

-   页面刷新 → 数据消失
    
-   软件关闭 → 信息丢失
    

有数据库：

-   用户注册信息可以保存
    
-   历史记录可以查询
    
-   数据可以长期存在
    

例如：

Instagram：

-   用户信息
    
-   图片描述
    
-   点赞记录
    

都会存储在数据库中。

# 三、数据库结构

数据库主要由：

## 1\. Table（表）

类似 Excel 表格。

例如 Todo 应用：

### users 表

| id | name |
| --- | --- |
| 001 | Tom |

### todos 表

| id | user_id | task | completed |
| --- | --- | --- | --- |
| 1 | 001 | 学习区块链 | false |

## 2\. Row（行）

代表一条数据。

例如：

```
001 Tom
```

是一条用户记录。

## 3\. Column（列）

代表数据字段。

例如：

```
name
age
email
```

都是字段。

# 四、SQL 数据库语言

SQL：

> Structured Query Language（结构化查询语言）

用于和数据库交流。

最常用四个操作：

## 1\. 查询 SELECT

读取数据：

```sql
SELECT * FROM todos
```

意思：

获取 todos 表所有数据。

## 2\. 添加 INSERT

增加数据：

```sql
INSERT INTO todos(task)
VALUES('学习Monad');
```

## 3\. 修改 UPDATE

更新数据：

```sql
UPDATE todos
SET completed=true
WHERE id=1;
```

## 4\. 删除 DELETE

删除数据：

```sql
DELETE FROM todos
WHERE id=1;
```

## 为什么需要懂 SQL？

即使 AI 帮你生成 SQL，也需要知道：

-   有没有查询错误
    
-   有没有删除全部数据
    
-   有没有权限漏洞
    
-   有没有 SQL 注入风险
    

AI 是工具，开发者需要理解结果。

# 五、Supabase

本课程使用：

> Supabase

它提供：

-   PostgreSQL 数据库
    
-   文件存储 Storage
    
-   用户认证 Auth
    
-   API 接口
    

优势：

-   免费额度
    
-   配置简单
    
-   适合快速开发
    

# 六、创建数据库流程

## Step 1：创建 Supabase 项目

流程：

```
Supabase
 ↓
New Project
 ↓
设置项目名称
 ↓
选择区域
 ↓
创建数据库密码
```

## Step 2：连接应用

需要两个重要参数：

### Project URL

数据库地址

### anon key

公开访问密钥

放入：

```
Environment Variables
```

环境变量。

例如：

```
SUPABASE_URL
SUPABASE_KEY
```

# 七、安全重点（重点）

## 1\. 不要暴露 service\_role key

Supabase 有两个 key：

### anon key

普通客户端使用。

权限有限。

### service\_role key

超级权限。

可以：

-   删除数据库
    
-   绕过安全规则
    
-   修改所有数据
    

如果泄露：

攻击者可以直接控制数据库。

所以：

```
service_role
只能服务器保存

不能放前端
不能上传GitHub
```

# 八、创建数据表

例如清单应用：

创建：

```
list_items
```

SQL：

```sql
CREATE TABLE public.list_items(
id BIGINT GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
name TEXT NOT NULL,
created_at TIMESTAMPTZ DEFAULT now()
);
```

生成：

| id | name | created_at |
| --- | --- | --- |
| 1 | 学习Monad | 时间 |

# 九、RLS（Row Level Security）

重点：

RLS：

> 行级安全策略。

作用：

控制：

“谁可以访问哪一条数据”。

例如：

用户A：

只能看到：

```
user_id=A
```

的数据。

用户B：

不能查看用户A的数据。

生产环境必须开启。

# 十、文件存储 Storage

数据库不适合存：

-   图片
    
-   视频
    
-   音频
    
-   PDF
    

原因：

-   文件太大
    
-   查询慢
    
-   成本高
    

所以使用：

```
Database + Storage
```

# 十一、Storage 工作方式

例如上传头像：

流程：

```
用户上传图片
↓
Storage保存图片
↓
返回文件地址(URL)
↓
数据库保存URL
```

数据库保存：

```
avatar_url
```

而不是保存图片本身。

# 十二、Bucket（存储桶）

Bucket：

类似文件夹。

例如：

```
Storage

 └── avatars

 └── videos

 └── documents
```

创建：

```
todo-attachments
```

用于保存附件。

# 十三、Storage 安全

默认：

> private bucket

只有授权用户可以访问。

公开文件：

例如：

-   网站头像
    
-   商品图片
    

可以：

```
public bucket
```

私人文件：

例如：

-   医疗记录
    
-   身份证明
    
-   合同
    

必须：

```
private bucket
```

# 十四、文件上传安全

上传文件必须限制：

## 1\. 文件类型

允许：

```
jpg
png
webp
pdf
```

禁止：

```
exe
html
js
```

防止：

-   恶意程序
    
-   XSS攻击
    

## 2\. 文件大小

例如：

最大：

```
5MB
```

防止：

用户上传几个GB文件导致：

-   存储爆炸
    
-   费用增加
    

## 3\. 文件名处理

不要直接使用用户文件名：

# 十五、数据库 vs 区块链

两者不是替代关系。

## 数据库适合：

私人数据

例如：

-   用户资料
    
-   聊天记录
    
-   商品信息
    

高频修改

例如：

-   点赞
    
-   浏览量
    

大量查询

例如：

搜索、排序。

* * *

## 区块链适合：

公共可信数据

例如：

-   NFT所有权
    
-   交易记录
    
-   投票结果
    

特点：

-   公开
    
-   不可修改
    
-   无中心控制
    

# 十六、真实应用通常混合使用

例如：

电商平台：

数据库：

```
用户信息
商品图片
订单信息
```

区块链：

```
商品所有权
交易证明
支付记录
```

Storage：

```
商品图片
用户上传文件
```

三者结合：

```
用户
 |
应用
 |
----------------
数据库
Storage
区块链
----------------
```

# 十七、本节最终搭建成果

完成后应用拥有：

数据持久化

用户刷新页面：

数据仍存在。

文件上传

用户可以上传：

图片、文件。

环境变量保护

API key 不暴露。

数据安全基础

使用：

-   RLS
    
-   文件限制
    
-   输入校验
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

# 第 06 课：重要的区块链概念

# 一、区块链运行原理

## 1\. Genesis State（创世状态）

一条区块链第一次正式启动时的初始状态。

**每条链只有一个 Genesis State**

\> Bitcoin：2009

Ethereum：2015

Monad：2025

## 2\. Node（节点）

区块链运行在全球大量节点上，而不是一台服务器。

每个节点都保存完整账本，并独立验证交易。

节点越多，网络越去中心化，容错能力越强。

### Validator（验证者）

节点中的特殊角负责打包新区块。

需要质押 MON，作弊会被罚没（Slashing）。

## 3\. Block（区块）

一批已经验证完成的交易集合。

每个区块都通过密码学与前一个区块相连接。

修改任何一个区块，后续所有区块都会失效，因此区块链具有**不可篡改**（Immutable）特性。

## 4\. Consensus（共识机制）

作用： 让全球所有节点对"当前链上状态"达成一致。

没有共识：

\> 每个节点都会认为自己的数据是正确的，区块链无法正常运行。

# 二、PoW 与 PoS

## Proof of Work（PoW）

代表：Bitcoin，Ethereum（Merge 前）

特点：靠算力竞争出块，安全性高，能耗极高

## Proof of Stake（PoS）

代表：Ethereum（现在）

## Monad

### 特点

通过质押代币获得出块资格。

验证者随机产生新区块。

作恶会被 Slashing（罚没质押资产）。

能耗远低于 PoW。

PoW 靠算力保证安全，PoS 靠经济惩罚保证安全。

# 三、Gas

Gas 是执行链上操作需要支付的手续费。

包括：转账，调用智能合约，部署智能合约

### Gas 计算

Gas Fee = Gas Used × Gas Price

### Gas 的作用

支付验证者。

防止垃圾交易攻击。

## Monad

TPS 更高。

区块空间充足。

Gas 费用远低于 Ethereum。

# 四、Finality（最终性）

最终性（Finality）：一笔交易被永久确认，之后无法撤销。

最终性越快：用户等待时间越短。支付体验越好。

## Monad 的优势

亚秒级最终性（约 800ms）

# 五、Monad 开发常用工具

Mainnet 与 Testnet

## Mainnet（主网）

真正的区块链。

使用真实 MON。

所有交易真实生效。

## Testnet（测试网）

用于开发测试。

代币没有价值。

所有实验都应先在 Testnet 完成。

## Chain ID

每条区块链的唯一编号。

### 作用

钱包识别当前连接的是哪条链。

添加 Monad 网络时需要填写。

## RPC（Remote Procedure Call）

RPC 就是钱包或应用连接区块链的接口（URL）。

所有链上操作都通过 RPC 完成，例如：

查询余额

发送交易

调用合约

**开发初期使用 Public RPC 即可，正式上线通常使用付费 RPC（如 Alchemy、QuickNode）以获得更稳定的服务**

## Faucet（水龙头）

作用： 免费领取 Testnet MON。

### 用途

部署测试合约

测试交易

学习开发

注意：只能领取测试币。每日通常有限额。

## Explorer

（区块浏览器）

作用： 查看链上的所有公开信息。

可查询：交易状态（Tx），钱包余额，智能合约，区块信息，Gas 消耗，开发过程中几乎每天都会使用
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

# Freshman Track 第01课：Vibecoding 入门笔记

# 1\. Vibecoding 是什么

Vibecoding 是一种利用 AI Agent 进行软件开发 的方式。

## 核心流程：

描述需求 → AI 构建 → 查看结果 → 提出反馈 → 持续迭代

开发者不再主要负责写代码，而是负责：

明确想构建什么

提供需求和反馈

判断最终结果是否符合目标

# 2\. AI Agent 与 Chatbot 区别

## Chatbot：

只能回答问题

输出文本

## AI Agent：

可以执行任务

创建文件

编写代码

运行命令

修复错误

Vibecoding 的核心就是人与 AI Agent 协作完成开发。

# 3\. Vibecoding 的关键能力：Prompt

Prompt 决定 AI 输出质量。

好的 Prompt 需要说明：

做什么产品

目标用户

页面结构

功能需求

UI 风格

例如：

“做一个网站” → 信息不足

“创建深色主题个人主页，包含项目展示、社交链接、移动端适配” → 更容易得到准确结果。

# 4\. AI Agent 安全问题

AI Agent 具有较高权限，可能：

读取文件

执行命令

访问外部内容

## 风险

Prompt Injection（提示注入）

即恶意内容通过隐藏指令影响 Agent 行为。

## 安全实践

使用沙箱环境（Replit / Codespaces）

不提供真实私钥、API Key

审查 AI 生成代码

# 5\. Vibecoding Debug 方法

遇到错误：

不要重新开始，也不要自己猜。

## 正确方式：

复制完整错误信息

提供给 AI Agent

根据反馈继续迭代

# 6\. Vibecoding 能力边界

## 可以

快速构建网站、工具、Demo

生成前端、数据库、API 等代码

加速产品验证

## 不能

保证一次生成完美项目

替代产品思考

替代安全和技术判断

# Freshman Track 第02课笔记

构建并发布你的第一个应用

# 一、核心目标

通过 Vibecoding 完成完整开发流程：

想法 → Prompt → AI Agent 构建 → 调试 → 部署 → 分享

最终得到一个可以被互联网访问的应用 URL。

# 二、开发环境选择

推荐：Replit

## 特点

浏览器运行

无需本地配置

AI Agent 内置

环境隔离，更安全

Local 环境

## 适合

熟悉本地开发流程

需要更多控制权

常见问题：

Node 环境

npm 配置

localhost 连接

（了解即可，实际遇到问题交给 AI Agent 处理）

# 三、使用 AI Agent 构建应用

## 流程

## 1\. 选择简单目标

## 第一个项目重点

不是复杂，而是完整跑通流程。

例如：

个人主页

Todo List

倒计时工具

小型 Web App

## 2\. 编写 Prompt

Prompt 越具体，结果越准确

### 需要描述

应用目的

页面结构

功能需求

UI 风格

用户场景

例如：

不要：

\> 做一个网站

应该：

\> 创建一个深色主题个人主页，包括个人介绍、项目展示、社交链接，并适配移动端。

# 四、Vibecoding 开发循环

## 核心循环

描述

↓

构建

↓

审视

↓

反馈

↓

重复

## 重点

第一版不会完美，需要通过不断反馈优化。

# 五、Debug 方法

遇到问题：

不要：删除项目重做

自己猜错误原因

## 正确方式

复制完整错误信息

描述实际现象

交给 AI Agent 分析

例如：

错误：

\> 页面打不开

正确：

\> 页面显示空白，控制台出现 XXX error。

# 六、应用发布

## 部署方式

Replit

直接 Publish：

自动构建

自动部署

获得线上 URL

GitHub + Vercel

## 流程

代码上传 GitHub

↓

Vercel 部署

↓

获得公开访问地址

# 七、核心知识点

## 1\. Deployment（部署）

将本地运行的应用发布到公网，让其他用户访问。

## 2\. Iteration（迭代）

通过不断反馈修改应用，是 Vibecoding 的核心过程。

## 3\. Production Mindset（生产思维）

完成 Demo 只是开始。

真实应用还需要：

数据存储

用户系统

支付

安全

# Freshman Track 第03课笔记

让你的应用具备生产级品质

# 一、Demo 和 Production 的区别

部署成功 ≠ 生产可用。

## Demo

能运行

能展示功能

## Production

### 需要满足

稳定

安全

可扩展

用户体验完整

# 二、产品体验优化

## Responsive Design（响应式设计）

应用需要适配：，手机，平板，桌面端

移动端流量占比高，不能只针对电脑设计

## Favicon / Page Title

提升产品完整度：

浏览器标签图标

搜索结果标题

## Accessibility（无障碍）

让不同用户都能使用：

键盘操作

屏幕阅读器

视觉障碍用户

# 三、让应用被发现

## SEO

提高搜索引擎收录和排名。

## Open Graph

控制应用链接分享到：

Twitter

Discord

iMessage

时显示的标题、图片和描述

## Analytics

用户数量

来源

使用行为

帮助判断产品情况。

## Custom Domain

**使用自己的域名**

例如：

[myapp.com](http://myapp.com)

相比平台默认域名更像正式产品。

# 四、真实应用需要的功能

## Database（数据库）

### 解决

刷新后数据丢失的问题。

### 用于保存

用户数据

内容

配置

### 常见

Supabase

Firebase

## Authentication（用户认证）

### 负责

注册登录

密码管理

OAuth

不要自己实现。

原因

密码哈希、Session 管理等容易出现安全问题。

## File Storage（文件存储）

### 用途

图片

文档

用户上传文件

**不要直接存服务器**

## Payments（支付）

使用第三方支付服务

例如 Stripe。

### 负责

支付

退款

税务

# 五、基础设施

## Environment Variables（环境变量）

**敏感信息不要写进代码**

例如：

API Key

Secret

Token

### 原因

GitHub 上存在自动扫描机器人，泄露后可能导致安全问题。

## CI/CD

自动化流程

代码提交

↓

自动测试

↓

自动部署

## Staging Environment

生产环境之前的测试环境。

### 用途

验证新功能

避免影响线上用户

# 六、可靠性

## Error Handling

避免用户看到空白页面。

应该捕获错误，给用户提示，Error Tracking

例如 Sentry：

记录

哪个错误

哪个用户

发生环境

## Testing

单元测试

集成测试

E2E 测试

## Backup

**数据库必须备份**

防止数据损坏，错误迁移，误删除

# 七、安全

## HTTPS

加密用户和服务器通信。

## Input Validation

**永远不要信任用户输入。**

防止：SQL Injection，XSS，Rate Limiting

## 限制请求次数

防止：API 被刷爆，服务崩溃，成本异常增加

# 八、性能优化

## 图片优化

使用：压缩，WebP / AVIF，合适尺寸

## Cache（缓存）

减少重复请求，提高速度

## Core Web Vitals

Google 衡量网页体验的重要指标，会影响 SEO

# 九、法律要求

Privacy Policy（隐私政策）

Terms of Service（服务条款）

Cookie Consent（Cookie 同意）

# Freshman Track 第04课笔记

为什么要构建去中心化应用？

传统应用在生产环境中依赖大量中心化服务：

数据库

身份认证

文件存储

支付服务

云服务器

这些服务虽然方便，但开发者需要信任第三方平台。一旦平台出现：

服务中断，政策变化，账号冻结，平台限制，应用和用户都会受到影响。

## 去中心化应用（DApp）的核心价值

应用运行在开放的区块链网络上，不依赖单一公司或服务器控制，因此更难被关闭、限制或篡改。

## 区块链提供的能力

用户真正拥有资产和数据

无需许可即可参与

规则由代码和协议执行

应用可以长期运行

典型案例：

稳定币降低跨境支付成本

ENS 让用户拥有链上域名

Uniswap 实现无需许可的资产交换

# Freshman Track 第05课笔记

什么是 Monad？

# 一、Monad 定位

Monad 是一条 高性能 EVM 兼容区块链。

## 目标

在保持去中心化的同时，让去中心化应用拥有接近传统互联网应用的速度和体验。

## 背景

Bitcoin：证明了去中心化价值转移可行，但功能有限。

Ethereum：提出“区块链作为通用计算平台”，支持智能合约和 DApp。

Monad：在 Ethereum 基础上进一步优化，提高去中心化计算性能。

# 二、Monad 核心指标

## 1\. 高吞吐量

10,000 TPS（每秒交易数）

意义：可以支持，大规模用户应用，高频交互场景，接近 Web2 的体验

## 2\. 快速最终确认

800ms Finality

Finality：交易被确认并不可逆的时间。

影响：用户操作后可以快速得到反馈。

## 3\. 低硬件要求

目标：降低运行节点的门槛。

意义：更多人可以参与验证网络，提高去中心化程度。

# 三、Monad 如何实现高性能

## MonadBFT

Monad 使用自己的共识机制：MonadBFT

作用：让多个验证者快速达成交易共识。

## RaptorCast

负责：高效传播区块信息。

解决：大量节点之间通信效率问题。

## MonadDB

负责：数据存储优化。提高链上数据读取和处理效率。

# 四、Monad 的去中心化设计

很多高性能链会牺牲去中心化：

例如：

提高硬件要求

限制验证者数量

## Monad 的方向

同时追求性能和去中心化

## 主要方式

### 1\. 降低节点硬件门槛

普通设备也可以运行节点

### 2\. 扩大 Validator 集合

传统共识：节点越多，通信成本越高。

MonadBFT：Fan-out → Fan-in

模式

Leader 分发区块

↓

验证者投票

↓

汇总结果

优势：通信成本不会随着节点数量快速增长。

# 五、智能合约

## 智能合约

\> 部署在区块链上的自动执行程序。

## 传统应用

服务器运行代码

### 特点：

公司控制

可以修改

可以关闭

## 智能合约

区块链运行代码。

### 特点

规则公开

自动执行

难以修改

不依赖单一服务器

类似：

自动售货机。

规则提前写好

投入资金 → 满足条件 → 自动执行。

# 六、Monad 与智能合约

Monad 使用**EVM**（Ethereum Virtual Machine，以太坊虚拟机）

**Monad = EVM Compatible**

Ethereum 上的：Solidity 合约，开发工具和开发经验可以直接迁移到 Monad。

## 优势

开发者不用重新学习新的合约语言和生态。

# 七、MON Token

## Monad 原生代币：

**MON**

## 用途

### 1\. Gas Fee

交易手续费。

例如：

转账

调用合约

部署应用

### 2\. Staking

质押 MON：帮助维护网络安全，获得奖励
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
