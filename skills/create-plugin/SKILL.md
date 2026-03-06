---
name: create-plugin
description: Claude Code 插件创建向导 - 帮助生成 agents、commands、skills、hooks 和完整插件结构
---

# Create Plugin Skill

帮助用户快速创建 Claude Code 插件及其组件。

## When to Activate

- 用户输入 `/create-plugin`
- 用户想要创建新的 Claude Code 插件
- 用户想要添加 agent、command、skill、hook 组件
- 用户询问如何创建插件
- 用户需要验证插件配置

---

## 组件类型快速参考

### Agent vs Command vs Skill vs Hook

| 类型 | 用途 | 触发方式 | 是否执行任务 |
|------|------|---------|-------------|
| **Agent** | 自动执行复杂任务 | 被 Command 或用户调用 | ✅ 是 |
| **Command** | 提供快捷入口 | 用户输入 `/命令名` | ❌ 只触发 Agent |
| **Skill** | 提供知识库 | 自动激活或手动调用 | ❌ 只提供信息 |
| **Hook** | 自动化处理 | 事件触发 | ✅ 是（脚本） |

### 选择建议

```
问题：你想实现什么功能？

┌─────────────────────────────────────────────────────────────┐
│ "我想要自动执行一系列操作"                                    │
│ → 选择 Agent                                                │
│   示例：自动部署、代码审查、运行测试                          │
├─────────────────────────────────────────────────────────────┤
│ "我想要一个简单的命令来触发功能"                              │
│ → 选择 Command                                              │
│   示例：/deploy、/review、/test                             │
├─────────────────────────────────────────────────────────────┤
│ "我想要提供领域知识和最佳实践"                                │
│ → 选择 Skill                                                │
│   示例：API 设计规范、安全最佳实践、编码风格                  │
├─────────────────────────────────────────────────────────────┤
│ "我想要在某些事件发生时自动执行操作"                          │
│ → 选择 Hook                                                 │
│   示例：记录日志、验证输入、自动提交                          │
└─────────────────────────────────────────────────────────────┘
```

---

## 插件结构模板

```
plugin-name/
├── .claude-plugin/
│   ├── plugin.json        # 必需：插件元数据
│   ├── marketplace.json   # 必需：市场配置
│   └── README.md          # 安装说明
├── agents/                # Agent 定义
│   └── agent-name.md
├── commands/              # Command 定义 (文件名即命令名)
│   └── command-name.md
├── skills/                # Skill 定义 (目录结构)
│   └── skill-name/
│       └── SKILL.md
├── hooks/                 # Hook 定义
│   └── hook-name.md
└── rules/                 # Rule 定义
    └── rule-name.md
```

---

## 文件格式详解

### Agent 格式

**文件位置：** `agents/agent-name.md`

```markdown
---
name: agent-name                    # 必需：小写、连字符分隔
description: Agent 的简短描述        # 必需：用于选择 Agent
tools: ["Read", "Write", "Edit", "Bash", "Grep", "Glob"]  # 可选
model: opus                         # 可选：opus | sonnet | haiku
---

# Agent 标题

你是 [角色描述]。

## 工作流程

### 步骤 1: [步骤名称]
[具体操作说明]

### 步骤 2: [步骤名称]
[具体操作说明]

## 输出格式

[期望的输出格式示例]
```

**可用工具：**
- `Read` - 读取文件
- `Write` - 创建文件
- `Edit` - 编辑文件
- `Bash` - 执行命令
- `Grep` - 搜索内容
- `Glob` - 查找文件
- `Agent` - 调用其他 Agent
- `WebFetch` - 获取网页
- `WebSearch` - 网络搜索
- `AskUserQuestion` - 询问用户

---

### Command 格式

**文件位置：** `commands/command-name.md`（文件名即命令名）

```markdown
---
description: 命令的简短描述（显示在命令列表中）
---

# 命令标题

[命令的详细说明]

## 用法

/命令名 [参数]

## 功能

- 功能 1
- 功能 2

## 示例

用户: /命令名
Agent: [执行示例]

## 相关 Agent

此命令会触发 `agent-name` Agent。
```

**重要提示：**
- ❌ 不要在 frontmatter 中添加 `name` 字段
- ✅ 只需要 `description` 字段
- ✅ 文件名决定命令名

---

### Skill 格式

**文件位置：** `skills/skill-name/SKILL.md`（必须是目录结构）

```markdown
---
name: skill-name                    # 必需
description: Skill 的简短描述        # 必需
---

# Skill 标题

[Skill 的详细内容]

## When to Activate

定义何时自动激活此 Skill：

- 用户询问关于 [主题] 的问题
- 代码中包含 [特定模式]
- 正在处理 [特定类型] 的任务

## 核心概念

[详细的知识内容]

## 最佳实践

[推荐的实践方法]

## 示例

[具体示例]
```

**重要提示：**
- ✅ 必须放在目录中：`skills/skill-name/SKILL.md`
- ✅ 文件名必须是 `SKILL.md`
- ✅ `When to Activate` 帮助 Claude 理解何时使用

---

### Hook 格式

**文件位置：** `hooks/hook-name.md`

```markdown
---
name: hook-name                     # 必需
event: PreToolCall                  # 必需：触发事件
tools: ["Bash", "Write"]            # 可选：监听特定工具
---

# Hook 标题

[Hook 的说明]

## 脚本

#!/bin/bash
# Hook 脚本内容
input=$(cat)
# 处理逻辑
echo "$input"
```

**可用事件：**
- `PreToolCall` - 工具调用前
- `PostToolCall` - 工具调用后
- `SessionStart` - 会话开始
- `SessionEnd` - 会话结束
- `Notification` - 通知事件
- `Stop` - 停止请求

---

## 配置文件模板

### plugin.json
```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "插件描述",
  "author": {
    "name": "作者名",
    "url": "https://github.com/author"
  },
  "license": "MIT",
  "keywords": ["claude-code", "plugin"]
}
```

### marketplace.json
```json
{
  "$schema": "https://anthropic.com/claude-code/marketplace.schema.json",
  "name": "plugin-name",
  "description": "插件描述",
  "owner": { "name": "作者名" },
  "plugins": [
    {
      "name": "plugin-name",
      "source": "./",
      "description": "插件描述",
      "version": "1.0.0",
      "author": { "name": "作者名" },
      "license": "MIT",
      "keywords": ["keyword1"],
      "category": "workflow",
      "strict": false
    }
  ]
}
```

---

## 验证清单

- [ ] plugin.json 格式正确
- [ ] marketplace.json 格式正确
- [ ] Agent 包含 name 和 description
- [ ] Command 只包含 description（不含 name）
- [ ] Skill 在正确目录结构中（skill-name/SKILL.md）
- [ ] Hook 包含 event 类型
- [ ] 所有文件 UTF-8 编码