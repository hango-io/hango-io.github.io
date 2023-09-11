# Hango插件市场

> 支持版本: v1.6.0+

Hango网关支持集成插件市场功能，允许用户通过配置插件市场来管理和使用各类自定义插件能力，以下罗列当前版本支持的插件类型、形态和作用域

| 插件类型 | 形态        | 范围      |
|------|-----------|---------|
| WASM | OCI（标准镜像） | 虚拟网关    |
| Lua  | OCI（标准镜像） | 路由/虚拟网关 |

## 1.WASM插件使用引导

Wasm是WebAssembly的缩写，它是一种二进制格式，通常我们可以将种编程语言的代码，包括C++、Rust、Go等编译成Wasm二进制文件，只不过各个语言都有自己编译成wasm的工具，如C++和Rust都可以使用Proxy-Wasm提供的方式编译成Wasm脚本，而Go语言则可以使用TinyGo将Golang代码文件转为Wasm二进制文件。

本文以一份Go代码为例，演示Wasm类型插件在Hango插件市场的使用流程

### 1.1.准备Golang代码

以下是一份Go代码，内容是将我们配置参数写入响应头，go api详见：[proxy-wasm-go-sdk api](https://github.com/tetratelabs/proxy-wasm-go-sdk)

```go
package main

import (
	"strings"

	"github.com/tidwall/gjson"

	"github.com/tetratelabs/proxy-wasm-go-sdk/proxywasm"
	"github.com/tetratelabs/proxy-wasm-go-sdk/proxywasm/types"
)

func main() {
	proxywasm.SetVMContext(&vmContext{})
}

type vmContext struct {
	// Embed the default VM context here,
	// so that we don't need to reimplement all the methods.
	types.DefaultVMContext
}

// Override types.DefaultVMContext.
func (*vmContext) NewPluginContext(contextID uint32) types.PluginContext {
	return &pluginContext{}
}

type pluginContext struct {
	// Embed the default plugin context here,
	// so that we don't need to reimplement all the methods.
	types.DefaultPluginContext

	// headerName and headerValue are the header to be added to response. They are configured via
	// plugin configuration during OnPluginStart.
	headerName  string
	headerValue string
}

// Override types.DefaultPluginContext.
func (p *pluginContext) NewHttpContext(contextID uint32) types.HttpContext {
	return &httpHeaders{
		contextID:   contextID,
		headerName:  p.headerName,
		headerValue: p.headerValue,
	}
}

func (p *pluginContext) OnPluginStart(pluginConfigurationSize int) types.OnPluginStartStatus {
	proxywasm.LogDebug("loading plugin config")
	data, err := proxywasm.GetPluginConfiguration()
	if data == nil {
		return types.OnPluginStartStatusOK
	}

	if err != nil {
		proxywasm.LogCriticalf("error reading plugin configuration: %v", err)
	}

	if !gjson.Valid(string(data)) {
		proxywasm.LogCritical(`invalid configuration format; expected {"header": "<header name>", "value": "<header value>"}`)
	}

	p.headerName = strings.TrimSpace(gjson.Get(string(data), "header").Str)
	p.headerValue = strings.TrimSpace(gjson.Get(string(data), "value").Str)

	if p.headerName == "" || p.headerValue == "" {
		proxywasm.LogCritical(`invalid configuration format; expected {"header": "<header name>", "value": "<header value>"}`)
	}

	proxywasm.LogInfof("header from config: %s = %s", p.headerName, p.headerValue)

	return types.OnPluginStartStatusOK
}

type httpHeaders struct {
	// Embed the default http context here,
	// so that we don't need to reimplement all the methods.
	types.DefaultHttpContext
	contextID   uint32
	headerName  string
	headerValue string
}

// Override types.DefaultHttpContext.
func (ctx *httpHeaders) OnHttpRequestHeaders(numHeaders int, endOfStream bool) types.Action {
	err := proxywasm.ReplaceHttpRequestHeader("test", "best")
	if err != nil {
		proxywasm.LogCritical("failed to set request header: test")
	}

	hs, err := proxywasm.GetHttpRequestHeaders()
	if err != nil {
		proxywasm.LogCriticalf("failed to get request headers: %v", err)
	}

	for _, h := range hs {
		proxywasm.LogInfof("request header --> %s: %s", h[0], h[1])
	}
	return types.ActionContinue
}

// Override types.DefaultHttpContext.
func (ctx *httpHeaders) OnHttpResponseHeaders(_ int, _ bool) types.Action {
	proxywasm.LogInfof("adding header: %s=%s", ctx.headerName, ctx.headerValue)

	// Add a hardcoded header
	if err := proxywasm.AddHttpResponseHeader("x-proxy-wasm-go-sdk-example", "http_headers"); err != nil {
		proxywasm.LogCriticalf("failed to set response constant header: %v", err)
	}

	// Add the header passed by arguments
	if ctx.headerName != "" {
		if err := proxywasm.AddHttpResponseHeader(ctx.headerName, ctx.headerValue); err != nil {
			proxywasm.LogCriticalf("failed to set response headers: %v", err)
		}
	}

	// Get and log the headers
	hs, err := proxywasm.GetHttpResponseHeaders()
	if err != nil {
		proxywasm.LogCriticalf("failed to get response headers: %v", err)
	}

	for _, h := range hs {
		proxywasm.LogInfof("response header <-- %s: %s", h[0], h[1])
	}
	return types.ActionContinue
}

// Override types.DefaultHttpContext.
func (ctx *httpHeaders) OnHttpStreamDone() {
	proxywasm.LogInfof("%d finished", ctx.contextID)
}
```

### 1.2.go依赖下载声明

```shell
## 初始化当前项目名为"wasm"（名称可任意），会基于代码文件生成go.mod文件
go mod init wasm

## 下载依赖
go mod tidy
```

### 1.3.编译Wasm二进制文件

我们将上述文件编译为Wasm二进制文件，运行以下命令后我们可以看到同级目录下生成了main.wasm的二进制文件
```shell
tinygo build -o main.wasm -scheduler=none -target=wasi ./main.go
```

### 1.4.本地调测

Envoy支持本地对Wasm插件的调试，但在调试前需要准备以下资源：

#### 安装docker-compose
开发环境必须安装docker-compose，如果没有需要先安装 https://docs.docker.com/compose/install/

#### 下载Rider SDK

通过git下载Rider SDK，以下基于SDK本地开发和调试WASM插件
```shell
git clone https://github.com/hango-io/rider.git
```

#### 配置envoy.yaml

将main.wasm脚本放到rider/rider目录下，同时打开`rider/scripts/dev/envoy.yaml`配置文件，在插件名`envoy.filters.http.wasm`（若没有则添加，添加在`http_filters`节点下，`envoy.filters.http.router`节点之前）下找到filename字段，并将值改为`/usr/local/lib/rider/rider/main.wasm`（修改为该固定值即可，为容器内目录），用于指定envoy在容器内加载wasm脚本的路径，完整配置如下：

```yaml
          - name: envoy.filters.http.wasm
            typed_config:
              "@type": type.googleapis.com/envoy.extensions.filters.http.wasm.v3.Wasm
              config:
                name: "my_plugin"
                root_id: "my_root_id"
                configuration:
                  "@type": type.googleapis.com/google.protobuf.StringValue
                  value: |
                    {
                      "header": "x-wasm-header",
                      "value": "demo-wasm"
                    }
                vm_config:
                  vm_id: "my_vm_id"
                  runtime: "envoy.wasm.runtime.v8"
                  code:
                    local:
                      filename: "/usr/local/lib/rider/rider/main.wasm"
```

#### 执行启动脚本验证

切换到rider项目根目录下执行`./scripts/dev/local-up.sh -f`启动 envoy和一个简单的HTTP服务。该HTTP服务的作用是将请求的详情放入响应中并返回给调用方。

通过`curl -v http://localhost:8002/static-to-header`，可以观察到响应头返回：`x-wasm-header: demo-wasm`（即envoy.yaml文件内所填mock参数）

### 1.5.构建Docker镜像

Dockerfile如下，通过空镜像构建，将wasm文件拷贝至镜像，其中拷贝到容器内的目标文件名必须是`plugin.wasm`

```Dockerfile
FROM scratch
COPY main.wasm plugin.wasm
```

执行镜像构建命令，并将其推至远程镜像仓库

```shell
docker build -t docker.io/hangoio/wasm-rider:v1 .
docker push docker.io/hangoio/wasm-rider:v1
```

### 1.6.上传wasm插件

通过如下页面配置插件基本信息

// TODO Hango配置界面

以下是测试脚本对应的插件表单

```json
{
   "formatter": {
     "kind": "Security",
     "type": "wasm",
     "need_to_response": "&need_to_response",
     "config": {
       "header": "&header",
       "value": "&value"
     }
   },
   "layouts": [
     {
       "key": "header",
       "alias": "请求头key",
       "help": "",
       "type": "input"
     },
     {
       "key": "value",
       "alias": "请求头value",
       "type": "input"
     }
   ]
 }
```

// TODO 插件样式图

完成插件配置后，在列表页面点击`上架插件`，并在网关的`插件配置`中打开插件开关，则当前配置的wasm插件为可用插件

// TODO 开关图

### 1.7.配置插件实例

完成上述步骤后，wasm插件已经上传完毕，我们可以在指定的虚拟网关界面看到插件列表，并找到上传的wasm插件进行实例配置和使用

// TODO 实例图

## 2.Lua插件使用引导

Lua插件上传插件市场的步骤与Wasm一致，以下仅给出Dockerfile和案例

### 2.1.Lua代码

以下定义一个简单Lua脚本，在响应头阶段，将插件配置解析，返回到响应头（x-lua-test：{配置JSON}）

```Lua
require('rider.v2')

-- 定义本地变量
local envoy = envoy
local respond = envoy.respond
local cjson = require "cjson"

local responseHeaderHandler = {}

responseHeaderHandler.version = 'v2'


-- 定义response的body阶段处理函数
function responseHeaderHandler:on_response_header()
    local config = envoy.get_route_config()

    envoy.logInfo('start lua responseHeader')

    -- 配置未定义报错
    if config == nil then
        envoy.logErr('no route config!')
        return
    end

    local header_to_add = "x-lua-test"
    local json_str = cjson.encode(config.data)

    envoy.resp.set_header(header_to_add, json_str)

end

return responseHeaderHandler
```

### 2.2.本地调测

与wasm一致，通过rider组件进行本地调测，不同点在于修改`rider/scripts/dev/envoy.yaml`文件的`proxy.filters.http.rider`filter，将该filter下的filename修改为正确的名称

### 2.3.插件表单

与案例Lua脚本对应的表单如下

```json
{
  "formatter": {
    "kind": "response-body-rewrite",
    "type": "lua",
    "config": {
        "data":"&request"
        }
  },
  "layouts": [
    {
      "key": "request",
      "alias": "请求",
      "type": "layouts",
      "layouts": [
        {
          "key": "requestSwitch",
          "alias": "JSON开关节点",
          "type": "switch",
          "default": false
        },
        {
          "key": "data",
          "type": "layouts",
          "visible": {
            "this.requestSwitch": true
          },
          "layouts": [
            {
              "key": "denylist",
              "type": "input",
              "alias": "JSON输入节点"
            }
          ]
        }
      ]
    }
  ]
}
```

### 2.3.Dockerfile

其中拷贝到容器内的目标文件名必须是`plugin.lua`

```Dockerfile
FROM scratch
COPY main.lua plugin.lua
```