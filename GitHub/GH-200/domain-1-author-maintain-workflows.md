# Domain 1 – Author and Maintain Workflows `40%`

> ⬅️ [Back to GH-200 Index](./index.md)

This is the **largest domain** — spend the most study time here.

---

## 1.1 Events That Trigger Workflows

Workflows are triggered by events defined under the `on:` key.

### Core Trigger Events

| Event | When It Fires | Common Use |
|-------|--------------|------------|
| `push` | Commit pushed to a branch | CI builds, tests |
| `pull_request` | PR opened, updated, or merged | Validation, code review |
| `schedule` | Cron-based timer | Nightly builds, cleanup |
| `workflow_dispatch` | Manually triggered via UI or API | On-demand deployments |
| `workflow_call` | Called by another workflow | Reusable workflows |
| `workflow_run` | After another workflow completes | Post-CI deployments |
| `release` | Release created or published | Publish packages |
| `issues` | Issue opened, closed, labeled | Automations |

### Webhook Events (less common but exam-relevant)

- `check_run` — check run created, rerequested, or completed
- `check_suite` — check suite created or completed
- `deployment` — deployment created
- `deployment_status` — deployment status updated
- `pull_request_review` — PR review submitted
- `create` / `delete` — branch or tag created/deleted
- `fork` — repository forked
- `gollum` — wiki page updated

### Trigger Syntax

```yaml
# Single event
on: push

# Multiple events
on: [push, pull_request]

# With filters
on:
  push:
    branches: [main, develop]
    branches-ignore: [feature/**]
    paths: ['src/**', '*.yml']
    paths-ignore: ['docs/**']
  pull_request:
    types: [opened, synchronize, reopened]

# Scheduled (cron)
on:
  schedule:
    - cron: '0 2 * * 1'   # Every Monday at 2am UTC

# Manual trigger with inputs
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deploy target'
        required: true
        default: 'staging'
        type: choice
        options: [staging, production]
      debug:
        type: boolean
        default: false
```

> ⭐ **Exam Tip:** Know `branches` vs `branches-ignore` and `paths` vs `paths-ignore`. They cannot be used together in the same event filter.

---

## 1.2 Workflow Components

### Workflow Hierarchy

| Component | Description | Key Detail |
|-----------|-------------|------------|
| Workflow | Top-level `.yml` file in `.github/workflows/` | Triggered by `on:` events |
| Job | Group of steps on the same runner | Runs in parallel by default |
| Step | Individual task inside a job | Uses action OR runs shell command |
| Action | Reusable unit of code | Referenced with `uses:` |
| Run | One execution of a workflow | Has logs, artifacts, status |

### Anatomy of a Workflow File

```yaml
name: CI Pipeline

on: [push]

env:
  GLOBAL_VAR: value    # Available to all jobs

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      JOB_VAR: value   # Available to all steps in job
    steps:
      - uses: actions/checkout@v4       # Action step
      - name: Run tests                 # Shell step
        run: npm test
        env:
          STEP_VAR: value               # Available to this step only
```

### Conditional Execution (`if:`)

```yaml
steps:
  - name: Deploy to prod
    if: github.ref == 'refs/heads/main' && success()
    run: ./deploy.sh

  - name: Always notify
    if: always()
    run: ./notify.sh
```

| Status Function | Meaning |
|----------------|---------|
| `success()` | All previous steps succeeded (default) |
| `failure()` | Any previous step failed |
| `always()` | Always runs, even on failure or cancel |
| `cancelled()` | Workflow was cancelled |

### Dependent Jobs (`needs:`)

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps: [...]

  deploy:
    runs-on: ubuntu-latest
    needs: test              # Waits for 'test' to complete
    steps: [...]

  notify:
    needs: [test, deploy]   # Wait for multiple jobs
    if: always()
    steps: [...]
```

### Matrix Strategy

```yaml
jobs:
  test:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node: [18, 20]
      fail-fast: false     # Don't cancel others on first failure
      max-parallel: 4      # Limit concurrent jobs
    runs-on: ${{ matrix.os }}
    steps:
      - run: node --version
        # Runs 6 times (3 os × 2 node versions)
```

**Matrix include/exclude:**
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    node: [16, 18]
    include:
      - os: ubuntu-latest    # Add extra variable to existing combo
        node: 18
        experimental: true
    exclude:
      - os: windows-latest   # Remove specific combo
        node: 16
```

### Runners

| GitHub-Hosted | Self-Hosted |
|--------------|-------------|
| Managed by GitHub | Managed by you |
| Fresh VM every run | Persistent environment |
| `ubuntu-latest`, `windows-latest`, `macos-latest` | Any OS/hardware |
| Limited minutes per plan | You pay infrastructure cost |
| No internal network access | Can access private resources |
| Auto-updated | You manage updates |

**Routing to specific runners with labels:**
```yaml
jobs:
  build:
    runs-on: [self-hosted, linux, gpu]  # Must match ALL labels
```

### Workflow Commands (Communicating with Runner)

```bash
# Set output (for use by subsequent steps or jobs)
echo "result=success" >> $GITHUB_OUTPUT

# Set environment variable (available to subsequent steps)
echo "MY_VAR=hello" >> $GITHUB_ENV

# Add to PATH
echo "/custom/path" >> $GITHUB_PATH

# Mask a value in logs
echo "::add-mask::$SECRET_VALUE"

# Group log lines
echo "::group::My Group Title"
echo "log lines here"
echo "::endgroup::"

# Set step summary
echo "## Build Results" >> $GITHUB_STEP_SUMMARY
echo "✅ All tests passed" >> $GITHUB_STEP_SUMMARY
```

> ⭐ **Exam Tip:** Use `$GITHUB_OUTPUT` and `$GITHUB_ENV` (append to files). The old `::set-output::` and `::set-env::` syntax is **deprecated**.

---

## 1.3 Encrypted Secrets and Environment Variables

### Secret Scopes

| Scope | Where Set | Accessible By |
|-------|-----------|---------------|
| Repository | Repo → Settings → Secrets | Workflows in that repo |
| Organization | Org → Settings → Secrets | Selected or all repos |
| Environment | Repo → Settings → Environments | Jobs targeting that environment |

### Using Secrets in Workflows

```yaml
steps:
  - name: Use secret
    run: ./deploy.sh
    env:
      API_KEY: ${{ secrets.API_KEY }}
```

### GITHUB_TOKEN

- Automatically created at the **start of every workflow run**
- Scoped to the repo — expires when workflow ends
- Used to authenticate with GitHub API
- Default permissions depend on org settings

```yaml
# Set explicit permissions (principle of least privilege)
permissions:
  contents: read
  pull-requests: write
  packages: write
  issues: write
```

> ⭐ **Exam Tip:** `GITHUB_TOKEN` is NOT a personal access token. It **cannot** access other repos. Use a PAT or GitHub App token for cross-repo access.

### Default Environment Variables

| Variable | Contains |
|----------|----------|
| `GITHUB_REPOSITORY` | `owner/repo` |
| `GITHUB_SHA` | Commit SHA that triggered the run |
| `GITHUB_REF` | Branch/tag ref (`refs/heads/main`) |
| `GITHUB_WORKFLOW` | Workflow name |
| `GITHUB_RUN_ID` | Unique run ID |
| `GITHUB_RUN_NUMBER` | Sequential run number |
| `GITHUB_ACTOR` | User who triggered the workflow |
| `GITHUB_EVENT_NAME` | Event name (`push`, `pull_request`, etc.) |
| `GITHUB_WORKSPACE` | Default working directory |
| `RUNNER_OS` | OS of runner (`Linux`, `Windows`, `macOS`) |

### Setting Custom Environment Variables

```yaml
# Workflow level
env:
  APP_ENV: production

jobs:
  build:
    env:
      JOB_VAR: build_specific    # Job level
    steps:
      - name: Run
        env:
          STEP_VAR: step_only    # Step level (most specific)
        run: echo $STEP_VAR
```

---

## 1.4 Create Workflows for Specific Purposes

### Publish to GitHub Packages (npm)

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    registry-url: 'https://npm.pkg.github.com'

- name: Publish
  run: npm publish
  env:
    NODE_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Publish to GitHub Container Registry (GHCR)

```yaml
- name: Log in to GHCR
  uses: docker/login-action@v3
  with:
    registry: ghcr.io
    username: ${{ github.actor }}
    password: ${{ secrets.GITHUB_TOKEN }}

- name: Build and push
  uses: docker/build-push-action@v5
  with:
    push: true
    tags: ghcr.io/${{ github.repository }}:latest
```

### Service Containers (Databases for Testing)

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: password
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
```

> ⭐ **Exam Tip:** Service containers only work with **Linux runners**.

### CodeQL Security Scanning

```yaml
- uses: github/codeql-action/init@v3
  with:
    languages: javascript, python

- uses: github/codeql-action/autobuild@v3

- uses: github/codeql-action/analyze@v3
```

### Creating GitHub Releases

```yaml
- name: Create Release
  uses: actions/create-release@v1
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  with:
    tag_name: ${{ github.ref }}
    release_name: Release ${{ github.ref }}
    draft: false
    prerelease: false
```

### Deploying to Cloud Providers

```yaml
# AWS example
- uses: aws-actions/configure-aws-credentials@v4
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: us-east-1

# Azure example
- uses: azure/login@v1
  with:
    creds: ${{ secrets.AZURE_CREDENTIALS }}
```

---

> ⬅️ [Back to Index](./index.md) | ➡️ [Domain 2 →](./domain-2-consume-workflows.md)
