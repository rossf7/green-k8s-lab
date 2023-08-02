# green-k8s-lab WIP

Provision bare metal k8s clusters with Equinix Metal for testing green
software tools.

## Bootstrap

Set env vars

- GITHUB_TOKEN
- GITHUB_USER
- PACKET_API_KEY

```sh
kind delete cluster && kind create cluster
clusterctl init --infrastructure packet
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=green-k8s-lab \
  --branch=main \
  --path=./clusters/kind \
  --personal
```

## management cluster (kind)

- flux
- carbon-aware-karmada-operator
- karmada
- grafana
- prometheus

## workload clusters (Equinix Metal)

- grid-intensity-exporter
- kepler
- scaphandre
- flux
- grafana
- prometheus

## Silicon Valley cluster

- carbon-aware-keda-operator
- kubernetes-carbon-intensity-exporter
- keda
