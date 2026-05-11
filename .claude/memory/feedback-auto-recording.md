---
name: feedback-auto-recording
description: 对话结束后必须自动沉淀笔记到 raw/，不可跳过
type: feedback
---

每次涉及书中内容的实质性对话结束后，**在同一轮回复中**执行以下操作，不用等用户提醒：

1. 写 raw/ 文件（`raw/chapter-XX.md` 或 `raw/topic-<slug>.md`）
2. 更新 `memory/project-state.md`
3. commit（**仅此场景允许自动 commit**，其他场景不要自动提交）

文件命名：
- `raw/chapter-XX.md` — 按章节组织的个人笔记
- `raw/topic-<slug>.md` — 跨章节专题讨论

**Why:** 用户不想每次提醒 AI 沉淀笔记，这应该是自动化的流程。

**How to apply:** 任何涉及书中内容的讨论结束后，立即在同一轮中完成写入 + 更新状态 + commit。
