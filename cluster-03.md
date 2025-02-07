install containerd

https://docs.docker.com/engine/install/ubuntu/


install kubeadm

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/


bootstaping

```bash
$ NODENAME=$(hostname -s)
$ IPADDR=$(ip route get 8.8.8.8 | sed -n 's/.*src \([^\ ]*\).*/\1/p')
$ POD_CIDR=192.168.0.0/16

```
