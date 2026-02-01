# GitHub Actions Events – Complete Cheat Sheet

This README provides a **complete list of commonly used GitHub Actions events** with clear explanations. You can use it as a quick reference while designing CI/CD workflows.

---

## Code & Repository Events

### `push`

Triggered when code is pushed to a branch or tag.
Commonly used for CI pipelines (build, test, lint).

### `pull_request`

Triggered on pull request activities such as `opened`, `synchronize`, or `closed`.
Used for validating PRs before merging.

### `pull_request_target`

Runs in the **base repository context** (has access to secrets).
Used carefully for PRs from forks.

### `create`

Triggered when a branch or tag is created.

### `delete`

Triggered when a branch or tag is deleted.

### `fork`

Triggered when someone forks the repository.

---

## Manual & External Triggers

### `workflow_dispatch`

Manually trigger a workflow from GitHub UI or API.
Supports user-defined inputs.

### `repository_dispatch`

Triggered via GitHub API from an external system.
Used for integrations and cross-system triggers.


workflow_dispatch

What it is
A manual trigger for a GitHub Actions workflow.
You (or a script) explicitly click Run workflow from the GitHub UI or call it via API.

Used inside GitHub Actions.

Why it exists

Sometimes you don’t want automation.
You want control.

Examples:

Deploy only when you are confident

Run a pipeline with custom inputs

Trigger a job on demand

How it works (simple)

Go to Actions tab

Select workflow

Click Run workflow

Optionally provide inputs

Example scenario (real team)

You are in a team of 5 developers.

CI runs on every push (tests, lint)

Deployment should happen only when the tech lead approves

So:

CI → automatic

Deploy → workflow_dispatch

The tech lead clicks Run workflow → Deploy to Prod

Example YAML
on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Where to deploy"
        required: true
        default: "staging"

When to use workflow_dispatch

Manual deployments

Hotfix releases

Emergency rollback

One-time jobs

Controlled production actions

repository_dispatch

What it is
A workflow trigger called from outside GitHub using the GitHub API.

This is for external systems → GitHub communication.

Why it exists

GitHub workflows normally react to GitHub events.
But what if:

Jenkins

External backend

SaaS tool

ML pipeline
needs to trigger GitHub?

That’s what repository_dispatch is for.

How it works (simple)

External system sends an HTTP POST to GitHub API

GitHub receives the event

Workflow starts

Example scenario (real team)

Your company has:

ML training pipeline running on a separate server

Code lives on GitHub

Deployment should start only after training finishes

Flow:

ML Server → GitHub API → repository_dispatch → Deploy workflow

Example YAML
on:
  repository_dispatch:
    types: [model_trained]

Example API call (conceptual)
POST /repos/OWNER/REPO/dispatches
{
  "event_type": "model_trained"
}

When to use repository_dispatch

External CI/CD tools

ML pipelines

Cross-repository automation

SaaS integrations

Custom backend triggers

---

## Time-Based Events

### `schedule`

Runs workflows on a cron schedule.
Used for nightly builds, cleanup jobs, and reports.

---

## Release & Versioning Events

### `release`

Triggered on release actions such as `published`, `created`, or `edited`.
Commonly used for packaging and deployment.

### `registry_package`

Triggered when a GitHub package is published or updated.

---

## Issues & Discussions Events

### `issues`

Triggered on issue activities like `opened`, `edited`, or `closed`.

### `issue_comment`

Triggered when a comment is added to an issue or pull request.

### `discussion`

Triggered on GitHub Discussions activity.

### `discussion_comment`

Triggered when a comment is added to a discussion.

### `projects`

Triggered on GitHub Projects activity.

---

## CI, Checks & Review Events

### `check_run`

Triggered when a check run is created or completed.

### `check_suite`

Triggered when a check suite is requested or completed.

### `status`

Triggered when a commit status changes.

### `pull_request_review`

Triggered when a PR review is submitted (approve or request changes).

### `pull_request_review_comment`

Triggered when a comment is added to a PR review.

---

## Security & Dependency Events

### `security_advisory`

Triggered when a security advisory is created or updated.

### `dependabot_alert`

Triggered when Dependabot detects a vulnerability.

---

## Deployment & Environment Events

### `deployment`

Triggered when a deployment is created.

### `deployment_status`

Triggered when the deployment status changes (success or failure).

### `environment`

Triggered when environment protection rules are applied.

---

## Workflow Chaining Events

### `workflow_run`

Triggered when another workflow completes.
Commonly used for CI → CD pipelines.

---

## Wiki, Pages & Documentation Events

### `gollum`

Triggered when wiki pages are created or updated.

### `page_build`

Triggered when a GitHub Pages site is built.

---

## GitHub App & Organization Events

### `installation`

Triggered when a GitHub App is installed or removed.

### `installation_repositories`

Triggered when repositories are added or removed from a GitHub App.

### `member`

Triggered when users are added or removed from an organization.

### `team`

Triggered on team creation, deletion, or configuration changes.

### `meta`

Triggered on GitHub system-wide events (rare).

---

## Summary

This cheat sheet covers **all major GitHub Actions events** used in real-world CI/CD pipelines. Combine these events with filters like `branches`, `paths`, and `types` to build precise and efficient workflows.

---

Happy Automating 🚀
