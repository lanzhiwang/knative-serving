# Installing Knative Serving using YAML files
使用 YAML 文件安装 Knative Serving

* https://knative.dev/docs/install/yaml-install/serving/install-serving-with-yaml/

This topic describes how to install Knative Serving by applying YAML files. This installation requires the following prerequisites:
本主题介绍如何通过应用 YAML 文件来安装 Knative Serving. 此安装需要以下先决条件:

- The [CLI Tools](https://knative.dev/docs/client/install-kn/) are installed.

- Sufficient hardware:
  硬件充足

One node requires at least 6 CPUs, 6 GB of memory, and 30 GB of disk storage.
一个节点至少需要 6 个 CPU、6 GB 内存和 30 GB 磁盘存储空间.

Multiple nodes require 2 CPUs, 4 GB of memory, and 20 GB of disk storage.
多节点需要 2 个 CPU、4 GB 内存和 20 GB 磁盘存储空间.

- The existing Kubernetes is running a supported version.
  现有的 Kubernetes 运行的是受支持的版本.

For information on other Knative installs, see the [Installation Roadmap](https://knative.dev/docs/install/#installation-roadmap).
有关其他 Knative 安装的信息, 请参阅[安装路线图](https://knative.dev/docs/install/#installation-roadmap).

## Install the Knative Serving component
安装 Knative Serving 组件

To install the Knative Serving component:
安装 Knative Serving 组件:

1. Install the required custom resources by running the command:
  运行以下命令安装所需的自定义资源:

```bash
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.22.0/serving-crds.yaml
```

2. Install the core components of Knative Serving by running the command:
  运行以下命令安装 Knative Serving 的核心组件:

```bash
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.22.0/serving-core.yaml
```

  > Info
  > For information about the YAML files in Knative Serving, see [Knative Serving installation files](https://knative.dev/docs/install/yaml-install/serving/serving-installation-files/).
  > 有关 Knative Serving 中的 YAML 文件的信息, 请参阅 [Knative Serving 安装文件](https://knative.dev/docs/install/yaml-install/serving/serving-installation-files/).
  >

## Install a networking layer
安装网络层

Expand the following tabs for instructions on installing network layers. For an overview of network layer options, architecture, and configurations see the [ingress providers](https://knative.dev/docs/serving/config-network-adapters/#ingress-providers) on the [Configure Knative Networking](https://knative.dev/docs/serving/config-network-adapters/) page.
展开以下选项卡, 查看有关安装网络层的说明. 有关网络层选项、架构和配置的概述, 请参阅 ["配置 Knative 网络"](https://knative.dev/docs/serving/config-network-adapters/) 页面上的[入口提供程序](https://knative.dev/docs/serving/config-network-adapters/#ingress-providers).

### Kourier

### Contour

### Istio

Use the following steps to install Istio and set it as the ingress controller.
按照以下步骤安装 Istio 并将其设置为入口控制器.

1. Install a properly configured Istio:
  安装配置正确的 Istio:

```bash
kubectl apply -l knative.dev/crd-install=true -f https://github.com/knative-extensions/net-istio/releases/download/knative-v1.22.0/istio.yaml
kubectl apply -f https://github.com/knative-extensions/net-istio/releases/download/knative-v1.22.0/istio.yaml
```

2. Install the Knative Istio controller:
  安装 Knative Istio 控制器:

```bash
kubectl apply -f https://github.com/knative-extensions/net-istio/releases/download/knative-v1.22.0/net-istio.yaml
```

3. Configure the `config-network` ConfigMap to use Istio:
  配置 `config-network` ConfigMap 以使用 Istio:

```bash
kubectl patch configmap/config-network \
--namespace knative-serving \
--type merge \
--patch '{"data":{"ingress-class":"istio.ingress.networking.knative.dev"}}'
```

4. Get the external IP address (FQDN) to later configure DNS:
  获取外部 IP 地址(FQDN), 以便稍后配置 DNS:

```bash
kubectl --namespace istio-system get service istio-ingressgateway
```

### Gateway API

## Verify the installation
验证安装情况

Monitor the Knative components until all of the components show a `STATUS` of `Running` or `Completed`. You can do this by running the following command and inspecting the output:
监控 Knative 组件, 直到所有组件的 `STATUS` 都显示为 `Running` 或 `Completed`. 您可以通过运行以下命令并检查输出来完成此操作:

```bash
kubectl get pods -n knative-serving
```

Example output:

```bash
NAME                                      READY   STATUS    RESTARTS   AGE
3scale-kourier-control-54cc54cc58-mmdgq   1/1     Running   0          81s
activator-67656dcbbb-8mftq                1/1     Running   0          97s
autoscaler-df6856b64-5h4lc                1/1     Running   0          97s
controller-788796f49d-4x6pm               1/1     Running   0          97s
webhook-859796bc7-8n5g2                   1/1     Running   0          96s
```

## Configure DNS
配置 DNS

You can configure DNS to avoid specifying the host header in curl commands, or to access the content with a web browser.
您可以配置 DNS 以避免在 curl 命令中指定主机头, 或者使用 Web 浏览器访问内容.

The following tabs show instructions for configuring DNS. Follow the procedure for the DNS of your choice.
以下标签页显示了配置 DNS 的说明. 请按照您选择的 DNS 服务器对应的步骤进行操作.

### Magic DNS (sslip.io)

Knative provides a Kubernetes Job called `default-domain` that configures Knative Serving to use [sslip.io](http://sslip.io/) as the default DNS suffix.
Knative 提供了一个名为 `default-domain` Kubernetes 作业, 用于配置 Knative Serving 使用 [sslip.io](http://sslip.io/) 作为默认 DNS 后缀.

```bash
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.22.0/serving-default-domain.yaml
```

This configuration works only if the cluster `LoadBalancer` Service exposes an IPv4 address or hostname. It does not work with IPv6 clusters or local setups such as minikube unless the [`minikube tunnel`](https://minikube.sigs.k8s.io/docs/commands/tunnel/) is running.
此配置仅在集群 `LoadBalancer` 服务暴露 IPv4 地址或主机名时才有效. 它不适用于 IPv6 集群或本地设置(例如 minikube), 除非 [`minikube tunnel`](https://minikube.sigs.k8s.io/docs/commands/tunnel/) 正在运行.

### Real DNS

To configure DNS for Knative, take the External IP or CNAME from setting up networking, and configure it with your DNS provider as follows:
要为 Knative 配置 DNS, 请从网络设置中获取外部 IP 或 CNAME, 并按如下方式将其配置到您的 DNS 提供商处:

- If the networking layer produced an External IP address, then configure a wildcard `A` record for the domain. In the following example, `knative.example.com` is the domain suffix for a cluster.
  如果网络层生成了外部 IP 地址, 则需要为该域配置通配符 `A` 记录. 在以下示例中, `knative.example.com` 是集群的域后缀.

```bash
*.knative.example.com == A 35.233.41.212
```

- If the networking layer produced a CNAME, then configure a CNAME record for the domain. In the following example, `knative.example.com` is the domain suffix for a cluster.
  如果网络层生成了 CNAME 记录, 则需要为该域配置 CNAME 记录. 在以下示例中, `knative.example.com` 是集群的域后缀.

```bash
*.knative.example.com == CNAME a317a278525d111e89f272a164fd35fb-1510370581.eu-central-1.elb.amazonaws.com
```

- After your DNS provider has been configured, direct Knative to use that domain:
  DNS 提供商配置完成后, 指示 Knative 使用该域名:

```bash
# Replace knative.example.com with your domain suffix
kubectl patch configmap/config-domain \
  --namespace knative-serving \
  --type merge \
  --patch '{"data":{"knative.example.com":""}}'
```

### No DNS

If you are using `curl` to access [the sample applications](https://knative.dev/docs/getting-started/first-service/), or your own Knative app, and are unable to use the "Magic DNS (sslip.io)" or "Real DNS" methods, you can evaluate Knative without altering your DNS configuration. You might have this need if minikube locally or IPv6 clusters.
如果您使用 `curl` 访问[示例应用程序](https://knative.dev/docs/getting-started/first-service/)或您自己的 Knative 应用程序, 并且无法使用"Magic DNS (sslip.io)"或"Real DNS"方法, 则可以在不更改 DNS 配置的情况下评估 Knative. 如果您在本地使用 minikube 或 IPv6 集群, 则可能需要这样做.

To access your application using `curl` using this method:
要使用 `curl` 通过以下方法访问您的应用程序:

1. Configure Knative to use a domain reachable from outside the cluster:
  配置 Knative 使用可从集群外部访问的域:

```bash
kubectl patch configmap/config-domain \
      --namespace knative-serving \
      --type merge \
      --patch '{"data":{"example.com":""}}'
```

2. After starting your application, get the URL of your application:
  启动应用程序后, 获取应用程序的 URL:

```bash
kubectl get ksvc
```

The output should be similar to:
输出结果应类似于:

```bash
NAME            URL                                        LATESTCREATED         LATESTREADY           READY   REASON
helloworld-go   http://helloworld-go.default.example.com   helloworld-go-vqjlf   helloworld-go-vqjlf   True
```

2. Instruct `curl` to connect to the External IP or CNAME defined by the networking layer mentioned in section 3, and use the `-H "Host:"` command-line option to specify the Knative application's host name. For example, if the networking layer defines your External IP and port to be `http://192.168.39.228:32198` and you wish to access the `helloworld-go` application mentioned earlier, use:

```bash
curl -H "Host: helloworld-go.default.example.com" http://192.168.39.228:32198
```

  In the case of the provided `helloworld-go` sample application, using the default configuration, the output is:
  对于提供的 `helloworld-go` 示例应用程序, 使用默认配置时, 输出结果为:

```
Hello Go Sample v1!
```

  Refer to the "Real DNS" method for a permanent solution.
  请参考"真实 DNS"方法以获得永久解决方案.

## Install optional Serving extensions
安装可选的服务器扩展

The following tabs expand to show instructions for installing each Serving extension.
以下标签页展开后会显示每个 Serving 扩展的安装说明.

### HPA autoscaling

Knative also supports the use of the Kubernetes Horizontal Pod Autoscaler (HPA) for driving autoscaling decisions.
Knative 还支持使用 Kubernetes Horizo​​ntal Pod Autoscaler (HPA) 来驱动自动扩缩容决策.

- Install the components needed to support HPA-class autoscaling by running the command:
  运行以下命令安装支持 HPA 级自动扩缩容所需的组件:

```bash
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.22.0/serving-hpa.yaml
```

### Knative encryption with cert-manager

Knative supports encryption features through [cert-manager](https://cert-manager.io/docs/). Follow the documentation in [Serving encryption](https://knative.dev/docs/serving/encryption/encryption-overview/) for more information.
Knative 通过 [cert-manager](https://cert-manager.io/docs/) 支持加密功能. 请参阅 ["提供加密服务"](https://knative.dev/docs/serving/encryption/encryption-overview/) 文档了解更多信息.
