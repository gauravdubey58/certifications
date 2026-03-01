# Domain 3 – Author and Maintain Actions `25%`

> ⬅️ [Back to GH-200 Index](./index.md)

---

## 3.1 Action Types

### Comparison of Action Types

| Type | Runtime | Speed | Platform | Best For |
|------|---------|-------|----------|----------|
| **JavaScript** | Node.js | ⚡ Fast (no container) | All platforms | Simple logic, API calls |
| **Docker Container** | Any language | 🐢 Slower (container spin-up) | Linux only | Complex tools, specific envs |
| **Composite** | YAML + shell | ⚡ Fast | All platforms | Reusing steps across workflows |

---

### JavaScript Actions

```yaml
# action.yml
name: 'My JS Action'
description: 'Does something useful'
inputs:
  myInput:
    description: 'An input parameter'
    required: true
    default: 'default value'
outputs:
  myOutput:
    description: 'The result'
runs:
  using: 'node20'
  main: 'dist/index.js'
  pre: 'dist/setup.js'     # Optional: runs before main
  post: 'dist/cleanup.js'  # Optional: runs after main (even on failure)
```

```javascript
// index.js
const core = require('@actions/core');
const github = require('@actions/github');

async function run() {
  try {
    const input = core.getInput('myInput');
    core.info(`Processing: ${input}`);
    core.setOutput('myOutput', 'result value');
  } catch (error) {
    core.setFailed(error.message);  // Sets exit code 1
  }
}

run();
```

**@actions/core key functions:**

| Function | Purpose |
|----------|---------|
| `core.getInput('name')` | Read workflow input |
| `core.setOutput('name', value)` | Set step output |
| `core.setFailed('msg')` | Fail the step with message |
| `core.info('msg')` | Log info message |
| `core.warning('msg')` | Log warning |
| `core.error('msg')` | Log error |
| `core.debug('msg')` | Log debug (only shown with debug logging) |
| `core.setSecret('value')` | Mask value in all future logs |
| `core.exportVariable('name', val)` | Set env var for subsequent steps |
| `core.addPath('/path')` | Add to PATH for subsequent steps |

> ⭐ **Exam Tip:** JavaScript actions must have their `node_modules` committed **OR** use a bundler like `@vercel/ncc` and commit the `dist/` folder. Source files alone are NOT enough.

**Troubleshooting JavaScript Actions:**
- Enable `ACTIONS_STEP_DEBUG=true` secret for verbose logs
- Check Node.js version compatibility (`node20` is current recommended)
- Verify `dist/` folder is committed and up to date
- Check `@actions/core` error handling and exit codes

---

### Docker Container Actions

```yaml
# action.yml
name: 'My Docker Action'
description: 'Runs in a container'
inputs:
  myInput:
    description: 'An input'
    required: true
runs:
  using: 'docker'
  image: 'Dockerfile'               # Build from local Dockerfile
  # OR use pre-built image:
  # image: 'docker://alpine:3.18'
  env:
    MY_ENV: ${{ inputs.myInput }}
  args:
    - ${{ inputs.myInput }}
```

```dockerfile
# Dockerfile
FROM alpine:3.18
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
```

```bash
#!/bin/sh
# entrypoint.sh
echo "Input: $1"
echo "result=done" >> $GITHUB_OUTPUT
```

**Troubleshooting Docker Actions:**
- Test `docker build` locally first
- Verify entrypoint is executable (`chmod +x`)
- Remember: Docker actions only run on **Linux runners**
- Inspect container startup logs for image pull/build errors

---

### Composite Actions

```yaml
# action.yml
name: 'My Composite Action'
description: 'Combines multiple steps'
inputs:
  working-directory:
    description: 'Directory to run in'
    default: '.'
outputs:
  result:
    description: 'The result'
    value: ${{ steps.do_thing.outputs.result }}
runs:
  using: 'composite'
  steps:
    - name: Setup
      shell: bash
      run: echo "Setting up in ${{ inputs.working-directory }}"

    - name: Do the thing
      id: do_thing
      shell: bash
      working-directory: ${{ inputs.working-directory }}
      run: |
        RESULT=$(./run.sh)
        echo "result=$RESULT" >> $GITHUB_OUTPUT

    - name: Use another action
      uses: actions/setup-node@v4
      with:
        node-version: '20'
```

> ⭐ **Exam Tip:** Composite actions require `shell:` to be specified on every `run:` step. This is different from regular workflow steps.

---

## 3.2 Action Components

### Required File and Directory Structure

| File | Required | Purpose |
|------|----------|---------|
| `action.yml` | ✅ Always | Metadata: inputs, outputs, runtime |
| `index.js` or `src/` | JS actions | Source code |
| `dist/` | JS actions | Compiled/bundled code (must be committed) |
| `Dockerfile` | Docker actions | Container definition |
| `README.md` | Recommended | Documentation and usage examples |

### action.yml Full Reference

```yaml
name: 'Action Display Name'             # Required
description: 'What this action does'    # Required
author: 'Your Name'

inputs:
  input-name:
    description: 'What this input does'
    required: true
    default: 'default value'
    type: string                        # string, boolean, number (composite only)

outputs:
  output-name:
    description: 'What this output contains'
    value: ${{ steps.step-id.outputs.value }}  # Composite only

runs:
  using: 'node20'        # node20, docker, or composite
  main: 'dist/index.js'

branding:                # For GitHub Marketplace listing
  icon: 'rocket'
  color: 'blue'
```

### Exit Codes

| Exit Code | Meaning | How to Set |
|-----------|---------|-----------|
| `0` | Success | Default / `core.setOutput()` / `exit 0` |
| `1` | Failure | `core.setFailed('msg')` / `exit 1` / `false` |

```bash
# Shell example
#!/bin/bash
if [ "$INPUT" == "valid" ]; then
  echo "Success"
  exit 0
else
  echo "::error::Invalid input"
  exit 1
fi
```

### Workflow Commands Inside Actions

```bash
# Error with annotation
echo "::error file=app.js,line=10,col=5::Missing semicolon"

# Warning annotation
echo "::warning::This is deprecated"

# Notice annotation
echo "::notice::Build completed"

# Debug (only shown when ACTIONS_STEP_DEBUG=true)
echo "::debug::Internal state: $STATE"

# Mask sensitive value
echo "::add-mask::$MY_SECRET"

# Group log output
echo "::group::Dependency Installation"
npm install
echo "::endgroup::"
```

---

## 3.3 Using Actions from the Marketplace

### Referencing Actions

```yaml
steps:
  # From GitHub Marketplace (public repo)
  - uses: actions/checkout@v4          # Pinned to major version tag

  # Pinned to exact SHA (most secure)
  - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683

  # From same repo
  - uses: ./.github/actions/my-action

  # From different repo
  - uses: my-org/my-repo/.github/actions/my-action@main

  # Docker Hub image
  - uses: docker://node:20-alpine
```

> ⭐ **Exam Tip:** Pinning to a full SHA is the most secure option for enterprise environments. It prevents supply chain attacks from tag mutation.

---

> ⬅️ [← Domain 2](./domain-2-consume-workflows.md) | [Back to Index](./index.md) | [Domain 4 →](./domain-4-manage-enterprise.md)
