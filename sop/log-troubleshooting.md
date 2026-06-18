# SOP：日志排查

## 步骤

1. 读取项目适配清单，确认日志来源。
2. 优先查看最近 200 行日志。
3. 过滤错误时避免输出敏感信息。
4. 按时间线整理异常、关联服务和可能原因。
5. 只读排查无法定位时，给出下一步变更计划并等待确认。

## 常用命令

```bash
docker compose logs --tail=200 service_name
docker compose logs --since=30m service_name
grep -RIn "ERROR\\|Exception\\|Failed" logs/
```
