# AGENTS.md

## General

- Do not add comments unless absolutely necessary.

## Fluxcd

- This project uses fluxcd to manage the deployment of applications and infrastructure in a GitOps manner. Fluxcd continuously monitors the Git repository for changes and applies them to the cluster automatically.
- Try not to make direct kubectl edits if not needed, instead make changes to the files and let fluxcd handle the deployment.

## Dependency Updates

- Dependency management is done with Renovate (`renovate.json`). Flux auto-updates are disabled: HelmRelease chart versions are pinned to exact versions and git sources use release tags.
- Renovate auto-merges minor/patch updates and opens PRs for major updates. Do not widen HelmRelease version constraints or revert to `:latest`/branch tracking; update versions by letting Renovate bump the pinned values.
