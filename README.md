# Claude Code Blueprint

Claude Code 引导程序（Bootstrap）集合 —— 用一段提示词驱动 Claude Code 自动构建自身的工作环境。

## 核心思路

Claude Code 的 CLAUDE.md 是可以被**引导生成**的。只需把蓝图内容复制粘贴到 Claude Code 的对话框里，CC 就会按照指令自动创建 CLAUDE.md、记忆系统、配置文件、甚至整个项目骨架。

本仓库本身不参与任何项目的运行，它只是一个**蓝图集合**，记录和管理各种引导程序，需要时去对应目录里复制就行。

## 目录结构

```
claude-code-blueprint/
├── bootstrap/       ← 引导程序：初始化 Claude Code 自身的工作环境
├── permission/      ← 权限蓝图：引导 Claude Code 开放或关闭特定配置
└── project/         ← 项目蓝图：引导 Claude Code 创建项目骨架和核心逻辑
```

## 蓝图列表

### Bootstrap（引导程序）

| 蓝图 | 说明 |
|------|------|
| `bootstrap/project_memory.md` | 在当前项目下构建完整的持久化记忆系统 |
| `bootstrap/global_memory.md` | 在 `~/.claude/` 下构建全局记忆系统，跨项目共享 |
| `bootstrap/quick_todo.md` | 轻量级杂事助手，随口说就记，按天分组，7 天自动清理 |

### Permission（权限配置）

| 蓝图 | 说明 |
|------|------|
| `permission/open_all.md` | 一键开启所有工具权限，免去确认提示 |
| `permission/interactive_setup.md` | 交互式引导用户逐项选择需要开放的权限 |

### Project（项目蓝图）

暂无，待补充。

## 如何编写自己的蓝图

蓝图就是一个 Markdown 文件，里面写清楚你希望 Claude Code 执行的每一步操作。几个要点：

1. **步骤要明确** —— 用"第一步""第二步"这样的结构，告诉 CC 按顺序执行
2. **给出文件内容** —— 用代码块写清楚每个文件应该包含什么内容
3. **路径要具体** —— 明确告诉 CC 文件应该创建在哪里
4. **末尾加确认** —— 让 CC 执行完后回读确认，防止遗漏

## License

MIT
