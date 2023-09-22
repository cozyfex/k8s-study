## Authentication Demo

### Cleanup bookfino app

```shell
./istio/samples/bookinfo/platform/kube/cleanup.sh
```

### Deploy bookinfo app

```shell
kubectl apply -f istio/samples/bookinfo/platform/kube/bookinfo.yaml
```

### Create namespace

```shell
kubectl create ns bar
```

### Apply httpbin

```shell
kubectl apply -f <(istioctl kube-inject -f istio/samples/httpbin/httpbin.yaml) -n bar
```

### Check http status

```shell
kubectl exec -it "$(kubectl get pod -l app=httpbin -n bar -o jsonpath='{.items..metadata.name}')" -c istio-proxy -n bar -- curl "http://productpage.default:9080" -s -o /dev/null -w "%{http_code}\n"
```

```shell
200
```

### Create PeerAuthentication

```shell
kubectl apply -n default -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT
EOF
```

### Check http status

```shell
000
command terminated with exit code 56
```
