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
# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# NFTBadge 合约（V0.1）评测报告

## 一、合约整体结构拆解

### 1\. 文件头部与版本声明

1.  MIT 开源协议声明 + 固定编译器版本 `0.8.26`，自带溢出保护，无需 SafeMath。
    
2.  内置 `IERC721Receiver` 接口：实现安全转账回调，兼容合约地址接收 NFT。
    

### 2\. 标准事件定义

实现全套 ERC721 规范事件：`Transfer` / `Approval` / `ApprovalForAll`，链下浏览器、前端可监听交易。

### 3\. 全局状态变量

-   权限变量：`owner` 合约管理员地址
    
-   只读常量：`name`、`symbol` NFT 系列名称标识
    
-   可配置变量：`imageBaseURI` 图片 IPFS 前缀
    
-   四组核心存储 mapping（私有，外部不可直接读取）：
    
    1.  `_owners`：tokenId → 持有人
        
    2.  `_balances`：地址 → 持有 NFT 数量
        
    3.  `_tokenApprovals`：单枚 NFT 授权地址
        
    4.  `_operatorApprovals`：全局批量授权
        

### 4\. 修饰器、构造函数

-   `onlyOwner`：管理员权限校验修饰器
    
-   `constructor`：部署时初始化 owner 与图片 URI
    

### 5\. 写入函数（Write，消耗 Gas）

1.  `setImageBaseURI`：管理员修改图片前缀
    
2.  `batchMint`：管理员批量铸造徽章（单次上限 100）
    
3.  `transferBadge`：简化版用户自主转账
    
4.  `approve`：单枚 NFT 授权
    
5.  `setApprovalForAll`：全部 NFT 批量授权
    
6.  `transferFrom`：标准 ERC721 转账
    
7.  `safeTransferFrom`：安全转账（校验合约接收者）
    

### 6\. 读取函数（Read，免费 call）

1.  `ownerOf`：查询 NFT 持有人
    
2.  `balanceOf`：查询地址持有数量
    
3.  `tokenURI`：读取 NFT 链上元数据
    
4.  `getApproved`：查询单枚授权地址
    
5.  `isApprovedForAll`：查询全局授权状态
    
6.  `supportsInterface`：ERC165 接口识别
    

### 7\. 内部工具函数（internal/private，外部无法调用）

`_mint` / `_transfer` / `_exists` / `_isApprovedOrOwner` / `_checkOnERC721Received` / `_toString`

负责底层铸造、转账、数字转字符串等底层逻辑。

* * *

# 二、6 项关键点逐项检查

## 1\. 合约是否能编译

**判断：可直接在 Remix 0.8.26 无报错编译**

理由：

1.  无外部 import 依赖，仅自定义接口，无库导入冲突；
    
2.  语法完全匹配 0.8.26，无过时 API；
    
3.  try/catch、abi.encodePacked、calldata/memory 区分均规范；
    
4.  所有 require 参数、函数返回值、mapping 读写逻辑无语法错误。
    

> 无需人工修改，直接复制进 Remix 即可编译。

## 2\. 函数是否符合预期需求

原始需求：

-   NFT Badge 徽章、管理员批量铸造；
    
-   至少 1 个 read、1 个 write 函数；
    
-   Remix 可用、标准 ERC721 转让授权；
    
-   单次批量铸造最多 100 枚。
    

**判断：全部满足预期**

1.  Write 代表：`batchMint`（批量铸造）；
    
2.  Read 代表：`ownerOf`（查询持有者）；
    
3.  铸造权限仅 owner，普通用户只能转账授权；
    
4.  tokenURI 生成链上 JSON 元数据，无需预上传 IPFS 即可测试；
    
5.  完整实现 ERC721 全套标准函数，兼容 NFT 市场。
    
    轻微不足：缺少单枚 mint 函数，Remix 小批量测试繁琐，属于体验优化，不影响功能合规。
    

## 3\. 是否存在明显权限问题

### 现有合理权限控制

1.  `batchMint` / `setImageBaseURI` 加 `onlyOwner`，仅部署者可操作；
    
2.  转账、授权仅持有人 / 授权操作员可调用；
    
3.  禁止铸造 / 转账至零地址；
    
4.  批量铸造校验 tokenId 不重复。
    

### 权限缺陷（人工判断）

**严重缺陷：无所有权转移函数** `transferOwnership`

-   若部署私钥丢失、项目交接，合约永久无法修改图片前缀、无法铸造新徽章；
    
-   无紧急暂停、管理员替换机制，生产环境存在单点故障风险。
    

## 4\. 是否使用了不必要的复杂逻辑

**判断：整体简洁，仅一处轻微冗余**

1.  自行实现全套 ERC721，没有引入 OpenZeppelin 重型库，轻量化适配作业 / Remix 演示；
    
2.  `_toString` 手写数字转字符串，无第三方依赖，适合教学；
    
3.  try-catch 安全转账逻辑为标准规范，不属于多余逻辑；
    
    冗余点：同时提供 `transferBadge`（简化转账）+ `transferFrom`（标准转账），功能重叠，教学场景可删除其一降低理解难度。
    

## 5\. 是否有潜在安全风险

### 低风险隐患（作业需标注）

1.  **批量铸造循环 gas 风险**
    
    batchMint 循环内逐个 mint，若传入 100 个合约地址接收 NFT，gas 消耗极高，极端情况交易区块 gas 不足导致交易失败，部分已铸造、部分未铸造，无回滚。
    
2.  **无铸造总量上限**
    
    任何人只要是 owner 可以无限次批量铸造，无总发行量限制，若业务需要固定供应量会失控。
    
3.  无管理员权限收回机制，owner 私钥丢失合约核心功能锁死。
    

### 无高危漏洞

无重入漏洞、无整数溢出（0.8.26 自带保护）、无任意铸造、无零地址铸造、无授权逻辑越权。

## 6\. 变量和函数命名是否容易理解

**判断：命名规范、可读性强**

1.  变量：`owner` / `imageBaseURI` / `_balances` 语义直观；
    
2.  函数：`batchMint`、`transferBadge`、`tokenURI` 符合行业命名习惯；
    
3.  私有函数统一下划线开头 `_xxx`，区分对外接口；
    
4.  注释清晰标注 read/write、参数作用、报错提示；
    
    无模糊、简写难懂命名。
    

# 合约源码：

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

/**
 * @title NFT Badge
 * @notice 基础 ERC-721 徽章合约：部署者批量铸造，持有者可转让。
 * @dev 本合约为独立示例，不需要导入外部库。
 */
contract NFTBadge {
    // ERC-721 安全接收回调的固定返回值：onERC721Received(address,address,uint256,bytes)
    // 将接口标识直接内联为常量，文件内仍只有 NFTBadge 一个可部署合约。
    bytes4 private constant _ERC721_RECEIVED = 0x150b7a02;

    // ERC-721 标准事件
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    /// @notice 合约部署者，也是批量铸造权限的唯一持有者。
    address public owner;

    /// @notice NFT 集合名称。
    string public constant name = "NFT Badge";

    /// @notice NFT 集合符号。
    string public constant symbol = "NBG";

    /// @notice 图片 URI 前缀。部署者可在初始化时提供 IPFS 目录链接。
    /// @dev 示例：ipfs://bafybeiexamplecid/ 。最终图片会拼为 imageBaseURI + tokenId + ".png"。
    string public imageBaseURI;

    // tokenId => 持有者地址
    mapping(uint256 => address) private _owners;

    // 持有者地址 => 持有数量
    mapping(address => uint256) private _balances;

    // tokenId => 单枚授权操作地址
    mapping(uint256 => address) private _tokenApprovals;

    // owner => operator => 是否获批操作全部 NFT
    mapping(address => mapping(address => bool)) private _operatorApprovals;

    /**
     * @notice 部署合约并指定图片 URI 前缀。
     * @param initialImageBaseURI 图片 IPFS 目录前缀；可先填占位值，后续用 setImageBaseURI 更新。
     */
    constructor(string memory initialImageBaseURI) {
        owner = msg.sender;
        imageBaseURI = initialImageBaseURI;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "NFTBadge: caller is not the owner");
        _;
    }

    /**
     * @notice 设置 NFT 图片 URI 前缀。
     * @dev 仅部署者可调用。为了让演示更简单，此设置会影响所有 tokenURI 中的图片链接。
     * @param newImageBaseURI 新的图片目录前缀，例如 ipfs://bafy.../。
     */
    function setImageBaseURI(string calldata newImageBaseURI) external onlyOwner {
        imageBaseURI = newImageBaseURI;
    }

    /**
     * @notice 【写入函数】由部署者批量铸造 NFT 徽章。
     * @dev 接收地址和徽章 ID 按相同下标一一匹配；单次最多 100 枚。
     * @param recipients 每枚徽章的接收地址数组，不能包含零地址。
     * @param tokenIds 要铸造的徽章 ID 数组，必须尚未被铸造。
     */
    function batchMint(
        address[] calldata recipients,
        uint256[] calldata tokenIds
    ) external onlyOwner {
        uint256 quantity = recipients.length;

        require(quantity > 0, "NFTBadge: mint quantity is zero");
        require(quantity == tokenIds.length, "NFTBadge: array length mismatch");
        require(quantity <= 100, "NFTBadge: max 100 tokens per batch");

        for (uint256 i = 0; i < quantity; i++) {
            require(recipients[i] != address(0), "NFTBadge: mint to zero address");
            require(!_exists(tokenIds[i]), "NFTBadge: token already minted");
            _mint(recipients[i], tokenIds[i]);
        }
    }

    /**
     * @notice 【写入函数】由当前持有者主动转让自己持有的徽章。
     * @dev 本函数仅允许 tokenId 的当前持有者调用，不接受代操作人调用。
     *      如需按 ERC-721 标准通过授权地址操作，可使用 transferFrom 或 safeTransferFrom。
     * @param to 接收者地址，不能为零地址。
     * @param tokenId 要转让的徽章 ID。
     */
    function transferBadge(address to, uint256 tokenId) external {
        require(to != address(0), "NFTBadge: transfer to zero address");
        require(ownerOf(tokenId) == msg.sender, "NFTBadge: caller is not token owner");
        _transfer(msg.sender, to, tokenId);
    }

    /**
     * @notice 【读取函数】查询指定徽章的当前持有者。
     * @param tokenId 徽章 ID。
     * @return 当前持有者地址；未铸造的 ID 会回退。
     */
    function ownerOf(uint256 tokenId) public view returns (address) {
        address tokenOwner = _owners[tokenId];
        require(tokenOwner != address(0), "NFTBadge: invalid token ID");
        return tokenOwner;
    }

    /**
     * @notice 【读取函数】查询某地址持有的徽章数量。
     * @param account 要查询的钱包地址。
     * @return 该地址当前持有的 NFT 数量。
     */
    function balanceOf(address account) external view returns (uint256) {
        require(account != address(0), "NFTBadge: zero address");
        return _balances[account];
    }

    /**
     * @notice 【读取函数】获取徽章元数据 URI。
     * @dev 返回 Data URI，其中包含 name、description、image 三项基础 JSON 信息。
     *      图片地址格式为 imageBaseURI + tokenId + ".png"。
     * @param tokenId 徽章 ID。
     * @return 符合常见 NFT 市场读取方式的 JSON Data URI。
     */
    function tokenURI(uint256 tokenId) external view returns (string memory) {
        require(_exists(tokenId), "NFTBadge: invalid token ID");

        return string(
            abi.encodePacked(
                "data:application/json;utf8,{\"name\":\"NFT Badge #",
                _toString(tokenId),
                "\",\"description\":\"A basic on-chain NFT badge.\",\"image\":\"",
                imageBaseURI,
                _toString(tokenId),
                ".png\"}"
            )
        );
    }

    /**
     * @notice 判断该合约是否实现 ERC-165、ERC-721、ERC-721 Metadata 接口。
     */
    function supportsInterface(bytes4 interfaceId) external pure returns (bool) {
        return
            interfaceId == 0x01ffc9a7 || // ERC-165
            interfaceId == 0x80ac58cd || // ERC-721
            interfaceId == 0x5b5e139f;   // ERC-721 Metadata
    }

    /** @notice 授权一个地址操作指定 tokenId。 */
    function approve(address to, uint256 tokenId) external {
        address tokenOwner = ownerOf(tokenId);
        require(to != tokenOwner, "NFTBadge: approval to current owner");
        require(
            msg.sender == tokenOwner || isApprovedForAll(tokenOwner, msg.sender),
            "NFTBadge: caller is not owner nor approved for all"
        );

        _tokenApprovals[tokenId] = to;
        emit Approval(tokenOwner, to, tokenId);
    }

    /** @notice 查询指定 tokenId 的单枚授权地址。 */
    function getApproved(uint256 tokenId) public view returns (address) {
        require(_exists(tokenId), "NFTBadge: invalid token ID");
        return _tokenApprovals[tokenId];
    }

    /** @notice 设置或取消对某操作地址的全部 NFT 授权。 */
    function setApprovalForAll(address operator, bool approved) external {
        require(operator != msg.sender, "NFTBadge: approve to caller");
        _operatorApprovals[msg.sender][operator] = approved;
        emit ApprovalForAll(msg.sender, operator, approved);
    }

    /** @notice 查询某操作地址是否被 owner 授权管理全部 NFT。 */
    function isApprovedForAll(address tokenOwner, address operator) public view returns (bool) {
        return _operatorApprovals[tokenOwner][operator];
    }

    /**
     * @notice 标准 ERC-721 转让函数。
     * @dev 当前持有者、单枚获批地址或全局获批地址均可调用。
     */
    function transferFrom(address from, address to, uint256 tokenId) public {
        require(_isApprovedOrOwner(msg.sender, tokenId), "NFTBadge: caller is not owner nor approved");
        require(ownerOf(tokenId) == from, "NFTBadge: incorrect from");
        require(to != address(0), "NFTBadge: transfer to zero address");
        _transfer(from, to, tokenId);
    }

    /**
     * @notice 标准 ERC-721 安全转让函数。
     * @dev 当接收方是合约时，会检查其是否能接收 ERC-721；否则交易回退，防止 NFT 被误锁。
     */
    function safeTransferFrom(address from, address to, uint256 tokenId) external {
        safeTransferFrom(from, to, tokenId, "");
    }

    /**
     * @notice 带附加数据的标准 ERC-721 安全转让函数。
     * @param from 当前持有者地址。
     * @param to 接收地址。
     * @param tokenId 要转让的徽章 ID。
     * @param data 传给接收方合约的附加数据。
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes memory data
    ) public {
        transferFrom(from, to, tokenId);
        require(_checkOnERC721Received(from, to, tokenId, data), "NFTBadge: unsafe recipient");
    }

    /** @dev 内部铸造逻辑。 */
    function _mint(address to, uint256 tokenId) internal {
        _owners[tokenId] = to;
        _balances[to] += 1;
        emit Transfer(address(0), to, tokenId);
    }

    /** @dev 内部转让逻辑：更新余额、所有权并清除单枚授权。 */
    function _transfer(address from, address to, uint256 tokenId) internal {
        delete _tokenApprovals[tokenId];
        _balances[from] -= 1;
        _balances[to] += 1;
        _owners[tokenId] = to;
        emit Transfer(from, to, tokenId);
    }

    /** @dev 判断 tokenId 是否已铸造。 */
    function _exists(uint256 tokenId) internal view returns (bool) {
        return _owners[tokenId] != address(0);
    }

    /** @dev 判断 spender 是否有权操作 tokenId。 */
    function _isApprovedOrOwner(address spender, uint256 tokenId) internal view returns (bool) {
        address tokenOwner = ownerOf(tokenId);
        return (
            spender == tokenOwner ||
            getApproved(tokenId) == spender ||
            isApprovedForAll(tokenOwner, spender)
        );
    }

    /**
     * @dev 当 to 为合约地址时，调用 onERC721Received 进行安全接收校验。
     *      使用低级调用及内联 selector，因此不需要单独定义 IERC721Receiver 接口。
     */
    function _checkOnERC721Received(
        address from,
        address to,
        uint256 tokenId,
        bytes memory data
    ) private returns (bool) {
        if (to.code.length == 0) {
            return true;
        }

        (bool success, bytes memory returndata) = to.call(
            abi.encodeWithSelector(_ERC721_RECEIVED, msg.sender, from, tokenId, data)
        );

        return success && returndata.length >= 32 && abi.decode(returndata, (bytes4)) == _ERC721_RECEIVED;
    }

    /** @dev uint256 转字符串，用于拼接 tokenURI。 */
    function _toString(uint256 value) internal pure returns (string memory) {
        if (value == 0) return "0";

        uint256 temp = value;
        uint256 digits;
        while (temp != 0) {
            digits++;
            temp /= 10;
        }

        bytes memory buffer = new bytes(digits);
        while (value != 0) {
            digits -= 1;
            buffer[digits] = bytes1(uint8(48 + uint256(value % 10)));
            value /= 10;
        }
        return string(buffer);
    }
}
```
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->


# Build Log初稿模板

文档版本：V1.0

周期：Week 1 钱包创建 + 领测试币 + 简易 NFT Badge 合约部署

网络信息：Monad 测试网 ChainID=10143 RPC：[https://rpc.testnet.monad.xyz](https://rpc.testnet.monad.xyz)

用途：个人学习过程记录、踩坑复盘、安全自查

## 统一记录表头

| 操作人 | 操作时间 | 操作内容 | 遇到问题 | 遇到问题 | 测试结果 | 安全自查 |
|   |   |   |   |   |   |   |
|   |   |   |   |   |   |   |

## 一、钱包模块 Build Log（本周基础操作）

### 阶段 1：创建适配 Monad 测试网钱包

1.  MetaMask 搭建钱包，生成助记词、私钥
    
2.  MetaMask 手动添加 Monad 测试网，校验 ChainID 避免假网络钓鱼
    
    | 功能点 | 操作内容 | 踩坑点 & 解决方案 | 测试结果 | 安全自查 |
    | --- | --- | --- | --- | --- |
    | 钱包创建 | 生成 12 位助记词、导出私钥 | 误把私钥发群内，立刻新建钱包作废旧地址 | 钱包地址正常生成 | 助记词离线手写保存，不上传聊天软件 |
    | 网络配置 | MetaMask 新增 Monad 测试网 | RPC 链接复制错误，无法切换网络 | 成功识别 ChainID10143 | 每次联网先核对链 ID，拒绝陌生自定义 RPC |
    | 测试币领取 | 官方水龙头领 MON 测试币 | 水龙头限流，多次提交才到账 | 钱包余额显示测试 MON | 不使用第三方不知名水龙头防盗币 |
    

### 阶段 2：基础转账测试

1.  钱包内部转账，测试 EIP1559 签名、Gas 设置
    
2.  观察 Monad 异步出块、交易确认延迟现象
    

-   问题：转账后余额迟迟不更新
    
-   解决：循环轮询区块，等待 Finalized 最终确认区块再读取余额
    

## 二、交易模块 Build Log（合约部署 + 转账交易）

### 阶段 1：交易完整流程实操

流程：构造合约部署交易→本地签名→广播至 Monad 测试网→查询交易回执

Monad 特殊点：并行执行，Gas 消耗波动大，手动调高 Gas 缓冲防止失败

| 问题编号 | 出现现象 | 原因 | 解决方案 | 验证状态 |
| --- | --- | --- | --- | --- |
| TX-W1-01 | 部署合约报错 OutOfGas | 默认 Gas 上限过低，Monad 执行消耗更高 | Gas 设置上调 1.5 倍缓冲 | 已解决，部署成功 |
| TX-W1-02 | 交易广播后长时间无回执 | 测试网节点拥堵，异步打包慢 | 增加 30 秒循环轮询查询交易状态 | 已解决 |

### 阶段 2：本周交易台账

1.  水龙头领币交易30笔
    
2.  钱包互转测试交易1笔
    
3.  NFT Badge 合约部署交易 1 笔
    
    全部交易保存哈希，可在 Monad 浏览器溯源
    

## 三、合约模块 Build Log（本周核心：简易 NFT Badge）

### 阶段 1：合约编写环境与代码

工具：remix

合约功能：极简 NFT 徽章，仅包含铸造 mint、读取持有者、总发行量，无复杂逻辑

核心代码特点：基础 ERC721 模板，关闭危险外部调用，仅部署者可批量铸造徽章

### 阶段 2：本地编译 + 测试网部署记录

| 漏洞 / 问题编号 | 风险等级 | 问题描述 | 修复方式 | 复测结果 |
| --- | --- | --- | --- | --- |
| CON-W1-01 | 低风险 | 未限制铸造权限，任何人可无限 mint | 添加 onlyOwner 铸造权限修饰符 | 修复通过 |

### 阶段 3：合约交互测试

调用 mint 函数铸造 3 枚测试徽章，钱包可读取持有 NFT，功能正常

## 四、AI 协作模块 Build Log

### 本周 AI 使用场景

1.  零基础看不懂 Solidity，AI 生成极简 NFT Badge 基础模板
    
2.  部署报错 OutOfGas，AI 解释 Monad Gas 特殊机制并给出修改代码
    
3.  自动整理代码注释、简化难懂的链交互逻辑
    

### AI 安全管控记录

1.  编写合约时不粘贴私钥、助记词、钱包地址到 AI 对话
    
2.  AI 生成代码后人工逐行检查，不直接复制部署
    
3.  高风险函数（转账、铸造）手动二次校验逻辑再上链
    

### 效果记录

快速上手基础 ERC721，缩短零基础写合约试错时间，识别代码简单漏洞

## 五、安全统一记录台账（本周重点自查）

### 5.1 本周遇到全部风险点

1.  私钥 / 助记词泄露风险
    
2.  虚假 Monad 测试网 RPC 钓鱼风险
    
3.  合约无权限控制，任意用户无限铸造
    
4.  第三方未知水龙头盗币风险
    

### 5.2 分模块安全自查清单

1.  钱包：私钥本地离线存储，不截图、不上传社交软件
    
2.  交易：每次转账前核对接收地址，确认链 ID 正确
    
3.  合约：基础权限控制、无未校验外部输入
    
4.  AI：对话屏蔽所有敏感钱包信息，AI 代码人工审核
    

### 本周安全事件记录

表格

| 漏洞 ID | 所属模块 | 严重程度 | 事件经过 | 处置方案 | 复盘优化 |
| --- | --- | --- | --- | --- | --- |
| SEC-W1-01 | 钱包 | 中 | 误将私钥粘贴至 AI 对话框 | 立即废弃该钱包地址，新建钱包，清除对话记录 | 以后所有 AI 对话绝不输入私钥、助记词 |

### 本周上线验收项

1.  合约编译无报错，基础权限防护到位
    
2.  所有操作使用官方 Monad 测试网 RPC
    
3.  敏感密钥离线保管，无二次泄露风险
    

## 六、文档修订记录

| 版本 | 日期 | 修改范围 | 编写人 | 审核 |
| --- | --- | --- | --- | --- |
| V1.0 | Week1 学习结束 | 完成第一周全流程操作记录 |   |   |

## 简易填写规范

1.  每完成一步操作立刻记录，记录报错截图 / 交易哈希留存
    
2.  合约部署、转账等关键操作截图附在文档末尾
    
3.  安全相关记录重点标注，作为每周复盘核心
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->



# 一、Monad 基础定义、诞生背景与核心定位

## 1\. 什么是 Monad

Monad 是兼容 EVM 的高性能 Layer1 公链，分布式节点运行，无单一企业 / 政府可关停，目标解决以太坊**速度慢、手续费高、无法承载 C 端高频应用**的痛点。

-   比特币：仅转账，无法构建通用应用
    
-   以太坊：实现链上通用计算，但设计初衷只验证去中心化可行，未面向民用流畅体验
    
-   Monad：完整继承以太坊 Solidity/EVM 生态，在此基础上突破性能瓶颈，打造可日常使用的去中心化应用底层基础设施
    

## 2\. 核心原生资产 MON

1.  Gas 支付：所有交易、合约部署、交互手续费
    
2.  质押 Staking：锁定 MON 成为验证者，维护网络并获取奖励
    
3.  网络安全抵押储备（Reserve Balance 机制）
    

## 3\. 智能合约核心特性（EVM 完全兼容）

1.  使用 Solidity 编写，以太坊所有合约、工具、教程**无需修改直接部署**
    
2.  部署后不可篡改、无运营方可下线屏蔽，规则自动执行（自动售货机类比）
    
3.  无地域、无审查，满足条件自动执行；代码全网节点同步存储执行
    

# 二、Monad 核心性能指标 & 底层去中心化设计

## 1\. 关键硬指标

表格

| 参数 | 数值 | 用户价值 |
| --- | --- | --- |
| TPS | 10000 | 支撑海量 C 端高频交互 |
| 出块时间 | 400ms | 接近软件实时反馈 |
| 交易最终性 | 800ms | 用户无感，无需等待确认 |
| 区块 Gas 上限 | 200M gas | 超大批量交易承载 |
| 单笔 Gas 上限 | 30M gas | 适配超大合约逻辑 |
| 最低基础 Gas 费 | 100 MON-gwei | 极低手续费门槛 |

## 2\. 兼顾速度 + 去中心化（解决区块链三难）

多数高性能链为提速牺牲验证者规模、抬高硬件门槛，Monad 双向优化：

### （1）低硬件门槛

节点可运行在普通消费级电脑，普通人可参与节点，分散网络控制权，避免寡头垄断。

### （2）MonadBFT 共识：线性通信，支撑海量验证者

传统 BFT：验证者全互联，通信开销指数上涨，只能限制几十位验证者

MonadBFT：分发 - 汇总（fan-out/fan-in）模型

1.  区块 Leader 广播区块给所有验证者
    
2.  验证者直接投票给下一任 Leader
    
3.  通信开销线性增长，可扩容数百上千验证者，去中心化程度持续提升
    

### 底层三大创新引擎

1.  MonadBFT：亚秒级共识
    
2.  RaptorCast：高效区块广播协议
    
3.  MonadDB：定制 SSD 存储引擎，支撑高并发读写
    

# 三、10k TPS + 800ms 最终性：不止更快，重塑产品赛道

以太坊延迟高、Gas 昂贵，大量应用必须做妥协（链下缓存、乐观 UI、专属 Rollup）；Monad 亚秒级确认直接消除约束，六大全新可落地赛道**Monad**：

## 1\. 原生链上订单簿 DEX

过去只能用 AMM（恒定池），挂单 / 撤单高频操作成本过高；

亚秒延迟下完整订单簿可上链，借贷清算、衍生品、各类 DeFi 协议可直接接入链上撮合层，形成通用金融基础设施。

## 2\. 持久开放链上游戏

传统游戏逻辑放服务器；Monad 适配**永久共享游戏世界**：

-   游戏逻辑、道具所有权全上链，开发者关停服务器游戏仍可运行
    
-   道具跨游戏、跨 DeFi 流通，链同步玩家实时操作节奏
    

## 3\. 完整链上社交图谱

现有 Lens/Farcaster 仅身份上链，点赞、转发、评论全部链下；

10k TPS 可将全部社交交互上链，社交图谱成为公共基础设施，任意应用可读取复用，不被单一平台垄断。

## 4\. 可用链上微支付

过往小额付费（付费阅读、API 按量计费、创作者打赏）Gas 成本高于收益，无法落地；

Monad 低廉稳定 Gas 实现流式小额支付，同时兼容整套 EVM DeFi 生态，支付与金融可自由组合。

## 5\. AI 链上智能体自动化交易

AI 机器人交易频率远超人类，以太坊延迟、Gas 阻碍自动化策略；

Monad 机器级响应速度，支持非托管、高频 AI 量化、自动清算智能体。

## 6\. 高速同步衍生品 / 代币化现实资产

预言机可同步中心化交易所实时价格，永续合约、代币股票、高频再平衡结构化产品可行，不会因区块滞后产生巨额滑点亏损。

## 必须留在链下的数据（混合架构不变）

1.  私密聊天、隐私数据
    
2.  大图 / 视频 / 大文件（仅哈希上链，文件存 IPFS/Supabase）
    
3.  每秒数十万传感器超高频数据流
    
4.  视频转码、AI 推理等重型计算
    
5.  可信任中心化服务商承载的业务逻辑
    

# 四、Web2 数据库（Supabase）vs 区块链：数据分层标准

## 1\. 传统 Web2 存储适用场景（Supabase 数据库 + Storage）

-   私有用户数据：头像、私信、个人资料、表单内容
    
-   高频可变轻数据：点赞数、浏览量、临时状态
    
-   复杂多条件搜索、筛选、排序需求
    
-   大规模低成本存储图片、视频、PDF 文件
    
-   业务迭代频繁、可随时修改逻辑的业务后台
    

### 数据库基础概念

1.  数据库 = 应用持久化记忆，由多张表组成，行 = 单条记录，列 = 字段
    
2.  SQL：通用数据库交互语言，四大核心指令
    
    -   SELECT 查询 / INSERT 新增 / UPDATE 修改 / DELETE 删除
        
3.  文件存储分工：文件本体存桶，数据库仅保存文件访问 URL，避免数据库臃肿卡顿
    

### Supabase 安全规范

1.  仅使用 anon 公开密钥，service\_role 密钥严禁暴露前端 / Git
    
2.  生产环境必须开启 RLS 行级权限隔离，本地测试可临时关闭
    
3.  上传限制 MIME 类型、文件大小，防止 XSS 与存储滥用
    
4.  Bucket 默认私有，仅公开资源才开启公共访问
    

## 2\. 区块链（Monad）适用场景

-   资产所有权、交易历史、合约规则（不可篡改、公开可验证）
    
-   需要无中介、永久留存、不受单一平台控制的数据
    
-   价值流转、质押、治理投票、NFT 权属、代币账务
    

## 3\. 标准混合 DApp 架构（行业通用）

链上（Monad）：资产、交易、合约逻辑、所有权证明

链下（Supabase/IPFS）：用户资料、图片、私信、统计数据、复杂检索

# 五、生产级应用完整建设清单（Web2 基础能力）

从 Demo 到商用产品必须覆盖五大模块，所有 Web3 前端 / 后台均依赖这套底层基建：

## 1\. 产品体验打磨

响应式布局、深色模式、无障碍适配、Favicon、页面标题、OpenGraph 社交分享预览

## 2\. 获客与数据分析

SEO 搜索引擎优化、用户行为埋点分析、独立自定义域名

## 3\. 核心业务功能

1.  数据库持久化
    
2.  用户认证（OAuth、密码哈希、会话、邮箱验证，禁止自研鉴权）
    
3.  文件对象存储
    
4.  支付服务商对接
    
5.  程序化邮件、消息通知
    

## 4\. 基础设施与安全

1.  环境变量保管密钥（API Key、RPC 地址，禁止硬编码进代码仓库）
    
2.  CI/CD 自动构建部署、预发布测试环境
    
3.  HTTPS 全站加密、输入消毒防 SQL 注入 / XSS
    
4.  API 接口限流防 CC 攻击、全局错误捕获、线上异常追踪
    
5.  数据库定时备份、单元 / 集成 / 端到端测试
    

## 5\. 合规法律文件

隐私政策、服务条款、欧盟 GDPR Cookie 授权弹窗

# 六、Monad vs 以太坊：开发关键差异

## 1\. 虚拟机与合约限制

1.  合约最大字节码：以太坊 24KB → Monad 128KB；部署初始化代码上限 256KB
    
2.  新增 secp256r1 预编译，原生支持 WebAuthn 通行密钥链上校验（EIP-7951）
    
3.  部分操作码、预编译 Gas 定价重新调整，适配并行执行资源消耗
    

## 2\. 交易机制重大区别

1.  **扣费规则：按 gas\_limit 全额扣费，而非实际 gas\_used**
    
    为异步并行执行防 DoS 攻击；预估 Gas 不可靠场景建议写死固定 Gas 上限
    
2.  无全局内存池，交易仅转发给后续少数区块 Leader
    
3.  不支持 EIP-4844 Blob 交易（Type3）
    
4.  Reserve Balance 储备余额机制：交易入块前校验账户足额 MON，执行失败依然扣除 Gas
    

## 3\. EIP-7702 账户代理特殊限制

1.  委托 EOA 账户余额不能低于 10 MON（解除委托后无限制）
    
2.  委托账户禁止使用 CREATE/CREATE2 部署新合约
    

## 4\. 历史数据与 RPC

1.  高吞吐量导致全节点不存储全部历史状态，高频查历史事件必须使用索引器（The Graph、Envio、Goldsky 等）
    
2.  RPC 接口存在部分差异化，开发需适配 Monad 专属 RPC 规范
    

# 七、Monad Gas 计费完整规则 & 开发者规范

## 1\. 计费公式

实际扣费 MON = gas\_limit × gas 单价

gas 单价 = min (基础 BaseFee + 用户优先级 PriorityFee, 用户最高 MaxFee)

## 2\. BaseFee 动态调节机制

1.  最低底价固定 100 MON-gwei
    
2.  区块目标 Gas 负载 160M（区块容量 80%）
    
3.  相比以太坊：拥堵时涨价更慢，空闲时降价更快，避免长期高额手续费
    

## 3\. 开发者强制最佳实践

1.  固定 Gas 上限：转账、简单合约调用 Gas 消耗固定，直接硬编码，不调用 eth\_estimateGas
    
    -   规避钱包估算失败自动填充超高 gas\_limit，造成用户资产损失
        
2.  区分同步 / 批量 RPC 请求，减少重复 Gas 预估网络往返
    

# 八、Monad 高性能开发最佳实践

## 1\. RPC 与链上读取优化

1.  固定 Gas 替代 Gas 预估，减少前端网络延迟
    
2.  多 eth\_call 合并：Multicall3 批量读取余额 / 授权，或自定义聚合合约
    
3.  RPC 请求批量并行发送，串行请求大幅拉高延迟
    
4.  高频历史事件监听**禁止循环调用 eth\_getLogs**，必须接入链上索引器
    

## 2\. 主流索引器适配 Monad 配置

| 索引工具 | Monad 主网标识 | 测试网 ID |
| --- | --- | --- |
| Envio HyperIndex | monad-mainnet | 10143 |
| Goldsky | monad-mainnet | monad-testnet |
| The Graph Subgraph | monad-mainnet | monad-testnet |
| Allium | monad_mainnet | monad_testnet |
| QuickNode Streams | monad-mainnet | monad-testnet |

## 3\. 批量交易并发提交

多笔交易循环串行发送效率极低，通过 Promise.all 批量并行提交，本地维护 nonce 避免 RPC 读取计数阻塞。

## 4\. 托管与成本优化

高流量生产环境推荐 AWS 架构：S3 静态文件 + CloudFront CDN + Lambda 服务 + ECS 容器 + RDS 数据库，精细化控制带宽、存储成本。

# 九、Web3 项目 Build Log（开发日志）标准化搭建

## 1\. Build Log 定义

全程记录项目从需求、合约开发、前端、测试网部署、主网上线、迭代故障、性能调优的完整开发档案，用于复盘、团队同步、审计溯源、新人交接。

## 2\. 日志分四大模块

### 模块 1：项目基础档案（固定头部）

-   项目名称、赛道、Monad 测试网 / 主网合约地址
    
-   技术栈：Solidity+Foundry、Viem、NextJS、Supabase、索引器
    
-   团队分工、迭代周期、里程碑规划
    

### 模块 2：每日开发流水（按日期记录）

1.  当日需求、合约新增 / 修改函数
    
2.  SQL 建表、Storage 桶配置、RLS 权限变更
    
3.  RPC / 索引器接入调试记录
    
4.  Gas 优化、交易并发、TPS 压力测试结果
    
5.  Bug、漏洞、复现步骤、修复方案
    
6.  链上测试交互截图、交易哈希留存
    

### 模块 3：部署 & 上线日志（核心溯源）

1.  测试网部署脚本、构造参数、部署钱包地址
    
2.  合约审计报告、高危漏洞修复记录
    
3.  主网部署时间、区块高度、初始参数配置
    
4.  上线监控指标：TPS、Gas 均价、用户交互量、报错率
    
5.  升级 / 代理合约变更记录、治理投票哈希
    

### 模块 4：性能与迭代复盘（周 / 月度）

1.  Monad 链上延迟、手续费用户体验数据
    
2.  数据库查询慢请求优化方案
    
3.  索引器同步延迟问题处理
    
4.  产品新增赛道落地进度（链上订单簿 / 游戏 / 社交）
    

## 3\. 工具选型

-   文档载体：Notion（团队协作）/Markdown 仓库（代码仓库同步 log 文件夹）
    
-   数据留存：每条链上操作附带 TxHash、区块浏览器链接
    
-   自动化补充：Sentry 错误日志、Prometheus 监控数据定期同步进 Build Log
    

# 十、Web3 三大职业方向完整总结（Tech / Ops / Research）

## 方向一：Tech 技术岗（核心开发层）

### 1\. 细分岗位与核心职责

（1）智能合约工程师

-   Solidity 合约开发（DeFi/NFT/DAO/RWA）、Foundry/Hardhat 单元测试
    
-   Gas 优化、兼容 Monad/EVM 规范、合约部署升级
    
-   对接前端，处理交易、授权、质押逻辑；修复重入、溢出等漏洞
    
    **技能栈**：Solidity、Foundry、Viem、EIP 标准、Monad 差异化规则
    

（2）合约安全审计工程师

-   静态 / 动态漏洞扫描（Slither、Mythril）、漏洞 PoC 复现
    
-   输出审计报告，提供修复方案，跟进复测；梳理 MEV、预言机操纵风险
    

（3）Web3 前端工程师

-   Next/React + Viem 钱包连接、链上数据渲染、批量交易交互
    
-   整合 Supabase 数据库、文件上传；处理交易确认、失败兜底 UI
    
-   适配 Monad 亚秒确认交互体验，实现高频点赞 / 挂单等功能
    

（4）后端 / 索引开发工程师

-   Node/Go API 服务、索引器配置、链上数据清洗入库
    
-   数据库分表优化、定时链上同步任务、风控监控接口
    

### 2\. Tech 岗通用能力要求

1.  精通 EVM 体系，掌握 Monad 高性能开发规范
    
2.  熟悉链下数据库 + 链上混合架构设计
    
3.  具备完整 Build Log 沉淀习惯，可完整溯源开发流程
    

## 方向二：Ops 运营运维岗（基础设施 + 社区生态）

### 1\. 细分岗位

（1）Web3 DevOps / 节点运维

-   Monad 验证者 / RPC 节点部署、容器 K8s 集群管理
    
-   CI/CD 合约自动部署、密钥 Vault 安全托管、监控告警（Prometheus）
    
-   节点性能调优、分叉升级、多区域分布式容灾架构
    

（2）社区运营

-   Discord/Telegram/X 社群维护、AMA、Twitter Space 线上活动
    
-   线下 Meetup、黑客松策划；KOL、跨项目社区联动
    
-   内容产出：项目教程、Monad 性能科普、新手开发指南
    

（3）产品运营

-   用户增长策略、链上行为数据分析、留存优化
    
-   产品迭代需求收集，对接 Tech 团队落地高频交互功能
    

### 2\. Ops 核心工具栈

-   运维：Docker、K8s、Terraform、Prometheus、Grafana
    
-   社区：Notion、Canva、Dune、Etherscan/Monad 区块浏览器
    
-   数据监控：Token Terminal、Glassnode、链上索引看板
    

## 方向三：Research 行业研究岗（市场、赛道、经济模型）

### 1\. 细分岗位

（1）行业研究员

-   赛道分析：DeFi、NFT、DAO、AI+Web3、RWA、高速公链（Monad 赛道）
    
-   竞品对比：以太坊 L2、Solana、Monad 性能与应用落地差异
    
-   输出行业周报、深度报告，预判 10k TPS 带来产品变革机会
    

（2）代币经济研究员

-   MON 类原生代币质押、Gas 销毁、通胀模型设计
    
-   流动性挖矿、DAO 金库分配、RWA 资产代币化机制建模
    

（3）链上数据分析师

-   SQL 查询索引器数据，监控 TVL、交易量、用户地址增长
    
-   分析 Monad 高频交易行为、AI 智能体链上活动、微支付落地数据
    

### 2\. Research 核心产出

1.  赛道机会白皮书（基于 Monad 10k TPS 新场景）
    
2.  代币经济学仿真测算
    
3.  竞品性能、手续费、用户体验横向对比报告
    
4.  项目投研尽调材料
    

## 三大方向核心差异对比

| 维度 | Tech 技术 | Ops 运维 / 社区 | Research 研究 |
| --- | --- | --- | --- |
| 核心产出 | 代码、合约、DApp 产品 | 节点基础设施、社区生态活动 | 行业报告、数据模型、赛道判断 |
| 核心目标 | 实现链上高性能产品 | 保障系统稳定、扩大用户社区 | 挖掘赛道机会、提供战略决策 |
| Monad 侧重点 | 适配高性能合约、索引开发 | 部署维护 Monad 节点、开发者社区运营 | 分析 10k TPS 赛道红利、生态增长数据 |
| 必备工具 | Foundry、Viem、Supabase | K8s、Discord、监控面板 | Dune、索引 SQL、数据可视化 |
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->




## 一、AI 生成最小 Solidity 合约（原则：技术辅助，绝不盲信）

### 1.1 「最小合约」明确定义

结合入门教学场景与安全规范，最小合约需满足**无业务冗余、无资金风险、逻辑自洽、适配 Remix 一键编译**四大标准，仅包含必要元素：

-   采用 Solidity 0.8.20 + 版本（编译器自带整数溢出检查，无需额外引入 SafeMath 库）；
    
-   仅实现链上基础数据存储（字符串 / 数字），不涉及 ETH 转账、外部合约调用、复杂权限逻辑；
    
-   必须包含：状态变量、构造函数（初始化链上存储）、`view`修饰的只读函数、`external`修饰的写入函数、带`indexed`参数的状态变更事件；
    
-   严格遵循 Checks-Effects-Interactions（CEI）安全模式，无复杂循环、无未校验的外部调用逻辑。
    

### 1.2 标准约束 AI 提示词

为避免 AI 输出存在语法漏洞、冗余代码或过时逻辑，需要精准约束生成规则，参考 Build Anything 的 Vibe 编程安全规范，提示词需明确以下要求：

```Plaintext
生成严格符合标准的Solidity合约，无任何冗余注释：

1. 基于Solidity 0.8.20+版本，顶部添加MIT SPDX开源协议；

2. 实现极简链上字符串存储，无代币转账、无外部合约依赖、无复杂权限控制；

3. 包含以下必备元素：状态变量、构造函数（部署时初始化存储）、view只读函数、external写入函数、带indexed参数的状态更新事件；

4. 严格遵循Checks-Effects-Interactions安全模式，写入函数对参数做非空校验；

5. 适配Remix编译器，正确使用memory/calldata内存修饰符，禁止使用0.8.x版本以前的SafeMath库；

6. 在函数注释中标明read/write属性、Gas消耗特性，禁止使用已过时的Solidity语法。
```

### 1.3 AI 输出校验后的标准最小合约示例

```Plaintext
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.20;

// 最小合约：链上极简留言存储板

contract MiniMessageBoard {

 // 状态变量：链上永久存储的核心数据

  string private storedMessage;

   // 状态变更事件：记录写入操作，添加indexed标签供前端高效过滤日志

   event MessageUpdated(address indexed updater, string newMessage);

   // 构造函数：合约部署时仅执行1次，初始化链上存储

   constructor(string memory initialMessage) {

     storedMessage = initialMessage;

       emit MessageUpdated(msg.sender, initialMessage);

   }

   // Read只读函数：view修饰，不修改链状态，免费调用无Gas消耗

      function getMessage() public view returns (string memory) {

      return storedMessage;
   }

   // Write写入函数：external修饰，修改链状态，需消耗Gas签名交易

   function setMessage(string calldata newMessage) external {

      // Checks：前置参数校验，避免无效操作

       require(bytes(newMessage).length > 0, "Message cannot be empty");

      // Effects：先更新合约状态（CEI安全模式核心，规避重入风险）

      storedMessage = newMessage;

       // Interactions：最后触发事件，无外部转账逻辑，彻底消除重入漏洞

       emit MessageUpdated(msg.sender, newMessage);

   }

}
```

### 1.4 强制流程：AI 合约三重校验（安全底线不可省略）

AI 仅能作为代码生成工具，其输出可能包含隐性语法错误、安全漏洞，必须执行以下三步校验后才可上链：

1.  **编译语法校验**：将 AI 生成的代码复制到 Remix 编译器，选择匹配的 Solidity 版本，若出现`ParserError`、`TypeError`等报错，需手动修复常见问题，包括版本声明不匹配、`memory/calldata`修饰符混用、关键字拼写错误、AI 自动引入的冗余依赖。
    
2.  **人工安全逻辑审查**：对照 Solidity 官方安全文档逐条检查：
    

-   敏感权限校验：无`onlyOwner`修饰的未授权管理函数，`msg.sender`的调用逻辑需明确校验；
    
-   CEI 模式合规性：写入函数必须先更新合约状态，再执行外部调用（本合约无外部转账，逻辑天然安全）；
    
-   剔除冗余代码：删除 AI 自动引入的不必要 OpenZeppelin 库、未使用的状态变量、冗余函数；
    
-   整数安全确认：0.8 + 合约禁用手动引入 SafeMath，编译器自动处理溢出检查。
    

3.  **静态漏洞扫描**：启用 Remix 内置的「Solidity Analyzer」插件，扫描重入攻击、未初始化状态变量、未标记`payable`的转账函数、硬编码敏感信息等隐性漏洞，**必须修复所有 Critical/High 级别的警告**，Medium/Low 级别的警告需人工确认无安全风险。
    

* * *

## 二、Remix 编译、检查、部署、交互完整步骤

### 2.1 前置环境准备

1.  打开 Remix 官方 IDE：[https://remix.ethereum.org/，推荐使用](https://remix.ethereum.org/，推荐使用) Chrome/Firefox 浏览器；
    
2.  安装 MetaMask 钱包，添加目标测试网（以 Monad Testnet 为例），配置 RPC URL、ChainID 等参数，参考 Monad 官方文档领取测试网代币（用于支付 Gas 费）；
    
3.  在 Remix 文件管理器（File Explorer）中新建文件`MiniMessageBoard.sol`，粘贴经过三重校验的合约代码。
    

### 2.2 步骤 1：编译合约（生成核心产物：ABI + 字节码）

1.  切换到左侧第 2 个图标「Solidity Compiler」面板；
    
2.  选择编译器版本：与合约顶部`pragma`声明的版本严格一致（本例为 0.8.20）；
    
3.  勾选「Auto compile」（自动编译），点击「Compile MiniMessageBoard.sol」；
    
4.  编译成功标志：面板右上角出现绿色对勾，自动生成两类核心产物：
    

-   **ABI（应用二进制接口）** ：JSON 格式的合约接口说明书，供链下程序解析合约函数；
    
-   **Bytecode（部署字节码）** ：合约部署到区块链的 EVM 可执行机器码。
    

### 2.3 步骤 2：安全静态检查

1.  切换到左侧第 4 个图标「Solidity Analyzer」面板（Slither 轻量化扫描工具）；
    
2.  勾选所有检查项，点击「Run Analyzer」启动扫描；
    
3.  扫描结果处理：修复所有「Critical」「High」级别的安全警告，「Warning」级别的提示需人工确认不影响合约安全逻辑。
    

### 2.4 步骤 3：选择部署环境

切换到左侧第 5 个图标「Deploy & Run Transactions」面板，根据测试需求选择三类环境：

-   **环境 1：Remix VM（本地虚拟链）** ：无需钱包授权、免费使用、重启后链数据自动清空，适合快速验证合约逻辑；
    
-   **环境 2：Injected Provider - MetaMask**：连接真实测试网 / 主网，需要钱包签名交易，用于合约上线前的正式验证；
    
-   **环境 3：Hardhat/Anvil 本地节点**：适配本地自研开发链，适合进阶全栈联调、Gas 优化测试。
    

### 2.5 步骤 4：部署合约（以 Monad 测试网为例）

1.  环境选择「Injected Provider - MetaMask」，在弹出的钱包授权窗口中，确认连接 Remix 与 MetaMask 钱包；
    
2.  确认 MetaMask 已切换至 Monad Testnet，部署面板显示当前钱包地址、网络 ChainID；
    
3.  输入构造函数参数：在「Deploy」按钮旁的输入框中，填写合约初始化留言，例如`"Hello Monad Testnet"`；
    
4.  点击「Deploy」按钮，MetaMask 弹出交易签名窗口，确认 Gas 费（测试网代币免费），点击「确认」广播交易；
    
5.  部署完成标志：Remix 终端打印部署交易哈希，下方「Deployed Contracts」区域生成合约地址。
    

### 2.6 步骤 5：合约读写交互

部署成功后，「Deployed Contracts」区域自动识别合约 ABI，渲染所有函数的交互式按钮：

1.  **调用 Read 只读函数（getMessage）** ：
    

-   按钮标识：蓝色背景，标注「view」，无 Gas 消耗提示；
    
-   操作：直接点击「getMessage」按钮，Remix 调用 RPC 节点的`eth_call`方法，在按钮下方即时返回合约存储的当前留言；
    
-   底层特性：只读调用不会被区块链打包，**不生成交易哈希**，无链上永久操作记录。
    

1.  **调用 Write 写入函数（setMessage）** ：
    

-   按钮标识：橙色背景，标注「transact」，提示需要消耗 Gas；
    
-   操作：在按钮旁的输入框中填写新留言，例如`"Monad测试网合约部署成功"`，点击「setMessage」；
    
-   流程：MetaMask 弹出签名窗口，确认支付 Gas 后，交易通过 RPC 节点广播到网络；矿工打包交易上链后，Remix 终端打印**唯一交易哈希（txHash）** ，合约状态更新完成。
    

### 2.7 步骤 6：交易与合约源码验证

1.  复制交易哈希：在 Remix 终端或 MetaMask「活动」页面，复制部署 / 交互的 txHash；
    
2.  打开对应区块浏览器：Monad Testnet 使用 Monad Scan，Sepolia 测试网使用 Etherscan；
    
3.  交易验证：粘贴 txHash，查看交易发起地址、目标合约地址、Gas 消耗、区块确认数、合约执行事件日志；
    
4.  合约源码验证：在区块浏览器的「Contract」标签，上传本地合约源码、ABI，验证链上字节码与本地代码一致性，通过后其他用户可直接在区块浏览器中交互式调用合约。
    

* * *

## 三、核心模块 3：五大核心要素关系解析

### 3.1 各要素专属核心定义

1.  **合约源码（Contract Source Code）** ：开发者 / AI 编写的 Solidity 可读业务逻辑，定义合约的状态变量、函数、事件、权限规则，是合约的原始逻辑依据，经编译转为 EVM 可执行字节码；
    
2.  **ABI（Application Binary Interface）** ：编译自动生成的 JSON 格式接口清单，记录合约所有函数的函数名、入参 / 出参数据类型、事件定义，是**链下程序与合约交互的唯一翻译规范**；
    
3.  **合约地址（Contract Address）** ：合约部署成功后，区块链根据发起地址、交易 nonce 计算生成的 20 字节十六进制唯一标识，是合约在链上的「存证地址」，同一份源码部署到不同网络，合约地址完全不同；
    
4.  **Read/Write 函数**：合约对外提供的两类交互接口，底层 RPC 逻辑、链上行为、业务场景完全不同；
    
5.  **Transaction Hash（txHash）** ：所有修改链状态的操作（合约部署、Write 函数调用）生成的 64 位十六进制全局唯一标识，是链上交易的「流水单号」，用于溯源、验证交易执行结果。
    

### 3.2 核心要素关系图

流程图完整覆盖：合约源码、ABI、合约地址、Read/Write函数、Transaction Hash（txHash）五大核心要素，严格对应合约编译、部署、交互、溯源全链路逻辑。遵循「源码编译→合约部署→链上交互→交易溯源」真实开发顺序，区分读写函数核心差异、txHash生成规则、ABI与合约地址的缺一不可特性。

![exported_image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/honora888/images/2026-07-09-1783605292742-exported_image.png)![合约五大核心要素高清可视化关系图.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/honora888/images/2026-07-09-1783605401205-________________.png)

### NOTE：

1\. 只有**修改链状态的操作**（部署、write函数）才会产生TxHash，read只读操作无TxHash；

2\. ABI只负责「翻译解析函数」，合约地址只负责「定位链上合约」，功能完全独立、缺一不可；

3\. 合约源码是底层逻辑，编译、部署、交互、溯源所有流程均依托源码展开；

4\. 读写函数的核心区别：是否消耗Gas、是否生成交易哈希、是否修改链上状态。

### **3.3 分阶段链路逻辑详解**

**链路 1：编译阶段（合约源码→ABI / 字节码）**

合约源码经 Remix 编译器校验语法后，输出两类不可拆分的核心产物：

-   **字节码**：包含合约的所有业务逻辑，被部署到区块链后，由 EVM 加载执行；
    
-   **ABI**：供 Remix、前端 Viem/Wagmi 等链下程序使用，将可读的函数名、入参类型，转换为区块链可识别的调用数据。
    

**链路 2：部署阶段（字节码→合约地址 + 部署 txHash）**

1.  Remix 将字节码、构造函数参数，通过 MetaMask 钱包私钥签名，打包成标准部署交易；
    
2.  交易通过 RPC 节点广播到区块链网络，矿工验证 Gas 费后，将交易打包进待确认区块；
    
3.  区块上链后，区块链根据交易发起地址、交易 nonce（账户发起交易的累计序号），计算生成**唯一合约地址**，同时为部署操作生成**唯一 txHash**；
    
4.  此时，合约地址与字节码一一对应，任何链下交互都必须同时使用合约地址和 ABI。
    

**链路 3：交互阶段（合约地址 + ABI→Read/Write 函数→txHash）**

链下程序必须同时持有**合约地址（定位目标合约）+ABI（解析合约函数）**，才能发起有效交互：

1.  **Read 函数调用链路**：
    
2.  链下程序通过 ABI 识别`view`/`pure`修饰的只读函数，拼接合约地址、函数签名、入参，调用 RPC 节点的`eth_call`方法；节点在本地 EVM 模拟执行合约，直接返回数据，**不广播交易、不消耗 Gas、不生成 txHash**，无链上永久记录，适合查询合约存储数据。
    
3.  **Write 函数调用链路**：
    
4.  链下程序通过 ABI 识别无`view`/`pure`修饰的写入函数，拼接合约地址、函数签名、入参，使用 MetaMask 私钥对交易进行 ECDSA 签名；通过 RPC 节点广播`eth_sendTransaction`请求，矿工将交易打包进区块，合约状态同步更新、触发状态变更事件，生成**唯一 txHash**；所有写入操作会被永久记录在区块链上，不可篡改。
    

**链路 4：溯源阶段（txHash→全链路反向查询）**

所有修改链状态的操作（部署、Write 函数调用）都会生成 txHash，在区块浏览器中输入 txHash，可反向还原完整操作链路：

-   查看交易关联的合约地址、发起钱包地址、目标合约地址；
    
-   查看合约执行的函数名称、入参、返回值，以及触发的事件日志；
    
-   查看 Gas 消耗、区块确认数、交易执行状态，定位交易失败原因；
    
-   验证链上合约源码与本地源码一致性，确保合约未被篡改。
    

* * *

## 四、整体知识框架总结

### 4.1 完整开发交互流程闭环

```Plaintext
AI生成标准最小合约 → 三重校验（编译+人工+静态扫描）→ Remix编译生成ABI/字节码 → 选择部署环境（本地VM→测试网）→ 签名部署交易生成合约地址+部署txHash → 链下程序绑定「合约地址+ABI」→ 发起Read/Write交互 → 写入操作生成txHash → 区块浏览器溯源验证 → 全链路功能确认
```

### 4.2 底层核心逻辑本质

1.  **AI 工具定位**：仅作为合约代码生成辅助工具，其输出存在语法漏洞、安全风险的概率极高，必须经过专业人工校验、静态扫描、多轮测试后，才可上链，绝对不可直接部署 AI 生成的原始代码；
    
2.  **Remix 定位**：集合约编写、编译、安全扫描、部署、多链交互于一体的 EVM 合约入门级开发 IDE，适配 Monad、Sepolia 等所有 EVM 兼容链，是新手验证合约逻辑的标准工具；
    
3.  **五大要素绑定关系**：合约源码是逻辑基础、ABI 是交互翻译规范、合约地址是链上定位标识、读写函数是两类交互接口、txHash 是链上操作的唯一凭证；**缺少 ABI 或合约地址，均无法发起任何合约交互**；
    
4.  **DApp 交互核心差异**：Read 函数对应链上数据查询，免费、无记录；Write 函数对应链上状态修改，消耗 Gas、永久留痕；链下程序无法直接访问区块链，必须通过 Alchemy/Infura/ 公共 RPC 节点中转交互请求；
    
5.  **安全不可妥协原则**：合约代码上链前，必须完成三重校验；遵循最小权限原则，入门合约优先部署 Remix 本地 VM，测试完整逻辑后，再部署测试网，绝对不要将未经过专业审计的合约部署到以太坊主网；任何链上操作，优先通过 txHash 验证执行结果。
    

### 4.3 入门实践流程

1.  严格遵循测试流程：先在 Remix 本地 VM 验证合约逻辑，再切换到 Monad/Sepolia 测试网进行正式交互验证，确认所有功能、Gas 消耗、事件日志正常后，再考虑主网部署；
    
2.  建立标准化校验清单：将「AI 生成→编译校验→人工安全审查→静态扫描→本地测试→测试网验证」作为合约上线的强制步骤，逐一确认无遗漏；
    
3.  妥善保存核心文件：合约 ABI、合约地址、部署 / 交互 txHash 必须归档保存，丢失 ABI 或合约地址，将无法识别、调用合约；
    
4.  利用区块浏览器调试：开发过程中，通过 txHash 在区块浏览器中查看事件日志、合约执行报错，快速定位逻辑问题，优化合约 Gas 消耗；
    
5.  结合参考文档拓展学习：通过 Solidity 官方文档深化安全语法、ABI 规范等底层知识；通过 Monad 官方文档学习 EVM 兼容链的特殊部署逻辑；通过 Build Anything 课程配套前端项目，实现完整 DApp 的前后端联调，从单合约开发延伸到全栈 Web3 应用开发。
<!-- DAILY_CHECKIN_2026-07-09_END -->

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
