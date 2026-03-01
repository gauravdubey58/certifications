# Domain 6 – Privacy, Security, and Administration `10%`

> ⬅️ [Back to GH-900 Index](./index.md)

---

## 6.1 Repository Security Features

### Branch Protection Rules
Branch protection rules **prevent direct pushes** and enforce quality gates before code reaches important branches like `main`.

**How to set up:** Repository → Settings → Branches → Add rule

| Protection Option | Description |
|------------------|-------------|
| **Require pull request reviews** | PRs must be approved by N reviewers before merge |
| **Dismiss stale reviews** | Approval is invalidated if new commits are pushed |
| **Require status checks to pass** | CI/CD checks must be green before merge |
| **Require branches to be up to date** | Branch must include latest changes from base |
| **Require conversation resolution** | All PR comments must be resolved before merge |
| **Require signed commits** | All commits must be GPG/SSH signed |
| **Restrict who can push** | Limit direct pushes to specific people/teams |
| **Allow force pushes** | Toggle (off by default for protected branches) |
| **Allow deletions** | Toggle (off by default for protected branches) |

> ⭐ **Exam Tip:** Branch protection rules are configured at the **repository level**, not the organization level. To enforce across all repos, use a GitHub Action or org-level rulesets.

### Rulesets (Modern Branch Protection)
- Newer, more flexible replacement for branch protection rules
- Can be applied at **repository or organization level**
- Supports targeting multiple branches with glob patterns
- Can be enforced in "evaluate" mode (report only) or "active" mode

---

## 6.2 Dependabot

Dependabot **automatically monitors your dependencies** for known security vulnerabilities and out-of-date versions.

### Dependabot Features

| Feature | Description |
|---------|-------------|
| **Dependabot alerts** | Notifies when a dependency has a known CVE (Common Vulnerabilities and Exposures) |
| **Dependabot security updates** | Auto-creates PRs to update vulnerable dependencies |
| **Dependabot version updates** | Auto-creates PRs to keep ALL dependencies up to date (not just vulnerable ones) |

### Enabling Dependabot
- Alerts: Repository → Settings → Security → Dependabot alerts → Enable
- Security updates: Automatically enabled when alerts are enabled
- Version updates: Requires a `dependabot.yml` file in `.github/`

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
```

### Dependabot vs Code Scanning vs Secret Scanning

| Tool | What It Scans | What It Finds |
|------|--------------|--------------|
| **Dependabot** | `package.json`, `requirements.txt`, etc. | Vulnerable third-party libraries |
| **Code scanning (CodeQL)** | Your source code | Bugs, logic errors, security flaws |
| **Secret scanning** | All files and commits | Leaked API keys, tokens, passwords |

---

## 6.3 Secret Scanning

- Automatically detects **secrets committed to repositories** (API keys, tokens, passwords)
- Supports 100+ patterns from providers like AWS, GitHub, Stripe, Azure
- Alerts repository owners when a secret is detected
- **Push protection** — blocks a push that contains a known secret pattern before it reaches the repo

### Enabling Secret Scanning
- Repository → Settings → Security → Secret scanning → Enable
- Push protection: separately enabled as an additional option
- Available on: all public repos (free), private repos on GitHub Advanced Security plans

---

## 6.4 Code Scanning with CodeQL

- Static analysis tool that finds **security vulnerabilities and bugs** in your code
- Integrated into GitHub as a GitHub Action
- Analyses code on every push or PR
- Supports: C/C++, C#, Go, Java, JavaScript/TypeScript, Python, Ruby, Swift

### Setting Up CodeQL
- Add the starter workflow: Actions → Security → CodeQL Analysis
- Creates `.github/workflows/codeql.yml`
- Results appear in: Security → Code scanning alerts

---

## 6.5 Repository Access and Permissions

### Repository Roles (recap)

| Role | Can Do |
|------|--------|
| **Read** | View and clone the repository |
| **Triage** | Manage issues and PRs (no code access) |
| **Write** | Push code, manage issues/PRs |
| **Maintain** | Manage repo settings (not destructive actions) |
| **Admin** | Full control including adding collaborators |

### Organization Teams
- Group members into **teams** for managing access across multiple repos
- Assign teams a role on multiple repos at once
- Nested teams: child teams inherit parent team membership
- Teams can be @mentioned in issues, PRs, and comments

### CODEOWNERS File
```
# .github/CODEOWNERS
# Auto-request reviews from the right team based on file paths

*.js                  @frontend-team
/docs/                @documentation-team
/src/payments/        @payments-team @security-team
```

- When a PR touches a file matched by CODEOWNERS, those owners are **automatically requested as reviewers**
- Requires "Require review from Code Owners" to be set in branch protection rules to enforce it

---

## 6.6 Administering GitHub Organizations

### Organization Structure

```
Organization
├── Members (with different roles)
├── Teams (groups of members)
├── Repositories (with team permissions)
└── Settings (policies, billing, security)
```

### Organization Roles

| Role | Permissions |
|------|-------------|
| **Owner** | Full admin access to the org and all repos |
| **Member** | Can create repos (if allowed), be added to teams |
| **Billing Manager** | Can manage billing only |
| **Security Manager** | Can manage security alerts across all repos |

### Organization-Level Security Settings
- **Two-factor authentication (2FA)** — require all members to have 2FA enabled
- **Allowed actions** — restrict which GitHub Actions can run
- **Default repository permissions** — set base permission level for all members
- **Member forking privileges** — allow/deny forking private repos
- **Repository creation** — restrict who can create public/private/internal repos

### Single Sign-On (SSO)
- Organizations can enforce **SAML SSO** for authentication
- Members must authenticate through your identity provider (Okta, Azure AD, etc.) before accessing org resources
- Available on: GitHub Enterprise Cloud (GHEC)

### Audit Log (Organization Level)
- Tracks all significant actions across the organization
- Events: member added/removed, repo created/deleted, policy changes, Copilot usage
- Searchable and exportable
- Available to: organization owners and security managers
- API accessible: `GET /orgs/{org}/audit-log`

---

> ⬅️ [← Domain 5](./domain-5-project-management.md) | [Back to Index](./index.md) | [Domain 7 →](./domain-7-community.md)
