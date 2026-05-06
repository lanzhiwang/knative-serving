# Installing Knative Serving using YAML files

## serving-crds.yaml

```bash
CustomResourceDefinition
  certificates.networking.internal.knative.dev
  configurations.serving.knative.dev
  clusterdomainclaims.networking.internal.knative.dev
  domainmappings.serving.knative.dev
  ingresses.networking.internal.knative.dev
  metrics.autoscaling.internal.knative.dev
  podautoscalers.autoscaling.internal.knative.dev
  revisions.serving.knative.dev
  routes.serving.knative.dev
  serverlessservices.networking.internal.knative.dev
  services.serving.knative.dev
  images.caching.internal.knative.dev

```

## serving-core.yaml

```bash
Namespace
  knative-serving

Role
  knative-serving-activator

ClusterRole
  knative-serving-activator-cluster
  knative-serving-aggregated-addressable-resolver
  knative-serving-addressable-resolver
  knative-serving-namespaced-admin
  knative-serving-namespaced-edit
  knative-serving-namespaced-view
  knative-serving-core
  knative-serving-podspecable-binding
  knative-serving-admin

ServiceAccount
  controller
  activator

ClusterRoleBinding
  knative-serving-controller-admin
  knative-serving-controller-addressable-resolver
  knative-serving-activator-cluster

RoleBinding
  knative-serving-activator

Certificate
  routing-serving-certs

Image
  queue-proxy

ConfigMap
  config-autoscaler
  config-certmanager
  config-defaults
  config-deployment
  config-domain
  config-features
  config-gc
  config-leader-election
  config-logging
  config-network
  config-observability
  config-tracing

HorizontalPodAutoscaler
  activator
  webhook

PodDisruptionBudget
  activator-pdb
  webhook-pdb

Deployment
  activator
  autoscaler
  controller
  webhook

Service
  activator-service
  autoscaler
  controller
  webhook

ValidatingWebhookConfiguration
  config.webhook.serving.knative.dev
  validation.webhook.serving.knative.dev

MutatingWebhookConfiguration
  webhook.serving.knative.dev

Secret
  webhook-certs

```

## istio.yaml

```bash
Namespace
  istio-system

CustomResourceDefinition
  gateways.networking.istio.io
  virtualservices.networking.istio.io
  destinationrules.networking.istio.io
  envoyfilters.networking.istio.io
  proxyconfigs.networking.istio.io
  serviceentries.networking.istio.io
  sidecars.networking.istio.io
  workloadentries.networking.istio.io
  workloadgroups.networking.istio.io
  peerauthentications.security.istio.io
  requestauthentications.security.istio.io
  authorizationpolicies.security.istio.io
  telemetries.telemetry.istio.io
  wasmplugins.extensions.istio.io

ServiceAccount
  istio-ingressgateway-service-account
  istio-reader-service-account
  istiod

ClusterRole
  istio-reader-clusterrole-istio-system
  istiod-clusterrole-istio-system
  istiod-gateway-controller-istio-system

ClusterRoleBinding
  istio-reader-clusterrole-istio-system
  istiod-clusterrole-istio-system
  istiod-gateway-controller-istio-system

ValidatingWebhookConfiguration
  istio-validator-istio-system

ConfigMap
  istio
  istio-sidecar-injector
  values

MutatingWebhookConfiguration
  istio-sidecar-injector

Deployment
  istio-ingressgateway
  istiod

PodDisruptionBudget
  istiod

Role
  istio-ingressgateway-sds
  istiod

RoleBinding
  istio-ingressgateway-sds
  istiod

HorizontalPodAutoscaler
  istiod

Service
  istio-ingressgateway
  istiod

```

## net-istio.yaml

```bash
ClusterRole
  knative-serving-istio

Gateway
  knative-ingress-gateway
  knative-local-gateway

Service
  knative-local-gateway
  net-istio-webhook

ConfigMap
  config-istio

PeerAuthentication
  webhook
  net-istio-webhook

Deployment
  net-istio-controller
  net-istio-webhook

Secret
  net-istio-webhook-certs

MutatingWebhookConfiguration
  webhook.istio.networking.internal.knative.dev

ValidatingWebhookConfiguration
  config.webhook.istio.networking.internal.knative.dev

```

## serving-default-domain.yaml

```bash
Job
  default-domain

Service
  default-domain-service

```

## serving-hpa.yaml

```bash
Deployment
  autoscaler-hpa

Service
  autoscaler-hpa

```

## serving-post-install-jobs.yaml

```bash
Job
  storage-version-migration-serving-
  cleanup-serving-

```

## serving-storage-version-migration.yaml

```bash
Job
  storage-version-migration-serving-

```
