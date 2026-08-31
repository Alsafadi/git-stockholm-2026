# Hands-On Exercise: Intermediate Git Commands

**Duration:** 45 minutes  
**Level:** Intermediate  
**Objective:** Master essential intermediate Git commands for daily workflow including checkout, merge, pull, push, and stash

## Overview

This exercise focuses on intermediate Git commands that are essential for daily development workflows. You'll practice various scenarios that developers commonly encounter when working with both local and remote repositories.

## Prerequisites

- Completed previous exercises (branching, merging, conflicts)
- Understanding of basic Git workflow
- Access to a remote repository (GitHub, GitLab, etc.)

## Learning Goals

By the end of this exercise, you will:

- Master different uses of `git checkout` and its modern alternatives
- Understand various merging strategies and when to use them
- Practice safe pulling and pushing with different scenarios
- Use `git stash` effectively for temporary work management
- Handle common workflow interruptions gracefully

## Exercise Steps

### Step 1: Advanced Checkout Operations (8 minutes)

1. **Setup a practice repository:**

   ```bash
   mkdir intermediate-git-practice
   cd intermediate-git-practice
   git init

   # Create initial structure
   echo "# Project Documentation" > README.md
   mkdir src tests
   echo "console.log('Hello World');" > src/app.js
   echo "// Test file placeholder" > tests/app.test.js

   git add .
   git commit -m "Initial project structure"
   ```

2. **Practice file-level checkout operations:**

   ```bash
   # Modify a file
   echo "console.log('Modified application');" > src/app.js
   echo "Modified documentation" >> README.md

   # Check what's changed
   git status
   git diff

   # Restore specific file to last commit
   git checkout HEAD -- src/app.js

   # Verify the restoration
   cat src/app.js
   git status
   ```

3. **Checkout files from specific commits:**

   ```bash
   # Create some history first
   echo "Version 2 of app" > src/app.js
   git add src/app.js
   git commit -m "Update app to version 2"

   echo "Version 3 of app" > src/app.js
   git add src/app.js
   git commit -m "Update app to version 3"

   # Get file from 2 commits ago
   git checkout HEAD~2 -- src/app.js
   git status
   cat src/app.js
   ```

4. **Practice modern alternatives:**

   ```bash
   # Modern syntax (Git 2.23+)
   git restore src/app.js              # Restore from HEAD
   git restore --source=HEAD~1 src/app.js  # Restore from specific commit
   ```

### Step 2: Advanced Merging Strategies (10 minutes)

1. **Create branches for merge practice:**

   ```bash
   git checkout main
   git checkout -b feature/user-auth

   # Add authentication feature
   echo "class UserAuth {" > src/auth.js
   echo "  login(username, password) {" >> src/auth.js
   echo "    // TODO: implement login" >> src/auth.js
   echo "  }" >> src/auth.js
   echo "}" >> src/auth.js

   git add src/auth.js
   git commit -m "feat: add user authentication skeleton"

   # Add more to the feature
   echo "  logout() {" >> src/auth.js
   echo "    // TODO: implement logout" >> src/auth.js
   echo "  }" >> src/auth.js

   git add src/auth.js
   git commit -m "feat: add logout functionality"
   ```

2. **Practice different merge strategies:**

   ```bash
   git checkout main

   # Strategy 1: Default merge (creates merge commit if needed)
   git merge feature/user-auth

   # View the result
   git log --oneline --graph
   ```

3. **Practice squash merge:**

   ```bash
   # Create another feature branch
   git checkout -b feature/user-profile

   echo "class UserProfile {" > src/profile.js
   echo "  getProfile(userId) {" >> src/profile.js
   echo "    // Get user profile" >> src/profile.js
   echo "  }" >> src/profile.js
   echo "}" >> src/profile.js
   git add src/profile.js
   git commit -m "Add user profile class"

   echo "  updateProfile(userId, data) {" >> src/profile.js
   echo "    // Update user profile" >> src/profile.js
   echo "  }" >> src/profile.js
   git add src/profile.js
   git commit -m "Add profile update method"

   echo "  deleteProfile(userId) {" >> src/profile.js
   echo "    // Delete user profile" >> src/profile.js
   echo "  }" >> src/profile.js
   git add src/profile.js
   git commit -m "Add profile deletion method"

   # Squash merge (combines all commits into one)
   git checkout main
   git merge --squash feature/user-profile
   git commit -m "feat: add complete user profile management"

   # Compare with previous merge
   git log --oneline --graph
   ```

### Step 3: Stash Operations (12 minutes)

1. **Basic stash workflow:**

   ```bash
   # Start working on something
   echo "// Work in progress feature" > src/wip-feature.js
   echo "console.log('Updated app with new feature');" >> src/app.js

   # Check current state
   git status

   # Emergency: need to switch branches but work isn't ready to commit
   git stash push -m "WIP: new feature development"

   # Verify clean working directory
   git status
   ls src/
   ```

2. **Stash management:**

   ```bash
   # Create more stashes
   echo "// Another work in progress" > src/another-wip.js
   git stash push -m "WIP: another feature"

   echo "// Quick fix needed" > src/quickfix.js
   git stash push -m "WIP: quick fix"

   # List all stashes
   git stash list

   # Show stash contents
   git stash show stash@{0}
   git stash show -p stash@{1}  # Show patch/diff
   ```

3. **Apply and manage stashes:**

   ```bash
   # Apply most recent stash (keeps it in stash list)
   git stash apply

   # Check what was applied
   git status

   # Apply specific stash
   git stash apply stash@{1}

   # Pop stash (apply and remove from stash list)
   git stash pop stash@{2}

   # Drop a stash without applying
   git stash drop stash@{0}

   # View remaining stashes
   git stash list
   ```

4. **Advanced stash operations:**

   ```bash
   # Stash only tracked files (ignore new files)
   echo "// New untracked file" > src/untracked.js
   echo "// Modify existing file" >> src/app.js

   git stash push --keep-index -m "Stash only tracked changes"

   # Stash including untracked files
   git stash push -u -m "Stash everything including untracked"

   # Create branch from stash
   git stash branch feature/stashed-work stash@{0}
   ```

### Step 4: Remote Operations and Pull Strategies (10 minutes)

1. **Setup remote repository simulation:**

   ```bash
   # Create a bare "remote" repository simulation
   # (bare = no working directory, just like a real GitHub/GitLab repo)
   cd ..
   git clone --bare intermediate-git-practice origin-repo.git
   cd intermediate-git-practice
   git remote add origin ../origin-repo.git

   # Create a second clone to simulate a teammate working against the same remote
   cd ..
   git clone origin-repo.git teammate-repo
   cd intermediate-git-practice
   ```

   **Note:** We use a bare repo for `origin` because a normal (non-bare) repo
   refuses pushes to whichever branch it has checked out. The `teammate-repo`
   clone stands in for "someone else's machine" pushing changes to the shared remote.

2. **Practice different pull strategies:**

   ```bash
   # Push current work
   git push -u origin main

   # Simulate a teammate's remote changes
   cd ../teammate-repo
   git pull origin main
   echo "// Remote change" >> README.md
   git add README.md
   git commit -m "Remote update to documentation"
   git push origin main

   cd ../intermediate-git-practice
   ```

3. **Handle pull conflicts:**

   ```bash
   # Make local changes that will conflict
   echo "// Local change" >> README.md
   git add README.md
   git commit -m "Local update to documentation"

   # Try to pull (will fail due to diverged history)
   git pull origin main

   # Resolve the conflict manually or use merge strategy
   # Edit README.md to resolve conflict, then:
   git add README.md
   git commit -m "Merge remote changes"
   ```

4. **Practice pull with rebase:**

   ```bash
   # Create another conflicting scenario
   echo "// Another local change" >> src/app.js
   git add src/app.js
   git commit -m "Local app modification"

   # Simulate a teammate's remote change
   cd ../teammate-repo
   git pull origin main
   echo "// Remote app change" >> src/app.js
   git add src/app.js
   git commit -m "Remote app modification"
   git push origin main

   cd ../intermediate-git-practice

   # Pull with rebase instead of merge
   git pull --rebase origin main
   # Resolve conflicts if any, then continue rebase
   # git rebase --continue
   ```

### Step 5: Advanced Push Operations (5 minutes)

1. **Practice safe pushing:**

   ```bash
   # Check what will be pushed
   git log origin/main..main --oneline

   # Push with lease (safer than force push)
   git push --force-with-lease origin main
   ```

2. **Push specific branches and tags:**

   ```bash
   # Create and push a new branch
   git checkout -b feature/new-feature
   echo "// New feature" > src/new-feature.js
   git add src/new-feature.js
   git commit -m "Add new feature"

   # Push new branch
   git push -u origin feature/new-feature

   # Create and push a tag
   git tag -a v1.0.0 -m "Version 1.0.0 release"
   git push origin v1.0.0
   ```

## Challenge Tasks (Additional Practice)

### Challenge 1: Interactive Rebase with Stash

1. Create multiple commits on a feature branch
2. Stash some work in progress
3. Use interactive rebase to clean up commit history
4. Apply stash and continue working

### Challenge 2: Cherry-pick with Conflicts

1. Create commits on different branches
2. Cherry-pick specific commits to another branch
3. Resolve any conflicts that arise
4. Use `git cherry-pick --continue` to finish

### Challenge 3: Bisect with Stash

1. Introduce a bug in your code history
2. Stash current work
3. Use `git bisect` to find the problematic commit
4. Apply stash and fix the issue

## Common Scenarios & Solutions

### Scenario 1: Interrupted Work

**Problem:** Working on feature but need to fix urgent bug.

**Solution:**

```bash
git stash push -m "WIP: feature in progress"
git checkout main
git checkout -b hotfix/urgent-bug
# Fix bug, commit, merge
git checkout feature-branch
git stash pop
```

### Scenario 2: Accidental Commit to Wrong Branch

**Problem:** Made commits on main instead of feature branch.

**Solution:**

```bash
git branch feature/my-feature  # Create branch from current position
git reset --hard HEAD~3        # Reset main back 3 commits
git checkout feature/my-feature
```

### Scenario 3: Need to Update Feature Branch with Latest Main

**Problem:** Feature branch is outdated and needs latest changes.

**Solution:**

```bash
git checkout feature-branch
git fetch origin
git merge origin/main  # Or git rebase origin/main
```

## Verification & Reflection

### Verify Your Work

1. **Check your repository state:**

   ```bash
   git status
   git log --oneline --graph --all -n 15
   git stash list
   git branch -a
   ```

2. **Verify remote connections:**

   ```bash
   git remote -v
   git branch -vv
   ```

### Reflection Questions

1. **When would you use `git stash` vs creating a temporary commit?**

2. **What's the difference between `git pull` and `git pull --rebase`?**

3. **When would you choose squash merge over regular merge?**

4. **How do you decide between `git checkout` and `git restore`?**

## Best Practices Summary

### Stashing

- **Use descriptive messages** with `git stash push -m "message"`
- **Don't let stashes accumulate** - apply or drop them regularly
- **Use stash branches** for complex stashed work

### Merging

- **Choose merge strategy** based on project needs
- **Use squash merge** for feature branches with messy history
- **Keep main branch** clean and stable

### Remote Operations

- **Always fetch before push** to avoid conflicts
- **Use `--force-with-lease`** instead of `--force`
- **Communicate** with team about force pushes

### Checkout/Restore

- **Use `git restore`** for file operations (modern syntax)
- **Use `git switch`** for branch operations (modern syntax)
- **Be careful** with checkout - it can lose uncommitted work

## Key Takeaways

- **Stash is your friend** for managing interruptions in workflow
- **Different merge strategies** serve different purposes
- **Modern Git commands** (`switch`, `restore`) are clearer than `checkout`
- **Remote operations** require careful consideration in team environments
- **Practice makes perfect** - these commands become second nature with use

## Next Steps

This exercise prepares you for the recovery and undo scenarios where you'll learn about reset, revert, and other powerful Git commands for fixing mistakes.
