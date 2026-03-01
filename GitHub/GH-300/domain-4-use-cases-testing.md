# Domain 4 – Developer Use Cases & Testing `23%`

> ⬅️ [Back to GH-300 Index](./index.md)

Covers **Developer Use Cases for AI (14%)** and **Testing with GitHub Copilot (9%)**.

---

## 4.1 How AI Improves Developer Productivity

### Common Productivity Use Cases

| Use Case | How Copilot Helps |
|----------|------------------|
| **Learning new languages/frameworks** | Generates idiomatic code in unfamiliar languages; explains patterns |
| **Language translation** | Converts code from one language to another (e.g., Python → TypeScript) |
| **Context switching** | Quickly re-understands a codebase after switching tasks |
| **Writing documentation** | Generates JSDoc, docstrings, README sections, inline comments |
| **Personalized context-aware responses** | Suggestions adapt to your specific codebase and style |
| **Generating sample/test data** | Creates realistic mock data for testing |
| **Modernizing legacy applications** | Converts old patterns to modern equivalents |
| **Debugging code** | Explains errors, suggests fixes, identifies root causes |
| **Data science** | Generates pandas, NumPy, and matplotlib code from descriptions |
| **Code refactoring** | Simplifies complex functions, removes duplication, improves readability |

### Copilot in the Software Development Lifecycle (SDLC)

| SDLC Phase | Copilot Contribution |
|------------|---------------------|
| **Planning** | Generate user stories, acceptance criteria from prompts |
| **Design** | Scaffold architecture, suggest design patterns |
| **Development** | Code completion, boilerplate generation, language translation |
| **Testing** | Unit/integration test generation, edge case detection |
| **Code Review** | PR summaries (Enterprise), identify security issues |
| **Documentation** | Auto-generate README, API docs, inline comments |
| **Deployment** | Suggest CI/CD workflows, Dockerfile, cloud config |
| **Maintenance** | Debugging, refactoring, modernizing legacy code |

### Limitations of GitHub Copilot in Developer Workflows
- Cannot run or execute code — it only generates it
- Does not have access to runtime state (live variables, memory)
- Suggestions may not account for your specific business logic
- Quality varies significantly by programming language
- Cannot browse the internet or access external APIs in real time

---

## 4.2 Generating Tests with GitHub Copilot

### Types of Tests Copilot Can Generate

| Test Type | Description | How to Invoke |
|-----------|-------------|--------------|
| **Unit tests** | Test individual functions/methods in isolation | `/tests` or "Write unit tests for this function" |
| **Integration tests** | Test interactions between components | "Write integration tests for this service" |
| **End-to-end tests** | Test full user workflows | "Write Playwright tests for the login flow" |
| **Snapshot tests** | Capture and compare UI output | "Generate snapshot tests for this component" |
| **Edge case tests** | Test boundary conditions and unusual inputs | "What edge cases should I test? Generate tests for them" |
| **Regression tests** | Test that bugs don't reappear | "Write a test that reproduces this bug" |

### Using `/tests` Slash Command

```python
# Select your function, then type /tests in Copilot Chat
def calculate_discount(price: float, discount_pct: float) -> float:
    """Apply a discount percentage to a price."""
    if discount_pct < 0 or discount_pct > 100:
        raise ValueError("Discount must be between 0 and 100")
    return price * (1 - discount_pct / 100)
```

Copilot generates:
```python
import pytest
from mymodule import calculate_discount

def test_basic_discount():
    assert calculate_discount(100, 10) == 90.0

def test_zero_discount():
    assert calculate_discount(100, 0) == 100.0

def test_full_discount():
    assert calculate_discount(100, 100) == 0.0

def test_negative_discount_raises():
    with pytest.raises(ValueError):
        calculate_discount(100, -5)

def test_discount_over_100_raises():
    with pytest.raises(ValueError):
        calculate_discount(100, 101)
```

### Identifying Edge Cases
- Ask Copilot: *"What edge cases should I consider for this function?"*
- Copilot identifies: empty inputs, boundary values, null/None, negative numbers, special characters, very large/small values
- Then ask: *"Generate test cases for all these edge cases"*

### Improving Existing Tests
- Select existing test → Ask Copilot to improve coverage or add assertions
- Ask: *"Are there any gaps in these tests?"*
- Copilot can spot: missing assertions, untested branches, hardcoded values that should be parameterized

### Generating Boilerplate for Test Frameworks

```javascript
// Ask: "Generate Jest test boilerplate for the UserService class"
describe('UserService', () => {
  let userService;
  
  beforeEach(() => {
    userService = new UserService();
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  it('should create a user', async () => {
    // ...
  });
});
```

---

## 4.3 Security and Performance with Copilot

### Identifying Security Vulnerabilities

Copilot can flag and suggest fixes for:

| Vulnerability | Example |
|--------------|---------|
| **SQL Injection** | String concatenation in SQL queries → suggest parameterized queries |
| **Hard-coded secrets** | API keys or passwords in code → suggest environment variables |
| **Insecure cryptography** | Use of MD5/SHA1 → suggest bcrypt or SHA-256 |
| **XSS vulnerabilities** | Unescaped user input in HTML → suggest sanitization |
| **Insecure deserialization** | Blind `eval()` or `pickle.loads()` → flag the risk |
| **Path traversal** | Unchecked file paths from user input |

**To use:** Select suspicious code → Ask Copilot Chat: *"Are there any security vulnerabilities in this code?"*

### Performance Suggestions

Copilot can suggest:
- Replacing nested loops with more efficient algorithms
- Using caching for expensive operations
- Identifying N+1 query problems in ORM code
- Suggesting more efficient data structures
- Recommending async/parallel patterns where appropriate

### Collaborative Code Review with Copilot Enterprise
- Copilot can be added as a reviewer on pull requests
- It provides: security analysis, style feedback, potential bug detection
- PR summaries automatically describe what changed and why

---

## 4.4 Productivity API

### What It Does
- The **Copilot for Business Productivity API** lets organizations measure the impact of Copilot
- Metrics available via: `GET /orgs/{org}/copilot/usage`

### Key Metrics

| Metric | Description |
|--------|-------------|
| **Active users** | Number of developers using Copilot in the period |
| **Suggestions shown** | Total inline suggestions displayed |
| **Suggestions accepted** | Number of suggestions accepted (Tab key) |
| **Acceptance rate** | Accepted / Shown — a key productivity indicator |
| **Lines of code accepted** | Total lines of AI-generated code merged |

> ⭐ **Exam Tip:** The acceptance rate is the most commonly cited metric for measuring Copilot's impact on productivity. Industry averages tend to be around 25–35%.

### Using the Productivity API
- Requires: Copilot Business or Enterprise
- Access: via GitHub REST API with appropriate token
- Use cases: quarterly reporting, ROI analysis, developer adoption tracking

---

## 4.5 Copilot SKUs and Configuration Options

### SKU Summary

| SKU | Data retained for training? | IP Indemnity | Org Policies |
|-----|----------------------------|-------------|-------------|
| Individual | Optional (opt out available) | ❌ | ❌ |
| Business | ❌ (never) | ✅ | ✅ |
| Enterprise | ❌ (never) | ✅ | ✅ + Knowledge Bases |

### The `.github/copilot-instructions.md` File (Copilot Config)
- Place a `copilot-instructions.md` file in the `.github/` folder of your repo
- Copilot uses this file as **persistent context** for all suggestions in that repo
- Use it to: define coding standards, preferred frameworks, naming conventions, patterns to avoid

```markdown
<!-- .github/copilot-instructions.md -->
## Coding Standards
- Use TypeScript strict mode
- Prefer functional components in React
- All functions must have JSDoc comments
- Use Zod for input validation
- Do not use any as a TypeScript type
```

---

> ⬅️ [← Domain 3](./domain-3-data-privacy.md) | [Back to Index](./index.md) | [⚡ Quick Reference →](./quick-reference.md)
