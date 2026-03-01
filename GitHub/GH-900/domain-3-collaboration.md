# Domain 3 – Collaboration Features `30%`

> ⬅️ [Back to GH-900 Index](./index.md)

This is the **largest domain** — master the GitHub collaboration workflow.

---

## 3.1 Forking Repositories

### What is a Fork?
- A **personal copy of someone else's repository** under your own account
- Allows you to freely experiment without affecting the original ("upstream") repo
- Changes in your fork don't affect the original — you propose changes back via Pull Request

### Fork vs Clone

| | Fork | Clone |
|--|------|-------|
| Where | GitHub server (your account) | Your local machine |
| Purpose | Contribute to repos you don't have write access to | Work locally on repos you have access to |
| Relationship | Linked to upstream repo | Direct link to remote |

### Keeping a Fork in Sync
```bash
git remote add upstream https://github.com/original-owner/repo.git
git fetch upstream
git merge upstream/main
```

Or via GitHub: Repo page → **"Sync fork"** button

---

## 3.2 Pull Requests

### The GitHub Flow (Standard Collaboration Workflow)

```
1. Create a branch → 2. Make commits → 3. Open a Pull Request →
4. Review and discuss → 5. Deploy (if needed) → 6. Merge
```

### Creating a Pull Request
- From a branch or fork → click **"New pull request"**
- Select: **base branch** (target) and **compare branch** (your changes)
- Add: title, description, linked issues, reviewers, labels, milestone

### PR States
| State | Description |
|-------|-------------|
| **Open** | Active PR, accepting reviews |
| **Draft** | Work in progress — not ready for review |
| **Closed** | Closed without merging |
| **Merged** | Successfully merged into base branch |

### Linking Issues to PRs
Use keywords in PR description to auto-close issues on merge:
- `Closes #123`
- `Fixes #456`
- `Resolves #789`

### Merge Strategies

| Strategy | Creates merge commit? | History |
|----------|----------------------|---------|
| **Create a merge commit** | ✅ Yes | Preserves full branch history |
| **Squash and merge** | New single commit | Squashes all PR commits into one clean commit |
| **Rebase and merge** | ❌ No | Replays commits linearly onto base branch |

---

## 3.3 Code Reviews

### Requesting Reviews
- In a PR → **Reviewers** section → add team members or `@mention`
- **CODEOWNERS** file automatically requests reviews from designated owners
- Organisations can require a minimum number of approving reviews

### Review Actions
| Action | Meaning |
|--------|---------|
| **Approve** | LGTM — ready to merge |
| **Request changes** | Must address feedback before merging |
| **Comment** | Feedback without blocking |

### Review Tools
- **Inline comments** — comment on specific lines of code
- **Suggestions** — propose exact code changes the author can accept with one click
- **Conversation threads** — discussions on specific lines; can be resolved
- **Review summary** — overall comment on the entire PR

### Code Review Best Practices
- Review the intent, not just the syntax
- Be constructive and specific in feedback
- Keep PRs small and focused
- Respond to all comments before merging
- Use suggestions for small changes

---

## 3.4 Issues

### Creating and Managing Issues
- **New Issue** → Choose template (if available) → Fill in title and description
- Can include: images, code blocks, checklists, @mentions, emoji

### Issue Organisation

| Feature | Purpose |
|---------|---------|
| **Labels** | Categorise issues (e.g., `bug`, `enhancement`, `good first issue`, `help wanted`) |
| **Assignees** | Assign responsibility to one or more team members |
| **Milestones** | Group issues targeting a specific release or date |
| **Projects** | Add to a project board for visual tracking |

### Issue Templates
- Stored in `.github/ISSUE_TEMPLATE/`
- Guide contributors to provide the right information (bug report, feature request)
- Reduces low-quality issues

### Closing Issues
- Manually: Close Issue button
- Automatically: `Closes #123` in a PR description (closed when PR is merged)

---

## 3.5 GitHub Actions for CI/CD

### What is GitHub Actions?
- Built-in **CI/CD and automation platform** on GitHub
- Define workflows in YAML files in `.github/workflows/`
- Triggered by: push, pull request, schedule, manual, events

### Core Concepts

| Concept | Description |
|---------|-------------|
| **Workflow** | YAML file defining automated processes |
| **Event** | Trigger that starts a workflow (push, PR, schedule) |
| **Job** | A set of steps that run on one runner |
| **Step** | A single command or action within a job |
| **Action** | A reusable unit of code (from Marketplace or custom) |
| **Runner** | The machine that executes jobs (GitHub-hosted or self-hosted) |

### Example Workflow
```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: npm test
```

### GitHub Actions Marketplace
- 10,000+ pre-built actions for common tasks
- Examples: `actions/checkout`, `actions/setup-node`, `docker/build-push-action`

### CI vs CD

| | CI (Continuous Integration) | CD (Continuous Delivery/Deployment) |
|--|----------------------------|-------------------------------------|
| Goal | Automatically build and test code on every push | Automatically deploy tested code to environments |
| Trigger | Push to branch / PR | After CI passes |
| Output | Test results, build artifacts | Deployed application |

---

> ⬅️ [← Domain 2](./domain-2-repositories.md) | [Back to Index](./index.md) | [Domain 4 →](./domain-4-modern-development.md)
