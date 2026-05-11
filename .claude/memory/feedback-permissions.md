---
name: feedback-permissions
description: 项目操作如创建目录、搭建骨架等无需逐次询问权限
type: feedback
---

项目内的文件操作（创建目录、搭建笔记骨架、写入 raw/、更新 project-state）无需逐次询问权限，直接执行即可。

**Why:** 用户表示"后续此类操作可以不用问我"——这些都属于笔记项目工作流内部的常规操作，逐次确认只会增加摩擦。

**How to apply:** 在读书笔记项目内，所有读写文件、创建目录、git commit（自动沉淀场景）都不用再确认。但 git push 等影响远程的操作仍需确认。
