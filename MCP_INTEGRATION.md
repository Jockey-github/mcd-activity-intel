# MCP 集成说明

## 项目概述

**麦门活动情报局 (mcd-activity-intel)** 是一个基于麦当劳 MCP Server 能力的对话式 Skill，通过智能编排多个 MCP 工具，将麦当劳官方活动信息转化为年轻化、友好、有趣的活动情报推送。

## 使用的 MCP Server

| 项目 | 信息 |
|------|------|
| Server 名称 | 麦当劳中国 MCP Server |
| 接入地址 | `https://mcp.mcd.cn` |
| 传输协议 | Streamable HTTP |
| 认证方式 | Bearer Token（Authorization Header） |

## 使用的 MCP Tools

本项目串联了 **12 个** MCP 工具，覆盖活动日历、优惠券、积分、商城、主题活动 5 大业务域：

### 基础工具

| Tool | 名称 | 用途 |
|------|------|------|
| `now-time-info` | 获取当前时间 | 获取准确时间，用于计算活动紧急度标签（剩余天数） |

### 活动日历

| Tool | 名称 | 用途 |
|------|------|------|
| `campaign-calendar` | 活动日历查询 | **核心工具**，拉取当月全部营销活动列表，区分进行中/即将开始/已结束 |

### 优惠券体系

| Tool | 名称 | 用途 |
|------|------|------|
| `available-coupons` | 麦麦省券列表 | 查询当前可领取的优惠券，展示优惠活动详情 |
| `query-my-coupons` | 我的优惠券 | 查询用户已有的券，避免重复推荐已领取的券 |
| `auto-bind-coupons` | 一键领券 | 唯一的写操作 — 用户确认后批量领取可用优惠券 |

### 积分体系

| Tool | 名称 | 用途 |
|------|------|------|
| `query-my-account` | 积分查询 | 查询用户积分余额，用于个性化推荐（积分够不够玩） |
| `query-lottery-info` | 抽奖活动查询 | 查询积分抽奖的奖品、消耗规则，计算用户可抽次数 |
| `mall-points-products` | 商城商品列表 | 查询积分可兑换的商品，按性价比推荐 |

### 主题活动 / 派对

| Tool | 名称 | 用途 |
|------|------|------|
| `query-party-city` | 主题活动城市 | 查询哪些城市有线下主题活动 |
| `query-party-store` | 主题活动门店 | 查询指定城市可参与的门店列表 |
| `query-partystore-date` | 可预约日期 | 查询指定门店的可预约日期 |
| `query-partystore-session` | 可预约场次 | 查询指定日期的可预约时段 |

## 工具调用流程

### 流程1：全量活动情报（串联5个工具）

```
用户: "最近有啥活动"
  │
  ├─ 1. now-time-info          → 获取当前时间
  ├─ 2. campaign-calendar      → 拉取本月活动日历
  ├─ 3. available-coupons      → 查可领优惠券
  ├─ 4. query-lottery-info     → 查积分抽奖活动
  └─ 5. query-party-city       → 查主题活动城市
  │
  ▼
  汇总所有活动信息，按紧急度排序，以情报卡片格式输出
```

### 流程2：今日精选情报（串联4个工具）

```
用户: "今天有啥可以薅的"
  │
  ├─ 1. now-time-info          → 获取当前时间
  ├─ 2. campaign-calendar      → 拉取活动日历
  ├─ 3. available-coupons      → 查可领券
  └─ 4. query-my-coupons       → 查已有券（去重）
  │
  ▼
  筛选今日可参与的活动，按紧急度排序输出
```

### 流程3：积分个性化推荐（串联4个工具）

```
用户: "我有多少积分能玩啥"
  │
  ├─ 1. query-my-account       → 查实际积分余额
  ├─ 2. query-lottery-info     → 查抽奖规则，算可抽次数
  ├─ 3. mall-points-products   → 查可兑商品
  └─ 4. campaign-calendar      → 查积分相关活动
  │
  ▼
  按性价比排序推荐积分使用方案
```

### 流程4：线下活动查询（串联5个工具）

```
用户: "上海有什么线下活动"
  │
  ├─ 1. mall-points-products       → 查派对/主题活动商品列表，获取 spuId
  ├─ 2. query-party-city           → 传入 spuId，查覆盖城市列表
  ├─ 3. query-party-store          → 传入城市 code + spuId，查可参与门店
  ├─ 4. query-partystore-date      → 传入 spuId + storeCode，查可预约日期
  └─ 5. query-partystore-session   → 传入 spuId + storeCode + 日期，查可预约场次
  │
  ▼
  输出门店+日期+场次信息，引导用户自行预约
```

> 注意：派对类查询链路需要 spuId 作为入参，必须先通过 mall-points-products 获取活动对应的 spuId。

### 流程5：一键领券（串联3个工具 + 用户确认）

```
用户: "帮我领个券"
  │
  ├─ 1. available-coupons      → 查可领券列表
  ├─ 2. 展示券列表，等待用户确认
  ├─ 3. auto-bind-coupons      → 用户确认后执行领取
  └─ 4. query-my-coupons       → 确认领取结果
  │
  ▼
  告知领取结果，提醒使用期限
```

## 业务价值

| 价值维度 | 说明 |
|----------|------|
| **降低信息门槛** | 用户无需翻找麦当劳App各个入口，一句话获取全部活动信息 |
| **提升活动参与率** | 通过紧急度提醒和主动推送，减少用户错过活动的概率 |
| **个性化体验** | 根据用户积分、地理位置提供定制化推荐 |
| **全域覆盖** | 串联活动日历、优惠券、积分、商城、线下派对 5 大业务域，提供一站式活动情报 |
| **安全可控** | 仅一键领券为写操作且需二次确认，其他均为只读查询 |
