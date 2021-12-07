# K3s

K3s is a lightweigth Kubernetes cluster designed for edge computing. Only 512 MB memory and 200 MB disk space are required.

It comes with some addons already installed, for example:

- CoreDNS
- Traefik

## Getting Started

### Master node Installation

```Shell
curl -sfL https://get.k3s.io | sh -

# Get authentication token. Key can be copied after ::server:
sudo cat /var/lib/rancher/k3s/server/node-token
```

### Worker node Installation

```Shell
export K3S_URL="https://k3s-master:6443"
export K3S_TOKEN="xxx"

curl -sfL https://get.k3s.io | sh -
```

### Run a service on the K3s cluster

First, replace the DNS name specified in `whoami.yml` or `nginx.yml`.

```Shell
# whoami test node
kubectl apply -f whoami.yml

# nginx test node
kubectl apply -f nginx.yml
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
