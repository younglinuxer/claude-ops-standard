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

## 打包与版本流转

- 后端打包命令：
- 前端打包命令：
- jar 包输出位置：
- 镜像仓库地址：
- 默认交付方式：Docker 镜像 / jar 包
- 开发 -> 测试 -> 生产流转方式：
- 当前版本记录位置：
- 回滚版本记录位置：

## 配置来源

- 打包配置来源：
- Nacos namespace：
- Nacos 配置 Data ID：
- 前端 API/WS 配置：
- 环境变量来源：
- 开发/测试环境密码、Token、私钥登记位置：本文件 / 私有适配清单
- 生产敏感信息：不得写入本文件

## 开发/测试环境配置和密钥

> 只记录开发/测试环境。生产环境账号、密码、Token、私钥不得写入本文件。

| 环境 | 类型 | 地址/路径 | 用户名/标识 | 密码/Token/私钥位置或值 | 备注 |
|------|------|----------|-------------|------------------------|------|
| dev | MySQL |  |  |  |  |
| test | MySQL |  |  |  |  |

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

- 不得新增生产环境明文密码、Token、私钥。
- 开发/测试环境配置、密码、Token、私钥必须登记在本文件或项目明确登记的私有适配清单中。
- 不得跳过 `claude-ops-standard/standards/safety.md`。
- 高风险操作必须命令级确认。
- 不得用 `docker run` 或手工 Docker 命令管理长期服务。
- 不得把同一环境拆成多个 Compose 文件。
- 服务器重大变更不得跳过 `/data/logs` 记录。
