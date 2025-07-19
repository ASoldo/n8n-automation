# Grafana

This folder contains Kubernetes manifests to deploy Grafana as a self-hosted monitoring frontend.

## Documentation

- Official Grafana documentation: https://grafana.com/docs/grafana/latest/installation/kubernetes/
- Official Docker image: https://hub.docker.com/r/grafana/grafana

## Deployment

```sh
kubectl apply -f k8s/_root/namespace.yaml \
              -f k8s/grafana/grafana-pvc.yaml \
              -f k8s/grafana/grafana-deployment.yaml \
              -f k8s/grafana/grafana-service.yaml \
              -f k8s/grafana/grafana-ingress.yaml
```

Then access Grafana at http://grafana.local:3000/.

Optionally add `grafana.local:port` to your `/etc/hosts` pointing to your cluster IP.

## Credentials
```sh
user: admin
password: "TT1!"
```
