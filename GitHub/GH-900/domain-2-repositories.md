# Domain 2 – Working with GitHub Repositories `8%`

> ⬅️ [Back to GH-900 Index](./index.md)

---

## 2.1 Managing Repository Settings

### Repository Visibility
| Visibility | Who Can See | Who Can Contribute |
|-----------|------------|-------------------|
| **Public** | Anyone on the internet | Owners/collaborators (others can fork) |
| **Private** | Only invited collaborators | Only invited collaborators |
| **Internal** (org) | All org members | Members with write permission |

### Repository Permissions (Collaborator Roles)

| Role | Read | Write | Admin |
|------|------|-------|-------|
| **Read** | ✅ | ❌ | ❌ |
| **Triage** | ✅ | Issues/PRs only | ❌ |
| **Write** | ✅ | ✅ | ❌ |
| **Maintain** | ✅ | ✅ | Partial |
| **Admin** | ✅ | ✅ | ✅ Full |

### Repository Templates
- Mark any repo as a **template** in Settings → General
- Others can create new repos with the same file structure and contents
- Template repos are marked with a **"Use this template"** button
- Useful for: standardising project structure across an org

### Key Settings
- **Default branch** — the branch that PRs target by default (usually `main`)
- **Branch protection** — rules preventing direct pushes, requiring reviews
- **Features** — enable/disable: Issues, Projects, Wiki, Discussions, Sponsorships
- **Danger Zone** — transfer, archive, or delete repository

---

## 2.2 Working with Files

### Adding and Editing Files on GitHub.com
- Browse to any file → click pencil icon to edit in browser
- Use **github.dev editor** (`E` key shortcut) — full VS Code in browser
- Use **`.`** shortcut anywhere in a repo to open the web editor

### File Versioning
- Every commit creates a new version of changed files
- View file history: file page → **History** button
- Compare any two versions: GitHub shows a **diff** (green = added, red = removed)
- **Blame view** — see which commit introduced each line of code

### GitHub Desktop
- GUI application for Git on Windows and Mac
- Visually manage: repos, branches, commits, push/pull
- Ideal for developers who prefer GUI over CLI
- Handles merge conflicts with a visual editor

### The `.github` Directory
Special directory at the repo root:
```
.github/
├── ISSUE_TEMPLATE/        # Templates for new issues
├── PULL_REQUEST_TEMPLATE.md  # Template for new PRs
├── workflows/             # GitHub Actions workflows
├── CODEOWNERS             # Auto-assign reviewers
└── copilot-instructions.md # Copilot persistent context
```

---

> ⬅️ [← Domain 1](./domain-1-git-github-intro.md) | [Back to Index](./index.md) | [Domain 3 →](./domain-3-collaboration.md)
