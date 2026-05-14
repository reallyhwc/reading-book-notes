> **[ARCHIVED]** 本文档是项目创建时的初始设计稿，部分内容已过时（如 raw/ 的定位已从"用户补充标注"演化为"对话式深度讨论"）。当前权威文档是 [CLAUDE.md](/CLAUDE.md) 和 [README.md](/README.md)。

# Reading Book Notes — 项目设计

## 目标

搭建一个以 Markdown 文档为基础、Git 管理的读书笔记项目，支持多本书管理，AI 辅助产出笔记，用户补充个人感想。

## 顶层结构

```
reading-book-notes/
├── README.md                    # 书房索引
├── CLAUDE.md                    # AI 协作指引
├── .gitignore
├── books/
│   └── the-worlds-i-see/        # 《我看见的世界》
│       ├── README.md            # 本书概览
│       ├── notes/               # AI 产出的完整笔记
│       │   └── chapter-XX.md
│       ├── raw/                 # 用户补充标注
│       │   └── chapter-XX.md
│       └── media/               # 相关图片
└── docs/                        # 项目说明
```

## 每章笔记模板

五个固定板块：
- A. 章节概述
- B. 技术要点
- C. 人文/思想洞察
- D. 关键人物/事件
- E. AI 补充背景

## 语言规范

中文正文，技术术语保留英文并括号注中文/缩写。
例：卷积神经网络（Convolutional Neural Network, CNN）

## Git 管理

- 按书分 commit，便于追溯
- .gitignore 忽略系统文件

## AI 协作流程

1. 用户指定书籍 → AI 产出全书 notes
2. 用户阅读，在 raw/ 中补充感想
3. 可选：用户标记触动点 → AI 深度扩展
