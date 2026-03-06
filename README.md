# custom-claude-package

自定义 Claude Code 扩展包，包含部署工作流 Agent。

## 安装

将 `.claude/` 目录复制到你的项目根目录即可使用。

```bash
cp -r .claude/ /path/to/your/project/
```

## 包含的 Agent

### deploy-workflow

完整的部署工作流代理，支持：
- 更新文档 → 自动提交 → 推送确认 → 推送执行 → 部署到云服务器

## 使用方法

### 1. 配置部署信息

在你的项目 `CLAUDE.md` 中添加：

```markdown
## Deployment Configuration

DEPLOY_SERVER=user@your-server.com
DEPLOY_PATH=/path/to/project
DEPLOY_METHOD=docker-compose  # docker-compose | pm2 | rsync
DEPLOY_SERVICE_NAME=your-service
```

### 2. 触发工作流

在 Claude Code 对话中输入：
- `work-done` - 执行完整工作流
- `deploy` - 仅执行部署流程

## 支持的部署方式

| 方式 | 说明 |
|------|------|
| `docker-compose` | Git pull + Docker Compose 重新构建 |
| `pm2` | Git pull + npm install + PM2 重启 |
| `rsync` | rsync 同步文件 + PM2 重启 |

## 前提条件

1. 本地已配置 SSH 密钥免密登录服务器
2. 服务器已安装 Git 并关联远程仓库
3. 服务器已配置 Docker 或 PM2（根据部署方式）

## 安全说明

- 推送和部署前都会询问用户确认
- SSH 密码由用户在终端手动输入（或使用密钥免密登录）
- 不在对话中存储任何敏感信息