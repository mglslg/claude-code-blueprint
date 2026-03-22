请在当前用户的全局 Claude Code 配置中初始化一套持久化工作记忆系统。

**全局配置路径说明：**
- Claude Code 的全局配置目录是 `~/.claude/`（即用户 home 目录下的 `.claude` 文件夹）
- 全局指令文件是 `~/.claude/CLAUDE.md`，对所有项目生效
- 先用 `echo $HOME` 确认 home 目录路径，后续操作全部使用绝对路径（不要用 `~`，某些工具不支持波浪号展开）

请严格按照以下步骤操作，不要遗漏任何一步。

## 第一步：确认 home 目录并创建目录结构

先执行 `echo $HOME` 获取 home 目录路径。

然后创建以下目录（如果 `~/.claude` 已存在则不要覆盖已有文件）：

```
~/.claude/
└── workspace/
    └── logs/
```

例如 home 目录是 `/root`，则创建 `/root/.claude/workspace/logs/`。

## 第二步：创建 ~/.claude/workspace/project.md

创建文件 `~/.claude/workspace/project.md`（使用绝对路径），写入以下内容：

```markdown
# 全局项目知识

> 存放跨项目的通用技术知识、基础设施信息等。
> 如果某个项目有自己的 .claude/workspace/project.md，以项目级为准。

（请在了解用户工作后补充此处内容）
```

## 第三步：创建 ~/.claude/workspace/memory.md

创建文件 `~/.claude/workspace/memory.md`（使用绝对路径），写入以下内容：

```markdown
# 长期记忆

> 存放用户偏好、反复强调的事、长期记忆。
> 每条记忆必须带时间戳，格式如下。

### [YYYY-MM-DD] 记忆标题

记忆内容...
```

## 第四步：创建 ~/.claude/workspace/todo.md

创建文件 `~/.claude/workspace/todo.md`（使用绝对路径），写入以下内容：

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

## 第五步：在 ~/.claude/CLAUDE.md 中写入记忆系统规则

打开 `~/.claude/CLAUDE.md` 文件（使用绝对路径。如果不存在就创建它），在文件最前面插入以下内容。如果文件已有内容，不要删除原有内容，把以下内容插入到最前面：

```markdown
# 全局记忆架构与自主进化规则

> 本文件只存放记忆系统的文件体系说明和自主进化规则。
> 全局项目知识 → `workspace/project.md`
> 用户偏好与长期记忆 → `workspace/memory.md`
> 工作任务 → `workspace/todo.md`
> 操作日志 → `workspace/logs/YYYY-MM-DD.md`
>
> 注意：以上路径均在 ~/.claude/ 下。读写时请始终使用绝对路径（将 ~ 替换为实际 home 目录）。

---

## 文件体系

~/.claude/
├── CLAUDE.md                     ← 本文件：只放记忆架构规则
├── workspace/
│   ├── project.md               ← 全局项目知识
│   ├── memory.md                ← 用户偏好、反复强调的事、长期记忆（带时间戳）
│   ├── todo.md                  ← 当前阶段 + 卡点 + 进行中 + 待办
│   └── logs/                    ← 按日操作日志（不自动加载，按需检索）
│       └── YYYY-MM-DD.md
└── settings.json                ← 权限配置

---

## 自主进化规则

以下规则是强制性的，在所有项目的每次对话中都必须遵守，不可省略。

### 规则 1：对话开始时恢复上下文

每次新对话开始时，你必须做的第一件事是读取以下三个文件（使用绝对路径）：
- ~/.claude/workspace/project.md
- ~/.claude/workspace/memory.md
- ~/.claude/workspace/todo.md

这三个文件优先级完全一样，都必须读取。读取后你就知道当前状态。不要跳过这一步。

### 规则 2：各文件职责分工

- CLAUDE.md（本文件）：记忆架构规则。仅当记忆体系本身需要调整时修改。
- project.md：跨项目的通用知识和基础设施信息。
- memory.md：用户偏好、反复强调的事、用户说"小本本"/"记住"/"记一下"时的内容。每条必须带时间戳，格式为 ### [YYYY-MM-DD] 标题。
- todo.md：当前阶段、卡点、进行中任务、待办任务。工作状态变化时更新。
- logs/YYYY-MM-DD.md：当天操作记录、完成的任务、技术决策。有实际操作时追加。需要标注涉及的项目名称或路径。

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
- 对项目或基础设施有新理解时 → 写入 project.md
- 代码有修改时 → 同步更新 project.md 中受影响的部分（架构、接口、依赖等）
- 运维/部署环境有变化时（端口、域名、进程管理、nginx 等）→ 同步更新 project.md
- 用户说"记一下"/"小本本"/"记住" → 写入 memory.md（带时间戳）
- 用户不耐烦反复告知同一件事 → 写入 memory.md（带时间戳）

### 规则 5：对话结束前保存状态

每当完成一个阶段性工作时，必须：
1. 更新 todo.md
2. 追加当天 logs/YYYY-MM-DD.md
3. 如有新的理解或用户偏好，同步写入 project.md 或 memory.md

### 规则 6：README.md 同步

当代码逻辑与实现和 README.md 有出入时，同步更新 README.md。

### 规则 7：日志文件不自动加载

workspace/logs/ 目录下的文件不在对话开始时读取。只在用户要求查看历史记录时才去读取。

### 规则 8：全局 vs 项目级记忆

- 本记忆系统是全局的，记录跨项目的状态和任务
- 如果某个项目自己也有 .claude/workspace/ 目录，那个是项目级记忆，优先使用项目级的
- 全局记忆记录的是：跨项目任务、用户偏好、通用待办
- 项目级记忆记录的是：该项目内部的具体进度和任务
- 两者不冲突，同时维护
```

## 第六步：确认

完成以上所有步骤后，告诉我你创建了哪些文件，并读取一遍 `~/.claude/CLAUDE.md` 确认规则已正确写入。

---

**以上就是全部操作。请现在开始执行。**
