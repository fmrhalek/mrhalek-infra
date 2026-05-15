# mrhalek-infra

Ansible configuration for the EC2 host serving https://mrhalek.xyz.

## Hosts

| Host | Role |
|---|---|
| `web` (EIP `3.125.221.7`, `eu-central-1`) | Docker host + nginx reverse proxy for [motion-site](https://github.com/fmrhalek/motion-site) |

## Local usage

```bash
ansible-galaxy collection install -r requirements.yml
ansible-playbook playbooks/site.yml --check --diff   # dry-run
ansible-playbook playbooks/site.yml                   # apply
```

Requires `~/.ssh/ansible-admin` private key.

## CI flow

- **PR → main**: `.github/workflows/plan.yml` runs `ansible-playbook --check --diff` and posts the diff as a PR comment.
- **Merge to main**: `.github/workflows/apply.yml` runs the real playbook, **gated on the `production` GitHub Environment requiring your approval**.

### Required GitHub Secrets (repo Settings → Secrets and variables → Actions)

| Name | Value |
|---|---|
| `ANSIBLE_SSH_KEY` | contents of `~/.ssh/ansible-admin` (private) |
| `EC2_HOST` | `3.125.221.7` |

### Required GitHub Environment

- Settings → Environments → `production` → tick **Required reviewers** → add yourself.
