## Istio study

This is with bookinfo sample of [Istio Github](https://github.com/istio/istio).

## System Installation

### Install Istio and addons

```shell
istioctl install --set profile=demo
```

### Install Kiali, Grafana, Prometheus, Jagger, Loki

```shell
kubectl apply -f samples/addons
```

### Useful commands

#### Install bookinfo sample

```shell
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```

#### Cleanup bookfino sample

```shell
./samples/bookinfo/platform/kube/cleanup.sh
```

#### Execute a command in a container

```shell
# kubectl exec -n <namespace> <pod-name> -c <container-name> -- <command>
kubectl exec -n demo-app curl-kwrtv -c curl -- curl -s 10.102.210.102
```

#### Execute repeat command

```shell
while sleep 0.01;do curl -sS 'http://'"$INGRESS_HOST"':'"$INGRESS_PORT"'/productpage'\ &> /dev/null; done
```

#### Apply sidecar

```shell
kubectl apply -f <(istioctl kube-inject -f samples/httpbin/httpbin.yaml) -n bar
```
