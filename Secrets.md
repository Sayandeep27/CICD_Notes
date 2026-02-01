# Environment Variables & Secrets in GitHub Actions (CI/CD)

## 1. Why Environment Variables & Secrets Exist

In CI/CD pipelines, workflows often need sensitive information, such as:

API keys

Database connection URLs

Cloud credentials (AWS, GCP, Azure)

Tokens (GitHub, Docker Hub, Slack, etc.)

❌ What you must NEVER do

AWS_ACCESS_KEY_ID: AKIAIOSFODNN7EXAMPLE

Hardcoding secrets is dangerous because:

Anyone with repo access can see them

Secrets leak via logs or commits

Once leaked, they must be rotated immediately

✅ Correct solution

Use Environment Variables and GitHub Secrets

## 2. Difference Between Environment Variables and Secrets

Environment Variables

Store non-sensitive configuration

Example:

ENV=production

APP_PORT=5000

Secrets

Store sensitive data

Encrypted by GitHub

Automatically masked in logs

Only accessible during workflow execution

Feature	Environment Variable	Secret
Sensitive	No	Yes
Visible in logs	Yes	No (masked)
Encrypted	No	Yes
Stored in repo	Yes	No

## 3. Where GitHub Secrets Are Stored

Secrets are stored securely in GitHub:

Repository
 └── Settings
     └── Secrets and variables
         └── Actions
             └── New repository secret

You add:

Name: AWS_ACCESS_KEY_ID

Value: actual AWS key

GitHub encrypts this value and never exposes it directly.

## 4. Types of Secrets in GitHub Actions

1. Repository Secrets

Available to all workflows in the repo

Most commonly used

2. Environment Secrets

Scoped to an environment (dev, staging, prod)

Can require approvals

3. Organization Secrets

Shared across multiple repositories

## 5. Using Secrets in GitHub Actions

Secrets are accessed via:

${{ secrets.SECRET_NAME }}

## 6. Using Secrets as Environment Variables (Workflow Level)

name: Deploy App

on: push

jobs:
  deploy:
    runs-on: ubuntu-latest

    env:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

    steps:
      - name: Print AWS key
        run: echo $AWS_ACCESS_KEY_ID

What happens internally

GitHub injects the secret at runtime

It exists only during job execution

Value is masked in logs

Output will look like:

***

## 7. Using Secrets Inside a Single Step

steps:
  - name: Deploy to AWS
    env:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
    run: |
      echo "Deploying with AWS credentials"

Why this is useful

Limits secret exposure

More secure than job-level secrets

Best practice for production pipelines

## 8. Why echo $AWS_ACCESS_KEY_ID Is Safe

Even if you run:

run: echo $AWS_ACCESS_KEY_ID

GitHub will:

Detect secret values

Mask them automatically

Prevent accidental leaks

Log output:

***

This masking happens even if the secret appears inside a longer string.

## 9. Secrets vs .env Files

❌ Do NOT commit .env files

AWS_SECRET_ACCESS_KEY=abcd1234

✅ Correct approach

Use .env.example (without values)

Store real values in GitHub Secrets

## 10. Using Secrets with Docker

- name: Build Docker image
  run: |
    docker build       --build-arg AWS_ACCESS_KEY_ID=${{ secrets.AWS_ACCESS_KEY_ID }}       .

Secrets are injected at build time, not stored in the image.

## 11. Using Secrets with Python Applications

env:
  DATABASE_URL: ${{ secrets.DATABASE_URL }}

run: python app.py

In Python:

import os

db_url = os.getenv("DATABASE_URL")

## 12. Security Best Practices (Very Important)

Always follow these rules

Never print secrets deliberately

Never commit credentials

Rotate secrets regularly

Use step-level secrets where possible

Use environment secrets for production

Use least-privilege IAM roles

## 13. Common Mistakes to Avoid

❌ Hardcoding credentials

❌ Committing .env files

❌ Logging secrets intentionally

❌ Sharing secrets across unrelated jobs

❌ Using the same secret for dev & prod

## 14. Real-World Example (AWS Deployment)

env:
  AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
  AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
  AWS_REGION: ap-south-1

Used by:

AWS CLI

Terraform

CDK

Docker

Kubernetes

## 15. Key Takeaway

Secrets in GitHub Actions are:

Secure

Encrypted

Temporary

Automatically masked

Essential for real CI/CD pipelines
