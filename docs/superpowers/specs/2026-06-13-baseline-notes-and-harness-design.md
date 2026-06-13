# 读书笔记系统 v2：Baseline Notes + Harness 校验 + Multi-Agent 架构

> 日期：2026-06-13
> 状态：已批准，待实现

## 一、问题陈述

当前系统的问题：
1. notes/ 仅在用户聊到时按需创建，前言/第一章常是空骨架
2. 笔记质量无强制校验（术语格式、事实准确性、跨书一致性）
3. 用户期望"新书开始时 AI 全量生成 baseline，阅读中讨论再加深"

## 二、新工作流

```
新书开始
  ├─→ [Multi-Agent 并行] 每 2-3 章派一个 agent
  │     WebSearch → 生成 notes/chapter-XX.md（A~E 板块）
  ├─→ [Harness 阻断审查] 每章 notes 写入前过校验，不通过打回重写
  └─→ 全部通过后 commit
      ↓
用户阅读 + 讨论
  ├─→ 讨论内容写入 raw/（现有逻辑不变）
  ├─→ 讨论中的新知识/修正 → 反向更新 notes/
  │     └─→ 更新后 notes 同样过 Harness 审查
  └─→ commit
```

## 三、Harness 校验层

每次写入 notes/ 前，过三道检查。全部阻断式。

| 检查层 | 内容 | 执行者 |
|--------|------|--------|
| L1 格式 | 术语标注格式、文件命名、A~F 板块完整性、文件头元信息 | 主 Agent 自检 |
| L2 事实 | 人名/年份/概念定义是否准确 | Sub-Agent（Content Reviewer） |
| L3 一致性 | 和已有笔记是否矛盾，跨书概念冲突 | Sub-Agent（Content Reviewer） |

## 四、Agent 角色

| Agent | 触发时机 | 职责 |
|-------|----------|------|
| Baseline Generator × N | 新书开始 | 并行检索 + 生成各章 notes/ |
| Content Reviewer | 每次 notes/ 写入/更新前 | L2 事实校验 + L3 一致性检查 |
| Main Agent | 持续 | 对话、写 raw/、调度 sub-agent、最终 commit |

## 五、TDD 化规则

writing-conventions.md 中的规则改为可检查断言，每条包含 CHECK 方法和 BLOCK 级别。

## 六、改动文件清单

| 文件 | 改动 |
|------|------|
| CLAUDE.md | 协作流程第 3 条改为 baseline 生成；新增 harness 流程说明 |
| .claude/memory/writing-conventions.md | 新增"可检查规则"段；修改 notes/ 创建时机 |
| .claude/memory/future-architecture.md | 补充 sub-agent 架构描述 |
