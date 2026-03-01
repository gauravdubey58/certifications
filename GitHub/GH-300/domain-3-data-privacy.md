# Domain 3 – How Copilot Works, Data Handling & Privacy `30%`

> ⬅️ [Back to GH-300 Index](./index.md)

Covers **How Copilot Works (15%)** and **Privacy & Context Exclusions (15%)**.

---

## 3.1 The Copilot Data Pipeline Lifecycle

### End-to-End Flow

```
1. Developer types code / sends chat message in IDE
         ↓
2. Copilot Extension collects CONTEXT
   (open files, cursor position, imports, recently viewed files)
         ↓
3. PROMPT is constructed
   (context + explicit request + chat history)
         ↓
4. Prompt sent to PROXY SERVICE (Microsoft/GitHub servers)
         ↓
5. FILTERS applied:
   - Responsible AI content filter
   - Duplication detection (vs public code)
   - PII detection
         ↓
6. Filtered prompt sent to LLM (Codex / GPT-based model)
         ↓
7. LLM GENERATES tokens (code suggestion)
         ↓
8. POST-PROCESSING on proxy:
   - Safety filters
   - Duplication check
   - Ranking of multiple suggestions
         ↓
9. SUGGESTION returned to IDE and displayed
         ↓
10. Developer ACCEPTS or REJECTS
    (accepted suggestions may be logged depending on settings)
```

### How Context is Gathered

| Source | Weight |
|--------|--------|
| Current file (open in editor) | Highest priority |
| Cursor position in file | Determines what precedes/follows |
| Other open tabs (neighbour files) | Important supplementary context |
| Recently viewed files | Secondary context |
| Imported modules and type signatures | Used for API-aware suggestions |
| Explicit chat input | Direct instructions override implicit context |

> ⭐ **Exam Tip:** Copilot uses **neighbouring open tabs** — keeping related files open improves suggestion quality. This is why suggestions improve when you have multiple relevant files open.

### How Copilot Identifies Matching Code
- Uses a **duplication detection** algorithm to check if a suggestion closely matches public code on GitHub
- If a match is found, the suggestion may be blocked or flagged
- Can be configured: **enabled (default for Business/Enterprise)** or **disabled (Individual may opt out)**

---

## 3.2 How Copilot Handles Data

### Data Flow for Code Completion

```
IDE context (code snippets) → Proxy Server → LLM → Suggestion → IDE
```

- **Individual plan:** Prompts and suggestions MAY be retained and used to improve the model (user can opt out)
- **Business/Enterprise plan:** Prompts and suggestions are **NOT retained or used for training** by default

### Data Flow for Copilot Chat

```
Chat message + context → Proxy → Content filter → LLM → Response → IDE
```

- Chat conversations are NOT stored permanently (session-only by default)
- GitHub does NOT use chat data for training with Business/Enterprise

### Types of Input Processing for Copilot Chat

| Input Type | Examples |
|-----------|---------|
| **Code generation** | "Write a function to sort a list" |
| **Explanation** | "Explain this function" |
| **Debugging** | "Why is this throwing a NullPointerException?" |
| **Refactoring** | "Simplify this method" |
| **Documentation** | "Write JSDoc for this function" |
| **Test generation** | "Write unit tests for this class" |

### Limitations of GitHub Copilot (and LLMs)

| Limitation | Detail |
|-----------|--------|
| **Context window** | Limited token count — large files or long chats lose early context |
| **Training data age** | Has a knowledge cut-off; unaware of very new libraries or breaking changes |
| **Most-seen examples** | Overrepresents common patterns; may not suggest niche or specialized solutions |
| **No real reasoning** | Pattern matching, not logical reasoning — verify numeric/algorithmic outputs |
| **Non-determinism** | Same prompt can produce different suggestions on different runs |

---

## 3.3 Privacy Settings and Data Controls

### Configuring Copilot Settings on GitHub.com

Navigate to: **Settings → Copilot** (individual) or **Org → Settings → Copilot** (business)

| Setting | Description |
|---------|-------------|
| **Duplication detection** | Toggle whether suggestions matching public code are blocked |
| **Prompt & suggestion collection** | Toggle whether your data is used to improve Copilot (Individual only) |
| **Content exclusions** | Specify files/folders excluded from Copilot context |

### Content Exclusions — Deep Dive

**Repository level:** Add to `.github/copilot-exclusions.yml` or via repo settings
**Organization level:** Configure in Org → Settings → Copilot → Content exclusions

```yaml
# Example content exclusion pattern
paths:
  - "**/*.env"
  - "secrets/**"
  - "config/production.yml"
  - "src/internal/payments/**"
```

**Effects of content exclusions:**
- Copilot will not use excluded files as context for suggestions
- Excluded files will not appear in Copilot Chat results
- Suggestions will not reference content from excluded paths

**Limitations of content exclusions:**
- Exclusions apply to **Copilot reading files automatically**, NOT to developers manually copying/pasting excluded content into chat
- There may be a delay of up to 30 minutes for exclusions to take effect across all clients
- Exclusions don't guarantee that no information from excluded files will ever influence suggestions (if the same code exists in included files)

> ⭐ **Exam Tip:** Content exclusions prevent Copilot from automatically reading sensitive files, but they **do NOT prevent** a developer from manually pasting that content into the chat window.

### Ownership of Copilot Outputs
- **You (the developer/organization)** own the code generated by Copilot
- GitHub does not claim ownership over Copilot outputs
- Business and Enterprise plans include **IP indemnity** — Microsoft will defend against third-party claims that Copilot output infringes on copyrighted code

---

## 3.4 Safeguards

### Duplication Detector Filter
- Scans suggestions against GitHub's index of public code
- If a suggestion **closely matches** (≥150 characters) public code, it may be:
  - **Blocked** (if duplication detection is ON)
  - **Shown with attribution** (in some configurations)
- Configurable per org — enable/disable as needed

### Contractual Protection (IP Indemnity)
- Available with **Business and Enterprise** plans only
- If your use of Copilot results in a copyright claim, Microsoft will defend you
- Conditions: you must not have disabled safety features that caused the issue

### Security Checks and Warnings
- Copilot scans suggestions for **common security vulnerabilities** (OWASP-style patterns)
- Warns when it detects potential issues like:
  - SQL injection risks
  - Hard-coded secrets
  - Insecure cryptographic usage
- These are warnings — developers can still choose to use or modify the suggestion

---

## 3.5 Troubleshooting Copilot

| Problem | Solution |
|---------|---------|
| Suggestions not showing in some files | Check if file is excluded via content exclusions; check if file extension is supported |
| Context exclusions not applied | Exclusions may take up to 30 min; restart IDE; verify syntax of exclusion config |
| Suggestions absent or not ideal | Add more context (comments, relevant open tabs); try inline chat; use `/fix` or rephrase |
| Copilot Chat not responding | Check network; ensure authenticated to GitHub; verify Copilot license assigned |

### Triggering Copilot When Suggestions Are Missing
1. Add a descriptive comment above where you want the suggestion
2. Open relevant files in other tabs (neighbour context)
3. Use `Ctrl+Enter` to manually open the suggestion panel
4. Use Inline Chat (`Ctrl+I`) as a fallback
5. Restart the IDE extension if suggestions stopped working entirely

---

> ⬅️ [← Domain 2](./domain-2-copilot-plans-features.md) | [Back to Index](./index.md) | [Domain 4 →](./domain-4-use-cases-testing.md)
