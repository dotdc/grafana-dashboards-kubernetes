# grafana-dashboards-kubernetes <!-- omit in toc -->

![logo](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/kubernetes-grafana-dashboards-logo.png)

## Table of contents <!-- omit in toc -->

- [Description](#description)
- [Releases](#releases)
- [Features](#features)
- [Dashboards](#dashboards)
- [Installation](#installation)
  - [Install manually](#install-manually)
  - [Install via grafana.com](#install-via-grafanacom)
  - [Install with ArgoCD](#install-with-argocd)
  - [Install with Helm values](#install-with-helm-values)
  - [Install as ConfigMaps](#install-as-configmaps)
  - [Install as ConfigMaps with Terraform](#install-as-configmaps-with-terraform)
  - [Install as GrafanaDashboard with Grafana Operator](#install-as-grafanadashboard-with-grafana-operator)
- [Known issue(s)](#known-issues)
  - [Broken panels due to a too-high resolution](#broken-panels-due-to-a-too-high-resolution)
  - [Broken panels on k8s-views-nodes when a node changes its IP address](#broken-panels-on-k8s-views-nodes-when-a-node-changes-its-ip-address)
  - [Broken panels on k8s-views-nodes due to the nodename label](#broken-panels-on-k8s-views-nodes-due-to-the-nodename-label)
  - [Windows support](#windows-support)
- [Contributing](#contributing)

## Description

This repository contains a modern set of [Grafana](https://github.com/grafana/grafana) dashboards for [Kubernetes](https://github.com/kubernetes/kubernetes).\
They are inspired by many other dashboards from `kubernetes-mixin` and `grafana.com`.

More information about them in my article: [A set of modern Grafana dashboards for Kubernetes](https://0xdc.me/blog/a-set-of-modern-grafana-dashboards-for-kubernetes/)

You can also download them on [Grafana.com](https://grafana.com/grafana/dashboards/?plcmt=top-nav&cta=downloads&search=dotdc).

## Releases

This repository follows [semantic versioning](https://semver.org) for releases.\
It relies on [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) to automate releases using [semantic-release](https://github.com/semantic-release/semantic-release).

## Features

These dashboards are made and tested for the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) chart, but they should work well with others as soon as you have [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) and [prometheus-node-exporter](https://github.com/prometheus/node_exporter) installed on your Kubernetes cluster.

They are not backward compatible with older Grafana versions because they try to take advantage of Grafana's newest features like:

- `gradient mode` introduced in Grafana 8.1 ([Grafana Blog post](https://grafana.com/blog/2021/09/10/new-in-grafana-8.1-gradient-mode-for-time-series-visualizations-and-dynamic-panel-configuration/))
- `time series` visualization panel introduced in Grafana 7.4 ([Grafana Blog post](https://grafana.com/blog/2021/02/10/how-the-new-time-series-panel-brings-major-performance-improvements-and-new-visualization-features-to-grafana-7.4/))
- `$__rate_interval` variable introduced in Grafana 7.2 ([Grafana Blog post](https://grafana.com/blog/2020/09/28/new-in-grafana-7.2-__rate_interval-for-prometheus-rate-queries-that-just-work/))

They also have a `Prometheus Datasource` variable so they will work on a federated Grafana instance.

As an example, here's how the `Kubernetes / Views / Global` dashboard looks like:

![screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-global.png "Kubernetes Global View Screenshot")

## Dashboards

| File name                  | Description | Screenshot |
|:---------------------------|:------------|:----------:|
| k8s-addons-prometheus.json | Dashboard for Prometheus. | [Screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-addons-prometheus.png) |
| k8s-addons-trivy-operator.json | Dashboard for the Trivy Operator from Aqua Security. | [Screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-addons-trivy-operator.png) |
| k8s-system-api-server.json | Dashboard for the API Server Kubernetes component. | [Screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-system-api-server.png) |
| k8s-system-coredns.json    | Show information on the CoreDNS Kubernetes component. | [Screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-system-coredns.png) |
| k8s-views-global.json      | `Global` level view dashboard for Kubernetes. | [Screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-global.png) |
| k8s-views-namespaces.json  | `Namespaces` level view dashboard for Kubernetes. | [Screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-namespaces.png) |
| k8s-views-nodes.json       | `Nodes` level view dashboard for Kubernetes. | [Screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-nodes.png) |
| k8s-views-pods.json        | `Pods` level view dashboard for Kubernetes. | [Screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-pods.png) |

## Installation

In most cases, you will need to clone this repository (or your fork):

```terminal
git clone https://github.com/dotdc/grafana-dashboards-kubernetes.git
cd grafana-dashboards-kubernetes
```

If you plan to deploy these dashboards using [ArgoCD](#install-with-argocd), [ConfigMaps](#install-as-configmaps) or [Terraform](#install-as-configmaps-with-terraform), you will also need to enable and configure the `dashboards sidecar` on the Grafana Helm chart to get the dashboards loaded in your Grafana instance:

```yaml
# kube-prometheus-stack values
grafana:
  sidecar:
    dashboards:
      enabled: true
      defaultFolderName: "General"
      label: grafana_dashboard
      labelValue: "1"
      folderAnnotation: grafana_folder
      searchNamespace: ALL
      provider:
        foldersFromFilesStructure: true
```

### Install manually

On the WebUI of your Grafana instance, put your mouse over the `+` sign on the left menu, then click on `Import`.\
Once you are on the Import page, you can upload the JSON files one by one from your local copy using the `Upload JSON file` button.

### Install via grafana.com

On the WebUI of your Grafana instance, put your mouse over the `+` sign on the left menu, then click on `Import`.\
Once you are on the Import page, you can put the grafana.com dashboard ID (see table below) under `Import via grafana.com` then click on the `Load` button. Repeat for each dashboard.

Grafana.com dashboard id list:

| Dashboard                          | ID    |
|:-----------------------------------|:------|
| k8s-addons-prometheus.json         | 19105 |
| k8s-addons-trivy-operator.json     | 16337 |
| k8s-system-api-server.json         | 15761 |
| k8s-system-coredns.json            | 15762 |
| k8s-views-global.json              | 15757 |
| k8s-views-namespaces.json          | 15758 |
| k8s-views-nodes.json               | 15759 |
| k8s-views-pods.json                | 15760 |

### Install with ArgoCD

If you are using ArgoCD, this will deploy the dashboards in the default project of ArgoCD:

```terminal
kubectl apply -f argocd-app.yml
```

You will also need to enable and configure the Grafana `dashboards sidecar` as described in [Installation](#installation).

### Install with Helm values

If you use the official Grafana helm chart or kube-prometheus-stack, you can install the dashboards directly using the `dashboardProviders` & `dashboards` helm chart values.

Depending on your setup, add or merge the following block example to your helm chart values.\
The example is for [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack), for the official [Grafana helm chart](https://github.com/grafana/helm-charts/tree/main/charts/grafana), remove the first line (`grafana:`), and reduce the indentation level of the entire block.

```yaml
grafana:
  # Provision grafana-dashboards-kubernetes
  dashboardProviders:
    dashboardproviders.yaml:
      apiVersion: 1
      providers:
      - name: 'grafana-dashboards-kubernetes'
        orgId: 1
        folder: 'Kubernetes'
        type: file
        disableDeletion: true
        editable: true
        options:
          path: /var/lib/grafana/dashboards/grafana-dashboards-kubernetes
  dashboards:
    grafana-dashboards-kubernetes:
      k8s-system-api-server:
        url: https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-system-api-server.json
        token: ''
      k8s-system-coredns:
        url: https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-system-coredns.json
        token: ''
      k8s-views-global:
        url: https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-views-global.json
        token: ''
      k8s-views-namespaces:
        url: https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-views-namespaces.json
        token: ''
      k8s-views-nodes:
        url: https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-views-nodes.json
        token: ''
      k8s-views-pods:
        url: https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-views-pods.json
        token: ''
```

### Install as ConfigMaps

Grafana dashboards can be provisioned as Kubernetes ConfigMaps if you configure the [dashboard sidecar](https://github.com/grafana/helm-charts/blob/main/charts/grafana/values.yaml#L667) available on the official [Grafana Helm Chart](https://github.com/grafana/helm-charts/tree/main/charts/grafana).

To build the ConfigMaps and output them on STDOUT :

```terminal
kubectl kustomize .
```

*Note: no namespace is set by default, you can change that in the `kustomization.yaml` file.*

To build and deploy them directly on your Kubernetes cluster :

```terminal
kubectl apply -k . -n monitoring
```

You will also need to enable and configure the Grafana `dashboards sidecar` as described in [Installation](#installation).

*Note: you can change the namespace if needed.*

### Install as ConfigMaps with Terraform

If you use Terraform to provision your Kubernetes resources, you can convert the generated ConfigMaps to Terraform code using [tfk8s](https://github.com/jrhouston/tfk8s).

To build and convert ConfigMaps to Terraform code :

```terminal
kubectl kustomize . | tfk8s
```

You will also need to enable and configure the Grafana `dashboards sidecar` as described in [Installation](#installation).

*Note: no namespace is set by default, you can change that in the `kustomization.yaml` file.*

### Install as GrafanaDashboard with Grafana Operator

If you use Grafana Operator to provision your Grafana dashboards, you can use the following manifests:

Make sure to use your proper namespace.

```yaml
apiVersion: grafana.integreatly.org/v1beta1
kind: GrafanaDashboard
metadata:
  name: k8s-system-api-server
  namespace: monitoring
spec:
  instanceSelector:
    matchLabels:
      dashboards: "grafana"
  url: "https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-system-api-server.json"
---
apiVersion: grafana.integreatly.org/v1beta1
kind: GrafanaDashboard
metadata:
  name: k8s-system-coredns
  namespace: monitoring
spec:
  instanceSelector:
    matchLabels:
      dashboards: "grafana"
  url: "https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-system-coredns.json"
---
apiVersion: grafana.integreatly.org/v1beta1
kind: GrafanaDashboard
metadata:
  name: k8s-views-global
  namespace: monitoring
spec:
  instanceSelector:
    matchLabels:
      dashboards: "grafana"
  url: "https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-views-global.json"
---
apiVersion: grafana.integreatly.org/v1beta1
kind: GrafanaDashboard
metadata:
  name: k8s-views-namespaces
  namespace: monitoring
spec:
  instanceSelector:
    matchLabels:
      dashboards: "grafana"
  url: "https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-views-namespaces.json"
---
apiVersion: grafana.integreatly.org/v1beta1
kind: GrafanaDashboard
metadata:
  name: k8s-views-nodes
  namespace: monitoring
spec:
  instanceSelector:
    matchLabels:
      dashboards: "grafana"
  url: "https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-views-nodes.json"
---
apiVersion: grafana.integreatly.org/v1beta1
kind: GrafanaDashboard
metadata:
  name: k8s-views-pods
  namespace: monitoring
spec:
  instanceSelector:
    matchLabels:
      dashboards: "grafana"
  url: "https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/master/dashboards/k8s-views-pods.json"
```

## Known issue(s)

### Broken panels due to a too-high resolution

A user reported in [#50](https://github.com/dotdc/grafana-dashboards-kubernetes/issues/50) that some panels were broken because the default value of the `$resolution` variable was too low. The root cause hasn't been identified precisely, but he was using Grafana Agent & Grafana Mimir. Changing the `$resolution` variable to a higher value (a lower resolution) will likely solve the issue.
To make the fix permanent, you can configure the `Scrape interval` in your Grafana Datasource to a working value for your setup.

### Broken panels on k8s-views-nodes when a node changes its IP address

To make this dashboard more convenient, there's a small variable hack to display `node` instead of `instance`.
Because of that, some panels could lack data when a node changes its IP address as reported in [#102](https://github.com/dotdc/grafana-dashboards-kubernetes/issues/102).

No easy fix for this scenario yet, but it should be a corner case for most people.
Feel free to reopen the issue if you have ideas to fix this.

### Broken panels on k8s-views-nodes due to the nodename label

The `k8s-views-nodes` dashboard will have many broken panels if the `node` label from `kube_node_info` doesn't match the `nodename` label from `node_uname_info`.

This situation can happen on certain deployments of the node exporter running inside Kubernetes(e.g. via a `DaemonSet`), where `nodename` takes a different value than the node name as understood by the Kubernetes API.

Below are some ways to relabel the metric to force the `nodename` label to the appropriate value, depending on the way the collection agent is deployed:

#### Directly through the Prometheus configuration file <!-- omit in toc -->

Assuming the node exporter job is defined through `kubernetes_sd_config`, you can take advantage of the internal discovery labels and fix this by adding the following relabeling rule to the job:

```yaml
# File: prometheus.yaml
scrape_configs:
- job_name: node-exporter
  relabel_configs:
  # Add this
  - action: replace
    source_labels: [ __meta_kubernetes_pod_node_name]
    target_label: nodename
```

#### Through a `ServiceMonitor` <!-- omit in toc -->

If using the Prometheus operator or the Grafana agent in operator mode, the scrape job should instead be configured via a `ServiceMonitor` that will dynamically edit the Prometheus configuration file. In that case, the relabeling has a slightly different syntax:

```yaml
# File: service-monitor.yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
  endpoints:
  - port: http-metrics
    relabelings:
    # Add this
    - action: replace
      sourceLabels: [ __meta_kubernetes_node_name]
      targetLabel: nodename
```

As a convenience, if using the [kube-prometheus-stack helm chart](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack), this added rule can be directly specified in your values.yaml:

```yaml
# File: kube-prometheus-stack-values.yaml
prometheus-node-exporter:
  prometheus:
    monitor:
      relabelings:
      - action: replace
        sourceLabels: [__meta_kubernetes_pod_node_name]
        targetLabel: nodename
```

#### With Grafana Agent Flow mode <!-- omit in toc -->

The Grafana Agent can [bundle its own node_exporter](https://grafana.com/docs/agent/v0.33/flow/reference/components/prometheus.exporter.unix/). In that case, relabeling can be done this way:

```river
prometheus.exporter.unix {
}

prometheus.scrape "node_exporter" {
  targets = prometheus.exporter.unix.targets
  forward_to = [prometheus.relabel.node_exporter.receiver]

  job_name = "node-exporter"
}

prometheus.relabel "node_exporter" {
  forward_to = [prometheus.remote_write.sink.receiver]

  rule {
    replacement = env("HOSTNAME")
    target_label = "nodename"
  }

  rule {
    # The default job name is "integrations/node_exporter" and needs to be replaced
    replacement = "node-exporter"
    target_label = "job"
  }
}
```

The `HOSTNAME` environment variable is injected by default by the [Grafana Agent helm chart](https://github.com/grafana/agent/blob/93cb1a2718f6fc90fd06ef33b6bcff519dbed662/operations/helm/charts/grafana-agent/templates/containers/_agent.yaml#L25)

### Windows support

The dashboards currently don't support Windows. We experimented with Windows support in the `k8s-views-global` dashboard, but it ended up breaking some panels, so it has been removed for now. You can download the [last Windows-compatible version](https://raw.githubusercontent.com/dotdc/grafana-dashboards-kubernetes/refs/tags/v2.10.1/dashboards/k8s-views-global.json) of the dashboard or download [revision 43 on Grafana.com](https://grafana.com/api/dashboards/15757/revisions/43/download).

See [#79](https://github.com/dotdc/grafana-dashboards-kubernetes/issues/79)

## Contributing

Feel free to contribute to this project:

- Give a GitHub ⭐ if you like it
- Create an [Issue](https://github.com/dotdc/grafana-dashboards-kubernetes/issues) to make a feature request, report a bug or share an idea.
- Create a [Pull Request](https://github.com/dotdc/grafana-dashboards-kubernetes/pulls) if you want to share code or anything useful to this project.
