# 笔记工作流优化

基于前面几轮对话中的深度分析，对读书笔记项目的工作流进行 4 项配置级改进，提升实际使用中的体验。所有改动仅涉及配置文件和模板约定，不重构目录结构。

## Proposed Changes

### 1. notes/ 按需生成，不预创建骨架

#### [MODIFY] [CLAUDE.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/CLAUDE.md)

在"协作流程"段落中，将"每本书新增时，按已有模板创建目录和笔记骨架"修改为按需创建：

```diff
 ## 协作流程（对话驱动模式）
 
 1. 用户在对话中聊感想、触动、质疑 → AI 提炼并写入 raw/ 对应文件
 2. raw/ 中的笔记积累后，可能反向更新 notes/ 中的正式笔记
-3. 每本书新增时，按已有模板创建目录和笔记骨架
+3. 每本书新增时，仅创建 README.md 和目录结构（notes/, raw/, media/），不预创建 chapter 骨架文件。聊到哪章时按需创建对应的 notes/ 和 raw/ 文件。
```

#### [MODIFY] [writing-conventions.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/.claude/memory/writing-conventions.md)

在文件命名规范段落后新增"文件创建时机"规则：

```markdown
## 文件创建时机

- **notes/chapter-XX.md**：当用户聊到某章内容、且对话中产生了足够的结构化信息时创建。不预创建空骨架。
- **raw/chapter-XX.md 或 raw/topic-*.md**：当对话满足自动沉淀触发条件时创建（见 feedback-auto-recording.md）。
- **新书加入时**：仅创建 `books/<slug>/README.md` 和 `notes/`、`raw/`、`media/` 三个空目录（含 .gitkeep）。
```

---

### 2. raw/ 支持追加模式

#### [MODIFY] [feedback-auto-recording.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/.claude/memory/feedback-auto-recording.md)

在执行步骤中新增追加模式规则：

```diff
 ## 执行步骤（严格顺序，不可拆分到下一轮）
 
 1. 写 `raw/chapter-XX.md` 或 `raw/topic-<slug>.md`
    - 跨书比较 → 写到**当前阅读**书目录的 `raw/topic-cross-<slug>.md`，文件头注明涉及的书
    - 阅读方法论 → 写到 `.claude/memory/journal-<date>.md`（项目级反思，不属于任何一本书）
+   - **追加规则**：如果已存在同主题的 raw/ 文件，追加新的对话记录到该文件中（用日期分割线区分轮次），而不是创建新文件。格式如下：
+     ```
+     ---
+     > 追加：YYYY-MM-DD
+     
+     （新一轮对话内容）
+     ```
+     追加后同步更新文件末尾的"要点提炼"和"待继续思考"部分。
 2. 更新 `.claude/memory/project-state.md`
```

---

### 3. notes/ 新增 F. 个人批注板块

#### [MODIFY] [CLAUDE.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/CLAUDE.md)

在"笔记板块"段落中新增 F 节：

```diff
 ## 笔记板块
 
 - A. 章节概述 — 简要概括本章内容
 - B. 技术要点 — 技术概念解析，含背景知识和延伸阅读
 - C. 人文/思想洞察 — 人文关怀、科学家精神、社会影响
 - D. 关键人物/事件 — 重要人物、里程碑事件
 - E. AI 补充背景 — 书中未展开但相关的背景知识
+- F. 个人批注 — 阅读时的个人触动、质疑和联想（从 `<!-- NOTES -->` 注释迁移为可见内容）
```

同时在"项目规范"段落中更新模板描述：

```diff
-- 笔记模板：每章固定 A~E 五个板块
+- 笔记模板：每章最多 A~F 六个板块（A 必填，B~F 按章节内容特点选填）
```

> [!IMPORTANT]
> 已有的 notes/ 文件中的 `<!-- NOTES -->` HTML 注释不需要立即批量迁移。后续聊到对应章节时，自然地将注释内容提升为 F 节即可。

---

### 4. A~E 模板改为按需选填

#### [MODIFY] [writing-conventions.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/.claude/memory/writing-conventions.md)

新增模板使用规则：

```markdown
## notes/ 模板使用规则

A~F 六个板块的使用原则：

- **A. 章节概述**：必填。每章必须有概述。
- **B. 技术要点**：按需。技术类章节填写，人文叙事章节可省略。
- **C. 人文/思想洞察**：按需。有人文维度的章节填写。
- **D. 关键人物/事件**：按需。有重要人物/里程碑事件时填写。
- **E. AI 补充背景**：按需。有值得补充的背景知识时填写。
- **F. 个人批注**：按需。有个人触动、质疑或联想时填写。

原则：宁可某个板块空缺，也不要机械填充没有实质价值的内容。一个只有 A + C + F 三个板块的笔记，比一个五个板块但内容稀薄的笔记更有价值。
```

---

## 不做的事情（及原因）

| 决策 | 原因 |
|------|------|
| **不引入 concepts/ 概念节点层** | 当前只有 3 本书，跨书概念交叉还不够密集。建议在第 4 本书开始时再引入 |
| **不迁移到 Obsidian** | 当前 "对话即笔记" 的工作流在 Claude Code / Aone Copilot 中无法被 GUI 工具替代。未来可考虑将 Git 仓库作为 Obsidian vault 打开以获得图谱可视化 |
| **不批量迁移已有 notes/ 的 `<!-- NOTES -->` 注释** | 后续聊到对应章节时自然迁移即可，避免一次性大量改动 |

## Verification Plan

### Automated Tests
- `git diff --stat` 确认改动文件数和行数符合预期
- 检查修改后的 Markdown 无语法错误

### Manual Verification
- 模拟一次新书加入流程，验证不再预创建 notes/ 骨架
- 模拟一次对已有 topic 文件的追加对话，验证追加格式是否清晰
- 在新会话中读取配置文件，验证 A~F 按需选填的规则是否明确

---
生成时间: 2026/5/14 11:48:11
planId: b9ae458b-bc8b-4783-b561-ef84a88d0150
plan_status: review