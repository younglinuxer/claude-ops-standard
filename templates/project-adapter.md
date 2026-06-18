# 项目运维适配清单模板

> 每个项目只维护本文件这类最小清单。通用规则放在 `claude-ops-standard`。

## 项目

- 项目名称：
- 仓库路径：
- 服务器项目目录：`/data/app/{project}`
- 默认 Compose 文件：`/data/app/{project}/compose.yml`
- 重大变更日志：`/data/logs/{project}-{env}-changes.md`
- 负责人：
- 适用环境：开发、测试

## 服务清单

| 服务 | 类型 | 端口 | 健康检查 | 备注 |
|------|------|------|----------|------|
| gateway | gateway |  |  |  |

## 环境入口

| 环境 | 网关入口 | 前端入口 | Compose 文件 | 变更日志 | 负责人 |
|------|----------|----------|--------------|----------|--------|
| dev |  |  | `/data/app/{project}/compose.yml` | `/data/logs/{project}-dev-changes.md` |  |
| test |  |  | `/data/app/{project}/compose.yml` | `/data/logs/{project}-test-changes.md` |  |

## Docker Compose 规则

- 所有 Docker 运行必须通过 Compose 编排。
- 同一环境的前端、后端、中间件、数据库、对象存储等必须在一个 Compose 文件中。
- 新增容器必须修改项目登记的 Compose 文件，不得执行 `docker run`。
- Compose 写法保持简单明了，避免复杂 anchors、`extends`、`profiles` 和动态脚本拼装。

## 配置来源

- Nacos namespace：
- Nacos 配置 Data ID：
- 前端 API/WS 配置：
- 环境变量来源：
- 敏感信息来源：

## 数据库

- 数据库名：
- 连接信息来源：
- 迁移脚本目录：
- 备份路径规范：

## 日志和排查

- Compose 日志命令：
- 宿主机日志路径：
- 重大变更日志路径：
- 常用健康检查：
- 常见问题：

## 禁止事项

- 不得新增明文密码、Token、私钥。
- 不得跳过 `claude-ops-standard/standards/safety.md`。
- 高风险操作必须命令级确认。
- 不得用 `docker run` 或手工 Docker 命令管理长期服务。
- 不得把同一环境拆成多个 Compose 文件。
- 服务器重大变更不得跳过 `/data/logs` 记录。
