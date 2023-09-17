## Gateway Demo

### Creat Gateway

```shell
kubectl apply -f istio/bookinfo/gateway/bookinfo-gateway.yaml
```

### Create VirtualService

```shell
kubectl apply -f istio/bookinfo/virtual-service/bookinfo-service.yaml
```

### Set ingress ip and port

```shell
export INGRESS_HOST=127.0.0.1
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
```

### Check host

```shell
curl -s -H Host:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage | grep -o "<title>.*</title>"
```

Result is

```shell
<title>Simple Bookstore App</title>
```

### Check Kiali

Check the kiali of Settings

### Reset Gateway

```shell
kubectl apply -f istio/bookinfo/gateway/bookinfo-gateway-wildcard.yaml
```