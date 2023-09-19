## Fault Injection

### Delay

```shell
kubectl apply -f istio/bookinfo/virtual-service/fault-delay-injection-service.yaml
```

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```

### Abort

```shell
kubectl apply -f istio/bookinfo/virtual-service/fault-abort-injection-service.yaml
```

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```
