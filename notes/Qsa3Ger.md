---
timezone: UTC+8
---

# Qsa3Ger

**GitHub ID:** Qsa3Ger

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
Day 1+2 学习总结 — Qsa3Ger

GitHub: Qsa3Ger

钱包地址: 0x630120B0Be8C204987B36c84677E08322EDBD402

Build Log 文件: notes/[Qsa3Ger.md](http://Qsa3Ger.md) → PR #18 ([https://github.com/IntensiveCoLearning/monad-builder-camp/pull/18](https://github.com/IntensiveCoLearning/monad-builder-camp/pull/18))

\---

7月6日（Day 1）— Monad 测试网配置 + 第一笔链上交易

① 网络配置

\- 确认 MetaMask 已有 Monad Testnet

\- 网络参数：Chain ID 10143 / RPC [https://testnet-rpc.monad.xyz](https://testnet-rpc.monad.xyz) / Explorer [testnet.monadvision.com](http://testnet.monadvision.com)

\- 通过水龙头 [faucet.monad.xyz](http://faucet.monad.xyz) 领取 5 MON 测试币

② 第一笔链上交易 🎯

Tx hash: 0x07640fdd90e1b2b963f49544f681c496054b93d5256658f3be36a97b79ebffdd

查看详情 ([https://testnet.monadvision.com/tx/0x07640fdd90e1b2b963f49544f681c496054b93d5256658f3be36a97b79ebffdd](https://testnet.monadvision.com/tx/0x07640fdd90e1b2b963f49544f681c496054b93d5256658f3be36a97b79ebffdd))

From

• 字段: From

• 值: 0x6301...d402

To

• 字段: To

• 值: 0x4870...119a（自己的另一个地址）

Value

• 字段: Value

• 值: 0.01 MON

Gas Used

• 字段: Gas Used

• 值: 21,000（标准转账）

Gas Price

• 字段: Gas Price

• 值: ~102.91 Gwei

手续费

• 字段: 手续费

• 值: 0.00216111 MON

Nonce

• 字段: Nonce

• 值: 0（该地址在 Monad 测试网的首笔交易 ✅）

Block

• 字段: Block

• 值: #42,830,459

时间

• 字段: 时间

• 值: 2026-07-06 17:27 UTC

③ 理解了 Monad 测试网 vs 主网的区别： 测试网币无真实价值，仅用于练习

\---

7月7日（Day 2）— 概念深度理解 + Build Log 提交

① 区块链核心概念复述（用自己的话）

\- Q1：区块链解决了中心化监管，区块=定时打包的交易+哈希指纹，链=按哈希串联的不可逆序列 ✅

\- Q2：比特币是 Web3 1.0（货币属性），以太坊通过 Solidity 实现可编程交易（智能合约=自动执行的规则代码）✅

\- Q3：私钥→椭圆曲线加密→公钥→Keccak-256 哈希取后20字节→地址 ✅

② Gas 机制深挖

\- 掌握了 Gas = 计算量单位（无货币单位），Gas Limit 为上限未用完退还

\- 理解了 EIP-1559 的 Base Fee（网络自动调）+ Priority Fee（给小费加速）

\- 独立完成了 Gas 费计算：21,000 × 102.91 Gwei = 0.00216111 MON ✅

\- 理解了 Block Confirmations 的意义（防止重组）

③ AI Prompt 实验

\- Prompt：用比喻解释区块/链/哈希的篡改后果

\- 核心收获：区块=乐高积木，哈希=防伪指纹，篡改一个就要改后面全部 + 全网监督 → 区块链不可篡改的本质

④ 红绿灯自我评估 🚦

🟢 能教别人

• 级别: 🟢 能教别人

• 内容: 区块/链/哈希关系、Gas 计算、Monad 测试网操作

🟡 有盲区

• 级别: 🟡 有盲区

• 内容: Base Fee 动态调整的详细公式

🔴 不会

• 级别: 🔴 不会

• 内容: Solidity 合约编写

⑤ 一句话总结

通过亲手完成 Monad 测试网的第一笔链上交易，区块打包→哈希不可逆→Gas 计费这条链路真正打通了，区块链从抽象概念变成了可操作的现实。

⑥ Build Log 提交

\- 已完成 Build Log 本地编写（notes/[Qsa3Ger.md](http://Qsa3Ger.md)）

\- Fork 仓库到 github.com/Qsa3Ger/monad-builder-camp

\- 已提交 PR #18 到原仓库，等待管理员合并 ✅

\---

🎯 两天总进度一览

区块链基础概念理解

• 板块: 区块链基础概念理解

• 状态: ✅ 完成

Monad 测试网配置 + 领水

• 板块: Monad 测试网配置 + 领水

• 状态: ✅ 完成

第一笔链上交易（含 tx hash）

• 板块: 第一笔链上交易（含 tx hash）

• 状态: ✅ 完成

Gas 费计算 + EIP-1559 理解

• 板块: Gas 费计算 + EIP-1559 理解

• 状态: ✅ 完成

Block Confirmations 机制

• 板块: Block Confirmations 机制

• 状态: ✅ 掌握

AI Prompt 实验

• 板块: AI Prompt 实验

• 状态: ✅ 完成

Build Log 撰写 + PR 提交

• 板块: Build Log 撰写 + PR 提交

• 状态: ✅ PR #18

Solidity 合约编写

• 板块: Solidity 合约编写

• 状态: ⏳ 待开始（DAY3 优先级）
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
