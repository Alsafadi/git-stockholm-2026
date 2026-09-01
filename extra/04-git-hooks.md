# Git Hooks: Automating with Pre-commit/Pre-push

_Automating quality checks and workflows_

---

### What are Git Hooks?

#### Definition:

**Scripts that run automatically at specific Git events**

#### When they trigger:

- **Before/after** commits
- **Before/after** pushes
- **Before/after** merges
- **On** checkouts
- **During** rebases

#### Purpose:

- **Enforce** coding standards
- **Run** tests automatically
- **Validate** commit messages
- **Deploy** applications
- **Send** notifications

---

### Types of Git Hooks

#### Client-side hooks:

- **pre-commit** - Before commit is made
- **prepare-commit-msg** - Before commit message editor
- **commit-msg** - Validate commit message
- **post-commit** - After commit is made
- **pre-push** - Before push to remote
- **post-checkout** - After checkout
- **post-merge** - After merge

#### Server-side hooks:

- **pre-receive** - Before push is accepted
- **update** - Before each branch update
- **post-receive** - After push is completed

---

### Hook Location and Setup

#### Where hooks live:

```
.git/hooks/
├── pre-commit.sample
├── pre-push.sample
├── commit-msg.sample
└── post-commit.sample
```

#### Enable a hook:

```bash
# Remove .sample extension
mv .git/hooks/pre-commit.sample .git/hooks/pre-commit

# Make executable
chmod +x .git/hooks/pre-commit
```

---

### Pre-commit Hook

#### Purpose:

**Run checks before commit is created**

#### Common uses:

- **Lint** code (ESLint, Pylint)
- **Format** code (Prettier, Black)
- **Run** unit tests
- **Check** for secrets
- **Validate** file sizes

#### If hook fails:

**Commit is aborted**

---

### Pre-commit Hook Example

#### Basic shell script:

```bash
#!/bin/sh
# .git/hooks/pre-commit

echo "Running pre-commit checks..."

# Run linter
npm run lint
if [ $? -ne 0 ]; then
    echo "Linting failed! Fix errors before committing."
    exit 1
fi

# Run tests
npm run dev
if [ $? -ne 0 ]; then
    echo "Tests failed! Fix tests before committing."
    exit 1
fi

echo "All checks passed!"
exit 0
```

---

### JavaScript Pre-commit Example

#### Node.js script:

```javascript
#!/usr/bin/env node
// .git/hooks/pre-commit

const { execSync } = require("child_process");

console.log("Running pre-commit checks...");

try {
  // Check for console.log statements
  execSync('git diff --cached --name-only | xargs grep -l "console.log"', {
    stdio: "pipe",
  });
  console.error("Found console.log statements");
  process.exit(1);
} catch (error) {
  // No console.log found (good)
}

try {
  // Run prettier
  execSync("npx prettier --check .", { stdio: "inherit" });
  console.log("Code formatting looks good");
} catch (error) {
  console.error("Code formatting issues found");
  process.exit(1);
}

console.log("All pre-commit checks passed!");
```

---

### Pre-push Hook

#### Purpose:

**Run checks before pushing to remote**

#### Common uses:

- **Integration** tests
- **Security** scans
- **Build** validation
- **Branch** naming checks
- **Commit message** validation

---

### Pre-push Hook Example

#### Shell script:

```bash
#!/bin/sh
# .git/hooks/pre-push

protected_branch='main'
current_branch=$(git symbolic-ref HEAD | sed -e 's,.*/\(.*\),\1,')

# Prevent direct push to main
if [ $protected_branch = $current_branch ]; then
    echo "Direct push to main branch is not allowed"
    echo "Please use pull requests for main branch"
    exit 1
fi

# Run full test suite before push
echo "Running full test suite..."
npm run test:full
if [ $? -ne 0 ]; then
    echo "Tests failed! Cannot push."
    exit 1
fi

echo "Pre-push checks passed!"
exit 0
```

---

### Commit Message Hook

#### Purpose:

**Validate commit message format**

#### Example validation:

```bash
#!/bin/sh
# .git/hooks/commit-msg

commit_regex='^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .{1,50}'

if ! grep -qE "$commit_regex" "$1"; then
    echo "Invalid commit message format!"
    echo "Format: type(scope): description"
    echo "Example: feat(auth): add login functionality"
    echo "Types: feat, fix, docs, style, refactor, test, chore"
    exit 1
fi

echo "Commit message format is valid"
```

---

### Using pre-commit Framework

#### Installation:

```bash
# Install pre-commit
pip install pre-commit

# Or with homebrew
brew install pre-commit
```

#### Configuration file (.pre-commit-config.yaml):

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files

  - repo: https://github.com/psf/black
    rev: 22.10.0
    hooks:
      - id: black

  - repo: https://github.com/pycqa/flake8
    rev: 5.0.4
    hooks:
      - id: flake8
```

---

### Installing pre-commit Hooks

#### Setup:

```bash
# Install hooks from config
pre-commit install

# Install pre-push hooks
pre-commit install --hook-type pre-push

# Run on all files
pre-commit run --all-files

# Update hooks
pre-commit autoupdate
```

---

### JavaScript/Node.js Hooks

#### Package.json scripts:

```json
{
  "scripts": {
    "precommit": "lint-staged",
    "prepush": "npm run test && npm run build"
  },
  "lint-staged": {
    "*.js": ["eslint --fix", "prettier --write"],
    "*.{json,md}": ["prettier --write"]
  }
}
```

#### Using husky:

```bash
# Install husky
npm install --save-dev husky

# Enable Git hooks
npx husky install

# Add pre-commit hook
npx husky add .husky/pre-commit "npm run precommit"
```

---

### Bypassing Hooks

#### Skip hooks when needed:

```bash
# Skip pre-commit hooks
git commit --no-verify -m "Emergency fix"

# Skip pre-push hooks
git push --no-verify origin main
```

#### When to bypass:

- **Emergency** fixes
- **Hotfixes** to production
- **Temporary** work-in-progress
- **Hook** is broken

**Use sparingly!**

---

### Advanced Hook Examples

#### Automatic dependency updates:

```bash
#!/bin/sh
# .git/hooks/post-merge

# Check if package.json changed
if git diff HEAD@{1} HEAD --name-only | grep -q "package.json"; then
    echo "package.json changed, updating dependencies..."
    npm install
fi
```

#### Automated deployment:

```bash
#!/bin/sh
# .git/hooks/post-receive (server-side)

if [ "$2" = "refs/heads/main" ]; then
    echo "Deploying to production..."
    cd /var/www/app
    git pull origin main
    npm install --production
    npm run build
    systemctl restart app
    echo "Deployment complete!"
fi
```

---

### Security Considerations

#### Secrets detection:

```bash
#!/bin/sh
# Check for potential secrets

git diff --cached --name-only | xargs grep -l "api_key\|password\|secret" && {
    echo "Potential secrets detected!"
    echo "Please review your changes"
    exit 1
}
```

#### File size limits:

```bash
#!/bin/sh
# Check file sizes

git diff --cached --name-only | while read file; do
    if [ -f "$file" ]; then
        size=$(stat -c%s "$file" 2>/dev/null || stat -f%z "$file")
        if [ $size -gt 1048576 ]; then  # 1MB limit
            echo "File $file is too large ($(($size/1024))KB)"
            exit 1
        fi
    fi
done
```

---

### Team Hook Management

#### Sharing hooks:

```bash
# Create hooks directory in repo
mkdir .githooks

# Configure Git to use it
git config core.hooksPath .githooks

# Make executable
chmod +x .githooks/*

# Team members run:
git config core.hooksPath .githooks
```

#### Template repository:

```bash
# Set up template with hooks
git config --global init.templateDir ~/.git-template

# Hooks in ~/.git-template/hooks/ will be copied
# to new repositories
```

---

### CI/CD Integration

#### GitHub Actions hook:

```yaml
# .github/workflows/pre-commit.yml
name: Pre-commit checks
on: [push, pull_request]

jobs:
  pre-commit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
      - uses: pre-commit/action@v3.0.0
```

#### GitLab CI hook:

```yaml
# .gitlab-ci.yml
pre-commit:
  stage: test
  script:
    - pre-commit run --all-files
```

---

### Debugging Hooks

#### Test hooks manually:

```bash
# Run pre-commit hook
.git/hooks/pre-commit

# Check exit code
echo $?

# Debug with set -x
#!/bin/sh
set -x  # Enable debug output
# ... rest of hook
```

#### Hook logs:

```bash
# Add logging to hooks
exec 1> >(logger -s -t pre-commit)
exec 2>&1
```

---

### Best Practices

#### Hook design:

✅ **Fast execution** - hooks should be quick  
✅ **Clear feedback** - good error messages  
✅ **Exit codes** - 0 for success, non-zero for failure  
✅ **Documentation** - explain what hooks do

#### Team adoption:

✅ **Gradual introduction** - start simple  
✅ **Team agreement** - everyone on board  
✅ **Easy bypass** - for emergencies  
✅ **Consistent environment** - same tools/versions

---

### Questions About Git Hooks?

**Hooks are powerful automation tools for quality and workflow**

**Coming up:** Pull Requests and Code Review workflows

**Any questions about implementing Git hooks?**
