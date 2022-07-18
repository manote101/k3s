# k3s

## 3 ways to run k3s
1. Command line Installation
```ShellSession
# Non-HA use SQLite as database
curl -sfL https://get.k3s.io | sh -

# see Token, copy to your clipboard
cat /var/lib/rancher/k3s/server/node-token

# Worker node, you can paste Token from the clipboard
curl -sfL https://get.k3s.io | K3S_URL=https://<ip-server>:6443 K3S_TOKEN=<server-token> sh - 

k3s kubectl get nodes

# HA configuration use Etcd
# master node#1
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --cluster-init --node-ip <ip-this-node> --node-external-ip <ip-this-node>" sh -s -
kubectl get nodes
cat /var/lib/rancher/k3s/server/token

# master node#2
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --server https://<ip-master#1>:6443 --node-ip <ip-this-node> --node-external-ip <ip-this-node>" K3S_TOKEN=<token> K3S_NODE_NAME=node2 sh -
kubectl get node

# master node#3
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --server https://<ip-master#1>:6443 --node-ip <ip-this-node> --node-external-ip <ip-this-node>" K3S_TOKEN=<token> K3S_NODE_NAME=node3 sh -
kubectl get node

# worker node
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --agent https://<ip-master#1>:6443 --node-ip <ip-this-node> --node-external-ip <ip-this-node>" K3S_TOKEN=<token> K3S_NODE_NAME=agent1 sh -

```

2. K3D
```ShellSession
# create single-node cluster
k3d cluster create
k3d cluster list
kubectl get node
kubectl get pod
k3d cluster delete

# create multi-node cluster
k3d cluster create --servers 3 --agents 1
k3d cluster list
# see docker process
kubectl get node
kubectl get pod
docker ps

```
3. Rancher Desktop, see https://rancherdesktop.io/
