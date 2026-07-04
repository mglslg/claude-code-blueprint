请在当前 Linux 服务器的全局 Claude Code 配置中初始化一套持久化工作记忆系统。

**全局配置路径说明：**
- Claude Code 的全局配置目录是 `~/.claude/`，全局指令文件是 `~/.claude/CLAUDE.md`
- 记忆数据存放在 `~/.llm-workspace/`（独立于 `.claude`，避免与 Claude Code 自身配置混淆）
- 先用 `echo $HOME` 确认 home 目录路径，后续操作全部使用绝对路径（不要用 `~`，某些工具不支持波浪号展开）

请严格按照以下步骤操作，不要遗漏任何一步。

## 第一步：确认 home 目录并创建目录结构

先执行 `echo $HOME` 获取 home 目录路径。

然后创建以下目录（如果已存在则不要覆盖已有文件）：

```
~/.llm-workspace/
├── logs/
└── resources/
```

例如 home 目录是 `/home/admin`，则创建 `/home/admin/.llm-workspace/logs/` 和 `/home/admin/.llm-workspace/resources/`。

## 第二步：创建 ~/.llm-workspace/server.md

创建文件 `~/.llm-workspace/server.md`（使用绝对路径），写入以下内容：

```markdown
# 服务器信息

> 存放当前服务器的相关信息：主机信息、硬件配置、安装的服务、配置路径、部署目录、网络拓扑等。
> 对服务器环境有新了解时更新此文件。

（请在了解服务器后补充此处内容）
```

## 第三步：创建 ~/.llm-workspace/memory.md

创建文件 `~/.llm-workspace/memory.md`（使用绝对路径），写入以下内容：

```markdown
# 长期记忆

> 存放用户偏好、反复强调的事、长期记忆。
> 每条记忆必须带时间戳，格式如下。

### [YYYY-MM-DD] 记忆标题

记忆内容...
```

## 第四步：创建 ~/.llm-workspace/rules.md

创建文件 `~/.llm-workspace/rules.md`（使用绝对路径），写入以下内容：

```markdown
# 服务器管理铁律

> 本文件存放服务器管理相关的原则、铁律和军规，运维操作时必须无条件遵守。

（请在了解服务器管理规范后补充此处内容）
```

## 第五步：创建 ~/.llm-workspace/todo.md

创建文件 `~/.llm-workspace/todo.md`（使用绝对路径），写入以下内容：

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

## 第六步：在 ~/.claude/CLAUDE.md 中写入记忆系统规则

**以下 markdown 代码块中的全部内容，就是最终要写入 `~/.claude/CLAUDE.md` 文件的内容。**

打开 `~/.claude/CLAUDE.md` 文件（使用绝对路径。如果不存在就创建它），将以下内容插入到文件最前面。如果文件已有内容，不要删除原有内容：

```markdown
# 全局记忆架构与自主进化规则

> 本文件只存放记忆系统的文件体系说明和自主进化规则。
> 服务器信息 → `~/.llm-workspace/server.md`
> 用户偏好与长期记忆 → `~/.llm-workspace/memory.md`
> 服务器管理铁律 → `~/.llm-workspace/rules.md`
> 工作任务 → `~/.llm-workspace/todo.md`
> 操作日志 → `~/.llm-workspace/logs/YYYY-MM-DD.md`
> 临时文件 → `~/.llm-workspace/resources/`
>
> 注意：读写时请始终使用绝对路径（将 ~ 替换为实际 home 目录）。

---

## 文件体系

~/
├── .claude/
│   ├── CLAUDE.md                 ← 本文件：只放记忆架构规则
│   └── settings.json             ← 权限配置
└── .llm-workspace/
    ├── server.md                 ← 服务器信息（主机、服务、配置路径、部署目录）
    ├── memory.md                 ← 用户偏好、反复强调的事、长期记忆（带时间戳）
    ├── rules.md                  ← 服务器管理铁律（无条件遵守）
    ├── todo.md                   ← 当前阶段 + 卡点 + 进行中 + 待办
    ├── logs/                     ← 按日操作日志（不自动加载，按需检索）
    │   └── YYYY-MM-DD.md
    └── resources/                ← 引用资源以及 LLM 临时创建的文件

---

## 自主进化规则

以下规则是强制性的，在所有项目的每次对话中都必须遵守，不可省略。

### 规则 1：对话开始时恢复上下文

每次新对话开始时，你必须做的第一件事是读取以下四个文件（使用绝对路径）：
- ~/.llm-workspace/server.md
- ~/.llm-workspace/memory.md
- ~/.llm-workspace/rules.md
- ~/.llm-workspace/todo.md

这些文件优先级完全一样，都必须读取。读取后你就知道当前服务器环境、用户偏好、管理铁律、工作进度。不要跳过这一步。

### 规则 2：各文件职责分工

- CLAUDE.md（本文件）：记忆架构规则。仅当记忆体系本身需要调整时修改。
- server.md：服务器主机信息、硬件配置、安装的服务（nginx、docker、数据库等）、配置路径、部署目录、网络拓扑、定时任务等。对服务器环境有新了解时更新。运维/部署环境有变化时同步更新。
- memory.md：用户偏好、反复强调的事、用户说"小本本"/"记住"/"记一下"时的内容。每条必须带时间戳，格式为 ### [YYYY-MM-DD] 标题。用户不耐烦反复告知同一件事时也写入。
- rules.md：服务器管理铁律。运维操作时必须无条件遵守的原则和军规。用户提到服务器管理相关规范时更新。
- todo.md：当前阶段、卡点、进行中任务、待办任务。工作状态变化时更新。
- logs/YYYY-MM-DD.md：当天操作记录、完成的任务、技术决策。有实际操作时追加。需要标注涉及的项目名称或路径。
- resources/：LLM 临时创建的文件、引用资源等统一放在此目录。

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
- 对服务器环境有新了解时 → 写入 server.md
- 运维/部署环境有变化时（端口、域名、进程管理、nginx 等）→ 同步更新 server.md
- 用户说"记一下"/"小本本"/"记住" → 写入 memory.md（带时间戳）
- 用户不耐烦反复告知同一件事 → 写入 memory.md（带时间戳）
- 用户提到服务器管理规范 → 写入 rules.md

### 规则 5：对话结束前保存状态

每当完成一个阶段性工作时，必须：
1. 更新 todo.md
2. 追加当天 logs/YYYY-MM-DD.md
3. 如有新的服务器信息或用户偏好，同步写入 server.md、memory.md 或 rules.md

### 规则 6：README.md 同步

当代码逻辑与实现和 README.md 有出入时，同步更新 README.md。

### 规则 7：日志文件不自动加载

~/.llm-workspace/logs/ 目录下的文件不在对话开始时读取。只在用户要求查看历史记录时才去读取。

### 规则 8：.llm-workspace 下不能随便创建文件

如果有临时创建的文件，统一放到 `~/.llm-workspace/resources/` 目录下。

### 规则 9：记忆系统对外不可见

~/.llm-workspace/ 是整个记忆系统的工作区，对外部绝对不可见：
- .llm-workspace 中的笔记可以引用外部内容（如其他项目路径、外部文档链接等）
- 项目内容（README.md、源码、配置文件等）决不可引用 ~/.llm-workspace/ 中的任何内容
- 记忆系统是私有工作空间，不在任何项目仓库中暴露

### 规则 10：全局 vs 项目级记忆

- 本记忆系统（~/.llm-workspace/）是服务器全局的，记录跨项目、跨会话的状态和任务
- 如果某个项目有自己的 .llm-workspace/ 目录，那个是项目级记忆，进入该项目时优先使用项目级的
- 全局记忆记录的是：服务器环境、跨项目任务、用户偏好、通用待办
- 项目级记忆记录的是：该项目内部的具体架构、进度和任务
- 两者不冲突，同时维护

### 规则 11：工作规范

在梳理需求、编写代码、设计代码时，有任何你觉得前后矛盾或者前后逻辑不一致的地方，或者有任何模糊的地方，都要先和用户确认清楚再动手，避免反复修改！

### 规则 12：集群协作

你是 cybercrew 集群的一个节点，节点角色和 ID 见 cybercrew 仓库的 `CREW.md`。集群仓库路径记录在各节点的 `~/.llm-workspace/server.md` 中。

**首次初始化**（本机 server.md 中没有集群仓库路径时）：
1. 向用户询问本机 cybercrew 仓库的本地绝对路径
2. 写入 `~/.llm-workspace/server.md` 的「集群信息」节
3. `cd` 到仓库目录，`git pull`
4. 探测本机环境（OS/CPU/内存/磁盘/网络/已安装服务），补录到仓库的 `CREW.md`
5. `git commit` + `git push`

**后续每次启动时**：
1. `cd` 到集群仓库，`git pull`
2. 读取 `CREW.md`、`SIGNAL.md`、`BOARD.md` 了解集群状态和待办

**结束时**：
1. 如有需要他人知晓的操作 → 写入对应 issue 的 `thread.yaml` 或在 `SIGNAL.md` 追加消息
2. `git commit` + `git push`

**角色**：
- hcm (本机) 是 **leader**：拆任务、指派 worker、review 验收
- 其他节点是 **worker**：认领任务、执行、通过 `need` 求助 leader
- super (用户) 是最高决策者：`decide` 覆盖一切，worker 仅域名/密钥/预算三类问题可 `escalate` 到 super

**通信**：默认 BBS 模式（SIGNAL + issue thread.yaml），特殊情况 leader 可 SSH 直连 worker。

**协议细节**见 cybercrew 仓库的 `RULES.md`。
```

## 第七步：确认

完成以上所有步骤后，告诉我你创建了哪些文件，并读取一遍 `~/.claude/CLAUDE.md` 确认规则已正确写入。

---

**以上就是全部操作。请现在开始执行。**
