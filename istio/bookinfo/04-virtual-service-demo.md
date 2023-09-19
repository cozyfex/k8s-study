## Virtual Service Demo

### Apply sample destination rule

```shell
kubectl apply -f istio/samples/bookinfo/networking/destination-rule-all.yaml
```

### Apply reviews virtual service

```shell
kubectl apply -f istio/bookinfo/virtual-service/reviews-service.yaml
```

### Validation Istio

```shell
istioctl analyze
```

### Set ingress ip and port

```shell
export INGRESS_HOST=127.0.0.1
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
```

### Generate requests

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```