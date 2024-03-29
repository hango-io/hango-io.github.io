# 核心概念

本文将介绍Hango的概念以及功能。

通过本文，您能了解到：

* Hango 是什么？

* Hango 的架构及核心概念

* 如何寻求帮助。

## Hango 介绍

Hango 命名取自函谷关，是一个基于 **Envoy** 构建的高性能、可扩展、功能丰富的云原生API网关。

Hango 提供请求代理、动态路由、负载均衡、限流、熔断、健康检查、安全防护、灰度发布等功能，提供了丰富的内置插件以及支持用户自定义插件扩展的能力。

我们可以在微服务网关、七层负载均衡、Kubernetes Ingress、Serverless等场景使用Hango。

### 主要特点

* **技术路线**：基于领先的网络代理组件 **Envoy** 构建，具备丰富的功能、优异的性能与可观测性

* **扩展性**：可用于生产的增强级 Lua 扩展框架 Rider；基于 WebAssembly 的多语言扩展插件能力（beta）

* **多场景**：具备支撑微服务网关、七层负载均衡、Kubernetes Ingress、Serverless网关等多种场景能力

* **云原生**：以云原生标准数据面组件 **Envoy** 作为核心引擎，天然亲和云原生；可作为 Kubernetes、Service Mesh、Serverless 的 Ingress、Gateway 实现

* **API生态与集成**：以 API 为中心的生态管理，提供数百种工业级协议快速集成能力（后续提供）

* **控制平面**：易用的控制台进行网关、服务、路由等多维度管理，简化使用者操作

### 主要特性

* 多协议: 可以使用Hango用于HTTP、Dubbo、WebSocket等多协议路由

* 动态加载: Hango的所有配置，包括路由、治理等均是热生效，意味着用户不需要重启服务进行配置更新

* 多注册中心支持: Hango支持Kubernetes、Eureka、Nacos等多注册中心发现

* 丰富的内置插件：支持多种内置插件，用于限流、熔断、降级、缓存等流量治理，以及黑白名单、Dos等安全防护

* 高性能：基于Envoy作为核心数据面代理，有优异的代理性能

* 多语言：支持用户通过Rider模块扩展自定义插件，实现自定义治理需求

## 架构及核心模块

下图是Hango的核心架构

![](./img/Hango%20arc.png)




