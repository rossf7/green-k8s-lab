# green-k8s-lab

My home lab.  A bare metal Kubernetes cluster running [K3s](https://k3s.io/)
for experimenting with green software tools. Especially for measuring energy
consumption using [RAPL](https://web.eece.maine.edu/~vweaver/projects/rapl/).

## Install K3s

```sh
curl -sfL https://get.k3s.io | sh -s -- --flannel-backend none \
  --disable traefik \
  --disable-network-policy
```

## Copy kubeconfig

```sh
mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $(whoami):$(whoami) ~/.kube/config
```

## Install Cilium CNI

```sh
CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt)
CLI_ARCH=amd64
if [ "$(uname -m)" = "aarch64" ]; then CLI_ARCH=arm64; fi
curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum
sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin
rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}
cilium install
cilium status --wait
```

## Bootstrap Flux

```sh
export GITHUB_TOKEN=***your_token***
export GITHUB_USER=***your_user***

curl -s https://fluxcd.io/install.sh | sudo bash
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=green-k8s-lab \
  --branch=main \
  --path=clusters \
  --personal
```
