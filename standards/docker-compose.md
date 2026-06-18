# Docker Compose 环境标准

开发环境和测试环境优先使用 Docker Compose 管理。业务项目可以逐步迁移，迁移前必须在项目适配清单登记真实 Compose 文件位置。

## 目录标准

推荐结构：

```text
envs/
  dev/
    compose.yml
    .env.example
    README.md
  test/
    compose.yml
    .env.example
    README.md
```

已经分散在服务器或其他仓库的 Compose 文件可以暂时保留，但项目适配清单必须记录：

- 环境名称。
- Compose 文件绝对路径或仓库地址。
- 执行用户和工作目录。
- 管理的服务列表。
- 数据卷和持久化目录。
- 负责人。

## 服务命名

- 容器名使用 `{project}-{env}-{service}`，例如 `v2x-dev-mysql`。
- 服务名使用稳定业务名，例如 `gateway`、`app`、`base`、`mysql`。
- 不同环境不能复用同一个容器名、volume 名或网络名。

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

默认禁止，除非完成强确认：

```bash
docker compose down -v
docker volume rm volume_name
docker system prune
```

## 健康检查

变更后必须验证：

- `docker compose ps` 中目标服务为 running 或 healthy。
- 服务端口监听正常。
- 项目适配清单中的健康检查 URL 正常返回。
- 关键日志没有启动失败、数据库连接失败、Nacos 注册失败。

## 回滚要求

- 发布前记录当前镜像 tag、jar 版本或 Compose 文件 Git commit。
- 修改 Compose 或 `.env` 前先查看 Git diff 或备份原文件。
- 回滚方式必须能恢复到上一个镜像、jar、配置或 Git commit。
