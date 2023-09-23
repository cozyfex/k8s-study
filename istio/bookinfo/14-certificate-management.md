## Certificate Management

### Make directory

```shell
mkdir ca-certs
cd ca-certs
```

### Generate Self Key

```shell
make -f ../istio/tools/certs/Makefile.selfsigned.mk root-ca
```

### Generate Local Cluster Key

```shell
make -f ../istio/tools/certs/Makefile.selfsigned.mk localcluster-cacerts
```

### Delete `istio-system` namespace

```shell
kubectl delete ns istio-system
```

### Cleanup bookfino app

```shell
cd ..
./istio/samples/bookinfo/platform/kube/cleanup.sh
```

### Create `istio-system` namespace

```shell
kubectl create ns istio-system
```

### Install Istio with tls

```shell
cd ca-certs/localcluster
```

This is read certs files in the directory

```shell
istioctl install --set profile=demo
```

### Install addons

```shell
cd ..
cd ..
kubectl apply -f istio/samples/addons
```

### Apply bookinfo app

```shell
kubectl apply -f istio/samples/bookinfo/platform/kube/bookinfo.yaml 
```

### Apply bookinfo gateway

```shell
kubectl apply -f istio/samples/bookinfo/networking/bookinfo-gateway.yaml
```

### Apply bookinfo destination rule

```shell
kubectl apply -f istio/samples/bookinfo/networking/destination-rule-all.yaml
```

### Check Istio

```shell
istioctl analyze
```

### Apply `PeerAuthentication`

```shell
kubectl apply -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
spec:
  mtls:
    mode: STRICT
EOF
```

### Check the certificate chain

```shell
kubectl exec "$(kubectl get pod -l app=details -o jsonpath='{.items..metadata.name}')" -c istio-proxy -- openssl s_client -showcerts -connect productpage:9080 > httpbin-proxy-cert.txt
```

