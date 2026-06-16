# Homelab

Homelab test

## Structure

- clusters
- infrastructure
- apps

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

# Force secret refresh
kubectl annotate externalsecret test-secret -n external-secrets force-sync=\$(date +%s) --overwrite
```

### Init Secrets

```bash
# create age.agekey from private key or copy from ~/.config/sops/age/keys.txt
cp ~/.config/sops/age/keys.txt age.agekey
kubectl create secret generic sops-age --namespace=flux-system --from-file=age.agekey=/dev/stdin
kubectl create secret generic sops-age -n flux-system --from-file age.agekey
rm age.agekey
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
