# Hands-On Exercise: Handling Pushed Secrets & History Modification

**Duration:** 40 minutes  
**Level:** Intermediate/Advanced  
**Objective:** Learn to safely handle accidentally committed secrets and modify Git history using rebase, filter-branch, and other advanced techniques

## ⚠️ CRITICAL SECURITY WARNING

**This exercise covers sensitive security scenarios. In real situations:**

- **Act immediately** when secrets are exposed
- **Revoke/rotate all compromised credentials** before fixing Git history
- **Notify your security team** about the incident
- **Consider the secret compromised** even after removal from Git

## Overview

Accidentally committing secrets (API keys, passwords, tokens) is a common and serious mistake. This exercise teaches you how to:

- Remove secrets from Git history safely
- Use advanced Git commands for history modification
- Implement preventive measures
- Handle team coordination during secret incidents

## Prerequisites

- Strong understanding of Git fundamentals
- Completed previous intermediate exercises
- Understanding of Git branching and merging
- **⚠️ Practice repository only** - never practice with real secrets!

## Learning Goals

By the end of this exercise, you will:

- Master interactive rebase for history modification
- Use git filter-branch for bulk history changes
- Understand the implications of rewriting public history
- Implement strategies to prevent secret commits
- Handle team coordination during security incidents

## Exercise Steps

### Step 1: Setup Compromised Repository Scenario (8 minutes)

1. **Create a realistic project structure:**

   ```bash
   mkdir secret-incident-practice
   cd secret-incident-practice
   git init

   # Create realistic project files
   echo "# Web Application Project" > README.md
   echo "node_modules/" > .gitignore
   echo "*.log" >> .gitignore

   mkdir src config

   # Create application code
   cat > src/app.js << 'EOF'
   const express = require('express');
   const config = require('../config/config');

   const app = express();

   app.get('/', (req, res) => {
     res.json({ message: 'Hello World', version: '1.0.0' });
   });

   app.listen(config.port, () => {
     console.log(`Server running on port ${config.port}`);
   });

   module.exports = app;
   EOF

   # Create package.json
   cat > package.json << 'EOF'
   {
     "name": "web-app-demo",
     "version": "1.0.0",
     "description": "Demo app for Git security practices",
     "main": "src/app.js",
     "scripts": {
       "start": "node src/app.js",
       "test": "echo \"No tests yet\""
     },
     "dependencies": {
       "express": "^4.18.0"
     }
   }
   EOF

   git add .
   git commit -m "Initial project setup"
   ```

2. **Create development history with secrets accidentally included:**

   ```bash
   # Legitimate development work
   echo "PORT=3000" > config/config.env
   echo "NODE_ENV=development" >> config/config.env
   echo "LOG_LEVEL=debug" >> config/config.env

   cat > config/config.js << 'EOF'
   require('dotenv').config({ path: './config/config.env' });

   module.exports = {
     port: process.env.PORT || 3000,
     environment: process.env.NODE_ENV || 'development',
     logLevel: process.env.LOG_LEVEL || 'info'
   };
   EOF

   git add config/
   git commit -m "Add basic configuration system"

   # Add feature with no secrets (legitimate commit)
   cat >> src/app.js << 'EOF'

   app.get('/health', (req, res) => {
     res.json({
       status: 'healthy',
       timestamp: new Date().toISOString()
     });
   });
   EOF

   git add src/app.js
   git commit -m "Add health check endpoint"

   # MISTAKE: Accidentally commit secrets
   cat > config/secrets.js << 'EOF'
   // NEVER COMMIT THIS FILE!
   module.exports = {
     // Database credentials
     DB_HOST: 'prod-db.company.com',
     DB_USER: 'admin_user',
     DB_PASSWORD: 'SuperSecret123!@#',

     // API Keys
     STRIPE_SECRET_KEY: 'sk_live_51234567890abcdef...',
     AWS_ACCESS_KEY: 'AKIAIOSFODNN7EXAMPLE',
     AWS_SECRET_KEY: 'wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY',

     // JWT Secret
     JWT_SECRET: 'ultra-secret-jwt-key-never-share-this',

     // Third-party integrations
     SENDGRID_API_KEY: 'SG.1234567890abcdef.ghijklmnop',
     GITHUB_TOKEN: 'ghp_1234567890abcdefghijklmnopqrstuvwxyz'
   };
   EOF

   # Update main config to use secrets (making it harder to just delete the file)
   cat >> config/config.js << 'EOF'

   // Load secrets in production
   if (process.env.NODE_ENV === 'production') {
     const secrets = require('./secrets');
     module.exports = { ...module.exports, ...secrets };
   }
   EOF

   git add config/
   git commit -m "Add database configuration and API integrations"

   # Add more legitimate work after the secret commit
   echo "console.log('Debug: Application starting...');" >> src/app.js
   git add src/app.js
   git commit -m "Add debug logging"

   echo "# Installation" >> README.md
   echo "npm install" >> README.md
   echo "npm start" >> README.md
   git add README.md
   git commit -m "Update README with installation instructions"
   ```

3. **Simulate remote push (the point of no return):**

   ```bash
   # Create a "remote" repository to simulate the push
   cd ..
   git clone --bare secret-incident-practice origin-repo.git
   cd secret-incident-practice
   git remote add origin ../origin-repo.git
   git push -u origin main

   # View the compromised history
   git log --oneline
   echo "SECRETS HAVE BEEN PUSHED TO REMOTE!"
   ```

### Step 2: Immediate Response Protocol (5 minutes)

1. **Security first - what to do IMMEDIATELY:**

   ```bash
   echo "=== IMMEDIATE SECURITY RESPONSE CHECKLIST ==="
   echo "1. STOP all development and deployments"
   echo "2. REVOKE/ROTATE all exposed credentials immediately"
   echo "3. NOTIFY security team and stakeholders"
   echo "4. DOCUMENT the incident for review"
   echo "5. Only THEN start fixing Git history"
   echo ""
   echo "In this exercise, we'll focus on step 5..."

   # Create incident log
   cat > SECURITY_INCIDENT.md << 'EOF'
   # Security Incident Report

   **Date:** $(date)
   **Type:** Accidentally committed secrets to Git repository
   **Status:** ACTIVE - Remediation in progress

   ## Exposed Credentials
   - Database credentials (prod-db.company.com)
   - Stripe API keys
   - AWS access keys
   - JWT secret
   - SendGrid API key
   - GitHub personal access token

   ## Actions Taken
   - [ ] Revoked all exposed API keys
   - [ ] Rotated database passwords
   - [ ] Generated new JWT secret
   - [ ] Notified security team
   - [ ] Removed secrets from Git history

   ## Commits Affected
   - commit: "Add database configuration and API integrations"
   - Files: config/secrets.js, config/config.js
   EOF
   ```

### Step 3: Interactive Rebase for Recent History (12 minutes)

1. **Use interactive rebase to modify recent commits:**

   ```bash
   # View the commits we need to fix
   git log --oneline -n 5

   # Start interactive rebase from before the secret commit
   # Find the commit hash before "Add database configuration and API integrations"
   git rebase -i HEAD~4
   ```

   **In the interactive editor that opens:**

   ```
   pick abc123 Add basic configuration system
   edit def456 Add database configuration and API integrations  ← Change this to 'edit'
   pick ghi789 Add debug logging
   pick jkl012 Update README with installation instructions
   ```

2. **Remove secrets during the rebase:**

   ```bash
   # Git will stop at the commit we marked for editing
   git status

   # Remove the secrets file completely
   rm config/secrets.js

   # Update config.js to remove secret references
   cat > config/config.js << 'EOF'
   require('dotenv').config({ path: './config/config.env' });

   module.exports = {
     port: process.env.PORT || 3000,
     environment: process.env.NODE_ENV || 'development',
     logLevel: process.env.LOG_LEVEL || 'info',

     // Database config from environment variables
     dbHost: process.env.DB_HOST,
     dbUser: process.env.DB_USER,
     dbPassword: process.env.DB_PASSWORD,

     // API keys from environment variables
     stripeKey: process.env.STRIPE_SECRET_KEY,
     awsAccessKey: process.env.AWS_ACCESS_KEY,
     awsSecretKey: process.env.AWS_SECRET_KEY,
     jwtSecret: process.env.JWT_SECRET
   };
   EOF

   # Stage the changes
   git add .

   # Amend the commit to remove secrets
   git commit --amend -m "Add database configuration (using environment variables)"

   # Continue the rebase
   git rebase --continue
   ```

3. **Handle conflicts and complete rebase:**

   ```bash
   # If there are conflicts during rebase, resolve them
   # git status will show any conflicts
   # Edit files to resolve conflicts, then:
   # git add <resolved-files>
   # git rebase --continue

   # Verify the history is clean
   git log --oneline
   grep -r "SuperSecret\|sk_live\|AKIA" . || echo "No secrets found in working directory"
   ```

### Step 4: Git Filter-Branch for Deep History Cleaning (10 minutes)

Let's simulate a more complex scenario where secrets are deeper in history:

1. **Create a scenario with secrets in deep history:**

   ```bash
   # First, let's create some commits with secrets buried deeper
   git checkout -b deep-history-scenario

   # Add several legitimate commits
   echo "# Testing" > tests/app.test.js
   git add tests/
   git commit -m "Add basic test structure"

   # Bury a secret in an older commit
   git checkout HEAD~6  # Go back to an earlier point
   git checkout -b temp-fix

   echo "LEGACY_API_KEY=secret-key-12345" >> config/config.env
   git add config/config.env
   git commit -m "Add legacy API configuration"

   # Merge this back creating a complex history
   git checkout deep-history-scenario
   git merge temp-fix --no-ff -m "Merge legacy API support"

   git log --oneline --graph
   ```

2. **Use filter-branch to remove secrets from all history:**

   ```bash
   # BACKUP first!
   git branch backup-before-filter-branch

   # Use filter-branch to remove secret patterns from ALL commits
   git filter-branch --tree-filter '
     # Remove any files containing secrets
     find . -name "*.js" -o -name "*.env" -o -name "*.json" | xargs sed -i "s/SuperSecret[^[:space:]]*/[REDACTED]/g"
     find . -name "*.js" -o -name "*.env" -o -name "*.json" | xargs sed -i "s/sk_live_[^[:space:]]*/[REDACTED]/g"
     find . -name "*.js" -o -name "*.env" -o -name "*.json" | xargs sed -i "s/AKIA[^[:space:]]*/[REDACTED]/g"
     find . -name "*.js" -o -name "*.env" -o -name "*.json" | xargs sed -i "s/secret-key-[^[:space:]]*/[REDACTED]/g"
   ' --all

   # Clean up the filter-branch backup refs
   git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d

   # Verify secrets are gone
   git log --oneline
   grep -r "SuperSecret\|sk_live\|AKIA\|secret-key" . || echo "All secrets removed from history"
   ```

3. **Alternative: BFG Repo-Cleaner simulation:**

   ```bash
   # BFG is a faster alternative to filter-branch
   # This is how you would use it (simulation only):

   echo "# BFG Repo-Cleaner Alternative"
   echo "# Download from: https://rtyley.github.io/bfg-repo-cleaner/"
   echo ""
   echo "# Create patterns file"
   cat > secrets-patterns.txt << 'EOF'
   SuperSecret*
   sk_live_*
   AKIA*
   ghp_*
   SG.*
   EOF

   echo "# Commands you would run with BFG:"
   echo "java -jar bfg.jar --replace-text secrets-patterns.txt my-repo.git"
   echo "cd my-repo.git"
   echo "git reflog expire --expire=now --all && git gc --prune=now --aggressive"
   ```

### Step 5: Force Push and Team Coordination (5 minutes)

1. **Coordinate with team before force pushing:**

   ```bash
   echo "=== TEAM COORDINATION PROTOCOL ==="
   echo "1. Notify all team members to STOP pushing"
   echo "2. Ensure everyone has pushed their work"
   echo "3. Coordinate the force push timing"
   echo "4. Have team members reset their local repos"
   echo ""

   # Check what will be force pushed
   git log --oneline origin/main..main

   # Force push with lease (safer than --force)
   git push --force-with-lease origin main

   # For team members after force push:
   echo "=== Instructions for team members ==="
   echo "git fetch origin"
   echo "git reset --hard origin/main"
   echo "# Or safer: backup work, clone fresh, re-apply changes"
   ```

2. **Verify the remote is clean:**

   ```bash
   # Clone fresh to verify
   cd ..
   git clone origin-repo.git verification-clone
   cd verification-clone

   # Check for any remaining secrets
   grep -r "SuperSecret\|sk_live\|AKIA" . || echo "Remote repository is clean"

   cd ../secret-incident-practice
   ```

## Advanced Scenarios & Techniques

### Scenario 1: Secrets in Binary Files

```bash
# If secrets are in binary files (images, compiled code, etc.)
# Use filter-branch with different approach
git filter-branch --index-filter 'git rm --cached --ignore-unmatch path/to/binary/file' HEAD
```

### Scenario 2: Secrets in Merge Commits

```bash
# Interactive rebase can't easily handle merge commits
# Use filter-branch or reset to before the merge and redo it
git log --oneline --merges  # Find problematic merge
git reset --hard <commit-before-merge>
# Manually redo the merge without the secrets
```

### Scenario 3: Multiple Branches with Secrets

```bash
# Clean all branches, not just main
git filter-branch --tree-filter 'find . -name "*.secret" -delete' --all

# Or handle each branch individually
for branch in $(git branch -r | grep -v HEAD); do
    git checkout ${branch#origin/}
    # Apply cleaning steps
    git push --force-with-lease origin ${branch#origin/}
done
```

## Prevention Strategies

### 1. Pre-commit Hooks

```bash
# Create pre-commit hook to detect secrets
mkdir -p .git/hooks
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/bash

# Check for common secret patterns
if git diff --cached --name-only | xargs grep -l "sk_live_\|AKIA\|password.*=\|secret.*=" 2>/dev/null; then
    echo "Potential secrets detected in staged files!"
    echo "Please review and remove secrets before committing."
    exit 1
fi

echo "No obvious secrets detected."
EOF

chmod +x .git/hooks/pre-commit
```

### 2. Git Attributes for Secret Files

```bash
# Add to .gitattributes to prevent accidental commits
echo "*.secret filter=secret-filter" >> .gitattributes
echo "config/secrets.* filter=secret-filter" >> .gitattributes

# Configure the filter to block secrets
git config filter.secret-filter.clean 'echo "# This file contains secrets and should not be committed"'
git config filter.secret-filter.smudge cat
```

### 3. Environment-based Configuration

```bash
# Create proper environment variable usage
cat > config/config.js << 'EOF'
// GOOD: Load all secrets from environment variables
module.exports = {
  port: process.env.PORT || 3000,

  // Database
  database: {
    host: process.env.DB_HOST || 'localhost',
    user: process.env.DB_USER || 'user',
    password: process.env.DB_PASSWORD || '',
  },

  // API Keys (fail fast if missing in production)
  apiKeys: {
    stripe: process.env.STRIPE_SECRET_KEY ||
      (process.env.NODE_ENV === 'production' ?
        (() => { throw new Error('STRIPE_SECRET_KEY required in production') })() :
        'test_key'),
    aws: {
      accessKey: process.env.AWS_ACCESS_KEY || '',
      secretKey: process.env.AWS_SECRET_KEY || '',
    }
  }
};
EOF
```

## Emergency Response Checklist

### Immediate Actions (First 30 minutes)

- [ ] **STOP** all deployments and builds
- [ ] **REVOKE** all exposed credentials immediately
- [ ] **ROTATE** all affected API keys and passwords
- [ ] **NOTIFY** security team and management
- [ ] **DOCUMENT** the incident with timestamps

### Technical Remediation

- [ ] **BACKUP** current repository state
- [ ] **COORDINATE** with team to stop pushing
- [ ] **CLEAN** Git history using appropriate method
- [ ] **VERIFY** secrets are completely removed
- [ ] **FORCE PUSH** cleaned history
- [ ] **COORDINATE** team repository reset

### Post-Incident

- [ ] **UPDATE** all applications with new credentials
- [ ] **IMPLEMENT** prevention measures (hooks, tools)
- [ ] **REVIEW** access logs for potential compromise
- [ ] **CONDUCT** incident retrospective
- [ ] **UPDATE** security procedures

## Common Pitfalls & Solutions

### Pitfall 1: Incomplete Secret Removal

**Problem:** Secrets remain in some commits or branches.

**Solution:**

```bash
# Search entire repository for any remaining secrets
git log --all --full-history --grep="secret\|password\|key"
git log --all --full-history -S "sk_live_" --source --all
```

### Pitfall 2: Force Push Conflicts

**Problem:** Team members have conflicting local changes.

**Solution:**

```bash
# Team coordination protocol
# 1. Everyone pushes current work to temporary branches
# 2. Clean main branch history
# 3. Everyone rebases their work onto clean main
```

### Pitfall 3: Incomplete Credential Rotation

**Problem:** Not all instances of compromised credentials were found.

**Solution:**

- Audit all systems using the credentials
- Check configuration management systems
- Review deployment scripts and documentation
- Check backup systems and logs

## Verification & Reflection

### Verify Your Work

1. **Check repository is completely clean:**

   ```bash
   # Search for any remaining secret patterns
   git log --all --oneline | head -20
   find . -name "*.js" -o -name "*.env" -o -name "*.json" | xargs grep -l "SuperSecret\|sk_live\|AKIA" || echo "No secrets found"

   # Verify remote is clean
   git fetch origin
   git log origin/main --oneline | head -10
   ```

2. **Test prevention measures:**

   ```bash
   # Test pre-commit hook
   echo "SECRET_KEY=test-secret-123" > test-secret.env
   git add test-secret.env
   git commit -m "Test commit with secret"  # Should be blocked
   rm test-secret.env
   ```

### Reflection Questions

1. **What should be the first priority when secrets are discovered in Git history?**

2. **Why is force-pushing potentially dangerous, and how do you mitigate the risks?**

3. **What are the trade-offs between interactive rebase and filter-branch?**

4. **How would you handle this situation differently in a large team vs. small team?**

5. **What prevention measures would you implement after this incident?**

## Key Takeaways

- **Security first** - Always revoke/rotate credentials before fixing Git
- **Team coordination** is critical for force push operations
- **Prevention is better** than remediation - use proper secret management
- **Multiple tools available** - rebase for recent commits, filter-branch for deep history
- **Verification is essential** - ensure secrets are completely removed
- **Document everything** - incident response requires good documentation
- **Learn from incidents** - implement prevention measures afterwards

## Real-World Tools & Resources

### Secret Detection Tools

- **git-secrets** (AWS) - Pre-commit hook for AWS credentials
- **detect-secrets** (Yelp) - Baseline secret detection
- **TruffleHog** - Searches Git history for secrets
- **GitLeaks** - SAST tool for detecting hardcoded secrets

### Repository Cleaning Tools

- **BFG Repo-Cleaner** - Faster alternative to filter-branch
- **git filter-repo** - Modern replacement for filter-branch

### Secret Management Solutions

- **HashiCorp Vault** - Enterprise secret management
- **AWS Secrets Manager** - Cloud-native secret storage
- **Azure Key Vault** - Microsoft's secret management
- **Environment variables** - Simple local development

## Next Steps

This exercise prepares you for advanced Git workflows and security practices. Consider implementing automated secret detection in your CI/CD pipelines and establishing clear incident response procedures for your team.
