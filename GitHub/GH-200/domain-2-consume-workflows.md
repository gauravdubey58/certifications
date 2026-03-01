# Domain 2 – Consume Workflows `20%`

> ⬅️ [Back to GH-200 Index](./index.md)

---

## 2.1 Interpret Workflow Effects

### Accessing Workflow Logs

**Via UI:**
1. Go to the **Actions** tab in the repository
2. Select the workflow run
3. Select a job
4. Expand any step to see its logs

**Via REST API:**
```
GET /repos/{owner}/{repo}/actions/runs/{run_id}/logs
GET /repos/{owner}/{repo}/actions/jobs/{job_id}/logs
```

- Log **retention default:** 90 days (configurable at repo/org level)
- Logs can be downloaded as a `.zip` file from the UI

### Identifying What Triggered a Workflow

```yaml
# Access trigger info in workflow
${{ github.event_name }}     # 'push', 'pull_request', 'schedule', etc.
${{ github.actor }}          # Who triggered it
${{ github.ref }}            # Branch/tag ref
${{ github.event }}          # Full event payload
```

### Enable Step Debug Logging

| Method | How |
|--------|-----|
| Secret — step debug | Add secret `ACTIONS_STEP_DEBUG = true` |
| Secret — runner diagnostic | Add secret `ACTIONS_RUNNER_DEBUG = true` |
| Re-run with debug | Actions tab → re-run → check **"Enable debug logging"** |

> ⭐ **Exam Tip:** Debug logging is enabled via **secrets**, not environment variables — so it works even when the workflow itself can't be edited easily.

### Diagnosing Failed Runs

- Click the **red ✗** on the failing step for the error message
- Use **"Re-run failed jobs"** to retry only failed jobs without rerunning successful ones
- Check if **secrets are missing** (they show as empty strings, not errors)
- Check **runner availability** for self-hosted runner issues
- **Skipped steps** show as grey — different from failed (red) or cancelled

### Step Status Meanings

| Status | Icon | Meaning |
|--------|------|---------|
| Success | ✅ Green | Step completed successfully |
| Failure | ❌ Red | Step failed (non-zero exit code) |
| Skipped | ⏭️ Grey | `if:` condition was false |
| Cancelled | 🚫 Grey | Run was cancelled |

---

## 2.2 Manage Workflow Runs

### Caching Dependencies

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

**Key rules:**
- `key` must **match exactly** to restore the cache
- `restore-keys` are **fallback prefixes** — used if exact key not found
- Cache is scoped to the **branch** — `main` branch cache is accessible to feature branches (not vice versa by default)
- **Size limit:** 10 GB per repository
- **Eviction:** entries removed after **7 days** of no access

**Common cache paths:**

| Ecosystem | Path to cache |
|-----------|--------------|
| Node.js (npm) | `~/.npm` |
| Node.js (yarn) | `~/.yarn/cache` |
| Python (pip) | `~/.cache/pip` |
| Java (Maven) | `~/.m2` |
| Ruby (gems) | `vendor/bundle` |

### Passing Data Between Jobs

```yaml
jobs:
  job1:
    runs-on: ubuntu-latest
    outputs:
      version: ${{ steps.get_version.outputs.version }}
    steps:
      - id: get_version
        run: echo "version=1.2.3" >> $GITHUB_OUTPUT

  job2:
    runs-on: ubuntu-latest
    needs: job1
    steps:
      - run: echo "Version is ${{ needs.job1.outputs.version }}"
```

### Artifacts

**Upload:**
```yaml
- uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: dist/
    retention-days: 5       # Override default (90 days)
```

**Download:**
```yaml
- uses: actions/download-artifact@v4
  with:
    name: build-output
    path: ./downloaded/    # Optional: where to download to
```

**Key differences — Artifacts vs Cache:**

| | Artifacts | Cache |
|--|-----------|-------|
| Purpose | Share files between jobs/runs | Speed up builds |
| Retention | 90 days (default) | 7 days since last access |
| Scope | Workflow runs | Branch-scoped |
| Size limit | No hard limit (repo storage) | 10 GB per repo |

**Remove artifacts:**
- UI: Actions tab → run → Artifacts section → Delete
- API: `DELETE /repos/{owner}/{repo}/actions/artifacts/{artifact_id}`

### Workflow Status Badge

```markdown
![CI](https://github.com/{owner}/{repo}/actions/workflows/{workflow-file}.yml/badge.svg)

# With branch filter
![CI](https://github.com/{owner}/{repo}/actions/workflows/{workflow-file}.yml/badge.svg?branch=main)
```

### Environment Protections

Configure at: Repo → Settings → Environments → (select environment)

| Protection | Description |
|-----------|-------------|
| Required reviewers | Named people must approve before job proceeds |
| Wait timer | Delay up to **30 days** before job starts |
| Deployment branches | Only specific branches can deploy to this env |
| Environment secrets | Secrets only accessible to jobs targeting this env |

### Approval Gates (using environments)

```yaml
jobs:
  deploy-production:
    environment: production    # Triggers required reviewers approval gate
    runs-on: ubuntu-latest
    steps:
      - run: ./deploy.sh
```

> ⭐ **Exam Tip:** The approval gate triggers automatically when a job references an `environment:` that has required reviewers configured. No extra syntax needed.

### Matrix Job Configuration

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    node: [18, 20]
    include:
      - os: ubuntu-latest    # Add extra data to specific combo
        node: 20
        experimental: true
    exclude:
      - os: windows-latest   # Remove a specific combination
        node: 18
  fail-fast: false           # All matrix jobs continue even if one fails
  max-parallel: 3            # Max concurrent jobs at once
```

---

## 2.3 Locate Workflows, Logs, and Artifacts

### Workflow File Location

All workflow files must be in:
```
.github/workflows/*.yml
.github/workflows/*.yaml
```

### Disable vs Delete a Workflow

| Action | Effect | When to Use |
|--------|--------|-------------|
| **Disable** | Workflow stops running; file stays; history preserved | Temporary pause |
| **Delete** | File removed; all run history gone | Permanent removal |

**To disable:** Actions tab → select workflow (left sidebar) → click `...` → **Disable workflow**

> ⭐ **Exam Tip:** This is a common exam question! Disabling preserves the file AND history. Deleting the `.yml` file removes both.

### Downloading Artifacts

**Via UI:**  
Actions tab → click a run → scroll to **Artifacts** section → click to download

**Via API:**
```
GET /repos/{owner}/{repo}/actions/runs/{run_id}/artifacts
GET /repos/{owner}/{repo}/actions/artifacts/{artifact_id}/zip
```

### Organization Templated Workflows

- Stored in the **`.github` repository** at org level
- Files go in: `.github/workflow-templates/`
- Each template needs:
  - `my-template.yml` — the workflow file
  - `my-template.properties.json` — metadata (name, description, categories)
- Members see them under **Actions → New workflow** in any org repo

```json
// my-template.properties.json
{
    "name": "My Org CI Template",
    "description": "Standard CI pipeline for all org repos",
    "iconName": "example-icon",
    "categories": ["continuous-integration"]
}
```

---

> ⬅️ [← Domain 1](./domain-1-author-maintain-workflows.md) | [Back to Index](./index.md) | [Domain 3 →](./domain-3-author-maintain-actions.md)
