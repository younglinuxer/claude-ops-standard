# SOP：服务重启

## 步骤

1. 读取项目适配清单，确认目标环境和服务名。
2. 查看当前状态和最近日志。
3. 输出命令级计划，说明影响范围和回滚方式。
4. 用户确认后执行单服务重启。
5. 查看状态、日志、健康检查。

## 推荐命令

```bash
docker compose ps service_name
docker compose logs --tail=200 service_name
docker compose restart service_name
docker compose logs -f --tail=200 service_name
```

## 注意事项

- 不得用 `down -v` 重启服务。
- 不得一次性重启所有服务，除非计划中说明顺序和影响范围。
- 网关、注册中心、数据库、消息队列重启前必须额外确认影响范围。
