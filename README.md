# Custom Claude Package

Claude Code 扩展包，包含两个独立插件。

**当前版本：1.2.0**

## 插件列表

### 1. Deploy Workflow

路径：`plugins/deploy-workflow`

部署工作流 Agent，支持：
- 自动 Git 提交
- 推送确认
- **推送前代码扫描**（支持选择扫描插件）
- 远程部署到云服务器

安装：
```
/plugin add /path/to/plugins/deploy-workflow
/reload-plugins
```

使用：
```
/workdone-deploy
```

### 2. Plugin Creator

路径：`plugins/plugin-creator`

插件创建向导，支持：
- 创建新插件骨架
- 添加 Agent、Command、Skill、Hook
- 验证插件配置
- **多插件仓库结构指导**（v1.1.0 新增）

安装：
```
/plugin add /path/to/plugins/plugin-creator
/reload-plugins
```

使用：
```
/create-plugin
```

## 组件类型说明

| 类型 | 用途 | 触发方式 |
|------|------|---------|
| Agent | 自主执行复杂任务 | 被 Command 调用 |
| Command | 斜杠命令入口 | 用户输入 `/命令名` |
| Skill | 知识库和最佳实践 | 自动激活 |
| Hook | 事件钩子脚本 | 事件触发 |

## 项目结构

```
custom-claude-package/
├── .claude-plugin/
│   └── marketplace.json      # 根目录市场配置（多插件必需）
├── plugins/
│   ├── deploy-workflow/      # 部署工作流插件
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json   # 子插件只需 plugin.json
│   │   ├── agents/
│   │   │   └── deploy-workflow.md
│   │   ├── commands/
│   │   │   └── workdone-deploy.md
│   │   └── skills/
│   │       └── workdone-deploy/SKILL.md
│   └── plugin-creator/       # 插件创建工具
│       ├── .claude-plugin/
│       │   └── plugin.json   # 子插件只需 plugin.json
│       ├── agents/
│       │   └── plugin-creator.md
│       ├── commands/
│       │   └── create-plugin.md
│       └── skills/
│           └── create-plugin/SKILL.md
└── README.md
```

> **注意**：多插件仓库中，子插件目录只需要 `plugin.json`，不需要 `marketplace.json`。

## 部署配置

使用 deploy-workflow 插件前，在项目的 `CLAUDE.md` 中添加：

```markdown
## Deployment Configuration

DEPLOY_SERVER=user@your-server.com
DEPLOY_PATH=/path/to/project
DEPLOY_METHOD=docker-compose
DEPLOY_SERVICE_NAME=your-service
```