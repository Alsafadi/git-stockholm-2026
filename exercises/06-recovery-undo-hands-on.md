# Hands-On Exercise: Recovery and Undo Scenarios

**Duration:** 45 minutes  
**Level:** Intermediate/Advanced  
**Objective:** Master Git's recovery and undo commands (reset, revert, clean) to handle common mistakes and cleanup scenarios

## Overview

This exercise focuses on Git's powerful undo and recovery capabilities. You'll learn to safely fix mistakes, clean up messy situations, and recover from various problematic scenarios that developers commonly encounter.

## Prerequisites

- Solid understanding of Git fundamentals
- Completed previous intermediate exercises
- Comfort with command line operations
- **⚠️ IMPORTANT:** These commands can be destructive - practice in a safe environment!

## Learning Goals

By the end of this exercise, you will:

- Understand the differences between reset, revert, and clean
- Safely undo commits with different strategies
- Recover from common Git mistakes
- Clean up untracked files and directories
- Practice safe recovery techniques

## Safety First!

**Before starting:** Always create backups when practicing destructive operations:

```bash
# Create a backup branch before dangerous operations
git branch backup-$(date +%Y%m%d-%H%M%S)
```

## Exercise Steps

### Step 1: Understanding Git Reset (15 minutes)

1. **Setup practice repository:**

   ```bash
   mkdir git-recovery-practice
   cd git-recovery-practice
   git init

   # Create initial content
   echo "# Project Recovery Practice" > README.md
   echo "function greet() { return 'Hello'; }" > app.js
   echo "body { margin: 0; }" > style.css

   git add .
   git commit -m "Initial commit"
   ```

2. **Create commit history for reset practice:**

   ```bash
   # Commit 1
   echo "function greet(name) { return 'Hello ' + name; }" > app.js
   git add app.js
   git commit -m "Add name parameter to greet function"

   # Commit 2
   echo "function greet(name) { return \`Hello \${name}!\`; }" > app.js
   echo "function farewell(name) { return \`Goodbye \${name}!\`; }" >> app.js
   git add app.js
   git commit -m "Use template literals and add farewell function"

   # Commit 3
   echo "# Project Recovery Practice" > README.md
   echo "This project demonstrates Git recovery techniques." >> README.md
   echo "## Functions" >> README.md
   echo "- greet(name): Says hello" >> README.md
   echo "- farewell(name): Says goodbye" >> README.md
   git add README.md
   git commit -m "Update README with function documentation"

   # View your history
   git log --oneline
   ```

3. **Practice different reset modes:**

   Each demo below commits a small throwaway change, then undoes it with a
   different reset mode. Re-committing between demos brings you back to the
   same starting point, so the three modes are easy to compare side by side
   (resetting `HEAD~1` twice in a row would undo two commits instead of
   demonstrating the same undo twice!).

   ```bash
   # --- SOFT RESET demo ---
   # moves HEAD but keeps staging area and working directory
   echo "// Work in progress" >> app.js
   git add app.js
   git commit -m "Temporary commit for soft reset demo"

   git reset --soft HEAD~1
   git status         # Notice: the change is still staged
   git log --oneline  # Notice: the temporary commit is gone

   git commit -m "Temporary commit for soft reset demo"  # back to baseline

   # --- MIXED RESET demo (default) ---
   # moves HEAD and unstages the change, but keeps it in the working directory
   git reset HEAD~1
   git status         # Notice: the change is now unstaged
   git log --oneline

   git add app.js
   git commit -m "Temporary commit for mixed reset demo"  # back to baseline

   # --- HARD RESET demo ---
   # ⚠️ DANGEROUS: moves HEAD and discards the change from staging AND the working directory
   git reset --hard HEAD~1
   git status  # Should be clean - the change is completely gone
   git log --oneline
   cat app.js  # The "Work in progress" line should be gone
   ```

### Step 2: Git Revert - Safe Public Undo (10 minutes)

1. **Setup scenario for revert:**

   ```bash
   # Rebuild some history
   echo "function greet(name) { return 'Hello ' + name; }" > app.js
   git add app.js
   git commit -m "Add name parameter to greet function"

   echo "function add(a, b) { return a + b; }" >> app.js
   git add app.js
   git commit -m "Add math function"

   echo "function subtract(a, b) { return a - b; }" >> app.js
   git add app.js
   git commit -m "Add subtract function"

   # Simulate pushing to remote (make commits "public")
   git log --oneline
   ```

2. **Practice revert operations:**

   ```bash
   # Revert the most recent commit
   git revert HEAD
   # Git will open editor for commit message - accept default or modify

   # Check the result
   git log --oneline
   cat app.js  # Subtract function should be gone

   # Revert a specific commit (middle one)
   git revert HEAD~2  # This will revert the "Add math function" commit

   # Check result
   cat app.js  # Add function should be gone, but subtract is back!

   # Revert multiple commits
   git revert HEAD~1..HEAD  # Revert a range of commits
   ```

3. **Practice revert without auto-commit:**

   ```bash
   # Add another function to revert
   echo "function multiply(a, b) { return a * b; }" >> app.js
   git add app.js
   git commit -m "Add multiply function"

   # Revert but don't auto-commit (useful for editing the revert)
   git revert --no-commit HEAD
   git status  # Changes are staged but not committed

   # You can modify the revert before committing
   echo "// Multiply function removed due to bug" >> app.js
   git add app.js
   git commit -m "Revert multiply function and add explanatory comment"
   ```

### Step 3: Git Clean - Removing Untracked Files (8 minutes)

1. **Create untracked files and directories:**

   ```bash
   # Create various untracked items
   echo "temporary work" > temp.txt
   echo "debug output" > debug.log
   echo "compiled code" > app.compiled.js

   mkdir temp-dir
   echo "temporary file in directory" > temp-dir/temp-file.txt

   mkdir -p build/dist
   echo "build artifact" > build/dist/bundle.js

   echo "node_modules/" > .gitignore
   mkdir node_modules
   echo "fake dependency" > node_modules/fake-lib.js

   git status
   ```

2. **Practice clean operations safely:**

   ```bash
   # DRY RUN - see what would be removed (ALWAYS do this first!)
   git clean -n

   # Clean only files (not directories)
   git clean -f

   git status  # Check what was removed
   ls -la      # Verify directories are still there

   # Clean directories too
   git clean -fd

   ls -la      # temp-dir and build should be gone

   # Clean ignored files too (be very careful!)
   echo "temp ignore file" > temp-ignored.tmp
   echo "*.tmp" >> .gitignore
   git add .gitignore
   git commit -m "Add gitignore rule"

   git clean -fdx  # This removes ignored files too!
   ls -la          # node_modules should be gone!
   ```

### Step 4: Recovery Scenarios (12 minutes)

1. **Scenario 1: Recover accidentally deleted branch:**

   ```bash
   # Create and delete a branch
   git checkout -b feature/important-work
   echo "important code" > important.js
   git add important.js
   git commit -m "Important work that must not be lost"

   git checkout main
   git branch -D feature/important-work  # Oops! Deleted wrong branch

   # Recovery using reflog
   git reflog  # Find the commit hash
   git checkout -b feature/recovered-work <commit-hash>
   # Or use git reflog output like: git checkout -b feature/recovered-work HEAD@{1}

   ls  # important.js should be back!
   ```

2. **Scenario 2: Recover after hard reset:**

   ```bash
   git checkout main

   # Create some commits
   echo "feature 1" >> app.js
   git add app.js
   git commit -m "Add feature 1"

   echo "feature 2" >> app.js
   git add app.js
   git commit -m "Add feature 2"

   # Accidentally reset too far
   git reset --hard HEAD~3

   # Recovery
   git reflog
   git reset --hard HEAD@{1}  # Or specific commit hash

   cat app.js  # Features should be back
   ```

3. **Scenario 3: Recover deleted file from previous commit:**

   ```bash
   # Accidentally delete and commit
   rm style.css
   git add style.css  # Stage the deletion
   git commit -m "Accidentally deleted CSS file"

   # Recover from previous commit
   git checkout HEAD~1 -- style.css
   git add style.css
   git commit -m "Recover accidentally deleted CSS file"

   cat style.css  # File should be back
   ```

4. **Scenario 4: Undo merge commit:**

   ```bash
   # Create a feature branch and merge it
   git checkout -b feature/bad-feature
   echo "bad code that breaks everything" > bad-feature.js
   git add bad-feature.js
   git commit -m "Add bad feature"

   git checkout main
   git merge feature/bad-feature

   # Realize the merge was bad and undo it
   git reset --hard HEAD~1  # If merge was most recent commit

   # Or if you already pushed (use revert instead)
   # git revert -m 1 HEAD  # -m 1 specifies which parent to revert to
   ```

## Advanced Recovery Techniques

### Using Git Fsck (File System Check)

```bash
# Find dangling commits (useful if reflog doesn't help)
git fsck --full

# Show dangling commits
git fsck --unreachable --no-reflogs

# Examine a dangling commit
git show <commit-hash>
```

### Interactive Reset

```bash
# Reset specific files only
git reset HEAD~1 -- specific-file.js

# Reset parts of files (interactive)
git reset -p  # Interactively choose what to unstage
```

## Common Recovery Scenarios Reference

### Lost Commits

**Symptoms:** Commits seem to have disappeared
**Solution:**

1. Check `git reflog`
2. Find the lost commit hash
3. Create new branch: `git checkout -b recovery <hash>`

### Accidentally Committed to Wrong Branch

**Symptoms:** Commits on main instead of feature branch
**Solution:**

```bash
git branch feature/my-work  # Create branch from current position
git reset --hard HEAD~N     # Reset main back N commits
```

### Mixed up Working Directory

**Symptoms:** Working directory has unwanted changes
**Solution:**

```bash
git stash  # Save work if needed
git reset --hard HEAD  # Clean working directory
git stash pop  # Restore work if stashed
```

### Deleted Files

**Symptoms:** Important files were deleted
**Solution:**

```bash
git checkout HEAD~1 -- <file>  # Restore from previous commit
git checkout <branch>:<file>   # Restore from specific branch
```

## Verification & Reflection

### Verify Your Understanding

1. **Test your knowledge:**

   ```bash
   # Check final repository state
   git status
   git log --oneline --graph
   git reflog | head -10
   git branch -a
   ```

2. **Practice quiz scenarios:**
   - How would you undo the last 3 commits without losing the work?
   - How would you remove all untracked files safely?
   - How would you undo a public commit that others might have pulled?

### Reflection Questions

1. **When would you use reset vs revert?**
2. **What's the difference between reset --soft, --mixed, and --hard?**
3. **How can you prevent needing these recovery commands?**
4. **What safety measures should you take before using destructive commands?**

## Safety Best Practices

### Before Destructive Operations

1. **Create backup branch:** `git branch backup-$(date +%Y%m%d)`
2. **Check working directory:** `git status` should be clean
3. **Use dry run options:** `git clean -n`, `git merge --no-commit`
4. **Double-check commands:** Especially with `--hard` or `--force`

### Communication

- **Warn teammates** before force pushing
- **Document recovery actions** for team awareness
- **Use protected branches** for important history

### Recovery Planning

- **Regular backups** of important repositories
- **Understand reflog limitations** (default 30-90 days)
- **Practice recovery** in safe environments
- **Know your hosting platform's** backup/restore features

## Key Takeaways

- **Reset changes history** - use carefully, especially `--hard`
- **Revert is safe for public commits** - creates new commits instead of changing history
- **Clean removes untracked files** - always use `-n` flag first
- **Reflog is your friend** - Git keeps track of where HEAD has been
- **Prevention is better than recovery** - good habits reduce the need for these commands
- **Practice in safe environments** - these commands can be destructive

## Next Steps

You now have the tools to handle most Git recovery scenarios. The next step is to integrate these techniques into your regular workflow and develop good habits that minimize the need for recovery operations.
