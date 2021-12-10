# Longhorn

## Installation with Helm

```Shell
helm repo add longhorn https://charts.longhorn.io
helm repo update

helm install longhorn/longhorn --name longhorn --namespace longhorn-system
```

## Make the dashboard available

```YAML
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
    name: ingressroute
    namespace: longhorn-system
spec:
    entryPoints:
    - web
    routes:
    - match: Host(`longhorn.home`)
    kind: Rule
    services:
    - name: longhorn-frontend
      port: 80
```

## References

- [Longhorn Docs](https://longhorn.io/docs/1.2.2/deploy/install/install-with-helm/)
