# Claude 运维标准

本仓库是开发环境和测试环境的通用运维标准。Claude 或开发人员处理部署、环境、Docker、数据库、Nacos、日志、服务重启、K8s 排查等任务时，必须先读取本仓库对应标准，再读取业务项目自己的适配清单。

Git 地址：`https://github.com/younglinuxer/claude-ops-standard.git`

## 使用流程

1. 识别任务类型。
2. 从 Git 仓库读取本标准，必要时先执行 `git clone https://github.com/younglinuxer/claude-ops-standard.git` 或在已有副本中执行 `git pull`。
3. 读取对应标准文件。
4. 读取业务项目适配清单，例如 `.claude/ops/project-adapter.md`。
5. 先输出命令级执行计划，包含目标环境、命令、影响范围、验证方式、回滚方式和重大变更日志位置。
6. 等待用户确认后再执行变更命令。

只读排查可以直接执行，但如果排查命令可能造成负载、泄露敏感信息或改变状态，也必须先确认。

## 标准索引

| 场景 | 必读文件 |
|------|----------|
| 任何运维操作 | `standards/safety.md` |
| Docker Compose 环境 | `standards/docker-compose.md` |
| 数据库操作 | `standards/database.md` |
| K8s 环境排查 | `standards/k8s-readonly.md` |
| 常见操作步骤 | `sop/` |
| 新项目接入 | `templates/project-adapter.md` |

## 基本原则

- 第一版只覆盖开发环境和测试环境，不覆盖生产变更。
- 开发人员可以自主管理环境，但高风险操作必须命令级确认。
- 开发/测试环境的环境配置、账号、密码、Token、私钥等识别信息，必须记录在业务项目 `.claude/ops/project-adapter.md` 或项目明确登记的私有适配清单中，方便 Claude 判断环境；生产信息不得写入本标准或普通项目文档。
- 所有 Docker 运行必须通过 Docker Compose 编排，不允许用 `docker run` 或手工 `docker start/restart/rm` 管理运行服务。
- 每个项目固定放在服务器目录 `/data/app/{project}`，默认 Compose 文件为 `/data/app/{project}/compose.yml`。
- 同一环境的前端、后端、中间件、数据库、对象存储等运行程序必须放在同一个 Compose 文件中，不拆多个 Compose。
- 服务器重大变更必须记录到 `/data/logs/{project}-{env}-changes.md`。
- 项目打包和运行配置必须可外部修改，不得写死在业务代码、不可覆盖的镜像层或固定脚本中。
- 开发 -> 测试 -> 生产的版本流转必须走 Docker 镜像，镜像地址必须以 `registry.cn-sccd1.ctyun.cn/cmat/` 开头；开发推送镜像，最终交给运维在目标环境替换版本。
- 现有 K8s 环境第一版只允许只读排查，变更必须升级给负责人确认。

## 给 Claude 的硬性要求

Claude 在处理运维任务时必须遵守：

- 不得只凭历史上下文或记忆回答。
- 不得跳过本仓库标准和业务项目适配清单。
- 不得在标准仓库、公开文档或生产环境文档中新增明文密码、Token、私钥、证书。
- 处理开发/测试环境时，必须优先从业务项目 `.claude/ops/project-adapter.md` 识别环境配置、账号、密码、Token、私钥等信息；如信息缺失，先要求补充，不得猜测。
- 涉及变更命令时，必须先给出命令级计划并等待确认。
- 涉及打包、发布或版本替换时，必须说明 Docker 镜像地址、目标环境、目标服务、配置来源和回滚镜像 tag。
- 涉及 Docker 运行时，必须使用 `docker compose` 命令和项目登记的 Compose 文件。
- 涉及数据库 DDL/DML 时，必须说明环境、库名、表名、SQL、影响范围、备份方式和回滚方式。
- 涉及服务器重大变更时，必须在计划中声明 `/data/logs` 下的日志文件路径，执行后追加记录。
- 涉及 K8s 写操作时，必须停止执行并要求负责人确认。
