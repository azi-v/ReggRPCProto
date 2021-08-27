# ReggRPCProto

register and login grpc protocol buf

<https://developers.google.com/protocol-buffers/docs/gotutorial>

<https://developers.google.com/protocol-buffers/docs/reference/go-generated>

<https://developers.google.com/protocol-buffers/docs/encoding>

<https://developers.google.com/protocol-buffers/docs/proto3>

<https://www.grpc.io/>

<https://github.com/grpc/grpc-go/tree/master/examples>

## go protobuf 使用前准备

```shell
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest

protoc --proto_path=src --go_out=out --go_opt=paths=source_relative foo.proto bar/baz.proto

export PATH="$PATH:$(go env GOPATH)/bin"
```

## protocl buf

地址:<>

## 服务设计方案

### 并发、横向伸缩、高可用？

- 并发: 消息队列

- 横向伸缩: 注册服务无状态、登录服务有状态

- 高可用: kubernates 自动重启Pod

### 主要组件

#### 服务发现

ETCD 分布式高可用

#### 网关（无状态的网关校验JWT）

用户身份校验、设备状态、风控等；
采用无状态设计（便于横向拓展，避免数据库性能瓶颈对注册登录服务拓展造成限制）

#### BBF（back for forontend）服务

业务数据聚合，相当于DDD中的应用服务；

熔断、限流、降级策略；

#### Account服务

领域服务；

服务内注册信息通过MQ同步各种数据给其他服务，避免网络直接调用其他服务接口；

修改用户密码等操作，不直接该库，通过MQ处理；

服务内不存在直接映射数据库表结构的对象，通过接口调用来实现对数据库的依赖倒置；

#### kong

内网服务间调用网关

#### 定时服务

定时检测数据库中预生成的账户；数量不足时批量生成（根据运营策略进行配置）；

#### 基础设施（依赖倒置设计）

- MQ队列（kafka）
- 数据库
- 缓存

DDD（所有基础设施、依赖倒置设计）

### 系统架构图

未完待续。。。
