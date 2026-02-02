# 工具说明

## MCP 工具 (gf-mcp)

当 AI 通过 MCP 连接 gf-mcp 时，可使用以下工具执行 GoFrame 开发任务。

| 工具 | 描述 | 典型参数 |
|------|------|----------|
| `gf_init` | 初始化新的 GoFrame 项目 | `project_name`, `monorepo`(可选) |
| `gf_gen_dao` | 从数据库生成 DAO/DO/Entity 层 | 依赖 hack/config.yaml 中 dao 配置 |
| `gf_gen_service` | 从 internal/logic 生成 service 接口 | 无 |
| `gf_gen_ctrl` | 从 api 定义生成 controller 存根 | 无 |
| `gf_gen_pb` | 从 .proto 生成 Go 与 gRPC 代码 | 无 |
| `gf_gen_pbentity` | 从数据库生成 protobuf 实体 | 无 |
| `gf_gen_enums` | 生成枚举代码 | 无 |
| `gf_run` | 以热重载模式运行项目 | 可选 main 路径 |

**使用顺序建议**：新项目 → `gf_init`；新表/改表 → `gf_gen_dao`；新 API → 写 api → `gf_gen_ctrl` → 写 logic → `gf_gen_service`；改 logic → `gf_gen_service`。

## gf-cli 命令（无 MCP 时）

在项目根目录手动执行以下命令，与 MCP 工具一一对应。

| 任务 | 命令 | 说明 |
|------|------|------|
| 新建项目 | `gf init <name>` | 单项目 |
| 新建 Monorepo | `gf init <name> -m` | 多服务共依赖 |
| 生成 DAO 层 | `gf gen dao` | 需在 hack/config.yaml 配置数据库与表 |
| 生成 Service 接口 | `gf gen service` | 根据 internal/logic 生成 internal/service |
| 生成 Controller | `gf gen ctrl` | 根据 api/*/v1/*.go 的 g.Meta 生成 controller |
| 生成 Protobuf | `gf gen pb` | 根据 .proto 生成 _grpc.pb.go 等 |
| 生成 PB 实体 | `gf gen pbentity` | 从数据库生成 proto 实体 |
| 生成枚举 | `gf gen enums` | 枚举代码生成 |
| 热重载运行 | `gf run main.go` | 开发时使用 |
| 生产构建 | `gf build` | 生成可执行文件 |
| Docker 构建 | `gf docker` | 生成 Dockerfile 并构建镜像 |

## 工具序列（推荐流程）

| 场景 | 步骤 |
|------|------|
| **新建 API 服务** | 定义 api/module/v1/*.go → `gf gen ctrl` → 实现 internal/logic → `gf gen service` → 在 internal/cmd 中绑定路由 → 编写 README.md |
| **新建实体 CRUD** | 建表 → 配置 hack/config.yaml → `gf gen dao` → 定义 api 的 Req/Res → `gf gen ctrl` → 实现 logic 的 Create/Update/Delete/GetOne/GetList → `gf gen service` |
| **仅改数据库** | 改表结构 → `gf gen dao` |
| **仅改 API 定义** | 改 api/*/v1/*.go → `gf gen ctrl` → 按需改 logic |
| **仅改业务逻辑** | 改 internal/logic/*.go → `gf gen service` |
| **新建 gRPC 服务** | 写 .proto → `gf gen pb` → 实现 controller 方法 → 配置注册与发现 |
| **部署** | `gf build` 或 `gf docker`，配合 manifest/deploy |

## 与 ai-context 的对应关系

- **有 MCP**：优先调用 gf-mcp 的 `gf_*` 工具，减少人工执行命令。
- **无 MCP**：按本表与 workflows.md 的步骤，在终端依次执行 `gf` 命令，并保持「先 API/DAO，再 gen，再实现 logic」的顺序。
