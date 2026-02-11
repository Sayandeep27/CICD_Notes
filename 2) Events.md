# 🚀 GitHub Actions Events – Complete & Easy Guide

A **GitHub Actions event** is something that happens in your repository that can trigger a workflow.

In simple words:

> "When X happens → run my workflow automatically."

This README explains all important GitHub Actions events in **clear, short, and practical language** with examples.

---

# 📌 Basic Workflow Structure

All events are defined under the `on:` keyword.

```yaml
name: Example Workflow

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Workflow Triggered"
```

---

# 📚 Important GitHub Actions Events

---

## 1️⃣ push

Triggers when code is pushed to a branch or tag.

### Example

```yaml
on:
  push:
    branches:
      - main
      - develop
```

### Use Case

* Run tests when code is pushed
* Build project after every commit

---

## 2️⃣ pull_request

Triggers when a pull request is opened, updated, or merged.

### Example

```yaml
on:
  pull_request:
    branches:
      - main
```

### Use Case

* Run CI checks before merging
* Validate PR changes

---

## 3️⃣ pull_request_target

⚠️ Advanced version of `pull_request`.

It runs in the **context of the base repository**, not the fork.

### Example

```yaml
on:
  pull_request_target:
    types: [opened, synchronize]
```

### Important

* Has access to secrets
* Used for labeling, commenting bots
* Be careful with untrusted code

---

## 4️⃣ fork

Triggers when someone forks your repository.

### Example

```yaml
on:
  fork
```

### Use Case

* Analytics
* Notification systems

---

## 5️⃣ create

Triggers when a branch or tag is created.

### Example

```yaml
on:
  create
```

---

## 6️⃣ delete

Triggers when a branch or tag is deleted.

### Example

```yaml
on:
  delete
```

---

## 7️⃣ release

Triggers when a release is created or published.

### Example

```yaml
on:
  release:
    types: [published]
```

### Use Case

* Deploy production
* Upload build artifacts

---

## 8️⃣ schedule

Runs workflow at a scheduled time using CRON.

### Example

```yaml
on:
  schedule:
    - cron: "0 0 * * *"
```

### Meaning

* Runs daily at midnight UTC

---

## 9️⃣ workflow_dispatch (Manual Trigger) ⭐ Detailed

This allows you to run a workflow manually from the GitHub UI.

You can also pass inputs.

---

### Basic Example

```yaml
on:
  workflow_dispatch
```

---

### With Inputs Example

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Choose environment"
        required: true
        default: "dev"

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying to ${{ github.event.inputs.environment }}"
```

---

### How It Works

1. Go to **Actions tab**
2. Select workflow
3. Click **Run workflow**
4. Provide inputs
5. Workflow runs manually

---

### Use Cases

* Manual production deployment
* Hotfix trigger
* Re-run with different configuration
* Admin controlled pipelines

---

## 🔟 repository_dispatch ⭐ Detailed

This event allows one repository or external system to trigger a workflow.

Think of it as:

> "Trigger this repo’s workflow from outside."

---

## 🔹 Why repository_dispatch is Powerful

* Cross-repo automation
* Microservices communication
* Trigger CI after external API call
* Connect external systems (Jenkins, backend server, etc.)

---

## 🔹 Step 1 – Define repository_dispatch in Workflow

```yaml
on:
  repository_dispatch:
    types: [deploy-event]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Triggered by external repo"
```

---

## 🔹 Step 2 – Trigger Using GitHub API

You must send a POST request.

```bash
curl -X POST \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  https://api.github.com/repos/OWNER/REPO/dispatches \
  -d '{"event_type":"deploy-event"}'
```

---

## 🔹 With Custom Data

```bash
-d '{
  "event_type": "deploy-event",
  "client_payload": {
    "environment": "production"
  }
}'
```

Access in workflow:

```yaml
- run: echo "Deploying to ${{ github.event.client_payload.environment }}"
```

---

## 🔹 Real Example Scenario

Repo A (Backend) finishes build →
Triggers Repo B (Deployment repo) →
Deployment workflow runs automatically.

---

# 📋 Other Important Events

| Event         | Purpose                                 |
| ------------- | --------------------------------------- |
| workflow_run  | Trigger when another workflow completes |
| issue_comment | Trigger on issue comment                |
| issues        | Trigger when issue is opened/closed     |
| label         | Trigger when label is added             |
| milestone     | Trigger on milestone changes            |
| check_run     | Trigger on check updates                |
| status        | Trigger on commit status change         |
| watch         | Trigger when repo is starred            |
| discussion    | Trigger on discussions                  |

---

# 🔒 Important Security Notes

* `pull_request` → safer for forked PRs
* `pull_request_target` → has secrets access (be careful)
* `repository_dispatch` → requires Personal Access Token
* Never expose secrets in logs

---

# 🎯 Quick Comparison Table

| Event               | Automatic | Manual | External | Has Secrets | Common Use         |
| ------------------- | --------- | ------ | -------- | ----------- | ------------------ |
| push                | ✅         | ❌      | ❌        | ✅           | CI                 |
| pull_request        | ✅         | ❌      | ❌        | Limited     | PR validation      |
| pull_request_target | ✅         | ❌      | ❌        | ✅           | Label bots         |
| workflow_dispatch   | ❌         | ✅      | ❌        | ✅           | Manual deploy      |
| repository_dispatch | ❌         | ❌      | ✅        | ✅           | Cross repo trigger |
| schedule            | ✅         | ❌      | ❌        | ✅           | Cron jobs          |
| release             | ✅         | ❌      | ❌        | ✅           | Production deploy  |

---

# 🏁 Final Summary

* **push** → When code is pushed
* **pull_request** → When PR is opened/updated
* **pull_request_target** → Advanced PR event (has secrets)
* **fork** → When repo is forked
* **create/delete** → Branch/tag creation or deletion
* **release** → When release is published
* **schedule** → Run at fixed time
* **workflow_dispatch** → Manual trigger (very useful)
* **repository_dispatch** → External trigger (very powerful)

---

# 💡 Pro Tip

Most real-world CI/CD pipelines use:

* `push` for CI
* `pull_request` for PR checks
* `workflow_dispatch` for manual deployments
* `repository_dispatch` for microservice orchestration

---

✔ This document covers all major GitHub Actions events in short, clear language.
✔ Ready to upload directly to GitHub as README.md
✔ Clean and professional format

---

END OF FILE
