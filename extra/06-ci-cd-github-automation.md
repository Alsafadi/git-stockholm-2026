# CI/CD Automation: GitHub Actions Overview

_Automating testing, building, and deployment workflows_

---

## What is CI/CD?

### Continuous Integration (CI):

**Automatically build and test code changes**

- **Early detection** - Catch issues quickly
- **Automated testing** - Run full test suites
- **Build verification** - Ensure code compiles
- **Quality checks** - Linting, security scans

### Continuous Deployment (CD):

**Automatically deploy tested code**

- **Consistent deployments** - Same process every time
- **Faster releases** - Reduce manual overhead
- **Rollback capability** - Quick recovery from issues
- **Environment parity** - Same process for all environments

---

## Why CI/CD with Git?

### Git integration benefits:

- **Trigger on events** - Push, PR, tag creation
- **Branch-based workflows** - Different pipelines per branch
- **Commit tracking** - Link deployments to code changes
- **Rollback support** - Deploy previous commits
- **Parallel workflows** - Multiple pipelines simultaneously

### Business benefits:

- **Faster time to market** - Automated releases
- **Higher quality** - Consistent testing
- **Reduced risk** - Smaller, frequent changes
- **Developer productivity** - Less manual work
- **Better collaboration** - Shared deployment process

---

## GitHub Actions Overview

### What are GitHub Actions?

**Native CI/CD platform built into GitHub**

### Key components:

- **Workflows** - Automated processes
- **Jobs** - Groups of steps
- **Steps** - Individual tasks
- **Actions** - Reusable code units
- **Runners** - Compute environments

### Workflow triggers:

```yaml
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
  schedule:
    - cron: "0 2 * * *" # Daily at 2 AM
  workflow_dispatch: # Manual trigger
```

---

## Basic Workflow Structure

### Simple Node.js workflow:

```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "18"
          cache: "npm"

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Run linter
        run: npm run lint
```

---

## Multi-Environment Workflows

### Development, staging, production:

```yaml
name: Deploy Pipeline

on:
  push:
    branches:
      - main # Deploy to staging
      - production # Deploy to production
  pull_request:
    branches: [main] # Test for staging

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run tests
        run: npm test

  deploy-staging:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    environment: staging
    steps:
      - name: Deploy to staging
        run: echo "Deploying to staging..."

  deploy-production:
    needs: test
    if: github.ref == 'refs/heads/production'
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Deploy to production
        run: echo "Deploying to production..."
```

---

## Matrix Testing

### Test across multiple versions:

```yaml
name: Matrix Testing

on: [push, pull_request]

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [16, 18, 20]

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install and test
        run: |
          npm ci
          npm test
```

---

## Artifact Management

### Build and store artifacts:

```yaml
name: Build and Store

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Build application
        run: npm run build

      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build-files
          path: dist/
          retention-days: 30

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: build-files
          path: dist/

      - name: Deploy artifacts
        run: echo "Deploying..."
```

---

## Secrets and Environment Variables

### Managing sensitive data:

```yaml
name: Deploy with Secrets

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Deploy to AWS
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          DATABASE_URL: ${{ secrets.DATABASE_URL }}
          API_KEY: ${{ secrets.API_KEY }}
        run: |
          aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
          aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
          npm run deploy
```

### Environment-specific secrets:

```yaml
deploy-prod:
  environment: production
  steps:
    - name: Deploy
      env:
        API_URL: ${{ vars.API_URL }} # Environment variable
        SECRET_KEY: ${{ secrets.SECRET_KEY }} # Environment secret
      run: deploy.sh
```

---

## Docker Workflows

### Build and push Docker images:

```yaml
name: Docker Build

on:
  push:
    branches: [main]
    tags: ["v*"]

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Login to DockerHub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v4
        with:
          images: myapp
          tags: |
            type=ref,event=branch
            type=ref,event=pr
            type=semver,pattern={{version}}

      - name: Build and push
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

---

## Testing Workflows

### Comprehensive testing pipeline:

```yaml
name: Test Suite

on: [push, pull_request]

jobs:
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run unit tests
        run: npm run test:unit

  integration-tests:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:13
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
      - uses: actions/checkout@v3
      - name: Run integration tests
        run: npm run test:integration

  e2e-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run Playwright tests
        run: npx playwright test

  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run security audit
        run: npm audit --audit-level high
```

---

## Deployment Strategies

### Blue-Green Deployment:

```yaml
name: Blue-Green Deploy

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Blue environment
        run: |
          kubectl apply -f k8s/blue-deployment.yml
          kubectl wait --for=condition=ready pod -l version=blue

      - name: Run health checks
        run: |
          curl -f http://blue.example.com/health

      - name: Switch traffic to Blue
        run: |
          kubectl patch service myapp -p '{"spec":{"selector":{"version":"blue"}}}'

      - name: Cleanup Green environment
        run: |
          kubectl delete deployment myapp-green
```

### Rolling Deployment:

```yaml
name: Rolling Deploy

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Update deployment
        run: |
          kubectl set image deployment/myapp container=${{ github.sha }}
          kubectl rollout status deployment/myapp

      - name: Verify deployment
        run: |
          kubectl get pods -l app=myapp
          curl -f http://myapp.example.com/health
```

---

## Conditional Workflows

### Smart workflow execution:

```yaml
name: Smart CI/CD

on: [push, pull_request]

jobs:
  detect-changes:
    runs-on: ubuntu-latest
    outputs:
      backend: ${{ steps.changes.outputs.backend }}
      frontend: ${{ steps.changes.outputs.frontend }}
      docs: ${{ steps.changes.outputs.docs }}
    steps:
      - uses: actions/checkout@v3
      - uses: dorny/paths-filter@v2
        id: changes
        with:
          filters: |
            backend:
              - 'api/**'
              - 'server/**'
            frontend:
              - 'client/**'
              - 'web/**'
            docs:
              - 'docs/**'
              - '*.md'

  test-backend:
    needs: detect-changes
    if: needs.detect-changes.outputs.backend == 'true'
    runs-on: ubuntu-latest
    steps:
      - name: Test backend
        run: npm run test:api

  test-frontend:
    needs: detect-changes
    if: needs.detect-changes.outputs.frontend == 'true'
    runs-on: ubuntu-latest
    steps:
      - name: Test frontend
        run: npm run test:client
```

---

## Workflow Optimization

### Caching strategies:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      # Cache Node modules
      - uses: actions/cache@v3
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}

      # Cache Docker layers
      - uses: actions/cache@v3
        with:
          path: /tmp/.buildx-cache
          key: ${{ runner.os }}-buildx-${{ github.sha }}
          restore-keys: ${{ runner.os }}-buildx-

      - name: Build with cache
        run: npm ci --cache ~/.npm --prefer-offline
```

### Parallel execution:

```yaml
jobs:
  test:
    strategy:
      matrix:
        test-group: [unit, integration, e2e]
    runs-on: ubuntu-latest
    steps:
      - name: Run ${{ matrix.test-group }} tests
        run: npm run test:${{ matrix.test-group }}
```

---

## Monitoring and Notifications

### Slack notifications:

```yaml
name: Notify on Failure

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        run: ./deploy.sh

      - name: Notify success
        if: success()
        uses: 8398a7/action-slack@v3
        with:
          status: success
          text: "🚀 Deployment successful!"
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}

      - name: Notify failure
        if: failure()
        uses: 8398a7/action-slack@v3
        with:
          status: failure
          text: "❌ Deployment failed!"
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}
```

---

## Advanced Patterns

### Reusable workflows:

```yaml
# .github/workflows/reusable-deploy.yml
name: Reusable Deploy

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
    secrets:
      deploy-token:
        required: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: ${{ inputs.environment }}
    steps:
      - name: Deploy to ${{ inputs.environment }}
        run: echo "Deploying..."
```

### Using reusable workflows:

```yaml
# .github/workflows/main.yml
name: Main Pipeline

on: [push]

jobs:
  deploy-staging:
    uses: ./.github/workflows/reusable-deploy.yml
    with:
      environment: staging
    secrets:
      deploy-token: ${{ secrets.STAGING_TOKEN }}

  deploy-prod:
    needs: deploy-staging
    uses: ./.github/workflows/reusable-deploy.yml
    with:
      environment: production
    secrets:
      deploy-token: ${{ secrets.PROD_TOKEN }}
```

---

## Security Best Practices

### Secure workflows:

```yaml
name: Secure Pipeline

permissions:
  contents: read
  security-events: write

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Run CodeQL Analysis
        uses: github/codeql-action/analyze@v2

      - name: Run dependency check
        uses: dependency-check/Dependency-Check_Action@main

      - name: Run secret scan
        uses: trufflesecurity/trufflehog@main
        with:
          path: ./

      - name: Upload SARIF results
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: results.sarif
```

---

## Troubleshooting Workflows

### Debug techniques:

```yaml
jobs:
  debug:
    runs-on: ubuntu-latest
    steps:
      - name: Debug context
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: echo "$GITHUB_CONTEXT"

      - name: List environment
        run: env | sort

      - name: Debug with tmate (SSH access)
        if: failure()
        uses: mxschmitt/action-tmate@v3
        timeout-minutes: 30
```

### Common issues:

- **Permissions errors** - Check repository settings
- **Secret access** - Verify secret names and scopes
- **Timeout issues** - Increase timeout or optimize steps
- **Cache misses** - Check cache key generation
- **Resource limits** - Consider using larger runners

---

## Cost Optimization

### Efficient resource usage:

```yaml
jobs:
  optimize:
    runs-on: ubuntu-latest
    # Use smaller runners for simple tasks
    # runs-on: ubuntu-latest-2-cores for heavy tasks

    steps:
      - name: Skip redundant builds
        if: "contains(github.event.head_commit.message, '[skip ci]')"
        run: exit 0

      - name: Cache everything possible
        uses: actions/cache@v3
        # Cache node_modules, build outputs, etc.

      - name: Use matrix strategically
        # Only test critical combinations
```

---

## Questions?

**CI/CD automates the path from code to production**

**Coming up:** Course summary and final Q&A

**Any questions about setting up automated pipelines?**
