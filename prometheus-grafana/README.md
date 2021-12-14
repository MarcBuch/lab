# Prometheus / Grafana Monitoring

As recommended in the [DevOps roadmap](https://roadmap.sh/devops), I picked Prometheus and Grafana for monitoring. In my lab, they were hosted outside the PiCluster.

## Install node-exporter on Kubernetes

```Shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus-node-exporter
```

If the Prometheus server is not hosted on the Kuberentes cluster, you need to expose port 9001.

```Shell
kubectl apply -f ingress.yaml
```

---

References:

- [sysdig - Monitor Kubernetes nodes with Prometheus](https://sysdig.com/blog/kubernetes-monitoring-prometheus/)
