# GoFrame Workflows

Standardized workflows for common development tasks.

## 1. Initializing a New Project

```bash
# Step 1: Create project
gf init my-project

# Step 2: Enter directory
cd my-project

# Step 3: Configure database (if needed)
# Edit manifest/config/config.yaml

# Step 4: Run with hot-reload
gf run main.go
```

## 2. Adding a New API Endpoint

```bash
# Step 1: Define API in api/module/v1/xxx.go
# Use g.Meta tags for routing and documentation

# Step 2: Generate controller stub
gf gen ctrl

# Step 3: Implement business logic in internal/logic/xxx/xxx.go
# Add init() function to register with service

# Step 4: Generate service interface
gf gen service

# Step 5: Test the endpoint
gf run main.go
```

## 3. Database-Driven Development

```bash
# Step 1: Create/modify database tables

# Step 2: Configure hack/config.yaml with database connection

# Step 3: Generate DAO layer
gf gen dao

# Step 4: Use DAO in logic layer
# Example: dao.User.Ctx(ctx).Where(...).One()

# Step 5: Run application
gf run main.go
```

## 4. gRPC Microservice Development (Monorepo)

```bash
# Step 1: Initialize Monorepo
gf init myproject -m
cd myproject

# Step 2: Install required components
go get -u github.com/gogf/gf/contrib/rpc/grpcx/v2
go get -u github.com/gogf/gf/contrib/drivers/mysql/v2
go get -u github.com/gogf/gf/contrib/registry/etcd/v2

# Step 3: Create service directory
mkdir -p app/user

# Step 4: Define .proto file in app/user/api/

# Step 5: Generate protobuf and controller
cd app/user && gf gen pb

# Step 6: Implement controller methods

# Step 7: Configure service in manifest/config/config.yaml
```

## 5. Full CRUD for a New Entity

```bash
# Step 1: Create database table

# Step 2: Generate DAO
gf gen dao

# Step 3: Define API endpoints in api/entity/v1/entity.go
# CreateReq, UpdateReq, DeleteReq, GetOneReq, GetListReq

# Step 4: Generate controller
gf gen ctrl

# Step 5: Implement logic in internal/logic/entity/
# Create, Update, Delete, GetOne, GetList methods

# Step 6: Generate service
gf gen service

# Step 7: Bind controller in internal/cmd/cmd.go
```

## 6. After Code Changes

| What Changed | Action Required |
|--------------|-----------------|
| `internal/logic/*.go` | Run `gf gen service` |
| `api/*/v1/*.go` | Run `gf gen ctrl` |
| Database schema | Run `gf gen dao` |
| `.proto` files | Run `gf gen pb` |
| Any `.go` file | `gf run` will auto-reload |

## 7. Production Build

```bash
# Build for current platform
gf build

# Build with specific options (configure in hack/config.yaml)
gf build -n my-app -v 1.0.0

# Build Docker image
gf docker
```
