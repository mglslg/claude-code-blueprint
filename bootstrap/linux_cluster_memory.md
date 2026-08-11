请将本机初始化为 cybercrew 集群节点。

**架构说明（记忆架构 v2.2，2026-08-11 起）：**

- `~/.claude/CLAUDE.md` 只放集群最小模板（中文交流 / 模型签名 / 多模型互评），不含任何记忆规则
- 服务器管理记忆放在集群仓库 `mglslg/cybercrew` 的 `nodes/<节点名>/` 目录下，随仓库同步，本机必须持有 clone
- 管理服务器 = 从 `~/github/cybercrew` 目录开启 Claude Code 会话（该仓库根目录的 CLAUDE.md 会接管记忆规则）；其它项目的会话不加载任何服务器记忆
- 集群有且仅有一个 leader（Harry），新节点一律注册为 worker，禁止自封 leader

执行前先向 super（当前对话的人类用户）询问两样东西：**本机节点名**和 **GitHub Token**（若 `gh auth status` 显示已登录则不用要 token）。其余全部自动完成，请严格按以下步骤执行。

## 第一步：配置节点身份

```bash
git config --global user.name "<节点名>"
git config --global user.email "<节点名>@entromatrix.com"
```

## 第二步：安装并登录 gh

1. 检查 `gh auth status`。未安装时优先从集群 OSS 分发桶获取：
   ```bash
   curl -fsSL -o /tmp/gh.deb https://oss.entropymatrix.site/mirror/claude/gh.deb && sudo dpkg -i /tmp/gh.deb
   ```
   OSS 不可用时再走 GitHub 官方 apt 源（cli.github.com）。
2. 未登录时用 super 提供的 token 登录：`echo "TOKEN" | gh auth login --with-token`
   **token 用完即弃，严禁写入任何仓库、Issue 或记忆文件。**
3. 执行 `gh auth setup-git`，让 git 对 github.com 走 token 认证（clone/push 私有仓库需要）。

## 第三步：写入全局最小模板

用集群权威源**整体覆盖** `~/.claude/CLAUDE.md`（如本机存在旧版记忆架构模板，一律整体替换，不要追加或保留）：

```bash
mkdir -p ~/.claude
gh api repos/mglslg/cybercrew/contents/knowledge/config/node-claude-template.md --jq '.content' | base64 -d > ~/.claude/CLAUDE.md
```

## 第四步：clone 集群仓库

```bash
mkdir -p ~/github && cd ~/github && gh repo clone mglslg/cybercrew
```

## 第五步：按协议完成注册

通读 `~/github/cybercrew/RULES.md`，执行其中「新节点注册（含记忆库初始化）」流程：复制 `nodes/_template/` 为 `nodes/<节点名>/` → 探测本机环境填入 `server.md` → 登记 `CREW.md` → 创建 `node/<节点名>` 标签 → commit + push → Issue #1 报到（签名）。

若本机存在旧版 `~/.llm-workspace/` 目录：把其中有价值的内容并入 `nodes/<节点名>/` 对应文件（密钥进 `.secrets.md`，不入库），然后删除 `~/.llm-workspace/`。

## 第六步：向 super 汇报

汇报内容：节点名与 git 身份、CREW.md 注册结果、报到帖链接、当前指派给本机的任务列表：

```bash
gh issue list --repo mglslg/cybercrew --label "node/<节点名>" --state open
```

---

**以上就是全部操作。请现在开始执行。**
