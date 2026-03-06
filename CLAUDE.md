# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Project Overview

自定义 Claude Code 扩展包，包含部署工作流 Agent。

## Git Workflow

用户输入 `work-done` 后，按照顺序执行：
1. **更新项目文档** - 更新 CLAUDE.md README.md
2. **自动提交** - 整理变更，生成 commit message 并提交
3. **推送确认** - 推送前**必须**询问用户确认
4. **推送执行** - 用户确认后执行 `git push`
5. **部署确认** - 部署前**必须**询问用户确认
6. **部署执行** - 用户确认后执行部署脚本

## Deployment Configuration

请根据实际项目修改以下配置：

```bash
# 服务器信息
DEPLOY_SERVER=root@your-server.com
DEPLOY_PATH=/path/to/project

# 部署方式: docker-compose | pm2 | rsync
DEPLOY_METHOD=docker-compose

# 服务名称
DEPLOY_SERVICE_NAME=your-service
```