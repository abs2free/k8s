# helm 使用指南

## 安装helm

```sh
$ brew install helm
```

## helm 仓库
## helm search 查找charts

1. 从 Artifact Hub 中查找并列出 helm charts。 Artifact Hub中存放了大量不同的仓库。
```sh
helm search hub
```

2. 从你添加（使用 helm repo add）到本地 helm 客户端中的仓库中进行查找。该命令基于本地数据进行搜索，无需连接互联网。
```sh
helm search repo
```

```sh
$ helm search hub wordpress
URL                                                 CHART VERSION APP VERSION DESCRIPTION
https://hub.helm.sh/charts/bitnami/wordpress        7.6.7         5.2.4       Web publishing platform for building blogs and ...
https://hub.helm.sh/charts/presslabs/wordpress-...  v0.6.3        v0.6.3      Presslabs WordPress Operator Helm Chart
https://hub.helm.sh/charts/presslabs/wordpress-...  v0.7.1        v0.7.1      A Helm chart for deploying a WordPress site on ...
```
### helm repo 的使用
1. 查看
```sh
helm repo list
```

2. 添加
```sh
helm repo add
```
3. 移除
```sh
helm repo remove
```

## helm 包管理
### helm install 安装一个helm包

```sh
$ helm install happy-panda bitnami/wordpress
NAME: happy-panda
LAST DEPLOYED: Tue Jan 26 10:27:17 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
** Please be patient while the chart is being deployed **
[..]
```

#### 更多安装方法
1. chart 的仓库（如上所述）
2. 本地 chart 压缩包（`helm install foo foo-0.1.1.tgz`）
3. 解压后的 chart 目录（`helm install foo path/to/foo`）
4. 完整的 URL（`helm install foo https://example.com/charts/foo-1.2.3.tgz`）



### helm status 查看release的状态

```sh
$ helm status happy-panda
NAME: happy-panda
LAST DEPLOYED: Tue Jan 26 10:27:17 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
** Please be patient while the chart is being deployed **
[..]
```

### 升级到 chart 的新版本，或是修改 release 的配置
```sh
helm upgrade -f panda.yaml happy-panda bitnami/wordpress
```

参数
- --timeout
- --wait表示必须要等到所有的 Pods 都处于 ready 状态，PVC 都被绑定，Deployments 都至少拥有最小 ready 状态 Pods 个数（Desired减去 maxUnavailable），并且 Services 都具有 IP 地址（如果是LoadBalancer， 则为 Ingress），才会标记该 release 为成功  
- --recreate-pods

### helm uninstall 卸载release

```sh
helm uninstall panda
```
如果你想保留删除记录，使用 helm uninstall --keep-history


### 安装自定义的chart

1. helm show values 查看 chart 中的可配置选项

```sh
$ helm show values bitnami/wordpress
## Global Docker image parameters
## Please, note that this will override the image parameters, including dependencies, configured to use the global value
## Current available global Docker image parameters: imageRegistry and imagePullSecrets
##
# global:
#   imageRegistry: myRegistryName
#   imagePullSecrets:
#     - myRegistryKeySecretName
#   storageClass: myStorageClass

## Bitnami WordPress image version
## ref: https://hub.docker.com/r/bitnami/wordpress/tags/
##
image:
  registry: docker.io
  repository: bitnami/wordpress
  tag: 5.6.0-debian-10-r35
  [..]
```

2. 使用 YAML 格式的文件覆盖上述任意配置项

```sh
$ echo '{mariadb.auth.database: user0db, mariadb.auth.username: user0}' > values.yaml
$ helm install -f values.yaml bitnami/wordpress --generate-name
```

> 安装过程中有两种方式传递配置数据：

- --values (或 -f)：使用 YAML 文件覆盖配置。可以指定多次，优先使用最右边的文件。
- --set：通过命令行的方式对指定项进行覆盖。



## 创建自己的chart

### helm create 快速创建
```sh
helm create deis-workflow

```
在编辑 chart 时，可以通过 helm lint 验证格式是否正确。

### helm package 打包分发
```sh
$ helm package deis-workflow
deis-workflow-0.1.0.tgz
```
### helm install 安装
```sh
helm install deis-workflow ./deis-workflow-0.1.0.tgz
```
