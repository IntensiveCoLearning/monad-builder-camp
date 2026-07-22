---
timezone: UTC+8
---

# ComAsUare

**GitHub ID:** ComAsUare

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
1.  参加钱包开发新人避坑与需求直播分享
    
2.  学习foundry测试框架
    
3.  笔记：onekey 一体化钱包
    
    硬件钱包，全开源、安全服务
    
    自托管，全链上
    
    1\. crpto 钱包行业特点
    
    crypto第一个应用，入口；容错率极低；
    
    盈利方式稳定
    
    2\. 账户体系
    
    自托管，助记词自保管；对比托管钱包；
    
    资产迁移方便，用户自行控制。
    
    3\. 外部攻击
    
    地址投毒（头尾若干位相同的假地址），
    
    假代币，
    
    同名代币不禁止创建
    
    恶意签名
    
    4， 钱包开发本质：创建和撮合
    
    1\. 创建钱包
    
    贴合用户传统账户使用习惯，安全
    
    传统创建钱包: genereate, backup, verify
    
    onekey简化： keyless技术，用户使用邮箱即可一键创建钱包
    
    本地生成并加密，密钥分片+保存在onekey后端和分布式存储网络两个地方，
    
    pin密码
    
    2\. 购买股票
    
    分解需求：
    
    ondo, xstocks, 稳定币私有链询价
    
    3，
    
    1.用户意图
    
    2\. 实际流程
    
    3\. 可删减部分，如何权衡
    
    4\. 安全考量
    
    4.
    
    热钱包，创建时联网，metamask。热钱包相对来说方便性优先于安全
    
    冷钱包设计更关注是安全，也有多个产品场景。例如无电池版本：防止多年囤币设备老化；
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->

参加building in AI era和stevie project两场直播分享

笔记：

```markdown
datadance：数据隐私公链
    erc7962, 钱包
    用户数据如何通过agent打通，安全的使用，更好权限(permissioness)管理。传统互联网数据流的再次进化
    1. lifecapsules.ai 
        各个app数据->记忆agent；人的情感、精神世界、阅历、感想。保存、管理、跨平台分享。
        各大互联网平台数据入口的抢夺；互联网分享精神不再，割裂世界。
        通用建议；上下文不丢失,得到累积

    2. 蒸馏
        输入messy, 输出清晰的
    3. 竞品
        1. windows recall , open    recall （反对，不认同）
            百分百监控，隐私侵犯->大众反感
            垃圾数据累积
        2. 主动捕获（认为认同）
            ai twin
            用户主动选择（快捷键）才会记录
            有了高质量数据，尊重隐私
        3. 本地优先（认为并非对手）
            obisidian。cursor
            本地，隐私好，门槛高
        当前产品都存在没有解决问题
    4. 迭代的lifecapsules,像个思想种子的平台
        相关方向产品很多，现状鱼龙混杂
    
    5. ai在语言之外的数据，边缘的数据，不容易采集的数据，是ai另一个方向。
        模拟是更好的实践，从主体、客体抽离，获得新的视角。
        非语言、非对齐(non liberal) ai方向。

很多都是不确定的，对当前流行的观点应该保持质疑精神。
1. ai做什么，不能做什么->我能让ai做什么。
    具体业务场景
    业务>流程>规则>ai
2. agent进入经济工作
    人不是直接面对软件
3. 沟通：不要试图教育老板和客户，没有收益且难度高。与人性有关
4.当前仍然没有高用户群体的agent web3应用。
    未来意义。
    行业现状：寻找需求远远大于技术

5. 案例： 企业微信部署agent, 进入销售流程。转化率低
    私域 用户数据 ， 公域 产品信息 注入agent。
    信任是agent未来重要方向。不能只有a2a信任，没有人对agent的信任。

6. https://app.virtuals.io/
    AI 代理界的代币发行与交易中心。
    发行agent, 交易agent代币

7. memory 行业共性难点。各大大厂memory架构
```
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->


1.学习数据库原理后构建清单应用

2.  dao投票系统合约部署到monad测试网上
    
3.  项目链接：[https://github.com/ComAsUare/LearnMonadBuild](https://github.com/ComAsUare/LearnMonadBuild)
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->



1.  builder anthing学习
    
2.  vibecoding完成投票solidity合约
    
3.  项目链接：[https://github.com/ComAsUare/LearnMonadBuild](https://github.com/ComAsUare/LearnMonadBuild)
    
4.  笔记：
    
    ```markdown
    1. vibecoding入门
       prompt injection, agent所能读入的一切信息中，混入隐藏指令
       预防：在沙箱中运行智能体
       绝不要把真实的私钥、密码或 API key 交给智能体
        1. 循环
            描述 — 用自然语言告诉智能体你想要什么
            构建 — 智能体编写代码并创建文件
            审视 — 在浏览器里查看它的成果
            反馈 — 告诉智能体需要改什么
            重复 — 不断迭代，直到你满意为止（第一版不会满意）
    2. 生产级品质代码
        1. 产品细节
            响应式设计（responsive design）； Favicon 与页面标题（favicon & page title）浏览器标签页小图标，搜索页标题；无障碍（accessibility）；深色主题
        2. 被发现
            搜索引擎优化（search engine optimization）;Open Graph tags;访问数据分析；自定义域名
        复杂部分：
        3. 真实功能
            1. 数据库：应用的记忆。Supabase、Firebase、PlanetScale
            2. 用户认证（user authentication）。让用户能创建账号并登录。这包括密码哈希、会话管理、OAuth（用 Google 登录）、邮箱验证和密码重置。永远不要从零自己实现这一套
            3. 文件存储（file storage） 云存储。如果存储在服务器，每次重新部署会清空。
            4. 支付。 比如stripe这样支付服务商，处理信用卡、税费、收据和退款。它们每笔大约抽 3%，并且有可能冻结你的账户。
        4. 基础设施
            环境变量，ci/cd, 预发布环境
        5. 可靠性
            error handling； error tracking ：sentry；testing; backup
        6. 安全
            HTTP应用和用户流量加密； input validation; rate limiting
        7. 优化
            image 压缩；caching;浏览器缓存、服务器缓存、CDN 缓存;
            Core Web Vitals —— Google 用来衡量页面速度和用户体验的指标。它们会直接影响你的搜索排名。
    3. 行业尚未被构建
        抗审查的工具
        可验证的身份和投票系统
        由用户真正掌控的数据隐私
        不容易被伪造的 AI 内容识别
        不容易被少数人操控的治理系统
        不会被平台随意收回的所有权
    4. 10000tps
        1. amm是慢链下不得不的选择，订单簿上链
        2. 链游：脱离开发者运行，玩家历史永久公开，物品跨游戏，跨defi使用
        3. 社交：Farcaster 和 Lens ；继续把交互数据上链。
        
        4. 哪些数据留在链下：
            私密数据，不宜公开数据，大文件留在链下。哈希适合上链；文件本身的字节，应该放进真正为字节存储设计的系统里。
    
            超高频事件留在链下。每秒几十万次更新的传感器数据流，不属于任何一条链。
    
            重计算留在链下。链负责共识和归属权，不负责视频转码，也不负责 AI 推理。
    
            如果某类交互本来就可以信任一个运营者，它也应该留在链下。一个应用如果能接受自己的排序器、撮合器或游戏服务器，就没有必要把链塞进这条路径。
    
            划分原则没有变：需要无信任、永久性、可组合性的部分上链，其余部分留在链下。变化的是这条线会落在新的位置，而不是这条线不再存在。
    5. 数据库
        数据库和存储是攻击者最先盯上的地方——用户数据就放在那里。
        1。组织
            users, todos, projects(这个表是如何组织todos)
        2. SQL
            Structured Query Language,
            SELECT 读取行
            INSERT 添加一行
            UPDATE 修改已有的行
            DELETE 删除行
        3. 文件存储
            数据库适合结构化存储（文本、数字、日期、各种关系）
            云端存储：头像和个人资料图片
            用户上传的图片、视频、音频、PDF
            各种导出文件、报表和生成文件
            
    
    ```
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->




1.  测试网交易
    
2.  vibe coding课程学习完毕
    
3.  直播回看：ai时代researcher, ai时代web3运营，builder path
    
4.  项目链接：[https://github.com/ComAsUare/LearnMonadBuild](https://github.com/ComAsUare/LearnMonadBuild)
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->





参加ai时代researcher和moss直播分享

项目：[https://github.com/ComAsUare/LearnMonadBuild/](https://github.com/ComAsUare/LearnMonadBuild/t)

笔记：

```markdown
1. gamma ppt
    https://gamma.app/docs/AI-Web3-Researcher--hlf1moi9jna2jey?mode=doc
2. Ethereum Attestation Service
    数字资料事实证明；公开资料验证；
    发明发现、先后判定；
    验证机构的公信力。
2. grokpedia 
    注册；上传文章；
    理解测试，事实测试等等
    用户通过grokpedia提问，grokpeia根据文章进行回答，并返回来源。
4. 专业化资金申请
    https://x.com/GCCofCommons/status/2069627888169791759 eth独立研究者资助计划
    https://x.com/LXDAO_Official/status/2038457544885072155 lxdao公共物品资助
```
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->






1.参加ai开发与实践直播分享

2.  参加co learning共学
    
3.  笔记：
    
4.  ```markdown
    ai成为可验证工程协作者，web3成为可拆解的系统
    1. 0->1
        1. 明确目标和用户群体
        2. 核心体验、流程能走通
        3. 清楚展示“解决了什么问题”
        4. 
        1->99 让demo成为系统
    
        0->1降低门槛，低廉；
    2. 门槛降低不等于责任降低
        效率提升：缩短想法，变更、验证结果；理解旧代码、重构更快；
        重构可以让跑偏的ai，重回可控的轨道。
    3. 感知环境：知道当前操作系统
        agent= 模型+上下文+记忆+工具+执行循环+权限边界+验证
        回顾历史。这是模型进步，与“完整行动系统”的共同演进
        hermes是闭环更完整的例子：通过互动，把工作流程记录为新的skill
    4. 用户/全局指令
        可复用，个人便好
        项目/目录级指令
        构建、测试、验证命令；
        开源项目应该重点关注agent.md，比文档更清晰的结构
    5. tdd测试驱动开发
        快速自我修正；防止破坏行为。
    6. sdd规范性开发
        先对齐意图，再生成代码
        记录意图、约束、规范、决策、验收标准
        工具：新项目sepckit, 进行一段项目新增需求：openspec
    7. 会描述、会拆解、会验证、会设计
        提升拆解任务、组织工作、设计系统、验证结果的工程能力
    8. 区块链
        理解->开发者视角
        异步性；不能简单称为传统一致性
    9. 岗位方向
        dapp, 智能合约、钱包开发、节点开发、安全审计
    10. 共同基础
        1. 密码学与密钥体系
        2. 区块链原理：交易、账户、共识、接口
    11. dapp = web工程+前端
        智能合约开发 ：验证fuzz, fork test
        钱包开发：密钥，连接，账户抽象，安全
        节点开发：客户端工程、分布式系统2
    ```
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->







实习手册整理：

```markdown
2. 远程工作习惯：
    0. 把关键结论沉淀到文档或 issue，并标注时区与预期响应时间，减少重复沟通和误读。

        * 标注时区与响应窗口（例如：UTC+8，24 小时内回复）
        * 关键决策同步到文档或 issue，并附上依据链接
        * 会议后 24 小时内补齐纪要和行动项
    1. okr记录
        OKR（目标与关键结果）不仅是一种管理工具，更是解决分布式团队核心痛点的系统性方案。远程团队因缺乏物理接触易陷入目标碎片化。OKR 要求目标（Objectives）全团队公开可见，关键结果（Key Results）量化可追踪
        * 1. 目标设计原则
            聚焦关键目标：每季度设定 3-5 个目标，避免分散精力。
            野心与可行平衡：目标应“令人不适但可达”，理想完成率在 60%-70%（Google 标准）。
            结果导向：聚焦成果而非产出（例：“提升客户留存率” 而非 “发送 10 份调研”）。
        * 2. 关键结果（KR）设计
            递进式量化；SMART (specific, messurable, achievable, relevant, time-bound)标准：确保 KR 可衡量、有时限
        * 3. 评分与复盘
            低分原因（目标过高/资源不足），高分则反思挑战性是否不足
    2. 远程会议
        * 1. 预约远程会议
            明确会议主题，期望目标；关键决策人，信息相关人参会，
            避免冗余参会；沟通上明确回复截止日期；预期时长。
        * 2. 日历工具
            标题应简洁明了；会议描述精要；设置提醒；与会者邮箱，批量自动发送提醒；时区设定与与会者匹配；视频会议链接嵌入
        * 3. 准备会议纪要，关键结论标红加粗
            ✅ 决策事项 + 行动项明确 DRI（直接责任人）+ Deadline（精确到时区）
3. web3常用工具：
    CoinMarketCap 全球最值得信赖的加密货币数据、洞见和社区来源：https://coinmarketcap.com/
    CoinGecko 是世界上最大的独立加密货币数据聚合器：https://www.coingecko.com/
    DefiLlama 是 DeFi（去中心化金融）领域最大的 TVL 聚合器：https://defillama.com/
    RootData 是一个 Web3 资产数据平台：https://www.rootdata.com/zh/Projects?influenceSort=2
    媒体：
    律动 BlockBeats：http://theblockbeats.info/
    PANews：https://www.panewslab.com/zh
    免费发推抽奖工具：https://apidance.pro/twitter_giveaway
    查看链上持仓：https://pummmm.com
    查看持币地址、筹码分布：https://bubblemaps.io/
    即将启动的加密项目 IDO、IEO、ICO 列表：https://www.coincarp.com/zh/upcoming-ido/
4. 职场软技能
    承诺管理法：接任务三要素：交付物，方向，反馈节点
    超预期交付技巧：交报告时附加"执行要点清单"
    向上汇报公式：进展 + 卡点 + 建议 + 需支持
    跨部门协作：提前了解对方 KPI（如市场部关注转化率），用共同利益点推动合作
    个人 SOP(standard operating procdure) 库：把工作生活步骤标准化，进行复用。如notion
    对工作留痕
    交接文件包：含操作指南 / 联系方式 / 踩坑记录
    事实代替情绪，化敌为友
5. 行业黑话
    1. 基础类
        DYOR	Do Your Own Research，投资前请自行研究，项目方常用于免责
        FUD     Fear, Uncertainty, Doubt，恐慌、不确定、怀疑，指唱衰情绪或舆论攻击
        WAGMI	We're All Gonna Make It，大家都会发财，社区常用打气口号
        NGMI	Not Gonna Make It，讽刺某人/项目做法不行
        REKT	被"爆锤"，损失惨重，如投资失败、合约被黑等
        Degen	"投机狗"，不问项目质量只冲高风险高回报机会的人
        Shill	宣传、推销（常含贬义），如"shill 项目"指恶意安利
        Exit Scam	项目方跑路，携款失联
    2. 技术类
        Rugpull	抽地毯，项目方卷款跑路（尤其在 DeFi 项目）
    3. 投资类
        Pump	拉盘，代币价格快速上涨
        Dump	砸盘，代币价格快速下跌
        HODL	原为"Hold"打错，后来变成文化，意为坚定持币不卖
        Bagholder	"接盘侠"，高位买入亏损后长期持币的人
        Alpha	内部消息/潜在机会，表示未公开但价值潜力大的信息
    4. 社区与文化
        Anon	匿名者，社区中不透露真实身份的成员
        KOL	Key Opinion Leader，意见领袖，影响力人物
        CT	Crypto Twitter，加密行业活跃的信息与讨论来源
        NFT PFP	NFT 头像项目，如 CryptoPunks、BAYC，PFP = Profile Picture
6. 岗位
    1. 前端
        1. react, vue前端框架（ui），wagmi状态管理库() viem以太坊通信库（替代替代了旧的 Ethers.js），负责链上打包，解析，通信呼叫。
        # 常用技术栈
            - HTML5
            - CSS3
            - JavaScript (ES6+)
            - React / Vue
            - TypeScript
            - Next.js
            - Viem
    2. 后端
        * 1. dapp后端服务：链上数据交互、智能合约集成和事务处理。
        * 2. 支持多种 Web3 钱包集成
        * 3. 前端从后端取数据：RESTful 或 GraphQL API
        * 4. 确保智能合约与后端服务的无缝连接，优化链上数据的读取和写入效率
        * 5. 优化后端性能，确保系统的高可用性、高吞吐量和低延迟，满足高并发访问需求
        * 6.技术栈：消息队列，事件驱动架构，能处理异步事务
            # 常用技术栈
                - Node.js / Go / Python
                - Viem / Web3.js / Ethers.js
                - RESTful API / GraphQL
                - MySQL / PostgreSQL
                - Docker / Kubernetes 
    3. 研究分析
        收集、整理并分析 Web3 行业市场与用户数据，编写可行性研究报告，为产品与运营提供决策支持；
        跟踪区块链协议技术演进及生态动态，撰写深度研究报告或白皮书；
        进行竞争对手分析，评估市场趋势与用户行为模式，为战略规划提供数据驱动的建议；
        支持项目的加密经济模型设计与博弈论分析，以保证项目的经济激励合理性。
        链上数据分析工具： Glassnode、Token Terminal
    
            
```
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->








1.  实习手册整理
    
2.  monad主网钱包，转账试用
    
3.  项目 链接：[https://github.com/ComAsUare/LearnMonadBuild/](https://github.com/ComAsUare/LearnMonadBuild/tree/main)
    
4.  笔记：
    
5.  ```markdown
    2. 远程工作习惯：
        0. 把关键结论沉淀到文档或 issue，并标注时区与预期响应时间，减少重复沟通和误读。
    
            * 标注时区与响应窗口（例如：UTC+8，24 小时内回复）
            * 关键决策同步到文档或 issue，并附上依据链接
            * 会议后 24 小时内补齐纪要和行动项
        1. okr记录
            OKR（目标与关键结果）不仅是一种管理工具，更是解决分布式团队核心痛点的系统性方案。远程团队因缺乏物理接触易陷入目标碎片化。OKR 要求目标（Objectives）全团队公开可见，关键结果（Key Results）量化可追踪
            * 1. 目标设计原则
                聚焦关键目标：每季度设定 3-5 个目标，避免分散精力。
                野心与可行平衡：目标应“令人不适但可达”，理想完成率在 60%-70%（Google 标准）。
                结果导向：聚焦成果而非产出（例：“提升客户留存率” 而非 “发送 10 份调研”）。
            * 2. 关键结果（KR）设计
                递进式量化；SMART (specific, messurable, achievable, relevant, time-bound)标准：确保 KR 可衡量、有时限
            * 3. 评分与复盘
                低分原因（目标过高/资源不足），高分则反思挑战性是否不足
        2. 远程会议
            * 1. 预约远程会议
                明确会议主题，期望目标；关键决策人，信息相关人参会，
                避免冗余参会；沟通上明确回复截止日期；预期时长。
            * 2. 日历工具
                标题应简洁明了；会议描述精要；设置提醒；与会者邮箱，批量自动发送提醒；时区设定与与会者匹配；视频会议链接嵌入
            * 3. 准备会议纪要，关键结论标红加粗
                ✅ 决策事项 + 行动项明确 DRI（直接责任人）+ Deadline（精确到时区）
    3. web3常用工具：
        CoinMarketCap 全球最值得信赖的加密货币数据、洞见和社区来源：https://coinmarketcap.com/
        CoinGecko 是世界上最大的独立加密货币数据聚合器：https://www.coingecko.com/
        DefiLlama 是 DeFi（去中心化金融）领域最大的 TVL 聚合器：https://defillama.com/
        RootData 是一个 Web3 资产数据平台：https://www.rootdata.com/zh/Projects?influenceSort=2
        媒体：
        律动 BlockBeats：http://theblockbeats.info/
        PANews：https://www.panewslab.com/zh
        免费发推抽奖工具：https://apidance.pro/twitter_giveaway
        查看链上持仓：https://pummmm.com
        查看持币地址、筹码分布：https://bubblemaps.io/
        即将启动的加密项目 IDO、IEO、ICO 列表：https://www.coincarp.com/zh/upcoming-ido/
    4. 职场软技能
        承诺管理法：接任务三要素：交付物，方向，反馈节点
        超预期交付技巧：交报告时附加"执行要点清单"
        向上汇报公式：进展 + 卡点 + 建议 + 需支持
        跨部门协作：提前了解对方 KPI（如市场部关注转化率），用共同利益点推动合作
        个人 SOP(standard operating procdure) 库：把工作生活步骤标准化，进行复用。如notion
        对工作留痕
        交接文件包：含操作指南 / 联系方式 / 踩坑记录
        事实代替情绪，化敌为友
    5. 行业黑话
        1. 基础类
            DYOR	Do Your Own Research，投资前请自行研究，项目方常用于免责
            FUD     Fear, Uncertainty, Doubt，恐慌、不确定、怀疑，指唱衰情绪或舆论攻击
            WAGMI	We're All Gonna Make It，大家都会发财，社区常用打气口号
            NGMI	Not Gonna Make It，讽刺某人/项目做法不行
            REKT	被"爆锤"，损失惨重，如投资失败、合约被黑等
            Degen	"投机狗"，不问项目质量只冲高风险高回报机会的人
            Shill	宣传、推销（常含贬义），如"shill 项目"指恶意安利
            Exit Scam	项目方跑路，携款失联
        2. 技术类
            Rugpull	抽地毯，项目方卷款跑路（尤其在 DeFi 项目）
        3. 投资类
            Pump	拉盘，代币价格快速上涨
            Dump	砸盘，代币价格快速下跌
            HODL	原为"Hold"打错，后来变成文化，意为坚定持币不卖
            Bagholder	"接盘侠"，高位买入亏损后长期持币的人
            Alpha	内部消息/潜在机会，表示未公开但价值潜力大的信息
        4. 社区与文化
            Anon	匿名者，社区中不透露真实身份的成员
            KOL	Key Opinion Leader，意见领袖，影响力人物
            CT	Crypto Twitter，加密行业活跃的信息与讨论来源
            NFT PFP	NFT 头像项目，如 CryptoPunks、BAYC，PFP = Profile Picture
        
    ```
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->









1、进入epf回放观看。

2、实习手册看完

3、参加周例会

项目记录仓库：[https://github.com/ComAsUare/LearnMonadBuild](https://github.com/ComAsUare/LearnMonadBuild)

相关笔记：

```markdown
1. ethereum protocal fellowship: 协议层学习，研究，提案，交付。
    区别于合约培训班：需要交付可验证性贡献。
    每年3-4月自学，填写申请表
2. epf资源：
    官方文档；https://epf.wiki/
    epf discord群组
    
3. 学习过程：
    1) 学习；补协议基础，理解自己感兴趣问题
    2）写作；每周进展，下一步计划。
    3）讨论： standup, office hour, issue, pr。mentor会跟进并交流
    4）交付： 路线图，交付
4. 申请通过后，6-11月：
    1. 准备 协议，read.
         协议的核心讨论都在github上：[eth-protocol-fellow](https://github.com/eth-protocol-fellows)
    2. 定方向
        了解项目领域，写topic of interest
    3. 成项目
        深挖问题，写project proposal
    4. 做交付
        执行roadmap, 每周development update
    5. 做展示
        项目wrap-up.持续贡献
5. 学习顺序要服务于一个目标
    1. 先了解地图。
```
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->










观看agent支付、进入epf两个分享。

```markdown
ai高效，安全的支付是核心问题；
1.和传统支付场景不同：小额高频支付，agent无身份认证
    速度，凭证(credentials),粒度(granularity): 传统0.5$, agent:0.001micropayment
    agent自动执行，也不会像传统那样每笔交易确认，点击。

2. fluxa产品 端到端ai payment 组件，包括钱包

3. 场景分类：
    *1 a2a
    *2. agent 2 merchant： 2b， 订酒店
    *3. agent 2 tool: 无需订阅，每次api调用，skills, mcp付费
    *4. 转账社交的逻辑。
4. 产品矩阵：
    *1. agent wallet
        一键撤销revoke, 更灵活spending control , offline approval, audit trail.
        钱包场景： 身份证明，拿到bugget，为服务付费，接受支付，把钱花出。
        x402协议与agent
    *2. agent card
        *1. agent card ai 身份认证是为了风控ai幻觉造成乱花钱。
        *2.  finantial harness engineering
    agent风控,固定bugget，一次性。
    *3. agent charge agent收付款。
    *4. 开发者skills. mcp收款
        agent embedded payment protocol. 类似于layer1, layer2。把收款人验证，接受服务放在链下，来提高效率
    *5. user case: 社交，agent红包
    *6  user case: 自动化任务平台
    *7. user case： 订票

```
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->











开营仪式，xiaohai build分享，直播回放。

相关笔记：

```markdown
1. 五周规时间划，关键节点：week3协作制作demo，week4黑客松
2. marcus分享monad：
    1. 链上活动增长需求->高吞吐量区块链:
        * 1. 稳定币供应量增加
        * 2. DEX现货份额，现货区分于衍生品/合约交易
        * 3. 预测市场交易量
        * 4. a2a
    2. 提高吞吐量、速度的技术：
        * 1. 本质是对区块链不可能三角进行权衡和优化
        * 2. block time，出块时间，生成一个区块的时长； TPS ，吞吐量，每秒处理交易数。
        * 3. 技术分类：
            ** 1. L1 基础层优化
                *** 1. Parallel Execution
                    Solana（SVM 架构）、Move 语言生态（Aptos、Sui），以及 Parallel EVM（如 Monad、Sei）
                *** 2. 共识机制与出块机制优化
                    Solana PoH（历史证明）
                *** 3. sharding分片技术
                    NEAR Protocol 的夜影协议（Nightshade）、TON（自适应无限分片架构）
            ** 2. L2 链下分层外包
                *** 1. rollup卷叠技术
                    Optimistic Rollups（乐观卷叠）， ZK Rollups（零知识证明卷叠）
                *** 2. State Channels
                    比特币闪电网络
    
    3. 实时验证者可视化网址： gmonads.com
    4. 节点门槛不提高，通过架构提高速率，而不是提高节点计算性能
    5. 除了Parallel Execution， monad在基础层使用的原语：asynchronous excution流水线化共识和执行，以及monadDB状态查询
        * 1. asynchronous excution
            传统共识-执行交错，共识耗时>>执行。
    6. monad在共识和runtime中使用的技术：
        * 1. JIT(just in time) compliation
        * 2. monadBFT 共识
        * 3. raptocast 区块传播
```
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
