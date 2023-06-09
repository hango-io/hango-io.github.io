# Hango-OpenAPI目录

> 支持版本: v1.4.0+

## 1 域名

### 1.1 CreateDomain- 创建域名

#### 请求方法

| 名称 | Path       | Action       | 描述     |
| ---- | ---------- | ------------ | -------- |
| POST | /v1/domain | CreateDomain | 创建域名 |

#### 请求参数

| 名称          | 类型   | 必填 | 描述                               | 示例值       |
| ------------- | ------ | ---- | ---------------------------------- | ------------ |
| Host          | String | 是   | 自定义域名，全局唯一，不支持泛域名 | hango.com    |
| Protocol      | String | 是   | 协议类型 HTTP/HTTPS                | HTTP         |
| CertificateId | Long   | 否   | SSL证书ID，若协议为HTTPS，该值必填 | 1            |
| Description   | String | 否   | 描述信息，200个字符以内            | 这是一个域名 |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Result    | Long   | 域名ID   | 1      |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "334c9542-c689-4aa5-9547-acaef8e3dd31",
  "Result": 10
}
```

### 1.2 UpdateDomain- 更新域名

#### 请求方法

| 名称 | Path       | Action       | 描述     |
| ---- | ---------- | ------------ | -------- |
| POST | /v1/domain | UpdateDomain | 更新域名 |

#### 请求参数

| 名称        | 类型   | 必填 | 描述                    | 示例值       |
| ----------- | ------ | ---- | ----------------------- | ------------ |
| DomainId    | Long   | 是   | 域名ID                  | 1            |
| Description | String | 否   | 描述信息，200个字符以内 | 这是一个域名 |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Result    | Long   | 域名ID   | 1      |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "334c9542-c689-4aa5-9547-acaef8e3dd31",
  "Result": 10
}
```

### 1.3 DeleteDomain- 删除域名

#### 请求方法

| 名称 | Path       | Action       | 描述                                       |
| ---- | ---------- | ------------ | ------------------------------------------ |
| GET  | /v1/domain | DeleteDomain | 删除域名，当域名已被网关使用，则不允许删除 |

#### 请求参数

| 名称        | 类型   | 必填 | 描述                    | 示例值       |
| ----------- | ------ | ---- | ----------------------- | ------------ |
| DomainId    | Long   | 是   | 域名ID                  | 1            |
| Description | String | 否   | 描述信息，200个字符以内 | 这是一个域名 |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Result    | Long   | 域名ID   | 1      |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "334c9542-c689-4aa5-9547-acaef8e3dd31",
  "Result": 10
}
```

### 1.4 DescribeDomainList- 查询域名列表

#### 请求方法

| 名称 | Path       | Action             | 描述         |
| ---- | ---------- | ------------------ | ------------ |
| POST | /v1/domain | DescribeDomainList | 查询域名列表 |

#### 请求参数

| 名称        | 类型   | 必填 | 描述                                 | 示例值   |
| ----------- | ------ | ---- | ------------------------------------ | -------- |
| VirtualGwId | Long   | 否   | 虚拟网关Id，查询虚拟网关下绑定的域名 | 1        |
| Pattern     | String | 否   | 域名模糊查询条件                     | hango.co |
| Protocol    | String | 否   | 协议类型 HTTP/HTTPS                  | HTTP     |

#### 返回参数

| 名称      | 类型                | 描述     | 示例值 |
| --------- | ------------------- | -------- | ------ |
| Code      | String              | 返回码   |        |
| Message   | String              | 返回信息 |        |
| RequestId | String              | 请求ID   |        |
| Result    | List<DomainInfoDto> | 域名列表 |        |

DomainInfoDto

| 名称            | 类型   | 描述     | 示例值       |
| --------------- | ------ | -------- | ------------ |
| DomainId        | Long   | 域名Id   | 2            |
| Host            | String | 域名内容 | hango.com    |
| Protocol        | String | 协议     | HTTP         |
| CertificateId   | Long   | 证书Id   | 1            |
| CertificateName | String | 证书名称 | gateway_cert |
| ProjectId       | Long   | 项目Id   | 证书所属项目 |
| Description     | String | 描述信息 | 这是一个域名 |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "02fc0f6b-8251-4bce-a006-f5d0b4a95ca6",
  "Result": [
    {
      "DomainId": 4,
      "Host": "test287.com",
      "ProjectId": 3,
      "Protocol": "HTTP",
      "CertificateId":"1",
      "CertificateName":"287cert",
      "Description":"这是一个域名"
    }
  ],
}
```

## 2 网关

### 2.1 UpdateProjectBinding- 关联项目

#### 请求方法

| 名称 | Path               | Action               | 描述     |
| ---- | ------------------ | -------------------- | -------- |
| POST | /v1/virtualGateway | UpdateProjectBinding | 关联项目 |

#### 请求参数

| 名称          | 类型       | 必填 | 描述                 | 示例值  |
| ------------- | ---------- | ---- | -------------------- | ------- |
| VirtualGwId   | Long       | 是   | 网关Id               | 1       |
| ProjectIdList | List<Long> | 是   | 需要关联的项目id列表 | [1,2,3] |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Result    | Long   | 网关ID   | 1      |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "334c9542-c689-4aa5-9547-acaef8e3dd31",
  "Result": 1
}
```

### 2.2 UnBindProject- 解绑项目

#### 请求方法

| 名称 | Path               | Action        | 描述     |
| ---- | ------------------ | ------------- | -------- |
| GET  | /v1/virtualGateway | UnBindProject | 解绑项目 |

#### 请求参数

| 名称        | 类型 | 必填 | 描述             | 示例值 |
| ----------- | ---- | ---- | ---------------- | ------ |
| VirtualGwId | Long | 是   | 网关id           | 1      |
| ProjectId   | Long | 是   | 需要解绑的项目Id | 2      |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Result    | Long   | 网关ID   | 1      |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "334c9542-c689-4aa5-9547-acaef8e3dd31",
  "Result": 10
}
```

### 2.3 BindDomainInfo- 绑定域名

#### 请求方法

| 名称 | Path               | Action         | 描述                                     |
| ---- | ------------------ | -------------- | ---------------------------------------- |
| POST | /v1/virtualGateway | BindDomainInfo | 网关绑定域名，要求域名所属项目已关联网关 |

#### 请求参数

| 名称        | 类型       | 必填 | 描述                 | 示例值  |
| ----------- | ---------- | ---- | -------------------- | ------- |
| VirtualGwId | Long       | 是   | 网关Id               | 1       |
| DomainIds   | List<Long> | 是   | 需要关联的域名id列表 | [1,2,3] |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Result    | Long   | 网关ID   | 1      |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "334c9542-c689-4aa5-9547-acaef8e3dd31",
  "Result": 1
}
```

### 2.4 UnBindDomainInfo- 解绑域名

#### 请求方法

| 名称 | Path               | Action           | 描述     |
| ---- | ------------------ | ---------------- | -------- |
| POST | /v1/virtualGateway | UnbindDomainInfo | 解绑域名 |

#### 请求参数

| 名称        | 类型       | 必填 | 描述                 | 示例值  |
| ----------- | ---------- | ---- | -------------------- | ------- |
| VirtualGwId | Long       | 是   | 网关id               | 1       |
| DomainIds   | List<Long> | 是   | 需要解绑的域名Id列表 | [1,2,3] |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Result    | Long   | 网关ID   | 1      |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "334c9542-c689-4aa5-9547-acaef8e3dd31",
  "Result": 10
}
```

### 2.5 DescribeVirtualGatewayList- 查询网关列表

#### 请求方法

| 名称 | Path               | Action                     | 描述         |
| ---- | ------------------ | -------------------------- | ------------ |
| POST | /v1/virtualGateway | DescribeVirtualGatewayList | 查询网关列表 |

#### 请求参数

| 名称          | 类型       | 必填 | 描述                                 | 示例值      |
| ------------- | ---------- | ---- | ------------------------------------ | ----------- |
| ProjectIdList | List<Long> | 否   | 项目Id列表，查询关联项目列表下的网关 | [1,2]       |
| Pattern       | String     | 否   | 网关名称模糊查询条件                 | gateway-pro |

#### 返回参数

| 名称      | 类型                    | 描述         | 示例值 |
| --------- | ----------------------- | ------------ | ------ |
| Code      | String                  | 返回码       |        |
| Message   | String                  | 返回信息     |        |
| RequestId | String                  | 请求ID       |        |
| Result    | List<VirtualGatewayDto> | 虚拟网关列表 |        |

VirtualGatewayDto

| 名称         | 类型                | 描述                       | 示例值                       |
| ------------ | ------------------- | -------------------------- | ---------------------------- |
| VirtualGwId  | Long                | 网关Id                     | 2                            |
| Name         | String              | 网关名称                   | vg-test-name                 |
| Code         | String              | 网关唯一标识               | gateway-proxy                |
| Addr         | String              | 网关访问地址               | http://59.111.23.12          |
| ProjectIds   | List<Long>          | 网关绑定项目Id列表         | [2,3,4]                      |
| ProjectNames | List<String>        | 网关绑定项目名称列表       | [project1,project2,project3] |
| DomainInfos  | List<DomainInfoDto> | 网关绑定域名               |                              |
| EnvId        | String              | 网关环境                   | prod                         |
| Protocol     | String              | 网关协议                   | HTTP                         |
| Port         | Int                 | 网关端口                   | 80                           |
| CreateTime   | Long                | 创建时间，以时间戳形式展示 | 1675850679661                |
| ModifyTime   | Long                | 更新时间，以时间戳形式展示 | 1675850679661                |
| Description  | String              | 备注信息                   | 这是一个网关                 |

DomainInfoDto

| 名称            | 类型   | 描述     | 示例值       |
| --------------- | ------ | -------- | ------------ |
| DomainId        | Long   | 域名Id   | 2            |
| Host            | String | 域名内容 | hango.com    |
| Protocol        | String | 协议     | HTTP         |
| CertificateId   | Long   | 证书Id   | 1            |
| CertificateName | String | 证书名称 | gateway_cert |
| ProjectId       | Long   | 项目Id   | 证书所属项目 |
| Description     | String | 描述信息 | 这是一个域名 |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "e705f3f9-d183-4065-ba08-eed1e7456c02",
  "Result": [
    {
      "VirtualGwId": 1,
      "Addr": "httpbin.org",
      "Code": "system",
      "Name": "gateway-proxy",
      "Port": 80,
      "ProjectIds": [
        3
      ],
      "ProjectNames": [
        "project1"
      ],
      "Protocol": "HTTP",
      "DomainInfos": [
        {
          "Description": "创建域名",
          "DomainId": 144,
          "Host": "test-qingzhou.com",
          "ProjectId": 3,
          "Protocol": "HTTP"
        }
      ],
      "EnvId": "prod",
      "CreateTime": 1675850679661,
      "ModifyTime": 1682513396396,
      "Description": "默认网关1"
    }
  ]
}
```

## 3 服务

### 3.1 CreateService- 创建服务

#### 请求方法

| 名称 | Path        | Action        | 描述     |
| ---- | ----------- | ------------- | -------- |
| POST | /v1/service | CreateService | 创建服务 |

#### 请求参数

http服务

| 名称               | 类型             | 必填 | 描述                                                         | 示例值                               |
| ------------------ | ---------------- | ---- | ------------------------------------------------------------ | ------------------------------------ |
| Name               | String           | 是   | 服务名称，以小写字母或数字开头和结尾，支持符号：-，63个字符以内 | bin                                  |
| Alias              | String           | 否   | 服务别名，支持中文                                           | httpbin服务                          |
| Hosts              | String           | 是   | 域名列表，以逗号分隔                                         | a.com,b.com                          |
| BackendService     | String           | 是   | 目标服务                                                     | httpbin.apigw-demo.svc.cluster.local |
| PublishType        | String           | 是   | 发布类型 支持STATIC/DYNAMIC发布， 其中静态发布目标服务的通过ip:pot指定，动态发布指定注册中心中的服务。 | DYNAMIC                              |
| RegistryCenterType | String           | 否   | 注册中心 Kubernetes/Eureka/Nacos，若PublishType为DYNAMIC，则该值必填。默认Kubernetes | Kubernetes                           |
| Protocol           | String           | 是   | 服务协议                                                     | http                                 |
| VirtualGwId        | Long             | 是   | 服务所属网关id                                               | 1                                    |
| TrafficPolicy      | TrafficPolicyDto | 否   | 高级配置，包含负载均衡、连接池配置                           |                                      |

TrafficPolicyDto

| 名称           | 类型            | 必填 | 描述                                                    | 示例值 |
| -------------- | --------------- | ---- | ------------------------------------------------------- | ------ |
| LoadBalancer   | LoadBalancerDto | 否   | 负载均衡策略，支持ROUND_ROBIN、RANDOM、LEAST_CONNECTION |        |
| ConnectionPool | ConnectionPool  | 否   | 连接池配置                                              |        |

LoadBalancer

| 名称   | 类型   | 必填 | 描述                                                         | 示例值     |
| ------ | ------ | ---- | ------------------------------------------------------------ | ---------- |
| Type   | String | 否   | 负载均衡策略类型，目前只开放Simple类型，默认为Simple类型     | Simple     |
| Simple | String | 否   | 具体的负载均衡算法 ROUND_ROBIN\|LEAST_CONN \|RANDOM，默认使用轮询 | LEAST_CONN |

ConnectionPool

| 名称 | 类型                  | 必填 | 描述           | 示例值 |
| ---- | --------------------- | ---- | -------------- | ------ |
| TCP  | TcpConnectionPoolDto  | 否   | tcp连接池配置  |        |
| HTTP | HttpConnectionPoolDto | 否   | http连接池配置 |        |

TcpConnectionPoolDto

| 名称           | 类型    | 必填 | 描述                         | 示例值 |
| -------------- | ------- | ---- | ---------------------------- | ------ |
| MaxConnections | Integer | 否   | 最大连接数，默认值1024       | 1024   |
| ConnectTimeout | Integer | 否   | tcp连接超时时间，默认值60000 | 6000   |

HttpConnectionPoolDto

| 名称                     | 类型    | 必填 | 描述                                                         | 示例值 |
| ------------------------ | ------- | ---- | ------------------------------------------------------------ | ------ |
| Http1MaxPendingRequests  | Integer | 否   | 最大等待HTTP请求数，仅适用于HTTP/1.1的服务，因为HTTP/2协议的请求在到来时会立即复用连接，不会在连接池等待，默认值是1024。 | 1024   |
| Http2MaxRequests         | Integer | 否   | 最大请求数，仅使用于HTTP/2的服务。HTTP/1.1的服务使用maxConnections即可，默认值是1024 | 1024   |
| MaxRequestsPerConnection | Integer | 否   | 每个连接的最大请求数。HTTP/1.1和HTTP/2连接池都遵循此参数，如果没有设置则没有限制，如果设置为1则表示禁用了keep-alive，0表示不限制最多处理的请求数为2^29，默认值0 | 0      |
| IdleTimeout              | Integer | 否   | 空闲超时，定义在多长时间内没有活动请求则关闭连接，默认值3000 | 3000   |

Dubbo服务

| 名称               | 类型             | 必填 | 描述                                                         | 示例值                                                       |
| ------------------ | ---------------- | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Name               | String           | 是   | 服务名称，以小写字母或数字开头和结尾，支持符号：-，63个字符以内 | dubbo-service                                                |
| Alias              | String           | 否   | 服务别名，支持中文                                           | dubbo测试服务                                                |
| BackendService     | String           | 是   | 目标服务，格式为interface:group:version，以.dubbo后缀结尾    | com.netease.apigateway.dubbo.api.TestService:group-a:v2.dubbo |
| PublishType        | String           | 是   | 发布类型 dubbo服务只允许配置DYNAMIC                          | DYNAMIC                                                      |
| RegistryCenterType | String           | 否   | 注册中心 dubbo服务只允许Zookeeper                            | Zookeeper                                                    |
| Protocol           | String           | 是   | 服务协议                                                     | dubbo                                                        |
| VirtualGwId        | Long             | 是   | 服务所属网关id                                               | 1                                                            |
| TrafficPolicy      | TrafficPolicyDto | 否   | 高级配置，包含负载均衡、连接池配置                           | 见HTTP服务                                                   |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Id        | Long   | 服务ID   | 1      |

#### 示例

```
{
  "RequestId": "77c3b3bd-c772-4c11-bc8c-14e4f2383bf5",
  "Message": "处理成功",
  "Id": 160,
  "Code": "Success"
}
```

### 3.2 UpdateService- 更新服务

#### 请求方法

| 名称 | Path        | Action        | 描述     |
| ---- | ----------- | ------------- | -------- |
| POST | /v1/service | UpdateService | 更新域名 |

#### 请求参数

| 名称           | 类型             | 必填 | 描述                               | 示例值                               |
| -------------- | ---------------- | ---- | ---------------------------------- | ------------------------------------ |
| Id             | Long             | 是   | 服务id                             | 1                                    |
| Alias          | String           | 否   | 服务别名，支持中文                 | httpbin服务                          |
| Hosts          | String           | 是   | 域名列表，以逗号分隔               | a.com,b.com                          |
| TrafficPolicy  | TrafficPolicyDto | 否   | 高级配置，包含负载均衡、连接池配置 |                                      |
| BackendService | String           | 是   | 后端服务实例                       | httpbin.apigw-demo.svc.cluster.local |

TrafficPolicyDto

| 名称           | 类型            | 必填 | 描述                                                    | 示例值 |
| -------------- | --------------- | ---- | ------------------------------------------------------- | ------ |
| LoadBalancer   | LoadBalancerDto | 否   | 负载均衡策略，支持ROUND_ROBIN、RANDOM、LEAST_CONNECTION |        |
| ConnectionPool | ConnectionPool  | 否   | 连接池配置                                              |        |

LoadBalancer

| 名称   | 类型   | 必填 | 描述                                                         | 示例值     |
| ------ | ------ | ---- | ------------------------------------------------------------ | ---------- |
| Type   | String | 否   | 负载均衡策略类型，目前只开放Simple类型，默认为Simple类型     | Simple     |
| Simple | String | 否   | 具体的负载均衡算法 ROUND_ROBIN\|LEAST_CONN \|RANDOM，默认使用轮询 | LEAST_CONN |

ConnectionPool

| 名称 | 类型                  | 必填 | 描述           | 示例值 |
| ---- | --------------------- | ---- | -------------- | ------ |
| TCP  | TcpConnectionPoolDto  | 否   | tcp连接池配置  |        |
| HTTP | HttpConnectionPoolDto | 否   | http连接池配置 |        |

TcpConnectionPoolDto

| 名称           | 类型    | 必填 | 描述                         | 示例值 |
| -------------- | ------- | ---- | ---------------------------- | ------ |
| MaxConnections | Integer | 否   | 最大连接数，默认值1024       | 1024   |
| ConnectTimeout | Integer | 否   | tcp连接超时时间，默认值60000 | 6000   |

HttpConnectionPoolDto

| 名称                     | 类型    | 必填 | 描述                                                         | 示例值 |
| ------------------------ | ------- | ---- | ------------------------------------------------------------ | ------ |
| Http1MaxPendingRequests  | Integer | 否   | 最大等待HTTP请求数，仅适用于HTTP/1.1的服务，因为HTTP/2协议的请求在到来时会立即复用连接，不会在连接池等待，默认值是1024。 | 1024   |
| Http2MaxRequests         | Integer | 否   | 最大请求数，仅使用于HTTP/2的服务。HTTP/1.1的服务使用maxConnections即可，默认值是1024 | 1024   |
| MaxRequestsPerConnection | Integer | 否   | 每个连接的最大请求数。HTTP/1.1和HTTP/2连接池都遵循此参数，如果没有设置则没有限制，如果设置为1则表示禁用了keep-alive，0表示不限制最多处理的请求数为2^29，默认值0 | 0      |
| IdleTimeout              | Integer | 否   | 空闲超时，定义在多长时间内没有活动请求则关闭连接，默认值3000 | 3000   |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Id        | Long   | 服务ID   | 1      |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "334c9542-c689-4aa5-9547-acaef8e3dd31",
  "Result": 10
}
```

### 3.3 DeleteService- 删除服务

#### 请求方法

| 名称 | Path        | Action        | 描述                                       |
| ---- | ----------- | ------------- | ------------------------------------------ |
| GET  | /v1/service | DeleteService | 删除服务，若服务已经关联路由，则不允许删除 |

#### 请求参数

| 名称 | 类型 | 必填 | 描述   | 示例值 |
| ---- | ---- | ---- | ------ | ------ |
| Id   | Long | 是   | 服务ID | 1      |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Id        | Long   | 服务ID   | 1      |

#### 示例

```
{
  "Code": "Success",
  "Message": "处理成功",
  "RequestId": "334c9542-c689-4aa5-9547-acaef8e3dd31",
  "Id": 10
}
```

### 3.4 DescribeServicePage- 查询服务列表

#### 请求方法

| 名称 | Path        | Action              | 描述                   |
| ---- | ----------- | ------------------- | ---------------------- |
| GET  | /v1/service | DescribeServicePage | 查询服务列表，支持分页 |

#### 请求参数

| 名称        | 类型   | 必填 | 描述                                         | 示例值 |
| ----------- | ------ | ---- | -------------------------------------------- | ------ |
| VirtualGwId | Long   | 否   | 虚拟网关Id，查询虚拟网关下的服务             | 1      |
| Pattern     | String | 否   | 模糊查询条件，模糊匹配服务名称或服务表示test |        |
| Offset      | Long   | 是   |                                              | 0      |
| Limit       | Long   | 是   | 最大1000                                     | 20     |

#### 返回参数

| 名称      | 类型                  | 描述     | 示例值 |
| --------- | --------------------- | -------- | ------ |
| Code      | String                | 返回码   |        |
| Message   | String                | 返回信息 |        |
| RequestId | String                | 请求ID   |        |
| Result    | List<ServiceProxyDto> | 服务列表 |        |

ServiceProxyDto

| 名称               | 类型             | 描述                                                         | 示例值                               |
| ------------------ | ---------------- | ------------------------------------------------------------ | ------------------------------------ |
| Id                 | Long             | 服务Id                                                       | 1                                    |
| Name               | String           | 服务名称，以小写字母或数字开头和结尾，支持符号：-，63个字符以内 | bin                                  |
| Alias              | String           | 服务别名，支持中文                                           | httpbin服务                          |
| Hosts              | String           | 域名列表，以逗号分隔                                         | a.com,b.com                          |
| BackendService     | String           | 目标服务                                                     | httpbin.apigw-demo.svc.cluster.local |
| PublishType        | String           | 发布类型 支持STATIC/DYNAMIC发布， 其中静态发布目标服务的通过ip:pot指定，动态发布指定注册中心中的服务。 | DYNAMIC                              |
| RegistryCenterType | String           | 注册中心 Kubernetes/Eureka/Nacos，若PublishType为DYNAMIC，则该值必填。默认Kubernetes | Kubernetes                           |
| Protocol           | String           | 服务协议                                                     | http                                 |
| VirtualGwId        | Long             | 服务所属网关id                                               | 1                                    |
| TrafficPolicy      | TrafficPolicyDto | 高级配置，包含负载均衡、连接池配置                           |                                      |
| UpdateTime         | Long             | 更新时间                                                     | 1682575653236                        |
| ProjectId          | Long             | 项目Id                                                       | 3                                    |

TrafficPolicyDto

| 名称           | 类型            | 必填 | 描述                                                    | 示例值 |
| -------------- | --------------- | ---- | ------------------------------------------------------- | ------ |
| LoadBalancer   | LoadBalancerDto | 否   | 负载均衡策略，支持ROUND_ROBIN、RANDOM、LEAST_CONNECTION |        |
| ConnectionPool | ConnectionPool  | 否   | 连接池配置                                              |        |

LoadBalancer

| 名称   | 类型   | 必填 | 描述                                                         | 示例值     |
| ------ | ------ | ---- | ------------------------------------------------------------ | ---------- |
| Type   | String | 否   | 负载均衡策略类型，目前只开放Simple类型，默认为Simple类型     | Simple     |
| Simple | String | 否   | 具体的负载均衡算法 ROUND_ROBIN\|LEAST_CONN \|RANDOM，默认使用轮询 | LEAST_CONN |

ConnectionPool

| 名称 | 类型                  | 必填 | 描述           | 示例值 |
| ---- | --------------------- | ---- | -------------- | ------ |
| TCP  | TcpConnectionPoolDto  | 否   | tcp连接池配置  |        |
| HTTP | HttpConnectionPoolDto | 否   | http连接池配置 |        |

TcpConnectionPoolDto

| 名称           | 类型    | 必填 | 描述                         | 示例值 |
| -------------- | ------- | ---- | ---------------------------- | ------ |
| MaxConnections | Integer | 否   | 最大连接数，默认值1024       | 1024   |
| ConnectTimeout | Integer | 否   | tcp连接超时时间，默认值60000 | 6000   |

HttpConnectionPoolDto

| 名称                     | 类型    | 必填 | 描述                                                         | 示例值 |
| ------------------------ | ------- | ---- | ------------------------------------------------------------ | ------ |
| Http1MaxPendingRequests  | Integer | 否   | 最大等待HTTP请求数，仅适用于HTTP/1.1的服务，因为HTTP/2协议的请求在到来时会立即复用连接，不会在连接池等待，默认值是1024。 | 1024   |
| Http2MaxRequests         | Integer | 否   | 最大请求数，仅使用于HTTP/2的服务。HTTP/1.1的服务使用maxConnections即可，默认值是1024 | 1024   |
| MaxRequestsPerConnection | Integer | 否   | 每个连接的最大请求数。HTTP/1.1和HTTP/2连接池都遵循此参数，如果没有设置则没有限制，如果设置为1则表示禁用了keep-alive，0表示不限制最多处理的请求数为2^29，默认值0 | 0      |
| IdleTimeout              | Integer | 否   | 空闲超时，定义在多长时间内没有活动请求则关闭连接，默认值3000 | 3000   |

```
{
  "TotalCount": 1,
  "RequestId": "99046c6f-22cf-4431-9c8f-be3c5de62b05",
  "Message": "处理成功",
  "Code": "Success",
  "Result": [
    {
      "Alias": "eee",
      "BackendService": "nginxcluster-1-clusterip-service.test.svc.cluster.local",
      "GwClusterName": "prod-gateway",
      "GwType": "envoy",
      "Hosts": "2.com",
      "Id": 202,
      "Name": "eee",
      "ProjectId": 3,
      "Protocol": "http",
      "PublishType": "DYNAMIC",
      "RegistryCenterType": "Kubernetes",
      "TrafficPolicy": {
        "ConnectionPool": {
          "HTTP": {
            "Http1MaxPendingRequests": 1024,
            "Http2MaxRequests": 1024,
            "IdleTimeout": 3000,
            "MaxRequestsPerConnection": 0
          },
          "TCP": {
            "ConnectTimeout": 60000,
            "MaxConnections": 1024
          }
        },
        "LoadBalancer": {
          "Simple": "ROUND_ROBIN",
          "Type": "Simple"
        },
      },
      "UpdateTime": 1682575653236,
      "VirtualGwName": "gateway-proxy"
    }
  ]
}
```

### 3.5 DescribeService 查询服务详情

#### 请求方法

| 名称 | Path        | Action          | 描述         |
| ---- | ----------- | --------------- | ------------ |
| GET  | /v1/service | DescribeService | 查询服务详情 |

#### 请求参数

| 名称 | 类型 | 必填 | 描述   | 示例值 |
| ---- | ---- | ---- | ------ | ------ |
| Id   | Long | 是   | 服务Id | 1      |

#### 返回参数

| 名称      | 类型            | 描述     | 示例值 |
| --------- | --------------- | -------- | ------ |
| Code      | String          | 返回码   |        |
| Message   | String          | 返回信息 |        |
| RequestId | String          | 请求ID   |        |
| Result    | ServiceProxyDto | 服务详情 |        |

ServiceProxyDto

| 名称               | 类型             | 描述                                                         | 示例值                               |
| ------------------ | ---------------- | ------------------------------------------------------------ | ------------------------------------ |
| Id                 | Long             | 服务Id                                                       | 1                                    |
| Name               | String           | 服务名称，以小写字母或数字开头和结尾，支持符号：-，63个字符以内 | bin                                  |
| Alias              | String           | 服务别名，支持中文                                           | httpbin服务                          |
| Hosts              | String           | 域名列表，以逗号分隔                                         | a.com,b.com                          |
| BackendService     | String           | 目标服务                                                     | httpbin.apigw-demo.svc.cluster.local |
| PublishType        | String           | 发布类型 支持STATIC/DYNAMIC发布， 其中静态发布目标服务的通过ip:pot指定，动态发布指定注册中心中的服务。 | DYNAMIC                              |
| RegistryCenterType | String           | 注册中心 Kubernetes/Eureka/Nacos，若PublishType为DYNAMIC，则该值必填。默认Kubernetes | Kubernetes                           |
| Protocol           | String           | 服务协议                                                     | http                                 |
| VirtualGwId        | Long             | 服务所属网关id                                               | 1                                    |
| TrafficPolicy      | TrafficPolicyDto | 高级配置，包含负载均衡、连接池配置                           | 1                                    |
| UpdateTime         | Long             | 更新时间                                                     | 1682575653236                        |
| ProjectId          | Long             | 项目Id                                                       | 3                                    |

TrafficPolicyDto

| 名称           | 类型            | 必填 | 描述                                                    | 示例值 |
| -------------- | --------------- | ---- | ------------------------------------------------------- | ------ |
| LoadBalancer   | LoadBalancerDto | 否   | 负载均衡策略，支持ROUND_ROBIN、RANDOM、LEAST_CONNECTION |        |
| ConnectionPool | ConnectionPool  | 否   | 连接池配置                                              |        |

LoadBalancer

| 名称   | 类型   | 必填 | 描述                                                         | 示例值     |
| ------ | ------ | ---- | ------------------------------------------------------------ | ---------- |
| Type   | String | 否   | 负载均衡策略类型，目前只开放Simple类型，默认为Simple类型     | Simple     |
| Simple | String | 否   | 具体的负载均衡算法 ROUND_ROBIN\|LEAST_CONN \|RANDOM，默认使用轮询 | LEAST_CONN |

ConnectionPool

| 名称 | 类型                  | 必填 | 描述           | 示例值 |
| ---- | --------------------- | ---- | -------------- | ------ |
| TCP  | TcpConnectionPoolDto  | 否   | tcp连接池配置  |        |
| HTTP | HttpConnectionPoolDto | 否   | http连接池配置 |        |

TcpConnectionPoolDto

| 名称           | 类型    | 必填 | 描述                         | 示例值 |
| -------------- | ------- | ---- | ---------------------------- | ------ |
| MaxConnections | Integer | 否   | 最大连接数，默认值1024       | 1024   |
| ConnectTimeout | Integer | 否   | tcp连接超时时间，默认值60000 | 6000   |

HttpConnectionPoolDto

| 名称                     | 类型    | 必填 | 描述                                                         | 示例值 |
| ------------------------ | ------- | ---- | ------------------------------------------------------------ | ------ |
| Http1MaxPendingRequests  | Integer | 否   | 最大等待HTTP请求数，仅适用于HTTP/1.1的服务，因为HTTP/2协议的请求在到来时会立即复用连接，不会在连接池等待，默认值是1024。 | 1024   |
| Http2MaxRequests         | Integer | 否   | 最大请求数，仅使用于HTTP/2的服务。HTTP/1.1的服务使用maxConnections即可，默认值是1024 | 1024   |
| MaxRequestsPerConnection | Integer | 否   | 每个连接的最大请求数。HTTP/1.1和HTTP/2连接池都遵循此参数，如果没有设置则没有限制，如果设置为1则表示禁用了keep-alive，0表示不限制最多处理的请求数为2^29，默认值0 | 0      |
| IdleTimeout              | Integer | 否   | 空闲超时，定义在多长时间内没有活动请求则关闭连接，默认值3000 | 3000   |

```json
{
  "RequestId": "99046c6f-22cf-4431-9c8f-be3c5de62b05",
  "Message": "处理成功",
  "Code": "Success",
  "Result":{
      "Alias": "eee",
      "BackendService": "nginxcluster-1-clusterip-service.test.svc.cluster.local",
      "GwClusterName": "prod-gateway",
      "GwType": "envoy",
      "Hosts": "2.com",
      "Id": 202,
      "Name": "eee",
      "ProjectId": 3,
      "Protocol": "http",
      "PublishType": "DYNAMIC",
      "RegistryCenterType": "Kubernetes",
      "TrafficPolicy": {
        "ConnectionPool": {
          "HTTP": {
            "Http1MaxPendingRequests": 1024,
            "Http2MaxRequests": 1024,
            "IdleTimeout": 3000,
            "MaxRequestsPerConnection": 0
          },
          "TCP": {
            "ConnectTimeout": 60000,
            "MaxConnections": 1024
          }
        },
        "LoadBalancer": {
          "Simple": "ROUND_ROBIN",
          "Type": "Simple"
        },
      },
      "UpdateTime": 1682575653236,
      "VirtualGwName": "gateway-proxy"
    }
}
```

## 4 路由

### 4.1 CreateRoute- 创建路由

#### 请求方法

| 名称 | Path      | Action      | 描述     |
| ---- | --------- | ----------- | -------- |
| POST | /v1/route | CreateRoute | 创建路由 |

#### 请求参数

| 名称                | 类型             | 必填 | 描述                                                         | 示例值                                                       |
| ------------------- | ---------------- | :--: | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Name                | String           |  是  | 路由名称，以小写字母或数字开头和结尾，支持符号：-，63个字符以内 | bin                                                          |
| Alias               | String           |  否  | 路由别名，支持中文                                           | httpbin路由                                                  |
| VirtualGwId         | Long             |  是  | 网关id                                                       | 2                                                            |
| EnableState         | String           |  是  | 路由状态 enable/disable                                      | enable                                                       |
| ServiceMetaForRoute | List<ServiceDto> |  是  | 路由关联的服务信息，支持多服务                               | [{ServiceId: "26", Weight: "100", Port: 80}]                 |
| Uri                 | MatchDto         |  是  | 路由匹配规则， http path                                     | { "Type": "exact", "Value": [ "/status" ] }                  |
| Method              | List<String>     |  是  | 路由匹配规则，http method                                    | ["POST","GET"]                                               |
| Headers             | List<MatchDto>   |  否  | 路由匹配规则，http  headers                                  | [ { "Type": "exact", "Value": [ "test" ], "Key": "x-test-header"} ] |
| QueryParams         | List<MatchDto>   |  否  | 路由匹配规则，http  query                                    | [ { "Type": "exact", "Value": [ "query" ], "Key": "querytest" } ] |
| Priority            | Long             |  否  | 路由匹配优先级，相同匹配条件，priority越大优先级越高，默认值50 | 30                                                           |
| Timeout             | Long             |  否  | 路由超时时间，默认60000ms                                    | 3000                                                         |
| HttpRetry           | HttpRetryDto     |  否  | 路由重试策略                                                 | { "IsRetry": true, "RetryOn": "5xx", "Attempts": "2", "PerTryTimeout": "60000" } |
| Description         | String           |  否  | 描述信息                                                     | 这是一个http路由                                             |

ServiceMetaForRoute

| 名称      | 类型 | 必填 | 描述             | 示例值 |
| --------- | ---- | ---- | ---------------- | ------ |
| ServiceId | Long | 是   | 服务Id           | 2      |
| Port      | Long | 否   | 服务端口，默认80 | 80     |
| Weight    | Long | 是   | 服务权重         | 50     |

HttpRetryDto

| 名称          | 类型    | 必填 | 描述                                                         | 示例值 |
| ------------- | ------- | ---- | ------------------------------------------------------------ | ------ |
| IsRetry       | Boolean | 否   | 是否开启重试，默认关闭                                       | true   |
| Attempts      | Long    | 否   | 重试次数，默认2次，最大10次                                  | 3      |
| PerTryTimeout | Long    | 否   | 重试超时时间，默认60000，单位ms                              | 2111   |
| RetryOn       | String  | 是   | 重试条件，支持5xx,gateway-error,connect-failure。  5xx目标服务响应任何 5xx；gateway-error：类似于5xx，但只会重试导致 502、503 或 504 的请求。connect-failure：如果由于与上游服务器的连接失败（连接超时等）导致请求失败，网关将尝试重试。（包含在*5xx*中） | 5xx    |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Result    | Long   | 路由ID   | 1      |

#### 示例

```
{
  "RequestId": "77c3b3bd-c772-4c11-bc8c-14e4f2383bf5",
  "Message": "处理成功",
  "Result": 160,
  "Code": "Success"
}
```

### 4.2 UpdateRoute- 更新路由

#### 请求方法

| 名称 | Path      | Action      | 描述     |
| ---- | --------- | ----------- | -------- |
| POST | /v1/route | UpdateRoute | 更新路由 |

#### 请求参数

| 名称                | 类型             | 必填 | 描述                                                         | 示例值                                                       |
| ------------------- | ---------------- | :--: | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Id                  | Long             |  是  | 路由Id                                                       | 2                                                            |
| Alias               | String           |  否  | 路由别名，支持中文                                           | httpbin路由                                                  |
| EnableState         | String           |  是  | 路由状态 enable/disable                                      | enable                                                       |
| ServiceMetaForRoute | List<ServiceDto> |  是  | 路由关联的服务信息，支持多服务                               | [{ServiceId: "26", Weight: "100", Port: 80}]                 |
| Uri                 | MatchDto         |  是  | 路由匹配规则， http path                                     | { "Type": "exact", "Value": [ "/status" ] }                  |
| Method              | List<String>     |  是  | 路由匹配规则，http method                                    | ["POST","GET"]                                               |
| Headers             | List<MatchDto>   |  否  | 路由匹配规则，http  headers                                  | [ { "Type": "exact", "Value": [ "test" ], "Key": "x-test-header"} ] |
| QueryParams         | List<MatchDto>   |  否  | 路由匹配规则，http  query                                    | [ { "Type": "exact", "Value": [ "query" ], "Key": "querytest" } ] |
| Priority            | Long             |  否  | 路由匹配优先级，相同匹配条件，priority越大优先级越高，默认值50 | 30                                                           |
| Timeout             | Long             |  否  | 路由超时时间，默认60000ms                                    | 3000                                                         |
| HttpRetry           | HttpRetryDto     |  否  | 路由重试策略                                                 | { "IsRetry": true, "RetryOn": "5xx", "Attempts": "2", "PerTryTimeout": "60000" } |
| Description         | String           |  否  | 描述信息                                                     | 这是一个http路由                                             |

ServiceMetaForRoute

| 名称      | 类型 | 必填 | 描述             | 示例值 |
| --------- | ---- | ---- | ---------------- | ------ |
| ServiceId | Long | 是   | 服务Id           | 2      |
| Port      | Long | 否   | 服务端口，默认80 | 80     |
| Weight    | Long | 是   | 服务权重         | 50     |

HttpRetryDto

| 名称          | 类型    | 必填 | 描述                                                         | 示例值 |
| ------------- | ------- | ---- | ------------------------------------------------------------ | ------ |
| IsRetry       | Boolean | 否   | 是否开启重试，默认关闭                                       | true   |
| Attempts      | Long    | 否   | 重试次数，默认2次，最大10次                                  | 3      |
| PerTryTimeout | Long    | 否   | 重试超时时间，默认60000，单位ms                              | 2111   |
| RetryOn       | String  | 是   | 重试条件，支持5xx,gateway-error,connect-failure。  5xx目标服务响应任何 5xx；gateway-error：类似于5xx，但只会重试导致 502、503 或 504 的请求。connect-failure：如果由于与上游服务器的连接失败（连接超时等）导致请求失败，网关将尝试重试。（包含在*5xx*中） | 5xx    |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |
| Result    | Long   | 路由ID   | 1      |

#### 示例

```
{
  "RequestId": "77c3b3bd-c772-4c11-bc8c-14e4f2383bf5",
  "Message": "处理成功",
  "Result": 160,
  "Code": "Success"
}
```

### 4.3 DeleteRoute- 删除路由

#### 请求方法

| 名称 | Path      | Action      | 描述     |
| ---- | --------- | ----------- | -------- |
| GET  | /v1/route | DeleteRoute | 删除路由 |

#### 请求参数

| 名称    | 类型 | 必填 | 描述   | 示例值 |
| ------- | ---- | ---- | ------ | ------ |
| RouteId | Long | 是   | 服务ID | 1      |

#### 返回参数

| 名称      | 类型   | 描述     | 示例值 |
| --------- | ------ | -------- | ------ |
| Code      | String | 返回码   |        |
| Message   | String | 返回信息 |        |
| RequestId | String | 请求ID   |        |

#### 示例

```json
{
  "RequestId": "11cfc904-cd58-405b-b8d3-04539c3c97e4",
  "Message": "处理成功",
  "Code": "Success"
}
```

### 4.4 DescribeRouteList- 查询路由列表

#### 请求方法

| 名称 | Path      | Action            | 描述                   |
| ---- | --------- | ----------------- | ---------------------- |
| GET  | /v1/route | DescribeRouteList | 查询路由列表，支持分页 |

#### 请求参数

| 名称         | 类型   | 描述                                  | 示例值 |
| ------------ | ------ | ------------------------------------- | ------ |
| VirtualGwId  | Long   | 网关Id，查询网关下的路由              | 1      |
| Pattern      | String | 模糊查询条件，模糊匹配名称、别名、uri | test   |
| EnableStatus | String | 使能状态                              | enable |
| Limit        | Long   | 最大1000                              | 20     |
| Offset       | Long   |                                       | 0      |

#### 返回参数

| 名称       | 类型           | 描述     | 示例值 |
| ---------- | -------------- | -------- | ------ |
| TotalCount | Long           | 路由总数 | 2      |
| RequestId  | String         | 请求ID   |        |
| Result     | List<RouteDto> | 路由列表 |        |

RouteDto

| 名称                | 类型             | 描述                                                         | 示例值                                                       |
| ------------------- | ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Id                  | Long             | 路由Id                                                       | 2                                                            |
| Alias               | String           | 路由别名，支持中文                                           | httpbin路由                                                  |
| EnableState         | String           | 路由状态 enable/disable                                      | enable                                                       |
| ServiceMetaForRoute | List<ServiceDto> | 路由关联的服务信息，支持多服务                               | [{ServiceId: "26", Weight: "100", Port: 80}]                 |
| Uri                 | MatchDto         | 路由匹配规则， http path                                     | { "Type": "exact", "Value": [ "/status" ] }                  |
| Method              | List<String>     | 路由匹配规则，http method                                    | ["POST","GET"]                                               |
| Headers             | List<MatchDto>   | 路由匹配规则，http  headers                                  | [ { "Type": "exact", "Value": [ "test" ], "Key": "x-test-header"} ] |
| QueryParams         | List<MatchDto>   | 路由匹配规则，http  query                                    | [ { "Type": "exact", "Value": [ "query" ], "Key": "querytest" } ] |
| Priority            | Long             | 路由匹配优先级，相同匹配条件，priority越大优先级越高，默认值50 | 30                                                           |
| Timeout             | Long             | 路由超时时间，默认60000ms                                    | 3000                                                         |
| HttpRetry           | HttpRetryDto     | 路由重试策略                                                 | { "IsRetry": true, "RetryOn": "5xx", "Attempts": "2", "PerTryTimeout": "60000" } |
| Description         | String           | 描述信息                                                     | 这是一个http路由                                             |
| UpdateTime          | Long             | 更新时间                                                     |                                                              |

ServiceMetaForRoute

| 名称           | 类型   | 描述             | 示例值                               |
| -------------- | ------ | ---------------- | ------------------------------------ |
| ServiceId      | Long   | 服务Id           | 2                                    |
| ServiceName    | String | 服务名称         | dubboService                         |
| Port           | Long   | 服务端口，默认80 | 80                                   |
| Weight         | Long   | 服务权重         | 50                                   |
| BackendService | String | 目标服务         | httpbin.apigw-demo.svc.cluster.local |
| Protocol       | String | 协议             | dubbo                                |

HttpRetryDto

| 名称          | 类型    | 必填 | 描述                                                         | 示例值 |
| ------------- | ------- | ---- | ------------------------------------------------------------ | ------ |
| IsRetry       | Boolean | 否   | 是否开启重试，默认关闭                                       | true   |
| Attempts      | Long    | 否   | 重试次数，默认2次，最大10次                                  | 3      |
| PerTryTimeout | Long    | 否   | 重试超时时间，默认60000，单位ms                              | 2111   |
| RetryOn       | String  | 是   | 重试条件，支持5xx,gateway-error,connect-failure。  5xx目标服务响应任何 5xx；gateway-error：类似于5xx，但只会重试导致 502、503 或 504 的请求。connect-failure：如果由于与上游服务器的连接失败（连接超时等）导致请求失败，网关将尝试重试。（包含在*5xx*中） | 5xx    |

#### 示例

```json
{
  "TotalCount": 1,
  "RequestId": "f56d098b-5e2a-49d2-9bdd-74bca1da6fda",
  "RouteList": [
    {
      "Alias": "hjhdemo",
      "CreateTime": 1683271053997,
      "Description": "",
      "EnableState": "enable",
      "EnvId": "prod",
      "GwType": "envoy",
      "Headers": null,
      "Hosts": [
        "qingzhou.com"
      ],
      "HttpRetry": {
        "Attempts": 2,
        "IsRetry": false,
        "PerTryTimeout": 60000,
        "RetryOn": ""
      },
      "Id": 276,
      "MetaMap": null,
      "Method": [],
      "MirrorSwitch": 0,
      "MirrorTraffic": null,
      "Name": "hjhdemo",
      "Orders": 50200723,
      "Priority": 50,
      "ProjectId": 3,
      "QueryParams": null,
      "ServiceMetaForRoute": [
        {
          "BackendService": "hjhdemo.ns1.svc.cluster.local",
          "DestinationServices": null,
          "Port": 3333,
          "Protocol": "http",
          "ServiceId": 269,
          "ServiceName": "hjh-demo",
          "ServiceSource": "Kubernetes",
          "VirtualGateway": "gateway-proxy",
          "Weight": 100
        }
      ],
      "Timeout": 60000,
      "UpdateTime": 1683271062689,
      "Uri": {
        "Type": "prefix",
        "Value": [
          "/hello"
        ]
      },
      "VirtualGwId": 1,
      "VirtualGwName": "gateway-proxy",
      "serviceIds": [
        269
      ]
    }
  ]
}
```

### 4.5 DescribeRoute 查询路由详情

#### 请求方法

| 名称 | Path      | Action        | 描述         |
| ---- | --------- | ------------- | ------------ |
| GET  | /v1/route | DescribeRoute | 查询路由详情 |

#### 请求参数

| 名称 | 类型 | 必填 | 描述   | 示例值 |
| ---- | ---- | ---- | ------ | ------ |
| Id   | Long | 是   | 路由Id | 1      |

#### 返回参数

| 名称      | 类型     | 描述     | 示例值 |
| --------- | -------- | -------- | ------ |
| RequestId | String   | 请求ID   |        |
| Route     | RouteDto | 路由详情 |        |

RouteDto

| 名称                | 类型             | 描述                                                         | 示例值                                                       |
| ------------------- | ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Id                  | Long             | 路由Id                                                       | 2                                                            |
| Alias               | String           | 路由别名，支持中文                                           | httpbin路由                                                  |
| EnableState         | String           | 路由状态 enable/disable                                      | enable                                                       |
| ServiceMetaForRoute | List<ServiceDto> | 路由关联的服务信息，支持多服务                               | [{ServiceId: "26", Weight: "100", Port: 80}]                 |
| Uri                 | MatchDto         | 路由匹配规则， http path                                     | { "Type": "exact", "Value": [ "/status" ] }                  |
| Method              | List<String>     | 路由匹配规则，http method                                    | ["POST","GET"]                                               |
| Headers             | List<MatchDto>   | 路由匹配规则，http  headers                                  | [ { "Type": "exact", "Value": [ "test" ], "Key": "x-test-header"} ] |
| QueryParams         | List<MatchDto>   | 路由匹配规则，http  query                                    | [ { "Type": "exact", "Value": [ "query" ], "Key": "querytest" } ] |
| Priority            | Long             | 路由匹配优先级，相同匹配条件，priority越大优先级越高，默认值50 | 30                                                           |
| Timeout             | Long             | 路由超时时间，默认60000ms                                    | 3000                                                         |
| HttpRetry           | HttpRetryDto     | 路由重试策略                                                 | { "IsRetry": true, "RetryOn": "5xx", "Attempts": "2", "PerTryTimeout": "60000" } |
| Description         | String           | 描述信息                                                     | 这是一个http路由                                             |
| UpdateTime          | Long             | 更新时间                                                     |                                                              |

ServiceMetaForRoute

| 名称           | 类型   | 描述             | 示例值                               |
| -------------- | ------ | ---------------- | ------------------------------------ |
| ServiceId      | Long   | 服务Id           | 2                                    |
| ServiceName    | String | 服务名称         | dubboService                         |
| Port           | Long   | 服务端口，默认80 | 80                                   |
| Weight         | Long   | 服务权重         | 50                                   |
| BackendService | String | 目标服务         | httpbin.apigw-demo.svc.cluster.local |
| Protocol       | String | 协议             | dubbo                                |

HttpRetryDto

| 名称          | 类型    | 必填 | 描述                                                         | 示例值 |
| ------------- | ------- | ---- | ------------------------------------------------------------ | ------ |
| IsRetry       | Boolean | 否   | 是否开启重试，默认关闭                                       | true   |
| Attempts      | Long    | 否   | 重试次数，默认2次，最大10次                                  | 3      |
| PerTryTimeout | Long    | 否   | 重试超时时间，默认60000，单位ms                              | 2111   |
| RetryOn       | String  | 是   | 重试条件，支持5xx,gateway-error,connect-failure。  5xx目标服务响应任何 5xx；gateway-error：类似于5xx，但只会重试导致 502、503 或 504 的请求。connect-failure：如果由于与上游服务器的连接失败（连接超时等）导致请求失败，网关将尝试重试。（包含在*5xx*中） | 5xx    |

#### 示例

```json
{
  "RequestId": "9fde1d4a-28b6-4280-9e1d-953c45c17b03",
  "Route": {
    "Alias": "grpc-scenario-route-rule",
    "CreateTime": 1683278921123,
    "Description": null,
    "EnableState": "enable",
    "Headers": null,
    "Hosts": [
      "test251-qingzhou.com"
    ],
    "HttpRetry": {
      "Attempts": 2,
      "IsRetry": false,
      "PerTryTimeout": 60000,
      "RetryOn": ""
    },
    "Id": 283,
    "Name": "grpc-scenario-route-rule",
    "Priority": 50,
    "ProjectId": 3,
    "QueryParams": null,
    "ServiceMetaForRoute": [
      {
        "BackendService": "grpc-demo-service.apigw-demo.svc.cluster.local:55055"
        "Port": 80,
        "Protocol": "grpc",
        "ServiceId": 281,
        "ServiceName": "grcp-scenario-service",
        "Weight": 100
      }
    ],
    "Timeout": 60000,
    "UpdateTime": 1683278921123,
    "Uri": {
      "Type": "exact",
      "Value": [
        "/helloworld.Greeter/SayHello"
      ]
    }
  }
}
```