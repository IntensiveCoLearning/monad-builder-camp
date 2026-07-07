---
timezone: UTC+8
---

# daidaidawang

**GitHub ID:** daidaidawang

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-07
<!-- DAILY_CHECKIN_2026-07-07_START -->
Freshman Track 第01课：Vibecoding 入门笔记

# 1\. Vibecoding 是什么

Vibecoding 是一种利用 AI Agent 进行软件开发 的方式。

## 核心流程：

描述需求 → AI 构建 → 查看结果 → 提出反馈 → 持续迭代

开发者不再主要负责写代码，而是负责：

明确想构建什么

提供需求和反馈

判断最终结果是否符合目标

# 2\. AI Agent 与 Chatbot 区别

## Chatbot：

只能回答问题

输出文本

## AI Agent：

可以执行任务

创建文件

编写代码

运行命令

修复错误

Vibecoding 的核心就是人与 AI Agent 协作完成开发。

# 3\. Vibecoding 的关键能力：Prompt

Prompt 决定 AI 输出质量。

好的 Prompt 需要说明：

做什么产品

目标用户

页面结构

功能需求

UI 风格

例如：

“做一个网站” → 信息不足

“创建深色主题个人主页，包含项目展示、社交链接、移动端适配” → 更容易得到准确结果。

# 4\. AI Agent 安全问题

AI Agent 具有较高权限，可能：

读取文件

执行命令

访问外部内容

## 风险

Prompt Injection（提示注入）

即恶意内容通过隐藏指令影响 Agent 行为。

## 安全实践

使用沙箱环境（Replit / Codespaces）

不提供真实私钥、API Key

审查 AI 生成代码

# 5\. Vibecoding Debug 方法

遇到错误：

不要重新开始，也不要自己猜。

## 正确方式：

复制完整错误信息

提供给 AI Agent

根据反馈继续迭代

# 6\. Vibecoding 能力边界

## 可以

快速构建网站、工具、Demo

生成前端、数据库、API 等代码

加速产品验证

## 不能

保证一次生成完美项目

替代产品思考

替代安全和技术判断

Freshman Track 第02课笔记

构建并发布你的第一个应用

# 一、核心目标

通过 Vibecoding 完成完整开发流程：

想法 → Prompt → AI Agent 构建 → 调试 → 部署 → 分享

最终得到一个可以被互联网访问的应用 URL。

# 二、开发环境选择

推荐：Replit

## 特点

浏览器运行

无需本地配置

AI Agent 内置

环境隔离，更安全

Local 环境

## 适合

熟悉本地开发流程

需要更多控制权

常见问题：

Node 环境

npm 配置

localhost 连接

（了解即可，实际遇到问题交给 AI Agent 处理）

# 三、使用 AI Agent 构建应用

## 流程

## 1\. 选择简单目标

## 第一个项目重点

不是复杂，而是完整跑通流程。

例如：

个人主页

Todo List

倒计时工具

小型 Web App

## 2\. 编写 Prompt

Prompt 越具体，结果越准确

### 需要描述

应用目的

页面结构

功能需求

UI 风格

用户场景

例如：

不要：

\> 做一个网站

应该：

\> 创建一个深色主题个人主页，包括个人介绍、项目展示、社交链接，并适配移动端。

# 四、Vibecoding 开发循环

## 核心循环

描述

↓

构建

↓

审视

↓

反馈

↓

重复

## 重点

第一版不会完美，需要通过不断反馈优化。

# 五、Debug 方法

遇到问题：

不要：删除项目重做

自己猜错误原因

## 正确方式

复制完整错误信息

描述实际现象

交给 AI Agent 分析

例如：

错误：

\> 页面打不开

正确：

\> 页面显示空白，控制台出现 XXX error。

# 六、应用发布

## 部署方式

Replit

直接 Publish：

自动构建

自动部署

获得线上 URL

GitHub + Vercel

## 流程

代码上传 GitHub

↓

Vercel 部署

↓

获得公开访问地址

# 七、核心知识点

## 1\. Deployment（部署）

将本地运行的应用发布到公网，让其他用户访问。

## 2\. Iteration（迭代）

通过不断反馈修改应用，是 Vibecoding 的核心过程。

## 3\. Production Mindset（生产思维）

完成 Demo 只是开始。

真实应用还需要：

数据存储

用户系统

支付

安全

# Freshman Track 第03课笔记

让你的应用具备生产级品质

# 一、Demo 和 Production 的区别

部署成功 ≠ 生产可用。

## Demo

能运行

能展示功能

## Production

### 需要满足

稳定

安全

可扩展

用户体验完整

# 二、产品体验优化

Responsive Design（响应式设计）

应用需要适配：

手机

平板

桌面端

移动端流量占比高，不能只针对电脑设计。

Favicon / Page Title

提升产品完整度：

浏览器标签图标

搜索结果标题

Accessibility（无障碍）

让不同用户都能使用：

键盘操作

屏幕阅读器

视觉障碍用户

# 三、让应用被发现

SEO

提高搜索引擎收录和排名。

Open Graph

控制应用链接分享到：

Twitter

Discord

iMessage

时显示的标题、图片和描述

Analytics

## 收集

用户数量

来源

使用行为

帮助判断产品情况。

Custom Domain

## 使用自己的域名

例如：

myapp.com

相比平台默认域名更像正式产品。

四、真实应用需要的功能

Database（数据库）

解决：

刷新后数据丢失的问题。

用于保存：

用户数据

内容

配置

常见：

Supabase

Firebase

\---

Authentication（用户认证）

负责：

注册登录

密码管理

OAuth

不要自己实现。

原因：

密码哈希、Session 管理等容易出现安全问题。

\---

File Storage（文件存储）

用于：

图片

文档

用户上传文件

不要直接存服务器。

\---

Payments（支付）

使用第三方支付服务：

例如 Stripe。

负责：

支付

退款

税务

\---

五、基础设施

Environment Variables（环境变量）

敏感信息不要写进代码：

例如：

API Key

Secret

Token

原因：

GitHub 上存在自动扫描机器人，泄露后可能导致安全问题。

\---

CI/CD

自动化流程：

代码提交

↓

自动测试

↓

自动部署

\---

Staging Environment

生产环境之前的测试环境。

用于：

验证新功能

避免影响线上用户

\---

六、可靠性

Error Handling

避免：

用户看到空白页面。

应该：

捕获错误

给用户提示

Error Tracking

例如 Sentry：

记录：

哪个错误

哪个用户

发生环境

Testing

包括：

单元测试

集成测试

E2E 测试

Backup

数据库必须备份。

防止：

数据损坏

错误迁移

误删除

\---

七、安全

HTTPS

加密用户和服务器通信。

Input Validation

永远不要信任用户输入。

防止：

SQL Injection

XSS

Rate Limiting

限制请求次数。

防止：

API 被刷爆

服务崩溃

成本异常增加

\---

八、性能优化

图片优化

使用：

压缩

WebP / AVIF

合适尺寸

Cache（缓存）

减少重复请求，提高速度。

Core Web Vitals

Google 衡量网页体验的重要指标，会影响 SEO。

\---

九、法律要求

需要关注：

Privacy Policy（隐私政策）

Terms of Service（服务条款）

Cookie Consent（Cookie 同意）

\---

本节总结

生产级应用不仅是“能运行”，还需要：

体验 + 数据 + 安全 + 稳定 + 合规

从 Demo 到产品，需要补齐：

1\. 用户系统

2\. 数据存储

3\. 安全防护

4\. 错误监控

5\. 性能优化

6\. 法律规范

核心理解：

\> AI 可以快速帮你构建应用，但生产级产品仍然需要工程化思维。真正上线面对用户时，稳定、安全和可维护性比快速生成代码更重要。
<!-- DAILY_CHECKIN_2026-07-07_END -->
<!-- Content_END -->
