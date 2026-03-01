# 📝 GH-200: GitHub Actions — 50 Practice MCQs

> Mapped to the official exam domains (updated January 2026):
> - **Domain 1:** Author and Maintain Workflows — 40%
> - **Domain 2:** Consume Workflows — 20%
> - **Domain 3:** Author and Maintain Actions — 25%
> - **Domain 4:** Manage GitHub Actions in the Enterprise — 15%

---

## 🟦 Domain 1: Author and Maintain Workflows (Q1–Q20)

---

**Q1.** A workflow should run only when a pull request targets the `main` or `release` branch. Which trigger configuration is correct?

- A) `on: push: branches: [main, release]`
- B) `on: pull_request: branches: [main, release]`
- C) `on: pull_request_target: branches: [main, release]`
- D) `on: merge: branches: [main, release]`

✅ **Answer: B**
*`pull_request` with a `branches` filter fires when a PR targets those branches. `pull_request_target` runs in the context of the base repo (used for fork PRs with elevated access). `push` fires on direct pushes, not PRs.*

---

**Q2.** You need a job to run only after both `build` and `test` jobs have succeeded. Which syntax achieves this?

- A) `needs: build, test`
- B) `needs: [build, test]`
- C) `depends-on: [build, test]`
- D) `after: [build, test]`

✅ **Answer: B**
*The `needs` keyword accepts an array of job IDs. Both jobs must succeed before the dependent job runs.*

---

**Q3.** Which `on:` trigger allows a workflow to be started manually from the GitHub UI with user-defined inputs?

- A) `workflow_run`
- B) `repository_dispatch`
- C) `workflow_dispatch`
- D) `workflow_call`

✅ **Answer: C**
*`workflow_dispatch` enables manual triggering via the UI or REST API. `workflow_call` makes a workflow reusable; `workflow_run` chains workflows based on another workflow's completion.*

---

**Q4.** A step should run only when the preceding step fails. Which conditional expression is correct?

- A) `if: success()`
- B) `if: failure()`
- C) `if: always()`
- D) `if: cancelled()`

✅ **Answer: B**
*`failure()` evaluates to `true` if any earlier step in the job has failed. `always()` runs regardless of status; `success()` runs only when all prior steps passed.*

---

**Q5.** You want to define a reusable block of YAML steps within a single workflow file to avoid repetition. Which YAML feature should you use?

- A) `include`
- B) Anchors (`&`) and aliases (`*`)
- C) `extends`
- D) `template`

✅ **Answer: B**
*GitHub Actions workflows support standard YAML anchors (`&name`) and aliases (`*name`) along with merge keys (`<<:`) to reuse repeated mappings within the same file.*

---

**Q6.** A matrix strategy builds across `[ubuntu-latest, windows-latest]` and `[node-16, node-18]`. How many total jobs will be created?

- A) 2
- B) 3
- C) 4
- D) 6

✅ **Answer: C**
*The matrix produces the Cartesian product: 2 OS × 2 Node versions = 4 jobs.*

---

**Q7.** You want a matrix job to continue running other combinations even if one combination fails. Which setting should you configure?

- A) `continue-on-error: true` at the job level
- B) `fail-fast: false` under `strategy`
- C) `max-parallel: 0`
- D) `ignore-errors: true`

✅ **Answer: B**
*`fail-fast: false` prevents GitHub Actions from cancelling other in-progress matrix jobs when one fails. `continue-on-error: true` marks a failing job as non-blocking but doesn't prevent cancellations.*

---

**Q8.** Which default environment variable contains the SHA of the commit that triggered the workflow?

- A) `GITHUB_REF`
- B) `GITHUB_SHA`
- C) `GITHUB_RUN_ID`
- D) `GITHUB_COMMIT`

✅ **Answer: B**
*`GITHUB_SHA` holds the full commit SHA. `GITHUB_REF` is the branch or tag ref. `GITHUB_RUN_ID` is the unique ID of the workflow run.*

---

**Q9.** A workflow needs to pass data from a `build` job to a `deploy` job. What is the correct mechanism?

- A) Environment variables set with `export`
- B) Job outputs defined with `outputs:` and referenced via `needs.<job>.outputs.<name>`
- C) Shared workspace mounted between jobs
- D) Writing to `$GITHUB_ENV` in the build job

✅ **Answer: B**
*Job outputs are defined using `outputs:` in the producing job and populated via `echo "name=value" >> $GITHUB_OUTPUT`. They're then consumed using `needs.<job_id>.outputs.<output_name>`.*

---

**Q10.** What is the correct way to set an environment variable that persists for all subsequent steps in a job?

- A) `export MY_VAR=value` in a `run:` step
- B) `echo "MY_VAR=value" >> $GITHUB_ENV`
- C) `env: MY_VAR: value` at the step level
- D) `set-env name=MY_VAR value=value`

✅ **Answer: B**
*Writing `NAME=VALUE` to `$GITHUB_ENV` persists the variable for all subsequent steps. The old `set-env` workflow command was deprecated due to security concerns.*

---

**Q11.** Which workflow trigger fires when another workflow completes, allowing you to chain workflows together?

- A) `workflow_dispatch`
- B) `workflow_call`
- C) `workflow_run`
- D) `repository_dispatch`

✅ **Answer: C**
*`workflow_run` triggers a workflow when a specified workflow completes (successfully, with failure, or on request). It's the correct way to chain separate workflow files.*

---

**Q12.** A step needs to run regardless of whether earlier steps succeeded or failed, but not if the workflow was cancelled. Which condition should be used?

- A) `if: always()`
- B) `if: success() || failure()`
- C) `if: !cancelled()`
- D) `if: failure()`

✅ **Answer: C**
*`!cancelled()` evaluates to `true` as long as the workflow was not cancelled, covering both success and failure states. `always()` would also include cancellations.*

---

**Q13.** You want to limit concurrency so that only one workflow run per branch is active at a time, cancelling any in-progress run when a new one starts. What configuration achieves this?

- A)
```yaml
concurrency:
  group: ${{ github.ref }}
  cancel-in-progress: true
```
- B)
```yaml
max-parallel: 1
```
- C)
```yaml
concurrency: single
```
- D)
```yaml
strategy:
  max-parallel: 1
```

✅ **Answer: A**
*The `concurrency` key with a group name and `cancel-in-progress: true` cancels any running workflow in the same group when a new run starts. `max-parallel` is for matrix jobs only.*

---

**Q14.** Which image registry domain should be used when pushing a container image to GitHub Container Registry?

- A) `docker.io`
- B) `registry.github.com`
- C) `ghcr.io`
- D) `packages.github.com`

✅ **Answer: C**
*GitHub Container Registry uses the `ghcr.io` domain. Image names follow the format `ghcr.io/<owner>/<image>:<tag>`.*

---

**Q15.** A workflow uses a service container running PostgreSQL. Which field is required for the job's container or runner to communicate with the service?

- A) `ports:`
- B) `network:`
- C) `expose:`
- D) `links:`

✅ **Answer: A**
*The `ports:` field under `services:` maps the container port to the host, allowing the workflow steps to connect to the service using `localhost` and the mapped port.*

---

**Q16.** You want to upload a test report as an artifact after each workflow run. Which action should you use?

- A) `actions/cache@v4`
- B) `actions/upload-artifact@v4`
- C) `actions/store-artifact@v4`
- D) `actions/publish-artifact@v4`

✅ **Answer: B**
*`actions/upload-artifact` stores files from the runner for download or use in later jobs. `actions/cache` caches dependencies for performance — it is not for storing build outputs.*

---

**Q17.** Which GITHUB_TOKEN permission must be explicitly set to `write` for a workflow to create a GitHub Release?

- A) `issues: write`
- B) `packages: write`
- C) `contents: write`
- D) `deployments: write`

✅ **Answer: C**
*Creating releases requires writing to the repository's contents. The `contents: write` permission grants access to create releases, tags, and modify repository content.*

---

**Q18.** A scheduled workflow runs at 08:00 UTC every weekday. Which `cron` expression is correct?

- A) `'0 8 * * 1-5'`
- B) `'8 0 * * MON-FRI'`
- C) `'0 8 * * 0-4'`
- D) `'8 * * * 1-5'`

✅ **Answer: A**
*Cron syntax: `minute hour day-of-month month day-of-week`. `0 8 * * 1-5` means at minute 0, hour 8, any day of month, any month, Monday–Friday (1–5).*

---

**Q19.** How should a workflow reference a repository secret named `API_TOKEN` in a `run:` step?

- A) `$API_TOKEN`
- B) `${{ env.API_TOKEN }}`
- C) `${{ secrets.API_TOKEN }}`
- D) `${{ vars.API_TOKEN }}`

✅ **Answer: C**
*Secrets are accessed via the `secrets` context: `${{ secrets.SECRET_NAME }}`. `vars` is used for non-sensitive configuration variables. `env` is for environment variables.*

---

**Q20.** Which workflow command is used to add a value to the system `PATH` for all subsequent steps?

- A) `echo "path" >> $GITHUB_PATH`
- B) `echo "path" >> $GITHUB_ENV`
- C) `export PATH=$PATH:/new/path`
- D) `add-path /new/path`

✅ **Answer: A**
*Appending a path to `$GITHUB_PATH` adds it to the `PATH` environment variable for all subsequent steps. The old `add-path` workflow command was deprecated.*

---

## 🟩 Domain 2: Consume Workflows (Q21–Q30)

---

**Q21.** You want to download an artifact produced by a `build` job within the same workflow run. Which action should you use?

- A) `actions/fetch-artifact@v4`
- B) `actions/download-artifact@v4`
- C) `actions/get-artifact@v4`
- D) `actions/checkout@v4`

✅ **Answer: B**
*`actions/download-artifact` retrieves artifacts uploaded by `actions/upload-artifact` within the same or a prior workflow run. Pair it with the artifact name set during upload.*

---

**Q22.** A reusable workflow is defined with `on: workflow_call`. How should a calling workflow invoke it?

- A) `uses: ./.github/workflows/reusable.yml` under a job's `steps:`
- B) `uses: org/repo/.github/workflows/reusable.yml@main` under a job's `uses:` key
- C) `import: org/repo/.github/workflows/reusable.yml`
- D) `workflow_run: reusable.yml`

✅ **Answer: B**
*Reusable workflows are called via the `uses:` key at the **job** level (not step level), referencing the workflow file path and a ref (branch, tag, or SHA).*

---

**Q23.** When does a dependency cache created with `actions/cache` get invalidated?

- A) After every workflow run
- B) When the `key` does not match any existing cache entry
- C) After 7 days regardless of usage
- D) When the repository is forked

✅ **Answer: B**
*The cache is a miss when the exact `key` doesn't match. GitHub then attempts `restore-keys` fallbacks. Caches that haven't been accessed in 7 days are evicted, but cache invalidation for the workflow specifically happens on key mismatch.*

---

**Q24.** A workflow needs a status badge displayed in a README to show the last run result. Which URL format provides this badge?

- A) `https://github.com/<owner>/<repo>/actions/badge.svg`
- B) `https://github.com/<owner>/<repo>/workflows/<workflow-name>/badge.svg`
- C) `https://github.com/<owner>/<repo>/status/<workflow-name>.svg`
- D) `https://actions.github.com/<owner>/<repo>/badge/<workflow-name>`

✅ **Answer: B**
*The standard workflow badge URL is `https://github.com/<owner>/<repo>/workflows/<workflow-name>/badge.svg`. Add a `?branch=<branch>` query parameter to show a specific branch's status.*

---

**Q25.** A caller workflow passes a secret to a reusable workflow. Which `workflow_call` configuration allows the reusable workflow to inherit all secrets from the caller without listing them individually?

- A) `secrets: inherit`
- B) `secrets: all`
- C) `secrets: pass-through`
- D) `secrets: '*'`

✅ **Answer: A**
*Using `secrets: inherit` in the `with:` block of a `uses:` job passes all of the caller's secrets to the reusable workflow automatically without explicit mapping.*

---

**Q26.** Which environment protection rule requires a named individual or team to approve a deployment before the job continues?

- A) `required-reviewers`
- B) `deployment-gate`
- C) `wait-timer`
- D) `branch-policy`

✅ **Answer: A**
*Environment protection rules include **Required reviewers** (named users or teams must approve), **Wait timer** (a delay before proceeding), and **Deployment branches** (restrict which branches can deploy).*

---

**Q27.** You need a job to wait for a manual approval before deploying to production. What is the correct approach?

- A) Add a `sleep` step in the workflow
- B) Use `workflow_dispatch` with required inputs
- C) Assign the job to an environment with a Required Reviewers protection rule
- D) Set `if: github.event_name == 'approved'`

✅ **Answer: C**
*Environments with Required Reviewers pause workflow execution at that job, waiting for an authorised user to approve via the GitHub UI before the deployment job runs.*

---

**Q28.** A workflow caches Node.js dependencies. The `package-lock.json` has changed. Which cache key strategy ensures the cache is rebuilt when the lockfile changes?

- A) `key: node-modules`
- B) `key: node-${{ runner.os }}-${{ github.sha }}`
- C) `key: node-${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}`
- D) `key: node-${{ github.run_id }}`

✅ **Answer: C**
*Hashing the lockfile ensures a unique key whenever dependencies change. Using `github.sha` creates a new cache every commit (too granular); a static key never invalidates.*

---

**Q29.** A workflow step needs to check the outcome of a previous step named `run-tests`. Which expression correctly reads its result?

- A) `${{ steps.run-tests.result }}`
- B) `${{ jobs.run-tests.outcome }}`
- C) `${{ steps.run-tests.outcome }}`
- D) `${{ steps.run-tests.conclusion }}`

✅ **Answer: C**
*`steps.<step_id>.outcome` gives the result before `continue-on-error` is applied. `steps.<step_id>.conclusion` gives the result after `continue-on-error`. Both are valid — `outcome` is more commonly used in conditionals to detect the raw failure.*

---

**Q30.** Which GitHub Actions context provides information about the repository, such as the owner name and repository name?

- A) `env` context
- B) `runner` context
- C) `github` context
- D) `job` context

✅ **Answer: C**
*The `github` context contains event metadata including `github.repository`, `github.repository_owner`, `github.ref`, `github.sha`, and more.*

---

## 🟧 Domain 3: Author and Maintain Actions (Q31–Q42)

---

**Q31.** Which file is required at the root of a custom action's repository and defines its inputs, outputs, and entry point?

- A) `workflow.yml`
- B) `action.yml`
- C) `config.json`
- D) `Dockerfile`

✅ **Answer: B**
*Every custom action requires an `action.yml` (or `action.yaml`) file that declares the action's `name`, `description`, `inputs`, `outputs`, and `runs` configuration.*

---

**Q32.** What are the three types of custom GitHub Actions?

- A) Shell, Python, and Node.js actions
- B) Docker container, JavaScript, and Composite actions
- C) Bash, PowerShell, and Docker actions
- D) Reusable, custom, and marketplace actions

✅ **Answer: B**
*GitHub Actions supports three action types: **Docker container** (runs code in a container), **JavaScript** (runs Node.js code directly on the runner), and **Composite** (chains multiple `run` steps and other actions).*

---

**Q33.** A JavaScript action should use which Node.js version as recommended by GitHub for the `runs.using` field in `action.yml`?

- A) `node14`
- B) `node16`
- C) `node20`
- D) `node22`

✅ **Answer: C**
*As of 2024–2026, `node20` is the recommended runtime for JavaScript actions. `node14` and `node16` are deprecated. Always check the official docs, as this is updated periodically.*

---

**Q34.** Which semantic versioning practice is recommended for publishing GitHub Actions so consumers can safely pin to a stable major version?

- A) Use timestamp-based tags like `2025.10.04`
- B) Publish only immutable commit SHAs
- C) Create `MAJOR.MINOR.PATCH` tags and maintain a floating major version tag (e.g., `v3`)
- D) Use branch names as version references

✅ **Answer: C**
*Best practice is to publish semantic version tags (`v3.1.2`) and maintain a mutable major tag (`v3`) that points to the latest stable release. This lets consumers use `@v3` and receive non-breaking updates automatically.*

---

**Q35.** In a composite action, how do you call another action as a step?

- A) `run: actions/checkout@v4`
- B) `uses: actions/checkout@v4` within a `steps:` entry
- C) `import: actions/checkout@v4`
- D) `include: actions/checkout@v4`

✅ **Answer: B**
*Composite actions support `steps:` with both `run:` (shell commands) and `uses:` (other actions) entries, allowing you to compose multiple actions together.*

---

**Q36.** A Docker action's `Dockerfile` uses `ENTRYPOINT`. Where should the `ENTRYPOINT` script write data so the calling workflow can read a step output?

- A) Standard output (`stdout`)
- B) A file at `/tmp/output`
- C) `$GITHUB_OUTPUT`
- D) `$GITHUB_ENV`

✅ **Answer: C**
*Actions write outputs using the workflow command format: `echo "name=value" >> $GITHUB_OUTPUT`. This is consistent across JavaScript, composite, and Docker actions.*

---

**Q37.** What is the purpose of `@actions/core` in a JavaScript action?

- A) To clone the repository
- B) To provide toolkit functions like `getInput()`, `setOutput()`, `setFailed()`, and `info()`
- C) To manage GitHub API calls
- D) To publish the action to the marketplace

✅ **Answer: B**
*`@actions/core` is the primary Actions toolkit package providing utilities to read inputs, set outputs, export environment variables, mask sensitive values, and log at different severity levels.*

---

**Q38.** A team wants to run a custom action stored in the same repository as the calling workflow. How should it be referenced?

- A) `uses: actions/<action-name>@v1`
- B) `uses: ./.github/actions/<action-name>`
- C) `uses: local/<action-name>`
- D) `uses: self/<action-name>`

✅ **Answer: B**
*Local actions are referenced using a relative path starting with `./`. The path points to the directory containing the action's `action.yml`.*

---

**Q39.** Which file should be committed to the repository when publishing a JavaScript action, to avoid requiring consumers to run `npm install`?

- A) `package.json`
- B) `node_modules/`
- C) `dist/index.js` (compiled bundle)
- D) `.npmrc`

✅ **Answer: C**
*JavaScript actions should include a compiled bundle (typically produced by tools like `@vercel/ncc`) in the repository. Committing `node_modules/` is not recommended; compiling to a single file is the best practice.*

---

**Q40.** An action input is marked as `required: true` in `action.yml`. What happens if a caller does not provide the input?

- A) The action uses the `default` value
- B) GitHub Actions skips the action silently
- C) The action fails with an error
- D) The input is treated as an empty string

✅ **Answer: C**
*When a required input is not supplied and no default is defined, the `@actions/core` toolkit raises an error and the step fails. The exact error depends on how the action reads inputs.*

---

**Q41.** Which `action.yml` configuration is correct for a Docker-based action that uses a pre-built image from Docker Hub?

- A)
```yaml
runs:
  using: docker
  image: docker://myorg/myimage:latest
```
- B)
```yaml
runs:
  using: container
  image: myorg/myimage:latest
```
- C)
```yaml
runs:
  using: node20
  main: docker://myorg/myimage:latest
```
- D)
```yaml
runs:
  using: docker
  dockerfile: Dockerfile
```

✅ **Answer: A**
*For pre-built images, the `image` field uses the `docker://` prefix. Using a local `Dockerfile` omits the prefix and uses `image: Dockerfile` instead.*

---

**Q42.** How can a GitHub Action mask a sensitive value so it does not appear in workflow logs?

- A) `core.hideInput(value)`
- B) `core.setSecret(value)`
- C) `echo "::add-mask::$VALUE"`
- D) Both B and C are correct

✅ **Answer: D**
*Both `core.setSecret(value)` (in JavaScript actions) and the `::add-mask::` workflow command (in `run:` steps) instruct the runner to redact the value from all subsequent log output.*

---

## 🟥 Domain 4: Manage GitHub Actions in the Enterprise (Q43–Q50)

---

**Q43.** An organisation admin wants to allow only actions authored by GitHub and verified marketplace publishers. Which policy setting achieves this?

- A) Allow all actions
- B) Allow local actions only
- C) Allow GitHub-authored actions and actions from verified marketplace publishers
- D) Disable GitHub Actions for the organisation

✅ **Answer: C**
*Organisation and enterprise admins can restrict actions to: all actions, only local actions, GitHub-authored plus verified creators, or specific allowed actions. Option C is the targeted policy described.*

---

**Q44.** What is the primary security risk of using `pull_request_target` instead of `pull_request` for workflows triggered by fork pull requests?

- A) The workflow has no access to secrets
- B) The workflow runs in the context of the base repository and may grant fork code access to secrets and elevated permissions
- C) The workflow cannot check out the PR's code
- D) The workflow cannot comment on the pull request

✅ **Answer: B**
*`pull_request_target` runs in the base repo's context with access to secrets, which is dangerous if untrusted fork code is checked out and executed in the same job. This can lead to secret exfiltration.*

---

**Q45.** A company requires that all self-hosted runners used in production workflows have a static egress IP for allowlisting. Which runner solution satisfies this requirement?

- A) GitHub-hosted `ubuntu-latest` runners
- B) GitHub-hosted larger runners with static IPs
- C) Self-hosted runners deployed in the company's own network
- D) Both B and C are correct

✅ **Answer: D**
*GitHub-hosted larger runners (available on GitHub Team/Enterprise) support static IP ranges. Self-hosted runners inherently use the organisation's own network IPs. Both satisfy a static egress IP requirement.*

---

**Q46.** Which GitHub Actions feature allows an organisation to define a standard set of steps (e.g., security scans, notifications) that are automatically injected into every workflow run, without requiring individual teams to add them?

- A) Reusable workflows
- B) Required workflows (Organisation-level)
- C) Starter workflows
- D) Composite actions

✅ **Answer: B**
*Required workflows (available at the organisation level) are enforced on all repositories matching a policy. They run automatically alongside repository workflows without requiring developers to include them. Starter workflows are templates, not enforced.*

---

**Q47.** A secret is 60 KB in size, exceeding the repository secret size limit. What is the recommended approach to use this secret in a workflow?

- A) Split the secret into multiple smaller secrets
- B) Store the secret in an external secrets manager (e.g., Azure Key Vault, HashiCorp Vault) and retrieve it at runtime
- C) Base64-encode the secret before storing it
- D) Commit the secret to a private repository and check it out

✅ **Answer: B**
*For large secrets, the recommended approach is to use an external secrets manager and a dedicated action (e.g., `azure/get-keyvault-secrets`) to fetch the secret during the workflow run.*

---

**Q48.** An enterprise wants to ensure that workflows can only deploy to production from the `main` branch. Which configuration enforces this?

- A) Branch protection rules on `main`
- B) A deployment environment protection rule restricting **Deployment branches** to `main`
- C) A `push` trigger filter: `branches: [main]`
- D) A reusable workflow with a branch check step

✅ **Answer: B**
*Environment protection rules include a **Deployment branches** policy that restricts which branches (or branch patterns) are permitted to deploy to that environment. This is enforced by GitHub, not just the workflow YAML.*

---

**Q49.** Which GITHUB_TOKEN permission level is granted by default to workflows triggered by pull requests from **forked** repositories?

- A) `write` for all scopes
- B) `read` for all scopes
- C) No permissions at all
- D) `write` for `pull-requests` only

✅ **Answer: B**
*For security, workflows triggered by fork pull requests receive a read-only `GITHUB_TOKEN` by default to prevent untrusted code from writing to the base repository.*

---

**Q50.** An administrator wants to view the total GitHub Actions usage (minutes consumed) across all repositories in an organisation during the current billing cycle. Where should they look?

- A) Repository Settings → Actions → Usage
- B) Organisation Settings → Billing and plans → Usage this month → Actions
- C) Repository Insights → Actions → Billing
- D) Enterprise Settings → Actions → Runners → Usage

✅ **Answer: B**
*Organisation-level billing for GitHub Actions minutes is found under **Organisation Settings → Billing and plans**. It shows minutes used per OS type (Linux, macOS, Windows) across all repositories for the current billing period.*

---

## 📊 Summary by Domain

| Domain | Questions | Count |
|--------|-----------|-------|
| 1 – Author & Maintain Workflows | Q1–Q20 | 20 |
| 2 – Consume Workflows | Q21–Q30 | 10 |
| 3 – Author & Maintain Actions | Q31–Q42 | 12 |
| 4 – Manage in the Enterprise | Q43–Q50 | 8 |
| **Total** | | **50** |

---

## 🔗 Study Resources

- 📘 [GH-200 Official Study Guide – Microsoft Learn](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/gh-200)
- 📖 [GitHub Actions Documentation](https://docs.github.com/en/actions)
- 🧪 [GitHub Skills – Actions Labs](https://skills.github.com)
- 🎓 [Exam Registration](https://examregistration.github.com/)

---

*Good luck with your GH-200 exam! 🚀*
