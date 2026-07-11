---
timezone: UTC+8
---

# Tingting Gan

**GitHub ID:** Tuskiand

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
# **链上待办合约**

> 主题：从零完成一个「个人链上待办 DApp」——Solidity 合约 + 单文件前端 + 部署上线。
> 
> 范围：覆盖合约原理、前端交互、部署链路，以及若干容易混淆的概念澄清。

* * *

## **一、项目概览**

本项目由三部分组成，可独立理解、独立部署：

| 模块 | 内容 | 位置 |
| --- | --- | --- |
| 智能合约 | PersonalDeadlineOnchainTodo | contracts/PersonalDeadlineOnchainTodo.sol |
| 前端 DApp | 单文件 HTML + ethers.js v6 | frontend/index.html |
| 部署链路 | GitHub 仓库 + Vercel 静态托管 | README.md / frontend/DEPLOY.md |

当前真实部署状态：合约已部署到 **Monad 测试网**（Chain ID `10143`），已使用vercel部署：

[个人链上待办 · PersonalDeadlineOnchainTodo](https://tuskiand-personal-onchain-todo.vercel.app/)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Tuskiand/images/2026-07-11-1783782119223-image.png)

* * *

## **二、智能合约（Solidity）知识点**

### **2.1 合约骨架**

```
 // SPDX-License-Identifier: MIT
 pragma solidity ^0.8.20;
 ​
 struct Todo {
     uint256 id;
     string content;
     uint256 deadline;   // Unix 时间戳（秒）
     bool completed;
 }
 ​
 mapping(address => Todo[]) private userTodos;
```

-   `pragma solidity ^0.8.20`：限定编译器版本，避免新版破坏行为。
    
-   `SPDX-License-Identifier`：源码许可声明，方便他人复用。
    

### **2.2 四个公开函数**

| 函数 | 关键逻辑 | 学习点 |
| --- | --- | --- |
| addTodo(string content, uint256 deadline) | require(deadline >= block.timestamp)；newId = myTodos.length；push | 时间戳校验防止过去时间；用数组长度做自增 id |
| getMyTodos() external view | return userTodos[msg.sender] | view 不消耗 gas；只返回调用者自己的数据 |
| completeTodo(uint256 todoId) | 越界校验后 completed = true | 只能改自己的数据 |
| deleteTodo(uint256 todoId) | 末位覆盖 + pop() | gas 优化删除 |

### **2.3 设计要点（核心价值）**

NaN. **隐私隔离靠** `msg.sender`：所有写操作都以 `msg.sender` 为身份，用户只能操作自己地址下的数据，看不到别人的待办。

NaN. `private` **变量的语义**：`userTodos` 标为 `private` 仅表示「外部合约/直接读取不可见」，但通过 `getMyTodos()` 仍可读取——它与「保密」无关，是访问控制手段。

NaN. **删除为何用「末位覆盖 + pop」**：Solidity 动态数组若从中间删除并前移元素，gas 成本随数组长度线性增长；用最后一项覆盖目标项再 `pop()`，只需一次写操作，省 gas。代价是：被移动的末项 `id` 会变为被删项的下标，需在覆盖后修正 `id` 以保持 `id == 下标`。

NaN. **时间用 Unix 时间戳**：`block.timestamp` 与前端 `Date.now()` 单位均为秒（前端注意 `Date.now()` 是毫秒，需 `/1000`）。

* * *

## **三、前端 DApp（ethers.js v6）知识点**

### **3.1 单文件架构**

-   一个 `index.html` 内含 HTML + CSS + JS，无构建步骤，双击即可在浏览器打开。
    
-   ethers.js v6 通过 CDN 引入；v6 与 v5 API 差异较大（`BrowserProvider` 替代 `Web3Provider`，`ethers.isAddress` 等）。
    
-   合约地址与链 ID 硬编码于文件顶部常量，普通用户连接钱包即用，无需输入地址。
    

```
 const CONTRACT_ADDRESS = "0x3bEd71932240266608Ef0e84bD9f96796261E7C9";
 const CONTRACT_CHAIN_ID = 10143;
```

### **3.2 钱包连接（MetaMask）**

```
 const provider = new ethers.BrowserProvider(window.ethereum);
 const signer = await provider.getSigner();
```

-   `window.ethereum` 是 MetaMask 注入的对象；未安装时需引导用户安装。
    
-   监听 `accountsChanged` / `chainChanged` 事件以刷新界面状态。
    

### **3.3 读操作与写操作**

-   **读（view）**：`await contract.getMyTodos()` 直接返回数据，不弹钱包、不消耗 gas。
    
-   **写（state-changing）**：`const tx = await contract.addTodo(...)` 返回交易对象，`await tx.wait()` 等待上链确认后再刷新界面。
    

### **3.4 状态展示逻辑**

-   **已逾期**：`deadline < 当前时间` 且未完成。
    
-   **即将到期 / 已完成**：分别按时间窗口与 `completed` 标记。
    
-   统计卡片（全部 / 进行中 / 已完成 / 已逾期）由前端计算，不依赖链上聚合。
    

* * *

## **四、部署全流程**

### **4.1 部署合约（Remix → 链上）**

NaN. Remix 编译合约，环境选 **Injected Provider - MetaMask**，连接目标网络。

NaN. 点击 Deploy，钱包确认交易；交易上链后合约获得固定地址。

NaN. 从 Remix 的 Deployed Contracts 复制地址，填入前端常量。

> 关键认知：部署是**一笔链上交易**，确认后即永久存在于该链，与 Remix 网页是否打开无关。

### **4.2 推送到 GitHub**

-   本地 `git init -b main` → `add` → `commit`；`.gitignore` 排除 `.workbuddy` 本地数据。
    
-   推送凭证使用环境变量 `GITHUB_TOKEN`，命令只引用变量名，真实 token 不写入仓库、remote 配置或文件；推送后 remote URL 不含 token。
    

### **4.3 部署前端到 Vercel**

NaN. Vercel 用 GitHub 登录，Import 该仓库。

NaN. **Framework 选** `Other`**，Root Directory 指向** `frontend/`（否则找不到 `index.html`）。

NaN. Deploy 后获得 `xxx.vercel.app`；之后 `git push` 自动重新部署。

* * *

## **五、关键概念澄清（避坑）**

| 误区 | 正解 |
| --- | --- |
| 关掉 Remix 网页会取消部署 | 不会。部署是链上交易，确认后永久存在；仅 Remix 内置的 JavaScript VM 环境关掉会丢失（那是内存模拟，非真实链） |
| 合约地址/链 ID/ABI 写进公开仓库会泄露隐私 | 不会。这些本就是公开链上数据，区块浏览器可查；真正敏感的是私钥、助记词、API Key，本项目均不含 |
| 别人拿到合约地址能改我的待办 | 不会。所有函数按 msg.sender 隔离，他人只能操作自己钱包名下的数据 |
| MetaMask 默认就有 Monad 测试网 | 不是。Monad 测试网（10143）需手动添加或由 chainlist 一键添加 |

* * *

## **六、当前配置记录**

-   **合约地址**：`0x3bEd71932240266608Ef0e84bD9f96796261E7C9`
    
-   **网络**：Monad 测试网（Chain ID `10143`，原生代币 `MON`）
    
-   **RPC**：`https://testnet-rpc.monad.xyz`
    
-   **区块浏览器**：`https://testnet.monadscan.com/`
    
-   **水龙头**：`https://faucet.monad.xyz`（领 MON 付 gas）
    
-   **GitHub 仓库**：`Tuskiand/personal-onchain-todo`（public）
    

* * *

## **七、核心收获**

NaN. **一条完整链路**：合约编写 → Remix 部署拿地址 → 前端填入配置 → GitHub → Vercel 上线分享，每一步都可独立验证。

NaN. **心智模型**：DApp = 链上合约（状态与逻辑）+ 前端（通过 ABI 调用合约）；前端本身不含业务逻辑，只负责组装交易与展示。

NaN. **公开 ≠ 泄露**：区块链是公开账本，地址/ABI/交易均透明；隐私靠合约逻辑（`msg.sender` 隔离）+ 私钥不暴露来保障。

NaN. **测试网注意事项**：需手动配网络、领水龙头；测试网可能重置导致地址失效，正式长期服务应部署到稳定网络。
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->



## 一、今日学习主题

本次参与Monad Builder Camp共学营第一周全员例会，聚焦**新人Web3零基础实操落地、AI辅助学习与开发、链上产品设计、行业入门求职**四大核心方向，有多位学员分享实操经验、周度学习规划与研究项任务解读，完成第一周体系化知识复盘与后续学习路径梳理。

## 二、核心学习收获

### （一）链上实操核心知识点与避坑要点

1\. **基础操作规范**：链上交互、钱包连接必须选择**测试网**而非主网，避免操作失误；所有链上转账、合约部署行为均可通过区块浏览器查询交易哈希，验证执行结果。

2\. **编译报错解决**：合约开发中遇到「stack too deep（栈深溢出）」报错，核心解决方法为**拆分长函数、精简单函数字段**，无需复杂底层优化。

3\. **零代码DApp落地流程**：零基础可依托AI全流程开发部署链上DApp，核心物料仅需钱包、已编译合约；技术组合为Remix编译合约+Wagmi前端框架+GitHub+Vercel一键上线，全程无需手动编写代码。

### （二）链上产品设计思维

1\. **核心设计逻辑**：链上产品的核心竞争力不是简单的数据上链，而是**场景化包装、降低用户认知门槛**，提升普通用户交互意愿。

2\. **数据分层存储**：核心用户数据（宠物状态、打卡记录、成长等级）上链存证、公开可溯源；图片、动画、前端特效等非核心资源存放于本地assets文件夹，大幅降低链上存储成本。

3\. **功能取舍原则**：优先将用户身份、链上行为、成长状态等核心数据上链；复杂审核、社交、特效等功能先在前端迭代，避免前期开发复杂度过高。

4\. **优质案例参考**：Monad宠物喂养Demo，将枯燥的链上打卡操作包装为宠物喂养、升级、连续打卡激励、排行榜等趣味场景，适配新人入门，可用于社区用户增长运营。

### （三）AI辅助Web3学习与开发方法论

1\. **高效提示词逻辑**：摒弃单纯让AI讲解概念的低效方式，要求AI扮演「专属老师」，完成**知识点诊断、错题复盘、习题测试、实操指导**全流程，倒逼自身吃透知识点。

2\. **零代码开发工作流**：需求头脑风暴→AI生成项目规划与UI设计→AI输出前端/合约代码→分模块迭代调试→部署上线，人为负责**需求把控、审美判断、功能取舍**，AI负责代码落地。

3\. **开发避坑技巧**：采用**空文件迭代、分模块开发**，不在原有问题代码上反复修改；跨模块特效、独立功能单独开发后再整合，大幅降低调试成本。

4\. **工具问题解决**：AI工具功能缺失、多模态识别失效等问题，可通过更换API、配置专属插件解决，无需手动开发。

### （四）AI Agent支付安全体系

1\. **三大核心风险**：超预算风险、用户意图偏离风险、提示词注入恶意攻击风险，叠加钱包私钥被盗隐患。

2\. **四重防控机制**：有限授权、预算上限管控、关键操作人工确认、交易全程可审计。

3\. **安全核心认知**：Web3支付安全不能仅依赖链上技术，需结合**钱包权限设计、风控系统、身份验证、人工监督**形成多层防护体系，AI Agent仅可在用户授权范围内执行操作。

### （五）Web3新人入门与求职准备

1\. **入门学习路径**：**先实操、后理论**，优先完成钱包操作、合约部署等实操任务，再反向理解Gas费、交易哈希、链分类等抽象概念，提升学习效率。

2.**行业基础认知**：Web3本质是**公开可验证的去中心化数字状态系统**，链上交易核心是状态变更，并非仅限资产买卖，Gas费为状态变更的执行成本。

3\. **求职必备准备**

-   掌握核心术语：JD、HC、SOP、mentor、landing期等职场常用词汇；
    
-   搭建信息渠道：中文关注深潮、星球日报，海外关注Milkroad，固定时段浏览，避免碎片化信息；
    
-   把握求职时机：Web3行业HC变动快，合适岗位需及时投递。
    

### （六）非技术背景适配学习心得

1\. 无技术、无Web3基础可全程依托AI工具跟进实操，无需死磕底层原理；

2\. 可将自身原有领域经验迁移至Web3，如英语教学、土木信息可视化等，挖掘蓝海落地场景；

3\. 推荐使用Obsidian搭建本地知识库，沉淀学习笔记、实操踩坑记录，搭配AI插件自动整理归档，避免数据丢失。

## 三、本周任务与后续规划

### （一）第一周收尾事项

1\. 作业/积分统计截止：**7月13日**，未完成作业可周末补做补积分；

2\. 奖励机制：周榜排名前10学员，可获得Monad等定制周边奖励。

### （二）第二周学习安排

1\. 开启**研究、运营、技术**三大赛道分方向培养，学员自主选择；

2\. 依托AI工具深耕所选方向，沉淀可写入简历的个人作品集；

3\. 本周学习成果将作为后续黑客松组队、项目落地、资源孵化的核心基础。

### （三）预告

1\. 即将上线**足球赛事预测量化平台**，底层逻辑对标Polymarket，对接真实赛事数据、开放API接口，支持量化模型自动下单；

2\. 积分激励差异化：研究项学员每日1000平台积分，普通学员每日100积分，优秀预测排名可获专属学分奖励；

3\. 平台预计7月内正式上线，前期自主实操探索，后续将配套官方引导教学。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->




# AI Agent安全+Web3线上共学分享会总结

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Tuskiand/images/2026-07-09-1783604121362-image.png)

### （一）AI Agent核心安全痛点

AI Agent安全攻防已从传统自动化转向自主化，其核心风险被定义为“无法审计的自主性”，也是AI时代的核心安全隐患。区别于常规工具，AI Agent具备自主决策能力，风险无明显前兆、难以提前干预，同时模型概率性输出的特性易产生内容幻觉、脱离用户指令自主执行操作，极易引发失控事故。AI在大幅提升工作效率的同时，也会成倍放大操作失误与安全风险。

### （二）四大类典型高风险场景及真实案例

1\. **过度授权风险**：为AI Agent开放过高权限是最常见风险。包括公共群聊Agent被他人诱导窃取本地密码文件、用户终止指令无效导致Agent持续误删邮件、海外SaaS平台Agent9秒越权删除全部生产及备份数据等事故，凸显无管控高权限的致命危害。

2\. **提示词注入风险**：攻击者通过隐藏指令、编码绕过、伪装教程等方式诱导Agent执行恶意操作，包括诱导链上转账、植入恶意代码包、摩斯密码绕过校验窃取加密资产等，隐蔽性极强，普通用户难以识别。

3\. **恶意工具风险**：主流AI工具平台存在大量风险插件，抽检显示超两成工具存在高危、严重漏洞，部分攻击者通过刷量登顶榜单，以高信任背书诱导用户下载，窃取私钥、API密钥、服务器凭证等核心数据。

4\. **底层不可控风险**：涵盖Agent网关高危漏洞、AI幻觉引发的DOS攻击、第三方依赖包供应链投毒、平台业务逻辑漏洞等底层问题，波及范围广，包括OpenAI、Meta等主流平台均曾受相关风险影响。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Tuskiand/images/2026-07-09-1783604009743-image.png)

### （三）通用安全防护规范与产品能力

1\. **基础防护规范**：严格遵循最小权限原则，动态梳理精简Agent权限；资金转账、数据删除等高危操作强制人工二次校验；对所有外部输入、工具保持零信任，仅使用官方认证工具；锁定第三方依赖版本，及时修复系统漏洞，规避供应链攻击。

2\. **专业防护产品（Agent Guard）**：端到端AI安全防护平台，具备实时高危操作拦截、恶意工具自动扫描、全维度安全体检、全球威胁情报匹配、操作全链路溯源等能力，支持自定义安全策略，可形成风险发现、验证、防护、治理的闭环体系。

3\. **行业方案局限认知**：多Agent交叉校验、投票风控的方案存在落地瓶颈，模型上下文遗忘、指令权重不足会导致校验机制失效，无法实现实时防护，底层模型特性优化才是核心解决方向。

## 二、Web3线上共学分享核心内容总结

本次Web3主题分享聚焦行业实战与学生发展刚需，从就业求职、链上安全、本校专业优势、产学研落地项目四个维度展开，打破了行业认知误区，明确了区块链+AI复合发展方向。

### （一）Web3行业求职与就业实战经验

1\. **求职渠道优先级**：内推 > 线下峰会/黑客松人脉 > 线上社群/平台投递。TG求职群可获取运营岗岗位，但诈骗高发需警惕；自主搭建推特、抖音等Web3垂直自媒体，可作为简历核心加分项。线下行业活动是当前低迷行情下获取实习、内推机会的最优路径。

2\. **各岗位求职特点**：技术岗需熟练背诵区块链八股文，行业裁员波动大，开发岗稳定性较弱；运营岗无大量背诵压力，可从社区志愿者起步积累实战经历；研究岗门槛较高，后续将由老师专项分享。

3\. **就业现状与简历重点**：本校区块链专业应届毕业生对口就业率极低，多数学生选择转行、考公；行业求职不看重论文，重点考察自媒体运营、线下活动、项目实战、实习经历。行业实习薪资可观，远程实习是主流形式，人脉内推是后期求职核心渠道。

### （二）Web3链上安全与反诈科普

行业链上被盗、诈骗案例频发，新人安全意识薄弱是主要问题。核心诈骗套路为钓鱼网站仿制官方空投、活动页面，诱导用户授权钱包，直接盗取全部链上资产。高危风险行为包括点击陌生私信链接、访问未知空投网站，同时个人人脸、邮箱、交易所身份信息存在AI伪造、泄露风险，需时刻保持警惕，杜绝陌生操作。

### （三）AI+Web3落地项目：喵星智能体实战解析

该项目是AI技术与区块链结合的典型落地案例，分工清晰、技术链路完整、风控体系完善，具备极强的学习和复用价值。

1\. **团队分工**：由师生联合开发，核心分工明确，苏生、花花负责智能合约与前端开发，守琼负责项目路演，叶老师担任项目经理统筹整体。

2\. **核心功能与技术链路**：通过设备采集猫咪体态、叫声、心率数据，经降噪切片、梅尔图谱转换、声纹特征提取后，依托RAG向量数据库和LLM大模型判定宠物情绪与需求，设置75分置信阈值，达标后可自动触发链上资金履约、宠物用品下单，并推送人工审批通知。

3\. **区块链价值与风控设计**：通过链上资金存证、多层合约校验、预言机对接，实现交易透明可溯源、自动履约，同时优化链上Gas费用；内置消费上限、健康防沉迷机制，规避不合理消费，且该AI识别逻辑可迁移至老人、儿童行为监测场景，拓展性极强。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->






# **AI 辅助开发｜用 AI 生成一个最小 Solidity 合约**

## 1.先使用chatgpt网页版根据任务内容，让ai将任务分解成几个阶段，逐步完成

**ai输出如下：**[**https://chatgpt.com/s/t\_6a4e5d42d15881918e8208bc0de8e0fa**](https://chatgpt.com/s/t_6a4e5d42d15881918e8208bc0de8e0fa)

### 第一阶段：确定需求并编写 Prompt

**目标：** 让 AI 生成一个最小可运行的 Solidity 合约。

建议需求尽量简单，例如：

> 开发一个链上 Todo 合约，支持添加待办事项、查看待办事项、标记完成，不需要删除，不需要管理员权限。

### 第二阶段：AI 生成 Solidity 合约

**目标：**

让 AI 根据 Prompt 输出 Solidity 代码。

得到代码以后：

-   不急着复制
    
-   不急着部署
    
-   先阅读
    

这一阶段只需要保存：

-   AI 输出的完整 Solidity 合约
    

### 第三阶段：理解 AI 输出

不要只复制代码，而是让 AI 再解释一遍。

可以继续提问：

```
请逐行解释这个 Solidity 合约。

包括：

1. pragma 的作用
2. contract 的作用
3. struct 为什么这样定义
4. Todo[] 为什么使用数组
5. addTodo 做了什么
6. toggleCompleted 做了什么
7. getTodo 返回什么
8. memory、storage 的区别
9. view 函数为什么不能修改状态
```

### 第四阶段：人工检查（重点）

检查1：是否可以编译

检查2：函数是否符合需求

检查3：权限问题

检查4：是否存在复杂逻辑

检查5：命名是否清晰

检查6：安全风险

### 第五阶段：记录修改和最终判断

## 2.让ai解释名词

```
举几个例子解释以下的合约方向：
简单合约方向：
 - 留言板 
- 投票合约 
- 打卡合约 
- NFT Badge 
- 积分记录 
- Onchain Todo
```

ai输出：[https://chatgpt.com/s/t\_6a4e5d42d15881918e8208bc0de8e0fa](https://chatgpt.com/s/t_6a4e5d42d15881918e8208bc0de8e0fa)

| 合约方向 | 现实中的例子 | 链上保存的数据 | 主要功能 | 难度 |
| --- | --- | --- | --- | --- |
| 留言板 | 学校公告栏留言 | 留言内容、留言人、时间 | 发布、查看留言 | ⭐ |
| 投票合约 | 班级投票班长 | 候选人、票数、投票记录 | 投票、统计结果 | ⭐⭐ |
| 打卡合约 | 每日签到 | 用户地址、打卡日期 | 签到、查询签到次数 | ⭐⭐ |
| NFT Badge | 获得电子奖章 | NFT 持有者、徽章信息 | 铸造 Badge、查询拥有情况 | ⭐⭐⭐ |
| 积分记录 | 校园积分系统 | 用户积分 | 增加积分、查询积分 | ⭐⭐ |
| Onchain Todo | 待办事项清单 | Todo 内容、完成状态 | 添加、完成、查询 | ⭐ |
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->







1.  了解到了链上产品和普通互联网产品有什么不同：
    
    普通互联网产品的数据和规则主要由公司服务器控制；链上产品的数据和规则部分放到了区块链上，由代码和网络共同维护。链上产品和普通互联网产品最大的区别是“东西放在哪里、谁说了算”。普通 App 的数据和资产都放在公司的服务器里，公司负责管理；而链上产品把重要数据记录在区块链上，由网络共同维护，用户通过钱包管理自己的数字资产。传统互联网更像“租用平台服务”，Web3 更强调“用户自己拥有和控制”。
    
2.  完成了第一笔测试网交易
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Tuskiand/images/2026-07-07-1783436664738-image.png)
3.  配置好codex，安装MONSKILLS
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Tuskiand/images/2026-07-07-1783437825772-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Tuskiand/images/2026-07-07-1783437992511-image.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->










1.  实时参加线上活动DevRel 的成长之路 —— 从 Builder 到生态连接者、Co-learning
    
    ![2709e8309d6b466339815f073ba6f251.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Tuskiand/images/2026-07-06-1783342056518-2709e8309d6b466339815f073ba6f251.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Tuskiand/images/2026-07-06-1783342121936-image.png)
2.  创建一个课程专用钱包，添加Monad Testnet 网络、在水龙头领取5MON币
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Tuskiand/images/2026-07-06-1783342145995-image.png)
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
