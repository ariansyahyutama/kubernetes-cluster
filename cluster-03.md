install containerd

https://docs.docker.com/engine/install/ubuntu/

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

```

```bash
sudo apt-get install containerd.io
```


install kubeadm

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

config cgroup

```bash
containerd config default | sed 's/SystemdCgroup = false/SystemdCgroup = true/' | sed 's/registry.k8s.io\/pause:3.6/registry.k8s.io\/pause:3.9/' | sudo tee /etc/containerd/config.toml
```

bootstaping

```bash
NODENAME=$(hostname -s)
IPADDR=$(ip route get 8.8.8.8 | sed -n 's/.*src \([^\ ]*\).*/\1/p')
POD_CIDR=192.168.0.0/16

```


```bash
sudo kubeadm init --apiserver-advertise-address=$IPADDR \
    --apiserver-cert-extra-sans=$IPADDR  \
    --pod-network-cidr=$POD_CIDR \
    --node-name $NODENAME \
    --ignore-preflight-errors Swap
```
