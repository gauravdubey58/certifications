# Domain 1 – Responsible AI & Prompt Engineering `16%`

> ⬅️ [Back to GH-300 Index](./index.md)

Covers **Responsible AI (7%)** and **Prompt Crafting & Engineering (9%)** from the official study guide.

---

## 1.1 Responsible AI

### Risks of Using Generative AI

| Risk | Description | Example |
|------|-------------|---------|
| **Hallucination** | Generates plausible but incorrect code or information | Copilot suggests a function that doesn't exist in a library |
| **Bias** | Reflects biases in training data | Suggestions may favour certain coding styles or patterns |
| **Security vulnerabilities** | May suggest insecure code patterns | SQL injection, hard-coded credentials |
| **Privacy leakage** | May reproduce fragments of training data | Private code patterns from public repositories |
| **Fairness** | Output may not be equitable across contexts | Different quality suggestions for different languages |
| **Copyright / IP** | Suggestions may match existing licensed code | Duplication of open-source code without attribution |
| **Over-reliance** | Developers may stop critically reviewing suggestions | Accepting wrong logic without testing |

### Limitations of Generative AI Tools

| Limitation | Details |
|-----------|---------|
| **Training data depth** | Knowledge cut-off date; unaware of newest libraries/APIs |
| **Context window** | Limited tokens — can't process entire large codebases at once |
| **No real computation** | Provides reasoning, not calculations — verify numeric outputs |
| **Stale suggestions** | Older code patterns may be outdated or deprecated |
| **Most-seen bias** | Over-represents common patterns from popular repos (GitHub) |
| **Language gaps** | Quality varies — better for Python/JavaScript than niche languages |

> ⭐ **Exam Tip:** GitHub Copilot does **reasoning and pattern matching**, not calculations. Always validate math, logic, and business rules from Copilot output.

### Validating AI Output
- Always **review and test** suggestions before committing
- Run tests, linters, and SAST tools on Copilot-generated code
- Never treat AI suggestions as authoritative
- Use code review processes even for AI-generated code

### Ethical AI Principles for Copilot

| Principle | What It Means |
|-----------|--------------|
| **Transparency** | Users should know when AI is assisting |
| **Accountability** | Developers are responsible for code they commit |
| **Fairness** | AI should provide equitable quality across user types |
| **Privacy** | Sensitive data should not be sent to AI services |
| **Human oversight** | AI augments, not replaces, human judgment |

### Mitigating Potential Harms
- Enable **content exclusions** to prevent sensitive files from being sent as context
- Use **duplication detection** to avoid reproducing licensed code
- Configure **organization policies** to enforce acceptable use
- Educate developers about AI limitations
- Enforce code review processes regardless of AI assistance

---

## 1.2 Prompt Crafting

### What is a Prompt?
A prompt is the **input sent to the AI model** — it includes everything Copilot uses to generate a suggestion: your code, comments, cursor position, open files, and explicit chat messages.

### Parts of a Prompt

| Component | Description |
|-----------|-------------|
| **Context** | Open files, cursor position, selected code |
| **Intent** | What you want (from comments, chat message) |
| **Examples** | Sample inputs/outputs you provide |
| **Constraints** | Format, language, framework requirements |

### Prompt Crafting Best Practices

- **Be specific** — vague prompts get vague suggestions
- **Provide context** — include relevant variables, types, imports
- **Use comments** — write descriptive comments above functions before invoking Copilot
- **State constraints** — "Use TypeScript", "avoid using loops", "return JSON"
- **Break down tasks** — ask for one thing at a time
- **Use examples** — show input/output pairs for complex transformations
- **Iterate** — refine the prompt if the first suggestion isn't right

```python
# Good prompt (as a comment):
# Function to validate UK phone numbers (07xxx xxxxxx format)
# Returns True if valid, False otherwise
# Uses regex, no external libraries
def validate_uk_phone(number: str) -> bool:
```

### Zero-Shot vs Few-Shot Prompting

| Type | Description | When to Use |
|------|-------------|-------------|
| **Zero-shot** | Ask directly with no examples | Simple, well-defined tasks |
| **Few-shot** | Provide 1–3 examples before the actual request | Complex patterns, specific output formats |
| **Chain-of-thought** | Ask to "think step by step" | Multi-step logic, debugging |

```python
# Few-shot example in a comment:
# Input: "hello world" -> Output: "Hello World"
# Input: "foo bar baz" -> Output: "Foo Bar Baz"
# Input: "my string here" -> Output:
```

### How Chat History is Used
- Copilot Chat maintains context from **previous turns in the conversation**
- Earlier messages influence subsequent suggestions
- You can reset context by starting a new chat session
- Long conversations may lose early context due to token window limits

---

## 1.3 Prompt Engineering

### Prompt Engineering Principles

| Principle | Description |
|-----------|-------------|
| **Clarity** | Use precise, unambiguous language |
| **Specificity** | Include concrete details (language, framework, constraints) |
| **Context** | Provide relevant background |
| **Completeness** | Include all information Copilot needs |
| **Iteration** | Refine prompts based on output quality |

### Training Methods (for LLMs)
- **Pre-training** — model learns patterns from massive code corpus (GitHub repos)
- **Fine-tuning** — model adapted for specific tasks
- **RLHF** — Reinforcement Learning from Human Feedback; aligns output with human preferences

### How Context is Determined by Copilot
1. Currently open file (most important)
2. Other open tabs in the editor (neighbour files)
3. Recently viewed files
4. Imported modules and their signatures
5. Explicit chat messages and conversation history

### Prompt Process Flow
```
Developer writes code/comment
        ↓
Copilot collects context (open files, cursor, imports)
        ↓
Proxy service receives prompt
        ↓
Filters applied (content policy, duplication detection)
        ↓
LLM generates suggestion tokens
        ↓
Post-processing (filter, rank suggestions)
        ↓
Suggestion displayed in editor
```

---

> ⬅️ [Back to Index](./index.md) | ➡️ [Domain 2 →](./domain-2-copilot-plans-features.md)
