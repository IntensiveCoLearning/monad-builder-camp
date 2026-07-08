---
timezone: UTC+8
---

# honora888

**GitHub ID:** honora888

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 一、Web3 国内合规与全维度法律安全

## 1.1 国内核心监管底线（硬性禁令）

1.  全面禁止 ICO/IEO/IDO 代币发行融资，任何带流通、融资属性的积分 / 治理 Token 均认定非法金融活动；空投、白名单募资同样涉非法集资。
    
2.  禁止境内虚拟货币交易所、撮合服务；禁止境外平台向中国居民提供交易服务。
    
3.  虚拟货币**无法定货币地位**，不得用于支付、薪资结算；挖矿业务全面关停淘汰。
    
4.  核心刑事罪名全覆盖（技术人员无法以 “仅写代码” 免责，属共犯）：
    
    | 罪名 | 典型业务模式 | 典型判例 |
    

| 非法吸收公众存款罪 | 矿机投资、积分兑换代币、保本理财 | AIP 平台矿机集资案，涉案 1.16 亿 |

| 开设赌场罪 | 链游充值抽奖、NFT 盲盒提现、虚拟币投注 | EOS BigGame 赌博平台，10 人均获刑 |

| 组织、领导传销罪 | 多级推广、团队计酬、门槛费 + 拉人头返利 | PlusToken 资金盘传销案 |

| 非法经营罪 | USDT 跨境换汇、地下场外大额兑外币 | TW711 换汇平台，非法兑换 2.2 亿人民币 |

| 洗钱 / 帮信罪 | 场外 OTC 收涉诈 / 涉赌资金、协助资金跨境洗白 | 虚拟货币跨境转移诈骗赃款洗钱案 |

## 1.2 个人从业者两大高频风险

### （1）入职 Web3 行业用工风险

1.  境外项目无境内用工主体，无法缴纳五险一金，劳动合同大概率无效；仅 EOR 名义雇主模式可部分弥补劳动关系，但纠纷认定仍存隐患。
    
2.  薪资风险：以 USDT / 项目原生 Token 发薪违反《劳动法》（工资必须人民币）；代币价值归零、出金冻卡、接收黑钱涉嫌帮信。
    
3.  入职必查：项目是否面向大陆用户、有无多层返利、是否承诺保本收益、是否发行流通代币。
    

### （2）个人资产与场外交易安全

1.  C2C/OTC 出金极易收到诈骗、赌博赃款，直接银行卡冻结，需全额退赔涉案资金。
    
2.  禁止参与 “高价出 U”、地下换汇、协助他人大额代币划转，规避洗钱共犯风险。
    
3.  留存所有资金来源凭证，仅使用小额、可信渠道兑换，薪资优先协商人民币发放。
    

## 1.3 网络安全通用攻击（学生 / 开发者高频中招）

1.  **钓鱼攻击（最高发）**
    
    -   域名钓鱼：Unicode 伪字符域名（如 [trẹzor.com](http://xn--trzor-o51b.com)≠[trezor.io](http://trezor.io)，Punycode 伪装）仿冒官方钱包、交易所；
        
    -   社交钓鱼：Telegram/Discord 冒充 HR、学长、客服，发送 “面试专用软件”“空投领取链接”；
        
    -   签名钓鱼：网页要求钱包签名验证身份，实则授权盗走全部资产。
        
2.  **木马 / 剪贴板劫持 Clipper**
    
    下载论文工具、面试安装包植入木马，复制钱包地址自动替换为黑客地址，转账直接被盗。
    
3.  **供应链风险** 恶意浏览器插件、开源依赖包后门，批量窃取私钥与 Cookie。
    

## 1.4 通用防护清单

1.  面试仅使用 Zoom / 腾讯会议，拒绝任何 “定制面试客户端”；
    
2.  空投、福利绝不使用主钱包，仅隔离测试钱包交互；
    
3.  软件 / 插件仅官方商店下载，限制浏览器插件数量；
    
4.  钱包开启 2FA，助记词离线手写保存，绝不截图、上传云端；
    
5.  转账前完整核对地址，禁止仅看前 / 后几位字符。
    

# 二、区块链底层核心概念

## 2.1 区块链基础运行逻辑

1.  **创世状态 Genesis State**：区块链网络第一次上线的初始空白账本（Ethereum2015、Monad2025）。
    
2.  **节点 Node**：全球分布式存储完整账本的服务器，无单点故障；特殊节点**验证者 Validator**质押代币打包区块，作恶会罚没资产（Slashing）。
    
3.  **区块 & 不可篡改**：交易批量打包为区块，密码学链接上一区块；修改任意区块，后续全部区块失效。Monad 出块速度 400ms，以太坊约 12s。
    
4.  **共识机制（PoW/PoS）**
    
    -   PoW 工作量证明：比特币、早期以太坊，算力竞争出块，高能耗；
        
    -   PoS 权益证明：以太坊合并后、Monad 采用，质押代币参与验证，低能耗，作恶质押资产罚没。
        
5.  **最终性 Finality**：交易永久结算、不可撤销；Monad 亚秒级最终性（800ms），以太坊需数分钟。
    

## 2.2 Gas 手续费体系（所有 EVM 链通用）

1.  Gas = 链上计算单位，转账、合约调用均消耗；操作越复杂 Gas 越高。
    
2.  计算公式：`Gas Fee = Gas消耗数量 × Gas单价`
    
3.  作用：支付验证者报酬、防止链上垃圾交易；网络拥堵时 Gas 价格自动上涨。
    
4.  Monad 优势：高 TPS（10000TPS），区块空间充足，Gas 费用长期低廉。
    

## 2.3 开发必备工具集（测试网高频使用）

1.  **Chain ID**：区块链唯一编号，钱包区分网络核心标识；Monad 测试网 10143，主网 143。
    
2.  **RPC 远程调用接口**：钱包 / 程序与区块链通信的接口，分为公共免费 RPC、付费稳定 RPC（Alchemy/QuickNode，上线生产环境使用）。
    
3.  **Faucet 水龙头**：免费发放测试网原生代币，无真实价值，仅用于测试交易 Gas；Monad 官方水龙头：[faucet.monad.xyz](http://faucet.monad.xyz)。
    
4.  **Block Explorer 区块浏览器**：链上可视化查询工具，可查交易、地址、合约、区块，**测试网开发核心调试工具**。
    

## 2.4 主网 Mainnet vs 测试网 Testnet（重点）

| 维度 | 主网 Mainnet | 测试网 Testnet（如 Monad Testnet） |
| --- | --- | --- |
| 代币价值 | 原生币具备真实金融价值 | 测试代币免费领取，无市场价值 |
| 使用场景 | 正式上线、真实用户、真实资产交互 | 合约调试、DApp 开发、新手练手 |
| 风险 | 操作失误永久损失资金 | 无资金损失风险，可反复测试 |
| 重置规则 | 永久数据不可重置 | 项目方可定时重置（Monad 测试网 2025.12.16 创世重置） |

实操准则：**所有开发、实验、合约调试必须先在测试网完成，验证无误再部署主网**。

# 三、加密钱包完整体系与安全规范

## 3.1 钱包核心定义

钱包≠存储代币，代币永久存于区块链；钱包是**访问链上账户的密钥管理工具**，管理签名、地址、私钥。

## 3.2 账户三要素（安全核心）

1.  **地址 Address（0x 开头）**
    
    公钥哈希生成的公开标识，可随意分享，仅用于收款，无资产被盗风险。
    
2.  **私钥 Private Key**
    
    账户唯一控制权，等同于账户密码；持有私钥即可任意转账、调用合约，**绝对不能泄露给任何人**。开发者禁止将私钥提交至 GitHub 等公开仓库。
    
3.  **助记词 Seed Phrase（12/24 单词）**
    
    钱包总密钥，可一次性恢复钱包内全部账户；比单私钥更重要，正规项目永远不会索要助记词。
    
    保存规范：离线手写、多份异地存放，禁止截图、备忘录、云端存储。
    

## 3.3 钱包分类（安全等级从高到低）

1.  硬件钱包（冷钱包）：离线存储密钥，最高安全等级；
    
2.  桌面钱包；3. 浏览器扩展钱包（MetaMask/Rabby，开发最常用）；4. 移动端钱包；5. 网页热钱包（风险最高）。
    

## 3.4 开发钱包隔离规范（强制要求）

1.  拆分两套钱包：**开发专用测试钱包**（仅存测试网代币）、**资产主钱包**（存放真实资产，绝不用于开发交互）；
    
2.  开发全程仅使用测试网隔离钱包，杜绝主钱包连接测试 DApp、AI 生成合约、未知网站。
    

# 四、Monad 高性能 L1 公链实操手册

## 4.1 Monad 核心特性

1.  EVM 全兼容，原生支持以太坊所有合约、钱包、开发工具；
    
2.  五大架构优化：MonadBFT 共识、异步执行、并行交易、RaptorCast 区块传输、MonadDb 存储；
    
3.  性能指标：10000TPS、400ms 出块、800ms 快速最终性，解决以太坊拥堵高 Gas 痛点。
    

## 4.2 Monad 测试网核心参数（开发必存）

-   测试网名称：Monad Testnet
    
-   Chain ID：10143
    
-   原生代币：MON（测试网免费领取）
    
-   官方 RPC：[https://testnet-rpc.monad.xyz](https://testnet-rpc.monad.xyz)
    
-   区块浏览器：[testnet.monadscan.com](http://testnet.monadscan.com)、[testnet.monadvision.com](http://testnet.monadvision.com)
    
-   水龙头：[https://faucet.monad.xyz（每日领取限额）](https://faucet.monad.xn--xyz-jx3eo93fs9hju1flyd8fo428gra)
    
-   临时沙盒 Tempnet：Chain ID 20143，用于新功能测试，数据会频繁重置。
    

## 4.3 MetaMask 添加 Monad 测试网两种方式

1.  一键添加（推荐）：访问 ChainList 搜索 Monad，勾选 Include Testnets，点击 Add to MetaMask；
    
2.  手动填写网络参数（RPC、ChainID、货币符号、浏览器地址）。
    

## 4.4 Monad 区块浏览器功能

1.  Monadscan（Etherscan 同源）：合约验证、交易解析、地址资产查询；
    
2.  MonadVision：区块可视化、链上数据索引；
    
3.  配套工具：Tenderly、Blocksec Phalcon，深度解析交易调用栈、资金流向，**测试网调试漏洞必备**。
    

## 4.5 测试网完整实操流程

1.  创建隔离测试钱包，备份助记词；
    
2.  MetaMask 添加 Monad Testnet；
    
3.  水龙头领取测试 MON（支付交易 Gas）；
    
4.  Remix 部署合约至测试网；
    
5.  复制交易哈希 TX Hash，在 Monadscan 解析完整交易行为；
    
6.  反复迭代调试，确认无漏洞后再考虑主网部署。
    

# 五、以太坊链上交易原理 + 区块浏览器完整解析一笔测试网交易

## 5.1 一笔链上交易完整结构（EIP-1559 Type2 标准交易）

```
{
from: 发起钱包地址（支付Gas）,
to: 接收/合约地址,
nonce: 账户自增序列号，防止交易重放,
value: 转账原生币数量,
input data: 合约调用函数+参数（十六进制Calldata）,
gasLimit: 最大消耗Gas单位,
maxFeePerGas/maxPriorityFeePerGas: Gas价格上限、验证者小费,
signature: 私钥签名，证明交易为本人发起
}
```

### 交易三大类型

1.  原生币转账：to 为普通钱包地址，input data 为空；
    
2.  合约部署：to 字段为空，input data 为合约字节码；
    
3.  合约交互：to 为合约地址，input data 存储调用函数与参数。
    

## 5.2 区块浏览器拆解 Monad 测试网交易（实操步骤）

### 步骤 1：定位交易核心标识

交易哈希 TX Hash：唯一 “快递单号”，全局唯一，复制至 Monadscan 搜索。

### 步骤 2：基础状态与参与方

1.  Status 状态：Pending（待打包）/Success（执行成功）/Fail（执行失败，Gas 消耗不退）；
    
2.  From：交易发起者，钱包地址；
    
3.  To：普通地址 = 转账；Contract = 智能合约交互。
    

### 步骤 3：手续费与区块信息

-   Gas Used：实际消耗计算单位；
    
-   Gas Price：单价，总手续费 = Gas Used × Gas Price；
    
-   Block Number：打包进哪一个区块，区块高度越高，交易确认越稳固。
    

### 步骤 4：Input Data 输入数据（开发核心调试项）

浏览器自动解码 Calldata，展示：调用函数名、传入参数；

例：`transfer(address,uint256)`可直接看到转账接收地址、代币数量，用于校验合约逻辑是否符合预期。

### 步骤 5：Logs 事件日志（追踪代币变动）

合约执行时触发的 Transfer、Approval 等事件，清晰展示：

-   ERC20 代币转账记录；
    
-   NFT 铸造 / 交易；
    
-   授权额度变更（高危：无限制授权会导致资产被盗）。
    

### 步骤 6：Internal Transactions 内部交易

合约内部互相调用产生的资产划转，普通转账不显示，DeFi 交换、质押逻辑必须查看此项，排查资金流转漏洞。

## 5.3 测试网浏览器开发调试用途

1.  校验合约部署是否成功，核对合约地址；
    
2.  验证函数调用参数是否传入正确；
    
3.  查看交易失败原因（Gas 不足、逻辑漏洞、权限错误）；
    
4.  检查代币授权是否过多，及时清理高危 Approval 授权；
    
5.  追踪完整资金流向，复现漏洞攻击路径。
    

# 六、链上 Vibe Coding（AI 氛围编程）安全注意事项（重点拓展）

## 6.1 Vibe Coding 定义

依靠大模型快速生成智能合约、Web3 前端代码，仅追求功能可运行，无严谨审计、底层逻辑校验、安全加固的快速开发模式。**区块链场景风险远高于传统 Web 开发**。

## 6.2 链上 Vibe Coding 致命底层风险

1.  **代码不可篡改，漏洞永久生效**
    
    传统 Web 后端可热更新修复漏洞；智能合约部署上链后无法修改，AI 生成的重入、整数溢出、权限漏洞会永久被黑客利用，资产永久丢失。
    
2.  AI 优先功能，完全忽略安全规范
    
    -   硬编码私钥、RPC 密钥、管理员权限明文写入代码，提交 GitHub 直接泄露；
        
    -   缺失重入锁、访问控制、转账校验、溢出防护；
        
    -   无授权检查，任何人可调用管理员核心函数。
        
3.  逻辑模糊，无完整业务校验
    
    AI 仅实现表层功能，忽略边界条件：零值转账、超大额度、黑名单、暂停机制，测试网看似正常，主网大额资金直接被盗。
    
4.  依赖不安全开源库
    
    AI 随机选用未审计、存在后门的合约库，供应链攻击风险极高。
    
5.  调试黑盒化，无法追溯根因
    
    多轮反复 prompt 迭代修复，代码无注释、架构混乱，测试网出现异常时难以定位漏洞。
    

## 6.3 测试网 Vibe Coding 强制安全操作规范

1.  **环境隔离红线**
    
    AI 生成合约仅部署**测试网**，绝不直接部署主网；全程使用独立空白测试钱包，不导入任何持有真实资产的私钥。
    
2.  分层校验流程（缺一不可）
    
    ① 人工通读全部合约代码，标记权限、转账、铸币核心逻辑；
    
    ② 本地 Remix/Hardhat 单元测试，覆盖边界场景（0 金额、超大额度、重复调用）；
    
    ③ 部署 Monad 测试网，通过 Monadscan 解析每一笔交互交易，校验事件、资金流向；
    
    ④ 使用 Tenderly/Phalcon 做漏洞自动化扫描，检测重入、溢出、未授权调用；
    
    ⑤ 模拟攻击测试（尝试越权调用、大额套利），确认无漏洞。
    
3.  密钥与代码管理
    
    -   私钥、RPC 地址、管理员地址全部存入环境变量，禁止硬编码；
        
    -   代码提交前扫描密钥泄露，绝不上传助记词 / 私钥至代码仓库；
        
    -   不使用 AI 生成的任意前端连接主钱包，仅测试钱包交互。
        
4.  授权安全管控
    
    AI 生成 DApp 极易出现无限授权逻辑，测试网交互后，通过 Revoke 工具批量取消合约授权，避免授权漏洞迁移至主网。
    
5.  绝对禁止行为
    
    -   直接将 AI 生成未审计合约部署主网；
        
    -   使用持有真实资产的钱包签名 AI 生成未知交易；
        
    -   复制 AI 提供的陌生 RPC、水龙头、合约地址，不核验官方文档。
        

## 6.4 安全开发替代方案

1.  优先使用开源审计过的标准合约模板（OpenZeppelin），再让 AI 填充业务逻辑；
    
2.  合约完成后必须人工审计 + 自动化工具扫描双重校验；
    
3.  测试网完整模拟全量业务流程，复现所有用户操作场景，确认无异常后再考虑主网部署。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->


# 延续2026/7/6——

# 三、NFT 入门实操笔记

## 1\. NFT 基础概念

NFT = Non-Fungible Token 非同质化代币

-   同质化代币（ETH/BTC）：可等价互换、可分割
    
-   NFT：唯一标识、不可拆分、一对一确权（头像、艺术品、游戏道具、会员凭证）
    

## 2\. NFT 四大价值

1.  所有权链上确权，平台无法删除没收资产
    
2.  交易全程透明可追溯，版权验证成本极低
    
3.  跨平台流通（游戏资产不再绑定单一游戏）
    
4.  衍生价值：社区门票、版权分润、实体资产代币化
    

## 3\. 完整 Mint 铸造六步流程

1.  安装 MetaMask 浏览器钱包，保存 12 位助记词（资产唯一凭证，丢失无法找回）
    
2.  学习 Web3 安全：钓鱼、私钥泄露、恶意签名风险
    
3.  钱包存入 ETH，用于支付 Gas 手续费
    
4.  DApp 点击 Connect Wallet 连接钱包（等同于 Web3 登录）
    
5.  选择素材组装 NFT，确认 Gas 并签名交易，上链铸造
    
6.  在钱包 / 市场查看、交易 NFT
    

## 4\. Web3 钱包安全核心规则

1.  助记词、私钥绝不分享、不上传云端
    
2.  冷钱包（硬件）存大额资产，热钱包仅小额日常使用
    
3.  陌生网站签名、空投授权谨慎，极易被盗资产
    
4.  区分官方域名，警惕仿冒钓鱼网站
    

## 5\. 行业术语

-   DYOR：自行研究项目，勿盲从热点
    
-   FOMO：踏空焦虑，追高风险
    
-   FUD：恐慌情绪，低位抛售
    

* * *

# 四、Vibecoding AI 开发新模式

## 1\. 定义（2025 诞生）

无需手动写代码，以**自然语言描述需求**，AI 智能体自动完成全流程开发；人是产品指挥官，AI 是工程团队。

### 聊天机器人 vs AI 智能体

-   聊天机器人：仅返回文字回答，无操作能力
    
-   AI 智能体：创建文件、编写代码、运行调试、自动修复 Bug
    

## 2\. 标准开发循环（核心流程）

描述需求 → AI 构建代码 → 预览效果 → 反馈修改 → 迭代重复

## 3\. 主流开发工具

1.  Replit（浏览器云端沙箱，新手首选，隔离本地文件）
    
2.  OpenAI Codex / Claude Code（终端本地 AI 编码）
    

## 4\. 安全红线：提示注入攻击

攻击者在素材、文件中隐藏劫持指令，控制 AI 读取本地文件、泄露密钥；

### 安全最佳实践

1.  全部在云端沙箱运行 AI，禁止本地电脑直接运行智能体
    
2.  绝不向 AI 输入真实私钥、API 密钥、账号密码
    
3.  AI 生成代码上线前人工完整审查
    

## 5\. 能力边界

### 可实现

网站、DApp 前端、数据库配置、交互组件、Bug 修复、Web3 页面开发

### 局限

无法自动保证生产级安全；复杂业务仍需持续迭代；不能替代需求设计能力

## 6\. 核心技能：高质量提示词 Prompt

差示例：帮我做 NFT 网站

优质示例：深色主题 NFT 铸造 DApp，集成 MetaMask 连接，上传图片生成 ERC721，页面适配手机，使用 Ethers.js 交互以太坊。

* * *

# 五、完整 Web3 技术栈汇总

## 1\. Web2 传统开发栈

`React + Node.js + MySQL`

## 2\. Web3 去中心化 DApp 开发栈（核心）

`React（前端页面） + Ethers.js（链交互工具） + Solidity（智能合约后端） + IPFS（去中心化文件存储）`

分工拆解：

1.  React：用户可视化界面、按钮交互
    
2.  Ethers.js：连接 MetaMask、读写智能合约、发送链上交易
    
3.  Solidity：部署以太坊的业务逻辑（转账、NFT、交易）
    
4.  IPFS：存储图片、元数据、前端静态资源，替代阿里云等中心化服务器
    

## 3\. Web 3.0 语义网栈

`Python + RDFLib + SPARQL`（W3C、DBpedia 知识图谱体系）

* * *

# 六、高频核心术语速查表

1.  分布式账本：全网多节点同步完整账本
    
2.  哈希：数据唯一指纹，篡改即变更
    
3.  共识机制：PoW 挖矿 / PoS 质押验证
    
4.  验证者：PoS 网络自动运行程序记账节点
    
5.  智能合约：链上自动执行代码，DApp 后端
    
6.  Gas：以太坊交易计算手续费
    
7.  EVM：以太坊运行合约的虚拟机
    
8.  Layer2：以太坊扩容二层网络，低手续费
    
9.  MetaMask：ConsenSys 出品主流 Web3 钱包
    
10.  Uniswap：以太坊头部去中心化交易所 DEX
     
11.  IPFS：分布式文件存储协议
     
12.  NFT：链上唯一确权数字资产
     
13.  Vibecoding：自然语言驱动 AI 自动编码
     
14.  DApp：去中心化区块链应用
     
15.  Slashing：PoS 验证者作恶罚没质押 ETH
     

# Web3 远程工作 + 岗位体系

# 第一部分：Web3 底层基础 —— 远程协作体系（行业通用能力底座）

## 一、Web3 远程办公核心特征

去中心化组织天然全球化协作，团队跨多时区（Seed Club 核心团队覆盖 12 个时区），全部依赖异步工具完成千万级项目孵化；无固定线下工位，异步沟通、信息留痕、时区管理是生存必备能力。

## 二、Web3 全栈工具清单（分协作 / 链上 / 效率三大类）

### （一）社群 & 即时沟通（社区 / DevRel 高频）

1.  **X（原 Twitter）**：行业信息发布、开发者发声、AMA、项目公告、开发者流量阵地
    
2.  **Telegram**：项目群、开发者私聊、频道推送；⚠️核心风险：手机号泄露、钓鱼诈骗
    
    -   隐私设置：手机号可见设为「无人」，禁止陌生人通过手机号检索账号
        
    -   反诈要点：官方不会私聊发链接 / 索要验证码，陌生 exe/apk 文件一律不打开
        
3.  **Discord**：开发者服务器、分频道管理（公告 / 开发答疑 / 闲聊 / 举报），ETHGlobal 等黑客松标配；规则：禁止私下发 ETH、绝不泄露私钥
    

### （二）会议 & 日程（跨时区远程必备）

1.  Zoom：视频会议、屏幕共享、会议纪要、虚拟背景；配套 Zoom Docs 做同步文档
    
2.  Calendly：智能预约工具，自动时区转换、联动 Zoom 生成会议链接，用于开发者对接、线下 meetup 约访、面试
    

### （三）协作 & 文档管理

1.  Notion：团队知识库、OKR、项目计划、开发者教程、活动 SOP 沉淀
    
2.  GitHub：代码托管、开源 Builder 协作、Issue 反馈、PR 代码评审（DevRel 必备）
    
3.  Figma：DApp 前端 UI、开发者 Demo 原型、活动物料设计
    

### （四）链上钱包 & 数据工具（所有生态岗通用）

1.  MetaMask：Web3 标准钱包，DApp 交互基础；仅 Chrome/Firefox 商店安装，拒绝第三方安装包
    
2.  链上数据：CoinGecko、CoinMarketCap、DefiLlama、RootData（生态 BD/DevRel 做竞品、项目调研）
    

### （五）AI 效率辅助

ChatGPT、DeepSeek、豆包：技术文档翻译、教程撰写、活动文案、会议纪要整理

## 三、Web3 远程标准化工作习惯（DevRel / 生态连接者核心工作方法）

### 1\. 异步沟通 + 全链路信息留痕（重中之重）

-   聊天只做即时同步，**所有决策、需求、开发者反馈沉淀 Notion/GitHub Issue**
    
-   内容强制标注时区 + 响应时效：`UTC+8，24小时内回复`
    
-   会议结束 24 小时内输出纪要：明确行动项、DRI 负责人、截止时区时间
    

### 2\. OKR 目标管理（分布式团队统一对齐）

适用生态、DevRel、开发全团队，核心逻辑：**重结果、不重在线时长**

-   季度目标 3-5 个，KR 量化可追踪，理想完成度 60%-70%
    
-   DevRel 典型 OKR 示例：
    
    Objective：提升公链开发者生态规模
    
    KR1：完成 12 场线上开发者 workshop，累计开发者参与≥2000 人
    
    KR2：孵化 8 个优质 Builder 项目接入主网，获得 Grant 资助
    
    KR3：开发者文档访问量提升 60%，社区技术提问减少 40%
    

### 3\. 跨时区会议规范（对接全球 Builder 必备）

1.  预约三要素：清晰议题、预期产出、精准参会人；时长控制 15/30/60 分钟
    
2.  优先选择团队**重叠工作时段**，避开各国深夜 / 午休
    
3.  需求拒绝模糊表述：
    
    ❌尽快出教程
    
    ✅UTC+0 6.15 前输出 Viem 交互 Demo 文档，附 GitHub 示例仓库
    

### 4\. 职场通用软技能（生态连接者加分项）

1.  中英文排版规范：中英文间加空格、中文全角标点、数字半角
    
2.  汇报万能结构：进展 + 卡点 + 解决方案 + 所需资源（对接内部技术、外部 Builder 通用）
    
3.  需求沟通模板：先确认诉求，再提供分档交付方案，降低协作冲突
    

## 四、Web3 行业基础黑话（DevRel / 生态日常高频）

### 1\. 社区通用

gm/gn（早晚问好）、Anon 匿名用户、DAO 去中心化自治组织、DYOR 自行研究、FOMO 踏空焦虑、WAGMI 一起致富

### 2\. 开发 / 生态术语

L1/L2 主链 / 扩容、EVM 以太坊虚拟机、Mint 铸造、Bridge 跨链桥、Gas 手续费、RPC 节点接口、Grant 生态资助金、Hackathon 黑客松

### 3\. 生态建设相关

Builder 链上开发者、Alpha 内部项目机会、Tokenomics 代币经济、Rug 项目跑路

# 第二部分：Web3 全岗位分层体系（技术岗 + 非技术岗）

## 一、技术岗（底层基建，DevRel 必须具备基础认知）

1.  **前端工程师**
    
    技术栈：React/Vue、TypeScript、Viem/Wagmi（行业主流，替代旧 Ethers.js）、MetaMask 钱包集成
    
    职责：DApp 页面、钱包交互、链上数据渲染、开发者 Demo 示例开发
    
2.  **后端工程师**
    
    技术栈：Node/Go/Python、GraphQL、数据库、消息队列
    
    职责：链上数据拉取、开发者 API、Grant 后台、活动数据统计
    
3.  **智能合约工程师**
    
    技术栈：Solidity、Foundry/Hardhat、ERC20/721 标准
    
    职责：协议合约开发、测试、Gas 优化，DevRel 需能读懂合约、写简易 Demo
    
4.  **合约审计工程师**：漏洞检测、重入 / 权限攻击排查，输出安全报告
    

## 二、非技术岗（生态 / DevRel / 社区核心赛道）

1.  产品运营：产品迭代、开发者增长指标、Go-to-market 上线策略
    
2.  **社区管理**：Telegram/Discord/X 社群运营、AMA 活动、用户活跃度维护
    
3.  行业研究：链上数据分析、竞品生态报告、代币经济模型测算
    
4.  **DevRel、生态 BD、Builder 孵化（本笔记核心重点）**
    

# 第三部分：核心角色深度解析 ——DevRel / Builder / 生态连接者

## 一、DevRel（开发者关系 / 开发者布道师）：协议与 Builder 的双向桥梁

### 1\. 核心定位

对内：代表全体 Builder，向产品 / 合约团队反馈开发痛点、优化 SDK、完善文档；

对外：协议技术代言人，降低开发者接入门槛，持续扩大 Builder 生态规模。

### 2\. 四大核心工作职责

（1）开发者教育与内容生产（基础工作）

-   产出技术内容：开发文档、实操教程、GitHub 示例 Demo、技术博客、X 长文科普
    
-   线上授课：Twitter Space、Discord Workshop、录播教学，拆解合约、Viem 集成、跨链交互
    
-   线下发声：ETHGlobal、Devcon、各地 Web3 Meetup 演讲，现场代码演示
    

（2）黑客松 & Builder 孵化（生态增长核心动作）

-   设计黑客松奖金、任务赛道，提供开发启动模板
    
-   活动全程实时技术答疑，赛后筛选优质项目对接 Grant、资源扶持
    
-   持续跟进获奖 Builder，推动项目主网上线、协议深度集成
    

（3）开发者社群维护

-   常驻 Discord/Telegram 开发者频道，处理合约报错、SDK 接入问题
    
-   沉淀高频问题知识库，同步技术团队修复工具缺陷
    
-   搭建开发者交流圈层，促成 Builder 之间互相合作
    

（4）内部产品反馈通道

记录开发者全量痛点：文档缺失、RPC 不稳定、合约 Gas 过高、SDK 兼容性问题

定期输出开发者体验报告，推动产品迭代优先级调整

### 3\. DevRel 硬性能力要求

1.  技术基础：能读懂 Solidity 合约、使用 Viem/Wagmi 编写简易 Demo、熟悉 GitHub 操作；无需审计级合约能力，但具备独立调试代码能力
    
2.  文字 & 表达：技术写作、公开演讲、通俗化拆解复杂链上概念
    
3.  工具熟练度：GitHub、Discord、Notion、Calendly、X、链上数据平台
    
4.  软素质：开发者同理心、跨时区异步沟通、活动全流程落地能力
    

## 二、Builder：Web3 生态建设者（DevRel 的核心服务对象）

### 1\. 定义

所有基于公链 / L2 协议开发 DApp、工具、基础设施的开发者团队 / 独立开发者，分为三类：

1.  底层 Builder：跨链桥、预言机、节点工具、链上数据分析工具开发者
    
2.  应用 Builder：DeFi、NFT、GameFi、DAO 治理、钱包 DApp 开发团队
    
3.  轻量 Builder：黑客松参赛独立开发者、插件、链上小工具创作者
    

### 2\. Builder 核心诉求（DevRel、生态连接者服务核心）

1.  低门槛开发：完善 SDK、清晰文档、开箱即用 Demo 模板
    
2.  资金扶持：生态 Grant、黑客松奖金、投融资对接
    
3.  流量曝光：官方 X 转发、社区 AMA、活动展位、生态合作联动
    
4.  技术支持：实时合约答疑、漏洞协助修复、链上部署指导
    

### 3\. Builder 与 DevRel 的双向价值

-   DevRel 服务 Builder → 更多应用部署，协议 TVL、生态活跃度提升
    
-   Builder 落地产品 → 提供真实使用反馈，反向优化协议底层功能
    

## 三、生态连接者（Ecosystem Lead / 生态 BD）：全生态资源枢纽

### 1\. 定位

介于 DevRel、项目方、资本、社区、公链之间的**全域连接器**，统筹生态增长，比 DevRel 业务边界更广，不局限于开发者服务。

### 2\. 三大核心工作模块

模块 1：Builder 生态孵化（和 DevRel 协同）

-   制定 Grant 发放规则，筛选优质 Builder 项目，跟进资金落地与项目进度
    
-   搭建 Builder 孵化体系：新手教程→黑客松→小额资助→深度战略合作
    
-   沉淀 Builder 资源库，持续维护长期合作开发团队
    

模块 2：跨项目生态合作（Web3 BD 核心）

-   挖掘互补项目：DeFi+NFT、L2 + 跨链桥、钱包 + 链上工具
    
-   策划双向联动：联合 AMA、联合黑客松、代币深度集成、流量互导
    
-   协调双方技术、市场、DevRel 团队对齐落地路径
    

模块 3：全域生态资源整合

对接交易所、投资机构、线下峰会、KOL、DAO 社区，为 Builder 提供曝光、融资、流量资源；

对内同步生态整体数据（新增 Builder 数量、集成项目数、生态 TVL），输出生态增长策略。

### 3\. 生态连接者 vs DevRel 核心区别

表格

| 维度 | DevRel（开发者关系） | 生态连接者（Ecosystem BD） |
| --- | --- | --- |
| 核心服务对象 | Builder 开发者 | 全生态：Builder、项目方、资本、社区、平台 |
| 核心能力侧重 | 技术讲解、开发支持、技术内容产出 | 商务谈判、资源整合、生态战略规划 |
| 产出成果 | 完善开发者工具、提升开发者留存 | 新增生态合作、孵化规模化 Builder 矩阵、生态规模扩张 |
| 技术要求 | 必须具备基础编码 / 合约阅读能力 | 懂基础链上逻辑即可，不强制写代码 |
| 协作关系 | 生态连接者的技术支撑，服务 Builder | 统筹 DevRel、市场、商务整体生态布局 |

## 四、三者协同工作闭环（Web3 生态增长标准流程）

1.  生态连接者：制定年度生态扩张计划、Grant 预算、黑客松活动规划，对接外部合作资源
    
2.  DevRel：落地开发者侧执行 —— 撰写教程、搭建 Demo、举办 Workshop、黑客松技术支持、收集 Builder 反馈
    
3.  Builder：基于协议开发产品，提出技术需求，完成链上部署，产出生态应用
    
4.  循环：DevRel 将 Builder 痛点同步产品团队；生态连接者基于 Builder 落地成果拓展更多行业合作，持续吸引新 Builder 入场
    

# 第四部分：入行学习路径总结

## 1\. 工具先行

熟练掌握 X、Telegram、Discord、GitHub、Notion、MetaMask、Calendly 全套远程协作工具，建立 Web3 工作环境。

## 2\. 夯实底层认知

弄懂 L1/L2、EVM、智能合约基础、Viem 开发库、链上基础数据指标，能看懂简单合约代码。

## 3\. 分角色能力搭建

-   想做 DevRel：重点练技术写作、Demo 开发、线上线下演讲、开发者社群答疑
    
-   想做生态连接者：重点练项目调研、商务沟通、活动统筹、资源整合，补充基础开发认知
    
-   想成为 Builder：深耕前端 / 智能合约开发，参与黑客松产出落地项目，是进入 DevRel / 生态岗的加分履历
    

## 4\. 远程工作必备习惯

坚持异步信息留痕、时区管理、OKR 结果导向、结构化沟通，适配 Web3 全球化分布式团队模式

## 风险提示（行业通用）

1.  Telegram、Discord 高频钓鱼，绝不泄露私钥、助记词、验证码；
    
2.  仅官方渠道下载 MetaMask 等工具，规避仿冒钱包盗资产；
    
3.  Web3 项目多远程异步协作，所有合作、资金、需求全部文档留痕，降低沟通纠纷。
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->




# **Blockchain Basics**

## 1\. Blockchain Definition

A decentralized distributed ledger made up of sequentially linked blocks. Transactions are stored and synchronized across all network nodes, making data transparent, traceable, and hard to tamper with.

Block Structure

Previous Block Hash: links to the previous block, forming a chain

Random Number: field for consensus computation

Batch of Transactions (up to around 4,000 per block)

Current Block Hash: fingerprint of the block’s data

Immutability Principle

Changing any historical transaction → current block hash changes → all subsequent block hashes break;

Changing only a few nodes’ ledgers is ineffective; you must control over 51% of nodes in the network to successfully tamper, which is very costly.

## 2\. Two Core Mechanisms: Chain Storage & Distributed Ledger

Distributed Network

Countless independent nodes (miners/validators) each store an identical complete ledger; no single center, so no single point of failure affects the network.

Node Incentives (Protocol Rewards) Nodes earn rewards for maintaining the network = block issuance rewards + user transaction fees (Gas fees).

## 3\. Three Main Types of Blockchain (from high to low decentralization)

| Public Chain | Anyone can join freely | Visible to everyone on the network | BTC, ETH, DeFi, NFT |

| Consortium Chain | Institutions approved by invitation | Only accessible to consortium members | Banking alliances, supply chain |

| Private Chain | Fully controlled by enterprise | Internal and closed data | Internal audits |

## 4\. Differences Between Web2 / Web 3.0 (Semantic Web) / Web3

Web2 (Current Internet)

Data resides on centralized servers like Alibaba, Tencent; platforms own user data, users only have usage rights; tech stack: React, Node, MySQL.

Web 3.0 (Semantic Web, W3C Standard)

Uses RDF and knowledge graphs (DBpedia) for machines to understand data, does not rely on blockchain; essentially an upgrade of traditional internet.

Web3 (Decentralized Internet)

Based on blockchain, smart contracts, and IPFS; users control digital assets and data via private keys; tech stack: React, Ethers.js, Solidity, IPFS; representative projects: Uniswap, MetaMask.

## 5\. Blockchain Pros and Cons

Advantages

No intermediary trust needed, no third-party guarantees Resistant to censorship, no platform bans Users have full ownership of digital assets Open source and anyone can develop DApps

Current Challenges

Public chains have low TPS and high fees during congestion Private keys are hard for average users to manage Anonymity brings compliance and money-laundering risks Smart contract code vulnerabilities can lead to asset theft

## 6\. Core Logic of Bitcoin (Blockchain 1.0)

Positioning: Decentralized digital gold, fixed total supply of 21 million coins, inflation-resistant

Consensus: Proof of Work (PoW), one block generated every 10 minutes

Anonymity Logic: Transactions only display random wallet addresses, not linked to individuals; once the address is exposed, privacy is compromised

Limitations: Only supports simple transfers, cannot execute complex programs, transaction speed is slow

## 7\. Complete Blockchain Operation Process

-   User wallet initiates a transaction (transfer / contract call)
    
-   Transaction is broadcast to all nodes in the network
    
-   Nodes verify signature, balance, and other legality
    
-   Consensus mechanism competes to pack a new block
    
-   Block is linked to the main chain, and the ledger is updated across the network
    
-   The node that successfully mines the block receives protocol rewards and transaction fees
    

# Ethereum Core System (Blockchain 2.0, World Computer)

## 1\. Ethereum Positioning

Ethereum, founded by Vitalik, is a programmable blockchain with smart contracts as its core innovation, supporting DeFi, NFTs, and DAOs. Its native token is ETH. In 2022, it completed The Merge, switching from PoW mining to PoS proof-of-stake.

## 2\. Complete Mechanism of PoS Validators

### Entry Threshold

Stake 32 ETH and run a client online 24/7.

### Two automatic functions of validators (the programs run fully automatically; human intervention is only for server maintenance)

-   Block Proposer (randomly selected): bundles transactions to create new blocks and receives high rewards
    
-   Attester (majority): verifies blocks and votes across the network to earn stable base rewards
    

### Reward and Penalty System

-   Rewards: block issuance of ETH, Gas fees, MEV earnings
    
-   Penalties (Slashing): small deductions for offline missed votes; severe forfeiture of staked ETH for double signing or malicious behavior
    

## 3\. Ethereum’s Three-Layer Architecture

**Execution Layer (Mainnet)**: Handles transactions, runs the EVM, and executes smart contracts

**Consensus Layer (Beacon Chain)**: Manages network validators, block ordering, and PoS voting

**Scaling Layer (Layer 2 Rollup)**: Arbitrum, zkSync, ConsenSys Linea—batch transactions to reduce gas fees

## 4\. Three Core Underlying Components

### (1) Account System

EOA External Account (wallet account, MetaMask): controlled by private keys, can actively initiate transactions

CA Contract Account: generated by Solidity deployment, no private key, can only be called by external accounts

### (2) Gas Fees

Users must pay to perform any on-chain operation, which goes to validators;

Fee = Gas used × Gas price; EIP-1559 breakdown: base fee (burned) + tip (given to validators).

### (3) EVM Ethereum Virtual Machine

Runs on every chain node, executes smart contract code uniformly, ensuring consistent computation across the network and security isolation.

## 5\. Ethereum Scaling Roadmap

**EIP-4844 (Cancun Upgrade):** Blob data storage, L2 transaction fees reduced by 70%-90%

**ZK-Rollup:** Batch verification using zero-knowledge proofs, secure and efficient

**Data Sharding:** Mainnet focuses on settlement, L2 handles massive transactions

## 6\. Representative Products of the Ethereum Ecosystem

**Wallet**: MetaMask (developed by ConsenSys)

**DEX**: Uniswap Decentralized Exchange

**Infrastructure**: Infura nodes, Besu client, Linea Layer-2 network
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
