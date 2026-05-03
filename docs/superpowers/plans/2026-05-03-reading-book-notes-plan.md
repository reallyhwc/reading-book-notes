# Reading Book Notes Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 搭建多书读书笔记项目脚手架，Markdown + Git 管理，产出《我看见的世界》全书 12 章笔记。

**Architecture:** 顶层 README 作为书房索引，books/ 下每本书独立目录，每本书内有 notes/（AI 产出笔记）和 raw/（用户补充感想）两层分离。

**Tech Stack:** Markdown, Git

---

### Task 1: 初始化 Git 仓库 & 项目根文件

**Files:**
- Create: `.gitignore`
- Create: `README.md` (书房索引)
- Create: `CLAUDE.md` (AI 协作指引)

- [ ] **Step 1: 初始化 Git**

```bash
cd /Users/xuhu/workspace/reading-book-notes && git init
```

- [ ] **Step 2: 创建 .gitignore**

```
.DS_Store
*.swp
*.swo
*~
```

- [ ] **Step 3: 创建 README.md（书房索引）**

书房索引包含：项目说明、已有书单、阅读状态总览。格式：

```markdown
# 读书笔记

以程序员视角记录 AI 及相关领域书籍的阅读笔记。AI 辅助产出纲要与摘要，个人补充思考与标注。

## 书单

| 书名 | 作者 | 状态 | 开始日期 | 完成日期 |
|------|------|------|----------|----------|
| [我看见的世界](books/the-worlds-i-see/) | 李飞飞 | 已读完 | - | - |

## 目录结构

每本书包含：
- `notes/` — AI 产出的章节笔记（A.概述 B.技术 C.人文 D.人物事件 E.补充背景）
- `raw/` — 个人补充感想与标注
- `media/` — 相关图片
```

- [ ] **Step 4: 创建 CLAUDE.md（AI 协作指引）**

```markdown
# CLAUDE.md — 读书笔记项目协作指引

## 项目规范

- 语言：中文正文，技术术语保留英文并括号注中文/缩写
- 例：卷积神经网络（Convolutional Neural Network, CNN）
- 笔记模板：每章固定 A~E 五个板块

## 笔记板块

- A. 章节概述 — 简要概括本章内容
- B. 技术要点 — 技术概念解析，含背景知识和延伸阅读
- C. 人文/思想洞察 — 人文关怀、科学家精神、社会影响
- D. 关键人物/事件 — 重要人物、里程碑事件
- E. AI 补充背景 — 书中未展开但相关的背景知识

## 协作流程

1. 用户指定书籍 → 产出全书 notes
2. 用户阅读后在 raw/ 中补充感想
3. 用户标记具体触动点 → 可深度扩展
```

- [ ] **Step 5: 首次提交**

```bash
git add .gitignore README.md CLAUDE.md
git commit -m "init: project scaffold with README and AI collaboration guide"
```

---

### Task 2: 创建《我看见的世界》书籍目录结构

**Files:**
- Create: `books/the-worlds-i-see/README.md`
- Create: `books/the-worlds-i-see/media/.gitkeep`

- [ ] **Step 1: 创建目录**

```bash
mkdir -p books/the-worlds-i-see/notes
mkdir -p books/the-worlds-i-see/raw
mkdir -p books/the-worlds-i-see/media
```

- [ ] **Step 2: 创建本书 README.md**

```markdown
# 《我看见的世界》

- **英文名**: The Worlds I See: Curiosity, Exploration, and Discovery at the Dawn of AI
- **作者**: 李飞飞 (Fei-Fei Li)
- **出版**: 中信出版社, 2024年4月
- **ISBN**: 9787521762181
- **阅读状态**: 已读完

## 内容简介

李飞飞的自传，记录了她从中国移民到美国、成为人工智能领域顶尖科学家的历程。书中交织了个人成长故事与人工智能（尤其是计算机视觉）的发展史，从 ImageNet 的诞生到 AI 伦理的思考。

## 目录

1. 如坐针毡的华盛顿之行
2. 逐梦之旅
3. 鸿沟渐窄
4. 心智探索
5. 第一道光
6. 北极星
7. 一个假设
8. 实验验证
9. 万物以外是什么
10. 似易实难
11. 无人可控
12. 下一颗北极星
```

- [ ] **Step 3: 创建 media/.gitkeep**

```bash
touch books/the-worlds-i-see/media/.gitkeep
```

- [ ] **Step 4: 提交**

```bash
git add books/the-worlds-i-see/
git commit -m "feat: add 《我看见的世界》book directory and README"
```

---

### Task 3: 产出第 1-4 章笔记

**Files:**
- Create: `books/the-worlds-i-see/notes/chapter-01.md`
- Create: `books/the-worlds-i-see/notes/chapter-02.md`
- Create: `books/the-worlds-i-see/notes/chapter-03.md`
- Create: `books/the-worlds-i-see/notes/chapter-04.md`

- [ ] **Step 1: 搜索第 1-4 章内容摘要**

搜索李飞飞《我看见的世界》每章核心内容，提取技术要点和人文故事。

- [ ] **Step 2: 编写 chapter-01.md（如坐针毡的华盛顿之行）**

模板：
```markdown
# 第 1 章 · 如坐针毡的华盛顿之行

## A. 章节概述

（简要概括本章内容）

## B. 技术要点

（AI/计算机视觉相关技术概念，以 `卷积神经网络（Convolutional Neural Network, CNN）` 格式标注术语）

## C. 人文/思想洞察

（人文关怀、科学家精神、社会影响等）

## D. 关键人物/事件

（本章提到的重要人物、里程碑事件）

## E. AI 补充背景

（书中未展开但相关的背景知识）

---

<!-- NOTES: 在此处补充个人感想 -->
```

按此模板填充每章内容。

- [ ] **Step 3: 同样编写 chapter-02 ~ chapter-04**

每章按模板产出。

- [ ] **Step 4: 提交**

```bash
git add books/the-worlds-i-see/notes/chapter-0*.md
git commit -m "feat: add chapters 1-4 notes for 《我看见的世界》"
```

---

### Task 4: 产出第 5-8 章笔记

**Files:**
- Create: `books/the-worlds-i-see/notes/chapter-05.md`
- Create: `books/the-worlds-i-see/notes/chapter-06.md`
- Create: `books/the-worlds-i-see/notes/chapter-07.md`
- Create: `books/the-worlds-i-see/notes/chapter-08.md`

- [ ] **Step 1: 搜索第 5-8 章内容摘要**
- [ ] **Step 2: 编写 chapter-05 ~ chapter-08**
- [ ] **Step 3: 提交**

```bash
git add books/the-worlds-i-see/notes/chapter-0[5-8].md
git commit -m "feat: add chapters 5-8 notes for 《我看见的世界》"
```

---

### Task 5: 产出第 9-12 章笔记

**Files:**
- Create: `books/the-worlds-i-see/notes/chapter-09.md`
- Create: `books/the-worlds-i-see/notes/chapter-10.md`
- Create: `books/the-worlds-i-see/notes/chapter-11.md`
- Create: `books/the-worlds-i-see/notes/chapter-12.md`

- [ ] **Step 1: 搜索第 9-12 章内容摘要**
- [ ] **Step 2: 编写 chapter-09 ~ chapter-12**
- [ ] **Step 3: 提交**

```bash
git add books/the-worlds-i-see/notes/chapter-09.md books/the-worlds-i-see/notes/chapter-10.md books/the-worlds-i-see/notes/chapter-11.md books/the-worlds-i-see/notes/chapter-12.md
git commit -m "feat: add chapters 9-12 notes for 《我看见的世界》"
```

---

### Task 6: 创建 raw/ 占位文件

**Files:**
- Create: `books/the-worlds-i-see/raw/chapter-01.md` ~ `chapter-12.md`

- [ ] **Step 1: 创建 12 章 raw 占位文件**

每文件内容为：
```markdown
# 第 X 章 · [章节名]

> 个人感想与标注区域。阅读 notes/ 对应章节后在此补充。

---
```

- [ ] **Step 2: 提交**

```bash
git add books/the-worlds-i-see/raw/
git commit -m "feat: add raw personal notes placeholder files"
```
