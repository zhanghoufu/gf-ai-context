# GoFrame 代码模式

本文档提炼 gf-skills 中的常用代码模式，供 AI 快速参考。详细说明见 [gf-skills](https://github.com/zhanghoufu/gf-skills)。

## API 定义 (api/module/v1/)

### 基础请求/响应
```go
package v1

import "github.com/gogf/gf/v2/frame/g"

type UserGetReq struct {
    g.Meta `path:"/user/:id" tags:"User" method:"get" summary:"Get user by ID"`
    Id     uint `json:"id" v:"required|min:1#ID is required|ID must be positive" dc:"User ID"`
}

type UserGetRes struct {
    Id    uint   `json:"id" dc:"User ID"`
    Name  string `json:"name" dc:"User name"`
    Email string `json:"email" dc:"User email"`
}

type UserCreateReq struct {
    g.Meta   `path:"/user" tags:"User" method:"post" summary:"Create new user"`
    Name     string `json:"name" v:"required|length:2,50#Name is required|Name length 2-50" dc:"User name"`
    Email    string `json:"email" v:"required|email#Email is required|Invalid email" dc:"User email"`
    Password string `json:"password" v:"required|password#Password is required|Password too weak" dc:"Password"`
}

type UserCreateRes struct {
    Id uint `json:"id" dc:"Created user ID"`
}
```

### 常用标签
| 标签 | 用途 | 示例 |
|------|------|------|
| `g.Meta` | 路由与 OpenAPI 文档 | `path:"/user" method:"post"` |
| `path` | URL 路径（支持 `:id`） | `path:"/user/:id"` |
| `method` | HTTP 方法 | `method:"get"` / `method:"post"` |
| `tags` | OpenAPI 分组 | `tags:"User"` |
| `summary` / `sm` | 接口摘要 | `sm:"获取用户"` |
| `v` | 校验规则 | `v:"required\|email"` |
| `dc` | 字段说明（OpenAPI） | `dc:"User email"` |
| `json` | JSON 字段名 | `json:"user_id"` |

## Controller 层

Controller 仅做参数绑定与调用 Service，不写业务逻辑。

```go
package user

import (
    "context"
    "project/api/user/v1"
    "project/internal/service"
)

type ControllerV1 struct{}

func NewV1() *ControllerV1 {
    return &ControllerV1{}
}

func (c *ControllerV1) Get(ctx context.Context, req *v1.UserGetReq) (res *v1.UserGetRes, err error) {
    user, err := service.User().GetById(ctx, req.Id)
    if err != nil {
        return nil, err
    }
    return &v1.UserGetRes{
        Id:    user.Id,
        Name:  user.Name,
        Email: user.Email,
    }, nil
}

func (c *ControllerV1) Create(ctx context.Context, req *v1.UserCreateReq) (res *v1.UserCreateRes, err error) {
    id, err := service.User().Create(ctx, service.UserCreateInput{
        Name:     req.Name,
        Email:    req.Email,
        Password: req.Password,
    })
    if err != nil {
        return nil, err
    }
    return &v1.UserCreateRes{Id: id}, nil
}
```

## Logic 层（业务逻辑）

所有业务逻辑放在 `internal/logic/`，入参/出参使用结构体，首参为 `ctx`。

```go
package user

import (
    "context"
    "project/internal/dao"
    "project/internal/model/do"
    "project/internal/model/entity"
    "github.com/gogf/gf/v2/errors/gerror"
)

type sUser struct{}

func init() {
    service.RegisterUser(New())
}

func New() service.IUser {
    return &sUser{}
}

func (s *sUser) GetById(ctx context.Context, id uint) (*entity.User, error) {
    var user *entity.User
    err := dao.User.Ctx(ctx).Where(dao.User.Columns().Id, id).Scan(&user)
    if err != nil {
        return nil, gerror.Wrap(err, "get user failed")
    }
    if user == nil {
        return nil, gerror.New("user not found")
    }
    return user, nil
}

func (s *sUser) Create(ctx context.Context, in service.UserCreateInput) (uint, error) {
    result, err := dao.User.Ctx(ctx).Data(do.User{
        Name:     in.Name,
        Email:    in.Email,
        Password: in.Password, // 实际应加密
        Status:   1,
    }).Insert()
    if err != nil {
        return 0, gerror.Wrap(err, "create user failed")
    }
    id, _ := result.LastInsertId()
    return uint(id), nil
}
```

## 配置

### 应用配置 (manifest/config/config.yaml)
```yaml
server:
  address:    ":8000"
  serverRoot: "resource/public"

database:
  default:
    link: "mysql:root:password@tcp(127.0.0.1:3306)/mydb"
    debug: true

log:
  level: "all"
  stdout: true
```

### CLI 配置 (hack/config.yaml，供 gf gen dao 等)
```yaml
gfcli:
  gen:
    dao:
    - link:            "mysql:root:password@tcp(127.0.0.1:3306)/mydb"
      tables:          ""
      removePrefix:    "gf_"
      descriptionTag:  true
      jsonCase:        "Snake"
```

### 代码中读取配置
```go
// 使用 g.Cfg() 或 g.Cfg("config.yaml")
port := g.Cfg().MustGet(ctx, "server.address").String()
dbLink := g.Cfg().MustGet(ctx, "database.default.link").String()
```

## 错误处理

```go
// 包装错误，保留堆栈
if err != nil {
    return gerror.Wrap(err, "user creation failed")
}

// 带错误码（便于 API 返回）
if err != nil {
    return gerror.WrapCode(gcode.CodeDbOperationError, err, "database error")
}

// 业务错误
return gerror.New("user not found")
```

## 校验 (v 标签)

```go
type CreateReq struct {
    Name     string `v:"required|length:2,50#名称必填|名称长度2-50"`
    Email    string `v:"required|email#邮箱必填|邮箱格式错误"`
    Age      int    `v:"min:0|max:150#年龄无效"`
    Status   int    `v:"in:0,1,2#状态只能是0/1/2"`
}
```

常用规则：`required`、`email`、`length:min,max`、`min`、`max`、`in:a,b,c`、`password`、`url` 等。详见 gf-skills [core-patterns.md](https://github.com/zhanghoufu/gf-skills/blob/main/references/core-patterns.md)。

## DAO 使用

```go
// 查询单条
var user *entity.User
err := dao.User.Ctx(ctx).Where(dao.User.Columns().Id, id).Scan(&user)

// 列表（条件 + 分页）
var list []*entity.User
err := dao.User.Ctx(ctx).
    Where(dao.User.Columns().Status, 1).
    OrderDesc(dao.User.Columns().CreatedAt).
    Limit(limit).Offset(offset).
    Scan(&list)

// 插入（使用 DO）
result, err := dao.User.Ctx(ctx).Data(do.User{
    Name:   name,
    Email:  email,
    Status: 1,
}).Insert()

// 更新
_, err := dao.User.Ctx(ctx).Where(dao.User.Columns().Id, id).Data(do.User{
    Name:   newName,
    Status: status,
}).Update()

// 删除（硬删）
_, err := dao.User.Ctx(ctx).Where(dao.User.Columns().Id, id).Delete()
```

**注意**：列名使用 `dao.User.Columns().Xxx`，不要手写字符串。

## 文档规范 (README.md)

- 服务概述与用途
- 功能列表
- API 端点说明（路径、方法、请求/响应示例）
- 配置说明（config.yaml 关键项）
- 运行方式：`gf run main.go` / `gf build`
- 测试示例：curl 或单元测试命令

## 生成与规范小结

- 先定义 API（api/module/v1/*.go），再 `gf gen ctrl`，再实现 logic，最后 `gf gen service`
- 所有函数首参为 `ctx context.Context`
- 入参/出参使用结构体，并用 `v` 做校验
- 错误统一用 `gerror.Wrap` / `gerror.New`，避免 `fmt.Errorf`、`errors.New`
- 不把业务逻辑写在 controller 或 dao 中
- 为新服务生成 README.md，包含 API 说明与示例
