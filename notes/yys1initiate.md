---
timezone: UTC+8
---

# yys1initiate

**GitHub ID:** yys1initiate

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
开始看开源项目 moss：

首先先了解一些概念：1、MEV：最大可提取价值；2、验证节点：买了很多昂贵的服务器、全天候开机维护 Monad 网络运行的机构或个人；3、**去中心化金融：Defi**;4、区块链里的特例：稳定币，usdc，其余有些币的价格就是波动的，和供求关系有关；5、Defi套利层：它们只在“交易所”、“借贷池”这些充满了公共资金和波动价格的地方打架。它们打架产生的“插队费”油水，最终通过流动性质押协议（如 `aPriori`），反哺给了在生态里存钱的普通用户；6、poS：权益证明；7、calldata 调用数据：只读的、一次性的

所以总结一下，由于代币的价值是波动的，所以就出现了算法+合约的组合去牟利，然后节点进行判断。

（ai解释：

-   **存入代币：** 你把你的 `MON` 代币存进 `aPriori` 这样的协议网站里。
    
-   **协议自动打散：** 协议的底层**算法大脑**会自动帮你去算账。它会把收到的资金打散，**自动按照最优比例，把一部分资金分配给那些服务器极其稳定、同时管理费又极低的小交警和中交警**，帮他们增加筹码。
    
-   **给你一张“收据”：** 存入后，协议会给你一个叫 `sMON`（或 `afMON`）的衍生代币。这个币代表了你的“本金+源源不断的利息”。）
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->

这次将我的项目部署到了github上，上传了我的第一个项目，刚开始原本是打算让claude替我完成的，但是api接的deepseek可能是能力不足，一直失败，所以选择手动部署，遇到问题问ai，这里记录下一些心得什么的：

### 1、进入你要部署的目录：cd

### 2、执行git init 初始化本地仓库

### 3、把文件提交到暂存区并提交

-   git add . （注意是add .）
    
-   git commit -m "first commit"
    

### 4、创建主分支：git branch -M main

5、关联远程仓库：git remote add origin [https://用户名：token@github.com/你的用户名/仓库名.git](https://github.com/你的用户名/仓库名.git)

-   遇到的问题，github现在有个什么精细分支，没设对它的权限，就一直报错，解决了很久
    
-   ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yys1initiate/images/2026-07-12-1783836510075-image.png)
-   解决方法：用回classic，勾选repo，然后成功就关联上了
    

### 5、推送到github：git push -u origin main

-   orign是什么？可以理解为变量名
    

### 6、其他：git status、git rm -r --cached .（清除缓存）

心得：自己过了一遍之后，确实更明白了一点
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->


休息日ing
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->




今天没学什么，但是打个卡
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->






前情提要：由于remix做的一直连不上钱包，换桌面版登录之后，找不到之前的文件了，ai额度也用完了，所以不用remix了，打算重新做。

1、确定需求：我想做一个基于monad测试网络的留言板，把需求发给ai之后，它给出的提示词如下：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yys1initiate/images/2026-07-09-1783598002570-image.png)

检查过后没有什么大问题，于是让ai帮我写一个智能合约，并且做一个对应的ui；结构如下，主要分为合约和ui两个方面，MessageBoard.sol就是智能合约的源码，Deploy.s.sol是脚本的部署文件，然后还需要有一个环境变量，环境变量需要根据自己的实际情况更改

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yys1initiate/images/2026-07-09-1783598098756-image.png)

由于我下载不了foundry，所以选择改写合约，换成hardhast的，

2、需要我们把合约上链，选择了用npm去部署合约，在合约过程中先输入：npm run compile 对它进行编译，然后运行 npm run deploy对它进行部署，然后部署成功了

ps：记得还要把私钥改成自己的

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yys1initiate/images/2026-07-09-1783602584247-image.png)

3、然后在ui界面还要重下npm， contracts/ 和 ui/ 是两个独立的项目，各有各的 package.json：

4、前端页面各种报错 终于解决了......

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yys1initiate/images/2026-07-09-1783604397668-image.png)
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->







今天主要学习了以太坊，和回顾了昨天的内容。

1、老师先是带领我们回顾了一下钱包的设置，推荐了一个好用的钱包phantom；

2、用ai去部署一个合约？重点学习如何用ai去部署一个合约：老师推荐用codex写，在把合约上传到github上时，要注意env等一下东西不要放上去，不然会被别人盗走收益，

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yys1initiate/images/2026-07-07-1783428421655-image.png)

3、但是对于智能合约方面我好像还是不是很了解？

4、做demo的思路（提示词相关）：帮我做一个发行nft的智能合约（idea相关？）

把它部署到monad test的测试网上（把合约部署到链上）

有了一个abi文件，利用它搭建一个前后端系统；

写一个有能力调用合约的前端

5、建议先走一个remix流程，学习完流程之后可以vibecoding
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->








今日co-learning:

1\\创建钱包(下载插件，创建main wallet 和 其他账户，确保安全)；

2\\配置Monad Testnet，添加monad并且连接钱包地址

2\\登录[Monad Faucet](https://faucet.monad.xyz/)

3\\把钱包地址填进去，领取我的测试币

4\\合约还没开始写，打算明天看看

![2.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yys1initiate/images/2026-07-06-1783343603855-2.png)

这是在创建钱包

![1.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yys1initiate/images/2026-07-06-1783343617100-1.png)

这是选择monad，钱包连接成功

![3.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/yys1initiate/images/2026-07-06-1783343630675-3.png)

成功领取测试币，但是我没找到水龙头在哪？
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
