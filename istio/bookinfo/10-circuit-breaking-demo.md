## Circuit Breaking Demo

### Apply DestinationRule

```shell
kubectl apply -f istio/bookinfo/destination-rule/circuit-breaking-rule.yaml
```

### Kubernetes Port Forwarding

```shell
kubectl port-forward --address=0.0.0.0 \
  $(kubectl get pod -l app=productpage -o jsonpath='{.items[0].metadata.name}') \
  8010:9080
```

### Install Benchmark Tool

```shell
brew install nghttp2
```

### Run Benchmark

```shell
h2load -n1000 -c1 --h1 http://localhost:8010
```

```shell
h2load -n1000 -c2 --h1 http://localhost:8010
```

```shell
h2load -n1000 -c3 --h1 http://localhost:8010
```

```shell
h2load -n1000 -c4 --h1 http://localhost:8010
```
