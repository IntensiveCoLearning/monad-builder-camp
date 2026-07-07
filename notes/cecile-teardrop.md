---
timezone: UTC+8
---

# Cynthia

**GitHub ID:** cecile-teardrop

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 1.交易测试

测试网交易测试，测试成果[https://testnet.monadvision.com/tx/0xb286974b737b741f66218169a6f8ad1d5d7b7afcc6b56e04104982340bf267f8](https://testnet.monadvision.com/tx/0xb286974b737b741f66218169a6f8ad1d5d7b7afcc6b56e04104982340bf267f8)  
转账交易，transaction hash：0xb286974b737b741f66218169a6f8ad1d5d7b7afcc6b56e04104982340bf267f8  
（发起方地址：0x6A804B989AAfb5206BBDbE708CB31Ee60b7297Dd，接收方地址：0x49e1d8D3d083390DB5Ddb8f1f12E1BFE4Be72556）  

# 2.**用 AI 生成一个最小 Solidity 合约**

**我提交的Prompt**

【请帮我用 Solidity 写一个**最小化的投票合约**，满足以下要求：

1\. 合约部署时通过构造函数传入候选人名单（字符串数组）。

2\. 每个地址只能投一次票，使用 mapping 记录是否已投票。

3\. 提供 vote() 函数，传入候选人索引进行投票。

4\. 提供查询函数：获取候选人数量、获取单个候选人信息、获取当前得票最多者。

5\. 使用 Solidity ^0.8.0，代码尽量简洁，加中文注释。

请输出完整合约代码，并简要解释合约结构。】

最开始生成的代码：

```
pragma solidity ^0.8.0;

contract Voting {
    struct Candidate {
        string name;
        uint voteCount;
    }

    address public owner;
    Candidate[] public candidates;
    mapping(address => bool) public hasVoted;

    constructor(string[] memory _candidateNames) {
        owner = msg.sender;
        for (uint i = 0; i < _candidateNames.length; i++) {
            candidates.push(Candidate({name: _candidateNames[i], voteCount: 0}));
        }
    }

    function vote(uint _candidateIndex) public {
        require(!hasVoted[msg.sender], "Already voted");
        require(_candidateIndex < candidates.length, "Invalid index");
        hasVoted[msg.sender] = true;
        candidates[_candidateIndex].voteCount += 1;
    }

    function getCandidateCount() public view returns (uint) {
        return candidates.length;
    }

    function getCandidate(uint _index) public view returns (string memory name, uint voteCount) {
        return (candidates[_index].name, candidates[_index].voteCount);
    }

    function getWinner() public view returns (string memory name, uint voteCount) {
        uint winningVote = 0;
        uint winningIndex = 0;
        for (uint i = 0; i < candidates.length; i++) {
            if (candidates[i].voteCount > winningVote) {
                winningVote = candidates[i].voteCount;
                winningIndex = i;
            }
        }
        return (candidates[winningIndex].name, candidates[winningIndex].voteCount);
    }
}
```

修改意见：1：缺少 SPDX 许可证标识，2：类型声明 uint 与 uint256 不统一，3：缺少事件（Event），4：getCandidate 缺少数组越界检查

修改后的：

```

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract Voting {

    struct Candidate {
        string name;
        uint256 voteCount;
    }

    address public owner;
    Candidate[] public candidates;
    mapping(address => bool) public hasVoted;

    event Voted(address indexed voter, uint256 indexed candidateIndex);
    event CandidateAdded(string name, uint256 index);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this");
        ;
    }

    constructor(string[] memory candidateNames) {
        require(_candidateNames.length > 0, "Must have at least one candidate");
        owner = msg.sender;
        for (uint256 i = 0; i < candidateNames.length; i++) {
            candidates.push(Candidate({name: candidateNames[i], voteCount: 0}));
        }
    }

    function vote(uint256 candidateIndex) external {
        require(!hasVoted[msg.sender], "You have already voted");
        require(candidateIndex < candidates.length, "Invalid candidate index");
        hasVoted[msg.sender] = true;
        candidates[_candidateIndex].voteCount += 1;
        emit Voted(msg.sender, candidateIndex);
    }

    function addCandidate(string memory name) external onlyOwner {
        require(bytes(_name).length > 0, "Name cannot be empty");
        candidates.push(Candidate({name: name, voteCount: 0}));
        emit CandidateAdded(name, candidates.length - 1);
    }

    function getCandidateCount() external view returns (uint256) {
        return candidates.length;
    }

    function getCandidate(uint256 index) external view returns (string memory name, uint256 voteCount) {
        require(index < candidates.length, "Invalid index");
        Candidate storage c = candidates[_index];
        return (c.name, c.voteCount);
    }

    function getWinner() external view returns (string memory name, uint256 voteCount) {
        require(candidates.length > 0, "No candidates");
        uint256 winningVote = 0;
        uint256 winningIndex = 0;
        for (uint256 i = 0; i < candidates.length; i++) {
            if (candidates[i].voteCount > winningVote) {
                winningVote = candidates[i].voteCount;
                winningIndex = i;
            }
        }
        return (candidates[winningIndex].name, candidates[winningIndex].voteCount);
    }
}
```

AI 生成代码需要人工检查的地方：  
编译规范  
权限于访问风险  
潜在安全性能风险  
边界情况是否处理等等
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
