---
name: code-scanning
event: PreToolCall
tools: ["Bash"]
---

# Code Scanning Hook

在代码提交前自动触发代码扫描，确保代码质量。

## 触发条件

- 当执行 `git commit` 相关命令时
- 由 `/workdone-deploy` 命令触发的提交流程

## 脚本

```bash
#!/bin/bash

# 读取 stdin 中的 JSON 输入
input=$(cat)

# 检查是否是 git commit 命令
command=$(echo "$input" | jq -r '.parameters.command // ""')

if echo "$command" | grep -qE "git\s+(commit|add|-c)"; then
    echo "=== 触发代码扫描 ===" >&2
    echo "检测到 git commit 操作，正在执行代码扫描..." >&2
    echo "" >&2

    # 调用 comprehensive-review agent 进行代码扫描
    # 注意：此处需要 Claude Code 在后续处理中调用 agent
    echo "$input" | jq '. + {"trigger_code_scan": true}'
else
    # 非提交命令，直接传递
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

- 如果是 git commit 命令：添加 `trigger_code_scan` 标记
- 其他命令：原样传递

## 代码扫描配置

使用的代码扫描工具：
- 路径：`C:\Users\14475\.claude\plugins\marketplaces\claude-code-workflows\plugins\comprehensive-review`
- 功能：全面代码审查，包括架构、安全性、性能等

## 注意事项

1. 此 Hook 仅标记需要扫描，实际扫描由 Agent 执行
2. 如果扫描发现问题，可以选择阻止提交或允许继续
3. 扫描结果会记录到日志中

## 相关文档

- [Claude Code Hooks 文档](https://docs.anthropic.com/claude-code/hooks)
- [comprehensive-review 插件](C:\Users\14475\.claude\plugins\marketplaces\claude-code-workflows\plugins\comprehensive-review)