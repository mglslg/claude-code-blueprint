# 交互式权限配置

请通过交互问答的方式，帮助用户配置 Claude Code 的权限。按照以下步骤严格执行。

## 操作步骤

### 第一步：确认 .claude 目录存在

检查当前项目根目录下是否存在 `.claude` 目录。如果不存在，创建它。

### 第二步：检查已有配置

检查是否已存在 `.claude/settings.local.json` 文件。如果存在，读取并展示当前配置给用户看，询问用户是要在此基础上修改，还是重新配置。

### 第三步：逐项询问用户需要哪些权限

依次询问以下每一项，用户回答"是"则加入允许列表，回答"否"则跳过。每次只问一项，不要一次全部列出：

1. **Shell 命令（Bash）**：是否允许自由执行 shell 命令？（如 npm、git、python 等）
2. **文件读取（Read/Glob/Grep）**：是否允许自由读取文件和搜索代码？
3. **文件写入（Write/Edit）**：是否允许自由创建和修改文件？
4. **网络访问（WebFetch/WebSearch）**：是否允许访问网页和搜索引擎？
5. **子代理（Agent）**：是否允许启动子代理执行任务？
6. **Jupyter Notebook（NotebookEdit）**：是否允许编辑 Jupyter Notebook？

### 第四步：询问权限模式

询问用户希望使用哪种权限模式：

- **A. 完全免确认（bypassPermissions）**：所有已允许的操作不再弹出确认提示
- **B. 普通模式（default）**：未在允许列表中的操作仍会弹出确认提示

### 第五步：生成配置并写入文件

根据用户的回答，构造 JSON 配置。对照下表将用户的选择映射为权限规则：

| 用户选择 | 对应的权限规则 |
|---------|--------------|
| Shell 命令 = 是 | `"Bash(*)"` |
| 文件读取 = 是 | `"Read(*)"`, `"Glob(*)"`, `"Grep(*)"` |
| 文件写入 = 是 | `"Write(*)"`, `"Edit(*)"` |
| 网络访问 = 是 | `"WebFetch(*)"`, `"WebSearch(*)"` |
| 子代理 = 是 | `"Agent(*)"` |
| Notebook = 是 | `"NotebookEdit(*)"` |

将所有用户选"是"的规则合并到 `allow` 数组中，再加上 `"Skill(*)"` 和 `"Task(*)"` 这两项（这两项始终包含，不需要询问）。

权限模式对应：
- 用户选 A → `"defaultMode": "bypassPermissions"`
- 用户选 B → `"defaultMode": "default"`

最终 JSON 格式如下：

```json
{
  "permissions": {
    "allow": [
      这里放用户选择的权限规则
    ],
    "defaultMode": "这里放用户选择的模式"
  }
}
```

将这个 JSON 写入 `.claude/settings.local.json`。

### 第六步：确认

读取 `.claude/settings.local.json`，将最终配置展示给用户确认。告诉用户哪些权限已开启、哪些未开启、当前使用的权限模式。

---

**以上就是全部操作。请现在开始执行。**
