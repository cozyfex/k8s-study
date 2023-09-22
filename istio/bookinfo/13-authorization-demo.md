## Authorization Demo

### Creat Gateway and VirtualService

```shell
kubectl apply -f istio/bookinfo/gateway/bookinfo-gateway.yaml
kubectl apply -f istio/bookinfo/virtual-service/bookinfo-service.yaml
```

### Set ingress ip and port

```shell
export INGRESS_HOST=127.0.0.1
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
```

### Check product page

```shell
curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage
```

### Apply `AuthorizationPolicy`

```shell
kubectl apply -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: allow-nothing
  namespace: default
spec:
  { }
EOF
```

### Check product page

```shell
curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage
```

```shell
RBAC: access denied
```

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```

### Allow product page viewer

```shell
kubectl apply -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: product-page-viewer
  namespace: default
spec:
  selector:
    matchLabels:
      app: productpage
  action: ALLOW
  rules:
    - to:
        - operation:
            methods:
              - GET
EOF
```

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```

### Allow details viewer from bookinfo-productpage `ServiceAccount`

```shell
kubectl apply -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: details-viewer
  namespace: default
spec:
  selector:
    matchLabels:
      app: details
  action: ALLOW
  rules:
    - from:
        - source:
            principals:
              - cluster.local/ns/default/sa/bookinfo-productpage
    - to:
        - operation:
            methods:
              - GET
EOF
```

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```

### Allow reviews viewer from bookinfo-productpage `ServiceAccount`

```shell
kubectl apply -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: reviews-viewer
  namespace: default
spec:
  selector:
    matchLabels:
      app: reviews
  action: ALLOW
  rules:
    - from:
        - source:
            principals:
              - cluster.local/ns/default/sa/bookinfo-productpage
    - to:
        - operation:
            methods:
              - GET
EOF
```

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```

### Allow ratings viewer from bookinfo-reviews `ServiceAccount`

```shell
kubectl apply -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: ratings-viewer
  namespace: default
spec:
  selector:
    matchLabels:
      app: ratings
  action: ALLOW
  rules:
    - from:
        - source:
            principals:
              - cluster.local/ns/default/sa/bookinfo-reviews
    - to:
        - operation:
            methods:
              - GET
EOF
```

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```



