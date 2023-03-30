# Kubernetes部署

## 前置条件

1.目前版本支持基于Kubernetes进行安装，如果没有Kubernetes环境，可以采用 [:material-arrow-right-thick: :material-kubernetes: Kubernetes](https://minikube.sigs.k8s.io/docs/start/) 进行安装。对于Kubernetes的版本，我们要求版本至少是1.17 版本。

2.请确保已安装helm。

3.如果是使用[minikube](https://minikube.sigs.k8s.io/docs/start/)进行部署，最少需要额外预留4G的内存给hango网关进行部署。
由于k8s组件也需要占据一定的内存空间，因此我们推荐给集群配置5G的内存空间， 要扩展Minikube的内存大小可以参考以下命令：
```shell
minikube config set memory 5120
```
停止 Minikube 并重新启动以使更改生效：
```shell
   minikube stop
   minikube start
```
### 安装hango网关

1、进入"hango-gateway/install"目录下，目录结构树如下
```xml
install
├─common
├─crds
├─helm
├─init-hango
├─install.sh
├─check.sh
└─uninstall.sh
```
2、您将看到3个脚本分别用于安装（install.sh）、检查状态（check.sh）、卸载（uninstall.sh），可直接执行命令

注意：在脚本执行前请确保权限足够
```shell
sh install.sh
```
3、等待脚本执行完毕后，可以通过执行下面的命令查看hango网关的运行状态
```shell
sh check.sh
```
正常的运行状态如下，若容器未就绪，请再耐心等待一段时间再检查
```shell

[install-check][14:50:49]
========= pods in namespace[hango-system] show below =========
NAME                               READY   STATUS    RESTARTS   AGE
gateway-proxy-55887cb579-mv9xh     1/1     Running   0          87s
hango-api-plane-6c4554cfc4-ndnx5   1/1     Running   0          101s
hango-portal-597bb489d6-45b2r      1/1     Running   0          101s
hango-ui-75458cc7dc-b4x6b          1/1     Running   0          101s
istio-e2e-app-85bb49bf75-t7slt     1/1     Running   0          101s
hango-istiod-697b5c4456-67l92      1/1     Running   0          95s
slime-75fcb44f68-w9x4x             1/1     Running   0          94s
```
4、访问hango的前端页面需要通过hango-ui的service暴露的端口进行访问
```shell
kubectl get svc -n hango-system |grep hango-ui
```
如果是基于minikube进行安装，可以运行以下命令来获取 hango-ui的Service的URL：
```shell
minikube service hango-ui --url
```
这将返回hango-ui的Service的URL，例如 http://192.168.49.2:30768，复制 URL 并在 Web 浏览器中打开它即可访问前端页面
### 其他方式安装hango网关

1、请参考脚本内容配置hango的k8s资源

### 卸载hango网关

1、进入"hango-gateway/install"目录下

2、执行命令运行脚本卸载hango网关

注意：在脚本执行前请确保权限足够
```shell
sh uninstall.sh
```
3、等待脚本执行完毕后，可以通过执行下面的命令查看hango网关的运行状态
```shell
sh check.sh
```
卸载完成后，命名空间和其下的容器将全部删除，正常状态如下，若仍然存于未删除资源，您可以再次执行uninstall.sh脚本或手动删除资源
```shell
[install-check][14:56:29]
========= pods in namespace[hango-system] show below =========
No resources found in hango-system namespace.
```