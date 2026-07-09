---
timezone: UTC+8
---

# Cool-CoCo

**GitHub ID:** Cool-CoCo

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
Day04学习记录：

本次学习紧密围绕三项核心目标，通过实践验证了学习效果：

1.  **AI辅助与人工校验结合**：借助AI工具生成了最简Solidity合约示例，但并未直接使用，而是逐行审查了代码的逻辑、语法及潜在漏洞，修正了不合理的部分。**核心收获是建立了“AI产出必须经过人工严格审核”的开发习惯**，杜绝了直接部署未经核验代码的风险。
    
2.  **全流程上链实战**：在Remix IDE中完整走通了智能合约开发的“编译-安全扫描-测试网部署-方法调用”全流程，**成功完成了个人首次智能合约的链上部署**，实现了从0到1的突破。
    
3.  **理解合约与链上交互的底层逻辑**：清晰掌握了合约源码、ABI（应用程序二进制接口）、合约地址各自的作用与关联。深刻理解了`read`函数（只读，不消耗Gas）与`write`函数（写入，发起链上交易、生成交易哈希）的本质区别，**打通了从代码到链上交易的完整认知链路**。
    

**文档研读与知识积累**

1.  **Web3实习手册**：系统学习了智能合约的基础开发流程、新手常见错误，并了解了合约安全审计的基本要点，具备了规避基础安全漏洞的意识。
    
2.  **Solidity官方文档**：通过查阅官方文档，熟悉了Solidity的基础语法、变量类型和函数分类规范，能够以此为标准校验AI生成代码的合规性。
    
3.  **Remix IDE与OpenZeppelin**：掌握了Remix的编译、漏洞检测、部署等分模块功能。同时，了解了OpenZeppelin标准化合约库，认识到复用经过行业验证的成熟代码是降低合约安全风险的有效手段。
    
4.  **Monad测试网部署**：通过适配Monad测试网的教程，解决了网络配置及钱包权限等部署报错，并借此了解了DApp的基础架构，明确了合约在去中心化应用中的核心地位。
    

**实操流程记录**

1.  **AI合约编写与审核**：使用AI生成简易数据存读合约，并逐行审核，剔除了存在安全隐患和不规范的代码片段。
    
2.  **编译与安全检查**：将定稿代码导入Remix，选择匹配的编译器版本进行编译，并利用静态漏洞扫描工具修复了所有告警问题。
    
3.  **测试网部署**：将钱包切换至Monad测试网，完成环境配置，成功部署合约，获取了专属的合约地址和ABI文件。
    
4.  **函数调用与验证**：分别调用了`read`函数（直接读取数据，无交易生成）和`write`函数（支付Gas费提交数据，生成交易哈希），并在区块浏览器中核验了上链结果。
    

**经验总结与后续规划**

**核心感悟与避坑指南：**

1.  **AI代码必须人工审核**：AI生成的代码存在版本适配错误和隐性安全漏洞，人工校验至关重要。
    
2.  **ABI与合约地址是关键凭证**：ABI是前端与合约交互的“说明书”，合约地址是链上合约的“定位符”，两者缺一不可，必须妥善保存。
    
3.  **依托成熟模板**：OpenZeppelin等成熟模板能有效防御重入攻击、权限失控等经典问题，后续开发应优先复用。
    

**下一步行动计划：**

1.  **强化实操**：在本次合约基础上添加简单业务逻辑，重复编译、部署、调用的全流程，提升工具熟练度。
    
2.  **建立作品集**：将本次合约源码及部署记录归档，作为个人工作证明。
    
3.  **深入学习安全**：系统学习合约安全基础知识，尝试排查简单合约漏洞，为开发更复杂的项目打下基础。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->

补充下昨天即2026-0-07未提交到Web3 Career Build上面的学习笔记：

### 本小节课程笔记：

本节课获取的关键信息：

更加深入了解了Web3行业的相关信息和知识——

从

一、DeFi：去中心化金融的创新实践

1\. Uniswap：去中心化交易所（DEX）

2\. Compound：去中心化借贷协议

3\. MakerDAO（现已更名为 Sky）：稳定币系统

到

二、NFT：数字所有权的革命

\- 1. NFT 的本质：数字资产的唯一性和所有权

\- 2. 智能合约：自动化的所有权转移和交易

\- 3. CryptoPunks：NFT 的先锋

\- 4. OpenSea：NFT 交易的中心

三、DAO：去中心化自治组织的崛起

\- 1. Nouns DAO：社区驱动的 NFT 艺术 DAO\]

\- 2. LXDAO：支持 Web3 公共物品的建设者\]

\- 3. ConstitutionDAO：一场疯狂的拍卖\]

四、MEME：文化与投资热潮

\- 1. DOGE：MEME 币的开山鼻祖

\- 2. PEPE：社区驱动的 MEME 币

\- 3. 新手如何规避 MEME 币风险？

五、交叉创新领域

\- 1. DeFi + NFT：数字资产金融化

\- 2. DAO + MEME：社区文化与治理融合

\- 3. AI + DeFi：智能化金融服务（仍在早期）

\- 4. RWA 资产上链：传统资产的链上表达

\- 5. WEB3 + 乡建：南塘 DAO 的探索

最后了解了

六、2026 年的新兴趋势（持续更新）

\- 1. Intent-Based 交易

\- 2. 账户抽象与智能钱包

\- 3. 模块化区块链

\- 4. AI + Web3 融合

5.

遇到的问题：今天新知识暂时只学习了一遍，感觉要做到比较熟悉其中的逻辑还需要再温习几遍。感谢各位老师们整理贡献汇总的线上文档！

下一步行动：

温习Day1和Day2这两天的知识，将新知识多看几遍，为后面任务打好基础；

跟着课程继续学习

![sp260708_000002啊.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Cool-CoCo/images/2026-07-07-1783440466975-sp260708_000002_.png)

### 今日（即2026-07-08）新的打卡笔记：

收获的新知识——

一、知道了Web3行业常用工具有哪些；

二、熟悉了Web3行业术语、专有名词、黑话等。关于项目上的沟通案例也方便我们能在实际项目管理中进行使用（感谢整理）；

三、更加学习巩固了一些远程工作习惯，比如远程项目沟通时需要注意的好的工作习惯。其实在看到老师们认真整理的“远程工作注意事项”的时候，回忆之前的工作场景，好像无论是线上还是线下工作，很大程度上都是可以复用的。里面提到的一些注意点也有自己独自复盘之前的工作项目时想到的内容。另外，职场技能、效率工具、Web3常用工具的整理也让我对于职场上的技能、经验有了系统性的把控。

特别摘录了下Web3相关工具的笔记——

-   **Web3 常用工具：**
    
    -   CoinMarketCap 全球最值得信赖的加密货币数据、洞见和社区来源：[**https://coinmarketcap.com/**](https://coinmarketcap.com/)
        
    -   CoinGecko 是世界上最大的独立加密货币数据聚合器：[**https://www.coingecko.com/**](https://www.coingecko.com/)
        
    -   DefiLlama 是 DeFi（去中心化金融）领域最大的 TVL 聚合器：[**https://defillama.com/**](https://defillama.com/)
        
    -   RootData 是一个 Web3 资产数据平台：[**https://www.rootdata.com/zh/Projects?influenceSort=2**](https://www.rootdata.com/zh/Projects?influenceSort=2)
        
    -   媒体：
        
        -   律动 BlockBeats：[**http://theblockbeats.info/**](http://theblockbeats.info/)
            
        -   PANews：[**https://www.panewslab.com/zh**](https://www.panewslab.com/zh)
            
    -   免费发推抽奖工具：[**https://apidance.pro/twitter\_giveaway**](https://apidance.pro/twitter_giveaway)
        
    -   查看链上持仓：[**https://pummmm.com**](https://pummmm.com)
        
    -   查看持币地址、筹码分布：[**https://bubblemaps.io/**](https://bubblemaps.io/)
        
    -   即将启动的加密项目 IDO、IEO、ICO 列表：[**https://www.coincarp.com/zh/upcoming-ido/**](https://www.coincarp.com/zh/upcoming-ido/)
        

四、了解了Web3 的工作方式 和 工具安装

五、明白了 AI Agent 如何拥有支付能力的：

遇到的问题——

1、《Web 3 实习手册》里行业赛道全览里面关于一些英文名字的称呼不太熟悉，还需要多看几遍再熟悉下；

2、对于交易方面的名词解释很容易理解，但是看其中 关于NFT的前端展示这块 只是单独拎出来“NFT头像”这部分进行了讲解吗，文中提到“[**CryptoPunks：NFT 的先锋**](https://web3intern.xyz/zh/industry-knowledge/#_3-cryptopunks-nft-%E7%9A%84%E5%85%88%E9%94%8B)”然后阐述了关于头像形式的数字化艺术品最为最早一瓶数字化收藏品之一。我想知道关于NFT的全概念、以及包含的所有部分（前端可以展示的所有形式、中端内容、后端逻辑……）、以及都能做什么（除了NFT数字化头像外还可以做成哪种形式的数字所有权的形式进行展现或交易呢？）

接下来的计划——

1、深入学习下NFT方面的东西，先在线上实习手册中寻找答案，如还不够深入了解，再在浏览器上或者结合AI更多了解或“自我解答”下关于前面整理的自己“遇到的问题”；

2、继续把没部署完整的需要在本地部署的东西部署一下：

如创建课程专用钱包，以方便做课程实验。

-   添加 Monad Testnet，领取测试资产。
    
-   完成第一笔测试网交易或应用交互
    

![Day03学习笔记部分记录截图（1）.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Cool-CoCo/images/2026-07-08-1783524536892-Day03___________1_.png)![Day03学习笔记部分记录截图（2）.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Cool-CoCo/images/2026-07-08-1783524552741-Day03___________2_.png)![Day03学习笔记部分记录截图（3）.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Cool-CoCo/images/2026-07-08-1783524565731-Day03___________3_.png)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->


## 本节中获取的知识：

一、熟悉以太坊的概念

二、知道Ethereum 与 Bitcoin 的差异

三、了解以太坊的定位与演进

1.  以太坊 1.0（PoW 阶段）
    
2.  以太坊 2.0 与 The Merge：从双链并行到完美合并
    
3.  未来升级路线图
    

四、大概知道了以太坊的生态概览：

L1、L2、Sidechains 等 以太坊生态分层架构

五、以太坊文化与价值观

核心价值观

六、以太坊核心机制：从账户到执行的完整链路

1.  账户系统：你的数字身份
    
2.  Gas 模型：交易的燃料费
    
3.  以太坊虚拟机（EVM）：代码的执行引擎
    

七、一般流程与类比

1.  一般流程
    
2.  相似类比
    

## 本节学习遇到的问题：

一些专有名词没怎么见过，还需多学习几遍熟悉熟悉

## 下一步行动：

完整Web3常用工具安装和账号准备；

理解 DevRel / Builder / 生态连接者的角色；

参与 Co-learning，开始把学习记录变为方便查看的形式。

![Day02学习小结（1）.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Cool-CoCo/images/2026-07-07-1783438902478-Day02_____1_.png)![Day02学习笔记部分记录截图（1）.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/Cool-CoCo/images/2026-07-07-1783438930682-Day02___________1_.png)
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
