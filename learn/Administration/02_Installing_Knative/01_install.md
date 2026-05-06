# Installing Knative

* https://knative.dev/docs/install/

This page provides guidance for Kubernetes administrators on how to install Knative on an existing Kubernetes cluster. Knative has three components: Eventing, Serving, and Functions. Serving and Eventing are installed into clusters. Functions is not installed into clusters given that the client `func` CLI tool builds and deploys stateless functions as standard containers.
本页面为 Kubernetes 管理员提供在现有 Kubernetes 集群上安装 Knative 的指南. Knative 包含三个组件: 事件处理 (Eventing)、服务 (Serving) 和函数 (Functions). 服务和事件处理组件会安装到集群中. 由于客户端 `func` CLI 工具会将无状态函数构建并部署为标准容器, 因此函数组件不会安装到集群中.

A Knative installation assumes you are familiar with the following:
Knative 安装假定您熟悉以下内容:

- Kubernetes and Kubernetes administration.

- The `kubectl` CLI tool. You can use existing Kubernetes management tools (policy, quota, etc) to manage Knative workloads.

- Using `cluster-admin` permissions or equivalent to install software and manage resources in all clusters in the namespace. For information about permissions, see [Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/AC).
  使用 `cluster-admin` 权限或等效权限, 可以在命名空间中的所有集群中安装软件和管理资源. 有关权限的信息, 请参阅 ["使用基于角色的访问控制 (RBAC) 授权"](https://kubernetes.io/docs/reference/access-authn-authz/rbac/AC).

- Familiarity is recommended with Cloud Native Computing Foundation (CNCF) projects such as [Prometheus](https://kubernetes.io/docs/concepts/cluster-administration/system-metrics/), [Istio](https://istio.io/), and [Strimzi](https://strimzi.io/), many of which can be used alongside Knative.
  建议熟悉云原生计算基金会 (CNCF) 项目, 例如 [Prometheus](https://kubernetes.io/docs/concepts/cluster-administration/system-metrics/)、[Istio](https://istio.io/) 和 [Strimzi](https://strimzi.io/), 其中许多项目可以与 Knative 一起使用.

You can install the Serving and Eventing components independently of one another. You can also add and remove plugins at any time, as well as optional integration tools that span observability, security, and testing. Plugins are described in the [Extensibility](https://knative.dev/docs/install/#extensibility) section.
您可以分别安装 Serving 和 Eventing 组件. 您还可以随时添加和移除插件, 以及涵盖可观测性、安全性和测试的可选集成工具. 插件的详细说明请参见 ["可扩展性"](https://knative.dev/docs/install/#extensibility) 部分.

## Installation roadmap

Use the following table to determine your installation method. If you just want to get an understanding of Knative functionality at this time, install the quickstart.
请使用下表确定您的安装方法. 如果您目前只想了解 Knative 的功能, 请安装快速入门版.

|                       | Quickstart                         | YAML-based                                                                                                                      | Knative Operator   |
| --------------------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| Purpose               | local                              | production                                                                                                                      | production         |
| Kubernetes deployment | local, either `kind` or `Minikube` | existing                                                                                                                        | existing           |
| Hardware              | 3 CPU, 3 GB RAM                    | One node: <br> 6 CPUs, 6 GB memory, 30 GB disk storage. <br> Multiple nodes: <br> 2 CPUs each, 4 GB memory, 20 GB disk storage. | same as YAML-based |

Supported platforms are Linux, MacOS, and Windows.
支持的平台有 Linux、MacOS 和 Windows.

The Knative Operator is a custom controller that extends the Kubernetes API to install Knative components. For more information about YAML-based and installations with the Knative Operator, see [YAML and Knative Operator installations compared](https://knative.dev/docs/install/#yaml-and-knative-operator-installations-compared).
Knative Operator 是一个自定义控制器, 它扩展了 Kubernetes API 以安装 Knative 组件. 有关基于 YAML 的安装以及使用 Knative Operator 的安装的更多信息, 请参阅 [YAML 和 Knative Operator 安装对比](https://knative.dev/docs/install/#yaml-and-knative-operator-installations-compared).

Use the following steps to install Knative depending on your installation method:
根据您的安装方式, 按照以下步骤安装 Knative:

**Quickstart**:

1. Install the [CLI Tools](https://knative.dev/docs/client/install-kn/).

2. Install the [Knative Quickstart plugin](https://knative.dev/docs/getting-started/quickstart-install/).

**YAML-based**:

Install using all YAML files. This option is the most useful if you're using GitOps tools such as [Flux](https://fluxcd.io/) or [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) to apply manifests checked into a Git repository. This is the lowest common denominator approach, giving you granular control of the process and resource definitions.
使用所有 YAML 文件进行安装. 如果您使用 [Flux](https://fluxcd.io/) 或 [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) 等 GitOps 工具将清单文件提交到 Git 仓库, 则此选项最为有用. 这是最基本的安装方式, 可让您对流程和资源定义进行精细控制.

1. Install the [CLI Tools](https://knative.dev/docs/client/install-kn/).

2. Install either or both components:

  - Install [Knative Serving](https://knative.dev/docs/install/yaml-install/serving/install-serving-with-yaml/).

  - Install [Knative Eventing](https://knative.dev/docs/install/yaml-install/eventing/install-eventing-with-yaml/).

**Knative Operator**:

Install using the Knative Operator as using manifests or [Helm](https://helm.sh/), or install using the Knative Operator that is installed with the Knative Operator CLI plugin.
可以使用清单或 [Helm](https://helm.sh/) 通过 Knative Operator 进行安装, 也可以使用随 Knative Operator CLI 插件一起安装的 Knative Operator 进行安装.

1. Install the [CLI Tools](https://knative.dev/docs/client/install-kn/) including the Knative Operator CLI plugin.

2. Install the Serving and Eventing components, by either of the following:

  - The [Knative Operator](https://knative.dev/docs/install/operator/knative-with-operators/) using Kubernetes manifests or by using Helm.

  - The [Knative Operator CLI](https://knative.dev/docs/install/operator/knative-with-operator-cli/).

All installations require a supported Kubernetes version. System requirements provided are recommendations only. The requirements for your installation may vary depending on which plugin components you use.
所有安装都需要受支持的 Kubernetes 版本. 所提供的系统要求仅供参考. 您的安装要求可能因您使用的插件组件而异.

For a list of commercial Knative products, see [Knative offerings](https://knative.dev/docs/install/knative-offerings/).
有关商业 Knative 产品的列表, 请参阅 [Knative 产品](https://knative.dev/docs/install/knative-offerings/).

## YAML and Knative Operator installations compared
YAML 和 Knative Operator 安装方式的比较

You install Knative using YAML files and other resources either aided or not by the Knative Operator. The Knative Operator allows you to automate applying, patching, and customizing the content.
您可以使用 YAML 文件和其他资源安装 Knative, 安装过程可以借助 Knative Operator, 也可以不借助它. Knative Operator 允许您自动应用、修补和自定义内容.

The Knative Operator alleviates installation complexities and is compatible with a GitOps approach. It also gives you a separation of the core Knative application definition and the ConfigMap and other changes you make. You install the Knative Operator either by using the Knative CLI Operator Plugin or by using Kubernetes Manifests or by Helm.
Knative Operator 简化了安装过程, 并与 GitOps 方法兼容. 它还能将核心 Knative 应用程序定义与 ConfigMap 和其他更改分离. 您可以通过 Knative CLI Operator 插件、Kubernetes Manifests 或 Helm 安装 Knative Operator.

Here are the considerations for installing using YAML or the Knative Operator:
以下是使用 YAML 或 Knative Operator 进行安装时需要考虑的事项:

| YAML-based install                                                                                                                                 | Knative Operator install                                                                                       |
| -------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| You can see exactly what you get. <br> 您可以清楚地看到您将获得什么.                                                                               | You specify choices at a higher level. <br> 您可以在更高层级指定选择.                                          |
| You can adjust any parameters by editing them directly. <br> 您可以直接编辑参数来调整任何参数.                                                     | Not every setting is exposed. <br> 并非所有设置都会公开显示.                                                   |
| If you make changes, you have to keep track of what you changed when you want to upgrade. <br> 如果你做了更改, 升级时就必须记录下你更改了哪些内容. | It's easy to separate your customizations from the base installation. <br> 将自定义设置与基础安装分离非常容易. |
| Version and audit control as YAML files are stored in a GitHub repository. <br> 版本控制和审计控制以 YAML 文件的形式存储在 GitHub 存储库中.        | Manage custom resources using command-line tools or manifests. <br> 使用命令行工具或清单管理自定义资源.        |

## Extensibility
可扩展性

Knative utilizes the existing infrastructure installed on your cluster and provides a developer-facing interface between similar components. The Serving and Eventing components support multiple underlying transports plugins within the same cluster. Serving supports pods with pluggable network ingress routes; and Eventing supports pods with pluggable message transports such as Kafka and RabbitMQ.
Knative 利用集群上已安装的基础架构, 并在类似组件之间提供面向开发者的接口. Serving 和 Eventing 组件支持同一集群内使用多种底层传输插件. Serving 支持具有可插拔网络入口路由的 Pod; Eventing 支持具有可插拔消息传输(例如 Kafka 和 RabbitMQ)的 Pod.

For LLM deployments, consider the [KServe](https://kserve.github.io/) platform. KServe is a Kubernetes-native model serving platform built on Knative Serving designed for production LLM deployments.
对于 LLM 部署, 可以考虑使用 [KServe](https://kserve.github.io/) 平台. KServe 是一个基于 Knative Serving 构建的 Kubernetes 原生模型服务平台, 专为生产环境的 LLM 部署而设计.

Knative supports installing additional plugins after the initial installation, so your initial choices don't lock you in. For example, you can migrate from one message transport or network ingress to another without losing messages.
Knative 支持在初始安装后安装其他插件, 因此您最初的选择不会限制您的使用. 例如, 您可以从一种消息传输或网络入口迁移到另一种, 而不会丢失消息.

### Networking plugins

If you don't have an ingress that meets the requirements, Knative provides [net-kourier](https://github.com/knative-extensions/net-kourier), a default lightweight HTTP routing implementation. Plugins include:
如果您没有符合要求的入口, Knative 提供了 [net-kourier](https://github.com/knative-extensions/net-kourier), 这是一个默认的轻量级 HTTP 路由实现. 插件包括:

- Istio

  Service mesh from [Istio](https://istio.io/). See [Installing Istio for Knative](https://knative.dev/docs/install/installing-istio/).

- Contour

  General-purpose ingress with a goal of enabling multi-team delegation. See [Contour](https://projectcontour.io/).

- Gateway API

  The Kubernetes [Gateway API](https://kubernetes.io/docs/concepts/services-networking/gateway/) (beta).

- Higress (Third-party integration)

  [Higress](https://higress.cn/en/) provides enhanced traffic governance, authentication, WAF protection, and observability for Knative services with WASM plugin extensibility. See [Higress: A Best Practice for Knative Ingress Gateway](https://www.alibabacloud.com/blog/higress-a-best-practice-for-knative-ingress-gateway_600549).
  [Higress](https://higress.cn/en/) 为 Knative 服务提供增强的流量治理、身份验证、WAF 保护和可观测性, 并支持 WASM 插件扩展. 请参阅 [《Higress: Knative Ingress 网关最佳实践》](https://www.alibabacloud.com/blog/higress-a-best-practice-for-knative-ingress-gateway_600549).

  > Note
  > This integration is not managed by the Knative community.
  > 此集成并非由 Knative 社区管理.

### Messaging plugins

Knative has default lightweight in-memory messaging implementation if you don't already have a solution.
如果您还没有解决方案, Knative 提供了一个默认的轻量级内存消息传递实现.

- Kafka

  Distributed event streaming platform from [Apache Kafka](https://kafka.apache.org/). In-order, high-thoughput but moderate complexity. See [Install Kafka for Knative](https://knative.dev/docs/install/eventing/kafka-install/).
  [Apache Kafka](https://kafka.apache.org/) 提供的分布式事件流平台. 支持序贯传输, 吞吐量高, 但复杂度适中. 请参阅["为 Knative 安装 Kafka"](https://knative.dev/docs/install/eventing/kafka-install/).

- RabbitMQ

  A messaging and streaming broker from [RabbitMQ](https://www.rabbitmq.com/). In-order, moderate throughput and complexity. See [Install RabbitMQ for Knative](https://knative.dev/docs/install/eventing/rabbitmq-install/)
  [RabbitMQ](https://www.rabbitmq.com/) 提供消息和流代理功能. 支持顺序传输, 吞吐量适中, 复杂度也较低. 请参阅 ["为 Knative 安装 RabbitMQ".](https://knative.dev/docs/install/eventing/rabbitmq-install/)

- NATS

  An event streaming platform from [NATS](https://nats.io/). Low complexity.

### Integration plugins

These plugins facilitate Knative operations.
这些插件可以简化 Knative 的操作.

- cert-manager

  For requesting TLS certificates in secure HTTPS connections. See [Install cert-manager](https://knative.dev/docs/install/installing-cert-manager/).

- Backstage

  Plugins for handling Knative backends. See [Installing backstage plugins](https://knative.dev/docs/install/installing-backstage-plugins/).
  用于处理 Knative 后端的插件. 请参阅 ["安装后台插件"](https://knative.dev/docs/install/installing-backstage-plugins/).

## Installation resources

Use the following links to maintain your installations.
使用以下链接来维护您的安装.

- [Upgrading Knative](https://knative.dev/docs/install/upgrade/)

- [Uninstall Knative](https://knative.dev/docs/install/uninstall/)

- [Check Knative version](https://knative.dev/docs/install/upgrade/check-install-version/)

- [Troubleshoot Knative installations](https://knative.dev/docs/install/troubleshooting/)
