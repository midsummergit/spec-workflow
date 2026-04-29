# 灵感捕手 (hookly) — 需求规格说明书 V2.8 

> **灵感捕手** `hookly` — 网络爆款，帮你抓得住，拆得透，做得出。
>
> **定位**：受顾小北启发（[原文](https://mp.weixin.qq.com/s/zn9hOzOki79pdQM8r0jPaQ)），做切中真实需求要害的切实好用工具。不是 1:1 还原。
>
> **工程名**：`hookly` — repo / package / CLI 统一使用此名。
>
> **文档结构**：按增量交付分 Part。**P0 开发只需读 §A + §B + Part 0**。
>
> **更新日志**：
> - V1.0-V2.5 (2026-04-28~29): 多轮迭代，详见历史版本
> - V2.6 (2026-04-29): 专家终审闭合
- V2.7 (2026-04-29): 一致性修复
- **V2.8 (2026-04-29): 开工前终版** — 13项修复; CLI先行; 24字段统一; GPM/路径/交互/TikTok全对齐

---

## 目录

- **§A 项目背景与价值** — What / Why / 举例（项目级总览）
- **§B 增量交付路线总览** — P0/P1/P2 三阶段 + Artifact 演化一览表
- **Part 0: P0 个人素材工作流** — 7 天 · $0
- **Part 1: P1 团队增强** — 4-6 周
- **Part 2: P2 平台化 + 数据闭环** — 8-12 周
- **Part 3: P3+ 未来探索**
- **附录** — 竞品 · 术语 · Gap 清单

---

# §A 项目背景与价值

> **灵感捕手** `hookly` — 网络爆款，帮你抓得住，拆得透，做得出。

### A.1 What — 这是什么？

**灵感捕手。**

刷到一条好视频 → 转发链接给飞书 bot，说一声"入库" → 等 1-5 分钟 → bot 告诉你这条视频好在哪、Hook 是什么、话术怎么抄。所有拆解结果自动存起来，以后搜一下"露营 痛点反转"，相关素材秒出。

它不是从一开始就很复杂。三个阶段自然生长：
- **P0 现在**：你一个人的小工具。每天刷视频随手入库，周末看一眼报告"这周我存了什么好东西"。
- **P1 以后**：团队工具。大家共享素材库，谁入库了什么都看得见。
- **P2 以后**：平台。数据回来告诉你"哪种 Hook ROI 最高"。

### A.2 Why — 为什么做？

做电商内容的都知道：

你每天刷视频找灵感。看到好的就收藏、截图、转发。过几天想用的时候——链接找不到了，找到也忘了当初为什么觉得它好。收藏夹里几百条，能用的没有。

**现有的办法都有问题：**
- 收藏夹 / Notion / Excel → 只存链接，不存"为什么好"
- 手动标注 → 太累，做不到每天坚持
- 想搜"露营场景的痛点反转 Hook" → 这些工具根本做不到

**这产品不一样在哪？**

它不只是存链接、下载视频。核心价值是把视频拆成可搜索、可复用的创意模板卡片。

刷到一条好视频，丢链接给 bot。AI 告诉你：Hook 是什么类型、前 3 秒原文说了什么、打中了谁的什么焦虑、卖点怎么讲的、哪句话可以直接抄。12 个维度，一条素材变成一张"模板卡片"。以后搜一下"露营 痛点反转"，秒出。

周末还有报告：这周你存了什么、在关注什么方向、推荐试试什么新类型。不是冷冰冰的工具，像有个助手帮你整理、回顾、偶尔给你一个灵感。

> 💡 **举个例子**：小王做跨境电商，卖便携咖啡机。以前翻了 20 个收藏夹花 2 小时写一条脚本。现在每天刷视频时随手转发给 bot，周末 bot 推送报告："本周入库 28 条，最关注户外便携品类，痛点切入是主要素材来源。推荐补充场景植入类视频"。写脚本搜"露营 痛点反转"，5 条拆好的素材秒出——每条都告诉他 Hook 是什么、话术怎么抄。15 分钟出大纲。

---

# §B 增量交付路线总览

> **核心原则**：每个阶段只做当前阶段需要的事，不为未来的规模提前做架构。

## B.1 P0/P1/P2 三阶段总览

| 阶段 | 业务视角：实现什么功能 / 拿到什么价值 | 技术视角：用什么工具 / 做什么支撑 |
|-----|-----------------------------------|----------------------------------|
| **P0 个人自动化**<br>7 天 · $0 增量 | ✅ 刷视频→丢链接→自动入库<br>✅ AI 拆解 12 核心字段<br>✅ 飞书多维表搜索复用<br>▸ 日/周反馈报告（P0 增强，核心闭环跑通后做）<br>**价值：一个人每天坚持用的素材工作流** | 飞书 Bot · yt-dlp · LLM JSON 拆解<br>飞书多维表为主库 · 本地文件<br>**不引入**：OSS / 队列 / 审计 |
| **P1 团队增强**<br>4-6 周 | ✅ 2-10 人共享素材库<br>✅ TikTok 素材支持<br>✅ 电商经营标签体系<br>✅ 团队简报/审核提醒/标签检查<br>**价值：团队共享素材资产，标签驱动决策** | OSS 云存储 · SQLite · Playwright 反爬<br>多用户权限 · 标签治理<br>**不引入**：安全审计 / 幂等队列 |
| **P2 平台化+闭环**<br>8-12 周 | ✅ 数据回流（播放/互动/分享，GPM 需 P3 店铺授权）<br>✅ 素材→成品 归因分析<br>✅ 竞品监控 · AI 周报<br>✅ 数据亮点/竞品简报（回流数据驱动）<br>**价值：数据驱动创作策略，归因到 ROI** | PostgreSQL · 持久化任务队列<br>安全审计 · webhook 验证 · 幂等<br>结构化日志 · 监控告警 |



## B.2 Artifact 演化一览表



| # | Artifact | P0 | P1 | P2 |
|---|----------|:-----------:|:-----------:|:-----------:|
| 1 | 执行流程图 | 6 步简版 | +TikTok+OSS 8 步 | +数据拉取+报告 |
| 2 | 数据模型 / DDL | 1 表 24 字段 | +user_roles +FTS | +publications +performance +ideas +assets |
| 3 | 状态机（图+表） | 8 状态 | +审核/采纳/改编/发布 15 状态 | +已复盘 |
| 4 | API / 接口规格 | yt-dlp + lark-cli 基础 | +Playwright + OSS SDK | +Apify 全量 + TikHub + LLM API |
| 5 | AI Prompt（含 Schema） | 12 字段 JSON | +FABV+分镜+证据链 | +周报 Prompt + 竞品 Prompt |
| 6 | 用户交互流程 | 渐进式进度推送<br>+每日/每周/触发型反馈 | +团队协作交互<br>+团队简报 | +数据报告推送<br>+竞品简报 |
| 7 | 字段定义 | 12 核心 + 12 系统 | +B 层电商标签 | +数据反馈字段 |
| 8 | 错误处理策略 | 4 种降级 | +TikTok 下载降级 | +API 限流 + webhook 重放 |
| 9 | 技术架构图 | 飞书+本地 | +OSS+SQLite | +PG+队列+监控 |
| 10 | 验收 Checklist | 12 条 | +团队验收 | +数据闭环验收 |
| 11 | 成本估算 | ~$0 | $200-300/月 | $100-500/月 |
| 12 | Implementation Contract | 5 份子契约 | +角色权限契约 | +幂等+审计契约 |
| 13 | 飞书多维表 Schema | P0 建表（24 字段） | P1 全量建表 | P2 仅展示 |
| 14 | 部署 / 环境配置 | 简版（复用已有） | +OSS + SQLite + Playwright | +PG + Redis + 监控 |
| 15 | 目录 / 文件结构 | — | 🆕 多模块组织 | 微服务化 |
| 16 | NFR / SLO | 简版 | 🆕 团队级 | 生产级 |
| 17 | 标签分类体系 | — | 🆕 A/B 两层 | +效果标签 |
| 18 | 实体关系图 (ERD) | — | 🆕 2 表关系 | 6 表关系 |
| 19 | 用户角色矩阵 | — | 🆕 5 角色权限 | +多租户 |
| 20 | 迁移脚本 / 升级路径 | — | 🆕 P0→P1 完整 | P1→P2 完整 |
| 21 | 依赖清单 | — | 🆕 Python + Node 包 | +系统服务 |
| 22 | 监控 / 告警 | — | — | 🆕 完整 |
| 23 | 安全模型 | — | — | 🆕 完整 |
| 24 | 竞争分析 | — | — | 🆕 完整 |
| 25 | 演进接口 | P0→P1 (7 条) | P1→P2 (5 条) | P2→P3 (4 条) |

---

# Part 0: P0 — 个人素材工作流

> **目标**：7 天内搭出一个自己每天真用的灵感捕手。
> **P0 开发只需读 §A + §B + Part 0**，Part 1/2/3 是后续扩展。

## §0.0 P0 定位

**7 天内搭出一个自己每天真用的灵感捕手。**

P0 是整个系统的起点。不追求完整，追求**能用且每天在用**。这是"你一个人的灵感捕手"，不是平台，不是 SaaS。

## §0.1 一句话定义

**AI 视频素材拆解与复用工具** — 刷到好视频 → 丢链接给飞书 bot → AI 自动拆解 12 个维度 → 飞书多维表可检索素材库。

P0 阶段它是你**一个人的灵感捕手**。P1 之后才逐步成长为团队工具和平台。

## §0.2 核心成功标准

> 🎯 **唯一验收条件：7 天后，你还在每天用它。**

如果搭完后只在刚搭好两天兴奋，第三天不用了 → 失败。
如果一个很土的流程每天都在省时间 → 成功。

量化指标：
- 连续 3 天每天入库 ≥3 条
- 第 7 天至少完成 1 次"检索→复用"

## §0.3 MVP 范围（做什么 / 不做什么）

| ✅ 做 | ❌ 不做（留到 P1+） |
|------|-------------------|
| 飞书 bot 接收链接 + "入库" | TikTok/抖音稳定下载 |
| YouTube / Bilibili（核心） / X（best effort） / 手动上传 | GPM / ROI / 转化率 |
| yt-dlp 下载视频 | 仪表盘 / 数据可视化 |
| 优先内嵌字幕，无字幕则弱分析 | OSS 云存储 |
| ffmpeg 抽 3-5 帧 | 多用户 / 权限管理 |
| AI 输出 12 核心字段 | 竞品监控 |
| 飞书多维表写入 + bot 回复摘要 | 多阶段 Prompt 链 |
| 状态机 8 基础状态 | 任务队列 / 幂等 |
| — | — |

> **P0 增强**（核心闭环跑通后可后置）：每日小结、周报、里程碑、闲置提醒。详见 §0.4。

## §0.4 用户交互（渐进式进度反馈）

> **设计原则**：不刷屏。默认只发确认和结果，超时才提醒。

```
用户 在飞书 bot 发送:
  入库 https://www.youtube.com/watch?v=xxx 便携咖啡机 主打露营场景

┌─ Bot（<1s 立即确认）─────────────────────────┐
│ 🟢 已收到，开始入库处理中 ⏳                   │
└────────────────────────────────────────────┘

┌─ Bot（渐进式进度推送，每阶段一次）─────────────┐
└────────────────────────────────────────────┘

┌─ Bot（完成摘要）──────────────────────────────┐
│ ✅ 已入库                                      │
│ 平台: YouTube  |  品类: 便携咖啡机              │
│ Hook: "露营时发现速溶咖啡太难喝了"              │
│ 可复用点: 场景痛点切入 + 前后对比演示            │
│ 标签: #露营 #便携设备 #咖啡 #户外用品 #痛点解决   │
│                                                │
│ 查看详情 → [飞书多维表链接]                     │
└────────────────────────────────────────────┘
```

### 响应时间与进度策略

| 阶段 | 典型耗时 | 推送内容 | 备注 |
|-----|---------|---------|------|
| 链接解析 | <1s | 不单独推送 | 太快，合并到确认消息 |
| **总计（多数）** | **1-5 分钟** | — | 有现成字幕+短视频可 30-90s |

> **P0 核心闭环**：发链接→AI拆解→写入飞书→回复摘要→可搜索复用。以下定时反馈为 **P0 增强**（核心闭环跑通后再做，可后置）。

> **超时**：单阶段 >3 分钟无进度 → "仍在处理中，请稍候..."；全长 >8 分钟 → "处理超时，已保存已获取数据"。
> **失败**：任意阶段失败 → "⚠️ xxx 失败：<原因>"，保留已完成的中间数据。

## §0.5 字段定义

### 12 核心字段（AI 输出或用户指定）

| # | 字段 | 类型 | 说明 |
|---|------|------|------|
| 1 | 标题 | 文本 | 原始视频标题 |
| 2 | 平台 | 单选 | YouTube / Bilibili / X / 手动上传 |
| 3 | 原链接 | URL | |
| 4 | 产品/品类 | 文本 | 用户入库时指定或 AI 推断 |
| 5 | Hook 原文 | 文本 | 前 3 秒字幕原文（不可改写） |
| 6 | Hook 类型 | 单选 | 悬念反转 / 痛点刺激 / 视觉奇观 / 身份代入 / 热梗借用 / 数据冲击 |
| 7 | 核心痛点 | 文本 | 一句话 |
| 8 | 卖点 | 文本 | 产品最打动人的地方 |
| 9 | 场景 | 文本 | 使用场景描述 |
| 10 | 目标人群 | 文本 | |
| 11 | 可复用话术 | 文本 | 可直接抄的文案 |
| 12 | 标签 | 多选 | 3-5 个关键词 |

### 12 系统字段（自动填充）

| # | 字段 | 类型 | 说明 |
|---|------|------|------|
| S1 | 序号 | 自动编号 | 给人看的序号（1, 2, 3...） |
| S2 | 素材ID | 文本 | 系统 UUID，用于迁移和跨表关联 |
| S3 | 入库时间 | 日期 | 自动生成 |
| S4 | 状态 | 单选 | 等待下载 / 下载中 / 下载失败 / 待转写 / 待分析 / 已拆解 / 已确认 / 已归档 |
| S5 | 时长(秒) | 数字 | 视频时长 |
| S6 | 截图路径 | 文本 | 本地路径（P0 不做 OSS） |
| S7 | 视频文件路径 | 文本 | 本地路径 |
| S8 | 来源账号 | 文本 | 视频原作者/频道名 |
| S9 | 语言 | 单选 | zh / en / ja / ko / other |
| S10 | 分析置信度 | 数字(0-1) | AI 对本次分析的整体信心 |
| S11 | 去重键 | 文本 | platform + platform_video_id |
| S12 | 入库备注 | 文本 | 用户入库时的原始输入 |

## §0.6 AI Prompt（P0 精简版）+ JSON Schema

```markdown
## 角色
短视频内容拆解专家。

## 输入
- 转录: {transcript}（优先内嵌字幕，无字幕时用标题+简介）
- 截图: {screenshots}
- 品类: {product_category}
- 平台: {platform}
- 用户备注: {user_notes}

## 输出
{
  "title": "原视频标题",
  "platform": "YouTube|Bilibili|X|manual",
  "original_url": "https://...",
  "product_category": "用户指定或推断",
  "hook_text": "前3秒字幕原文，不可改写",
  "hook_type": "悬念反转|痛点刺激|视觉奇观|身份代入|热梗借用|数据冲击",
  "pain_point": "一句话描述痛点",
  "selling_point": "产品最打动人的地方",
  "scenario": "使用场景",
  "target_audience": "目标人群",
  "reusable_copy": "可直接复用的话术/文案",
  "tags": ["tag1", "tag2", "tag3"],
  "_system": {
    "schema_version": "p0_v1",
    "analysis_confidence": 0.8,
    "language": "zh",
    "source_account": "频道名"
  }
}

## 规则
1. 不确定的填"待确认"，不要瞎猜
2. hook_text 必须原文抓取，不可改写
3. tags 是关键词不是句子（每条 3-5 个）
4. 看不出产品 → product_category 填"通用"
5. 用户备注中的信息优先于 AI 推断
```

### JSON Schema（机器校验用）

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["title", "platform", "original_url", "product_category", "hook_text", "hook_type", "pain_point", "selling_point", "scenario", "target_audience", "reusable_copy", "tags"],
  "properties": {
    "title": {"type": "string", "minLength": 1},
    "platform": {"type": "string", "enum": ["YouTube", "Bilibili", "X", "manual"]},
    "original_url": {"type": "string", "format": "uri"},
    "product_category": {"type": "string", "minLength": 1},
    "hook_text": {"type": "string", "minLength": 1},
    "hook_type": {"type": "string", "enum": ["悬念反转", "痛点刺激", "视觉奇观", "身份代入", "热梗借用", "数据冲击"]},
    "pain_point": {"type": "string"},
    "selling_point": {"type": "string"},
    "scenario": {"type": "string"},
    "target_audience": {"type": "string"},
    "reusable_copy": {"type": "string"},
    "tags": {"type": "array", "items": {"type": "string"}, "minItems": 3, "maxItems": 5},
    "_system": {
      "type": "object",
      "properties": {
        "schema_version": {"type": "string", "const": "p0_v1"},
        "analysis_confidence": {"type": "number", "minimum": 0, "maximum": 1},
        "language": {"type": "string", "enum": ["zh", "en", "ja", "ko", "other"]},
        "source_account": {"type": "string"},
        "needs_human_review": {"type": "boolean"},
        "risk_flags": {"type": "array", "items": {"type": "string"}}
      }
    }
  }
}
```

## §0.7 技术架构 + 执行流程

### 技术架构图（P0）

```
┌──────────────────────────────────────────────────────────┐
│                        用户手机                           │
│              飞书 App → 发送"入库 <链接>"                  │
└──────────────────────┬───────────────────────────────────┘
                       │
┌──────────────────────▼───────────────────────────────────┐
│                    触发层                                 │
│  飞书 Bot → OpenClaw 当前 channel（零额外配置）           │
└──────────────────────┬───────────────────────────────────┘
                       │
┌──────────────────────▼───────────────────────────────────┐
│                    处理层                                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────┐ │
│  │ yt-dlp   │  │ ffmpeg   │  │ LLM      │  │lark-cli │ │
│  │ 下载视频  │  │ 抽帧3-5  │  │ JSON拆解 │  │ 写多维表 │ │
│  └──────────┘  └──────────┘  └──────────┘  └─────────┘ │
└──────────────────────┬───────────────────────────────────┘
                       │
┌──────────────────────▼───────────────────────────────────┐
│                    存储层                                 │
│  本地文件（视频/截图/transcript） + 飞书多维表            │
└──────────────────────────────────────────────────────────┘
```

### 自动执行流程图（P0）

```
用户发送"入库 <链接> <品类> <备注>"
         │
         ▼
    ┌─────────────┐
    │ 1. 消息解析   │  提取 URL、品类、备注 → 立即回复"已收到 ⏳"
    └──────┬──────┘
           │
    ┌──────▼──────┐
    │ 2. 视频下载   │  yt-dlp {url} → 本地文件
    └──┬───────┬──┘
       │ 成功   │ 失败（重试3次后）
       │       └──→ 状态=下载失败，存链接+备注，通知用户
       │
    ┌──▼──────────┐
    │ 3. 字幕提取   │  优先内嵌字幕（--write-subs --write-auto-subs）
    └──┬───────┬──┘
       │有字幕  │ 无字幕
       │       └──→ 用标题+简介做弱分析（仍产出基础字段）
       │
    ┌──▼──────┐
    │ 4. 抽帧   │  ffmpeg -vf fps=1/3 → 取首/中/末 3 帧
    └──┬──────┘
       │
    ┌──▼──────┐
    │ 5. AI分析 │  调 LLM with Prompt（§0.6）→ JSON
    │          │  → JSON Schema 校验 → 不合法则 retry 1 次
    └──┬───────┬──┘
       │ 成功   │ AI 失败
       │       └──→ 状态=待分析，保留已有数据，通知用户
       │
    ┌──▼──────────┐
    │ 6. 写入飞书   │  lark-cli base records create → 24 字段
    └──┬──────────┘
       │
    ┌──▼──────┐
    │ 7. 回复   │  Bot 推摘要 ✅（Hook/卖点/标签/链接）
    │          │  状态=已拆解
    │          │  若全程 >60s → 中间插入一条状态推送
    └─────────┘

定时触发（cron / OpenClaw cron jobs）:
  ┌──────────────────────────────────────┐
  │ 每日 21:00: 统计今日入库 → 推送小结   │
  │ 每周一 09:00: 统计本周 → LLM 一句话  │
  │                → 推送周报            │
  │ 事件触发: 素材数达到里程碑 → 庆祝消息 │
  │          连续3天未入库 → 闲置提醒     │
  └──────────────────────────────────────┘
```

## §0.8 API / 接口规格（P0）

### yt-dlp 下载命令

```bash
# YouTube/Bilibili 下载
yt-dlp \
  -f "bestvideo[height<=1080]+bestaudio/best[height<=1080]" \
  --write-subs \
  --write-auto-subs \
  --sub-lang "en.*,zh.*,zh-Hans,zh-Hant" \
  --convert-subs srt \
  -o "../data/hookly/%(extractor)s/%(upload_date>%Y%m%d)s/%(id)s/%(id)s.%(ext)s" \
  "{url}"

# 推荐分两步（避免 stdout 混乱）
# Step 1: 获取元信息
yt-dlp --dump-json "{url}" | jq '{title, duration, upload_date, channel, subtitles}' > metadata.json
# Step 2: 下载视频+字幕
yt-dlp -f "bestvideo[height<=1080]+bestaudio/best[height<=1080]" \
  --write-subs --write-auto-subs --sub-lang "en.*,zh.*" --convert-subs srt \
  -o "../data/hookly/%(extractor)s/%(upload_date>%Y%m%d)s/%(id)s/%(id)s.%(ext)s" "{url}"

# 仅获取信息（不下载）
yt-dlp --dump-json "{url}" | jq '{title, duration, upload_date, channel, subtitles}'
```

### lark-cli 基础命令（P0 够用）

```bash
# 安装
npm install -g @larksuite/cli

# 认证（一次性）
lark-cli auth login

# 多维表操作
lark-cli base records list --app-token <id> --table-id <id>
lark-cli base records create --app-token <id> --table-id <id> --records '[{"fields": {...}}]'
lark-cli base fields list --app-token <id> --table-id <id>

# 消息发送
lark-cli im messages send --receive-id <open_id> --msg-type text --content "..."

# 搜索
lark-cli base records search --app-token <id> --table-id <id> --query "露营 痛点反转"

# ── 定时主动反馈（P0 内置功能）──

# 每日小结（cron 每日 21:00）— 纯统计，无需 LLM
lark-cli base records search --app-token <id> --table-id <id> \
  --filter 'CurrentValue.[入库时间] >= Today()' --limit 1000  # P0 用飞书字段名；P1 SQLite 用 stored_at
# 统计后 → lark-cli im messages send 推送

# 每周报告（cron 周一 09:00）— 统计 + LLM 一句话
# 1. 同上面搜索上周记录并统计
# 2. LLM call: "输入上周入库{N}条，Top品类{}, Top Hook{}, Top标签{}。用一句话总结关注趋势并推荐互补方向(<80字)"
# 3. lark-cli im messages send 推送结果
# 成本: ~$0.01/次（1 次 LLM call, ~200 tokens out）
```

## §0.9 数据模型（P0）

### 单表 DDL（飞书多维表映射）

```yaml
表名: hookly_P0
字段总数: 24（12 核心 + 12 系统）

12 核心字段:
  - 标题: 文本（必填）
  - 平台: 单选（YouTube | Bilibili | X | 手动上传）（必填）
  - 原链接: URL（必填）
  - 产品/品类: 文本（必填）
  - Hook原文: 文本（必填）
  - Hook类型: 单选（悬念反转 | 痛点刺激 | 视觉奇观 | 身份代入 | 热梗借用 | 数据冲击）（必填）
  - 核心痛点: 文本
  - 卖点: 文本
  - 场景: 文本
  - 目标人群: 文本
  - 可复用话术: 文本
  - 标签: 多选

12 系统字段:
  - 序号: 自动编号（给人看）
  - 素材ID: 文本（UUID，系统生成）
  - 入库时间: 日期（自动）
  - 状态: 单选（等待下载 | 下载中 | 下载失败 | 待转写 | 待分析 | 已拆解 | 已确认 | 已归档）
  - 时长(秒): 数字
  - 截图路径: 文本
  - 视频文件路径: 文本
  - 来源账号: 文本
  - 语言: 单选（zh | en | ja | ko | other）
  - 分析置信度: 数字
  - 去重键: 文本
  - 入库备注: 文本
```

## §0.10 状态机（P0）

### 状态图

```
等待下载 ──→ 下载中 ──→ 下载失败（存链接+标记）
                │
                ▼
             待转写 ──→ 待分析 ──→ 已拆解 ──→ 已确认 ──→ 已归档
```

### 状态转换规则

| 当前状态 | 可流转到 | 触发条件 | 重试 |
|---------|---------|---------|------|
| 等待下载 | 下载中 | 系统拾取 | — |
| 下载中 | 待转写 | 下载成功 | — |
| 下载中 | 下载失败 | 重试 3 次均失败 | 3 次 |
| 待转写 | 待分析 | 字幕提取完成 | — |
| 待分析 | 已拆解 | AI 输出合法 JSON | — |
| 已拆解 | 已确认 | 用户查看确认 | — |
| 已确认 | 已归档 | 手动归档 | — |
| 下载失败 | 等待下载 | 人工重试 | — |

## §0.11 错误处理策略（P0）

| 失败场景 | 处理 | 用户看到 |
|---------|------|---------|
| 下载失败（重试3次后） | 状态=下载失败，存链接+备注 | "⚠️ 下载失败：<原因>。已保存链接，可稍后重试" |
| 无字幕 | 用标题+简介做弱分析（降级） | 正常回复（字段可能标记"待确认"） |
| AI 输出不合法 JSON | Retry 1 次 → 仍失败则状态=待分析 | "⚠️ AI 分析失败，可手动重试" |
| 飞书写入失败 | Retry 3 次 → 本地缓存 → 定时重试 | "⚠️ 写入素材库失败，已缓存，稍后自动重试" |
| 重复入库 | 不创建记录，返回已有记录 | "该素材已存在：<标题> [查看→链接]" |
| 单阶段超时 >3分钟 | 推送进度提示 | "仍在处理中，请稍候..." |
| 全长超时 >8分钟 | 保留已获取数据 | "处理超时，已保存已获取数据，可稍后重试" |

## §0.12 数据库策略

| 阶段 | 主数据库 | 飞书角色 | 切换条件 |
|-----|---------|---------|---------|
| **P0** | 飞书多维表 | 唯一 source of truth | 当前 |
| **P1** | SQLite + 飞书 | 飞书降为展示层 | 数据量>500条或多表查询 |
| **P2** | PostgreSQL + 飞书 | 飞书仅展示 | 多用户 SaaS 化 |

## §0.13 成本估算

| 项目 | P0 成本 | 说明 |
|------|--------|------|
| 服务器 | $0 | 复用已有 OpenClaw 环境 |
| 存储 | $0 | 本地文件，视频定期清理 |
| AI 模型 | $0 增量 | 用当前订阅 |
| 飞书 | $0 | 已有 channel |
| Apify/TikHub | $0 | P1 才需要 |
| Whisper API | $0 | P0 不依赖（降级策略） |
| 周报 LLM | ~$0.01/周 | 1 次简短 LLM call |
| **P0 总增量** | **≈ $0** | |

## §0.14 部署 / 环境配置（P0）

```bash
# 系统依赖
sudo apt install yt-dlp ffmpeg

# 目录结构
mkdir -p ../data/hookly/  # 默认相对路径；部署时可通过 HOOKLY_DATA_DIR 环境变量覆盖{youtube,bilibili,x,manual}
mkdir -p "$HOOKLY_DATA_DIR/screenshots"

# OpenClaw Feishu channel 已配置，无需额外设置

# lark-cli 认证
lark-cli auth login  # 按提示完成
lark-cli base fields list --app-token <多维表app_token> --table-id <表id>  # 验证

# 环境变量
export FEISHU_APP_TOKEN="xxx"
export FEISHU_TABLE_ID="xxx"
export FEISHU_BOT_OPEN_ID="xxx"
```

## §0.15 执行顺序（风险优先）

> 按"哪里最可能炸"排序，最少返工。

| 天 | 任务 | 风险点 |
|----|------|--------|
| **Day 0** | **飞书多维表写入验证**（建表+假数据） | 🔴 最大不确定点 |
| Day 1 | 本地 CLI：yt-dlp 下载 + 字幕提取 → metadata.json | yt-dlp 兼容性 |
| Day 2 | 本地 CLI：AI JSON 拆解 → analysis.json + Schema 校验 | prompt 质量 |
| Day 3 | 本地 CLI：`--write-feishu` → 端到端写入飞书表 | 飞书写入 |
| Day 4 | 接飞书 Bot 指令解析 + 状态机 + 去重 + 回复 | 消息路由 |
| Day 5 | 降级处理 + bug 修复 + 狗粮测试开始 | 鲁棒性 |
| Day 6-7 | 狗粮：每天入库 ≥3 条 + 检索复用。P0 增强反馈模块在核心闭环稳定后逐步加 | 验收 |

## §0.16 验收 Checklist

- [ ] Day 0：飞书多维表写入测试通过（一条假数据）
- [ ] "入库 <链接> <品类> <备注>" → bot 立即回复"已收到 ⏳"
- [ ] YouTube ≥3 条成功下载+拆解
- [ ] Bilibili ≥1 条成功下载+拆解
- [ ] X best effort，不进硬验收
- [ ] 多数 1-5 分钟内收到完成回复
- [ ] 飞书多维表记录完整 24 字段
- [ ] 下载失败 → 降级处理（存链接+标记）
- [ ] 无字幕 → 弱分析降级
- [ ] 重复入库 → "该素材已存在"
- [ ] 用户备注保留（如"主打露营场景"）
- [ ] 连续 3 天每天入库 ≥3 条
- [ ] 搜索标签找到之前入库的素材
- [ ] 至少 1 次"检索→复用"事件
- [ ] P0 Build Contract 验证通过（§0.18）

── P0 增强（核心闭环跑通后实现，可后置）──
- [ ] 每日 21:00 收到今日入库小结
- [ ] 连续入库 → 每日推送带 🔥 连续天数
- [ ] 素材达 50 条 → 收到里程碑庆祝
- [ ] 连续 3 天未入库 → 收到闲置提醒
- [ ] 周一 09:00 收到周报（含 AI 一句话总结）

## §0.17 P0 Implementation Contract

### (a) 飞书表字段契约
见 §0.9 — 24 字段完整定义。

### (b) LLM JSON Schema 契约
见 §0.6 — 开发时直接用此 Schema 做 JSON repair + validation。

### (c) 去重键规则
```
canonical_key = f"{platform}:{video_id}"
# YouTube: URL v=参数 → "youtube:dQw4w9WgXcQ"
# Bilibili: URL video/后 → "bilibili:BV1xx411c7mD"
# X: URL status/后 → "x:123456789"
# 无法提取 → normalization + SHA256(url) → "hash:abc..."
```

### (d) 状态转换规则
见 §0.10。

### (e) 飞书写入行为
```
成功: 状态=已拆解 → 回复摘要 ✅
下载失败: 状态=下载失败 → 保留链接+备注 → 通知原因
AI 失败: 状态=待分析 → 保留已有 → "分析失败，可手动重试"
重复: 不创建 → "该素材已存在：<标题>" + 已有链接
部分成功: 写已完成字段 → 状态标记失败阶段
手动上传（P0.5）: 用户发视频文件 + "入库 <品类> <备注>" → 跳过下载 → 抽帧+AI+写表。Feishu 文件下载能力验证后再做
```

## §0.18 P0 Build Contract（开工前验证）

> **来源**：Codex V3 审查。P0 最大风险是飞书/Bot/Schema/LLM 边界的集成漂移。

### (a) 飞书端验证清单

```
□ 多维表已创建（hookly_P0），24 字段，类型与 §0.5（字段定义）、§0.9（数据模型）一致
□ 单选字段枚举值已配置（平台/Hook类型/状态/语言）
□ 多选字段（标签）已配置
□ app_token 和 table_id 已知
□ Bot 有该多维表的读写权限
□ 测试写入一条假数据成功
□ Bot 能向用户发消息成功
```

### (b) Schema 归一化映射

> Prompt 输出 JSON key → 飞书字段名。§0.5、§0.6、§0.11 均以此为准。

| JSON key | 飞书字段名 | 类型 | 约束 |
|-----|------|------|------|
| `title` | 标题 | 文本 | 必填 |
| `platform` | 平台 | 单选 | YouTube/Bilibili/X/手动上传 |
| `original_url` | 原链接 | URL | 必填 |
| `product_category` | 产品/品类 | 文本 | 必填 |
| `hook_text` | Hook原文 | 文本 | 必填 |
| `hook_type` | Hook类型 | 单选 | 同上枚举 |
| `pain_point` | 核心痛点 | 文本 | |
| `selling_point` | 卖点 | 文本 | |
| `scenario` | 场景 | 文本 | |
| `target_audience` | 目标人群 | 文本 | |
| `reusable_copy` | 可复用话术 | 文本 | |
| `tags` | 标签 | 多选 | 3-5 items |
| `_system.analysis_confidence` | 分析置信度 | 数字 | 0-1 |
| `_system.language` | 语言 | 单选 | zh/en/ja/ko/other |
| `_system.source_account` | 来源账号 | 文本 | |

### (c) Fixture 视频（3 条开发测试用例）

| # | 类型 | 说明 | 预期结果 |
|---|------|------|---------|
| 1 | YouTube 有字幕 | ≤3min 英文视频 | 下载→字幕→12 字段完整 |
| 2 | YouTube 无字幕 | 无 CC 字幕 | 下载→弱分析→部分"待确认" |
| 3 | 无效/私密链接 | 不存在 URL | 下载失败→状态=下载失败→降级 |

### (d) Day 0 Smoke Test

```bash
#!/bin/bash
# Test 1+2 不通过则先修环境，不进 Day 1

# 1. 飞书写入
# 0. 先检查字段列表
lark-cli base fields list --app-token $FEISHU_APP_TOKEN --table-id $FEISHU_TABLE_ID
# 确认: 24 字段存在；单选枚举已配；URL/多选类型正确

# 1. 飞书写入
lark-cli base records create --app-token $FEISHU_APP_TOKEN --table-id $FEISHU_TABLE_ID \
  --records '[{"fields": {
    "标题":"SMOKE_TEST","平台":"YouTube","原链接":"https://example.com",
    "产品/品类":"测试","Hook原文":"test","Hook类型":"痛点刺激",
    "去重键":"youtube:smoke_test_001"}}]'

# 2. 飞书推送
lark-cli im messages send --receive-id $FEISHU_BOT_OPEN_ID \
  --msg-type text --content "✅ hookly Day 0 smoke test passed"

# 3. yt-dlp
yt-dlp --version

# 4. LLM 可达性
# 用已有 LLM 调用方式发一个最小请求

echo "Smoke tests done."
```

> **门禁**：Test 1+2 必须通过才进 Day 1。Test 3+4 有问题可并行排查。

## §0.19 为 P1 预留的演进接口

| # | P0 设计 | P1 会怎么用 |
|---|---------|-----------|
| 1 | 素材ID 用 UUID | 迁移 SQLite/PostgreSQL 时 ID 全局唯一 |
| 2 | JSON Schema 带 `schema_version` | P1 扩展 prompt 时多版本兼容 |
| 3 | 文件路径 `{platform}/{date}/{video_id}/` | P1 切 OSS 一行 prefix 替换 |
| 4 | 标签用多选数组（flat） | P1 扩展 A/B 分层时不改表结构 |
| 5 | 状态机 8 状态只增不删 | P1 在原状态间插入新状态 |
| 6 | 24 字段已确定 | P1 SQLite 表直接映射 |
| 7 | bot 消息与下载/分析解耦 | P1 引入队列时只换入口 |

---

# Part 1: P1 — 团队增强

> **前置条件**：P0 验收通过。**本 Part 自包含** — 只写相对于 P0 的新增和变更（增量 diff）。
> **目标**：4-6 周内，把个人工具加固为 2-10 人团队可用的素材管理系统。
> **新增 Artifact**：用户角色矩阵 · ERD · 标签体系 · 完整 Prompt · 目录结构 · 依赖清单 · 迁移脚本 · NFR · 部署配置

## §1.0 P1 定位

### P0 → P1 关键变化

| 维度 | P0 | P1 |
|-----|-----|-----|
| 用户 | 1 人 | 2-10 人 |
| 存储 | 本地文件 | OSS 云存储 |
| 数据库 | 飞书多维表为主库 | SQLite 为主库，飞书为展示层 |
| 素材来源 | YouTube/Bilibili/X | + TikTok/抖音 |
| 标签 | 12 核心 flat tags | A/B 两层电商经营标签 |
| 工作流 | 入库→拆解 | + 审核→改编→发布 |
| 反馈 | 实时进度 + 每日小结 + 周报 + 里程碑 + 闲置提醒 | + 团队简报 + 审核提醒 + 标签健康检查 |
| 成本 | $0 增量 | $200-300/月 |

### P1 设计哲学

**工程加固（Production Hardening）** — 把 P0 能跑的代码变得可靠、可维护、其他人也能用。不是只加功能。

### P1 验收标准

- [ ] 2 人可同时从同一素材库搜索、入库
- [ ] TikTok 素材入库成功率 60-70%（非硬 SLA，失败降级保存链接+基础信息）
- [ ] OSS 上可访问已上传的视频和截图
- [ ] 按电商经营标签筛选素材
- [ ] 素材状态含"已采纳/已改编/已发布"

## §1.1 用户角色矩阵（P1 新增）

| 角色 | 人数 | 核心操作 | 权限边界 |
|-----|------|---------|---------|
| 管理员 | 1-2 | 标签治理、权限配置、删除/归档 | 全部读写 |
| 运营/采购 | 2-5 | 入库、浏览、搜索、打标 | 读写自己的入库记录，读全部 |
| 内容策划 | 2-3 | 拆解分析、标签管理、创意复用 | 读全部，写标签/分析字段 |
| 剪辑师 | 1-3 | 下载素材、查看分镜 | 只读 |
| 管理者 | 1 | 看数据、定策略 | 只读 |

### 实现方案

```
飞书多维表原生权限 + 应用层角色映射

飞书侧: 多维表分享设置 → 添加成员（可编辑/可查看）
应用层: bot 识别 open_id → 查角色表 → 校验权限

配置文件 config/roles.yaml:
  admins: [ou_xxx, ou_yyy]
  operators: [ou_aaa, ou_bbb]
  planners: [ou_ccc]
  viewers: [ou_ddd]
```

## §1.2 用户交互增量（P1）

在 P0 渐进式进度基础上，P1 增加团队协作交互：

```
# 搜索素材
用户: 搜索 露营 咖啡 痛点反转
Bot:  找到 5 条素材：
      1. [YouTube] "露营咖啡机测评" | Hook: 痛点刺激 | 标签: #露营 #咖啡
      2. ...

# 协作标记
用户: 采纳 om_xxx（素材ID）
Bot:  ✅ 已标记为"已采纳"。策划 <name> 可以开始改编。

# 查看团队活跃
用户: 本周入库
Bot:  本周团队入库 42 条 | 你: 15 条 | <同事A>: 12 条 | <同事B>: 15 条
```

### P1 主动反馈增量

在 P0 已有的每日小结和每周报告基础上，P1 新增团队级反馈：

```
# 团队每日简报（附在 P0 每日小结中）
👥 今日团队入库 12 条 | 你:3 | A:5 | B:4
📌 待审核: 5 条 | 待修正: 2 条

# 审核提醒（每日 09:00，有待审核素材时触发）
📌 你有 5 条素材待审核（超48h标红）:
1. YouTube: "露营咖啡机测评" — 2 天前
2. YouTube: "便携灯对比" — 1 天前
回复"审核"开始处理。

# 标签健康检查（每周一，未打B层标签>10条时触发）
🏷️ 素材库有 20 条缺少 B 层电商标签。有标签的素材搜索命中率高 3 倍。
回复"补标"一键补标。
```

## §1.3 数据模型增量（P1）

### 实体关系图 (ERD)

```
┌──────────────┐         ┌──────────────┐
│  materials   │         │  user_roles  │
│              │         │              │
│ id (PK)      │         │ open_id (PK) │
│ title        │         │ role         │
│ platform     │         │ display_name │
│ ... (24字段)  │         │ added_at     │
│ created_by ──┼───────→│              │
│ tags          │         └──────────────┘
│ tags_business │ (P1 新增列)
│ status        │
└──────────────┘
```

### SQLite DDL（P1 新增）

```sql
-- 素材主表（扩自 P0 飞书 24 字段）
CREATE TABLE materials (
    id TEXT PRIMARY KEY,             -- UUID
    title TEXT NOT NULL,
    platform TEXT NOT NULL,
    original_url TEXT NOT NULL,
    product_category TEXT NOT NULL,
    hook_text TEXT NOT NULL,
    hook_type TEXT NOT NULL,
    pain_point TEXT,
    selling_point TEXT,
    scenario TEXT,
    target_audience TEXT,
    reusable_copy TEXT,
    tags TEXT,                       -- JSON array
    
    -- 系统字段
    stored_at TEXT NOT NULL DEFAULT (datetime('now')),
    status TEXT NOT NULL DEFAULT '等待下载',
    duration_seconds INTEGER,
    screenshot_path TEXT,
    video_file_path TEXT,            -- P1: OSS URL
    source_account TEXT,
    language TEXT DEFAULT 'unknown',
    analysis_confidence REAL,
    duplicate_key TEXT UNIQUE,
    user_notes TEXT,
    created_by TEXT NOT NULL,        -- P1 新增
    
    -- P1 新增：电商标签
    content_direction TEXT,          -- 种草|测评|教程|对比|vlog|开箱|场景植入
    tags_business TEXT,              -- JSON: B层标签
    shooting_difficulty TEXT,
    replication_cost TEXT
);

-- 全文搜索
CREATE VIRTUAL TABLE materials_fts USING fts5(
    title, product_category, hook_text, pain_point,
    selling_point, scenario, target_audience, reusable_copy, tags
);

-- 用户角色表
CREATE TABLE user_roles (
    open_id TEXT PRIMARY KEY,
    role TEXT NOT NULL,
    display_name TEXT,
    added_at TEXT DEFAULT (datetime('now'))
);
```

## §1.4 字段增量：B 层电商经营标签（P1 新增）

> P0 的 12 核心字段不变。P1 在 materials 表上增列，扩展为 A/B 两层标签。

### A 层：内容拆解标签（P0 已有，P1 增强）

| 维度 | P0 | P1 增强 |
|-----|-----|--------|
| Hook 类型 | 单选 6 类 | 不变 |
| 内容方向 | — | 种草/测评/教程/对比/vlog/开箱/场景植入 |
| 演示类型 | — | 场景化演示/对比测试/细节特写/开箱/用户证言 |
| 风格属性 | — | 视觉风格/音乐调性/节奏/色彩（供剪辑师筛选） |
| 创意资产 | 可复用话术 | 镜头分镜/视觉手法/音频元素 |

### B 层：商业决策标签（P1 全新）

```yaml
市场与渠道:
  target_market: [美国, 英国, 东南亚, 中东, 拉美, 日韩, 欧洲]
  sales_channel: [TikTokShop, Amazon, Shopify, Shopee, Temu, 独立站]
  traffic_type: [自然流, 付费投流, 达人带货, 直播, 混合]

品类与定价:
  product_category_l1: string    # 一级类目
  product_category_l2: string    # 二级类目
  product_price_band: [低价<$10, 中价$10-50, 高价$50-200, 超高$200+]
  purchase_intent: [冲动消费, 比价决策, 解决问题, 送礼, 自我奖赏]
  seasonality: [四季常青, 季节性, 节日型, 趋势型]
  repurchase_potential: [高频复购, 偶尔复购, 一次性]

制作成本:
  shooting_difficulty: [手机可拍, 需要灯光, 专业设备, 需要演员, 需要外景]
  editing_difficulty: [简单剪辑, 中等特效, 复杂后期]
  replication_cost: [极低<$50, 低$50-200, 中$200-500, 高$500+]

合规:
  copyright_risk: [无风险, 需标注来源, 不可直接复用]
  claim_risk: [无功效声明, 轻微声明, 强功效声明需资质]

商业评分（P1: AI 建议 + 人工确认；P2: 数据回流校准）:
  awareness_score: 1-10 | conversion_intent_score: 1-10
  ugc_fit_score: 1-10   | ad_fit_score: 1-10
  shop_fit_score: 1-10  | risk_score: 1-10
  overall_suitability: 1-10
```

## §1.5 标签使用规范（P1）

```yaml
标签索引与搜索:
  每个素材: 5-8 个标签（行业最佳实践）
  三级结构: core(1-2) + precision(2-3) + long_tail(1-2)
  标签必须是关键词，不是完整句子
  
AI 自动打标:
  根据素材内容推荐标签
  根据热度数据自动排序标签权重
  定期清理低效标签
  
搜索:
  SQLite FTS5 全文搜索 + 标签过滤
  飞书多维表仅同步摘要供手动浏览
```

## §1.6 AI Prompt 增量：P1 完整拆解体系

在 P0 12 字段基础上，P1 增加完整拆解 Prompt（FABV + 分镜 + 证据链 + 置信度）：

```markdown
## 角色
跨境/带货短视频内容分析专家。精通 TikTok/抖音/YouTube Shorts 算法和爆款创作。

## 输入
- 转录: {transcript} | 截图: {screenshots} | 平台: {platform}
- 产品类目: {product_category}

## 分析方法
### 第一步：结构拆解
识别 Hook(0-3s)→Problem(3-10s)→Proof(10-25s)→CTA(最后5s) 边界

### 第二步：FABV 卖点分析
Feature→Advantage→Benefit→Value

### 第三步：元素提取
可复用话术/视觉手法/音频元素/分镜表

### 第四步：标签生成
core(1-2)+precision(2-3)+long_tail(1-2)

## 输出（严格 JSON）
{
  "_metadata": {
    "schema_version": "1.1", "prompt_version": "analysis_p1_v1",
    "analysis_confidence": 0.78, "needs_human_review": true,
    "risk_flags": ["product_unclear"]
  },
  "hook": {
    "type": "悬念反转|痛点刺激|...",
    "text": "原文字幕", "duration_seconds": 3, "confidence": 0.85,
    "evidence": [{"type":"transcript","timestamp_start":0,"timestamp_end":3,"text":"..."}]
  },
  "problem": { "scenario": "...", "emotion_trigger": "认同感|好奇|焦虑|渴望", "confidence": 0.8 },
  "proof": { "type": "场景化演示|对比测试|细节特写|开箱|证言", "before_after": true, "confidence": 0.75 },
  "cta": { "type": "购物车引导|关注引导|评论互动|链接点击", "timing_seconds": 25 },
  "fabv": { "feature": "...", "advantage": "...", "benefit": "...", "value": "..." },
  "tags": { "core":["..."], "precision":["..."], "long_tail":["..."] },
  "business_tags": { "target_market":["美国"], "price_band":"中价", "seasonality":"四季常青", ... },
  "creative_assets": {
    "copy_templates": [{"text":"...","usage":"开头钩子"}],
    "shot_list": [{"order":1,"duration":3,"type":"特写","content":"...","key":true}],
    "audio_elements": [{"type":"BGM","name":"...","mood":"欢快"}]
  },
  "quality_scores": { "awareness_score":7, "conversion_intent_score":8, "ugc_fit_score":6, "ad_fit_score":7, "shop_fit_score":5, "risk_score":2, "overall_suitability":7 }
}
```

## §1.7 技术架构 + 执行流程（P1 版本）

### 技术架构图（P1）

```
┌──────────────────────────────────────────────────────────────┐
│                         用户层                               │
│          飞书 App（多用户）→ 入库/搜索/审核/改编               │
└──────────────────────────┬───────────────────────────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│                       触发层                                 │
│     飞书 Bot + 角色鉴权（查 user_roles 表）                   │
└──────────────────────────┬───────────────────────────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│                       处理层                                 │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐       │
│  │ yt-dlp   │ │Playwright│ │ ffmpeg   │ │ LLM      │       │
│  │ YT/Bili  │ │ TikTok   │ │ 抽帧     │ │ P1完整   │       │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘       │
│  ┌──────────────────────────────────────────────────┐       │
│  │              lark-cli（读写飞书）                   │       │
│  └──────────────────────────────────────────────────┘       │
└──────────────────────────┬───────────────────────────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│                   存储层（P1 升级）                           │
│  OSS 云存储（视频/截图） + SQLite（主库）+ 飞书多维表（展示层）│
└──────────────────────────────────────────────────────────────┘
```

### 执行流程图（P1 版本 — 在 P0 基础上加 TikTok 和 OSS）

```
用户发送"入库 <链接>"（支持 TikTok 链接）
         │
    ┌────▼─────┐
    │ 1. 解析   │  判断平台 → 选下载器（yt-dlp 或 Playwright）
    │  + 鉴权   │  查 user_roles → 校验入库权限
    └────┬─────┘
         │
    ┌────▼──────────────┐
    │ 2. 下载（双引擎）   │
    │ YT/Bili/X: yt-dlp  │
    │ TikTok: Playwright │
    │ → 推送"正在下载..." │
    └────┬──────┬────────┘
         │ 成功  │ 失败 → 降级（同P0）
         │       └──→ 换代理IP重试1次
         │
    ┌────▼──────┐
    │ 3. 字幕    │  同 P0（三级降级）
    └────┬──────┘
         │
    ┌────▼──────┐
    │ 4. 抽帧    │  同 P0
    └────┬──────┘
         │
    ┌────▼──────┐
    │ 5. AI分析  │  P1 完整 Prompt（FABV+分镜+B层标签）
    └────┬──────┘
         │
    ┌────▼──────────────┐
    │ 6. 存储（两路写）   │
    │ ① OSS 上传视频+截图 │
    │ ② SQLite 写入      │
    │ ③ 飞书同步摘要      │
    └────┬──────────────┘
         │
    ┌────▼──────┐
    │ 7. 回复    │  推送摘要 ✅
    └───────────┘
```

## §1.8 API 规格增量（P1 新增）

### Playwright TikTok 下载器

```python
from playwright.sync_api import sync_playwright

def download_tiktok(url, output_dir, proxy=None):
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=True)
        context = browser.new_context(
            user_agent=random.choice(USER_AGENTS),
            proxy={"server": proxy} if proxy else None
        )
        page = context.new_page()
        page.add_init_script("""
            Object.defineProperty(navigator, 'webdriver', {get: () => false});
        """)
        page.goto(url, wait_until='networkidle')
        # 拦截视频 URL → 下载 → 保存
        # 详见完整实现
```

### OSS 上传

```bash
# 阿里云 OSS
ossutil cp /tmp/{video_id}.mp4 oss://hookly-bucket/{platform}/{date}/{video_id}.mp4

# AWS S3
aws s3 cp /tmp/{video_id}.mp4 s3://hookly-bucket/{platform}/{date}/{video_id}.mp4
```

### lark-cli 全量命令（P1）

```bash
# P0 基础 + P1 新增
lark-cli base records update --app-token <id> --table-id <id> --records '[...]'  # 更新状态
lark-cli im messages send --receive-id <id> --msg-type interactive --card '...'  # 卡片消息
lark-cli docs create --title "..." --folder-token "..."  # 创建文档
lark-cli docs update --token "..." --content "..."       # 更新文档
lark-cli drive files upload --file-path /tmp/v.mp4 --parent-token "..."  # 上传
```

## §1.9 状态机增量（P1）

> P0 8 状态不变。P1 在"已拆解→已归档"之间插入新状态。

```
P1 完整状态图:

等待下载 → 下载中 → 下载失败
              │
              ▼
           待转写 → 待分析 → 已拆解
                              │
                    ┌─────────▼─────────┐
                    │    待审核 (🆕)     │
                    └────┬─────────┬────┘
                         │         │
                    ┌────▼─┐  ┌───▼──┐
                    │已采纳 │  │需修正 │→ 回到待审核
                    │ (🆕)  │  │ (🆕)  │
                    └──┬───┘  └──────┘
                       │
                  ┌────▼────┐
                  │ 已改编   │ (🆕)
                  └────┬────┘
                       │
                  ┌────▼────┐
                  │ 已发布   │ (🆕)
                  └────┬────┘
                       │
                  ┌────▼────┐
                  │ 已复盘   │ (P2 才用)
                  └────┬────┘
                       │
                  ┌────▼────┐
                  │ 已归档   │
                  └─────────┘
```

### P1 状态转换规则（增量部分）

| 当前状态 | 可流转到 | 触发条件 | 谁操作 | P0/P1 |
|---------|---------|---------|--------|-------|
| 已拆解 | 待审核 | 自动 | 系统 | 🆕 P1 |
| 待审核 | 已采纳 | 人工确认 | 策划/管理员 | 🆕 P1 |
| 待审核 | 需修正 | 标记问题 | 策划/管理员 | 🆕 P1 |
| 需修正 | 待审核 | 修正完成 | 运营/策划 | 🆕 P1 |
| 已采纳 | 已改编 | 开始制作成片 | 剪辑师 | 🆕 P1 |
| 已改编 | 已发布 | 成片发布到平台 | 运营 | 🆕 P1 |
| 已发布 | 已复盘 | P2 数据回流触发 | 系统 | P2 |
| 已复盘 | 已归档 | 手动 | 管理员 | P0 ✓ |

## §1.10 飞书多维表全量 Schema（P1）

```yaml
表名: hookly_P1
# P1 在 P0 24 字段基础上新增 30+ 字段（电商标签 + 创意资产 + 扩展分析）

P0 已有 24 字段: 同 Part 0 §0.9

P1 新增字段:
  内容方向: 单选（种草|测评|教程|对比|vlog|开箱|场景植入）
  演示类型: 多选（场景化|对比测试|细节特写|开箱|证言）
  是否前后对比: 勾选
  视觉风格: 单选（原生感|专业感|治愈感|科技感|生活感|潮流感）
  BGM调性: 单选（电子|流行|古典|R&B|ASMR|无音乐）
  节奏: 单选（快|中|慢）
  色彩调性: 单选（暖色调|冷色调|高饱和|低饱和|黑白）
  适用产品: 文本
  镜头分镜: 多行文本（JSON）
  可复用视觉手法: 多行文本
  目标市场: 多选（美国|英国|东南亚|中东|拉美|日韩|欧洲）
  销售渠道: 多选（TikTokShop|Amazon|Shopify|Shopee|Temu|独立站）
  流量类型: 多选（自然流|付费投流|达人带货|直播|混合）
  一级类目: 文本
  二级类目: 文本
  价格带: 单选（低价|中价|高价|超高）
  购买意图: 单选（冲动消费|比价决策|解决问题|送礼|自我奖赏）
  季节性: 单选（四季常青|季节性|节日型|趋势型）
  复购潜力: 单选（高频复购|偶尔复购|一次性）
  拍摄难度: 单选（手机可拍|需要灯光|专业设备|需要演员|需要外景）
  剪辑难度: 单选（简单剪辑|中等特效|复杂后期）
  复制成本: 单选（极低|低|中|高）
  版权风险: 单选（无风险|需标注来源|不可直接复用）
  功效声明风险: 单选（无声明|轻微声明|强声明需资质）
  认知度评分: 数字(1-10)
  转化意图评分: 数字(1-10)
  UGC适配评分: 数字(1-10)
  投流适配评分: 数字(1-10)
  商品卡适配评分: 数字(1-10)
  风险评分: 数字(1-10)
  综合推荐度: 数字(1-10)
```

## §1.11 存储升级（本地 → OSS）

### 骨架

```
服务商: 阿里云 OSS（国内）/ AWS S3（海外）/ MinIO 自建
路径规范: {platform}/{date}/{video_id}/（沿用 P0 结构，加 bucket prefix）

文件生命周期:
  视频: 保留 30 天自动过期
  字幕: 永久保留
  截图: 保留 90 天
  AI 分析 JSON: 永久保留
```

### 迁移脚本（P0 → P1）

```bash
#!/bin/bash
# 将 P0 本地文件上传 OSS，批量更新飞书路径字段
for dir in "$HOOKLY_DATA_DIR"/*/; do
  platform=$(basename "$dir")
  ossutil cp -r "$dir" "oss://hookly-bucket/$platform/" --update --recursive
done
# 然后跑 SQL: UPDATE materials SET video_file_path = REPLACE(video_file_path, './data/hookly/', 'oss://hookly-bucket/');
```

## §1.12 轻量并发保护（P1 新增）

> P1 不引入独立任务队列，但需以下轻量保护避免团队并发时数据损坏。

```yaml
措施:
  - SQLite WAL mode（PRAGMA journal_mode=WAL）— 支持并发读
  - duplicate_key UNIQUE 约束 — 防重复入库
  - 插入冲突时返回已有记录 — 不丢数据
  - 单 worker 串行处理下载 — 避免资源争抢
  - 状态更新用 transaction — 保证原子性
```

> 这不是复杂队列。5 条配置级措施，足够 2-10 人团队使用。

## §1.13 错误处理增量（P1）

在 P0 基础上新增 TikTok 相关处理：

| 场景 | 处理 | 用户看到 |
|-----|------|---------|
| TikTok 下载失败 | 重试3次→换代理IP重试1次→仍失败降级为弱分析 | "⚠️ TikTok 下载失败，已保存基本信息" |
| 角色权限不足 | 拒绝操作 | "⛔ 你当前角色不支持此操作" |
| OSS 上传失败 | Retry 3 次 → 本地缓存 → 通知管理员 | "⚠️ 云端上传失败，已本地缓存" |

## §1.14 部署 / 环境配置（P1）

```bash
# 系统依赖
sudo apt install yt-dlp ffmpeg sqlite3
npx playwright install chromium

# Python 依赖
pip install oss2 boto3

# 目录结构（P1 多模块）
hookly/
├── bot/           # 飞书消息处理 + 权限鉴权
├── downloader/    # yt-dlp + Playwright
├── analyzer/      # LLM 调用 + JSON 校验
├── storage/       # OSS 上传 + SQLite 读写
├── sync/          # SQLite ↔ 飞书多维表同步
├── config/        # roles.yaml + app 配置
└── data/          # SQLite 数据库 + 本地缓存
```

## §1.15 依赖清单（P1）

```
系统工具: yt-dlp, ffmpeg, sqlite3, chromium (for Playwright)
Node: @larksuite/cli
Python: playwright, oss2, boto3, openai (或 anthropic), jsonschema
外部服务: 飞书 App（已配置）, 阿里云 OSS / AWS S3 账号
```

## §1.16 成本估算（P1）

| 项目 | 月成本 | 说明 |
|------|--------|------|
| LLM 订阅 | $150-200 | Claude/OpenAI 月度 |
| OSS 存储 | $5-10 | ~50GB |
| Apify | $10-30 | TikTok 数据抓取 |
| 服务器 | $0-30 | 复用已有或轻量云服务器 |
| **P1 合计** | **$200-300/月** | |

## §1.17 NFR（P1 团队级）

| 类别 | 要求 |
|-----|------|
| 可靠性 | TikTok 下载目标 60-70%（非硬 SLA）；OSS 上传成功 ≥99% |
| 性能 | SQLite 查询 <100ms；飞书多维表同步 <2s |
| 可扩展性 | 支持 2-10 人同时使用 |
| 存储 | OSS，视频 30 天自动过期 |
| 安全 | OSS 签名 URL；飞书最小权限 |

> 安全审计、webhook 验证、幂等 → Part 2。

## §1.18 P1 Implementation Contract 增量

在 P0 Contract（§0.17）基础上新增：

```yaml
角色权限契约:
  - 入库: operator/admin
  - 标签修改: planner/admin
  - 删除/归档: admin only
  - 查看: 所有角色

团队写入契约:
  - 同一素材多人同时入库 → 先入为主，后者收到"已存在"提示
  - 标签修改冲突 → last-write-wins，记录修改人+时间
```

## §1.19 验收 Checklist（P1）

- [ ] 2 人可同时入库不同素材，不互相阻塞
- [ ] TikTok 素材入库成功率 60-70%（非硬 SLA，失败降级保存链接+基础信息）
- [ ] OSS 上可访问已有视频和截图
- [ ] B 层电商标签可正常筛选
- [ ] 素材可走完整状态链：入库→审核→采纳→改编→发布
- [ ] 搜索"美国+低价+痛点刺激"返回正确结果
- [ ] P0 数据完整迁移（本地文件→OSS，飞书→SQLite）
- [ ] 团队每日简报正常推送
- [ ] 审核提醒正常推送
- [ ] 标签健康检查正常推送

## §1.20 为 P2 预留的演进接口

| # | P1 设计 | P2 会怎么用 |
|---|---------|-----------|
| 1 | SQLite 用标准 SQL | P2 迁移 PostgreSQL DDL 直接可执行 |
| 2 | `derived_from` 字段（TEXT） | P2 扩展为完整 CreativeIdea→Asset→Publication 映射表 |
| 3 | Apify 调用抽象为独立模块 | P2 加定时调度复用同一模块 |
| 4 | OSS 路径含 platform+date 前缀 | P2 加 CDN/生命周期策略按前缀配置 |
| 5 | 商业评分字段存 1-10 整数 | P2 数据回流后自动覆盖校准 |

---

# Part 2: P2 — 平台化 + 数据闭环

> **前置条件**：P1 验证通过（团队稳定使用 ≥2 周）。**本 Part 自包含** — 只写相对于 P1 的新增和变更。
> **目标**：8-12 周内，数据回流驱动创作决策 + 工程化加固到 SaaS 级。
> **新增 Artifact**：监控告警 · 安全模型 · 竞争分析 · Apify 全量 · LLM API · PostgreSQL 迁移 · 反馈增强

## §2.0 P2 定位

### P1 → P2 关键变化

| 维度 | P1 | P2 |
|-----|-----|-----|
| 数据库 | SQLite | PostgreSQL（按需） |
| 数据流 | 单向：入库→拆解 | 闭环：入库→拆解→发布→回流→分析 |
| 工程化 | 脚本级可靠性 | 持久化队列 + 幂等 + 结构化日志 |
| 安全 | 飞书基础权限 | webhook 验证 + 审计日志 + 数据留存 |
| 分析 | 无 | AI 周报 + 竞品监控 + 归因分析 |
| 反馈 | P0+P1 9 项触点 | + 数据亮点（回流数据） + 竞品简报 |
| 用户 | 单一团队 | 可扩展到多团队 |

### P2 设计哲学

**平台化** — 把 Codex 评估中提出的工程化要求逐一补上（之前 P0/P1 刻意不做）。同时打开数据闭环。

## §2.1 数据回流（Apify / TikHub 全量规格）

### 骨架

```
已发布素材链接 → Apify/TikHub 定时拉取 → Performance 数据 → SQLite/PostgreSQL
→ AI 周报生成 → 飞书文档推送

拉取频率:
  每日 08:00 UTC: 过去7天发布素材的最新指标
  每周一: 全量快照（趋势分析）
```

### Apify TikTok Scraper 全量规格

```
Actor: apidojo/tiktok-scraper-api
API: https://api.apify.com/v2/acts/apidojo~tiktok-scraper-api/runs
定价: $0.006/query, $0.0003/post
速度: 200 posts/s, 98% success

生命周期:
  1. POST /runs {"videoUrls": [...], "maxPosts": 100} → runId
  2. Poll GET /runs/{runId} → READY→RUNNING→SUCCEEDED/FAILED
  3. GET /runs/{runId}/dataset/items → 结构化 JSON
  4. 失败 → 重试或降级手动

输出字段（稳定 ✅ / 不稳定 ⚠️）:
  ✅ playCount, diggCount, commentCount, shareCount
  ⚠️ downloadUrl（时效性，需及时下载）
  ⚠️ authorMeta（可能被封/私密）

备选: TikHub API（多平台聚合，按月订阅）
```

### Python 拉取实现

```python
def fetch_performance(video_urls):
    resp = requests.post(
        "https://api.apify.com/v2/acts/apidojo~tiktok-scraper-api/runs",
        json={"videoUrls": video_urls}, params={"token": APIFY_TOKEN}
    )
    run_id = resp.json()["data"]["id"]
    
    while True:
        status = requests.get(
            f"https://api.apify.com/v2/acts/apidojo~tiktok-scraper-api/runs/{run_id}",
            params={"token": APIFY_TOKEN}
        ).json()["data"]["status"]
        if status == "SUCCEEDED": break
        if status == "FAILED": raise Exception("Apify run failed")
        time.sleep(10)
    
    return requests.get(
        f"https://api.apify.com/v2/acts/apidojo~tiktok-scraper-api/runs/{run_id}/dataset/items",
        params={"token": APIFY_TOKEN}
    ).json()
```

### P2 数据范围说明

> **硬边界**：P2a = 公开互动数据（播放/点赞/评论/分享/收藏/互动率），Apify/TikHub 可获取。
> **P2b/P3 = 商业数据**（GPM/GMV/ROI/conversions/orders/revenue），需 TikTok Shop / Amazon / Shopify 店铺授权。
> **没有店铺授权，不承诺 ROI/GPM。** 以下"数据亮点"和"周报增强"均基于公开数据。

### 数据回流后的反馈增强

P2 的每日小结和每周报告在 P0/P1 基础上叠加公开数据洞察：

```
# 每日数据亮点（附在每日小结中）
🔥 昨天发布《便携咖啡机测评》播放 50K，本周 Top 1
📈 本周发布 8 条，平均播放 52K（上周 45K ↑15%）

# 每周报告增强 Prompt
输入: 过去一周 Performance 数据 + 素材拆解结果
分析: Top 5 爆款特征 / Bottom 5 诊断 / Hook 效率排名 / 标签排名 / 竞品趋势
输出: Markdown → lark-cli docs create → 群聊推送
```

## §2.2 LLM API 规格（P2）

```
模型选择:
  Primary: GPT-5.5（月度订阅，固定成本，复杂分析远超人工）
  Fallback: DeepSeek V4 Pro / Claude（按需切换）

调用时机:
  入库拆解: 每条素材 1 次（~500 output tokens）
  数据周报: 每周 1 次（输入 ~5K-10K tokens）
  竞品分析: 每周 1-2 次

成本控制:
  月费模式（订阅）> 按量
  优先内嵌字幕（免费）→ 再调转录 API
  截图只传关键帧（3-5 帧），不传全视频
```

## §2.3 数据模型增量（P2）

### 实体关系图（P2 — 6 表关系）

```
┌──────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  materials   │────→│  creative_ideas  │────→│ produced_assets │
│  (P1 已有)   │     │  (P2 🆕)         │     │  (P2 🆕)         │
└──────────────┘     └──────────────────┘     └────────┬────────┘
                                                       │
                                              ┌────────▼────────┐
                                              │  publications   │
                                              │  (P2 🆕)         │
                                              └────────┬────────┘
                                                       │
                                              ┌────────▼────────┐
                                              │  performance_   │
                                              │  snapshots      │
                                              │  (P2 🆕)         │
                                              └─────────────────┘
```

### DDL 增量

```sql
-- 创意概念
CREATE TABLE creative_ideas (
    id TEXT PRIMARY KEY,
    title TEXT NOT NULL,
    direction TEXT,
    source_materials TEXT,       -- JSON: [material_id, ...]
    hook_inspiration TEXT,
    created_by TEXT NOT NULL,
    created_at TEXT DEFAULT (datetime('now'))
);

-- 自有成片
CREATE TABLE produced_assets (
    id TEXT PRIMARY KEY,
    creative_idea_id TEXT REFERENCES creative_ideas(id),
    file_url TEXT,
    editor TEXT,
    duration_seconds INTEGER,
    created_at TEXT DEFAULT (datetime('now'))
);

-- 发布记录
CREATE TABLE publications (
    id TEXT PRIMARY KEY,
    asset_id TEXT REFERENCES produced_assets(id),
    platform TEXT NOT NULL,
    publish_url TEXT NOT NULL,
    published_at TEXT NOT NULL,
    variant TEXT,               -- A/B 测试版本
    created_at TEXT DEFAULT (datetime('now'))
);

-- 数据快照（每日）
CREATE TABLE performance_snapshots (
    id TEXT PRIMARY KEY,
    publication_id TEXT REFERENCES publications(id),
    captured_at TEXT NOT NULL,
    views INTEGER DEFAULT 0,
    likes INTEGER DEFAULT 0,
    comments INTEGER DEFAULT 0,
    shares INTEGER DEFAULT 0,
    completion_rate REAL,
    gpm REAL,               -- P3 only (需店铺授权), nullable
    conversions INTEGER     -- P3 only (需店铺授权), nullable
);
```

## §2.4 素材→成品归因（P2 完整实体模型）

```
SourceMaterial (参考素材 — P0/P1 已在素材库)
    │
    ▼
CreativeIdea (创意概念 — P2 新增) ← 可参考 1-N 个 SourceMaterial
    │
    ▼
ProducedAsset (自有成片 — P2 新增)
    │
    ├──→ Publication (发布记录) → PerformanceSnapshot (数据快照)
    │
    └──→ 可能产生多个发布版本（不同标题/封面/平台）

归因逻辑（启发式，非严格因果）:
  成品 C 参考了素材 A(Hook) + B(分镜)
  → C 在 TikTok: 100K 播放, 互动率 8.2%
  → 归因: A 贡献 Hook 权重 ~40%, B 贡献分镜权重 ~30%
```

## §2.5 工程化加固（Codex 建议落地 5 项）

### (a) 持久化任务队列

```
方案: Redis + BullMQ (Node.js) / RQ (Python) / SQLite 简易队列

队列设计:
  ingest    → 下载+字幕+截图
  analyze   → AI 拆解
  sync      → 飞书写入+通知
  data_fetch → 定时批量拉取（每日）

Job 结构: {job_id: UUID, idempotency_key, max_retries: 3, retry_delay: [30s,60s,120s], created_at, started_at, completed_at}
```

### (b) 幂等设计

```
幂等 key: sha256(f"{action}:{url}:{user_id}")

场景:
  重复入库同一链接 → 幂等命中 → 返回已有结果
  定时拉取重复执行 → 幂等 key 按日期区分 → 同一天只拉一次
  webhook 重放 → 缓存 24h → 重复消息丢弃
```

### (c) 结构化日志

```python
log = {
    "run_id": "uuid", "stage": "download|analyze|...",
    "material_id": "uuid", "platform": "youtube",
    "duration_ms": 1234, "status": "success|failed|retry",
    "error": "...", "retry_count": 0, "timestamp": "ISO8601"
}
# 输出: JSON 文件 / stdout → Loki / CloudWatch
```

### (d) Webhook 验证

```python
def verify_feishu_request(timestamp, nonce, body, signature):
    import hashlib
    raw = f"{timestamp}{nonce}{FEISHU_ENCRYPT_KEY}{body.decode()}"
    return hashlib.sha256(raw.encode()).hexdigest() == signature
```

### (e) 安全与审计

| 能力 | 实现 |
|-----|------|
| Token 存储 | 环境变量 / Vault / KMS |
| 审计日志 | 谁/何时/什么操作/结果 |
| 数据留存 | 视频 30 天/截图 90 天/分析结果永久 |
| 访问控制 | 基于角色的 API 鉴权 |
| OSS 签名 | 下载链接 7 天有效期 |

## §2.6 监控与告警（P2 新增）

```yaml
关键指标:
  - 每日入库量（<3 条/天 → 告警）
  - 各平台下载成功率（<50% → 告警）
  - AI 分析成功率（<90% → 告警）
  - 飞书写入延迟（>5s → 告警）
  - Apify/TikHub 调用剩余额度

告警渠道: 飞书群消息 / 邮件
  
每日故障摘要（飞书推送）:
  - 今日总入库 X 条 | 成功 Y 条 | 失败 Z 条
  - 失败详情: [链接1: 下载超时, 链接2: AI 输出非法JSON]
```

## §2.7 安全模型（P2 新增）

```
认证: OpenClaw 网关层（已有）+ 飞书 webhook 签名验证
授权: 5 角色映射（同 P1）+ API 级权限校验（多租户扩展）
审计: 所有写操作记录到 audit_log 表
数据保护: OSS 签名 URL + DB 加密 + 定期备份
合规: 仅公开数据；不重新发布原视频；用户数据隔离
```

## §2.8 PostgreSQL 迁移（按需）

```
触发条件: 多团队 SaaS / 并发写入 >10人 / 复杂聚合查询需求

迁移策略:
  1. SQLite → PostgreSQL DDL（pgloader 或脚本）
  2. 双写 SQLite+PG 过渡 1-2 周
  3. 切读流量 → 下线 SQLite

成本: $15-50/月（Supabase/RDS 托管）
```

## §2.9 竞品监控（P2 新增）

```
功能: 输入竞品账号 → 定时抓取新视频 → 自动入库分析 → 对比

数据源: Apify TikTok Profile Scraper
对比维度: Hook 类型分布 / 发布频率 / 内容方向 / 互动率趋势 / 己方 vs 竞品
```

## §2.10 技术架构图（P2）

```
┌──────────────────────────────────────────────────────────────┐
│                    用户层（多团队）                           │
│        飞书 App → 入库/搜索/审核/改编/数据看板                 │
└──────────────────────────┬───────────────────────────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│                  Webhook 验证 + 角色鉴权                      │
└──────────────────────────┬───────────────────────────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│                   持久化任务队列 (Redis/BullMQ)               │
│  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────────┐     │
│  │ingest│  │analyze│  │ sync │  │fetch │  │report    │     │
│  │队列   │  │队列   │  │队列  │  │队列  │  │队列      │     │
│  └──┬───┘  └──┬───┘  └──┬───┘  └──┬───┘  └────┬─────┘     │
│     │         │         │         │           │             │
│  ┌──▼──┐  ┌──▼──┐  ┌──▼──┐  ┌──▼────┐  ┌───▼──────┐      │
│  │yt-dlp│  │LLM  │  │lark │  │Apify  │  │LLM周报   │      │
│  │+PW   │  │     │  │-cli │  │TikHub │  │生成      │      │
│  └─────┘  └─────┘  └─────┘  └───────┘  └──────────┘      │
└──────────────────────────┬───────────────────────────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│               PostgreSQL + OSS + 飞书（展示）                 │
│         结构化日志 (Loki) + 监控告警 (飞书群)                 │
└──────────────────────────────────────────────────────────────┘
```

## §2.11 状态机增量（P2）

在 P1 基础上新增"已复盘"状态：

```
已发布 → (数据回流触发) → 已复盘 → 已归档
```

## §2.12 错误处理增量（P2）

| 场景 | 处理 |
|-----|------|
| Apify API 限流 | 429 → 等 Retry-After header → 重试 |
| Webhook 重放 | 幂等 key 缓存 24h → 重复消息丢弃 |
| 任务队列积压 | 监控 worker 数量 → 自动扩容 |
| PostgreSQL 连接池耗尽 | 限流 + 告警 |

## §2.13 用户交互增量（P2）

```
# 数据查询
用户: 上周 播放 Top 5
Bot:  📊 上周 播放 Top 5:
      1. [素材标题] 播放 82K | Hook: 痛点刺激 | 参考: om_abc
      2. ...

# 竞品对比
用户: 竞品 @competitor_account 本周趋势
Bot:  📊 @competitor_account 本周: 发布 12 条 | 平均播放 45K
      己方: 发布 8 条 | 平均播放 52K ✅
      竞品 Top Hook: 痛点刺激(5条) | 己方 Top Hook: 悬念反转(4条)
```

## §2.14 部署 / 环境（P2）

```bash
# 新增系统依赖
Redis, PostgreSQL, Prometheus + Grafana（可选）

# 服务架构
├── bot/            # 飞书消息 + webhook 验证
├── queue/          # 任务队列 + 幂等
├── workers/        # ingest / analyze / sync / fetch / report
├── api/            # REST API（供外部调用，多租户）
├── db/             # PostgreSQL migrations
└── monitoring/     # 结构化日志 + 指标采集
```

## §2.15 迁移脚本（P1 → P2）

```bash
# SQLite → PostgreSQL
pgloader sqlite:///data/hookly.db postgresql://user:pass@host/db

# 数据回流初始化
python scripts/backfill_performance.py --days 30  # 回填过去30天

# 启用结构化日志
export LOG_FORMAT=json
export LOG_LEVEL=info
```

## §2.16 成本估算（P2）

| 项目 | 月成本 | 说明 |
|------|--------|------|
| LLM 订阅 | $150-200 | 同 P1 |
| OSS 存储 | $10-30 | 数据量增长 |
| Apify/TikHub | $30-100 | 定时拉取增加用量 |
| PostgreSQL | $15-50 | Supabase/RDS |
| Redis | $0-15 | 任务队列 |
| 服务器 | $20-50 | 轻量云服务器 |
| **P2 合计** | **$100-500/月** | 视用量浮动 |

## §2.17 NFR（P2 生产级）

| 类别 | 要求 |
|-----|------|
| 可靠性 | 持久化队列 + 幂等 + 可恢复阶段 + 结构化日志 |
| 性能 | PostgreSQL 多租户；素材库单表 100 万行 |
| 安全 | Webhook 验证 + token 安全存储 + 审计日志 + 数据留存 |
| 合规 | 用户数据隔离、访问控制、仅公开数据 |
| 监控 | 结构化日志、run ID、每阶段计时、错误分类、每日摘要 |
| 成本 | 按量计费，可控可预测 |

## §2.18 验收 Checklist（P2）

- [ ] Apify 每日自动拉取发布数据，写入 Performance 表
- [ ] AI 周报自动生成并推送到飞书群
- [ ] 素材→成品→发布→数据 归因链路可查询
- [ ] 竞品账号数据可对比
- [ ] 任务队列正常运行，失败自动重试
- [ ] Webhook 签名验证生效
- [ ] 审计日志记录所有写操作
- [ ] 监控告警可用（入库量/成功率/延迟）
- [ ] P1 数据完整迁移到 PostgreSQL（如需要）

## §2.19 竞争分析（P2）

| 系统 | 开发方 | 相似度 | 差异 |
|-----|-------|--------|------|
| 饼干哥哥 OpenHands | 饼干哥哥 | 80% | iOS 快捷指令触发，侧重个人 |
| 刘小排 RPA | 刘小排 | 50% | 多平台自动发布，非素材分析 |
| 数字酋长 ERP | 数字酋长 | 40% | 商品上架管理，非内容研发 |
| liangdabiao lark-workflow | @liangdabiao | 60% | 通用办公自动化 |

**差异化**：本系统专注"内容素材拆解→复用→归因"全链，从个人到 SaaS 逐步演化，与竞品的单品定位形成错位。

## §2.20 为 P3 预留的演进接口

| # | P2 设计 | P3 会怎么用 |
|---|---------|-----------|
| 1 | `performance_snapshots` 含 `gpm`/`conversions` | P3 接店铺 API 自动填充真实 ROI |
| 2 | 归因用 `source_materials` JSON 数组 | P3 加权重计算（Hook 40% + 分镜 30% + 话术 30%） |
| 3 | AI 周报输出 Markdown | P3 改交互式仪表盘（飞书/BI 工具） |
| 4 | `produced_assets` 含 `editor` 字段 | P3 剪辑辅助做智能推荐 |

---

# Part 3: P3+ — 未来探索

> **状态**：不做当前承诺。P0-P2 验证完再评估。以下为方向性描述。

## §3.0 P3 商业闭环（GPM/ROI 归因）

**前置条件**：TikTok Shop / Amazon SP-API / 独立站订单数据接入。

**目标**：素材 → 参考它做出的成品 → 销售额/ROI → 归因到源素材。"这类 Hook + 这个品类 = 最高 ROI"。

**数据源**：店铺 API（需授权，非公开数据）。

## §3.1 P4 半自动创作

**方向**：AI 根据素材库 + 品类生成脚本大纲；参考素材分镜做新脚本分镜规划；素材元素组合（Hook A + 演示 B + 话术 C）。

**依赖**：P2 归因模型完成 + 素材库 ≥500 条高质量素材。

## §3.2 剪辑辅助

单独立项，不混入主线。AI 粗剪 / BGM / 字幕样式。

---

# 附录 A: 竞品/参照系统

| 系统 | 开发方 | 相似度 | 差异 |
|-----|-------|--------|------|
| 饼干哥哥 OpenHands | 饼干哥哥 | 80% | iOS 快捷指令触发 |
| 刘小排 RPA | 刘小排 | 50% | 多平台自动发布 |
| 数字酋长 ERP | 数字酋长 | 40% | 商品上架管理 |
| liangdabiao lark-workflow | @liangdabiao | 60% | 通用办公自动化 |

# 附录 B: 术语中英对照

| 中文 | 英文 | 说明 |
|-----|------|------|
| 钩子 | Hook | 视频前 3 秒注意力捕获 |
| 完播率 | Completion Rate | 完整看完的视频占比 |
| 千次播放销售额 | GPM | Gross Profit per Mille |
| 多维表格 | Base / Bitable | 飞书数据库 |
| 种草 | Seeding | 内容激发购买欲望 |
| 工程加固 | Production Hardening | MVP 后把代码变得可靠可维护 |

# 附录 C: Gap 清单

| # | Gap | 影响 | 假设 | 验证 |
|---|-----|------|------|------|
| 1 | 触发渠道 | P0 | 飞书 bot | ✅ 已确认 |
| 2 | Prompt 模板 | P0 | 基于行业标准 | P0 开发调优 |
| 3 | 飞书字段 | P0 | 推演 | Day 0 验证 |
| 4 | 团队模型 | P1 | 2-10 人 | P0 验证后细化 |
| 5 | TikTok 稳定性 | P1 | 目标 60-70%，非硬 SLA，失败降级 | P1 实测 |
| 6 | Apify vs TikHub | P2 | Apify 优先 | P2 前对比 |
| 7 | OSS 方案 | P1 | 阿里云 OSS | P1 前确认 |
| 8 | 店铺 API | P3 | TikTok Shop / Amazon | P3 时评估 |
