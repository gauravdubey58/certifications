# ⚡ GH-900 Quick Reference Cheat Sheet

> ⬅️ [Back to GH-900 Index](./index.md)

---

## 🔧 Essential Git Commands

| Command | What It Does |
|---------|-------------|
| `git init` | Initialise a new local repo |
| `git clone <url>` | Clone a remote repo locally |
| `git status` | Show working tree status |
| `git add .` | Stage all changes |
| `git add <file>` | Stage a specific file |
| `git commit -m "msg"` | Commit staged changes |
| `git push origin main` | Push commits to remote |
| `git pull` | Fetch + merge from remote |
| `git fetch` | Fetch from remote (don't merge) |
| `git branch` | List branches |
| `git branch <name>` | Create a new branch |
| `git checkout <branch>` | Switch to a branch |
| `git checkout -b <name>` | Create and switch to new branch |
| `git switch <branch>` | Modern way to switch branches |
| `git merge <branch>` | Merge branch into current |
| `git rebase <branch>` | Reapply commits on top of another branch |
| `git log --oneline` | Compact commit history |
| `git diff` | Show unstaged changes |
| `git stash` | Temporarily save uncommitted changes |
| `git stash pop` | Restore stashed changes |
| `git revert <hash>` | Undo a commit safely (new commit) |
| `git reset --hard HEAD~1` | Undo last commit (destructive) |
| `git tag v1.0.0` | Create a tag |

---

## 🔀 GitHub Workflow at a Glance

```
Fork (for open source) OR Clone (for your own repo)
         ↓
Create a new feature branch
         ↓
Make changes, commit locally
         ↓
Push branch to GitHub
         ↓
Open a Pull Request
         ↓
Code review (request reviewers, address feedback)
         ↓
CI checks pass (GitHub Actions)
         ↓
Merge PR → branch deleted
         ↓
Issue closed (if linked with "Closes #123")
```

---

## 🔑 Key GitHub Concepts

| Concept | What It Is |
|---------|-----------|
| **Repository** | A project's entire history, files, and configuration |
| **Fork** | Your own copy of another repo; used for contributing to others' code |
| **Clone** | A local copy of a remote repo on your machine |
| **Branch** | An isolated line of development |
| **Commit** | A snapshot of changes with a message |
| **Pull Request (PR)** | Request to merge one branch into another; enables code review |
| **Issue** | A tracked task, bug report, or feature request |
| **Label** | Tag on an issue/PR for filtering and categorisation |
| **Milestone** | A group of issues/PRs tracking progress towards a goal |
| **GitHub Projects** | Kanban/Table/Roadmap view for tracking work |
| **Release** | A versioned build published from a tag |
| **Gist** | A lightweight shareable code snippet |
| **GitHub Pages** | Host a static website from a repository |
| **GitHub Actions** | CI/CD and automation workflows |
| **Dependabot** | Auto-PRs for dependency updates and security fixes |
| **CODEOWNERS** | Auto-assign reviewers based on changed file paths |
| **Discussions** | Forum-style conversation in a repo |
| **GitHub Codespaces** | Cloud-based VS Code development environment |

---

## 🔐 Security Features Summary

| Feature | What It Does | Where to Enable |
|---------|-------------|-----------------|
| **Branch protection rules** | Require reviews, status checks; prevent force-push | Repo → Settings → Branches |
| **Dependabot alerts** | Notify about vulnerable dependencies | Repo → Settings → Security |
| **Dependabot security updates** | Auto-PR to fix vulnerable deps | Enabled with alerts |
| **Dependabot version updates** | Auto-PR to keep deps current | `.github/dependabot.yml` |
| **Secret scanning** | Detect leaked API keys/tokens | Repo → Settings → Security |
| **Push protection** | Block pushes containing secrets | Repo → Settings → Security |
| **Code scanning (CodeQL)** | Find code vulnerabilities | Actions → Security → CodeQL |
| **2FA enforcement** | Require MFA for all org members | Org → Settings → Authentication |
| **CODEOWNERS** | Auto-assign code reviewers | `.github/CODEOWNERS` |

---

## 🏢 Repository Roles

| Role | Read | Write | Settings |
|------|------|-------|----------|
| Read | ✅ | ❌ | ❌ |
| Triage | ✅ | Issues/PRs | ❌ |
| Write | ✅ | ✅ | ❌ |
| Maintain | ✅ | ✅ | Partial |
| Admin | ✅ | ✅ | ✅ Full |

---

## 👥 Organization Roles

| Role | Access |
|------|--------|
| **Owner** | Full admin — org settings, billing, all repos |
| **Member** | Can create repos (if allowed), join teams |
| **Billing Manager** | Billing only |
| **Security Manager** | Manage security alerts across all repos |

---

## 📋 Community Health Files

| File | Purpose |
|------|---------|
| `README.md` | Project overview, setup, usage |
| `CONTRIBUTING.md` | How to contribute |
| `CODE_OF_CONDUCT.md` | Community behaviour standards |
| `LICENSE` | Legal usage terms |
| `SECURITY.md` | How to report vulnerabilities |
| `CHANGELOG.md` | History of changes |

---

## 🔖 Issue and PR Keywords

### Auto-close Issues from PRs
When merged, these keywords close the referenced issue automatically:
```
Closes #123
Fixes #456
Resolves #789
```

### PR Review States
| State | Meaning |
|-------|---------|
| **Approve** | LGTM — ready to merge |
| **Request changes** | Must address feedback before merge |
| **Comment** | Neutral feedback, no blocking |

---

## 🎯 Top Exam Topics

| Topic | Key Point |
|-------|-----------|
| Fork vs Clone | Fork = your own GitHub copy; Clone = local copy on your machine |
| PR review states | Approve / Request changes / Comment |
| Branch protection | Require reviews + status checks before merge |
| `good first issue` label | Attract new open-source contributors |
| Dependabot | Alerts = notify; Security updates = auto-PR for vulnerable deps; Version updates = auto-PR to keep all deps current |
| Secret scanning | Detect leaked credentials; Push protection blocks secrets before they reach the repo |
| CODEOWNERS | `.github/CODEOWNERS` — auto-assign reviewers by file path |
| GitHub Discussions | For open-ended Q&A and conversation; Issues are for tasks/bugs |
| `Closes #123` | Automatically closes the referenced issue when the PR is merged |
| Milestones | Group issues/PRs towards a release goal; shows % complete |
| InnerSource | Open-source practices within a private organisation |
| MIT licence | Most permissive; GPL is copyleft |
| GitHub Actions triggers | `push`, `pull_request`, `schedule`, `workflow_dispatch` |
| Draft PRs | Signal WIP; reviews not required; cannot be merged until ready |
| `.github/` directory | Special directory for Actions, templates, CODEOWNERS, Copilot instructions |
| Release vs Tag | Tag is a pointer to a commit; Release is a tag + assets + notes |

---

> 📝 [Practice with 50 MCQs →](./mcqs.md) | ⬅️ [Back to Index](./index.md)
