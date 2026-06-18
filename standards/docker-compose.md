# Docker Compose 环境标准

开发环境和测试环境的所有 Docker 运行必须通过 Docker Compose 编排。不得用 `docker run` 临时启动长期服务，也不得用 `docker start`、`docker restart`、`docker rm` 等命令手工管理运行服务。

## 目录标准

每个项目在服务器上使用固定目录：

```text
/data/app/{project}/
  compose.yml
  .env.example
  README.md
```

默认 Compose 文件为 `/data/app/{project}/compose.yml`。如果开发环境和测试环境临时部署在同一台服务器，可以使用同一项目目录下的 `compose.dev.yml`、`compose.test.yml`，但每个环境仍必须由一个 Compose 文件完整管理。

真实密钥不得写入 Git 仓库或 Markdown 文档。`.env.example` 只保留变量名和说明；真实值必须来自服务器受限权限文件、Nacos、企业密码库或负责人指定私有渠道。

## 单 Compose 要求

同一环境的所有运行程序必须放在同一个 Compose 文件中，包括：

- 前端服务。
- 后端业务服务。
- MySQL、Redis、Nacos、MQ、对象存储、任务调度等中间件。
- 项目运行所需的其他长期容器。

不得按业务服务、中间件、前端拆成多个 Compose 文件。新增容器时，必须修改项目登记的 Compose 文件，而不是执行 `docker run`。

## Compose 写法

Compose 文件必须简单明了、易于维护：

- 服务必须显式列出，服务名使用稳定业务名，例如 `gateway`、`base`、`app`、`mysql`。
- 容器名使用 `{project}-{env}-{service}`，例如 `v2x-dev-mysql`。
- 不同环境不能复用同一个容器名、volume 名或网络名。
- 避免复杂 anchors、`extends`、`profiles` 和动态脚本拼装。
- 数据卷、端口、环境变量来源必须能从 Compose 文件或项目适配清单直接追踪。

## 常用命令

所有命令必须在 Compose 文件所在目录执行，或显式使用 `-f`。

```bash
docker compose ps
docker compose logs --tail=200 service_name
docker compose logs -f --tail=200 service_name
docker compose up -d service_name
docker compose restart service_name
docker compose stop service_name
docker compose pull service_name
```

需要命令级确认：

```bash
docker compose up -d
docker compose restart service_name
docker compose stop service_name
docker compose down
```

默认禁止，除非完成强确认并记录重大变更日志：

```bash
docker compose down -v
docker volume rm volume_name
docker system prune
```

## 重大变更记录

以下 Docker/服务器变更必须记录到 `/data/logs/{project}-{env}-changes.md`：

- 修改 Compose 文件、`.env`、启动脚本或数据卷路径。
- 新增、删除、升级、回滚镜像或 jar 包。
- 修改端口、防火墙、系统服务或 Docker 配置。
- 调整数据库、中间件、对象存储等持久化目录。
- 清理容器、镜像、volume 或宿主机数据。

记录字段必须包含：时间、操作人、环境、主机、变更原因、执行命令、影响范围、备份位置、回滚方式、验证结果。

## 健康检查

变更后必须验证：

- `docker compose ps` 中目标服务为 running 或 healthy。
- 服务端口监听正常。
- 项目适配清单中的健康检查 URL 正常返回。
- 关键日志没有启动失败、数据库连接失败、Nacos 注册失败。
- 重大变更已经追加到 `/data/logs/{project}-{env}-changes.md`。

## 回滚要求

- 发布前记录当前镜像 tag、jar 版本或 Compose 文件 Git commit。
- 修改 Compose 或 `.env` 前先查看 Git diff 或备份原文件。
- 回滚方式必须能恢复到上一个镜像、jar、配置或 Git commit。
