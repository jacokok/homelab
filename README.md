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
