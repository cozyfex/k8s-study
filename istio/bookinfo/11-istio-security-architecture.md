# Istio Security Architecture

## Authentication

### Mutual TLS

#### Workload-specific policy

```yaml
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: example-peer-policy
  namespace: book-info
spec:
  selector:
    matchLabels:
      app: reviews
  mtls:
    mode: STRICT
```

#### Namespace-wide policy

```yaml
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: example-peer-policy
  namespace: book-info
spec:
  mtls:
    mode: STRICT
```

#### Mesh-wide policy

```yaml
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: example-peer-policy
  namespace: istio-system # Here!!
spec:
  mtls:
    mode: STRICT
```

### Request Authentication

Policy Kind: `ReuqestAuthentication`

- [JWT](https://jwt.io)
- [ORY Hydra](https://www.ory.sh)
- [Keycloak](https://www.keycloak.org)
- [Auth0](https://auth0.com)
- [Firebase Auth](https://firebase.google.com/docs/auth/)
- [Google Auth](https://developers.google.com/identity/openid-connect/openid-connect)

## Authorization

### Methods

- GET
- POST
- PATCH
- DELETE

### Actions

- CUSTOM
- DENY
- ALLOW
- AUDIT

### `AuthorizationPolicy`

```yaml
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: auth-deny-policy
  namespace: book-info
spec:
  action: DENY
  rules:
    - from:
        - source:
            namespaces:
              - bar
      to:
        - operation:
            methods:
              - POST
```
