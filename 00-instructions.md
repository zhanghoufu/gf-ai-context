# AI Instructions for GoFrame (GF)

## File Priority

1. `workflows.md` - Standard task patterns
2. [gf-skills](https://github.com/zhanghoufu/gf-skills/blob/main/SKILL.md) - Comprehensive knowledge base

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

### Never Do
- Put business logic in controllers or DAOs
- Manually create service interfaces (use `gf gen service`)
- Hard-code database column names (use `dao.Table.Columns()`)
- Use `errors.New()` instead of `gerror.New()` or `gerror.Wrap()`
- Skip context propagation
- Use `fmt.Errorf` instead of `gerror.Wrapf()`
