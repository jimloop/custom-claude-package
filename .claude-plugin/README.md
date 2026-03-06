# Custom Claude Package Plugin

这是一个 Claude Code 插件，包含部署工作流 Agent 和 Command。

## 安装

```bash
# 方式一：通过 Claude Code 安装
# 在 Claude Code 中输入：
/install-plugin https://github.com/jimloop/custom-claude-package

# 方式二：手动安装
git clone https://github.com/jimloop/custom-claude-package.git
cp -r custom-claude-package/.claude ~/.claude/plugins/cache/custom-claude-package/
```

## 包含的组件

### Agents
- `deploy-workflow` - 完整的部署工作流代理

### Commands
- `workdone-deploy` - 快速触发部署工作流的命令

## 使用方法

1. 安装插件后，在你的项目 `CLAUDE.md` 中配置部署信息：

```markdown
## Deployment Configuration

DEPLOY_SERVER=user@your-server.com
DEPLOY_PATH=/path/to/project
DEPLOY_METHOD=docker-compose
DEPLOY_SERVICE_NAME=your-service
```

2. 在对话中输入 `work-done` 触发工作流

## 前提条件

- 本地已配置 SSH 密钥免密登录服务器
- 服务器已安装 Git 并关联远程仓库
- 服务器已配置 Docker 或 PM2