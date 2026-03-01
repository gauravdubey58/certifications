# ⚡ GH-200 Quick Reference Cheat Sheet

> ⬅️ [Back to GH-200 Index](./index.md)

Your last-minute revision guide — everything you need at a glance.

---

## 🗂️ Complete Workflow Template

```yaml
name: Full Example Workflow

on:
  push:
    branches: [main]
    paths: ['src/**']
  pull_request:
    types: [opened, synchronize]
  schedule:
    - cron: '0 9 * * 1-5'      # Mon–Fri 9am UTC
  workflow_dispatch:
    inputs:
      deploy_env:
        type: choice
        options: [dev, staging, prod]
        default: dev

env:
  GLOBAL_VAR: value             # Available to all jobs and steps

jobs:
  test:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    outputs:
      version: ${{ steps.get_ver.outputs.version }}
    steps:
      - uses: actions/checkout@v4

      - uses: actions/cache@v4
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}

      - id: get_ver
        run: echo "version=$(cat VERSION)" >> $GITHUB_OUTPUT

      - name: Run tests
        run: npm test
        env:
          SECRET_VAL: ${{ secrets.MY_SECRET }}

      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: test-results
          path: coverage/

  deploy:
    needs: test
    environment: production       # Triggers approval gate
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying version ${{ needs.test.outputs.version }}"
```

---

## 🎯 Most Tested Topics

| Topic | Key Point to Remember |
|-------|-----------------------|
| Workflow triggers | `push`, `pull_request`, `schedule` (cron), `workflow_dispatch` (manual), `workflow_call` (reusable) |
| Secret scopes | Enterprise → Org → Repo → Environment (most specific wins) |
| GITHUB_TOKEN | Auto-created per run, repo-scoped, expires on completion. NOT a PAT |
| Forks + secrets | Forks cannot access secrets by default — security feature |
| Outputs (steps) | `echo "key=value" >> $GITHUB_OUTPUT` |
| Outputs (jobs) | `outputs:` block + `needs.jobname.outputs.key` syntax |
| Env vars | `echo "KEY=value" >> $GITHUB_ENV` — available to subsequent steps |
| Cache | `actions/cache`; key must match exactly; 10 GB limit; 7-day expiry |
| Artifacts vs Cache | Artifacts = share files; Cache = speed up dependency installs |
| Disable vs Delete | Disable = stops runs, keeps file + history; Delete = removes everything |
| Runners | `runs-on: [label1, label2]` — runner must match ALL labels |
| Action types | JS (fast, cross-platform), Docker (any lang, Linux only), Composite (YAML steps) |
| action.yml | Required for ALL custom actions. Defines `inputs`, `outputs`, `runs` |
| Reusable workflows | `workflow_call` trigger; call with `uses: org/repo/.github/workflows/file.yml@ref` |
| Matrix | `include` adds combos; `exclude` removes them; `fail-fast: false` lets all run |
| Debug logging | Set secret `ACTIONS_STEP_DEBUG=true` — via secrets, not env vars |
| IP allow lists | Add GitHub-hosted runner IPs from `api.github.com/meta` |
| Environment protection | Required reviewers, wait timer, branch rules — job must reference `environment: name` |
| Service containers | Linux runners only; great for databases in testing |
| CodeQL | 3 steps: `init` → `autobuild` → `analyze` |

---

## 📋 Trigger Event Quick Reference

```yaml
on:
  push:               # Code pushed
  pull_request:       # PR activity
  release:            # Release created/published
  schedule:           # Cron job
  workflow_dispatch:  # Manual button click
  workflow_call:      # Called by another workflow
  workflow_run:       # After another workflow finishes
  issues:             # Issue activity
  issue_comment:      # Comment on issue/PR
  check_run:          # Check run created/completed
  check_suite:        # Check suite activity
  deployment:         # Deployment created
  deployment_status:  # Deployment status changed
  create:             # Branch/tag created
  delete:             # Branch/tag deleted
  fork:               # Repo forked
  label:              # Label activity
  milestone:          # Milestone activity
  page_build:         # GitHub Pages built
  project:            # Project activity
  pull_request_review: # PR review submitted
  registry_package:   # Package published/updated
  status:             # Commit status changed
  watch:              # Repo starred
  gollum:             # Wiki updated
```

---

## 🔑 Context Variables Quick Reference

```yaml
${{ github.event_name }}     # 'push', 'pull_request', etc.
${{ github.ref }}            # 'refs/heads/main'
${{ github.sha }}            # Full commit SHA
${{ github.actor }}          # Username who triggered
${{ github.repository }}     # 'owner/repo'
${{ github.run_id }}         # Unique run ID
${{ github.run_number }}     # Sequential run number
${{ github.workflow }}       # Workflow name
${{ github.job }}            # Current job ID
${{ github.workspace }}      # Working directory path
${{ runner.os }}             # 'Linux', 'Windows', 'macOS'
${{ runner.temp }}           # Temp directory
${{ secrets.NAME }}          # Access a secret
${{ vars.NAME }}             # Access a variable (non-secret)
${{ env.NAME }}              # Access env variable
${{ needs.jobname.outputs.key }}  # Output from dependency job
${{ matrix.property }}       # Current matrix value
${{ inputs.name }}           # workflow_dispatch / workflow_call input
```

---

## 🐚 Workflow Commands Cheat Sheet

```bash
# Outputs
echo "key=value" >> $GITHUB_OUTPUT

# Environment variables (for subsequent steps)
echo "MY_VAR=hello" >> $GITHUB_ENV

# Add to PATH
echo "/my/path" >> $GITHUB_PATH

# Job summary
echo "## Results" >> $GITHUB_STEP_SUMMARY

# Mask a secret value
echo "::add-mask::$MY_VALUE"

# Annotations
echo "::error file=main.js,line=1::Error message"
echo "::warning::Warning message"
echo "::notice::Notice message"
echo "::debug::Debug message"

# Log grouping
echo "::group::Group Name"
# ... log lines ...
echo "::endgroup::"
```

---

## 🏃 Runner Labels Cheat Sheet

```yaml
# GitHub-hosted
runs-on: ubuntu-latest
runs-on: ubuntu-22.04
runs-on: windows-latest
runs-on: macos-latest
runs-on: macos-14          # Apple Silicon

# Self-hosted (must match ALL labels)
runs-on: [self-hosted, linux]
runs-on: [self-hosted, windows, gpu]
runs-on: [self-hosted, linux, x64, high-memory]
```

---

## 📦 Common Actions Reference

```yaml
# Checkout code
- uses: actions/checkout@v4

# Setup language runtimes
- uses: actions/setup-node@v4
  with:
    node-version: '20'
- uses: actions/setup-python@v5
  with:
    python-version: '3.12'
- uses: actions/setup-java@v4
  with:
    java-version: '21'

# Cache
- uses: actions/cache@v4

# Artifacts
- uses: actions/upload-artifact@v4
- uses: actions/download-artifact@v4

# Docker
- uses: docker/login-action@v3
- uses: docker/build-push-action@v5

# Security
- uses: github/codeql-action/init@v3
- uses: github/codeql-action/autobuild@v3
- uses: github/codeql-action/analyze@v3

# Release
- uses: actions/create-release@v1
```

---

## ✅ Exam Day Checklist

- [ ] Know all trigger event types and their filters
- [ ] Understand `GITHUB_TOKEN` vs PAT vs environment secrets
- [ ] Can write `needs:` chains and pass outputs between jobs
- [ ] Know difference between artifacts and cache
- [ ] Understand `disable` vs `delete` for workflows
- [ ] Know all 3 action types and when to use each
- [ ] Understand `action.yml` structure and required fields
- [ ] Know self-hosted vs GitHub-hosted runner differences
- [ ] Understand secret scoping and precedence
- [ ] Know reusable workflow syntax (`workflow_call`, `uses:`, `secrets: inherit`)
- [ ] Understand matrix `include` / `exclude` / `fail-fast`
- [ ] Know how to enable debug logging (`ACTIONS_STEP_DEBUG` secret)
- [ ] Understand environment protection rules and approval gates

---

> Good luck! 🍀 You've got this.

> ⬅️ [← Domain 4](./domain-4-manage-enterprise.md) | [Back to Index](./index.md)
