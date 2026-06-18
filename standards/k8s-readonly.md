# K8s 只读排查标准

第一版不授权普通开发或 Claude 对 K8s 环境做变更。K8s 仅用于只读排查，写操作必须升级给负责人确认。

## 允许的只读命令

```bash
kubectl config current-context
kubectl get ns
kubectl get pods -n namespace
kubectl get svc -n namespace
kubectl get ingress -n namespace
kubectl describe pod pod_name -n namespace
kubectl logs pod_name -n namespace --tail=200
kubectl logs deployment/deploy_name -n namespace --tail=200
kubectl top pods -n namespace
kubectl top nodes
```

## 禁止直接执行的命令

```bash
kubectl apply
kubectl delete
kubectl edit
kubectl patch
kubectl scale
kubectl rollout restart
kubectl rollout undo
kubectl create
kubectl replace
kubectl cordon
kubectl drain
```

如果用户要求执行上述命令，Claude 必须停止，并输出命令级计划、影响范围、回滚方式，要求负责人确认。

## 排查流程

1. 确认 `current-context`、namespace、目标服务。
2. 查看 Pod、Service、Ingress 状态。
3. 查看事件和最近日志。
4. 给出只读结论和建议。
5. 如需变更，升级到负责人确认流程。
