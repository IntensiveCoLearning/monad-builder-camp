---
timezone: UTC+8
---

# WendyYuHuan

**GitHub ID:** WendyYuHuan

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
# 学习记录

学习 Solidity 智能合约开发。[https://docs.soliditylang.org/](https://docs.soliditylang.org/)
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->

# 学习记录

1.Monad 开发入门与部署指南。[https://docs.monad.xyz/](https://docs.monad.xyz/)
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->


# 学习记录

1.学习笔记分享例会及co-learning

学习总结分享网站：[https://kokaro233.github.io/vibe-coding-levels/](https://kokaro233.github.io/vibe-coding-levels/)
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->



# 学习记录

1.参加Co-leaning

2.了解 Ethereum、EVM 与 Web3 基础知识
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->




# 学习记录

1.了解 Web3 运营的完整工作流

2.co-learning：智能合约常见事故案例
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->





# 学习记录

1.WEB3职业方向：ops、dev、research的职责和职业发展

2.WEB3简历构建方向：参与社媒讨论、输出观点，构建影响力；PR&issue等开源贡献；黑客松积累作品；协议层研究

3.规范提交MOSS PR Skills：[https://github.com/mattpocock/skills](https://github.com/mattpocock/skills)

4.补充Monad Demo Idea方案文档链接：[https://github.com/WendyYuHuan/Ducoments/blob/main/Monad\_Pet\_Adventure\_%E6%96%B9%E6%A1%88%E6%96%87%E6%A1%A3.md](https://github.com/WendyYuHuan/Ducoments/blob/main/Monad_Pet_Adventure_%E6%96%B9%E6%A1%88%E6%96%87%E6%A1%A3.md)
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->






# 学习记录

1.Monad高频交互Demo Idea：

```markdown
轻量级链上宠物养成、冒险探索和装扮交易游戏
```

2.AI如何0-1完成WEB3应用开发分享及Co-learning
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->







# 学习记录

Chatgpt生成NFT Badge合约并部署

```
// SPDX-License-Identifier: MIT
```

```
pragma solidity ^0.8.20;
```

```
/**
```

```
 * Minimal NFT Badge
```

```
 * No external libraries
```

```
 */
```

```
contract NFTBadge {
```

```
    string public name = "Minimal Badge";
```

```
    string public symbol = "BADGE";
```

```
    address public owner;
```

```
    uint256 private nextTokenId = 1;
```

```
    // tokenId => owner
```

```
    mapping(uint256 => address) private owners;
```

```
    // owner => balance
```

```
    mapping(address => uint256) private balances;
```

```
    // tokenId => metadata URI
```

```
    mapping(uint256 => string) private tokenURIs;
```

```
    event Transfer(
```

```
        address indexed from,
```

```
        address indexed to,
```

```
        uint256 indexed tokenId
```

```
    );
```

```
    modifier onlyOwner() {
```

```
        require(msg.sender == owner, "Not contract owner");
```

```
        _;
```

```
    }
```

```
    constructor() {
```

```
        owner = msg.sender;
```

```
    }
```

```
    /**
```

```
     * Mint a badge NFT
```

```
     */
```

```
    function mint(
```

```
        address to,
```

```
        string memory uri
```

```
    )
```

```
        external
```

```
        onlyOwner
```

```
        returns(uint256)
```

```
    {
```

```
        require(to != address(0), "Invalid address");
```

```
        uint256 tokenId = nextTokenId;
```

```
        nextTokenId++;
```

```
        owners[tokenId] = to;
```

```
        balances[to]++;
```

```
        tokenURIs[tokenId] = uri;
```

```
        emit Transfer(
```

```
            address(0),
```

```
            to,
```

```
            tokenId
```

```
        );
```

```
        return tokenId;
```

```
    }
```

```
    /**
```

```
     * Transfer badge
```

```
     */
```

```
    function transfer(
```

```
        address to,
```

```
        uint256 tokenId
```

```
    )
```

```
        external
```

```
    {
```

```
        require(
```

```
            owners[tokenId] == msg.sender,
```

```
            "Not token owner"
```

```
        );
```

```
        require(
```

```
            to != address(0),
```

```
            "Invalid address"
```

```
        );
```

```
        owners[tokenId] = to;
```

```
        balances[msg.sender]--;
```

```
        balances[to]++;
```

```
        emit Transfer(
```

```
            msg.sender,
```

```
            to,
```

```
            tokenId
```

```
        );
```

```
    }
```

```
    /**
```

```
     * Query NFT owner
```

```
     */
```

```
    function ownerOf(
```

```
        uint256 tokenId
```

```
    )
```

```
        external
```

```
        view
```

```
        returns(address)
```

```
    {
```

```
        require(
```

```
            owners[tokenId] != address(0),
```

```
            "Token does not exist"
```

```
        );
```

```
        return owners[tokenId];
```

```
    }
```

```
    /**
```

```
     * Query balance
```

```
     */
```

```
    function balanceOf(
```

```
        address user
```

```
    )
```

```
        external
```

```
        view
```

```
        returns(uint256)
```

```
    {
```

```
        return balances[user];
```

```
    }
```

```
    /**
```

```
     * Metadata URI
```

```
     */
```

```
    function tokenURI(
```

```
        uint256 tokenId
```

```
    )
```

```
        external
```

```
        view
```

```
        returns(string memory)
```

```
    {
```

```
        require(
```

```
            owners[tokenId] != address(0),
```

```
            "Token does not exist"
```

```
        );
```

```
        return tokenURIs[tokenId];
```

```
    }
```

```
    /**
```

```
     * Transfer contract ownership
```

```
     */
```

```
    function transferContractOwnership(
```

```
        address newOwner
```

```
    )
```

```
        external
```

```
        onlyOwner
```

```
    {
```

```
        require(
```

```
            newOwner != address(0),
```

```
            "Invalid owner"
```

```
        );
```

```
        owner = newOwner;
```

```
    }
```

```
}
```
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->








# 学习记录

1.Web3实习手册文档阅读

2.BuildAnything文档阅读（Vibecoding 入门）

# 明日计划

AI生成NFT Badge合约并部署，完成Week1课程任务
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->









# 学习记录

1.WEEK1例会

2.学习Web3实习手册

# 明日计划

学习Web3实习手册
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->










# 学习记录

1.AI高危案例分享会及Co-learning

2.学习Solidity基础语法

# 明日计划

学习Web3实习手册
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->











# 学习记录

1.  参加AI Agent分享会及co-learing
    
2.  学习Solidity基础语法
    
3.  尝试Codex自动部署合约
    

# 明日计划

学习Solidity基础语法
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->












# 学习记录

1.  《普通开发者如何进入以太坊协议层》分享会及co-learning（Codex演示自动部署智能合约）
    
2.  学习Remix IDE的使用
    
3.  完成第一笔链上交易、部署solidity合约
    

# 明日计划

Codex部署合约实操
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->













# 学习记录

1.  Web3实习手册文档阅读（区块链基础概念、以太坊概览）
    
2.  DevRel分享会
    
3.  Co-learning学习及实操
    

创建MetaMask钱包-连接Monad test网络-Monad Faucet领水-区块链浏览器查询钱包

钱包地址：0xF6CB170E122c467cE590C6292dC318a4d68Ff531

# 明日计划

完成链上测试网交易、部署solidity合约
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
