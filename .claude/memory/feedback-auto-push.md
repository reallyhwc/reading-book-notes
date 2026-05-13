---
name: feedback-auto-push
description: 笔记类 commit 完成后自动 push，不区分分支
type: feedback
---

本项目不需要分支管理，commit 完成后直接 `git push`。

**Why:** 用户确认——"除非重构，不需要区分分支的概念，commit 完了之后就可以顺便 push 下"。这是个人笔记项目的简单工作流，没有 PR/review 需求。

**How to apply:** 每次完成笔记沉淀 commit 后，在同一轮中执行 `git push`。
