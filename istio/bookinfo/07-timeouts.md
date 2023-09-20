## Timeouts

### Apply timeouts with fault injection

```shell
kubectl apply -f istio/bookinfo/virtual-service/timeouts-service.yaml
```

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```

### Delete timeouts

```shell
kubectl delete -f istio/bookinfo/virtual-service/timeouts-service.yaml
```