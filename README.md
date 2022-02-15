# grafana-dashboards-kubernetes

![logo](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/kubernetes-grafana-dashboards-logo.png)

## Description

This repository contains a modern set of [Grafana](https://github.com/grafana/grafana) dashboards for [Kubernetes](https://github.com/kubernetes/kubernetes).\
They are inspired by many other dashboards from `kubernetes-mixin` and `grafana.com`.

You can also find and download them on [Grafana.com](https://grafana.com/grafana/dashboards/?plcmt=top-nav&cta=downloads&search=dotdc).

## Features

Theses dashboards are made and tested for the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) chart, but they should work well with others as soon as you have [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) and [prometheus-node-exporter](https://github.com/prometheus/node_exporter) installed in your Kubernetes cluster .

They are not backward compatible with older Grafana versions because they try to take advantage of Grafana's newest features like:

- `Time series` visualization panel introduced in Grafana 7.4 ([Grafana Blog post](https://grafana.com/blog/2021/02/10/how-the-new-time-series-panel-brings-major-performance-improvements-and-new-visualization-features-to-grafana-7.4/)).
- `$__rate_interval` introduced in Grafana 7.2 ([Grafana Blog post](https://grafana.com/blog/2020/09/28/new-in-grafana-7.2-__rate_interval-for-prometheus-rate-queries-that-just-work/)).

They also all have a `Prometheus Datasource` variable in case you want to use them on a federated Grafana instance.

As an example, here's how "**Kubernetes / Views / Global**" looks like:

![screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-global.png "Kubernetes Global View Screenshot")

## Dashboards

| File name                  | Description | Screenshot |
|:---------------------------|:------------|:-----------|
| k8s-system-api-server.json | Dashboard for the API Server Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-system-api-server.png) |
| k8s-system-coredns.json    | Show information on the CoreDNS Kubernetes component. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-system-coredns.png) |
| k8s-views-global.json      | `Global` level view dashboard for Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-global.png) |
| k8s-views-namespaces.json  | `Namespaces` level view dashboard for Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-namespaces.png) |
| k8s-views-nodes.json       | `Nodes` level view dashboard for Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-nodes.png) |
| k8s-views-pods.json        | `Pods` level view dashboard for Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-pods.png) |

## Contributing

Feel free to contribute to this project:

- Create an `Issue` to make a feature request, report a bug or share an idea.
- Create a `Pull Request` if you want to share code or anything useful to this project.
