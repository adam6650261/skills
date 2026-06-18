# Matt Pocock Skills — 中文指南

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

一套面向真正工程实践的 Agent 技能集合——不是 vibe coding，而是扎扎实实的软件工程。

开发真实应用是困难的。GSD、BMAD、Spec-Kit 等方法试图通过掌控整个流程来解决这个问题，但它们在掌控的同时也剥夺了你的控制权，并且让流程中的 bug 难以定位和修复。

这些技能的设计理念是：**小巧、易适配、可组合**。它们适配任何模型，基于数十年的工程经验打磨而成。你可以随意改造它们，让它们成为你自己的工具。

> 本仓库是 [mattpocock/skills](https://github.com/mattpocock/skills) 的 fork。

---

## 快速开始（30 秒配置）

1. 运行 skills.sh 安装器：

```bash
npx skills@latest add mattpocock/skills
```

2. 选择你需要的技能，以及要安装到哪些编码 Agent 上。**务必选择 `/setup-matt-pocock-skills`**。

3. 在 Agent 中运行 `/setup-matt-pocock-skills`。它会：
   - 询问你使用哪个 Issue 跟踪器（GitHub 或本地文件）
   - 询问你对工单分类时使用的标签（`/triage` 会用到标签）
   - 询问你希望将生成的文档保存在哪里

4. 大功告成。

---

## 为什么需要这些技能

这些技能旨在解决 Claude Code、Codex 等编程 Agent 中常见的四类失败模式。

### 问题一：Agent 没有按我的想法做事

> "没有人能确切知道自己想要什么。"
>
> — David Thomas & Andrew Hunt，《程序员修炼之道》

**根因**：软件开发中最常见的失败模式就是认知偏差。你以为开发者（Agent）知道你想要什么，结果看到产物才发现——它根本没理解你的意图。

在 AI 时代这个问题完全一样。你和 Agent 之间存在沟通鸿沟。修复方法是一场**拷问式访谈**——让 Agent 详细追问你到底要构建什么。

**解决方案**：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md) — 适用于非编码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) — 和 `/grill-me` 相同，但附带更多功能（见下文）

这些是最受欢迎的技能。它们帮助你在动手之前和 Agent 对齐认知，并促使你深入思考自己要做出的改变。**每次**你想要做出改动时，都建议先用它们。

### 问题二：Agent 话太多了

> "有了通用语言，开发者之间的交流以及代码的表达都源自同一个领域模型。"
>
> — Eric Evans，《领域驱动设计》

**根因**：项目初期，开发者和软件的受众（领域专家）通常说着不同的语言。

我和 Agent 之间也感受到了同样的张力。Agent 通常被扔进一个项目，被要求自己摸索术语。结果它们用 20 个词表达本来 1 个词就能说清的东西。

**解决方案**：建立一套**共享语言**。它是一个文档，帮助 Agent 解码项目中使用的术语。

<details>
<summary>示例</summary>

这是来自我的 `course-video-manager` 仓库的 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md) 示例。哪个更容易阅读？

- **之前**："当课程中某个章节下的某节课被设为'真实'时（即在文件系统中获得一个位置），会出现问题"
- **之后**："物化级联（materialization cascade）出了问题"

这种简洁性在一次次对话中持续累积收益。

</details>

这一机制内置于 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) 中。它不仅是一次拷问式访谈，还帮助你与 AI 建立共享语言，并将难以解释的决策记录为 ADR（架构决策记录）。

很难说清楚这有多强大。它可能是这个仓库里最酷的技巧。试一试，你就明白了。

> **提示**：共享语言除了减少冗余外还有更多好处：
>
> - **变量、函数和文件命名一致**，基于共享语言
> - 因此，**Agent 更容易在代码库中导航**
> - Agent 在**思考时消耗的 token 更少**，因为可以用更精炼的语言表达

### 问题三：代码跑不通

> "始终走小步、走稳每一步。反馈的速度就是你的速度上限。永远不要接一个太大的任务。"
>
> — David Thomas & Andrew Hunt，《程序员修炼之道》

**根因**：即便你和 Agent 对需求对齐了，当 Agent 仍然产出烂代码时怎么办？

是时候审视你的反馈回路了。如果没有对代码运行情况的反馈，Agent 就是在盲飞。

**解决方案**：你需要常规的反馈回路：静态类型检查、浏览器访问、自动化测试。

对于自动化测试，**红-绿-重构**循环至关重要。Agent 先写一个失败测试，再修复它——这能给 Agent 持续级别的反馈，产出质量更高的代码。

我构建了 **[`/tdd`](./skills/engineering/tdd/SKILL.md)**，可以接入任何项目。它鼓励红-绿-重构循环，并给 Agent 提供了充足的好/坏测试指导。

对于调试，我还构建了 **[`/diagnosing-bugs`](./skills/engineering/diagnosing-bugs/SKILL.md)**，将最佳调试实践封装为一个简单的循环流程。

### 问题四：我们构建了一坨💩山

> "每一天都要投资于系统的设计。"
>
> — Kent Beck，《解析极限编程》

> "最好的模块是深的。它们通过简单的接口让大量功能可被访问。"
>
> — John Ousterhout，《软件设计的哲学》

**根因**：大多数用 Agent 构建的应用都复杂且难以修改。因为 Agent 可以极大地加速编码，它们也同时加速了软件熵增。代码库以前所未有的速度变得复杂。

**解决方案**：一种全新的 AI 驱动开发方式——**关注代码的设计**。

这已经融入到了技能体系的每一层：

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md) — 在创建 PRD 之前，追问你涉及哪些模块

至关重要的是，[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) 帮助你拯救已经变成泥球的代码库。建议每隔几天在代码库上运行一次。

### 总结

软件工程基本功比以往任何时候都重要。这些技能是我将这些基本功浓缩为可重复实践的最佳成果，帮助你交付职业生涯中最好的应用。

---

## 技能组织架构

技能按用途分类到 `skills/` 目录下的分组（bucket）文件夹中：

| 分组 | 用途 |
|------|------|
| `engineering/` | 日常编码工作 |
| `productivity/` | 日常非编码工作流工具 |
| `misc/` | 保留但较少使用 |
| `personal/` | 与个人配置相关，不对外推广 |
| `in-progress/` | 尚未完成发布的草稿 |
| `deprecated/` | 已废弃不再使用 |

每个技能都有一份 `SKILL.md`，其中包含技能的全部指令和元数据。

### 技能调用方式

技能沿一条轴线分为两类——**谁能调用它**：

- **用户调用（User-invoked）**：只能由人类输入调用（`disable-model-invocation: true`）。在 `SKILL.md` 的 frontmatter 中设置。这类技能的角色是编排指挥。
- **模型调用（Model-invoked）**：可由模型自动触发或用户手动调用。这类技能包含丰富的触发措辞，让 Agent 在合适场景下自动使用它们。

> **重要规则**：用户调用技能可以调用模型调用技能，但绝不能调用另一个用户调用技能。

---

## 技能总览

### Engineering（工程开发）

日常编码工作技能。

#### 用户调用

| 技能 | 说明 |
|------|------|
| **[/ask-matt](./skills/engineering/ask-matt/SKILL.md)** | 帮你判断当前场景适合用哪个技能或流程——一个到所有用户调用技能的路由器 |
| **[/grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** | 拷问式访谈 + 构建领域模型，边聊边更新 `CONTEXT.md` 和 ADR |
| **[/triage](./skills/engineering/triage/SKILL.md)** | 通过状态机驱动 Issue 的分类流转 |
| **[/improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** | 扫描代码库寻找深化机会，生成可视化 HTML 报告，然后针对你的选择进行拷问式深入 |
| **[/setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)** | 配置本仓库用于工程技能（Issue 跟踪器、分类标签、领域文档布局）。每个仓库运行一次 |
| **[/to-issues](./skills/engineering/to-issues/SKILL.md)** | 将任何计划、规格或 PRD 拆分为可独立领取的 Issue（使用垂直切片） |
| **[/to-prd](./skills/engineering/to-prd/SKILL.md)** | 将当前对话转化为 PRD 并发布到 Issue 跟踪器。无需访谈——直接综合已讨论的内容 |
| **[/prototype](./skills/engineering/prototype/SKILL.md)** | 构建一次性原型来充实设计——可以是针对状态/业务逻辑问题的终端应用，也可以是多种不同 UI 变体的可切换展示 |

#### 模型调用

| 技能 | 说明 |
|------|------|
| **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)** | 困难 Bug 和性能回归的诊断循环：复现 → 最小化 → 假设 → 埋点 → 修复 → 回归测试 |
| **[tdd](./skills/engineering/tdd/SKILL.md)** | 红-绿-重构循环的测试驱动开发。一次构建一个垂直切片 |
| **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)** | 主动构建和打磨项目的领域模型——用术语表检验术语、用边界场景压力测试、实时更新 `CONTEXT.md` 和 ADR |
| **[codebase-design](./skills/engineering/codebase-design/SKILL.md)** | 设计深层模块的共享准则和词汇：大量行为封装在小接口后，位于清晰的接缝处，通过该接口可测试 |

### Productivity（效率工具）

通用工作流工具，非编码专属。

#### 用户调用

| 技能 | 说明 |
|------|------|
| **[/grill-me](./skills/productivity/grill-me/SKILL.md)** | 就一个计划或设计对你进行无情的追问，直到决策树的每个分支都有了答案 |
| **[/handoff](./skills/productivity/handoff/SKILL.md)** | 将当前对话压缩为交接文档，让另一个 Agent 可以继续工作 |
| **[/teach](./skills/productivity/teach/SKILL.md)** | 在多轮会话中教你新技能或概念，使用当前目录作为有状态的教学工作区 |
| **[/writing-great-skills](./skills/productivity/writing-great-skills/SKILL.md)** | 编写和编辑优秀技能的参考指南：让技能行为可预测的词汇与原则 |

#### 模型调用

| 技能 | 说明 |
|------|------|
| **[grilling](./skills/productivity/grilling/SKILL.md)** | 就一个计划或设计对用户进行无情的追问，是 `grill-me` 和 `grill-with-docs` 背后的可复用循环 |

### Misc（杂项工具）

保留但较少使用。

| 技能 | 说明 |
|------|------|
| **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)** | 设置 Claude Code hooks 来拦截危险的 git 命令（push、reset --hard、clean 等） |
| **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.md)** | 将测试文件中的 `as` 类型断言迁移到 @total-typescript/shoehorn |
| **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.md)** | 创建包含章节、问题、解答和讲解的练习目录结构 |
| **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.md)** | 在当前仓库中配置 Husky pre-commit hooks（lint-staged、Prettier、类型检查、测试） |

---

## 领域模型

本项目定义了一套精简的领域术语：

- **Issue Tracker（问题跟踪器）**：托管一个仓库 Issue 的工具——GitHub Issues、Linear、或本地 `.scratch/` 标记文件约定等。`to-issues`、`to-prd`、`triage` 等技能会从中读写数据。

- **Issue（问题）**：问题跟踪器中的一个被跟踪的工作单元——Bug、任务、PRD，或由 `to-issues` 产出的切片。

- **Triage Role（分类角色）**：一个规范的状态机标签，应用于 Issue 分类流程（如 `needs-triage`、`ready-for-afk`）。每个角色通过 `docs/agents/triage-labels.md` 映射到问题跟踪器中的实际标签字符串。

关系：
- 一个问题跟踪器包含多个 Issue
- 一个 Issue 在任一时刻只携带一个分类角色

---

## 相关资源

- 官方仓库：[mattpocock/skills](https://github.com/mattpocock/skills)
- 技能安装器：[skills.sh](https://skills.sh/mattpocock/skills)
- Matt Pocock 的 Newsletter：[AI Hero](https://www.aihero.dev/s/skills-newsletter)
- 调用方式详解：[docs/invocation.md](./docs/invocation.md)
