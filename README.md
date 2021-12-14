# Lab

This repo describes my experiments with various Kubernetes and Docker deployments. It was deployed on a Raspberry Pi 4 cluster running Ubuntu 20.04.3 LTS and K3s as a Kubernetes provider.

## Initial Setup

The Pi cluster was equiped with the following tools:

1. [Setup K3s](./k3s/README.md)
2. [Setup cert-manager for SSL certificates](./cert-manager/README.md)
3. [Setup longhorn for storage](./longhorn/README.md)
4. [Setup node exporter for Prometheus](./prometheus-grafana/README.md)

## Applications

For actual user applications I implemented:

- [Minio](./minio/README.md)
- [Vaultwarden](./vaultwarden/README.md)

## Findings

- [K3s](./k3s/README.md) already comes with Traefik installed. Only the `IngressRoute` Kubernetes resource needs to be deployed to use it.
- [Rancher](./rancher/README.md) with built-in monitoring made the Pis unusable, as it took to many resources.
