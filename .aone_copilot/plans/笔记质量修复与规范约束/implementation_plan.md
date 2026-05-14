# 笔记质量修复与规范约束

修复三本书已产出笔记中发现的 10 处事实性错误、4 类术语规范问题、2 处 README 链接缺失、1 处结构性缺失（raw/ 目录），并新增配置文件约束术语格式和文件命名规范。

## Proposed Changes

### 1. 事实性错误修正（《我看见的世界》）

#### [MODIFY] [chapter-08.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/books/the-worlds-i-see/notes/chapter-08.md)

**修正 1：Caffe 混淆（D 节，Krizhevsky 条目）**

```diff
- 负责将 Hinton 的理论构想转化为能够在两块 GTX 580 上高效运行的 CUDA C++/Caffe 代码。
+ 负责将 Hinton 的理论构想转化为能够在两块 GTX 580 上高效运行的 CUDA C++ 代码（注：Caffe 框架由贾扬清于 2013 年开发，AlexNet 的原始训练代码并非基于 Caffe）。
```

**修正 2：Sutskever 角色（D 节，Sutskever 条目）**

```diff
- 2018 年带领团队开发了 GPT 系列大语言模型。
+ 作为 OpenAI 首席科学家主导了 GPT-2/3/4 等大语言模型的研发方向（GPT-1 于 2018 年发表，核心作者为 Alec Radford 等人）。
```

**修正 3：DNN Research 收购价（D 节，Hinton 条目）**

```diff
- DNN Research 被 Google 以 4400 万美元收购——这个价格现在看是 AI 史上最便宜的收购。
+ DNN Research 被 Google 收购（传闻价格约 4400 万美元，但该数字未经官方确认）——无论实际金额多少，这都是 AI 史上最被低估的收购之一。
```

#### [MODIFY] [chapter-01.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/books/the-worlds-i-see/notes/chapter-01.md)

**修正 4：Brockman 职称（D 节）**

```diff
- **Greg Brockman**：OpenAI 联合创始人兼 CTO，同场证人
+ **Greg Brockman**：OpenAI 联合创始人兼总裁（President），同场证人
```

**修正 5：HCAI → HAI 统一（B 节）**

```diff
- **以人为本的人工智能（Human-Centered AI, HCAI）**
+ **以人为本的人工智能（Human-Centered AI, HAI）**
```

#### [MODIFY] [chapter-05.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/books/the-worlds-i-see/notes/chapter-05.md)

**修正 6：WordNet 创建者（B 节）**

```diff
- 普林斯顿大学语言学家克里斯蒂安·费尔鲍姆（Christiane Fellbaum）等人构建的英语词汇层级分类体系
+ 普林斯顿大学认知科学家乔治·米勒（George A. Miller）创建、克里斯蒂安·费尔鲍姆（Christiane Fellbaum）共同编辑的英语词汇层级分类体系
```

#### [MODIFY] [topic-the-real-story-behind-the-legend.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/books/the-worlds-i-see/raw/topic-the-real-story-behind-the-legend.md)

**修正 7：h-index 数值**

```diff
- 300+ 论文，h-index ~150。
+ 300+ 论文，h-index ~180+（截至 2025 年中，Google Scholar 数据）。
```

#### [MODIFY] [topic-cnn-rnn-transformer-explained.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/books/the-worlds-i-see/raw/topic-cnn-rnn-transformer-explained.md)

**修正 8：CNN 平移不变性措辞**

在提到"平移不变性"处添加精确说明：CNN 卷积层本身具有的是平移等变性（translation equivariance），加上池化层后才近似实现平移不变性（translation invariance）。

---

### 2. 事实性标注（《人工智能之不能》）

#### [MODIFY] [chapter-02.md (notes)](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/books/what-ai-cannot-do/notes/chapter-02.md)

**修正 9：充足理由律归类标注（A 节）**

```diff
- 本章梳理形式逻辑的四大基本规律（同一律、矛盾律、排中律、充足理由律）
+ 本章梳理形式逻辑的四大基本规律（同一律、矛盾律、排中律、充足理由律）。注：充足理由律由莱布尼茨提出，学术界通常将其归入认识论/形而上学范畴而非经典形式逻辑的基本规律，此处沿用原书分类。
```

**修正 10：补全术语英文标注（B 节）**

```diff
- **同一律**：A = A
+ **同一律（Law of Identity）**：A = A
- **矛盾律**：¬(P ∧ ¬P)
+ **矛盾律（Law of Non-Contradiction）**：¬(P ∧ ¬P)
- **排中律**：P ∨ ¬P
+ **排中律（Law of Excluded Middle）**：P ∨ ¬P
- **充足理由律**：任何判断必须有充足的理由才能被认定为真。
+ **充足理由律（Principle of Sufficient Reason）**：任何判断必须有充足的理由才能被认定为真。
```

---

### 3. README 链接修复

#### [MODIFY] [README.md (what-ai-cannot-do)](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/books/what-ai-cannot-do/README.md)

更新 raw 列链接，将已有内容的文件从 `—` 改为实际链接：

```diff
- | 第二章 · 基本的逻辑 | [→](notes/chapter-02.md) | — |
+ | 第二章 · 基本的逻辑 | [→](notes/chapter-02.md) | [→](raw/chapter-02.md) |
```

---

### 4. 结构性补全

#### 为《世界的逻辑》创建 raw/ 目录

当前 `books/the-logic-of-the-world/` 缺少 `raw/` 目录，与另外两本书结构不一致。

创建 `books/the-logic-of-the-world/raw/.gitkeep` 以确保目录被 Git 跟踪。

---

### 5. 新增术语规范与文件命名约束配置

#### [NEW] [writing-conventions.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/.claude/memory/writing-conventions.md)

新增一个 memory 文件，约束术语格式和文件命名规范，确保两个 AI 环境都能遵循：

```markdown
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
- ❌ CNN（卷积神经网络）——顺序颠倒
- ❌ 卷积神经网络 ——缺英文
- ❌ Convolutional Neural Network ——缺中文

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

## 笔记写入前检查清单

每次写入 notes/ 或 raw/ 文件前，AI 必须自检以下事项：

1. [ ] 术语首次出现是否按「中文（English, 缩写）」格式标注？
2. [ ] 文件名是否符合命名规范？
3. [ ] 提到的人物职称/角色是否经过验证？
4. [ ] 引用的数据（年份、数字、论文作者）是否准确？如不确定，标注"据报道"或"约"
5. [ ] raw/ 文件是否包含完整的双层结构（对话记录 + 要点提炼 + 待继续思考）？
6. [ ] 与已有笔记是否存在事实性矛盾？

此清单不需要输出给用户，AI 内部自检即可。
```

#### [MODIFY] [MEMORY.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/.claude/memory/MEMORY.md)

在索引中添加新文件：

```diff
 - [项目操作无需确认](feedback-permissions.md)
+- [写作规范](writing-conventions.md) — 术语格式、文件命名、写入前检查清单
```

#### [MODIFY] [CLAUDE.md](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/CLAUDE.md)

在"项目规范"段落中补充对 writing-conventions.md 的引用：

```diff
 ## 项目规范
 
 - 语言：中文正文，技术术语保留英文并括号注中文/缩写
 - 例：卷积神经网络（Convolutional Neural Network, CNN）
 - 笔记模板：每章固定 A~E 五个板块
+- 术语格式与文件命名详细规范见：`.claude/memory/writing-conventions.md`
```

---

### 6. 清理空壳占位文件（《我看见的世界》raw/）

#### [DELETE] raw/chapter-01.md ~ raw/chapter-12.md（12 个文件）

这 12 个文件每个仅 123-124 字节，只有占位符模板：
```
# 第 X 章 · [章节名]
> 个人感想与标注区域。阅读 notes/ 对应章节后在此补充。
---
```

它们从未被填充，且与当前的"对话驱动"工作流不匹配（raw/ 应该由对话自动沉淀产生，而非预创建空壳）。notes/ 文件尾部的 `<!-- NOTES -->` 注释中已有个人感想，后续可在对话中自然产生 raw/ 文件。

> [!IMPORTANT]
> 删除后，《我看见的世界》README 的 raw 列链接会失效（chapter-01~12）。需同步更新 README 表格，将这些 raw 链接改为 `—`。

#### [MODIFY] [README.md (the-worlds-i-see)](file:///Users/xuhu/workspace/xuhuLocal/reading-book-notes/books/the-worlds-i-see/README.md)

将 raw 列中 chapter-01~12 的链接改为 `—`：

```diff
- | 1. 如坐针毡的华盛顿之行 | [→](notes/chapter-01.md) | [→](raw/chapter-01.md) |
+ | 1. 如坐针毡的华盛顿之行 | [→](notes/chapter-01.md) | — |
```
（对 12 个章节全部执行此修改）

---

## 不做的事情（及原因）

| 决策 | 原因 |
|------|------|
| **不整合 chapter-04/05 的内容重叠** | 两章笔记各有侧重（04 偏学术动机，05 偏家庭抉择），且内容量大，整合风险高。在两章各自添加一行注释即可 |
| **不修改 raw/ topic 文件的术语风格** | raw/ 的定位是"保留对话语气"，不要求严格术语标注。约束仅对新写入的文件生效 |
| **不删除《人工智能之不能》和《世界的逻辑》的空壳 notes/** | 这些书仍在阅读中，骨架有预期填充价值 |

## Verification Plan

### Automated Tests
- `git diff --stat` 确认所有改动文件和行数符合预期
- 检查所有 Markdown 链接是否有效（无断链）

### Manual Verification
- 在新会话中读取 `writing-conventions.md`，模拟一次笔记写入，验证检查清单是否可执行
- 检查修正后的事实性内容是否准确

---
生成时间: 2026/5/14 11:16:11
planId: b9ae458b-bc8b-4783-b561-ef84a88d0150
plan_status: review