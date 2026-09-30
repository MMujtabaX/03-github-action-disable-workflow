# ⏸️ 03 — GitHub Actions: Disabling Workflows

![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI%2FCD-2088FF?logo=githubactions&logoColor=white)
![Status](https://img.shields.io/badge/Workflow-Disabled-lightgrey)

Part 3 of my hands-on **CI/CD and DevOps** series. Sometimes you need to **stop a workflow without deleting it**: to pause a noisy scheduled job, stop a broken pipeline, or temporarily halt deployments. This repo demonstrates disabling a workflow so it **doesn't run even when its trigger fires**.

## 🧩 The Workflow

```yaml
name: Disable Workflow Demo

on:
  push:
    branches: [main]

jobs:
  disable-workflow:
    runs-on: ubuntu-latest
    steps:
      - name: Print message
        run: echo "This workflow is disabled. Even after push, this won't get executed."
```

The YAML is an ordinary push-triggered workflow. It has been **disabled from GitHub's settings**, so pushing to `main` does **not** start a run. The file stays in the repo, ready to be switched back on at any time.

## 🔧 Method 1: Disable via the GitHub UI

1. Open the **Actions** tab of the repository.
2. Select the workflow (**Disable Workflow Demo**) in the left sidebar.
3. Click the **⋯** menu (top right), then **Disable workflow**.

To turn it back on, return to the same page and click **Enable workflow**.

## 💻 Method 2: Disable via GitHub CLI

```bash
gh workflow list                                  # List all workflows and their state
gh workflow disable "Disable Workflow Demo"       # Disable
gh workflow enable "Disable Workflow Demo"        # Re-enable
```

## 📝 Method 3: Skip Jobs Inside the YAML

You can also stop jobs from running by editing the workflow file itself:

```yaml
jobs:
  disable-workflow:
    if: false               # Job is always skipped
    runs-on: ubuntu-latest
```

To skip a **single push**, add `[skip ci]` to the commit message:

```bash
git commit -m "Update docs [skip ci]"
```

## ⚖️ Comparing the Approaches

| Method | Scope | Requires a commit? | Best for |
|--------|-------|--------------------|----------|
| UI or CLI disable | Whole workflow | ❌ No | Pausing a workflow temporarily |
| `if: false` | Specific job | ✅ Yes | Permanently turning off one job |
| `[skip ci]` | One push | ✅ Yes (in the message) | Skipping CI for trivial changes |
| Deleting the file | Whole workflow | ✅ Yes | Removing a workflow for good |

## 🎯 What I Learned

- A workflow can be switched off without deleting or editing its YAML
- Disabled workflows ignore their triggers until re-enabled
- The difference between disabling a workflow, skipping a job, and skipping a single run
- Managing workflows from the command line with `gh`

## 🗺️ Series Roadmap

- [x] [**01:** Hello World](https://github.com/MMujtabaX/01-github-action-hello-world): workflow anatomy and push triggers
- [x] [**02:** Scheduled Workflows](https://github.com/MMujtabaX/02-github-actions-schedule): cron triggers and manual runs
- [x] **03:** Disabling Workflows (this repo)
- [ ] Checking out code and running tests on push
- [ ] Linting and build checks on pull requests
- [ ] Using secrets and environment variables
- [ ] Deploying an application automatically

## 👤 Author

**Muhammad Mujtaba Khan Suri** — CS @ UBIT, University of Karachi
[GitHub](https://github.com/MMujtabaX)
