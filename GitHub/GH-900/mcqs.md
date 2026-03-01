# 📝 GH-900 Practice MCQs — 50 Questions with Answers

> ⬅️ [Back to GH-900 Index](./index.md)

**Instructions:** Attempt each question before reading the answer. Aim for ≥ 80%.

---

## Domain 1 – Introduction to Git and GitHub (Questions 1–13)

**Q1.** What is Git?
- A) A cloud platform for hosting repositories
- B) A distributed version control system for tracking changes in source code
- C) A programming language for automation
- D) A GitHub feature for managing issues

> ✅ **Answer: B — A distributed version control system**
> Git is a distributed VCS that allows multiple developers to work on the same project simultaneously by tracking every change to every file.

---

**Q2.** What is the difference between `git fetch` and `git pull`?
- A) They are identical commands
- B) `fetch` downloads changes without merging; `pull` downloads and merges
- C) `pull` downloads changes without merging; `fetch` downloads and merges
- D) `fetch` only works with remote repos; `pull` works locally

> ✅ **Answer: B**
> `git fetch` retrieves remote changes into your local remote-tracking branches. `git pull` does `fetch` + `merge` in one step.

---

**Q3.** Which command creates a new local Git repository in the current directory?
- A) `git start`
- B) `git new`
- C) `git init`
- D) `git create`

> ✅ **Answer: C — `git init`**
> `git init` initializes a new Git repository, creating a `.git` directory in the current folder.

---

**Q4.** What does `git commit -m "message"` do?
- A) Sends changes to the remote repository
- B) Saves a snapshot of staged changes to the local repo with a message
- C) Stages all modified files
- D) Creates a new branch

> ✅ **Answer: B**
> `git commit` saves staged changes as a snapshot (commit) in the local repository history with a descriptive message.

---

**Q5.** A developer wants to download an existing GitHub repository to their local machine. Which command should they use?
- A) `git init`
- B) `git fork`
- C) `git pull`
- D) `git clone <url>`

> ✅ **Answer: D — `git clone <url>`**
> `git clone` copies a remote repository (including all its history) to your local machine.

---

**Q6.** What is a branch in Git?
- A) A copy of an entire repository
- B) An isolated, parallel line of development
- C) A read-only snapshot of code
- D) A tag pointing to a specific commit

> ✅ **Answer: B — An isolated, parallel line of development**
> Branches allow developers to work on features or fixes without affecting the main codebase.

---

**Q7.** A developer stages changes with `git add` and commits with `git commit`. The changes are still not visible to teammates on GitHub. What is missing?
- A) `git merge`
- B) `git push`
- C) `git fetch`
- D) `git tag`

> ✅ **Answer: B — `git push`**
> `git push` sends committed changes from the local repo to the remote repository on GitHub.

---

**Q8.** What is a GitHub Pull Request?
- A) A request to download the latest code from a remote repository
- B) A way to ask the repository owner to pull changes from your branch into another branch
- C) A request to reset all changes to the last commit
- D) A way to fork a repository

> ✅ **Answer: B**
> A Pull Request is a request to merge changes from one branch into another, enabling code review and discussion before the merge.

---

**Q9.** What is the default name of the primary branch created when initializing a new GitHub repository?
- A) `master`
- B) `trunk`
- C) `main`
- D) `develop`

> ✅ **Answer: C — `main`**
> GitHub changed the default branch name from `master` to `main`. New repositories on GitHub default to `main`.

---

**Q10.** Which of the following best describes the difference between a local and a remote repository?
- A) A local repo is read-only; a remote repo is writable
- B) A local repo is on your machine; a remote repo is hosted on a server (e.g., GitHub)
- C) They are identical copies with no functional differences
- D) A remote repo stores issues; a local repo stores code

> ✅ **Answer: B**
> A local repository lives on your computer. A remote repository (like on GitHub.com) is hosted on a server and shared with others.

---

**Q11.** What does `git merge` do?
- A) Deletes a branch after it's no longer needed
- B) Integrates changes from one branch into the current branch
- C) Resets the current branch to a specific commit
- D) Fetches the latest commits from the remote

> ✅ **Answer: B**
> `git merge <branch>` combines the commit history of the specified branch into the currently checked-out branch.

---

**Q12.** Which GitHub feature allows you to track bugs, tasks, and feature requests within a repository?
- A) Pull Requests
- B) Releases
- C) Issues
- D) Actions

> ✅ **Answer: C — Issues**
> GitHub Issues is the built-in task and bug tracking system where teams can create, assign, label, and close work items.

---

**Q13.** You want to automatically close Issue #42 when a PR is merged. What should you add to the PR description?
- A) `Ref #42`
- B) `Closes #42`
- C) `Fix issue 42`
- D) `Merge #42`

> ✅ **Answer: B — `Closes #42`**
> Using `Closes`, `Fixes`, or `Resolves` followed by `#<issue-number>` in a PR description automatically closes the linked issue when the PR is merged.

---

## Domain 2 – Working with GitHub Repositories (Questions 14–18)

**Q14.** What is the purpose of the `.github/CODEOWNERS` file?
- A) List all contributors to the project
- B) Automatically assign reviewers to PRs based on which files were changed
- C) Define which files are excluded from search indexing
- D) Store repository configuration settings

> ✅ **Answer: B**
> CODEOWNERS maps file paths to GitHub users or teams who are automatically requested as reviewers when those files are changed in a PR.

---

**Q15.** Which keyboard shortcut opens the GitHub web-based VS Code editor (github.dev) for any repository?
- A) `G`
- B) `Ctrl+E`
- C) `.` (period key)
- D) `Shift+S`

> ✅ **Answer: C — `.` (period key)**
> Pressing `.` on any GitHub repo page opens github.dev — a full VS Code editor in the browser.

---

**Q16.** A repository is set to "Internal" visibility. Who can see it?
- A) Everyone on the internet
- B) Only invited collaborators
- C) All members of the organization
- D) Only the repository owner

> ✅ **Answer: C — All members of the organization**
> "Internal" is an organization-level visibility available on GitHub Enterprise Cloud. It's visible to all org members but not the public.

---

**Q17.** What does the "Blame" view in a GitHub file show?
- A) Which contributors have made the most errors
- B) Which commit and author last modified each line of a file
- C) A list of all open issues related to the file
- D) A diff between the current file and the main branch

> ✅ **Answer: B**
> Git blame shows which commit introduced each line of a file and who authored that commit — useful for understanding why code was written a certain way.

---

**Q18.** What is a repository template on GitHub?
- A) A pre-built README file
- B) A repository that others can use as a starting point for new repos with the same structure
- C) A branch with starter code that auto-populates on clone
- D) A GitHub Pages theme

> ✅ **Answer: B**
> Marking a repo as a template adds a "Use this template" button, letting others create new repos with the same files and directory structure.

---

## Domain 3 – Collaboration Features (Questions 19–28)

**Q19.** What is the difference between forking and cloning a repository?
- A) Forking is done via CLI; cloning is done in the browser
- B) Forking creates a copy on GitHub under your account; cloning creates a local copy on your machine
- C) Forking is for public repos; cloning is for private repos
- D) They are the same operation

> ✅ **Answer: B**
> Fork = a new copy of the repo under your GitHub account (server-side). Clone = download a repo to your local machine.

---

**Q20.** What does "Squash and merge" do when merging a Pull Request?
- A) Deletes all commits from the feature branch permanently
- B) Combines all commits from the PR into one single commit on the base branch
- C) Replays each commit individually onto the base branch
- D) Resets the base branch to the state before the PR

> ✅ **Answer: B — Combines all commits into one**
> Squash and merge collapses all PR commits into a single commit, keeping the main branch history clean.

---

**Q21.** A PR has been reviewed and changes have been requested. The author pushes a new commit to address the feedback. Which review setting automatically invalidates the previous approval?
- A) Require status checks
- B) Dismiss stale pull request approvals when new commits are pushed
- C) Require conversation resolution before merging
- D) Require signed commits

> ✅ **Answer: B — Dismiss stale pull request approvals**
> This branch protection option re-requires review approval whenever new commits are pushed to the PR branch.

---

**Q22.** What is a "Draft Pull Request"?
- A) A PR that has been rejected and sent back for revision
- B) A PR that signals work-in-progress and cannot be merged until marked ready
- C) A PR created automatically by Dependabot
- D) A PR with no description or title

> ✅ **Answer: B**
> Draft PRs signal that work is still in progress. They can receive early feedback but cannot be merged until the author marks them "Ready for review."

---

**Q23.** What is the purpose of GitHub Actions in a repository?
- A) Provide AI code suggestions in the IDE
- B) Automate workflows such as CI/CD, testing, and deployment
- C) Manage repository access and permissions
- D) Track issues and pull requests

> ✅ **Answer: B**
> GitHub Actions automates tasks triggered by events like `push`, `pull_request`, or `schedule` — enabling CI/CD, testing, notifications, and more.

---

**Q24.** Which of the following is a valid GitHub Actions trigger event?
- A) `on: commit`
- B) `on: save`
- C) `on: push`
- D) `on: deploy`

> ✅ **Answer: C — `on: push`**
> Valid triggers include `push`, `pull_request`, `schedule`, `workflow_dispatch`, `release`, and many others. `commit` and `save` are not valid events.

---

**Q25.** Which label is specifically designed to invite new contributors to an open-source project?
- A) `needs-review`
- B) `bug`
- C) `good first issue`
- D) `priority: high`

> ✅ **Answer: C — `good first issue`**
> This is the standard GitHub label for issues that are suitable for new contributors — GitHub also surfaces these on the Explore page.

---

**Q26.** What does the `@mentions` feature do in GitHub issues and PRs?
- A) Tags a file for inclusion in the PR
- B) Notifies the mentioned user or team by sending them a notification
- C) Assigns the issue to the mentioned user automatically
- D) Labels the issue with the mentioned category

> ✅ **Answer: B**
> `@username` or `@team-name` in a comment triggers a notification to that person or team, drawing their attention to the discussion.

---

**Q27.** In a GitHub Actions workflow, what is a "job"?
- A) A single command run in the workflow
- B) A set of steps that run on the same runner
- C) A configuration file for the workflow
- D) A trigger event for the workflow

> ✅ **Answer: B**
> A job is a group of steps that execute on the same runner. Multiple jobs in a workflow can run in parallel or sequentially with `needs:`.

---

**Q28.** You want to prevent any code from being merged directly into `main` without at least one approving review. Which feature should you use?
- A) Repository templates
- B) Branch protection rules — Require pull request reviews
- C) CODEOWNERS file
- D) GitHub Actions

> ✅ **Answer: B — Branch protection rules**
> Branch protection rules on `main` with "Require pull request reviews before merging" enforces this requirement.

---

## Domain 4 & 5 – Modern Development & Project Management (Questions 29–36)

**Q29.** What is Continuous Integration (CI)?
- A) Automatically deploying code to production on every push
- B) Automatically building and testing code whenever changes are pushed
- C) A method of merging branches once a week
- D) A GitHub feature for monitoring repository health

> ✅ **Answer: B**
> CI automatically builds and runs tests whenever code is pushed, catching integration issues early and giving fast feedback.

---

**Q30.** What is the purpose of GitHub Environments in GitHub Actions?
- A) Define different runner machine types
- B) Store secrets that apply globally to all workflows
- C) Define deployment targets with protection rules and required approvals
- D) Group multiple workflows into one pipeline

> ✅ **Answer: C**
> Environments (dev, staging, production) let you add protection rules, required reviewers, and environment-specific secrets for deployment jobs.

---

**Q31.** Which GitHub Projects view provides a timeline/Gantt-style layout for planning releases?
- A) Board view
- B) Table view
- C) Roadmap view
- D) Calendar view

> ✅ **Answer: C — Roadmap view**
> The Roadmap view in GitHub Projects shows items on a timeline, useful for sprint planning and release scheduling.

---

**Q32.** What is the purpose of a Milestone in GitHub?
- A) Mark the most important commit in a branch
- B) Group issues and PRs that belong to a specific goal or release, showing progress
- C) Tag a specific version of the code
- D) Pin an important issue to the top of the issues list

> ✅ **Answer: B**
> Milestones group related issues and PRs under a shared goal (e.g., v2.0 release), showing a percentage of completion.

---

**Q33.** What does "shift-left testing" mean in DevOps?
- A) Moving all tests to run after deployment
- B) Running tests earlier in the development lifecycle to catch bugs sooner
- C) Moving the CI server to a different region
- D) Reducing the number of test cases

> ✅ **Answer: B**
> Shift-left means testing earlier — in development rather than at the end — catching defects when they are cheapest to fix.

---

**Q34.** A project board has columns "Backlog", "In Progress", and "Done". This is an example of which GitHub Projects view?
- A) Table view
- B) Roadmap view
- C) Board view (Kanban)
- D) Calendar view

> ✅ **Answer: C — Board view (Kanban)**
> The Board view uses column-based layout — classic Kanban-style workflow visualisation.

---

**Q35.** What is a "reusable workflow" in GitHub Actions?
- A) A workflow that runs on a scheduled timer
- B) A workflow that can be called by other workflows using `uses:`
- C) A workflow that automatically restarts after failure
- D) A pre-built workflow from GitHub Marketplace

> ✅ **Answer: B**
> Reusable workflows use the `workflow_call` trigger and can be called from other workflows with `uses: org/repo/.github/workflows/file.yml@main`.

---

**Q36.** Which GitHub feature allows you to automate moving an issue to "In Progress" when a linked PR is opened?
- A) GitHub Discussions
- B) GitHub Projects automation / workflows
- C) Dependabot
- D) Branch protection rules

> ✅ **Answer: B — GitHub Projects automation**
> GitHub Projects has built-in workflow automation (e.g., auto-set status based on PR or issue state changes).

---

## Domain 6 – Security and Administration (Questions 37–44)

**Q37.** Dependabot raises an alert for a critical vulnerability in a dependency. What does Dependabot Security Updates do automatically?
- A) Deletes the vulnerable dependency from the project
- B) Sends an email to the repository owner only
- C) Creates a Pull Request to update the dependency to a safe version
- D) Reverts the last commit that introduced the dependency

> ✅ **Answer: C**
> Dependabot Security Updates automatically creates a PR that bumps the vulnerable dependency to the patched version.

---

**Q38.** A developer accidentally pushes an API key to a GitHub repository. Which GitHub feature would detect this?
- A) Dependabot
- B) Code scanning (CodeQL)
- C) Secret scanning
- D) Branch protection rules

> ✅ **Answer: C — Secret scanning**
> Secret scanning detects patterns matching known credentials (API keys, tokens) in repository content and alerts the owner.

---

**Q39.** What is the purpose of "Push Protection" in GitHub secret scanning?
- A) Prevent pushes to protected branches without a review
- B) Block a push that contains a detected secret before it is committed to the repo
- C) Require GPG-signed commits for all pushes
- D) Scan pushed code for security vulnerabilities

> ✅ **Answer: B**
> Push protection intercepts a push at the point of `git push` and blocks it if it contains a known secret pattern — preventing leakage before it occurs.

---

**Q40.** Which role in a GitHub organization has the ability to manage security alerts across ALL repositories?
- A) Member
- B) Billing Manager
- C) Security Manager
- D) Repository Admin

> ✅ **Answer: C — Security Manager**
> The Security Manager role is an org-level role specifically for managing security alerts (Dependabot, code scanning, secret scanning) across all repositories.

---

**Q41.** What does the "Dismiss stale reviews" branch protection option do?
- A) Automatically merges PRs after 7 days of inactivity
- B) Invalidates existing approvals when new commits are pushed to the PR
- C) Closes PRs that have been inactive for 30 days
- D) Prevents reviews from being added to draft PRs

> ✅ **Answer: B**
> When enabled, any new commit pushed to the PR branch dismisses existing approvals, requiring fresh reviews of the updated code.

---

**Q42.** Where do you configure Dependabot version updates (keeping ALL dependencies up to date)?
- A) Repository → Settings → Security
- B) `.github/dependabot.yml` file
- C) Organization → Settings → Dependabot
- D) Repository → Insights → Dependency graph

> ✅ **Answer: B — `.github/dependabot.yml`**
> Dependabot version updates require a `dependabot.yml` configuration file in the `.github/` directory specifying the package ecosystem and schedule.

---

**Q43.** SAML Single Sign-On (SSO) for GitHub organizations requires which GitHub plan?
- A) GitHub Free
- B) GitHub Pro
- C) GitHub Team
- D) GitHub Enterprise Cloud (GHEC)

> ✅ **Answer: D — GitHub Enterprise Cloud**
> SAML SSO is an enterprise feature available on GitHub Enterprise Cloud (GHEC), allowing orgs to authenticate members via their identity provider.

---

**Q44.** Which of the following is true about Code Scanning with CodeQL?
- A) It only scans for leaked credentials
- B) It scans third-party dependencies for known CVEs
- C) It performs static analysis of your source code to find security vulnerabilities and bugs
- D) It runs only when manually triggered

> ✅ **Answer: C**
> CodeQL is a static analysis engine that finds logic errors and security vulnerabilities in your own code by analysing its structure and flow.

---

## Domain 7 – GitHub Community (Questions 45–50)

**Q45.** What is the key difference between GitHub Issues and GitHub Discussions?
- A) Issues support code comments; Discussions do not
- B) Issues are for tracking tasks and bugs; Discussions are for open-ended conversations and Q&A
- C) Discussions are private; Issues are public
- D) Issues can be assigned; Discussions cannot

> ✅ **Answer: B**
> Issues are task-oriented and closeable. Discussions support threaded Q&A, ideas, and community conversation with a markable "best answer."

---

**Q46.** Which open-source licence is considered the most permissive and widely used on GitHub?
- A) GPL v3
- B) Apache 2.0
- C) MIT
- D) BSD 3-Clause

> ✅ **Answer: C — MIT**
> The MIT licence is the most permissive commonly used licence — allows use, modification, and distribution with minimal restrictions (just include the copyright notice).

---

**Q47.** What is InnerSource?
- A) A GitHub feature for scanning internal code for vulnerabilities
- B) Applying open-source collaboration practices (fork + PR) within a private organization
- C) A plan for enterprise customers with private repos
- D) GitHub's AI-powered code search feature

> ✅ **Answer: B**
> InnerSource brings open-source contribution practices (forks, PRs, reviews) to internal software development, encouraging cross-team collaboration.

---

**Q48.** What is the purpose of a `CONTRIBUTING.md` file in a repository?
- A) List all past contributors with their commit counts
- B) Explain how external contributors can submit changes and follow project standards
- C) Define the licence under which the project is released
- D) Automate contribution tracking with GitHub Actions

> ✅ **Answer: B**
> `CONTRIBUTING.md` guides potential contributors on how to report bugs, suggest features, and submit pull requests according to the project's standards.

---

**Q49.** How do you make your profile README visible on your GitHub profile page?
- A) Upload a README.md to your company's organization
- B) Create a repository with the same name as your GitHub username and add a README.md
- C) Set your profile visibility to "Public" in settings
- D) Pin a repository to your profile

> ✅ **Answer: B**
> A "special" repository named the same as your username (e.g., `gauravdubey58/gauravdubey58`) displays its `README.md` on your profile page.

---

**Q50.** Which GitHub label is paired with `good first issue` to specifically signal that a project welcomes outside help?
- A) `priority: low`
- B) `status: open`
- C) `help wanted`
- D) `up for grabs`

> ✅ **Answer: C — `help wanted`**
> `help wanted` is the standard GitHub label signalling that the maintainers welcome contributions on this issue. It's often paired with `good first issue`.

---

## 📊 Score Tracker

| Domain | Questions | Your Score |
|--------|-----------|-----------|
| Domain 1 – Git and GitHub Intro | Q1–Q13 (13 questions) | /13 |
| Domain 2 – Working with Repositories | Q14–Q18 (5 questions) | /5 |
| Domain 3 – Collaboration Features | Q19–Q28 (10 questions) | /10 |
| Domain 4 & 5 – Modern Dev & Project Mgmt | Q29–Q36 (8 questions) | /8 |
| Domain 6 – Security and Administration | Q37–Q44 (8 questions) | /8 |
| Domain 7 – GitHub Community | Q45–Q50 (6 questions) | /6 |
| **Total** | **50 questions** | **/50** |

**Scoring guide:**
- 45–50 ✅ Excellent — You're ready!
- 38–44 🟡 Good — Review weak areas
- Below 38 🔴 Keep studying — revisit the notes

---

> ⬅️ [Back to GH-900 Index](./index.md) | ⚡ [Quick Reference](./quick-reference.md)
