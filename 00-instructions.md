# AI Instructions for GoFrame (GF)

## File Priority

1. `workflows.md` - 标准任务流程与决策矩阵
2. `tools.md` - MCP 工具与 gf-cli 命令、工具序列
3. `patterns.md` - 常用代码模式（API/Controller/Logic/Config/DAO）
4. [gf-skills](https://github.com/zhanghoufu/gf-skills/blob/main/SKILL.md) - 详细模式与最佳实践（需要时查阅）

## Quick Reference (gf-skills)

| Topic | Reference File | Use When |
|-------|----------------|----------|
| Core Components | [core-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/core-patterns.md) | gcfg, glog, gerror, gvalid, gconv, gcache, gcmd, gview, gi18n, gres |
| Components List | [components-list.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/components-list.md) | Full component reference |
| Web Development | [web-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/web-patterns.md) | HTTP server, routing, middleware |
| Database & ORM | [database-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/database-patterns.md) | DAO, transactions, queries |
| CLI Automation | [cli-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/cli-patterns.md) | gf-cli commands |
| Microservices | [micro-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/micro-patterns.md) | gRPC, service discovery |
| Deployment | [deployment.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/deployment.md) | Standalone, Docker, K8s |
| FAQ | [faq.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/faq.md) | Common issues |

## Core Rules

### CLI-First Approach
- ALWAYS prefer `gf-cli` tools for code generation
- NEVER manually write DAO, DO, or Entity layers
- AUTOMATICALLY run `gf gen service` after modifying logic
- USE `gf gen ctrl` to generate controller stubs from API definitions

### Project Modes
- **Single-repo**: `gf init <name>` - Standard single project
- **Monorepo**: `gf init <name> -m` - Multiple services sharing dependencies

### Architecture (Three-Tier)
```
api/          → Interface definitions (Req/Res structs)
controller/   → Presentation layer (HTTP handlers, NO business logic)
logic/        → Business logic layer (ALL business logic here)
service/      → Interfaces (auto-generated from logic)
dao/          → Data access layer (auto-generated from DB)
```

### Code Generation Commands
| Task | Command | Output |
|------|---------|--------|
| New project | `gf init <name>` | Project scaffold |
| DAO from DB | `gf gen dao` | `dao/`, `model/do/`, `model/entity/` |
| Service interfaces | `gf gen service` | `service/*.go` |
| Controller stubs | `gf gen ctrl` | `controller/*.go` |
| Protobuf | `gf gen pb` | `.pb.go`, `_grpc.pb.go` |
| Hot-reload dev | `gf run main.go` | Development server |
| Production build | `gf build` | Binary executable |

## Decision Tree

```
User Request →
├─ New Project? → gf init → Configure config.yaml
├─ New Table? → gf gen dao → Generate DAO/DO/Entity
├─ New API?
│   ├─ Define api/module/v1/xxx.go with g.Meta
│   ├─ gf gen ctrl → Generate controller stub
│   ├─ Implement internal/logic/xxx.go
│   └─ gf gen service → Generate service interface
├─ New gRPC Service?
│   ├─ Define .proto file
│   ├─ gf gen pb → Generate Go bindings + controller
│   └─ Implement controller logic
├─ Deployment?
│   ├─ Standalone → gf build + systemd/supervisor
│   ├─ Docker → gf docker or Dockerfile
│   └─ Kubernetes → ConfigMap + Deployment
└─ Code Change?
    ├─ Logic changed → gf gen service
    ├─ API changed → gf gen ctrl
    └─ DB schema changed → gf gen dao
```

## GoFrame Conventions

### Always Do
- **Context First**: All functions accept `ctx context.Context` as first parameter
- **Structured Input/Output**: Use structs for function parameters, not multiple args
- **Error Handling**: Use `gerror.Wrap()` for error wrapping with stack traces
- **Validation**: Use `v` tag in structs for automatic request validation
- **Configuration**: Use `g.Cfg()`, config in `manifest/config/config.yaml`
- **Logging**: Use `g.Log()` with context for structured logging

### Documentation
- 为新服务生成 README.md：服务概述、功能列表、API 端点说明、配置说明、运行与测试示例
- API 定义使用 `g.Meta` 的 `summary`/`dc` 便于自动生成 OpenAPI 文档

### Never Do (Avoid)
- 在 controller 或 dao 中写业务逻辑
- 手写 service 接口（必须用 `gf gen service`）
- 手写数据库列名字符串（使用 `dao.Table.Columns().Xxx`）
- 用 `errors.New()` / `fmt.Errorf` 代替 `gerror.New()` / `gerror.Wrap()`
- 忽略 context 传递（首参必须为 `ctx context.Context`）
- 跳过请求校验（API 请求体使用 `v` 标签）
- 只生成空壳逻辑（logic 层需实现完整业务）

## Detailed Patterns

完整实现模式请查阅 [gf-skills](https://github.com/zhanghoufu/gf-skills)：

- Web / API → [web-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/web-patterns.md)
- 核心组件与设计原则 → [core-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/core-patterns.md)
- 数据库与 ORM → [database-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/database-patterns.md)
- CLI 与代码生成 → [cli-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/cli-patterns.md)
- 微服务与 gRPC → [micro-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/micro-patterns.md)
- 部署 → [deployment.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/deployment.md)
- 常见问题 → [faq.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/faq.md)、[troubleshooting/common-issues.md](https://github.com/zhanghoufu/gf-skills/blob/main/troubleshooting/common-issues.md)
