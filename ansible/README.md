# Ansible

Automates OS-level provisioning for the homelab nodes: system updates, package
installs, k3s installation (master + workers), and registry configuration.

> Fluxcd owns the in-cluster Kubernetes manifests (`../cluster`, `../infrastructure`,
> `../apps`). Ansible only provisions the nodes underneath the cluster.

## Setup (with mise)

[mise](https://mise.jdx.dev/) pins `python` and `ansible-core` for the project.
From the repository root:

```bash
mise install      # installs python + ansible-core (pipx backend)
mise trust        # one-time, to allow mise.toml in this repo
eval "$(mise activate bash)"  # or let your shell hook do it
ansible --version
```

The root `mise.toml` sets `ANSIBLE_CONFIG` to `ansible/ansible.cfg`, so commands
can be run from the `ansible/` directory.

## Inventory

Edit `inventory.yml` to match your nodes. From the existing `k3s.md`:

- `k3s_masters` — the manager node (`earth`)
- `k3s_workers` — agent nodes that join the master

## Usage

All commands run from `ansible/`.

```bash
# Everything (update -> k3s -> registries)
ansible-playbook playbooks/site.yml

# Just one stage
ansible-playbook playbooks/system.yml     # apt update/upgrade + packages + iscsid
ansible-playbook playbooks/k3s.yml        # install k3s master + join workers
ansible-playbook playbooks/registries.yml # write /etc/rancher/k3s/registries.yaml
```

## Secrets

Keep credentials out of git. Put them in a vault-encrypted file:

```bash
ansible-vault create group_vars/vault.yml
# add e.g.:
# registry_username: ...
# registry_password: ...
# k3s_token: ...
```

Then reference the variables from the playbooks. Run with:

```bash
ansible-playbook playbooks/site.yml --ask-vault-pass
```

## SSH

Ansible uses SSH directly. Make sure:

1. Your SSH key is authorized on each node (`~/.ssh/authorized_keys` for
   `ansible_user`, default `doink` in `group_vars/all.yml`).
2. The nodes are reachable by the hostnames in `inventory.yml` (add them to your
   SSH config or `/etc/hosts`).

This covers the "Ansible ssh" TODO: once passwordless sudo + key auth work,
the playbooks need no interactive credentials.
