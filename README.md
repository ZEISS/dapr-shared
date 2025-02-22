# Dapr Shared

Dapr Shared allows you to create Dapr Applications using the `daprd` Sidecar as a Kubernetes `Daemonset` or `Deployment`. This enables other use cases where Sidecars are not the best option.

By running `daprd` as a Kubernetes `DaemonSet` resource, the `daprd` container will be running in each Kubernetes Node, reducing the network hops between the applications and Dapr. You can also choose to run

For each Dapr Application, you need to deploy this chart using different `shared.appId`s.


## Getting Started

Before installing Dapr Shared, please ensure you have Dapr installed in your cluster. 

If you want to get started with Dapr Shared, you can easily create a new Dapr Shared instance by installing the official Helm Chart:

```
helm install my-dapr-shared oci://registry-1.docker.io/daprio/dapr-shared-chart --set shared.appId=<DAPR_APP_ID> --set shared.remoteURL=<REMOTE_URL> --set shared.remotePort=<REMOTE_PORT>
```

If you want to look at a step-by-step tutorial using some applications and interacting with Dapr Components, check out the [step-by-step tutorial using Kubernetes KinD here](tutorial/README.md).

## From Source

To deploy this chart from the source you can run from inside the `chart/dapr-shared` directory:

```
helm install my-shared . --set shared.appId=<DAPR_APP_ID> --set shared.remoteURL=<REMOTE_URL>


```

Where `<DAPR_APP_ID>` is the Dapr App Id that you can use in your components (for example, for scopes) and `<REMOTE_URL>` is a reachable URL where `dapr-shared` will forward notifications received by the Dapr sidecar.

Future versions include forwarding notifications to multiple remote URLs.

## Customize Dapr Shared
Customize Dapr Shared using custom Helm values


| Key | Type | Default | Description |
|-----|------|---------|-------------|
| shared.controlPlane.namespace | string | `"dapr-system"` | Namespace where Dapr Control Plane is. |
| shared.daprd.apiLogging.enabled | bool | `true` | Enables API logging for the daprd. |
| shared.daprd.app.protocol | string | `"http"` | Dapr which protocol your application is using. Valid options are `http`` and `grpc``. |
| shared.daprd.grpcPort | int | `50001` | gRPC port for the Dapr Internal API to listen on. |
| shared.daprd.httpPort | int | `3500` | The HTTP port for the Dapr API. |
| shared.daprd.image.name | string | `"daprd"` | Daprd image. |
| shared.daprd.image.pullPolicy | string | `"Always"` | Daprd image pull policy. |
| shared.daprd.image.registry | string | `"docker.io/daprio"` | Daprd image registry. |
| shared.daprd.image.tag | string | `"1.11.0"` | Daprd image version. |
| shared.daprd.internalGrpcPort | int | `50002` | gRPC port for the Dapr Internal API to listen on. |
| shared.daprd.listenAddresses | string | `"0.0.0.0"` | Comma separated list of IP addresses that daprd will listen to. Defaults to all in standalone mode. Defaults to [::1],127.0.0.1 in Kubernetes. To listen to all IPv4 addresses, use 0.0.0.0. To listen to all IPv6 addresses, use [::]. |
| shared.daprd.metrics.enabled | bool | `true` | Enable prometheus metric. |
| shared.daprd.metrics.port | int | `9090` | Sets the port for the sidecar metrics server. |
| shared.daprd.mtls.enabled | bool | `false` | Enables automatic mTLS for daprd to daprd communication channels. |
| shared.daprd.publicPort | int | `3501` | The HTTP public port for the Dapr API. |
| shared.daprd.token | string | `""` | Dapr API token to use for token based API authentication. |
| shared.deployment.replicas | int | `1` | The quantity of replicas. This property is set only when `shared.strategy` is equal to `deployment` |
| shared.initContainer.image.name | string | `"dapr-shared"` | The dapr-shared image name. |
| shared.initContainer.image.pullPolicy | string | `"Always"` | The init container pull policy. |
| shared.initContainer.image.registry | string | `"docker.io/matheuscruzdev"` | The dapr-shared image registry. |
| shared.initContainer.image.tag | string | `"latest"` | The dapr-shared-init image tag. |
| shared.initContainer.token | string | `""` | The dapr API token. |
| shared.log.json | bool | `true` | The daprd log format. |
| shared.log.level | string | `"info"` | The daprd log level. |
| shared.remotePort | int | `0` | The remote port. |
| shared.service.type | string | `"ClusterIP"` | The daprd service type. |
| shared.serviceAccount.annotations | object | `{}` |  |
| shared.serviceAccount.create | bool | `true` | Allows the option to create or not the service account. |
| shared.serviceAccount.name | string | `""` | Kubernetes Service Account name. |
| shared.strategy | string | `"daemonset"` | The default strategy to run dapr in shared mode. Possible values `daemonset`, `deployment`. |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.11.0](https://github.com/norwoodj/helm-docs/releases/v1.11.0)

### Building from source

I've used the [CNCF `ko` project](https://ko.build/) to build multiplatform images for the proxy.
You can run the following command to build containers for the `dapr-shared` proxy:

```
ko build --platform=linux/amd64,linux/arm64
```

