# K3s


## K3s Manager
```bash
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644
```

## K3S worker
```bash
sudo cat /var/lib/rancher/k3s/server/node-token
curl -sfL https://get.k3s.io | K3S_TOKEN=token K3S_URL=https://earth:6443 sh -   s - --write-kubeconfig-mode 644
```

## Post Install 

```bash
# list node
k3s kubectl get node

# list pods
kubectl get pod -n kube-system
```

### Remote kubectl copy

```bash
# Remote kubectl copy
sudo scp doink@server2:/etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chmod 644 ~/.kube/config
```

### Uninstall

```bash
sudo /usr/local/bin/k3s-uninstall.sh
sudo /usr/local/bin/k3s-agent-uninstall.sh
```
