# GitHub Actions Runners – Complete Explanation

A **runner** is the machine where your GitHub Actions **job actually runs**.

In simple words:

* A runner executes the steps defined in your workflow
* It checks out your code
* It runs scripts, tests, builds, deployments, etc.

Every GitHub Actions job **must specify a runner** using the `runs-on` keyword.

---

## What is a Runner in GitHub Actions?

A **runner** is a system (virtual machine or physical machine) that:

* Listens for jobs from GitHub Actions
* Downloads your repository
* Runs the steps defined in your workflow
* Sends logs and status back to GitHub

Without a runner, a GitHub Actions workflow **cannot execute**.

---

## Types of Runners in GitHub Actions

There are **two main types of runners**:

1. **GitHub-hosted runners**
2. **Self-hosted runners**

---

## 1. GitHub-Hosted Runners

GitHub-hosted runners are **virtual machines provided and fully managed by GitHub**.

You do not need to create servers, install software, or manage infrastructure.

---

### Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

### What this means

* GitHub automatically creates a **fresh Linux virtual machine**
* Your job runs on that VM
* After the job finishes, the VM is **destroyed**

Each workflow run gets a **clean, isolated environment**.

---

### Available GitHub-Hosted Runners

#### Linux Runners

```yaml
runs-on: ubuntu-latest
runs-on: ubuntu-22.04
runs-on: ubuntu-20.04
```

#### Windows Runners

```yaml
runs-on: windows-latest
runs-on: windows-2022
```

#### macOS Runners

```yaml
runs-on: macos-latest
runs-on: macos-14
```

---

### Characteristics of GitHub-Hosted Runners

* **Ephemeral** (a new machine is created for every job)
* Comes with **pre-installed tools**, such as:

  * Git
  * Docker
  * Python
  * Node.js
  * Java
* Has **execution time limits** (typically up to 6 hours)
* Has **limited CPU and RAM**
* Uses **GitHub Actions minutes** (free for public repos, limited for private repos)

---

### When to Use GitHub-Hosted Runners

GitHub-hosted runners are ideal when:

* You want **zero infrastructure management**
* Your CI/CD pipeline uses **standard tools**
* You want **fast setup and easy scaling**
* You do not need special hardware or private network access

---

## 2. Self-Hosted Runners

A **self-hosted runner** is a machine **you own and manage**, connected to GitHub Actions.

This machine can be:

* Your local laptop
* An on-premise server
* A cloud VM (EC2, GCP, Azure VM)
* A Kubernetes node
* A GPU-enabled machine

---

### Example

```yaml
jobs:
  train-model:
    runs-on: self-hosted
```

Or using labels:

```yaml
jobs:
  gpu-job:
    runs-on: [self-hosted, linux, gpu]
```

---

### What this means

* GitHub sends the job to **your machine**
* The job runs on **your infrastructure**
* The machine is **not destroyed** after job completion

---

### How Self-Hosted Runners Work

1. You create or choose a machine
2. You install the GitHub Actions runner software
3. You register the runner with:

   * A repository, or
   * An organization, or
   * An enterprise
4. The runner continuously polls GitHub for jobs
5. When a job matches its labels, the runner executes it

---

### Characteristics of Self-Hosted Runners

* **Persistent machines** (state remains unless you clean it)
* Full control over:

  * Operating system
  * Installed software
  * Hardware configuration
* No GitHub Actions minute limits
* Can access:

  * Private networks
  * Internal databases
  * Internal services
* You are responsible for:

  * Security
  * OS updates
  * Scaling
  * Maintenance

---

### Common Labels for Self-Hosted Runners

```yaml
runs-on: [self-hosted, linux]
runs-on: [self-hosted, windows]
runs-on: [self-hosted, gpu]
runs-on: [self-hosted, ec2]
```

Labels help GitHub decide **which runner is eligible** to pick up a job.

---

## GitHub-Hosted vs Self-Hosted Runners (Comparison)

| Feature                   | GitHub-Hosted Runner | Self-Hosted Runner           |
| ------------------------- | -------------------- | ---------------------------- |
| Managed by                | GitHub               | You                          |
| Setup effort              | None                 | High                         |
| Cost                      | GitHub minutes       | Infrastructure cost          |
| Custom hardware           | No                   | Yes                          |
| GPU support               | No                   | Yes                          |
| Private network access    | No                   | Yes                          |
| Clean environment per run | Yes                  | No (manual cleanup required) |

---

## Real-World Usage Examples

### Example 1: Standard CI Pipeline

```yaml
runs-on: ubuntu-latest
```

Used for:

* Code linting
* Unit testing
* Docker image builds
* Python or Node.js applications

---

### Example 2: Machine Learning Training with GPU

```yaml
runs-on: [self-hosted, linux, gpu]
```

Used for:

* Deep learning model training
* CUDA-based workloads
* Heavy computational tasks

---

### Example 3: Deployment Inside a Private Network

```yaml
runs-on: [self-hosted, production]
```

Used for:

* Deploying applications to internal servers
* Running infrastructure scripts
* Accessing private databases

---

## Key Takeaways

* **Runner = the machine where your job runs**
* The `runs-on` key decides **which machine executes the job**
* GitHub-hosted runners are **easy, disposable, and managed**
* Self-hosted runners are **powerful, customizable, and flexible**

---

This document provides a **complete and line-by-line explanation** of GitHub Actions runners without skipping any concept.
