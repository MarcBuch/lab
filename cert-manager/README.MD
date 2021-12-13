# cert-manager

cert-manager provides certificates to Kubernetes resources.

## Helm Installation

Add the chart repository if it wasn't added already.

```Shell
helm repo add jetstack https://charts.jetstack.io
helm repo update
```

Install `customResourceDefinitions`.

```Shell
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.6.1/cert-manager.crds.yaml
```

Install cert-manager.

```Shell
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.6.1
```

## Add a certificate to a Kubernetes deployment

First you need to create the certificate resource itself.

```YAML
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: traefik.home
  namespace: kube-system
spec:
  dnsNames:
    - traefik.home
  secretName: traefik.home
  issuerRef:
    name: selfsigned
    kind: ClusterIssuer
```

Then the certificate resource can be added to a service as `secretName` below tls.

```YAML
---
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: traefik-dashboard
  namespace: kube-system
spec:
  entryPoints:
    - websecure
  routes:
    - match: Host(`traefik.home`) && (PathPrefix(`/dashboard`) || PathPrefix(`/api`))
      kind: Rule
      services:
        - name: api@internal
          kind: TraefikService
  tls:
    secretName: traefik.home
```

---

References:

- [cert-manager Docs](https://cert-manager.io/docs/)
