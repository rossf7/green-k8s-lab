# green-k8s-lab

My home lab a bare metal Kubernetes cluster running [K3s](https://k3s.io/)
for experimenting with green software tools. Especially for measuring energy
consumption using [RAPL](https://web.eece.maine.edu/~vweaver/projects/rapl/).

## Bootstrap Flux

Set env vars

- GITHUB_TOKEN
- GITHUB_USER

```sh
clusterctl init --infrastructure packet
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=green-k8s-lab \
  --branch=main \
  --path=./clusters \
  --personal
```
