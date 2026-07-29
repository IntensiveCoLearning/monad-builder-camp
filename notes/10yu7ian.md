- GitHub ID: 253485376
- Name: 10yu7ian
- Timezone: UTC+8
- Application: Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
<!-- DAILY_CHECKIN_2026-07-27_START -->
# 2026-07-27

由于残酷共学平台问题导致本人没有打上卡，此条记录为补卡。
<!-- DAILY_CHECKIN_2026-07-27_END -->

# 2026-07-25
<!-- DAILY_CHECKIN_2026-07-25_START -->
# 2026-07-25

今天开始做小项目了，确定了个大概的方向
<!-- DAILY_CHECKIN_2026-07-25_END -->

# 2026-07-24
<!-- DAILY_CHECKIN_2026-07-24_START -->
# 2026-07-24

找黑客松的方向，学习python
<!-- DAILY_CHECKIN_2026-07-24_END -->

# 2026-07-23
<!-- DAILY_CHECKIN_2026-07-23_START -->
# 2026-07-23

学习python…
<!-- DAILY_CHECKIN_2026-07-23_END -->

# 2026-07-22
<!-- DAILY_CHECKIN_2026-07-22_START -->
# 2026-07-22

听了钱包设计的分享会，学习python中
<!-- DAILY_CHECKIN_2026-07-22_END -->

# 2026-07-21
<!-- DAILY_CHECKIN_2026-07-21_START -->
# 2026-07-21

学习python，找黑客松的想法
<!-- DAILY_CHECKIN_2026-07-21_END -->

# 2026-07-20
<!-- DAILY_CHECKIN_2026-07-20_START -->
# 2026-07-20

学习python语言
<!-- DAILY_CHECKIN_2026-07-20_END -->

# 2026-07-19
<!-- DAILY_CHECKIN_2026-07-19_START -->
# 2026-07-19

[https://app.notion.com/p/ops-39e437a56411801c9ab2d7bdf51281bc?source=copy\_link](https://app.notion.com/p/ops-39e437a56411801c9ab2d7bdf51281bc?source=copy_link)
<!-- DAILY_CHECKIN_2026-07-19_END -->

# 2026-07-18
<!-- DAILY_CHECKIN_2026-07-18_START -->
# 2026-07-18

今天做了week2的任务，昨天忘记打卡了…
<!-- DAILY_CHECKIN_2026-07-18_END -->

# 2026-07-16
<!-- DAILY_CHECKIN_2026-07-16_START -->
# 2026-07-16

学习python中…
<!-- DAILY_CHECKIN_2026-07-16_END -->

# 2026-07-15
<!-- DAILY_CHECKIN_2026-07-15_START -->
# 2026-07-15

去学习了解moss的项目
<!-- DAILY_CHECKIN_2026-07-15_END -->

# 2026-07-14
<!-- DAILY_CHECKIN_2026-07-14_START -->
# 2026-07-14

还在学习AI
<!-- DAILY_CHECKIN_2026-07-14_END -->

# 2026-07-13
<!-- DAILY_CHECKIN_2026-07-13_START -->
# 2026-07-13

今天补了第一周的任务
<!-- DAILY_CHECKIN_2026-07-13_END -->

# 2026-07-12
<!-- DAILY_CHECKIN_2026-07-12_START -->
# 2026-07-12

我今天去学了python的基础，学了一点点，想做后端…
<!-- DAILY_CHECKIN_2026-07-12_END -->

# 2026-07-11
<!-- DAILY_CHECKIN_2026-07-11_START -->
# 2026-07-11

我今天去补了AI大模型的概念，面试被拷打了……
<!-- DAILY_CHECKIN_2026-07-11_END -->

# 2026-07-10
<!-- DAILY_CHECKIN_2026-07-10_START -->
# 2026-07-10

参加了例会，听了大家的分享
<!-- DAILY_CHECKIN_2026-07-10_END -->

# 2026-07-09
<!-- DAILY_CHECKIN_2026-07-09_START -->
# 2026-07-09

今天把合约部署到monad的测试网上了，去听了agent安全方面的分享会
<!-- DAILY_CHECKIN_2026-07-09_END -->

# 2026-07-08
<!-- DAILY_CHECKIN_2026-07-08_START -->
# 2026-07-08

今天用ai辅助生成了合约，但在remix上没有找到monad的测试网......

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/monad-builder-camp/main/assets/10yu7ian/images/2026-07-08-1783517918097-image.png)
<!-- DAILY_CHECKIN_2026-07-08_END -->

# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
# 2026-07-07

还在考试…明天补
<!-- DAILY_CHECKIN_2026-07-07_END -->

# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
# 2026-07-06

做了一两个小任务，还在考试复习......
<!-- DAILY_CHECKIN_2026-07-06_END -->

<!-- DAILY_CHECKIN_2026-07-28_START -->
# 2026-07-28

已经跑通了“Alpha Agent + 任务1 + 链上写入”的闭环，今天不加新功能，只做稳定性加固：

1. 将任务1的标准答案从“手动写死”升级为独立生成脚本
2. 给 `/api/run-evaluation` 增加完善的错误处理
3. 写本地测试脚本，验证结果稳定性和链上交易可查性

### **✅ 完成情况**

**1. 标准答案生成脚本**

* 创建 `scripts/generate_ground_truth.py`
* 输入 3-5 个固定 Monad 测试地址，用和 Agent 相同的查询工具跑一次
* 结果存入 `backend/data/ground_truth.json`
* 已手动核对 JSON 数据合理性

**2. API 错误处理增强**

* Claude API 调用失败 → 返回明确错误信息，服务不崩溃
* 合约写入失败（RPC 超时等）→ 记录日志，返回“待重试”状态，不卡住请求

**3. 本地稳定性测试**

* 编写测试脚本，连续跑 5 次评测
* 验证结果：同一地址多次评测分数接近（稳定）
* 所有链上交易均能在 Monad 浏览器查到，附 tx hash 记录

### **🔧 遇到的问题与解决**

| **问题**             | **解决方案**                |
| :----------------- | :---------------------- |
| 标准答案生成时查询工具返回数据不一致 | 增加了数据规范化处理，统一格式后再存 JSON |
| 合约写入失败时前端长时间等待     | 改为异步返回“待重试”状态，后台记录失败日志  |

### **📚 今日收获**

* 理解了“预存标准答案”方案的优势——Demo 现场不受链上数据源波动影响，结果可预期
* 学会了为 AI 产品设计降级策略：API 失败不阻塞整体流程
* 验证了评测结果的可重复性，这是信誉系统可信度的基础
<!-- DAILY_CHECKIN_2026-07-28_END -->
<!-- Content_END -->
