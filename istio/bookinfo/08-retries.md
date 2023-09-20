## Retries

### VirtualService for retries

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: my-service
spec:
  hosts:
    - my-service # This is defined in Gateway
  http:
    - route:
        - destination:
            host: my-service
            subset: v1
      retries:
        attempts: 3
        perTryTimeout: 2s
```

## Istio Defaults

- 25ms+ intervals after 1st fail
- 2 tries before returning an error