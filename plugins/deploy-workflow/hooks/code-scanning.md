---
name: code-scanning
event: PreToolCall
tools: ["Bash"]
---

# Code Scanning Hook

在 git push 之前触发代码扫描，扫描通过后才允许推送。

## 触发条件

- 当执行 `git push` 命令时
- 由 `/workdone-deploy` 命令触发的推送流程

## 工作流程

```
git push 触发
      ↓
Hook 拦截 push 命令
      ↓
┌─────────────────────────────────────┐
│  检测已安装的代码扫描插件           │
│  ├─ 找到 → 执行扫描，等待完成       │
│  └─ 未找到 → 跳过，继续 push        │
└─────────────────────────────────────┘
      ↓
扫描通过 → 允许 push 执行
扫描失败 → 显示问题，询问用户
```

## 脚本

```bash
#!/bin/bash

# 读取 stdin 中的 JSON 输入
input=$(cat)

# 检查是否是 git commit 命令
command=$(echo "$input" | jq -r '.parameters.command // ""')

if echo "$command" | grep -qE "git\s+push"; then
    echo "" >&2
    echo "╔══════════════════════════════════════════════════╗" >&2
    echo "║        🔍 代码扫描 Hook 已触发                    ║" >&2
    echo "║        正在检测代码扫描插件...                    ║" >&2
    echo "╚══════════════════════════════════════════════════╝" >&2
    echo "" >&2

    # 设置扫描标记
    # Agent 将自动搜索已安装的代码扫描插件
    echo "$input" | jq '. + {
        "hooks": {
            "code_scan_required": true,
            "scan_type": "auto-detect",
            "scan_message": "请在 push 前执行代码扫描"
        }
    }'
else
    # 非 push 命令，直接传递
    echo "$input"
fi
```

## 输入格式

PreToolCall 事件会传入以下 JSON 结构：

```json
{
  "tool": "Bash",
  "parameters": {
    "command": "git commit -m '...'"
  }
}
```

## 输出格式

检测到 `git commit` 时，输出添加扫描请求标记：

```json
{
  "tool": "Bash",
  "parameters": {
    "command": "git commit -m '...'"
  },
  "hooks": {
    "code_scan_required": true,
    "scan_type": "auto-detect",
    "scan_message": "请在 commit 前执行代码扫描"
  }
}
```

## 插件自动检测

Agent 收到扫描请求后，按以下顺序检测已安装的代码扫描插件：

1. **检测方式**：搜索已注册的 Agent 中具有代码审查/扫描功能的
2. **优先级**（按名称匹配）：
   - `comprehensive-review:code-reviewer`
   - `everything-claude-code:code-reviewer`
   - 其他包含 `review`、`scan`、`audit` 关键词的 Agent
3. **未找到**：输出提示信息，跳过扫描，继续 commit

## Agent 集成流程

`deploy-workflow` Agent 收到 `code_scan_required` 标记后：

```markdown
## 代码扫描步骤

1. **检测扫描插件**
   - 查找已安装的代码审查类 Agent
   - 匹配关键词：review, scan, audit, linter

2. **执行扫描**（如果找到插件）
   - 调用对应的 Agent 执行代码扫描
   - 阻塞等待扫描完成
   - 显示扫描结果

3. **处理结果**
   - 扫描通过 → 继续执行 commit
   - 扫描发现问题 → 列出问题，询问用户操作

4. **未找到插件**
   - 输出："未检测到代码扫描插件，跳过扫描"
   - 继续执行 commit
```

## 支持的代码扫描插件

以下插件类型会被自动检测：

| 插件类型 | 匹配关键词 | 示例 |
|---------|-----------|------|
| 综合审查 | `comprehensive-review` | `comprehensive-review:code-reviewer` |
| 代码审查 | `code-reviewer` | `everything-claude-code:code-reviewer` |
| 安全扫描 | `security-review`, `security-scan` | `everything-claude-code:security-reviewer` |
| 性能检查 | `performance` | `full-stack-orchestration:performance-engineer` |

## 日志与反馈

```
╔══════════════════════════════════════════════════╗
║        🔍 代码扫描 Hook 已触发                    ║
║        正在检测代码扫描插件...                    ║
╚══════════════════════════════════════════════════╝

[检测结果]
✓ 找到插件: comprehensive-review:code-reviewer
→ 正在执行扫描...

[扫描结果]
✓ 代码扫描通过
→ 继续执行 commit
```

或未找到插件时：

```
╔══════════════════════════════════════════════════╗
║        🔍 代码扫描 Hook 已触发                    ║
║        正在检测代码扫描插件...                    ║
╚══════════════════════════════════════════════════╝

[检测结果]
⚠ 未检测到代码扫描插件
→ 跳过扫描，继续 commit
```

## 注意事项

1. Hook 设置标记，Agent 负责实际检测和执行
2. 扫描是阻塞的，必须等待完成
3. 未找到扫描插件时自动跳过，不阻止 commit
4. 用户可手动选择忽略扫描结果继续提交

## 相关文档

- [Claude Code Hooks 文档](https://docs.anthropic.com/claude-code/hooks)