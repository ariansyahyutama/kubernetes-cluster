# Solution

To prepare an Ubuntu-based host for a Kubernetes cluster, you first need to turn on IPv4 forwarding and enable iptables to see bridged traffic:

```bash
$ cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

$ sudo modprobe overlay
$ sudo modprobe br_netfilter

$ cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

$ sudo sysctl --system


Key elements to note:
1. Use `#` for the main heading
2. Use regular text for descriptions
3. Use triple backticks (\`\`\`) with `bash` specification for code blocks
4. Indent command blocks properly
5. Use proper spacing between sections

When you view this in GitHub, it will render with proper formatting and syntax highlighting for the code blocks.
