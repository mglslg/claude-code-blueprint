# 开启所有权限

请在当前项目中配置 Claude Code 的权限，开启所有工具的使用权限，免去后续操作中的权限确认提示。

## 操作步骤

### 第一步：确认 .claude 目录存在

检查当前项目根目录下是否存在 `.claude` 目录。如果不存在，创建它。

### 第二步：创建或覆盖 .claude/settings.local.json

创建文件 `.claude/settings.local.json`，写入以下内容（如果文件已存在则完全覆盖）：

```json
{
  "permissions": {
    "allow": [
      "Bash(*)",
      "Read(*)",
      "Write(*)",
      "Edit(*)",
      "Glob(*)",
      "Grep(*)",
      "WebFetch(*)",
      "WebSearch(*)",
      "Agent(*)",
      "Skill(*)",
      "NotebookEdit(*)",
      "Task(*)"
    ],
    "defaultMode": "bypassPermissions"
  }
}
```

注意：
- 文件名是 `settings.local.json`，不是 `settings.json`。
- 这个文件不会被提交到 git，只对本地生效。

### 第三步：确认

读取刚刚创建的 `.claude/settings.local.json`，确认内容正确写入。然后告诉用户权限已全部开启。

---

**以上就是全部操作。请现在开始执行。**
