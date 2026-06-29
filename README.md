# Homelab

Homelab test

## Structure

- clusters
- infrastructure
- apps

### Init Secrets

```bash
# create age.agekey from private key or copy from ~/.config/sops/age/keys.txt
cp ~/.config/sops/age/keys.txt age.agekey
kubectl create secret generic sops-age -n flux-system --from-file age.agekey
rm age.agekey
```

sudo apt install open-iscsi
sudo systemctl enable --now iscsid

### Bootstrap Flux

```bash
flux check --pre
flux bootstrap github \
  --owner=jacokok \
  --repository=homelab \
  --branch=main \
  --path=./cluster \
  --personal
```

### Flux

```bash
# Get kustomizations
flux get kustomizations
flux get kustomizations -w

# Reconcile
flux reconcile source git flux-system
flux reconcile kustomization infrastructure

kubectl describe kustomization apps -n flux-system
kubectl describe kustomization infrastructure -n flux-system

kubectl get events -n flux-system --sort-by='.lastTimestamp'

```

### Manage Secrets
```bash
# Install sops and age
# Import age private key
# Encrypt file
sops encrypt secret.yaml > test-secret.yaml
sops decrypt test-secret.yaml
kubeseal --cert=pub-sealed-secrets.pem --format=yaml < secret.yaml > sealed-secret.yaml
```

### install k3s

```bash
curl -sfL https://get.k3s.io | sh -s - --disable traefik --write-kubeconfig-mode 644
```

### TODO

- [ ] Setup ansible to install k3s
- [ ] Ansible update
- [ ] Ansible ssh
- [ ] Ansible install packages
- [ ] Ansible setup registries.yaml

```bash
sudo mkdir -p /etc/rancher/k3s
sudo nano /etc/rancher/k3s/registries.yaml
sudo systemctl restart k3s
sudo systemctl restart k3s-agent
```
```yaml
configs:
  "index.docker.io":
    auth:
      username: "<YOUR_DOCKERHUB_USERNAME>"
      password: "<YOUR_DOCKERHUB_TOKEN_OR_PASSWORD>"
```
