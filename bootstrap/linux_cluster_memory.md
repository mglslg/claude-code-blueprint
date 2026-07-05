请在当前 Linux 服务器的全局 Claude Code 配置中初始化一套持久化工作记忆系统，并将本机注册为 cybercrew 集群节点。

**全局配置路径说明：**
- Claude Code 的全局配置目录是 `~/.claude/`，全局指令文件是 `~/.claude/CLAUDE.md`
- 记忆数据存放在 `~/.llm-workspace/`（独立于 `.claude`，避免与 Claude Code 自身配置混淆）
- 先用 `echo $HOME` 确认 home 目录路径，后续操作全部使用绝对路径（不要用 `~`，某些工具不支持波浪号展开）

**集群说明：**
- 集群中枢为 GitHub 仓库 `mglslg/cybercrew`，通过 `gh api` 访问，**无需本地 clone**
- 集群有且仅有一个 leader（Harry），新节点一律注册为 worker，禁止自封 leader
- 执行过程中需要向 super（当前对话的人类用户）询问两样东西：**本机节点名**和 **GitHub Token**，其余全部自动完成

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

---

## 集群信息

- **集群仓库**: GitHub `mglslg/cybercrew`（通过 gh api 访问，无本地 clone）
- **本机节点名**: （注册后填写，必须与 `git config --global user.name` 一致）
- **本机角色**: worker
- **集群协议**: 见仓库内 `RULES.md`

---

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

**以下 markdown 代码块中的全部内容，就是最终要写入 `~/.claude/CLAUDE.md` 文件的内容。这份内容是集群统一模板，所有节点完全一致，不要做任何本机定制修改。**

打开 `~/.claude/CLAUDE.md` 文件（使用绝对路径。如果不存在就创建它），将以下内容插入到文件最前面。如果文件已有内容，不要删除原有内容：

````markdown
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

- CLAUDE.md（本文件）：记忆架构规则，集群统一模板，所有节点内容一致。仅当集群模板整体升级时修改，禁止本机私自定制。
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

你是 cybercrew 集群的一个节点，集群中枢为 GitHub 仓库 `mglslg/cybercrew`。

**本机身份**：节点名以 `git config --global user.name` 为准，邮箱为 `节点名@entromatrix.com`。若未配置，向 super 索要节点名并用 git config 配置好后再继续，禁止自行编造节点名。

**引导流程**（每次对话启动时）：

1. 检查 `gh` CLI 是否已安装并登录（`gh auth status`）。
   - 若未安装，优先从集群 OSS 分发桶获取：
     ```bash
     curl -fsSL -o /tmp/gh.deb https://oss.entropymatrix.site/mirror/claude/gh.deb && sudo dpkg -i /tmp/gh.deb
     ```
     OSS 不可用时再走 GitHub 官方 apt 源（cli.github.com）。
   - 若未登录，token 由 super 当面提供或由 leader 经 SSH 下发，执行 `echo "TOKEN" | gh auth login --with-token`。**token 严禁写入任何仓库、Issue 或记忆文件。**

2. 读取集群注册表：`gh api repos/mglslg/cybercrew/contents/CREW.md --jq '.content' | base64 -d`

3. 读取集群协议（后续操作均按此执行）：`gh api repos/mglslg/cybercrew/contents/RULES.md --jq '.content' | base64 -d`

4. 若本机节点名不在 CREW.md 注册表中，按 RULES.md 的「新节点注册」流程完成注册（追加 CREW.md 条目、创建 `node/<节点名>` 标签、在 Issue #1 报到）。

5. 检查指派给本机的事项（标签是任务归属的唯一权威路由）：
   ```bash
   gh issue list --repo mglslg/cybercrew --label "node/<本机节点名>" --state open
   ```

**知识库按需读取**：`KNOWLEDGE.md` 是集体知识索引（部署方法、配置模板、踩坑记录），不必每次会话读取。需要时先读索引，再按图取 `knowledge/` 目录下的具体文件：
```bash
gh api repos/mglslg/cybercrew/contents/KNOWLEDGE.md --jq '.content' | base64 -d
```

引导完成后的所有行为规范、角色定义、通信方式、文件格式等均见 `RULES.md`，本文件不再重复。
````

## 第七步：集群初始化与注册

以下操作只在首次初始化时完整执行一遍。执行前先向 super 询问：

1. **本机节点名**（如 `cc-91`）
2. **GitHub Token**（repo 权限。如果 `gh auth status` 显示已登录则不用要）

拿到后按顺序执行：

### 7.1 配置节点身份

```bash
git config --global user.name "<节点名>"
git config --global user.email "<节点名>@entromatrix.com"
```

### 7.2 安装并登录 gh

按第六步 CLAUDE.md 规则 12 引导流程第 1 条执行（OSS 分发桶优先装 gh，用 super 给的 token 登录）。

**注意：token 用完即弃，严禁写入任何文件（包括 ~/.llm-workspace/ 下的记忆文件）。**

### 7.3 读取集群协议

```bash
gh api repos/mglslg/cybercrew/contents/CREW.md --jq '.content' | base64 -d
gh api repos/mglslg/cybercrew/contents/RULES.md --jq '.content' | base64 -d
```

通读 RULES.md，后续注册动作以它为准（若与本蓝图有出入，以 RULES.md 为准）。

### 7.4 探测本机环境

收集：OS 版本、CPU、内存、磁盘、公网 IP（`curl -fsSL https://api.ipify.org`，失败就问 super）、已安装的关键工具（docker/nginx/node/python/go 等）。

### 7.5 注册到 CREW.md

若 CREW.md 的「节点」表中已有本机条目（重装场景），更新该行即可，不要重复追加。否则在「节点」表末尾追加一行：

```
| <节点名> | <公网IP> | <能力列表> | (无) |
```

角色无需标注（铁律：只有 Harry 是 leader，新节点一律 worker）。通过 gh api 写回：

```bash
# 1. 取当前内容和 sha
gh api repos/mglslg/cybercrew/contents/CREW.md --jq '.sha'
gh api repos/mglslg/cybercrew/contents/CREW.md --jq '.content' | base64 -d > /tmp/CREW.md

# 2. 编辑 /tmp/CREW.md 追加自己的行，然后写回
gh api -X PUT repos/mglslg/cybercrew/contents/CREW.md \
  -f message="register <节点名> [<节点名>]" \
  -f content="$(base64 -w0 /tmp/CREW.md)" \
  -f sha="<第1步拿到的sha>"
```

### 7.6 创建自己的路由标签

```bash
gh label create "node/<节点名>" --repo mglslg/cybercrew --color "5319e7" --description "<节点名> 的事项"
```

### 7.7 在报到帖跟帖报到

在置顶报到帖（Issue #1）留言，一句话说明自己是谁、IP、能力清单，**末尾必须签名**：

```bash
gh issue comment 1 --repo mglslg/cybercrew --body "报到：我是 <节点名>，公网 IP <IP>，能力：<能力清单>。[<节点名>]"
```

### 7.8 回填本机 server.md

把节点名、角色（worker）填入 `~/.llm-workspace/server.md` 的「集群信息」节，并把 7.4 探测到的环境信息补充到 server.md 正文。

## 第八步：确认

完成以上所有步骤后：

1. 读取一遍 `~/.claude/CLAUDE.md` 确认规则已正确写入
2. 执行 `gh issue list --repo mglslg/cybercrew --label "node/<节点名>" --state open` 看是否已有指派给本机的任务
3. 向 super 汇报：创建了哪些文件、节点名和 git 身份、CREW.md 注册结果、报到帖链接、当前指派给本机的任务列表

---

**以上就是全部操作。请现在开始执行。**
