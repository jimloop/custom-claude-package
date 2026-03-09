---
name: workdone-deploy
description: 执行完整的部署工作流 - 更新文档、自动提交、推送确认、推送执行、部署确认、自动部署到云服务器
---

# Workdone Deploy

执行完整的 Git 工作流和部署流程。

## When to Activate

- 用户输入 `/workdone-deploy` 或 `work-done`
- 完成开发工作后需要提交和部署
- 需要自动化的 Git 工作流

## 执行流程

按顺序执行以下步骤：

1. **更新项目文档** - 分析变更，更新 CLAUDE.md 和 README.md
2. **自动提交** - 生成 Conventional Commits 格式的提交信息并提交
3. **推送确认** - 询问用户是否推送到远程仓库
4. **推送执行** - 用户确认后执行 git push
5. **部署确认** - 询问用户是否部署到云服务器
6. **分支选择** - 让用户选择要部署的分支
7. **自动部署** - 用户确认后执行部署脚本

## 前提条件

在项目的 `CLAUDE.md` 中配置部署信息：

```markdown
## Deployment Configuration

DEPLOY_SERVER=user@your-server.com
DEPLOY_PATH=/path/to/project
DEPLOY_METHOD=docker-compose
DEPLOY_SERVICE_NAME=your-service
```

## 安全机制

- 推送前必须用户确认
- 部署前必须用户确认
- 部署后自动验证服务状态
- 支持一键回滚