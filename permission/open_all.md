# 开启所有权限

请在当前项目中配置 Claude Code 的权限，开启所有工具的使用权限，免去后续操作中的权限确认提示。

## 操作步骤

### 第一步：确认 .claude 目录存在

检查当前项目根目录下是否存在 `.claude` 目录。如果不存在，创建它。

### 第二步：创建或覆盖 .claude/settings.json

创建文件 `.claude/settings.json`，追加以下内容（只覆盖重复的配置项！不要覆盖原配置项！）：

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


### 第三步：确认

读取刚刚创建的 `.claude/settings.json`，确认内容正确写入。然后告诉用户权限已全部开启。

---

**以上就是全部操作。请现在开始执行。**
