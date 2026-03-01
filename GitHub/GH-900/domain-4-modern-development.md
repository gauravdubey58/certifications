# Domain 4 – Modern Development `13%`

> ⬅️ [Back to GH-900 Index](./index.md)

---

## 4.1 DevOps and GitHub

### DevOps Principles
- **Collaboration** between development and operations teams
- **Automation** of testing, building, and deployment
- **Continuous Improvement** through fast feedback loops
- **Shift-left testing** — test earlier in the development cycle
- **Infrastructure as Code (IaC)** — manage infrastructure via code

### GitHub's Role in DevOps
- **Source control** — Git repositories
- **CI/CD** — GitHub Actions
- **Collaboration** — Issues, PRs, Projects, Discussions
- **Security** — Dependabot, secret scanning, code scanning
- **Observability** — deployment environments, status checks

---

## 4.2 GitHub Actions for Automation

### Common Automation Use Cases

| Use Case | Example |
|----------|---------|
| Run tests on PR | Run unit/integration tests before merge |
| Build and publish Docker image | Push to GitHub Container Registry on merge |
| Deploy to cloud | Deploy to Azure/AWS after tests pass |
| Lint code | Run ESLint/Prettier/Black on every push |
| Notify team | Send Slack/Teams message on failure |
| Release management | Auto-create GitHub Release on tag push |

### CI/CD Pipeline Structure
```
Developer pushes code
        ↓
CI: Lint → Build → Test → Security scan
        ↓ (if all pass)
CD: Deploy to staging → Run smoke tests → Deploy to production
```

### GitHub Environments
- Define **deployment targets** (dev, staging, production)
- Each environment can have: protection rules, required reviewers, secrets
- Jobs reference environments with `environment: production`
- Deployment history visible in the repo

### Reusable Workflows
- Share workflow logic across repositories
- Called with `uses: org/repo/.github/workflows/file.yml@main`
- Reduces duplication, enforces standards

---

## 4.3 Code Review Best Practices

- **Small, focused PRs** — easier and faster to review
- **Descriptive PR titles and descriptions** — explain the why, not just the what
- **Link related issues** — gives reviewers full context
- **Self-review first** — review your own PR before requesting others
- **Respond to all comments** — even to say "acknowledged"
- **Use draft PRs** — signal work in progress; get early feedback
- **Review promptly** — keep PRs from going stale

---

> ⬅️ [← Domain 3](./domain-3-collaboration.md) | [Back to Index](./index.md) | [Domain 5 →](./domain-5-project-management.md)
