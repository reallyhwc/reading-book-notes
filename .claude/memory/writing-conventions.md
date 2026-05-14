---
name: writing-conventions
description: 笔记写作规范——术语格式、文件命名、格式检查清单
type: project
---
# 写作规范

## 术语标注格式（强制）

所有技术术语首次出现时必须按以下格式标注：

**中文全称（English Full Name, 缩写）**

示例：
- 卷积神经网络（Convolutional Neural Network, CNN）
- 以人为本的人工智能（Human-Centered AI, HAI）
- 反向传播（Backpropagation）——无常用缩写时省略缩写

反例（禁止）：
- 错误：CNN（卷积神经网络）——顺序颠倒
- 错误：卷积神经网络 ——缺英文
- 错误：Convolutional Neural Network ——缺中文

同一术语在同一文件中后续出现时，可只用中文或缩写，不必重复标注。

## 术语一致性（强制）

以下术语在全项目中必须统一使用指定写法：

| 统一写法 | 禁止写法 |
|---------|---------|
| 以人为本的人工智能（Human-Centered AI, HAI） | HCAI |
| 视觉词袋（Bag of Visual Words, BoVW） | 词袋模型（Bag of Words） |
| 平移等变性（Translation Equivariance） | 平移不变性（仅在含池化时可用） |

如需新增统一术语，直接在此表中追加。

## 文件命名规范

### notes/ 目录
- 章节笔记：`chapter-XX.md`（XX 为两位数字，01-99）
- 前言/序言：`chapter-00-preface.md` 或 `chapter-00-introduction.md`
- 结语/后记：`chapter-XX-conclusion.md` 或 `chapter-XX-postscript.md`

### raw/ 目录
- 章节对话：`chapter-XX.md`（与 notes 对应）
- 跨章节专题：`topic-<slug>.md`（slug 为英文短横线分隔）
- 跨书比较：`topic-cross-<slug>.md`（文件头注明涉及的书）

### 文件头元信息（raw/ 文件必须包含）

```
# 专题/章节：标题
> 触发：触发对话的具体内容
> 日期：YYYY-MM-DD
```

## 文件创建时机

- **notes/chapter-XX.md**：当用户聊到某章内容、且对话中产生了足够的结构化信息时创建。不预创建空骨架。
- **raw/chapter-XX.md 或 raw/topic-*.md**：当对话满足自动沉淀触发条件时创建（见 feedback-auto-recording.md）。
- **新书加入时**：仅创建 `books/<slug>/README.md` 和 `notes/`、`raw/`、`media/` 三个空目录（含 .gitkeep）。

## notes/ 模板使用规则

A~F 六个板块的使用原则：

- **A. 章节概述**：必填。每章必须有概述。
- **B. 技术要点**：按需。技术类章节填写，人文叙事章节可省略。
- **C. 人文/思想洞察**：按需。有人文维度的章节填写。
- **D. 关键人物/事件**：按需。有重要人物/里程碑事件时填写。
- **E. AI 补充背景**：按需。有值得补充的背景知识时填写。
- **F. 个人批注**：按需。有个人触动、质疑或联想时填写。

原则：宁可某个板块空缺，也不要机械填充没有实质价值的内容。一个只有 A + C + F 三个板块的笔记，比一个五个板块但内容稀薄的笔记更有价值。

## 笔记写入前检查清单

每次写入 notes/ 或 raw/ 文件前，AI 必须自检以下事项：

1. 术语首次出现是否按「中文（English, 缩写）」格式标注？
2. 文件名是否符合命名规范？
3. 提到的人物职称/角色是否经过验证？
4. 引用的数据（年份、数字、论文作者）是否准确？如不确定，标注"据报道"或"约"
5. raw/ 文件是否包含完整的双层结构（对话记录 + 要点提炼 + 待继续思考）？
6. 与已有笔记是否存在事实性矛盾？

此清单不需要输出给用户，AI 内部自检即可。
