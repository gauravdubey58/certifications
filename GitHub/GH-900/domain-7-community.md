# Domain 7 – Benefits of the GitHub Community `10%`

> ⬅️ [Back to GH-900 Index](./index.md)

---

## 7.1 Open Source and the GitHub Community

### What is Open Source?
- Software with source code that **anyone can inspect, modify, and distribute**
- Licensed under open-source licences (MIT, Apache 2.0, GPL, etc.)
- Built on the principle of community collaboration
- GitHub hosts the largest collection of open-source projects in the world

### Why Contribute to Open Source?
- Build skills and experience with real-world codebases
- Improve software you already use
- Grow your professional network and portfolio
- Learn from experienced developers through code review
- Give back to the community

### How to Find Projects to Contribute To
- Search by topic, language, or label on github.com/explore
- Look for repos with **`good first issue`** or **`help wanted`** labels
- Check **GitHub Trending** for popular projects
- Explore **GitHub Sponsors** to find projects worth supporting
- Look at the tools and libraries your current project depends on

---

## 7.2 Contributing to Open Source

### Typical Contribution Workflow

```
1. Find a project and an issue to work on
         ↓
2. Fork the repository (creates your own copy)
         ↓
3. Clone your fork locally
         ↓
4. Create a feature branch
         ↓
5. Make your changes and commit
         ↓
6. Push to your fork
         ↓
7. Open a Pull Request to the original (upstream) repo
         ↓
8. Respond to review feedback
         ↓
9. PR merged by maintainers ✅
```

### Understanding Open Source Licences

| Licence | Key Rules |
|---------|-----------|
| **MIT** | Very permissive; use freely, must include copyright notice |
| **Apache 2.0** | Permissive; includes patent protection |
| **GPL v3** | Copyleft; derivative work must also be open source |
| **BSD 2/3-clause** | Similar to MIT, slightly more restrictions |
| **Creative Commons** | Used for docs/media (not code) |

> ⭐ **Exam Tip:** The **MIT licence** is the most permissive and widely used on GitHub. GPL is "copyleft" — any derivative work must also be open source.

### Community Health Files
GitHub recognizes special files that define how a community operates:

| File | Location | Purpose |
|------|----------|---------|
| `README.md` | Root or `docs/` | Project overview, setup, usage |
| `CONTRIBUTING.md` | Root or `docs/` | Guidelines for contributors |
| `CODE_OF_CONDUCT.md` | Root or `docs/` | Community behaviour expectations |
| `LICENSE` | Root | Legal terms for using the code |
| `SECURITY.md` | Root or `.github/` | How to report security vulnerabilities |
| `CHANGELOG.md` | Root or `docs/` | History of changes per version |
| `SUPPORT.md` | Root or `.github/` | Where to get help |

> ⭐ **Exam Tip:** `CONTRIBUTING.md` tells people **how to contribute**. `CODE_OF_CONDUCT.md` sets **community behaviour expectations**. Both appear in many open-source repos.

### InnerSource
- Applying open-source **practices within a private organization**
- Internal teams contribute to each other's codebases using fork + PR workflows
- Promotes knowledge sharing, reuse, and cross-team collaboration
- GitHub supports InnerSource with: internal repos, org-wide forks, visibility controls

---

## 7.3 GitHub Discussions

**GitHub Discussions** is a community forum built directly into repositories and organizations.

### Discussions vs Issues

| | Issues | Discussions |
|--|--------|------------|
| Purpose | Track bugs and tasks | Open-ended conversation, Q&A, ideas |
| Format | Task-oriented, closeable | Conversational, threaded |
| Typical use | "Bug: login fails" | "What's the best approach for X?" |
| Can be answered | ❌ | ✅ (mark an answer) |
| Can be converted | PR can close issue | Discussion can be converted to issue |

### Discussion Categories
- **Q&A** — questions with a markable best answer
- **Ideas** — suggest new features
- **Show and tell** — share projects
- **General** — open conversation
- **Announcements** — one-way communication from maintainers

### Polls in Discussions
- Create polls within a discussion for community voting
- Useful for deciding between competing approaches or feature priorities

---

## 7.4 GitHub Stars, Watching, and Notifications

### Stars ⭐
- Bookmark a repository you find interesting or useful
- A star signals appreciation to the project maintainers
- Your starred repos are visible on your profile
- Used by GitHub to populate trending and discover features

### Watching 👁️
- Subscribe to notifications for a repository's activity
- Watch options: **All activity**, **Issues only**, **PRs only**, **Releases only**, **Ignore**
- Default for repos you own/contribute to: "Participating and @mentions"

### Notifications
- Receive updates for: issues you opened/commented on, PRs assigned to you, @mentions
- Manage via: github.com/notifications or email
- Can be configured per-repo or globally in profile settings

---

## 7.5 GitHub Sponsors and Marketplace

### GitHub Sponsors
- Financially support open-source developers and organizations directly on GitHub
- Sponsors get: recognition (badge on profile), tier-based rewards
- Maintainers can set up sponsorship tiers
- Organizations can also receive and give sponsorships

### GitHub Marketplace
- Discover and install **third-party apps and GitHub Actions** that integrate with GitHub
- Categories: CI/CD, code quality, project management, security, monitoring
- Apps can be: free or paid, organization-scoped or user-scoped

---

## 7.6 GitHub Profile and Social Features

### GitHub Profile README
- Create a repo with the same name as your username (e.g., `gauravdubey58/gauravdubey58`)
- The `README.md` in this repo appears on your public GitHub profile
- Showcase: projects, skills, stats, contributions, contact info

### GitHub Achievements and Contributions Graph
- Contribution graph shows your daily commit/PR/review activity
- Achievements (badges) earned for: first PR, first issue, first PR merged to a popular repo, etc.
- Public profile shows your activity to potential employers and collaborators

---

> ⬅️ [← Domain 6](./domain-6-security-admin.md) | [Back to Index](./index.md) | [⚡ Quick Reference →](./quick-reference.md)
