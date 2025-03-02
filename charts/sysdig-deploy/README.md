# Sysdig

---
**WARNING**

This chart is still experimental and may have issues! Please use the [supported chart](https://github.com/sysdiglabs/charts/tree/master/charts/sysdig) for production deployments.

---

[Sysdig](https://sysdig.com/) is a unified platform for container and microservices monitoring, troubleshooting,
security and forensics. Sysdig platform has been built on top of [Sysdig tool](https://sysdig.com/opensource/sysdig/)
and [Sysdig Inspect](https://sysdig.com/blog/sysdig-inspect/) open-source technologies.

## Introduction

This chart deploys various Sysdig components into your Kubernetes cluster.

Currently included components:
- [Sysdig agent](https://github.com/sysdiglabs/charts/tree/master/charts/agent)

## Prerequisites

- Kubernetes 1.9+ with Beta APIs enabled
- Helm v3+

## Installation

Add the Sysdig Helm repo:

```bash
helm repo add sysdig https://charts.sysdig.com/
```

You will need to have the following values ready:
- ACCESS_KEY: This is your Sysdig access key
- SAAS_REGION: The Sysdig SAAS region the agents will connect to
- CLUSTER_NAME: An identifier for your cluster

Using the release name `sysdig`, run the following snippet to install the release into the namespace `sysdig-agent` (ensure that the namespace has been created before this):

```bash
helm install sysdig sysdig/sysdig-deploy \
    --namespace sysdig-agent \
    --set global.sysdig.accessKey=ACCESS_KEY \
    --set global.sysdig.region=SAAS_REGION \
    --set global.clusterConfig.name=CLUSTER_NAME
```

Or to install with a values file, create a new file `values.sysdig.yaml`:

```yaml
global:
  sysdig:
    accessKey: ACCESS_KEY
    region: SAAS_REGION
  clusterConfig:
    name: CLUSTER_NAME
```

and install it with:

```bash
helm install sysdig sysdig/sysdig-deploy -f values.sysdig.yaml
```

Further configuration information can be found below.

## Configuration

| Parameter                        | Description                                                       | Default        |
| -------------------------------- | ----------------------------------------------------------------- | -------------- |
| `global.clusterConfig.name`      | Identifier for this cluster                                       | `""`           |
| `global.sysdig.accessKey`        | Sysdig Agent Access Key                                           | `""`           |
| `global.sysdig.accessKeySecret`  | The name of a Kubernetes secret containing an 'access-key' entry. | `""`           |
| `global.sysdig.region`           | The SaaS region for these agents                                  | `"us1"`        |
| `global.image.registry`          | Container image registry                                          | `quay.io`      |
| `global.proxy.httpProxy`         | Sets `http_proxy` on the Agent container                          | `""`           |
| `global.proxy.httpsProxy`        | Sets `https_proxy` on the Agent container                         | `""`           |
| `global.proxy.noProxy`           | Sets `no_proxy` on the Agent container                            | `""`           |
| `agent`                          | Config specific to the [Sysdig Agent](#agent)                     | `{}`           |
| `agent.enabled`                  | Enable the agent component in this chart                          | `true`         |
| `nodeAnalyzer`                   | Config specific to the [Sysdig nodeAnalyzer](#nodeAnalyzer)       | `{}`           |
| `nodeAnalyzer.enabled`           | Enable the nodeAnalyzer component in this chart                   | `true`         |
| `nodeAnalyzer.nodeAnalyzer.apiEndpoint`           | nodeAnalyzer apiEndpoint                         | `""`           |
| `cspmCollector.enabled`          | Enable the cspmCollector component in this chart                  | `false`        |
| `cspmCollector.apiEndpoint`      | cspmCollector apiEndpoint                                         | `""`           |
## Agent

For possible configuration values of the Agent, please refer to the Agent subchart [README](https://github.com/sysdiglabs/charts/tree/master/charts/agent/README.md). All agent-specific configuration can be prefixed with `agent.` to apply them to this chart.

Example: override proxy variable for Agent chart

As a command line parameter:
```bash
helm install sysdig sysdig/sysdig-deploy \
    --namespace sysdig-agent \
    --set global.sysdig.accessKey=ACCESS_KEY \
    --set global.sysdig.region=SAAS_REGION \
    --set global.clusterConfig.name=CLUSTER_NAME \
    --set global.proxy.httpProxy=PROXY_URL \
    --set agent.proxy.httpProxy=OVERRIDE_PROXY_URL
```

As a values file:
```yaml
global:
  sysdig:
    accessKey: ACCESS_KEY
    region: SAAS_REGION
  clusterConfig:
    name: CLUSTER_NAME
  proxy:
    httpProxy: PROXY_URL

agent:
  proxy:
    httpProxy: OVERRIDE_PROXY_URL
```

## NodeAnalyzer

For possible configuration values of the node-analyzer, please refer to the node-analyzer subchart [README](https://github.com/sysdiglabs/charts/blob/master/charts/node-analyzer/README.md). All agent-specific configuration can be prefixed with `nodeAnalyzer.` to apply them to this chart.

Example: override apiEndpoint variable for nodeAnalyzer chart

As a command line parameter:
```bash
helm install sysdig sysdig/sysdig-deploy \
    --set global.sysdig.accessKey=ACCESS_KEY \
    --set agent.sysdig.settings.collector=COLLECTOR_ENDPOINT \
    --set nodeAnalyzer.nodeAnalyzer.apiEndpoint=API_ENDPOINT
```

As a values file:
```yaml
global:
  sysdig:
    accessKey: ACCESS_KEY

agent:
  enabled: false

nodeAnalyzer:
  nodeAnalyzer:
    apiEndpoint: API_ENDPOINT
```

## cspmCollector

For possible configuration values of the cspm-collector, please refer to the cspm-collector subchart [README](https://github.com/sysdiglabs/charts/blob/master/charts/cspm-collector/README.md). All agent-specific configuration can be prefixed with `cspmCollector.` to apply them to this chart.

Example: override apiEndpoint variable for cspmCollector chart

As a command line parameter:
```bash
helm install sysdig sysdig/sysdig-deploy \
    --set global.sysdig.accessKey=ACCESS_KEY \
    --set cspmCollector.apiEndpoint=API_ENDPOINT
```

As a values file:
```yaml
global:
  sysdig:
    accessKey: ACCESS_KEY

cspmCollector:
  apiEndpoint: API_ENDPOINT
```
