---
name: spec-workflow
description: Product spec workflow — from idea research to implementable spec. Covers research → V0.1 skeleton → V1.0 full spec → external review → iteration → branding → freeze. Ensures mid-level programmers can build from Part 0 without ambiguity. Activated by "研究立项", "写 Spec", "按 Playbook".
---

# Spec Workflow Skill

> 从 Idea 到可开工 Spec 的标准工作流。沉淀自灵感捕手 (hookly) 项目。
> 完整参考：[PLAYBOOK.md](PLAYBOOK.md)

---

## 触发条件

当用户说以下任意内容时，激活此 skill：

- "研究一下 XX" / "调研 XX"
- "写 Spec" / "写规格书" / "产品规格"
- "按 Playbook 来" / "按流程走"
- "立项" / "这个想法怎么样"

---

## 流程速查

```
Step 1 ─ 调研 ─── 搜索来源 → 竞品分析 → 技术可行性 → 定 @PLACEHOLDER@
Step 2 ─ V0.1 ─── 骨架（~400行，全阶段框架）→ 决策者确认方向
Step 3 ─ V1.0 ─── 全量 Spec（1250+行）→ 完整 artifact
Step 4 ─ 评审 ─── 送 coding agent + 专家 → 整理意见 → 决策者逐条确认
Step 5 ─ 迭代 ─── 改完送审 → 循环到评分 ≥8.5 + 专家认可
Step 6 ─ 品牌 ─── 决策者定名 → sed 替换 @PLACEHOLDER@
Step 7 ─ 冻结 ─── 决策者确认 → git tag frozen-vX.X → 程序员读 Part 0 开工
```

---

## 关键规则

### 1. 权限边界（最高优先级）

| 事项 | Agent 可以做 | 必须决策者确认 |
|------|:----------:|:----------:|
| 调研、起草、送审 | ✅ | |
| 分析评审意见 + 标注建议 | ✅ | |
| 执行已确认的修改 | ✅ | |
| 修改方向/范围/功能 | | ✅ |
| 品牌命名 | | ✅ |
| 冻结版本 | | ✅ |
| **绝不私自标记 frozen** | | ✅ |

### 2. @PLACEHOLDER@ 规范

```
从 V0.1 就用占位符：
  @PROJECT@  — 项目中文名
  @ENG@      — 项目英文名（repo/package/CLI）
  @SLOGAN@   — 一句话 Slogan

品牌化阶段一行替换：
  sed -i 's/@PROJECT@/正式名/g; s/@ENG@/engname/g; s/@SLOGAN@/正式Slogan/g' spec.md
  grep '@' spec.md  # 必须返回空
```

### 3. 评审处理

- Agent 列出所有评审意见，每条标注：建议采纳 / 建议不采纳 / 需讨论 + 理由
- 决策者逐条确认 → 形成最终修改计划
- 最终决定权在决策者，不在 Agent 也不在评审者

### 4. 质量标准

- Coding agent 评分 ≥8.5
- 专家表示"可以冻结了"
- 剩余问题全是 nice to have，不是 blocker
- 程序员只读 Part 0 就能无歧义开工

### 5. 交付前自检

每轮送审前必跑：
```
□ 字段计数全文一致
□ SLA 数字全文一致
□ 路径/目录名全文一致
□ Schema 对齐（Prompt ⊇ JSON Schema ⊇ 数据模型 ⊇ Build Contract）
□ 品牌名/placeholder 无残留
```

### 6. 文档结构

详见 [PLAYBOOK.md](PLAYBOOK.md)

---

## 反模式速查

| ❌ 不要 | ✅ 应该 |
|--------|--------|
| V1.0 写太细才送审 | V0.1 骨架先送审确认方向 |
| 评审意见全盘接受 | 列出所有意见，决策者逐条确认 |
| Agent 私自标记 frozen | 等决策者说 frozen |
| 硬编码临时项目名 | 全程用 @PLACEHOLDER@ |
| 想一次写完美 | 小步快跑，每轮只改当前问题 |
