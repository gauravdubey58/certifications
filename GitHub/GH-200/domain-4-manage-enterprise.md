# Domain 4 – Manage GitHub Actions in the Enterprise `15%`

> ⬅️ [Back to GH-200 Index](./index.md)

---

## 4.1 Distribute Actions and Workflows to the Enterprise

### Action Distribution Strategies

| Approach | How | Best For |
|----------|-----|----------|
| Public repo | Publish to public GitHub repo | Open source / community |
| Internal repo | Store in internal org repo | Private enterprise actions |
| Reusable workflows | `workflow_call` trigger | Standardized pipelines |
| Required workflows | Set at org level | Compliance enforcement |

### Controlling Action Access (Org Level)

Configure at: **Org → Settings → Actions → General → Actions permissions**

| Policy | Effect |
|--------|--------|
| Allow all actions | Any action from any source |
| GitHub + verified Marketplace | Only GitHub and verified creators |
| Allow selected actions | Specify exact repos or glob patterns |
| Disable all actions | No workflows can run |

**Allow selected actions patterns:**
```
actions/checkout@*          # All versions of specific action
my-org/*@*                  # All actions from your org
docker/*@v2                 # Specific version of docker actions
```

### Reusable Workflows

**Creating a reusable workflow:**
```yaml
# .github/workflows/deploy.yml (in the reusable repo)
name: Reusable Deploy

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
      version:
        required: false
        type: string
        default: 'latest'
    secrets:
      DEPLOY_KEY:
        required: true
    outputs:
      deployment-url:
        description: 'URL of deployment'
        value: ${{ jobs.deploy.outputs.url }}

jobs:
  deploy:
    runs-on: ubuntu-latest
    outputs:
      url: ${{ steps.deploy.outputs.url }}
    steps:
      - name: Deploy
        id: deploy
        run: ./deploy.sh ${{ inputs.environment }}
        env:
          KEY: ${{ secrets.DEPLOY_KEY }}
```

**Calling a reusable workflow:**
```yaml
# In any repo that needs to deploy
jobs:
  deploy-staging:
    uses: my-org/.github/.github/workflows/deploy.yml@main
    with:
      environment: staging
      version: '1.2.3'
    secrets:
      DEPLOY_KEY: ${{ secrets.STAGING_KEY }}

  # OR inherit all secrets automatically
  deploy-prod:
    uses: my-org/.github/.github/workflows/deploy.yml@main
    with:
      environment: production
    secrets: inherit
```

**Reusable workflow rules:**
- Must use `workflow_call` trigger
- Can be nested up to **4 levels** deep
- Caller must have access to the repo containing the reusable workflow
- Secrets must be explicitly passed **OR** use `secrets: inherit`
- Outputs from reusable workflows can be passed back to the caller

### Required Workflows (Enterprise/Org)

- Set at **org level** — automatically added to all matching repos
- **Cannot be disabled** by repo admins
- Common uses: security scans, license checks, compliance gates
- Configure at: Org → Settings → Actions → Required workflows

### Organization Workflow Templates

```
# Structure in org's .github repo
.github/
└── workflow-templates/
    ├── ci-template.yml              # The workflow file
    └── ci-template.properties.json # Metadata
```

```json
// ci-template.properties.json
{
    "name": "CI Pipeline Template",
    "description": "Standard CI pipeline for all org repos",
    "iconName": "ci-icon",
    "categories": ["continuous-integration", "testing"]
}
```

---

## 4.2 Manage Runners for the Enterprise

### GitHub-Hosted vs Self-Hosted Comparison

| Feature | GitHub-Hosted | Self-Hosted |
|---------|--------------|-------------|
| Setup | Zero configuration | Download + configure agent |
| OS options | Ubuntu, Windows, macOS | Any OS |
| Cost | Included minutes + billing | Infrastructure cost only |
| Isolation | Fresh VM every run | Shared (unless ephemeral) |
| Internet access | Full internet | Configurable via proxy |
| Internal network | ❌ No | ✅ Yes |
| Updates | Automatic | Manual or auto-update |
| Hardware specs | Standardized | You choose |
| Security | Managed by GitHub | You are responsible |

### Self-Hosted Runner Groups

- **Purpose:** Access control — restrict which repos use which runners
- **Default group:** All runners not assigned to a specific group
- **Enterprise groups:** Can be shared across organizations
- **Restriction:** A runner can belong to only **one group** at a time

**Managing groups:**
```
Org → Settings → Actions → Runner groups → Manage access
```

### Configuring Self-Hosted Runners

| Setting | How to Configure |
|---------|-----------------|
| Proxy | Set `HTTPS_PROXY` / `HTTP_PROXY` env vars or configure in `.env` file |
| Labels | Add during setup with `--labels` flag or via API afterwards |
| Ephemeral | Use `--ephemeral` flag — runner deregisters after **one** job |
| Networking | Needs outbound HTTPS to `github.com`, `api.github.com`, `*.actions.githubusercontent.com` |

**Ephemeral runners** are recommended for enterprise — each job gets a clean environment, preventing data leakage between runs.

### Runner Labels

```yaml
# Using labels to route jobs
jobs:
  build:
    runs-on: [self-hosted, linux, gpu]    # Must match ALL labels
  test:
    runs-on: [self-hosted, windows, high-memory]
```

> ⭐ **Exam Tip:** A runner with labels `[self-hosted, linux, gpu]` can be targeted with `runs-on: [self-hosted, linux, gpu]`. The job runs on a runner that has **all** the specified labels.

### IP Allow Lists

- If your org uses **IP allow lists**, GitHub-hosted runner IPs must be added
- GitHub publishes its IP ranges via the API:
  ```
  GET https://api.github.com/meta
  ```
- **Self-hosted runners** use YOUR network IPs — easier to manage in allow lists
- The actions themselves (e.g., `actions/checkout`) run **on the runner**, not on GitHub servers — they are not affected by allow lists

### Monitoring and Troubleshooting Self-Hosted Runners

| Task | Where |
|------|-------|
| View runner status | Org/Repo → Settings → Actions → Runners |
| Runner logs | `_diag/` folder in runner installation directory |
| Auto-update | Enabled by default; can be disabled |
| Idle runners | Still connected to GitHub, listening for jobs |
| Offline runners | Jobs queue up or fail depending on timeout |

**Common troubleshooting steps:**
1. Check runner is **online** in Settings → Actions → Runners
2. Check `_diag/` logs for connection errors
3. Verify runner has required **labels** for the job
4. Check **network connectivity** to GitHub endpoints
5. Verify **proxy settings** if behind a corporate proxy

---

## 4.3 Manage Encrypted Secrets in the Enterprise

### Secret Scope Hierarchy

| Scope | Location | Accessible By |
|-------|----------|---------------|
| Enterprise | Enterprise Settings → Secrets | Selected organizations |
| Organization | Org Settings → Secrets | All or selected repos |
| Repository | Repo Settings → Secrets | Workflows in that repo only |
| Environment | Repo Settings → Environments → Secrets | Jobs targeting that environment |

**Precedence (most specific wins):**
```
Environment > Repository > Organization > Enterprise
```

### Accessing Secrets in Workflows

```yaml
# In workflow YAML
env:
  MY_SECRET: ${{ secrets.MY_SECRET }}

steps:
  - name: Use secret
    run: ./deploy.sh
    env:
      API_KEY: ${{ secrets.API_KEY }}
```

### Managing Organization-Level Secrets

- Set at: Org → Settings → Secrets and variables → Actions
- Can be scoped to:
  - All repositories
  - Private repositories only
  - Selected repositories (specify list)

### Managing Repository-Level Secrets

- Set at: Repo → Settings → Secrets and variables → Actions
- Only accessible to workflows in that specific repo
- Repository admins can manage them

### Secret Best Practices

| Practice | Why |
|----------|-----|
| Use environment secrets for env-specific creds | Prevents prod secrets reaching staging jobs |
| Rotate secrets regularly | Limits exposure from accidental leaks |
| Never `echo` secrets directly | Even though they're masked, it's bad practice |
| Limit org secrets to repos that need them | Principle of least privilege |
| Use audit log | Org → Settings → Logs to track secret access |
| Use `GITHUB_TOKEN` where possible | Auto-expires, no manual rotation needed |

> ⭐ **Exam Tip:** Secrets are **not passed to forked repos** by default. PRs from forks cannot access secrets — this is intentional security behaviour. You must explicitly enable this in repo settings if needed (not recommended for public repos).

---

> ⬅️ [← Domain 3](./domain-3-author-maintain-actions.md) | [Back to Index](./index.md) | [⚡ Quick Reference →](./quick-reference.md)
