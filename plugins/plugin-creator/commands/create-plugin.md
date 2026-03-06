---
description: 创建 Claude Code 插件向导 - 引导生成 agents、commands、skills、hooks 和完整插件结构
---

# Create Plugin Command

启动插件创建向导，帮助用户快速生成 Claude Code 插件。

## 功能

- 🆕 创建新插件骨架
- 🤖 添加 Agent（自主执行任务的 AI 助手）
- 📝 添加 Command（斜杠命令）
- 📖 添加 Skill（知识库和最佳实践）
- 🪝 添加 Hook（事件钩子）
- ✅ 验证插件配置

## 用法

```
/create-plugin              # 创建新插件（交互式向导）
/create-plugin --add-agent  # 添加 Agent
/create-plugin --add-command # 添加 Command
/create-plugin --add-skill  # 添加 Skill
/create-plugin --add-hook   # 添加 Hook
/create-plugin --validate   # 验证当前插件
```

## 组件类型说明

### 🤖 Agent

**用途：** 自主执行复杂多步骤任务

**适合场景：**
- 自动化工作流（部署、测试、发布）
- 代码审查和分析
- 文档生成
- 需要读取/修改文件或执行命令的任务

**示例：** 部署 Agent、代码审查 Agent、测试运行 Agent

---

### 📝 Command

**用途：** 提供用户可调用的斜杠命令

**适合场景：**
- 触发 Agent 执行任务
- 提供快速入口点
- 显示使用说明

**示例：** `/deploy`、`/review`、`/test`

---

### 📖 Skill

**用途：** 提供领域知识和最佳实践

**适合场景：**
- 编码规范和风格指南
- API 设计模式
- 安全最佳实践
- 框架特定知识

**示例：** API 设计规范、Python 模式、安全审查清单

---

### 🪝 Hook

**用途：** 在特定事件时自动执行脚本

**适合场景：**
- 工具调用前后验证
- 日志记录
- 自动通知
- 会话状态管理

**示例：** 记录工具调用、验证 Bash 命令、自动提交

---

## 示例交互

```
用户: /create-plugin

Agent:
🚀 Claude Code 插件创建向导

请选择你要创建的组件类型：

1. 🤖 Agent - 自主执行复杂任务的 AI 助手
   适合：自动化工作流、代码审查、测试运行

2. 📝 Command - 用户通过 /命令名 触发的快捷方式
   适合：提供简单的入口点、触发 Agent

3. 📖 Skill - 知识库和最佳实践集合
   适合：提供领域知识、编码规范、设计模式

4. 🪝 Hook - 在特定事件时自动执行的脚本
   适合：验证输入、记录日志、自动化处理

5. 📦 完整插件 - 创建包含多种组件的新插件

请输入选项编号（1-5）或描述你想实现的功能：
```

## 快速选择指南

| 你想实现... | 选择 |
|------------|------|
| 自动执行一系列操作 | Agent |
| 提供简单的命令入口 | Command |
| 提供知识/规范/模式 | Skill |
| 在事件时自动处理 | Hook |
| 完整的插件包 | 完整插件 |

---

## ⚠️ 多插件仓库注意事项

如果你计划在一个仓库中管理多个插件：

1. **目录结构**：使用 `plugins/` 目录存放所有子插件
2. **marketplace.json**：放在根目录 `.claude-plugin/` 下
3. **子插件**：只需要 `plugin.json`，不需要 `marketplace.json`
4. **source 路径**：必须包含 `plugins/` 前缀，如 `./plugins/plugin-name`

详细说明请参考 Agent 的多插件结构文档。