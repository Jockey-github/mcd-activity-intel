# 🕵️ 麦门活动情报局 (mcd-activity-intel)

<p align="center">
  <img src="./assets/banner.svg" alt="麦门活动情报局 Banner" width="1000" />
</p>

<p align="center">
  <strong>你的麦当劳活动私人情报员 — 不再错过任何薅羊毛的机会</strong>
</p>

<p align="center">
  <a href="#快速开始">快速开始</a> · <a href="#能干什么">功能介绍</a> · <a href="#对话效果预览">效果预览</a> · <a href="#mcp-工具使用">MCP 工具</a>
</p>

---

> 每次刷到"活动已结束"四个字，是不是血压直接拉满？

**麦门活动情报局**是你的麦当劳活动私人情报员 —— 帮你第一时间掌握所有活动动态，看清活动、看懂玩法、不再错过任何一次薅羊毛的机会。

基于 **麦当劳 MCP** 能力开发，串联 **12 个 MCP 工具**，覆盖活动日历、优惠券、积分、商城、线下派对 5 大业务域。

## 痛点

| 😤 以前 | 😎 现在 |
|---------|---------|
| 活动在哪看？App 里翻半天找不到 | 一句"最近有啥活动"，全部活动一览无余 |
| 活动规则写了一堆看不懂 | 情报员帮你翻译成人话，一句话说清玩法 |
| 活动结束了才知道有这回事 | 紧急度标签自动提醒，再也不错过 |
| 有券不知道，过期了才发现 | 一键查券+一键领券，该薅的一个不落 |
| 积分攒了一堆不知道干嘛 | 根据你的积分智能推荐最划算的花法 |
| 线下派对不知道自己城市有没有 | 报城市名，门店+日期+场次一步到位 |

## 能干什么

| 场景 | 你说 | 情报员做什么 |
|------|------|-------------|
| 📋 全量情报 | "最近有啥活动" | 本月全部活动，按紧急度分类展示 |
| ⚡ 今日精选 | "今天有啥可以薅的" | 仅展示今日可参与的，标注紧急程度 |
| 🎯 定向查询 | "有没有抽奖活动" | 按类型筛选：优惠券/积分/派对/抽奖 |
| 💰 积分推荐 | "我有多少积分能玩啥" | 查你积分余额，推荐最划算的花法 |
| 📍 附近可玩 | "上海有什么线下活动" | 查门店+可约日期+场次 |
| 🎫 一键领券 | "帮我领个券" | 查可领券→展示清单→确认后一键全领 |

## 快速开始

### 前置条件

1. 拥有麦当劳 MCP Token（[申请教程](https://github.com/M-China/mcd-mcp-server#1-%E7%94%B3%E8%AF%B7-mcp-token)）
2. 一个支持 MCP 的 AI 客户端（Claude Code / WorkBuddy / Cursor / Trae 等）

### 3 步跑起来

**第 1 步：配置 MCP Server**

将以下 JSON 添加到你的 MCP 客户端配置中，替换 `YOUR_MCP_TOKEN` 为你的真实 Token：

```json
{
  "mcpServers": {
    "mcd-mcp": {
      "type": "streamablehttp",
      "url": "https://mcp.mcd.cn",
      "headers": {
        "Authorization": "Bearer YOUR_MCP_TOKEN"
      }
    }
  }
}
```

**第 2 步：加载 Skill Prompt**

将 [`skill/system-prompt.md`](./skill/system-prompt.md) 的内容作为 System Prompt 加载到你的 AI 客户端中。

不同客户端的加载方式：

| 客户端 | 加载方式 |
|--------|---------|
| Claude Code | 将 `system-prompt.md` 内容添加到项目的 `CLAUDE.md` 文件中 |
| WorkBuddy | 创建专家，将 prompt 粘贴到专家设定中 |
| Cursor | 添加到 `.cursor/rules` 或 System Prompt 设置中 |
| Trae | 添加到自定义 System Prompt 设置中 |
| Cherry Studio | 创建 Agent，粘贴到 System Prompt 中 |

**第 3 步：开始对话**

在对话框中输入任何关于麦当劳活动的问题，比如：

```
最近有啥活动
```

情报员会自动接管，开始为你服务 🕵️

## 对话效果预览

### "最近有啥活动"

```
🕵️ 收到！情报员已接入麦当劳情报网络，正在扫描本月活动... 🔍

📊 2026年10月活动全景情报

共发现 5个进行中 + 2个即将开始 的活动：

━━━━━━━━━━━━━━━━━━━━
🏷️ 【麦麦省 · 10月优惠券专场】
📅 10月1日 - 10月31日 | 📌 还有21天，慢慢薅
💡 玩法：进入麦麦省页面，一键领取本月优惠券
🎁 奖励：巨无霸立减5元、麦辣鸡腿堡买一送一等
👉 参与方式：对我说"帮我领个券"，一键全领！
🎯 情报员点评：白送的券不领是跟钱包过不去 🐑
━━━━━━━━━━━━━━━━━━━━
(更多活动...)
```

### "帮我领个券"

```
🕵️ 让我看看现在有啥券可以薅... 🎫

📋 当前可领券清单（共5张）：
1. 巨无霸立减5元（10月31日前）
2. 麦辣鸡腿堡买一送一（10月20日前）
3. 中薯条免费加大（10月31日前）
4. 麦旋风第二杯半价（10月15日前）
5. 满30减6元（10月31日前）

⚠️ 第2张和第4张快过期了，领了尽快用！

回复"领"我就帮你一键全领～
```

> 更多对话示例请查看 [`skill/examples.md`](./skill/examples.md)

## MCP 工具使用

本项目串联了 **12 个** 麦当劳 MCP 工具：

| 业务域 | 工具 | 用途 |
|--------|------|------|
| 基础 | `now-time-info` | 获取当前时间，计算紧急度 |
| 活动日历 | `campaign-calendar` | 拉取本月全部活动（核心） |
| 优惠券 | `available-coupons` | 查可领券 |
| | `query-my-coupons` | 查已有券（去重） |
| | `auto-bind-coupons` | 一键领券 |
| 积分 | `query-my-account` | 查积分余额 |
| | `query-lottery-info` | 查抽奖活动 |
| | `mall-points-products` | 查积分商城 |
| 线下派对 | `query-party-city` | 查活动城市 |
| | `query-party-store` | 查门店 |
| | `query-partystore-date` | 查可约日期 |
| | `query-partystore-session` | 查可约场次 |

> 详细的 MCP 集成说明请查看 [`MCP_INTEGRATION.md`](./MCP_INTEGRATION.md)

## 项目结构

```
mcd-activity-intel/
├── README.md                    # 你正在看的这个文件
├── CONTEST_DECLARATION.md       # 参赛声明
├── MCP_INTEGRATION.md           # MCP 工具集成说明
├── mcp-config.example.json      # MCP 配置示例
├── skill/
│   ├── system-prompt.md         # 核心 Skill Prompt（灵魂所在）
│   └── examples.md              # 对话示例集
└── assets/
    └── (banner 等资源文件)
```

## 支持的客户端

任何支持 **Streamable HTTP** 协议的 MCP 客户端均可使用：

| 客户端 | 官网 |
|--------|------|
| WorkBuddy | [workbuddy.cn](https://www.workbuddy.cn/) |
| Cherry Studio | [cherry-ai.com](https://docs.cherry-ai.com/) |
| Cursor | [cursor.com](https://cursor.com/) |
| Kiro | [kiro.dev](https://kiro.dev/) |
| Trae | [trae.cn](https://docs.trae.cn/) |
| VS Code (Copilot) | [code.visualstudio.com](https://code.visualstudio.com/) |
| Claude Code | [claude.com/claude-code](https://claude.com/claude-code) |

## 关于

**麦门活动情报局**是 [麦当劳程序员创意开发大赛](https://github.com/M-China/mcd-developer-innovation-challenge) 的参赛作品。

本项目为参赛者独立开发，非麦当劳官方产品。项目输出仅供参考，不构成任何专业建议；餐品信息、价格及活动供应状态以麦当劳官方渠道的实时结果为准。

## License

MIT

