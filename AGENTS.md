# AGENTS.md

## Fluxcd

- This project uses fluxcd to manage the deployment of applications and infrastructure in a GitOps manner. Fluxcd continuously monitors the Git repository for changes and applies them to the cluster automatically.
- Try not to make direct kubectl edits if not needed, instead make changes to the files and let fluxcd handle the deployment.
