# GitHub Actions – Artifacts vs Cache (Complete Guide)

A **professional, GitHub-ready README** that explains **Artifacts and Cache from absolute basics**, with **clear mental models, tables, examples, and best practices**.

This document is designed so that:

* Beginners finally *get* the difference
* You can revise quickly before interviews
* You can directly use the examples in real CI/CD pipelines

---

## Table of Contents

1. What Problem Artifacts and Cache Solve
2. Basics: What Are Artifacts?
3. Why Artifacts Are Needed
4. How Artifacts Work Internally
5. Artifact Example (Step-by-Step)
6. Where to Find Artifacts in GitHub UI
7. Using Artifacts Between Jobs
8. What Is Cache?
9. Why Cache Is Needed
10. How Cache Works Internally
11. Cache Example (Step-by-Step)
12. Cache Keys Explained
13. Cache vs Artifacts (Core Difference)
14. Cache vs Artifacts (Comparison Table)
15. What to Cache and What NOT to Cache
16. What to Artifact and What NOT to Artifact
17. Real CI/CD Pipeline Example (Cache + Artifact)
18. ML-Specific Example
19. Common Mistakes
20. Interview-Ready One-Liners
21. Final Golden Rule

---

## 1. What Problem Artifacts and Cache Solve

Every **GitHub Actions job runs on a fresh virtual machine**.

This means:

* No files from previous jobs exist
* No dependencies are installed
* Everything starts from zero

So GitHub gives us **two different tools**:

* **Cache** → to avoid doing slow work again
* **Artifacts** → to store or share files produced by a job

---

## 2. Basics: What Are Artifacts?

**Artifacts are files or folders that a workflow saves after a job finishes.**

They allow you to:

* Store build outputs
* Share files between jobs
* Download results later from GitHub UI

Artifacts are **outputs**, not optimizations.

---

## 3. Why Artifacts Are Needed

Each job runs in isolation.

So this will NOT work by default:

* Job 1 builds files (for example `dist/`)
* Job 2 tries to deploy those files

Because Job 2 **cannot see Job 1’s filesystem**.

Artifacts solve this exact problem.

---

## 4. How Artifacts Work Internally

1. Job finishes execution
2. Files are compressed
3. Files are uploaded to GitHub storage
4. Artifact is attached to the workflow run
5. Other jobs or humans can download it

Artifacts are **always uploaded explicitly**.

---

## 5. Artifact Example (Step-by-Step)

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: build-files
    path: dist/
```

### Explanation

* `actions/upload-artifact@v4` → official GitHub action
* `name: build-files` → artifact name shown in UI
* `path: dist/` → folder to upload

Everything inside `dist/` is saved.

---

## 6. Where to Find Artifacts in GitHub UI

Path in GitHub:

```
Repository → Actions → Workflow Run → Artifacts
```

Artifacts can be downloaded as `.zip` files.

---

## 7. Using Artifacts Between Jobs

### Build Job

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: build-files
          path: dist/
```

### Deploy Job

```yaml
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build-files
      - run: ls dist
```

Now Job 2 has access to Job 1’s output.

---

## 8. What Is Cache?

**Cache is used to reuse dependencies or intermediate files across workflow runs.**

Cache exists to **speed up CI**, not to store results.

---

## 9. Why Cache Is Needed

Installing dependencies every run is slow:

* `pip install`
* `npm install`
* Maven / Gradle downloads

Cache prevents downloading the same things again and again.

---

## 10. How Cache Works Internally

1. Workflow checks if a cache with the given key exists
2. If yes → cache is restored
3. Job runs
4. Cache is updated at the end (if needed)

Cache is **best-effort**.

---

## 11. Cache Example (Step-by-Step)

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key: pip-${{ hashFiles('requirements.txt') }}
```

### Explanation

* `path` → folder to cache
* `key` → unique identifier for cache

---

## 12. Cache Keys Explained

```yaml
key: pip-${{ hashFiles('requirements.txt') }}
```

Meaning:

* If `requirements.txt` changes → new cache
* If it doesn’t → reuse old cache

Cache keys control **correctness**.

---

## 13. Cache vs Artifacts (Core Difference)

* **Cache** → reuse work
* **Artifact** → reuse results

---

## 14. Cache vs Artifacts (Comparison Table)

| Feature             | Cache              | Artifact              |
| ------------------- | ------------------ | --------------------- |
| Purpose             | Speed up workflows | Store / share outputs |
| Exists across runs  | Yes                | No                    |
| Shared between jobs | Not reliable       | Yes                   |
| Needs a key         | Yes                | No                    |
| Human download      | No                 | Yes                   |
| Best-effort         | Yes                | No                    |

---

## 15. What to Cache and What NOT to Cache

### Cache These

* `node_modules`
* Python package caches
* HuggingFace model downloads
* Maven / Gradle dependencies

### Do NOT Cache

* Build outputs
* `dist/`
* Trained models
* Deployment files

---

## 16. What to Artifact and What NOT to Artifact

### Artifact These

* Build outputs (`dist/`)
* Trained ML models
* Logs
* Test reports

### Do NOT Artifact

* Dependencies
* Package caches
* `node_modules`

---

## 17. Real CI/CD Pipeline Example (Cache + Artifact)

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: pip-${{ hashFiles('requirements.txt') }}
      - run: pip install -r requirements.txt
      - run: python train.py
      - uses: actions/upload-artifact@v4
        with:
          name: model
          path: model.pkl

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: model
      - run: python deploy.py
```

---

## 18. ML-Specific Example

* Cache:

  * Tokenizers
  * Pretrained model downloads
  * Python dependencies

* Artifact:

  * Trained models
  * Evaluation metrics
  * Plots

---

## 19. Common Mistakes

* Caching build outputs
* Using artifacts to store dependencies
* Assuming cache will always exist
* Skipping artifact upload between jobs

---

## 20. Interview-Ready One-Liners

* Cache is for **speed**
* Artifact is for **results**
* Cache is optional
* Artifact is required

---

## 21. Final Golden Rule

> If the workflow can survive without it → **CACHE**
>
> If the workflow breaks without it → **ARTIFACT**

---

### End of Document

This README is ready to be:

* Downloaded
* Added to a GitHub repository
* Used for revision or teaching
