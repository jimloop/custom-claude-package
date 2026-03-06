# custom-claude-package

自定义 Claude Code 扩展包，包含部署工作流 Agent。

## 安装

将 `.claude/` 目录复制到你的项目根目录即可使用。

## 包含的 Agent

### deploy-workflow

完整的部署工作流代理：
- 更新文档 → 自动提交 → 推送确认 → 推送执行 → 部署到云服务器

## 使用方法

在对话中输入：
- `work-done` - 执行完整工作流
- `deploy` - 仅执行部署流程

## 配置

在 `CLAUDE.md` 中添加部署配置：

```markdown
## Deployment Configuration

DEPLOY_SERVER=root@your-server.com
DEPLOY_PATH=/path/to/project
DEPLOY_METHOD=docker-compose
DEPLOY_SERVICE_NAME=your-service
```

## 前提条件

1. 本地已配置 SSH 密钥免密登录服务器
2. 服务器已安装 Git 并关联远程仓库
3. 服务器已配置 Docker 或 PM2