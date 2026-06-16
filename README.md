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
