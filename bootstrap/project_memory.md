请在当前项目中初始化一套持久化工作记忆系统。请严格按照以下步骤操作，不要遗漏任何一步。

## 第一步：创建目录结构

在项目根目录下创建以下文件夹：

```
.claude/
.llm-workspace/
└── logs/
```

## 第二步：创建 .llm-workspace/project.md

创建文件 `.llm-workspace/project.md`，写入以下内容：

```markdown
# 项目知识

> 存放项目架构、技术细节、设计约定等不会频繁变化的项目信息。
> 对项目产生新理解时更新此文件。

（请在了解项目后补充此处内容）
```

## 第三步：创建 .llm-workspace/memory.md

创建文件 `.llm-workspace/memory.md`，写入以下内容：

```markdown
# 长期记忆

> 存放用户偏好、反复强调的事、长期记忆。
> 每条记忆必须带时间戳，格式如下。

### [YYYY-MM-DD] 记忆标题

记忆内容...
```

## 第四步：创建 .llm-workspace/rules.md

创建文件 `.llm-workspace/rules.md`，写入以下内容：

```markdown
# 项目代码风格规范

> 本文件存放所有项目代码风格相关的铁律，写代码时必须无条件遵守。

（请在了解项目代码风格后补充此处内容）
```

## 第五步：创建 .llm-workspace/todo.md

创建文件 `.llm-workspace/todo.md`，写入以下内容：

```markdown
# 工作状态

## 当前阶段

（在这里写当前正在进行的工作阶段）

## 卡点

（无）

## 进行中

（无）

## 待办

（无）
```

## 第六步：在 .claude/CLAUDE.md 中写入记忆系统规则

创建文件 `.claude/CLAUDE.md`，写入以下内容：

```markdown
# 记忆架构与自主进化规则

> 本文件只存放记忆系统的文件体系说明和自主进化规则。
> 项目知识 → `.llm-workspace/project.md`
> 用户偏好与长期记忆 → `.llm-workspace/memory.md`
> 代码风格铁律 → `.llm-workspace/rules.md`
> 工作任务 → `.llm-workspace/todo.md`
> 操作日志 → `.llm-workspace/logs/YYYY-MM-DD.md`

---

## 文件体系

.claude/
├── CLAUDE.md                     ← 本文件：只放记忆架构规则
└── settings.json                ← 权限配置

.llm-workspace/
├── project.md                   ← 项目知识（架构、技术细节、约定）
├── memory.md                    ← 用户偏好、反复强调的事、长期记忆（带时间戳）
├── rules.md                     ← 代码风格铁律（无条件遵守）
├── todo.md                      ← 当前阶段 + 卡点 + 进行中 + 待办
└── logs/                        ← 按日操作日志（不自动加载，按需检索）
    └── YYYY-MM-DD.md
└── resources/                   ← 项目引用资源以及llm临时创建的文件都应该放在这个里面

---

## 自主进化规则

以下规则是强制性的，每次对话都必须遵守，不可省略。

### 规则 1：对话开始时恢复上下文

每次新对话开始时，你必须做的第一件事是读取以下文件：
- .llm-workspace/project.md
- .llm-workspace/memory.md
- .llm-workspace/rules.md
- .llm-workspace/todo.md

这些文件优先级完全一样，都必须读取。读取后你就知道项目背景、用户偏好、代码风格、当前进度。不要跳过这一步。

### 规则 2：各文件职责分工

- project.md：项目架构、技术细节、设计约定。对项目产生新理解时更新。
- memory.md：用户偏好、反复强调的事、用户说"小本本"/"记住"/"记一下"时的内容。每条必须带时间戳，格式为 ### [YYYY-MM-DD] 标题。
- rules.md：代码风格铁律。写代码时必须无条件遵守的规范。用户提到代码风格相关指示时更新。
- todo.md：当前阶段、卡点、进行中任务、待办任务。工作状态变化时更新。
- logs/YYYY-MM-DD.md：当天操作记录、完成的任务、技术决策。有实际操作时追加。

### 规则 3：todo.md 只放未完成任务

- 只包含未完成的任务
- 任务完成后立即从中删除
- 删除的同时把完成记录写到当天的 logs/YYYY-MM-DD.md 中
- 不要在 todo.md 里保留"已完成"条目

### 规则 4：对话过程中持续更新

- 开始一个任务时 → 更新 todo.md 的"进行中"
- 完成一个任务时 → 从 todo.md 中删除，记入当天 log
- 新增任务时 → 添加到 todo.md
- 遇到卡点时 → 写入 todo.md 的"卡点"
- 对项目有新理解时 → 写入 project.md
- 代码有修改时 → 同步更新 project.md 中受影响的部分（架构、接口、依赖等）
- 运维/部署环境有变化时（端口、域名、进程管理、nginx 等）→ 同步更新 project.md
- 用户说"记一下"/"小本本"/"记住" → 写入 memory.md（带时间戳）
- 用户不耐烦反复告知同一件事 → 写入 memory.md（带时间戳）
- 用户提到代码风格规范 → 写入 rules.md

### 规则 5：对话结束前保存状态

每当完成一个阶段性工作时，必须：
1. 更新 todo.md
2. 追加当天 logs/YYYY-MM-DD.md
3. 如有新的项目理解或用户偏好，同步写入 project.md、memory.md 或 rules.md

### 规则 6：README.md 同步

当代码逻辑与实现和 README.md 有出入时，同步更新 README.md。

### 规则 7：日志文件不自动加载

.llm-workspace/logs/ 目录下的文件不在对话开始时读取。只在用户要求查看历史记录时才去读取。

### 规则 8：.llm-workspace下不能随便创建文件

如果有临时创建的文件，统一放到`.llm-workspace/resources/`这个目录下
```

## 第七步：确认

完成以上所有步骤后，告诉我你创建了哪些文件，并读取一遍 `.claude/CLAUDE.md` 确认规则已正确写入。

---

**以上就是全部操作。请现在开始执行。**
