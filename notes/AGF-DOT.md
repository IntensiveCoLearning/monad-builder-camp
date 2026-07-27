- GitHub ID: 240708285
- Name: AGF-DOT
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-26
<!-- DAILY_CHECKIN_2026-07-26_START -->
````markdown
预言机（Oracle）技术与安全实战笔记

日期：2026年7月26日
主题：预言机核心原理、Chainlink 2026最新特性、TWAP预言机实现、安全攻击分析
关联项目：OpenPerp永续DEX（清算触发依赖预言机价格）
核心目标：理解预言机技术栈，掌握安全设计模式，避免2026年典型攻击

---

第一部分：预言机核心原理

1.1 什么是预言机

预言机是连接链上智能合约与链下真实世界数据的中间件。区块链本身是封闭系统，只能处理链上数据，无法直接访问外部API、交易所价格、天气数据等。

核心架构：
```
链下数据源（交易所API、政府统计、IoT设备）
        |
        v
预言机网络（去中心化节点，如Chainlink DON）
        |
        v
链上预言机合约（存储数据，供智能合约读取）
        |
        v
消费合约（借贷、DEX、保险等）
```

1.2 预言机的三大作用

作用1：数据输入
- 价格数据（ETH/USDT、BTC/USD）
- 事件数据（体育比赛结果、选举结果）
- 环境数据（天气、温度）

作用2：数据输出
- 智能合约触发链下操作
- 自动支付、物流通知

作用3：跨链通信
- 一条链的合约状态同步到另一条链
- 资产跨链桥接

1.3 预言机分类

按驱动模式：
| 类型 | 原理 | 优点 | 缺点 | 代表 |
|-----|------|------|------|------|
| Push型 | 预言机主动推送数据到链上合约 | 实时性高，适合快速更新 | 需要监控更新频率 | Chainlink Data Feeds |
| Pull型 | 消费合约主动请求，预言机响应 | 按需计费，节省gas | 有网络延迟 | Chainlink Data Streams |
| 混合型 | 结合Push和Pull | 灵活 | 复杂 | Chainlink Functions |

按数据源：
| 类型 | 原理 | 优点 | 缺点 |
|-----|------|------|------|
| 第三方API | 调用中心化服务API | 数据权威 | 依赖单一来源 |
| DEX TWAP | 从链上AMM计算时间加权均价 | 去中心化，抗操纵 | 依赖DEX流动性 |
| 聚合型 | 多个数据源取中位数 | 容错强 | 实现复杂 |

1.4 预言机与DeFi的关系

借贷协议（Aave/Compound）：
- 读取抵押品价格，计算健康因子
- 价格下跌触发清算
- 预言机故障导致坏账或错误清算

DEX（Uniswap）：
- V3 Concentrated Liquidity依赖价格预言机
- TWAP用于滑点计算和价格区间设置

衍生品（OpenPerp）：
- 永续合约标记价格计算
- 清算触发（当标记价格低于维持保证金时）
- 资金费率计算

稳定币（MakerDAO）：
- ETH价格预言机用于CDP清算
- 预言机延迟导致CDP集体清算（2020年3月）

---

第二部分：Chainlink 2026最新特性

2.1 Chainlink核心架构

Chainlink采用去中心化预言机网络（DON, Decentralized Oracle Network），由多个独立节点组成，聚合数据后以共识方式写入链上。

组件拆解：
```
外部适配器（External Adapter）
    | 处理特定数据源（交易所API等）
    v
链下节点（Chainlink Node）
    | 每个节点独立获取数据
    v
共识聚合（Aggregation）
    | 取中位数/众数，去除异常值
    v
链上合约（Oracle / Access Control）
    | 验证节点签名，更新数据
    v
消费合约
```

2.2 Chainlink Data Feeds（Push型预言机）

Data Feeds是Chainlink最成熟的产品，Aave、Compound等头部协议都在使用。

工作原理：
1. DON定期从多个交易所API获取价格（Binance、Coinbase、Kraken等）
2. 每个节点独立提交价格报告
3. 链上合约验证节点签名，取中位数
4. 当价格偏离超过阈值或时间到期时更新链上数据

关键参数：
```solidity
// Chainlink AggregatorV3Interface核心函数
interface AggregatorV3Interface {
    function decimals() external view returns (uint8);
    function description() external view returns (string memory);
    function version() external view returns (uint256);
    
    // 获取最新价格
    function latestRoundData() external view returns (
        uint80 roundId,      // 轮次ID
        int256 answer,       // 价格（带decimals）
        uint256 startedAt,   // 轮次开始时间
        uint256 updatedAt,  // 最后更新时间
        uint80 answeredInRound
    );
    
    // 获取历史价格
    function getRoundData(uint80 _roundId) external view returns (...);
}
```

安全检查模式（OpenPerp必用）：
```solidity
contract PriceConsumer {
    AggregatorV3Interface internal priceFeed;
    
    constructor(address _priceFeed) {
        priceFeed = AggregatorV3Interface(_priceFeed);
    }
    
    function getVerifiedPrice() external view returns (int256 price, bool isHealthy) {
        (uint80 roundId, int256 answer, , uint256 updatedAt, uint80 answeredInRound) = 
            priceFeed.latestRoundData();
        
        // 安全检查1：价格非负
        if (answer <= 0) return (0, false);
        
        // 安全检查2：预言机未过期（超过1小时视为异常）
        if (block.timestamp - updatedAt > 1 hours) return (answer, false);
        
        // 安全检查3：检查轮次连续性（防止跳轮攻击）
        if (answeredInRound < roundId) return (answer, false);
        
        // 安全检查4：与历史价格对比（防止操纵）
        (, int256 prevPrice, , , ) = priceFeed.getRoundData(roundId - 1);
        if (prevPrice > 0) {
            uint256 change = abs(answer - prevPrice);
            uint256 threshold = prevPrice / 20; // 5%阈值
            if (change > threshold) return (answer, false); // 价格异动
        }
        
        return (answer, true);
    }
    
    function abs(int256 x) internal pure returns (uint256) {
        return x >= 0 ? uint256(x) : uint256(-x);
    }
}
```

2.3 Chainlink Data Streams（Pull型预言机）

Data Streams是Chainlink 2025-2026推出的高频率预言机，面向衍生品市场。

与Data Feeds对比：
| 特性 | Data Feeds | Data Streams |
|-----|------------|--------------|
| 更新频率 | 分钟级（通常20-30分钟） | 毫秒级（最高10ms） |
| 驱动模式 | Push（预言机主动推送） | Pull（消费方主动请求） |
| 延迟 | 较高 | 极低 |
| 适用场景 | 借贷协议、稳定币 | 高频衍生品、TWAP |
| 2026新功能 | - | 支持24/7股市数据 |

2026年重要合作：
- Polymarket集成Data Streams，推出5分钟/15分钟涨跌预测市场，交易额超50亿美元
- 24/7美股ETF数据流上线，Lighter、BitMEX等衍生品平台采用
- Aave V4集成Data Streams支持高频借贷

2.4 Chainlink CCIP（跨链互操作）

CCIP是Chainlink跨链通信协议，已成为DeFi跨链标准（LayerZero被黑客攻击后，大量项目迁移到CCIP）。

CCIP安全架构：
- 16个独立节点验证每条跨链消息（LayerZero仅1-2个节点）
- 独立风险管理网络进行额外验证
- 支持速率限制、熔断、权限控制

2026年CCIP发展：
- 超过40亿美元TVL迁移到CCIP
- 美国商务部宏观经济数据（GDP、失业率）通过CCIP上链
- SolvV2（Chainlink解决预言机可验证性的方案）测试网

---

第三部分：2026年典型预言机攻击案例

3.1 Bonzo Lend（Hedera，损失900万美元）

攻击时间：2026年7月11日
攻击类型：预言机签名验证缺陷
根本原因：零签名通过BLS验证

攻击原理：
```
Bonzo Lend使用Supra预言机，Supra采用BLS签名验证
攻击者提交全零签名的虚假价格
Supra验证器未检查签名是否为零
在BN254曲线上，零签名的配对验证恒成立
虚假价格（SAUCE币价格被抬高数千倍）上链
攻击者用250枚SAUCE（市价几美元）借出663万USDC + 3450万HBAR
```

伪代码还原漏洞：
```solidity
// Supra验证器的漏洞实现
function verify(bytes calldata proof, bytes calldata msg) internal view returns (bool) {
    // 缺少：if (proof.length == 0 || isZero(proof)) return false;
    
    // BN254配对验证
    bool valid = pairingCheck(proof, msg);
    return valid; // 当proof=0时，配对恒成立
}
```

修复方案：
```solidity
// 安全的验证实现
function verify(bytes calldata proof, bytes calldata msg) internal view returns (bool) {
    // 关键修复：零签名检查
    if (proof.length == 0) return false;
    if (isZero(proof)) return false;
    
    bool valid = pairingCheck(proof, msg);
    return valid;
}

function isZero(bytes calldata data) internal pure returns (bool) {
    for (uint i = 0; i < data.length; i++) {
        if (data[i] != 0) return false;
    }
    return true;
}
```

OpenPerp启示：所有签名验证必须显式拒绝零值，特别是BLS/BN254等曲线密码学

3.2 Ostium（Arbitrum，损失1800万美元）

攻击时间：2026年7月15日
攻击类型：预言机私钥泄露
根本原因：预言机签名节点私钥被窃取

攻击原理：
```
Ostium使用Stork Network预言机
攻击者获取预言机签名者私钥
利用已注册的PriceUpkeep转发器提交伪造价格
先低价开仓（BTC空单），再高价平仓（触发900%利润上限）
循环操作20次，从金库提取约1186万USDC
```

攻击流程：
```
Step 1: 获取私钥 -> 构造有效签名
Step 2: 提交伪造的价格报告（BTC从6万跌到5万）
Step 3: 用100 USDC开BTC空单（标记价格5万）
Step 4: 提交第二份伪造报告（BTC从5万涨到10万）
Step 5: 标记价格10万，触发900%利润（100 USDC变1000 USDC）
Step 6: 重复20次，从金库提走1186万USDC
```

防御方案：
- 预言机节点密钥采用HSM（硬件安全模块）存储
- 节点运行环境隔离（air-gapped）
- 价格变动超过阈值（如20%）触发链上熔断
- 签名验证增加时间戳检查（拒绝过期签名）
- 预言机节点地理分布（防止单点故障）

3.3 Lien Finance（Ethereum，损失54万美元）

攻击时间：2026年7月24日
攻击类型：预言机操纵
根本原因：任何人可以注册新的债券组并设置虚假价格

攻击原理：
```
Lien Finance的GeneralizedDotc池允许任何人注册新的债券组
攻击者注册了虚假债券组，设置虚高的债券价格
结合预言机操纵，铸造超额衍生代币
从流动性池提取54万USDC
```

OpenPerp启示：限制预言机白名单，只允许已验证的价格源

3.4 2026年预言机攻击总结

| 项目 | 时间 | 损失 | 攻击类型 | 根因 |
|-----|------|------|---------|------|
| Bonzo Lend | 2026-07 | 900万 | 零签名验证绕过 | BLS曲线特性未考虑 |
| Ostium | 2026-07 | 1800万 | 私钥泄露 | HSM未启用 |
| Lien Finance | 2026-07 | 54万 | 预言机操纵 | 白名单缺失 |
| LayerZero | 2026-05 | 3亿 | 跨链桥漏洞 | 节点过于中心化 |

2026年预言机攻击总损失：约6.3亿美元

---

第四部分：Uniswap V3 TWAP预言机实现

4.1 TWAP原理

TWAP（Time-Weighted Average Price）是在指定时间窗口内的平均价格。Uniswap V3通过价格累积值（Cumulative Price）实现TWAP计算。

数学原理：
```
TWAP(t1, t2) = (PriceCumulative(t2) - PriceCumulative(t1)) / (t2 - t1)

其中 PriceCumulative(t) = 从创世到t时刻的价格积分
```

4.2 Uniswap V3 Oracle合约

```solidity
// Uniswap V3 Oracle库
library Oracle {
    // 获取指定时间点的累积价格
    function getSqrtPriceX96AtTime(
        address pool,
        uint32 secondsAgo
    ) internal view returns (uint160 sqrtPriceX96) {
        // 从Uniswap V3 Pool合约读取
        (, sqrtPriceX96, , , , , ) = IUniswapV3Pool(pool).slot0();
        
        // 如果secondsAgo=0，返回当前价格
        if (secondsAgo == 0) return sqrtPriceX96;
        
        // 否则从observation查询历史累积价格
        uint32[] memory secondsAgos = new uint32[](2);
        secondsAgos[0] = secondsAgo;
        secondsAgos[1] = 0;
        
        (int56[] memory tickCumulatives, uint160[] memory secondsPerLiquidityCumulativeX128s) = 
            IUniswapV3Pool(pool).observe(secondsAgos);
        
        // 转换为sqrtPriceX96
        int56 tickCumulativeDelta = tickCumulatives[1] - tickCumulatives[0];
        int24 tick = int24(tickCumulativeDelta / int56(uint56(secondsAgo)));
        uint160 sqrtPrice = TickMath.getSqrtRatioAtTick(tick);
        
        return sqrtPrice;
    }
    
    // 计算TWAP价格
    function getTWAP(
        address pool,
        uint32 secondsAgo
    ) internal view returns (uint256 price) {
        // 点1：secondsAgo前的累积价格
        uint32[] memory secondsAgos = new uint32[](2);
        secondsAgos[0] = secondsAgo;
        secondsAgos[1] = 0;
        
        (int56[] memory tickCumulatives, ) = IUniswapV3Pool(pool).observe(secondsAgos);
        
        // TWAP tick
        int56 tickCumulativeDelta = tickCumulatives[1] - tickCumulatives[0];
        int24 avgTick = int24(tickCumulativeDelta / int56(uint56(secondsAgo)));
        
        // 转换为价格
        uint160 sqrtPriceX96 = TickMath.getSqrtRatioAtTick(avgTick);
        price = uint256(sqrtPriceX96) * uint256(sqrtPriceX96) >> (96 * 2);
        
        return price;
    }
}
```

4.3 Uniswap V4 TWAP Hook

OpenPerp可以在Uniswap V4 Pool上部署自定义Hook，实现TWAP预言机：

```solidity
// OpenPerp的TWAP Oracle Hook
contract TWAPOracleHook is BaseHook {
    struct TWAPState {
        uint256 price;        // 当前TWAP
        uint256 lastUpdate;   // 最后更新时间
        uint256 windowSize;  // TWAP窗口大小（如30分钟）
        int56 tickCumulative; // 累积tick值
        uint32 observationTime; // 观测时间
    }
    
    mapping(address => TWAPState) public twapStates;
    
    constructor(IPoolManager _poolManager) BaseHook(_poolManager) {}
    
    // 在swap后更新累积tick
    function afterSwap(
        address sender,
        PoolKey calldata key,
        IPoolManager.SwapParams calldata params,
        BalanceDelta delta
    ) external override onlyPoolManager {
        TWAPState storage state = twapStates[poolManager.getPool(key)];
        
        // 从V4的PoolState获取累积tick
        (, int24 currentTick, , , , , ) = poolManager.getSlot0(key);
        uint32 currentTime = uint32(block.timestamp);
        
        // 更新累积tick
        uint32 timeDelta = currentTime - state.observationTime;
        state.tickCumulative += int56(int256(currentTick)) * int56(uint56(timeDelta));
        state.observationTime = currentTime;
        
        // 计算TWAP
        if (currentTime - state.lastUpdate >= state.windowSize) {
            int24 avgTick = int24(state.tickCumulative / int56(uint56(state.windowSize)));
            uint160 sqrtPriceX96 = TickMath.getSqrtRatioAtTick(avgTick);
            state.price = uint256(sqrtPriceX96) * uint256(sqrtPriceX96) >> 192;
            state.lastUpdate = currentTime;
            
            // 触发价格更新事件
            emit TWAPUpdated(poolManager.getPool(key), state.price, currentTime);
        }
    }
    
    // OpenPerp清算合约调用此函数获取价格
    function getPrice(address pool) external view returns (uint256, bool) {
        TWAPState memory state = twapStates[pool];
        
        // 健康检查
        if (state.price == 0) return (0, false);
        if (block.timestamp - state.lastUpdate > 1 hours) return (state.price, false);
        
        return (state.price, true);
    }
}
```

4.4 TWAP vs Chainlink对比

| 特性 | TWAP | Chainlink |
|-----|------|-----------|
| 数据源 | DEX链上流动性 | 多交易所API |
| 去中心化 | 完全去中心化 | 半去中心化（节点运营） |
| 抗操纵性 | 窗口越大越强 | 强（多源聚合） |
| 更新频率 | 可配置（分钟级） | 分钟级（Data Feeds）/毫秒级（Data Streams） |
| 适用场景 | 长尾代币、DeFi原生资产 | 主流资产、跨链数据 |
| 实现复杂度 | 中等 | 低（直接调用） |
| 2026价格 | ETH/USDC V3 TWAP: $2100-2500 | ETH/USD: $2150-2480 |

OpenPerp建议：主流资产用Chainlink，长尾资产用TWAP，组合使用增加安全性

---

第五部分：OpenPerp预言机安全架构

5.1 安全设计原则

原则1：多源验证
- 主价格源：Chainlink Data Streams（高频）
- 备用价格源：Uniswap V3 TWAP（去中心化）
- 最终价格：两个源取中位数，差异超过2%触发熔断

原则2：时间窗口验证
- 清算价格使用5分钟TWAP
- 避免单次价格操纵触发错误清算
- Chainlink价格必须在30分钟内更新

原则3：异常检测
- 价格变动超过10%/分钟触发告警
- 预言机节点离线超过10分钟触发熔断
- 累积tick跳变超过阈值触发告警

原则4：权限最小化
- 清算合约只读调用预言机（STATICCALL）
- 预言机白名单可升级（需要DAO投票）
- 紧急熔断由多签钱包控制（3/5签名）

5.2 OpenPerp预言机合约架构

```solidity
// OpenPerp Price Oracle - 主合约
contract OpenPerpOracle {
    address public chainlinkFeed;    // Chainlink预言机地址
    address public uniswapPool;      // Uniswap V3池地址
    address public owner;            // 管理员
    uint256 public maxDeviation;     // 最大价格偏差（2%）
    uint256 public twapWindow;       // TWAP窗口（30分钟）
    
    // 状态变量
    enum PriceStatus { HEALTHY, STALE, MANIPULATED, FUSED }
    event PriceUpdated(uint256 price, PriceStatus status, uint256 timestamp);
    event EmergencyStop(address operator, string reason);
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }
    
    constructor(
        address _chainlinkFeed,
        address _uniswapPool,
        uint256 _maxDeviation,
        uint256 _twapWindow
    ) {
        chainlinkFeed = _chainlinkFeed;
        uniswapPool = _uniswapPool;
        maxDeviation = _maxDeviation;
        twapWindow = _twapWindow;
        owner = msg.sender;
    }
    
    // 获取主价格（供清算合约调用）
    function getMarkPrice() external view returns (uint256 price, PriceStatus status) {
        // 1. 从Chainlink获取价格
        (uint256 chainlinkPrice, bool chainlinkHealthy) = _getChainlinkPrice();
        
        // 2. 从Uniswap V3获取TWAP
        (uint256 twapPrice, bool twapHealthy) = _getTWAPPrice();
        
        // 3. 多源验证
        if (chainlinkHealthy && twapHealthy) {
            uint256 deviation = _calcDeviation(chainlinkPrice, twapPrice);
            if (deviation <= maxDeviation) {
                // 两个源一致，取中位数
                price = (chainlinkPrice + twapPrice) / 2;
                status = PriceStatus.HEALTHY;
                return (price, status);
            } else {
                // 偏差过大，标记为操纵
                status = PriceStatus.MANIPULATED;
                return (chainlinkPrice, status);
            }
        } else if (chainlinkHealthy) {
            return (chainlinkPrice, PriceStatus.STALE);
        } else if (twapHealthy) {
            return (twapPrice, PriceStatus.STALE);
        } else {
            return (0, PriceStatus.FUSED); // 完全熔断
        }
    }
    
    function _getChainlinkPrice() internal view returns (uint256, bool) {
        AggregatorV3Interface feed = AggregatorV3Interface(chainlinkFeed);
        (uint80 roundId, int256 answer, , uint256 updatedAt, uint80 answeredInRound) = 
            feed.latestRoundData();
        
        // 安全检查
        if (answer <= 0) return (0, false);
        if (block.timestamp - updatedAt > 30 minutes) return (uint256(answer), false);
        if (answeredInRound < roundId) return (uint256(answer), false);
        
        return (uint256(answer), true);
    }
    
    function _getTWAPPrice() internal view returns (uint256, bool) {
        IUniswapV3Pool pool = IUniswapV3Pool(uniswapPool);
        
        uint32[] memory secondsAgos = new uint32[](2);
        secondsAgos[0] = uint32(twapWindow);
        secondsAgos[1] = 0;
        
        try pool.observe(secondsAgos) returns (int56[] memory tickCumulatives, uint160[] memory) {
            int56 delta = tickCumulatives[1] - tickCumulatives[0];
            int24 avgTick = int24(delta / int56(uint56(twapWindow)));
            uint160 sqrtPriceX96 = TickMath.getSqrtRatioAtTick(avgTick);
            uint256 price = uint256(sqrtPriceX96) * uint256(sqrtPriceX96) >> 192;
            
            if (price == 0) return (0, false);
            return (price, true);
        } catch {
            return (0, false);
        }
    }
    
    function _calcDeviation(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0 || b == 0) return 100;
        if (a > b) return ((a - b) * 10000) / b;
        return ((b - a) * 10000) / a;
    }
    
    // 管理函数
    function setDeviation(uint256 _maxDeviation) external onlyOwner {
        maxDeviation = _maxDeviation;
    }
    
    function setTWAPWindow(uint256 _twapWindow) external onlyOwner {
        twapWindow = _twapWindow;
    }
    
    function emergencyStop() external onlyOwner {
        emit EmergencyStop(msg.sender, "Manual emergency stop");
    }
}
```

5.3 清算价格安全检查

```solidity
// OpenPerp清算合约的价格验证逻辑
contract Liquidation {
    OpenPerpOracle public oracle;
    
    function liquidate(address user) external {
        // 获取价格并验证状态
        (uint256 markPrice, OpenPerpOracle.PriceStatus status) = oracle.getMarkPrice();
        
        // 清算安全检查
        require(status == OpenPerpOracle.PriceStatus.HEALTHY, 
            "Oracle unhealthy, cannot liquidate");
        
        require(markPrice > 0, "Invalid price");
        
        // 使用价格计算健康因子
        uint256 healthFactor = calculateHealthFactor(user, markPrice);
        require(healthFactor < 1e18, "User not liquidatable");
        
        // 执行清算...
    }
}
```

---

第六部分：今日学习总结

6.1 预言机核心认知

1. 预言机是DeFi的"眼睛"，价格操纵是DeFi最大的安全威胁之一
2. 单一数据源预言机极易被攻击，必须多源验证
3. TWAP抗操纵性与时间窗口正相关，但实时性与窗口负相关，需要平衡
4. 签名验证必须考虑边界情况（零签名、过期签名等）

6.2 2026年攻击教训

| 攻击 | 教训 | OpenPerp对策 |
|-----|------|-------------|
| Bonzo Lend（零签名） | BLS验证必须拒绝零值 | 签名验证加零值检查 |
| Ostium（私钥泄露） | 预言机密钥必须用HSM | 节点密钥air-gap存储 |
| Lien Finance（白名单） | 限制可信价格源 | Chainlink+TWAP双源验证 |
| LayerZero（中心化） | 跨链必须多节点 | 采用CCIP（16节点验证） |

6.3 Chainlink 2026趋势

1. Data Streams：毫秒级更新，面向高频衍生品
2. CCIP：成为跨链事实标准，LayerZero衰落
3. SolvV2：解决预言机可验证性，支持链上挑战
4. RWA集成：大量机构数据通过Chainlink上链


````
<!-- DAILY_CHECKIN_2026-07-26_END -->

# 2026-07-25
<!-- DAILY_CHECKIN_2026-07-25_START -->

````markdown
EVM底层原理与前沿技术笔记

日期：2026年7月25日
主题：EVM执行模型、存储架构、2026最新特性（Monad并行EVM、EIP-1153瞬态存储）
关联项目：OpenPerp永续DEX
核心目标：从底层理解EVM运行机制，掌握2026最新优化方向，解决实际开发中的性能问题

---

第一部分：EVM基础执行模型

1.1 EVM是什么

EVM（Ethereum Virtual Machine）是以太坊的运行时环境，负责执行智能合约的字节码。它是一个栈式虚拟机，所有计算都基于256位（32字节）的字长。

核心特性：
- 确定性执行：相同输入必然产生相同输出
- 隔离环境：每个合约在独立沙箱中运行
- Gas计量：每个操作消耗计算资源，防止恶意合约耗尽节点资源
- 图灵完备：支持循环、条件分支等所有计算逻辑

1.2 EVM架构分层

```
用户层
    |
    v
JSON-RPC接口
    |
    v
交易池（Mempool）
    |
    v
共识层（PoS）
    |
    v
执行层（EVM）
    |
    v
状态存储（LevelDB/RocksDB）
```

1.3 EVM三大存储区域

存储层级对比表：

| 存储类型 | 生命周期 | 作用域 | Gas成本 | 典型用途 |
|---------|---------|--------|---------|---------|
| Stack | 单次调用 | 仅当前调用 | 1-3 gas | 临时计算，最多1024层 |
| Memory | 单次调用 | 可跨内部调用 | 3 gas/字（扩展时线性+二次增长） | 中间变量、返回数据 |
| Storage | 永久 | 合约全生命周期 | SLOAD: 100-2100 gas, SSTORE: 2900-20000 gas | 合约状态、余额、配置 |

关键字长：EVM所有操作基于256位（32字节）的word。使用uint8/uint128等小类型不会节省计算Gas，但多个小类型可以打包存储（Storage Packing）节省Storage槽位。

1.4 四类合约调用对比

```solidity
// 1. CALL: 普通调用，可修改目标合约状态
(bool success, bytes memory data) = target.call{value: 1 ether}(abi.encodeWithSignature("transfer(address,uint256)", user, amount));

// 2. DELEGATECALL: 委托调用，目标合约代码在调用者上下文执行，可修改调用者状态
(bool success, bytes memory data) = target.delegatecall(abi.encodeWithSignature("implement()"));

// 3. STATICCALL: 静态调用，目标合约只读，禁止修改状态
(bool success, bytes memory data) = target.staticcall(abi.encodeWithSignature("balanceOf(address)", user));

// 4. CALLCODE: 代码调用（已废弃，建议用DELEGATECALL）
(bool success, bytes memory data) = target.callcode(data);
```

对OpenPerp的影响：
- 清算逻辑需要用CALL调用用户仓位合约
- 预言机读取应用STATICCALL防止恶意修改
- 升级代理模式用DELEGATECALL

1.5 核心操作码（Opcode）分类

低Gas操作（1-10 gas）：
- STOP、RETURN、REVERT、INVALID
- ADD、MUL、SUB、DIV、MOD、EXP
- LT、GT、EQ、ISZERO
- AND、OR、XOR、NOT、BYTE、SHL、SHR、SAR
- POP、MLOAD、MSTORE、MSTORE8、SLOAD（已暖缓存）

中Gas操作（50-500 gas）：
- SLOAD（冷缓存，2100 gas）
- SSTORE（暖写入，2900 gas）
- JUMP、JUMPI、PC、MSIZE、GAS
- BALANCE、ORIGIN、CALLER、ADDRESS
- CALLDATALOAD、CALLDATASIZE、CALLDATACOPY

高Gas操作（1000+ gas）：
- SSTORE（冷写入，20000 gas）
- SHA3（Keccak256）
- LOG0-LOG4（事件日志）
- CALL、DELEGATECALL、STATICCALL（2600 gas基础）
- CREATE、CREATE2（32000 gas基础）
- Precompiles：ecrecover(3000)、modexp(200+)等

---

第二部分：EIP-1153 瞬态存储（Transient Storage）

2.1 什么是瞬态存储

瞬态存储是EIP-1153引入的新存储区域，专门解决「单次交易内需要跨调用传递临时状态」的痛点。

核心特性：
- 生命周期：单次交易结束后自动清除
- 作用域：合约全交易上下文（包括内部调用）
- Gas成本：TLOAD(100 gas)、TSTORE(100 gas)，恒定成本无冷热区分
- 与Storage的区别：不写入永久状态，无需清除逻辑

2.2 为什么需要瞬态存储

传统问题：
- 用Storage做临时标记需要20000 gas初始化，还需要清除逻辑（另2900 gas）
- 用Memory做临时标记无法跨内部调用（Memory仅在当前调用帧有效）
- 2300 gas限制：低Gas调用（如transfer/send）无法执行SSTORE

Transient Storage解决方案：
- 100 gas恒定成本
- 自动清除，无残留风险
- 2300 gas限制不适用

2.3 Solidity中的使用

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract TransientExample {
    // 声明transient变量（仅支持值类型）
    uint256 private transient _lock;
    address private transient _flashLoanInitiator;
    
    function executeFlashLoan() external {
        // 存储临时标记
        _flashLoanInitiator = msg.sender;  // TSTORE: 100 gas
        
        // 执行闪电贷逻辑...
        
        // 自动清除，无需手动重置
    }
    
    function checkInitiator() external view returns (address) {
        // 读取临时标记（仅在当前交易内有效）
        return _flashLoanInitiator;  // TLOAD: 100 gas
    }
}
```

2.4 重入锁优化对比

传统Storage重入锁（OpenPerp可能在用）：
```solidity
abstract contract StorageReentrancyGuard {
    uint256 private _status;
    uint256 private constant NOT_ENTERED = 1;
    uint256 private constant ENTERED = 2;
    
    constructor() {
        _status = NOT_ENTERED;  // SSTORE: 20000 gas
    }
    
    modifier nonReentrant() {
        require(_status != ENTERED, "ReentrancyGuard: reentrant call");
        _status = ENTERED;  // SSTORE: 2900 gas（暖写入）
        _;
        _status = NOT_ENTERED;  // SSTORE: 2900 gas（清除）
    }
}
```

Transient Storage重入锁（优化版）：
```solidity
abstract contract TransientReentrancyGuard {
    uint256 private transient _status;
    uint256 private constant NOT_ENTERED = 1;
    uint256 private constant ENTERED = 2;
    
    // 无需构造函数初始化！
    modifier nonReentrant() {
        require(_status != ENTERED, "ReentrancyGuard: reentrant call");
        _status = ENTERED;  // TSTORE: 100 gas（恒定）
        _;
        _status = NOT_ENTERED;  // TSTORE: 100 gas（可省略，自动清除）
    }
}
```

Gas成本对比：
| 操作 | Storage版 | Transient版 | 节省比例 |
|-----|-----------|-------------|---------|
| 初始化 | 20000 gas | 0 gas | 100% |
| 进入修饰符 | 2900 gas | 100 gas | 97% |
| 退出修饰符 | 2900 gas | 0-100 gas | 66-100% |
| 单次调用总计 | 5800 gas | 100-200 gas | 96-98% |

2.5 EIP-1153安全风险（重要）

风险1：TSTORE突破2300 gas限制
- 传统认为transfer/send（2300 gas）无法修改合约状态
- 但2300 gas足够执行TSTORE（100 gas）
- 攻击场景：低Gas调用可修改transient状态，影响后续操作

风险2：交易间残留风险
- Transient在交易间自动清除，不会跨交易残留
- 但在同一交易的不同调用间会共享状态
- 攻击案例：SIR.trading（2025年3月，$355K被盗）

风险3：Storage与Transient混淆
- 同一slot index可同时用于Storage和Transient
- 开发者混淆导致逻辑错误

2.6 SIR.trading攻击案例分析

攻击原理：
1. SIR.trading在transient slot 0x1存储Uniswap池地址
2. 闪电贷回调结束后，slot 0x1被复用来存储金额
3. 攻击者从特定地址调用uniswapV3SwapCallback，绕过地址校验
4. 因为slot 0x1残留了金额值，刚好匹配攻击者的合约地址

攻击代码简化：
```solidity
// SIR.trading的漏洞代码
function mint(address recipient, uint256 amount) external {
    // 存储池地址到transient
    tstore(PoolSlot, address(pool));  // TSTORE
    
    // 执行swap
    pool.swap(...);
    
    // 复用同一slot存金额（覆盖了池地址）
    tstore(PoolSlot, amount);  // TSTORE覆盖
}

function uniswapV3SwapCallback(int256 amount0, int256 amount1, bytes calldata data) external {
    // 读取池地址（但已被覆盖为金额！）
    address poolAddress = tload(PoolSlot);  // TLOAD
    
    // 校验调用者
    require(msg.sender == poolAddress, "Invalid pool");  // 被绕过！
}
```

OpenPerp防御措施：
1. Transient变量命名清晰，避免复用同一slot
2. 关键校验逻辑不依赖transient状态
3. 结合modifier确保状态一致性

---

第三部分：Monad并行EVM架构

3.1 Monad是什么

Monad是2026年最热的高性能L1，核心技术是并行执行的EVM（Parallel EVM），目标解决传统EVM的串行瓶颈。

性能指标：
- TPS：10000（测试网稳定2000-3000）
- 出块时间：0.4秒
- 最终确认：0.8秒
- EVM兼容性：100%字节码兼容，无需修改Solidity代码

3.2 并行执行核心原理

传统EVM：串行执行，一个交易完成才开始下一个
Monad EVM：并行执行无依赖交易，有依赖的交易仍串行

依赖分析：
- 读集（Read Set）：交易读取的所有Storage slot
- 写集（Write Set）：交易修改的所有Storage slot
- 冲突检测：读集与写集有交集时，交易间存在依赖

3.3 乐观并发控制（OCC）

Monad采用数据库经典的OCC（Optimistic Concurrency Control）算法：

```
交易1: 读取Slot A, 修改Slot A
交易2: 读取Slot B, 修改Slot B
-> 无依赖，可并行执行

交易1: 读取Slot A, 修改Slot A
交易2: 读取Slot A, 修改Slot C
-> 读集冲突，需串行执行
```

执行流程：
1. 预执行：所有交易并行运行
2. 收集：记录每个交易的读集和写集
3. 检测：按交易顺序检查冲突
4. 重试：冲突交易重新执行（最多重试N次）
5. 提交：无冲突的交易写入状态

3.4 MonadDB存储引擎

MonadDB是为并行执行优化的存储层：
- 并发随机读取优化
- 支持多线程同时访问
- 延迟比LevelDB低30%

3.5 对OpenPerp的影响

积极影响：
1. 高TPS：清算密集型操作无压力
2. 低延迟：0.8秒确认适合高频交易
3. EVM兼容：可直接部署，无需重构

需要注意的点：
1. 合约设计影响并行度
   - 不同用户的仓位独立：可并行清算
   - 全局配置修改：强制串行
   - 预言机更新：依赖链上时间戳，可能影响并行度

2. Storage访问模式优化
   - 避免全局Storage热点
   - 用Mapping隔离不同用户数据
   - 减少跨合约调用依赖

3. 测试方法更新
   - 模拟并行执行场景
   - 检查竞态条件（Race Condition）
   - 验证最终状态确定性

OpenPerp清算逻辑优化建议：
```solidity
// 优化前：串行清算
function batchLiquidate(address[] calldata users) external {
    for (uint i = 0; i < users.length; i++) {
        liquidate(users[i]);  // 每个调用都访问全局Storage
    }
}

// 优化后：并行友好清算
function liquidateUser(address user) external {
    // Mapping隔离每个用户状态，无全局依赖
    Position storage pos = positions[user];
    require(pos.isLiquidatable(), "Not liquidatable");
    
    // 独立计算用户债务
    uint256 debt = calculateDebt(user);
    uint256 collateral = pos.collateral;
    
    // 更新用户状态（仅修改user对应的slot）
    pos.isLiquidated = true;
    pos.debt = 0;
    
    // 转账给清算者（独立调用）
    payable(msg.sender).transfer(collateral - debt);
}
```

---

第四部分：2026 EVM前沿技术

4.1 EIP-7732 ePBS

将Proposer-Builder Separation（PBS）写入协议层：
- 当前PBS依赖Flashbots等服务
- ePBS让Builder直接向Proposer投标
- 减少MEV泄露，提升透明度
- 对OpenPerp的影响：减少三明治攻击，用户交易更公平

4.2 EIP-7748 并行执行

以太坊原生支持并行：
- 基于交易依赖图（TDG）
- 预计L1 TPS提升到100-200
- 2026年H2纳入Glamsterdam升级

4.3 新Gas表提案

2026年多个EIP建议调整Gas定价：
- 降低PUSH、DUP、SWAP操作码（3->2 gas）
- 提高ECRECOVER（3000->12000 gas）
- 调整TLOAD/TSTORE（100->20/50 gas）
- 对OpenPerp的影响：重新评估签名验证成本（清算需要验证用户签名）

4.4 JIT编译

Monad等链支持JIT编译：
- 将热点字节码编译为本地机器码
- 执行速度提升5-10倍
- 对OpenPerp的影响：清算计算逻辑（高频热点）可获大幅加速

---

第五部分：OpenPerp的EVM优化清单

5.1 Storage优化（最重要）

立即执行：
1. 所有重入锁改用Transient Storage
2. 用户仓位用Mapping隔离，避免全局Storage热点
3. 打包小类型变量（Address + uint64 + bool -> 1个slot）
4. 只读配置用immutable（不占Storage）

5.2 操作码优化

立即执行：
1. 外部函数参数用calldata（避免memory复制）
2. 循环减少SLOAD次数（缓存到Stack/Memory）
3. 计算用Stack变量，避免临时Storage
4. 事件索引选择（indexed参数适合过滤查询）

5.3 调用模式优化

立即执行：
1. 预言机用STATICCALL（2100 gas保底）
2. 批量操作用multicall（节省2100 gas基础费用）
3. 避免跨合约调用（减少CALL开销）
4. Delegatecall谨慎使用（不修改调用者敏感状态）

5.4 Monad适配

待执行：
1. 测试并行场景（多用户同时清算）
2. 依赖Monad并行特性优化合约结构
3. 监控Monad主网gas价格（调整gaslimit）
4. 升级到Solidity 0.8.24+（支持transient关键字）

---

第六部分：今日学习总结

6.1 EVM核心认知

1. 栈式虚拟机：所有操作基于256位word
2. 存储分层：Stack（临时）-> Memory（调用内）-> Storage（永久）
3. Gas计量：精确到每个操作码，防止恶意消耗
4. 调用隔离：CALL/DELEGATECALL/STATICCALL有明确边界

6.2 2026关键趋势

1. 并行执行：从串行到并行，TPS提升10-100倍
2. 瞬态存储：更高效的临时状态管理，需注意安全边界
3. JIT编译：热点代码加速5-10倍
4. Gas调整：更精细的成本定价


6.3 关键提醒

1. Transient Storage的2300 gas豁免是双刃剑
2. 并行执行下需重新审视竞态条件
3. 不要假设所有EVM都支持最新EIP（需检查兼容性）
4. 测试覆盖所有边界条件（特别是安全边界）

---

参考资料

1. Monad官方文档：https://docs.monad.xyz
2. EIP-1153：https://eips.ethereum.org/EIPS/eip-1153
3. Monad并行EVM深度解析：https://monadblock.com
4. 以太坊黄皮书：https://ethereum.org/yellowpaper
5. EVM操作码表：https://evm.codes
6. 2026新Gas提案：EIP-7904

````
<!-- DAILY_CHECKIN_2026-07-25_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->


````markdown
Uniswap V4 学习笔记

日期：2026年7月24日
主题：V4核心架构 + Hooks技术实现 + 2026年安全漏洞 + OpenPerp应用
学习目标：系统掌握V4的技术细节，为OpenPerp的V4 Hook开发做准备，同时积累项目经验用于简历

---

一、2026年Uniswap最新数据（锚定当前地位）

（数据来源）Uniswap Foundation、DefiLlama、Dune Analytics
1. 总交易量：累计$148B/月（2026年6月），连续周交易量$15B+（2026年7月）
2. V4占比：占Uniswap总交易量的30%，V3占60%（V4在快速抢占市场）
3. V4 TVL：$682M（2026年7月），累计交易量$422B，日均$870M
4. Hooks数量：超过41,000个已部署，Hook Pools占总交易量的15-20%
5. 跨链部署：15+条链，包括Ethereum、Arbitrum、Base、Monad、Robinhood Chain
6. 机构采用：Blackrock的BUIDL（美债代币）用Uniswap V4交易，RWA合规池上线

核心地位：Uniswap是DeFi的"流量入口"，任何想做DeFi的项目都必须考虑和Uniswap的集成（作为流动性来源或交易入口）

---

二、V4核心架构（技术层面）

2.1 V4 vs V3的本质差异

| 维度 | V3 | V4 |
|------|----|----|
| 架构 | 每个Pool一个独立合约 | 所有Pool在一个PoolManager合约（Singleton） |
| 创建Pool | 部署新合约，Gas ~2M | 状态更新，Gas ~20K（降低99%） |
| 多跳交易 | 每个Pool单独转账 | Flash Accounting，只结算净额（减少87% Gas） |
| ETH支持 | 需要包装成WETH | 原生支持ETH（省50% Gas） |
| 可定制 | 固定费率（4档） | Hooks（1024种组合） |
| 安全模型 | 信任Uniswap | 信任Uniswap + 信任Hook（安全边界下沉到Pool） |

2.2 Singleton架构详解

```
V3架构（每个Pool独立合约）：
Pool_ETH_USDC (合约地址A)
Pool_ETH_DAI (合约地址B)
Pool_USDC_DAI (合约地址C)
多跳交易：3次合约调用，3次Token转账

V4架构（Singleton）：
PoolManager (唯一合约地址)
  - Pool_ETH_USDC (状态变量)
  - Pool_ETH_DAI (状态变量)
  - Pool_USDC_DAI (状态变量)
多跳交易：1次合约调用，2次Token转账（净额结算）
```

关键改进：Pool不再是合约，而是PoolManager的一个状态条目（由PoolKey唯一标识）
```solidity
struct PoolKey {
    CurrencyId currency0;  // 代币0（可以是ETH或ERC20）
    CurrencyId currency1;  // 代币1
    uint24 fee;            // 费率
    int24 tickSpacing;     // 价格精度
    IHooks hooks;          // Hook合约地址（可以为address(0)）
}
// Pool ID = keccak256(PoolKey)
```

2.3 Flash Accounting（闪电结算）

（原理）交易过程中，所有Token余额变化只在PoolManager的内部账本（transient storage）记录，交易结束时只结算净额（net delta）

```
多跳交易：ETH -> USDC -> DAI
V3流程：
  1. 从用户转ETH到Pool_ETH_USDC
  2. Pool_ETH_USDC转USDC到用户
  3. 从用户转USDC到Pool_USDC_DAI
  4. Pool_USDC_DAI转DAI到用户
  = 4次Token转账

V4流程：
  1. 内部记账：ETH减少，USDC增加，DAI减少，DAI增加
  2. 结算净额：只转ETH出去，DAI进来
  = 2次Token转账（省50%）
```

（实现）EIP-1153 Transient Storage：一种新的存储类型，交易结束后自动清空，Gas更便宜

2.4 ERC-6909（代币凭证）

（背景）V4引入了"代币凭证"概念：用户在PoolManager内部持有余额，不需要每次都deposit/withdraw

```solidity
// 用户在PoolManager内部的余额
mapping(address => mapping(CurrencyId => uint256)) public balances;

// 用户可以在内部余额之间直接交易，不需要外部转账
function tradeInternal(CurrencyId from, CurrencyId to, uint256 amount) external {
    balances[msg.sender][from] -= amount;
    // 计算汇率，更新to的余额
    balances[msg.sender][to] += amount * rate;
}
```

好处：高频交易者可以省掉大量Gas（不需要每次都approve和transfer）

---

三、Hooks技术实现（核心）

3.1 Hook的8个回调点

Hooks可以在Pool的10个生命周期节点插入自定义逻辑，其中8个是回调点：

| 回调点 | 触发时机 | 典型应用 |
|--------|---------|---------|
| beforeInitialize | Pool初始化前 | 权限检查（谁能创建这个Pool） |
| afterInitialize | Pool初始化后 | 初始化额外状态（如价格预言机） |
| beforeAddLiquidity | 添加流动性前 | 限制流动性范围、设置KYC检查 |
| afterAddLiquidity | 添加流动性后 | 发放奖励、触发保险 |
| beforeRemoveLiquidity | 移除流动性前 | 锁定期检查、紧急情况限制 |
| afterRemoveLiquidity | 移除流动性后 | 扣除提前支取罚款 |
| beforeSwap | 交易前 | 动态费率、MEV保护、限价单检查 |
| afterSwap | 交易后 | 触发清算、更新预言机 |
| beforeDonate | 捐赠前 | 限制捐赠者 |
| afterDonate | 捐赠后 | 分配奖励 |

3.2 Hook的地址编码（最反直觉的设计）

（关键）Hook的权限不是在合约内部设置的，而是**编码在合约地址的低位**！

```solidity
// Hook权限位定义
uint160 public constant BEFORE_INITIALIZE_FLAG = 1 << 0;   // 第0位
uint160 public constant AFTER_INITIALIZE_FLAG = 1 << 1;    // 第1位
uint160 public constant BEFORE_ADD_LIQUIDITY_FLAG = 1 << 2;
uint160 public constant AFTER_ADD_LIQUIDITY_FLAG = 1 << 3;
uint160 public constant BEFORE_REMOVE_LIQUIDITY_FLAG = 1 << 4;
uint160 public constant AFTER_REMOVE_LIQUIDITY_FLAG = 1 << 5;
uint160 public constant BEFORE_SWAP_FLAG = 1 << 6;
uint160 public constant AFTER_SWAP_FLAG = 1 << 7;
// ... 更多标志位

// PoolManager检查Hook地址的权限
function checkHookPermissions(address hook) internal pure {
    uint160 flags = uint160(hook);
    // 如果Hook实现了beforeSwap，但地址第6位不是1，则不会被调用
    if (flags & BEFORE_SWAP_FLAG == 0) {
        revert("beforeSwap not enabled");
    }
}
```

（部署方法）必须用CREATE2挖矿，找到一个低位符合权限要求的地址：
```solidity
// OpenZeppelin的HookMiner工具
function mineHookAddress(bytes32 salt, uint160 requiredFlags) returns (address) {
    address hook;
    for (uint256 i = 0; i < 1000; i++) {
        hook = address(uint160(keccak256(abi.encodePacked(salt, i)))) & 0xFFFF;
        if (uint160(hook) & requiredFlags == requiredFlags) {
            return hook;
        }
    }
    revert("Hook address not found");
}
```

（安全隐患）如果Hook实现了beforeSwap，但地址第6位不是1，则这个函数永远不会被调用——开发者容易犯这个错！

3.3 Hook的调用流程

```
用户调用 PoolManager.swap()
  ↓
PoolManager检查：Hook地址第6位是否为1（beforeSwap权限）
  ↓（是）
PoolManager调用 Hook.beforeSwap()
  ↓
Hook返回：(selector, BeforeSwapDelta, feeOverride)
  ↓
PoolManager执行核心swap逻辑
  ↓
PoolManager检查：Hook地址第7位是否为1（afterSwap权限）
  ↓（是）
PoolManager调用 Hook.afterSwap()
  ↓
PoolManager结算净额
```

3.4 Hook的返回值影响核心逻辑

（beforeSwap返回值）
```solidity
struct BeforeSwapDelta {
    int128 specified;   // 指定的Delta（0表示不修改）
    int128 unspecified; // 未指定的Delta
}

// 返回值可以：
// 1. 修改费率（feeOverride）
// 2. 修改交易数量（beforeSwapDelta）
// 3. 完全接管swap逻辑（返回指定的Delta）
```

这意味着Hook可以**完全控制Pool的行为**，风险极高！

3.5 最简Hook代码示例

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import {BaseHook} from "v4-periphery/src/base/hooks/BaseHook.sol";
import {IPoolManager} from "v4-core/src/interfaces/IPoolManager.sol";
import {Hooks} from "v4-core/src/libraries/Hooks.sol";
import {PoolKey} from "v4-core/src/types/PoolKey.sol";
import {BeforeSwapDelta, toBeforeSwapDelta} from "v4-core/src/types/BeforeSwapDelta.sol";

// 动态费率Hook：波动率高时费率增加
contract DynamicFeeHook is BaseHook {
    uint24 public constant BASE_FEE = 3000;      // 基础费率0.3%
    uint24 public constant HIGH_FEE = 10000;     // 高费率1%
    uint256 public constant VOLATILITY_THRESHOLD = 1000; // 波动率阈值

    constructor(IPoolManager _poolManager) BaseHook(_poolManager) {}

    // beforeSwap：根据波动率决定费率
    function _beforeSwap(
        address sender,
        PoolKey calldata key,
        IPoolManager.SwapParams calldata params,
        bytes calldata hookData
    ) internal override returns (bytes4, BeforeSwapDelta, uint24) {
        // 计算最近1小时的波动率（简化版）
        uint256 volatility = calculateVolatility(key);
        
        // 波动率高时用高费率
        uint24 fee = volatility > VOLATILITY_THRESHOLD ? HIGH_FEE : BASE_FEE;
        
        // 返回费率覆盖值
        return (this.beforeSwap.selector, toBeforeSwapDelta(0, 0), fee);
    }

    // Hook权限：启用beforeSwap
    function getHookPermissions() public pure override returns (Hooks.Permissions memory) {
        return Hooks.Permissions({
            beforeInitialize: false,
            afterInitialize: false,
            beforeAddLiquidity: false,
            afterAddLiquidity: false,
            beforeRemoveLiquidity: false,
            afterRemoveLiquidity: false,
            beforeSwap: true,    // 第6位必须为1！
            afterSwap: false,
            beforeDonate: false,
            afterDonate: false,
            beforeSwapReturnDelta: false,
            afterSwapReturnDelta: false
        });
    }

    // 简化的波动率计算（实际需要TWAP）
    function calculateVolatility(PoolKey calldata key) internal view returns (uint256) {
        // 实际实现：用key.currency0和key.currency1的价格方差
        return 500; // 示例值
    }
}
```

3.6 Hook的安全检查（OpenZeppelin BaseHook已实现）

```solidity
// BaseHook自动检查：
// 1. 调用者必须是PoolManager
modifier onlyPoolManager() {
    if (msg.sender != address(poolManager)) revert NotPoolManager();
    _;
}

// 2. 地址权限位必须匹配
function validateHookAddress(Hooks.Permissions memory permissions) internal pure {
    uint160 addressFlags = uint160(address(this));
    uint160 requiredFlags = getFlaggedPermissions(permissions);
    if (addressFlags & requiredFlags != requiredFlags) {
        revert HookAddressMismatch();
    }
}
```

---

四、2026年Hook安全漏洞（真实案例）

4.1 Hook安全的核心困境

V3安全模型：信任Uniswap协议本身（50%+ TVL，没有被黑过）
V4安全模型：信任Uniswap协议 + 信任每个Pool绑定的Hook（安全边界分散）

（2026年数据）Beosin报告：Hook相关安全事件12起，损失$3.2M，主要漏洞类型：
- 未授权调用（28%）
- 重入攻击（23%）
- 地址权限位错误（18%）
- 闪电贷操纵（15%）
- 其他（16%）

4.2 漏洞1：未授权调用Hook

（原理）Hook的回调函数没有检查调用者是否是PoolManager，任何人都能调用

（案例）2026年3月，一个JIT（即时流动性）Hook被攻击：攻击者直接调用beforeSwap，绕过PoolManager的检查，套取了$120K的LP奖励

（防御）用OpenZeppelin的BaseHook，自带onlyPoolManager modifier
```solidity
// 错误实现
contract BadHook {
    function beforeSwap(...) external returns (...) { // 没有权限检查！
        // 攻击者可以直接调用
    }
}

// 正确实现
contract GoodHook is BaseHook {
    function _beforeSwap(...) internal override returns (...) { // BaseHook自动检查
        // 只有PoolManager能调用
    }
}
```

4.3 漏洞2：Hook重入攻击

（原理）Hook在beforeSwap中调用外部合约，外部合约又回调Hook，形成重入

（案例）2026年5月，一个限价单Hook被重入攻击：攻击者在beforeSwap中调用Uniswap V3，V3的回调又触发V4的beforeSwap，反复执行，套取了$80K

（防御）在Hook中加ReentrancyGuard
```solidity
contract SafeHook is BaseHook, ReentrancyGuard {
    uint256 private locked;
    
    modifier noReentrant() {
        require(locked == 0, "reentrant");
        locked = 1;
        _;
        locked = 0;
    }
    
    function _beforeSwap(...) internal override noReentrant returns (...) {
        // 即使外部合约回调，也会被locked阻塞
    }
}
```

4.4 漏洞3：地址权限位错误

（原理）Hook实现了beforeSwap，但地址的第6位不是1，导致永远不会被调用；或者地址的第6位是1，但Hook没有实现beforeSwap，导致PoolManager调用时revert

（案例）2026年2月，一个团队部署了Hook，但地址权限位设置错误，导致Pool的swap全部revert，损失了$50K的LP资金（无法撤回）

（防御）部署前用HookMiner正确挖矿地址，部署后验证权限
```solidity
// 部署验证
function validateDeployment() {
    uint160 addressFlags = uint160(address(this));
    require(addressFlags & BEFORE_SWAP_FLAG != 0, "beforeSwap not enabled");
    require(addressFlags & AFTER_SWAP_FLAG == 0, "afterSwap should not be enabled");
}
```

4.5 漏洞4：闪电贷操纵Hook

（原理）Hook用内部价格（而不是TWAP）判断波动率/价格，攻击者用闪电贷操纵价格

（案例）2026年1月，一个动态费率Hook被攻击：攻击者用闪电贷在Hook的Pool中大额交易，触发高费率（1%），然后在另一个Pool用低费率（0.3%）交易，套取了$150K

（防御）Hook必须用TWAP，不能用内部价格
```solidity
// 错误：用内部价格
function calculateVolatility(PoolKey calldata key) internal view returns (uint256) {
    (uint160 sqrtPriceX96, , , , , , ) = poolManager.getSlot0(key.toId());
    // 这个价格可以被闪电贷操纵！
}

// 正确：用TWAP
function calculateVolatility(PoolKey calldata key) internal view returns (uint256) {
    uint256 twap = getTWAP(key, 3600); // 1小时TWAP
    // TWAP很难被操纵（需要持续1小时的大额资金）
}
```

---

五、2026年热门Hook应用

5.1 动态费率Hook（解决V3固定费率问题）

（原理）根据波动率、交易量、滑点等因素动态调整费率
（案例）Volatile Hook：波动率高时费率从0.3%升到1%，波动率低时降到0.05%
（好处）高费率保护LP免受无常损失，低费率吸引交易量

```solidity
// OpenPerp可以复用：清算触发时费率提升（惩罚恶意清算）
contract LiquidationHook is BaseHook {
    uint24 public constant NORMAL_FEE = 3000;   // 0.3%
    uint24 public constant LIQUIDATION_FEE = 10000; // 1%（清算场景）
    
    function _beforeSwap(...) internal override returns (...) {
        bool isLiquidation = checkLiquidation(msg.sender);
        uint24 fee = isLiquidation ? LIQUIDATION_FEE : NORMAL_FEE;
        return (this.beforeSwap.selector, toBeforeSwapDelta(0, 0), fee);
    }
}
```

5.2 限价单Hook（解决V3只支持市价单问题）

（原理）在beforeSwap中检查是否有等待的限价单，如果有则匹配
（案例）Limit Order Hook：用户挂限价单，价格到了自动执行
（代码简化）
```solidity
contract LimitOrderHook is BaseHook {
    struct Order {
        address maker;
        uint256 price;  // 限价
        uint256 amount; // 数量
        bool isBuy;     // true=买，false=卖
    }
    mapping(bytes32 => Order[]) public orders; // 每个Pool的订单簿
    
    function _beforeSwap(...) internal override returns (...) {
        // 检查是否有匹配的限价单
        Order[] storage poolOrders = orders[key.toId()];
        for (uint256 i = 0; i < poolOrders.length; i++) {
            if (isMatch(poolOrders[i], params)) {
                // 匹配成功，执行限价单
                return executeLimitOrder(poolOrders[i], params);
            }
        }
        // 没有匹配，继续正常swap
        return (this.beforeSwap.selector, toBeforeSwapDelta(0, 0), 3000);
    }
}
```

5.3 MEV保护Hook（解决三明治攻击问题）

（原理）在beforeSwap中检查是否是三明治攻击，拒绝或惩罚
（案例）MEV Guard Hook：检测交易是否在被夹，夹则收高费率或拒绝
（方法）在Hook中检查交易发送者是否在最近的交易中大额买入/卖出同一代币

5.4 JIT流动性Hook（解决JIT问题？或者利用JIT？）

（背景）JIT（Just-In-Time）流动性：LP在交易前瞬间添加流动性，交易后立即移除，获取所有手续费，导致普通LP收益下降
（两种态度）
- 反对JIT：Hook在afterAddLiquidity中检查是否立即移除，收取罚款
- 利用JIT：Hook自动提供JIT流动性，作为MEV机会

5.5 RWA合规Hook（2026年最热应用）

（背景）Blackrock的BUIDL（美债代币）需要合规：只有KYC通过的地址能交易
（实现）Hook在beforeSwap中检查交易地址是否在白名单
```solidity
contract KYCWhitelistHook is BaseHook {
    IWhitelist public whitelist; // 合规白名单合约
    
    function _beforeInitialize(...) internal override returns (...) {
        require(whitelist.isApproved(msg.sender), "not approved");
    }
    
    function _beforeSwap(...) internal override returns (...) {
        require(whitelist.isApproved(params.recipient), "recipient not approved");
    }
}
```

（意义）V4 + Hook = 合规化AMM，打通RWA和DeFi

---

六、OpenPerp的V4 Hook应用（核心）

6.1 OpenPerp可以用V4 Hook做什么

| 功能 | Hook类型 | 解决的问题 | 实现难度 |
|------|---------|-----------|---------|
| 动态清算费率 | beforeSwap | 清算时收高费率，保护LP | 低 |
| 价格预警 | afterSwap | 价格接近清算线时触发通知 | 低 |
| 防闪电贷清算 | beforeSwap | 检测闪电贷，延迟清算 | 中 |
| 自动再平衡 | afterSwap | 清算后自动补充流动性 | 中 |
| 机构白名单 | beforeSwap | 只有机构能交易某些Pool | 低 |
| MEV保护 | beforeSwap | 检测三明治攻击 | 高 |
| 限价单清算 | beforeSwap | 清算单作为限价单挂出 | 高 |

6.2 最简OpenPerp Hook实现（清算费率）

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import {BaseHook} from "v4-periphery/src/base/hooks/BaseHook.sol";
import {IPoolManager} from "v4-core/src/interfaces/IPoolManager.sol";
import {Hooks} from "v4-core/src/libraries/Hooks.sol";
import {PoolKey} from "v4-core/src/types/PoolKey.sol";
import {BeforeSwapDelta, toBeforeSwapDelta} from "v4-core/src/types/BeforeSwapDelta.sol";

// OpenPerp清算Hook
contract OpenPerpLiquidationHook is BaseHook {
    uint24 public constant NORMAL_FEE = 3000;       // 正常费率0.3%
    uint24 public constant LIQUIDATION_FEE = 20000; // 清算费率2%（惩罚性）
    
    address public immutable clearingHouse;         // 清算所地址
    
    constructor(IPoolManager _poolManager, address _clearingHouse) BaseHook(_poolManager) {
        clearingHouse = _clearingHouse;
    }
    
    // beforeSwap：检查是否是清算交易
    function _beforeSwap(
        address sender,
        PoolKey calldata key,
        IPoolManager.SwapParams calldata params,
        bytes calldata hookData
    ) internal override returns (bytes4, BeforeSwapDelta, uint24) {
        // 检查是否是清算交易（由清算所触发）
        bool isLiquidation = (sender == clearingHouse);
        
        // 清算交易收高费率
        uint24 fee = isLiquidation ? LIQUIDATION_FEE : NORMAL_FEE;
        
        // 记录清算事件
        if (isLiquidation) {
            emit LiquidationDetected(key.currency0, key.currency1, params.amountSpecified);
        }
        
        return (this.beforeSwap.selector, toBeforeSwapDelta(0, 0), fee);
    }
    
    // Hook权限：启用beforeSwap和afterSwap
    function getHookPermissions() public pure override returns (Hooks.Permissions memory) {
        return Hooks.Permissions({
            beforeInitialize: false,
            afterInitialize: false,
            beforeAddLiquidity: false,
            afterAddLiquidity: false,
            beforeRemoveLiquidity: false,
            afterRemoveLiquidity: false,
            beforeSwap: true,
            afterSwap: true,   // 价格预警用
            beforeDonate: false,
            afterDonate: false,
            beforeSwapReturnDelta: false,
            afterSwapReturnDelta: false
        });
    }
    
    // afterSwap：价格接近清算线时触发
    function _afterSwap(
        address sender,
        PoolKey calldata key,
        IPoolManager.SwapParams calldata params,
        BalanceDelta delta,
        bytes calldata hookData
    ) internal override {
        // 计算当前价格
        (uint160 sqrtPriceX96, , , , , , ) = poolManager.getSlot0(key.toId());
        uint256 currentPrice = sqrtPriceX96ToPrice(sqrtPriceX96);
        
        // 检查是否接近清算线（示例：价格偏离10%）
        uint256 liquidationPrice = getLiquidationPrice(sender);
        if (isCloseToLiquidation(currentPrice, liquidationPrice)) {
            emit LiquidationWarning(sender, currentPrice, liquidationPrice);
            // 可以触发链下Keeper自动补充保证金
        }
    }
    
    // 价格转换（简化版）
    function sqrtPriceX96ToPrice(uint160 sqrtPriceX96) internal pure returns (uint256) {
        return (uint256(sqrtPriceX96) * uint256(sqrtPriceX96)) >> 192;
    }
    
    // 简化的价格检查
    function isCloseToLiquidation(uint256 current, uint256 liquidation) internal pure returns (bool) {
        if (liquidation == 0) return false;
        uint256 diff = current > liquidation ? current - liquidation : liquidation - current;
        return diff * 1000 / liquidation < 100; // 偏离<10%
    }
    
    // 简化的清算价格获取
    function getLiquidationPrice(address user) internal view returns (uint256) {
        // 实际实现：从清算所获取用户的清算价格
        return 0;
    }
    
    // 事件
    event LiquidationDetected(CurrencyId indexed token0, CurrencyId indexed token1, int256 amount);
    event LiquidationWarning(address indexed user, uint256 currentPrice, uint256 liquidationPrice);
}
```

---

七、V4开发工具链

7.1 核心依赖

```solidity
// remappings.txt（Foundry）
v4-core=lib/v4-core/
v4-periphery=lib/v4-periphery/
@openzeppelin/=lib/openzeppelin-contracts/
```

```bash
# 安装依赖
forge install Uniswap/v4-core
forge install Uniswap/v4-periphery
forge install OpenZeppelin/openzeppelin-contracts
```

7.2 测试用例框架

```solidity
// Foundry测试
import {DynamicFeeHook} from "../src/DynamicFeeHook.sol";
import {IPoolManager} from "v4-core/src/interfaces/IPoolManager.sol";
import {PoolManager} from "v4-core/src/PoolManager.sol";

contract DynamicFeeHookTest is Test {
    DynamicFeeHook public hook;
    PoolManager public poolManager;
    
    function setUp() public {
        poolManager = new PoolManager(address(this));
        hook = new DynamicFeeHook(poolManager);
    }
    
    function testBeforeSwap() public {
        // 测试beforeSwap返回正确的费率
        IPoolManager.SwapParams memory params = IPoolManager.SwapParams({
            zeroForOne: true,
            amountSpecified: 1000,
            sqrtPriceLimitX96: 0
        });
        
        (bytes4 selector, BeforeSwapDelta delta, uint24 fee) = hook.beforeSwap(
            address(this),
            PoolKey({...}),
            params,
            ""
        );
        
        assertEq(fee, 3000); // 基础费率
    }
    
    function testDynamicFee() public {
        // 测试波动率高时费率增加
        // ... 设置高波动率 ...
        (, , uint24 fee) = hook.beforeSwap(...);
        assertEq(fee, 10000); // 高费率
    }
    
    // Fuzz测试（随机输入）
    function testFuzz_BeforeSwap(uint256 randomAmount) public {
        vm.assume(randomAmount > 0 && randomAmount < 1e18);
        // 测试随机金额的swap
    }
}
```

7.3 部署流程

```bash
# 1. 用HookMiner找到正确的地址
npx @uniswap/v4-hook-miner --before-swap true --after-swap true

# 2. 部署Hook合约
forge create src/OpenPerpLiquidationHook.sol:OpenPerpLiquidationHook \
    --constructor-args $POOL_MANAGER $CLEARING_HOUSE \
    --rpc-url $MONAD_RPC \
    --private-key $KEY

# 3. 创建Pool（绑定Hook）
cast send $POOL_MANAGER "initialize(PoolKey,sqrtPriceX96,bytes)" \
    $POOL_KEY $INITIAL_PRICE "" \
    --rpc-url $MONAD_RPC \
    --private-key $KEY
```

---

八、今日总结

8.1 V4的核心价值

1. **Singleton架构**：Pool创建成本降99%，多跳交易Gas降87%
2. **Hooks**：把Uniswap从"固定规则的AMM"变成"可编程的流动性平台"
3. **Flash Accounting**：净额结算，减少Token转账次数
4. **机构采用**：Blackrock用V4做RWA交易，合规化AMM成为可能

8.2 Hook的核心风险

1. **地址权限位错误**：开发者容易犯，导致Hook不工作或Pool revert
2. **未授权调用**：没有onlyPoolManager检查，任何人都能调用
3. **重入攻击**：Hook调用外部合约时容易被重入
4. **闪电贷操纵**：用内部价格判断波动率会被操纵，必须用TWAP

8.3 OpenPerp的应用机会

1. **动态清算费率**：清算时收高费率（2% vs 0.3%），保护LP
2. **价格预警**：afterSwap检测价格接近清算线，触发Keeper自动补充保证金
3. **机构白名单**：Hook检查KYC，允许机构合规交易
4. **防闪电贷清算**：beforeSwap检测闪电贷，延迟清算或收取额外费用

````
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->



````markdown
可升级合约学习笔记

日期：2026年7月23日
主题：三种主流代理模式对比 + 2026年最新漏洞案例 + OpenPerp项目技术选型
学习目标：系统掌握可升级合约的原理、安全风险，为OpenPerp的合约架构做技术决策

---

一、为什么需要可升级合约

（背景）EVM智能合约一旦部署，字节码永久不可变——这是区块链的核心信任基础，但也带来现实问题：
1. 代码有bug无法修复（2023年Aave V3的bug导致$80M暂时被锁3天）
2. 无法添加新功能（Uniswap每次升级都要迁移，数亿美元TVL搬家）
3. 无法紧急暂停（2026年某DeFi协议被攻击后，无法紧急冻结资金）

（数据）根据2026年最新研究，54%的活跃以太坊合约使用了代理模式，说明可升级已是行业标配

核心矛盾：可升级=灵活性，但也=中心化风险（谁有权升级？）；不可变=信任，但也=僵化（bug无法修复）

（解决思路）混合架构：核心层不可变（保证信任），功能层可升级（保证灵活性）——Aave V4采用的就是这个思路

---

二、可升级合约的核心原理

2.1 代理模式的本质

```
用户调用 Proxy（地址不变）
  ↓
Proxy 通过 delegatecall 调用 Implementation（逻辑合约）
  ↓
Implementation 的代码在 Proxy 的存储里执行
  ↓
升级 = 换一个新的 Implementation 地址
```

delegatecall的关键特性：
- 被调用合约（Implementation）的代码在调用者（Proxy）的存储里执行
- msg.sender 保持为原始调用者
- 只有代码变了，存储不变——这是可升级的核心

2.2 EIP-1967：标准化存储槽

（背景）早期代理的致命问题：Proxy的存储槽0放implementation地址，但Implementation的状态变量也从槽0开始存——写Implementation的第一个变量会覆盖Proxy的implementation地址，导致代理失效

（解决）EIP-1967用keccak256生成伪随机存储槽，避免冲突：
```solidity
// Implementation 地址存储槽（EIP-1967标准）
bytes32 private constant IMPLEMENTATION_SLOT = 
    bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1);
// 硬编码为 0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc

// Admin 地址存储槽
bytes32 private constant ADMIN_SLOT = 
    bytes32(uint256(keccak256("eip1967.proxy.admin")) - 1);
// 硬编码为 0xb53127684a568b3173ae13b9f8a6016e243e63b6e8ee1178d6a717850b5d6103
```

---

三、三种主流代理模式

3.1 Transparent Proxy（透明代理）

（提出者）OpenZeppelin，最早的标准化代理

工作原理：
```
Proxy 内部存储：
  - implementation（EIP-1967槽）
  - admin（EIP-1967槽）

调用路由：
  if msg.sender == admin:
      执行Proxy的管理函数（upgradeTo, changeAdmin）
  else:
      delegatecall 到 Implementation
```

优点：
- Admin和用户调用完全隔离，不会误触发
- 逻辑清晰，审计简单

缺点：
- 每次调用都要判断msg.sender，Gas略贵（多15-20%）
- Admin权限在Proxy里，无法通过DAO治理

代码示例（OpenZeppelin标准）：
```solidity
// 部署
address proxy = upgrades.deployProxy(
    BoxImplementation,  // 实现合约
    [42],                // 初始化参数
    { kind: "transparent" }
);

// 升级
address newImpl = upgrades.prepareUpgrade(
    proxy.address,
    BoxImplementationV2
);
Proxy(proxy).upgradeTo(newImpl);
```

适用场景：
- 对Gas成本不敏感的DeFi协议（如Aave V3）
- 需要严格区分管理员和用户操作的场景

3.2 UUPS（Universal Upgradeable Proxy Standard）

（提出者）EIP-1822，OpenZeppelin现在推荐的标准

工作原理：
```
Proxy 内部存储：
  - implementation（EIP-1967槽）
  - （没有admin，没有任何逻辑）

升级逻辑在Implementation里：
  function upgradeTo(address newImpl) public onlyOwner {
      _setImplementation(newImpl);
  }

Proxy 只有 fallback：
  fallback() external payable {
      delegatecall(implementation);
  }
```

优点：
- Proxy极简，部署Gas节省20%
- 升级权限在Implementation里，可以用onlyOwner、multisig、timelock、DAO治理
- 升级逻辑本身也可以升级（比如移除升级权限）

缺点：
- Implementation必须包含upgradeTo函数，否则永久锁定
- 如果Implementation的权限检查（onlyOwner）配置错了，任何人都能升级

代码示例（OpenZeppelin标准）：
```solidity
// Implementation 合约
contract MyContract is Initializable, UUPSUpgradeable, OwnableUpgradeable {
    uint256 public value;

    function initialize(uint256 _value) public initializer {
        __Ownable_init();
        __UUPSUpgradeable_init();
        value = _value;
    }

    // 升级权限检查：只有owner能升级
    function _authorizeUpgrade(address newImplementation) internal override onlyOwner {}

    function upgradeValue(uint256 newValue) public {
        value = newValue;
    }
}

// 部署
address proxy = upgrades.deployProxy(
    MyContract,
    [42],
    { kind: "uups" }  // 指定用UUPS
);
```

（OpenZeppelin官方建议）2026年起，新项目优先选UUPS，因为更轻量、更灵活

3.3 Diamond Proxy（钻石模式，EIP-2535）

（提出者）Nick Mudge，2026年增长最快的代理模式

工作原理：
```
一个 Diamond（Proxy），多个 Facet（逻辑合约）
  - 每个函数选择器（selector）映射到不同的Facet
  - 可以动态添加/替换/删除函数

存储：
  mapping(bytes4 => address) facets;  // selector -> Facet地址
```

优点：
- 突破24KB字节码限制（单个合约有大小限制）
- 细粒度升级：只换某个功能的Facet，不影响其他
- 模块化：不同团队开发不同Facet

缺点：
- 实现复杂，调试困难
- 工具链（Foundry、Slither）支持不完善
- 跨Facet调用需要额外gas

代码示例：
```solidity
contract Diamond {
    struct FacetAddressAndPosition {
        address facetAddress;
        uint96 functionSelectorPosition;
    }
    mapping(bytes4 => FacetAddressAndPosition) public facetAddressAndPosition;

    fallback() external payable {
        address facet = facetAddressAndPosition[msg.sig].facetAddress;
        require(facet != address(0), "Function not found");
        delegatecall(facet);
    }
}

// Facet 合约
contract SwapFacet {
    function swap(address tokenIn, address tokenOut, uint256 amount) external {
        // 交换逻辑
    }
}

contract OrderFacet {
    function placeOrder(uint256 price, uint256 amount) external {
        // 下单逻辑
    }
}
```

适用场景：
- 超大型协议（Uniswap V5、Aave V5可能用）
- 需要频繁迭代的项目
- 模块化架构的DeFi协议

3.4 三种模式对比

| 维度 | Transparent | UUPS | Diamond |
|------|------------|------|---------|
| Proxy复杂度 | 中 | 极简 | 高 |
| 部署Gas | 中 | 低（省20%） | 高 |
| 升级位置 | Proxy里 | Implementation里 | Diamond里 |
| 权限管理 | Proxy固定admin | Implementation自定义 | Diamond自定义 |
| 24KB限制 | 无 | 无 | 突破 |
| 审计难度 | 低 | 中 | 高 |
| 社区推荐 | 传统标准 | 2026首选 | 未来趋势 |
| 代表项目 | Aave V3 | Aave V4 | 新项目探索 |

---

四、可升级合约的安全漏洞（2026年最新案例）

（背景）根据2026年OWASP TOP 10，代理升级漏洞成为第10类（首次入选），1年发生37起，7起损失超$10M，2起超$100M

4.1 漏洞类型1：未初始化的Implementation

（原理）代理合约不能用constructor（因为constructor在Implementation的存储里执行，不是Proxy的），所以用initialize()函数代替constructor。但如果initialize()没被加权限检查，任何人都能调用，设置自己为owner，然后升级到恶意合约

（案例）Kinto Protocol（2025.7）：攻击者发现某个Implementation没加initializer权限，调用initialize()设置自己为owner，然后升级到恶意合约，盗取$1.55M

（防御）必须给Implementation的constructor加_disableInitializers()：
```solidity
contract MyContract is Initializable, UUPSUpgradeable {
    constructor() {
        _disableInitializers();  // 锁定初始化状态，任何人无法再initialize
    }
}
```

（注意）必须在deploy后立即调用initialize()，不能有任何间隔——即使间隔1个block，也是攻击窗口

4.2 漏洞类型2：Admin密钥泄露

（原理）谁有权升级，谁就能控制合约。如果admin密钥被盗，攻击者可以升级到恶意合约，直接盗走所有资金

（案例）UPCX Protocol（2025）：admin是单个EOA，密钥被盗，攻击者升级合约，盗取$70M

（案例）Drift Protocol（2026.4）：多签治理（2-of-5），攻击者通过9天社会工程学攻击，骗取2个签名，升级合约盗取$2.85M

（防御）
- 不用单个EOA做admin，用multisig（至少3-of-5，签名人独立）
- 所有升级必须经过timelock（至少48小时，给用户时间退出）
- 升级前必须在链上发布提案，让社区审核

4.3 漏洞类型3：存储布局不兼容

（原理）升级时，新Implementation的状态变量顺序/类型必须和旧Implementation完全一致，否则会覆盖Proxy的存储槽，导致数据错乱

（示例）
```solidity
// V1 实现
contract V1 {
    address public owner;      // 槽0
    uint256 public totalSupply; // 槽1
}

// V2 实现（错误！顺序变了）
contract V2 {
    uint256 public totalSupply; // 槽0（覆盖了owner地址！）
    address public owner;      // 槽1
}
```

（防御）
- 用OpenZeppelin的存储布局检查工具：`upgrades.validateStorageLayout`
- 新版本合约加注释：`@custom:oz-upgrades-from V1`
- 不要在已有变量中间插入新变量，只能在末尾加

（OpenZeppelin 2026新工具）Foundry/ Hardhat的upgrades插件会自动检查存储布局

4.4 漏洞类型4：升级到恶意合约

（原理）即使Admin密钥没泄露，如果Proxy的implementation地址被篡改（比如通过存储槽冲突），也会执行恶意代码

（防御）
- 不要手动写Proxy，用OpenZeppelin的标准实现
- 在升级前验证新Implementation的代码（至少人工审核）
- 用Forta/OpenZeppelin Defender监控升级事件

---

五、OpenPerp项目的可升级合约选型

5.1 选型决策

（我的判断）OpenPerp应该用UUPS模式，理由：

1. 轻量：Proxy极简，部署Gas省20%——对新项目很重要
2. 灵活：升级权限可以先设为multisig（3-of-5），后期过渡到DAO
3. 安全：OpenZeppelin标准实现，社区验证充分
4. 未来：可以添加UpgradeableBeacon模式（多个相同逻辑的代理批量升级），适合多个交易对的清算合约

5.2 混合架构设计

（思路）核心层不可变，功能层可升级——参考Aave V4的Hub-and-Spoke

```
OpenPerp 架构：
  ├─ Core（不可变，部署后锁定upgradeTo）
  │   ├─ OrderBook（订单簿）
  │   ├─ ClearingHouse（清算所）
  │   └─ Oracle（预言机）
  │
  ├─ Modules（可升级，UUPS）
  │   ├─ DynamicFeeModule（动态费率，V4 Hook）
  │   ├─ LiquidationModule（清算逻辑）
  │   └─ RewardModule（奖励机制）
  │
  └─ Governance（可升级，UUPS）
      ├─ Multisig（3-of-5，升级权限）
      └─ Timelock（48小时延迟）
```

（实现方式）Core通过interface调用Modules，Modules的Implementation地址存在Core里，升级时只换Modules的Implementation，Core本身不动

5.3 权限管理设计

```solidity
contract OpenPerpCore is Initializable {
    // 权限
    address public multisig;     // 3-of-5多签
    address public timelock;     // 48小时时间锁
    mapping(bytes32 => address) public moduleImpl;  // 模块实现映射

    // 升级必须满足：multisig签名 + timelock通过
    function upgradeModule(bytes32 moduleId, address newImpl) external {
        require(msg.sender == timelock, "only timelock");
        address oldImpl = moduleImpl[moduleId];
        // 验证新实现的接口兼容性
        require(IERC165(newImpl).supportsInterface(MODULE_INTERFACE), "invalid interface");
        moduleImpl[moduleId] = newImpl;
        emit ModuleUpgraded(moduleId, oldImpl, newImpl);
    }

    // Core的升级：只允许紧急暂停，不允许逻辑变更
    // 或者用_disableInitializers()彻底锁定
}
```

5.4 安全措施清单（OpenPerp专属）

1. 每个Implementation的constructor都加_disableInitializers()
2. 部署后立即调用initialize()，用OpenZeppelin的一键部署
3. 不用单个EOA做admin，用Safe（Gnosis Safe）的3-of-5 multisig
4. 所有升级必须经过48小时timelock
5. 升级前在链上发布提案（Snapshot投票），让社区审核
6. 用Forta监控所有upgradeModule事件，10秒内触发报警
7. Core合约部署后，通过DAO投票锁定upgrade权限
8. 所有升级必须经过至少1次外部审计（Trail of Bits或OpenZeppelin）

---

六、可升级 vs 不可变：如何抉择

（决策框架）

选不可变的情况：
- 协议逻辑简单，不会变（如ERC20代币）
- 对安全要求极高（如跨链桥的核心验证）
- 需要最大的信任（如稳定币的铸币逻辑）
- 可以接受用户迁移成本

选可升级的情况：
- 协议逻辑复杂，可能有bug（如DeFi的清算逻辑）
- 需要频繁添加新功能（如DEX的新交易对）
- 需要紧急暂停功能（如安全漏洞）
- 无法接受用户迁移成本

（行业趋势）2026年越来越多协议采用"混合架构"：核心功能不可变，边缘功能可升级——既保证信任，又保留灵活性

（我的判断）OpenPerp应该用混合架构：Core不可变（订单簿、清算所），Modules可升级（费率、奖励、清算细节）。这样既保证核心安全，又保留迭代空间

---

七、代码实操：UUPS代理的完整流程

7.1 用Foundry部署

```solidity
// remappings.txt
@openzeppelin/contracts/=lib/openzeppelin-contracts-upgradeable/lib/openzeppelin-contracts/contracts/
@openzeppelin/contracts-upgradeable/=lib/openzeppelin-contracts-upgradeable/contracts/
```

```solidity
// OpenPerpCoreV1.sol
contract OpenPerpCore is Initializable, UUPSUpgradeable, OwnableUpgradeable {
    uint256 public orderCount;
    address public feeModule;

    function initialize(address _feeModule) public initializer {
        __Ownable_init();
        __UUPSUpgradeable_init();
        feeModule = _feeModule;
    }

    function _authorizeUpgrade(address newImplementation) internal override onlyOwner {
        // 这里可以加timelock检查
    }

    function placeOrder(uint256 price, uint256 amount) external {
        orderCount++;
        emit OrderPlaced(msg.sender, price, amount);
    }
}
```

```bash
# 部署
forge create OpenPerpCoreV1 --constructor-args 0x...feeModule... --rpc-url $RPC --private-key $KEY

# 升级（用OpenZeppelin Foundry Upgrades插件）
# 1. 准备新版本
forge create OpenPerpCoreV2 --constructor-args $PROXY_ADDRESS --rpc-url $RPC --private-key $KEY
# 2. 升级
cast send $PROXY "upgradeTo(address)" $NEW_IMPL_ADDRESS --rpc-url $RPC --private-key $KEY
```

7.2 用Hardhat部署

```javascript
// deploy.js
const { ethers, upgrades } = require("hardhat");

async function main() {
    // 部署
    const OpenPerpCore = await ethers.getContractFactory("OpenPerpCore");
    const proxy = await upgrades.deployProxy(OpenPerpCore, [feeModule], {
        kind: "uups",
        initializer: "initialize",
    });
    await proxy.deployed();
    console.log("Proxy deployed to:", proxy.address);

    // 升级
    const OpenPerpCoreV2 = await ethers.getContractFactory("OpenPerpCoreV2");
    const upgraded = await upgrades.upgradeProxy(proxy.address, OpenPerpCoreV2);
    console.log("Upgraded to:", upgraded.address);
}
```

---

八、今日总结

8.1 核心结论

1. 2026年可升级合约已成为DeFi标配，但代理漏洞也成了OWASP TOP 10第10类
2. OpenZeppelin推荐从Transparent转向UUPS，因为更轻量、更灵活
3. OpenPerp应该用"混合架构"：Core不可变+Modules可升级，平衡安全和灵活性
4. 所有升级必须经过multisig+timelock+链上投票，不能用单个EOA

8.2 关键代码片段

```solidity
// 1. 锁定Implementation的初始化
constructor() {
    _disableInitializers();
}

// 2. 用UUPS模式
contract MyContract is UUPSUpgradeable, OwnableUpgradeable {
    function _authorizeUpgrade(address) internal override onlyOwner {}
}

// 3. Core不可变（彻底锁定升级）
// Core不继承UUPSUpgradeable，或者加_disableInitializers()
```

---

````
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->




学习笔记

日期：2026年7月22日 主题：钱包产品设计——新人避坑逻辑拆解与跨场景迁移 核心任务：从新人钱包的痛点出发，拆解避坑设计的底层逻辑，迁移到OpenPerp（永续DEX）的新人引导与安全设计中

* * *

一、2026年新人钱包的生存现状

（数据支撑）根据Beosin、Dune、Chainalysis 2026年上半年报告：

1.  90%的新Web3用户在钱包创建阶段弃用，未完成首次交易
    
2.  70%的钱包用户仅完成一次交易后流失（Dune Wallet Report v2）
    
3.  2026年上半年Web3诈骗总额120亿美元，其中60%来自DeFi钱包层面的操作失误
    
4.  地址投毒（复制粘贴错误地址）导致的损失达32亿美元，同比增长170%
    
5.  无限授权导致的钱包被盗损失72亿美元，其中85%是新人误操作
    

核心矛盾：新人的所有安全风险，本质是「把需要专业认知的安全决策，交给了不懂的用户」，而不是「用户蠢」

* * *

二、新人的三大核心场景与对应的坑

按用户生命周期，拆解每个场景的真实痛点与对应诈骗/失误案例：

（一）创建钱包阶段

1.  核心坑：假冒钱包/钓鱼下载
    
    -   真实案例：2026年第二季度，Google搜索「MetaMask下载」Top3结果中，有2个是假冒钱包，偷取助记词后盗走总价值1.2亿美元的资产
        
    -   新人误操作逻辑：直接搜关键词→点前几个链接→输入助记词→资产被盗，全程无任何验证环节
        
2.  次要坑：助记词丢失/泄露
    
    -   真实数据：68%的新人把助记词存到手机相册（易被黑客通过相册漏洞窃取），12%直接发聊天软件给朋友（易被社交工程学窃取）
        

（二）首次交易阶段

1.  核心坑：无限授权
    
    -   真实案例：2026年1月，1名新人首次在Uniswap交易，默认同意了DAI的无限授权，3个月后该合约被攻击，新人损失120万美元
        
    -   新人误操作逻辑：看不懂「无限授权=允许合约扣你所有DAI」→ 直接点确认→ 后续被盗
        
2.  次要坑：地址投毒
    
    -   真实案例：2026年3月，1名新人给朋友转ETH，从历史记录复制了攻击者伪装的地址（仅中间字符不同，肉眼难识别），损失18万美元
        
3.  次要坑：Gas费看不懂
    
    -   真实数据：67%的新人因看不懂Gas费，要么放弃交易，要么付了过高的Gas（2026年平均新人多付3倍Gas费）
        

（三）日常使用阶段

1.  核心坑：私钥泄露
    
    -   真实案例：2026年6月，SecondFi钱包（原Yoroi）因私钥生成漏洞被盗2000万美元，178个新人钱包受影响
        
2.  次要坑：诈骗诱导
    
    -   真实案例：2026年上半年，AI生成的钓鱼链接（伪装成「官方空投」「钱包升级」）导致新人损失35亿美元，同比增长1400%
        

* * *

三、钱包避坑的底层设计逻辑

所有有效的避坑设计，核心是「强制降低用户的决策成本，把专业安全逻辑埋到产品流程里，而不是靠用户学习」，具体分三类设计：

（一）前置式拦截设计：在风险发生前，直接阻止用户操作

1.  假冒钱包拦截：
    
    -   设计逻辑：钱包启动时，自动校验下载源的域名签名（比如MetaMask仅允许从metamask.io的签名包启动），如果是假冒版本，直接弹窗报错并关闭
        
    -   落地案例：Zengo钱包2026年上线该功能后，假冒版本的盗币事件降为0
        
2.  地址验证拦截：
    
    -   设计逻辑：用户输入/粘贴地址时，自动匹配地址前4位+后4位的「指纹」，如果是地址投毒场景（和历史地址仅中间不同），直接弹窗红色警告并禁止转账
        
    -   落地数据：Trust Wallet上线该功能后，地址投毒导致的新人损失下降82%
        

（二）降级式授权设计：把高风险权限，默认降级为低风险

1.  无限授权降级：
    
    -   设计逻辑：所有DeFi应用请求授权时，钱包默认显示「仅授权本次需要的金额」（比如转100USDC就授权100USDC），而不是默认无限，用户可以手动选择「无限授权」（但需要额外弹窗确认风险）
        
    -   落地案例：Coinbase Wallet 2026年改了授权逻辑后，新人无限授权的比例从85%降到17%
        
2.  权限分级显示：
    
    -   设计逻辑：把授权的权限分成「可扣小额」「可扣大额」「可全扣」三个等级，用红黄绿颜色标注，新人默认只能选择「可扣小额」
        

（三）教育式引导设计：在操作时，用大白话讲清风险，而不是给专业术语

1.  助记词引导：
    
    -   设计逻辑：创建钱包后，强制3次助记词验证（第1次显示，第2次隐藏后让用户填写，第3次要求用户手动朗读助记词），并且自动提示「不要存相册、不要发聊天软件」
        
    -   落地数据：MetaMask 2026年升级该引导后，助记词泄露的新人比例下降63%
        
2.  Gas费引导：
    
    -   设计逻辑：把Gas费翻译成大白话「本次交易的手续费约等于3元人民币，如果你急着交易，可以加钱提速」，而不是显示「Gas Price: 20 gwei」
        

* * *

四、设计逻辑迁移：对应到OpenPerp（永续DEX）的新人引导

因为OpenPerp的用户大概率是Web3新人（想玩永续但不知道怎么用Uniswap/GMX的人），所以钱包的避坑逻辑完全可以迁移，核心是「把永续的专业风险，埋到产品流程里，不让用户做专业决策」：

（一）创建/连接钱包阶段（对应钱包的创建阶段）

1.  风险点：新人连了假冒的OpenPerp钓鱼网站
    
    -   迁移设计：OpenPerp前端自动校验域名的SSL证书+链上合约的地址（只允许连接已部署的官方合约地址），如果是钓鱼网站，直接弹窗「非官方地址，请从官网进入」
        
2.  风险点：新人连了钱包但不知道授权是什么
    
    -   迁移设计：钱包连接后，弹出大白话提示「我们不会扣你的钱，只是需要授权你可以用钱包里的USDC开单」，并且把授权金额默认降级为「本次开单的金额」（比如开100USDC的单，就授权100USDC），不默认无限
        

（二）首次开单阶段（对应钱包的首次交易阶段）

1.  风险点：新人不懂杠杆，开了10倍杠杆后爆仓，损失全部本金
    
    -   迁移设计：
        
        -   默认杠杆为1倍（无杠杆），用户要开高杠杆时，必须手动拖动滑块（而不是直接选「10倍」），并且每升高1倍杠杆，弹出风险提示「开2倍杠杆=价格跌5%就爆仓，损失100%本金」
            
        -   首次开单前，强制完成1分钟的「杠杆风险测试」（3道选择题，比如「10倍杠杆下，ETH跌10%你会损失多少？」），答错就不能开高杠杆
            
2.  风险点：新人不知道永续的资金费率，开单后被资金费率扣钱
    
    -   迁移设计：开单界面直接显示「本次开单预计每8小时扣X元资金费率」，用人民币标注，而不是显示「Funding Rate: 0.01%」
        
3.  风险点：新人不知道清算线，开单后被清算
    
    -   迁移设计：开单界面直接显示「你的清算价格是X元，距离当前价格还有Y元」，并且用颜色标注（比如距离10%以上是绿色，5%以下是红色）
        

（三）日常交易阶段（对应钱包的日常使用阶段）

1.  风险点：新人收到钓鱼链接，诱导开假仓
    
    -   迁移设计：OpenPerp前端自动拦截非官方的跳转链接，并且在用户点击陌生链接时，弹窗「这不是OpenPerp的官方链接，请勿输入私钥/助记词」
        
2.  风险点：新人不知道自己的仓位被清算，没有及时补保证金
    
    -   迁移设计：仓位接近清算线时（比如距离5%），自动弹出红色警告，并且发送邮件/短信提醒（需要用户绑定手机号）
        

* * *

五、今日顿悟

1.  新人的安全问题，本质是「产品设计的懒政」，不是「用户的蠢」——把专业的安全决策丢给不懂的用户，是产品方的责任
    
2.  避坑设计的核心是「强制降低决策成本」，而不是「教育用户」——因为90%的新人不会看教育内容，但100%的新人会跟着产品流程走
    
3.  钱包和DEX的安全逻辑是通的：都是「把高风险权限降级」「前置拦截风险」「大白话讲清风险」，而不是靠用户学习专业知识
    

* * *

六、待解决问题

1.  OpenPerp的新人引导，哪些是必须强制的（比如杠杆风险测试），哪些是可选的（比如资金费率的详细说明）？
    
2.  无限授权的降级设计，会不会影响老用户的使用体验（比如老用户不想每次都重新授权）？
    
3.  怎么实现「地址投毒拦截」？需要和链上的地址指纹库（比如Etherscan的标签）集成吗？
    
4.  新人的手机号绑定，会不会有隐私问题？有没有其他提醒方式（比如钱包内的弹窗）？
    

* * *

七、明日计划

1.  画OpenPerp的新人引导流程图：从连接钱包到首次开单，每个环节的风险提示和强制拦截点
    
2.  写「杠杆风险测试」的3道选择题，确保能筛出不懂杠杆的新人
    
3.  查Etherscan的地址标签API，看能不能集成到OpenPerp的前端，实现地址投毒拦截
    
4.  对比GMX和Hyperliquid的新人引导，看哪些是我们可以抄的，哪些是我们要优化的
    

* * *

八、参考资料

1.  Beosin 2026年上半年Web3安全报告：[https://beosin.com](https://beosin.com)
    
2.  Dune Wallet Report v2：[https://ndlabs.dev/crypto-wallet-user-retention](https://ndlabs.dev/crypto-wallet-user-retention)
    
3.  Chainalysis 2026年诈骗报告：[https://chainalysis.com](https://chainalysis.com)
    
4.  Trust Wallet 2026年安全升级说明：[https://trustwallet.com](https://trustwallet.com)
    
5.  Coinbase Wallet授权逻辑升级：[https://coinbase.com/wallet](https://coinbase.com/wallet)
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->





````markdown
合约安全学习笔记

日期：2026年7月21日
主题：基于OWASP Top 10 2026的智能合约安全体系
学习目标：系统掌握2025-2026年DeFi攻击态势，针对OpenPerp项目识别安全风险点，建立安全开发思维

---

一、安全格局总览（2025-2026）

1.1 攻击趋势变化
根据Chain Labs和OpenZepton 2025年度报告，以及OWASP 2026排名变化：

1）重入攻击从第2位降到第8位：不是消失了，是工具成熟了（ReentrancyGuard、静态分析工具能检测），但跨合约重入和只读重入仍有发生（2026 Solv Protocol ERC-3525事件$270万）

2）私钥泄露成为头号杀手：2026年Chain报告显示，私钥泄露（含社会工程学攻击）占所有加密盗窃的88%，远高于2024年的55%。代表事件：Bybit冷钱包$14亿（2025.2）、Drift Protocol $2.85亿（2026.4）

3）业务逻辑漏洞跃升第2位：代码没错但逻辑错，代表事件：Cetus协议溢出漏洞$2.23亿（2025.5）、Balancer V2精度损失$1.28亿（2025.11）、Aave滑点问题$5000万（2026.3）

4）闪电贷从第7位升到第4位：闪电贷本身不是漏洞，是放大器，让原本不可行的攻击（需要大量资金做市操纵）变得零成本。2026年所有预言机操纵攻击都配合了闪电贷

5）跨链桥攻击成重灾区：2026 Kelp DAO $2.92亿（最大DeFi事件），不是代码漏洞，是LayerZero DVN验证节点被投毒

1.2 2025-2026年十大安全事件（按损失金额）

| 事件 | 金额 | 类型 | 根因 |
|------|------|------|------|
| Bybit冷钱包被盗 | $14亿 | 私钥泄露 | 钱包供应商JS植入恶意代码，签名界面被篡改 |
| Kelp DAO跨链桥 | $2.92亿 | 基础设施投毒 | LayerZero验证节点被替换，伪造跨链消息 |
| Drift Protocol | $2.85亿 | 私钥泄露 | 9天社会工程学攻击，骗取多签签名 |
| Cetus协议 | $2.23亿 | 整数溢出 | u256运算溢出，闪电贷放大 |
| Mango Markets | $1.17亿 | 预言机操纵 | 闪电贷操纵单一价格源 |
| Balancer V2 | $1.28亿 | 精度损失 | 1 wei精度差异被利用 |
| GMX V1 | $4200万 | 重入攻击 | 2025.7 executeDecreaseOrder函数重入 |
| Uniswap V4 Hook | $34万 | Hook漏洞 | 2026.3 无限铸造LP Token |
| Aave滑点 | $5000万 | 业务逻辑 | 2026.3 闪电贷+预言机组合攻击 |
| Solv Protocol | $270万 | 只读重入 | ERC-3525证券化代币重入 |

核心教训：70%以上的攻击不是代码bug，而是"业务逻辑+闪电贷+预言机"的组合拳，或者是私钥管理失误

---

二、OWASP Smart Contract Top 10 2026 详解

按排名从高到低，结合代码示例和真实案例

2.1 SC01 访问控制漏洞（Access Control）
排名第1（和2025持平）

漏洞类型：
- 缺失权限检查（少了一个onlyOwner，攻击者就能mint代币）
- 使用tx.origin验证身份（可以通过中间合约绕过）
- 代理合约初始化未锁定（攻击者抢跑设置恶意参数）
- 管理员权限过大（单一地址能做任何事）

代码示例（错误写法）：

```solidity
// 危险：没有权限检查，任何人都能调用
function withdrawAll() external {
    payable(msg.sender).transfer(address(this).balance);
}

// 危险：使用tx.origin验证，可被绕过
function isOwner() public view returns (bool) {
    return tx.origin == owner;  // 如果owner是普通用户，攻击者可以用恶意合约诱导
}

// 安全写法
function withdrawAll() external onlyOwner {
    payable(msg.sender).transfer(address(this).balance);
}

// 安全：使用msg.sender
function isOwner() public view returns (bool) {
    return msg.sender == owner;
}
```

2026年代表案例：Kinto Protocol $155万（2025.7），ERC1967代理未初始化，攻击者设置恶意owner

防御方案：
- 必用OpenZeppelin Ownable/AccessControl
- 代理合约部署后立即调用initialize并锁定
- 敏感操作用多签+时间锁（Timelock）
- 权限最小化：每个角色只给必要的权限

2.2 SC02 业务逻辑漏洞（Business Logic）
排名第2（从第3上升）

定义：代码按预期运行，但逻辑设计有缺陷，在极端条件或组合攻击下导致损失

常见类型：
- 价格计算错误（舍入误差、精度丢失）
- 抵押率设计不合理
- 提现优先级错误（先转账后扣余额）
- 合约状态边界条件处理不当
- 闪电贷+业务逻辑的组合攻击

代码示例（舍入误差）：

```solidity
// 危险：精度丢失导致资金损失
// Balancer V2事件，攻击者存入极小金额，反复提取获利
function getBptFromAmountOut(uint256 amountOut) external view returns (uint256) {
    // 1 wei差异被利用
    return (amountOut * totalSupply) / getPoolValue();
}

// 安全：向下取整保护协议
function getBptFromAmountOut(uint256 amountOut) external view returns (uint256) {
    uint256 bpt = (amountOut * totalSupply) / getPoolValue();
    require(bpt > 0, "insufficient output");
    return bpt - 1;  // 向下取整，最后1 wei留给协议
}
```

2026年代表案例：Aave滑点$5000万（2026.3），闪电贷操纵预言机，业务逻辑未考虑极端情况

防御方案：
- 先定义不变量（协议永远不应该破的规则），再写代码
- 用属性测试（Property Testing）验证不变量
- 模拟所有极端场景（价格暴跌、大额闪电贷、多协议组合）
- 业务逻辑和代码分开审查，找懂DeFi经济模型的人review

2.3 SC03 预言机操纵（Price Oracle）
排名第3（从第2下降）

漏洞原理：攻击者用闪电贷大量买卖，操纵池价格，预言机被操纵的价格欺骗，导致错误计算

攻击流程：
1. 从Aave借巨额闪电贷
2. 用借贷资金在目标池大量买入，抬高价格
3. 被操纵的价格被预言机（链上TWAP或单次价格）读取
4. 攻击者用虚高的价格抵押，借出更多价值的资产
5. 卖回资产，价格恢复
6. 偿还闪电贷+利息，剩余为利润

代码示例（危险的预言机使用）：

```solidity
// 危险：直接读取单次价格，可被操纵
function getETHPrice() public view returns (uint256) {
    return IUniswapV3Pool(pool).slot0().sqrtPriceX96;
}

// 安全：使用TWAP（时间加权平均价格）
function getETHPriceTWAP() public view returns (uint256) {
    uint32[] memory secondsAgos = new uint32[](2);
    secondsAgos[0] = 3600;  // 1小时前
    secondsAgos[1] = 0;    // 现在
    (int56[] memory tickCumulatives, ) = IUniswapV3Pool(pool).observe(secondsAgos);
    // 计算TWAP价格
    return calculatePriceFromTickCumulatives(tickCumulatives);
}

// 更安全：使用Chainlink TWAP
function getPriceSafe() public view returns (uint256) {
    uint256 price = getETHPriceTWAP();
    uint256 chainlinkPrice = getChainlinkPrice();
    // 两个预言机的差异不超过5%
    require(abs(price - chainlinkPrice) * 100 / chainlinkPrice <= 5, "oracle divergence");
    return price;
}
```

2026年代表案例：Jupiter $4400万（2025.10）、Kelp DAO $2.92亿（跨链桥验证节点被投毒，本质也是预言机类问题）

防御方案：
- **不用单次价格**，必用TWAP（时间窗口至少30分钟）
- **多预言机组合**（Chainlink + Uniswap TWAP + Pyth）
- **设置偏离阈值**：两个预言机差异超过5%时暂停协议
- **余额检查**：闪电贷后检查池子余额变化，超过阈值拒绝交易
- 预言机更新频率不能太高（给攻击者操纵窗口），也不能太低（数据过时）

2.4 SC04 闪电贷攻击（Flash Loan）
排名第4（从第7大幅上升）

重要认识：闪电贷不是漏洞，是**放大器**，让原本需要数亿资金才能实施的攻击变成零成本

放大的漏洞类型：
- 预言机操纵（最常见）
- 价格计算精度问题
- 治理投票操纵
- 重入攻击的资金支持
- 舍入误差

攻击四阶段：
1. 借钱：从Aave/Dyod借闪电贷（零抵押，需在同一tx内偿还）
2. 操纵：用借来的钱操纵目标池子价格或投票
3. 获利：利用被操纵的价格/投票，借出资产或获利
4. 还钱：偿还闪电贷+手续费，剩余为利润

代码示例（闪电贷攻击模式）：

```solidity
contract Attacker {
    function exploit(address victim, address pool, uint256 amount) external {
        // 阶段1：借闪电贷
        IAavePool(aavePool).flashLoanSimple(address(this), asset, amount, "");
        
        // 后续在executeOperation中实现
    }
    
    function executeOperation(
        address asset,
        uint256 amount,
        uint256 premium,
        address initiator,
        bytes calldata params
    ) external returns (bool) {
        // 阶段2：操纵价格
        IERC20(asset).approve(uniswap, amount);
        IUniswapRouter(uniswap).swapExactTokensForTokens(
            amount, 0, path, address(this), block.timestamp
        );
        
        // 阶段3：用虚高价格抵押借款
        uint256 borrowed = IVictim(victim).borrow(maxBorrowable);
        
        // 阶段4：卖回资产，偿还闪电贷
        IERC20(weth).approve(uniswap, borrowed);
        // 卖回...
        
        // 偿还闪电贷
        uint256 repayAmount = amount + premium;
        IERC20(asset).approve(aavePool, repayAmount);
        return true;
    }
}
```

防御方案（闪电贷本身无法禁止，只能让被放大的漏洞不存在）：
- 预言机必用TWAP，时间窗口够长
- 价格计算正确处理精度
- 治理投票设置时间锁
- 敏感操作前检查池子余额变化
- 闪电贷后、还款前，检查所有关键状态是否一致

2.5 SC05 输入验证不足
排名第5（从第4下降）

漏洞类型：
- 参数范围未检查（amount=0、deadline过期）
- 数组长度不匹配
- 地址零值检查
- 类型转换溢出

代码示例：

```solidity
// 危险：未检查参数
function createOrder(address token, uint256 amount) external {
    // 如果token是address(0)或amount=0，可能出问题
    orders[token] += amount;
}

// 安全：完整验证
function createOrder(address token, uint256 amount, uint256 deadline) external {
    require(token != address(0), "zero address");
    require(amount > 0, "zero amount");
    require(deadline >= block.timestamp, "expired");
    require(orders[token] + amount <= MAX_ORDER, "exceed limit");
    orders[token] += amount;
}
```

防御方案：所有public函数的参数必须用require检查，边界条件写完整

2.6 SC06 未检查的外部调用
排名第6（和2025持平）

漏洞类型：
- 调用外部合约后未检查返回值
- 调用可能失败但合约未处理
- 调用恶意合约导致重入

代码示例：

```solidity
// 危险：未检查转账是否成功
function transferFunds(address recipient, uint256 amount) external {
    recipient.transfer(amount);
    // 如果recipient合约没有fallback函数，transfer会失败
    // 但函数继续执行，导致状态不一致
    emit Transfer(recipient, amount);
}

// 安全：检查返回值或用低级调用
function transferFundsSafe(address recipient, uint256 amount) external {
    (bool success, ) = recipient.call{value: amount}("");
    require(success, "transfer failed");
    emit Transfer(recipient, amount);
}

// 危险：未检查ERC20返回值
function swap(address token, uint256 amount) external {
    IERC20(token).transferFrom(msg.sender, address(this), amount);
    // USDT早期版本不返回bool，transferFrom会静默失败
}

// 安全：用SafeTransferLib或显式检查
function swapSafe(address token, uint256 amount) external {
    require(IERC20(token).transferFrom(msg.sender, address(this), amount), "transfer failed");
}
```

2.7 SC07 算术错误（舍入与精度）
排名第7（2026新分类，从整数错误中拆分）

2025-2026年多起攻击根因：
- Balancer V2 $1.28亿：1 wei精度差异
- Cetus $2.23亿：u256溢出
- Aave滑点$5000万：舍入误差

Solodity 0.8+内置溢出检查，但舍入和精度问题仍存在：

```solidity
// 舍入方向对协议利益的影响
function calculateShares(uint256 assets) public view returns (uint256) {
    // 方案A：向上取整（对用户有利，协议亏损）
    return (assets * totalSupply + 1) / totalSupply;
    
    // 方案B：向下取整（对协议有利，用户少得）
    return (assets * totalSupply) / totalSupply;
    
    // 方案C：精确计算（不丢失精度）
    return FixedPointMathLib.mulDiv(assets, totalSupply, getPoolValue());
}
```

防御方案：
- 用Solidity 0.8+（内置溢出检查）
- 用OpenZeppelin SafeCast处理类型转换
- 精度敏感运算用FixedPointMathLib或ABDKMath64x64
- 每处算术运算都要想清楚舍入方向对谁有利

2.8 SC08 重入攻击
排名第8（从第2大幅下降）

虽然工具成熟，但**跨合约重入**和**只读重入**仍需注意

经典重入攻击：

```solidity
// 危险：先转账后更新状态
function withdraw() external {
    uint256 amount = balances[msg.sender];
    payable(msg.sender).transfer(amount);  // 先转账
    balances[msg.sender] = 0;               // 后更新
}
// 攻击者在fallback函数中再次调用withdraw，反复提现
```

跨合约重入（2026年新趋势）：

```solidity
// 协议A调用协议B，协议B回调协议A，形成跨合约重入
contract ProtocolA {
    function deposit() external {
        IERC20(token).transferFrom(msg.sender, address(this), amount);
        IB(protocolB).receiveDeposit(msg.sender, amount);  // B可能回调A
        userDeposits[msg.sender] += amount;
    }
}

contract ProtocolB {
    function receiveDeposit(address user, uint256 amount) external {
        // B在接收时回调A的其他函数
        IA(protocolA).someFunction(user);  // A的someFunction可能依赖deposit已完成
    }
}
```

只读重入（最隐蔽）：

```solidity
// 危险：view函数读取的状态可能被回调修改
// 攻击者在重入时修改价格，而view函数返回的是旧价格
function getPrice() external view returns (uint256) {
    return prices[token];  // 如果prices在之前的调用中被缓存，这里可能过时
}
```

防御方案：
- 必用CEI模式（Checks-Effects-Interactions）：先检查，再改状态，最后外部调用
- 加ReentrancyGuard（OpenZeppelin提供）
- 对跨合约调用，假设对方会恶意回调，所有依赖状态在调用前已读取并缓存
- 只读重入无法用ReentrancyGuard防御，只能在设计上避免

```solidity
// CEI模式正确写法
function withdrawSafe() external nonReentrant {
    // 1. Checks：检查条件
    uint256 amount = balances[msg.sender];
    require(amount > 0, "no balance");
    
    // 2. Effects：先更新状态
    balances[msg.sender] = 0;
    emit Withdraw(msg.sender, amount);
    
    // 3. Interactions：最后外部调用
    payable(msg.sender).call{value: amount}("");
}
```

2.9 SC09 整数溢出
排名第9（从第8下降）

Solodity 0.8+内置检查，但unchecked块和低级操作仍可能出问题：

```solidity
// Solodity 0.8+ 默认有溢出检查
function add(uint256 a, uint256 b) public pure returns (uint256) {
    return a + b;  // 溢出自动revert
}

// 但unchecked块仍可能出问题
function mulUnchecked(uint256 a, uint256 b) public pure returns (uint256) {
    unchecked {
        return a * b;  // 溢出不检查
    }
}
```

2025 Cetus $2.23亿事件就是u256溢出，配合闪电贷让攻击有利可图

防御方案：用Solidity 0.8+，非必要不用unchecked

2.10 SC10 代理与升级漏洞
排名第10（2026新分类）

常见类型：
- 代理未初始化：攻击者抢跑设置恶意owner和逻辑合约
- 逻辑合约未锁定：升级可以被劫持
- delegatecall到恶意地址
- 升级前未检查新逻辑的存储布局兼容性

代码示例：

```solidity
// 危险：代理未锁定
contract MyProxy {
    address implementation;
    address admin;
    
    function upgradeTo(address newImpl) external {
        require(msg.sender == admin);
        implementation = newImpl;
    }
    // 没有时间锁，管理员可以一秒升级到恶意合约
}

// 安全：带时间锁和多签
contract MyProxyV2 {
    address implementation;
    address[] admins;  // 多签
    TimelockController timelock;
    
    function scheduleUpgrade(address newImpl) external {
        require(isAdmin(msg.sender));
        timelock.schedule(newImpl, 3 days);  // 3天时间锁
    }
    
    function executeUpgrade() external {
        require(timelock.isReady());
        implementation = timelock.getScheduledImpl();
    }
}
```

2026年代表案例：Kinto Protocol $155万（2025.7），ERC1967代理未初始化

防御方案：
- 用OpenZeppelin UUPS或Transparent Proxy标准
- 代理部署后立即initialize并锁定
- 升级操作必须经过多签+时间锁
- 存储布局用OpenZeppelin StorageSlot保证兼容性

---

三、针对OpenPerp项目的安全重点

3.1 高风险攻击面识别

OpenPerp = Monad L1 + Uniswap V4 Hook + Perpetual DEX

| 攻击向量 | 风险等级 | 根因 | 对应OWASP条目 |
|----------|---------|------|--------------|
| V4 Hook重入 | 极高 | Hook在swap前调用，可能被重入利用 | SC08 重入 |
| 预言机操纵 | 极高 | Perp依赖价格做清算，操纵=虚假清算 | SC03 预言机 |
| 闪电贷放大 | 极高 | 配合预言机或价格计算漏洞 | SC04 闪电贷 |
| 清算逻辑错误 | 高 | 舍入误差、优先级错误 | SC02/SC07 |
| 跨合约重入 | 高 | 多Hook组合调用时的状态竞争 | SC08 |
| Monad并行执行风险 | 高 | 并行交易可能导致状态竞争（需要研究Monad特性） | SC02 |
| 管理员权限 | 高 | 紧急暂停、参数修改的权限 | SC01 |
| 价格计算精度 | 中 | 开仓/平仓金额的舍入 | SC07 |

3.2 必做安全措施

第一类：编码阶段必须遵守
- 所有外部调用后检查返回值
- 所有算术用Solidity 0.8+（内置溢出检查）
- 价格计算用FixedPointMathLib，明确舍入方向
- V4 Hook中：swap前所有状态已确认，swap后再更新
- Perp清算：先冻结仓位，再计算价格，最后执行清算
- 紧急暂停：多签+时间锁

第二类：V4 Hook专属安全
- V4 Hook的10个节点（before/after swap、add/remove liquidity等）每个都要考虑重入
- beforeSwap中不能修改会影响pool价格的状态
- afterSwap中处理完所有逻辑再return
- Hook调用失败时pool应回滚整个swap（测试这个行为）
- 参考Uniswap官方Hook安全指南

第三类：Monad L1专属安全
- Monad并行执行时，同一合约的不同storage slot可能被同时读写
- 研究Monad交易执行模型，确保关键状态更新不会并发冲突
- 可以用Monad提供的锁原语（如果有的话）保护关键路径
- 在Monad测试网模拟高并发场景（1000笔同时清算）

第四类：部署前必做
- 至少2家安全审计（如Trail of Bits + OpenZeppelin）
- Foundry模糊测试（Fuzzing）
- 不变量测试（Invariant Testing）
- 闪电贷模拟（用Aave flashLoan测试所有流程）
- 预言机TWAP验证（模拟价格操纵场景）
- Bug Bounty Program（至少$100k起步）
- 主网上线前在测试网跑3个月压力测试

---

四、安全开发流程

参考OpenZepton Secure Smart Contract Development Roadmap

4.1 规划阶段（Plan）
- 写威胁模型（Threat Model）：列出所有可能攻击
- 定义安全不变量：协议永远不应该破的规则
- 设计权限体系：每个角色的最小权限

4.2 编码阶段（Code）
- 用经过审计的库（OpenZeppelin Contracts）
- 代码清晰，注释每个函数的安全假设
- 用静态分析工具：Slither、Foundry Fuzz
- CEI模式强制执行

4.3 测试阶段（Test）
- 功能测试：所有正常场景
- 安全测试：边界条件、极端值、组合调用
- 闪电贷测试：模拟所有流程在闪电贷中执行
- 形式化验证：关键函数用Certora或K Framework证明正确性
- Fork测试：在主网Fork上模拟真实攻击

4.4 审计阶段（Audit）
- 至少2家独立审计
- 审计前做好准备：代码文档清晰，测试覆盖全面
- 审计后：所有Critical/High问题必须修复，Medium问题要有处理方案

4.5 部署与升级（Deploy）
- 主网前在测试网至少跑3个月
- 升级用多签+时间锁
- 每次升级前做Rollback计划（如果升级有问题怎么回退）

4.6 监控（Monitor）
- 实时监控：预言机价格偏离、大额转账、异常swap
- 报警：Forta、OpenZepton Defender
- 紧急暂停开关（Pausable）
- 定期安全审查

---

五、安全工具清单

| 工具 | 用途 | 免费/付费 |
|------|------|----------|
| Slither | 静态分析，检测常见漏洞 | 免费 |
| Foundry | 测试框架（含模糊测试） | 免费 |
| Mythril | 符号执行，检测边界条件 | 免费 |
| Certora Prover | 形式化验证 | 付费 |
| Scribble | 不变量测试 | 免费 |
| Echidna | 属性测试 | 免费 |
| OpenZepton Defender | 链上监控、紧急操作 | 有免费版 |
| Forta | 实时监控、报警 | 有免费版 |
| Tenderly | 交易模拟、调试 | 有免费版 |

---

六、今日总结

6.1 关键认知
- 2026年攻击趋势已变：私钥泄露>业务逻辑>闪电贷放大，重入不再是头号威胁
- 70%以上的攻击是组合拳（闪电贷+预言机+业务逻辑），不是单一漏洞
- OpenPerp的高风险点：V4 Hook安全、Monad并行执行特性、清算逻辑的精度

6.2 立即行动项
- 读Uniswap官方Hook安全指南
- 研究Monad L1并行执行模型的安全影响
- 设计OpenPerp的清算逻辑和Hook交互流程（先不写代码，先画流程图，标记每个可能被攻击的点）
- 看Certora验证Aave V3的报告（理解专业审计怎么做的）

6.3 下一步学习
- 闪电贷攻击代码实操：用Foundry复现一个2025年的闪电贷攻击
- Monad并行执行安全：写简单合约测试并行状态更新的行为
- V4 Hook安全审计实践：审计一个简单的Hook实现

---

参考资料
1. OWASP Smart Contract Top 10 2026：https://scs.owasp.org/sctop10
2. OpenZepton 2025 Rewind：https://openzepton.com/news/web3-security-auditors-2025-rewind
3. Zealynx闪电贷深度分析：https://zealynx.io/blogs
4. OWASP Top 10 2026解读：https://zealynx.io/research/industry/owasp-2026
5. Kelp DAO事件分析：https://openzepton.com/news/lessons-from-kelpdao-hack
6. OpenZepton安全开发路线图：https://openzepton.com/readiness-guide
7. Uniswap V4 Hook安全：https://docs.uniswap.org/contracts/v4/security

笔记完
````
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->






````markdown
# 今日学习笔记

**日期**: 2026年7月20日
**主题**: DeFi三大支柱深度研究 + 个人项目构思
**核心成果**: 产出3份研究文档（Uniswap V4 Reading Card、Morpho Market Brief、Perpetual DEX Research Brief）

---

## 今日学习目标

1. 掌握DeFi三大支柱（DEX、借贷、衍生品）的前沿协议机制
2. 学会「Reading Card」和「Market Brief」的专业分析框架
3. 将研究成果落地为可执行的项目方案
4. 建立「研究→洞察→行动」的思维闭环

---

## 今日核心产出（3份文档）

### 文档1：Uniswap V4 Reading Card

**研究方法**: 5W1H框架 + 技术深度拆解

**核心洞察**:
```
V3的痛点 -> V4的解决方案

V3问题              V4解法（技术实现）
每个池子一个合约    Singleton（PoolManager统一管理）
固定4档费率        Hooks（8个节点插入自定义逻辑）
WETH包装费         原生ETH支持（Currency抽象）
跨池Gas爆炸        Flash Accounting（仅结算净额）
```

**最震撼的数字**:
- 多跳交易Gas降低 **99.5%**（440,000 -> 72,000）
- Hooks数量突破 **41,000**（2026.7）
- V4累计交易量 **$4,220亿**

**我的判断**: V4不是升级，是范式转换——把DEX从「固定规则的交易场所」变成「可编程的流动性基础设施」。Hooks就是DeFi的「App Store」。

---

### 文档2：Morpho Blue Market Brief

**研究方法**: 事实-判断分离 + 反方观点验证

**核心洞察**:
```
Aave vs Morpho：不是竞争，是垂直分工

Aave = 链上银行（共享池、治理慢、服务散户）
Morpho = 链上信用基础设施（隔离池、无许可、服务机构）

关键事件：2026.4 Kelp DAO攻击
- Aave损失：$78亿（共享池被污染）
- Morpho损失：$0（完全隔离）
- Morpho获得：$8亿新资金（机构用脚投票）
```

**最有价值的发现**:
- Morpho Blue核心代码仅 **650行**（Aave V4约10,000行）-> 极简=安全
- 机构10分钟部署借贷池（Aave需要30天DAO投票）-> 快决策=竞争力
- **$1.75亿融资**（2026.6）-> 机构背书的滚雪球效应

**我的判断**: Morpho的成功证明了「机构的核心痛点是**风险隔离**，不是收益率」。这和我之前想的不一样——我以为机构要高APY，其实他们要的是「我的钱不会因为别人的代币暴雷而损失」。

---

### 文档3：OpenPerp项目研究简报

**研究方法**: 用户痛点验证 + 竞品差异化 + 风险评估

**核心洞察**:
```
Hyperliquid的成功 = 性能（200K TPS）
Hyperliquid的软肋 = 闭源 + 无自定义

我的机会 = 补全Hyperliquid的软肋
- 开源（100%可审计）
- 动态费率（V4 Hooks实现）
- 隔离清算池（Morpho模式）
- 目标用户：对闭源有顾虑的高频量化团队
```

**最关键的假设验证**:
| 假设 | 验证状态 | 证据来源 |
|------|---------|---------|
| 68% Perp DEX用户想要开源 | ✓ | Dune Analytics 2026.6调查 |
| 54%用户想要动态费率 | ✓ | Dune Analytics 2026.6调查 |
| Monad TPS >= 10,000 | 待验证 | 测试网10K，生产可能3-5K |

**我的判断**: 不要做「另一个Hyperliquid」，要做「补全Hyperliquid不做的事」——开源、动态费率、隔离清算。

---

## 今日学到的DeFi核心概念

### 1. AMM vs CLOB：两种做市机制的取舍

| 维度 | AMM（自动做市商） | CLOB（订单簿） |
|------|------------------|---------------|
| 代表 | Uniswap, GMX | Hyperliquid, dYdX |
| 原理 | x*y=k公式定价 | 买卖双方挂单撮合 |
| 优点 | 无需对手方，永远有流动性 | 支持限价单，价格精确 |
| 缺点 | 滑点大，无法精确挂单 | 需要足够深度才能成交 |
| 适用 | 小额、高频、长尾币 | 大额、机构、主流币 |

**我的理解**：Hyperliquid用CLOB击败了GMX的AMM，证明了Perp场景用户更看重「精确价格」和「限价单」，而不是「永远有流动性」。

### 2. 隔离池 vs 共享池：安全与效率的权衡

```
共享池（Aave）：所有资产在一个池
- 优点：流动性深，资金效率高
- 缺点：一个资产暴雷，全部受影响（Kelp DAO事件损失$78亿）

隔离池（Morpho）：每个资产独立池
- 优点：风险隔离，一个暴雷不影响其他
- 缺点：流动性分散，资金效率低
```

**我的理解**：机构选择隔离池，是因为「不亏」比「赚多」更重要。我应该在Perp场景也用隔离清算池设计。

### 3. 可编程流动性：DeFi的下一个范式

```
V1-V3：固定规则的协议
- 只能选预设的费率、预设的曲线

V4/Hooks：可编程的协议
- 可以自定义费率（波动率感知）、自定义逻辑（JIT流动性）
```

**类比**：这就像从「功能手机」变成「智能手机」——原来只能用预设功能，现在可以装APP扩展功能。

### 4. 极简架构的安全优势

```
Morpho Blue：650行核心代码
- 审计简单（2人2周完成）
- Bug少（代码量与Bug数正相关）
- 可升级（通过添加Layer而非修改核心）

Aave V4：~10,000行核心代码
- 审计复杂（需要4人1个月）
- Bug多（历史上多次被黑）
- 升级困难（修改核心风险高）
```

**我的理解**：我的项目应该追求「核心层极简，功能层可插拔」。

---

## 三大支柱的关联发现

### 发现1：DeFi的模块化趋势不可逆
```
Uniswap V4 -> 把DEX拆成Pool + Hooks
Morpho Blue -> 把借贷拆成Pool + Curator
我的项目 -> 把Perp拆成Pool + Clearing + Hook

共同点：核心层极简（650-1000行），功能层可插拔
```

### 发现2：机构需求正在重塑DeFi
```
2025年：机构还在观望
2026年：机构已经下场
- Coinbase用Morpho做$20亿借贷
- Robinhood用Morpho服务2500万用户
- Hyperliquid被CFTC审查（合规压力）

结论：下一个牛市的驱动力是机构，不是散户
```

### 发现3：安全是DeFi的第一优先级
```
2026.3 Uniswap V4 Hook漏洞 -> $34万损失
2026.4 Kelp DAO攻击 -> Aave损失$78亿
2026.7 Hyperliquid宕机 -> 量化团队$230万损失

我的项目安全设计：
- Monad L1本身安全（单故障域）
- Morpho式隔离清算池（一个爆不影响全局）
- V4 Hook审计（Trail of Bits + Spearbit）
- $1M Bug Bounty
```

---

## 今日顿悟

### 顿悟1：「Reading Card」是最有效的深度研究方法
之前我写研究文档总是散文化，今天发现Reading Card的结构化方法（核心机制、关键数据、风险）能帮我快速抓住重点。以后研究任何协议都应该用这个框架。

### 顿悟2：事实-判断分离是专业研究的基本功
今天在Morpho Brief里刻意区分「事实」和「判断」，这让我的论点更有说服力。比如我说「机构的核心痛点是风险隔离」，但我有事实支撑（Kelp DAO事件的TVL变化），而不是凭感觉。

### 顿悟3：研究不是目的，项目才是目的
今天研究三个协议，最终落地到OpenPerp的项目构思。这很重要——如果研究不能指导行动，那就是浪费时间。我以后的学习都应该围绕「解决我项目的某个问题」来展开。

### 顿悟4：DeFi的未来在L2和模块化
Hyperliquid证明了自建L1的威力（200K TPS），但V4和Morpho证明了模块化的可扩展性。我的项目选择Monad L1 + V4 Hooks，正好踩在这两个趋势的交叉点上。

---

## 待解决问题

| # | 问题 | 我的初步想法 | 优先级 |
|---|------|-------------|-------|
| 1 | Monad生产环境TPS到底有多少？ | 找Monad Discord问团队 | P0 |
| 2 | V4 Hooks在Perp场景有没有先例？ | 搜GitHub和Twitter | P0 |
| 3 | 怎么联系到愿意合作的量化团队？ | 先做Demo，再发邮件 | P1 |
| 4 | OpenPerp的MVP应该包含哪些功能？ | 核心清算+动态费率+基础UI | P1 |
| 5 | 怎么证明「隔离清算池」比单一池安全？ | 做模拟攻击测试 | P2 |

---

## 明日计划

| 优先级 | 任务 | 目标 | 判断逻辑 | 预期产出 |
|--------|------|------|---------|---------|
| P0 | 写Monad TPS验证报告 | 确认技术可行性 | 如果实际TPS < 3000，考虑改用Arbitrum或Base | 1页测试结果 |
| P0 | 搜V4 Hook Perp先例 | 确认差异化空间 | 如果无先例 -> 差异化真实存在（好消息）；如果有先例 -> 分析其优缺点 | 研究笔记 |
| P1 | 画OpenPerp架构图 | 理清模块关系 | 模块化设计（核心层+功能层），参考Morpho和V4 | 架构图 |
| P1 | 写MVP功能清单 | 明确开发范围 | 只做核心：订单簿+清算+动态费率，UI用最简单的 | 清单文档 |
| P2 | 搜Monad生态基金申请要求 | 准备申请材料 | $1M基金，要求有可运行的Prototype | 申请指南 |

---

## DeFi学习路径（补漏计划）

根据今天的研究，我发现以下知识还需要补充：

### 紧急补漏（影响MVP开发）
1. **清算引擎设计**：研究Aave和Morpho的清算逻辑，学习如何实现快速清算
2. **预言机集成**：研究Chainlink和Pyth，学习如何在Monad上集成预言机
3. **智能合约安全**：学习重入攻击、闪电贷攻击的原理和防御方法

### 进阶学习（影响项目竞争力）
4. **MEV保护**：研究如何防止三明治攻击、抢跑
5. **动态费率实现**：学习如何用V4 Hooks实现波动率感知的费率
6. **Quant交易基础**：了解量化交易的需求，设计API接口

### 资料来源
- Monad文档：https://docs.monad.xyz
- Uniswap V4开发者文档：https://docs.uniswap.org/contracts/v4/overview
- Chainlink预言机：https://docs.chain.link
- OpenZeppelin安全库：https://docs.openzeppelin.com

---

## 今日产出文件

| 文件 | 核心内容 | 价值 |
|------|---------|------|
| Uniswap_V4_Reading_Card.md | V4机制深度拆解 | 理解Hooks、Singleton架构 |
| Morpho_Market_Brief.md | Morpho机构客群分析 | 找到差异化：风险隔离 |
| Perpetual_Dex_Research_Brief.md | OpenPerp项目构思 | 从研究到行动 |

---

## 一句话总结

> 不要为了研究而研究，要为了做项目而研究——每一份Reading Card都应该回答「这对我的项目有什么用」。

---

## 相关链接

- Uniswap V4文档: https://docs.uniswap.org/contracts/v4
- Morpho文档: https://docs.morpho.org
- Monad网站: https://monad.xyz
- Hyperliquid: https://hyperliquid.xyz

````
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->







````markdown
# 📚 今日学习笔记

**日期**: 2026年7月19日  
**主题**: 从零到一完成一场 Web3 入门活动的全流程设计  
**核心成果**: 产出 5 份活动策划文档（2周工作量）

---

## 🎯 今日学习目标

1. 理解一场线上活动从「想法」到「落地」的完整路径
2. 掌握活动运营的 4 大核心模块：定位、内容、执行、传播
3. 学会用「用户心理」视角设计活动，而非「流程视角」
4. 探索 AI 工具在运营工作中的边界：能做什么、不能做什么

---

## 📋 今日核心内容（5 大模块）

### 模块 1: 活动定位（为什么做这场活动）

#### 关键概念

| 概念 | 解释 | 我的应用 |
|------|------|---------|
| **差异化定位** | 在红海中找到蓝海 | 别人教 Web3，我教「用 AI 学 Web3」 |
| **用户分层** | 不同用户有不同痛点 | 把 Web3 新手拆成 4 类，每类单独设计 CTA |
| **数据驱动** | 用数据而非感觉做决策 | 68% 开发者认 AI 降门槛 → 把 AI 作为核心卖点 |

#### 我的 3 个定位选项

```
❌ 选项 1: Web3 入门讲座（红海，无差异化）
❌ 选项 2: AI 编程工具分享（太泛，不聚焦）
✅ 选项 3: AI × Web3 入门实战（蓝海，有明确用户群）
```

#### 关键决策

> **核心判断**: 用户的「决策焦虑」>「能力焦虑」。他们不是学不会，而是怕学不会。

---

### 模块 2: 活动内容设计（90 分钟怎么排）

#### 黄金时间分配

```
10-20-35-15-10 法则
├─ 10min: 破冰（建立信任）
├─ 20min: 认知（消除恐惧）
├─ 35min: 实操（核心价值）
├─ 15min: 讨论（深度连接）
└─ 10min: 收尾（留存转化）
```

#### 2 个关键设计技巧

**技巧 1: 5 分钟「同步时间」**
- 不是让观众「等」，是给他们「安全垫」
- 利用心理学：人在有「退路」时，更愿意尝试

**技巧 2: 3 层 CTA 递进**
```
Layer 1（低门槛）→ 发合约地址（10 秒完成）
Layer 2（中门槛）→ 扫码加群（1 分钟完成）
Layer 3（高价值）→ 领学习路线图（长期留存）
```

#### 我的设计公式

```
好的活动 = 认知负荷 × 决策焦虑 × 时间成本
         ↓ 我要做的 ↓
         最小化这三个变量
```

---

### 模块 3: 活动执行（怎么落地）

#### 4 周倒排期

| 周次 | 阶段 | 关键任务 | 交付物 |
|------|------|---------|--------|
| Week 4 | 立项 | 定主题、邀嘉宾、选平台 | 需求文档 |
| Week 3 | 筹备 | 做 PPT、写代码、测工具 | 完整素材包 |
| Week 2 | 宣传 | 设计海报、投放渠道 | 曝光量 1000+ |
| Week 1 | 彩排 | 全流程走 2 遍 | 彩排录像 |

#### 团队角色分工

```
核心团队（6人）
├─ 项目负责人（1人）→ 管进度、做决策
├─ 主持人（1人）→ 控场、串场、CTA
├─ 技术嘉宾（2人）→ 演示、答疑
├─ 技术支持（1人）→ 保障设备、处理技术问题
└─ 运营（1人）→ 宣传、报名、社群
```

#### 风险预案（Top 3）

| 风险 | 概率 | 预案 |
|------|------|------|
| 断网 | 中 | 手机热点备用 |
| 演示失败 | 中 | 预录屏兜底，自嘲化解 |
| 冷场 | 中 | 准备 3 个「托」问题 |

---

### 模块 4: 活动传播（怎么让人来）

#### 3 段式宣传节奏

```
阶段 1: 蓄水期（活动前 10-7 天）
└─ 动作: 社群种草、嘉宾转发、朋友圈预热
└─ 内容: 悬念海报（只露主题，不露细节）

阶段 2: 爆发期（活动前 5-3 天）
└─ 动作: 公众号发文、社群轰炸、老用户召回
└─ 内容: 完整海报 + 嘉宾介绍

阶段 3: 提醒期（活动前 2-0 天）
└─ 动作: 倒计时、私信提醒、实时通知
└─ 内容: 简单直接的「来就对了」
```

#### 转化率漏斗设计

```
曝光 100%
  ↓ 30-40%（钩子: 前 30 名送 AI 月卡）
报名 30-40%
  ↓ 40-50%（钩子: 3 次提醒 + 准备清单）
到场 12-20%
  ↓ 60-70%（钩子: 5 分钟同步时间）
完成实操 7-14%
  ↓ 80-90%（钩子: 发合约地址）
加群 6-13%
```

#### 文案写作 5 原则

| 原则 | ✅ 正确示例 | ❌ 错误示例 |
|------|-----------|-----------|
| 用户视角 | "90 分钟让你写出合约" | "我们教授智能合约开发" |
| 具体数字 | "前 30 名送月卡" | "报名有好礼" |
| 降低门槛 | "不会 Solidity 没关系" | "需要一定编程基础" |
| 制造稀缺 | "限 50 人，报满即止" | "欢迎大家参加" |
| 展示真实 | "这是上届的合约地址" | "活动很有价值" |

---

### 模块 5: AI 工具应用

#### AI vs. 我 的分工

| 环节 | AI 做什么 | 我做什么 |
|------|----------|---------|
| 主题选则 | 给 3 个选项 | 选最有差异化的 1 个 |
| 受众定义 | 给笼统画像 | 拆成 4 类精准用户 |
| 议程设计 | 给 3 个时间分配 | 选最适合用户心理的 1 个 |
| 文案撰写 | 生成初稿 | 改写成「人话」 |
| 风险预案 | 列 15 个风险 | 筛掉 10 个低概率的 |

#### 我的 AI 使用心得

**AI 擅长**: 速度、数量、结构化  
**AI 不擅长**: 判断、取舍、温度

```
AI 是「加速器」，不是「替代品」
  ↓ 正确用法 ↓
先让 AI 给 5 个选项 → 我选最好的 1 个 → 我再打磨 10%
```

#### 反 AI 感修改示例

```
【AI 原文】
"本次活动将带您了解智能合约开发的基本流程，
包括 Solidity 语法基础、Remix IDE 使用、
以及如何部署到以太坊测试网。"

【我改后】
"90 分钟，你会亲手写一个真的智能合约。
不用学 Solidity——AI 帮你；
不用装软件——浏览器就能用；
不用怕失败——有完整教程兜底。
你只需要一台能上网的电脑。"
```

---

## 💡 今日顿悟

### 顿悟 1: 运营的本质是「降低用户的决策成本」

以前以为运营是「搞活动」「发文案」「拉社群」——今天才明白，运营的核心是帮用户做一个他们本来不敢做的决定。

### 顿悟 2: 用户的恐惧比能力更致命

60% 报名 Web3 课程的人会放弃。不是因为学不会，是因为怕学不会。「不会 Solidity 没关系」这句话的力量，比「我们教 Solidity 入门」大 10 倍。

### 顿悟 3: 5 分钟「停顿」比 5 分钟「讲解」更有价值

在 90 分钟的活动里，我特意留了 10 分钟给用户「同步」。这 10 分钟不是浪费，是给用户「安全垫」——跟不上可以先录，不会焦虑。

### 顿悟 4: 3 层 CTA > 1 层 CTA

直接说「扫码加群」，转化率只有 5%。但如果说「先发合约地址 → 再扫码加群 → 最后领路线图」，转化率能到 20%。用户需要「台阶」，不是「跳高」。

---

## ❓ 待解决问题

| # | 问题 | 我的想法 |
|---|------|---------|
| 1 | 怎么找到第一批种子用户？ | 联系 Web3 高校社团，合作举办「校园专场」 |
| 2 | AI 到底能帮我节省多少时间？ | 下周做一个时间对比实验 |
| 3 | 用户说的「感兴趣」和「真报名」差在哪？ | 做一个 5 问小调研，找到真阻碍 |
| 4 | 怎么设计活动后的「留钩子」？ | 在群里每天发一个 15 分钟小任务 |

---

## 📅 明日计划

| 优先级 | 任务 | 目标 |
|--------|------|------|
| P0 | 写「报名落地页」文案 | 让用户 3 秒内决定报名 |
| P0 | 设计「社群冷启动」方案 | 活动前就有 50 人在群里聊 |
| P1 | 画一张「用户旅程图」 | 把用户从看到海报到留存的每一步画出来 |
| P1 | 做一个「AI vs. 人工」时间对比实验 | 量化 AI 的价值 |
| P2 | 写一份「3 天活动复盘模板」 | 活动后可以快速输出复盘 |

---

## 📁 今日产出文件

| 文件 | 用途 | 核心内容 |
|------|------|---------|
| [OPS_Case_Study.md](OPS_Case_Study.md) | 作品集 | 完整的 2 周复盘 |
| [Space活动策划案.md](Space活动策划案.md) | 内部对齐 | 活动定位、用户、价值 |
| [活动内容流程设计.md](活动内容流程设计.md) | 嘉宾沟通 | 90 分钟议程 |
| [活动执行预案.md](活动执行预案.md) | 项目管理 | 排期、分工、预案 |
| [活动运营物料包.md](活动运营物料包.md) | 直接使用 | 文案、模板、话术 |
| [学习笔记_20260719.md](学习笔记_20260719.md) | 今日笔记 | 就是这份 |

---

## 🎯 一句话总结

好的活动运营，不是让用户「参加一场活动」，而是让用户「做出一个决定」——决定去尝试，决定去坚持，决定留在这个社群里。
````
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->








````markdown
# 📚 学习笔记 - 2026年7月18日

**主题**: Web3 + AI 去中心化投票 DApp 开发  
**项目链接**: https://github.com/AGF-DOT/aaa

---

## 🎯 今日目标

1. 修复项目构建和运行问题
2. 完善 AI 功能集成
3. 生成项目文档
4. 上传代码到 GitHub

---

## 📝 学习内容

### 1. 问题排查与修复

#### 1.1 依赖缺失问题

**问题**: `Cannot find module 'baseline-browser-mapping'`

**原因**: node_modules 损坏或不完整

**解决方案**:
```bash
# 删除所有依赖
rm -rf node_modules package-lock.json

# 重新安装
npm install
```

**知识点**:
- `npm install` 会根据 `package.json` 重新安装所有依赖
- `package-lock.json` 锁定依赖版本，确保一致性
- 如果遇到奇怪的模块错误，重新安装通常能解决

---

#### 1.2 React DOM 渲染错误

**错误信息**:
```
Runtime NotFoundError
Failed to execute 'insertBefore' on 'Node': 
The node before which the new node is to be inserted is not a child of this node.
```

**原因**: 使用 Fragment (`<>...</>`) 进行条件渲染时，React 在状态切换过程中无法正确识别 DOM 节点。

**解决方案**:
```jsx
// ❌ 错误写法 - 没有 key 的 Fragment
{isAnalyzing ? (
  <>
    <Loader2 className="w-4 h-4 mr-2 animate-spin" />
    分析中...
  </>
) : (
  <>
    <Bot className="w-4 h-4 mr-2" />
    开始分析
  </>
)}

// ✅ 正确写法 - 有 key 的真实元素
{isAnalyzing ? (
  <span key="analyzing" className="flex items-center">
    <Loader2 className="w-4 h-4 mr-2 animate-spin" />
    分析中...
  </span>
) : (
  <span key="idle" className="flex items-center">
    <Bot className="w-4 h-4 mr-2" />
    开始分析
  </span>
)}
```

**知识点**:
- React 的 `key` 属性帮助识别元素的唯一性
- Fragment (`<>...</>`) 没有真实的 DOM 节点，可能导致渲染问题
- 条件渲染使用不同的 `key` 可以确保 React 正确更新 DOM
- 状态切换时，使用真实元素（如 `div`、`span`）比 Fragment 更稳定

---

### 2. AI 功能架构设计

#### 2.1 三层架构

```
┌─────────────────────────────────────────────────┐
│  UI Layer (AIAnalyzer.jsx)                       │
│  - 用户交互                                      │
│  - 展示结果                                      │
└─────────────────────┬───────────────────────────┘
                      │ useAIProposal()
┌─────────────────────▼───────────────────────────┐
│  Hook Layer (useAIProposal.js)                   │
│  - 状态管理                                      │
│  - 错误处理                                      │
│  - 加载状态                                      │
└─────────────────────┬───────────────────────────┘
                      │ aiService.analyze()
┌─────────────────────▼───────────────────────────┐
│  Service Layer (aiService.js)                    │
│  - API 调用                                      │
│  - Mock 数据生成                                 │
│  - 响应解析                                      │
└─────────────────────┬───────────────────────────┘
                      │
        ┌─────────────┴─────────────┐
        ▼                           ▼
┌───────────────┐           ┌───────────────┐
│  OpenAI API    │           │  Mock Data    │
│  (真实调用)    │           │  (备用方案)    │
└───────────────┘           └───────────────┘
```

#### 2.2 Mock 模式的重要性

**为什么需要 Mock 模式？**
1. **开发阶段** - 不需要真实 API Key 就能开发 UI
2. **演示目的** - 给面试官/评委展示时可以正常运行
3. **降级方案** - API 不可用时系统仍能工作

**Mock 数据设计**:
```javascript
const mockResponse = {
  summary: "100字以内的提案摘要",
  keyPoints: ["要点1", "要点2", "要点3"],
  risks: ["风险1", "风险2"],
  benefits: ["收益1", "收益2"],
  recommendation: "support",  // support | oppose | abstain
  confidence: 0.78,           // 置信度 0-1
  explanation: "推荐理由"
};
```

---

### 3. Monad 网络配置

#### 3.1 网络参数

| 参数 | 测试网 | 主网 |
|------|--------|------|
| **Chain ID** | 10143 | 143 |
| **RPC URL** | `https://testnet-rpc.monad.xyz` | `https://rpc.monad.xyz` |
| **货币** | MON | MON |
| **区块浏览器** | https://testnet.monadscan.com | https://monadscan.com |

#### 3.2 Hardhat 配置

```javascript
// hardhat.config.js
networks: {
  monadTestnet: {
    url: "https://testnet-rpc.monad.xyz",
    accounts: [process.env.PRIVATE_KEY],
    chainId: 10143,
  },
  monadMainnet: {
    url: "https://rpc.monad.xyz",
    accounts: [process.env.PRIVATE_KEY],
    chainId: 143,
  }
}
```

#### 3.3 前端配置

```javascript
// src/lib/constants.js
export const CHAIN_IDS = {
  MONAD_TESTNET: 10143,
  MONAD_MAINNET: 143,
};

// src/lib/web3/provider.js
const chains = [
  {
    id: 10143,
    name: 'Monad Testnet',
    rpcUrls: ['https://testnet-rpc.monad.xyz'],
    nativeCurrency: { name: 'MON', symbol: 'MON', decimals: 18 },
  }
];
```

---

### 4. Git 操作与项目上传

#### 4.1 完整的 Git 流程

```bash
# 1. 检查状态
git status

# 2. 添加文件
git add .

# 3. 提交
git commit -m "feat: 添加新功能"

# 4. 添加远程仓库（首次）
git remote add origin https://github.com/username/repo.git

# 5. 设置主分支
git branch -M main

# 6. 推送
git push -u origin main
```

#### 4.2 .gitignore 配置

```gitignore
# 依赖
node_modules/
.pnp

# 构建产物
.next/
out/
build/

# 环境变量
.env
.env.local
.env.*.local

# 系统文件
.DS_Store
*.log

# IDE
.vscode/
.idea/

# Hardhat
cache/
artifacts/
```

#### 4.3 常见问题

**问题**: 子目录有独立的 .git 仓库

**解决**:
```bash
# 移除子仓库的 .git
rm -rf subdirectory/.git

# 从主仓库移除缓存
git rm --cached subdirectory

# 重新添加
git add subdirectory
```

---

### 5. Next.js 16 新特性

#### 5.1 Turbopack

- Next.js 16 默认使用 Turbopack 替代 Webpack
- 更快的编译速度
- 但可能存在兼容性问题

#### 5.2 React Compiler

- Next.js 16 实验性支持 React Compiler
- 自动优化 React 组件
- 如果遇到问题可以禁用：

```javascript
// next.config.mjs
const nextConfig = {
  experimental: {
    reactCompiler: false,  // 禁用
  }
};
```

#### 5.3 常见警告处理

**警告**: `Slow filesystem detected`

**解决**:
- 将项目移到本地 SSD
- 排除 .next 目录的杀毒软件扫描

---

## 🛠️ 工具与命令速查

### Git 命令

```bash
git status                    # 查看状态
git add .                      # 添加所有文件
git commit -m "message"       # 提交
git push                       # 推送
git pull                       # 拉取
git log --oneline              # 查看历史
```

### npm 命令

```bash
npm install                    # 安装依赖
npm run dev                    # 开发模式
npm run build                  # 生产构建
npm run start                  # 启动生产服务器
npm run lint                   # 代码检查
```

### Hardhat 命令

```bash
npx hardhat compile            # 编译合约
npx hardhat test               # 运行测试
npx hardhat node               # 启动本地节点
npx hardhat run script.js      # 运行脚本
npx hardhat ignition deploy    # 部署合约
```

---

## 📚 推荐学习资源

### 官方文档

1. **Next.js 16**: https://nextjs.org/docs
2. **React 19**: https://react.dev/reference/react
3. **OpenAI API**: https://platform.openai.com/docs/api-reference
4. **Monad**: https://docs.monad.xyz
5. **Wagmi**: https://wagmi.sh
6. **Viem**: https://viem.sh

### 实践项目

- 本项目: https://github.com/AGF-DOT/aaa

---

## 🎓 今日收获

### 技术技能

| 技能 | 掌握程度 | 说明 |
|------|----------|------|
| React 条件渲染 | ⭐⭐⭐⭐⭐ | 理解了 key 和 Fragment 的正确用法 |
| Next.js 16 | ⭐⭐⭐⭐ | 掌握了 Turbopack 和新特性 |
| AI 集成 | ⭐⭐⭐⭐ | 学会了 Mock 模式设计 |
| Web3 开发 | ⭐⭐⭐⭐ | 熟悉了 Monad 网络配置 |
| Git 工作流 | ⭐⭐⭐⭐⭐ | 掌握了完整的上传流程 |

### 项目经验

1. **错误处理很重要** - 遇到错误不要慌，先看错误信息
2. **Mock 模式是必备的** - 开发阶段没有真实服务时很有用
3. **文档要同步更新** - README 要包含所有新功能
4. **版本控制要规范** - 每次重要更新都要 commit

---

## 📅 明日计划

1. [ ] 测试完整的 AI 功能流程
2. [ ] 部署智能合约到 Monad 测试网
3. [ ] 截图记录项目成果
4. [ ] 准备黑客松提交材料
5. [ ] 写简历项目描述

---

## 💡 思考与总结

### 关于 Web3 + AI

今天的开发让我深刻体会到：

1. **结合点** - Web3 提供去中心化的信任，AI 提供智能化的分析
2. **用户价值** - 降低普通用户参与链上治理的门槛
3. **技术挑战** - 需要同时掌握区块链和 AI 两个领域

### 关于项目开发

1. **文档先行** - 好的文档能帮助自己和他人理解项目
2. **小步快跑** - 每次只做一件事，做完再做下一件
3. **持续集成** - 代码要及时上传，避免丢失

### 关于学习方法

1. **边做边学** - 实际项目比单纯看文档更有效
2. **记录笔记** - 好记性不如烂笔头
3. **复盘总结** - 每天结束前回顾当日收获

````
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-17
<!-- DAILY_CHECKIN_2026-07-17_START -->









# 今日学习笔记：ERC721 非同质化代币标准

学习日期：2026-07-17

学习主题：ERC721 原理、标准接口、特性、漏洞、与 ERC20 对比、开源贡献拓展思路

## 一、基础概念

### 1\. ERC721 定义

ERC721（Ethereum Request for Comments 721）是以太坊**非同质化代币（NFT）** 标准。

-   核心：每一枚代币拥有唯一 `tokenId`，代币之间不可互换、不可拆分。
    
-   对比 ERC20：ERC20 是同质化，1 枚代币等价任意另一枚；ERC721 每一个 Token 独一无二。
    

### 2\. 适用场景

数字藏品、头像 NFT、游戏道具、链上凭证、域名、资产确权、门票等。

## 二、ERC721 核心四大标准函数

合约必须实现以下基础接口，是钱包、市场识别 NFT 的统一规范：

1.  **balanceOf(address owner)**
    
    查询某地址持有多少个 NFT 藏品，返回持有数量。
    
2.  **ownerOf(uint256 tokenId)**
    
    输入唯一 tokenId，返回该 NFT 当前持有者地址，核心确权函数。
    
3.  **transferFrom(address from, address to, uint256 tokenId)**
    
    直接转移指定 tokenId 的 NFT，需要当前持有者授权。
    
4.  **approve(address to, uint256 tokenId)**
    
    授权第三方地址可以操作某一个指定 tokenId。
    
5.  **setApprovalForAll(address operator, bool approved)**
    
    批量授权：允许第三方操作你名下全部 NFT（NFT 市场挂单必备）。
    
6.  **getApproved(uint256 tokenId)**
    
    查询单个 NFT 授权给了哪个地址。
    
7.  **isApprovedForAll(address owner, address operator)**
    
    查询是否拥有全部 NFT 批量授权权限。
    

## 三、可选拓展元数据接口 ERC721Metadata

几乎所有 NFT 项目都会实现，用于展示图片、名称、描述：

1.  `name()`：集合名称
    
2.  `symbol()`：集合简称
    
3.  `tokenURI(uint256 tokenId)`
    
    返回该 NFT 的资源链接（JSON 元数据，包含图片、属性、文案）
    
    元数据 JSON 示例：
    

json

```
{
  "name": "NFT #1",
  "image": "ipfs://xxx",
  "description": "测试藏品",
  "attributes": [{"trait_type":"颜色","value":"蓝色"}]
}
```

存储方式：中心化服务器、IPFS（去中心化首选）。

## 四、ERC721 关键特性

1.  **唯一性**
    
    每个 tokenId 独立，所有权分开记录，不存在合并、拆分。
    
2.  **不可分割**
    
    不能转账 0.5 个 NFT，最小单位是完整 1 枚。
    
3.  **独立确权**
    
    每个 NFT 单独记录持有者，可单独授权、单独售卖。
    
4.  **批量授权机制**
    
    `setApprovalForAll` 一键授权全部藏品，Opensea 等交易市场依赖该接口挂单。
    
5.  元数据解耦
    
    链上只存 tokenId 与所有权，图片、属性存链下，降低 Gas 成本。
    

## 五、ERC721 vs ERC20 核心对比

表格

| 维度 | ERC20（同质化代币） | ERC721（NFT） |
| --- | --- | --- |
| 代币属性 | 可互换、可拆分 | 唯一、不可拆分 |
| 标识 | 无唯一 ID，仅余额 | 每个 tokenId 独立标识 |
| 授权逻辑 | approve 授权额度 | 单 NFT 授权 / 全量批量授权 |
| 转账单位 | 小数精度（18 位） | 只能完整单个转移 |
| 典型用途 | 稳定币、治理币 | 数字藏品、游戏道具 |

## 六、ERC721 常见安全漏洞（可用于修复开源 Demo，做 GitHub 贡献）

1.  **批量授权风险 setApprovalForAll**
    
    一键授权全部 NFT 给市场合约，若合约被盗，名下所有 NFT 会被转走；用完需关闭授权。
    
2.  **重入攻击**
    
    `transferFrom` 转账时触发接收者 fallback，存在重入窃取 NFT 风险，OpenZeppelin 标准库已内置防护。
    
3.  **未校验 tokenId 合法性**
    
    mint、transfer 时未判断 tokenId 是否已铸造，造成重复铸造、转移不存在的 NFT。
    
4.  **元数据中心化风险**
    
    tokenURI 使用 http 中心化链接，项目方关停服务器后 NFT 图片无法加载；最佳实践 IPFS。
    
5.  **mint 无权限控制**
    
    铸造函数缺少 onlyOwner，任何人可无限铸造 NFT，破坏藏品稀缺性。
    
6.  **接收者未实现 ERC721Receiver**
    
    合约地址接收 NFT 时未实现`onERC721Received`，NFT 会永久锁死在合约内无法转出。
    

## 七、配套拓展标准

1.  **ERC721A**：Azuki 优化版，批量铸造大幅降低 Gas，主流 NFT 项目使用；
    
2.  **ERC1155**：混合标准，同时支持同质化代币 + 多份 NFT，适合游戏道具；
    
3.  **ERC2981**：NFT 二级交易版税标准，每次二级市场成交自动给创作者分润。
    

## 八、今日学习总结

1.  分清 ERC20 同质化与 ERC721 非同质化的底层差异，掌握 7 个核心交互函数；
    
2.  理解 NFT 元数据作用与 IPFS 去中心化存储优势；
    
3.  梳理 NFT 高频安全风险，看懂批量授权、合约锁 NFT 等典型漏洞；
<!-- DAILY_CHECKIN_2026-07-17_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->










# 今日学习笔记：智能合约常见安全漏洞汇总

学习日期：2026-07-16

学习主题：Solidity/ERC20 智能合约各类漏洞原理、危害与修复方案

学习目的：掌握合约安全风险，用于修复开源 Demo Bug，完成 GitHub 开源贡献 PoW

## 一、高危资金类漏洞（直接造成资产被盗）

### 1\. 重入攻击 Reentrancy

1.  原理：合约向外部地址转账 ETH 时，外部合约接收 ETH 会触发 fallback/receive 函数，可递归回调当前取款逻辑，在余额未更新前重复提取资金。
    
2.  错误代码特征：先转账、再修改账户余额（违背 CEI 规范）
    
3.  ERC20 场景风险：质押、提款、兑换类合约极易中招
    
4.  修复方案
    
    -   遵循 CEI 模式：先修改状态变量，再执行外部转账交互
        
    -   引入 OpenZeppelin `ReentrancyGuard` 防重入修饰器
        

### 2\. 整数溢出 / 下溢 Overflow & Underflow

1.  原理：uint 无负数边界，数值超出范围会循环归零 / 超大值；Solidity 0.8.0 以下无内置检查。
    
2.  危害：mint 无限增发代币、转账绕过余额校验、销毁负数代币。
    
3.  修复
    
    -   0.8 及以上版本自带溢出检查；低版本引入 SafeMath 库
        
    -   使用 OpenZeppelin 标准化 ERC20，内置安全数学运算
        

### 3\. ERC20 授权安全漏洞

1.  无限授权风险：用户授权最大值`type(uint256).max`，第三方合约被盗后会清空用户全部代币。
    
2.  重复授权漏洞：未先 approve (0) 直接修改授权额度，部分旧合约额度叠加或报错。
    
3.  修复
    
    -   提供 increaseAllowance、decreaseAllowance 安全增减授权接口
        
    -   资产操作完成后及时清零授权额度
        

### 4\. ETH 转账 Gas 限制漏洞

1.  问题：transfer () /send () 仅传递 2300gas，外部合约含复杂逻辑会转账失败，资金卡死。
    
2.  修复：统一使用`call{value: amount}("")`转账，并校验返回值 success。
    

### 5\. 外部调用不校验返回值

调用合约、转账后不判断 success，转账失败依旧判定操作成功，造成账务错乱。

修复：必须接收返回值并 require 校验。

## 二、权限控制类漏洞（管理员权限丢失、任意操作）

### 1\. 缺少管理员权限校验

mint、burn、pause、升级等核心函数未加 onlyOwner，任意地址可调用增发、销毁代币。

修复：基于 OpenZeppelin Ownable，函数添加 onlyOwner 修饰器。

### 2\. 老式构造函数漏洞

0.4.22 以前构造函数与合约同名，命名写错会变为公开普通函数，任何人可接管合约所有权。

修复：统一使用 constructor 关键字定义构造函数。

### 3\. 可升级合约未初始化

代理合约部署后未调用 initialize 初始化函数，任意用户可调用初始化抢走管理员权限。

修复：初始化函数添加 onlyInitializing 修饰，部署后立即执行初始化。

## 三、外部交互与链上特性漏洞

### 1\. 不安全随机数（可预测）

依赖 block.timestamp、blockhash 生成抽奖、盲盒随机数，矿工可操纵区块数据提前预判结果。

修复：使用 Chainlink VRF 链下预言机获取加密安全随机数。

### 2\. 抢跑攻击 Front-running

DEX 交易、NFT 发售、代币兑换场景，攻击者监听内存池，抬高 Gas 抢先执行交易，套利或抢购资产。

缓解方案：时间锁、提交披露机制、批量拍卖。

### 3\. 短地址攻击

转账地址长度不足 20 字节，后端计算转账金额时发生移位，合约扣除远超用户输入数量的代币。

修复：前端与合约双重校验地址字节长度严格等于 20。

### 4\. selfdestruct 自毁与资金锁死

1.  合约存在 selfdestruct，攻击者可强制向合约转入 ETH，破坏余额计算逻辑；
    
2.  取款逻辑缺失、转账条件错误，合约内 ETH 永久锁死无法提取。
    

## 四、代理 / 可升级合约专属漏洞

存储布局冲突：升级合约时新增变量插槽错乱，覆盖原有余额、管理员等关键数据，代币数据清零。

修复：遵循 OpenZeppelin 可升级标准，严格隔离存储插槽，不随意调整变量顺序。

## 五、业务逻辑缺陷（适合开源贡献：修复 Demo、完善示例）

不属于底层语法漏洞，但大量开源 ERC20 Demo 存在此类缺陷，可作为第 5 类开源贡献提交 PR 修复：

1.  transferFrom 调用前未校验用户授权额度 allowance，执行直接报错；
    
2.  销毁函数\_burn 未校验调用者余额，出现下溢风险；
    
3.  部署脚本硬编码地址、链 ID，无法适配多测试网；
    
4.  缺少暂停 pause 紧急机制，合约出现漏洞无法止损；
    
5.  缺少 allowance 查询示例、完整 approve+transferFrom 交互流程；
    
6.  合约无注释、部署步骤缺失，新手无法正常运行 Demo。
    

## 六、今日学习总结

1.  合约漏洞分为资产安全、权限、链交互、升级架构、业务 Demo 缺陷五大类；
    
2.  重入、溢出、权限缺失是线上合约最常见高危漏洞，开发 ERC20 必须使用 OpenZeppelin 标准库规避；
    
3.  大量开源 Solidity Demo 存在逻辑残缺、代码不规范问题，修复这类 Bug、完善示例代码，可制作真实可验证的 GitHub 开源贡献 Proof of Work；
    
4.  后续实操计划：寻找开源 ERC20 Hardhat 示例仓库，修复一处 Demo 逻辑漏洞，提交 PR 完成开源贡献。
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->











**\# 今日学习笔记**

**日期**: 2026年7月15日  

**主题**: Moss Framework Demo 完善实践  

**关键词**: TypeScript ESM、NodeNext、Chalk、CLI 参数解析、错误处理、Moss Simulator

\---

**\## 学习目标**

理解 Moss Framework 的 `discover → load → action → simulate` 核心流程，通过完善 Demo 提升代码质量和用户体验。

\---

**\## 问题与解决**

**\### 问题 1: Chalk 导入错误**

**现象**:

\`\`\`

SyntaxError: The requested module 'chalk' does not provide an export named 'chalk'

\`\`\`

**原因**:

Chalk v5 使用 ESM 模块，默认导出是 `default`，而非命名导出。

**解决方案**:

\`\`\`typescript

// 错误

import { chalk } from "chalk";

// 正确

import chalk from "chalk";

\`\`\`

**反思**:

使用第三方库前应查看其导出方式，特别是 ESM 模块的默认导出与命名导出区别。

\---

**\### 问题 2: TypeScript TS2835 错误**

**现象**:

\`\`\`

TS2835: Relative import paths need explicit file extensions

\`\`\`

**原因**:

项目使用 `NodeNext` 模块解析策略，要求所有相对导入必须包含文件扩展名（.js）。

**解决方案**:

\`\`\`typescript

// 错误

import { log } from "./utils/logger";

// 正确

import { log } from "./utils/logger.js";

\`\`\`

**反思**:

\- NodeNext 模式下，TypeScript 编译后的文件会保留 `.js` 扩展名

\- 运行时 Node.js 需要确切的文件扩展名才能找到模块

\- 这是 TypeScript 向 ESM 标准靠拢的重要变化

\---

**\### 问题 3: 零金额导致的模拟错误**

**现象**:

\`\`\`

invalid parameter "amount": amount must be positive

\`\`\`

**原因**:

当 swap 输出金额过小时，计算 `half = minTokenOut / 2` 可能得到 0，导致 Plan B 构建失败。

**解决方案**:

\`\`\`typescript

if (parseFloat(half) <= 0) {

  log.warning("Output amount too small for Plan B, skipping...");

  // 只模拟 Plan A

} else {

  // 模拟 Plan A 和 Plan B

}

\`\`\`

**反思**:

\- 金额计算需要考虑边界情况（小数精度、零值）

\- 对于 USDC 等 6 位小数的代币，需要正确处理精度转换

\- 防御性编程：在构建 Plan 前验证参数有效性

\---

**\### 问题 4: Confirmation Missing Warning**

**现象**:

\`\`\`

plan declares confirmation "swapResult" but no observer is wired

\`\`\`

**原因**:

Simulator 需要 observer 来跟踪和验证 Plan 声明的 confirmation。

**解决方案**:

\`\`\`typescript

const simulator = createTraceSimulator(runtime, {

  observer: [registry.observer](http://registry.observer)()

});

\`\`\`

**反思**:

\- Moss 的 Simulator 不仅执行交易，还验证预期效果

\- observer 机制用于追踪状态变化和确认预期结果

\- 这是 Moss Framework 的安全验证核心

\---

**\### 问题 5: Logger 语法错误**

**现象**:

\`\`\`

error TS1005: ',' expected.

\`\`\`

**原因**:

模板字符串中使用了多余的括号：

\`\`\`typescript

// 错误

.map((h, i) => chalk.bold(h.padEnd(colWidths\[i\]))))

// 正确

.map((h, i) => chalk.bold(h.padEnd(colWidths\[i\])))

\`\`\`

**反思**:

\- 链式调用时注意括号配对

\- TypeScript 严格模式下语法检查更严格

\---

**\## Moss Framework 架构理解**

**\### 核心流程**

\`\`\`

discover → load → action → simulate

\`\`\`

1. **discover**: 发现可用的 capabilities（协议、方法）

2. **load**: 加载 capability stubs

3. **action**: 构建 Plan（声明 intent、risk、expects）

4. **simulate**: 在模拟器中验证 Plan 的效果

**\### 关键组件**

| 组件 | 作用 |

|------|------|

| `Registry` | 注册和管理 manifests，提供 discover/load/action 接口 |

| `Runtime` | 区块链运行时环境，封装 RPC 调用 |

| `Simulator` | 交易模拟器，验证 Plan 是否符合预期 |

| `Observer` | 状态观察者，跟踪交易执行过程中的状态变化 |

**\### Plan 结构**

\`\`\`typescript

interface Plan {

  intent: string;           // 意图描述

  declaredRisk: string\[\];   // 声明的风险

  expects: {                // 预期效果

    in?: Asset\[\];           // 输入资产

    out?: Asset\[\];          // 输出资产

  };

  txs: Transaction\[\];       // 交易列表

}

\`\`\`

\---

**\## 实践心得**

**\### CLI 参数解析**

实现了 kebab-case 到 camelCase 的转换：

\`\`\`typescript

const kebabToCamel = (str: string): string => {

  return str.replace(/-(\\w)/g, (\_, c) => (c ? c.toUpperCase() : ""));

};

// --token-in → tokenIn

\`\`\`

**\### 错误处理模式**

创建自定义错误类，统一错误处理：

\`\`\`typescript

class MossError extends Error {

  constructor(public code: string, public message: string) {

    super(message);

  }

}

\`\`\`

**\### 用户体验提升**

\- 使用 chalk 实现彩色输出，区分不同类型的日志

\- 表格格式化数据展示，提高可读性

\- 步骤进度跟踪，让用户了解执行流程

\---

**\## �� 后续学习方向**

1. **深入理解 Simulator 机制**: 学习如何编写自定义 observer

2. **探索更多 Protocol**: Kuru、ERC20 等协议的实现细节

3. **构建更复杂的工作流**: 跨链、借贷等场景的组合示例

4. **理解 Registry 扩展机制**: 如何添加新的 Adapter

5. **学习测试框架**: Moss 的 e2e 测试和单元测试策略

\---
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->












**\# Moss 项目学习笔记**

\> 学习时间：2026-07-14

\> 项目地址：[https://github.com/nishuzumi/moss](https://github.com/nishuzumi/moss)

\> 学习目标：理解 Moss 的设计理念、核心能力及应用场景

\---

**\## 一、项目简介**

**\### 1.1 Moss 是什么**

Moss 是一个面向 **Monad 区块链** 的 AI Agent 交互框架，它把复杂的 DApp/协议交互抽象为 Agent 可调用的统一能力层。核心流程可以概括为四个步骤：

\`\`\`

discover → load → action → simulate

发现    → 加载 → 行动  → 模拟验证

\`\`\`

**一句话总结：** Moss 让 AI Agent 用人类可读的语言操作区块链，不用关心底层技术细节，并且每笔交易在签名前都经过模拟验证。

**\### 1.2 核心理念**

Moss 的设计哲学是\*\*"系统负责组装正确的交易，Agent 负责理解用户意图"\*\*。它强调两条安全红线：

| 规则 | 负责方 | 说明 |

|------|--------|------|

| Moss 永不签名、永不发送 | 框架 | 私钥留在钱包，最终决定权在用户 |

| 每笔写操作签名前必须验证 | 机械执行 | 模拟交易，对比声明效果与实际效果 |

\---

**\## 二、核心问题**

**\### 2.1 AI Agent 操作区块链的痛点**

如果没有 Moss，一个 AI Agent 要完成"在 DEX 上用 1 MON 换 USDC"这个简单操作，需要手动处理以下所有细节：

\`\`\`

❌ 知道 DEX 路由合约地址

❌ 读取并解析 Router 合约的 ABI

❌ 区分 exactInput / exactOutput 两种调用方式

❌ 处理原生 MON → WMON 的包装（如果路由只收 WMON）

❌ 计算滑点（还要考虑 decimals 换算，USDC 是 6 位，MON 是 18 位）

❌ 构建 multicall 数组（交换 + 退款 + 扫尾清理）

❌ 批准代币授权（approve）

\`\`\`

**现实问题：** Agent 只要弄错其中一项，就可能导致：

\- 交易回滚（损失 Gas）

\- 滑点计算错误（损失资金）

\- 授权给错误的地址（资金被盗）

\- 遗漏退款逻辑（小额资金永久锁在合约里）

**\### 2.2 Moss 的解决方案**

Moss 把这些复杂性全部封装到**\*\*能力层\*\***后面，Agent 只需要说人话：

\`\`\`

用户：用 1 MON 换 USDC

   ↓

Agent：discover(swap) → load(kuru.swap) → action(kuru, swap, { tokenIn: MON, tokenOut: USDC, amount: 1 })

   ↓

Moss 自动：构建未签名交易 + 声明预期效果（expects）

   ↓

Moss 自动：在真实链上模拟，对比实际效果 vs 声明效果

   ↓

零警告 → 交给用户钱包签名

\`\`\`

\---

**\## 三、核心能力**

**\### 3.1 四步调用流程**

**\#### 1️⃣ discover（发现）**

Agent 告诉 Moss "我想做什么动作"，Moss 返回匹配的能力列表。

\`\`\`ts

discover(verb?: 'swap' | 'wrap' | 'transfer', category?: 'token' | 'dex')

// → 返回所有匹配的协议方法

\`\`\`

**设计意义：** Agent 不需要提前知道有哪些协议，能力是动态发现的。

**\#### 2️⃣ load（加载）**

获取某个具体能力的详细定义：意图描述、风险标签、参数说明。

\`\`\`ts

load({ protocol: 'kuru', method: 'swap' })

// → { intent, risk: \['fundOut', 'slippage'\], params: { tokenIn, tokenOut, amount } }

\`\`\`

**设计意义：** Agent 用这些信息向用户确认操作，确保意图对齐。

**\#### 3️⃣ action（行动）**

根据用户输入参数，构建**\*\*未签名交易计划（Plan）\*\***。

Plan 包含两个关键部分：

\- **txs\[\]**：未签名的交易数组（可能是多笔，如 approve + swap）

\- **expects**：声明的预期效果（流出/流入哪些资产、金额范围）

\`\`\`

expects 示例：

  流出：最多 1 MON

  流入：最少 1234.56 USDC（扣除滑点后的下限）

  授权：\[\] 或 \[{ spender, token, amount }\]

\`\`\`

**\#### 4️⃣ simulate（模拟验证）**

在真实链上状态执行 Plan，提取实际效果，与 expects 对账。

\`\`\`

模拟结果结构：

  reverted: false           // 交易是否回滚

  effects: { assetsOut, assetsIn, approvals, recipients }

  warnings: \[\]              // 任何差异都会产生 warning

\`\`\`

**关键安全机制：**

| 检查项 | 说明 |

|--------|------|

| 资产对账 | expects 声明的资产流入/流出范围是否匹配 |

| 授权检查 | 是否有未声明的 approve 操作 |

| 收款人验证 | 资金是否流向预期地址 |

| Native 追踪 | 不发 Transfer 事件的原生 MON 流动也被追踪 |

| 跨 Plan 状态 | Plan B 可以花掉 Plan A 模拟结果里刚获得的代币 |

**\### 3.2 两条安全规则**

\`\`\`

规则一（机械，服务器侧）：Effects 对账

  → 模拟提取实际发生的一切，任何未声明差异都产生 warning

  → 有 warning 即停，不允许签名

规则二（语义，Agent 侧）：意图对齐

  → 把 effects 摘要和用户的原话对比

  → 只有 Agent 拿着用户的原始意图

\`\`\`

**\### 3.3 当前支持的协议**

| 协议 | 能力 | 查询 |

|------|------|------|

| WMON（封装 MON） | wrap, unwrap | balanceOf |

| ERC20（任意代币） | transfer | balanceOf, allowance |

| ERC721（任意 NFT） | transfer | ownerOf, balanceOf |

| Kuru（链上 CLOB DEX） | swap（市价单） | quote, markets |

\---

**\## 四、可能应用场景**

**\### 4.1 场景一：AI 交易助手（DeFi 自动化）**

**用户需求：**

\> "我想把钱包里的 50% MON 换成 USDC，然后把 USDC 的一半存到借贷协议里赚利息，剩下的一半定投某个代币，每天自动执行。"

**Moss 做什么：**

\`\`\`

1\. discover 找到：swap + 借贷协议的 supply

2\. 多步 action：swap(MON→USDC) → supply(USDC) → 记录定投计划

3\. simulate 串联验证：swap 得到的 USDC 可以被 supply 使用

4\. 每一步都有 expects 对账，确保没有未声明的资金流动

\`\`\`

**价值：** 用户不用关心任何协议细节，AI 全程操作，且每一步都经过验证。

**\### 4.2 场景二：NFT 自动化管理**

**用户需求：**

\> "帮我监控某个 NFT 系列的地板价，如果跌到 0.5 MON 以下就自动买 1 个，然后挂到市场上以 1 MON 出售；如果 7 天没卖出就降价 10%。"

**Moss 做什么：**

\`\`\`

1\. erc721 查询：ownerOf(系列地址) + balanceOf(我的账户)

2\. erc20 转账：准备购买资金（可能需要 approve）

3\. NFT 市场协议 action：listNFT / buyNFT

4\. 每笔 action 都 simulate 验证：

   - 确认 NFT 真正转入我的钱包

   - 确认挂单授权金额正确

   - 确认版税和手续费符合预期

\`\`\`

**\### 4.3 场景三：链上资产管理仪表盘（企业级）**

**企业需求：**

\> "我们公司有一个多签钱包，分散在 5 个协议里有资产，需要每天生成资产报告，发现异常授权自动撤销，定期将收益转回国库地址。"

**Moss 做什么：**

\`\`\`

1\. discover 加载所有相关协议：staking、lp、lending、vesting

2\. action 查询每个协议的用户资产余额

3\. 汇总生成资产报表（effects 结构天然适合做这个）

4\. 异常授权检测：

   - allowance 查询所有大额授权

   - 发现未使用的无限授权 → action(approve, 0) → simulate → 提交多签

5\. 定期转账：

   - 计算各协议应收收益

   - 构建 multi-step Plan：claim → swap → transfer(国库)

   - simulate 全链路验证

\`\`\`

**\### 4.4 场景四：去中心化社交 + 打赏机器人**

**场景描述：**

在 Farcaster / Lens 等去中心化社交协议中，一个 AI Bot 监听用户提到"打赏"关键词，自动给优质内容创作者转账，同时自动兑换成创作者想要的币种。

\`\`\`

用户发帖："这篇分析太赞了，打赏 5 MON！"

   ↓

Bot 解析：

  1. erc20 查询：我的余额够不够 5 MON

  2. 如果不够，discover → swap(其他代币→MON)

  3. action：transfer(MON, 创作者地址, 5)

  4. simulate 验证：

     - 确实转出 5 MON（不多不少）

     - 收款人确实是创作者地址

     - 没有额外的 approve 操作

  5. 零警告 → Bot 触发签名

\`\`\`

**\### 4.5 场景五：跨协议套利（需谨慎）**

**思路：** 监控同一个代币在不同 DEX 上的价差，发现套利空间时自动执行。

**Moss 的角色：**

\`\`\`

1\. 同时查询多个 DEX 的 quote（kuru、未来接入的 Uniswap 等）

2\. 发现价差 → 构建 Plan A：买入（DEX A） → Plan B：卖出（DEX B）

3\. simulate 串联执行：

   - Plan A 得到的代币立即用于 Plan B

   - 验证扣除 Gas 和手续费后确实盈利

   - 验证没有意外的滑点损失

4\. 有 warning → 放弃执行（市场可能已变化）

\`\`\`

\---

**\## 五、个人理解**

**\### 5.1 Moss 在 AI + Web3 栈中的位置**

\`\`\`

用户自然语言层

    ↓

   AI Agent（LLM）← Moss 的 MCP 工具接口

    ↓              discover / load / action / simulate

   Moss 能力层

    ↓              协议适配器（每个协议一个包）

   区块链层（Monad）

\`\`\`

**我的判断：** Moss 解决的是 AI Agent "手搓交易" 的不可靠问题。之前 LLM 直接调 viem/ethers，就像让实习生直接操作公司银行账户 —— 他可能很聪明，但不熟悉流程，弄错一个数字就是灾难性损失。

Moss 相当于在中间加了一层**\*\*财务审核系统\*\***：

| 传统企业 | Moss |

|----------|------|

| 填报销单 | load + action（声明用途和金额） |

| 财务审核 | simulate（验证实际流向 vs 声明） |

| 老板签字 | 用户钱包签名 |

| 打款 | 用户自己发送（Moss 不干） |

**\### 5.2 模拟验证机制的深度理解**

Moss 的 `debug_traceCall` 模拟机制是整个系统的灵魂。我理解它的价值有三层：

**第一层：技术正确性**

\- 合约有没有回滚？

\- 实际转出/转入金额对不对？

\- 有没有意外的 approve？

**第二层：协议理解正确性**

\- 很多协议有隐藏逻辑（比如 Kuru 的市价单可能遇到盘口深度不足）

\- 模拟能发现"代码正确但业务结果不对"的情况

\- expects 就是 Moss 版的"单元测试断言"

**第三层：信任边界**

\- 用户不需要信任 Agent，不需要信任 Moss

\- 只需要信任：**\*\*模拟结果显示我会转出 1 MON，收回 1200 USDC，收款人正确，没有异常授权\*\***

\- Moss 不持有私钥，做不了恶

**\### 5.3 为什么选择 Monad 首发**

Monad 是一个高吞吐量的 EVM 兼容链，这对 Moss 的设计非常契合：

1. **高频交互**：Monad 能处理大量并发交易，Agent 自动化场景的吞吐量瓶颈在链上被消除

2. **Gas 费低**：simulate 免费（dry-run），实际执行的 Gas 成本低 → 更适合 Agent 频繁操作

3. **debug\_traceCall 支持**：Monad 默认 RPC 支持 Moss 需要的模拟接口，第三方免费 RPC 大约有一半不支持

**\### 5.4 对未来的思考**

Moss 现在的架构是"每个协议手动写适配器"，这保证了质量，但扩展速度慢。我推测未来可能的方向：

1. **协议适配器市场**：类似 Chainlink 的节点运营，第三方可以贡献适配器并获得激励

2. **AI 自动生成适配器**：用 LLM 读取合约 ABI + 文档，自动生成 protocol 包，然后自动跑测试验证

3. **跨链扩展**：现在 Moss 明确不支持跨链（因为目标链 effects 无法验证），但如果未来有跨链证明系统，可以支持

4. **策略编排层**：在 Moss 之上再加一层，用声明式 DSL 描述投资策略，自动编译为 multi-step Plan

**\### 5.5 风险提示与边界**

学习过程中我也注意到 Moss 明确声明的限制：

| 不支持的功能 | 原因 |

|--------------|------|

| Permit / typed-data 签名 | 签名流的 effects 无法通过 call trace 验证 |

| 跨链桥 | 目标链的 effects 无法在源链模拟 |

| 闪电贷原子组合 | 组合后的 effects 超出单个协议的 expects 范围 |

| 模拟结果 ≠ 执行保证 | 链上状态随时变化，模拟只是快照 |

这些边界恰恰说明 Moss 的设计是**\*\*负责任的\*\*** —— 它不承诺做不到的事，只在能验证的范围内提供价值。

\---

**\## 六、总结**

| 维度 | Moss 的价值 |

|------|------------|

| 对 Agent | 不用懂 ABI、不用算 decimals、不用管 multicall，用人话就能操作区块链 |

| 对用户 | 每笔签名前都有"效果预览 + 差异警告"，大幅降低误操作风险 |

| 对协议方 | 写一个适配器包就能让所有接入 Moss 的 Agent 自动支持你的协议 |

| 对整个 AI + Web3 生态 | 建立了"声明 → 验证 → 执行"的安全范式，而不是让 LLM 瞎蒙 |

Moss 不是"又一个 Web3 SDK"，它是 AI Agent 安全地与价值网络交互的基础设施。正如它的名字 —— Moss（苔藓），不起眼，但覆盖整个地表，为更高层的生态提供坚实的基础。

\---

_学习笔记完_
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->













````markdown


 今日学习目标

1. 完成代币迁移合约开发与测试
2. 完成 Week 2 职业选择任务
3. 创建 AI 协作记录
4. 完成 Week 3 Role Statement



一、代币迁移合约开发

 1.1 项目结构

contracts/TokenMigration.sol
├── OldToken      旧版代币合约
├── NewToken      新版代币合约（带手续费）
└── TokenMigrator 迁移合约
```

1.2 核心功能

OldToken 改进：
- 新增 `holders` 数组，记录所有代币持有者
- 新增 `migrationController` 机制，允许迁移合约暂停旧合约
- 新增 `pauseForMigration()` 函数，由迁移控制器调用

NewToken 特性：
- 支持交易手续费（默认 0.1%）
- `initializeAfterMigration()` 函数，批量初始化余额
- `setTransferFee()` 和 `setFeeCollector()` 管理函数

TokenMigrator 流程：
1. `deployNewToken()` - 部署新合约
2. `startMigration()` - 暂停旧合约，标记开始
3. `executeMigration()` - 收集余额，初始化新合约，标记完成

1.3 关键技术点



持有者追踪 | 通过 `holders` 数组 + `_isHolder` mapping 实现 
 权限控制 | 使用 `onlyOwner` 和 `onlyMigrationController` 修饰器 
迁移安全 | 暂停旧合约防止余额变动，`migrationCompleted` 防止重复迁移 

 



 二、单元测试

 2.1 测试覆盖

| 测试模块 | 测试用例 | 状态 |
|----------|----------|------|
| OldToken | 部署、转账、持有者追踪、暂停 | ✅ |
| NewToken | 迁移初始化、手续费扣除 | ✅ |
| TokenMigrator | 部署、迁移流程、数据获取、重复初始化保护 | ✅ |

2.2 测试结果


12 passing (649ms)


2.3 测试文件

[TokenMigration.js](file:///d:/Web3暑期实习/solidity-project/test/TokenMigration.js)



 三、部署脚本

 3.1 部署流程

```bash
npx hardhat run scripts/deploy-migration.js --network monadTestnet
```

3.2 脚本步骤

1. 部署 OldToken（初始供应量 10000 OTK）
2. 部署 TokenMigrator
3. 设置迁移控制器
4. 通过 Migrator 部署 NewToken
5. 开始迁移（暂停旧合约）
6. 执行迁移（余额迁移）

 3.3 部署文件

[deploy-migration.js](file:///d:/Web3暑期实习/solidity-project/scripts/deploy-migration.js)

-

四、Week 2 职业选择任务

 4.1 角色选择

主方向：Dev（智能合约开发者）

选择理由：
- 技术背景匹配：已掌握 Solidity、Hardhat、ethers.js 等核心技术
- 项目经验：完成了 MessageBoard dApp 和代币迁移系统
- 市场需求：智能合约开发是 Web3 领域最紧缺的岗位之一

 4.2 任务完成

| 任务 | 文件 |
|------|------|
| Role Choice Card | Week2-Role-Log.md |
| Role Log | Week2-Role-Log.md |
| AI 协作记录 | AI-Collaboration-Record.md |
| Week 3 Role Statement | Week3-Role-Statement.md |



五、遇到的问题与解决方案

 问题 1: 迁移控制器权限问题

| 项目 | 详情 |
|------|------|
| 现象 | TokenMigrator 调用 OldToken.pause() 失败，报错 "Not owner" |
| 原因| OldToken 的 onlyOwner 检查的是调用者（Migrator 合约），而非部署者 |
| 解决方案| 添加 migrationController 机制，允许 Migrator 暂停旧合约 |

 问题 2: BOM 字符编译错误

| 项目 | 详情 |
|------|------|
| 现象| 合约编译失败，提示 "Expected pragma" |
| 原因 | 文件开头有 UTF-8 BOM 字符 |
| 解决方案 | 使用 UTF-8 无 BOM 编码重新创建文件 |

问题 3: 地址收集困难

| 项目 | 详情 |
|------|------|
| 现象| 无法遍历 mapping 获取所有代币持有者 |
| 原因 | Solidity mapping 无法直接遍历 keys |
| 解决方案 | 在 OldToken 中维护 holders 数组 |

---

六、今日学习收获

 收获 1: 代币迁移设计模式

重要性: ⭐⭐⭐⭐⭐

学会了代币迁移的完整设计模式：
1. 暂停机制：迁移前暂停旧合约
2. 余额快照：收集所有用户余额
3. 批量初始化：一次性设置新合约余额
4. 完成标记：防止重复迁移

 收获 2: 权限控制设计

重要性: ⭐⭐⭐⭐

理解了智能合约中权限控制的重要性：
- 使用修饰器封装权限检查
- 支持多角色权限（owner + migrationController）
- 最小权限原则：只授予必要的权限

 收获 3: 测试驱动开发

重要性: ⭐⭐⭐⭐⭐

学会了使用测试驱动开发方法：
1. 先写测试用例定义预期行为
2. 实现代码使测试通过
3. 通过测试验证功能正确性

 收获 4: AI 协作流程

重要性: ⭐⭐⭐⭐

总结了与 AI 协作的最佳实践：
- AI 适合快速生成代码框架和测试用例
- 人类负责审核代码、做关键决策、管理安全敏感操作
- 建立清晰的协作流程和分工

---

 七、Week 3 学习计划

 7.1 技术学习目标

| 目标 | 优先级 | 说明 |
|------|--------|------|
| DeFi 协议深度解析 | 高 | AMM、借贷、稳定币 |
| 安全审计入门 | 高 | 常见漏洞分析、审计工具 |
| 合约升级模式 | 中 | 代理模式、钻石标准 |
| Layer 2 开发 | 中 | Rollup、ZK 证明 |

 7.2 实践计划

1. 复现简单 AMM 合约：学习 Uniswap V2/V3 原理，实现简易版本
2. 安全审查练习：使用 Slither 分析开源合约，输出审计报告
3. 参与团队项目：与队友协作完成 Week 3 团队任务

---

八、需要帮助的问题

1. DeFi 协议原理：AMM 的数学原理和实现细节
2. 安全审计工具：Slither、Mythril 的使用方法
3. 合约升级：代理模式的安全注意事项
4. Layer 2 开发：ZK Rollup 的开发框架和工具

---

 总结

今天的学习非常充实！主要完成了：

1. ✅ 代币迁移合约开发（OldToken、NewToken、TokenMigrator）
2. ✅ 单元测试编写（12/12 通过）
3. ✅ 部署脚本编写
4. ✅ Week 2 职业选择任务全部完成
5. ✅ AI 协作记录创建
6. ✅ Week 3 Role Statement 创建

通过今天的实践，我对智能合约开发有了更深入的理解，特别是代币迁移系统的设计模式和权限控制机制。同时也完成了 Week 2 的所有职业选择任务，为 Week 3 的团队协作做好了准备。期待 Week 3 的团队项目！
````
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->














今日学习笔记：ERC20代币完整知识点梳理

**学习日期**：2026年07月12日

**学习主题**：ERC20代币标准原理、特性与应用

## 一、学习前言

本次学习聚焦以太坊生态最核心、最通用的代币标准ERC20，弄懂其设计逻辑、核心规则、基础函数以及实际应用场景。ERC20是区块链代币开发、链上转账、DeFi交互的基础核心知识点，绝大多数以太坊及兼容公链的代币均遵循该标准，掌握ERC20是入门区块链代币体系的关键。

## 二、ERC20基础定义

ERC20全称 **Ethereum Request for Comments 20**，是以太坊社区提出的**同质化代币技术标准**，也是一套统一的智能合约接口规范。

1\. **同质化代币核心特点**：代币之间完全等价、可互换、可分割，无唯一标识。例如1枚A项目ERC20代币和另一枚同项目代币价值、权益完全一致，可自由兑换、转账。

2\. **标准意义**：统一的接口让所有ERC20代币可以兼容以太坊钱包、交易所、DeFi协议，无需单独适配，实现一键转账、充值、交易、质押等操作，极大降低了区块链生态的交互成本。

## 三、ERC20核心关键特性

这是ERC20代币区别于其他代币的核心属性，也是日常链上交互的基础：

### 1\. 同质化、可互换

同一种ERC20代币无编号、无差异，任意两枚代币价值对等，可自由替换，这也是主流平台币、稳定币、项目治理币均采用ERC20标准的核心原因。

### 2\. 可分割

ERC20代币支持小数位分割，默认精度为18位，最小单位可精确到小数点后18位，能够满足小额转账、交易、兑换等场景需求，不会出现代币无法拆分使用的问题。

### 3\. 总量固定/可控

代币发行总量由智能合约代码提前设定，分为固定总量、可增发、可销毁三种模式，所有代币流转规则公开透明，链上可查，无法被人为篡改。

### 4\. 链上公开透明

所有ERC20代币的转账记录、余额、合约代码、持币地址分布均记录在以太坊区块链上，任何人可通过区块浏览器查询，公开可溯源、不可篡改。

## 四、ERC20六大核心标准函数

ERC20标准规定了智能合约必须实现的基础接口，所有合规ERC20代币都包含以下核心方法，是链上交互的核心逻辑：

### 1\. totalSupply（总发行量）

查询代币的最大发行总量，该数值在合约部署时确定，部分代币合约支持后续增发或销毁修改总量。

### 2\. balanceOf（查询余额）

输入任意钱包地址，即可查询该地址持有当前ERC20代币的余额，是钱包、交易所展示用户资产的核心接口。

### 3\. transfer（直接转账）

核心转账函数，用户可直接将自己钱包内的代币转账至其他地址，转账成功后实时扣减余额、记录链上交易。**注意**：仅能转账本人钱包内的代币。

### 4\. approve（授权）

核心授权函数，用户授权第三方地址（交易所、DeFi合约、机器人等），允许对方调用自己的代币资产，是质押、交易、理财等DeFi操作的前提。

### 5\. transferFrom（代付转账）

配合approve授权使用，第三方地址在获得用户授权额度后，可代为划转用户的代币资产，交易所提币、DeFi流动性挖矿均依赖该函数实现。

### 6\. allowance（查询授权额度）

查询某个地址对第三方的剩余授权额度，可用于核对授权是否过期、是否需要重新授权，规避资产授权风险。

## 五、ERC20代币优缺点总结

### 优点

-   **通用性极强**：全生态通用，适配所有以太坊钱包、交易所、DeFi协议，兼容性拉满；
    
-   **交互便捷**：接口统一，转账、授权、交易逻辑标准化，开发和用户使用成本低；
    
-   **公开透明**：所有数据链上可查，规则透明，无暗箱操作；
    
-   **灵活性高**：支持分割、增发、销毁、授权，适配各类金融场景。
    

### 缺点

-   **无原生安全校验**：早期ERC20标准无内置安全机制，存在重入攻击、授权漏洞等风险；
    
-   **转账不可逆**：链上转账一旦出错（地址输错、转账失误），无法撤回、无法追回；
    
-   **依赖矿工费**：所有转账、授权操作均需要支付ETH作为Gas手续费，以太坊拥堵时手续费高、转账延迟。
    

## 六、易混淆知识点区分

1\. **ETH vs ERC20代币**：ETH是以太坊原生主币，用于支付Gas手续费；ERC20是基于以太坊合约发行的衍生代币，本身无Gas支付能力，转账必须消耗ETH手续费。

2\. **ERC20 vs NFT(ERC721)**：ERC20是同质化代币，可互换、可分割；ERC721是非同质化代币，每一枚独一无二、不可互换、不可分割。

## 七、今日学习总结与后续计划

**学习总结**：今日系统掌握了ERC20代币的定义、核心特性、六大标准函数、应用场景及优缺点，清晰区分了原生币与合约代币、同质化与非同质化代币的核心差异，理解了转账、授权的底层逻辑，夯实了区块链代币生态的基础认知。

**后续学习计划**：下一步将深入学习ERC20安全漏洞、授权风险、代币销毁/增发逻辑，以及对比ERC223、ERC777等升级代币标准，进一步完善代币体系知识。
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->















Solidity 进阶语法 · 今日学习笔记（0.8.x 稳定版）

**学习定位**：夯实进阶核心语法，适配合约工程开发、安全优化、Gas 优化，重点掌握 0.8.x 版本独有特性与易踩坑知识点

**核心原则**：懂底层区别、会优化 Gas、规避编译/运行时风险

* * *

## 一、数据类型进阶：自定义值类型

### 1\. 核心作用

0.8.8+ 新增，基于基础类型封装独立自定义类型，**杜绝隐式类型转换**，解决业务参数传参错误、类型混淆问题，提升合约强安全性。

-   包装：`自定义类型.wrap(原始值)`
    
-   解包：`自定义类型.unwrap(实例)`
    

### 3\. 极简示例

```solidity
type TokenId is uint256;
type Amount is uint256;

contract CustomTypeDemo {
    // 两种类型无法互相赋值，编译报错
    function mint(TokenId id, Amount amt) public pure returns(uint, uint) {
        return (TokenId.unwrap(id), Amount.unwrap(amt));
    }
}
```

### 4\. 笔记总结

仅做**编译期类型校验**，底层存储仍为原基础类型，无 Gas 损耗，纯业务安全优化。

* * *

## 二、数据位置深度辨析（进阶重中之重）

三大数据位置：**calldata / memory / storage**，核心差异在拷贝机制、Gas 消耗、使用场景

| 数据位置 | 特性 | Gas | 适用场景 |
| --- | --- | --- | --- |
| calldata | 只读、不拷贝、外部入参专属 | 最低（最优） | external 函数参数，字符串/结构体/数组入参 |
| memory | 临时拷贝、函数内有效、可修改 | 中等 | 函数内部临时变量、返回值组装 |
| storage | 链上持久存储、指针引用、可修改 | 极高 | 合约全局状态变量 |

### 关键踩坑点

`storage` 局部变量是**指针引用**，修改会直接改动链上数据；`memory` 是值拷贝，修改不影响原数据。

* * *

## 三、常量与不可变变量（Gas 优化核心）

### 1\. constant 编译常量

-   编译期硬编码，部署后永久固定
    
-   仅支持数值、字符串，**不能通过构造函数赋值**
    
-   无存储占用，读取 Gas 极低
    

### 2\. immutable 不可变变量

-   部署时（构造函数中）赋值，部署后不可修改
    
-   支持地址、数值等复杂类型
    
-   存储在代码段，而非状态存储槽，读 Gas 接近 0
    

### 示例代码

```solidity
contract ConstDemo {
    uint public constant MAX_SUPPLY = 10000; // 编译常量
    address public immutable OWNER; // 部署固化

    constructor() {
        OWNER = msg.sender;
    }
}
```

### 学习总结

固定业务阈值用 `constant`，部署时确定的唯一值（管理员地址、合约地址）用 `immutable`。

* * *

## 四、错误处理进阶：自定义错误 Custom Error

0.8.4+ 核心特性，**全面替代 require 字符串报错**

### 1\. 核心优势

-   大幅节省 Gas：无需存储字符串信息
    
-   支持传参：可返回具体错误数值、地址，方便链下排查
    
-   代码更简洁，条件判断更灵活
    

### 2\. 标准用法

```solidity
contract ErrorDemo {
    // 定义自定义错误（可携带参数）
    error InsufficientBalance(uint256 have, uint256 need);
    error NotOwner(address caller);

    address public immutable owner;
    uint256 public balance = 100;

    constructor() {
        owner = msg.sender;
    }

    function withdraw(uint256 amt) external {
        if (msg.sender != owner) revert NotOwner(msg.sender);
        if (amt > balance) revert InsufficientBalance(balance, amt);
        balance -= amt;
    }
}
```

### 笔记重点

优先使用 `if + revert 自定义错误`，放弃 `require(条件, "文字报错")`，工程开发标配。

* * *

## 五、函数进阶与特殊函数

### 1\. 可见性与可变性最优实践

-   **external**：外部专属，Gas 比 public 更低，外部调用函数优先用 external
    
-   **view/pure**：只读不写状态，无交易 Gas，仅查询
    
-   **payable**：唯一可接收 ETH 的函数修饰符
    

### 2\. 修饰符 Modifier 进阶

支持**传参修饰符**，可复用权限、参数校验逻辑，代码解耦

```solidity
modifier onlyMax(uint256 max) {
    require(msg.value <= max, "Over limit");
    _; // 执行原函数逻辑
}

function pay() external payable onlyMax(100) {}
```

### 3\. 接收 ETH 双函数机制

-   `receive()`：专属接收**纯 ETH 转账（无 calldata）**
    
-   `fallback()`：函数不存在 / 有 calldata 转账时触发
    

* * *

## 六、继承与重写（工程架构核心）

### 1\. 核心关键字

-   **virtual**：父函数允许被子合约重写
    
-   **override**：子合约重写父函数
    
-   **abstract**：抽象合约，包含未实现函数，无法部署
    
-   **interface**：接口，仅定义函数签名，无逻辑实现，用于合约交互
    

### 2\. 多重继承冲突解决

多父类存在同名函数时，必须显式声明 `override(父类1, 父类2)`，并指定具体调用父类逻辑

```solidity
contract A { function f() public virtual pure returns(uint) { return 1; } }
contract B { function f() public virtual pure returns(uint) { return 2; } }

contract C is A, B {
    function f() public override(A,B) pure returns(uint) {
        return B.f(); // 显式指定父类
    }
}
```

* * *

## 七、库 Library 进阶

### 1\. 两类库函数

-   内存库：操作 memory/calldata 数据，无状态，纯计算逻辑
    
-   存储库：接收 storage 指针，可直接修改合约状态数据
    

### 2\. 全局绑定特性

`using 库 for 类型 global;`可实现**全局合约生效**，无需每个合约重复引入。

* * *

## 八、ABI 编码与底层调用

### 1\. 三大编码函数

-   `abi.encode()`：32字节对齐编码，用于合约 call 调用
    
-   `abi.encodePacked()`：紧凑编码，用于哈希、签名计算（更省空间）
    
-   `abi.decode()`：解码合约返回的字节数据
    

### 2\. 底层调用核心区别

-   **call**：调用外部合约，使用**目标合约存储**，可传 ETH
    
-   **delegatecall**：复用外部合约逻辑，使用**当前合约存储**，代理模式核心
    

* * *

## 九、今日学习复盘（核心口诀）

1.  入参优先 calldata，临时变量用 memory，状态变量固定 storage
    
2.  固定值 constant，部署值 immutable，Gas 优化必用
    
3.  报错不用字符串，自定义 error 省 Gas 易排查
    
4.  父函数 virtual，子重写 override，多继承必须显式指定
    
5.  外部调用优先 external，转账区分 receive/fallback
    

## 十、明日待精进方向

1\. 插槽存储布局与变量打包优化 2. Yul 内联汇编极简优化 3. 事件 indexed 索引与链下数据筛选
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
















Ethers.js 今日学习笔记（v6 最新版）

**学习版本**：Ethers.js v6（当前官方主推稳定版本）

**学习目标**：掌握 Ethers.js 核心概念、五大核心模块、基础实操代码，区分版本差异，具备简单链上交互开发能力

## 一、Ethers.js 基础认知

### 1\. 什么是 Ethers.js

Ethers.js 是一款**轻量、安全、高性能**的 JavaScript/TypeScript 区块链开发库，主要用于以太坊及所有 EVM 兼容公链（Polygon、BSC、Arbitrum 等）的链上交互，是目前 Web3 开发的主流工具库。

核心作用：让浏览器/Node.js 程序实现**读取链上数据、连接钱包、签名交易、调用智能合约、监听链上事件**等全套 Web3 操作。

### 2\. 与 Web3.js 核心区别

Web3.js 是以太坊早期官方库，目前逐渐被淘汰；Ethers.js 是社区主流替代方案，优势显著：

-   **体积更小**：压缩后仅80KB左右，前端加载速度更快
    
-   **架构清晰**：严格区分「读（Provider）」和「写（Signer）」操作，逻辑分层明确
    
-   **安全性更高**：原生支持助记词、私钥加密，内置安全钱包机制
    
-   **开发体验好**：原生支持 TypeScript，API 简洁易懂，错误提示精准
    
-   **规避精度问题**：自动处理链上大整数（BigInt），杜绝数值丢失问题
    

### 3\. 版本核心差异

-   **v5**：老牌稳定版本，存量项目居多，API 相对繁琐
    
-   **v6**：官方主推新版，重构大量 API，模块化更强、性能更优，本次学习核心版本
    

## 二、Ethers.js 五大核心模块（重点）

Ethers.js 所有功能均围绕五大模块展开，是开发的核心基础。

### 1\. Provider 区块链读取器（只读）

Provider 是程序与区块链节点的**只读连接入口**，仅负责查询链上数据，无私钥、无签名、无法发起交易。

**核心能力**：查询账户余额、区块信息、交易详情、合约公开数据、解析链上日志

**常用类型**：

-   JsonRpcProvider：自定义 RPC 节点（Infura、Alchemy、本地节点，前后端通用）
    
-   BrowserProvider：v6 新版专属，替代 v5 Web3Provider，专门用于连接浏览器钱包（MetaMask）
    
-   StaticJsonRpcProvider：稳定 RPC 连接，支持自动重试，适合后端批量查询
    

### 2\. Signer 签名器（写入操作）

Provider 只能读链，**所有链上写入操作（转账、合约调用）必须通过 Signer 签名**，是区分读写操作的核心模块。

**两类核心签名器**：

-   浏览器端 JsonRpcSigner：依托用户钱包签名，私钥由钱包保管，安全无风险，DApp 主流方案
    
-   后端 Wallet：代码导入私钥/助记词签名，适用于脚本、链上机器人，需严格做好私钥保密
    

### 3\. Contract 智能合约交互器

专门用于与链上智能合约交互的核心对象，可理解为「链上合约翻译官」，将 JS 代码与链上二进制数据互相转换。

**合约交互三要素（缺一不可）**：

1.  合约地址：智能合约在区块链上的唯一标识
    
2.  ABI：合约接口描述文件，定义合约所有方法、参数、返回值
    
3.  Provider/Signer：读操作传 Provider，写操作必须传 Signer
    

**合约方法分类**：

-   读方法（view/pure）：免费、无需签名、不消耗 Gas，仅查询数据
    
-   写方法：需要签名、消耗 Gas、修改链上数据，交易上链生效
    

### 4\. Utils 工具函数库

Ethers 内置高频工具方法，无需手动封装，解决链上通用数据处理问题：

-   单位转换：parseEther（ETH 转 wei）、formatEther（wei 转 ETH）
    
-   数据加密：keccak256 哈希计算、签名验证
    
-   格式校验：地址合法性校验、ABI 编解码
    
-   钱包工具：BIP39 助记词生成、BIP44 分层钱包推导
    

### 5\. Event 链上事件监听

实时监听智能合约触发的日志事件，可监控转账、NFT 铸造、合约交互等链上动态，常用于 DApp 实时数据更新、链上数据监控。

## 三、v6 核心实操代码（可直接复用）

### 1\. 安装依赖

```bash
npm install ethers
```

### 2\. 前端连接钱包 + 查询余额

```javascript
import { ethers } from "ethers";

// 1. 初始化浏览器钱包提供者（v6 新语法）
const provider = new ethers.BrowserProvider(window.ethereum);

// 2. 请求用户连接钱包
await provider.send("eth_requestAccounts", []);

// 3. 获取签名器（用于后续写入操作）
const signer = await provider.getSigner();

// 4. 获取钱包地址、查询余额
const address = await signer.getAddress();
const balanceWei = await provider.getBalance(address);
const balanceEth = ethers.formatEther(balanceWei);

console.log("钱包地址：", address);
console.log("账户余额：", balanceEth, "ETH");
```

### 3\. 基础 ETH 转账交易

```javascript
// 构建交易参数
const txParams = {
  to: "接收地址",
  value: ethers.parseEther("0.01") // 0.01 ETH 转为 wei
};

// 发起交易（钱包签名）
const tx = await signer.sendTransaction(tx);

// 等待交易上链确认
await tx.wait();
console.log("交易成功，哈希：", tx.hash);
```

### 4\. 读取智能合约数据

```javascript
// 合约基础信息
const contractAddr = "合约地址";
const contractABI = [/* 粘贴合约 ABI */];

// 初始化合约实例（只读，使用 Provider）
const contract = new ethers.Contract(contractAddr, contractABI, provider);

// 调用合约读方法
const result = await contract.合约方法名();
console.log("合约查询结果：", result);
```

## 四、核心易错点 & 避坑总结

-   **读写分离**：Provider 仅可读，调用合约写方法、转账必须传入 Signer，否则报错
    
-   **单位转换必做**：链上所有数值均为 wei（BigInt），展示需 formatEther，传参需 parseEther，直接运算会精度丢失
    
-   **v6 语法变更**：废弃 Web3Provider，统一使用 BrowserProvider；部分工具函数命名简化
    
-   **私钥安全**：前端禁止手动导入私钥，必须依赖钱包签名；后端私钥需配置环境变量，禁止硬编码
    
-   **交易确认**：sendTransaction 后必须执行 wait()，否则无法确保交易上链成功
    

## 五、今日学习总结

1\. 明确 Ethers.js 核心定位：Web3 链上交互主流库，凭借轻量、安全、架构清晰的优势，全面替代 Web3.js，是 DApp 开发、合约调试的核心工具。

2\. 掌握核心架构逻辑：**Provider 读、Signer 写、Contract 交互、Utils 工具、Event 监听**五大模块分工明确，读写分离是 Ethers 最核心的设计思想。

3\. 熟练掌握基础实操：完成钱包连接、余额查询、ETH 转账、合约只读查询基础操作，理解链上数据单位转换、交易签名、上链确认的完整流程。

4\. 后续学习方向：深入学习合约写入调用、链上事件监听、批量数据查询、交易异常处理、Gas 费用自定义配置。
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->

















## 今日学习笔记

DApp 架构、开发流程、以太坊基础开发环境、RPC 节点基础认知

## 一、DApp 架构核心组成

DApp（去中心化应用）区别于传统中心化 Web 应用，运行在区块链分布式网络，逻辑与数据无单一主体管控，由全网参与者共同维护，整体分为三大核心模块：

### 1\. 前端（User Interface）

1.  技术栈：HTML/CSS/JS，主流框架 React、Vue，和传统 Web 前端技术栈一致。
    
2.  链交互逻辑（和传统 Web 最大区别）：前端**不直接直连区块链**，依靠两种渠道通信：
    
    -   钱包注入的 Provider（MetaMask 等浏览器钱包）
        
    -   第三方 RPC 节点服务商
        
3.  两类交互操作：
    
    -   只读调用（eth\_call）：读取合约数据、链上状态、事件日志，无需签名、不消耗 Gas；
        
    -   写交易操作：修改合约数据，前端构造交易数据，交由用户钱包签名后，通过 RPC 广播上链执行，消耗 Gas。
        
4.  必备集成：Web3 钱包（MetaMask），负责用户链上身份校验、交易签名，保障资产与隐私安全。
    

### 2\. 智能合约（Smart Contracts）

1.  定位：DApp 的业务核心，全部业务规则写在合约内，部署在区块链上；
    
2.  特性：执行规则自动、交易透明、数据不可篡改；
    
3.  以太坊开发标准：使用 Solidity 语言编写，运行在 EVM（以太坊虚拟机）中；
    
4.  作用：存储持久化数据、定义转账 / 业务逻辑、对外提供读写接口供前端调用。
    

### 3\. 区块链网络（底层链）

承载智能合约运行、交易打包共识、数据永久存储的底层分布式网络（以太坊、Monad、Polygon 等 EVM 兼容链）。

## 二、DApp 完整开发流程（梳理总结）

1.  **需求设计**：确定 DApp 业务逻辑，拆分合约存储结构、交互功能；
    
2.  **合约开发**：使用 Solidity 编写智能合约，完成业务逻辑；
    
3.  **本地测试**：搭建本地开发链，单元测试合约，规避漏洞；
    
4.  **前端开发**：搭建页面，集成 web3/ethers 库，对接钱包与 RPC；
    
5.  **联调交互**：前端实现合约读、写两类接口调用；
    
6.  **测试网部署**：将合约部署至公链测试网（如 Monad Testnet），全流程实机测试；
    
7.  **主网上线**：审计合约后部署主网，正式对外提供服务。
    

## 三、以太坊开发环境搭建要点

### 1\. 基础环境准备

-   运行环境：Node.js + npm/yarn（前端、合约编译测试工具依赖）
    
-   合约 IDE：Remix（在线快速调试）、VS Code+Solidity 插件（本地开发）
    
-   钱包工具：MetaMask，用于测试网转账、交易签名、前端页面连接
    

### 2\. 本地开发链

本地私有链（Hardhat、Foundry、Ganache），优势：无 Gas 消耗、交易秒确认，适合合约迭代与单元测试，无需测试网代币。

### 3\. 钱包与前端交互核心逻辑

前端通过注入 Provider 检测用户钱包，监听链切换、账户切换事件；

-   读数据：无需用户授权，直接 RPC 调用合约 view 函数；
    
-   写交易：需要用户连接钱包、授权签名，等待区块确认后更新页面状态。
    

## 四、RPC 节点服务核心认知

### 1\. RPC 定义

JSON-RPC 是区块链与外部程序通信的标准协议，前端 / 工具通过 RPC 接口向节点发送指令，实现查询链数据、广播交易。

### 2\. RPC 在 Web3 开发中的作用

1.  作为前端与区块链中间桥梁，转发读写请求；
    
2.  同步全网区块、交易、合约状态数据；
    
3.  打包用户签名后的交易，发送至矿工节点打包上链。
    

### 3\. RPC 服务分类

1.  公共免费 RPC：上手简单，但延迟高、并发限制大，适合学习；
    
2.  付费专业 RPC（Alchemy、Infura 等）：稳定低延迟、高并发，适合线上 DApp；
    
3.  自建 RPC 节点：完全自主可控，维护成本高，多用于大型项目。
    

### 4\. RPC 使用最佳实践

-   开发阶段：公共测试网 RPC 快速调试；
    
-   线上项目：多服务商 RPC 做故障备份，避免单点服务宕机；
    
-   区分读写请求：高频读操作做缓存，减少 RPC 请求频次。
    

## 五、今日学习小结

1.  理清 DApp 三层分离架构：前端（交互层）+ 智能合约（业务层）+ 区块链（底层存储执行层），分清读写链上操作的区别；
    
2.  掌握 DApp 从合约到前端、本地到测试网的完整开发链路；
    
3.  理解 RPC 节点是前端和区块链通信的核心载体，区分免费 / 商用 RPC 的适用场景；
    
4.  明确开发必备工具栈：Remix/Hardhat 合约开发、MetaMask 钱包、RPC 节点服务、React/Vue 前端框架。
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->


















**#学习笔记 - 2026年7月8日**

**今日学习目标**

1. 修复钱包连接问题

2. 完成 dApp 前端开发

3. 将 dApp 部署到 Vercel

4. 整理 Mini Demo 0 和 Week 1 Build Log

5. 确认 Week 2 方向

\---

**一、钱包连接问题排查与修复**

**1.1 问题现象**

点击 "Connect MetaMask" 按钮后，页面显示 "Not Connected"，MetaMask 没有弹出授权窗口。

**1.2 问题原因**

使用 wagmi 框架时，状态同步出现问题。虽然 `window.ethereum` 存在且 MetaMask 可以正常工作，但 wagmi 的 `useAccount()` hook 无法正确检测到连接状态。

**1.3 解决方案**

**放弃 wagmi，改用 ethers.js 直接实现钱包连接**

\`\`\`javascript

// 使用 ethers.js 直接连接钱包

const provider = new ethers.BrowserProvider(window.ethereum)

await provider.send('eth\_requestAccounts', \[\])

const signer = await provider.getSigner()

const addr = await signer.getAddress()

\`\`\`

**1.4 关键代码**

**钱包连接组件**: \[WalletConnect.jsx\](file:///d:/Web3暑期实习/solidity-project/my-app/src/components/WalletConnect.jsx)

\`\`\`javascript

useEffect(() => {

  const checkConnection = async () => {

    if (typeof window !== 'undefined' && window.ethereum) {

      try {

        const provider = new ethers.BrowserProvider(window.ethereum)

        const signer = await provider.getSigner()

        const addr = await signer.getAddress()

        setAccount(addr)

      } catch (err) {

        setAccount(null)

      }

    }

  }

  checkConnection()

}, \[\])

\`\`\`

**监听账户变化**:

\`\`\`javascript

const handleAccountsChanged = (accounts) => {

  if (accounts.length > 0) {

    setAccount(accounts\[0\])

  } else {

    setAccount(null)

  }

}

window.ethereum?.on('accountsChanged', handleAccountsChanged)

\`\`\`

**1.5 学习收获**

| 知识点 | 理解 |

| **wagmi vs ethers.js** | wagmi 是封装层，ethers.js 是底层库；当封装层出现问题时，直接使用底层库更可靠 |

| **钱包连接原理** | 通过 `eth_requestAccounts` 请求授权，然后获取 signer 和地址 |

| **事件监听** | 需要监听 `accountsChanged` 事件，处理用户切换账户的情况 |

\---

**二、dApp 前端开发**

**2.1 技术栈**

| 技术 | 版本 | 用途 |

|------|------|------|

| Next.js | 16.2.10 | 前端框架 |

| Tailwind CSS | 4.4.14 | 样式框架 |

| ethers.js | 6.x | 区块链交互 |

**2.2 组件结构**

\`\`\`

src/

├── app/

│   └── page.js          # 主页（客户端组件）

└── components/

    ├── WalletConnect.jsx    # 钱包连接组件

    ├── MessageList.jsx      # 消息列表组件（Read Function）

    └── PostMessage.jsx      # 发布消息组件（Write Function）

\`\`\`

**2.3 合约交互**

**Read Function（读取消息）**

\`\`\`javascript

const provider = new ethers.BrowserProvider(window.ethereum)

const contract = new ethers.Contract(contractAddress, contractAbi, provider)

const messages = await contract.getMessages()

\`\`\`

**特点**: 不消耗 gas，直接从节点读取数据

**Write Function（发布消息）**

\`\`\`javascript

const provider = new ethers.BrowserProvider(window.ethereum)

const signer = await provider.getSigner()

const contract = new ethers.Contract(contractAddress, contractAbi, signer)

const tx = await contract.addMessage(content)

await tx.wait()

\`\`\`

**特点**: 消耗 gas，需要等待交易确认

**2.4 关键代码**

**消息列表组件**: \[MessageList.jsx\](file:///d:/Web3暑期实习/solidity-project/my-app/src/components/MessageList.jsx)

**发布消息组件**: \[PostMessage.jsx\](file:///d:/Web3暑期实习/solidity-project/my-app/src/components/PostMessage.jsx)

\---

**三、部署到 Vercel**

**3.1 部署步骤**

1. **安装 Vercel CLI**:

   \`\`\`bash

   npm install -g vercel

   \`\`\`

2. **登录 Vercel**:

   \`\`\`bash

   vercel login

   \`\`\`

3. **部署项目**:

   \`\`\`bash

   cd my-app

   vercel deploy --prod

   \`\`\`

**3.2 部署结果**

| 链接类型 | URL |

|----------|-----|

| **主链接** | [https://my-gvhshqdyd-zxb1.vercel.app](https://my-gvhshqdyd-zxb1.vercel.app) |

| **备用链接** | [https://my-app-hazel-eight-93.vercel.app](https://my-app-hazel-eight-93.vercel.app) |

**\### 3.3 学习收获**

| 知识点 | 理解 |

|--------|------|

| **Vercel 部署** | Next.js 项目可以轻松部署到 Vercel，提供免费的 HTTPS 访问 |

| **环境要求** | 部署前需要先构建成功`npm run build`） |

| **域名分配** | Vercel 会自动分配一个随机域名 |

\---

**四、Mini Demo 0 整理**

**4.1 Demo 内容**

| 项目 | 详情 |

|------|------|

| **合约地址** | `0x92e00ffd40925aB9364884901290C18Ce2e060B9` |

| **网络** | Monad Testnet |

| **Demo URL** | [https://my-gvhshqdyd-zxb1.vercel.app](https://my-gvhshqdyd-zxb1.vercel.app) |

**4.2 真实链上操作**

\- ✅ 合约部署（已验证）

\- ✅ 3 次消息发布（已验证）

\- ✅ 区块浏览器可查

**4.3 AI 辅助与人工判断**

| 类型 | AI 贡献 | 人工决策 |

|------|----------|----------|

| 代码编写 | 部署脚本、交互脚本、前端组件 | 私钥管理、技术栈选择 |

| 问题排查 | BigInt 类型错误、RPC 连接问题 | RPC 验证、安全配置 |

| 文档编写 | README、Build Log | 功能需求定义 |

\---

**五、Week 1 Build Log**

**5.1 本周核心产出**

1. **智能合约**: MessageBoard.sol

2. **部署脚本**: scripts/deploy.js, scripts/interact.js

3. **dApp 前端**: Next.js + ethers.js

4. **部署结果**: 合约已上线 Monad Testnet，dApp 已部署到 Vercel

**5.2 方向选择：Tech**

**选择理由**:

\- 兴趣驱动：对智能合约开发和 dApp 构建有浓厚兴趣

\- 能力匹配：已有 Java、Vue 开发经验

\- 实践成果：成功完成合约开发、部署和 dApp 构建

\---

**六、遇到的问题与解决方案**

**问题 1: wagmi 钱包连接失败**

| 项目 | 详情 |

|------|------|

| **现象** | 点击连接按钮后，MetaMask 不弹出授权窗口 |

| **原因** | wagmi 的状态同步问题 |

| **解决方案** | 改用 ethers.js 直接实现 |

**问题 2: Hardhat Ignition 需要交互式确认**

| 项目 | 详情 |

|------|------|

| **现象** | `npx hardhat ignition deploy` 需要用户确认 |

| **原因** | Hardhat Ignition 的安全机制 |

| **解决方案** | 使用自定义部署脚本 |

**问题 3: BigInt 类型错误**

| 项目 | 详情 |

|------|------|

| **现象** | `TypeError: Cannot mix BigInt and other types` |

| **原因** | Solidity 的 `uint` 返回 JavaScript 的 `BigInt` |

| **解决方案** | 使用 BigInt 字面量 |

**\### 问题 4: RPC 连接超时**

| 项目 | 详情 |

|------|------|

| **现象** | 使用官方 RPC 连接超时 |

| **原因** | 公共 RPC 节点不稳定 |

| **解决方案** | 切换到 Ankr 的公共 RPC |

\---

**七、今日学习收获**

  **收获 1: ethers.js 直接连接钱包**

**重要性**: ⭐⭐⭐⭐⭐

学会了使用 ethers.js 直接与 MetaMask 交互，理解了钱包连接的底层原理：

1. 通过 `BrowserProvider` 连接到 MetaMask

2. 通过 `eth_requestAccounts` 请求授权

3. 通过 `getSigner()` 获取签名者

4. 通过 `getAddress()` 获取钱包地址

**收获 2: 合约交互的两种方式**

**重要性**: ⭐⭐⭐⭐

理解了区块链上两种操作的区别：

| 操作类型 | 特点 | Gas 消耗 |

|----------|------|----------|

| **Read Function** | 只读，直接从节点获取 | 免费 |

| **Write Function** | 需要签名，写入链上 | 消耗 gas |

**收获 3: dApp 部署流程**

**重要性**: ⭐⭐⭐⭐⭐

学会了将 dApp 部署到 Vercel 的完整流程，理解了：

1. 构建项目`npm run build`）

2. 登录 Vercel 账号

3. 执行部署命令

4. 获取公开访问链接

**收获 4: 问题排查能力**

**重要性**: ⭐⭐⭐⭐

学会了通过调试信息和日志来定位问题：

1. 添加调试面板查看关键信息

2. 使用测试按钮验证底层功能

3. 通过浏览器控制台查看错误日志

4. 根据错误信息调整技术方案

\---

**八、Week 2 学习计划**

**8.1 技术学习目标**

| 目标 | 优先级 | 说明 |

|------|--------|------|

| Solidity 高级特性 | 高 | 继承、接口、修饰器、库 |

| 前端与智能合约交互 | 高 | Wagmi/Viem 框架深入学习 |

| ERC 标准合约 | 中 | ERC20、ERC721 |

| 智能合约测试 | 中 | Hardhat 测试框架 |

**8.2 实践计划**

1. **完善 dApp**: 添加更多功能（用户消息过滤、消息点赞等）

2. **学习 Wagmi**: 解决之前的状态同步问题，掌握 Wagmi 的正确用法

3. **学习 ERC20**: 编写一个代币合约并部署

4. **编写测试**: 使用 Hardhat 编写合约测试用例

\---

**九、需要帮助的问题**

1. **智能合约安全审计**: 常见漏洞和安全编码最佳实践

2. **Wagmi 状态同步**: 如何正确配置和使用 Wagmi

3. **Gas 优化**: 如何降低合约的 gas 消耗

4. **前端性能**: dApp 的性能优化技巧

\---

 **总结**

今天的学习非常充实！主要完成了：

1. ✅ 修复了钱包连接问题（从 wagmi 切换到 ethers.js）

2. ✅ 完成了 dApp 前端开发（消息列表、发布消息）

3. ✅ 将 dApp 部署到 Vercel，获得了公开访问链接

4. ✅ 整理了 Mini Demo 0 和 Week 1 Build Log

5. ✅ 确认了 Week 2 的 Tech 方向

通过今天的实践，我对 Web3 开发有了更深入的理解，特别是钱包连接和合约交互的底层原理。在遇到问题时，学会了通过调试和测试来定位问题，并选择合适的技术方案。期待 Week 2 的深入学习！
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->



















````markdown
学习笔记 - 2026年7月7日



 一、今日学习目标

1.  配置 Hardhat 连接 Monad Testnet
2.  编译 Solidity 智能合约
3.  将合约部署到 Monad Testnet
4.  调用合约的 Read Function
5.  调用合约的 Write Function
6.  学习智能合约安全最佳实践

---

 二、项目概述

### 项目名称
MessageBoard（消息板智能合约）

### 功能描述
一个简单的去中心化消息板应用，允许用户发布和查看消息。

### 技术栈
- **框架**: Hardhat
- **语言**: Solidity ^0.8.28
- **网络**: Monad Testnet (Chain ID: 10143)
- **钱包**: MetaMask
- **交互**: ethers.js

---

## 三、关键知识点

### 3.1 Hardhat 网络配置

在 [hardhat.config.js](file:///d:/Web3暑期实习/solidity-project/hardhat.config.js) 中添加自定义网络：

```javascript
require("@nomicfoundation/hardhat-toolbox");
require("dotenv").config();

module.exports = {
  solidity: "0.8.28",
  networks: {
    monadTestnet: {
      url: process.env.MONAD_TESTNET_RPC_URL,
      accounts: [process.env.WALLET_PRIVATE_KEY],
      chainId: 10143,
    },
  },
};
```

知识点:
- 使用 `dotenv` 加载环境变量，避免硬编码敏感信息
- `accounts` 字段接受私钥数组，第一个私钥作为部署者账户
- `chainId` 用于验证网络身份

### 3.2 环境变量配置

在 `.env` 文件中存储敏感信息：

```env
MONAD_TESTNET_RPC_URL=https://rpc.ankr.com/monad_testnet
WALLET_PRIVATE_KEY=你的钱包私钥
```

**知识点**:
- `.env` 文件必须添加到 `.gitignore` 中
- 使用公共 RPC（如 Ankr）可以快速连接测试网
- 私钥是钱包的核心，泄露会导致资产损失

### 3.3 合约编译

```shell
npx hardhat compile
```

**知识点**:
- 编译产物存放在 `artifacts/` 目录
- Hardhat 会自动检测 Solidity 版本
- 编译错误会在终端显示详细信息

### 3.4 合约部署

创建部署脚本 [scripts/deploy.js](file:///d:/Web3暑期实习/solidity-project/scripts/deploy.js)：

```javascript
const { ethers } = require("hardhat");

async function main() {
  const [deployer] = await ethers.getSigners();
  console.log("Deploying with account:", deployer.address);

  const MessageBoard = await ethers.getContractFactory("MessageBoard");
  const messageBoard = await MessageBoard.deploy();
  await messageBoard.waitForDeployment();

  const contractAddress = await messageBoard.getAddress();
  console.log("Deployed to:", contractAddress);
}

main();
```

执行部署：
```shell
npx hardhat run scripts/deploy.js --network monadTestnet
```

**知识点**:
- `ethers.getSigners()` 获取配置的账户列表
- `ContractFactory.deploy()` 部署合约到链上
- `waitForDeployment()` 等待交易确认
- `getAddress()` 获取部署后的合约地址

### 3.5 部署结果

| 项目 | 值 |
|------|-----|
| 合约地址 | `0x92e00ffd40925aB9364884901290C18Ce2e060B9` |
| 交易 Hash | `0x372d13974a796aa4c37c74b9b35f0c8366c91875972313e867498d84f52b2866` |
| 部署者地址 | `0x000ca106EC26a06c3180a51aC1D56b0b2521bB46` |

### 3.6 合约交互

#### Read Function（只读调用，无需 gas）

```javascript
const count = await contract.getMessageCount();      // 获取消息数量
const messages = await contract.getMessages();       // 获取所有消息
const msg = await contract.getMessageByIndex(0);     // 获取指定索引的消息
```

**知识点**:
- Read Function 不修改链上状态
- 调用 Read Function 不需要支付 gas
- 返回值直接从节点获取

#### Write Function（写操作，需要 gas）

```javascript
const tx = await contract.addMessage("Hello World!");
await tx.wait();
```

**知识点**:
- Write Function 会修改链上状态
- 需要支付 gas 费用
- 返回 Transaction 对象，需等待确认

### 3.7 智能合约安全

#### 敏感信息保护

在 [.gitignore](file:///d:/Web3暑期实习/solidity-project/.gitignore) 中添加：

```
.env                    # 私钥
deployment-info.json    # 部署信息
```

**安全原则**:
1. 永远不要将私钥提交到 Git
2. 使用环境变量管理密钥
3. 定期轮换密钥
4. 使用 Secret Scanning 工具

---

## 四、遇到的问题与解决方案

### 问题 1: RPC 连接超时

**现象**: 使用 `https://testnet-rpc.monad.xyz` 连接超时

**原因**: 部分公共 RPC 可能不稳定或被墙

**解决方案**: 切换到 Ankr 的公共 RPC `https://rpc.ankr.com/monad_testnet`

### 问题 2: Hardhat Ignition 需要交互式确认

**现象**: `npx hardhat ignition deploy` 会询问是否确认部署

**解决方案**: 使用自定义部署脚本 `scripts/deploy.js`，避免交互式确认

### 问题 3: BigInt 类型错误

**现象**: `TypeError: Cannot mix BigInt and other types`

**原因**: Solidity 的 `uint` 类型在 ethers.js 中返回 `BigInt`，不能直接与普通数字运算

**解决方案**: 使用 `BigInt` 字面量（如 `1n`）或转换方法

```javascript
// 错误写法
const lastMsg = await contract.getMessageByIndex(newCount - 1);

// 正确写法
const lastMsg = await contract.getMessageByIndex(newCount - 1n);
```

---

## 五、学习收获

### 技术层面
1. ✅ 掌握了 Hardhat 配置自定义网络的方法
2. ✅ 学会了使用 ethers.js 部署和交互智能合约
3. ✅ 理解了 Read Function 和 Write Function 的区别
4. ✅ 掌握了环境变量和 .gitignore 的安全配置

### 实践层面
1. ✅ 成功将合约部署到真实测试网
2. ✅ 完成了完整的合约交互流程
3. ✅ 学会了排查 RPC 连接问题
4. ✅ 掌握了 BigInt 类型的处理方法

### 安全意识
1. ✅ 理解了私钥保护的重要性
2. ✅ 学会了配置 .gitignore 防止敏感信息泄露
3. ✅ 了解了测试网和主网的区别

---

## 六、下一步学习计划

1. 📚 学习 Solidity 高级特性（继承、接口、库）
2. 📚 学习测试网代币获取方法
3. 📚 学习前端与智能合约交互（Wagmi/Viem）
4. 📚 学习智能合约安全审计基础
5. 📚 尝试部署到其他测试网（Sepolia、Goerli）

````
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->




















今天已经把安装插件、创建钱包、添加网络、领水、看区块链浏览器这些步骤全部完成了
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
