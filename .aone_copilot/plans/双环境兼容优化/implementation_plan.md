# 双环境兼容优化：Aone Copilot / Claude Code + DeepSeek

本次修改旨在解决读书笔记项目在两个 AI 环境间切换时的 7 个已识别问题，确保配置文件对两边都友好。

## Proposed Changes

### 1. 修复矛盾指令

#### [MODIFY] [feedback-permissions.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/.claude/memory/feedback-permissions.md)

当前 `feedback-permissions.md` 说 "git push 等影响远程的操作仍需确认"，与 `feedback-auto-push.md` 的 "commit 完成后直接 git push" 直接矛盾。不同模型会对矛盾指令做出不同处理。

```diff
- 在读书笔记项目内，所有读写文件、创建目录、git commit（自动沉淀场景）都不用再确认。但 git push 等影响远程的操作仍需确认。
+ 在读书笔记项目内，所有读写文件、创建目录、git commit + push（自动沉淀场景）都不用再确认（详见 feedback-auto-push.md）。
```

---

### 2. 增强 CLAUDE.md 的跨环境兼容性

#### [MODIFY] [CLAUDE.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/CLAUDE.md)

当前的 "Read .claude/memory/xxx" 伪指令在非 Claude Code 环境中无效。需要：
- 将关键状态信息内联到 CLAUDE.md 中，作为"最低限度上下文"
- 新增"环境兼容"段落，声明项目支持多环境
- 新增"跨书讨论"和"元讨论"的归档规则，填补 raw/ 触发条件的盲区

改动后的 CLAUDE.md 结构：

```markdown
# CLAUDE.md — 读书笔记项目协作指引

## 快速状态（每次会话的最低上下文）

- 当前书籍：《世界的逻辑》（马兆远）— 阅读中，第一章起
- 暂停书籍：《人工智能之不能》（马兆远）— 第二章
- 已完成：《我看见的世界》（李飞飞）— 12 章 + 3 专题
- 详细状态：.claude/memory/project-state.md
- 用户偏好：.claude/memory/user-preferences.md

## 环境兼容

本项目支持在以下环境中使用：
- **Claude Code + DeepSeek**：原生环境，memory 文件自动加载，superpowers skill 可用
- **Aone Copilot（IDE 插件）**：需手动读取 memory 文件恢复上下文；superpowers skill 需通过 read_file 加载后手动遵循流程

两个环境共享同一套文件和 Git 仓库，切换时无需额外操作。

## 会话连续性

每次新会话启动时，读取以下记忆文件（如环境不自动加载，需手动读取）：
- .claude/memory/project-state.md — 当前进度
- .claude/memory/user-preferences.md — 协作偏好

（以下板块保持原有内容不变：项目规范、笔记板块、协作流程等）
```

> [!IMPORTANT]
> "快速状态"段落需要在每次切换书籍时同步更新，与 project-state.md 保持一致。这是跨环境兼容的关键——非 Claude Code 环境依赖此段落获取最低上下文。

---

### 3. 补全 raw/ 触发条件盲区

#### [MODIFY] [feedback-auto-recording.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/.claude/memory/feedback-auto-recording.md)

新增两条触发条件，覆盖跨书比较和阅读方法论讨论：

```diff
 ## 触发条件（满足任一条即触发）
 1. 用户提到章节号
 2. 用户讨论书中具体概念
 3. 用户分享对书中内容的感想、质疑、触动
 4. 我主动讲解或延伸了书中涉及的技术/思想概念
+5. 用户进行跨书比较（涉及两本或以上书的内容对比）
+6. 用户讨论阅读方法论、笔记系统本身的改进思考
```

同时在执行步骤中新增跨书/元讨论的归档规则：

```diff
 ## 执行步骤
 1. 写 `raw/chapter-XX.md` 或 `raw/topic-<slug>.md`
+   - 跨书比较 → 写到**当前阅读**书目录的 `raw/topic-cross-<slug>.md`，文件头注明涉及的书
+   - 阅读方法论 → 写到 `.claude/memory/journal-<date>.md`（项目级反思，不属于任何一本书）
 2. 更新 `.claude/memory/project-state.md`
 3. commit
 4. push
```

---

### 4. 统一 MEMORY.md 索引

#### [MODIFY] [MEMORY.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/.claude/memory/MEMORY.md)

将环境兼容信息加入索引，让任何模型读到 MEMORY.md 时都能快速理解项目的多环境特性：

```diff
 - [项目状态](project-state.md)
 - [用户偏好](user-preferences.md)
 - [自动记录笔记](feedback-auto-recording.md)
 - [笔记 commit 后自动 push](feedback-auto-push.md)
 - [项目操作无需确认](feedback-permissions.md)
+
+环境说明：本项目在 Claude Code + DeepSeek 和 Aone Copilot 两个环境间切换使用。
+非 Claude Code 环境需手动读取上述文件恢复上下文，CLAUDE.md 中的"快速状态"段落提供最低限度信息。
```

---

### 5. 归档过时的 superpowers 文档

#### [MODIFY] [docs/superpowers/specs/2026-05-03-reading-book-notes-design.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/docs/superpowers/specs/2026-05-03-reading-book-notes-design.md)

在文件头部添加归档标记，说明此文档是项目创建时的初始设计，当前权威文档是 CLAUDE.md：

```diff
+> **[ARCHIVED]** 本文档是项目创建时的初始设计稿，部分内容已过时（如 raw/ 的定位已从"用户补充标注"演化为"对话式深度讨论"）。当前权威文档是 [CLAUDE.md](/CLAUDE.md) 和 [README.md](/README.md)。
+
 # Reading Book Notes — 项目设计
```

#### [MODIFY] [docs/superpowers/plans/2026-05-03-reading-book-notes-plan.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/docs/superpowers/plans/2026-05-03-reading-book-notes-plan.md)

同样添加归档标记：

```diff
+> **[ARCHIVED]** 本文档是项目搭建阶段的实施计划，所有任务已完成。保留作为项目演化记录。
+
 # Reading Book Notes Implementation Plan
```

---

## 不做的事情（及原因）

| 决策 | 原因 |
|------|------|
| **不重命名 `.claude/` 目录** | 这是 Claude Code 的约定目录，改名会破坏 Claude Code 的自动加载机制。通过 CLAUDE.md 中的"环境兼容"段落来桥接即可 |
| **不删除 notes/ 空骨架文件** | 《世界的逻辑》的 16 个骨架已经被 commit 并推送，删除会产生不必要的 git 噪音。保持现状，后续聊到哪章就填充哪章 |
| **不删除 superpowers 文档** | 虽然过时，但作为项目演化记录有价值，加归档标记即可 |

## Verification Plan

### Automated Tests

- 检查修改后的文件没有 Markdown 语法错误
- `git diff` 确认所有改动符合预期
- `git commit` + `git push` 验证工作流正常

### Manual Verification

- 在 Claude Code + DeepSeek 环境中开新会话，验证 CLAUDE.md 的"快速状态"段落能被正确读取
- 在 Aone Copilot 环境中开新会话，验证手动读取 memory 文件后能恢复完整上下文
- 模拟一次跨书比较对话，验证新的触发条件和归档规则是否清晰

---
生成时间: 2026/5/14 11:02:16
planId: b9ae458b-bc8b-4783-b561-ef84a88d0150
plan_status: review