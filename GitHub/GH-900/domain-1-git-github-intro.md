# Domain 1 – Introduction to Git and GitHub `22%`

> ⬅️ [Back to GH-900 Index](./index.md)

---

## 1.1 What is Git?

- **Git** is a **distributed version control system (DVCS)** — tracks changes to files over time
- Created by Linus Torvalds in 2005
- Every developer has a **full copy of the repository** (history included) — no single point of failure
- **Open source** and free to use
- Works offline — you can commit locally without internet access

### Why Use Git?
- Track every change ever made to code
- Revert to any previous version
- Work in parallel via branching without affecting others
- Collaborate across teams and geographies

### Git vs GitHub

| | Git | GitHub |
|--|-----|--------|
| What it is | Version control software | Cloud platform hosting Git repos |
| Where it runs | Your local machine | Microsoft's cloud (github.com) |
| Purpose | Track changes locally | Collaborate, share, review, automate |
| Requires internet? | ❌ No | ✅ Yes |

---

## 1.2 Core Git Concepts

### Repository (Repo)
- A folder that Git tracks — contains all project files plus the `.git` directory (history, branches, config)
- **Local repo** — on your machine
- **Remote repo** — hosted on GitHub, GitLab, Bitbucket, etc.

### The Three States of Git

```
Working Directory → Staging Area (Index) → Repository (Committed)
      (edit files)   (git add)                (git commit)
```

| State | Description |
|-------|-------------|
| **Working Directory** | Where you edit files — untracked changes |
| **Staging Area (Index)** | Files staged for the next commit (`git add`) |
| **Repository (.git)** | Committed history — permanent snapshots |

### Commits
- A **snapshot** of staged changes at a point in time
- Each commit has: unique SHA hash, author, timestamp, message, pointer to parent commit
- Commits form a **chain (history)**

### Branches
- A **lightweight movable pointer** to a specific commit
- Default branch: `main` (or historically `master`)
- Branches allow **parallel development** without affecting the main codebase
- **HEAD** — pointer to the currently checked-out branch/commit

### Merging vs Rebasing

| Operation | What It Does | Result |
|-----------|-------------|--------|
| **Merge** | Combines two branches; creates a merge commit | Preserves full history |
| **Fast-forward merge** | Moves pointer forward — only possible if no divergent changes | Linear history, no merge commit |
| **Rebase** | Replays commits on top of another branch | Cleaner linear history; rewrites commit SHAs |

---

## 1.3 Essential Git Commands

### Setup
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### Repository Operations
```bash
git init                    # Initialise a new local repo
git clone <url>             # Clone a remote repo locally
git status                  # Show current state of working directory
git log                     # Show commit history
git log --oneline           # Compact commit history
```

### Making Changes
```bash
git add <file>              # Stage a specific file
git add .                   # Stage all changes
git commit -m "message"     # Commit staged changes with message
git commit --amend          # Modify the last commit (before pushing)
```

### Branching
```bash
git branch                  # List branches
git branch <name>           # Create a new branch
git switch <name>           # Switch to a branch (modern)
git checkout <name>         # Switch to a branch (classic)
git switch -c <name>        # Create and switch to new branch
git branch -d <name>        # Delete a branch (safe)
git branch -D <name>        # Force delete a branch
```

### Remote Operations
```bash
git remote -v               # List remote connections
git remote add origin <url> # Add a remote called 'origin'
git fetch                   # Download remote changes (don't merge)
git pull                    # Fetch + merge remote changes
git push origin <branch>    # Push a branch to remote
git push -u origin main     # Push and set upstream tracking
```

### Merging
```bash
git merge <branch>          # Merge a branch into current branch
git merge --no-ff <branch>  # Force merge commit even if fast-forward possible
git rebase <branch>         # Rebase current branch onto specified branch
```

### Undoing
```bash
git restore <file>          # Discard unstaged changes to a file
git restore --staged <file> # Unstage a file (keep changes)
git reset HEAD~1            # Undo last commit (keep changes staged)
git reset --hard HEAD~1     # Undo last commit and discard all changes
git revert <commit-sha>     # Create new commit that undoes a previous commit (safe for shared history)
```

> ⭐ **Exam Tip:** `git revert` is safe for shared/remote history — it adds a new commit. `git reset --hard` rewrites history — dangerous after pushing.

---

## 1.4 Navigating GitHub

### Creating a GitHub Account
- Sign up at github.com
- Choose: username, email, password
- Account types: **Personal**, **Organization**

### GitHub Repository Structure
- **Code** — browse files and commits
- **Issues** — track bugs and feature requests
- **Pull Requests** — proposed code changes for review
- **Actions** — CI/CD workflow runs
- **Projects** — project management boards
- **Wiki** — documentation
- **Settings** — repo configuration

### README.md
- Written in **Markdown** — displayed on the repo's home page
- Best practice to include: project description, installation, usage, contributing guide, license

### Issues
- Track **bugs, features, tasks, and questions**
- Can be assigned to users, labelled, added to milestones and projects
- Referenced in commits and PRs with `#issue-number`
- Closed automatically when a PR with `Closes #123` is merged

### Pull Requests (PRs)
- Propose **merging changes from one branch into another**
- Include: description, linked issues, reviewers, labels, checks
- States: Open, Closed, Merged, Draft

---

> ⬅️ [Back to Index](./index.md) | ➡️ [Domain 2 →](./domain-2-repositories.md)
