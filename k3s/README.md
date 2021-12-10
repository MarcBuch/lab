# K3s

K3s is a lightweigth Kubernetes cluster designed for edge computing. Only 512 MB memory and 200 MB disk space are required.

It comes with some addons already installed, for example:

- CoreDNS
- Traefik

## Getting Started

### Install the master node

```Shell
curl -sfL https://get.k3s.io | sh -

# Get authentication token. Key can be copied after ::server:
sudo cat /var/lib/rancher/k3s/server/node-token
```

### Optional - Install a worker node

```Shell
export K3S_URL="https://k3s-master:6443"
export K3S_TOKEN="xxx"

curl -sfL https://get.k3s.io | sh -
```

### Make your kubeconfig available outside the cluster

On the cluster master:

```Shell
sudo chmod 660 /etc/rancher/k3s/k3s.yaml
```

On your workstation:

```Shell
scp marc@k3s-master:/etc/rancher/k3s/k3s.yaml ~/.kube/config

# Change the ip address of the cluster
vim ~/.kube/config
```

### Install cert-manager

```Shell
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.2.0 \
  --create-namespace \
  --set installCRDs=true
```

### Expose the Traefik dashboard with HTTPS

```Shell
kubectl apply -f dashboard-ingress.yaml

curl -k -H 'Host: traefik.home' https://traefik.home/dashboard/
```

### Run a service on the cluster

First, replace the DNS name specified in `whoami.yaml` or `nginx.yaml`.

```Shell
# whoami HTTP test node
kubectl apply -f whoami.yaml

# nginx HTTPS test node
kubectl apply -f nginx.yaml
```

## Uninstall

```Shell
# Master
sudo /usr/local/bin/k3s-uninstall.sh

# Agent
sudo /usr/local/bin/k3s-agent-uninstall.sh
```

## References

- [Rancher K3s Docs](https://rancher.com/docs/k3s/latest/en/)
