# ⚡ GH-300 Quick Reference Cheat Sheet

> ⬅️ [Back to GH-300 Index](./index.md)

---

## 📋 Copilot Plans — Full Comparison

| Feature | Individual | Business | Enterprise |
|---------|:----------:|:--------:|:----------:|
| IDE code suggestions | ✅ | ✅ | ✅ |
| Copilot Chat (IDE) | ✅ | ✅ | ✅ |
| Copilot CLI | ✅ | ✅ | ✅ |
| Mobile (Xcode/Android Studio) | ✅ | ✅ | ✅ |
| IP Indemnity | ❌ | ✅ | ✅ |
| Data used for training | Opt-out | ❌ Never | ❌ Never |
| Organization policy management | ❌ | ✅ | ✅ |
| Content exclusions (org-wide) | ❌ | ✅ | ✅ |
| Audit logs | ❌ | ✅ | ✅ |
| REST API management | ❌ | ✅ | ✅ |
| Copilot Chat on GitHub.com | ❌ | ❌ | ✅ |
| PR summaries | ❌ | ❌ | ✅ |
| Knowledge Bases | ❌ | ❌ | ✅ |
| Custom fine-tuned models | ❌ | ❌ | ✅ |

---

## ⌨️ Key Keyboard Shortcuts

| Action | Windows/Linux | Mac |
|--------|--------------|-----|
| Accept suggestion | `Tab` | `Tab` |
| Dismiss suggestion | `Esc` | `Esc` |
| Next suggestion | `Alt+]` | `Option+]` |
| Previous suggestion | `Alt+[` | `Option+[` |
| Open suggestion panel | `Ctrl+Enter` | `Ctrl+Enter` |
| Open Inline Chat | `Ctrl+I` | `Ctrl+I` |

---

## 💬 Copilot Chat Slash Commands

| Command | What It Does |
|---------|-------------|
| `/explain` | Explain selected code |
| `/fix` | Fix bugs in selected code |
| `/tests` | Generate tests for selected code |
| `/doc` | Generate documentation/comments |
| `/simplify` | Refactor/simplify code |
| `/new` | Scaffold a new project |
| `/clear` | Clear chat history |

## 🔗 Chat Context Variables

| Variable | Scope |
|----------|-------|
| `@workspace` | Entire workspace |
| `@github` | GitHub repos, issues, PRs |
| `@terminal` | Terminal output |
| `#file` | A specific file |
| `#selection` | Currently selected code |
| `#codebase` | Full codebase search |

---

## 🔄 Copilot Data Pipeline (Simplified)

```
Code/Chat Input
    → Copilot Extension collects Context (open files, cursor, imports)
    → Prompt built → Sent to Proxy Server
    → Filters: content policy + duplication detection
    → LLM generates suggestion
    → Post-processing (safety + ranking)
    → Suggestion shown in IDE
```

---

## 🔒 Privacy Controls

| Control | Where | Effect |
|---------|-------|--------|
| Duplication detection | Org/user settings | Blocks suggestions matching public code |
| Prompt & suggestion collection | Individual settings | Opts out of data used for training |
| Content exclusions | Repo / org settings | Excludes files from Copilot context |
| IP indemnity | Business/Enterprise plan | Microsoft defends copyright claims |

**Key limit:** Content exclusions prevent Copilot *auto-reading* files — they do NOT prevent a developer from manually pasting that content into chat.

---

## 🧪 Testing with Copilot

| Test Type | How to Request |
|-----------|---------------|
| Unit tests | `/tests` or "Write unit tests for this function" |
| Edge cases | "What edge cases should I test?" |
| Integration tests | "Write integration tests for this service" |
| Improve existing tests | Select tests → "Are there any gaps in coverage?" |

---

## 🤖 Responsible AI — Key Risks

| Risk | Mitigation |
|------|-----------|
| Hallucination | Always test and review AI output |
| Bias | Understand training data limitations |
| Security vulnerabilities | Run SAST tools; ask Copilot to review for security |
| Copyright/duplication | Enable duplication detection |
| Over-reliance | Maintain mandatory code review processes |

---

## ⭐ Top Exam Topics

| Topic | Key Point |
|-------|----------|
| IP Indemnity | Business and Enterprise only — NOT Individual |
| Data training | Individual can opt out; Business/Enterprise never trained on |
| Content exclusions | Prevent auto-reading files; don't block manual paste into chat |
| Knowledge Bases | Enterprise only — custom org docs used as Copilot context |
| PR summaries | Enterprise only |
| Copilot Chat on GitHub.com | Enterprise only |
| Duplication detection | Blocks suggestions matching ≥150 chars of public code |
| Copilot CLI install | `gh extension install github/gh-copilot` |
| `/tests` command | Generates unit tests for selected code |
| Context sources | Current file → open tabs → recent files → imports |
| Neighbour files | Keeping related files open improves suggestion quality |
| Context window limit | Long chats and large files lose early context |
| Copilot instructions file | `.github/copilot-instructions.md` — persistent context for a repo |
| Acceptance rate | Key metric from Productivity API; ~25–35% industry average |
| LLM limitation | Does reasoning/pattern matching — NOT real computation |
| Zero-shot vs few-shot | Few-shot provides examples; zero-shot asks directly |

---

> 📝 [Practice with 50 MCQs →](./mcqs.md) | ⬅️ [Back to Index](./index.md)
