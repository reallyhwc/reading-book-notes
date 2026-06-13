# 读书笔记系统 v2 实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 改造读书笔记系统，实现"新书开始时 Multi-Agent 并行生成全量 baseline notes + Harness 阻断式校验"工作流

**Architecture:** 修改 CLAUDE.md 和 writing-conventions.md 的规则定义，将"按需创建"改为"baseline 预生成 + 讨论加深"；引入 TDD 化的可检查规则；为《人工智能之不能》13章生成 baseline notes

**Tech Stack:** Markdown, Git, Claude Code Sub-Agent

---

### Task 1: 修改 CLAUDE.md — 协作流程规则

**Files:**
- Modify: `CLAUDE.md:56-68`

- [ ] **Step 1: 替换协作流程第 3 条和新增 Harness 流程说明**

将第 56-68 行的"协作流程（对话驱动模式）"段替换为以下内容：

```markdown
## 协作流程（Baseline + 讨论驱动模式）

### 新书开始

1. 用户决定读某本新书 → AI 创建 `books/<slug>/` 目录结构（README.md + notes/ + raw/ + media/）
2. AI 分派多个 Sub-Agent 并行检索并生成全量 baseline notes（notes/chapter-XX.md，每章填 A~E 板块）
3. 每章 notes 写入前过 Harness 阻断式审查（格式 + 事实 + 一致性），不通过打回 Agent 重写
4. 全部通过后 commit

### 阅读讨论中

1. 用户在对话中聊感想、触动、质疑 → AI 提炼并写入 raw/ 对应文件
2. raw/ 中的笔记积累后，可反向更新 notes/ 中的正式笔记（同样过 Harness 审查）
3. 文件创建时机：raw/ 文件在讨论触发时按需创建或追加

### Harness 校验层

每次写入 notes/ 前，过三道检查。全部阻断式——不通过不写入。

| 检查层 | 内容 | 执行者 |
|--------|------|--------|
| L1 格式 | 术语标注格式、文件命名、A~F 板块完整性、文件头元信息 | 主 Agent 自检 |
| L2 事实 | 人名/年份/概念定义是否准确，引用是否可验证 | Sub-Agent (Content Reviewer) |
| L3 一致性 | 和已有笔记是否矛盾，跨书概念冲突 | Sub-Agent (Content Reviewer) |

### Agent 角色

| Agent | 触发时机 | 职责 |
|-------|----------|------|
| Baseline Generator × N | 新书开始 | 并行 WebSearch + 生成各章 notes/（A~E） |
| Content Reviewer | 每次 notes/ 写入/更新前 | L2 事实校验 + L3 一致性检查 |
| Main Agent | 持续 | 对话、写 raw/、调度 sub-agent、最终 commit |

### 对话自动沉淀规则（不可跳过）

每次涉及书中内容的实质性对话结束后，**在同一轮回复中**执行以下操作，不需要等用户提醒：

1. 写 raw/ 文件（`raw/chapter-XX.md` 或 `raw/topic-<slug>.md`）
2. 更新 `memory/project-state.md`
3. commit（**仅此场景允许自动 commit**，其他场景不要自动提交）

文件命名：
- `raw/chapter-XX.md` — 按章节组织的个人笔记
- `raw/topic-<slug>.md` — 跨章节专题讨论
```

- [ ] **Step 2: 更新快速状态行**

将第 5 行的快速状态更新为当前实际进度：

```markdown
- **阅读中**：《秦二世必须死》（马伯庸）— 小说轻量模式；《人工智能之不能》（马兆远）— 第二章
- **暂停书籍**：《世界的逻辑》（马兆远）— 第三章（兴趣不匹配）
- **已完成**：《我看见的世界》（李飞飞）— 12 章 + 3 专题
```

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "docs: 重构协作流程为 Baseline+讨论驱动模式，新增 Harness 校验和 Agent 角色定义"
```

---

### Task 2: 修改 writing-conventions.md — 新增可检查规则

**Files:**
- Modify: `.claude/memory/writing-conventions.md`

- [ ] **Step 1: 在文件末尾追加"可检查规则"段**

Read `.claude/memory/writing-conventions.md` 后，在文件末尾追加以下内容：

```markdown

## 可检查规则（Harness 校验输入）

以下规则在每次写入 notes/ 前作为 Harness 校验的输入。全部阻断式（BLOCK），不通过拒绝写入。

### R1: 术语首次出现格式
- **CHECK**: 正文中每个英文术语首次出现时，是否匹配 `中文全称（English Full Name, 缩写）` 格式
- **SEVERITY**: BLOCK
- **反例**: "CNN（卷积神经网络）"（顺序颠倒）、"卷积神经网络"（缺英文）
- **豁免**: 同一术语在同一文件中后续出现可只用中文或缩写

### R2: 文件命名
- **CHECK**: notes/ 下文件名是否匹配 `chapter-\d{2}\.md` 或 `chapter-\d{2}-(preface|introduction|postscript|conclusion)\.md`
- **SEVERITY**: BLOCK

### R3: 板块 A 完整性
- **CHECK**: A. 章节概述 板块是否存在且非空（至少 50 字）
- **SEVERITY**: BLOCK

### R4: 板块 B~E 合理性
- **CHECK**: B/C/D/E 板块如填写，内容是否与板块定义匹配（技术要点归 B，思想洞察归 C，人物归 D，AI 补充归 E）
- **SEVERITY**: BLOCK

### R5: 人物/事件准确性
- **CHECK**: 提到的人物全名、生卒年份、职称/角色是否经 WebSearch 验证
- **SEVERITY**: BLOCK

### R6: 跨书概念一致性
- **CHECK**: 同一概念在不同书的 notes/ 中定义是否一致；如不一致，是否有明确说明
- **SEVERITY**: BLOCK

### R7: raw/ 文件双层结构
- **CHECK**: raw/ 文件是否包含：对话记录（正文 80%）+ 要点提炼（3-5 bullet）+ 待继续思考（2-3 问题）
- **SEVERITY**: BLOCK

### R8: raw/ 文件头元信息
- **CHECK**: raw/ 文件开头是否有 `> 触发：...` 和 `> 日期：YYYY-MM-DD` 元信息行
- **SEVERITY**: BLOCK
```

- [ ] **Step 2: 修改文件创建时机规则**

Read `.claude/memory/writing-conventions.md` 第 60-61 行，将：

```markdown
- **notes/chapter-XX.md**：当用户聊到某章内容、且对话中产生了足够的结构化信息时创建。不预创建空骨架。
```

替换为：

```markdown
- **notes/chapter-XX.md**：新书开始时由 Multi-Agent 并行检索并全量生成 baseline（A~E 板块）。阅读讨论中如产生新知识或修正，反向更新对应章节的 notes/。
```

- [ ] **Step 3: Commit**

```bash
git add .claude/memory/writing-conventions.md
git commit -m "docs: 新增 Harness 可检查规则 R1~R8，修改 notes 创建时机为 baseline 预生成"
```

---

### Task 3: 为《人工智能之不能》生成全量 baseline notes

**Files:**
- Create/Update: `books/what-ai-cannot-do/notes/chapter-00-preface.md`
- Create/Update: `books/what-ai-cannot-do/notes/chapter-01.md` 到 `chapter-12.md`
- Create/Update: `books/what-ai-cannot-do/notes/chapter-13-postscript.md`
- Update: `books/what-ai-cannot-do/README.md`

- [ ] **Step 1: 分派 4 个 Baseline Generator Agent 并行检索和生成**

将 13 章分配给 4 个 Sub-Agent，每 Agent 负责 3-4 章：

| Agent | 负责章节 | 检索关键词 |
|-------|----------|-----------|
| Agent A | 前言 + 第1-3章 | 马兆远 人工智能之不能 回归常识 知无不言 逻辑 哥德尔 |
| Agent B | 第4-6章 | 马兆远 人工智能之不能 图灵 聊天 哥德尔不完备定理 理性 |
| Agent C | 第7-9章 | 马兆远 人工智能之不能 AI不能 量子力学 物理学不完备 |
| Agent D | 第10-12章 + 后记 | 马兆远 人工智能之不能 数学边界 认知边界 创新突破 |

每个 Agent 的 Prompt：

```
你是 Baseline Notes Generator。为《人工智能之不能》（马兆远著，中信出版社 2020）的以下章节生成 notes/ 文件：

负责章节：[章节列表]

要求：
1. 对每章进行 WebSearch，检索：章节主题、书中核心观点、涉及的关键人物/概念
2. 按 A~E 模板填充每章 notes/chapter-XX.md：
   - A. 章节概述（必填，100-200字概括）
   - B. 技术要点（按需，技术概念解析）
   - C. 人文/思想洞察（按需）
   - D. 关键人物/事件（按需）
   - E. AI 补充背景（按需，书中未展开的相关知识）
3. 术语格式：首次出现必须 `中文全称（English Full Name, 缩写）`
4. 不确定的信息标注"据报道"或"约"，不要编造
5. 每章输出完整的 markdown 文件内容

已知上下文：
- 前言"回归常识的科学"：定调 AI 热潮需回归基本原理
- 第一章"知无不言"：知识能否被语言/符号系统完整表达
- 第二章"基本的逻辑"：形式逻辑四大基本规律，已讨论矛盾律/排中律
- 核心论点：从哥德尔不完备定理出发，论证不存在超级通用人工智能
- 作者背景：马兆远，清华大学未来实验室首席研究员，牛津大学物理学博士

输出格式：每章一个独立文件，文件名为 chapter-XX.md（XX 为两位数字），直接输出文件内容。
```

- [ ] **Step 2: 等待 4 个 Agent 完成，收集输出**

4 个 Agent 并行运行，等待全部返回。每个 Agent 返回 3-4 章 notes 内容。

- [ ] **Step 3: 对全部 13 章执行 Harness L1 格式审查**

逐章检查（主 Agent 自检）：
- R1 术语格式 ✓/✗
- R2 文件命名 ✓/✗
- R3 A 板块非空且 ≥50 字 ✓/✗
- R4 B~E 内容与板块匹配 ✓/✗

任何 ✗ 项打回对应 Agent 重写该章。

- [ ] **Step 4: 派 Content Reviewer Agent 执行 L2 事实 + L3 一致性审查**

```
你是 Content Reviewer。审查以下 notes/ 文件的事实准确性和跨章节一致性：

文件列表：books/what-ai-cannot-do/notes/chapter-*.md（全部 13 个文件）

检查项：
1. 人名全称、生卒年份是否准确（WebSearch 交叉验证）
2. 核心概念定义是否与已知学术共识一致
3. 同一概念在不同章节的表述是否一致
4. 与已有笔记（chapter-02.md raw/、topic-godel-incompleteness-and-mingjia.md）是否存在矛盾

对每个问题报告：文件、行、问题描述、建议修正。
```

- [ ] **Step 5: 根据审查结果修正，全部通过后写入**

每个 BLOCK 问题修正后写入对应文件。

- [ ] **Step 6: 更新 README.md 阅读状态**

Read `books/what-ai-cannot-do/README.md`，将阅读状态更新为"阅读中（已生成全量 baseline notes）"。

- [ ] **Step 7: Commit**

```bash
git add books/what-ai-cannot-do/notes/ books/what-ai-cannot-do/README.md
git commit -m "feat: 《人工智能之不能》全13章 baseline notes 生成，通过 Harness 审查"
```

---

### Task 4: 更新 project-state.md

**Files:**
- Modify: `.claude/memory/project-state.md`

- [ ] **Step 1: 更新笔记状态行**

将 project-state.md 第 13 行更新为：

```markdown
- **笔记状态**：《人工智能之不能》全 13 章 baseline notes 已生成（Multi-Agent + Harness 审查）；《世界的逻辑》引言 + 第1~3章 raw/ 共 9 轮讨论；跨书专题 2 个
```

- [ ] **Step 2: Commit**

```bash
git add .claude/memory/project-state.md
git commit -m "docs: 更新项目状态——《人工智能之不能》baseline 已生成"
```

---

### Task 5: 更新 CLAUDE.md 快速状态

- [ ] **Step 1: 确认 CLAUDE.md 快速状态行已反映最新进度**

验证 CLAUDE.md 第 5 行快速状态与 project-state.md 一致。

- [ ] **Step 2: 如需要则 commit**

```bash
git add CLAUDE.md
git commit -m "docs: 同步快速状态"
```
