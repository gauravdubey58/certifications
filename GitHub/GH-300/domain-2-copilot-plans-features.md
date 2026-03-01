# Domain 2 – GitHub Copilot Plans and Features `31%`

> ⬅️ [Back to GH-300 Index](./index.md)

This is the **largest domain** — know the differences between plans and every major feature.

---

## 2.1 GitHub Copilot Plans

### Plan Comparison

| Feature | Individual | Business | Enterprise |
|---------|-----------|----------|------------|
| **Target** | Single developers | Organizations | Large orgs on GHEC |
| **IDE suggestions** | ✅ | ✅ | ✅ |
| **Copilot Chat (IDE)** | ✅ | ✅ | ✅ |
| **Copilot CLI** | ✅ | ✅ | ✅ |
| **Organization policy management** | ❌ | ✅ | ✅ |
| **Content exclusions (org-wide)** | ❌ | ✅ | ✅ |
| **Audit logs** | ❌ | ✅ | ✅ |
| **IP indemnity** | ❌ | ✅ | ✅ |
| **Data excluded from training** | Optional | ✅ (default) | ✅ (default) |
| **Knowledge Bases** | ❌ | ❌ | ✅ |
| **Copilot Chat on GitHub.com** | ❌ | ❌ | ✅ |
| **PR summaries** | ❌ | ❌ | ✅ |
| **Custom models** | ❌ | ❌ | ✅ |
| **REST API management** | ❌ | ✅ | ✅ |

> ⭐ **Exam Tip:** **IP indemnity** (protection against copyright claims on AI-generated code) is only available in **Business and Enterprise** plans, NOT Individual.

### Copilot for Non-GitHub Customers
- Copilot Individual is available to anyone with a GitHub account
- Copilot Business requires a GitHub organization
- Copilot Enterprise requires **GitHub Enterprise Cloud (GHEC)**

---

## 2.2 GitHub Copilot in the IDE

### How to Trigger Copilot

| Trigger Method | How |
|---------------|-----|
| **Ghost text (inline suggestions)** | Just start typing — suggestions appear automatically |
| **Accept suggestion** | Press `Tab` |
| **Dismiss suggestion** | Press `Esc` |
| **See next suggestion** | `Alt+]` (Windows/Linux) / `Option+]` (Mac) |
| **See previous suggestion** | `Alt+[` / `Option+[` |
| **Open suggestion panel** | `Ctrl+Enter` — shows multiple suggestions |
| **Inline Chat** | `Ctrl+I` — opens chat overlay in the editor |
| **Copilot Chat panel** | Click the Copilot icon in the sidebar |

### Copilot Chat Slash Commands

| Command | Purpose |
|---------|---------|
| `/explain` | Explain selected code |
| `/fix` | Fix a bug or error in selected code |
| `/tests` | Generate tests for selected code |
| `/doc` | Generate documentation |
| `/simplify` | Simplify or refactor code |
| `/new` | Scaffold a new project/file |
| `/newNotebook` | Create a new Jupyter notebook |
| `/clear` | Clear the chat history |

### Chat Context Variables (`@` and `#`)

| Variable | Description |
|----------|------------|
| `@workspace` | References the entire workspace for context |
| `@github` | Searches GitHub repositories and issues |
| `@terminal` | References terminal output |
| `#file` | Reference a specific file |
| `#selection` | Reference the selected text |
| `#codebase` | Search the entire codebase |

### Improving Copilot Chat Performance
- Be specific about what you want
- Reference relevant files with `#file`
- Use `@workspace` for project-wide questions
- Provide failing test output or error messages for debugging
- Start a new chat to clear stale context

---

## 2.3 GitHub Copilot Business Features

### Organization-Wide Policy Management
- Set at: **Org → Settings → Copilot → Policies**
- Control: who can use Copilot, which features are enabled
- Configurable policies:
  - Enable/disable suggestions matching public code
  - Enable/disable duplication detection
  - Enable/disable prompt and suggestion collection
  - Configure content exclusions

### Content Exclusions
- Exclude specific files or directories from being sent to Copilot as context
- Configured at: **repository level** or **organization level**
- Syntax: glob patterns (e.g., `**/*.env`, `secrets/**`)
- **Effects:** Copilot will not read or suggest content from excluded files
- **Limitation:** Exclusions apply to the IDE — they don't prevent a developer from manually pasting excluded content into chat

### Audit Logs
- Track Copilot usage events across the organization
- Searchable via: GitHub UI or REST API
- Events logged: suggestions accepted, chat sessions, policy changes
- Useful for compliance and usage reporting

### REST API for Copilot Business
- Manage subscriptions, add/remove seats, view usage metrics
- Endpoint: `GET /orgs/{org}/copilot/usage`
- Useful for automating license management at scale

---

## 2.4 GitHub Copilot Enterprise Features

### Copilot Chat on GitHub.com
- Use Copilot Chat **directly in the browser** on github.com
- Ask questions about repositories, issues, PRs, and code without opening an IDE
- Integrates with GitHub search and your org's codebase

### Pull Request Summaries
- Copilot automatically generates a **summary of changes** in a PR
- Summary includes: what changed, why, affected files
- Reduces time for reviewers to understand context
- Configurable: can be set to auto-generate or manual trigger

### Knowledge Bases
- Custom collections of documents (markdown, code) that Copilot uses as context
- Types of knowledge: coding standards, best practices, architecture docs, API specs
- Benefits: consistent, org-specific suggestions grounded in YOUR standards
- Configuration: index documents → Copilot references them during chat
- Require indexing before use

### Custom Models
- Fine-tune Copilot suggestions on your organization's private codebase
- Provides more relevant, context-aware suggestions for your specific tech stack
- Available only in Enterprise plan

---

## 2.5 GitHub Copilot in the CLI

### Installation
```bash
gh extension install github/gh-copilot
```
Requires: GitHub CLI (`gh`) to be installed and authenticated.

### Common CLI Commands

| Command | Description |
|---------|-------------|
| `gh copilot suggest` | Ask Copilot for a shell command suggestion |
| `gh copilot explain` | Explain a shell command |
| `gh copilot alias` | Configure shell aliases for Copilot |

### CLI Configuration Settings
- Preferred shell (bash, zsh, fish, PowerShell)
- Opt-in/out of usage data collection
- Set default response language

---

## 2.6 Agent Mode, Edit Mode & MCP

### Agent Mode
- Copilot operates **autonomously** to complete multi-step tasks
- Can browse files, read terminal output, write code, and run commands
- Manages **Agent Sessions** — context preserved across steps
- Can delegate to **Sub-Agents** for parallelism and context optimization
- Available in: VS Code, JetBrains (experimental)

### Edit Mode
- Copilot makes **targeted, precise edits** to existing code
- Select a region → describe what you want changed
- Copilot proposes a diff you can accept, reject, or modify

### MCP – Model Context Protocol
- Open protocol allowing **third-party tools** to provide context to Copilot
- Extends Copilot with external data sources (databases, APIs, documentation)
- Enables richer, more relevant suggestions beyond just code files

### GitHub Copilot Spaces
- Dedicated **AI-powered workspaces** within GitHub
- Attach repos, knowledge, and instructions for focused Copilot sessions

---

> ⬅️ [← Domain 1](./domain-1-responsible-ai-prompt-engineering.md) | [Back to Index](./index.md) | [Domain 3 →](./domain-3-data-privacy.md)
