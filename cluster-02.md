# Configuring Swap for kubeadm Compatibility

For compatibility with the `kubeadm` tool, the swap needs to be turned off on the node.

Execute the following commands:

```bash
$ sudo apt install cron -y
$ sudo swapoff -a
$ (sudo crontab -l 2>/dev/null; echo "@reboot /sbin/swapoff -a") | sudo crontab -
|| true
