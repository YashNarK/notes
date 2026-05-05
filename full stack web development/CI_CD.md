# CI/CD (Continuous Integration / Continuous Deployment)

## Table of contents

- [CI/CD (Continuous Integration / Continuous Deployment)](#cicd-continuous-integration--continuous-deployment)
  - [Table of contents](#table-of-contents)
  - [What is CI/CD?](#what-is-cicd)
  - [Core Concepts](#core-concepts)
  - [GitHub Actions Fundamentals](#github-actions-fundamentals)
  - [Frontend CI Pipeline](#frontend-ci-pipeline)
  - [Frontend CD Pipeline](#frontend-cd-pipeline)
  - [Environment Management](#environment-management)
  - [Secrets and Security](#secrets-and-security)
  - [Caching in GitHub Actions](#caching-in-github-actions)
  - [Matrix Builds](#matrix-builds)
  - [Branch Protection and PR Workflows](#branch-protection-and-pr-workflows)
  - [Deployment Targets](#deployment-targets)
  - [Monitoring and Rollbacks](#monitoring-and-rollbacks)

---

## What is CI/CD?

| Concept | Definition |
|---|---|
| **CI** (Continuous Integration) | Automatically build, lint, and test code on every push/PR |
| **CD** (Continuous Delivery) | Automatically prepare a release that can be deployed with one click |
| **CD** (Continuous Deployment) | Automatically deploy every passing build to production |

**Why CI/CD?**
- Catch bugs early — fail fast
- Consistent, repeatable deployments
- Eliminate "works on my machine"
- Enable small, frequent releases (reduces risk)
- Free up developers from manual deployment steps

---

## Core Concepts

```
Developer pushes code
         ↓
  CI Pipeline runs
  ├── Lint (ESLint, Prettier)
  ├── Type check (tsc --noEmit)
  ├── Unit tests (Jest)
  ├── Integration tests
  └── Build (npm run build)
         ↓
  CD Pipeline runs
  ├── Deploy to staging
  ├── Run E2E tests (Playwright)
  └── Deploy to production (manual approval or auto)
```

**Key terms:**
- **Pipeline** — automated sequence of steps
- **Stage** — a logical grouping (build, test, deploy)
- **Job** — runs in its own VM/container
- **Step** — individual command or action
- **Artifact** — file(s) produced by a job (build output)
- **Environment** — a deployment target (staging, production)
- **Trigger** — event that starts the pipeline (push, PR, schedule)

---

## GitHub Actions Fundamentals

GitHub Actions uses YAML files in `.github/workflows/`.

```yaml
# .github/workflows/ci.yml
name: CI                          # workflow name

on:                               # triggers
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:                             # one or more jobs
  build-and-test:                 # job name (can be anything)
    runs-on: ubuntu-latest        # runner OS

    steps:
      - name: Checkout code
        uses: actions/checkout@v4            # action (pre-built step)

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'                       # cache node_modules

      - name: Install dependencies
        run: npm ci                           # shell command

      - name: Lint
        run: npm run lint

      - name: Type check
        run: npm run type-check

      - name: Run tests
        run: npm test -- --coverage

      - name: Upload coverage
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}

      - name: Build
        run: npm run build

      - name: Upload build artifact
        uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/
          retention-days: 7
```

**Key syntax:**
```yaml
# Expressions and contexts
${{ github.sha }}               # commit SHA
${{ github.ref_name }}          # branch name
${{ github.event_name }}        # 'push', 'pull_request', etc.
${{ secrets.MY_SECRET }}        # repository secret
${{ vars.MY_VAR }}              # repository variable (non-secret)
${{ env.MY_ENV }}               # environment variable set in the workflow

# Conditional steps
- name: Deploy only on main
  if: github.ref == 'refs/heads/main' && github.event_name == 'push'
  run: npm run deploy

# Output from one step used in another
- name: Get version
  id: version
  run: echo "tag=$(node -p 'require("./package.json").version')" >> $GITHUB_OUTPUT

- name: Use version
  run: echo "Deploying v${{ steps.version.outputs.tag }}"
```

---

## Frontend CI Pipeline

```yaml
# .github/workflows/ci.yml
name: Frontend CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  quality:
    name: Code Quality
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run format:check    # prettier --check
      - run: npx tsc --noEmit        # type check without emitting files

  test:
    name: Unit & Integration Tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm test -- --coverage --watchAll=false
      - uses: actions/upload-artifact@v4
        with:
          name: coverage-report
          path: coverage/

  build:
    name: Build
    runs-on: ubuntu-latest
    needs: [quality, test]          # only run if quality and test pass
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
        env:
          VITE_API_URL: ${{ vars.STAGING_API_URL }}
      - uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/

  e2e:
    name: E2E Tests (Playwright)
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium
      - uses: actions/download-artifact@v4
        with:
          name: dist
          path: dist/
      - run: npx playwright test
      - uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: playwright-report
          path: playwright-report/
```

---

## Frontend CD Pipeline

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      artifact-id: ${{ steps.upload.outputs.artifact-id }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
        env:
          VITE_API_URL: ${{ vars.PROD_API_URL }}
      - name: Upload artifact
        id: upload
        uses: actions/upload-artifact@v4
        with:
          name: dist-${{ github.sha }}
          path: dist/

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    environment: staging        # uses GitHub Environment protection rules
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: dist-${{ github.sha }}
          path: dist/
      - name: Deploy to Vercel (staging)
        run: npx vercel --prod --token=${{ secrets.VERCEL_TOKEN }}
        env:
          VERCEL_ORG_ID: ${{ secrets.VERCEL_ORG_ID }}
          VERCEL_PROJECT_ID: ${{ secrets.VERCEL_PROJECT_ID }}

  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://myapp.com
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: dist-${{ github.sha }}
          path: dist/
      - name: Deploy to production
        run: ./scripts/deploy.sh
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

---

## Environment Management

GitHub Environments provide deployment protection rules.

```
Repository → Settings → Environments

production:
  ✅ Required reviewers: [tech-lead]
  ✅ Wait timer: 10 minutes
  ✅ Deployment branches: main only
  Secrets:
    PROD_DATABASE_URL
    PROD_API_KEY
```

**Environment-specific variables:**
```yaml
# In your workflow
environment:
  name: production
  url: https://myapp.com

# Access environment-specific secrets
run: ./deploy.sh
env:
  DB_URL: ${{ secrets.DATABASE_URL }}    # scoped to 'production' environment
```

---

## Secrets and Security

```yaml
# Store secrets in: Repo Settings → Secrets and Variables → Actions

# Use in workflow
- run: aws s3 sync dist/ s3://${{ secrets.S3_BUCKET }}
  env:
    AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
    AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

# Never echo secrets
- run: echo "${{ secrets.MY_SECRET }}"   # ❌ GitHub will mask it, but still bad practice

# Security best practices
# - Use OIDC instead of long-lived access keys when possible
# - Pin action versions to a commit SHA
- uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683  # v4.2.2
# - Restrict permissions
permissions:
  contents: read
  id-token: write     # for OIDC

# OIDC — keyless auth with AWS/GCP/Azure
- name: Configure AWS credentials
  uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: arn:aws:iam::123456789:role/GitHubActions
    aws-region: us-east-1
```

---

## Caching in GitHub Actions

```yaml
# Cache node_modules between runs
- uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'          # built-in npm cache (uses package-lock.json as key)

# Custom cache
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-

# Cache Playwright browsers
- uses: actions/cache@v4
  id: playwright-cache
  with:
    path: ~/.cache/ms-playwright
    key: ${{ runner.os }}-playwright-${{ hashFiles('**/package-lock.json') }}

- name: Install Playwright browsers
  if: steps.playwright-cache.outputs.cache-hit != 'true'
  run: npx playwright install --with-deps
```

---

## Matrix Builds

Test across multiple Node.js versions or OSes in parallel.

```yaml
jobs:
  test:
    strategy:
      matrix:
        node-version: ['18', '20', '22']
        os: [ubuntu-latest, windows-latest]
      fail-fast: false    # don't cancel other matrix jobs on failure
    
    runs-on: ${{ matrix.os }}
    
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm ci
      - run: npm test
```

---

## Branch Protection and PR Workflows

```yaml
# Required status checks before merge:
# Repo → Settings → Branches → Branch protection rules → main

# Require status checks to pass:
# - CI / Code Quality
# - CI / Unit & Integration Tests
# - CI / Build

# PR-specific workflow
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  preview:
    name: Deploy Preview
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
      - name: Deploy to Vercel Preview
        id: preview
        run: |
          url=$(npx vercel --token=${{ secrets.VERCEL_TOKEN }})
          echo "url=$url" >> $GITHUB_OUTPUT
      - name: Comment PR with preview URL
        uses: actions/github-script@v7
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: '🚀 Preview deployed to: ${{ steps.preview.outputs.url }}'
            })
```

---

## Deployment Targets

| Platform | CLI / Action |
|---|---|
| **Vercel** | `vercel` CLI or `amondnet/vercel-action` |
| **Netlify** | `netlify` CLI or `nwtgck/actions-netlify` |
| **AWS S3 + CloudFront** | `aws s3 sync` + `aws cloudfront create-invalidation` |
| **GitHub Pages** | `actions/deploy-pages` |
| **Firebase Hosting** | `FirebaseExtended/action-hosting-deploy` |
| **Docker + ECS/K8s** | Build image → push to ECR → update task definition |

**AWS S3 deployment example:**
```yaml
- name: Deploy to S3
  run: aws s3 sync dist/ s3://${{ secrets.S3_BUCKET }} --delete
  env:
    AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
    AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    AWS_DEFAULT_REGION: us-east-1

- name: Invalidate CloudFront
  run: aws cloudfront create-invalidation --distribution-id ${{ secrets.CF_DIST_ID }} --paths "/*"
```

---

## Monitoring and Rollbacks

```yaml
# Post-deploy health check
- name: Health check
  run: |
    for i in {1..5}; do
      status=$(curl -s -o /dev/null -w "%{http_code}" https://myapp.com/health)
      if [ "$status" = "200" ]; then
        echo "Health check passed"
        exit 0
      fi
      echo "Attempt $i failed (status: $status), retrying..."
      sleep 10
    done
    echo "Health check failed after 5 attempts"
    exit 1

# Notify on failure
- name: Notify Slack on failure
  if: failure()
  uses: slackapi/slack-github-action@v1
  with:
    payload: |
      {"text": "❌ Deploy failed on ${{ github.ref }} by ${{ github.actor }}"}
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}
```

**Rollback strategy:**
- Tag every production deploy: `git tag v1.2.3 && git push --tags`
- Keep build artifacts for N days (upload-artifact `retention-days`)
- Blue-green deployments — switch traffic back to old version
- Feature flags — disable a feature without rollback
