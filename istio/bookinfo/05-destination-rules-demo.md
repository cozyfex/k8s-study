## Destination Rules

### Apply reviews for test in this demo

```shell
kubectl delete -f istio/bookinfo/destination-rule/demo/reviews.yaml
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews.yaml
```

- Add `test: beta` to `labels` of `reviews` `Service`
- Add `test: beta` to `labels` of `reviews-v2` and `reviews-v3` `Deployment`
- Add `test: beta` to `matchLables` of `reviews-v2` and `reviews-v3` `selector` of `Deployment`
- Add `test: beta` to `lables` of `reviews-v2` and `reviews-v3` `template` of `Deployment`

### Chane Destination Rule

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-destination-rule01.yaml
```

- Delete `v2` and `v3` subset
- Add `test` subset with `test: beta` label

### Change Virtual Service

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-service01.yaml
```

- Route to `v1` subset of `reviews` host with weight 90
- Route to `test` subset of `reviews` host with weight 10

### Check the network on Kiali

```shell
while sleep 0.01;do curl -s -HHost:bookinfo.app http://$INGRESS_HOST:$INGRESS_PORT/productpage &> /dev/null; done
```

And in `Kiali`, `Services` > `reviews`.

### Change the weight of subnets in Virtual Service

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-service02.yaml
```

- Change weight to 10 of `v1` subset
- Change weight to 90 of `test` subset

### Apply trafficPolicy for test subset

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-destination-rule02.yaml
```

- Add `trafficPolicy` to `test` subset
- The `trafficPolicy` is `loadBalancer` with `RANDOM` algorithm

### Apply same trafficPolicy for all subsets of reviews

```shell
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-destination-rule03.yaml
kubectl apply -f istio/bookinfo/destination-rule/demo/reviews-service03.yaml
```

- Delete all `subsets`
- Add `trafficPolicy` to `test` subset
- The `trafficPolicy` is `loadBalancer` with `ROUND_ROBIN` algorithm
- Remain only one `destination` with `reviews` host in `VirtualService`

