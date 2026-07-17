---
timezone: UTC+8
---

# BoweiTu

**GitHub ID:** BoweiTu

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->
学习DeFi基础模块，理解流动性池、AMM自动做市商的运行原理。对比不同协议的代币经济模型，分析激励机制设计逻辑。

很多经济模型设计环环相扣，还需要慢慢拆解，进一步研究实际项目落地案例。
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->

深入弄懂Monad的共识机制Monad‑BFT，区分了异步执行架构和传统公链的运行差异。研读官方文档，理清LayerZero、Hyperlane跨链方案的适用场景。浏览简单的合约源码，学习MCP‑API的调用逻辑，了解开发者配置RPC节点的方法，注重代码安全性，稳步夯实区块链相关开发认知。
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->


今日Web3学习打卡：系统了解Monad公链底层架构，读懂了MCP协议和传统API的区别，明白了Monad依托C++与Rust实现高性能异步执行。查阅官方RPC配置参数，学习跨链桥OFT‑CCIP相关原理，简单阅读Solidity基础代码。意识到合约开发安全至关重要，后续继续练习编写极简智能合约，夯实区块链开发基础。
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->



今天系统梳理了Monad公链相关基础内容，弄懂了L1底层链和通用API、MCP协议的层级区别：API是通用互联网调用接口，MCP是专门适配大模型交互的协议。

学习了链上跨链资产逻辑，不同代币通过LayerZero、CCIP、Hyperlane等桥部署到Monad，用户可通过原生桥前端或聚合跨链工具完成资产划转。

动手看懂最简Solidity链上待办合约结构，区分状态存储、读写函数，人工排查编译报错、权限控制、数据覆盖三类常见安全隐患，学会用AI生成合约初稿后人工校验修正。

顺带理清链上代币列表文件来源，看懂主网Chain ID、RPC节点、区块浏览器等开发者配置参数，分清限频公共RPC与商用高性能节点的使用场景。
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->




学习了智能合约相关内容 注册了Meta Mask并转给自己了第一笔资金
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->





学习了以太坊的技术演进 数据分片 Layer2等等还有**Pectra 与后续扩容**
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->






学习了Web3和Web3.0之间的关系相关知识 非常有趣！
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->







完整代币信息可查阅仓库原始 JSON。其他链原生资产通过 LayerZero OFT、CCIP、Hyperlane、2/2 NTT 桥接入 Monad，桥接方案由发行方定。 用户跨链有两种方式：一是对应官方桥前端（Stargate/Transporter/Nexus/MonadBridge），资产封装逻辑完整；二是 Jumper 聚合器，适合跨链兑换。 文档列出美元、ETH、BTC、其他品类桥接代币，附带合约地址、来源链与主流交易池；WMON 为 Monad 原生封装币。 可通过 GeckoTerminal、MonadVision、DeFiLlama 查看市场数据，跨链桥完整清单见跨链专栏，文末介绍 MON 在其他公链相关信息。
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->








今天我学习了Monad 主网链配置文档，包含链基础参数、RPC 节点、区块浏览器、标准合约地址、开发配套资源等等知识点 感到受益匪浅
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->









今天是我学习web3的第一天 主要学习了Monad公链 是什么 可以怎么应用

它是一条兼顾高性能、去中心化与EVM兼容的一层公链，目标打破去中心化与性能不可兼得的困境。它硬件门槛低，普通人即可运行节点，性能依靠C++与Rust自研的开源软件架构实现。其五大创新架构包括MonadBFT共识、RaptorCast区块传输、异步执行、并行执行+JIT编译、MonadDb状态存储，在兼容EVM字节码与以太坊RPC接口的前提下，实现约10000TPS、400ms出块、800ms最终确认。主网于2025年11月24日上线，原生兼容以太坊开发工具，对开发者友好。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
