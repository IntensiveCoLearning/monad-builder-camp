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
# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
**📝 Dev 产出模板：AI-assisted Dev Plan**

# **🛠️ AI-assisted Dev Plan**

## **1\. 产品概念 (一句话)**

**\[如：一个网页，用户连钱包后，能向指定地址匿名发送一条消息并上链。\]**

## **2\. 核心用户路径**

**连接钱包 -> 输入文本 -> 点击发送 -> MetaMask 弹窗 -> 页面显示成功**

## **3\. 边界控制 (三栏表)**

-   **Real: 1. 合约存储消息 2. ethers.js 调用**
    
-   **Mock: 1. 页面 UI 极简，无 CSS 框架 2. 不做消息列表查询功能**
    
-   **Out of Scope: 1. 加密功能 2. NFT 化消息**
    

## **4\. 必读文档 (只读这些)**

-   **Monad Docs: RPC 节点怎么填**
    
-   **ethers.js: Provider 怎么实例化**
    

## **5\. AI 协作边界**

-   **AI 做: 生成合约基础框架、写前端 fetch 逻辑、帮我找 Remix 报错原因。**
    
-   **我做: 决定架构、审核合约有没有漏洞、手动在 MetaMask 里确认交易。**
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->

**1\. 什么是 AI-assisted Web3 Prototype？** 你不再是苦力，而是**架构师 + 审计员**。你用 Cursor/ChatGPT 写脚手架、生成基础合约、解释报错；你负责把模块拼起来，并确保不把私钥喂给 AI。

**2\. 终极心法：三栏 Scope 控制表** 在写任何代码前，必须把功能填入下面三个盒子里。**这是本周不熬夜、能交差的唯一保障。**

| 边界类型 | 定义 | 举例（假设做“链上投票 DApp”） |
| --- | --- | --- |
| ✅ Real (真实现实) | 必须写真实逻辑，这是本周核心交付物。 | 1. 写 Solidity 投票合约并部署到 Monad。2. 前端用 ethers.js 真正调用合约投票。 |
| 🧱 Mock (模拟/写死) | 假装能跑，底层是假数据。为了展示完整流程，但不花时间做底层。 | 1. 候选人列表不动态读取，直接前端写死 ["Alice", "Bob"]。2. 用户的钱包余额，直接写死为 "100 MON"。 |
| 🚫 Out of Scope (绝不碰) | 本周绝对不碰。哪怕你觉得“加个登录界面也就 5 分钟”。 | 1. 复杂的代币经济学（如按持币量加权）。2. 后台管理系统。3. 精美的 UI 动画。 |

**3\. 推荐的最小技术栈 (Week 2 限定)**

-   **合约**：Solidity + **Remix IDE**（别折腾本地 Hardhat/Foundry 环境配置了，Remix 能省 80% 时间）。
    
-   **前端**：原生 HTML/JS，或者最简单的 React (Vite)。
    
-   **交互库**：**ethers.js**（文档最多，AI 生成代码准确率最高）。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->


**1\. 活动定义的 5 个核心锚点**

-   **活动目标**：是为了解决“新人不懂怎么用钱包”（教育），还是“给新上线的测试网引流”（传播）？
    
-   **目标听众**：千万不要说“面向所有 Web3 爱好者”。要精确到：“有 MetaMask 钱包，但没在 Monad 测试网发过交易的开发者”。
    
-   **核心主题**：标题即主题。❌《Web3 的未来》；✅《30 分钟搞懂 Monad 并行执行如何提升 DApp 速度》。
    
-   **关键动作 (CTA)**：听众听完必须做一件事。是扫码进群？是去 Faucet 领水？还是提交一个合约地址？
    
-   **执行边界**：明确说明“本次只做 1 小时线上语音，不做线下、不做视频回放剪辑”。
    

**2\. 基础闭环流程** `预热（发推预告+报名链接）` → `参与（开场抛出核心问题）` → `互动（Q&A环节引导听众发言）` → `反馈（散场前再次强调CTA）` → `复盘（统计报名转化率）`

> 📝 **Ops 产出模板：Space 策划 Brief**
> 
> -   **活动名称**：\[精准主题\]
>     
> -   **唯一 CTA**：\[听众离开页面时需要完成的动作\]
>     
> -   **听众参与路径草图**：`看到推文` -> `点击 Luma 链接` -> `添加日历提醒` -> `准时进入 X Spaces` -> `听到特定口令` -> `在 Discord 频道提交答案领勋章`
>
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->



**1\. “两条线”研究法**

-   **线 A（规范线）**：读 EIP（以太坊改进提案）或 MIP（Monad 改进提案）。理解底层的“游戏规则”是怎么被制定出来的。
    
-   **线 B（产品线）**：从 DefiLlama Top TVL 中挑一个协议（如 Uniswap、Morpho），研究真实产品是怎么落地的。
    

**2\. 认知护城河：区分事实与判断** 在写 Research 时，必须清晰地给自己的每一句话打标签，这是专业度的体现：

-   **事实**：“Uniswap V3 引入了集中流动性机制。”（无可争议）
    
-   **数据**：“该协议当前 TVL 为 45 亿美金。”（可验证）
    
-   **假设**：“如果 Gas 费下降 90%，该协议的日活可能会增加 3 倍。”（有逻辑前提，待验证）
    
-   **推断**：“基于过去 3 个月的资金流向，主力资金正在从 L1 转移至 L2。”（基于数据的延伸）
    
-   **个人判断**：“我认为 Morpho 的杠杆模型优于 Aave，值得长期持有。”（主观观点）
    

> 📝 **Research 产出模板：Research Question Card**
> 
> -   **研究对象**：\[如：Morpho Blue\]
>     
> -   **核心研究问题 (RQ)**：\[如：Morpho Blue 是如何通过“孤立风险池”设计来降低坏账风险的？\]
>     
> -   **资料边界（只看这 5 个来源）**：1. Morpho 官方白皮书 2. DefiLlama 数据面板 3. 两篇 Messari 深度研报 4. 以太坊区块浏览器 5. Monad Docs（对比视角）
>
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->




![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/SelinaAnn/images/2026-07-14-1784037939936-image.png)
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->





# **🛠️ AI-assisted Dev Plan**

## **1\. 产品概念**

-   **名字**：\[给起个酷炫或好记的名字，如 Monad-Gas-Tracker\]
    
-   **一句话描述**：\[用户能在这个页面上做一件什么事？如：用户输入一个地址，能看到该地址在 Monad 测试网上的历史交易记录。\]
    

## **2\. 用户核心路径**

1.  用户连接 MetaMask 钱包。
    
2.  用户点击 \[某个按钮\]。
    
3.  页面展示 \[某个结果\]。
    

## **3\. 技术组件清单**

-   **智能合约**：\[需要吗？如果需要，合约里存什么状态？如果不需要，写“无合约，纯前端读取链上数据”\]
    
-   **前端页面**：\[比如：1个输入框 + 1个查询按钮 + 1个展示列表\]
    
-   **链上交互**：\[比如：使用 ethers.js 调用 Monad RPC 的 `getTransactionCount`\]
    

## **4\. 边界控制 - _最重要的一步！_**

-   **✅ Real (本周真做)**：
    
    -   1.
        
    
-   **🧱 Mock (本周假装做了)**：
    
    1.  \[例如：页面头部的 Logo 和导航栏，直接写死，不做路由跳转\]
        
    
    2.  \[例如：不连接钱包时，展示的示例数据直接写死在代码里\]
        
-   **🚫 Out of Scope (本周绝不碰)**：
    
    1.  \[例如：多链切换功能\]
        
    
    2.  \[例如：交易详情的深度解析\]
        

## **5\. 本周必读文档**

-   \[粘贴链接，如：Monad RPC 节点连接文档\]
    
-   \[粘贴链接，如：ethers.js 官方 Provider 文档\]
    

## **6\. AI 协作策略**

-   **让 AI 做**：生成前端 React 组件脚手架、写 CSS 样式、提供 ethers.js 连接钱包的标准代码片段。
    
-   **我自己做**：将 AI 生成的碎片代码拼装起来、在 Remix 中手动测试合约逻辑、排查 RPC 连接不上的网络问题。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->






![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/SelinaAnn/images/2026-07-12-1783819303776-image.png)
<!-- DAILY_CHECKIN_2026-07-12_END -->

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
