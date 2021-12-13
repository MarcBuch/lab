# Rancher

Rancher is a container management tool like Portainer, but for Kubernetes.

**Requires significant resources**. Not very suited for a small PiCluster.

## Install Rancher with helm

First, you need to create a namespace for rancher. It has to be called `cattle-system`.

```Shell
kubectl create namespace cattle-system
```

Then you either need lets encrypt, cert-manager or a certificate file for SSL certificates. I've chosen cert-manager.

```Shell
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.5.1/cert-manager.crds.yaml
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.5.1
```

After you've installed a source for SSL certificates, you can install Rancher.

```Shell
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
helm repo update
helm install rancher rancher-latest/rancher \
    --namespace cattle-system
    --set hostname=rancher.home
```

To check the current status of the rollout:

```Shell
kubectl -n cattle-system rollout status deploy/rancher
```

After a successful deployment, you need to obtain the bootstrap secret.

```Shell
kubectl get secret --namespace cattle-system bootstrap-secret \
    -o go-template='{{.data.bootstrapPassword|base64decode}}{{ "\n" }}'
```

You can use this secret to login to the Rancher UI.

---

References:

- [Rancher Docs](https://rancher.com/docs/rancher/v2.6/en/overview/)
