---
timezone: UTC+8
---

# Suzuki-yki

**GitHub ID:** Suzuki-yki

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
## **今日学习日记：**

（主播才发现其实每天都有学习任务来着，因为第一次参加我才看到旁边那个课程安排每天该做什么）

主播今天也有点偷懒了，真的拜托不要这样下去了好吗......好痛苦。

我的**代码思路**是这样的：按照CEI的标准先checks，用户前端点击售卖（调用我的智能合约），这时我接收前端传来的数据，当然不能相信前端，所以需要check seller调用的地址是否=msg.sender，其次seller是否拥有该NFT，我目前想到的就只有这些checks，我开始构思代码的逻辑时候，ai说先教我ERC721再开始写代码吧！

OpenZepplin 的**ERC721**.，ERC721就是一个写好的NFT的engine，然后其中关键的代码：

-   mapping (uint256 => address) public \_owners;
    
-   mapping (address => uint256) public \_balances;
    

这两个都是tokenid 和地址,虽然重复了，但是是有重复的必要的，因为如果用户查询你直接遍历的话需要花费非常多的gas去存储，所以Space For Time（用空间换时间）！

### **ERC721和IERC721的区别：**

ERC721是implementation，里面有mapping，ownerof()等等...它可以运行。

IERC721是interface，里面没有代码只是定义你如果是ERC721你需要包含哪些函数！  

Marketplace 面向的是"标准（Interface）"，而不是某一个具体实现。然后我学到了这句话：**Program to an Interface, not an Implementation.（面向接口编程，而不是面向实现编程。）**（当然主播其实并没有完全理解这句话的意思 (。. 。；） ）

学到的一个**知识点**：**_address(0)_**叫**_Zero Address_**(零地址)，它表示的意义是不存在。

这里还要说到我前面写的关于cei了，写代码要以cei为规范进行思考，当我运行我的maekrtplace的智能合约时第一步不是创建liastings而是先check你是**owner**吗？

所以串联我这些天学到的知识，大致流程图如下所示：

React Frontend

│

│ TokenId

▼

Marketplace

│

│ ownerOf(tokenId)

▼

ERC721 Contract

│

│ 查询 \_owners Mapping

▼

返回真正 Owner

│

▼

Marketplace 比较： owner == msg.sender

今天主播就学到这里，明天一定要学完！（而且我是一边学一边写笔记...其实学完了把今天所学的知识整理成笔记才更利于我学习，明天就按照这样做吧！）
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->

## **今日学习笔记：**

智能合约的运行其实是contract A调用contract B,执行 contract B的代码再返回contract A.

所以智能合约之间本质就是**函数的调用**。

所以区块链有两种地址：

1.  **EOA**（externally Owned Account）普通钱包 ：这个就只是接收和转账
    
2.  **Contract account** 一个程序的地址 ：这些地址里面有代码，可以执行
    

这就要提到我今天学的关于**重入攻击(Reentrancy Attack)**是如何运行的了。

之前ai一直教我说管理listings的合约必须先删除listings再操作我其实一直不理解，今天我知道了重入攻击的原理就是利用了“智能合约之间本质就是函数调用”。正常情况下我的marketplace的智能合约这样运行：

调用 contract => 给seller转ETH =>给buyer转NFT，

而重入攻击后则这样运行：

attack contract 调用我的 contract => buyItem() 给seller转ETH =>此时这里attack contract利用receive()再次调用我的contract ，因为listings未删除，还显示存在，所以继续 transfer ETH ...如此循环

历史上著名的The DAO Attack 就是利用了合约是先转再更新状态这一特点发动了攻击。

所以这里就引入了**CEI（checks efffects interactives）**的知识，先delect listings然后再转ETH时，attack contract再次进行调用时因为listings不存在了，所以直接revert，无法攻击，CEI是企业设计规范（Enterprise Security Pattern）。

AI告诉我一句话：**Update your own state before interacting with others** .

然后我了解到只有在用到这些方法时必须需要引入Reentrancy Guard：call()，transfer()，send()，safeTransferFrom()，调用其他合约，ETH Transfer。我理解的是只要有**_外部交互_**就需要小心！

今天姑且就学了这么点，**明天任务是把所有的代码都完成**。

（另外我还了解到其实先转NFT再转ETH和先转ETH再转NFT两种都没区别。。。。结果昨天学的时候ai还告诉我其中一种更好，但是无论哪一种两个都属于interactions啊，所以先后顺序根本没有统一标准！！！）
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->


### 今日学习笔记：

（主播在兼职工作所以为手机写的笔记可能不够精美）

因为第一节可hai老师主要在回答大家的问题，go leaning时间我是便挂会议边上班，所以没有认真听！主播就分享一下今天主播自学的一些笔记吧！（我认真听完了hao老师的一些回答，现在为了能够找到工作所以决定优先以运营开始了。）

我今天自己的小项目学习到要写一个**smart contract管理物品listings的状态。**

我很讨厌ai直接甩代码让我复制粘贴，所以叫她先给我讲思路然后我理顺了再开始写代码。

我自己整理了一下思路：

_这都是我请教AI了解到的知识(｡ì \_ í｡)_

像opeansea一些平台的marketplace都是一个合约管理多个NFT。但是根据不同的需求mapping也有不同的写法

一般有两种方式

**A** 把NFT的合约地址加上NFT的tokenid组成一个新的键（这种适用于无限NFT系列，然后storage更简单

**B**采用嵌套mapping，代码类似于_mapping（address => mapping（uint256 => Listing））public listings_ （这种就适合分类，因为如果要出不同系列的NFT那前端设计需要进行区别

后续学到了设计protocol，和CEI，check effect interactions。以及AI问我关于lisings是先transfer nft 然后transfer eth还是先transfer eth再transfer nft，我选择的是先转NFT再转ETH，然后ai跟我说其实两种只要任意环节失败了都会取消所有交易恢复到初始状态，但是大多数都选择先NFT然后ETH，是因为CEI，然后我后面要开始学习关于安全的问题了。

**以上就是今天学习的进度，因为手机打字设计排版实在太麻烦了，主播只写这么多了，明天我要先学了之后用电脑完成我的学习日记！ヽ(；▽；)ノ**
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
