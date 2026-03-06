# Deploy Workflow

Claude Code 部署工作流插件，支持自动提交、推送、部署到云服务器。

## 安装

```
/plugin add /path/to/deploy-workflow
/reload-plugins
```

## 使用

```
/workdone-deploy
```

## 功能

- 更新项目文档
- 自动 Git 提交
- 推送确认
- 远程部署

## 配置

在项目的 `CLAUDE.md` 中添加：

```markdown
## Deployment Configuration

DEPLOY_SERVER=user@your-server.com
DEPLOY_PATH=/path/to/project
DEPLOY_METHOD=docker-compose
DEPLOY_SERVICE_NAME=your-service
```