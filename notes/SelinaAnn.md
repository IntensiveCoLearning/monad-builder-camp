---
timezone: UTC+8
---

# SelinaAnn

**GitHub ID:** SelinaAnn

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
### **Remix IDE 实战指南**

[**Remix IDE**](https://remix.ethereum.org/) 是一个基于浏览器的集成开发环境，无需安装，是学习和快速原型开发的首选工具。

**详细操作步骤：**

1.  **编写与编译**：
    
    -   在 Remix 的文件管理器中，创建一个新文件 `SimpleStorage.sol`。
        
    -   将你（经过审计的）合约代码粘贴进去。
        
    -   打开左侧 **“Solidity Compiler”** 插件。确保编译器版本与代码中的 `pragma` 声明匹配。
        
    -   点击 **“Compile SimpleStorage.sol”**。成功后，你会看到绿色的对勾，并在下方看到生成的 **ABI** 和 **字节码**（Bytecode）。
        
2.  **部署到 Monad Testnet**：
    
    -   打开左侧 **“Deploy & Run Transactions”** 插件。
        
    -   **环境 (Environment)**：选择 **“Injected Provider - MetaMask”**。你的 MetaMask 应该会弹窗，确保你已连接到 **Monad Testnet** 并拥有测试币。
        
    -   **合约 (Contract)**：选择你刚刚编译的 `SimpleStorage`。
        
    -   点击 **“Deploy”**。MetaMask 会弹窗，确认交易并支付 Gas（使用测试币）。
        
    -   **成功！** 在下方“Deployed Contracts”区域，你会看到你的合约实例，旁边就是它的**合约地址**。**请务必记录下这个地址。**
        
3.  **交互与验证**：
    
    -   展开你部署的合约，你会看到：
        
        -   `storedData`：一个蓝色按钮（Read）。点击它，初始值应为 `0`。
            
        -   `set`：一个橙色按钮（Write）。在输入框中输入一个数字（如 `42`），然后点击 `set`。MetaMask 会再次弹窗确认交易。
            
        -   交易确认后，再次点击蓝色的 `storedData`，你应该能看到返回值变成了 `42`。
            
    -   **在区块浏览器中验证**：复制你调用 `set` 函数时产生的 **Transaction Hash**，前往 [**Monad Block Explorer**](https://docs.monad.xyz/tooling-and-infra/block-explorers) 进行查询。在交易详情的 **“To”** 字段，你应该能看到你的**合约地址**（而不是一个普通钱包地址）。这证明你成功与链上合约进行了交互。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->

### **从“交易”到“合约”的思维跃迁**

在 DAY 3，你通过钱包发起了一笔**转账交易**。今天，你将部署一个**智能合约**，它本质上是一段**自动执行、不可篡改的代码**，被永久存储在区块链上。理解以下核心概念的关联，是今天学习的关键。

| 概念 | 它是... | 作用 | 在今天实践中的体现 |
| --- | --- | --- | --- |
| 合约源码 | 你编写或AI生成的 .sol 文件（如 SimpleStorage.sol）。 | 定义合约的逻辑、状态变量和函数。 | 你在 Remix 中编辑和查看的代码。 |
| ABI (Application Binary Interface) | 合约的 “使用说明书” 或**“接口字典”**。 | 以 JSON 格式精确描述了合约有哪些函数、每个函数的参数类型和返回值类型。 | Remix 在编译后自动生成。钱包和 DApp 前端需要通过它来知道如何与你的合约对话。 |
| 合约地址 | 合约部署到区块链后生成的唯一链上标识。 | 指向这段特定代码和其存储状态的位置。类似于一个 DApp 的“网址”。 | 你在 Remix 中点击“部署”后，终端显示的绿色地址。 |
| Read/Write Function | 合约中定义的函数。 | view/pure 函数 (Read)：只读取链上数据，免费，不产生交易。非 view/pure 函数 (Write)：会修改链上状态（如变量值、转账），需要支付 Gas，并产生交易哈希。 | 在 Remix 的“Deploy & Run Transactions”面板中，蓝色按钮是 Read，橙色按钮是 Write。 |
| Transaction Hash | 一次 写入操作（状态变更） 的唯一凭证。 | 用于在区块浏览器中查询该操作的详情和最终状态。 | 你调用一个写入函数后，MetaMask 弹窗确认后生成的哈希值。 |

> 💡 **核心关系链**： `你的 Solidity 源码` --(编译)--> `ABI + 字节码` --(部署交易)--> `合约地址`  
> `用户/前端` --(通过 ABI 构造调用数据)--> `钱包签名` --(发送交易到合约地址)--> `产生新的交易哈希`
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->


![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/SelinaAnn/images/2026-07-09-1783601532564-image.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->



### **硬核解读：用 Block Explorer 拆解交易**

Block Explorer（区块浏览器）是链上的“搜索引擎”，它将底层区块链的数据翻译成人类可读的格式。将你的 Tx Hash 粘贴到 Monad 的 Block Explorer 搜索框中，你将看到如下核心字段（这是今天的**硬核知识点**）：

| 字段名 | 含义 | 今天这笔交易中的表现 |
| --- | --- | --- |
| Transaction Hash | 交易的唯一标识符。 | 你查询的那串字符。 |
| Status | 交易状态。 | 应为 Success（成功）。如果失败，会显示 Reverted 并给出原因。 |
| Block | 交易被打包进哪个区块。 | 一个数字（例如 #10245）。数字越大代表越新。 |
| From | 交易的发起方（签名者）。 | 你的课程专用钱包地址。 |
| To | 交易的接收方。(注意：如果是与智能合约交互，这里显示的是合约地址) | 你转账的对方地址 / DApp 的合约地址。 |
| Value | 转移的原生代币数量。 | 你转账的 MON 数量（如果是 0，说明是单纯的合约调用）。 |
| Transaction Fee | 你实际支付的手续费。 | Gas Used × Gas Price。虽然你用的是测试币，但这里的计算逻辑与主网完全一致。 |
| Gas Limit | 你愿意为这笔交易支付的最大 Gas 数量（由发起者设置）。 | 转账通常是 21,000。如果调用了复杂合约，这个数值会很高。 |
| Gas Used | 这笔交易实际消耗的 Gas 数量。 | 必须 ≤ Gas Limit。未用完的 Gas 会退还。 |

> 💡 **理解 Gas 的绝佳时机**： 在 Explorer 中对比 **Gas Limit** 和 **Gas Used**。你会发现普通转账的 Gas Used 刚好是 21,000。这就解释了为什么以太坊将基础转账手续费固定在这个数值——这是执行一次最简单转账所需的计算步骤数。

* * *
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->




![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/SelinaAnn/images/2026-07-07-1783431460944-image.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->





![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/SelinaAnn/images/2026-07-06-1783336548272-image.png)
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
