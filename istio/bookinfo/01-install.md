## Install

### Install istioctl

#### MacOS

```shell
brew install istioctl
```

#### Linux

```shell
curl -L https://istio.io/downloadIstio | sh -
cd istio-1.19.0
export PATH=$PWD/bin:$PATH
```

### Install Istio

#### Install with demo profile

```shell
istioctl install --set profile=demo -y
```

#### Set label to namespace for sidecar(In this case *default*)

```shell
kubectl label namespace default istio-injection=enabled
```

### Install Kiali, Grafana, Prometheus, Jagger, Loki

```shell
kubectl apply -f samples/addons
```

### Set ingress ip and port

```shell
export INGRESS_HOST=127.0.0.1
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
```