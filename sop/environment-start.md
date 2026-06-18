# SOP：环境启动检查

## 适用范围

开发环境和测试环境的 Docker Compose 环境。

## 步骤

1. 读取项目适配清单，确认环境、Compose 文件位置和服务列表。
2. 进入 Compose 工作目录或使用 `docker compose -f`。
3. 执行 `docker compose ps` 查看当前状态。
4. 如需启动，先输出命令级计划并确认。
5. 执行 `docker compose up -d service_name` 或按项目顺序启动。
6. 查看 `docker compose ps`、服务日志和健康检查 URL。

## 验收

- 容器状态正常。
- 网关和核心服务健康检查正常。
- 日志无数据库连接、配置中心注册、端口占用错误。
