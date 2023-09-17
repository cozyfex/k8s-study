## Destination Rules

### Apply reviews for test in this demo

```shell
kubectl delete -f istio/bookinfo/destination-rule/demo/reviews.yaml
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews.yaml
```

### Chane Destination Rule

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-destination-rule01.yaml
```

### Change Virtual Service

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-service01.yaml
```

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```

And in Kiali, Services > reviews.

### Change the weight of subnets in Virtual Service

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-service02.yaml
```

### Apply trafficPolicy for test subset

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-destination-rule02.yaml
```

### Apply same trafficPolicy for all subsets of reviews

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-destination-rule03.yaml
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-service03.yaml
```

