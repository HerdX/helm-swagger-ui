# Helm Chart for swagger-ui
## Introduction

This [Helm](https://github.com/kubernetes/helm) chart installs [swagger-ui](https://github.com/swagger-ui-api/swagger-ui) in a Kubernetes cluster.

## Prerequisites

- Kubernetes cluster 1.10+
- Helm 2.8.0+
- PV provisioner support in the underlying infrastructure.

## Installation

### Original Authors
This chart is derived from `https://cetic.github.io/helm-charts/swagger-ui`

### Configure the chart

The following items can be set via `--set` flag during installation or configured by editing the `values.yaml` directly (need to download the chart first).

#### Configure the way how to expose swagger-ui service:

- **Ingress**: The ingress controller must be installed in the Kubernetes cluster.
- **ClusterIP**: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster.
- **NodePort**: Exposes the service on each Node’s IP at a static port (the NodePort). You’ll be able to contact the NodePort service, from outside the cluster, by requesting `NodeIP:NodePort`.
- **LoadBalancer**: Exposes the service externally using a cloud provider’s load balancer.

### Install the chart

Install the swagger-ui helm chart with a release name `my-release`:

```bash
docker build -t your_tag .
# Change yaml to point to your image
# If necessary add a imagePullSecret
## Example of Image Pull Secret in values.yaml
# imagePullSecrets:
#   - name: quay
helm install --name my-release cetic/swaggerui
```

## Uninstallation

To uninstall/delete the `my-release` deployment:

```bash
helm delete --purge my-release
```

## Configuration

The following table lists the configurable parameters of the swagger-ui chart and the default values.

| Parameter                                                                   | Description                                                                                                        | Default                         |
| --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------| ------------------------------- |
| **Image**                                                                   |
| `image.repository`                                                          | swagger-ui Image name                                                                                              | `swaggerapi/swagger-ui`         |
| `image.tag`                                                                 | swagger-ui Image tag                                                                                               | `v3.23.10`                      |
| `image.pullPolicy`                                                          | swagger-ui Image pull policy                                                                                       | `IfNotPresent`                  |
| **swaggerui**                                                              |
| `swaggerui.jsonPath`                                                       | location of the configuration json file file                                                                       | `""`                            |
| `swaggerui.jsonUrl`                                                        | location of the configuration json file file                                                                       | `http://petstore.swagger.io/v2/swagger.json` |
| `swaggerui.server.url`                                                     | Url of a custom server                                                                                             | `"http://www.google.be"`        |
| `swaggerui.server.description`                                             | descripton of a custom server                                                                                      | `"helm-online"`                 |
| **Service**                                                                 |
| `service.type`                                                              | Type of service for swagger-ui frontend                                                                            | `NodePort`                      |
| `service.port`                                                              | Port to expose service                                                                                             | `8080`                          |
| `service.nodePort`                                                          | Port where the service is reachable                                                                                | `30245`                         |
| `service.loadBalancerIP`                                                    | LoadBalancerIP if service type is `LoadBalancer`                                                                   | `nil`                           |
| `service.loadBalancerSourceRanges`                                          | Address that are allowed when svc is `LoadBalancer`                                                                | `[]`                            |
| `service.annotations`                                                       | Service annotations                                                                                                | `{}`                            |
| **Ingress**                                                                 |
| `ingress.enabled`                                                           | Enables Ingress                                                                                                    | `false`                         |
| `ingress.annotations`                                                       | Ingress annotations                                                                                                | `{}`                            |
| `ingress.path`                                                              | Path to access frontend                                                                                            | `/`                             |
| `ingress.hosts`                                                             | Ingress hosts                                                                                                      | `[]`                            |
| `ingress.tls`                                                               | Ingress TLS configuration                                                                                          | `[]`                            |
| **ReadinessProbe**                                                          |
| `readinessProbe`                                                            | Rediness Probe settings                                                                                            | `nil`                           |
| **LivenessProbe**                                                           | 
| `livenessProbe.httpGet.path`                                                | Liveness Probe settings                                                                                            | `/`                             |
| `livenessProbe.httpGet.port`                                                | Liveness Probe settings                                                                                            | `http`                          |
| `livenessProbe.initialDelaySeconds`                                         | Liveness Probe settings                                                                                            | `60`                            |
| `livenessProbe.periodSeconds`                                               | Liveness Probe settings                                                                                            | `30`                            |
| `livenessProbe.timeoutSeconds`                                              | Liveness Probe settings                                                                                            | `10`                            |
| **Resources**                                                               |
| `resources`                                                                 | CPU/Memory resource requests/limits                                                                                | `{}`                            |

## Contributing

Feel free to contribute by making a [pull request](https://github.com/cetic/swagger-ui/pull/new/master).

Please read the official [Contribution Guide](https://github.com/helm/charts/blob/master/CONTRIBUTING.md) from Helm for more information on how you can contribute to this Chart.

## License

[Apache License 2.0](/LICENSE.md)
