# Hands-On Exercise: Tagging and Release Management

**Duration:** 25 minutes  
**Level:** Intermediate  
**Objective:** Master Git tagging for version control, release management, and marking important milestones in project history

## Overview

This exercise focuses on Git's tagging system, which is essential for release management and marking important points in your project's history. You'll learn to create, manage, and work with both lightweight and annotated tags.

## Prerequisites

- Understanding of Git commits and branching
- Basic knowledge of semantic versioning
- Familiarity with Git remote operations

## Learning Goals

By the end of this exercise, you will:

- Understand the difference between lightweight and annotated tags
- Create and manage tags for release workflows
- Practice semantic versioning with Git tags
- Learn to share tags with remote repositories
- Handle tag-related scenarios in team environments

## Exercise Steps

### Step 1: Setup Release Practice Repository (5 minutes)

1. **Create a project structure for release management:**

   ```bash
   mkdir release-management-practice
   cd release-management-practice
   git init

   # Create initial project structure
   echo "# Release Management Demo" > README.md
   echo "version = \"0.1.0\"" > version.txt

   mkdir src
   echo "class Calculator {" > src/calculator.js
   echo "  add(a, b) { return a + b; }" >> src/calculator.js
   echo "  subtract(a, b) { return a - b; }" >> src/calculator.js
   echo "}" >> src/calculator.js

   cat > package.json << 'EOF'
   {
     "name": "calculator-demo",
     "version": "0.1.0",
     "description": "Demo project for Git tagging",
     "main": "src/calculator.js"
   }
   EOF

   git add .
   git commit -m "Initial project setup"
   ```

### Step 2: Creating Lightweight Tags (5 minutes)

1. **Create lightweight tags for development milestones:**

   ```bash
   # Create a lightweight tag for initial setup
   git tag initial-setup

   # Add some development work
   echo "  multiply(a, b) { return a * b; }" >> src/calculator.js
   git add src/calculator.js
   git commit -m "Add multiply function"

   # Tag this development milestone
   git tag dev-milestone-1

   # Add more functionality
   echo "  divide(a, b) {" >> src/calculator.js
   echo "    if (b === 0) throw new Error('Division by zero');" >> src/calculator.js
   echo "    return a / b;" >> src/calculator.js
   echo "  }" >> src/calculator.js
   git add src/calculator.js
   git commit -m "Add divide function with error handling"

   git tag dev-milestone-2
   ```

2. **View and work with lightweight tags:**

   ```bash
   # List all tags
   git tag

   # List tags with pattern
   git tag -l "dev-*"

   # Show tag information (lightweight tags show commit info)
   git show initial-setup
   git show dev-milestone-1

   # Check out code at a specific tag
   git checkout dev-milestone-1
   cat src/calculator.js  # Should only have add, subtract, multiply

   git checkout main  # Return to main branch
   ```

### Step 3: Creating Annotated Tags for Releases (8 minutes)

1. **Prepare for first release:**

   ```bash
   # Update version information
   echo "version = \"1.0.0\"" > version.txt
   sed -i 's/"version": "0.1.0"/"version": "1.0.0"/' package.json

   # Add release notes
   # (unquoted EOF so $(date ...) below actually expands to today's date)
   cat > CHANGELOG.md << EOF
   # Changelog

   ## [1.0.0] - $(date +%Y-%m-%d)

   ### Added
   - Basic calculator functionality
   - Add, subtract, multiply, divide operations
   - Error handling for division by zero

   ### Features
   - Clean, modular code structure
   - Comprehensive test coverage ready
   EOF

   git add .
   git commit -m "Prepare for v1.0.0 release"
   ```

2. **Create annotated release tag:**

   ```bash
   # Create annotated tag with message
   git tag -a v1.0.0 -m "Release version 1.0.0

   Features:
   - Complete calculator functionality
   - Error handling
   - Clean API design

   This is the first stable release of the calculator."

   # View the annotated tag (shows tag object info)
   git show v1.0.0
   ```

3. **Continue development and create more releases:**

   ```bash
   # Add new feature for v1.1.0
   echo "  power(a, b) { return Math.pow(a, b); }" >> src/calculator.js
   echo "  sqrt(a) { return Math.sqrt(a); }" >> src/calculator.js

   git add src/calculator.js
   git commit -m "Add power and square root functions"

   # Update version
   echo "version = \"1.1.0\"" > version.txt
   sed -i 's/"version": "1.0.0"/"version": "1.1.0"/' package.json

   # Update changelog
   cat >> CHANGELOG.md << EOF

   ## [1.1.0] - $(date +%Y-%m-%d)

   ### Added
   - Power function (a^b)
   - Square root function

   ### Enhanced
   - Extended mathematical operations
   EOF

   git add .
   git commit -m "Prepare for v1.1.0 release"

   # Create release tag
   git tag -a v1.1.0 -m "Release version 1.1.0 - Extended Math Functions"
   ```

### Step 4: Tag Management Operations (4 minutes)

1. **List and inspect tags:**

   ```bash
   # List all tags
   git tag

   # List tags sorted by version
   git tag -l --sort=-version:refname

   # Show tags with commit messages
   git tag -n

   # Show detailed tag information
   git show v1.0.0
   git show v1.1.0
   ```

2. **Create tags for specific commits:**

   ```bash
   # Create tag for a previous commit
   git log --oneline

   # Tag the commit where multiply was added
   git tag -a v0.2.0 HEAD~3 -m "Release 0.2.0 - Added multiply function"

   # Create lightweight tag for bug fix point
   git tag bugfix-checkpoint HEAD~1

   # View chronological tag history
   git log --oneline --decorate --graph
   ```

3. **Delete and rename tags:**

   ```bash
   # Delete a tag
   git tag -d bugfix-checkpoint

   # Rename a tag (delete old, create new)
   git tag -a v0.2.1 v0.2.0^{} -m "Renamed to v0.2.1 for clarity"
   git tag -d v0.2.0

   git tag  # Verify changes
   ```

### Step 5: Working with Remote Tags (3 minutes)

1. **Simulate remote repository:**

   ```bash
   # Create a "remote" repository
   cd ..
   git clone --bare release-management-practice origin-repo.git
   cd release-management-practice
   git remote add origin ../origin-repo.git
   ```

2. **Push tags to remote:**

   ```bash
   # Push a specific tag
   git push origin v1.0.0

   # Push all tags
   git push origin --tags

   # Verify tags were pushed
   git ls-remote --tags origin
   ```

3. **Fetch tags from remote:**

   ```bash
   # Simulate fetching in a new clone
   cd ..
   git clone origin-repo.git team-member-repo
   cd team-member-repo

   # List available tags
   git tag

   # Fetch specific tag if not present
   git fetch origin --tags

   # Check out a specific release
   git checkout v1.0.0
   cat package.json  # Should show version 1.0.0

   cd ../release-management-practice
   ```

## Challenge Tasks (Additional Practice)

### Challenge 1: Hotfix Release Process

1. Simulate a critical bug in v1.1.0
2. Create a hotfix branch from v1.1.0 tag
3. Fix the bug and create v1.1.1 patch release
4. Tag the hotfix appropriately

### Challenge 2: Release Branch Workflow

1. Create a release branch for v2.0.0
2. Make final adjustments on the release branch
3. Tag the release from the release branch
4. Merge back to main and develop branches

### Challenge 3: Semantic Version Management

1. Create a script that automatically increments version numbers
2. Practice creating major, minor, and patch releases
3. Maintain consistent versioning across multiple files

## Common Tagging Scenarios

### Scenario 1: Retroactive Tagging

**Problem:** Forgot to tag important releases.

**Solution:**

```bash
git log --oneline  # Find the commit
git tag -a v1.0.0 abc123 -m "Retroactive tag for v1.0.0 release"
```

### Scenario 2: Moving Tags

**Problem:** Tagged wrong commit.

**Solution:**

```bash
git tag -d v1.0.0
git tag -a v1.0.0 correct-commit -m "Corrected tag position"
git push origin :refs/tags/v1.0.0  # Delete from remote
git push origin v1.0.0  # Push corrected tag
```

### Scenario 3: Release Rollback

**Problem:** Need to rollback to previous release.

**Solution:**

```bash
git checkout v1.0.0
git checkout -b rollback-branch
# Make necessary fixes
git tag -a v1.0.1 -m "Rollback release with fixes"
```

## Best Practices for Tagging

### Naming Conventions

- **Semantic versioning:** v1.2.3 (major.minor.patch)
- **Consistent prefixes:** v for versions, rc for release candidates
- **Descriptive names:** feature-complete, stable-baseline

### Tag Messages

- **Annotated tags for releases:** Include changelog summary
- **Lightweight tags for milestones:** Quick reference points
- **Consistent format:** Follow team conventions

### Team Workflow

- **Tag protection:** Protect release tags from accidental deletion
- **Automated tagging:** Use CI/CD for consistent release process
- **Communication:** Announce releases with tag information

## Verification & Reflection

### Verify Your Work

1. **Check your tag structure:**

   ```bash
   git tag -l --sort=-version:refname
   git log --oneline --decorate --graph
   ```

2. **Verify tag content:**

   ```bash
   git show v1.0.0
   git show v1.1.0
   ```

### Reflection Questions

1. **When would you use lightweight vs annotated tags?**
2. **How do tags help with release management?**
3. **What information should be included in release tag messages?**
4. **How do tags integrate with semantic versioning?**

## Integration with Release Workflow

### Automated Releases

```bash
# Example release script
#!/bin/bash
VERSION=$1
git checkout main
git pull origin main
npm version $VERSION  # Updates package.json
git add package.json
git commit -m "Release $VERSION"
git tag -a v$VERSION -m "Release version $VERSION"
git push origin main --tags
```

### Continuous Integration

- **Trigger builds** on tag creation
- **Deploy specific versions** using tags
- **Generate changelogs** from tag ranges

## Key Takeaways

- **Lightweight tags** are simple pointers - good for temporary markers
- **Annotated tags** are full objects - essential for releases
- **Semantic versioning** with tags provides clear release history
- **Tag management** is crucial for team collaboration
- **Remote tag operations** require explicit commands
- **Tags are immutable** - think carefully before creating them
- **Automation** helps maintain consistent tagging practices
