# Longhorn

Longhorn enables persistent block storate on non-cloud Kubernetes clusters.

## Installation with Helm

```Shell
helm repo add longhorn https://charts.longhorn.io
helm repo update

helm install longhorn longhorn/longhorn --namespace longhorn-system
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

## Optional - Change the default storage class to Longhorn

```Shell
# List the storage classes of your cluster
kubectl get storageclass

# Mark the current defautl class as non-default
kubectl patch storageclass <default storage class> -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'
```

## Create a peristent volume with Longhorn

```YAML
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: minio-pvc
    namespace: minio
spec:
    accessModes:
        - ReadWriteOnce
    storageClassName: longhorn
    resources:
        requests:
            storage: 2Gi
```

## References

- [Longhorn Docs](https://longhorn.io/docs/1.2.2/deploy/install/install-with-helm/)
