# CI/CD Code Quality Checks (Flake8, Black, Bandit)

A **professional, GitHub-ready guide** explaining **Linting**, **Formatting**, and **Security Scanning** in a **CI/CD pipeline** using **GitHub Actions**, written in **simple language with real examples**.

---

## Table of Contents

1. What is CI/CD Code Quality?
2. Linting with Flake8
3. Formatting with Black
4. Security Scanning with Bandit
5. Comparison Table
6. CI/CD Workflow Flow
7. GitHub Actions Example
8. Real-World Benefits

---

## 1. What is CI/CD Code Quality?

CI/CD pipelines do **automatic checks** every time code is pushed to GitHub.

These checks ensure that:

* Code is **written correctly**
* Code is **formatted consistently**
* Code is **secure and safe to deploy**

The three most common quality gates are:

| Purpose                  | Tool   |
| ------------------------ | ------ |
| Code correctness & style | Flake8 |
| Code formatting          | Black  |
| Security vulnerabilities | Bandit |

---

## 2. Linting – Flake8

### What is Linting?

Linting checks your code for **mistakes and bad practices** before it runs.

Think of Flake8 as a **strict code reviewer**.

### What Flake8 Checks

* Syntax errors
* Unused imports
* Unused variables
* Line length & spacing (PEP8)
* Complex or unreadable code

### ❌ Bad Code Example

```python
import os

def add(a,b):
    c = a + b
    return a+b
```

### Flake8 Issues Found

```text
F401 'os' imported but unused
E231 missing whitespace after ','
F841 local variable 'c' is assigned to but never used
```

### Key Point

* **Flake8 does NOT fix code**
* It only **reports problems**

### Role in CI/CD

* Runs automatically on every push or pull request
* If errors exist → **pipeline fails**

---

## 3. Formatting – Black

### What is Formatting?

Formatting makes code **look clean and consistent**.

Black removes arguments like:

* tabs vs spaces
* line length debates
* personal coding styles

### Why Black?

* Fully automatic
* No configuration needed
* Same format everywhere

### ❌ Unformatted Code

```python
def multiply(a,b):return a*b
```

### ✅ Black-Formatted Code

```python
def multiply(a, b):
    return a * b
```

### Key Difference from Flake8

| Tool   | Fixes Code? |
| ------ | ----------- |
| Flake8 | ❌ No        |
| Black  | ✅ Yes       |

### Role in CI/CD

Common CI command:

```bash
black --check .
```

* Checks formatting
* Fails pipeline if formatting is incorrect

---

## 4. Security Scan – Bandit

### What is Bandit?

Bandit scans Python code for **security vulnerabilities**.

Think of Bandit as a **security auditor** for your code.

### What Bandit Detects

* Hardcoded passwords
* Dangerous system commands
* Unsafe use of `eval`, `exec`, `os.system`
* Insecure cryptography

### ❌ Dangerous Code Example

```python
import os

os.system("rm -rf /")
```

### Bandit Warning

```text
B605: Starting a process with a shell, possible injection
```

### ❌ Hardcoded Credentials

```python
password = "admin123"
```

Bandit flags this as:

```text
B105: Hardcoded password string
```

### Role in CI/CD

* Runs automatically
* Blocks deployment if high-severity issues are found

---

## 5. Tool Comparison Summary

| Feature           | Flake8 | Black | Bandit |
| ----------------- | ------ | ----- | ------ |
| Code style issues | ✅      | ❌     | ❌      |
| Auto-fix code     | ❌      | ✅     | ❌      |
| Security issues   | ❌      | ❌     | ✅      |
| CI/CD gate        | ✅      | ✅     | ✅      |

---

## 6. CI/CD Workflow Flow

```text
Developer Pushes Code
        ↓
Linting (Flake8)
        ↓
Formatting Check (Black)
        ↓
Security Scan (Bandit)
        ↓
Tests
        ↓
Build & Deploy
```

❌ If any step fails → deployment stops

---

## 7. GitHub Actions Workflow Example

```yaml
name: Code Quality Checks

on: [push, pull_request]

jobs:
  quality:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Setup Python
      uses: actions/setup-python@v5
      with:
        python-version: "3.11"

    - name: Install tools
      run: |
        pip install flake8 black bandit

    - name: Lint with Flake8
      run: flake8 .

    - name: Check formatting with Black
      run: black --check .

    - name: Security scan with Bandit
      run: bandit -r .
```

---

## 8. Why This Matters in Real Projects

Without CI quality checks:

* Bugs reach production
* Code becomes unreadable
* Security vulnerabilities leak data

With Flake8 + Black + Bandit:

* Clean codebase
* Consistent formatting
* Secure deployments
* High engineering standards

---

## 9. One-Line Understanding

* **Flake8** → Finds coding mistakes
* **Black** → Fixes code formatting
* **Bandit** → Finds security risks

---

## 10. Recommended Usage Order in CI

1. Flake8 (code correctness)
2. Black (format consistency)
3. Bandit (security scan)
4. Pytest (testing)
5. Build & Deploy

---

### ✅ This README is ready to:

* Upload directly to GitHub
* Use in ML, Flask, FastAPI, or GenAI projects
* Extend with Pytest, Coverage, Docker, or DVC

---

**Happy Shipping Clean & Secure Code 🚀**
